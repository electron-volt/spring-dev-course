# LAB: EC2 - Old console

These instructions are for the old EC2 console experience. Skip this if you used the new console.&#x20;

## Creating our first EC2 instance

We will create our first ever EC2 instance! We will be using exclusively Linux instances, sorry Bill G.&#x20;

1. Navigate to EC2 > instances
2. Click Launch instances

You will be taken through all the steps required to launch your first instance.&#x20;

### Choose an AMI&#x20;

We will be using Amazon Linux. Look for this symbol&#x20;

![Free tier eligible Amazon Linux](<../../.gitbook/assets/image (151).png>)

If there are several kernel versions to choose from, pick the most recent (Kernel 5.10 as I write this)&#x20;

Make sure 64-bit (x86) is chosen and hit Select.&#x20;

### Choose an instance type

We want to keep our costs down, so look for an <mark style="color:orange;">instance type that is free tier eligible</mark>. This might change based on your region, but look for this:

![Free tier eligible instance type](<../../.gitbook/assets/image (432).png>)

The t3.micro has 2 virtual CPUs (vCPU) and 1 GiB of memory. Quite a modest instance type, suitable for testing and development. Feel free to scroll around to see how much vCPU and memory the other instance types offer.&#x20;

Once you've had a look, click the grey Configure Instance details button.&#x20;

### Configure instance details

#### Select default VPC

We want to launch our instance in our default VPC. Make sure it is selected in **Network**:

![Network settings ](<../../.gitbook/assets/image (137).png>)

Leave all other settings as they are.

Scroll all the way down to **Advanced details:**

![User data in additional settings](<../../.gitbook/assets/image (276).png>)

Leave all other settings as they are, but in the text area **User data** paste the following:

```
#!/bin/bash
yum update -y
yum install httpd -y
systemctl start httpd
systemctl enable httpd
cd /var/www/html
echo "This is a test page running on my first EC2 instance." > index.html
```

This script turns our instance into a web server that prints a simple message (on line 7).

Then hit **Next: Add storage.**

### Add storage

This is where we can add additional storage to our instances. Notice that the instance we are about to launch has a root volume - every instance has a root device volume or boot volume. Ours has 8 GiB.

![root volume for an EC2 instance](<../../.gitbook/assets/image (165).png>)

It has been set to be deleted on termination. This is the default behaviour.&#x20;

* Leave all of these settings as they are.&#x20;
* Hit **Next: Add tags.**&#x20;

### Tags - optional&#x20;

Tags are key-value-pairs that can help you identify, group and track your resources.&#x20;

We could for example tag all the resources we create today with the following tag

![Tag for day: 2](<../../.gitbook/assets/Screenshot 2022-01-02 at 14.28.18.png>)

and then at the end of the day we would have an easier time deleting all resources for day 2, because we could find and identify them based on the tag. Tags have no other effect on the function of AWS services, they are only for your convenience.&#x20;

* Add tags if you want
* hit **Next: Configure Security Group.** &#x20;

### Security groups

Security groups are instance level firewalls (as opposed to network access control lists which we will look at later, which are subnet level firewalls). They are stateful.

Security groups by default block all incoming traffic. Any connection that you want to open up you must explicitly allow in the security group rules.&#x20;

We are going to create a security group that we can reuse in later labs. It will allow SSH (TCP port 22) and HTTP (TCP port 80) from your machine's IP address.&#x20;

1. Make sure "create a **new** security group" is selected&#x20;
2. Give it the name **ssh-and-http**
3. Description is optional&#x20;

You may find a rule allowing SSH is already present.&#x20;

#### If you have AWS CLI installed locally on your machine

Just go to **source** and from the dropdown choose **My IP**. AWS will automatically put your IPv4 address in. Description is optional.

![Using your IP address as source](<../../.gitbook/assets/image (219).png>)

#### If you are using CloudShell instead of AWS CLI locally

Go into the CloudShell terminal and run the following command:

```
curl http://checkip.amazonaws.com
```

It will return a public IP address.&#x20;

* Copy it
* go into the security group settings and the SSH rule
* for source, choose custom
* paste in the IP from CloudShell
* add /32 to the end of the IP&#x20;

So if the IP address was 1.2.3.4, put 1.2.3.4/32 as the source. &#x20;

#### HTTP rule

Go ahead and add another rule to allow HTTP. What happens if I select anywhere for source?&#x20;

![Rule allowing HTTP from anywhere.](<../../.gitbook/assets/image (149).png>)

Notice how the source is 0.0.0.0/0, ::/0? This means all IPv4 (and IPv6) addresses would be able to access your web server. That is not what we want for this lab, so let's also change the source to your IP (following the same steps as above for either local CLI or CloudShell).&#x20;

Once you've made those changes, click **Review and Launch**.&#x20;

#### Launch&#x20;

You are able to review and edit details before you launch. We are happy with what we have, so let's go ahead and Launch our instance!&#x20;

### Key pair

If we want to be able to SSH into our instance, we need to create a key pair. When you press launch, a dialogue appears.&#x20;

1. Select "create a new key pair"
2. Key pair type is RSA
3. Give it the name "yourname-keys"
4. Download key pair.&#x20;
5. Launch instance.&#x20;

## Connecting to the instance

Your EC2 instance is ready to be SSH'd into when you see this:

![EC2 instance is running](<../../.gitbook/assets/image (341).png>)

#### Name the instance

Click on the - in the Name column.

Name your instance **yourname-EC2.**&#x20;

#### Details

Click on the blue instance id to view more information about the instance.&#x20;

Once you find the **public IPv4 address** of your instance, open it in your browser. You should see:

![It works!](<../../.gitbook/assets/image (113) (1).png>)

## Optional - SSH into the instance

Now let's SSH into our instance and maybe change the text to something else.&#x20;

### Windows users using PuTTY

If you want to use PuTTY, then instructions are here: [https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html)

### MacOS or Linux

1. Open a terminal window
2. Change to the directory where your key pair (day2labs.pem) file is&#x20;
3. Run `chmod 400 day2labs.pem`
4. Then connect to your instance with `ssh -i day2labs.pem ec2-user@<public-ipv4>`
5. Type yes and hit enter when asked to continue connecting.

Once you are in, run this command:

`sudo yum update -y`

This updates packages on the system**.**

![if you know you know](<../../.gitbook/assets/image (238) (1).png>)

### Customize your own instance!

By changing the text in /var/www/html/index.html, you can change what your web server is serving up to the world.&#x20;

![New text](<../../.gitbook/assets/image (248) (1).png>)

Maybe do a little \<marquee> to make it feel like it's 1998 again.&#x20;

## The end

We will not be needing this instance for now, so we can stop it (instance state > stop instance > confirm) and go on to create another instance via the CLI.&#x20;
