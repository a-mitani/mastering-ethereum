# Update your book using GIT

When your book is created on **gitbook.com**, you need to push some content to it. To do so, you can use the web editor or the command line.

If you want to update your book from the command line, you can use [GIT](http://git-scm.com) to push your content:

### GIT Url

Each book is associated with a Git HTTPS url. The ssh protocol is not yet supported on the GitBook's git server.

The format for the git url is:

```
https://git.gitbook.com/{{UserName}}/{{Book}}.git
```

### Authentication

The git server is using your basic GitBook login to authenticate you. When prompted enter your GitBook username and your password (you can also use your API token).

### Create a new repository on the command line

```
touch README.md SUMMARY.md
git init
git add README.md SUMMARY.md
git commit -m "first commit"
git remote add gitbook https://git.gitbook.com/{{UserName}}/{{Book}}.git
git push -u gitbook master
```

### Push an existing repository

```
git remote add gitbook https://git.gitbook.com/{{UserName}}/{{Book}}.git
git push -u gitbook master
```


