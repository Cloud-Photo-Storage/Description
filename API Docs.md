# Cloud Photos Storage API v1
- [Overview](#1-overview)
- [Authentication](#2-authentication)
  - [Signup](#21-signup) 
  - [Sign in](#22-sign-in) 
  - [Refresh Token](#23-refresh-token) 
  - [Verify email](#24-verify-email)
  - [Check username](#25-check-username)
- [Resources](#3-resources)
  - [User](#31user)
    - [Get User](#311-get-user)
    - [Update User](#312-update-user)
    - [Delete User](#313-delete-user)
  - [Account](#32-account)
    - [Reset Password](#321-reset-password)
    - [Reset Access Token](#322-reset-access-token)
- [Response Entity](#4-response-entity)
- [Testing](#5-testing)

## 1. Overview
Cloud Photos Storage's API is a JSON-based OAuth2 API. All requests are made to endpoints beginning: 

`https://cloud-fotos-storage.de/api/v1`

All requests must be secure, i.e. `https`, not `http`.

## 2. Authentication

![Drag Racing](https://i.postimg.cc/dQz1HthL/Screenshot-2022-03-13-at-21-04-25.png)
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

**Response**
```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
  "status": {{status}},
  "timestamp": {{timestamp}},
  "message": {{message}},
  "data": {{AuthResponse}}
}
```
| Parameter | Type | Required | Description |
| --- |---|---|---|
| data| [AuthResponse](#authresponse)|required| NULL when login failed|


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
  "status": {{status}},
  "timestamp": {{timestamp}},
  "message": {{message}},
  "data": {{AuthResponse}}
}
```
| Parameter | Type | Required | Description |
| --- |---|---|---|
| data| [AuthResponse](#authresponse)|required| NULL when login failed|

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
      "status": {{status}},
      "timestamp": {{timestamp}},
      "message": {{message}},
      "data": {
        "tokenType": "Bearer",
        "accessToken": {{accessToken}},
        "refreshToken": {{refreshToken}}
      }
      

 }
```
| Parameter | Type | Required | Description |
| --- |---|---|---|
| data| JSON|required| NULL when 'refreshToken' invalid|
| tokenType| string|required| token type|
| accessToken| string|required| token of your account|
| refreshToken| string|required| A token that does not expire which may be used to acquire a new accessToken|

### 2.4 Verify email
To verify your email, then you can use this account.

**Request**

`GET /auth/verify-email?token={{token-verify-email}}`

**Response**

``` 
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

 {
      "status": {{status}},
      "timestamp": {{timestamp}},
      "message": {{message}},
      "data": NULL

 }
```

### 2.5 Check username
Check if username available.

**Request**

`GET /auth/check-username?username={{username}}`

**Response**

``` 
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

 {
      "status": {{status}},
      "timestamp": {{timestamp}},
      "message": {{message}},
      "data": NULL

 }
```
## 3. Resources
### 3.1 User
#### 3.1.1 Get User
- Get user information. `Every one`, who has account, can access all user informations.
##### **Request**

`GET /user/{{userId}}`
```
Content-Type: application/json
Accept: application/json
Accept-Charset: utf-8
Authorization: Bearer {{accessToken}}

{}
```
##### **Response**
``` 
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

 {
      "status": {{status}},
      "timestamp": {{timestamp}},
      "message": {{message}},
      "data": {{data}}

 }
```

| Parameter | Type | Required | Description |
| --- |---|---|---|
| data| [UserResponse](#userresponse)|required| user details|

#### 3.1.2 Update User
Only user own this account can edit their information.

`PUT /user/{{userId}}`

```
Content-Type: application/json
Accept: application/json
Accept-Charset: utf-8
Authorization: Bearer {{accessToken}}

{
    "firstName": {{firstName}},
    "lastName": {{lastName}},
    "email": {{email}},
    "birthday": {{birthday}},
    "city": {{city}},
    "country": {{country}},
    "phone": {{phone}}
}
```

| Parameter | Type | Required |
| --- |---|---|
| firstName | string|not required| 
| lastName | string|not required| 
| email | string|not required|
| birthday | string|not required| 
| city | string|not required| 
| country | string|not required|
| phone | string|not required|

##### **Response**
``` 
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

 {
      "status": {{status}},
      "timestamp": {{timestamp}},
      "message": {{message}},
      "data": NULL

 }
```

#### 3.1.3 Delete User

- Only user own this account can delete their information.

##### **Request**

`DELETE /user/{{userId}}`
```
Content-Type: application/json
Accept: application/json
Accept-Charset: utf-8
Authorization: Bearer {{accessToken}}

{}
```
##### **Response**
``` 
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

 {
      "status": {{status}},
      "timestamp": {{timestamp}},
      "message": {{message}},
      "data": NULL

 }
```

### 3.2 Account
#### 3.2.1 Reset Password

User forgot password or just want to change their password. `It also will reset the access token of account`.

`POST /account/reset-password`
```
Content-Type: application/json
Accept: application/json
Accept-Charset: utf-8
Authorization: Bearer {{accessToken}}

{
    "newPassword": {{newPassword}}
}
```
##### **Response**
``` 
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

 {
      "status": {{status}},
      "timestamp": {{timestamp}},
      "message": {{message}},
      "data": {{AuthResponse}}

 }
```

| Parameter | Type | Required | Description |
| --- |---|---|---|
| data| [AuthResponse](#authresponse)|required| user details|


#### 3.2.2 Reset Access Token

User want to logout all devices.

`GET /account/reset-access-token`
```
Content-Type: application/json
Accept: application/json
Accept-Charset: utf-8
Authorization: Bearer {{accessToken}}

{}
```
##### **Response**
``` 
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

 {
      "status": {{status}},
      "timestamp": {{timestamp}},
      "message": {{message}},
      "data": {{AuthResponse}}

 }
```

| Parameter | Type | Required | Description |
| --- |---|---|---|
| data| [AuthResponse](#authresponse)|required| user details|

### 3.3 Image
#### 3.3.1 Upload Image

Who has account can upload image.

`POST /image`


#### 3.3.2 Get Image

User can see this image when they own this image or this image is set public.

`GET /image/{{imageId}}`


#### 3.3.3 Edit Image

Just only user, who own this image can edit.

`PUT /image/{{imageId}}`

#### 3.3.4 Delete Image
Just only user, who own this image can delete.

`DELETE /image/{{imageId}}`


### 3.4 Share

#### 3.4.1 Create a Share

`POST /share`


#### 3.4.2 Edit a Share

`PUT /share/{{tokenId}}`


#### 3.4.3 Delete a Share

`DELETE /share/{{tokenId}}`

#### 3.4.4 Get a Share

`GET /share/{{tokenId}}`

## 4. Response Entity

#### Status Code

| Code | Description |
| --- |---|
|0| Success|
|1| Fail|
|...|...|


#### CommonResponse

| Parameter | Type | Required | Description |
| --- |---|---|---|
| status| string|required| status|
| timestamp| string|required| timestamp of response |
| message| string|required| message of response|

#### AuthResponse

| Parameter | Type | Required | Description |
| --- |---|---|---|
| id| string|required| id of your account|
| username| string|required| username of your account|
| email| string|required| email of your account|
| tokenType| string|required| token type|
| accessToken| string|required| token of your account|
| refreshToken| string|required| A token that does not expire which may be used to acquire a new accessToken|

#### UserResponse

| Parameter | Type | Required | Description |
| --- |---|---|---|
| username| string|required| username of your account|
| firstName| string|required| firstName of your account|
| lastName| string|required| lastName  of your account|
| phone| string|required| phone of your account|
| city| string|required| city of your account|
| country| string|required| country of your account|
| birthday| string|required| birthday of your account|

## 5. Testing
