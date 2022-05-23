# LAB: LAMP stack

## LAMP stack&#x20;

In this lab we build a LAMP stack (Linux, Apache, MySQL, PHP) using a CloudFormation template that uses bootstrap scripts to install the packages and files necessary to deploy&#x20;

* the Apache web server
* PHP and&#x20;
* MySQL&#x20;

at instance launch time.&#x20;

## Region&#x20;

For the template to work as-is, please use region **us-east-1.**&#x20;

## Keys

You will need a key pair in the region that you are going to create the stack in.&#x20;

You can create a key pair in the EC2 console.&#x20;

## Default VPC

The template assumes that the region us-east-1 has a default VPC.

If you do not have a default VPC, you can create one via the console or the CLI.&#x20;

## Template

Save the following on your machine as **lamp-template.json.**

{% code title="lamp-template.json" %}
```
{
    "AWSTemplateFormatVersion" : "2010-09-09",
    
    "Description" : "AWS CloudFormation Sample Template LAMP_Single_Instance: Create a LAMP stack using a single EC2 instance and a local MySQL database for storage. This template demonstrates using the AWS CloudFormation bootstrap scripts to install the packages and files necessary to deploy the Apache web server, PHP and MySQL at instance launch time. **WARNING** This template creates an Amazon EC2 instance. You will be billed for the AWS resources used if you create a stack from this template.",
    
    "Parameters" : {
        
      "KeyName": {
        "Description" : "Name of an existing EC2 KeyPair to enable SSH access to the instance",
        "Type": "AWS::EC2::KeyPair::KeyName",
        "Default": "",
        "ConstraintDescription" : "must be the name of an existing EC2 KeyPair."
      },    
  
      "DBName": {
        "Default": "MyDatabase",
        "Description" : "MySQL database name",
        "Type": "String",
        "MinLength": "1",
        "MaxLength": "64",
        "AllowedPattern" : "[a-zA-Z][a-zA-Z0-9]*",
        "ConstraintDescription" : "must begin with a letter and contain only alphanumeric characters."
      },
  
      "DBUser": {
        "NoEcho": "true",
        "Description" : "Username for MySQL database access",
        "Type": "String",
        "MinLength": "1",
        "MaxLength": "16",
        "AllowedPattern" : "[a-zA-Z][a-zA-Z0-9]*",
        "ConstraintDescription" : "must begin with a letter and contain only alphanumeric characters."
      },
  
      "DBPassword": {
        "NoEcho": "true",
        "Description" : "Password for MySQL database access",
        "Type": "String",
        "MinLength": "1",
        "MaxLength": "41",
        "AllowedPattern" : "[a-zA-Z0-9]*",
        "ConstraintDescription" : "must contain only alphanumeric characters."
      },
  
      "DBRootPassword": {
        "NoEcho": "true",
        "Description" : "Root password for MySQL",
        "Type": "String",
        "MinLength": "1",
        "MaxLength": "41",
        "AllowedPattern" : "[a-zA-Z0-9]*",
        "ConstraintDescription" : "must contain only alphanumeric characters."
      },
  
      "InstanceType" : {
        "Description" : "WebServer EC2 instance type",
        "Type" : "String",
        "Default" : "t2.micro",
        "AllowedValues" : ["t2.micro"]
  ,
        "ConstraintDescription" : "must be a valid EC2 instance type."
      },
  
      "SSHLocation" : {
        "Description" : " The IP address range that can be used to SSH to the EC2 instances",
        "Type": "String",
        "MinLength": "9",
        "MaxLength": "18",
        "Default": "0.0.0.0/0",
        "AllowedPattern": "(\\d{1,3})\\.(\\d{1,3})\\.(\\d{1,3})\\.(\\d{1,3})/(\\d{1,2})",
        "ConstraintDescription": "must be a valid IP CIDR range of the form x.x.x.x/x."
      } 
    },
    
    "Mappings" : {
      "AWSInstanceType2Arch" : {
        "t2.micro"    : { "Arch" : "HVM64"  }
      },
  
      "AWSInstanceType2NATArch" : {
        "t2.micro"    : { "Arch" : "NATHVM64"  }
      }
  ,
       "AWSRegionArch2AMI" : { 
        "us-east-1"      : { "HVM64" : "ami-0080e4c5bc078760e" } 
      }
  
    },
      
    "Resources" : {     
        
      "WebServerInstance": {  
        "Type": "AWS::EC2::Instance",
        "Metadata" : {
          "AWS::CloudFormation::Init" : {
            "configSets" : {
              "InstallAndRun" : [ "Install", "Configure" ]
            },
  
            "Install" : {
              "packages" : {
                "yum" : {
                  "mysql"        : [],
                  "mysql-server" : [],
                  "mysql-libs"   : [],
                  "httpd"        : [],
                  "php"          : [],
                  "php-mysql"    : []
                }
              },
  
              "files" : {
                "/var/www/html/index.php" : {
                  "content" : { "Fn::Join" : [ "", [
                    "<html>\n",
                    "  <head>\n",
                    "    <title>AWS CloudFormation PHP Sample</title>\n",
                    "    <meta http-equiv=\"Content-Type\" content=\"text/html; charset=ISO-8859-1\">\n",
                    "  </head>\n",
                    "  <body>\n",
                    "    <h1>Welcome to the AWS CloudFormation PHP Sample</h1>\n",
                    "    <p/>\n",
                    "    <?php\n",
                    "      // Print out the current data and time\n",
                    "      print \"The Current Date and Time is: <br/>\";\n",
                    "      print date(\"g:i A l, F j Y.\");\n",
                    "    ?>\n",
                    "    <p/>\n",
                    "    <?php\n",
                    "      // Setup a handle for CURL\n",
                    "      $curl_handle=curl_init();\n",
                    "      curl_setopt($curl_handle,CURLOPT_CONNECTTIMEOUT,2);\n",
                    "      curl_setopt($curl_handle,CURLOPT_RETURNTRANSFER,1);\n",
                    "      // Get the hostname of the intance from the instance metadata\n",
                    "      curl_setopt($curl_handle,CURLOPT_URL,'http://169.254.169.254/latest/meta-data/public-hostname');\n",
                    "      $hostname = curl_exec($curl_handle);\n",
                    "      if (empty($hostname))\n",
                    "      {\n",
                    "        print \"Sorry, for some reason, we got no hostname back <br />\";\n",
                    "      }\n",
                    "      else\n",
                    "      {\n",
                    "        print \"Server = \" . $hostname . \"<br />\";\n",
                    "      }\n",
                    "      // Get the instance-id of the intance from the instance metadata\n",
                    "      curl_setopt($curl_handle,CURLOPT_URL,'http://169.254.169.254/latest/meta-data/instance-id');\n",
                    "      $instanceid = curl_exec($curl_handle);\n",
                    "      if (empty($instanceid))\n",
                    "      {\n",
                    "        print \"Sorry, for some reason, we got no instance id back <br />\";\n",
                    "      }\n",
                    "      else\n",
                    "      {\n",
                    "        print \"EC2 instance-id = \" . $instanceid . \"<br />\";\n",
                    "      }\n",
                    "      $Database   = \"localhost\";\n",
                    "      $DBUser     = \"", {"Ref" : "DBUser"}, "\";\n",
                    "      $DBPassword = \"", {"Ref" : "DBPassword"}, "\";\n",
                    "      print \"Database = \" . $Database . \"<br />\";\n",
                    "      $dbconnection = mysql_connect($Database, $DBUser, $DBPassword)\n",
                    "                      or die(\"Could not connect: \" . mysql_error());\n",
                    "      print (\"Connected to $Database successfully\");\n",
                    "      mysql_close($dbconnection);\n",
                    "    ?>\n",
                    "    <h2>PHP Information</h2>\n",
                    "    <p/>\n",
                    "    <?php\n",
                    "      phpinfo();\n",
                    "    ?>\n",
                    "  </body>\n",
                    "</html>\n"
                  ]]},
                  "mode"  : "000600",
                  "owner" : "apache",
                  "group" : "apache"
                },
  
                "/tmp/setup.mysql" : {
                  "content" : { "Fn::Join" : ["", [
                    "CREATE DATABASE ", { "Ref" : "DBName" }, ";\n",
                    "GRANT ALL ON ", { "Ref" : "DBName" }, ".* TO '", { "Ref" : "DBUser" }, "'@localhost IDENTIFIED BY '", { "Ref" : "DBPassword" }, "';\n"
                    ]]},
                  "mode"  : "000400",
                  "owner" : "root",
                  "group" : "root"
                },
                "/etc/cfn/cfn-hup.conf" : {
                  "content" : { "Fn::Join" : ["", [
                    "[main]\n",
                    "stack=", { "Ref" : "AWS::StackId" }, "\n",
                    "region=", { "Ref" : "AWS::Region" }, "\n"
                  ]]},
                  "mode"    : "000400",
                  "owner"   : "root",
                  "group"   : "root"
                },
  
                "/etc/cfn/hooks.d/cfn-auto-reloader.conf" : {
                  "content": { "Fn::Join" : ["", [
                    "[cfn-auto-reloader-hook]\n",
                    "triggers=post.update\n",
                    "path=Resources.WebServerInstance.Metadata.AWS::CloudFormation::Init\n",
                    "action=/opt/aws/bin/cfn-init -v ",
                    "         --stack ", { "Ref" : "AWS::StackName" },
                    "         --resource WebServerInstance ",
                    "         --configsets InstallAndRun ",
                    "         --region ", { "Ref" : "AWS::Region" }, "\n",
                    "runas=root\n"
                  ]]},
                  "mode"    : "000400",
                  "owner"   : "root",
                  "group"   : "root"
                }
              },
  
              "services" : {
                "sysvinit" : {  
                  "mysqld"  : { "enabled" : "true", "ensureRunning" : "true" },
                  "httpd"   : { "enabled" : "true", "ensureRunning" : "true" },
                  "cfn-hup" : { "enabled" : "true", "ensureRunning" : "true",
                                "files" : ["/etc/cfn/cfn-hup.conf", "/etc/cfn/hooks.d/cfn-auto-reloader.conf"]}
                }
              }
            },
  
            "Configure" : {
              "commands" : {
                "01_set_mysql_root_password" : {
                  "command" : { "Fn::Join" : ["", ["mysqladmin -u root password '", { "Ref" : "DBRootPassword" }, "'"]]},
                  "test" : { "Fn::Join" : ["", ["$(mysql ", { "Ref" : "DBName" }, " -u root --password='", { "Ref" : "DBRootPassword" }, "' >/dev/null 2>&1 </dev/null); (( $? != 0 ))"]]}
                },
                "02_create_database" : {
                  "command" : { "Fn::Join" : ["", ["mysql -u root --password='", { "Ref" : "DBRootPassword" }, "' < /tmp/setup.mysql"]]},
                  "test" : { "Fn::Join" : ["", ["$(mysql ", { "Ref" : "DBName" }, " -u root --password='", { "Ref" : "DBRootPassword" }, "' >/dev/null 2>&1 </dev/null); (( $? != 0 ))"]]}
                }
              }
            }
          }
        },
        "Properties": {
          "ImageId" : { "Fn::FindInMap" : [ "AWSRegionArch2AMI", { "Ref" : "AWS::Region" },
                            { "Fn::FindInMap" : [ "AWSInstanceType2Arch", { "Ref" : "InstanceType" }, "Arch" ] } ] },
          "InstanceType"   : { "Ref" : "InstanceType" },
          "SecurityGroups" : [ {"Ref" : "WebServerSecurityGroup"} ],
          "KeyName"        : { "Ref" : "KeyName" },
          "UserData"       : { "Fn::Base64" : { "Fn::Join" : ["", [
               "#!/bin/bash -xe\n",
               "yum update -y aws-cfn-bootstrap\n",
  
               "# Install the files and packages from the metadata\n",
               "/opt/aws/bin/cfn-init -v ",
               "         --stack ", { "Ref" : "AWS::StackName" },
               "         --resource WebServerInstance ",
               "         --configsets InstallAndRun ",
               "         --region ", { "Ref" : "AWS::Region" }, "\n",
  
               "# Signal the status from cfn-init\n",
               "/opt/aws/bin/cfn-signal -e $? ",
               "         --stack ", { "Ref" : "AWS::StackName" },
               "         --resource WebServerInstance ",
               "         --region ", { "Ref" : "AWS::Region" }, "\n"
          ]]}}        
        },
        "CreationPolicy" : {
          "ResourceSignal" : {
            "Timeout" : "PT10M"
          }
        }
      },
      
      "WebServerSecurityGroup" : {
        "Type" : "AWS::EC2::SecurityGroup",
        "Properties" : {
          "GroupDescription" : "Enable HTTP access via port 80",
          "SecurityGroupIngress" : [
            {"IpProtocol" : "tcp", "FromPort" : "80", "ToPort" : "80", "CidrIp" : "0.0.0.0/0"},
            {"IpProtocol" : "tcp", "FromPort" : "22", "ToPort" : "22", "CidrIp" : { "Ref" : "SSHLocation"}}
          ]
        }      
      }          
    },
    
    "Outputs" : {
      "WebsiteURL" : {
        "Description" : "URL for newly created LAMP stack",
        "Value" : { "Fn::Join" : ["", ["http://", { "Fn::GetAtt" : [ "WebServerInstance", "PublicDnsName" ]}]] }
      }
    }
  }
```
{% endcode %}

## Create stack&#x20;

Create the stack in CloudFormation using the lamp-template.json template.

You will be asked to&#x20;

* name the stack: yourname-lamp-stack
* create a DB user and password: make your own&#x20;
* provide a key pair
* provide an IP range that the security group of your instance will allow SSH from.&#x20;

You can find your IP address or just use 0.0.0.0/0 for the last step.&#x20;

### Patience&#x20;

The web server takes a while to get ready. The EC2 console will show the instance as running loooong before it is actually done.&#x20;

Wait for 1-5 minutes. When the stack is ready, you should see this in the **Events** tab in CloudFormation:

![Events](<../../.gitbook/assets/image (67).png>)

## Explore your LAMP stack&#x20;

Once the web server is ready - that is, when the CloudFormation console shows that stack creation is complete - you can view your PHP front page.

In the **CloudFormation** console, go to the **Outputs** tab.&#x20;

![Outputs ](<../../.gitbook/assets/image (397).png>)

You can open the URL in a new tab:

![LAMP stack](<../../.gitbook/assets/image (184).png>)

### SSH into the instance - can be skipped

This part is optional.

You can SSH into your instance and see that there really is a MySQL database running:

![](<../../.gitbook/assets/image (338).png>)

### For fun

Estimate how long it would have taken for you to bring up a LAMP stack

* in a traditional data center, beginning with you ordering a physical server and waiting for it to be delivered, hooked up to the network etc.&#x20;
* in AWS without CloudFormation.&#x20;

## Delete the stack&#x20;

You can conveniently tear down the entire thing by just deleting the stack in CloudFormation.&#x20;
