# LAB: Monitoring EC2

CloudWatch concepts might seem quite abstract at first, so let's see them in action.

We are going to&#x20;

* launch an EC2 instance in the default VPC
* create a dashboard that shows metrics for this instance&#x20;
* create an alarm that notifies us via email if the CPU utilization increases
* generate load to cause the CPU to spike.

#### Outdated screenshots?&#x20;

CloudWatch just rolled out a new visual layout, so your views may look different from some of the screenshots.&#x20;

#### Windows users

Instructions are written from a Bash perspective.&#x20;

We will need to SSH into the instance that we launch. When you are creating the EC2 instance, use a key pair that you can use with Putty.&#x20;

## Launch an EC2 instance

* Go to the EC2 console and click **Launch instance**
* Name it **yourname-monitoring**
* Application and OS image: use the default Amazon Linux 2 AMI&#x20;
* Instance type: free tier eligible
* Key pair: use **yourname-keys**
* In Network settings:
  1. For network, choose **default vpc**
  2. Subnet: no preference
  3. Auto-assign public IP: **enable**
* Security group:
  * **allow SSH from your IP address.**&#x20;
* Under **Additional details,**&#x20;
  * &#x20;Detailed CloudWatch monitoring: **Enable**.&#x20;

![](<../../.gitbook/assets/image (224).png>)

* Click **Launch instance.**

Note: we have now enabled detailed monitoring for our EC2 instance.&#x20;

* By default CloudWatch records data from EC2 with a 5-minute interval.&#x20;
* You can optionally enable detailed monitoring that records data in 1-minute intervals, as we did.&#x20;

Detailed monitoring costs extra, so it shouldn't be enabled for all instances.&#x20;

We will leave the instance up and running and go to CloudWatch.&#x20;

## CloudWatch overview

### Automatic dashboard

Now let's go see what CloudWatch has to say about our running instance.&#x20;

1. Navigate to CloudWatch&#x20;
2. On the front page you will probably see CloudWatch: Overview
3. Click Overview and select EC2.

If you are instead dropped in to the new console, go to Dashboards > Automatic dashboards > Ec2.

![](<../../.gitbook/assets/image (34).png>)

## Generate some activity

Let's now SSH into our EC2 instance, which has hopefully launched.&#x20;

Once you have SSH'd into your instance, run the following command:&#x20;

```
sudo yum update -y
```

Let's go take a look at some graphs in CloudWatch. You may have to refresh the view using the refresh symbol on the upper right hand corner:

![Refresh symbol and CW graphs](<../../.gitbook/assets/image (96).png>)

As we can see from these graphs, we are getting information about network packets from our EC2 instance.&#x20;

#### Maximize graphs

If you hover over a graph, you can refresh it, maximize it or even download data as .csv (from the three dots).&#x20;

In the new layout, you have to click the three dots, then select Enlarge.&#x20;

![Option to maximize graphs](<../../.gitbook/assets/image (282).png>)

### Metrics

Let's take a closer look at the metrics CloudWatch offers.&#x20;

1. Navigate to CloudWatch > Metrics
2. Scroll down and click on EC2

![EC2 in CW Metrics](<../../.gitbook/assets/image (307).png>)

Then click on Per-Instance Metrics.&#x20;

Find some metric names (for example NetworkIn and NetworkOut), click the triangle next to the name and choose "Add to graph" / "Add to search".

![new layout](<../../.gitbook/assets/image (357).png>)

You can play around with different kinds of graphs (Line, pie, stacked) and time periods.&#x20;

### Metrics for memory?&#x20;

You might notice that the EC2 instance metrics cover things like network, CPU... but no memory consumption?&#x20;

We would need the Unified CloudWatch agent for that. That is beyond the scope of the course. Further reading if you are interested:

[https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/metrics-collected-by-CloudWatch-agent.html](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/metrics-collected-by-CloudWatch-agent.html)

## Dashboard

We've seen automatic dashboards. Now let's create our own dashboard and populate it with graphs that are of interest to us.&#x20;

1. In CloudWatch, on the left side panel click Dashboards
2. Click Create dashboard
3. Give it a name my-ec2
4. Create dashboard

### First widget

1. A dialogue appears asking if you want to add a widget. Pick "Number".&#x20;
2. Next you will be asked to add a metric graph.&#x20;
   1. Pick EC2 metrics
   2. Per-instance metrics
   3. Pick CPUCreditUsage.&#x20;
3. Create widget.

I end up with a number that tells me what the CPUCreditUsage of my new EC2 instance is.

![New CloudWatch Number widget](<../../.gitbook/assets/image (427).png>)

### Second widget

Try creating a widget that shows the CPU utilization of the EC2 instance.&#x20;

1. Click Create widget
2. Pick "Line"&#x20;
3. EC2, per-instance metrics, CPUUtilization
4. Create widget

Once you've created the widgets, click Save dashboard.&#x20;

![Our modest EC2 dashboard](<../../.gitbook/assets/image (295).png>)

## Alarms

What if we wanted to get notified if our EC2 instance's CPU utilization suddenly spiked or say, went above 60%? This is where alarms come in.

### Create alarm

Let's create an alarm that notifies us when CPU utilization goes above a certain threshold.&#x20;

* In CloudWatch, from the left side panel click **Alarms**
* Click **Create alarm**
* Click **select metric**
* In the Browse tab, scroll down to EC2 and click on it&#x20;
* Choose **Per-instance metrics**
* Find the instance **yourname-monitoring**, then the Metric **CPUUtilization**
* Check the checkbox on that row
* Click **Select metric.**&#x20;

![](<../../.gitbook/assets/image (111).png>)

### Specify metric and conditions

On the next page, keep Metric name and InstanceID as they are.&#x20;

Statistic should be **average** and period **1 minute**.&#x20;

![](<../../.gitbook/assets/image (238).png>)

Under Conditions

* Threshold type: static
* Greater/equal
* than.... 60 (this will be a percentage)
* Click next

![](<../../.gitbook/assets/image (450).png>)

### Notification

Under Notifications (we will discuss SNS later. For now just roll with it)

* Alarm state trigger: in alarm&#x20;
* Select an SNS topic: Create new topic
* Topic name: **yourname-ec2-cpu-above-60**
* Email endpoint: enter a valid email you have access to
  * work email is fine
* Click **Create topic**

Skip the other fields and click **Next**.

For the alarm:

* Alarm name: **yourname-ec2-cpu-above-60**
* **Next**

You can see a preview.&#x20;

* **Create alarm.**

#### Confirm subscription

Go to your email inbox and look for email from AWS Notifications. In the email, click **confirm subscription.**&#x20;

We will now receive email alerts if the CPU utilization of our EC2 instance is 60% or greater.&#x20;

## Generate load

We will use a tool called stress to generate some CPU load.

### Install the stress tool&#x20;

In your EC2 instance, run these commands

```
sudo amazon-linux-extras install epel -y
sudo yum install stress -y
stress --version
```

This installs the stress tool on the instance [https://linux.die.net/man/1/stress](https://linux.die.net/man/1/stress)

Let's start to consume CPU with this command

```
sudo stress --cpu 8
```

![](<../../.gitbook/assets/image (287).png>)

### Monitor graphs in CloudWatch&#x20;

This feels redundant since we have an alarm set precisely so we don't have to sit and stare at graphs, but let's do it anyway.&#x20;

1. Go to CloudWatch&#x20;
2. Click the my-ec2 dashboard
3. Expand the CPUUtilization graph&#x20;
4. You can get it to auto-update:

![Set the graph to refresh ](<../../.gitbook/assets/image (391) (1).png>)

It will take a little while for the CPU to start increasing. Eventually we do see an alarm:

![Alarm in CW](<../../.gitbook/assets/image (87).png>)

You should also receive an email notification.&#x20;

### Stop the stress tool

Go to the EC2 instance and type Ctrl+C to stop the stress tool.&#x20;

## Clean up&#x20;

Go ahead and&#x20;

* terminate the EC2 instance&#x20;
* delete the CloudWatch alarm
* delete the dashboard

To delete the SNS topic, navigate to SNS.

Click topics, select the topic we created and delete.&#x20;

There is also a subscription to delete.&#x20;
