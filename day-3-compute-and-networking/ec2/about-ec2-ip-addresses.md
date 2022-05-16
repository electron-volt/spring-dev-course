# About EC2 IP addresses

## A word about IP addresses

There are three different kinds of IP addresses that an EC2 instance can have:

#### Private IP&#x20;

Every instance has a private IP address.&#x20;

#### Dynamic public IP&#x20;

This is what we used to SSH into the instance.&#x20;

#### Static public IP

These are called Elastic IP's or EIP's in AWS. More about these later.&#x20;

For now let's look at private and dynamic public IP's and how they behave if the instance is stopped and then started or rebooted.

### Reboot

Let's try rebooting our instance. Go to the EC2 console in AWS.

#### IP addresses before reboot

First make note of what the private and public IP's of your instance are. Mine were

![IP's initially ](<../../.gitbook/assets/image (454).png>)



![reboot](<../../.gitbook/assets/image (207).png>)

fo

![IP's after reboot ](<../../.gitbook/assets/image (7).png>)





asdfg

### Stop and start

Let's now try stopping our instance and then restarting it. The IP's are still the same as after the reboot.&#x20;

Go to instance state > stop instance. Confirm.&#x20;

Once your instance is in state "Stopped", go back to instance state > start instance.&#x20;

Click the refresh symbol next to Connect if the screen doesn't refresh automatically. What has happened to the IP's?&#x20;

![IP's after the restart](<../../.gitbook/assets/image (231).png>)

Looks like our dynamic public IP has changed. The private one stays the same.&#x20;

If we wanted a public IP address that can remain unchanged even when instances are stopped and restarted, we would need an elastic IP.&#x20;

#### The end



