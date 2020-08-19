## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

!ELK Project Diagram.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  filebeat.yml

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly reliable, in addition to restricting traffic to the network.

What aspect of security do load balancers protect? 
Protects organization from denial-of-service attacks by adding layers of security to the website without changes to the application and directing attack traffic away from important servers.
What is the advantage of a jump box?_
You can access and manage devices on a separate security zone instead of on your own. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the metrices and system logs.

What does Filebeat watch for?_
Forwards and centralizes log data by monitoring the log files or locations, collects log events and fowards the information to Elasticsearch.

What does Metricbeat record?_
Collects metrics and statistics then ships them to a specified output. 

The configuration details of each machine may be found below.


| Name      | Function               | IP Address | Operating System |
|-----------|------------------------|------------|------------------|
| Jump Box  | Gateway                | 10.0.0.5   | Linux            |
| Web 1     | Connect to ELK website | 10.0.0.9   | Linux            |
| Web 2     | Connect to ELK website | 10.0.0.10  | Linux            |
| Web 3     | Connect to ELK website | 10.0.0.11  | Linux            |
| ELK Stack | Hosts ELK Server       | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the ELK machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
10.0.0.0/24 and my personal IP address.

Machines within the network can only be accessed by ELK VM and 10.1.0.4.

A summary of the access policies in place can be found in the table below.

| Name      | Publicly Accessible | Allowed IP Addresses                           |
|-----------|---------------------|------------------------------------------------|
| Jump Box  | No?                 | 10.0.0.0/24, 52.149.230.186 and my personal IP |
| Web 1     | No                  | 10.0.0.0/24                                    |
| Web 2     | No                  | 10.0.0.0/24                                    |
| Web 3     | No                  | 10.0.0.0/24                                    |
| ELK Stack | Yes                 | 10.0.0.0/24 and my personal IP                 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

What is the main advantage of automating configuration with Ansible?_
It helps with productivity by automating tedious tasks through Ansible so that users could work on other, more important tasks. 

The playbook implements the following tasks:

- Create an nsg, and virtual network in Azure, then creating the virtual machine to connect to both. 
- Once connected to the vm, download and configure Ansible within VM.
- Through Ansible, download Docker and be sure you can connect to the ELK Server, check to see if you can connect to Kibana
- Within the ELK server, install and configure filebeat and metricbeat

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance:

---
- name: Configure Elk VM with Docker
  hosts: elkservers
  become: true
  tasks:
    - name: Install docker.io
      apt:
        force_apt_get: yes
        name: docker.io
        state: present

    - name: Install python-pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

    - name: Install Docker module
      pip:
        name: docker
        state: present

    - name: Increase Virtual Memory
      sysctl:
        name: vm.max_map_count

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
10.0.0.9, 10.0.0.10, 10.0.0.11

We have installed the following Beats on these machines:
Filebeats and Metricbeats

These Beats allow us to collect the following information from each machine:
Filebeat collects data to libbeat (aggregates the events and sends the data to the output you configured). Metricbeat collects metrics from: Apache, HAProxy, MongoDB, MySQL, Nginx, PostgreSQL, Redis, System, and Zookeeper.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the YAML file to /etc/ansible/pentest 
- Update the HOSTS file to include...username and VM IP
- Run the playbook, and navigate by SSH-ing to the new VM from Ansible to check that the installation worked as expected.

Answer the following questions to fill in the blanks:_

-Which file is the playbook? Where do you copy it?_ 
Usually the file that ends with ".yml" is considered a playbook, but it's best to label it as such. You 'copy' it to the /etc/ansible directory (for this project)
 
-Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
You update the HOSTS file to make Ansible to run the playbook. To install ELK, you must create a YAML cofig file to: /etc/ansible and edit the HOSTS file to include the elkserver user with the ELK VM public IP. To install filebeat, must create a file into the /etc/ansible/roles file path, then create and configure a YAML filebeat playbook. 

-Which URL do you navigate to in order to check that the ELK server is running?
http://ELK.VM.IP:5601

**Bonus**
First, be sure to edit the HOSTS file to include the username and IP of your VM.

Create YAML playbook file:
'nano /etc/ansible/filename.yml' (then configure what you want your playbook to do. )

When finished, (configuring your your playbook) run your playbook: 
'ansible-playbook filename.yml'

After the playbook successfully configures and updates all changes, SSH into that VM to be sure the server works. 'ssh username@vm_configured_IP'

Last but not least, test the connection 'curl localhosts/setup.php
