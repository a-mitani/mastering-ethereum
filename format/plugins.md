# Plugins

Plugins are the best way to extend GitBook functionalities (ebook and website). There exist plugins to do a lot of things: bring math formulas display support, track visits using Google Analytic, ...

### How to find plugins?

Plugins can be easily searched on [plugins.gitbook.com](http://plugins.gitbook.com).

### How to install a plugin?

Once you find a plugin that you want to install, you need to add it to your `book.json`:

```
{
	"plugins": ["myPlugin", "anotherPlugin"]
}
```

You can also specify a specific version using: `"myPlugin@0.3.1"`, this is usefull when you're using an outdated version of GitBook.

If you're building your book locally, download and prepare plugins simply by running: `gitbook install`.



