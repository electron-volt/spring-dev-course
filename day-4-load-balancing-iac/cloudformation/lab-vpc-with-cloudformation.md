# LAB: VPC with CloudFormation

Life's about to get so easy.&#x20;

Here is a template that creates&#x20;

* a VPC with CIDR block 10.0.0.0/16
* an internet gateway&#x20;
* a public subnet with CIDR 10.0.0.0/24
* a private subnet with CIDR 10.0.1.0/24
* the route tables for both private and public subnets.

All you need to do is

1. Save the template on your machine as **vpc-template-1.json**
2. Change "yourname" to your actual name&#x20;
3. Create a CloudFormation stack called **yourname-vpc-stack** with the template.&#x20;

```
{
    "AWSTemplateFormatVersion": "2010-09-09",
    "Description": "Deploys a VPC with an Internet gateway with two subnets: one public and one private.",
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
        }
    }
}
```

## End result

![results in the console](<../../.gitbook/assets/image (198).png>)

![](<../../.gitbook/assets/image (407).png>)

### Do NOT delete stack yet&#x20;

Keep the stack and the VPC and move on to the next lab please.
