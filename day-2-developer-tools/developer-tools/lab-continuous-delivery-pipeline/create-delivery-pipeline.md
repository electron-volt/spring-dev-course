# Create delivery pipeline

In this module, you will use AWS CodePipeline to set up a continuous delivery pipeline with source, build, and deploy stages. The pipeline will&#x20;

* detect changes in the code stored in your GitHub repository
* build the source code using AWS CodeBuild
* then deploy your application to AWS Elastic Beanstalk.

Our app.js from GitHub should eventually be shown in our Elastic Beanstalk web app.

What we'll do:

* Set up a continuous delivery pipeline on AWS CodePipeline
* Configure a source stage using your GitHub repo
* Configure a build stage using AWS CodeBuild
* Configure a deploy stage using your AWS ElasticBeanstalk application
* Deploy the application hosted on GitHub to ElasticBeanstalk through a pipeline.

## Create Pipeline

### Pipeline

* In a browser window, open the AWS CodePipeline Console.
* Click the **orange "Create pipeline" button**.&#x20;
* Name: Yourname-**Pipeline**
* Visually confirm that **"New service role"** is selected.
* Click the **orange "Next" button**.

![](<../../../.gitbook/assets/image (396).png>)

### Source

* Select **"GitHub"** from the "Source provider" dropdown menu.&#x20;
  1. Pick **Version 1** and ignore the warning. We will continue with OAuth for this lab.&#x20;
* Click the **white "Connect to GitHub" button**. A new browser tab will open asking you to give AWS CodePipeline access to your GitHub repo.
* Click the **green "Authorize aws-codesuite" button**.&#x20;
* In AWS, click **Confirm**.&#x20;

![](<../../../.gitbook/assets/image (419).png>)

* From the "Repository" dropdown **select the aws-elastic-beanstalk.... one**
* Select **"main"** from the "branch" dropdown menu.
* Visually confirm that **"GitHub webhooks"** is selected.
* Click the **orange "Next" button**.

### Build stage

1. From "Build provider dropdown", select "**AWS CodeBuild"**
2. Check that you are in your usual region&#x20;
3. Select your yourname-pipeline-project CodeBuild project
4. Use single build&#x20;
5. Click N**ext**.

### Deploy stage

1. Select **"AWS ElasticBeanstalk"** from the "Deploy provider" dropdown menu.
2. Check that you are in your usual region&#x20;
3. Click the field under "Application name" and confirm you can see the Elastic Beanstalk app you created earlier
4. Select the correspoding environment from the "Environment name" text box.
5. Click the **orange "Next" button**. You will now see a page where you can review the pipeline configuration.

### Review

Review your settings and then create the pipeline.&#x20;

## Successful execution&#x20;

You will now see your pipeline. It has three stages: Source, Build and Deploy.&#x20;

It should begin to execute automatically.&#x20;

It should execute successfully:

<img src="../../../.gitbook/assets/image (378).png" alt="" data-size="original">

## Elastic Beanstalk

Now let's go to Elastic Beanstalk and see what our app looks like now.&#x20;

![](<../../../.gitbook/assets/image (203) (1).png>)

You should see a white background and whatever text you used to modify the line 5 of app.js in our first commit to our repo.&#x20;

## Where we're at

![architecture now](<../../../.gitbook/assets/image (280) (1).png>)

We have created a continuous delivery pipeline on AWS CodePipeline with three stages: source, build, and deploy.&#x20;

* The source code from the GitHub repo we created is part of the source stage.&#x20;
* That source code is then built by AWS CodeBuild in the build stage.&#x20;
* Finally, the built code is deployed to the AWS Elastic Beanstalk environment.
