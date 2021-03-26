# NW_CyberSecurity
Northwestern CyberSecurity Boot Camp
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![CyberSecurity_Project1_Drawing.pdf](Images/diagram_filename.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

| Script Name                 | Script Description                                   |
|-----------------------------|------------------------------------------------------|
| ansible.cfg                 | Variables used to control Ansible                    |
| filebeat-playbook.yml       | Playbook file to deploy filebeats to webservers      |
| files/filebeat-config.yml   | Filebeat config file                                 |
| files/metricbeat-config.yml | Metricbeat config file                               |
| hosts                       | Anisble Host file, contains target hosts             |
| install-elk.yml             | Playbook file to deploy the ELK stack to Elk server  |
| metricbeat-playbook.yml     | Playbook file to deploy metricbeats to webservers    |
| pentest.yml                 | Playbook file to deploy DVWA container on webservers |

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting denial of service attacks to the network.
- _TODO: 
  -What aspect of security do load balancers protect?  Availibility/Resilancy, protection agains DoS
  -What is the advantage of a jump box? A jump box can limit administrative access to a network, with enhanced security.  When combined with a tool like ansible it can automate tasks as well.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
- _TODO: What does Filebeat watch for?_Logs, or changes in data, e.g. Audit Logs
- _TODO: What does Metricbeat record?_System Resources, e.g. CPU Usage, Memory, Disk and Network activity.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name                     | Function      | Internal IP Address | Exernal IP Address | Operating System |
|--------------------------|---------------|---------------------|--------------------|------------------|
| Jump Box Provisioner     | Gateway       | 10.0.0.4            | 104.43.233.245     | Ubuntu 20.04 LTS |
| DVWA-VM3                 | Webserver     | 10.0.0.5            |                    | Ubuntu 20.04 LTS |
| DVWA-VM4                 | Webserver     | 10.0.0.6            |                    | Ubuntu 20.04 LTS |
| Web1                     | Webserver     | 10.0.0.7            |                    | Ubuntu 20.04 LTS |
| Web2                     | Webserver     | 10.0.0.8            |                    | Ubuntu 20.04 LTS |
| Elk01                    | ELK server    | 10.1.0.4            | 52.229.29.62       | Ubuntu 20.04 LTS |
| CyberSecurity_Lab01_LB01 | Load Balancer |                     | 20.37.132.27       |                  |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box and ELK Server machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _TODO: Add whitelisted IP addresses_ 73.208.12.246

Machines within the network can only be accessed by the Jump Box running an Ansible Container.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name                     | Publicly Acessible | Allowed IP Address                           |
|--------------------------|--------------------|----------------------------------------------|
| Jump Box Provisioner     | No                 | 73.208.12.246 -> TCP/22                      |
| CyberSecurity_Lab01_LB01 | No                 | 73.208.12.246 -> TCP/80                      |
| ELK01                    | No                 | 10.0.0.4 -> TCP/22 73.208.12.246 -> TCP/5601 |
| Web1                     | No                 | 10.0.0.4 -> TCP/22                           |
| Web2                     | No                 | 10.0.0.4 -> TCP/22                           |
| DVWA1                    | No                 | 10.0.0.4 -> TCP/22                           |
| DVWA2                    | No                 | 10.0.0.4 -> TCP/22                           |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_ Consistancy, a close second would be time.

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- Download Docker and Python3
- Install Docker and Python3
- Set virtual memory allocation
- Download, Launch, and Configure Elk container network settings
- Enable Docker to start at boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Images\ELK01_container_capture.PNG]

### Target Machines & Beats
This ELK server is configured to monitor the following machines:

| Elk Monitored Hosts |
|---------------------|
| DVWA1/10.0.0.5      |
| DVWA2/10.0.0.6      |
| Web1/10.0.0.7       |
| Web2/10.0.0.8       |

We have installed the following Beats on these machines:

| Host           | Beats Installed |
|----------------|-----------------|
| DVWA1/10.0.0.5 | File & Metric   |
| DVWA2/10.0.0.6 | File & Metric   |
| Web1/10.0.0.7  | File & Metric   |
| Web2/10.0.0.8  | File & Metric   |
| ELK01/10.1.0.4 | File & Metric   |

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
- Filebeat: Monitors changes in logs
- Metricbeat Monitors system resources like CPU Usage, Memory, Disk and Network utilization

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the ansible.cfg & host file to /etc/ansible folder.
- Update the host file to include targert servers
- Copy the install-elk.yml file to the /etc/ansible folder.
- Run the playbook, and navigate to Kibana, Add Log Data, System Logs, Check Data Source (http://KibannaExt:5601/app/kibana) to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_install-elk.yml -> /etc/ansible
- _Which file do you update to make Ansible run the playbook on a specific machine? host file
    -How do I specify which machine to install the ELK server on versus which to install Filebeat on?_Use a heading [ELK], place the target IP under this heading.
- _Which URL do you navigate to in order to check that the ELK server is running?  http://52.229.29.62:5601/app/kibana 

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
