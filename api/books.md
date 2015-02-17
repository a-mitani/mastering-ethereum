# Books

{% include "./message.md" %}

### List your books

List books for the authenticated user. This includes books from organizations the user can access.

```
GET /books/
```

You can include only books created by the authenticated user:

```
GET /books/author
```

#### List all public books

This provides a dump of every public repository, in the order that they were created.

```
GET /books/all
```


