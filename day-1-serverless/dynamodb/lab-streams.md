# LAB: Streams

## DynamoDB Streams and Lambda

In this lab we will enable a DynamoDB Stream on a table, associate the stream with a Lambda function and then make changes to the table.&#x20;

### Create the Lambda function

#### Create the execution role

We have seen how to create execution roles via the CLI. Now let's create one via the console.&#x20;

* Go to IAM > Roles
* Choose **create role**

![](<../../.gitbook/assets/image (33).png>)

* Create a role with the following properties_:_
  * trusted entity: **Lambda**

![](<../../.gitbook/assets/image (36).png>)

* Permissions policies: **AWSLambdaDynamoDBExecutionRole**

![](<../../.gitbook/assets/image (112).png>)

* role name**: YOURNAME-lambda-dynamodb-role**
* **Create role.**

While still in IAM > Roles, search for your role. When you click on the name, you will be taken to a Summary page where you will find the ARN of the role. Copy it and store it somewhere. It will look like this:

arn:aws:iam::123412341234:role/YOURNAME-lambda-dynamodb-role.

#### Create the function

We will be using the CLI/CloudShell to create the function. You can of course use the console as well.&#x20;

* [ ] Make a file called index.js and paste this code into it

{% code title="index.js" %}
```javascript
console.log('Loading function');
exports.handler = function(event, context, callback) {   
    console.log(JSON.stringify(event, null, 2)); 
    event.Records.forEach(function(record) { 
        console.log(record.eventID); 
        console.log(record.eventName); 
        console.log('DynamoDB Record: %j', record.dynamodb); 
    }); 
    callback(null, "This is a message from Lambda"); 
};
```
{% endcode %}

Create a deployment package with&#x20;

```
zip function.zip index.js
```

* [ ] Then create the function with the following command&#x20;

Replace the correct ARN for your execution role.

Change the name of the function on line 1 by replacing YOURNAME.&#x20;

```
aws lambda create-function --function-name YOURNAME-ProcessDynamoDBRecords \
--zip-file fileb://function.zip \
--handler index.handler \
--runtime nodejs12.x \
--role ADD THE ARN OF YOUR ROLE HERE
```

#### Test the function

Create a file called input.txt and paste the following into it

{% code title="input.txt" %}
```json
{ "Records":[ 
{ "eventID":"1", "eventName":"INSERT", "eventVersion":"1.0", "eventSource":"aws:dynamodb", 
"awsRegion":"us-east-1", "dynamodb":{ "Keys":{ "Id":{ "N":"101" } }, 
"NewImage":{ "Message":{ "S":"New item!" }, "Id":{ "N":"101" } }, 
"SequenceNumber":"111", "SizeBytes":26, 
"StreamViewType":"NEW_AND_OLD_IMAGES" }, 
"eventSourceARN":"stream-ARN" }, 
{ "eventID":"2", "eventName":"MODIFY", "eventVersion":"1.0", "eventSource":"aws:dynamodb", "awsRegion":"us-east-1", 
"dynamodb":{ "Keys":{ "Id":{ "N":"101" } }, 
"NewImage":{ "Message":{ "S":"This item has changed" }, "Id":{ "N":"101" } }, 
"OldImage":{ "Message":{ "S":"New item!" }, "Id":{ "N":"101" } }, 
"SequenceNumber":"222", "SizeBytes":59, "StreamViewType":"NEW_AND_OLD_IMAGES" }, 
"eventSourceARN":"stream-ARN" }, { "eventID":"3", "eventName":"REMOVE", "eventVersion":"1.0", 
"eventSource":"aws:dynamodb", "awsRegion":"us-east-1", "dynamodb":{ "Keys":{ "Id":{ "N":"101" } }, 
"OldImage":{ "Message":{ "S":"This item has changed" }, "Id":{ "N":"101" } }, 
"SequenceNumber":"333", "SizeBytes":38, "StreamViewType":"NEW_AND_OLD_IMAGES" }, 
"eventSourceARN":"stream-ARN" } ] }
```
{% endcode %}

Then run this command

```
aws lambda invoke --function-name YOURNAME-ProcessDynamoDBRecords \
 --payload fileb://input.txt outputfile.txt
```

You can verify that the **outputfile.txt** has the message "This is a message from Lambda".

### Enable the stream

* [ ] Navigate to **DynamoDB** and the **Dogs** table
* [ ] Go to the tab **exports and streams**
* [ ] Under DynamoDB Streams details, click **Enable**
* [ ] Select **New and old images**
* [ ] &#x20;Click **Enable**.&#x20;

![](<../../.gitbook/assets/image (398).png>)

There are different choices for what information is written to the stream. The options are&#x20;

* **Key attributes only** — Only the key attributes of the modified item.
* **New image** — The entire item, as it appears after it was modified.
* **Old image** — The entire item, as it appeared before it was modified.
* **New and old images** — Both the new and the old images of the item.

New and old images gives us the most information.&#x20;

The stream is now created. Take note of the ARN of the stream. &#x20;

arn:aws:dynamodb:aws-region:123412341234:table/tablename/stream/yyyy-mm-ddThh:mm:ss

### Create the Event Source Mapping

Next we need to create an event source mapping. This way Lambda knows to poll for new events in the stream. Run the following command, using the ARN of the stream we just created:

```
aws lambda create-event-source-mapping \
--function-name YOURNAME-ProcessDynamoDBRecords \
--starting-position LATEST \
--event-source-arn ADD ARN OF STREAM HERE
```

![output](<../../.gitbook/assets/image (211) (1).png>)

### Make changes to the table

Next we want to test if this mapping works. Make changes to the DynamoDB table: edit, create and delete some items. You can verify from CloudWatch logs that the Lambda function gets triggered.

* Navigate to **CloudWatch**&#x20;
* Go to **Logs > Log groups**
* Look for log group **/aws/lambda/YOURNAME-ProcessDynamoDBRecords**

You should see Log events. Once expanded, you will see the events contain information like this:

![item has been edited](<../../.gitbook/assets/image (429).png>)

### End result

There you have it: a Lambda function that gets triggered when an item in a DynamoDB table gets created, modified or deleted. Our example is quite modest, but it does illustrate the main principle.&#x20;
