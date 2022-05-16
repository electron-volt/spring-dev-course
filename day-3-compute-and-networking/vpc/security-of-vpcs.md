# Security of VPC's

## Security groups&#x20;

A security group is an instance-level virtual firewall. Some features of security groups:

* SG's have allow rules, but not deny rules.
* An SG has separate rules for inbound and outbound traffic.
* Security group rules enable you to filter traffic based on protocols and port numbers.
* Security groups are **stateful** — if you send a request from your instance, the response traffic for that request is allowed to flow in regardless of inbound security group rules. Responses to allowed inbound traffic are allowed to flow out, regardless of outbound rules.

## NACL

A _network access control list (ACL)_ or NACL is a subnet-level virtual firewall. Some features of NACL's:

* A network ACL has separate inbound and outbound rules, and each rule can either allow or deny traffic.
* Network ACLs are **stateless**, which means that responses to allowed inbound traffic are subject to the rules for outbound traffic (and vice versa).
* A network ACL contains a numbered list of rules. We evaluate the rules in order, starting with the lowest numbered rule, to determine whether traffic is allowed in or out of any subnet associated with the network ACL.

More details can be found in [https://docs.aws.amazon.com/vpc/latest/userguide/security.html](https://docs.aws.amazon.com/vpc/latest/userguide/security.html).

## Example

You have a machine in your corporate network. You have an EC2 instance running in AWS, in a public subnet.&#x20;

Your local machine's IP is 1.2.3.4. You want to SSH into the instance.&#x20;

In the NACL of the public subnet, you need two rules:

* allow traffic from IP 1.2.3.4 to TCP port 22 for SSH **INBOUND**
* allow traffic to IP 1.2.3.4 to ephemeral ports **OUTBOUND**

In the Security group of the EC2 instance you need one rule:

* allow SSH traffic to TCP port 22 from IP 1.2.3.4 **INBOUND**.

You do not need to configure an outbound rule in the security group, because the security group is stateful and will recognize that the traffic to ephemeral ports to IP 1.2.3.4 is a response to an allowed request and will allow it automatically.&#x20;
