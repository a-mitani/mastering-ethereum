# Create a plugin

A GitBook plugin is a node package published on NPM that follow a defined convention.

### package.json

The `package.json` contains general informations about your plugin (name, version, description, ...):

```
{
    "name": "gitbook-plugin-mytest",
    "version": "0.0.1",
    "description": "This is my first GitBook plugin",
    "engines": {
        "gitbook": ">1.x.x"
    }
}
```

You can learn more about `package.json` from the [NPM documentation](https://docs.npmjs.com/files/package.json).


