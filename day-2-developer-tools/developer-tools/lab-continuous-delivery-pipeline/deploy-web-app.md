# Deploy web app

## Elastic Beanstalk environment

In the AWS console, navigate to Elastic Beanstalk. We will create a web application using Node.js.

Look for the orange "Create application" button:

&#x20;

![](<../../../.gitbook/assets/image (170).png>)

The wizard is very easy to use.&#x20;

We want these settings:

* Name: **pipeline-app**
* No tags
* Platform **Node.js**&#x20;
  * use whatever branch and version AWS suggests
* Application code: **sample application**
* Click **configure more options.**

For Configuration presets, Ensure that **"**Single instance _(Free Tier eligible)"_ is selected.

* Click **create app.**&#x20;

![environment is being prepared.](<../../../.gitbook/assets/image (12).png>)

Wait for a few minutes for the environment to come up.&#x20;

## Test it

Click on the URL of your environment to make sure it's running.&#x20;

![](<../../../.gitbook/assets/image (440).png>)

## Where we're at

![](<../../../.gitbook/assets/image (161) (1).png>)
