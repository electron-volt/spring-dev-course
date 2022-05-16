# LAB: Auto-scaling

In this lab we are going to create&#x20;

* a launch template that launches an Amazon Linux EC2 instance
* an auto-scaling group that ensures that we always have at least and at most one instance running at all times.&#x20;

We will have the auto-scaling group create an instance. We will then terminate the instance - to simulate an error - and watch as the ASG automatically creates a new one.&#x20;

## VPC

In this lab we will be working with the default VPC. It already has a public subnet in every AZ, with auto-assign public IP enabled.&#x20;

It is useful to know&#x20;

* the VPC ID of the default VPC, or at least the last 3 characters of the ID
* the security group ID of the default security group of the default VPC.&#x20;

You can find this information from the console.

## Create a launch template

Let's navigate to the EC2 console.&#x20;

On the left side panel, under Instances you will see Launch templates.&#x20;

#### Side note

On the same left side panel, you may notice that at the very bottom it says Auto Scaling and Launch configurations. Launch configurations are a legacy feature. If you try to create a launch configuration, you will see a banner that recommends launch templates instead.&#x20;

The key difference is that a launch template can be modified after it's created, whereas a configuration is immutable. If you want to make changes to a configuration, you have to delete it and create a new one.&#x20;

In the Launch template console,&#x20;

* Click **Create launch template**
* Name: **yourname-template**
* Check the box next to **Auto Scaling guidance**

![](<../../.gitbook/assets/image (134).png>)

* For **Application and OS images**, choose Quick start and then
  1. Amazon Linux
  2. Amazon Linux 2 AMI
  3. 64-bit x86&#x20;

![hey I've seen this before!](<../../.gitbook/assets/image (180).png>)

* For instance type, pick the one that is free-tier eligible
* For key-pairs, choose **yourname-keys**&#x20;
* For network settings,&#x20;
  1. For subnet: don't include in launch template (we will let the ASG decide)
  2. For security group: choose the default security group of the default VPC. Here is where you need the last three characters of the Id's.
* For storage: keep the defaults
* Skip Tags and advanced details
* Click **Create launch template.**

## Create an auto-scaling group&#x20;

In the EC2 console, on the left side panel scroll all the way down to **Auto Scaling.**&#x20;

Select **auto scaling groups.**&#x20;

* Click **Create auto-scaling group**&#x20;
* Name: **yourname-asg**
* Launch template: choose **yourname-template**
* Click **Next**

![](<../../.gitbook/assets/image (130).png>)

In Network

* VPC: pick the **default VPC**
* For subnets, **click all of them**

![](<../../.gitbook/assets/image (405).png>)

* Skip instance type requirements: these come from the launch template
* Click **Next**

Skip the page that starts with Configure advanced options. Click Next to get to Configure group size and scaling policies.

* Group size: Desired/minimum/maximum are all 1.&#x20;
* Scaling policies: None

![](<../../.gitbook/assets/image (322).png>)

* You can click **Skip to review**
* Then click **Create**.

The ASG has been created.&#x20;

![The yourname-asg.](<../../.gitbook/assets/image (53).png>)

## Test the ASG

### Verify that it worked

Give the ASG some time to work its magic and then go see the list of EC2 instances. There is an instance running!&#x20;

### Terminate the instance

Let's manually terminate the EC2 instance. You will be notified that the instance is in an auto-scaling group.&#x20;

Click **Stop**.

![](<../../.gitbook/assets/image (273).png>)

The auto-scaling group will do the following:&#x20;

* it will detect that we do not have a running instance&#x20;
* it will realize that our minimum and desired capacity is 1
* it will realize that 0 is not 1&#x20;
* it will launch a new EC2 instance.

You can watch this happen if you go to Auto Scaling > Auto Scaling Groups > click on yourname-asg and the Activity tab.&#x20;

Keep refreshing Activity history. You will see the ASG launches a new instance. &#x20;

![](<../../.gitbook/assets/image (203).png>)



## Clean up

Delete the **yourname-asg** ASG. You will notice the new EC2 instance is also terminated.&#x20;

The launch template does not need to be deleted. We can use it in a future lab.&#x20;
