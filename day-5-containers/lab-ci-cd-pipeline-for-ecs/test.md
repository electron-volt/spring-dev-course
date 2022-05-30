# Test

## Approve

In CodePipeline, once the execution has reached the Approve stage, click on **Review**

![yes I had to fix my buildspec ](<../../.gitbook/assets/image (446).png>)



Type an optional comment and then click **Approve**.&#x20;

Execution will move to the last DeployProd stage:

![in progress now](<../../.gitbook/assets/image (98).png>)

## CodeDeploy

Here is what we see in CodeDeploy now:

![CodeDeploy deployment is deploying](<../../.gitbook/assets/image (121).png>)

![](<../../.gitbook/assets/image (212).png>)

## Test it out&#x20;

We can see that our production ALB is now serving the same greeting:

```
save_var PROD_ALB_URL $( \
aws cloudformation describe-stacks \
--stack-name prod-cluster \
--query "Stacks[0].Outputs[?OutputKey == 'ExternalUrl'].OutputValue" \
--output text) 

curl $PROD_ALB_URL/hello/developer
```

Or in the browser:

![](<../../.gitbook/assets/image (399).png>)



## What did we accomplish exactly?&#x20;
