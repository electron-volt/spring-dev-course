# LAB: REST API 2

## Lambda non-proxy integration

In this lab we are going to create a Lambda function (`GetStartedLambdaIntegration`) and expose the function through an API Gateway API with a generic HTTP method.&#x20;

* We use the request body, a URL path variable, a query string, and a header to receive required input data from the client.&#x20;
* We turn on the API Gateway request validator for the API to ensure that all of the required data is properly defined and specified.&#x20;
* We configure a mapping template for API Gateway to transform the client-supplied request data into the valid format as required by the backend Lambda function.

Phew, a lot of work.

### Create the Lambda

#### Create the function

Name: Yourname-`GetStartedLambdaIntegration`

Runtime: Node.js&#x20;

Permissions:

1. Create a new role from AWS policy templates
2. Name: Yourname-GetStartedLambdaIntegrationRole
3. Use simple microservices permissions

![](<../../.gitbook/assets/image (12) (1) (1).png>)

Code:

```javascript
'use strict';
var days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];            
var times = ['morning', 'afternoon', 'evening', 'night', 'day'];

console.log('Loading function');

exports.handler = function(event, context, callback) {
  // Parse the input for the name, city, time and day property values
  let name = event.name === undefined ? 'you' : event.name;
  let city = event.city === undefined ? 'World' : event.city;
  let time = times.indexOf(event.time)<0 ? 'day' : event.time;
  let day = days.indexOf(event.day)<0 ? null : event.day;

  // Generate a greeting
  let greeting = 'Good ' + time + ', ' + name + ' of ' + city + '. ';
  if (day) greeting += 'Happy ' + day + '!';
  
  // Log the greeting to CloudWatch
  console.log('Hello: ', greeting);
  
  // Return a greeting to the caller
  callback(null, {
      "greeting": greeting
  }); 
};
```

#### Test the function

Create a test event by pressing the orange Test button. This opens a dialogue to create a new test event.

Event name: testing

The contents should be as follows:

```
{ "name": "Bruce", "city": "Gotham", "time": "night", "day": "Saturday" }
```

![Creating a test event](<../../.gitbook/assets/image (215).png>)

Now when you save the test event and then press the orange Test button again, you should get this response in the execution results tab:

![test results](<../../.gitbook/assets/image (9).png>)

This test event mimicks the eventual functionality of our API. That is it for the Lambda, now let's create the API!&#x20;

### Create the API&#x20;

1. Go to API Gateway.&#x20;
2. Click **Create API**
3. Under REST API, click **build**.
   1. there are two kinds of REST API's. We want the one that is **not** private.
4. Choose **REST API**
5. **new API**&#x20;
6. Give it a name **yourname-lambda-integration**
7. Endpoint type is **regional**
8. &#x20;**Create API**

#### Create the resource

1. With the root resource highlighted, click _Actions_ and create resource.&#x20;
2. Leave proxy resource **unchecked**
3. Resource name: **city**&#x20;
4. Resource path: **{city}**
5. **Check** Enable API gateway CORS
6. **Create resource**

![city resource](<../../.gitbook/assets/image (181).png>)

#### Create the method

![Create method for city](<../../.gitbook/assets/image (368).png>)

1. With the new resource city highlighted, click **Actions** and **create method**
2. For method choose **ANY**
   1. **note:** there is an OPTIONS method that gets added by default. Ignore it.&#x20;
3. Click the checkmark next to ANY
4. For method integration, choose Lambda
5. Leave proxy integration **unchecked**
6. Check you are in the same region as your Lambda
7. Choose the yourname-**GetStartedLambdaIntegration**
8. Use Default timeout: **check**
9. Click **Save**
10. When prompted, click OK to give Lambda API Gateway permissions.&#x20;

![ANY method ](<../../.gitbook/assets/image (166).png>)

#### Method execution

Once you have saved changes, you will see a screen with four boxes: Method request, Integration request, Method response and Integration response.&#x20;

#### Method request

* Click on **Method request.**
* Expand the **URL Query String Parameters** section. Choose **Add query string**.&#x20;

![add query string](<../../.gitbook/assets/image (146).png>)

* Type `time` for **Name**. Click the checkmark on the right hand side of the screen:

![](<../../.gitbook/assets/image (64).png>)

Once you click the checkmark, checkboxes will appear under Required and Caching.&#x20;

* Select the **Required** option.&#x20;
* Leave **Caching** cleared to avoid an unnecessary charge for this exercise.

Don't mind the orange triangle next to required.&#x20;

![choose required](<../../.gitbook/assets/image (428).png>)

Expand the **HTTP Request Headers** section. Choose **Add header**.&#x20;

![](<../../.gitbook/assets/image (442).png>)

* Type `day` for **Name**.&#x20;
* Click the checkmark.
* Select the **Required** option.  Leave **Caching** cleared to avoid an unnecessary charge for this exercise.

![](<../../.gitbook/assets/image (73).png>)

Next we need to define the method request payload.&#x20;

* On the left hand side of the screen, click **Models**
* Click **Create**

![](<../../.gitbook/assets/image (208).png>)

* Name: **GetStartedLambdaIntegrationUserInput**
* Content type: application/json
* Leave description blank&#x20;
* Copy the following into the Model schema editor, then click **create model**

```json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "title": "GetStartedLambdaIntegrationInputModel",
  "type": "object",
  "properties": {
    "callerName": { "type": "string" }
  }
}
```

#### Integration request

* Go back into **Resources**
* Click the **ANY** method of the **city** resource
* Click **Integration request**
* At the bottom you see **Mapping templates**, expand it&#x20;

![mapping templates](<../../.gitbook/assets/image (464) (1).png>)

* Click **add mapping template**
* Content type is **application/json**
* Click the checkmark

You will be asked to secure the integration. **Click "yes, secure this integration"**

* The radio button for "**When there are no templates defined"** should be selected
* From the "generate template" dropdown, choose the model we created: GetStartedLambdaIntegrationUserInput
* Replace the mapping script with the following and click **save**.

```
#set($inputRoot = $input.path('$'))
{
  "city": "$input.params('city')",
  "time": "$input.params('time')",
  "day":  "$input.params('day')",
  "name": "$inputRoot.callerName"
}
```

It should look like this:

![](<../../.gitbook/assets/image (446).png>)

Nothing special happens when you click save. You may see the refresh symbol flash over the save button, but just rest assured that it has been saved.&#x20;

Now we will not configure integration or method response.&#x20;

### Test the method

At the top of the screen you should see a blue arrow and text Method execution. Click on it.&#x20;

![click Method execution](<../../.gitbook/assets/image (431).png>)

This takes you back to the screen with four boxes. The method execution view has a test option:

![Test plus lightning bolt](<../../.gitbook/assets/image (357) (1).png>)

#### Create test event

Click on the **TEST**.

1. Method is **POST**
2. Path is **Gotham**
3. Query string is **time=night**
4. Headers is **day:Saturday**
5. Request body is **{ "callerName":"Bruce" }**
6. hit **Test**

![Test values](<../../.gitbook/assets/image (330) (1).png>)

#### Test results

You should see this:

![test result with our values](<../../.gitbook/assets/image (263) (1).png>)

The output should contain these lines, among others:

```
Endpoint response body before transformations: {"greeting":"Good night, Bruce of Gotham. Happy Saturday!"}
Method response body after transformations: {"greeting":"Good night, Bruce of Gotham. Happy Saturday!"}
```

This is the equivalent of the following HTTP request:

```http
POST /Gotham?time=night
day:Saturday

{
    "callerName": "Bruce"
}
```

### Deploy the API&#x20;

The test invocation is a simulation and has limitations. For example, it bypasses any authorization mechanism enacted on the API. To test the API execution in real time, you must deploy the API first. To deploy an API, you create a **stage** to create a snapshot of the API at that time. The stage name also defines the base path after the API's default host name. The API's root resource is appended after the stage name. When you modify the API, you must redeploy it to a new or existing stage before the changes take effect.

1. In the **Actions** dropdown, choose **Deploy API**&#x20;
2. Choose new stage
3. Give the stage the name **test**
4. Descriptions can be blank
5. **Deploy**

### Test the API

You can now test the API using for example cURL. Postman and Visual Studio Code work too.

When testing, use&#x20;

* path /city
* query string time
* header day:
* body {"callerName": "your name here"}

You can test the API in CloudShell using cURL with the command below, just replace the API ID on line 2.

```
curl -X POST \
  https://your api id HERE .execute-api.eu-central-1.amazonaws.com/test/Gotham?time=night \
  -H 'content-type: application/json' \
  -H 'day: Saturday' \
  -d '{"callerName": "Bruce"}'
```

Example with different values for parameters (the && echo "" is added just to move the prompt to a new line):

![](<../../.gitbook/assets/image (84).png>)

### End result

There you go, an actual REST API even if it is a very modest one.&#x20;
