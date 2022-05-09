# Deploy web app

## Before you start

For this lab to work, your AWS account needs to have something called a default VPC.&#x20;

If you are not sure if your account has a default VPC, you can create one via the AWS CLI with the command

```
aws ec2 create-default-vpc
```

If you are doing this lab in a shared account or you are not sure if you're allowed to create VPC's in your account, ask your instructor for help.&#x20;

## Elastic Beanstalk environment

In the AWS console, navigate to Elastic Beanstalk. We will create a web application using Node.js.

Look for the orange "Create application" button:

&#x20;

![](<../../../.gitbook/assets/image (170).png>)

The wizard is very easy to use.&#x20;

We want these settings:

* Name: **pipeline-lab**
* No tags
* Platform **Node.js**&#x20;
  * use whatever branch and version AWS suggests
* Application code: **sample application**
* Click **configure more options.**

For Configuration presets, Ensure that **"**Single instance _(Free Tier eligible)"_ is selected.

* Click **create app.**&#x20;

![environment is being prepared.](<../../../.gitbook/assets/image (12).png>)

Wait for a few minutes for the environment to come up.&#x20;

## Test it&#x20;

When your environment's health turns to OK, you will see a URL under the Pipelinelab-env text.&#x20;

![environment is healthy](<../../../.gitbook/assets/image (197).png>)



Click on the URL of your environment to make sure it's running.&#x20;

![snippet of the running web app ](<../../../.gitbook/assets/image (440).png>)

## Where we're at

We have created a web application running in Elastic Beanstalk!&#x20;

![](<../../../.gitbook/assets/image (161) (1).png>)
