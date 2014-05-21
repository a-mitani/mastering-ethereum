# Custom Domains

All books on **Gitbook.io** are accessible via the a page on **www.gitbook.io**.

But you can configure your book to use a custom domain name (a free feature on GitBook.io).

The process to add a custom domain to you book is easy.

1. Add your domain name in your book settings.

In order to use your own domain, you will need to make changes with your domain registrar:

1. Log in to your domain registrar and find the section that allows you to add/edit host records, often found in a settings menu called 'Edit DNS', 'Host Records' or 'Zone File Control'.

2. Set the www record to a CNAME and set the URL field to: ```www.gitbook.io```.

3. To redirect the naked domain (`yourdomain.com`) to `www.yourdomain.com`, find the option to forward your domain. This can usually be found under 'Forwarding', 'URL Forwarding' or 'URL Redirect'.


It may take a few hours for domain changes to propagate. To check if it's ready or to setup your custom domain with GitBook, enter your domain name (including the www) below:
