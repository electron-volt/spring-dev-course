# LAB: First template

In this lab we will use a very simple CloudFormation template to launch an EC2 instance.&#x20;

## Template

The template below will create one EC2 instance with a given AMI and an instance type.&#x20;

**NOTE**:&#x20;

We will be creating an EC2 instance. It will get created **in the default VPC.** For the lab to work, please go into a region where your AWS account has a default VPC or create the default VPC first.&#x20;

#### Parameters for the template

Depending on which region you are in, look up the following information from the EC2 console:

* the AMI ID of the Amazon Linux 2 AMI&#x20;
* which instance type is free-tier eligible.&#x20;

#### Finding values

You can find these values for example by going to the EC2 console and clicking launch instance.&#x20;

Then click **cancel** to exit without launching an instance.&#x20;

Here is a screenshot from the eu-north-1 region:

![eu-north-1 values](<../../.gitbook/assets/image (451).png>)

#### Create template

* Save the following on your computer as **EC2instance.yml.**&#x20;

If you are not in the eu-north-1 region:

* Replace the ImageId on line 5 with the correct AMI ID&#x20;
* Replace the InstanceType on line 6 with correct the instance type.

{% code title="EC2instance.yml" %}
```yaml
Resources:    
  TestInstance:  
    Type: AWS::EC2::Instance
    Properties:
      ImageId: ami-08bdc08970fcbd34a
      InstanceType: t3.micro
```
{% endcode %}

## Create stack

Navigate to the **CloudFormation** console.&#x20;

If the region already has stacks in it, you will probably be taken to the list of stacks.&#x20;

If not, you will land on a landing page.&#x20;

In both cases there will be a **Create stack** button.&#x20;

![](<../../.gitbook/assets/image (13).png>)

* Click **Create stack.**
* Choose "Template is ready"&#x20;
* Specify template: upload a template file&#x20;
* Then upload the file we just created.
* Click **Next**.
* Name your stack **yourname-ec2-stack**
* Leave all other settings as they are and just click **Next** until you get to "**create stack"**.&#x20;

## Monitor

In the CloudFormation console, you see all the events of your stack creation:

![instance creation](<../../.gitbook/assets/image (155).png>)

The instance has a logical ID of TestInstance because of line 2 of the EC2instance.yml file. If that line 2 had said SpongeBob instead, then the logical ID of the instance would be SpongeBob.

You can go to EC2 > instances to verify that an instance has been created:

![Instance is up and running](<../../.gitbook/assets/image (273) (1).png>)

Back in the CloudFormation console, click the tab Resources:

![Resources show the instance](<../../.gitbook/assets/image (26).png>)

## Clean up&#x20;

Let's delete the stack. By deleting the stack, the instance also gets deleted.

There is a Delete button you can click.

The stack's status changes to DELETE IN PROGRESS.

The Events tab shows the short-lived life of our instance and stack:

![](<../../.gitbook/assets/image (330).png>)

You may have to refresh the view to see newer events.
