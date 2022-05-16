# LAB: Build a VPC (CLI)

We will do the exact same thing as in the last lab, but this time using the CLI. We will have:

* A VPC with CIDR block **10.1.0.0/16**
* Two subnets:&#x20;
  * one _private_ with CIDR block **10.1.1.0/24**&#x20;
  * one _public_ with CIDR block **10.1.2.0/24**
* An Internet gateway
* two route tables.&#x20;

#### Windows users

These commands are designed for a bash shell. If you are using a Windows machine, please use

* Git Bash or
* AWS CloudShell.

If you really want to use the Windows command line, then the AWS commands will work as-is but setting the shell variables will not work.&#x20;

## Create the VPC

```
VPC=$(aws ec2 create-vpc --cidr-block 10.1.0.0/16 --query Vpc.VpcId --output text)
```

Now there is a shell variable VPC that has the VPC ID. To see the value of this variable, run

```
echo $VPC
```

Later in commands where we need to reference the VPC ID, we will use the variable $VPC.

## Create the subnets

To create our first subnet, run this command:

```
aws ec2 create-subnet --vpc-id $VPC --cidr-block 10.1.1.0/24
```

To create the second subnet, run this command:

```
aws ec2 create-subnet --vpc-id $VPC --cidr-block 10.1.2.0/24
```

The output of these commands will include information such as the subnet ID. Copy the subnet ID of the latter subnet with CIDR 10.1.2.0/24

![output](<../../.gitbook/assets/image (12).png>)

Assign this value to a shell variable&#x20;

```
SUBNET2='your subnet id'
echo $SUBNET2
```

Like this:

![](<../../.gitbook/assets/image (334).png>)

## Create the internet gateway

```
IGW=$(aws ec2 create-internet-gateway --query InternetGateway.InternetGatewayId --output text)
```

This command returns the ID of the IGW, which is now stored as shell variable $IGW.

Attach the IGW to your new VPC (there is no output):

```
aws ec2 attach-internet-gateway --vpc-id $VPC --internet-gateway-id $IGW 
```

## Create route tables

The private subnet - the first one we created - is implicitly associated with the main route table of the new VPC.

We will create the route table that we want to use with the public subnet, with subnet id $SUBNET2.

#### Create route table

&#x20;Here is how we create a new route table:

```
RT=$(aws ec2 create-route-table --vpc-id $VPC --query RouteTable.RouteTableId --output text)
```

Shell variable $RT now has the route table ID of this route table.&#x20;

#### Add route to route table

Next we want to add a route to the IGW into this route table:

```
aws ec2 create-route --route-table-id $RT --destination-cidr-block 0.0.0.0/0 --gateway-id $IGW
```

If it succeeded, the command will return `{"Return": true}.`

#### Associate route table with second subnet

Now we want to associate the subnet with CIDR 10.1.2.0/24 with this new route table:

```
aws ec2 associate-route-table --subnet-id $SUBNET2 --route-table-id $RT
```

If it succeeded, it will return information like association id and state.

## End result&#x20;

We have created a VPC with private and public subnets via the CLI.&#x20;
