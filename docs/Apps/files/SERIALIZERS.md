# Serializers

# FileChunkSerializer
This is the Serializer that handles files uploaded by chunks.
It first receive the first chunk and create the file instance using the `create()` function, then for every other chunk received it updates the instance using the `update()` function.

## create()

This function receive the first chunk of the uploaded file and create an instance of the `FileChunkSerializer` that will represent the uploaded file. If the file has only one chunk, the file is directly created using that chunk and the `file` field is filled with the created file url.

```python
def create(self, validated_data):
        file = validated_data["chunk_file"]
        chunk_index = validated_data["chunk_index"]
        nbr_chunk = validated_data["nbr_chunk"]
        fileName = validated_data["file_name"]
        project_file = validated_data["to_project_files"]
        # create the FileChunk instance
        file_obj = FileChunk.objects.create(
            cumulated_chunk = 1,
            nbr_chunk = nbr_chunk,
            file_name = fileName,
            to_project_files = project_file
        )
       
        file_data = file.read()
        dir_name = project_file.project.name +'/'+ project_file.file_type.label +'/' +project_file.name + '/' +fileName.split('.')[0]
        file_id = f"{fileName}_{chunk_index + 1}"
        # create directory if not exist
        if not os.path.exists(f'buff/{dir_name}/tmp'): 
            os.makedirs(f'buff/{dir_name}/tmp')
        temp_file_path = ""
        # if the file has only one chunk it creates the file directly
        if nbr_chunk == 1:
            temp_file_path = f'buff/{dir_name}/{fileName}'
            temp_google_path = f'projects/{project_file.project.proj_type.label}/{dir_name}/{fileName}'
        # else it create a temp file for the chunk received
        else:
            temp_file_path = f'buff/{dir_name}/tmp/{file_id}'
            temp_google_path = f'projects/{project_file.project.proj_type.label}/{dir_name}/tmp/{file_id}'

        # creating the file in google cloud storage and local storage
        with open(temp_file_path, 'wb') as temp_file:
            temp_file.write(file_data)

        with default_storage.open(temp_google_path,'wb') as google_temp_file:
            google_temp_file.write(file_data)

        if nbr_chunk == 1:
            
            # retrieve created file url if the file has only one chunk (the chunk created is the file itself)
            file_obj.file = default_storage.url(temp_google_path)
            file_obj.save()
        return file_obj
```
## update()

this function receive the remaining chunks of an uploaded file, it the create a temperary file uing the chunk's data received. If all the file's chunks are received, this function proceed to group the chunks and create a final file from their data. It then fill the  `file` field  with the created file url.

```python
def update(self, instance, validated_data):
        file = validated_data["chunk_file"]
        chunk_index = validated_data["chunk_index"]
        # update number of chunks received
        instance.cumulated_chunk = instance.cumulated_chunk +1
        file_id = f"{instance.file_name}_{chunk_index + 1}"
        # write the received chunk data to a temp file
        dir_name = instance.to_project_files.project.name +'/'+ instance.to_project_files.file_type.label + '/' +instance.to_project_files.name + '/' +instance.file_name.split('.')[0]
        temp_file_path = f'buff/{dir_name}/tmp/{file_id}'
        temp_google_path = f'projects/{instance.to_project_files.project.proj_type.label}/{dir_name}/tmp/{file_id}'

        # writing the chunks
        with open(temp_file_path, 'wb') as temp_file:
            temp_file.write(file.read())

        with default_storage.open(temp_google_path, 'wb') as google_temp_file:
            google_temp_file.write(file.read())
        
        # save the instance modifications
        instance.save()

        # if we received all the chunks for a file
        if instance.cumulated_chunk == instance.nbr_chunk:
            # create the combined/final file path
            final_file_path = f'buff/{dir_name}/{instance.file_name}'
            final_google_path = f'projects/{ instance.to_project_files.project.proj_type.label}/{dir_name}/{instance.file_name}'
            print("combining chunks")

            # Combining chunks in local storage to form the final file
            with open(final_file_path, 'wb') as final_file:
                # reading all the chunks created 
                for i in range(instance.nbr_chunk):
                    chunk_file_path = f'buff/{dir_name}/tmp/{instance.file_name}_{i + 1}'
                    print(f"reading chunk {instance.file_name}_{i + 1}")
                    with open(chunk_file_path, 'rb') as chunk_file:
                        # writing the chunk's data to the final file
                        final_file.write(chunk_file.read())
            print("combining chunks in default storage")

             # Combining chunks in Cloud storage to form the final file
            with default_storage.open(final_google_path, 'wb') as google_final_file:

                # reading all the chunks created 
                for i in range(instance.nbr_chunk):
                    chunk_file_path = f'projects/{instance.to_project_files.project.proj_type.label}/{dir_name}/tmp/{instance.file_name}_{i + 1}'
                    print(f"reading chunk from default storage {instance.file_name}_{i + 1}")

                    # writing the chunk's data to the final file
                    with default_storage.open(chunk_file_path, 'rb') as chunk_file:
                        google_final_file.write(chunk_file.read())
                    
                    # delete the chunk read
                    default_storage.delete(chunk_file_path)
            
            # retrieve created file url
            instance.file = default_storage.url(final_google_path)

            instance.save()
            
        return instance
```
# ProjectFileChunkSerializer
This serializer creates a `ProjectFile` instance, this instance is linked to multiple `FileChunk` therefor it should be created before adding the `FileChunkSerializer`  instances

# DataTypeSerializer

This serializer creates a  `DataType` object, it needs either a `xlsx` file, a `csv` file or a collection of files forming a `shapefile` file. The data is then extracted form the files in order to extract the fields names and type and create a `DataType` and a model accordingly. 
## create()
This function uses an uploaded file and request parameters to create a `DataType` object accordingly, then a model is generated from the extracted data and then migrated and saved as a [`CustomDataType`](/Apps/files/MODELS/#customdatatype) instance for future migration purposes, the model expression is also saved in the `customModels.custom_models` file.

# Functions used

## upload_raster()
This function is used to extract a pyramid view from the raster file uploaded and save it as a `RasterLayer` object. [see what pyramid view means](https://developer.myptv.com/en/documentation/raster-maps-api/concepts/tiles)
### raster_save_data_task()
This function is a celery task used to process the uploaded `raster` file. It is run in background, and its job is to read the pixel matrix of raster file, and create the pyramid view from it saving the newly created matrixes to the database as `RasterTile` objects.
## create_color_map()
This function is used to read a .SLD file and extract the color map from it creating a `Legend` object and assigning this object to the newly created  `RatsterLayer`.
## upload_shapefile()
This function collect the `shapefile` file uploaded and the `DataType` of the `ProjectFile` and then calls the de [`save_geo_data`](#save_geo_data) function to extract the file's data.

## upload_shapefile_images()
This function collect the `shapefile` file uploaded and the `DataType` of the `ProjectFile` and then calls the de [`save_geo_data`](#save_geo_data) function to extract the file's data. It then retrieves the url for each picture uploaded with the shapefile and replace the corresponding path for the picture in the shapefile with the url retrieved.

## save_geo_data()

This function extract the data from an `shapefile` file of a `ProjectFile` object and then save this data in the table related to the `DataType` of this object using the `mapping` object to map the attribute name of the `shapefile` to the column name of the table used.

```python
def save_geo_data(path, object_type, projectfile, verbose=True):
    this_path = os.path.join(Path(__file__).resolve().parent.parent / 'buff', path)
    # retrieving the custom model linked to the Contenttype instence of this projectfile
    model = apps.get_model(app_label=object_type.app_label, model_name=object_type.model)
    mapping = {}
    if model == TopoData:
        mapping = topoData_mapping
    elif model == IlotDeChalleurData:
        mapping = ilotDeChalleur_mapping
    else:
        mapping = CustomDataType.objects.get(table_name=object_type.model).mapping
    lm = CustomLayerMapping(model, this_path, mapping)
    lm.save(projectfile=projectfile,strict=True, verbose=False)

```
## upload_xlsx()

This function collect the `xlsx` file uploaded and the `DataType` of the `ProjectFile` and then calls the de [`save_excel`](#save_excel) function to extract the file's data.

## save_excel()


This function extract the data from an `xlsx` file of a `ProjectFile` object and then save this data in the table related to the `DataType` of this object
```python
def save_excel(path, object_type, projectfile):
    this_path = os.path.join(Path(__file__).resolve().parent.parent / 'buff', path)
    model = apps.get_model(app_label=object_type.app_label, model_name=object_type.model)
    excel_data_df = pandas.read_excel(this_path)
    new_dict = excel_data_df.to_dict('records')
    # objs= json.loads(json_str)
    for obj in new_dict:
        new_obj = {}
        for key in obj:
            new_obj[str(key).lower().strip().replace(" ", "_")] = obj[key]
        test = model.objects.create(**new_obj)
        test.project_file = projectfile
        test.save()

```
## upload_csv()
This function collect the `csv` file uploaded and the `DataType` of the `ProjectFile` and then call the de [`save_csv`](#save_csv) function to extract the file's data.
## save_csv()

This function extract the data from an `csv` file of a `ProjectFile` object and then save this data in the table related to the `DataType` of this object

```python
def save_csv(path, object_type, projectfile):
    this_path = os.path.join(Path(__file__).resolve().parent.parent / 'buff', path)
    model = apps.get_model(app_label=object_type.app_label, model_name=object_type.model)
    excel_data_df = pandas.read_csv(this_path)
    new_dict = excel_data_df.to_dict('records')
    # objs= json.loads(json_str)
    for obj in new_dict:
        new_obj = {}
        for key in obj:
            key_name = str(key).lower().strip().replace(" ", "_")
            if key_name[0] == "\"":
                key_name = key_name[1:]
            if key_name[-1] == "\"":
                key_name = key_name[:-1]
            new_obj[key_name] = obj[key]
        test = model.objects.create(**new_obj)
        test.project_file = projectfile
        test.save()
```





## create_datatype_shapefile()
This function extracts the attributes from a shapefile and create a model from it saving it as a `CustomDataType` instance.
## create_datatype_csv()
This function extracts the attributes from a file csv and create a model from it saving it as a `CustomDataType` instance.
## create_datatype_excel()
This function extracts the attributes from an excel file and create a model from it saving it as a `CustomDataType` instance.

## create_model()
This function uses a `CustomDataType` and returns a model from it.

## migrate_datatype()
This function reads  the `CustomDataType` object created, creates a model from it and saving the model expression (The model Class definition) in the `customModels.custom_models` file, then running a migration in order to migrate the model created and synchronize the table with the new `DataType`. This newly created model is available on runtime and is deleted on server restart, but writing the model expression in the `customModels.custom_models` keeps the model active even after restarting the server


## migrate_all()
This function reads all the `CustomDataType` objects created, creates and saves the model expressions (The model Class definitions) in the `customModels.custom_models` file. This function is only used to sync the written models expressions in the `customModels.custom_models` with the `CustomDataType` objects available, and therefor its main purpose is to remove the deleted `DataType` objects from the `customModels.custom_models` file. The server must be restarted after the function call in order to take the `customModels.custom_models` changes into consideration. You can then run `makemigrations` and `migrate` after the server is restarted, or just call the `/api/refresh-datatype/` api endpoint while de server is on, in order to sync the models with the database.

!! warning
    If a model is not found in the `customModels.custom_models` file, it's table will be deleted on migrations and therefor all the data for this model (`DataType`) will be lost. Be careful when running a migration after editing this file.

