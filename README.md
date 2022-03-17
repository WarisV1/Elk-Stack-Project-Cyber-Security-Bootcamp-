# Elk-Stack-Project-Cyber-Security-Bootcamp-
Project for UPenn Cybersecurity Bootcamp, 03-10-2022.

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Map](https://user-images.githubusercontent.com/95242764/158420954-13dd77c7-5974-4cfd-8aef-5d7d6a510bae.png)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yml and config file may be used to install only certain pieces of it, such as Filebeat.

  - [First Playbook](https://github.com/WarisV1/Elk-Stack-Project-Cyber-Security-Bootcamp-/blob/76adea3d3ecd33f6fb87460a429715f19fb2d58b/First%20Playbook "First Playbook")
  - [Hosts](https://github.com/WarisV1/Elk-Stack-Project-Cyber-Security-Bootcamp-/blob/c4549dd7710c0aaf982b795acd4102031577fa68/Hosts "Hosts") 
  - [Ansible Config](https://github.com/WarisV1/Elk-Stack-Project-Cyber-Security-Bootcamp-/blob/c4549dd7710c0aaf982b795acd4102031577fa68/Ansible%20Config)
  - [Ansible Elk Config](https://github.com/WarisV1/Elk-Stack-Project-Cyber-Security-Bootcamp-/blob/c4549dd7710c0aaf982b795acd4102031577fa68/Ansible%20Elk%20Config)
  - [Filebeat Config](https://github.com/WarisV1/Elk-Stack-Project-Cyber-Security-Bootcamp-/blob/c4549dd7710c0aaf982b795acd4102031577fa68/Filebeat%20Config)
  - [Filebeat Playbook](https://github.com/WarisV1/Elk-Stack-Project-Cyber-Security-Bootcamp-/blob/c4549dd7710c0aaf982b795acd4102031577fa68/Filebeat%20Playbook)
  - [Metricbeat Config](https://github.com/WarisV1/Elk-Stack-Project-Cyber-Security-Bootcamp-/blob/c4549dd7710c0aaf982b795acd4102031577fa68/Metricbeat%20Config)
  - [Metricbeat Playbook](https://github.com/WarisV1/Elk-Stack-Project-Cyber-Security-Bootcamp-/blob/c4549dd7710c0aaf982b795acd4102031577fa68/Metricbeat%20Playbook)
  
This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly functional and available, in addition to restricting traffic to the network.
 What aspect of security do load balancers protect? What is the advantage of a jump box? 
 - Load balancers add resiliency by rerouting traffic from one service to another if a server becomes unavailble. Also the Jumpbox Provision is crucial as it prevents Azure VMs from being exposed to Public IP Addresses. This allows monitoring and logging in a single server. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network and system logs.

What does Filebeat watch for? 
- Filebeat monitors the log files or locations that you specify, collects log events/files.

What does Metricbeat record? 
- Metricbeat records statistics and metrics, collects and ships to either Elasticsearch or Logstash. 

The configuration details of each machine may be found below.

| Name              | Function        | IP Address               | Operating System   |
|-------------------|-----------------|--------------------------|--------------------|
| Jump Box          | Gateway         | 10.1.0.4 / 20.85.232.66  | Linux              |
| Web-1             | UbuntuServer    | 10.1.0.5 / 40.114.124.38 | Linux              |
| Web-2             | UbuntuServer    | 10.1.0.6 / 40.114.124.38 | Linux              |
| DVWA-VM3          | UbuntuServer    | 10.1.0.7 / 40.114.124.38 | Linux              |
| ELKserver         | UbuntuServer    | 10.2.0.4 / 20.84.136.248 | Linux              |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the **_Jump-Box-Provisioner_** machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- **_Workstation MY Public IPv4 through TCP 5601._**

Machines within the network can only be accessed by **_Workstation and Jump-Box-Provisioner through SSH Jump-Box._**

- Which machine did you allow to access your ELK VM?
  - **_Jump-Box-Provisioner IP : 10.1.0.4 via SSH port 22_**
- What was its IP address?
  - **_Workstation MY Public IP via port TCP 5601_**

A summary of the access policies in place can be found in the table below.
|        Name        |  Publicly Accessible  |          Allowed IP Addresses           |
|--------------------|-----------------------|-----------------------------------------|
| Jump Box           | Yes                   | 20.85.232.66 (Workstation IP on SSH 22) |
| Web-1              | No                    | 10.1.0.4 on SSH 22                      |
| Web-2              | No                    | 10.1.0.4 on SSH 22                      |
| DVWA-VM3           | No                    | 10.1.0.4 on SSH 22                      |
| ELKserver          | No                    | Workstation MY Public IP using TCP 5601 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- What is the main advantage of automating configuration with Ansible?
  - **_There are multiple advantages, Ansible lets you quickly and easily deploy multiple applications througe a YML playbook._**
  - **_You don't need to write custom code to automate your systems._**
  - **_Ansible will also figure out how to get your systems to the state you want them to be in._**


The playbook implements the following tasks:
- In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._

- - Specify a different group of machines:
      ```yaml
        - name: Config elk VM with Docker
          hosts: elk
          become: true
          tasks:
      ```
  - Install Docker.io
      ```yaml
        - name: Install docker.io
          apt:
            update_cache: yes
            force_apt_get: yes
            name: docker.io
            state: present
      ``` 
  - Install Python-pip
      ```yaml
        - name: Install python3-pip
          apt:
            force_apt_get: yes
            name: python3-pip
            state: present

          # Use pip module (It will default to pip3)
        - name: Install Docker module
          pip:
            name: docker
            state: present
            `docker`, which is the Docker Python pip module.
      ``` 
  - Increase Virtual Memory
      ```yaml
       - name: Use more memory
         sysctl:
           name: vm.max_map_count
           value: '262144'
           state: present
           reload: yes
      ```
  - Download and Launch ELK Docker Container (image sebp/elk)
      ```yaml
       - name: Download and launch a docker elk container
         docker_container:
           name: elk
           image: sebp/elk:761
           state: started
           restart_policy: always
      ```
  - Published ports 5044, 5601 and 9200 were made available
      ```yaml
           published_ports:
             -  5601:5601
             -  9200:9200
             -  5044:5044   
      ```

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

ELK Server 
![docker_ps_output_ELKserver](https://github.com/WarisV1/Elk-Stack-Project-Cyber-Security-Bootcamp-/blob/fc336f84368252925248d96ad4555fe85d234d53/docker_ps_output_ELKserver.png)

Jump-Box-Provisioner
![docker_ps_output_Jump-Box-Provisioner](https://github.com/WarisV1/Elk-Stack-Project-Cyber-Security-Bootcamp-/blob/fc336f84368252925248d96ad4555fe85d234d53/docker_ps_output_Jump-Box-Provisioner.png) 

Web-1 VM
![docker_ps_output_Web-1](https://github.com/WarisV1/Elk-Stack-Project-Cyber-Security-Bootcamp-/blob/fc336f84368252925248d96ad4555fe85d234d53/docker_ps_output_Web-1.png)

Web-2 VM
![docker_ps_output_Web-2](https://github.com/WarisV1/Elk-Stack-Project-Cyber-Security-Bootcamp-/blob/fc336f84368252925248d96ad4555fe85d234d53/docker_ps_output_Web-2.png) 

DVWA-VM3
![docker_ps_output_DVWA-VM3](https://github.com/WarisV1/Elk-Stack-Project-Cyber-Security-Bootcamp-/blob/fc336f84368252925248d96ad4555fe85d234d53/docker_ps_output_DVWA-VM3.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- List the IP addresses of the machines you are monitoring 
  - Web-1: 10.1.0.5
  - Web-2: 10.1.0.6
  - DVWA-VM3: 10.1.0.7

We have installed the following Beats on these machines:
- [Metricbeat Module Status Screenshot](https://github.com/WarisV1/Elk-Stack-Project-Cyber-Security-Bootcamp-/blob/22f9588642197fde3ef891bea2873e2d30c806f2/Metricbeat_data_successful.png)
- [Filebeat Module Status Screenshot](https://github.com/WarisV1/Elk-Stack-Project-Cyber-Security-Bootcamp-/blob/22f9588642197fde3ef891bea2873e2d30c806f2/Filebeat_data_successful.png)

These Beats allow us to collect the following information from each machine:

- Filebeat will be used to collect log files from very specific files such as Apache, Microsft Azure tools and web servers, MySQL databases.
  - [Filebeat Module Kibana Dashboard](https://github.com/WarisV1/Elk-Stack-Project-Cyber-Security-Bootcamp-/blob/8db89ad3edfc37841406f5daf9e695f13d13933e/Filebeat_Systems.png) 
- Metericbeat will be used to monitor VM stats, per CPU core stats, per filesystem stats, memory stats and network stats.
  - [Metricbeat Module Kibana Dashboard](https://github.com/WarisV1/Elk-Stack-Project-Cyber-Security-Bootcamp-/blob/8db89ad3edfc37841406f5daf9e695f13d13933e/Metricbeat_Docker_Overview_ECS_dashboard.png)

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 
- Verify the correct Public IP address and Security Rules.
SSH into the control node and follow the steps below:
- Copy the yml file to ansible directory.
- Update the config file to include remote users and ports. 
- Run the playbook, and navigate to Kibana(Personal Public IP Address):5601 to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_ 
   - For Metricbeat create [Metricbeat Playbook](https://github.com/WarisV1/Elk-Stack-Project-Cyber-Security-Bootcamp-/blob/8db89ad3edfc37841406f5daf9e695f13d13933e/Metricbeat%20Playbook)
   - For Filebeat create [Filebeat Playbook](https://github.com/WarisV1/Elk-Stack-Project-Cyber-Security-Bootcamp-/blob/8db89ad3edfc37841406f5daf9e695f13d13933e/Filebeat%20Playbook)
   - For Ansible create [Your Own Playbook](https://github.com/WarisV1/Elk-Stack-Project-Cyber-Security-Bootcamp-/blob/8db89ad3edfc37841406f5daf9e695f13d13933e/First%20Playbook)
   - And these should all be copied to your /etc/ansible location. 
   
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_ 
   - You update the /etc/ansible/hosts file with your VM IP Address. I also created two groups in the /etc/ansible/hosts file. One group named Elkserver which has the IP Address of the VM I will install ELK to. The other group is named webservers for the remaining VMs I install filebeat and metricbeat. 
- _Which URL do you navigate to in order to check that the ELK server is running? 
   - http://40.118.131.101:5601/app/kibana#/home
_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._ 

|            COMMAND                               | PURPOSE                                               |
|--------------------------------------------------|-------------------------------------------------------|                         
|`ssh-keygen`                                      |  create a ssh key for setup VM's                      |
|`sudo cat .ssh/id_rsa.pub`                        |  to view the ssh public key                           |
|`ssh azadmin@Jump-Box-Provisioner IP address`     |  to log into the Jump-Box-Provisioner                 |
| `sudo docker container list -a`                  | list all docker containers                            |
| `sudo docker start dremy_elbakyan`               | start docker container dremy_elbakyan                 |
|`sudo docker ps -a`                               |  list all active/inactive containers                  |
|`sudo docker attach dremy_elbakyan`               |  effectively sshing into the dremy_elbakyan container |
|`cd /etc/ansible`                                 | Change directory to the Ansible directory             |
|`ls -laA`                                         | List all file in directory (including hidden)         |
|`nano /etc/ansible/hosts`                         |  to edit the hosts file                               |
|`nano /etc/ansible/ansible.cfg`                   |  to edit the ansible.cfg file                         |
|`nano /etc/ansible/pentest.yml`                   |  to edit the My-Playbook                              |
|`ansible-playbook [location][filename]`           |  to run the playbook                                  |
|`sudo lsof /var/lib/dpkg/lock-frontend`           | unlocking a locked file                               |
|`ssh ansible@Web-1 IP address`                    |  to log into the Web-1 VM                             |
|`ssh ansible@Web-2 IP address`                    |  to log into the Web-2 VM                             |
|`ssh ansible@DVWA-VM3 IP address`                 |  to log into the DVWA-VM3 VM                          |
|`ssh ansible@ELKserver IP address`                |  to log into the ELKserver VM                         |
|`exit`                                            | to exit out of docker containers/Jump-Box-Provisioners|
|`nano /etc/ansible/ansible.cfg`                   |  to edit the ansible.cfg file                         |
|`nano /etc/ansible/hosts`                         |  to edit the hosts file                               |
|`nano /etc/ansible/pentest.yml`                   |  to edit the My-Playbook                              |
|`ansible-playbook [location][filename]`           |  to run the playbook                                  |
|`sudo apt-get update` 				                     |  this will update all packages                        |
|`sudo apt install docker.io`				               |  install docker application		                       |
|`sudo service docker start`				               |  start the docker application                         |
|`sudo systemctl status docker`				             |  status of the docker application                     |
|`sudo systemctl start docker`                     |  start the docker service                             |
|`sudo docker pull cyberxsecurity/ansible`	       |  pull the docker container file                       |
|`sudo docker run -ti cyberxsecurity/ansible bash` |  run and create a docker container image              |
|`ansible -m ping all`                             |  check the connection of ansible containers           |
|`curl -L -O [location of the file on the web]`    |  to download a file from the web                      |
|`dpkg -i [filename]`                              |  to install the file i.e. (filebeat & metricbeat)     |
|`http://20.84.136.248:5601//app/kibana`           | Open web browser and navigate to Kibana Logs          |
|`nano filebeat-config.yml`                        | create and edit filebeat config file                  |
|`nano filebeat-playbook.yml`                      | write YAML file to install filebeat on webservers     |
|`nano metricbeat-config.yml`                      | create metricbeat config file and edit it             |
|`nano metricbeat-playbook.yml`                    | write YAML file to install metricbeat on webservers   |  
------------------------------------------------------------------------------------------------------------

