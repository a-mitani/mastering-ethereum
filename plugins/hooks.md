# Hooks

Hooks is a method of augmenting or altering the behavior of the process, with custom callbacks.

### List of hooks

| Name | Description | Arguments |
| ---- | ----------- | --------- |
| `init` | 



### Asynchronous

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