# API

{% include "./message.md" %}

#### Schema

All API access is over HTTPS, and accessed through `www.gitbook.com/api/` . All data is sent and received as **JSON**.


#### Authentication

There is currently only one way to authenticate through GitBook API. Requests that require authentication will return `404 Not Found`, instead of `403 Forbidden`, in some places. This is to prevent the accidental leakage of private books to unauthorized users.

###### Basic Authentication

```
$ curl -u "username:token" https://www.gitbook.com/api/books/
```

#### Error Format

Error are returned as JSON: 

```js
{
    "error": "Not found",
    "code": 404
}
```

#### Pagination

###### Parameters

| Name | Type | Description |
| ---- | ---- |------------ |
| skip | int | Number of items to skip |
| limit | int | Number of items to list |

###### Return

```js
{
    list: [],
    skip: 0,
    limit: 100,
    total: 0
}
```

