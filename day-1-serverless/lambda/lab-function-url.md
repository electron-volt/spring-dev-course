# LAB: Function URL

This is a brand new feature that was launched in April of 2022.&#x20;

It used to be the case that it was not possible to invoke your Lambda function through a browser unless you wrote some code or used API Gateway as a front. However with the new feature of function URL's, you will be able to view the output of your Lambda directly in your browser!&#x20;

## Function URL's

We will continue to use the my-cli-lambda for this lab. Make sure you are working in the $LATEST version of your function.&#x20;

* click the tab **Configuration**.&#x20;
* On the left hand side panel, click **Function URL.**&#x20;
* click **Create function URL**

![function URL's](<../../.gitbook/assets/image (294).png>)

A dialogue appears asking about authorization:

![Function URL dialogue](<../../.gitbook/assets/image (245).png>)

For this Lambda function that does not do anything special, we can select NONE. In other cases you could use AWS\_IAM to limit access to your URL to authenticated IAM users and roles.&#x20;

Once you have created the function URL, you can view it in the console:

![new URL ](<../../.gitbook/assets/image (423) (1).png>)

If you click on the URL, you will see the output of your function in your browser:

![Lambda output in the browser](<../../.gitbook/assets/image (384).png>)

### Unintentional edits

Since we created the URL for the $LATEST, I am able to change the function code, hit deploy and changes are immediately reflected in the browser as soon as I refresh the tab:

![changes made to my-cli-lambda](<../../.gitbook/assets/image (167).png>)

![yikes](<../../.gitbook/assets/image (66).png>)

## Optional: URL and alias&#x20;

Obviously it would be a better idea to create the function URL for the alias "prod" instead of the $LATEST.&#x20;

The steps are quite simple, so I will outline them broadly:

* navigate to the prod alias
* you will find the option to add a function URL in the Configuration tab here as well
* create the function URL&#x20;

If you like, you can also create a new version of the code, change the alias prod to point to that version and see if the changes are reflected when you go to the function URL!&#x20;
