---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-12"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# GitHub events source
{: #openwhisk_catalog_github}

The `/whisk.system/github` package offers a convenient way to use the [GitHub APIs](https://developer.github.com/).
{: shortdesc}

The package includes the following feed:

| Entity | Type | Parameters | Description |
| --- | --- | --- | --- |
| `/whisk.system/github` | package | username, repository, accessToken | Interact with the GitHub API |
| `/whisk.system/github/webhook` | feed | events, username, repository, accessToken | Fire trigger events on GitHub activity |

Creating a package binding with the `username`, `repository`, and `accessToken` values is suggested.  With binding, you don't need to specify the values each time that you use the feed in the package.

## Firing a trigger event with GitHub activity

The `/whisk.system/github/webhook` feed configures a service to fire a trigger when there is activity in a specified GitHub repository. The parameters are as follows:

- `username`: The user name of the GitHub repository.
- `repository`: The GitHub repository.
- `accessToken`: Your GitHub personal access token. When you [create your token](https://github.com/settings/tokens), be sure to select the **repo:status** and **public_repo** scopes. Also, make sure that you don't have any webhooks that are already defined for your repository.
- `events`: The [GitHub event type](https://developer.github.com/v3/activity/events/types/) of interest.

In the following example, a trigger is created that fires each time a new commit to a GitHub repository.

1. Generate a GitHub [personal access token](https://github.com/settings/tokens). The access token will be used in the next step.

2. Create a package binding that is configured for your GitHub repository and with your access token.
  ```
  ibmcloud fn package bind /whisk.system/github myGit \
    --param username myGitUser \
    --param repository myGitRepo \
    --param accessToken aaaaa1111a1a1a1a1a111111aaaaaa1111aa1a1a
  ```
  {: pre}

3. Create a trigger for the GitHub `push` event type by using your `myGit/webhook` feed.
  ```
  ibmcloud fn trigger create myGitTrigger --feed myGit/webhook --param events push
  ```
  {: pre}

  A commit to the GitHub repository by using a `git push` causes the trigger to be fired by the webhook. If a rule matches the trigger, then the associated action is invoked. The action receives the GitHub webhook payload as an input parameter. Each GitHub webhook event has a similar JSON schema, but is a unique payload object that is determined by its event type. For more information about the payload content, see the [GitHub events and payload](https://developer.github.com/v3/activity/events/types/) API documentation.
