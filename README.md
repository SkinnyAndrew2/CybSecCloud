## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

(diagrams/Project_1_network.jpg)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.

  ansible/
	ansible.cfg
	elk-playbook.yml
	filebeat-configuration.yml
	filebeat-playbook.yml
	metricbeat-configuration.yml
	metricbeat-playbook.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Demonstration Vulnerable Web Application.

Load balancing ensures that the application will be highly redundant, in addition to restricting outside traffic to the network.
- What aspect of security do load balancers protect? Load balancers prevent direct access to the Webservers and hides their IP addresses. What is the advantage of a jump box? The advantage of a jump box is that it enables there to only be the jumpbox that exposes the one outside address for administration access. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the configuration files and system files/logs.
- What does Filebeat watch for? Filebeat watches for SSH logins, linux account logs, and sudo commands
- What does Metricbeat record? Metricbeat watches for CPU, RAM, and network usage.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name         | Function                 | IP Address | Operating System            |
|--------------|--------------------------|------------|-----------------------------|
| Jump Box     | Gateway                  | 10.10.0.4  | Ubuntu Linux                |
| ELKServer    | Elastic Stack Server     | 10.1.0.4   | Ubuntu Linux                |
| Webserver1   | Webserver                | 10.0.0.6   | Ubuntu Linux                |
| Webserver2   | Webserver                | 10.0.0.5   | Ubuntu Linux                |
| LoadBalancer | Webserver Load Balancer  | 10.0.0.2   | Azure Virtual Load Balancer |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Add whitelisted IP addresses - My personal PC on my home network has the only IP address that gets access to the Jumpbox, due to being the only external machine with the RSA Public Key.

Machines within the network can only be accessed by Jumpbox.
- Which machine did you allow to access your ELK VM? What was its IP address?
  - Jumpbox gets access ELK VM. Jumpbox IP: 10.10.0.4

A summary of the access policies in place can be found in the table below.

| Name        | Publicly Accessible | Allowed IP Addresses          |
|-------------|---------------------|-------------------------------|
| Jump Box    | Yes                 | 10.1.0.4                      |
| Webservers  | Port TCP 443 only   | 10.0.0.2                      |
| LoadBalancer| Port TCP 443 only   |                               |
| ELKServer   | No                  | 10.10.0.4, 10.0.0.6, 10.0.0.5 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because of ease of use, deployment, and prone to less human errors.
- What is the main advantage of automating configuration with Ansible?
  - Ease of use, faster deployment times, and less prone to mistakes

The playbook implements the following tasks:
- _In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- Install the docker application
- Install pip3
- Install python docker module
- Allocated more memory for the ELK VM
- Download and launch an elk container and allowed it to start on reboot of VM

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

(Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- List the IP addresses of the machines you are monitoring
  - Webserver 1: 10.0.0.6
  - Webserver 2: 10.0.0.5

We have installed the following Beats on these machines:
- Specify which Beats you successfully installed - The beats that were successfully installed were Filebeat and Metricbeat

These Beats allow us to collect the following information from each machine:
- In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc.
  - Filebeat collects SSH logins, linux logins, and sudo commands
  - Metricbeat collects CPU, RAM, and network usage.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the yaml file to /etc/ansible.
- Update the `hosts` file to include the ip addresses of the groups of VMs that you want the script to install to
- Run the playbook, and navigate to `/etc/filebeat` to check that the installation worked as expected.

Answer the following questions to fill in the blanks:_
- Which file is the playbook? Where do you copy it? - The file that is the playbook is filebeat.yml and it is copied into the /etc/ansible folder
- Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
   - `/etc/ansible/hosts` file
- _Which URL do you navigate to in order to check that the ELK server is running?
  - http://elkip:5601

