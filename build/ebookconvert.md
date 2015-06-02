### Installing ebook-convert

ebook-convert is required to generate ebooks (epub, mobi, pdf).

#### Mac

Download the [Calibre app](http://calibre-ebook.com/download). After moving the `calibre.app` to your Applications folder create a symbolic link to the `ebook-convert` tool:

```
$ sudo ln -s ~/Applications/calibre.app/Contents/MacOS/ebook-convert /usr/bin
```

You can replace `/usr/bin` with any directory that is in your `$PATH`.