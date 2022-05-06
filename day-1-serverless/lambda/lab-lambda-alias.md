# LAB: Lambda alias

## Creating a prod alias

Let's pretend our Lambda **my-cli-lambda** will actually run in production. We have made the necessary changes and published version 2. Now we want to create an alias for the version, so we do not need to remember the version number.&#x20;

### Navigate to version 2

In the Lambda console, in the Versions tab, click on the version 2:

![Click 2](<../../.gitbook/assets/image (93).png>)

You should land on a tab called Configuration. Under Aliases, click **+ Create alias.**&#x20;

![Create alias](<../../.gitbook/assets/image (108) (1).png>)

A dialogue appears.&#x20;

* Name: **prod**
* Description: alias that points to production-ready code
* click **Save**.&#x20;

![creating an alias](<../../.gitbook/assets/image (382).png>)

A green banner that says "Alias prod was successfully created and is now pointing to version 2" is a sign of success.&#x20;

![we did it! ](<../../.gitbook/assets/image (30).png>)

### ARN of an alias

You can see in the console that the ARN of this new alias is&#x20;

**arn:aws:lambda:aws-region:acct-id:function:my-cli-lambda:prod.**

This is how customers would invoke your production function. You are free to change the version the alias points to - the ARN remains the same.&#x20;

You can always see which version of your function the alias is pointing to by navigating to the $LATEST, then opening tab **Aliases**.&#x20;

![Aliases](<../../.gitbook/assets/image (95).png>)
