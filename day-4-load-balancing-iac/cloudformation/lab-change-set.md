# LAB: Change set

So we have our VPC with one private and one public subnet. All is good - but what if we needed more subnets?&#x20;

Do we delete the stack, change the template and create a new stack?&#x20;

No - we create a change set!&#x20;

## New template

Save this on your machine as **vpc-template-2.json**

Change **yourname** to your actual name - the same one you used in **vpc-template-1.json.**&#x20;

{% code title="vpc-template-2.json" %}
```
{
    "AWSTemplateFormatVersion": "2010-09-09",
    "Description": "Deploys a VPC with an Internet gateway with three subnets: one public and two private.",
    "Resources": {
        "VPC": {
            "Type": "AWS::EC2::VPC",
            "Properties": {
                "CidrBlock": "10.0.0.0/16",
                "EnableDnsHostnames": true,
                "Tags": [
                    {
                        "Key": "Name",
                        "Value": "yourname-VPC"
                    }
                ]
            }
        },
        "InternetGateway": {
            "Type": "AWS::EC2::InternetGateway",
            "Properties": {
                "Tags": [
                    {
                        "Key": "Name",
                        "Value": "yourname-IGW"
                    }
                ]
            }
        },
        "AttachGateway": {
            "Type": "AWS::EC2::VPCGatewayAttachment",
            "Properties": {
                "VpcId": {
                    "Ref": "VPC"
                },
                "InternetGatewayId": {
                    "Ref": "InternetGateway"
                }
            }
        },
        "PublicSubnet1": {
            "Type": "AWS::EC2::Subnet",
            "Properties": {
                "VpcId": {
                    "Ref": "VPC"
                },
                "CidrBlock": "10.0.0.0/24",
                "AvailabilityZone": {
                    "Fn::Select": [
                        "0",
                        {
                            "Fn::GetAZs": ""
                        }
                    ]
                },
                "Tags": [
                    {
                        "Key": "Name",
                        "Value": "yourname Public Subnet 1"
                    }
                ]
            }
        },
        "PrivateSubnet1": {
            "Type": "AWS::EC2::Subnet",
            "Properties": {
                "VpcId": {
                    "Ref": "VPC"
                },
                "CidrBlock": "10.0.1.0/24",
                "AvailabilityZone": {
                    "Fn::Select": [
                        "0",
                        {
                            "Fn::GetAZs": ""
                        }
                    ]
                },
                "Tags": [
                    {
                        "Key": "Name",
                        "Value": "yourname Private Subnet 1"
                    }
                ]
            }
        },
        "PublicSubnet2": {
            "Type": "AWS::EC2::Subnet",
            "Properties": {
                "VpcId": {
                    "Ref": "VPC"
                },
                "CidrBlock": "10.0.2.0/24",
                "AvailabilityZone": {
                    "Fn::Select": [
                        "1",
                        {
                            "Fn::GetAZs": ""
                        }
                    ]
                },
                "Tags": [
                    {
                        "Key": "Name",
                        "Value": "yourname Public Subnet 2"
                    }
                ]
            }
        },
        "PrivateSubnet2": {
            "Type": "AWS::EC2::Subnet",
            "Properties": {
                "VpcId": {
                    "Ref": "VPC"
                },
                "CidrBlock": "10.0.3.0/24",
                "AvailabilityZone": {
                    "Fn::Select": [
                        "1",
                        {
                            "Fn::GetAZs": ""
                        }
                    ]
                },
                "Tags": [
                    {
                        "Key": "Name",
                        "Value": "yourname Private Subnet 2"
                    }
                ]
            }
        },
        "PublicRouteTable": {
            "Type": "AWS::EC2::RouteTable",
            "Properties": {
                "VpcId": {
                    "Ref": "VPC"
                },
                "Tags": [
                    {
                        "Key": "Name",
                        "Value": "yourname Public Route Table"
                    }
                ]
            }
        },
        "PublicRoute": {
            "Type": "AWS::EC2::Route",
            "Properties": {
                "RouteTableId": {
                    "Ref": "PublicRouteTable"
                },
                "DestinationCidrBlock": "0.0.0.0/0",
                "GatewayId": {
                    "Ref": "InternetGateway"
                }
            }
        },
        "PublicSubnetRouteTableAssociation1": {
            "Type": "AWS::EC2::SubnetRouteTableAssociation",
            "Properties": {
                "SubnetId": {
                    "Ref": "PublicSubnet1"
                },
                "RouteTableId": {
                    "Ref": "PublicRouteTable"
                }
            }
        },
        "PublicSubnetRouteTableAssociation2": {
            "Type": "AWS::EC2::SubnetRouteTableAssociation",
            "Properties": {
                "SubnetId": {
                    "Ref": "PublicSubnet2"
                },
                "RouteTableId": {
                    "Ref": "PublicRouteTable"
                }
            }
        },
        "PrivateRouteTable": {
            "Type": "AWS::EC2::RouteTable",
            "Properties": {
                "VpcId": {
                    "Ref": "VPC"
                },
                "Tags": [
                    {
                        "Key": "Name",
                        "Value": "yourname Private Route Table"
                    }
                ]
            }
        },
        "PrivateSubnetRouteTableAssociation1": {
            "Type": "AWS::EC2::SubnetRouteTableAssociation",
            "Properties": {
                "SubnetId": {
                    "Ref": "PrivateSubnet1"
                },
                "RouteTableId": {
                    "Ref": "PrivateRouteTable"
                }
            }
        },
        "PrivateSubnetRouteTableAssociation2": {
            "Type": "AWS::EC2::SubnetRouteTableAssociation",
            "Properties": {
                "SubnetId": {
                    "Ref": "PrivateSubnet2"
                },
                "RouteTableId": {
                    "Ref": "PrivateRouteTable"
                }
            }
        }
    },
    "Outputs": {
        "VPC": {
            "Description": "VPC",
            "Value": {
                "Ref": "VPC"
            }
        },
        "AZ1": {
            "Description": "Availability Zone 1",
            "Value": {
                "Fn::GetAtt": [
                    "PublicSubnet1",
                    "AvailabilityZone"
                ]
            }
        },
        "AZ2": {
            "Description": "Availability Zone 2",
            "Value": {
                "Fn::GetAtt": [
                    "PublicSubnet2",
                    "AvailabilityZone"
                ]
            }
        }
    }
}no
```
{% endcode %}

## Change set

In the CloudFormation console, go to your **yourname-vpc** stack and click on the **Change sets** tab.

Then click the **Create change set** button.&#x20;

Select Replace current template.&#x20;

Upload a template file: **vpc-template-2.json.**&#x20;

Click **next**, next, next until you get to **Create change set.**&#x20;

**Give the change set a name.** Description can be left blank.

Click **Create change set.**&#x20;

![](<../../.gitbook/assets/image (14).png>)

### See changes

Give the change set a moment to get ready.&#x20;

Eventually you should see the **Changes** tab:

![ch-ch-ch-changes](<../../.gitbook/assets/image (119).png>)

### Execute

There is an orange Execute button on the top right corner. Click it.&#x20;

We can proceed with "roll back all stack resources".

Click **Execute change set.**&#x20;

![](<../../.gitbook/assets/image (342).png>)

## End result&#x20;

Now we have four subnets!&#x20;

![](<../../.gitbook/assets/image (199).png>)

We could also have achieved the same result if we had clicked Update. However in that situation we would not have had the option to preview the changes, like we did with change sets.&#x20;

Change sets are a more secure way of deploying changes to your pre-existing stacks.

### NO Clean up&#x20;

We can use the yourname-vpc in a future lab, so you do not need to delete the stack.&#x20;
