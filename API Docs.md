# Cloud Photos Storage API v1
- [Overview](#1-overview)
- [Authentication](#2-authentication)
- [Resources](#3-resources)
- [Testing](#4-testing)

## 1. Overview
Cloud Photos Storage's API is a JSON-based OAuth2 API. All requests are made to endpoints beginning:
`https://cloud-fotos-storage.de/api/v1`

All requests must be secure, i.e. `https`, not `http`.

## 2. Authentication
### 2.1 Signup
`POST /auth/signup` 

### 2.2 Sign in
``` 
 POST /auth/signin
 Host: api.medium.com
 Content-Type: application/x-www-form-urlencoded
 Accept: application/json
 Accept-Charset: utf-8 
```

##### 2.2.1 **Request**
| Parameter | Type | Required | Description |
| --- |---|---|---|
| username| string|required| username of your account|
| password| string|required| password of your account|

##### 2.2.2 **Response**
- SUCCESS
```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
  "id": {{id}},
  "username": {{username}},
  "email": {{email}},
  "tokenType": "Bearer",
  "accessToken": {{accessToken}},
  "refreshToken": {{refreshToken}}
}
```

| Parameter | Type | Required | Description |
| --- |---|---|---|
| id| string|required| id of your account|
| username| string|required| username of your account|
| email| string|required| email of your account|
| tokenType| string|required| token type|
| accessToken| string|required| token of your account|
| refreshToken| string|required| A token that does not expire which may be used to acquire a new access_token.|
- FAIL
```
HTTP/1.1 401 Unauthorized
Content-Type: application/json; charset=utf-8

{
  "path": {{path}},
  "error": "Unauthorized",
  "message": "Your token is invalid.",
  "status": 401
}
```

| Parameter | Type | Required | Description |
| --- |---|---|---|
| path| string|required| path|
| error| string|required| name of error|
| message| string|required| message from server|
| status| string|required| status code|


## 3. Resources


## 4. Testing
