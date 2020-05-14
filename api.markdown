---
title: API
nav_order: 100
---

# Webhook.site API
{: .no_toc }

The Webhook.site API is public, free to use, doesn't require authentication and is relatively easy to use. 
Please note that fair use guidelines and other limitations apply as described by the [Terms of Service](https://webhook.site/terms).

At its core, Webhook.site takes your data (HTTP requests) and returns it back to you, and also letting you execute various actions based on the contents with [Custom Actions](/custom-actions.html).

Base URL: `https://webhook.site`.

We recommend that you set the Accept header to `application/json`.

1. TOC
{:toc}

## Authentication

While most functions in the API work without any authentication whatsoever, some endpoints do require authentication, or will return a `401 Unauthorized` status code.

### API Key

An API Key can be generated in the Control Panel, and provides access to Tokens that are either a) password protected or b) require login.

To specify an API Key in a request, use a HTTP header: `Api-Key: [your API Key]`

### Password

If you have enabled password authentication on a Webhook.site URL/Token, you can use the `?password=[your password]` query string.

## Tokens

A **token** is a container for incoming requests and emails, and corresponds to a Webhook.site URL or Email. A **token ID** is a 36 character UUID consisting of hexadecimal characters and dashes.

Simply, the token ID is the part after `https://webhook.site/` in the URL, or before `@email.webhook.site` in the email address.

### Create token

**POST** `/token`

After creating your token, the URL at `https://webhook.site/{token.uuid}` becomes accessible.

#### Request

```json
{
  "default_status": 200,
  "default_content": "Hello world!",
  "default_content_type": "text/html",
  "timeout": 0
}
```

#### Response

`200 OK`

```json
{
  "redirect": false,
  "alias": null,
  "timeout": 0,
  "premium": true,
  "uuid": "9981f9f4-657a-4ebf-be7c-1915bedd4775",
  "ip": "127.0.0.1",
  "user_agent": "Paw\/3.1.8 (Macintosh; OS X\/10.14.6) GCDHTTPRequest",
  "default_content": "Hello world!",
  "default_status": 200,
  "default_content_type": "text\/plain",
  "premium_expires_at": "2019-10-22 10:52:20",
  "created_at": "2019-09-22 10:52:20",
  "updated_at": "2019-09-22 10:52:20"
}
```

Note about expiry: If there's no incoming requests for about a week, and the token is not upgraded to premium, the token is automatically deleted along with any other data.

### Update token

* Can require authentication.

**PUT** `/token/:token_id`

#### Request

[*See **POST** `/token`*](#11-create-token)

#### Response

[*See **POST** `/token`*](#11-create-token)

### Set password (Premium)

* Can require authentication.
* Requires user with Premium upgrade.

**PUT** `/token/:token_id/password`

Sets a password to view the requests of a token.

#### Request

```json
{"password": "hunter2", "old_password": "hunter1"}
```

#### Response

[*See **POST** `/token`*](#11-create-token)

### Set alias (Premium)

* Can require authentication.
* Requires user with Premium upgrade.

**PUT** `/token/:token_id/alias`

Sets the alias for the token. (Can be used when creating requests.)

#### Request

```json
{"alias": "my-webhook"}
```

#### Response

[*See **POST** `/token`*](#11-create-token)

### Toggle CORS

Attaches CORS headers to the response of the Token, allowing browsers to request it from all domains.

**PUT** `/token/:token_id/cors/toggle`

#### Response

```json
{ "enabled": true}
```

### Get token

* Can require authentication.

**GET** `/token/:token_id`

#### Response

[*See **POST** `/token`*](#11-create-token)

### Delete token 

* Can require authentication.

**DELETE** `/token/:token_id`

#### Response

`204 No Content`

## Requests

### Create request

***(any method)*** `/:tokenId` <br>
***(any method)*** `/:tokenId/:statusCode` <br>
***(any method)*** `/:tokenId/(anything)`

If `statusCode` is valid, that HTTP status will be used in the response (instead of the default.)

Instead of `tokenId`, an alias can also be supplied.

#### Request

*(Anything.)*

#### Response

*(The default response of the Token.)*

### Get requests

* Can require authentication.

**GET** `/token/:token_id/requests`

Lists all request sent to a token. 

#### Response

```json
{
  "data": [
    {
      "uuid": "a2a6a4ae-4130-4063-953a-84fa29d81d43",
      "token_id": "a94a7294-c4aa-4074-ab77-c4cf86fd53b1",
      "ip": "127.0.0.1",
      "hostname": "webhook.site",
      "method": "POST",
      "user_agent": "Paw\/3.1.8 (Macintosh; OS X\/10.14.6) GCDHTTPRequest",
      "content": "{\"first_name\":\"Arch\",\"last_name\":\"Weber\"}",
      "query": {
        "action": "create"
      },
      "headers": {
        "content-length": [
          "271"
        ],
        "user-agent": [
          "Paw\/3.1.8 (Macintosh; OS X\/10.14.6) GCDHTTPRequest"
        ]
      },
      "url": "https:\/\/webhook.site\/a94a7294-c4aa-4074-ab77-c4cf86fd53b1\/201?",
      "created_at": "2019-10-03 19:06:35",
      "updated_at": "2019-10-03 19:06:35"
    }
  ],
  "total": 1,
  "per_page": 50,
  "current_page": 1,
  "is_last_page": true,
  "from": 1,
  "to": 1
}
```

### Get single request

* Can require authentication.

**GET** `/token/:token_id/request/:request_id`

#### Response

```json
{
  "uuid": "a2a6a4ae-4130-4063-953a-84fa29d81d43",
  "token_id": "a94a7294-c4aa-4074-ab77-c4cf86fd53b1",
  "ip": "127.0.0.1",
  "hostname": "webhook.site",
  "method": "POST",
  "user_agent": "Paw\/3.1.8 (Macintosh; OS X\/10.14.6) GCDHTTPRequest",
  "content": "{\"first_name\":\"Arch\",\"last_name\":\"Weber\"}",
  "query": {
    "action": "create"
  },
  "headers": {
    "content-length": [
      "271"
    ],
    "user-agent": [
      "Paw\/3.1.8 (Macintosh; OS X\/10.14.6) GCDHTTPRequest"
    ]
  },
  "url": "https:\/\/webhook.site\/a94a7294-c4aa-4074-ab77-c4cf86fd53b1\/201?",
  "created_at": "2019-10-03 19:06:35",
  "updated_at": "2019-10-03 19:06:35"
}
```

### Get raw request content

* Can require authentication.

**GET** `/token/:token_id/request/:request_id/raw`

Returns the request as a response (body, content-type.)

### Delete request

* Can require authentication.

**DELETE** `/token/:token_id/request/(:request_id)`

Deletes a request. 

If no ID, all requests related to the token will be deleted.

#### Response

`204 No Content`
