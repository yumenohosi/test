# NoServ REST API

## Summary
API URL은 현재 noserv.io:2337로 설정되어있음

#### Objects

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/classes/:className | POST | Creating Objects
| /1/classes/:className/:objectId | GET | Retrieving Objects |
| /1/classes/:className/:objectId | PUT | Updating Objects |
| /1/classes/:className | GET | Queries |
| /1/classes/:className/:objectId | DELETE | Deleting Objects |

#### Users

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/users | POST | Signing Up |
| /1/login | GET | Logging In |
| /1/users/:objectId | GET | Retrieving Users |
| /1/users/me | GET | Validating Session Tokens / Retrieving Current User |
| /1/users/:objectId | PUT | Updating Users |
| /1/users | GET | Querying Users |
| /1/users/:objectId | DELETE | Deleting Users |
| /1/requestPasswordReset | POST | Requesting A Password Reset |

#### Apps

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/apps | POST | Creating Apps |
| /1/apps/:objectId | GET | Retrieving Apps |
| /1/apps/:objectId | PUT | Updating Apps |
| /1/apps | GET | Queries |
| /1/apps/:objectId | DELETE | Deleting Apps |

#### Installations

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/installations | POST | Creating Installations |
| /1/installations/:objectId | GET | Retrieving Installations |
| /1/installations/:objectId | PUT | Updating Installations |
| /1/installations | GET | Queries |
| /1/installations/:objectId | DELETE | Deleting Installations |


#### Push Notifications

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/push | POST | Sending Pushes |
| /1/push/:objectId | GET | Retrieving Pushes |
| /1/push | GET | Queries |

#### Files

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/files/:fileName | POST | Uploading Files |
| /1/files/:fileName | DELETE | Deleting Files |

_ _ _
## 공통
### request
* POST, PUT일 때 데이터는 body에 JSON 형식의 text로 보내고, 헤더에 Content-Type: application/json 을 넣어준다.
* 모든 데이터는 생성과 수정시에 문서에 명시된 필드 외에 다른 필드도 추가할 수 있다.

### response
* status Codes
| code | 이름 | 내용 |
|------|------|------|
|200|ok| 성공 |
|201|created| 생성됨 |
|403|NotAuthorized| token이나 key값이 잘못되었을 경우 |
|404|NotFound| 잘못된 URL |
|409|InvalidArgument| 주로 키값이 겹쳤을 때 발생 |
|500|InternalServerError | 알수없는 서버 에러 |

위의 코드 외에도 상황에 따라 표준 서버 에러가 발생할 수 있음.

에러 유무는 statusCode로 판별하며, 오류가 발생했을 때 response body는 다음과 같은 형식

```json
{
  "code":"NotAuthorized",
    "message":"Session Token is required"
}
```

### Query
Query는 GET 호출이므로 모든 속성은 urlencode하여 호출한다.
호출시 데이터는 다음과 같으며, 모두 생략 가능하다.

| Data | Sample Value |
|------|------|
|where|{"score":10, "name":"park"}|
|order|score,-name|
|limit|10|
|skip|20|
|count|1|

* where
다음과 같은 옵션을 조합해 사용할 수 있다.
| Key | Operation | Sample |
|------|------| ------ |
|$lt | Less Than| {"num" : {"$lt" : 2 } } |
|$lte | Less Than Or Equal To| {"num" : {"$lte" : 2 } } |
|$gt | Greater Than| {"num" : {"$gt" : 2 } } |
|$gte | Greater Than Or Equal To| {"num" : {"$gte" : 2 } } |
|$ne | Not Equal To|{"num" : {"$ne" : 2 } } |
|$in | Contained In| {"arr" : {"$in" : ["c", "b"] } } |
|$nin | Not Contained in| {"arr" : {"$nin" : ["b"] } } |
|$exists | A value is set for the key|{"arr" : {"$exists" : true } }|
|$all | Contains all of the given values|{"arr" : {"$all" : ["b", "c"] } }|
|$or| or | {"$or" : [{"arr" : {"$nin" : ["b"] } }, {"num" : {"$gte" : 2 } }] } |

* order
소트할 컬럼명을 설정한다. 여러개일 경우 콤마(,)로 구분하여 나열하며, descending일 경우 컬럼명 앞에 마이너스부호(-)를 넣는다.

* limit
조회 결과의 최대 갯수를 설정

* skip
조회 결과의 앞에서부터 스킵할 갯수를 설정

* count
count = 1로 설정할 경우 where 조건에 맞는 갯수만을 리턴한다. 이 때 리턴 데이터는 아래와 같이 results는 비워두고 count를 추가한다.
```json
{
  "results": [
  ],
  "count": 1337
}
```

## Objects
| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/classes/:className | POST | Creating Objects
| /1/classes/:className/:objectId | GET | Retrieving Objects |
| /1/classes/:className/:objectId | PUT | Updating Objects |
| /1/classes/:className | GET | Queries |
| /1/classes/:className/:objectId | DELETE | Deleting Objects |

_ _ _

### Creating Objects
| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/classes/:className | POST | Creating Objects

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Application-Id|fWmCni6GmXiBs3Twx6j9cOPNaZWt7P6vnS4vHnDU|
|X-Noserv-REST-API-Key|o3ifa8izHAhhcj2cHXjUrZtWnZhJKn3QJIn6x2RF|
|Content-Type|application/json|

##### response
==Success==

Status: 201 Created
createdAt, objectId가 항상 들어온다.
```json
{
  "createdAt": "2011-08-20T02:06:57.931Z",
  "objectId": "Ed1nuqPvcm"
}
```
_ _ _
### Retrieving Objects

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/classes/:className/:objectId | GET | Retrieving Objects |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Application-Id|fWmCni6GmXiBs3Twx6j9cOPNaZWt7P6vnS4vHnDU|
|X-Noserv-REST-API-Key|o3ifa8izHAhhcj2cHXjUrZtWnZhJKn3QJIn6x2RF|

##### response
==Success==
createdAt, updatedAt, objectId가 항상 들어온다. 나머지는 예제
```json
{
  "score": 1337,
  "playerName": "Sean Plott",
  "cheatMode": false,
  "skills": [
    "pwnage",
    "flying"
  ],
  "createdAt": "2011-08-20T02:06:57.931Z",
  "updatedAt": "2011-08-20T02:06:57.931Z",
  "objectId": "Ed1nuqPvcm"
}
```
_ _ _
### Updating Objects

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/classes/:className/:objectId | PUT | Updating Objects |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Application-Id|fWmCni6GmXiBs3Twx6j9cOPNaZWt7P6vnS4vHnDU|
|X-Noserv-REST-API-Key|o3ifa8izHAhhcj2cHXjUrZtWnZhJKn3QJIn6x2RF|
|Content-Type|application/json|

##### response
==Success==
updatedAt이 항상 들어온다.
```json
{
  "updatedAt": "2011-08-21T18:02:52.248Z"
}
```
_ _ _
### Queries

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/classes/:className | GET | Queries |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Application-Id|fWmCni6GmXiBs3Twx6j9cOPNaZWt7P6vnS4vHnDU|
|X-Noserv-REST-API-Key|o3ifa8izHAhhcj2cHXjUrZtWnZhJKn3QJIn6x2RF|

##### response
==Success==
항상 배열을 results로 감싸서 리턴한다.
```json
{
  "results": [
    {
      "playerName": "Jang Min Chul",
      "updatedAt": "2011-08-19T02:24:17.787Z",
      "cheatMode": false,
      "createdAt": "2011-08-19T02:24:17.787Z",
      "objectId": "A22v5zRAgd",
      "score": 80075
    },
    {
      "playerName": "Sean Plott",
      "updatedAt": "2011-08-21T18:02:52.248Z",
      "cheatMode": false,
      "createdAt": "2011-08-20T02:06:57.931Z",
      "objectId": "Ed1nuqPvcm",
      "score": 73453
    }
  ]
}
```
_ _ _
### Deleting Objects

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/classes/:className/:objectId | DELETE | Deleting Objects |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Application-Id|fWmCni6GmXiBs3Twx6j9cOPNaZWt7P6vnS4vHnDU|
|X-Noserv-REST-API-Key|o3ifa8izHAhhcj2cHXjUrZtWnZhJKn3QJIn6x2RF|

##### response
==Success==
리턴 데이터 없음.


_ _ _
## Users
| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/users | POST | Signing Up |
| /1/login | GET | Logging In |
| /1/users/:objectId | GET | Retrieving Users |
| /1/users/me | GET | Validating Session Tokens / Retrieving Current User |
| /1/users/:objectId | PUT | Updating Users |
| /1/users | GET | Querying Users |
| /1/users/:objectId | DELETE | Deleting Users |
_ _ _
### Signing Up
| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/users | POST | Signing Up |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Application-Id|fWmCni6GmXiBs3Twx6j9cOPNaZWt7P6vnS4vHnDU|
|X-Noserv-REST-API-Key|o3ifa8izHAhhcj2cHXjUrZtWnZhJKn3QJIn6x2RF|
|Content-Type|application/json|

_ _ _
| Data | Sample Value |
|------|------|
|username|test|
|password|test|

위의 데이터는 필수이고, 나머지 앱에서 회원관리에 필요한 데이터를 더 받아서 넣으면 됨.

##### response
==Success==

Status: 201 Created
createdAt, objectId, sessionToken이 항상 들어온다.
```json
{
  "createdAt": "2011-08-20T02:06:57.931Z",
  "objectId": "Ed1nuqPvcm",
  "sessionToken": "pnktnjyb996sj4p156gjtp4im"
}
```
_ _ _
### Logging In
| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/login | GET | Logging In |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Application-Id|fWmCni6GmXiBs3Twx6j9cOPNaZWt7P6vnS4vHnDU|
|X-Noserv-REST-API-Key|o3ifa8izHAhhcj2cHXjUrZtWnZhJKn3QJIn6x2RF|

_ _ _
| Data | Sample Value |
|------|------|
|username|test|
|password|test|

Get방식이므로 username과 password는 urlencode해서 보내야 함.

##### response
==Success==

createdAt, updatedAt, objectId, sessionToken이 항상 들어온다. 나머지 user데이터도 password만 빼고 보내준다.
```json
{
  "username": "cooldude6",
  "phone": "415-392-0202",
  "createdAt": "2011-11-07T20:58:34.448Z",
  "updatedAt": "2011-11-07T20:58:34.448Z",
  "objectId": "g7y9tkhB7O",
  "sessionToken": "pnktnjyb996sj4p156gjtp4im"
}
```
_ _ _
### Retrieving Users
| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/users/:objectId | GET | Retrieving Users |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Application-Id|fWmCni6GmXiBs3Twx6j9cOPNaZWt7P6vnS4vHnDU|
|X-Noserv-REST-API-Key|o3ifa8izHAhhcj2cHXjUrZtWnZhJKn3QJIn6x2RF|

##### response
==Success==

createdAt, updatedAt, objectId가 항상 들어온다. 나머지 user데이터도 password만 빼고 보내준다.
```json
{
  "username": "cooldude6",
  "phone": "415-392-0202",
  "createdAt": "2011-11-07T20:58:34.448Z",
  "updatedAt": "2011-11-07T20:58:34.448Z",
  "objectId": "g7y9tkhB7O"
}
```
_ _ _
### Validating Session Tokens / Retrieving Current User
현재 접속자의 정보를 가져온다.
토큰값으로 접속자를 판단하므로 토큰값 검증에도 이용한다.

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/users/me | GET | Validating Session Tokens / Retrieving Current User |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Application-Id|fWmCni6GmXiBs3Twx6j9cOPNaZWt7P6vnS4vHnDU|
|X-Noserv-REST-API-Key|o3ifa8izHAhhcj2cHXjUrZtWnZhJKn3QJIn6x2RF|
|X-Noserv-Session-Token|pnktnjyb996sj4p156gjtp4im|

##### response
==Success==

createdAt, updatedAt, objectId가 항상 들어온다. 나머지 user데이터도 password만 빼고 보내준다.
```json
{
  "username": "cooldude6",
  "phone": "415-392-0202",
  "createdAt": "2011-11-07T20:58:34.448Z",
  "updatedAt": "2011-11-07T20:58:34.448Z",
  "objectId": "g7y9tkhB7O"
}
```
_ _ _
### Updating Users

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/users/:objectId | PUT | Updating Users |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Application-Id|fWmCni6GmXiBs3Twx6j9cOPNaZWt7P6vnS4vHnDU|
|X-Noserv-REST-API-Key|o3ifa8izHAhhcj2cHXjUrZtWnZhJKn3QJIn6x2RF|
|X-Noserv-Session-Token|pnktnjyb996sj4p156gjtp4im|
|Content-Type|application/json|

##### response
==Success==

updatedAt이 항상 들어온다.
```json
{
  "updatedAt": "2011-11-07T21:25:10.623Z"
}
```
_ _ _
### Querying Users

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/users | GET | Querying Users |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Application-Id|fWmCni6GmXiBs3Twx6j9cOPNaZWt7P6vnS4vHnDU|
|X-Noserv-REST-API-Key|o3ifa8izHAhhcj2cHXjUrZtWnZhJKn3QJIn6x2RF|

##### response
==Success==

항상 배열을 results로 감싸서 리턴한다.
```json
{
  "results": [
    {
      "username": "bigglesworth",
      "phone": "650-253-0000",
      "createdAt": "2011-11-07T20:58:06.445Z",
      "updatedAt": "2011-11-07T20:58:06.445Z",
      "objectId": "3KmCvT7Zsb"
    },
    {
      "username": "cooldude6",
      "phone": "415-369-6201",
      "createdAt": "2011-11-07T20:58:34.448Z",
      "updatedAt": "2011-11-07T21:25:10.623Z",
      "objectId": "g7y9tkhB7O"
    }
  ]
}
```
_ _ _
### Deleting Users

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/users/:objectId | DELETE | Deleting Users |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Application-Id|fWmCni6GmXiBs3Twx6j9cOPNaZWt7P6vnS4vHnDU|
|X-Noserv-REST-API-Key|o3ifa8izHAhhcj2cHXjUrZtWnZhJKn3QJIn6x2RF|
|X-Noserv-Session-Token|pnktnjyb996sj4p156gjtp4im|

##### response
==Success==
리턴 데이터 없음.



_ _ _
## Apps
웹 전용 API로, 앱 관리에 사용

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/apps | POST | Creating Apps |
| /1/apps/:objectId | GET | Retrieving Apps |
| /1/apps/:objectId | PUT | Updating Apps |
| /1/apps | GET | Queries |
| /1/apps/:objectId | DELETE | Deleting Apps |
_ _ _
### Creating Apps
| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/apps | POST | Creating Apps |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Session-Token|pnktnjyb996sj4p156gjtp4im|
|Content-Type|application/json|

_ _ _
| Data | Sample Value |
|------|------|
|appname|test|

##### response
==Success==

Status: 201 Created
createdAt, objectId, 각종 key token들이 생성되어 들어온다. 
```json
{
  "createdAt": "2011-08-20T02:06:57.931Z",
  "objectId": "Ed1nuqPvcm",
  "applicationId":"94dpGeRxeBiFJw7lX7JuLGXNTE8JvuAD",
  "clientKey":"nCBnEcpuKb0YHRLJl5TZTR1xDiy6SSeU",
  "javascriptKey":"oJAoQ8Jmn5zr6DzWWUPcMNWc3bceoz7n",
  "dotNetKey":"5v1Yl8z9oNsNASa6VwHgm3ZC5Zgo7v6D",
  "restApiKey":"4WdHTzXPWXnJWz053pN6uvS9GmoQ4q5X",
  "masterKey":"pSbixs1SpzVe5DI0rCTtO24uz0VFv6uU"
}
```
_ _ _
### Retrieving Apps

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/apps/:objectId | GET | Retrieving Apps |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Session-Token|pnktnjyb996sj4p156gjtp4im|

##### response
==Success==
createdAt, updatedAt, objectId, 각종 key가 항상 들어온다. 데이터가 더 있다면 함께 들어온다.
```json
{
  "createdAt": "2011-08-20T02:06:57.931Z",
  "updatedAt": "2011-08-20T02:06:57.931Z",
  "objectId": "Ed1nuqPvcm",
  "applicationId":"94dpGeRxeBiFJw7lX7JuLGXNTE8JvuAD",
  "clientKey":"nCBnEcpuKb0YHRLJl5TZTR1xDiy6SSeU",
  "javascriptKey":"oJAoQ8Jmn5zr6DzWWUPcMNWc3bceoz7n",
  "dotNetKey":"5v1Yl8z9oNsNASa6VwHgm3ZC5Zgo7v6D",
  "restApiKey":"4WdHTzXPWXnJWz053pN6uvS9GmoQ4q5X",
  "masterKey":"pSbixs1SpzVe5DI0rCTtO24uz0VFv6uU"
}
```
_ _ _
### Updating Apps

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/apps/:objectId | PUT | Updating Apps |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Session-Token|pnktnjyb996sj4p156gjtp4im|
|Content-Type|application/json|

##### response
==Success==
updatedAt이 항상 들어온다.
```json
{
  "updatedAt": "2011-08-21T18:02:52.248Z"
}
```
_ _ _
### Queries
세션토큰에 해당하는 유저가 생성한 app만 조회한다.

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/apps | GET | Queries |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Session-Token|pnktnjyb996sj4p156gjtp4im|

##### response
==Success==
항상 배열을 results로 감싸서 리턴한다.
```json
{
  "results": [
    {
      "createdAt": "2011-08-20T02:06:57.931Z",
      "updatedAt": "2011-08-20T02:06:57.931Z",
      "objectId": "Ed1nuqPvcm",
      "appname": "test1",
      "applicationId":"94dpGeRxeBiFJw7lX7JuLGXNTE8JvuAD",
      "clientKey":"nCBnEcpuKb0YHRLJl5TZTR1xDiy6SSeU",
      "javascriptKey":"oJAoQ8Jmn5zr6DzWWUPcMNWc3bceoz7n",
      "dotNetKey":"5v1Yl8z9oNsNASa6VwHgm3ZC5Zgo7v6D",
      "restApiKey":"4WdHTzXPWXnJWz053pN6uvS9GmoQ4q5X",
      "masterKey":"pSbixs1SpzVe5DI0rCTtO24uz0VFv6uU"
    },
    {
      "createdAt": "2011-08-20T02:06:57.931Z",
      "updatedAt": "2011-08-20T02:06:57.931Z",
      "objectId": "Ed1nuqPvcm",
      "appname": "test2",
      "applicationId":"94dpGeRxeBiFJw7lX7JuLGXNTE8JvuAD",
      "clientKey":"nCBnEcpuKb0YHRLJl5TZTR1xDiy6SSeU",
      "javascriptKey":"oJAoQ8Jmn5zr6DzWWUPcMNWc3bceoz7n",
      "dotNetKey":"5v1Yl8z9oNsNASa6VwHgm3ZC5Zgo7v6D",
      "restApiKey":"4WdHTzXPWXnJWz053pN6uvS9GmoQ4q5X",
      "masterKey":"pSbixs1SpzVe5DI0rCTtO24uz0VFv6uU"
    }
  ]
}
```
_ _ _

### Deleting Apps

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/apps/:objectId | DELETE | Deleting Apps |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Session-Token|pnktnjyb996sj4p156gjtp4im|

##### response
==Success==
리턴 데이터 없음.



_ _ _
## Installations
모바일 Push를 위한 클라이언트 장비 정보를 관리함.

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/installations | POST | Creating Installations |
| /1/installations/:objectId | GET | Retrieving Installations |
| /1/installations/:objectId | PUT | Updating Installations |
| /1/installations | GET | Queries |
| /1/installations/:objectId | DELETE | Deleting Installations |
_ _ _
### Creating Installations
| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/installations | POST | Creating Installations |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Application-Id|fWmCni6GmXiBs3Twx6j9cOPNaZWt7P6vnS4vHnDU|
|X-Noserv-REST-API-Key|o3ifa8izHAhhcj2cHXjUrZtWnZhJKn3QJIn6x2RF|
|Content-Type|application/json|

_ _ _
| Data | Sample Value |
|------|------|
|deviceType|android|
|deviceToken|0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef|
|pushType|gcm|

* ios의 경우 pushType이 없으며, deviceType은 ios로 한다.

##### response
==Success==

Status: 201 Created
createdAt, objectId가 항상 들어온다.
```json
{
  "createdAt": "2011-08-20T02:06:57.931Z",
  "objectId": "Ed1nuqPvcm"
}
```
_ _ _
### Retrieving Installations

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/installations/:objectId | GET | Retrieving Installations |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Application-Id|fWmCni6GmXiBs3Twx6j9cOPNaZWt7P6vnS4vHnDU|
|X-Noserv-REST-API-Key|o3ifa8izHAhhcj2cHXjUrZtWnZhJKn3QJIn6x2RF|

##### response
==Success==
createdAt, updatedAt, objectId가 항상 들어온다. 데이터가 더 있다면 함께 들어온다.
```json
{
  "deviceType": "ios",
  "deviceToken": "0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef",
  "channels": [
    ""
  ],
  "createdAt": "2012-04-28T17:41:09.106Z",
  "updatedAt": "2012-04-28T17:41:09.106Z",
  "objectId": "mrmBZvsErB"
}
```
_ _ _
### Updating Installations

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/installations/:objectId | PUT | Updating Installations |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Application-Id|fWmCni6GmXiBs3Twx6j9cOPNaZWt7P6vnS4vHnDU|
|X-Noserv-REST-API-Key|o3ifa8izHAhhcj2cHXjUrZtWnZhJKn3QJIn6x2RF|
|Content-Type|application/json|

##### response
==Success==
updatedAt이 항상 들어온다.
```json
{
  "updatedAt": "2011-08-21T18:02:52.248Z"
}
```
_ _ _
### Queries
조회에는 Master Key가 필요하다.

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/installations | GET | Queries |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Application-Id|fWmCni6GmXiBs3Twx6j9cOPNaZWt7P6vnS4vHnDU|
|X-Noserv-Master-Key|o3ifa8izHAhhcj2cHXjUrZtWnZhJKn3QJIn6x2RF|

##### response
==Success==
항상 배열을 results로 감싸서 리턴한다.
```json
{
  "results": [
    {
      "deviceType": "ios",
      "deviceToken": "0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef",
      "channels": [
        ""
      ],
      "createdAt": "2012-04-28T17:41:09.106Z",
      "updatedAt": "2012-04-28T17:41:09.106Z",
      "objectId": "mrmBZvsErB"
    },
    {
      "deviceType": "ios",
      "deviceToken": "fedcba9876543210fedcba9876543210fedcba9876543210fedcba9876543210",
      "channels": [
        ""
      ],
      "createdAt": "2012-04-30T01:52:57.975Z",
      "updatedAt": "2012-04-30T01:52:57.975Z",
      "objectId": "sGlvypFQcO"
    }
  ]
}
```
_ _ _

### Deleting Installations
삭제에는 Master Key가 필요하다.

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/installations/:objectId | DELETE | Deleting Installations |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Application-Id|fWmCni6GmXiBs3Twx6j9cOPNaZWt7P6vnS4vHnDU|
|X-Noserv-Master-Key|o3ifa8izHAhhcj2cHXjUrZtWnZhJKn3QJIn6x2RF|

##### response
==Success==
리턴 데이터 없음.


_ _ _
## Push Notifications
모바일 Push를 발송한다.

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/push | POST | Sending Pushes |
| /1/push/:objectId | GET | Retrieving Pushes |
| /1/push | GET | Queries |
_ _ _
### Sending Pushes
| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/push | POST | Sending Pushes |


##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Application-Id|fWmCni6GmXiBs3Twx6j9cOPNaZWt7P6vnS4vHnDU|
|X-Noserv-REST-API-Key|o3ifa8izHAhhcj2cHXjUrZtWnZhJKn3QJIn6x2RF|
|Content-Type|application/json|

* Using Channels
body를 다음과 같은 형태로 구현하면 channels를 포함하는 installation들에 push를 보낸다.
```json
{
    "channels": [
      "Giants"
    ],
    "data": {
      "alert": "The Giants won against the Mets 2-3."
    }
}
```
* Using Advanced Targeting
다음과 같이 installation에 추가한 컬럼들을 통해 구독자를 검색할 수 있다.
```json
{
	"where": {
        "scores": true,
        "gameResults": true,
        "injuryReports": true
    },
    "data": {
      "alert": "The Giants won against the Mets 2-3."
    }
}
```
* data는 다음과 같은 컬럼을 사용할 수 있다.
| 컬럼 | Device Type | 설명 | 
|------|------|------|
| alert | All | the notification's message |
| badge | iOS | the value indicated in the top right corner of the app icon. This can be set to a value or to Increment in order to increment the current value by 1 |
| sound | iOS | the name of a sound file in the application bundle |
| contentAvailable | iOS | If you are a writing a Newsstand app, or an app using the Remote Notification Background Mode introduced in iOS7 (a.k.a. "Background Push"), set this value to 1 to trigger a background download |
| category | iOS | the identifier of the UIUserNotificationCategory for this push notification |
| action | Android | the Intent should be fired when the push is received. If no title or alert values are specified, the Intent will be fired but no notification will appear to the user |
| title | Android | the value displayed in the Android system tray notification |

##### response
==Success==

Status: 201 Created
createdAt, objectId가 항상 들어온다.
```json
{
  "createdAt": "2011-08-20T02:06:57.931Z",
  "objectId": "Ed1nuqPvcm"
}
```

_ _ _
### Retrieving Pushes

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/push/:objectId | GET | Retrieving Push |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Application-Id|fWmCni6GmXiBs3Twx6j9cOPNaZWt7P6vnS4vHnDU|
|X-Noserv-Master-Key|o3ifa8izHAhhcj2cHXjUrZtWnZhJKn3QJIn6x2RF|

##### response
==Success==
createdAt, updatedAt, objectId가 항상 들어온다. 데이터가 더 있다면 함께 들어온다.
Master Key가 필요하다.
```json
{
    "_sendCount": 1,
    "data": {
      "alert": "The Giants won against the Mets 2-3."
    },
    "where": {
      "scores": true,
      "gameResults": true,
      "injuryReports": true
    },
    "createdAt": "2012-04-28T17:41:09.106Z",
    "updatedAt": "2012-04-28T17:41:09.106Z",
    "objectId": "mrmBZvsErB"
}
```
_ _ _
### Queries
조회에는 Master Key가 필요하다.

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/installations | GET | Queries |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Application-Id|fWmCni6GmXiBs3Twx6j9cOPNaZWt7P6vnS4vHnDU|
|X-Noserv-Master-Key|o3ifa8izHAhhcj2cHXjUrZtWnZhJKn3QJIn6x2RF|

##### response
==Success==
항상 배열을 results로 감싸서 리턴한다.
```json
{
  "results": [
    {
      "_sendCount": 1,
      "data": {
        "alert": "The Giants won against the Mets 2-3."
      },
      "where": {
        "scores": true,
        "gameResults": true,
        "injuryReports": true
      },
      "createdAt": "2012-04-28T17:41:09.106Z",
      "updatedAt": "2012-04-28T17:41:09.106Z",
      "objectId": "mrmBZvsErB"
    },
    {
      "_sendCount": 1,
      "data": {
        "alert": "The Giants won against the Mets 2-3."
      },
      "where": {
        "scores": true,
        "gameResults": true,
        "injuryReports": true
      },
      "createdAt": "2012-04-30T01:52:57.975Z",
      "updatedAt": "2012-04-30T01:52:57.975Z",
      "objectId": "sGlvypFQcO"
    }
  ]
}
```
_ _ _
## Files

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/files/:fileName | POST | Uploading Files |
| /1/files/:fileName | DELETE | Deleting Files |

_ _ _
### Uploading Files
| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/files/:fileName | POST | Uploading Files |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Application-Id|fWmCni6GmXiBs3Twx6j9cOPNaZWt7P6vnS4vHnDU|
|X-Noserv-REST-API-Key|o3ifa8izHAhhcj2cHXjUrZtWnZhJKn3QJIn6x2RF|
|Content-Type|image/jpeg|

* Content-Type에 파일 형식에 맞는 타입을 넣고, Binary Data를 body에 전송. 정해지지 않은 타입일 경우 octet-stream으로 세팅.
* 일반 웹의 multipart-post 형식도 사용 가능.
* 파일당 10MByte 제한.


##### response
==Success==

Status: 201 Created
url, name이 항상 들어온다.
```json
{
  "url": "http://noserv.com/files/tfss-db295fb2-8a8b-49f3-aad3-dd911142f64f-hello.txt",
  "name": "db295fb2-8a8b-49f3-aad3-dd911142f64f-hello.txt"
}
```

### Deleting Files
삭제에는 Master Key가 필요하다.

| URL | HTTP Verb | Functionality |
|--------|--------|--------|
| /1/files/:fileName | DELETE | Deleting Files |

##### request
| Header | Sample Value |
|------|------|
|X-Noserv-Application-Id|fWmCni6GmXiBs3Twx6j9cOPNaZWt7P6vnS4vHnDU|
|X-Noserv-Master-Key|o3ifa8izHAhhcj2cHXjUrZtWnZhJKn3QJIn6x2RF|

##### response
==Success==
리턴 데이터 없음.
