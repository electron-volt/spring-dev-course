# Setup

## Region&#x20;

Choose one of these regions to build this lab in and put all of your resources there:

* us-west-2 (US West - Oregon)
* us-east-2 (US East - Ohio)
* us-east-1 (US East - Northern Virginia)
* eu-central-1 (Europe - Frankfurt)&#x20;

## Git credentials&#x20;

You will need one of these:

* HTTPS git credentials created in IAM OR
* git-remote-commit in your local terminal or CloudShell.&#x20;

## Cloud9 IDE

Create a Cloud9 environment in your chosen region and open the terminal.&#x20;

If using a shared environment, name your environment yourname-ide.

Note: if you are using CloudShell due to git-remote-commit, then **pick a region where CloudShell is supported**.&#x20;

Eu-central-1 works well.&#x20;

## Set up our Git repository

### Create Git repository

We will use CodeCommit to create a Git repository.

Create a repository called yourname-**wildrydes-site**.

In the Cloud9 terminal, clone the repo with the HTTPS URL.

### Populate the repository

On your local machine or in the Cloud9 terminal

* change directory into the wildrydes-site &#x20;
* &#x20;run the commands below.

These commands change directory into your repository, copy some static files from a public S3 bucket and push them into your repo.

```
aws s3 cp s3://wildrydes-us-east-1/WebApplication/1_StaticWebHosting/website ./ --recursive
git add .
git commit -m 'initial'
git push
```

Here is what it looks like now in the repo:

![](<../../.gitbook/assets/image (292).png>)
