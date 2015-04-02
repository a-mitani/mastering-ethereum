# Hooks

Hooks is a method of augmenting or altering the behavior of the process, with custom callbacks.

### List of hooks

### Relative to the global pipeline

> It is recommended using [blocks](./blocks.md) to extend page parsing.

| Name | Description | Arguments |
| ---- | ----------- | --------- |
| `init` | Called after parsing the book, before generating output and pages. | None |
| `finish:before` | Called after generating the pages, before copying assets, cover, ... | None |
| `finish` | Called after everything else. | None |

### Relative to the page pipeline

| Name | Description | Arguments |
| ---- | ----------- | --------- |
| `page:before` | Called before running the templating engine on the page | Page Object |
| `page` | Called before outputting and indexing the page. | Page Object |

##### Page Object

```js
{
    // Parser named
    "type": "markdown",
    
    // File Path relative to book root
    "path": "page.md",
    
    // Absolute file path
    "rawpath": "/usr/...",
    
    // Progress of the page in summary
    "progress": {
        ...
    },
    
    // Page Content (available in "page:before")
    "content": "# Hello",
    
    // Page Content splited in sections (available in "page")
    "sections": [
        {
            "content": "<h1>Hello</h1>",
            "type": "normal"
        }
    ]
}
```

##### Example to add a title

```js
{
    "page:before": function(page) {
        page.content = "# Title\n" +page.content;
        return page;
    }
}
```

##### Example to replace some html

```js
{
    "page": function(page) {
        page.sections[0].content = page.sections[0].content.replace("<b>", "<strong>")
            .replace("</b>", "</strong>");
        return page;
    }
}
```


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