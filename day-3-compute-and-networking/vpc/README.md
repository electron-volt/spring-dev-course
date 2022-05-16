# VPC

Amazon Virtual Private Cloud (VPC) is a virtual network in the cloud.&#x20;

## Key concepts&#x20;

* **Virtual private cloud (VPC)** — A virtual network dedicated to your AWS account.
* **Subnet** — A range of IP addresses in your VPC. A subnet can be private (no internet access) or public (has internet access).&#x20;
* **Route table** — A set of rules, called routes, that are used to determine where network traffic is directed.&#x20;
* **Internet gateway** — A gateway that you attach to your VPC to enable communication between resources in your VPC and the internet.

What we mean by public subnet is really: a subnet that has a route to the IGW in its route table. Similarly a private subnet is a subnet that does not have a route to the IGW in its route table.&#x20;

![example VPC](<../../.gitbook/assets/image (108).png>)

In this example we have a VPC with an internet gateway. There is a private subnet and a public subnet. The public subnet's route table has a route to the internet (destination 0.0.0.0/0 has target igw).&#x20;

## Default VPC

If you are using a personal AWS account, then each region will have a default VPC that is created automatically.&#x20;

If your AWS account is centrally managed by an AWS organization, then the default VPC may be replaced with your organization's VPC.&#x20;

The default VPC has:

* an internet gateway &#x20;
* a VPC with size /16 IPv4 CIDR block 172.31.0.0/16
* size /20 default subnets in each AZ.

All the subnets in the default VPC are public.&#x20;

### Why a default VPC?&#x20;

New AWS accounts come out-of-the-box with this default VPC so that customers who are interested in creating web applications or hosting static web content in their account can immediately get started with their deployments.&#x20;

They do not need to know anything about networking, internet gateways, CIDR blocks, public IP assignment settings in subnets, route tables or NACL's.&#x20;

We are not that lucky - we have to get our hands dirty and spend some time understanding all this complex networking stuff!&#x20;

To help us do that, let's first take a closer look at the default VPC.&#x20;

