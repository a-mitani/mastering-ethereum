# What is GitBook?

GitBook is a tool for building beautiful books using Git and Markdown.

### Format

GitBook uses a convention on top of markdown files.

A book is a Git repository containing at least 2 files: README.md and SUMMARY.md.

##### README.md

As usual, it should contains an introduction for your book. It will be automatically added to the final summary.

##### SUMMARY.md

The SUMMARY.md defines your book's structure. It should contain a list of chapters, linking to their respective pages.

Example:

```
# Summary

This is the summary of my book.

* [section 1](section1/README.md)
    * [example 1](section1/example1.md)
    * [example 2](section1/example2.md)
* [section 2](section2/README.md)
    * [example 1](section2/example1.md)
```

Files that are not included in the SUMMARY.md will not be processed by gitbook.


### Output Formats

GitBook can generate your book into multiple formats:

* **Static Website**: This is the default format, it generates a complete interactive static website.
* **PDF**: A complete PDF book with exercise solutions at the end of the book.
* **eBook**: A complete eBook with exercise solutions at the end of the book.
