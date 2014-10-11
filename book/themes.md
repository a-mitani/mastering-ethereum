# Themes

Themes can be use to customize your book homepage using preddefined or custom HTML template. A list of all published themes can be found at [www.gitbook.io/themes](https://www.gitbook.io/themes).

### Format

Themes on GitBook are using the [SWIG](http://paularmstrong.github.io/swig/docs/) syntax.

#### Book representation

| Variable | Type | Description |
| -------- | ---- | ----------- |
| `book.id` | `string` | Complete id of the book (ex: `Username/Test`) |
| `book.name` | `string` | Name of the book |
| `book.title` | `string` | Title of the book |
| `book.description` | `string` | Description of the book |
| `book.public` | `boolean` | False if the book is private |
| `book.price` | `number` | Price of the book (0 equals free) |
| `book.githubId` | `string` | ID of the book on GitHub (ex: `Username/Test`) |
| `book.categories` | `array[string]` | List of tags associated with this book |
| `book.cover.small` | `string` | Url of the cover (size small) |
| `book.cover.large` | `string` | Url of the cover (size large) |
| `book.urls.access` | `string` | Url to access the book homepage |
| `book.urls.homepage` | `string` | Url of the homepage (using custom domain) |
| `book.urls.read` | `string` | Url to read the book |
| `book.urls.reviews` | `string` | Url to the reviews page |
| `book.urls.subscribe` | `string` | Url to submit email subscriptions |
| `book.urls.download.epub` | `string` | Url download ebook (as EPUB) |
| `book.urls.download.mobi` | `string` | Url download ebook (as Mobi) |
| `book.urls.download.pdf` | `string` | Url download ebook (as PDF) |
