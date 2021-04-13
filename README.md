# Elk-Stack-Project
Project 1 from Cybersecurity Bootcamp

The files in this repository were used to configure the network depicted below.

https://go.gliffy.com/go/publish/13479233

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the __yml___ file may be used to install only certain pieces of it, such as Filebeat.


  - nano /etc/ansible/roles/filebeat-playbook.yml
  - ---
- name: installing and launching filebeat
  hosts: elk
  become: yes
  tasks:
  - name: download filebeat deb
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb
  - name: install filebeat deb
    command: dpkg -i filebeat-7.4.0-amd64.deb
  - name: drop in filebeat.yml
    copy:
      src: /etc/ansible/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml
  - name: enable and configure system module
    command: filebeat modules enable system
  - name: setup filebeat
    command: filebeat setup
  - name: Start filebeat service
    command: service filebeat start
    

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
- The aspect of security that Load Balancers protect is an organization against DDoS attacks. 
- The advantage of a jump box is that it is a secure computer that all administrators first connect to before launching any administrative task or use as an origination point to connect to other servers or untrusted environments

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
- Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
- Metricbeat periodically collects metrics from the operating system and from services running on the server.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name       | Function  | IP Address   | Operating System |
|---------------|--------------|------------------|--------------------------|
| Jump Box | Gateway  |70.37.97.139| Linux                     |
| ElkVM      |  VM1        |10.0.0.4        | Linux                     |
| Web1       |  VM2        |10.1.0.7        | Linux                     |
| Web2       |  VM3        |10.1.0.8        | Linux                     |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 75.17.244.59 (public ip address of my local machine)

Machines within the network can only be accessed by __SSH___.
- My ElkVM is accessed through my local machine  IP address 75.17.244.59

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 |75.17.244.59       |
| Web 1    | Yes                 |10.1.0.4  70.37.97.139|
| Web 2    | Yes                 |10.1.0.4  70.37.97.139|

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- The primary benefit of Ansible is it allows IT administrators to automate away the drudgery from their daily tasks. That frees them to focus on efforts that help deliver more value to the business by spending time on more important tasks.

The playbook implements the following tasks:
- Install Docker 
- Install python3-pip
- Install Docker module
- Increase virtual memory
- Use more memory
- Download and launch a docker elk container
- List the ports that ELK runs on


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.




### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.4
- 10.1.0.7
- 10.1.0.8

We have installed the following Beats on these machines:
- 10.0.0.4 Elk-VM

These Beats allow us to collect the following information from each machine:
-  Filebeat monitors the log files or locations that you specify, collects log events, and forwards to Elasticsearch for indexing.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-7.4.0-amd64.deb to /etc/filebeat/filebeat.yml
- Update the filebeat-playbook.yml file to include filebeat modules enable system, filebeat setup, service filebeat start
- Run the playbook, and navigate to http://13.91.64.255:5601/app/kibana

