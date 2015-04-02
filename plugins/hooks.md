# Hooks

Hooks is a method of augmenting or altering the behavior of the process, with custom callbacks.

### List of hooks

### Relative to the global pipeline

| Name | Description | Arguments |
| ---- | ----------- | --------- |
| `init` | Called after parsing the book, before generating output and pages. | None |
| `finish:before` | Called after generating the pages, before copying assets, cover, ... | None |
| `finish` | Called after everything else. | None |

### Relative to the page pipeline

| Name | Description | Arguments |
| ---- | ----------- | --------- |
| `page:before` | Called  | Page Object |
| `page` | Called after everything else. | Page Object |


### Asynchronous Operations

Hooks callbacks can be asynchronous and return a promise.

Example:

```js
{
    "init": function() {
        return writeSomeFile()
        .then(function() {
            return writeAnotherFile();
        });
    }
}
```