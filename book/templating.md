# Templating

This is an overview of the templating features available in GitBook. GitBook uses the Nunjucks and Jinga2 syntax.

### Variables

A variable looks up a value from the book context.

Variables are defined in the `book.json` file:

```json
{
    "variables": {
        "myVariable": "Hello World"   
    }
}
```


#### Display Variables
```
{{ book.myVariable }}
```

This looks up `myVariable` from the book variables and displays it. Variable names can have dots in them which lookup properties. You can also use the square bracket syntax.

```
{{ book.foo.bar }}
{{ book["bar"] }}
```

If a value is undefined, nothing is displayed. The following all output nothing if `foo` is undefined: `{{ foo }}`, `{{ foo.bar }}`, `{{ foo.bar.baz }}`.


#### Context variables

Some variables are also available to get informations about the current file or the GitBook instance:

| Name | Description |
| ---- | ----------- |
| `file.path` | Path of the file relative to the book |
| `file.mtime` | |








