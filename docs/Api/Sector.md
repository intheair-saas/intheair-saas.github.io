# Sectors Endpoints

This page regroups a set of endpoints to interact with the sectors related data.

## Retrieve data
- ### Single sector data : `sector/<id>`
To retrieve a sector data you can request the api endpoint `/api/sector/<id>`:

```http
GET / HTTP/1.1
Host: <BASE_URL>/api/sector/<id>
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<id>` : The sector's `id`  of whom you want to retrieve data, it is the `"activity_sector"` property.
- `<ACCESS TOKEN>` : The connected user's access token.


This endpoints response is a json object that contains the sector's data:

```json
{
    "activity_sector": "activity sector label",
    "description": "activity sector description",
}
```
- ### All sectors Data : `sector/`

To retrieve the list of all the sectors and their data at once you can request the api endpoint `/api/sector/`:

```http
GET / HTTP/1.1
Host: <BASE_URL>/api/sector/
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<ACCESS TOKEN>` : The connected user's access token.

This endpoint response is a list of json objects that contains sectors data (The list of all sectors):

```json
[
    {
    "activity_sector": "sector ",
    "description": "description",
    },
    {
    "activity_sector": "sector 2",
    "description": "description",
    },
    ...
    ...
]
```


## Delete Sector

!!! important
    Only `ADMIN` users can delete sectors.

To delete a sector data you can request the api endpoint `/api/sector/<id>`:

```http
DELETE / HTTP/1.1
Host: <BASE_URL>/api/sector/<id>
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<id>` : The sector's `id`  of whom you want to retrieve data, it is the `"activity_sector"` property.
- `<ACCESS TOKEN>` : The connected user's access token.

## Create Sector

!!! important
    Only `ADMIN` or `AG_DATA` users can create sectors with this API.

To create a sector data you can request the api endpoint `/api/sector/`:

```http
POST / HTTP/1.1
Host: <BASE_URL>/api/sector/
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
Body: {
    "activity_sector": "activity sector label",
    "description": "activity sector description",
}
```
- `<ACCESS TOKEN>` : The connected user's access token.