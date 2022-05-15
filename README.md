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
  - name: Install docker.io
      apt:
        update_cache: yes
        force_apt_get: yes
        name: docker
        state: present
•	installed python 3-pip
  - name: Install python3-pip
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present
      # Use pip module (It will default to pip3)
•	installed docker container module
      - name: Install Docker module
      pip:
        name: docker
        state: present
      # Use command module
    - name: Increase virtual memory
      command: sysctl -w vm.max_map_count=262144
•	set systemctl modules
   # Use sysctl module
    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes
•	designate ports for the elk container
  - name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        # Please list the ports that ELK runs on
        published_ports:
          -  5601:5601
          -  9200:9200
          -  5044:5044


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
- _TODO: Specify which Beats you successfully installed_

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _____ file to _____.
- Update the _____ file to include...
- Run the playbook, and navigate to ____ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
