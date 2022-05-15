# First-VM-Conifguration
My First VM configuration with ssh to jumpbox, ansible container and 3 VM
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/Clayton-Tso/First-VM-Conifguration/blob/24a3694139ac1a04f0ea8945459ee259bc06347f/Network%20Map.pdf

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of these ansible playbooks can be used to install only certain pieces of it, such as filebeat.

https://github.com/Clayton-Tso/First-VM-Conifguration/blob/52d78ea14296d4d3081e5f4ae94eb1e388a12440/Ansible/Docker
https://github.com/Clayton-Tso/First-VM-Conifguration/blob/52d78ea14296d4d3081e5f4ae94eb1e388a12440/Ansible/Elk%20Playbook
https://github.com/Clayton-Tso/First-VM-Conifguration/blob/52d78ea14296d4d3081e5f4ae94eb1e388a12440/Ansible/Filebeat-Playbook
https://github.com/Clayton-Tso/First-VM-Conifguration/blob/52d78ea14296d4d3081e5f4ae94eb1e388a12440/Ansible/Metricbeat-%20Playbook

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly efficient, in addition to restricting access to the network from unauthorized connections.

What aspect of security do load balancers protect? 

•	Load balancer was used to ensure incoming traffics through the firewall are evenly distrubtured to 3 VMs to ensure there is isnt an overload of usage and also to maintain server efficiency. 

What is the advantage of a jump box?

•	A jumpbox VM was created to act as the sole link to the 3 VMs via SSH private keys while the 3 VMs contain the SSH public keys. This method creates a added layer of protection for accessing the servers and also establishes a secure connection that can only be used through the jumpbox for additional security measure to restricts accessibility and also limited exposure of the servers to the internet directly.  

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network and system logs

What does Filebeat watch for?

•	Filebeat monitors the log files of the 3 Vms, it also helps to collect, create and forward logs to the Elk server for further diagnostic and/or helps with record keeping in case of an intrusions.

What does Metricbeat record?

•	Metricbeat helps to monitors and collects statistics such as system and services, mySQL and forwards the raw data to the ELK server for readbale evaluations.  

The configuration details of each machine may be found below.

| Name         | Function | IP Address              | Operating System |
|--------------|----------|-------------------------|------------------|
| Jump Box     | Gateway  | 10.1.0.4 / 20.89.76.3   | Linux            |
| TeamRed-ELK  | Server   | 10.0.0.4 / 20.51.104.15 | Linux            |
| Web-1        | Server   | 10.1.0.5 / 138.91.11.44 | Linux            |
| Web-2        | Server   | 10.1.0.6 / 138.91.11.44 | Linux            |
| Web-VM3      | Server   | 10.1.0.7                | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only jumpbox virtual machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 
my host public IP address through SSH port 22

Machines within the network can only be accessed by Jumpbox VM.
•	only the Jumpbox VM is allowed access to the Elk server with the IP address of 10.1.0.4 with SSH port 22

A summary of the access policies in place can be found in the table below.

| Name         | Publicly Accessible | Allowed IP Addresses           |
|--------------|---------------------|--------------------------------|
| Jump Box     | yes                 | 20.89.76.3 through SSH port 22 |  
| TeamRed-ELK  | no                  | 10.0.0.4 through SSH Port 22   |
| Web-1        | no                  | 10.1.0.5 through SSH Port 22   |
| Web-2        | no                  | 10.1.0.6 through SSH Port 22   |
| Web-VM3      | no                  | 10.1.0.7 through SSH Port 22   |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

What is the main advantage of automating configuration with Ansible?
•	With ansible, main advantage are the automation ability to install the necessary initial system setup on multiple machines simultaneously while only needing to configure small amounts of files, no need for setting up each system individually and physically and ability to push updated configuration instantly to all linked machines.   

The playbook implements the following tasks:

•	installed docker into the VM
![image](https://user-images.githubusercontent.com/105409403/168459417-7453e5b5-cd50-4258-b8b8-8f1affb6b514.png)
        
•	installed python 3-pip
![image](https://user-images.githubusercontent.com/105409403/168459432-d45b59b0-67e3-41bd-be3e-6fad722f54f6.png)
      
•	installed docker container module
![image](https://user-images.githubusercontent.com/105409403/168459520-c5f1b910-15cc-445d-b427-8db046594de4.png)
      
•	increase virtual memory
![image](https://user-images.githubusercontent.com/105409403/168459489-f6bc4879-8f05-423b-931f-8bff83283d5c.png)
    
The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

Jumpbox docker 
![image](https://user-images.githubusercontent.com/105409403/168458996-75448d33-64fc-4fbe-819c-0fdd1f9e18c4.png)
Elk docker
![image](https://user-images.githubusercontent.com/105409403/168459054-833b5273-8524-42e3-88b7-b765db8d2fec.png)
Web 1 docker
![image](https://user-images.githubusercontent.com/105409403/168459147-d00ff013-84d3-4480-b7e6-3d356f2a1cfd.png)
Web 2 docker
![image](https://user-images.githubusercontent.com/105409403/168459181-1153b1cb-8bc5-45d8-a89c-775aad341dee.png)
Web VM3 docker
![image](https://user-images.githubusercontent.com/105409403/168459213-8001320e-fe89-4499-8b85-74a20988d850.png)



### Target Machines & Beats
This ELK server is configured to monitor the following machines:
Jumpbox VM - Ip address 10.0.0.4
Web 1 VM - Ip address 10.1.0.5
Web 2 VM - Ip address 10.1.0.6
Web VM3 - Ip address 10.1.0.7

We have installed the following Beats on these machines:

Filebeat
![image](https://user-images.githubusercontent.com/105409403/168460107-670f83fb-0265-430d-a3dc-838285183cca.png)

Metricbeat
![image](https://user-images.githubusercontent.com/105409403/168460156-05423d12-51ff-41f5-9851-045c63624a0b.png)

These Beats allow us to collect the following information from each machine:

With filebeats, syslog was monitored and data was captured

Web -1, Web -2 and Web-VM3
![image](https://user-images.githubusercontent.com/105409403/168460438-1fd4a1a7-77a6-49dc-8965-e2b925fb7557.png)

With Metricbeat, ayatem information and inbound/outbound traffic are captured

Web -1
![image](https://user-images.githubusercontent.com/105409403/168460511-fbc4a0a5-93fd-4a0c-a876-b65bba402252.png)

Web -2
![image](https://user-images.githubusercontent.com/105409403/168460553-6f66a1ba-2408-4d9a-9d31-f35d7ae41474.png)

Web-VM3
![image](https://user-images.githubusercontent.com/105409403/168460570-a59c26e8-ee80-474d-8697-9d4385896c8f.png)



### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the YML file to ANSIBLE
- Update the CONFIG file to include REMOTE USERS AND PORTS
- Run the playbook, and navigate to KIBANA (http://20.51.104.15:5601/app/kibana#) to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
