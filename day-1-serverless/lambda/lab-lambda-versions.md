# LAB: Lambda versions

## Overview

In this lab we are going to create some new versions of a Lambda function.

We will&#x20;

* change the function code
* deploy the changes
* publish the new code as a new version of our function.&#x20;

## Versions in the console

Let's go back to the AWS console and navigate to Lambda.&#x20;

Pick the **my-cli-lambda function** and click on its name to get to the code editor.&#x20;

## Version 1

Let's go to the code editor. We are now looking at and editing $LATEST. Let's change the string that the function outputs to "This is version 1".&#x20;

It should now look like this:

![version 1](<../../.gitbook/assets/image (269).png>)

Deploy your changes:

![changes deployed](<../../.gitbook/assets/image (436).png>)

Then on the right upper hand corner of the screen you'll see a dropdown that says "**Actions**". Select  "**publish version**".

![publish new version ](<../../.gitbook/assets/image (69).png>)

A dialogue appears offering to give your version a description.&#x20;

* Type "**version1**" for the description.&#x20;
* Press **Publish**.&#x20;

![version publishing ](<../../.gitbook/assets/image (407) (1) (1).png>)

You should see a green banner that says "Successfully created version 1 for function my-cli-lambda".

![sign of success!](<../../.gitbook/assets/image (90).png>)

If you now go back to the Code tab, you'll see you are not able to edit the code of this version: it is fixed. Once a version of your code has been published, it is immutable. If you want to make changes, you have to make changes to $LATEST and then publish a new version.&#x20;

![Cannot edit a published version. ](<../../.gitbook/assets/image (171).png>)

## Version prod

The version1 example was not that exciting. It might be easier to see why you would want versions of your code if you imagine that it will be running in production. In that case you would want any changes to production code to be rolled out in a controlled manner, not just anyone editing the code in the console and pressing "Deploy".&#x20;

So let's create a production-ready version of our Lambda function. In order to make changes to the code, we need to find our way back to $LATEST.

At the top of the screen you should see this:

![breadcrumbs](<../../.gitbook/assets/image (188) (1).png>)

Click on the name of your function to get back to $LATEST.

![We get the deploy button back! ](<../../.gitbook/assets/image (291) (1).png>)

Edit the code to return "**This is the production version!"** and deploy your changes.&#x20;

Then go to Actions and publish version "**prod**".&#x20;

### View your versions

Once your prod version is successfully published, let's go back to $LATEST.&#x20;

There is a tab called **Versions**. Once you click on it, you should see the following:

![versions list](<../../.gitbook/assets/image (105).png>)

### End result

That is it for this lab! We have seen how we can freeze a snapshot of our code and ensure that no unintentional changes are made to it. Next we will see what this strange Aliases column is about.&#x20;
