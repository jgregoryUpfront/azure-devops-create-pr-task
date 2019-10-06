Build | Release | Extension
:-----| :-------| :--------
[![Build Status](https://dev.azure.com/shaykia/AzureDevOpsExtensions/_apis/build/status/shayki5.AzureDevOps-CreatePRTask?branchName=master)](https://dev.azure.com/shaykia/AzureDevOpsExtensions/_build/latest?definitionId=34&branchName=master) | [![Release Status](https://vsrm.dev.azure.com/shaykia/_apis/public/Release/badge/3372e1d4-189a-4d9e-aa4d-0cb86eff3c2e/1/2)](https://vsrm.dev.azure.com/shaykia/_apis/public/Release/badge/3372e1d4-189a-4d9e-aa4d-0cb86eff3c2e/1/2) | [![Extnesion](https://vsmarketplacebadge.apphb.com/version/ShaykiAbramczyk.CreatePullRequest.svg)](https://vsmarketplacebadge.apphb.com/version/ShaykiAbramczyk.CreatePullRequest.svg)

## Azure DevOps Create Pull Request Task

An easy way to create automatically a Pull Request from your Build or Release Pipeline.

You can create a Pull Request to a Azure DevOps (Repos) repository or to a GitHub repository.

Supports also multi target branch (PR from one source branch to many target branches).

## Prerequisites

- **The task works currently only in Windows machines.**

### For Azure DevOps Repository:

- You need to enable the "Allow scripts to access the OAuth token": 

  - If you use the classic editor, go to the Agent job options, scroll down and check the checkbox "Allow scripts to acess the OAuth token":

    ![Oauth](https://i.imgur.com/trYBvHG.png)

  - If you use `yaml` build, you need to map the variable in the task:

    ```yaml
     env:
       System_AccessToken: $(System.AccessToken)
    ```
- You need to give permissions to the build user (in Microaoft hosted agnet is "Build Service (user-name)"):

    ![Permissions](https://i.imgur.com/Us401RM.png)

### For GitHub Repository:

- You need to create a GitHub service connection with Personal Access Token (PAT) - with `repo` permissions: 

    ![GithubConnection](https://i.imgur.com/imWdnT7.png)

- To create the GitHub PAT go to https://github.com/settings/tokens/new

    ![PAT](https://i.imgur.com/AmKuY7d.png)

## Usage

**In the classic editor:**

![Task](https://i.imgur.com/Dm4I2Af.png)

- **Git repository type**: Azure DevOps (Repos) or GitHub. When you choose GitHub you need to choose from the list the GitHub service connection (that use PAT authorization.)

- **GitHub Connection (authorized with PAT)**: When you choose GitHub in `Git repository type` you need to specify here the GitHub service connection.

- **Source branch name:** The source branch that will be merged. The default value is the build source branch - `$(Build.SourceBranch)`.

- **Target branch name:** The target branch name that the source branch will be merge to him. For example: `master`. Supports also multi target branch with `*`, for example: `test/*`.

- **Title:** The Pull Request title.

- **Description:** The Pull Request description. *(Optional)*.

- **Reviewers:** The Pull Request reviewers *(Optional)* . For Azure DevOps - one or more email addresses separated by semicolon. For example: `test@test.com;pr@pr.com`. For GitHub:  one or more usernames separated by semicolon. For example: `test;user`.

**In yaml piepline:**

```yaml
- task: CreatePullRequest@1
  inputs:
    repoType: Azure DevOps / GitHub
    githubEndpoint: 'my-github' # When you choose GitHub in `repoType` you need to specify here the GitHub service connection
    sourceBranch: '$(Build.SourceBranch)'
    targetBranch: 'master'
    title: 'Test'
    description: 'Test' # Optional
    reviewers: For Azure DevOps: 'test@test.com'. For GitHub: `username` # Optional
  env:
    System_AccessToken: $(System.AccessToken)
```

## Known issue(s)

 - In Azure DevOps Server (TFS) you can't use reviewers. still can create a PR without it.

## Release Notes

### New in 1.2.0

- Supports also GitHub repositories!

### New in 1.0.31

 - Multi target branch (For example: `feature/*`)

### New in 1.0.0

 - First version.

