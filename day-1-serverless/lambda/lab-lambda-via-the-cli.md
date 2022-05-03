---
description: >-
  In this lab we will learn a new way of deploying Lambda functions: via the CLI
  with a .zip file.
---

# LAB: Lambda via the CLI

## Overview

* we will create an execution role&#x20;
* then we will create a deployment package, a ZIP file
* we will then create the Lambda using the ZIP&#x20;
* we will test our Lambda.&#x20;

If you do not have AWS CLI installed, you can use CloudShell for this lab.&#x20;

### Shared account users

If you are working in a shared account, when you create your resources, name them YOURNAME-resource.&#x20;

This lab has many commands where you will need to change the name of your function or role. This is where CloudShell comes in handy.&#x20;

If you are working in CloudShell, there should be a feature called Safe Paste that is on by default. When you paste a multi-line command you have a chance to edit the command like this:

![Safe Paste in CloudShell](<../../.gitbook/assets/image (239).png>)

Then you can easily change the name of the function in the text editor.&#x20;

## Creating a Lambda function via the CLI&#x20;

AWS resources by default have no privileges or permissions. Therefore if we are going to use them for something, we need to grant them the necessary permissions.&#x20;

For this purpose we need to start by creating an execution role for our Lambda function. This role is what allows your function to access AWS resources, for example write logs to CloudWatch.&#x20;

### Create an execution role

#### Create a trust policy

A trust policy is a JSON file that allows a principal to assume a role. Basically this policy says "we will let Lambda assume the role that this policy is attached to".

* Create a .json file with the following content&#x20;
* Name the file `trust-policy.json`

{% code title="trust-policy.json" %}
```json
{ 
    "Version": "2012-10-17", 
    "Statement": [
        {
            "Effect": "Allow", 
            "Principal": { 
                "Service": "lambda.amazonaws.com"
            }, 
            "Action": "sts:AssumeRole" 
        }
    ]
}
```
{% endcode %}

#### Create the role

The following command creates a role called **lambda-ex** and associates the policy specified in the JSON above with the role.&#x20;

If working in a shared account, change the role name on line 2 to **YOURNAME-lambda-ex.**

* Run the command.&#x20;

```
aws iam create-role \
--role-name lambda-ex \
--assume-role-policy-document file://trust-policy.json
```

The output should look like this:

```json
{
    "Role": {
        "Path": "/",
        "RoleName": "lambda-ex",
        "RoleId": "AROA34F5GH6SOS",
        "Arn": "arn:aws:iam::123123123123:role/lambda-ex" ....
```

🎯 Note: on line 6 there is an ARN or Amazon Resource Name. Every resource in AWS has a unique ARN. We are going to need the ARN of this role, so please **copy the ARN** and store it somewhere!&#x20;

#### Add permissions to the role

Thus far we have created a role and used a trust policy to say that we will let Lambda assume the role. However the role itself has no permissions.&#x20;

Next we will add basic Lambda execution permissions to the role. For this we will use an AWS-managed policy.&#x20;

* Run the following command
* If in a shared account, change line 2 to read **YOURNAME-lambda-ex.**

```
aws iam attach-role-policy \
--role-name lambda-ex \
--policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
```

You have now created an execution role called **lambda-ex** with basic execution permissions.&#x20;

### Prepare the .zip file

For this example we will be using Python.

* Create a file called `lambda_function.py`

The contents of this file are

{% code title="lambda_function.py" %}
```python
import json
def lambda_handler(event, context): 
    return { 
        'statusCode': 200, 
        'body': json.dumps('Hello from the cli lab!') 
    }
```
{% endcode %}

* Then create the .zip:

```
zip my-deployment-package.zip lambda_function.py
```

Your deployment package called `my-deployment-package.zip` is now ready.

### Deploy the .zip file

We will deploy the .zip file via the CLI using the `aws lambda create-function` command. We need to provide the following information:

* **function name**: my-cli-lambda
  * if in a shared environment, name it **YOURNAME-cli-lambda**
* **zip file**: my-deployment-package.zip
* **handler**: for the Python example this is `lambda_function.lambda_handler`
* **runtime**: python3.9&#x20;
* **role**: you need the ARN of the **lambda-ex** role from before

The complete command looks like this - just be sure to replace the ARN on line 4.&#x20;

```
aws lambda create-function \
--function-name YOURNAME-cli-lambda \
--zip-file fileb://my-deployment-package.zip \
--role <ARN of the role here> \
--handler lambda_function.lambda_handler \
--runtime python3.9
```

You will get some JSON back as a response to a successfully run command.&#x20;

### Test the function

Once the function has been deployed, we can test it via the CLI like this

```
aws lambda invoke \
--function-name YOURNAME-cli-lambda \
--cli-binary-format raw-in-base64-out \
--payload '{ "key": "value" }' response.json
```

The content of the payload does not matter in this case. You should get the following response:

```json
{
   "StatusCode": 200,
   "ExecutedVersion": "$LATEST"
}
```

To see some logs, we can run

```
aws lambda invoke --function-name YOURNAME-cli-lambda out --log-type Tail
```

The response should look like this:

```json
{ 
    "StatusCode": 200,
    "LogResult": base64-encoded string 
    "ExecutedVersion": "$LATEST"
}
```

The logs aren't of much use to us if they are base64 encoded. To decode them, run

```
aws lambda invoke \
--function-name YOURNAME-cli-lambda \
out --log-type Tail \
--query 'LogResult' \
--output text | base64 -d
```

You get information about the duration, memory used and other metrics.&#x20;

### End result

&#x20;Try running

```
aws lambda list-functions
```

it should list two Lambda functions: the one we created via the console and the one we just created via the CLI.&#x20;
