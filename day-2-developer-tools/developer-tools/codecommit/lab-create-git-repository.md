# LAB: Create Git repository

In this lab we will create a Git repository and push some changes into it. If you don't know git, no worries - you'll learn everything you need as we go along.&#x20;

We will&#x20;

* Set up the credentials that we need for CodeCommit and Git
* Create a repository
* Clone it
* Push some content into our repo.&#x20;

Windows users: I recommend Git Bash for this lab.&#x20;

## Setting up access&#x20;

### Option 1: Set up HTTPS Git credentials for CodeCommit

If you are using a stand-alone personal AWS accout where you have full administrator access, you can use HTTPS Credentials. Just make sure that you are signed in as the user that you intend to create credentials for.&#x20;

Go to **IAM > Users** and select your user.&#x20;

On the user details page, choose the **Security Credentials** tab, and in **HTTPS Git credentials for AWS CodeCommit**, choose **Generate**.

![Generate credentials](<../../../.gitbook/assets/image (102).png>)

Be sure to download your credentials or store them in a password manager. You cannot choose your own username and **you cannot view the password later.**&#x20;

### **Option 2: Git-remote-commit**

If for some reason you can't access your IAM user permissions - for example your AWS account is part of an AWS organization and you do not have direct access to your IAM user -  then you can use git-remote-commit. It works well when used inside **AWS CloudShell**.&#x20;

Follow these instructions:

{% embed url="https://github.com/aws/git-remote-codecommit" %}

## Create the Git repository&#x20;

Head over to **CodeCommit**.

![landing page](<../../../.gitbook/assets/image (376).png>)

Click **create repository.**

Name your repo **yourname-repo**

Click **create.**

![](<../../../.gitbook/assets/image (204).png>)

That's it!&#x20;

## Clone the repository

### Opion 1: If you have the Git credentials (personal account)&#x20;

From the CodeCommit console, you will get the HTTPS URL that you need to clone your repository;

![](<../../../.gitbook/assets/image (457).png>)

Then in a terminal, run the following command and ignore the warning.&#x20;

For **Username** and **password** use the IAM credentials we created earlier:

```
git clone https://git-codecommit.region.amazonaws.com/v1/repos/name
Cloning into 'name'...
Username for ...: *****
Password for ...: ****
warning: You appear to have cloned an empty repository.
```

**Optional** - set main as the default branch:

```
git config --global init.defaultBranch main
```

Otherwise the default branch will be called master. If you are okay with that, then skip this step.&#x20;

### Option 2: GRC

Go to **CloudShell**

No need to configure credentials. No access keys, no profile, no nothing. Just wait until you see a prompt.

In the shell, run these commands:

```
sudo yum install pip -y
pip install git-remote-codecommit
```

You should eventually see this text:

"Successfully installed botocore-1.20.112 git-remote-codecommit-1.16 ...."

Navigate back to **CodeCommit.**&#x20;

Look for the tab HTTPS (GRC):

![GRC tab](<../../../.gitbook/assets/image (280).png>)

Scroll down until you see Step 3 (please ignore steps 1 and 2):

![clone URL](<../../../.gitbook/assets/image (276).png>)

**Copy** this string to your clipboard.&#x20;

Navigate back to CloudShell and paste the text into the terminal:

![your prompt may look different ](<../../../.gitbook/assets/image (248).png>)

## First commit

In your terminal where you cloned the repo, go into the **yourname-repo** directory.&#x20;

Run **git config** to set up your username and email:

```
git config --local user.name "your-user-name" 
git config --local user.email your-email-address
```

Create two text files on your machine, `foo.txt` and `bar.txt.`&#x20;

The content of these files does not matter, it can be lorem ipsum or asdf.

Run **git add**&#x20;

```
git add foo.txt bar.txt
```

Optional: run the useful and informative **git status:**

```
git status
```

Then **git commit** with a message:

```
git commit -m "Added two text files."
```

![example](<../../../.gitbook/assets/image (108).png>)

## Push&#x20;

We have made a commit, but we have yet to push the changes to the repo.&#x20;

#### If you did the optional step of changing your default branch from master to main

Use this:

```
git push -u origin main
```

#### If you did not change the default branch to main

Use&#x20;

```
git push
```

Then run git status again:

```
git status
```

![it is hard to find a good git meme. ](<../../../.gitbook/assets/image (324).png>)

## Let's take a look&#x20;

Let's navigate back to the console and CodeCommit.&#x20;

![there they are! ](<../../../.gitbook/assets/image (464).png>)

Under commits, you see the commit with the message you used:

![](<../../../.gitbook/assets/image (161).png>)

## More testing

If you are already familiar with Git, then feel free to skip this step.&#x20;

Back in the terminal, try making some changes to your text files. For example:

Delete bar.txt.&#x20;

Change the contents of foo.txt.

Make another file like cat.jpeg.&#x20;

Then use&#x20;

* git add to stage the changes&#x20;
* git commit to commit them&#x20;
* git push to push the changes to your repository
* git status to check what status your repo is in.&#x20;

Extra: make changes to the repo in the console.&#x20;

Go to Code, select a file, then click Edit.

Make changes, add an author and an email and then save changes.&#x20;

Then use **git pull** in the terminal to fetch the changed files.&#x20;

## End result

We have seen how easy it is to use Git and CodeCommit. We have learned how to use GRC.&#x20;

That's it for CodeCommit, although it will resurface later in our CodeBuild lab.
