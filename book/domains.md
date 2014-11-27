# Custom Domains

All books on **Gitbook.io** are accessible via the url **http://{{author}}.gitbooks.io/{book}/**, and content is accessible at **http://{{author}}.gitbooks.io/{book}/content/**.

But you can configure your book to use a custom domain name (a free feature on GitBook). Domain name can be use for the homepage and the content (or both).

The process to add a custom domain to your book is easy.

1. Add your domain name in your book settings.

In order to use your own domain, you will need to make a few changes with your domain registrar:

1. Log in to your domain registrar and find the section that allows you to add/edit host records, often found in a settings menu called 'Edit DNS', 'Host Records' or 'Zone File Control'.

2. Set the www record to a CNAME and set the URL field to: ```www.gitbook.com```.

3. To redirect the naked domain (`yourdomain.com`) to `www.yourdomain.com`, find the option to forward your domain. This can usually be found under 'Forwarding', 'URL Forwarding' or 'URL Redirect'.


It may take a few hours for domain changes to propagate.
