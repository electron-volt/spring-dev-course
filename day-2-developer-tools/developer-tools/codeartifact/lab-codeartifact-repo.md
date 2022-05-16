# LAB: CodeArtifact repo

## Walk-through

In this lab we are going to create a CodeArtifact repository.

This lab requires that you have the Python package installer pip installed, either locally on your machine or in CloudShell.&#x20;

## Install pip

For this lab to work you will need a terminal with pip installed.&#x20;

If you want to download pip locally, here are instructions:

{% embed url="https://pypi.org/project/pip" %}
pip
{% endembed %}

#### CloudShell

If you are using CloudShell, run this command:

```
sudo yum install pip -y
```

## Create the repository

Navigate to CodeArtifact.&#x20;

![every lab starts with an orange button it seems](<../../../.gitbook/assets/image (347).png>)

* Click **create repository**.
* Name the repo **yourname-repo**
* Description can be left blank.&#x20;
* For public upstream repositories, use **pypi-store**.
* Click **next**.

![](<../../../.gitbook/assets/image (352).png>)

### Domain&#x20;

* Under Domain, for AWS account pick "**this AWS account"**.&#x20;

We have to create a domain as we don't have one yet - and each repo has to belong to a domain.&#x20;

* name the domain **yourname-domain**&#x20;

![domain settings](<../../../.gitbook/assets/image (423).png>)

### Review

Review the settings and then click **create repository.**

## Connection instructions

When your repo has been created, you will be taken to a summary page.&#x20;

Click on **View connection instructions**.

This dialogue opens up. Choose **pip** from the dropdown to reveal the CLI command.&#x20;

![connection URL](<../../../.gitbook/assets/image (320).png>)

* Copy the CLI command and run it in your local terminal or in CloudShell.

You will ideally see this response:

```
Successfully configured pip to use AWS CodeArtifact repository https://repo-account.d.codeartifact.eu-north-1.amazonaws.com/pypi/my-repo/ 
Login expires in 12 hours at 2022-01-17 05:24:54+02:00
```

### Install something

While still in your terminal, try installing something. For example

```
pip install scipy
```

It may take a moment, but eventually your CodeArtifact repository will show the newly installed package:

![from Cloudshell](<../../../.gitbook/assets/image (135).png>)

![to Artifact.](<../../../.gitbook/assets/image (329) (1).png>)

## Clean up&#x20;

Time to delete things:

* In CodeArtifact > repositories, delete the repo you created yourname-repo and pypi-store.&#x20;
* In CodeArtifact > domain, delete your domain.

Note: the domain can only be deleted once it has 0 repos in it.&#x20;
