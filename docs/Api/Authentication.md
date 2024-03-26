# Authentication API

!!! note 
    This section is a documentation describing the usage of the Auth endpoints API.

## Login

The login system is based on a `JWT` systeme, that can be explained in the graph below:

To login to the backend server and retrieve the data accessible by the user, a post request should be sent by the user to the `/api/login` endpoint:

![Jason Web Token](../assets/JWT.jpg "Jason Web Token")

### Request

In order to login to the API you need to send a `POST` request containing the `email` and `password` of the user.

```http
POST / HTTP/1.1
Host: <BASE_URL>/api/login/
Header:
    Content-Type: application/json
Body: {
    email: <EMAIL>,
    password: <PASS>
}
```
- `<BASE URL>` : Being the base url of the backend.
- `<EMAIL>` : The users email.
- `<PASS>` : The users Password in clear text.

### Response

The response for this request is a json object that contains 2 tokens, a `refresh` token and an `access` token.

here is an example:

```json
{"refresh":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImV4cCI6MTY5OTEwMTA0MiwiaWF0IjoxNjk2NTA5MDQyLCJqdGkiOiI5ZjA3NjdjMDNmZmM0ZTI3YmQxODYzMTJiZmI2YjdhZiIsInVzZXJfaWQiOjEsInVzZXJuYW1lIjoiaGFpdGFtIiwidXNlcl90eXBlIjoiQURNSU4ifQ.vzuSpBGo9SxNmHhCpfUkgxZWRM20dvUl5eJQCStxW1M","access":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjk2NTA5NDAyLCJpYXQiOjE2OTY1MDkwNDIsImp0aSI6ImM0Y2EzMWFhMjc1ZTQxOTQ4ZTk1MDY0YWJmNWQ3NTQ3IiwidXNlcl9pZCI6MSwidXNlcm5hbWUiOiJoYWl0YW0iLCJ1c2VyX3R5cGUiOiJBRE1JTiJ9.t8Dw6dCRVWIJf6yJOTALjCbBfgUhdfUC3Swtm2-KP5g"}
```

- `access` : The access token is used for the authentication process on each request, it expires every **5 minutes** (to be configured in the backend setup)and needs to be refreshed with the refresh token generated with it.
- `refresh` : The refresh token is used to refresh the access token and keep the session open. A refresh token gets expired after 30 days

### Data

The access token is JWT encoded and contains some data related to the user connected. if we decode the token we can retrieve this object:

```json
{
  "token_type": "access",
  "exp": 1696509402,
  "iat": 1696509042,
  "jti": "c4ca31aa275e41948e95064abf5d7547",
  "user_id": 1,
  "username": "haitam",
  "user_type": "ADMIN"
}
```


 - `username` : username of the user connected.
 - `user_type`: Type of the user connected (you can check the user types available in [user types](../Apps/user/Models.md/#user-types))

## Refresh

The `access` used for authentication gets expired every 5 minutes to maintain a certain level of security. To keep the connection open the user needs to refresh that token using the refresh token generated with it.

### Request

To refresh the `access` token you need to send a request as follow:

```http
POST / HTTP/1.1
Host: <BASE_URL>/api/token/refresh/
Header:
    Content-Type: application/json
Body: { 
    refresh: <REFRESH TOKEN>
    }
```

- `<BASE URL>` : Being the base url of the backend.
- `<REFRESH TOKEN>` : The refresh token generated earlier.

### Response

The response for the refresh token is as follow :

```json
{"refresh":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImV4cCI6MTY5OTEwMTA0MiwiaWF0IjoxNjk2NTA5MDQyLCJqdGkiOiI5ZjA3NjdjMDNmZmM0ZTI3YmQxODYzMTJiZmI2YjdhZiIsInVzZXJfaWQiOjEsInVzZXJuYW1lIjoiaGFpdGFtIiwidXNlcl90eXBlIjoiQURNSU4ifQ.vzuSpBGo9SxNmHhCpfUkgxZWRM20dvUl5eJQCStxW1M","access":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjk2NTA5NDAyLCJpYXQiOjE2OTY1MDkwNDIsImp0aSI6ImM0Y2EzMWFhMjc1ZTQxOTQ4ZTk1MDY0YWJmNWQ3NTQ3IiwidXNlcl9pZCI6MSwidXNlcm5hbWUiOiJoYWl0YW0iLCJ1c2VyX3R5cGUiOiJBRE1JTiJ9.t8Dw6dCRVWIJf6yJOTALjCbBfgUhdfUC3Swtm2-KP5g"}
```

- `access` : New generated access token.
- `refresh` : New refresh token.

!!! important
    After a `refresh` token is used, it is being **blacklisted**, therefore impossible to use a second time to refresh the access token. You will need to use the newly generated `refresh` token every time you refresh the `access` token.