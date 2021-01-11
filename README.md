## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

[ELK Network Diagram](images/network_diagram_elk.JPG)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the everything.yml file may be used to install only certain pieces of it, such as Filebeat.
The everything.yml file combines the individual playbooks used in this setup. I recommend using the individual playbooks rather than the 'everything' file when recreating this build.
The "everything' file only contains the playbooks, you will still need to update your ansible hosts file, the metricbeat config file, and filebeat config file as appropriate for your build.

  - [Everything Playbook](Ansible/everything.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly reliable, in addition to restricting traffic to the network.

Load balancers have become a intergral part in thrawting DDoS attacks, by shifting traffic from one server to a public cloud provider.
Load balancers can also be configured to alter or remove HTTP headers, making it a bit more difficult for attackers to obtain useful
information that can be used in an attack.

The advantage of using a jump box is the increased security. It further segregates a target network and users by adding another layer of
segregation. Jump boxes are also generally used for amdinistrative purposes, so there aren't any risks of opening a maliciuous email or 
clicking a malicious link in a web browser. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the system services and system log data.
 - Beats modules are used as lightweight data shippers, that centralize and forward data for monitoring purposes. There are many different
 Beats modules, in this stack, we will be using Filebeat and Metricbeat.
- Filebeat collects log data from the servers/machines it is installed onto, making it easier to monitor any log changes across multiple machines.
- Metricbeat collects metrics, such as disk usage and memory usage for various system processes and services. 

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function   | IP Address | Operating System |
|----------|----------  |------------|------------------|
| Jump Box | Gateway    | 10.0.0.4   | Linux            |
| Web1     | DVWA Server| 10.0.0.7   | Linux            |
| Web2     | DVWA Server| 10.0.0.6   | Linux            |
| Web3     | DVWA Server| 10.0.0.8   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
67.165.199.178

Machines within the network can only be accessed by the Jump Box via a Docker Ansible container.
The Jump Box with IP 10.0.0.4 is how the Elk server is accessed.


A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 67.165.199.178       |
| ELK      | No                  | 10.0.0.4             |
|          |                     |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because
automating the configuration means we can configure every machine in our network at the same time, rather than going through each one
individually. In this case we've only congfigured 1 ELK server, and 3 webservers using Ansible,  but imagine having a system with dozens
or hundreds of machines. Automation saves a lot of time and headaches.

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- Installs Docker
- Installs Python3 Pip and the Docker Module via Pip
- Increases and allows enough system memory to allow ELK to run
- Finally, downloads sebp/elk image and launches it.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

[docker ps](images/docker_ps_elk.JPG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:

| Name     | IP Address |
|----------|------------|
| Web1     | 10.0.0.7   |
| Web2     | 10.0.0.6   |
| Web3     | 10.0.0.8   |


We have installed the following Beats on these machines:
- Filebeats
- Metricbeats

These Beats allow us to collect the following information from each machine:
Filebeat will harvest log data from specified locations, such as /var/logs/*.log and send it out to Elasticsearch. You should expect to see syslog data and SSH login data.
Metricbeat will periodically collect metrics from system process and services running. You should expect to see CPU and memory usage. 


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the [elk_install.yml](Ansible/elk_install.yml) file to /etc/ansible/playbooks (note: you don't need a "playbooks" directory, but it helps to keep things organized.)
- Update the [hosts](Ansible/Ansible_host_example) file to include your ELK server IP address. You will need to create a new host group, I used ELK, and supply the ELK server IP 
  address within the new group.
- Run the playbook, and navigate to your ELK server IP via SSH and run 'docker ps' to check that the installation worked as expected.
- Once everything is running, you should be able to view the data by going to http://<ELK server public IP>:5601/app/kibana
