# Readme and Introduction

The first page of your book is extracted from the `README.md`. If the file is not present in the `SUMMARY`, it will be added as a first entry.

### Use another file than README.md

Some books hosted on GitHub prefer to keep the README.md as a project introduction instead of a book introduction.

Since GitBook `>2.0.0`, it's possible to define which file to use as a README in the `book.json`:

```
{
    "structure": {
        "readme": "myIntro.md"
    }
}
```
