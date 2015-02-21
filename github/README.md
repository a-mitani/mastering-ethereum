# GitHub Integration

GitBook integrates perfectly with [GitHub](https://github.com) as a host for your book's source.

## Connect your account / Permissions

Before integrating your book with GitHub, you need to authorize GitBook to get access to your GitHub account.

In your [account settings](https://www.gitbook.com/settings), connect your GitHub account with the correct permissions:

- **Default permissions**: only access your GitHub account to authenticate you during login
- **Access to webhook**: access your GitHub account to create a webhook on your specified repository (See [webhooks](#webhooks)).
- **Access to public repositories**: access your GitHub repository from the webeditor so that you can edit your book easily from GitBook (only public repositories)
- **Access to private repositories**: same as above but with access to private repositories

## Webhooks

Webhooks notify GitBook when your GitHub repository changes.

If your GitHub changes are not available on GitBook, the main source of the problem is the webhook. You can check the state of your webhook in your repository's settings.


## Link a book and a GitHub repository

When your GitHub account is correctly linked to your GitBook account, linking a book to a repository is easy.

**Caution:** When you specify a GitHub repository in your book's settings, it will take priority over GitBook's git repository, this means that the editor will directly edit content on GitHub.

**The sync is unidirectional**, only changes made on GitHub will trigger builds on GitBook, GitBook **will not** update your GitHub repository with any content written before.

1. Open the GitHub section in your book settings
2. Enter your repository id (such as: `YourGitHubUserName/RepoName`)
3. Save your settings
4. Click on the button **Add a deployment webhook**

You can now edit your GitHub repository from the web editor (if you have authorized the correct permissions), and your commit on GitHub will trigger builds on GitBook

## Common Errors:

##### My Book is not being updated / I can't see any builds

If you linked your GitHub repository to a GitBook but editing content doesn't trigger any build.
Verify that the webhook has been correctly added to your GitHub repository (in your GitHub repository settings -> Webhook). If the webhook is not present or invalid, add it back from your book's settings.

##### Change of book owner

If you transferred your book to a new owner, the webhook is now invalid, you need to add it back.
