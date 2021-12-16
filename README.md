# Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![alt text](https://github.com/UndyingDest/ELK_Stack_Deployment/blob/main/Diagrams/ELK_Diagram.png "ELK Network Diagram")

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yaml file may be used to install only certain pieces of it, such as Filebeat.

[Elk Playbook](https://github.com/UndyingDest/ELK_Stack_Deployment/blob/main/Ansible/ELK_pb.yml)

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Damn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

* Having a jump box keeps public network exposure to a minimum, allowing a smaller attack surface over the network. Adding a load balancer assists in keeping the machines available during heavy traffic loads.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network and system files.

* Filebeat monitors changes in files on the host system along with collection of logs, making potential malicious activity easier to spot.
* Metricbeat monitors system and network resource usage on host system, allowing a security analyst to monitor for potential malicious activity.

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System    |
|----------|----------|------------|---------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux (ubuntu 20.04)|
| Web-1    | Endpoint | 10.0.0.5   | Linux (ubuntu 20.04)|
| Web-2    | Endpoint | 10.0.0.6   | Linux (ubuntu 20.04)|
| ELK      | Reporting| 10.1.0.4   | Linux (ubuntu 20.04)|

### Access Policies

The machines on the internal network are not exposed to the public internet. 

Only the local machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

* 35.107.163.84

Machines within the network can only be accessed by the jump box at 10.0.0.4.


A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 35.107.163.84        |
| ELK      | No                  | 10.0.0.4             |
| Web-1/2  | No                  | 10.0.0.5             |
| Web-1/2  | No                  | 10.0.0.6             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which makes it easy to add machines with the required configuration, update existing machines with new software configurations, or rebuild compromised machines.

The playbook implements the following tasks:

* Install Docker
* Install Python3
* Install and setup ELK stack with specified ports
* Enable Docker on machine boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![alt text](https://github.com/UndyingDest/ELK_Stack_Deployment/blob/main/Linux/Docker_Ps.png "Docker ps")

### Target Machines & Beats
This ELK server is configured to monitor the following machines:

* Web-1: 10.0.0.5
* Web-2: 10.0.0.6

We have installed the following Beats on these machines:

* Filebeat
* Metricbeat

These Beats allow us to collect the following information from each machine:

Filebeat reports log data from installed filebeat modules. These logs are sent to the ELK stack and visible for querying in Elasticsearch or logstash.

Metricbeat collects information on installed systems, reporting information such as network traffic and cpu usage. Metricbeat helps keep an eye on the health and resource usage of installed systems. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

* Copy the ELK_pb.yml file to your Ansible vm to /etc/ansible/.
* Update the /etc/ansible/hosts file to include the IP addresses for your webservers and ELK server.
* Run the playbook and navigate to HTTP://<ELK_VM_public_ip>:5601/app/kibana/ to check that the installation worked as expected.

- _Which file is the playbook? Where do you copy it?

  * ELK_pb.yml is the playbook you'll need to set up your ELK vm's Docker instance. Copy this file to /etc/ansible/ on your Ansible node.

- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?

	Make sure to update your hosts file in /etc/ansible/ to include the IP addresses of your webservers and ELK server.

- _Which URL do you navigate to in order to check that the ELK server is running?

	Open a new tab on your workstation and browse to HTTP://<ELK_VM_public_ip>:5601/app/kibana/ which will open the Kibana web portal

### Setting up from Ansible jumpbox

	1. SSH to the machine your Ansible machine and download the following yml playbooks to /etc/ansible/
		* ELK_pb.yml
		* dvwa_pb.yml
		* filebeat_config.yml
		* filebeat-pb.yml
		* metricbeat_config.yml
		* metricbeat_pb.yml

	2. Download the latest filebeat.deb and metricbeat.deb files to the /etc/ansible/ folder

	3. Update the Ansible hosts file with the target IPs for your DVWA webservers and ELK webserver.

	4. Update the filebeat-pb and metricbat-pb with the cuirrent version of filebeat.deb and metricbeat.deb

	5. Run the the playbooks in order below
		* dvwa_pb.yml (Installs DVWA)
		* ELK_pb.yml (Installs ELK container)
		* filebeat_pb.yml (Installs filebeat on ELK vm)
		* metricbeat_pb.yml (Installs metricbeat on ELK vm)

	6. Navigate to Kibana (HTTP://<ELK_VM_public_ip>:5601) and add filebeat/metricbeat as sources

	Optional: SSH to DVWA webservers to generate traffic
