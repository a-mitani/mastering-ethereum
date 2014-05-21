# Common Errors

Here is a list of common buidl errors and there associated fixed.

---------

```
Error loading plugins: plugin1, ...
```

This error is happening because Gitbook can't resolve a plugin (or the plugin is invalid).
External plugins need to be specified in the `dependencies` field of a node.js `package.json` file. [Learn more about the package.json format](https://www.npmjs.org/doc/json.html).

For example, if your book depend on the **Autocover** plugin, you need a `package.json` file with the following content:

```json
{
    "name": "mybook",
    "version": "0.0.0",
    "description": "",
    "repository": {
        "type": "git",
        "url": "https://github.com/Me/mybook.git"
    },
    "author": "Me <me@gmail.com>",
    "dependencies": {
        "gitbook-plugin-autocover": "0.0.5"
    }
}
```

---------




