
# Types API

This page contains all the api calls to handle the different types used by the saas platform

## Project Types

Project types are labels that define projects domain or field, it is a classification label that enables advanced queries.

## Retrieve data

- ### Single Project Type data : `projecttype/<id>`

To retrieve a project type data you can request the api endpoint `/api/projecttype/<id>`:

```http
GET / HTTP/1.1
Host: <BASE_URL>/api/projecttype/<id>
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<id>` : The project type's `id`  of whom you want to retrieve data.
- `<ACCESS TOKEN>` : The connected user's access token.


This endpoints response is a json object that contains the project types's data:

```json
{
    "id":1,
    "label":"project type 1"
}
```
- ### All Project Types Data : `projecttype/`

To retrieve the list of all the project types and their data at once you can request the api endpoint `/api/projecttype/`:

```http
GET / HTTP/1.1
Host: <BASE_URL>/api/projecttype/
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<ACCESS TOKEN>` : The connected user's access token.

This endpoint response is a list of json objects that contains project types data (The list of all project types):

```json
[
    {
        "id":1,
        "label":"project type 1"
    },
    {
        "id":2,
        "label":"project type 2"
    },
    ...
    ...
    ]
```


## Delete Project Types

!!! important
    Only `ADMIN` users can delete project types.

To delete a project type you can request the api endpoint `/api/projecttype/<id>`:

```http
DELETE / HTTP/1.1
Host: <BASE_URL>/api/projecttype/<id>
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<id>` : The project type's `id`  of whom you want to delete.
- `<ACCESS TOKEN>` : The connected user's access token.

## Create Project Type

!!! important
    Only `ADMIN` or `AG_DATA` users can create project types with this API.

To create a project type data you can request the api endpoint `/api/projecttype/`:

```http
POST / HTTP/1.1
Host: <BASE_URL>/api/projecttype/
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
Body: {
    "label": "project type's label",
}
```
- `<ACCESS TOKEN>` : The connected user's access token.

## File Types

File Types are labels that defines the type of file used in a project, it enables a process selection to process the file depending on its type, but also enables advanced queries.

## Retrieve data
- ### Single file type data : `filetype/<id>`
To retrieve a file type data you can request the api endpoint `/api/filetype/<id>`:

```http
GET / HTTP/1.1
Host: <BASE_URL>/api/filetype/<id>
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<id>` : The file type's `id`  of whom you want to retrieve data.
- `<ACCESS TOKEN>` : The connected user's access token.


This endpoints response is a json object that contains the filetype's data:

```json
{ 
    "id":1,
    "label":"Geo_File",
    "description":"Files that contains Geo Data"
}
```
- ### All File Types Data : `filetype/`

To retrieve the list of all the file types and their data at once you can request the api endpoint `/api/filetype/`:

```http
GET / HTTP/1.1
Host: <BASE_URL>/api/filetype/
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<ACCESS TOKEN>` : The connected user's access token.

This endpoint response is a list of json objects that contains file types data (The list of all file types):

```json
[
    {
        "id":1,
        "label":"Geo_File",
        "description":"Files that contains Geo Data"
    },
    {
        "id":2,
        "label":"Analytics",
        "description":"Files that contains Annalytic Data"
    },
    {
        "id":3,
        "label":"3D",
        "description":"3D Files"
    },
    {
        "id":4,
        "label":"Images",
        "description":"Image Files"
    },
    {
        "id":5,
        "label":"PDF",
        "description":"PDF Compressed files"
    }
    ...
    ...
]
```


## Delete File Type

!!! important
    Only `ADMIN` users can delete filetypes.

To delete a filetype data you can request the api endpoint `/api/filetype/<id>`:

```http
DELETE / HTTP/1.1
Host: <BASE_URL>/api/filetype/<id>
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<id>` : The file type's `id`  of whom you want to delete data.
- `<ACCESS TOKEN>` : The connected user's access token.

## Create File Type

!!! important
    Only `ADMIN` or `AG_DATA` users can create file types with this API.

To create a file type data you can request the api endpoint `/api/filetype/`:

```http
POST / HTTP/1.1
Host: <BASE_URL>/api/filetype/
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
Body: {
        "label":"file type label",
        "description":"file type description"
    }
```
- `<ACCESS TOKEN>` : The connected user's access token.

## Data Types

Data Types are labels that defines the type of data used in a file, it enables a visualization and processing selection to process the and display the data in each file depending on its type, it also enables advanced queries and classification.

## Retrieve data

To retrieve the list of all the datatypes and their data at once you can request the api endpoint `/api/datatype/`:

```http
GET / HTTP/1.1
Host: <BASE_URL>/api/datatype/
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<ACCESS TOKEN>` : The connected user's access token.

This endpoint response is a list of json objects that contains datatypes data (The list of all datatypes):

```json
[
    {
        "id":1,
        "label":"data type label",
        "description":"data type description",
        "fields":[
            "field_1",
            "field_2",
            "field_3",
            "field_4",
            ...
            ...
            ]
    },
    {
        "id":2,
        "label":"data type label",
        "description":"data type description",
        "fields":[
            "field_1",
            "field_2",
            "field_3",
            "field_4",
            ...
            ...
            ]
    },
    ...
    ...
]
```


## Delete datatype

!!! important
    Only `ADMIN` users can delete data types.

To delete a datatype data you can request the api endpoint `/api/datatype/<id>`:

```http
DELETE / HTTP/1.1
Host: <BASE_URL>/api/datatype/<id>
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<id>` : The datatype's `id`  you wanna delete.
- `<ACCESS TOKEN>` : The connected user's access token.

## Create datatype

!!! important
    Only `ADMIN` or `AG_DATA` users can create data types with this API.

To create a datatype data you can request the api endpoint `/api/datatype/`:

```http
POST / HTTP/1.1
Host: <BASE_URL>/api/datatype/
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
Body: {
    "label": "data type label",
    "description": "data type description",
    "uploaded_files": [XLSX File OR SHP File and its dependencies]
}
```
- `<ACCESS TOKEN>` : The connected user's access token.

## File Extensions

File Extensions are labels that defines the extension of a file, it is used to define the technology used for a file, but also enables an advanced classification. Each File Extension is linked to a File Type


## Retrieve data

To retrieve the list of all the file Extensions at once you can request the api endpoint `/api/fileext/`:

```http
GET / HTTP/1.1
Host: <BASE_URL>/api/fileext/
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<ACCESS TOKEN>` : The connected user's access token.

This endpoint response is a list of json objects that contains file Extensions data (The list of all file Extensions):

```json
[
    {
        "id":3,
        "extention":"pdf",
        "file_type":5
    },
    {
        "id":2,
        "extention":"png",
        "file_type":6
    },
    ...
    ...
]
```


## Delete File Extensions

!!! important
    Only `ADMIN` users can delete data types.

To delete a file Extensions data you can request the api endpoint `/api/fileext/<id>`:

```http
DELETE / HTTP/1.1
Host: <BASE_URL>/api/fileext/<id>
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<id>` : The file Extension's `id`  you wanna delete.
- `<ACCESS TOKEN>` : The connected user's access token.

## Create File Extensions

!!! important
    Only `ADMIN` or `AG_DATA` users can create data types with this API.

To create a file Extensions data you can request the api endpoint `/api/fileext/`:

```http
POST / HTTP/1.1
Host: <BASE_URL>/api/fileext/
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
Body: {
    "extention": "extension label",
    "file_type": "id of the file type linked to the extension
}
```
- `<ACCESS TOKEN>` : The connected user's access token.
