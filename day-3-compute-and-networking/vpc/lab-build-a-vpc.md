# LAB: Build a VPC (console)

We are going to build a VPC. It will have the following:

* A VPC with CIDR block **10.0.0.0/16**
* Two subnets:&#x20;
  * one _private_ with CIDR block **10.0.1.0/24**&#x20;
  * one _public_ with CIDR block **10.0.2.0/24**
* An Internet gateway
* Two route tables.

#### CIDR overlap

If your AWS account already has non-default VPC's - that is, VPC's with CIDR block other than 172.31.0.0/16 - then make sure that the CIDR blocks of those VPC's **do not overlap** with 10.0.0.0/16.

If you are not sure, please ask your instructor.&#x20;

![Our VPC](<../../.gitbook/assets/image (20) (1).png>)

## Create the VPC

Navigate to the VPC console.

* On the left hand side panel, go to **Your VPCs.**&#x20;
* Click the **Create VPC** button (top right corner)

![](<../../.gitbook/assets/image (97).png>)

* We want **IPv4 CIDR manual input** for our CIDR block&#x20;
* IPV4 CIDR: **10.0.0.0/16**
* No IPv6 block&#x20;
* Tenancy: **default**&#x20;
* Skip the tags&#x20;
* **Create VPC.**

There it is, our new VPC!&#x20;

![VPC details page](<../../.gitbook/assets/image (293).png>)

### Main route table

Notice that there is also a route table that has been created. This is the main route table of the new VPC.&#x20;

Click on the route table ID.&#x20;

Name this route table **yourname-vpc-main-rt.**

![main route table of new vpc](<../../.gitbook/assets/image (21).png>)

If you click on the route table ID and look at the Routes tab, you will see that this main route table does not have a route to the IGW (In fact, this VPC does not even have an IGW). This is different from the default VPC.&#x20;

The use case for the default VPC is to provide internet connectivity to AWS resources as easily and transparently as possible. Now we are creating our own VPC and we have complete control over which subnets have internet connectivity, if any.&#x20;

Therefore all new subnets in this non-default VPC will be implicitly associated with the route table, which only has a "local" route. That means all subnets in this VPC are able to communicate with each other, but nothing outside of the VPC.&#x20;

## Create the subnets

Next we want to create two subnets.&#x20;

* On the left side panel, go to **Subnets**.
* Click **Create subnet** (button on the top right corner)

![](<../../.gitbook/assets/image (329).png>)

* From the drop down choose the newly created **yourname-vpc.**
* Under subnet settings, for the first subnet:
  1. Subnet name: **yourname-private**
  2. AZ: no preference
  3. CIDR block: **10.0.1.0/24**
* Click **Add new subnet**
* Under subnet settings, for the second subnet:
  1. Subnet name: **yourname-public**
  2. AZ: no preference
  3. CIDR block: **10.0.2.0/24**
* Click **Create subnets.**&#x20;

We have our two subnets ready.&#x20;

## Create the Internet gateway

If we want our VPC to be able to communicate with the internet, we need to create an IGW. A VPC can only have one IGW and all internet traffic goes through it.&#x20;

1. On the left hand side panel, go to **Internet gateways**
2. Click&#x20;
3. Give it a name: **yourname-vpc-igw**
4. Create **Internet gateway**

This IGW needs to be attached to a VPC. You should see a green banner and a button on the right hand side that says **Attach to VPC.**&#x20;

* select the **yourname-vpc**
* **Attach**.&#x20;

#### Situation so far

We have a VPC with an IGW and two subnets. So we're done, right? Not quite.&#x20;

## Create Route tables

Route tables are what determine where traffic in your VPC goes. The real difference between a private and public subnet is that the public subnet's route table has a route to the IGW whereas the private one's doesn't.&#x20;

### Main route table&#x20;

When we created our VPC, it also created the main route table that we named **yourname-vpc-main-rt.**

The two subnets that we created in this VPC are both implicitly associated with this route table. Ths ensures that the two subnets can communicate with each other.&#x20;

Our main route table does not have a route to the IGW, so now both our subnets are private (despite the fact that one is called public).&#x20;

![our subnets use the main route table. ](<../../.gitbook/assets/image (224) (1).png>)

### Public route table

The private subnet yourname-private can keep using the main route table. However we need to create another route table for the yourname-public subnet, to give it internet access.&#x20;

We just need to&#x20;

* create a new route table
* add a route to the IGW in it&#x20;
* associate the route table with the yourname-public subnet.&#x20;

#### Create route table

* Navigate to **Route tables**
* Click the **Create route table** button (top right corner)&#x20;

![so easy ](<../../.gitbook/assets/image (207) (1).png>)

* name the route table **yourname-vpc-public-rt**
* select **yourname-vpc**
* click **Create route table.**&#x20;

#### **Add route to IGW**

The newly created route table still only has one route in it: the local one. We need to add a route to the IGW.&#x20;

* In the **Routes** tab, click button **Edit routes.**
* Click button **Add route**

![adding route](<../../.gitbook/assets/image (243).png>)

* For Destination choose **0.0.0.0/0**
* For Target, pick **Internet Gateway**&#x20;
* Pick the **yourname-vpc-igw**
* Click **Save changes.**&#x20;

Now the new route table has two routes:

![new routes.](<../../.gitbook/assets/image (327).png>)

### End result

We have now successfully created a VPC that has two subnets: one with internet access and one without.&#x20;

![Our VPC](<../../.gitbook/assets/image (451).png>)
