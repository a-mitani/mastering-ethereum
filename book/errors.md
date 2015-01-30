# Common Errors

Here is a list of common build errors and their associated fixes.

---------

```
Error loading plugins: plugin1, ...
```

This error happens because GitBook can't resolve a plugin (or the plugin is invalid).
External plugins need to be installed using `gitbook install`.

It's also possible that some plugins can't be loaded because they need another version of GitBook. In this case, you need to find the correct version of your plugin that support the GitBook version you are using and define it in your `book.json`:

```
{
    "plugins": ["myPlugin@1.2.3"]
}
```

---------

```
Need to install ebook-convert from Calibre
```


To get around the error while trying to build your project as a PDF, ePub or mobi ebook, you must have the [Calibre](http://calibre-ebook.com) eBook reader/manager installed _AND_ the command-line tools installed.

To install the Calibre command-line tools from the Mac version, from the menu select: **calibre - Preferences - Miscellaneous - Install command line tools**

Once this is completed, your ebook builds will be successful.

---------
