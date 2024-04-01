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
    "id": 1,
    "project_type_label": "projectType 1",
    "user_username": "g_b_m",
    "delimitation_field": {
        "type": "FeatureCollection",
        "features": [
            {
                "id": 1,
                "type": "Feature",
                "geometry": {
                    "type": "MultiPolygon",
                    "coordinates": [
                        [
                            [
                                [
                                    -0.628928171672778,
                                    43.44337246385466,
                                    0
                                ],
                                ...,
                                [
                                    -0.630138420045127,
                                    43.44103583242925,
                                    0
                                ],
                                [
                                    -0.628928171672778,
                                    43.44337246385466,
                                    0
                                ]
                            ]
                        ]
                    ]
                },
                "properties": {
                    "project": 1
                }
            }
        ]
    },
    "name": "project_test",
    "description": "gregreergerw",
    "hubspot_proj_id": "johfodsfhof",
    "last_edit": "2024-01-26T14:56:29.900846Z",
    "area_file": "https://storage.googleapis.com/inthe_bucket/projects/projectType%201/project_test/Arthez.kml?X-Goog-Algorithm=GOOG4-RSA-SHA256&X-Goog-Credential=inthe-account-med%40natural-point-401007.iam.gserviceaccount.com%2F20240328%2Fauto%2Fstorage%2Fgoog4_request&X-Goog-Date=20240328T231454Z&X-Goog-Expires=86400&X-Goog-SignedHeaders=host&X-Goog-Signature=3e933fb285a6d71a80a78b838ed04c908483e2a543ee1b245606d6529fc87eb54cc34f540d61ba9005958b848d0319b17f06226476c2ef64edf63e8c234b1d6f3338d2209e28224031cf7dcf5f0bad127bce3c669ec6848bb4e4681235ffa0ce0abf83ac45304723e31717fc634cc122b7d8e3eb3cd57604cb5804a9f320906843f43fc3f6e8f1c1670178697ab75705137ca8324f5743f2759f770f0310187b1150032ef4ad747b8fd8a1fe1b9591002b6fac28bac67e5a547b035994dfe12a1cd92bf64fd7229557e0c933d0318a389fb2fe3bfdc8cb885d60e2fa1de786a701a129c8131342178e1830c950324013e8e194da70234ac25f733c3d6a06d7ba",
    "area": 169751.05240838812,
    "lat": -0.6289281716727779,
    "long": 43.44337246385466,
    "is_archived": false,
    "proj_type": 1,
    "user": 2
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
    "id": 1,
    "project_type_label": "projectType 1",
    "user_username": "g_b_m",
    "delimitation_field": {
        "type": "FeatureCollection",
        "features": [
            {
                "id": 1,
                "type": "Feature",
                "geometry": {
                    "type": "MultiPolygon",
                    "coordinates": [
                        [
                            [
                                [
                                    -0.628928171672778,
                                    43.44337246385466,
                                    0
                                ],
                                ...,
                                [
                                    -0.630138420045127,
                                    43.44103583242925,
                                    0
                                ],
                                [
                                    -0.628928171672778,
                                    43.44337246385466,
                                    0
                                ]
                            ]
                        ]
                    ]
                },
                "properties": {
                    "project": 1
                }
            }
        ]
    },
    "name": "project_test",
    "description": "gregreergerw",
    "hubspot_proj_id": "johfodsfhof",
    "last_edit": "2024-01-26T14:56:29.900846Z",
    "area_file": "https://storage.googleapis.com/inthe_bucket/projects/projectType%201/project_test/Arthez.kml?X-Goog-Algorithm=GOOG4-RSA-SHA256&X-Goog-Credential=inthe-account-med%40natural-point-401007.iam.gserviceaccount.com%2F20240328%2Fauto%2Fstorage%2Fgoog4_request&X-Goog-Date=20240328T231454Z&X-Goog-Expires=86400&X-Goog-SignedHeaders=host&X-Goog-Signature=3e933fb285a6d71a80a78b838ed04c908483e2a543ee1b245606d6529fc87eb54cc34f540d61ba9005958b848d0319b17f06226476c2ef64edf63e8c234b1d6f3338d2209e28224031cf7dcf5f0bad127bce3c669ec6848bb4e4681235ffa0ce0abf83ac45304723e31717fc634cc122b7d8e3eb3cd57604cb5804a9f320906843f43fc3f6e8f1c1670178697ab75705137ca8324f5743f2759f770f0310187b1150032ef4ad747b8fd8a1fe1b9591002b6fac28bac67e5a547b035994dfe12a1cd92bf64fd7229557e0c933d0318a389fb2fe3bfdc8cb885d60e2fa1de786a701a129c8131342178e1830c950324013e8e194da70234ac25f733c3d6a06d7ba",
    "area": 169751.05240838812,
    "lat": -0.6289281716727779,
    "long": 43.44337246385466,
    "is_archived": false,
    "proj_type": 1,
    "user": 2
},
    {
    "id": 1,
    "project_type_label": "projectType 1",
    "user_username": "g_b_m",
    "delimitation_field": {
        "type": "FeatureCollection",
        "features": [
            {
                "id": 1,
                "type": "Feature",
                "geometry": {
                    "type": "MultiPolygon",
                    "coordinates": [
                        [
                            [
                                [
                                    -0.628928171672778,
                                    43.44337246385466,
                                    0
                                ],
                                ...,
                                [
                                    -0.630138420045127,
                                    43.44103583242925,
                                    0
                                ],
                                [
                                    -0.628928171672778,
                                    43.44337246385466,
                                    0
                                ]
                            ]
                        ]
                    ]
                },
                "properties": {
                    "project": 1
                }
            }
        ]
    },
    "name": "project_test",
    "description": "gregreergerw",
    "hubspot_proj_id": "johfodsfhof",
    "last_edit": "2024-01-26T14:56:29.900846Z",
    "area_file": "https://storage.googleapis.com/inthe_bucket/projects/projectType%201/project_test/Arthez.kml?X-Goog-Algorithm=GOOG4-RSA-SHA256&X-Goog-Credential=inthe-account-med%40natural-point-401007.iam.gserviceaccount.com%2F20240328%2Fauto%2Fstorage%2Fgoog4_request&X-Goog-Date=20240328T231454Z&X-Goog-Expires=86400&X-Goog-SignedHeaders=host&X-Goog-Signature=3e933fb285a6d71a80a78b838ed04c908483e2a543ee1b245606d6529fc87eb54cc34f540d61ba9005958b848d0319b17f06226476c2ef64edf63e8c234b1d6f3338d2209e28224031cf7dcf5f0bad127bce3c669ec6848bb4e4681235ffa0ce0abf83ac45304723e31717fc634cc122b7d8e3eb3cd57604cb5804a9f320906843f43fc3f6e8f1c1670178697ab75705137ca8324f5743f2759f770f0310187b1150032ef4ad747b8fd8a1fe1b9591002b6fac28bac67e5a547b035994dfe12a1cd92bf64fd7229557e0c933d0318a389fb2fe3bfdc8cb885d60e2fa1de786a701a129c8131342178e1830c950324013e8e194da70234ac25f733c3d6a06d7ba",
    "area": 169751.05240838812,
    "lat": -0.6289281716727779,
    "long": 43.44337246385466,
    "is_archived": false,
    "proj_type": 1,
    "user": 2
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


