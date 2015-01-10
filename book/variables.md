# Variables and Import

Some macros can be used in the markdown source to import files or variables.

### Define variables

Variables are defined in the `book.json` file:

```json
{
    "variables": {
        "myVariable": "Hello World"   
    }
}
```

### Import variables

Variables can be imported in all markdown files using `{{ }}`.

For example to show the variable from the book.json (`myVariable`):

```
# This is a test: {{ myVariable }}

```

### Import files

In the same way, files