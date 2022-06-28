# UPENN ELKstack Project
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![image](https://github.com/ozk649/UPENN-ELKSTACK-PROJECT/blob/main/Diagram/Project%201.jpg)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

- [Helpful Linux Commands](https://github.com/ozk649/UPENN-ELKSTACK-PROJECT/blob/main/Bash/command-ssh-keygeneration.txt)
- [Docker/Python Playbook](https://github.com/ozk649/UPENN-ELKSTACK-PROJECT/blob/main/Ansible/pentest.yml)
- [ELK installation Playbook](https://github.com/ozk649/UPENN-ELKSTACK-PROJECT/blob/main/Ansible/install-ELK.yml)
- [Filebeat Playbook](https://github.com/ozk649/UPENN-ELKSTACK-PROJECT/blob/main/Ansible/filebeat-playbook.yml)
- [Metricbeat Playbook](https://github.com/ozk649/UPENN-ELKSTACK-PROJECT/blob/main/Ansible/metricbeat-playbook.yml)

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
- A Jumpbox acts as a gateway to the network, providing a single point of entry to the network which reduces the attack surface and also provides for better manageability for the deployment of various system components.
- A Load balancer helps distribute service load across the system to provide high availability. It also provides a single point of entry to the network for users accessing a service running on the system.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log files and system resources.
- Filebeat is an agent that collects system events such as logins (events, locationsc, etc)  and sends it the ELK stack as an output for monitoring and reporting purposes.
- Metricbeat is an agent that collects metrics and statistics from the operating system and services running on the server and sends the data to the ELK stack as an output.

The configuration details of each machine may be found below.


| Name         | Function                                            | IP Address | Operating System |
|----------    |-----------------------------------------------------|------------|------------------|
| Jump Box     | Gateway                                             | 10.1.0.4   | Linux            |
| ELK Stack    | Kibana/Filebeat/Metricbeat                          | 10.2.0.4   | Linux            |
| LoadBalancer | User service access gateway/high availability tool  | 10.1.0.x   | Linux            |
| Web 1        | Webserver host                                      | 10.1.0.12  | Linux            |
| Web 2        | Webserver host                                      | 10.1.0.13  | Linux            |
| Web 3        | Redundant webserver host                            | 10.1.0.14  | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine was only allowed from the following IP addresses:
- 173.91.28.219
Machines within the network can only be accessed by the Jumpbox.

SSH access was set up for ELK VM from within the internal network on port 22. For the Kibana service running on the server, access was granted to the external IP of 173.91.28.219 for testing purposes.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses                           |
|----------|---------------------|----------------------------------------------- | 
| Jump Box | Yes                 | 10.1.0.X, 173.91.28.219 (port #22, #80)        |
| ELK      | Yes                 | 10.1.0.4 (port #22), 173.91.28.219 (port #5601)|
| Web-1    | Yes                 | 10.1.0.4, Load Blanacer (52.170.235.87)        |
| Web-2    | Yes                 | 10.1.0.4, Load Blanacer (52.170.235.87)        |
| Web-3    | Yes                 | 10.1.0.4, Load Blanacer (52.170.235.87)        |
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because docker containers can be deployed enmasse with consistency across various servers.

The playbook implements the following tasks:
- Installed Docker, python, DVWA on the ELK Machine
- Installed the ELK stack on the ELK machine
- lnstalled Filebeat, Metricbeat on the ELK machine
The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

  ![image](https://user-images.githubusercontent.com/44678341/159196357-ebcb4cef-5085-4c90-a1b3-f47ace5399ef.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.1.0.12
- 10.1.0.13
- 10.1.0.14

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat collects login events; such as you can view data on user IDs used to attempt login, along with date and time.
- Metricbeat collects operating system data; for instance CPU usage, memory usage, etc.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the install filebook yml file/files to /etc/ansible directory or subdirectories.
- Update the host file to include webservers (Web-1, Web-2, Web-3) and ELK (10.2.0.4)
- Run the playbook, and navigate to http://[loadbalancerIP:5601]/app/kibana to check that the installation worked as expected.
- The ansible host file (located in /etc/ansible/ directory) needed to be updated for IP addresses and names where Ansible ran the playbooks.
- Config files for ELK and Filebeat/Metricbeat determine which machines Ansible will run the playbook on (screenshot below)

    ![image](https://user-images.githubusercontent.com/44678341/159197539-70064e61-a55a-412c-87f5-17a205426429.png)

- http://[loadbalancerIP:5601]/app/kibana can be used in order to check that the ELK server is running

- Specific Commands:
        - To create a playbook file: touch [playbook file name].yml
        - To run the Ansible playbooks:/etc/ansible/: ansible-playbook [playbook file name]   
        - To update configuration files: nano [file name]
        - To download Filebeat/Metric beat configuration files: curl [URL of the config file] 
