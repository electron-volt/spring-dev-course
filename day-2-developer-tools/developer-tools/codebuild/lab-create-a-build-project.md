# LAB: Create a build project

## Walk-through&#x20;

We are going to have CodeBuild build some Java code for us and produce an artifact.&#x20;

Here is what happened:

* get source code from GitHub as ZIP&#x20;
* put the code into S3
* create a build project in CodeBuild
* trigger the build.

#### Scope

For this lab, we are only interested in how CodeBuild works. You do not need to understand the source code or understand what the buildspec.yml file does line-by-line.&#x20;

## Preparation

First we have to prepare the source code that we want to compile. All the code is in a public GitHub repository.&#x20;

* We'll create input and output S3 buckets
* We will get the source code as a ZIP file
* We will unzip the file and upload the contents to the input S3 bucket.

### Create S3 buckets

We will need two buckets. When you create them, just give them names and hit create - **all other settings can be kept as default.**&#x20;

For the names, call them **YOURNAME-CB-EXAMPLE** and have one of them end with "**input**" and the other with "**output**". For example:

![example S3 bucket names](<../../../.gitbook/assets/image (420).png>)

#### Bucket name not available?&#x20;

If for some reason the name happens to be taken, then try yourname-ddmm-input. &#x20;

### Get source code

Go to [https://github.com/electron-volt/codebuild-example](https://github.com/electron-volt/codebuild-example)

* Click the green button that says Code
* then choose **Download ZIP.**

![Git repo](<../../../.gitbook/assets/image (264).png>)

Once the ZIP has been downloaded,&#x20;

* **Unzip** it.

You should have a folder called **codebuild-example-main** with the following contents.&#x20;

![ZIP and its contents](<../../../.gitbook/assets/image (463) (1).png>)

### Buildspec.yml

Here is what the buildspec.yml looks like. You do not need to do anything to this file, just take a look at its contents. It's fine if you don't fully understand each line.&#x20;

{% code title="buildspec.yml" %}
```yaml
version: 0.2

phases:
  install:
    runtime-versions:
      java: corretto11
  pre_build:
    commands:
      - echo Nothing to do in the pre_build phase...
  build:
    commands:
      - echo Build started on `date`
      - mvn install
  post_build:
    commands:
      - echo Build completed on `date`
artifacts:
  files:
    - target/messageUtil-1.0.jar
```
{% endcode %}

### Upload files to S3

In S3, navigate to the bucket that ends with -**input**.&#x20;

Choose **Upload > Add folder**

Add the entire **codebuild-example-main** folder to S3. (Do not upload the zip file)

You should have the following structure in your input bucket:

![Top level has one folder](<../../../.gitbook/assets/image (52).png>)

![files in a folder](<../../../.gitbook/assets/image (310) (1).png>)

## Create the build

Let's navigate to **CodeBuild.**&#x20;

Your landing page may look different, but look for an orange "**Create project**" or "**Create Build project"** button.

![](<../../../.gitbook/assets/image (98).png>)

Name the project **YOURNAME-cb-project.**

### Source

Under Source, use the following settings (choose your own input bucket in the Bucket field).

For S3 object key, use

```
codebuild-example-main/
```

![source settings](<../../../.gitbook/assets/image (137) (1).png>)

### Environment

Under Environment, use the following settings:

![env settings](<../../../.gitbook/assets/image (449).png>)

### Service role

Accept the defaults:&#x20;

* new service role&#x20;
* accept the name that AWS suggests.&#x20;

![service role settings](<../../../.gitbook/assets/image (206).png>)

### Buildspec

Use these settings:

![buildspec settings](<../../../.gitbook/assets/image (422).png>)

For buildspec name, put in&#x20;

```
buildspec.yml
```

Adding the buildspec name is mandatory as otherwise our build would fail.

### Artifacts

Use these settings, again substituting your output S3 bucket:

![Artifact settings](<../../../.gitbook/assets/image (340).png>)

* Leave **name, path** and **namespace** blank.&#x20;
* Packaging: **None**.

### Logs

Uncheck both CloudWatch and S3. Who needs logs. 🤷🏽

### Create build

Create build project.&#x20;

## Start build

Start the build with the orange button that says "Start build".&#x20;

![](<../../../.gitbook/assets/image (157).png>)

You will be able to follow along as it progresses by clicking the **Phase details** tab:

![build phase details](<../../../.gitbook/assets/image (221).png>)

If it looks stuck in "In progress", refresh the page.&#x20;

### Run the build:

Everything runs smoothly:

![100% success](<../../../.gitbook/assets/image (182).png>)

### Artifact

My build project has successfully uploaded an artifact into the output bucket:

![](<../../../.gitbook/assets/image (155) (1).png>)

### End result & clean up

We created our first build project!&#x20;

We can delete

* our S3 buckets
* our code build project.&#x20;
