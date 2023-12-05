# Creating Networking Resources in an Amazon Virtual Private Cloud (VPC) 

Objectives

Summarize the customer scenario
Create a VPC, Internet Gateway, Route Table, Security Group, Network Access List, and EC2 instance to create a routable network within the VPC
Familiarize yourself with the console
Develop a solution to the customers issue found within this lab.
The lab is complete once you can successfully utilize the command ping outside the VPC.


Scenario
Your role is a Cloud Support Engineer at Amazon Web Services (AWS). During your shift, a customer from a startup company requests assistance regarding a networking issue within their AWS infrastructure. The email and an attachment of their architecture is below.

EMAIL FROM THE CUSTOMER
Hello Cloud Support!

I previously reached out to you regarding help setting up my VPC. I thought I knew how to attach all the resources to make an internet connection, but I cannot even ping outside the VPC. All I need to do is ping! Can you please help me set up my VPC to where it has network connectivity and can ping? The architecture is below.

Thanks!

Brock, startup owner

A picture of the customer VPC architecture which consists of a VPC with a CIDR range of 192.168.0.0/18, an Internet Gateway, public subnet with a CIDR range of 192.168.1.0/26, and a security group which is around an EC2 instance

Customer VPC architecture

AWS service restrictions
In this lab environment, access to AWS services and service actions might be restricted to the ones that are needed to complete the lab instructions. You might encounter errors if you attempt to access other services or perform actions beyond the ones that are described in this lab.


For task 1, you will investigate the customer’s request and build a VPC that has network connectivity. You will complete this lab when you can successfully ping from your EC2 instance to the internet showing that the VPC has network connectivity.

In the scenario, Brock, the customer requesting assistance, has requested help in creating resources for his VPC to be route table to the internet. Keep the VPC CIDR at 192.168.0.0/18 and public subnet CIDR of 192.168.1.0/26.

On the left hand navigation pane in the VPC main page, you will see a pattern to build a VPC. Starting with "Your VPCs" working your way down.

Figure: A great guide to building a VPC is to follow the left hand navigation pane, starting from “Your VPCs” and working your way down.

Before you start, let’s review VPC and its components to make it network compatible.

A Virtual Private Gateway (VPC) is like a data center but in the cloud. Its logically isolated from other virtual networks from which you can spin up and launch your AWS resources within minutes.
Private Internet Protocol (IP) addresses are how resources within the VPC communicate with each other. An instance needs a public IP address for it to communicate outside the VPC. The VPC will need networking resources such as an Internet Gateway (IGW) and a route table in order for the instance to reach the internet.
An Internet Gateway (IGW) is what makes it possible for the VPC to have internet connectivity. It has two jobs: perform network address translation (NAT) and be the target to route traffic to the internet for the VPC. An IGW’s route on a route table is always 0.0.0.0/0.
A subnet is a range of IP addresses within your VPC.
A route table contains routes for your subnet and directs traffic using the rules defined within the route table. You associate the route table to a subnet. If an IGW was on a route table, the destination would be 0.0.0.0/0 and the target would be IGW.
Security groups and Network Access Control Lists (NACLs) work as the firewall within your VPC. Security groups work at the instance level and are stateful, which means they block everything by default. NACLs work at the subnet level and are stateless, which means they do not block everything by default.
STEPS
Once in the AWS console, click VPC under Recently visited services. If it is not there, navigate to the top left corner, and select VPC under Networking and Content Delivery in the Services navigation pane.
A picture showing the AWS Management Console and the recently visited services. Under the Recently visited services, there should be a VPC service that you can select. If not you will have to go to the services drop down.

Figure: Recently visited services in the AWS console

A picture showing the services drop down menu and how to find the VPC service by going to the search bar up top, or scrolling down to the bottom under Networking and Content Delivery section and selecting VPC.

Figure: Services navigation drop down

CREATING THE VPC
Recall that a VPC is like a data center, but located in the cloud. It’s logically isolated from other virtual networks. Now you will build a VPC.

Start at the top of the left navigation pane at Your VPCs and work your way down. Select Your VPCs, navigate to the top right corner, and select Create VPC.
Note: You will be using a top-down theory with the top being the VPC.

This picture shows the VPC home page when "your VPCs" is clicked. This is where you will navigate to the top right button and select Create VPC.

Figure: Navigate to “Your VPCs” and select Create VPC.

Name the VPC: 

Test VPC
IPv4 CIDR block: 

192.168.0.0/18

Leave everything else as default, and select Create VPC.
CREATING SUBNETS
Recall that a subnet is a range of IP addresses within your VPC. In your VPC, you can create a public and a private subnet. You can separate subnets according to specific architectural needs. For example, if you have servers that shouldn’t be directly accessed by the internet, you would put them in the private subnet. For test servers or instances that require internet connectivity can be placed in the public subnet. Companies also separate subnets according to offices, teams, or floors.

Now that the VPC is complete, look at the left navigation pane and select Subnets. In the top right corner, select Create subnet.
Note: Although almost anything can be created in any order, it is easier to have an approach. Having a flow or an approach will assist you in troubleshooting issues and ensure that you do not forget a resource.

This picture shows the subnet home page when "Subnets" is clicked. This is where you will navigate to the top right button and select Create subnet.

Figure: Select Create subnet

Configure like the following picture:
This picture shows how it should be configured. VPC ID will be the "Test VPC", under subnet settings, subnet name will be "Public subnet", Availability Zone will be "no preference", IPv4 CIDR block will be "192.168.1.0/28" once complete, hit launch.

Figure: Subnet configuration

Choose Create subnet.
CREATE ROUTE TABLE
Recall that a route table contains the rules or routes that determine where network traffic within your subnet and VPC will go. It controls the network traffic like a router, and, just like a router, it stores IP addresses within the VPC. You associate a route table to each subnet and put the routes that you need your subnet to be able to reach. For this step, you will create the route table first, and then add the routes as you create AWS resources for the VPC.

Navigate to the left navigation pane, and select Route Tables. In the top right corner select Create route table.
This picture shows the route table home page when "Route Tables" is clicked. This is where you will navigate to the top right button and select Create route table.

Figure: Select Create route table.

Configure like the following picture:
The route table should be configured like the following: Name will be "Public route table", VPC will be "Test VPC" and everything else is left as default and select "Create route table".

Figure: Route table configuration

Choose Create route table.
CREATE INTERNET GATEWAY AND ATTACH INTERNET GATEWAY
Recall that an IGW is what allows the VPC to have internet connectivity and allows communication between resources in your VPC and the internet. The IGW is used as a target in the route table to route internet-routable traffic and to perform network address translation (NAT) for EC2 instances. NAT is a bit beyond the scope of this lab, but it is referenced in the reference section if you’d like to dive deeper.

From the left navigation pane, select Internet Gateways. Create an Internet Gateway (IGW) by selecting Create internet gateway at the top right corner.
This picture shows the Internet Gateways home page when "Internet Gateways" is clicked. This is where you will navigate to the top right button and select Create internet gateway.

Figure: Select Create internet gateway

Configure like the following picture:
The internet gateway is configured like the following: the name will be "IGW test VPC" and everything else is default. Select "Create internet gateway".

Figure: Internet gateway configuration

Choose Create internet gateway.

Once created, attach the Internet Gateway to the VPC by selecting Attach to a VPC.

Choose the Test VPC.

Choose Attach internet gateway.

In this picture, it shows that it takes you back to the IGW you created and for you to navigate to the actions button at the top and select "Attach to VPC".

Figure: Attaching the IGW that was just created.

Now your IGW is attached! You now need to add its route to the route table and associate the subnet you created to the route table.

ADD ROUTE TO ROUTE TABLE AND ASSOCIATE SUBNET TO ROUTE TABLE
Navigate to the Route Table section on the left navigation pane. Select Public Route Table, and the scroll to the bottom and select the Routes tab. Select the Edit routes button located in the routes box.
On the Edit routes page, the first IP address is the local route and cannot be changed.

Select Add route.

In the Destination section, type 0.0.0.0/0 in the search box. This is the route to the IGW. You are telling the route table that any traffic that needs internet connection will use 0.0.0.0/0 to reach the IGW so that it can reach the internet.
Click in the Target section and select Internet Gateway since you are targeting any traffic that needs to go to the internet to the IGW. Once you select the IGW, you will see your TEST VPC IGW appear. Select that IGW, navigate to the bottom right, and select Save changes.
In this picture, you are adding the IGW route as 0.0.0.0/0 in the destination section in the route table, and IGW (IGW test VPC) as the target and saving the changes.

Figure: Adding the IGW in the route table (0.0.0.0/0 as the destination and IGW as the target).

Now your traffic has a route to the internet via the IGW.

From the Public route table dashboard, select the Subnet associations tab. Select the Edit subnet associations button.
When selecting he subnet to associate, select the Public subnet and select save.

Figure: Associate the Public subnet and select save association.

Select Save associations.
Note: Every route table needs to be associated to a subnet. You are now associating this route table to this subnet. As you probably noticed, the naming convention is kept the same (public route table, public subnet, etc) in order to associate the same resources together. Keep this in mind when your network and resources grow. You can have multiples of the same resources and it can get confusing to which belongs where.

CREATING A NETWORK ACL
Recall that an NACL is a layer of security that acts like a firewall at the subnet level. The rules to set up a NACL are similar to security groups in the way that they control traffic. The following rules apply: NACLs must be associated to a subnet, NACLs are stateless, and they have the following parts:

Rule number: The lowest number rule gets evaluated first. As soon as a rule matches traffic, its applied; for example: 10 or 100. Rule 10 would get evaluated first.
Type of traffic; for example: HTTP or SSH
Protocol: You can specify all or certain types here
Port range: All or specific ones
Destination: Only applies to outbound rules
Allow or Deny specified traffic.
From the left navigation pane, select Network ACLs. Navigate to the top right corner and select Create network ACL to create a Network Access Control Lists (NACLs).

Name it 

Public Subnet NACL
. Under VPC select the Test VPC.

Choose Create network ACL.

This picture shows the Network ACLs home page when "Network ACLs" is selected. This is where you will navigate to the top right button and select Create network ACL.

Figure: Select Create network ACL

Select the Public Subnet NACL. Choose the Inbound rules tab and select Edit inbound rules.

Choose Add new rule. For Rule number use 

100
. For Type select All traffic.

Leave Source and Allow/Deny as the default and choose Save changes.

Do the same for Outbound rules.

Inbound After creating the NACL, it will should look like the following. This indicates there is only one rule number, which is 100, that states that all traffic, all protocols, all port ranges, from any source (0.0.0.0/0) are allowed to enter (inbound) the subnet. The asterisk * indicates that anything else that does not match this rule is denied.

This picture shows the inbound rules for the NACL, rule number is 100 which is allowing all traffic from all protocols, from any range, from any source that comes inside the subnet. It will deny anything else that does not match this rule.

Figure: Default inbound rule configuration for NACL. This will allow all traffic from anywhere and deny anything else that does not match this rule at the subnet level.

Outbound What do you think this rule says? This picture shows the outbound rules for the NACL, rule number is 100 which is allowing all traffic from all protocols, from any range, from any source to go outside the subnet. It will deny anything else that does not match this rule.

Figure: Default outbound rule configuration for NACL. This will allow all traffic from anywhere and deny anything else that does not match this rule at the subnet level.

CREATING A SECURITY GROUP
Recall that a security group is a virtual firewall at the instance level that controls inbound and outbound traffic. Just like a NACL, security groups control traffic; however, security groups cannot deny traffic. Security groups are stateful; you must allow traffic through the security group as it blocks everything by default, and it must be associated to an instance. A security group has the following parts for both inbound and outbound rules:

Inbound Source: It can be an IP or a specific resource
Outbound Destination: Can by an IP such as anywhere (0.0.0.0/0)
Protocol: Example UDP or TCP
Port range: All or specific range
Description: You can input a description
From the left navigation pane, select Security Groups. Navigate to the top right corner and select Create security group to create a security group.
This picture shows the Security Groups home page when "Security Groups" is selected. This is where you will navigate to the top right button and select Create security group.

Figure: Select Create security group

Configure like the following image of the Basic details page:

Note: In the VPC portion, remove the current VPC, and select Test VPC.

This is the basic details configuration of the security group. The security group name is "public security group", description is "allows public access", and VPC is the "test public VPC".

Figure: Configure the Basic details page

The completed security group is shown below. This indicates that for Inbound rules you are allowing SSH, HTTP, and HTTPS types of traffic, each of which has its own protocols and port range. The source from which this traffic reaches your instance can be originating from anywhere. For Outbound rules, you are allowing all traffic from outside your instance.

This picture shows the inbound and outbound rules configuration of the security group. This picture has the inbound rules allowing traffic from SSH, HTTP and HTTPS from anywhere. And outbound all traffic from anywhere.

Figure: Configuration details for inbound and outbound rules for the security group

Choose Create security group.
You now have a functional VPC. The next task is to launch an EC2 instance to ensure that everything works.

Task 2: Launch EC2 instance and SSH into instance
In task 2, you will launch an EC2 instance within your Public subnet and test connectivity by running the command ping. This will validate that your infrastructure is correct, such as security groups and network ACLs, to ensure that they are not blocking any traffic from your instance to the internet and vice versa. This will validate that you have a route to the IGW via the route table and that the IGW is attached.

Navigate to Services at the top left, and select EC2. From the EC2 dashboard, select Instances.
This picture shows the Instances home page when "Instances" is selected. This is where you will navigate to the top right button and select Create instance.

Figure: Launch an EC2 instance by selecting Launch instance

Select Launch instances from the top right corner. Then use the following settings:

Name the Instance: 

Test EC2

Under Application and OS Images (Amazon Machine Image) leave the default Amazon Linux 2023 AMI selected.

For the Instance Type leave the default t2.micro selected.

For Key pair (login) choose the drop-down and select the AWSLabsKeyPair-*

Under Network settings choose Edit and make sure the Test VPC is selected.

Under Subnet make sure the Public Subnet is selected.

Under Auto-assign public IP choose the drop-down and select Enable.

Under Firewall (security groups) choose Select existing security group.

Select the public security group.

This picture shows what the Network settings will look like for the instance.

Figure: Network settings

Under Configure storage leave the default gp3 selected.

Choose Launch instance.

At the bottom choose View all instances.

Select the Test EC2 instance and under the Details tab copy down the Public IPv4 address.

While waiting for the instance to enter the ready state, at the left of the instructions under the Lab Information pane. Download the PPK file, if on Windows, or the PEM file, if on a Mac.

Use SSH to connect to an Amazon Linux EC2 instance
In this task, you will connect to a Amazon Linux EC2 instance. You will use an SSH utility to perform all of these operations. The following instructions vary slightly depending on whether you are using Windows or Mac/Linux.

 WINDOWS USERS: USING SSH TO CONNECT
 These instructions are specifically for Windows users. If you are using macOS or Linux, skip to the macOS instructions.

From the Lab Information pane, select the PPK link and save the file. Typically your browser will save it to the Downloads directory.

Download PuTTY to SSH into the Amazon EC2 instance. If you do not have PuTTY installed on your computer, download it here.

Open putty.exe

Configure PuTTY timeout to keep the PuTTY session open for a longer period of time.:

Select Connection
Set Seconds between keepalives to 

30
Configure your PuTTY session:
Select Session
Host Name (or IP address): Paste the Public IPv4 address of the instance you made a note of earlier.
Back in PuTTY, in the Connection list, expand  SSH
Select Auth (don’t expand it)
Select Browse
Browse to and select the ppk file that you downloaded
Select Open to select it
Select Accept
When prompted login as, enter: 

ec2-user
 This will connect you to the EC2 instance.

Windows Users: Skip ahead to the next task.

MACOS  AND LINUX  USERS
These instructions are specifically for Mac/Linux users. If you are a Windows user, skip ahead to the next task.

From the Lab Information pane, select the PEM link and save the file.

Open a terminal window, and change directory 

cd
 to the directory where the pem file was downloaded. For example, if the file was saved to your Downloads directory, run this command:


cd ~/Downloads
Change the permissions on the key file to be read-only, by running this command (replace <ssh-key> with the name of the file you downloaded earlier):

chmod 400 <ssh-key>
For example:


chmod 400 Ec2KeyPair-PEM.pem
Note: Your file name may be different from the example.

Run the below command. (replace <ssh-key> with the name of the file you downloaded earlier, and <public-ip> with the PublicIP address you copied earlier):

ssh -i <ssh-key> ec2-user@<public-ip>
Type 

yes
 when prompted to allow the first connection to this remote SSH server. Because you are using a key pair for authentication, you will not be prompted for a password.

Task 3: Use ping to test internet connectivity
Run the following command to test internet connectivity:

ping google.com
After a few seconds, exit ping by holding CTRL+C on Windows or CMD+C on Mac to exit. You should get the following result:

Successful ping: When running the command ping google.com you will see replies noting that you have internet connectivity.

Run ping to test connectivity. The above results are saying you have replies from google.com and have 0% packet loss.

If you are getting replies back, that means that you have connectivity.

End lab
Follow these steps to close the console and end your lab.

Return to the AWS Management Console.

At the upper-right corner of the page, choose AWSLabsUser, and then choose Sign out.

Choose End lab and then confirm that you want to end your lab.

Recap
In this lab, you have investigated the customer’s environment and analyzed the customer’s request to build a fully functional VPC. Through this experience, you were able to apply a blended approach of the OSI model and how AWS cloud fits in that model. You created resources starting with a VPC, and working your way down the left hand navigation pane to assist the customer in having their EC2 instance successfully have network connectivity.
