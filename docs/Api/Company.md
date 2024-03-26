# Company Endpoints

This page regroups a set of endpoints to interact with the companies related data.

## Retrieve data
- ### Single company data : `company/<id>`
To retrieve a company data you can request the api endpoint `/api/company/<id>`:

```http
GET / HTTP/1.1
Host: <BASE_URL>/api/company/<id>
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<id>` : The company's `id`  of whom you want to retrieve data.
- `<ACCESS TOKEN>` : The connected user's access token.


This endpoints response is a json object that contains the sector's data:

```json
{
    "id":27,
    "N_SIRET":12345,
    "commercial_name":"com_name",
    "legal_name":"legal_name",
    "address":"address",
    "telephone_number":"0612345789",
    "hubspot_company_id":"HUBSPOT_ID",
    "company_logo":"http://127.0.0.1:8000/media/logos/legal_name/logo.png",
    "activity_sector":"sector 1"
}
```
- ### All Companies Data : `company/`

To retrieve the list of all the companies and their data at once you can request the api endpoint `/api/company/`:

```http
GET / HTTP/1.1
Host: <BASE_URL>/api/company/
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<ACCESS TOKEN>` : The connected user's access token.

This endpoint response is a list of json objects that contains companies data (The list of all companies):

```json
[
    {
    "id":27,
    "N_SIRET":12345,
    "commercial_name":"com_name",
    "legal_name":"legal_name",
    "address":"address",
    "telephone_number":"0612345789",
    "hubspot_company_id":"HUBSPOT_ID",
    "company_logo":"http://127.0.0.1:8000/media/logos/legal_name/logo.png",
    "activity_sector":"sector 1"
    },
    {
    "id":25,
    "N_SIRET":12345,
    "commercial_name":"com_name_2",
    "legal_name":"legal_name_2",
    "address":"address_2",
    "telephone_number":"0617895899",
    "hubspot_company_id":"HUBSPOT_ID_2",
    "company_logo":"http://127.0.0.1:8000/media/logos/legal_name_2/logo.png",
    "activity_sector":"sector 2"
    },
    ...
    ...
]
```


## Delete Company

!!! important
    Only `ADMIN` users can delete companies.

To delete a company data you can request the api endpoint `/api/company/<id>`:

```http
DELETE / HTTP/1.1
Host: <BASE_URL>/api/company/<id>
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<id>` : The company's `id`  you want to delete.
- `<ACCESS TOKEN>` : The connected user's access token.

## Create company

!!! important
    Only `ADMIN` or `AG_DATA` users can create companies with this API.

To create a company data you can request the api endpoint `/api/company/`:

```http
NEEDS TO BE EDITED
POST / HTTP/1.1
Host: <BASE_URL>/api/company/
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
Body: {
    "N_SIRET":12345,
    "commercial_name":"com_name",
    "legal_name":"legal_name",
    "address":"address",
    "telephone_number":"0612345789",
    "hubspot_company_id":"HUBSPOT_ID",
    "company_logo":[PNG/JPG Object],
    "activity_sector":"sector 1"
    }
```
- `<ACCESS TOKEN>` : The connected user's access token.

## Update company

!!! important
    Only `ADMIN` or `AG_DATA` users can update companies with this API.

To update a company's data you can request the api endpoint `/api/company/<id>`:

```http
PUT / HTTP/1.1
Host: <BASE_URL>/api/company/<id>/
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
Body: {
    "N_SIRET":12345,
    "commercial_name":"com_name",
    "legal_name":"legal_name",
    "address":"address",
    "telephone_number":"0612345789",
    "hubspot_company_id":"HUBSPOT_ID",
    "company_logo":[PNG/JPG Object],
    "activity_sector":"sector 1"
    }
```
- `<ACCESS TOKEN>` : The connected user's access token.
- `<id>` : The company's `id`  you want to update.