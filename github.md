# GitHub Integration

GitBook can integrates perfectly with GitHub, so that your book sources can be hosted on it.

## Connect your account / Permissions

Before integrating your book with GitHub, you need to authorize GitBook to access some informations from your GitHub account.

In your [account settings](https://www.gitbook.com/settings), connect your GitHub account with the correct permissions:

- **Default permissions**: only access your GitHub account to identify you during login
- **Access to webhook**: access your GitHub account to create a webhook on your specified repository (See [webhooks](#webhooks)).
- **Access to public repositories**: access your GitHub repository from the webeditor so that you can edit your book easily from GitBook (only public repositories)
- **Access to private repositories**: same as before but with access to private repositories

## Webhooks

Webhooks provide a way for notifications to be delivered to GitBook whenever your GitHub repository changed.

