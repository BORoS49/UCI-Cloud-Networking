# UCI-Cloud-Networking
Step-by-step guide and diagram on setting up a basic cloud network within Amazon AWS 

Step 1: Use CloudFormation to form a Network Stack: VPC, Subnets, Route Tables, IGW, NAT

< Go to https://aws.amazon.com/cloudformation/ and upload your basic network template file. Click next and then launch the network stack according to your template.

Step 2: Set IPV4 to auto for both public subnets

< Go to your subnets page, highlight your public subnets, go to actions, click on "Modify auto-assign IP Settings" and then check the box titled "Enable auto-assign public IPv4 address"

Step 3: Launch Public Amazon Linux EC2 Instance

< Amazon Linux 2 AMI

< Set to VPC1 created with cloudformation template

< Public1 Subnet

< t2 Micro (Free Tier)

< Configure Security Group (Port 22, SSH)

< Select or create a Key Pair (mine is called "OregonKey.pem")


Step 4: Setup 2 Ubuntu Private Subnet Instances for the ansible container

< Ubuntu Server 20.04 LTS (HVM)

< t2 Micro (Free Tier)

< Private1 Subnet

< Configure Security Group (Port 22 and 80, SSH & HTTP)

< Select Key Pair used for EC2 Instance (OregonKey.pem)

< Launch Instance

Step 5: Verify Instances are running


Step 6: Connect to your Public Instance via CMD on your work machine VIA your public key for the instance

< Need to be in the folder where your key pair is stored

< ssh -i "OregonKey.pem" ec2-user@ec2-54-68-215-46.us-west-2.compute.amazonaws.com



Step 7: Use another CMD console to import your public key and the ansible container to your public instance

< scp -i "Key" "file to be transferred" "location":/home/"user"

< scp -i "OregonKey.pem" OregonKey.pem ansible_config.yml ec2-user@ec2-54-68-215-46.us-west-2.compute.amazonaws.com:/home/ec2-user



Step 8: chmod 400 the key on your EC2 instance

< sudo chmod 400 "key"

< sudo chmod 400 OregonKey.pem

Step 9: Get Docker on your EC2 instance

sudo yum install docker



Step 10: Start docker and verify its running

< sudo service docker start

< sudo service docker status


Step 11: Pull the cybersecurity ansible container image onto your public instance and verify its there.

< sudo docker pull cyberxsecurity/ansible

< sudo docker images



Step 12: Set up the daimon.json file for docker

< cd /etc/docker

< sudo nano daemon.json 

< Paste the following into the .json and save it

{
"default-address-pools":
[
{"base":"10.10.0.0/24","size":16}
]
}

Step 13: Restart docker

< sudo service docker restart


Step 14: Connect to the docker image pulled previously 

< sudo docker run -ti cyberxsecurity/ansible bash

< Should change to root@"container ID" 

Step 15: add the public key and ansible config/playbook to the docker container running from another ec2 terminal in cmd 

< Launch new cmd and ssh into your ec2 instance

< ssh -i "OregonKey.pem" ec2-user@ec2-54-68-215-46.us-west-2.compute.amazonaws.com

< sudo docker ps (This pulls up the active docker containers)

< sudo docker cp OregonKey.pem c98c5b496c89:/root

< sudo docker cp ansible_config.yml c98c5b496c89:/root



Step 16: SSH from the ansible container into both private instances and update/upgrade them

< ssh -i "OregonKey.pem" ubuntu@10.10.2.32

< sudo apt-get update

< sudo apt-get upgrade

exit

< ssh -i "OregonKey.pem" ubuntu@10.10.2.52

< sudo apt-get update

< sudo apt-get upgrade

< exit

Step 17: Get into the /etc/ansible folder and edit the hosts and ansible.cfg files to allow the private machines to hosts the webservers

< cd /etc/ansible

< nano hosts (Uncomment "Webservers" and add IP addresses of both private instances underneath)

< nano ansible.cfg (Change remote user to ubuntu, "remote_user = ubuntu")

Step 18: Run the ansible playbook from the ~ folder in the container

< ansible-playbook ansible_config.yml --key-file OregonKey.pem

Step 19: Set up a windows instance to connect to DVWA

Step 20: Connect to windows and verify DVWA is running via the private DNS of one of the containers the ansible playbook deployed on

Step 21: Create Elk Stack Instance

< Ubunutu

< T3 medium

< ELK Stack Security Group ports: 22, 80, 5044, 5601, 9200, 9300, 9600

Step 22: Run Elk

1. Download the install-elk.yml file
2. Edit contents of install-elk.yml so that "remote_user = ubuntu"
3. Move install-elk.yml to ansible docker via "sudo docker cp <file> <docker process>:/root"
4. In docker process (aka ansible), make sure to add (if not there) the following beneath "[webservers]" and the ip addresses in /etc/ansible/hosts:
[Elk]
<Private IPv4 Address of your ELK server>


5. Ensure that elkserver ubuntu instance has been updated and upgraded (sudo apt-get update/upgrade)
6. Ensure before running install-elk.yml that you have sshed into the ELK server
7. Ensure that inbound rules on your ELK server allow for ports 5044, 5061, and 9200 to be open.
8. Run ansible-playbook install-elk.yml --key-file=<your key>
9. Connect by copying the private ip address of your ELK server and paste it into your Windows machine and connect via port 5601.

Step 23: CELEBRATE GOOD TIMES CMON

Network Map
