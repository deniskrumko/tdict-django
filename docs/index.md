# API documentation

Authorization:
- [POST /api/token/](#post-apitoken)

Dictionary:
- [GET /api/words/](#get-apiwords)
- [POST /api/words/](#post-apiwords)
- [GET /api/words/\<word_id\>/](#get-apiwordsword_id)
- [PATCH /api/words/\<word_id\>/](#patch-apiwordsword_id)
- [DELETE /api/words/\<word_id\>/](#delete-apiwordsword_id)

## POST /api/token/

Get auth token for using private API. Redeemed token used for `Authorization` header with value like
`Token d210ef6e0dea94adff006808f3e5b8e4b689b28e`.

**Request**
```
{
    "username": "deniskrumko",
    "password": "my cool password"
}
```

**Response (HTTP 200)** - token for auth
```
{
    "token": "d210ef6e0dea94adff006808f3e5b8e4b689b28e"
}
```

**Response (HTTP 400)**
```
{
    "non_field_errors": [
        "Unable to log in with provided credentials."
    ]
}
```

## GET /api/words/

Get paginated response with own created words.

Requires `Authorization` header.

**Query params**:
- `?q=` — search words by `word_from`, `word_to` and `description` fields
- `?language_from=` — filter by source word language ("RU", "EN")
- `?language_to=` — filter by translated word language ("RU", "EN")
- `?page=` — for pagination. By default is page 1

**Response (HTTP 200)**
```
{
    "count": 10,
    "next": "http://127.0.0.1:8000/api/words/?page=2",
    "previous": null,
    "results": [
        {
            "id": 114,
            "word_from": "lizard",
            "word_to": "ящерица",
            "language_from": "EN",
            "language_to": "RU",
            "description": null,
            "source": null,
        }
    ]
}
```

**Response (HTTP 401)** - no token provided
```
{
    "detail": "Authentication credentials were not provided."
}
```

## POST /api/words/

Create new word translation.

Requires `Authorization` header.

**Request**:
```
{
    "word_from": "lizard",
    "word_to": "ящерица",
    "language_from": "EN",
    "language_to": "RU",
    "description": "Description",
    "source": "Source"
}
```

**Response (HTTP 201)**
```
{
    "id": 114,
    "word_from": "lizard",
    "word_to": "ящерица",
    "language_from": "EN",
    "language_to": "RU",
    "description": "Description",
    "source": "Source"
}
```

**Response (HTTP 400)** - translation already exists
```
{
    "non_field_errors": [
        "The fields word_from, word_to must make a unique set."
    ]
}
```

**Response (HTTP 401)** - no token provided
```
{
    "detail": "Authentication credentials were not provided."
}
```

## GET /api/words/\<word_id\>/

Get single word by ID.

Requires `Authorization` header.

**Response (HTTP 200)**
```
{
    "id": 114,
    "word_from": "lizard",
    "word_to": "ящерица",
    "language_from": "EN",
    "language_to": "RU",
    "description": null,
    "source": null
}
```

**Response (HTTP 401)** - no token provided
```
{
    "detail": "Authentication credentials were not provided."
}
```

**Response (HTTP 404)**
```
{
    "detail": "Not found."
}
```

## PATCH /api/words/\<word_id\>/

Update single word by ID.

Requires `Authorization` header.

**Request**
```
{
    "word_from": "lizard",
    "word_to": "ящерица",
    "language_from": "EN",
    "language_to": "RU",
    "description": null,
    "source": null
}
```

Request can contain all fields or only those fields that needed to be updated. Like this:
```
{
    "description": "New description",
}
```

**Response (HTTP 200)**
```
{
    "id": 114,
    "word_from": "lizard",
    "word_to": "ящерица",
    "language_from": "EN",
    "language_to": "RU",
    "description": null,
    "source": null
}
```

**Response (HTTP 401)** - no token provided
```
{
    "detail": "Authentication credentials were not provided."
}
```

**Response (HTTP 404)**
```
{
    "detail": "Not found."
}
```

## DELETE /api/words/\<word_id\>/

Delete single word by ID.

Requires `Authorization` header.

**Response (HTTP 204)** - word successfully deleted
```
# response has empty body
```

**Response (HTTP 401)** - no token provided
```
{
    "detail": "Authentication credentials were not provided."
}
```

**Response (HTTP 404)**
```
{
    "detail": "Not found."
}
```
