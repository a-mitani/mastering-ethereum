# Content References

Content referencing (conref) is a convenient mechanism for reuse of content from other files or books.

### Importing local files

Importing an other file's content is really easy using the `include` tag:

```
{% include "./test.md" %}
```

### Importing file from an other book

GitBook can also resolve the include path by using git:

```
{% include "git+https://github.com/GitbookIO/documentation.git/README.md#0.0.1" %}
```