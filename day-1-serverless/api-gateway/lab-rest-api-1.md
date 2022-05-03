# LAB: REST API 1

## Lambda proxy integration

First we will see what a proxy integration looks like. Lambda proxy integration is a lightweight, flexible API Gateway API integration type that allows you to integrate an API method – or an entire API – with a Lambda function. Because it's a proxy integration, you can change the Lambda function implementation at any time without needing to redeploy your API.

### Create the Lambda function&#x20;

Create a Lambda function with the following information:

Function name: Yourname-`GetStartedLambdaProxyIntegration`.

Runtime: Node.js

Architecture: x86\_64

![creating the function](<../../.gitbook/assets/image (43).png>)

Permissions:&#x20;

1. Create new role from AWS policy templates
2. Role name: Yourname-`GetStartedLambdaBasicExecutionRole`.
3. Policy templates: leave blank

![execution role](<../../.gitbook/assets/image (15).png>)

Function code: paste in the following and deploy changes.&#x20;

```javascript
'use strict';
console.log('Loading hello world function');
 
exports.handler = async (event) => {
    let name = "you";
    let city = 'World';
    let time = 'day';
    let day = '';
    let responseCode = 200;
    console.log("request: " + JSON.stringify(event));
    
    if (event.queryStringParameters && event.queryStringParameters.name) {
        console.log("Received name: " + event.queryStringParameters.name);
        name = event.queryStringParameters.name;
    }
    
    if (event.queryStringParameters && event.queryStringParameters.city) {
        console.log("Received city: " + event.queryStringParameters.city);
        city = event.queryStringParameters.city;
    }
    
    if (event.headers && event.headers['day']) {
        console.log("Received day: " + event.headers.day);
        day = event.headers.day;
    }
    
    if (event.body) {
        let body = JSON.parse(event.body)
        if (body.time) 
            time = body.time;
    }
 
    let greeting = `Good ${time}, ${name} of ${city}.`;
    if (day) greeting += ` Happy ${day}!`;

    let responseBody = {
        message: greeting
    };
    
    let response = {
        statusCode: responseCode,
        headers: {
            "x-custom-header" : "my custom header value"
        },
        body: JSON.stringify(responseBody)
    };
    console.log("response: " + JSON.stringify(response));
    return response;
};
```

That's it for the Lambda function, which will serve as our backend.&#x20;

### Create the REST API&#x20;

Let's navigate back to API Gateway.&#x20;

* Click **Create API**&#x20;
* Go to REST API and click **build** (note: make sure it is NOT the _private_ REST API)&#x20;

![Green one plz](<../../.gitbook/assets/Screenshot 2022-04-29 at 16.04.58.png>)

* Protocol is **REST**.
* We want a **new API.**&#x20;
* Give it the name Yourname-`LambdaSimpleProxy`
* Endpoint type is **regional**.&#x20;
* Click **create API.**&#x20;

![REST API creation ](<../../.gitbook/assets/image (85).png>)

### Create the resource

Next we will create a resource in our API.

![](<../../.gitbook/assets/image (278).png>)

With the root resource / highlighted, click Actions and choose Create Resource.&#x20;

Proceed with these settings:

* Type in resource name **helloworld**.&#x20;
* Leave all checkboxes unchecked.&#x20;
* Click **create resource**.&#x20;

![resource creation](<../../.gitbook/assets/image (345).png>)

### Create the method

In a proxy integration, the entire request is sent to the backend Lambda function as-is, via a catch-all `ANY` method that represents any HTTP method. The actual HTTP method is specified by the client at run time. The `ANY` method allows you to use a single API method setup for all of the supported HTTP methods: `DELETE`, `GET`, `HEAD`, `OPTIONS`, `PATCH`, `POST`, and `PUT`.&#x20;

Let's create the ANY method:

With the /helloworld resource highlighted, click **Actions** and **Create Method**

![](<../../.gitbook/assets/image (275).png>)

Choose the method **ANY** from the dropdown and click the checkmark next to it to confirm:

![](<../../.gitbook/assets/image (246).png>)

Once the checkmark has been pressed, you can finish the setup with these settings:

* Integration type: **Lambda function**
* Use Lambda proxy integration: **check**
* Region: the same one where you created your Lambda function
* Lambda: Yourname-GetStartedLambdaProxyIntegration
* Use default timeout: **check**
* **Save**.

![Set up for ANY method](<../../.gitbook/assets/image (40).png>)

A pop up window appears with text&#x20;

"Add Permission to Lambda Function You are about to give API Gateway permission to invoke your Lambda function:..."

click **OK**.&#x20;

### Deploy the API&#x20;

Let's go back to the familiar **Actions** button and under API actions choose **deploy**. &#x20;

![Deploy API ](<../../.gitbook/assets/image (81).png>)

* Choose **New Stage**
* give your stage the name **beta**
* **deploy**.&#x20;

![Deploy to new stage beta](<../../.gitbook/assets/image (201).png>)

### Test the API&#x20;

We will test the API with a simple GET request via the browser.&#x20;

You should see the invoke URL of your API on your screen:&#x20;

![invoke URL of API ](<../../.gitbook/assets/image (326).png>)

Copy the invoke URL. Paste it into a new tab in the browser.

Then add to the end&#x20;

/helloworld?city=Gotham\&name=Bruce.

You should get this response:

![response in browser](<../../.gitbook/assets/image (383).png>)

The response will be JSON that says

```
"message":"Good day, Bruce of Gotham."
```

So we have our invoke URL, the stage beta, the resource helloworld and query string parameters city and name. The parameters are used in the greeting that our Lambda function returns to us.&#x20;

### End result

We made an API with a Lambda backend. Since we did a proxy integration, we get the following:

![proxy integration](<../../.gitbook/assets/image (304).png>)

With this type of integration whatever the Lambda returns is also returned to the client as-is. Next we will look at a non-proxy integration.&#x20;
