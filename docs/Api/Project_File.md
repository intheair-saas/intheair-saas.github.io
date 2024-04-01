# Project File API

This page regroups a list of API calls to manage the project files for each project.

## Retrieve data
- ### Single Project File data : `data/<id>`
To retrieve a project file data you can request the api endpoint `/api/data/<id>`:

```http
GET / HTTP/1.1
Host: <BASE_URL>/api/data/<id>
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<id>` : The project file's `id`  of whom you want to retrieve data.
- `<ACCESS TOKEN>` : The connected project file's access token.


This endpoints response is a json object that contains the project file's data, the project depends on the data type used by the project file:

- If the file is a `shapefile` it returns a GEOJson Object
- If the file is a `ANALYTICS` file type, it returns a JSON object containing the file's data

- ### Raster file

To display the raster file on a map you need to retreive the raster `id` from the project file, then use the Raster Tile API point to retreive the raster tiles depending on `x`, `y`, and `zoom` of the map

#### Retreive raster id:

to retreive the raster `id` from the project file you need to call this API endpoint


```http
GET / HTTP/1.1
Host: <BASE_URL>/api/rasterdata/<id>
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<ACCESS TOKEN>` : The connected project file's access token.
- `<id>` : id of the project file from whom you want to retrieve the rater id .

The response is as follow :

```json 
{
    "project_file": 2,
    "raster":36,
    "lon":,
    "lat":,
}
```
- `raster` : the raster layer id

#### Retreive raster tiles

to retreive the tiles of a raster layer you can use this API endpoint:

```http
GET / HTTP/1.1
Host: <BASE_URL>/api/raster/tiles/<layer>/<z>/<x>/<y>
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<ACCESS TOKEN>` : The connected project file's access token.
- `<layer>` : id of the raster layer, retreived with the `/api/rasterdata/<id>` request .
- `<x>` : the x position of the map.
- `<y>` : the y position of the map .
- `<z>` : the zomm level of the map.

This request returns the tile of the raster at the position <x>, <y>, and at the zoom level <z> as an image. You can use this endpoint tp retreive a TileLayer using other libraries like `Leadlet`
- ### All Project Files of a project : `projectfile/<id>`

To retrieve the list of all the project files and their data at once you can request the api endpoint `/api/projectfile/<id>/`:

```http
GET / HTTP/1.1
Host: <BASE_URL>/api/projectfile/<id>/
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<ACCESS TOKEN>` : The connected project file's access token.
- `<id>` : id of the project whom you want to retrieve the project files.

This endpoint response is a list of json objects that contains project files data (The list of all project files):

```json
[
    {
    "id": 236,
    "name": "VCLBRAG",
    "description": "bclabk",
    "file_type": "Geo_File",
    "data_type": null,
    "project": 1,
    "files": [
        {
            "id": 446,
            "nbr_chunk": 8,
            "file_name": "Orthomosaïque.tif",
            "file": "https://storage.googleapis.com/inthe_bucket/projects/projectType%201/project_test/VCLBRAG/Orthomosa%C3%AFque/Orthomosa%C3%AFque.tif?X-Goog-Algorithm=GOOG4-RSA-SHA256&X-Goog-Credential=inthe-account-med%40natural-point-401007.iam.gserviceaccount.com%2F20240301%2Fauto%2Fstorage%2Fgoog4_request&X-Goog-Date=20240301T182618Z&X-Goog-Expires=86400&X-Goog-SignedHeaders=host&X-Goog-Signature=373af158405dd8c5a7dc73d24556a30d3f7f2c6aaaeea0ed8928b601999967c9835401298a570594b08f6e678f33cdfc8a601cd7c41eef7e3030c73c7d7a1b800ebd6652e9409edccb79463c3bdf2f978d532dce34aa0c6d5cf5545080f0655396092b5fc0acc1a4508869f0d269ef9eecf2b03924744f9b48a88cb5563eca4ea06740228284e1c7befcb087b303be083b88a8a0e9eebd2c87ea562e2770f1d0d07a46177bc9a487feff7aaad57c246b06c3be6b04d19c9364e034bd15f999d3e652199d7b361fffeb27891542d648d45890bada1e0f5786216c5433619c4ee5c477d744b1dff0f659b5c462889ad1801d69b247e007f43fabc6eac7d703a3c2",
            "to_project_files": 236
        },
        ...
        ...
    ],
    "file_extention": "raster",
    "processed": false,
    "error": false,
    "error_message": ""
},
   {
    "id": 236,
    "name": "VCLBRAG",
    "description": "bclabk",
    "file_type": "Geo_File",
    "data_type": null,
    "project": 1,
    "files": [
        {
            "id": 446,
            "nbr_chunk": 8,
            "file_name": "Orthomosaïque.tif",
            "file": "https://storage.googleapis.com/inthe_bucket/projects/projectType%201/project_test/VCLBRAG/Orthomosa%C3%AFque/Orthomosa%C3%AFque.tif?X-Goog-Algorithm=GOOG4-RSA-SHA256&X-Goog-Credential=inthe-account-med%40natural-point-401007.iam.gserviceaccount.com%2F20240301%2Fauto%2Fstorage%2Fgoog4_request&X-Goog-Date=20240301T182618Z&X-Goog-Expires=86400&X-Goog-SignedHeaders=host&X-Goog-Signature=373af158405dd8c5a7dc73d24556a30d3f7f2c6aaaeea0ed8928b601999967c9835401298a570594b08f6e678f33cdfc8a601cd7c41eef7e3030c73c7d7a1b800ebd6652e9409edccb79463c3bdf2f978d532dce34aa0c6d5cf5545080f0655396092b5fc0acc1a4508869f0d269ef9eecf2b03924744f9b48a88cb5563eca4ea06740228284e1c7befcb087b303be083b88a8a0e9eebd2c87ea562e2770f1d0d07a46177bc9a487feff7aaad57c246b06c3be6b04d19c9364e034bd15f999d3e652199d7b361fffeb27891542d648d45890bada1e0f5786216c5433619c4ee5c477d744b1dff0f659b5c462889ad1801d69b247e007f43fabc6eac7d703a3c2",
            "to_project_files": 236
        },
        ...
        ...
    ],
    "file_extention": "raster",
    "processed": false,
    "error": false,
    "error_message": ""
},
    ...
    ...
    ]
```


## Delete Project File

!!! important
    Only `ADMIN` project files can delete project files.

To delete a projectfile data you can request the api endpoint `/api/projectfile/<id>`:

```http
DELETE / HTTP/1.1
Host: <BASE_URL>/api/projectfile/<id>
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<id>` : The project file's `id`  of whom you want to retrieve data.
- `<ACCESS TOKEN>` : The connected project file's access token.

## Upload/Create a Project File

!!! important
    Only `ADMIN` or `AG_DATA` project files can create project files with this API.

To create a projectfile data you can request the api endpoint `/api/projectfile/<id>/`:

first you need to create the projectfile :
```http
POST / HTTP/1.1
Host: <BASE_URL>/api/projectfile/<id>/
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
Body: {
    "name":"name of the file",
    "file_ext":"file extension id of the project file",
    "data_type":"id of the data type used by the project file",
    "project":"id of the project linked to the file",
    "description":"description of the project file",
}
```
- `<id>` : The project `id` to whom you want to link the file.
- `<ACCESS TOKEN>` : The connected project file's access token.


This endpoints response is a json object that contains the project file's data, the project depends on the data type used by the project file:

```json
{
    "id":263,
    "name":"pdf",
    "description":"dqdewq",
    "file_type":"PDF",
    "data_type":null,
    "project":1,
    "files":[],
    "file_extention":"pdf",
    "processed":false,
    "error":false,
    "error_message":""}
```

Then you need to upload the files (by chunks) to the projectfile using the following requests

!!! important
    The files are expected to be uploaded by chunks, you need to send the requst for each chunk.
    You first need to upload the fist chunk with the `/api/upload_file/` `POST` request in order to create the `FileChunk`([link to model](/Apps/files/MODELS#filechunk)). Then if the file has multiple chunks, you upload the rest of the chunks using the `/api/upload_file/<id>` `PUT` request. 
```http
POST / HTTP/1.1
Host: <BASE_URL>/api//api/upload_file/
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
Body: {
    "to_project_files":"name of the file",
    "chunk_file":"file extension id of the project file",
    "chunk_index":"id of the data type used by the project file",
    "nbr_chunk":"id of the project linked to the file",
    "file_name":"description of the project file",
}
```

```http
PUT / HTTP/1.1
Host: <BASE_URL>/api//api/upload_file/<id>
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
Body: {
    "to_project_files":"id of the projectfile this file is linked to",
    "chunk_file":"file/blob containing the chunk of the main file",
    "chunk_index":"index of the chunk relativaly to the main file",
    "nbr_chunk":"number of chunks the file is divided to ",
    "file_name":"main file name",
}
```
- `<id>` : The `FileChunk` `id` to whom you want to upload the chunk.
- `<ACCESS TOKEN>` : The connected project file's access token.

This endpoints response is a json object that contains the file's attributes:

```json
{
    "id":499,
    "nbr_chunk":8,
    "file_name":"Orthomosaïque.tif",
    "file":null,
    "to_project_files":264
}
```
when all chunks are uploaded and combined, the final chunk upload request returns this response:

```json
{
    "id":499,
    "nbr_chunk":8,
    "file_name":"Orthomosaïque.tif",
    "file":"https://storage.googleapis.com/inthe_bucket/projects/projectType%201/project_test/PDF/vvsvsdavsa/Orthomosa%C3%AFque/Orthomosa%C3%AFque.tif?X-Goog-Algorithm=GOOG4-RSA-SHA256&X-Goog-Credential=inthe-account-med%40natural-point-401007.iam.gserviceaccount.com%2F20240328%2Fauto%2Fstorage%2Fgoog4_request&X-Goog-Date=20240328T234224Z&X-Goog-Expires=86400&X-Goog-SignedHeaders=host&X-Goog-Signature=37aa0c996d795d25eed966f326861c2652760ba7f6a29067d991a68b73dd73ca67a6c29bc15d5ecb4bf6499c1d6148fcf1eb9f08074d31e6542bd16b61dc78a3e994233f1d4efb2eb7ee567599c09754c7ced4bbc5e6dd7f80d5ce2eb3e8e552388f4b3559d9d2b03963df3d61d64eb55d2d2890231e806b30e9175052b6732410ab15a92d771f81d46cdce6499c4d70f34ac03025cd9a43e1546941b03bdd2434a722659bcd7cf6885dfae22c0e2f8a9774ff4cc877d2df31876aa82886252088eb694a368449bbae5ef19c5972225f73ec52c124290679694e92f4a8620dddc4456e384b6b6a9c0b1917674aac70daab09fc05c6debcd4122a8df6e8b38861",
    "to_project_files":264
}
```
-  `file` : url of the uploaded file in the cloud storage



## Process a Project File

!!! note
    After uploading all the chunks of the files related to a `ProjectFile`, you need to call `api/process_file/<id>` to process the `ProjectFile`



```http
GET / HTTP/1.1
Host: <BASE_URL>/api//api/upload_file/<id>
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```

The response for this request is the processing state of the file, either a successeful response or an error response containing the error that occured.


## ProjectFile LayerOrder on the map