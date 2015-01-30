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


### Tags

Tags are special blocks that perform operations on sections of the template.

#### If

**if** tests a condition and lets you selectively display content. It behaves exactly as a programming language's if behaves.

````
{% if variable %}
  It is true
{% endif %}
```

If `variable` is defined and evaluates to true, "It is true" will be displayed. Otherwise, nothing will be.

You can specify alternate conditions with elif and else:

```
{% if hungry %}
  I am hungry
{% elif tired %}
  I am tired
{% else %}
  I am good!
{% endif %}
```

#### for

**for** iterates over arrays and dictionaries.

Let's consider your variables in the `book.json`: 
```
{
    "variables": {
        "authors": [
            { "name": "Samy" },
            { "name": "Aaron" }
        ]
    }
}
```

```
# Authors


{% for author in authors %}
  <li>{{ item.title }}</li>
{% endfor %}
</ul>
The above example lists all the posts using the title attribute of each item in the items array as the display value.




