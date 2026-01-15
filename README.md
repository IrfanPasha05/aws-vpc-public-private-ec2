🧱 STEP-BY-STEP IMPLEMENTATION

✅ Step 1: Create Custom VPC



Name: Demo-custom-vpc



CIDR Block: 10.0.0.0/16



✅ Step 2: Create Subnets

Public Subnet



Name: Public-subnet



CIDR: 10.0.1.0/24



AZ: us-east-1a



Private Subnet



Name: Private-subnet



CIDR: 10.0.2.0/24



AZ: us-east-1b



✅ Step 3: Create and Attach Internet Gateway



Name: Demo-igw



Attach to Demo-custom-vpc



✅ Step 4: Create NAT Gateway



Subnet: Public-subnet



Allocate Elastic IP



Name: Demo-nat



✅ Step 5: Configure Route Tables

Public Route Table (Public-RT)



Routes:



10.0.0.0/16 → local

0.0.0.0/0   → Internet Gateway



Associate with:



Public-subnet



Private Route Table (Private-RT)



Routes:



10.0.0.0/16 → local

0.0.0.0/0   → NAT Gateway



Associate with:



Private-subnet



✅ Step 6: Configure Security Groups

Web-SG (Public EC2)



SSH (22) → My IP



HTTP (80) → 0.0.0.0/0



Private-SG (Private EC2)



SSH (22) → Source: Web-SG



🚀 Step 7: Launch EC2 Instances

Public EC2 (Web Server)



AMI: Ubuntu 22.04 LTS



Subnet: Public-subnet



Auto-assign Public IP: Enabled



Security Group: Web-SG



Private EC2 (Backend Server)



AMI: Ubuntu 22.04 LTS



Subnet: Private-subnet



Auto-assign Public IP: Disabled



Security Group: Private-SG



🌐 Step 8: Host Website on Public EC2

sudo apt update -y

sudo apt install apache2 -y

sudo systemctl start apache2

sudo systemctl enable apache2



Create a simple webpage:



cd /var/www/html

sudo nano index.html

<h1>AWS VPC Project</h1>

<p>Website hosted on Ubuntu EC2 in a Public Subnet</p>



Access in browser:



http://<Public-IP>

🔐 Step 9: Bastion Host Access (Best Practice)



Copy key from local → Public EC2 using scp



SSH into Public EC2



From Public EC2, SSH into Private EC2 using private IP



ssh -i demo-key.pem ubuntu@10.0.2.128

🌍 Step 10: Verify NAT Gateway



From Private EC2:



ping google.com



✅ Confirms outbound internet access via NAT Gateway

