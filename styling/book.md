# Extend website and ebook style

You can extend css used in the ebooks and the website by creating files in the `styles` directory:

| Type | File |
| ---- | ---- |
| `website` | `styles/website.css` |
| `pdf` | `styles/pdf.css` |
| `epub` | `styles/epub.css` |
| `mobi` | `styles/mobi.css` |
| `pdf`, `epub`, `mobi` | `styles/ebook.css` |


These paths can be changed in the `book.json` configuration, for example to use file in the root folder:

```json
{
    "styles": {
        "website": "website.css",
        "ebook": "ebook.css",
        "pdf": "pdf.css",
        "mobi": "mobi.css",
        "epub": "epub.css"
    }
}
```