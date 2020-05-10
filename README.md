## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/erfanwhb/ELKserver-Project/blob/master/images/ELK-stack-network.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook.yml file may be used to install only certain pieces of it, such as Filebeat.

https://github.com/erfanwhb/ELKserver-Project/blob/master/Resources/dvwa-playbook.yml      
https://github.com/erfanwhb/ELKserver-Project/blob/master/Resources/elk-playbook.yml    
https://github.com/erfanwhb/ELKserver-Project/blob/master/Resources/filebeat-playbook.yml        
https://github.com/erfanwhb/ELKserver-Project/blob/master/Resources/metricbeat-playbook.yml    


This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available (protect the availability aspect) by managing the inbound traffic to the virtual machines in addition to restricting the DDoS attack to the network.

The jump box is identical to the gateway router. It is exposed to the public internet and sits in front of other machines that are not exposed to the public internet. It controls access to the other machines by allowing connections from specific IP addresses and forwarding to those machines.

ELK stack server will allow you to monitor every machine on the network. Easily collect logs from multiple machines into a single database and Quickly execute complex searches, Build graphs, charts and visualize all network data on readable and relible dashboard called Kibana.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to a specific files such as /etc/passwd and/or specific system information such as machine's uptime.


- Filebeat is built to collect data about specific files on remote machines, it must be installed on the VMs that you want to monitor. 


- Metricbeat makes it easy to collect specific information about the machines in the network and tells analysts how "healthy" they are.

The configuration details of each machine may be found below.


|          Name         |   Function   | IP Address | Operating System |
|:---------------------:|:------------:|:----------:|:----------------:|
| Jump-Box-Provesioner  |    Gateway   |  10.0.0.4  |       Linux      |
| DVWA-VM1              |  VM w/Docker |  10.0.0.5  |       Linux      |
| DVWA-VM2              |  VM w/Docker |  10.0.0.7  |       Linux      |
| DVWA-VM3              |  VM w/Docker |  10.0.0.8  |       Linux      |
| DVWA-VM4              |  VM w/Docker |  10.0.0.9  |       Linux      |
| Elk-Server            | ELK w/Docker |  10.0.0.6  |       Linux      |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box Provesioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP address 52.191.187.216     (The IP changes every time that machine start)

Machines within the network can only be accessed by JumpBoxProvisioner.

Only JumpBoxProvisioner machine can access the ELK VM and its IP address is 52.183.37.77 (The IP changes every time the ELK start)

A summary of the access policies in place can be found in the table below.

|          Name         | Publicly Accessible | Allowed IP Address |
|:---------------------:|:-------------------:|:------------------:|
| Jump-Box-Provesioner  |         Yes         |   52.191.187.216   |
| DVWA-VM1              |          No         |      10.0.0.5      |
| DVWA-VM2              |          No         |      10.0.0.7      |
| DVWA-VM3              |          No         |      10.0.0.8      |
| DVWA-VM4              |          No         |      10.0.0.9      |
| Elk-Server            |          No         |      10.0.0.6      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allowed making configurations and moving files on multiple servers from one control machine here in this case from an ansible running from JumpBoxProvisioner.

The playbook implements the following tasks:
- Create a new VM within the network.
- Download and configure an ELK stack Docker container on the new VM.
- Install Filebeat and Metricbeat on the other machines on the network.


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/erfanwhb/ELKserver-Project/blob/master/images/elk-container.png 

### Target Machines & Beats
This ELK server is configured to monitor the following machines:

|   Name   | IP Address |
|:--------:|:----------:|
| DVWA-VM1 |  10.0.0.5  |
| DVWA-VM2 |  10.0.0.7  |
| DVWA-VM3 |  10.0.0.8  |
| DVWA-VM4 |  10.0.0.9  |

We have installed the following Beats on these machines:

|   Name   |           Beats           |
|:--------:|:-------------------------:|
| DVWA-VM1 | File Beat And Metric Beat |
| DVWA-VM2 | File Beat And Metric Beat |
| DVWA-VM3 | File Beat And Metric Beat |
| DVWA-VM4 | File Beat And Metric Beat |

These Beats allow us to collect the following information from each machine:

Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.

Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the configuration and playbook files to the target directories.
- Update the hosts and ansible.cfg files to include the target machine's IP.
- Run the playbook, and navigate to elk webpage (Kibana) to check that the installation worked as expected.

The playbook Beat files are located in /etc/ansible/roles/ And the configuration files are located in directory created under the ansible such as /etc/ansible/files/ 

When we run The playbook files they are using the configuration files to download and install Beats files in the target machines. 
Usualy we update the configuration files to be applicable with the exact source location in the ansible.

By updating the file /etc/ansible/hosts with the target machine's IP addresses to specify where the Beats installed.

After running the Beats playbooks Navigate to the URL (public IP of the elk:5601) to check the collected log data.

## CMD's used to run the playbooks:

- eval "$(ssh-agent -s)"
- ssh-add                                                   
- ansible -m ping all                                                                              
- ansible-playbook dvwa-playbook.yml                                                                      
- ansible-playbook elk-playbook.yml                                                                     
- ansible-playbook filebeat-playbook.yml                                                                     
- ansible-playbook metricbeat-playbook.yml                                                                   

## files 

https://github.com/erfanwhb/ELKserver-Project/blob/master/Resources/dvwa-playbook.yml   
https://github.com/erfanwhb/ELKserver-Project/blob/master/Resources/elk-playbook.yml   
https://github.com/erfanwhb/ELKserver-Project/blob/master/Resources/filebeat-playbook.yml    
https://github.com/erfanwhb/ELKserver-Project/blob/master/Resources/metricbeat-playbook.yml     
