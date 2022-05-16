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

* Click on the subnet ID of the first subnet on the list. &#x20;

You will see a summary of information under Details.&#x20;

![subnet details](<../../.gitbook/assets/image (185).png>)

On the details page you can see which Availability Zone the subnet is in.&#x20;

Scroll down and select the tab **Route table.**&#x20;

![route table](<../../.gitbook/assets/image (309).png>)

Here you can see the main route table of the default VPC.

### Name the subnets

* Go back to the Subnets page.&#x20;
* Click on the - in the Name column next to the first subnet, whose details we just looked at.&#x20;
* Name the subnet **default-public-1x** where x stands for the last letter of the AZ that the subnet is in.&#x20;
  * that is, if your subnet is in eu-central-1a, call the subnet default-public-1a.&#x20;

Repeat these steps for all three subnets in the default VPC.&#x20;

## Route table&#x20;

Now we have named our VPC and its subnets, to make them easier to recognize.

Let's now name the main route table.&#x20;

* Navigate to **Route tables** in the left side panel.
* Use the filter to pick out your default VPC.

![route tables](<../../.gitbook/assets/image (271).png>)

* Click on the - in the Name column&#x20;
* Name the route table **default-vpc-main-rt.**

#### Subnet associations

Notice how this route table has no explicit subnet associations?&#x20;

* Click on the **Route table ID**
* Click on the tab **Subnet associations**

You should see the following:

![](<../../.gitbook/assets/image (150).png>)

In the default VPC, there is one main route table. It has a route to the IGW. All subnets are implicitly associated with this route table, giving them a route to the internet and thus making them public subnets.&#x20;

We will see later that the behaviour is different for non-default VPC's.&#x20;

## IGW

As you have probably guessed, we will pick the next item from the left side panel, the IGW and give it a name.&#x20;

* Select **Internet gateways** from the left side panel
* Filter for your VPC ID&#x20;
* Click the - in the Name column&#x20;
* Name your IGW **default-vpc-igw.**

![igw once it has been named.](<../../.gitbook/assets/image (142).png>)

### End result

We have taken a little tour of the default VPC. Now we are ready to create one of our own!&#x20;
