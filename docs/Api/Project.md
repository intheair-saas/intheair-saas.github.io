#  Project Endpoints

This page regroups a set of endpoints to interact with the projects related data.

## Retrieve data
- ### Single project data : `project/<id>`
To retrieve a project data you can request the api endpoint `/api/project/<id>`:

```http
GET / HTTP/1.1
Host: <BASE_URL>/api/project/<id>
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<id>` : The project's `id`  of whom you want to retrieve data.
- `<ACCESS TOKEN>` : The connected user's access token.


This endpoints response is a json object that contains the project's data:

```json
{
    "id":2,
    "project_type_label":"project type 1",
    "user_username":"user_test",
    "delimitation_field":[geojson object],
    "name":"project_1",
    "description":"Shapefiles for geodata",
    "hubspot_proj_id":"ffwefw",
    "last_edit":"2023-09-16T12:59:40.080984Z",
    "area_file":"http://127.0.0.1:8000/media/projects/project%20type%201/project_1/Arrete_ZICAD_01-2023_1.kml",
    "is_archived":false,
    "proj_type":1,
    "user":2
}
```
- `project_type_label` : label of the project type the project is associated with.
- `delimitation_field` : A geojson object generated from the kml file used to create the project, it is the delimitation polygone of the project on the map.
- `area_file` : Link to the kml file of the project, it is the file that contains the delimitation shape of the project on the map.
- `proj_type` : `id` of the project type the project is associated to.
- `user` : `id` of the user the project is associated to

- ### All Projects Data : `project/`

To retrieve the list of all the projects and their data at once you can request the api endpoint `/api/project/`:

```http
GET / HTTP/1.1
Host: <BASE_URL>/api/project/
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<ACCESS TOKEN>` : The connected user's access token.

This endpoint response is a list of json objects that contains projects data (The list of all projects):

```json
[
    {
        "id":2,
        "project_type_label":"project type 1",
        "user_username":"user_test",
        "delimitation_field":[geojson object],
        "name":"project_1",
        "description":"Shapefiles for geodata",
        "hubspot_proj_id":"ffwefw",
        "last_edit":"2023-09-16T12:59:40.080984Z",
        "area_file":"http://127.0.0.1:8000/media/projects/project%20type%201/project_1/Arrete_ZICAD_01-2023_1.kml",
        "is_archived":false,
        "proj_type":1,
        "user":2
    },
    {
        "id":3,
        "project_type_label":"project type 1",
        "user_username":"user_test",
        "delimitation_field":[geojson object],
        "name":"project_2",
        "description":"Geographic Data with points",
        "hubspot_proj_id":"ffwefw",
        "last_edit":"2023-09-16T13:00:57.562308Z",
        "area_file":"http://127.0.0.1:8000/media/projects/project%20type%201/project_2/Arrete_ZICAD_01-2023_1.kml",
        "is_archived":false,
        "proj_type":1,
        "user":2
    }
    ...
    ...
]
```


## Delete project

!!! important
    Only `ADMIN` users can delete projects.

To delete a project data you can request the api endpoint `/api/project/<id>`:

```http
DELETE / HTTP/1.1
Host: <BASE_URL>/api/project/<id>
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<id>` : The project's `id`  you want to delete.
- `<ACCESS TOKEN>` : The connected user's access token.

## Create project

!!! important
    Only `ADMIN` or `AG_DATA` users can create projects with this API.

To create a project data you can request the api endpoint `/api/project/`:

```http
NEEDS TO BE EDITED
POST / HTTP/1.1
Host: <BASE_URL>/api/project/
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
Body: {
    "name":"project_2",
    "description":"Geographic Data with points",
    "hubspot_proj_id":"ffwefw",
    "area_file":[KML FILE],
    "proj_type":1,
    "user":2
}
```
- `<ACCESS TOKEN>` : The connected user's access token.
- `proj_type` : `id` of the project type the project is associated to.
- `user` : `id` of the user the project is associated to


## Update project

!!! important
    Only `ADMIN` or `AG_DATA` users can update projects with this API.

To update a project's data you can request the api endpoint `/api/project/`:

```http
NEEDS TO BE EDITED
PUT / HTTP/1.1
Host: <BASE_URL>/api/project/<id>
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
Body: {
    "name":"project_2",
    "description":"Geographic Data with points",
    "hubspot_proj_id":"ffwefw",
    "area_file":[KML FILE],
    "proj_type":1,
    "user":2
}
```
- `<ACCESS TOKEN>` : The connected user's access token.
- `<id>` : The project's `id`  you want to update.


