# Chat-App

## Lastenheft

### 1. Authentifizierung
- [ ] Ein Nutzer muss sich registrieren können
- [ ] Ein Nutzer muss sich anmelden können

### 2. User API
- [ ] Ein Nutzer muss andere Nutzer mit dem Username suchen können
- [ ] Ein Nutzer kann seinen Account löschen



### 2. Channel
- [ ] Ein Nutzer muss einen neuen Channel erstellen können, er ist diesem automatisch als Admin zugeordnet
- [ ] Es gibt zwei Arten von Channels PRIVATE und GROUP
- [ ] Ein Nutzer kann mehrere Channels zugeordnet sein
- [ ] Mehrere Nutzer können einem Channel zugeordnet sein
- [ ] Nur der Adminuser kann dem Channel neue Nutzer hinzufügen
- [ ] Nur der Adminuser kann Nutzer aus einem Channel entfernen
- [ ] Ein Nutzer kann den Channel verlassen

### 3. Message

- [ ] Ein Nutzer kann einen anderen Nutzer eine Nachricht schicken, dabei wird automatisch (falls noch nicht vorhanden) einen PRIVATE Channel erstellt
- [ ] Mehrere Nutzer können in einem Channel Nachrichten schicken
- [ ] Ein Nutzer kann die letzten 50 Nachrichten von einem Channel abrufen

> #### Info: Cookie mit jwt token wird automatisch beim erfolgreichen sign in gesetzt 

!!! note Pagination
* ##### Die Endpoints die mehrere Elemente zurückgeben, unterstützen pagination. 
z.B.
``` http 
GET /channels?page=0&size=2
````
##### Response Body:
```js
{
    "_embedded": {
        "channelList": [
            {
                "id": 1,
                "topic": "testTopic",
                "users": [
                    {
                        "id": "1",
                        "username": "someusername"
                    },
                    {
                        "id": "2",
                        "username": "someusername2"
                    }
                ]
            }
        ]
    },
    "_links": {
        "self": {
            "href": "http://localhost:8080/channels"
        }
    },
    "page": {
        "size": 2,
        "totalElements": 1,
        "totalPages": 1,
        "number": 0
    }
}
``` 
!!!

## Auth API

### SignUp 
``` http
POST /signup
Request Body:
{
"username":"user1",
"password":"password1"
"confirmpassword":"password1"
}
```
> ##### Errors: -> Status: 400 -> bspws. :
> ```js
> {"error":"Nutzername schon vergeben"}

### SignIn
```http
POST /signin
Request Body:
{
"username":"user1",
"password":"password1"
}
Response Body:
````

``` js 
{"id":"1","username":"user1"}
```

## Channel API
### Retrieve one channel with id
``` http
GET /channels/{id}
Cookies: authToken="sampleToken"
Response Body:
```
``` js
{
    "id": "1",
    "topic": "someTopic",
    "users": [
        {
            "id": "1",
            "username": "someusername"
        },
        {
            "id": "2",
            "username": "seconduser"
        }
    ],
    "admin": {
        "id": "1",
        "username": "someusername"
    },
    "type": "group"
}


```


### Retrieve list of joined channels
``` http 
GET /channels 
Cookies: authToken="sampleToken"
Response Body:
````

``` js 
[
    {
        "id": "1",
        "topic": "someTopic",
        "users": [
            {
                "id": "1",
                "username": "someusername"
            },
            {
                "id": "2",
                "username": "seconduser"
            }
        ],
        "admin": {
            "id": "1",
            "username": "someusername"
        },
        "type": "group"
    },
    {
        "id": "2",
        "topic": "secondGroup",
        "users": [
            {
                "id": "1",
                "username": "someusername"
            },
            {
                "id": "2",
                "username": "seconduser"
            }
        ],
        "admin": {
            "id": "2",
            "username": "seconduser"
        },
        "type": "group"
    }
]
```
> wird in pagination objekt gewrappt
### Create new Channel, creator = adminUser
``` http
PUT /channels
Cookies: authToken="sampleToken"
Request Body: 
{
"topic":"sampleGroupTopic", 
"type":"group",
}
```

### Add user to existing channel (only admin)
``` http
PUT /channels/{id}/users/{id}
Cookies: authToken="sampleToken"
```

### Remove user from channel or leave channel
``` http
DELETE /channels/{id}/users/{id}
Cookies: authToken="sampleToken"
```
>##### Info: Admin can remove other users. Others can use this endpoint to leave the channel

### Retrieve list of channels by topic 
```http 
GET /channels?topic={topic}
```






## Message API
### Send Message To Channel

``` http
POST /channels/{id}/messages
Request Headers:
Cookies: authToken="sampleToken"
Request Body:
```
``` js
{"content":"hallo"}
```

### Retrieve messages of channel
```http
GET /channels/{id}/messages

Response Body:
```
```js
[
    {
        "id": "1",
        "content": "text",
        "author": {
            "id": "1",
            "username": "user1"
        }
    },
    {
        "id": "2",
        "content": "text",
        "author": {
            "id": "2",
            "username": "user2"
        }
    }
]
```

