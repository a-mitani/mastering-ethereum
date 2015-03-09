# Configuration

All configurations are stored as JSON in a file named `book.json`.

You can paste your `book.json` at [jsonlint.com](http://jsonlint.com) to validate the JSON syntax.

All fields are optionals or default to some extracted values.


## Fields

#### title

```
{ "title": "My Awesome Book" }
```

This option defines the title of your book, by default this value is extracted from the **README** (first title).

On **gitbook.com**, this value is defined from the title entered on the platform.

#### description

```
{ "description": "This is such a great book!" }
```

This option defines the description of your book, by default this value is extracted from the **README** (first paragraph).

On **gitbook.com**, this value is defined from the description entered on the platform.

#### isbn

```
{ "isbn": "978-3-16-148410-0" }
```

This option defines the ISBN associated with your book 

