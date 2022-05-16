# LAB: EC2 via the CLI

## Creating an EC2 instance with the CLI

We will repeat the steps from the previous lab with the CLI.&#x20;

### Create a key pair

We could use the same key pair that we just used, but just to get a little more practice with the CLI let's create a new one.&#x20;

Windows users: you can follow the same instructions. We will not need to actually SSH into this instance.&#x20;

We will create a key pair called yourname-cli-keys.pem.

Run with the following command, but change the **--key-name on line 2 and the output file name on line 4.**&#x20;

```
aws ec2 create-key-pair \
--key-name YOURNAME-cli-keys \
--query 'KeyMaterial' \
--output text > yourname-cli-keys.pem
```

You can check that the keys were created using&#x20;

```
aws ec2 describe-key-pairs
```

### Create a security group&#x20;

First we must find out what our VPC ID of our default VPC is. Run this:

```
aws ec2 describe-vpcs
```

In the output find the VPC with CidrBlock 172.31.0.0/16.

Copy its VpcId value, it will be vpc-123abcsomething.

Bash users: store it in a shell variable using&#x20;

```
VPC='vpc-asdf123123'
```

Next create the security group called **yourname-cli-sg**: change the value on line 2.&#x20;

If you are not using a Bash shell, then change line 4 to your VpcId.&#x20;

```
aws ec2 create-security-group \
--group-name yourname-cli-sg \
--description "My CLI group" \
--vpc-id $VPC
```

As output, the id of the security group is returned. You will need it in the next phase.

Bash users, copy it and store it as GROUPID.

#### Find your IP

Easy way to find your IP:

```
curl https://checkip.amazonaws.com
```

Bash users: you can store the IP as shell variable MYIP, but add /32 to the end.&#x20;

```
MYIP='a.b.c.d/32'
```

#### Add rules to the security group&#x20;

Allow SSH connections from your IP with this command. Non-Bash users, put your security group id on line 2 and your IP address on line 4. Add your IP in the format  x.x.x.x/32.

```
aws ec2 authorize-security-group-ingress \
--group-id $GROUPID \
--protocol tcp --port 22 \
--cidr $MYIP
```

If the command runs successfully, it will return some JSON.&#x20;

We won't be setting up a web server in this instance so you do not need to allow traffic to port 80.

### Launch an instance

First we will need the ID of the AMI that we used last time. Run the following:

```
aws ec2 describe-instances
```

This returns all the information about our running instances. From the output, we can pick out the ImageId of the AMI that we used to launch the instance.&#x20;

In eu-central-1, the AMI ID is ami-09439f09c55136ecf.&#x20;

We will launch a new instance with this command:

```
aws ec2 run-instances --image-id ami-09439f09c55136ecf \
--count 1 --instance-type t2.micro \
--key-name yourname-cli-keys \
--security-group-ids $GROUPID
```

Note:&#x20;

* if your region's free tier eligible instance type is not t2.micro, then change the type on line 2.
* If you are not using a Bash shell, then use the group id on line 4 instead.&#x20;
* There is no .pem after the key name on line 3.&#x20;

The output will contain information about the recently launched instance. While it is still initializing, it won't have a public IP yet:

{% code title="Snippet of the output" %}
```
            "PrivateDnsName": "..... ",
            "PrivateIpAddress": "......",
            "ProductCodes": [],
            "PublicDnsName": "",
            "State": {
                "Code": 0,
                "Name": "pending"
            },
```
{% endcode %}

Make note of what the instance id of your new instance is. Wait for a little while. Then run&#x20;

`aws ec2 describe-instances --instance-ids <instance id of new instance>`&#x20;

The output now contains the following:

{% code title="snippet of the output" %}
```
                    "PrivateDnsName": ".....",
                    "PrivateIpAddress": ".....",
                    "ProductCodes": [],
                    "PublicDnsName": ".....",
                    "PublicIpAddress": "16.171.8.247",
                    "State": {
                        "Code": 16,
                        "Name": "running"
```
{% endcode %}

We now have the public IP of our instance.&#x20;

We could SSH into it if we wanted, but we will skip that for now. &#x20;

## Cleaning up&#x20;

While our EC2 instances are free tier eligible, we will still delete them straight away.&#x20;

To delete instances via the CLI, run

```
aws ec2 terminate-instances --instance-ids <instance id> 
```

You can find the instance id's of your instances with&#x20;

`aws ec2 describe-instances`

OR&#x20;

Via the console, go to EC2 > instances, select both instances, then Instance state > terminate instance & confirm.
