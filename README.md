## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Alt text](https://github.com/BORoS49/UCI-Cloud-Networking/blob/main/Diagrams/Network%20Diagram.png?raw=true)

These files have been tested and used to generate a live ELK deployment on AWS. They can be used to either recreate the entire deployment pictured above.

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

DVWA is a PHP/MySQL web application that is damn vulnerable. Its main goals are to be an aid for security professionals to test their skills and tools in a legal environment, help web developers better understand the processes of securing web applications and aid teachers/students to teach/learn web application security in a class room environment. It will accomplish by using a complicated/generated key or .pem file and using the SSH protocols. The Jump Box will also house and run the needed containers and images to allow the webservers and databases to communicate.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
- When Filebeat is deployed it will monitor the network and simplify the data logs to be reviewed at a later time/date
- When metricbeat is added it will record the statistical data from the OS, such as CPU usage and RAM usage, and record services running on the server. Although metricbeats is not deployed in my network this is a highly recommended practice.   

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Operating System 	|       Name      	|    Subnet    	| Access Policy 	| Security Group 	|   Function  	|
|:----------------:	|:---------------:	|:------------:	|:--------------:	|:--------------:	|:-----------:	|
|      Ubuntu t3 medium     	|    ELK Server   	| 10.10.2.x/24 	|     Private    	|  ELK-SG  	|    Web Server   	|
|      Ubuntu t2 micro    	|  DVWA 1 Server  	| 10.10.2.x/24 	|     Private    	|  WebServer-SG  	|    Web Server   	|
|      Ubuntu t2 micro     	|  DVWA 2 Server  	| 10.10.2.x/24 	|     Private    	|  WebServer-SG  	|    Web Server   	|
|      Windows     	| Windows Machine 	| 10.10.0.x/24 	|     Public     	|   Windows-SG   	| Viewing Kibana/DVWA 	|
|   Amazon Linux EC2  	| Ansible/Docker/Jumpbox 	| 10.10.0.x/24 	|     Public     	|   Jumpbox-SG   	|   Gateway   	|

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

- Only the Jump Box and RDP machines can accept connections from the Internet. Access to the private machines can only happen through the jumpbox.
- Machines within the network can only be accessed by SSH protocol for Linux and RDP secure for windows.
- The Jump Box is the only machine accesable by my own home network 


### Elk Configuration

Ansible was used to automate configuration of the DVWA/ELK machines. No manual installation or configuration is needed outside of minor changes to the configuration files for your specific IP's or instances

The playbook implements the following tasks:
- downloading and and configuring DVWA/Elk to our target machines
- deploying the webserver


### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- DWVA 10.10.2.x
- DVWA2 10.10.2.x
- Filebeats 10.10.2.x


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into your jumpbox and follow the steps below:
- Copy the necessary files to your jumpbox
- deploy your ansible container
- configure your container and config files
- deploy your ansible-playbook
- deploy your elk-playbook
- deploy your filebeat-playbook
- Use your windows machine to verify everything

The files that were used and include are: 
- filebeat-playbook.yml
- filebeat-configuration.yoml
- metricbeat-playbook.yml
- metricbeat-configuration.yml
- ansible_config.yml
- install-elk.yml

- **BE SURE TO CONFIGURE YOUR FILES TO YOUR SPECIFIC INSTANCES/IPs**

- **Bonus** **Step by Step**

Step 1: Use CloudFormation to form a Network Stack: VPC, Subnets, Route Tables, IGW, NAT

- Go to https://aws.amazon.com/cloudformation/ and upload your basic network template file. Click next and then launch the network stack according to your template.

The CloudFormation built here demonstrates creating a basic network and automatic deployment on AWS that utilizes an ELK server and load-balanced DVWAs for testing, practicing, and learning purposes. 

![Alt text](https://raw.githubusercontent.com/BORoS49/UCI-Cloud-Networking/main/Ansible/Images/Cloudformation%20stack.png?raw=true "Cloud Formation Stack")

![Alt text](https://raw.githubusercontent.com/BORoS49/UCI-Cloud-Networking/main/Ansible/Images/Create%20Stack.png?raw=true "Create Stack")

![Alt text](https://raw.githubusercontent.com/BORoS49/UCI-Cloud-Networking/main/Ansible/Images/Basuc-Network.png?raw=true "Cloud Formation Stack")


Step 2: Set IPV4 to auto for both public subnets

- Go to your subnets page, highlight your public subnets, go to actions, click on "Modify auto-assign IP Settings" and then check the box titled "Enable auto-assign public IPv4 address"

![Alt text](https://github.com/BORoS49/UCI-Cloud-Networking/blob/main/Ansible/Images/Set%20IPV4.png?raw=true)

![Alt text](https://github.com/BORoS49/UCI-Cloud-Networking/blob/main/Ansible/Images/Set%20IPV4%202.png?raw=true)

Step 3: Launch Public Amazon Linux EC2 Instance

- Amazon Linux 2 AMI

- Set to VPC1 created with cloudformation template

- Public1 Subnet

- t2 Micro (Free Tier)

- Configure Security Group (Port 22, SSH)

- Select or create a Key Pair (mine is called "OregonKey.pem")

![Alt text](https://github.com/BORoS49/UCI-Cloud-Networking/blob/main/Ansible/Images/Launch%20EC2%20Instance.png?raw=true)

![Alt text](https://user-images.githubusercontent.com/75498072/112550142-2750cd00-8d7c-11eb-8aaa-77132cab1d75.png)

![Alt text](https://github.com/BORoS49/UCI-Cloud-Networking/blob/main/Ansible/Images/Configuring%20EC2%20Instance.png?raw=true)

![Alt text](https://github.com/BORoS49/UCI-Cloud-Networking/blob/main/Ansible/Images/EC2%20Security%20Group.png?raw=true)

![Alt text](https://github.com/BORoS49/UCI-Cloud-Networking/blob/main/Ansible/Images/Key%20Pair.png?raw=true)


Step 4: Setup 2 Ubuntu Private Subnet Instances for the ansible container

- Ubuntu Server 20.04 LTS (HVM)

- t2 Micro (Free Tier)

- Private1 Subnet

- Configure Security Group (Port 22 and 80, SSH & HTTP)

- Select Key Pair used for EC2 Instance (OregonKey.pem)

- Launch Instance

![Alt text](https://github.com/BORoS49/UCI-Cloud-Networking/blob/main/Ansible/Images/Setting%20Up%20Private%20Instances.png?raw=true)

![Alt text](https://github.com/BORoS49/UCI-Cloud-Networking/blob/main/Ansible/Images/Private%20Instances%20SG.png?raw=true)

![Alt text](https://github.com/BORoS49/UCI-Cloud-Networking/blob/main/Ansible/Images/Private%20Instance%20State%20before%20launch.png?raw=true)

Step 5: Verify Instances are running


Step 6: Connect to your Public Instance via CMD on your work machine VIA your public key for the instance

- Need to be in the folder where your key pair is stored

- ```ssh -i "OregonKey.pem" ec2-user@ec2-54-68-215-46.us-west-2.compute.amazonaws.com```



Step 7: Use another CMD console to import your public key and the ansible container to your public instance

- ```scp -i "Key" "file to be transferred" "location":/home/"user"```

- ```scp -i "OregonKey.pem" OregonKey.pem ansible_config.yml ec2-user@ec2-54-68-215-46.us-west-2.compute.amazonaws.com:/home/ec2-user```



Step 8: chmod 400 the key on your EC2 instance

- ```sudo chmod 400 "key"```

- ```sudo chmod 400 OregonKey.pem```

Step 9: Get Docker on your EC2 instance

- ```sudo yum install docker```



Step 10: Start docker and verify its running

- ```sudo service docker start```

- ```sudo service docker status```


Step 11: Pull the cybersecurity ansible container image onto your public instance and verify its there.

- ```sudo docker pull cyberxsecurity/ansible```

- ```sudo docker images```



Step 12: Set up the daimon.json file for docker

- ```cd /etc/docker```

- ```sudo nano daemon.json ```

- Paste the following into the .json and save it

```
{
"default-address-pools":
[
{"base":"10.10.0.0/24","size":16}
]
}
```
Step 13: Restart docker

- ```sudo service docker restart```


Step 14: Connect to the docker image pulled previously 

- ```sudo docker run -ti cyberxsecurity/ansible bash```

- ```Should change to root@"container ID" ```

Step 15: add the public key and ansible config/playbook to the docker container running from another ec2 terminal in cmd 

- Launch new cmd and ssh into your ec2 instance

- ```ssh -i "OregonKey.pem" ec2-user@ec2-54-68-215-46.us-west-2.compute.amazonaws.com```

- ```sudo docker ps``` (This pulls up the active docker containers)

- ```sudo docker cp OregonKey.pem c98c5b496c89:/root```

- ```sudo docker cp ansible_config.yml c98c5b496c89:/root```



Step 16: SSH from the ansible container into both private instances and update/upgrade them

- ```ssh -i "OregonKey.pem" ubuntu@10.10.2.32```

- ```sudo apt-get update```

- ```sudo apt-get upgrade```

- ```exit```

- ```ssh -i "OregonKey.pem" ubuntu@10.10.2.52```

- ```sudo apt-get update```

- ```sudo apt-get upgrade```

- ```exit```

Step 17: Get into the /etc/ansible folder and edit the hosts and ansible.cfg files to allow the private machines to hosts the webservers

- ```cd /etc/ansible```

- ```nano hosts`````` (Uncomment "Webservers" and add IP addresses of both private instances underneath)

- ```nano ansible.cfg``` (Change remote user to ubuntu, "remote_user = ubuntu")

Step 18: Run the ansible playbook from the ~ folder in the container

- ```ansible-playbook ansible_config.yml --key-file OregonKey.pem```

Step 19: Set up a windows instance to connect to DVWA

Step 20: Connect to windows and verify DVWA is running via the private DNS of one of the containers the ansible playbook deployed on

Step 21: Create Elk Stack Instance

- Ubunutu

- T3 medium

- ELK Stack Security Group (ports open are: 22, 80, 5044, 5601, 9200, 9300, 9600)

Step 22: Run Elk

- Download the install-elk.yml file
- Edit contents of install-elk.yml so that "remote_user = ubuntu"
- Move install-elk.yml to ansible docker 
- ```sudo docker cp <file> <docker process>:/root```
- In docker process (aka ansible), make sure to add (if not there) the following beneath "[webservers]" and the ip addresses in /etc/ansible/hosts:
[Elk]
- Private IPv4 Address of your ELK server>


- Ensure that elkserver ubuntu instance has been updated and upgraded 

- ```sudo apt-get update``` or upgrade

- Ensure before running install-elk.yml that you have sshed into the ELK server
- Ensure that inbound rules on your ELK server allow for ports 5044, 5061, and 9200 to be open.
- Run the following command within our ansible container ```ansible-playbook install-elk.yml --key-file="your key"```

- Connect by copying the private ip address of your ELK server and paste it into your Windows machine and connect via port 5601.

Step 23: CELEBRATE GOOD TIMES CMON
