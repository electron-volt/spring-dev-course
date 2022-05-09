# Create build project

We are going to use CodeBuild to build the source code previously stored in your GitHub repository. We will

* Create a build project with AWS CodeBuild
* Set up GitHub as the source provider for a build project
* Run a build on AWS CodeBuild.

## Create a project in CodeBuild

In a new browser tab open the AWS CodeBuild Console.

### Source

* Click the **orange "Create project" button**.
* Project name: pipeline-project
* Select **"GitHub"** from the "Source provider" dropdown menu.&#x20;
* Visually confirm that the **"Connect using OAuth" radio button** is selected.
* Click the **white "Connect to GitHub" button**. After clicking this button, a new browser tab will open asking you to give AWS CodeBuild access to your GitHub repo.

![connecting to GitHub](<../../../.gitbook/assets/image (255).png>)

* Click the **green "Authorize aws-codesuite" button**.
* Type in your GitHub password&#x20;
  * this step is not necessary if you are already logged in to GitHub in your browser
* Back in AWS CodeBuild, click the **orange "Confirm" button**.
* Select "Repository in my GitHub account."
* Type **"aws-elastic-beanstalk-express-js-sample"** in the search field.

Leave the webhook section unchecked.&#x20;

### Environment

In the Environment section,&#x20;

* Make sure that **"Managed Image"** is selected.
* Select **"Amazon Linux 2"** from the "Operating system" dropdown menu.
* Select **"Standard"** from the "Runtime(s)" dropdown menu.
* Select **"aws/codebuild/amazonlinux2-x86\_64-standard:3.0"** from the "Image" dropdown menu.
* Visually confirm that **"Always use the latest image for this runtime version"** is selected for "Image version."
* Visually confirm that **"Linux"** is selected for "Environment type."
* Visually confirm that **"New service role"** is selected.

### Buildspec&#x20;

1. Select **"Insert build commands."**
2. Click on **"Switch to editor."**
3. **Replace** the Buildspec in the editor with the code below:

```yaml
version: 0.2
phases:
    build:
        commands:
            - npm i --save
artifacts:
    files:
        - '**/*'
```

It should look like this:

![](<../../../.gitbook/assets/image (311).png>)

Complete the creation with **Create build project**.

## Test the build

You should be taken to a screen with an orange "Start build" button on the right hand upper corner.&#x20;

Try building the project. We expect this level of success:

![](<../../../.gitbook/assets/image (283).png>)

## Where we're at

We have our Git repository set up.&#x20;

We have a web app running in Elastic Beanstalk.&#x20;

We have created a build project that builds our GitHub repo source automatically.&#x20;

![](<../../../.gitbook/assets/image (138).png>)
