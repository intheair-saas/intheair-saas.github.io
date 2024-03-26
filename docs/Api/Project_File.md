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

#### Geo_Data

    TO ADD LATER

#### Analytics
    
    TO ADD LATER

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
        "id":1,
        "name":"shapefile",
        "description":"Shapefiles for geodata",
        "file_type":"Geo_File",
        "data_type":2,
        "project":2,
        "files":[
            {
                "id":1,
                "file_type":"shapefile",
                "file":"http://127.0.0.1:8000/media/projects/project%20type%201/project_1/Geo_File/shapefile/Exemple_TOPO.shx",
                "to_project_files":1
            },
            {
                "id":2,
                "file_type":"shapefile",
                "file":"http://127.0.0.1:8000/media/projects/project%20type%201/project_1/Geo_File/shapefile/Exemple_TOPO.shp",
                "to_project_files":1
            },
            ...
            ...
            ],
        "processed":true
    },
    {
        "id":1,
        "name":"shapefile",
        "description":"Shapefiles for geodata",
        "file_type":"Geo_File",
        "data_type":2,
        "project":2,
        "files":[
            {
                "id":1,
                "file_type":"shapefile",
                "file":"http://127.0.0.1:8000/media/projects/project%20type%201/project_1/Geo_File/shapefile/Exemple_TOPO.shx",
                "to_project_files":1
            },
            {
                "id":2,
                "file_type":"shapefile",
                "file":"http://127.0.0.1:8000/media/projects/project%20type%201/project_1/Geo_File/shapefile/Exemple_TOPO.shp",
                "to_project_files":1
            },
            ...
            ...
            ],
        "processed":true
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

## Upload Project File

!!! important
    Only `ADMIN` or `AG_DATA` project files can create project files with this API.

To create a projectfile data you can request the api endpoint `/api/projectfile/<id>/`:

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
    "uploaded_files":[List of the files that forms the project file]
}
```
- `<id>` : The project `id` to whom you want to link the file.
- `<ACCESS TOKEN>` : The connected project file's access token.
