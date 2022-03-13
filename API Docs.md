# Cloud Photos Storage API v1
- [Overview](#1-overview)
- [Authentication](#2-authentication)
  - [Signup](#21-signup) 
  - [Sign in](#22-sign-in) 
  - [Refresh Token](#23-refresh-token) 
- [Resources](#3-resources)
- [Testing](#4-testing)

## 1. Overview
Cloud Photos Storage's API is a JSON-based OAuth2 API. All requests are made to endpoints beginning: 

`https://cloud-fotos-storage.de/api/v1`

All requests must be secure, i.e. `https`, not `http`.

## 2. Authentication
![Drag Racing](https://postimg.cc/s14s1z9r)
### 2.1 Signup
`POST /auth/signup` 

``` 
 Content-Type: application/json
 Accept: application/json
 Accept-Charset: utf-8

 {
    "username": {{username}},
    "password": {{password}},
    "firstName": {{firstName}},
    "lastName": {{lastName}},
    "email": {{email}}
 }
```
| Parameter | Type | Required | Description |
| --- |---|---|---|
| username| string|required| username of your account|
| password| string|required| password of your account|
| firstName | string|required| first name of your account|
| lastName | string|required| last name of your account|
| email | string|required| email of your account|

### 2.2 Sign in
`POST /auth/signin`

##### 2.2.1 **Request**
``` 
 Content-Type: application/json
 Accept: application/json
 Accept-Charset: utf-8 

 {
    "username": {{username}},
    "password": {{password}}
 }
```
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
| refreshToken| string|required| A token that does not expire which may be used to acquire a new accessToken|
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

### 2.3 Refresh Token
`POST /auth/refreshToken`

##### 2.3.1 **Request**
``` 
 Content-Type: application/json
 Accept: application/json
 Accept-Charset: utf-8

 {
    "refreshToken": {{refreshToken}}
 }
```
| Parameter | Type | Required | Description |
| --- |---|---|---|
| refreshToken | string|required| "refreshToken" of your account|

##### 2.3.2 **Response**
``` 
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

 {
      "tokenType": "Bearer",
      "accessToken": {{accessToken}},
      "refreshToken": {{refreshToken}}

 }
```
| Parameter | Type | Required | Description |
| --- |---|---|---|
| tokenType| string|required| token type|
| accessToken| string|required| token of your account|
| refreshToken| string|required| A token that does not expire which may be used to acquire a new accessToken|


## 3. Resources


## 4. Testing
