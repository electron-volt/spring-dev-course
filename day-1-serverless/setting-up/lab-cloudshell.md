# LAB: CloudShell

Let's navigate to CloudShell. On the left hand side, on top of the screen you'll find a search box. Type in CloudShell.

![Search for CloudShell](<../../.gitbook/assets/image (305).png>)

## Bash shell

Once you've clicked on CloudShell, it will open a terminal inside the AWS console. Give it some time to boot up. Eventually you will arrive at a prompt:

![CloudShell prompt](<../../.gitbook/assets/image (426) (1).png>)

It is a fully fledged Bash shell, as you can see when you play around with it a little:

![I am cloudshell-user](<../../.gitbook/assets/image (126) (1).png>)

## Customization

On the upper right hand corner you will see a button "Actions".&#x20;

![Actions and a cog](<../../.gitbook/assets/image (416).png>)

Here is where you can change the font size and other settings. The cog allows you to toggle dark mode on and off.

## AWS CLI&#x20;

The CloudShell shell comes bundled with many tools, including the AWS CLI. To verify this, run&#x20;

```
aws --version
```

The output will be something like&#x20;

![Current AWS CLI version.](<../../.gitbook/assets/image (24).png>)

## API calls to AWS services

To verify that we are able to make API calls to AWS services from within CloudShell, you can try running the following commands:

```
aws s3 ls
```

This lists all your S3 buckets.&#x20;

```
aws ec2 describe-instances
```

This will list all running EC2 instances in this region, if you have any.&#x20;

Don't worry about the output of the commands or if you understand what they are doing. We are only interested in making sure that Cloudshell is set up correctly.&#x20;
