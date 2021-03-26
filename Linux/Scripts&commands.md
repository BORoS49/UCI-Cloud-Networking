Scripts/commands **I** used in order

- ```ssh -i "OregonKey.pem" ec2-user@ec2-54-68-215-46.us-west-2.compute.amazonaws.com```

- ```scp -i "OregonKey.pem" OregonKey.pem ansible_config.yml ec2-user@ec2-54-68-215-46.us-west-2.compute.amazonaws.com:/home/ec2-user```

- ```sudo chmod 400 OregonKey.pem```
- ```sudo yum install docker```
- ```sudo service docker start```
- ```sudo service docker status```
- ```sudo docker pull cyberxsecurity/ansible```
- ```sudo docker images```
- ```cd /etc/docker```
- ```sudo nano daemon.json ```
```
{
"default-address-pools":
[
{"base":"10.10.0.0/24","size":16}
]
}
```
- ```sudo service docker restart```
- ```sudo docker run -ti cyberxsecurity/ansible bash```
- ```ssh -i "OregonKey.pem" ec2-user@ec2-54-68-215-46.us-west-2.compute.amazonaws.com```
- ```ssh -i "OregonKey.pem" ubuntu@10.10.2.32```
- ```sudo apt-get update```
- ```sudo apt-get upgrade```
- ```exit```
- ```ssh -i "OregonKey.pem" ubuntu@10.10.2.52```
- ```sudo apt-get update```
- ```sudo apt-get upgrade```
- ```exit```
- ```cd /etc/ansible```
- ```nano hosts``````
- ```nano ansible.cfg```
- ```ansible-playbook ansible_config.yml --key-file OregonKey.pem```
- ```sudo docker cp <file> <docker process>:/root```
