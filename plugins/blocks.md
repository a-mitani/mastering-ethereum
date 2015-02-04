# Extend blocks

Extending templating blocks is the best way to provide extra functionnalities to authors.

The most common usage is to process the content within some tags at runtime. It's like [filters](./filters.md), but on steroids because you aren't confined to a single expression.

### Define a new block

Blocks are defined by the plugin, blocks is a map of name associated with a block descriptor. The block descriptor needs to contain at least a `process` method.

```
module.exports = {
    blocks: {
        tag1: {
            process: function(block) {
                return "Hello "+block.body+", How are you?";
            }
        }
    }
};
```

The `process` should return the html content that will replace the tag.

### Handling block arguments

Arguments can be passed to blocks:

```
{% tag1 "argument 1", "argument 2", name="Test" %}
This is the body of the block.
{% endtag1 %}
```

And arguments are easily accessible in the `process` method:

```
module.exports = {
    blocks: {
        tag1: {
            process: function(block) {
                // block.args equals ["argument 1", "argument 2"]
                // block.kwargs equals { "name": "Test" }
            }
        }
    }
};
```
