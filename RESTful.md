# RESTful API documentation
by siriuskoan

## bot
Header should contain `Authorization: Bearer <user token>`.
### /status
`GET /api/status`
Get simplified status

**request**
```
{}
```

**response**
```
{
    {
        "IP": "10.1.1.1",
        "status": True
    },
    {
        "IP": "10.1.1.2",
        "status": False
    }
}
```

`GET /api/status?mode=verbose`
Get verbose status

**request**
```
{}
```

**response**
```
{
    {
        "IP": "10.1.1.1"
        "status": True,
        "active": True,
        "uptime": "86400",
        "reconnect_times": "5",
        "role": "client",
    },
    {
        "IP": "10.1.1.2",
        "status": True,
        "active": False,
        "uptime": "1234",
        "reconnect_times": "10",
        "role": "host"
    }
}
```

notes:
 - The `reconnect_times` is represented in second.
 - If the `reconnect_times` field is zero and the client is disconnected, it means the client disconnect by himself or herself.

## client
Header should contain `Authorization: Bearer <user token>` except for `/login`
### /login
`POST /api/login`

**request**
```
{
	"password": "123456",
	"key": "<ssh public key>"
}
```

**response**
```
{
	"status": "OK",
	"token": "<user token>"
}
```

```
{
	"status": "Failed",
	"message": "Wrong password"
}
```
The error message can be:
 - Wrong password (*from backend server*)

### /connect
`GET /api/connect`
Get connection status.

**request**
```
{}
```

**response**
```
{"status": "OK"}
```

```
{"status": "Failed", "message": "Invalid token."}
```
The error messages can be:
 - Invalid token. (*from backend server*)
 - Server does not allow new connection now. (*from backend server*)
 - No resources are available. (*from ssh tunnel lib*)

`POST /api/connect`
Tell the server you are going to connect to ssh tunnel.

**request**
```
{}
```

**response**
```
{"status": "OK"}
```

```
{"status": "Failed", "message": "Invalid token."}
```

The error messages are the same as `GET`.

`DELETE /api/connect`
Tell the server you are disconnecting from the ssh tunnel.

**request**
```
{}
```

**response**
The response is useless.
```
{"status": "OK"}
```

`PUT /api/connect`
Tell the server you are reconnecting to the ssh tunnel.

**request**
```
{}
```

**response**
```
{"status": "OK"}
```

```
{"status": "Failed", "message": "Invalid token."}
```

The error messages are the same as `GET`.

### /host
Header should contain `Authorization: Bearer <user token>`

`POST /api/host`
Tell the server you want to be the host, if the host has retired, you can be the new host.

**request**
```
{}
```

**response**
```
{"status": "OK"}
```

```
{"status": "Failed", "message": "Host is some one else now."}
```

`DELETE /api/host`
Tell the server you don't want to be the host and allow others to be the new host.

**request**
```
{}
```

**response**
```
{"status": "OK"}
```

```
{"status": "Failed", "message": "You are not host."}
```
