# Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![alt text](https://github.com/tjjenkins76/Azure-Project/blob/master/Diagrams/Network_Diagram.jpg)



These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

[filebeat-playbook.yml](https://github.com/tjjenkins76/Azure-Project/blob/master/Ansible/filebeat-playbook.yml)





This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

The job of the load balancer is to protect the network or organization against denial of service attacks. The advantage of using a jump box is that it acts as a gateway. It sits in front of the virtual machines and protects them from the public internet.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log files and system metrics. Filebeat watches for and collects data about the file system. Metricbeat collects data of the machine’s performance, such as the uptime.

The configuration details of each machine may be found below.


|       Name          |  Function   | IP Address | Operating System |
|----------------------|----------------|----------------|--------------------------|
| Jump Box         | Gateway     |   10.0.0.1  |     Linux                  |
| DVWA-Web1    | Webserver  |   10.0.0.7  |     Linux                  |
| DVWA-Web2    | Webserver  |   10.0.0.8  |     Linux                  |
| DVWA-Web3    | Webserver  |   10.0.0.9  |     Linux                  |
| Elk-VM              | Monitors web servers    |   10.1.0.4  |     Linux                  |









### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP address: 

- 71.63.124.128

Machines within the network can only be accessed by Port 22.

- 10.0.0.4

A summary of the access policies in place can be found in the table below.

|  Name 	    | Publicly Accessible   | Allowed IP Addresses   |
|----------------------|-----------------------------|---------------------------------|
| Load Balancer  |          Yes		    |     any 	                     |   	
| Jump Box	    |	   No		    |     71.63.124.128 	         |
| DVWA-VM’s      | 	   No		    |     10.0.0.4	    	         | 
| Elk Stack           | 	   No		    |     71.63.124.128	         	         | 


### Elk Configuration

Ansible was used to automate the configuration of the ELK machine. No configuration was performed manually, which is advantageous because…

- Running Ansible from the command line ensures the provisioning scripts are run identically everywhere on every machine.

The playbook implements the following tasks:

- Installed docker
- Installed python3-pip
- Installed docker python module
- Updated the max map count
- Download and launch the docker web container Elk


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![alt text](https://github.com/tjjenkins76/Azure-Project/blob/master/Images/docker_ps_output.png)




### Target Machines & Beats

This ELK server is configured to monitor the following machines:

- 10.0.0.7
- 10.0.0.8
- 10.0.0.9

We have installed the following Beats on these machines:

- Filebeats
- Metricbeats

These Beats allow us to collect the following information from each machine:
- Filebeats collects system files and log events, then forwards the data to Logstash and Elasticserver.
- Metricbeats collect systems and service metrics, such as memory and CPU usage, and forwards the information to Logstash and Elasticserver.


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the configuration file to the Ansible container.
- Update the hosts file to include 10.0.0.7, 10.0.0.8, 10.0.0.9, 10.1.0.4.
- Run the playbook, and navigate to Kibana to check that the installation worked as expected.

1. Copy the configuration file to the Ansible container
2. Add the VM’s to Ansible’s hosts file by typing the command nano /etc/ansible/hosts. When you run playbooks with Ansible, you specify which group to run them on. This allows you to run certain playbooks on some machines, but not on others.

     ```
     /etc/ansible/hosts

     [webservers]
     10.0.0.4 ansible_python_interpreter=/usr/bin/python3
     10.0.0.5 ansible_python_interpreter=/usr/bin/python3
     10.0.0.6 ansible_python_interpreter=/usr/bin/python3

     [elk]
     10.1.0.4 ansible_python_interpreter=/usr/bin/python3
     ```




3. Create the `install-elk.yml playbook` and save it to `etc/ansible/roles directory`.

4. Run the playbook using the command `ansible-playbook install-ek.yml`

5. Navigate to Kibana to check that the installation worked as expected: http://13.66.220.177:5601/app/kibana.

How do I specify which machine to install the ELK server on versus which to install Filebeat on? The IP address for the Elk group will need to be updated in the hosts file. Within the header of the install-elk.yml, the “hosts” field needs to be specified as Elk.

```
- hosts: elk
  become: true
  tasks:
  - name: docker.io
    apt:
      update_cache: yes
      name: docker.io
      state: present
```


### Specific Commands Used to Run and Edit the Playbook

- Connect to the Jumpbox using the command:
ssh sysadmin@52.188.157.214  (Note: the IP address is the public IP for the Jumpbox)
- Connect to the Ansible container by using the following commands:
  - sudo docker container list -a
  - sudo docker start ndly_clarke
  - sudo docker attach ndly_clarke
- Use the following command to run the playbook:
  - ansible-playbook etc/ansible/roles/filebeat-playbook.yml
- To update or edit the playbook, run the command:
  - nano /etc/ansible/roles/filebeat-playbook.yml



