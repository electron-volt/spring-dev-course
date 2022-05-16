# LAB: Default VPC walkthrough

#### If using a personal AWS account&#x20;

Your account should already have a default VPC in every region.&#x20;

#### If using a centrally managed account

If your organization has removed the default VPC's, then you can create one via the CLI or CloudShell using this command:

```
aws ec2 create-default-vpc
```

## VPC

First let's navigate to **VPC > Your VPCs.**&#x20;

Here is a view of the default VPC.&#x20;

![](<../../.gitbook/assets/image (220).png>)

1. Choose **your VPCs** from the left panel.&#x20;
2. The default VPC will have CIDR 172.31.0.0/16.&#x20;
3. Click on the **-** in the Name column.&#x20;

![naming a VPC](<../../.gitbook/assets/image (113).png>)

Once you click on the icon (square with a line through the corner), a dialogue appears prompting you for a name.

* Name the VPC **default-vpc.**
* Click **Save.**&#x20;

#### VPC ID

In some views in the console you will only see the VPC ID, not the name default VPC.&#x20;

To make your default VPC easier to recognize, notice the last three characters in the VPC ID. In my case the ID ends in "b2c".

Just make a mental note of the last three characters.&#x20;

## Subnets

On the left side panel, click on **Subnets**.&#x20;

Click on the search box that says "**filter subnets**" and select **VPC**.&#x20;

![filtering based on VPC](<../../.gitbook/assets/image (310).png>)

From the dropdown, pick your default VPC. You will recognize it based on the last three characters of the ID. &#x20;

![subnets in default VPC](<../../.gitbook/assets/image (258).png>)

There are three subnets in the default VPC.&#x20;

Here I have already named the subnets, using the same method as for the VPC. You will only see a - in the name column for all three subnets.&#x20;

* Click on the subnet ID of any one of the subnets.&#x20;

You will see a summary of information under Details.&#x20;

![subnet details](<../../.gitbook/assets/image (185).png>)

Notice the Availability zone that ends in 1b?&#x20;



