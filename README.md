## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![alt text]( https://github.com/laurapratt87/ELK-Stack-Project/blob/main/Diagrams/RedTeam%20Network%20Diagram.png "Diagram")

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

I created an ELK stack that allows the automatation of monitoring the performance of multiple virtual machines in one database.  By using the ELK Server in conjunction with containers, the benefits are:

  - Scalability and Elasticity
  - Efficiency of Resources
  - Increased Security
  - App Isolation

More information about [Cloud Domain, Containers, and the Security Benefits](https://github.com/laurapratt87/ELK-Stack-Project/blob/main/Additional%20Resources/Container_Interview_Question.docx).

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

Machines within the network can only be accessed by Local workstation via the Jumpbox.

The Jumpbox VM was allowed access to the ELK VM.
Jumpbox VM: IP 10.0.0.4 Local Workstation via SSH IP 70.160.134.235
A summary of the access policies in place can be found in the table below.

|   Name   | Publicly Accessible | Allowed IP Addresses  |
|----------|---------------------|-----------------------|
| Jump Box |        Yes          |    70.160.134.235     |
|   Web-1  |        No           |      10.0.0.4         |
|   Web-2  |        No           |      10.0.0.4         |
|   Web-3  |        No           |      10.0.0.4         |
|ELK-Server|        No           |      10.0.0.4         |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because the playbook, much like the ones for Filebeat and Metricbeat, saved time and remove [some] elements of human error.  

[Link to Playbooks and Config Files](https://github.com/laurapratt87/ELK-Stack-Project/tree/main/Ansible).

|ELK_Playbook tasks    |
|----------|
| Install: docker.io |
| Install: Python-pip  |
| Install: docker |
|Launch docker container: elk|

The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.

![alt text](https://github.com/laurapratt87/ELK-Stack-Project/blob/main/Linux/elk_docker.png "elk container success")

### Target Machines & Beats

This ELK server is configured to monitor the following machines:

| Name     | Function | IP Address |
|----------|----------|------------|
| Web-1    | Server   | 10.0.0.6   |
| Web-2    | Server   | 10.0.0.7   |
| Web-3    | Server   | 10.0.0.10  |

We have installed the following Beats on these machines. These Beats allow us to collect the following information from each machine:

- **Filebeat** can handle audit logs, deprecation logs, gc logs, server logs, and slow logs. 

- **Metricbeat** collects machine metrics. For example, Metricbeat can be used to monitor and analyze system CPU, memory and load.

The Kibana dashboard provides lots of system information, including: heatmap, sankey chart, response codes, unique visitors, total requests, etc. 
These data points are helpful for things like a [Kibana Exploration Activity](https://github.com/laurapratt87/ELK-Stack-Project/blob/main/Additional%20Resources/Kibana%20Exploration.docx).

### Using the Playbook

There were four playbooks used when creating this network.  

| Playbook     | Action(s) |
|----------|----------|
| [pentest_yml](https://github.com/laurapratt87/ELK-Stack-Project/blob/main/Ansible/pentest_yml.txt) | This is the initial playbook to add a container to the Jump Box and install docker.io, pip3, Docker python | 
| [ELK_playbook_yml](https://github.com/laurapratt87/ELK-Stack-Project/blob/main/Ansible/ELK_playbook_yml.txt) | This playbook increased the resources for the ELK server, added the container, and installed docker.io, pip3, Docker python  | 
| [filebeat-playbook_yml](https://github.com/laurapratt87/ELK-Stack-Project/blob/main/Ansible/filebeat-playbook_yml.txt) | This playbook pulled the download, config file, and .yml for Filebeat | 
| [metricbeat-playbook_yml](https://github.com/laurapratt87/ELK-Stack-Project/blob/main/Ansible/metricbeat-playbook_yml.txt) | This playbook pulled the download, config file, and .yml for Metricbeat  | 

  - [Playbooks and Config Files](https://github.com/joshblack07/UR-Cyber-Security-ELK-Stack-Project/tree/main/Ansible)
  
Since [pentest_yml](https://github.com/laurapratt87/ELK-Stack-Project/blob/main/Ansible/pentest_yml.txt) was the playbook created as part of the Cloud Security unit, it would need to be implemented first to have an Ansible control node configured. Assuming you have such a control node provisioned:

SSH into the control node and follow the steps below:

- Copy the playbook file to /etc/ansible.
- Update the host file to include the webservers and ELK server (and IP addresses).
  [hosts](https://github.com/laurapratt87/ELK-Stack-Project/blob/main/Ansible/hosts.txt)
- Run the playbook, and navigate to command line to check that the installation worked as expected.
- Playbook: [ELK_playbook](https://github.com/laurapratt87/ELK-Stack-Project/blob/main/Ansible/ELK_playbook_yml.txt)

To check if the ELK-SERVER is running, the URL is: http://13.64.78.144:5601/app/kibana. 

If you shut your machines down, and need to run this after restarting your ELK-SERVER VM, it would just need to be run with http://<your ELK-SERVER public ip>:5601/app/kibana.
This will only need to be done if you have a dynamic IP as it changes every time you shut down, and restart your machine. 
  
[Kibana Dashboad Success](https://github.com/laurapratt87/ELK-Stack-Project/blob/main/Additional%20Resources/Kibana_Dashboard.PNG)

## Bonus
[Link to Bonus: Commands](https://github.com/laurapratt87/ELK-Stack-Project/blob/main/Linux/Bonus%20Commands.txt)

## Group
- [Laura Pratt](https://github.com/laurapratt87) 
- [Josh Black](https://github.com/joshblack07)
- [Courtney Templeton](https://github.com/cltempleton1127)
