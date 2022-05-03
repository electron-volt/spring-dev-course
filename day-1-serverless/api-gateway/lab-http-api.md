# LAB: HTTP API

We are going to build a serverless API. We will create an HTTP API and a Lambda function. When we invoke the HTTP API, API Gateway routes the request to our Lambda function. Lambda runs the Lambda function and returns a response to API Gateway. API Gateway then returns a response to us.

## Create the Lambda function

We will use the console to create this Lambda function.&#x20;

* Create a Lambda function.&#x20;
* Choose "author from scratch".
* Name: **YOURNAME-api-lambda**
* Runtime: **Node.js 14.x, x86\_64**&#x20;
* Permissions: select **Create a new role with basic Lambda permissions**.&#x20;

You can change the message that the function returns to be anything you want, like "This is my first serverless api!"

## Create the HTTP API&#x20;

We will create an HTTP API.&#x20;

* Navigate to API Gateway
* Pick **HTTP API** & click **Build**

![](<../../.gitbook/assets/image (5).png>)

* Next click **Add integration**&#x20;
* For type, choose **Lambda**. Select your **YOURNAME-api-lambda** function.&#x20;
* Name your API yourname**-serverless-api**

![](<../../.gitbook/assets/Screenshot 2022-04-29 at 14.18.49.png>)

* Click **Review and create**.

![review the API](<../../.gitbook/assets/image (209).png>)

* Click **Create.**

Once your API is created, you will see an overview of its details. It will have an API ID, a URL, a stage called $default and so on.&#x20;

![overview of new API](<../../.gitbook/assets/image (51).png>)

## Test the API&#x20;

We will test the API through our browser. If you click on the **invoke URL** of your API as it is, it will return this:

![nope](<../../.gitbook/assets/image (373).png>)

Let's instead add the name of our function api-lambda to the end of the URL so it looks like this:

[https://YOUR API ID HERE.execute-api.aws-region.amazonaws.com/YOURNAME-api-lambda](https://gibberish.execute-api.aws-region.amazonaws.com/api-lambda)

Now it should return the message that you customized in your Lambda function.&#x20;

![Success!](<../../.gitbook/assets/image (360).png>)

### End result

Before April of 2022 and the new **Function URL** feature for Lambda functions, using an HTTP API as the front used to be the easiest way to see the output of your Lambda function in the browser. Now the same functionality could be achieved with just Lambda function URLs.&#x20;

Not the most exciting lab, I know.
