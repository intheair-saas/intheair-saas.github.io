# User Endpoints

This page regroups a set of endpoints to interact with the users related data.

## Retrieve data
- ### Single user data : `user/<id>`
To retrieve a user data you can request the api endpoint `/api/user/<id>`:

```http
GET / HTTP/1.1
Host: <BASE_URL>/api/user/<id>
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<id>` : The user's `id`  of whom you want to retrieve data.
- `<ACCESS TOKEN>` : The connected user's access token.

!!! important
    If a user is a `CLIENT` type, he will only access and retrieve his own data via the API. If the user is a `ADMIN` or `AG_DATA` type, then he can access all the users data via the API.

This endpoints response is a json object that contains the user's data:

```json
{
    "id": 4,
    "last_login": "2023-12-13T10:05:19Z",
    "last_edit_date": "2023-12-13T10:05:19.029683Z",
    "email": "data2@gmail.com",
    "username": "user_test_data",
    "first_name": "user",
    "last_name": "user",
    "telephone_number": "0612345789",
    "position": "feqwfewfewf",
    "linkedin_url": "",
    "hubspot_user_id": "wfwqfdfwefewfewf",
    "company": "intheair",
    "company_id": 1,
    "user_type": "AG_DATA",
    "is_superuser": true,
    "is_staff": true
}

```
- ### All users Data : `user/`

To retrieve the list of all the users and their data at once you can request the api endpoint `/api/user/`:

```http
GET / HTTP/1.1
Host: <BASE_URL>/api/user/
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<ACCESS TOKEN>` : The connected user's access token.

!!! important
    If a user is a `CLIENT` type, he will only access and retrieve his own data via the API. If the user is a `ADMIN` or `AG_DATA` type, then he can access all the users data via the API.

This endpoint response is a list of json objects that contains users data (The list of all users):

```json
[
    {
        "id": 4,
        "last_login": "2023-12-13T10:05:19Z",
        "last_edit_date": "2023-12-13T10:05:19.029683Z",
        "email": "data2@gmail.com",
        "username": "user_test_data",
        "first_name": "user",
        "last_name": "user",
        "telephone_number": "0612345789",
        "position": "feqwfewfewf",
        "linkedin_url": "",
        "hubspot_user_id": "wfwqfdfwefewfewf",
        "company": "intheair",
        "company_id": 1,
        "user_type": "AG_DATA",
        "is_superuser": true,
        "is_staff": true
    },
    {
        "id": 4,
        "last_login": "2023-12-13T10:05:19Z",
        "last_edit_date": "2023-12-13T10:05:19.029683Z",
        "email": "data2@gmail.com",
        "username": "user_test_data",
        "first_name": "user",
        "last_name": "user",
        "telephone_number": "0612345789",
        "position": "feqwfewfewf",
        "linkedin_url": "",
        "hubspot_user_id": "wfwqfdfwefewfewf",
        "company": "intheair",
        "company_id": 1,
        "user_type": "AG_DATA",
        "is_superuser": true,
        "is_staff": true
    },
    ...
    ...
]
```
## Update User data

To update a user data you can request the api endpoint `/api/user/<id>`:

```http
PUT / HTTP/1.1
Host: <BASE_URL>/api/user/<id>
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
Body: {
    "email":"user@gmail.com",
    "username":"user_test",
    "first_name":"user",
    "last_name":"test test",
    "telephone_number":"0612345678",
    "position":"dev",
    "linkedin_url":"https://user.linkedin.com",
    "company":"company",
    "user_type":"CLIENT",
}
```
- `<id>` : The user's `id`  of whom you want to retrieve data.
- `<ACCESS TOKEN>` : The connected user's access token.

!!! important
    If a user is a `CLIENT` type, he can only update his own data via this API endpoint and can't update his password. If the user is a `ADMIN` or `AG_DATA` type, then he can update all the users data via this API call, he can also change user's password by adding the propriety `"password" : "new pass"` to the body object.

This endpoints response is a json object that contains the new user's data:

```json
{
    "id": 4,
    "last_login": "2023-12-13T10:05:19Z",
    "last_edit_date": "2023-12-13T10:05:19.029683Z",
    "email": "data2@gmail.com",
    "username": "user_test_data",
    "first_name": "user",
    "last_name": "user",
    "telephone_number": "0612345789",
    "position": "feqwfewfewf",
    "linkedin_url": "",
    "hubspot_user_id": "wfwqfdfwefewfewf",
    "company": "intheair",
    "company_id": 1,
    "user_type": "AG_DATA",
    "is_superuser": true,
    "is_staff": true
}
```

## Delete User

!!! important
    Only `ADMIN` users can delete users.

To delete a user data you can request the api endpoint `/api/user/<id>`:

```http
DELETE / HTTP/1.1
Host: <BASE_URL>/api/user/<id>
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
```
- `<id>` : The user's `id`  of whom you want to retrieve data.
- `<ACCESS TOKEN>` : The connected user's access token.

## Create User

!!! important
    Only `ADMIN` or `AG_DATA` users can create users with this API.

To create a user data you can request the api endpoint `/api/user/`:

```http
POST / HTTP/1.1
Host: <BASE_URL>/api/user/
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
Body: {
    "email":"user@gmail.com",
    "username":"user_test",
    "first_name":"user",
    "last_name":"test test",
    "telephone_number":"0612345678",
    "position":"dev",
    "linkedin_url":"https://user.linkedin.com",
    "password": "user password",
    "company":"company",
    "user_type":"CLIENT",
}
```
- `<ACCESS TOKEN>` : The connected user's access token.


## Change Password

This endpoint is for `CLIENT` type users to enable them to change their password.

To change the users password you need to call the api `/api/passwordchange/`.

```http
POST / HTTP/1.1
Host: <BASE_URL>/api/passwordchange/
Header:
    Authorization: Bearer <ACCESS TOKEN>
    Content-Type: application/json
Body: {
    "old_password": "Old user's password",
    "new_password": "New user's password",
}
```

- `<ACCESS TOKEN>` : The connected user's access token.