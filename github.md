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

If your modifications on GitHub are not available on GitBook, the main source of the problem is the webhook. You can check the state of your webshook in your GitHub repository settings.


## Link a book and a GitHub repository

When your GitHub account is correctly linked to your GitBook account, linking a book to a repository is easy.

**Caution:** When a GitHub repository is specified in your book settings, it will prioritized to the git repository on GitBook, this means that the editor will edit content on GitHub directly.

**The sync is unidirectional**, only modifications on GitHub will trigger builds on gitBook, GitBook **will not** update your GitHub repository content with the content of the book before the integration.

1. Open the GitHub section in your book settings
2. Enter your repository id (for example: `YourGitHubUserName/RepoName`)
3. Save your settings
4. Click on the button **Add a deployment webhook**

You can now edit your GitHub repository from the web editor (if you have authorized the correct permissions), and your commit on GitHub will trigger builds on GitBook

## Common Errors:

##### My Book is not being updated / No build is triggered

If you linked your GitHub repository to a GitBook but no build is triggered when editing content.
Verify that the webhook has been correctly added to your GitHub repository (in your GitHub repository settings -> Webhook). If the webhook is not present or invalid, add it back fom yoru GitBook book settings.

##### Change of book owner

If you transferred your book to a new owner, the webhook is now invalid, you need to add it back.




