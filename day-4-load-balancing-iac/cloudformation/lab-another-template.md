# LAB: Another template

In this lab we want to expand on our first template.&#x20;

## Starting point

This is the template that we started with, along with comments that explain it line by line:

```yaml
Resources:                           # mandatory 
  TestInstance:                      # you could name this anything 
    Type: AWS::EC2::Instance         # this line states we want an EC2 instance
    Properties:
      ImageId: ami-06bfd6343550d4a29 # AMI to use 
      InstanceType: t3.micro         # instance type to use
```

## New template

Take a moment to read through this longer template:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: 'This stack creates an EC2 instance.'
Parameters:
  KeyName:
    Description: Name of an existing EC2 KeyPair to enable SSH access to the instance
    Type: AWS::EC2::KeyPair::KeyName
    ConstraintDescription: must be the name of an existing EC2 KeyPair.
  InstanceType:
    Description: EC2 instance type
    Type: String
    Default: t3.micro
    AllowedValues: [t2.nano, t2.micro, t3.micro]
    ConstraintDescription: must be a valid EC2 instance type.
Mappings: 
  RegionMap: 
    eu-north-1: 
      "HVM64": "ami-08bdc08970fcbd34a"
    eu-west-1: 
      "HVM64": "ami-0c1bc246476a5572b"
    eu-central-1:
      "HVM64": "ami-09439f09c55136ecf"
Resources:
  EC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: !Ref 'InstanceType'
      KeyName: !Ref 'KeyName'
      ImageId: !FindInMap [RegionMap, !Ref "AWS::Region", HVM64]
Outputs:
  InstanceId:
    Description: InstanceId of the newly created EC2 instance
    Value: !Ref 'EC2Instance'
  AZ:
    Description: Availability Zone of the newly created EC2 instance
    Value: !GetAtt [EC2Instance, AvailabilityZone]
  PublicIP:
    Description: Public IP address of the newly created EC2 instance
    Value: !GetAtt [EC2Instance, PublicIp]
```

#### Instance types

In eu-central-1 and eu-west-1, the free tier eligible instance type is t2.micro. In eu-north-1 it is t3.micro.&#x20;

Eu-north-1 is a very new region and AWS no longer support the "older generation" t2.micro instance type there.&#x20;

### Parameters

When we create a stack with this template, we will be asked to provide the following information:

* a stack name (that always happens, not specific to this template)
* a key name (with a dropdown of all key pairs in the region we are in)&#x20;
* an instance type (t3.micro will be selected, but the dropdown will also have two other options).

This is a huge improvement from the first template, where we had to hard-code these values in the template itself.&#x20;

### Mappings

Here is where we make sure that the template will launch the Amazon Linux 2 AMI, independent of whether we are in Stockholm, Ireland or Frankfurt. The template will use the correct AMI ID for the region.&#x20;

### Resources

This template will launch only one AWS resource: an EC2 instance. It will have properties

* Instance type: selected by the user&#x20;
* Key pair: selected by the user
* Image id: determined by the region that the user is in.

### Outputs

The template will output

* The instance ID of the launched instance, of the form i-123abc456def.
* The AZ that the instance is in
* the public IP address.

Since we did not specify a VPC or a security group, the instance will be launched in some public subnet of the default VPC and will be associated with the default security group of the VPC.

### Save it

Save the template on your computer as **yourname-template.yml.**

## Ireland 🍀

Let's log in to the AWS console, but switch to the eu-west-1 region.&#x20;

You can use eu-central-1 or eu-north-1 as well. For the lab to work, use a region where you have at least 1 key pair and a default VPC.&#x20;

### Key pair

We need to have a key pair in the Ireland region for this lab to work, so if you do not have one then let's create it.

1. Go to **EC2**
2. Under **Network & security** click **Key pair**
3. **Create key pair**
4. Name it **yourname-irish-keys.**
5. Pick type **RSA** and **.pem**&#x20;

Note: We won't actually be SSH'ing into the instance.

### Create the stack&#x20;

Next - while still in the Ireland region - go to CloudFormation.&#x20;

Create a stack using the **yourname-template.yml** file.&#x20;

If in a shared account, name the stack **yourname-irish-stack.**

#### **Stack parameters**

You will be asked for an instance type and a key pair.&#x20;

&#x20;

![instanceType parameter and its dropdown menu](<../../.gitbook/assets/image (143).png>)

* pick an instance type
* pick the keys that you have created in the irish region.&#x20;
* click **Next** until you get to **Create stack.**&#x20;

Once the stack is in state CREATE COMPLETE, you can view the outputs:

![So cool](<../../.gitbook/assets/image (379) (1).png>)

### Clean up&#x20;

Delete the stack. This will also delete the instance.&#x20;

About the key pair: we are going to need a key pair in the Ireland region on day 5, when we look at containers. You do not have to delete the key pair.&#x20;
