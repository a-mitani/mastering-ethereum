# Rousseau

{% include "./message.md" %}

Rousseau provides an API to easily proofready and spellcheck texts. The proofreading API is built on top of the open source utility [Rousseau](https://github.com/GitbookIO/rousseau). The spellchecker is using the Hunspell dictionaries.

The API is accessible at `https://rousseau.gitbook.com`.


### Proofread a text

```
POST https://rousseau.gitbook.com/document

{ "document": "Hello World" }
```

### Spellcheck a list of words

```
POST https://rousseau.gitbook.com/words

{ "words": ["Hello", "World"] }
```
