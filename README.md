## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

Diagrams/RedTeam_Network-Diagram.PNG

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

/etc/ansible/ELK_playbook.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly efficient, in addition to restricting access to the network. It is also a level of redundancy, ensuring that if one server is overwhelmed or not able to receive traffic, it will automatically route to the other server(s). 

The Jump Box is a tool used to connect to devices within a security zone. Because it was set up with SSH Keys instead of passwords, it protects the machines from DDoS attacks.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system metrics and statistics.

Filebeat collects data about the file system. Metricbeat collects machine metrics, such as uptime. 

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web-1    | Server   | 10.0.0.6   | Linux            |
| Web-2    | Server   | 10.0.0.7   | Linux            |
| Web-3    | Server   | 10.0.0.10  | Linux            |
|ELK-SERVER| Server   | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

My home IP address (70.160.134.235)

Machines within the network can only be accessed by Local workstation and Jumpbox.

The Jumpbox VM was allowed access to the ELK VM.
Jumpbox VM: IP 10.0.0.4 Local Workstation via SSH IP 70.160.134.235
A summary of the access policies in place can be found in the table below.

|   Name   | Publicly Accessible | Allowed IP Addresses  |
|----------|---------------------|-----------------------|
| Jump Box |        No      |      70.160.134.235        |
|   Web-1  |        No      |10.0.0.6 & 40.71.92.3       |
|   Web-2  |        No      |10.0.0.7 & 40.71.92.3       |
|   Web-3  |        No      |10.0.0.10 & 40.71.92.3      |
|ELK-Server|        No      |10.1.0.4 & 40.71.92.3       |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because the playbook, much like the ones for Filebear and Metricbeat, saved time and remove [some] elements of human error.  

The playbook implements the following tasks:

- Install docker.io
- name: Install docker.io apt: update_cache: yes name: docker.io state: present
- Install Python-pip
- name: Install pip3 apt: force_apt_get: yes name: python3-pip state: present
- Install: docker
- name: Install Docker python module pip: name: docker state: present
- Command: sysctl -w vm.max_map_count=262144
- Launch docker container: elk
- name: download and launch a docker elk container docker_container: name: elk image: sebp/elk:761 state: started restart_policy: always published_ports: - - 5601:5601 - 9200:9200 - 5044:5044

The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.

Diagrams/dockerps.PNG

### Target Machines & Beats

This ELK server is configured to monitor the following machines:

- Web-1 10.0.0.6

- Web-2 10.0.0.7

We have installed the following Beats on these machines:

- Filebeat and Metricbeat

These Beats allow us to collect the following information from each machine:

- Filebeat can handle audit logs, deprecation logs, gc logs, server logs, and slow logs. 

- Metricbeat collects machine metrics. For example, Metricbeat can be used to monitor and analyze system CPU, memory and load.

### Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

SSH into the control node and follow the steps below:

- Copy the playbook file to /etc/ansible .
- Update the host file to include the webservers and ELK server (and IP addresses).
- Run the playbook, and navigate to command line to check that the installation worked as expected.
- Playbook: ELK_server.yml Location: /etc/ansible/ELK_server.yml

/etc/ansible/hosts:

[webservers]

- 10.0.0.6 ansible_python_interpreter=/usr/bin/python3

- 10.0.0.7 ansible_python_interpreter=/usr/bin/python3

[elk]

- 10.1.0.4 ansible_python_interpreter=/usr/bin/python3

To check if the ELK server is running, the URL is: http://13.83.81.121:5601/app/kibana
http://13.83.81.121:5601/setup.php

## Bonus

The following are the specific commands the user will need. Some commands are written generally since the command would be dependent on specific information (username, IP, container name, etc.)

- <sudo docker ps> will display which containers are present and running.
- <sudo docker container start [container name]> will start the specified container. 
- Example: sudo docker container start relaxed kapitsa
- <sudo docker container attach [container name]> will log the user into the specific container. 
- Example: sudo docker container start relaxed kapitsa
- <ssh username@IP> will log a user into a new machine as long as it is running and allowing access. 
- Example: ssh sysadmin@10.2.0.4
- <ansible-playbook [playbook name].yml> will run the playbook. 
- Example: ansible-playbook ELK-server.yml
- <curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat >> /etc/ansible/filebeat-config.yml> will pull the Filebeat config file from the specified website.
- the <nano> command was used to add/edit configuration files, playbooks, hosts, etc. 
- Example: nano ELK-server.yml

As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc.


