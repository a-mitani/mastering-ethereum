# Format

GitBook uses a convention on top of markdown files.

A book is a Git repository containing at least 2 files: README.md and SUMMARY.md.

#### README.md

Typically, this should be the introduction for your book. It will be automatically added to the final summary.

#### SUMMARY.md

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

Files that are not included in SUMMARY.md will not be processed by gitbook.


#### Multi-Languages

GitBook supports building books written in multiple languages. Each language should be a sub-directory following the normal GitBook format, and a file named `LANGS.md` should be present at the root of the repository with the following format:

```
* [English](en/)
* [French](fr/)
* [Español](es/)
```

You can see a complete example with the [Learn Git](https://github.com/GitbookIO/git) book.

#### Ignoring files & folders

GitBook will read the `.gitignore`, `.bookignore` and `.ignore` files to get a list of files and folders to skip. (The format inside those files, follows the same convention as `.gitignore`)
