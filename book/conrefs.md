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

The format of git url is:

```
git+https://user@hostname/project/blah.git/file#commit-ish
```

The real git url part should finish with `.git`, the filename to import is extracted after the `.git` till the fragment of the url.

The `commit-ish` can be any tag, sha, or branch which can be supplied as an argument to `git checkout`. The default is `master`.

### Inheritance

An other way to reference content is to inherit from an another file:

```
{% extends "./mypage.md" %}

{% block pageContent %}
# This is my page content
{% endblock %}
```

In the file `mypage.md`, you should specify the blocks that can be extent:

```
{% block pageContent %}
This is the default content
{% endblock %}

# License

{% import "./LICENSE" %}
```





