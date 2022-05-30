# LAB: EC2 (Console)

Navigate to the EC2 console. You may land on a different page, either the EC2 Dashboard or Instances.&#x20;

In both cases you should see an orange **Launch instances** button.&#x20;

## Create instance

* click on **Create instance**

#### New console

You will most likely see the new console experience, which looks like this:&#x20;

![new console ](<../../.gitbook/assets/image (407) (1).png>)

If instead you see a different console, then please switch over to the next lab called EC2 - Old console.&#x20;

### Application and OS images

If you have these default settings selected already, you do not need to change anything.

![default AMI ](<../../.gitbook/assets/image (230).png>)

### Instance type&#x20;

We want whatever says "Free tier eligible".&#x20;

In most regions the free tier eligible instance type is t2.micro. In some newer regions like eu-north-1, it will be t3.micro instead.&#x20;

The free tier eligible instance type will most likely already be selected.&#x20;

![free!](<../../.gitbook/assets/image (450) (1).png>)

### Key pair

Next up is key pair. You may have to expand this section by clicking the black arrow.&#x20;

Click on the blue link **Create new key pair.** A dialogue will open.&#x20;

![Key pair creation. ](<../../.gitbook/assets/image (301).png>)

* Name: **yourname-keys**
* Key pair type: **RSA**
* Format:&#x20;
  * Mac, Git Bash, Linux, CloudShell: use **.pem**
  * Windows: use **.ppk**
* **Create key pair**

Your browser should automatically download the key file. Store it somewhere safe!&#x20;

### Network settings

#### Default VPC

* Click the white Edit button&#x20;

![network settings](<../../.gitbook/assets/image (171).png>)

We want&#x20;

* VPC: **default VPC**
* Subnet: **no preference**
* Auto-assign public IP: **Enable.**&#x20;

#### Security group&#x20;

For security group,&#x20;

* **Create security group** should be selected
* Name: **yourname-sg**
* Description: Allow SSH from my IP and HTTP from anywhere.

![Creating a security group](<../../.gitbook/assets/image (468).png>)

### Source IP&#x20;

This depends on if you are using CloudShell or AWS CLI.&#x20;

#### If you are using CloudShell&#x20;

Go into the CloudShell terminal and run the following command:

```
curl http://checkip.amazonaws.com
```

It will return your CloudShell terminal's public IP address.&#x20;

* Copy it
* Return to the EC2 console

There is by default an SSH rule in the security group. All you need to do is change the source.&#x20;

![configure IP ](<../../.gitbook/assets/image (167).png>)

* For **source type**, choose **Custom**&#x20;
* In **Source**, paste in the public IP&#x20;
* Add /32 to the end
* Desription is optional.&#x20;

#### If you are using AWS CLI locally on your machine&#x20;

Stay in the EC2 console.&#x20;

There is by default an SSH rule in the security group. All you need to do is change the source.&#x20;

* Under **Source type,** select **My IP**
* AWS automatically completes the source.
* Description is optional.&#x20;

#### Add HTTP rule

This step is the same for AWS CLI and CloudShell.&#x20;

* Click Add security group rule&#x20;
* Type: **HTTP**&#x20;
* Source type: Anywhere

![](<../../.gitbook/assets/image (250).png>)

Ignore the warning.&#x20;

Skip **configure storage.**&#x20;

### User data

Expand **Additional details.**&#x20;

Scroll all the way to the bottom.&#x20;

In the text area called User data, paste in the script below:

```
#!/bin/bash
yum update -y
yum install httpd -y
systemctl start httpd
systemctl enable httpd
cd /var/www/html
echo "This is a test page running on my first EC2 instance." > index.html
```

![user data](<../../.gitbook/assets/image (143) (1).png>)

This script will run as our instance is created. It turns our instance into a web server that prints out the text on line 7.&#x20;

### Launch instance

On the right hand side, click the orange **Launch instance** button.&#x20;

You will land on a page that has your instance's instance id (looks like i-123abc) as a blue link. Click on it.&#x20;

![instance is launching](<../../.gitbook/assets/image (290).png>)

This will take you to the Instances page.&#x20;

Wait for the Instance state to turn to a green Running.

Then click on the instance ID.

The summary page may still show your instance as Pending.&#x20;

![instance details](<../../.gitbook/assets/image (193).png>)

Notice the Public IPv4 address? Copy it.&#x20;

## SSH into your instance

### In CloudShell

Go into CloudShell. In the upper right corner click Actions > Upload file.&#x20;

Upload the key pair that you downloaded previously, called yourname-keys.pem.&#x20;

Run the following command:

```
chmod 400 yourname-keys.pem
```

Then SSH into your instance with&#x20;

```
ssh -i yourname-keys.pem ec2-user@YOUR PUBLIC IP ADDRESS
```

Are you sure you want to continue connecting? Type yes.&#x20;

![](<../../.gitbook/assets/image (289).png>)

You can run&#x20;

```
sudo yum update -y
```

to update packages on the instace.&#x20;

![](<../../.gitbook/assets/image (121) (1).png>)

### The end&#x20;

If you access the public IP of your instance in the browser, you can see your awesome web server:

![](<../../.gitbook/assets/image (262).png>)

Let's leave the instance running and move on the next lab.&#x20;
