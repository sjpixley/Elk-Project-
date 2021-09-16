## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](Images/diagram_filename.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

  - _TODO: 
  --
- name: Config Web VM with Docker
  hosts: webservers
  become: true
  tasks:
  - name: docker.io
    apt:
      force_apt_get: yes
      update_cache: yes
      name: docker.io
      state: present

  - name: Install pip3
    apt:
      force_apt_get: yes
      name: python3-pip
      state: present

  - name: Install Docker python module
    pip:
      name: docker
      state: present

  - name: download and launch a docker web container
    docker_container:
      name: dvwa
      image: cyberxsecurity/dvwa
      state: started
      published_ports: 80:80

  - name: Enable docker service
    systemd:
      name: docker
      enabled: yes
      
ELK Install
      hosts: elk
become: true
tasks:


name: docker.io
apt:
update_cache: yes
name: docker.io
state: present


sysctl:
name: vm.max_map_count
value: '262144'
state: present


name: Install python3-pip3
apt:
force_apt_get: yes
name: python3-pip
state: present


name: Install Docker python module
pip:
name: docker
state: present


name: download and launch a docker web container
docker_container:
name: elk
image: sebp/elk:761
state: present
restart_policy: always
published_ports:
- 5601:5601
- 9200:9200
- 5044:5044


name: Enable docker service
systemd:
name: docker
enabled: yes

Install Filebeat
name: installing and launching filebeat
hosts: webservers
become: yes
tasks:


name: download filebeat deb
command: curl -L -O "https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb"


name: install filebeat deb
command: dpkg -i filebeat-7.4.0-amd64.deb


name: drop in filebeat.yml
copy:
src: /etc/ansible/files/filebeat-config.yml
dest: /etc/filebeat/filebeat.yml


name: enable and configure system module
command: filebeat modules enable system


name: setup filebeat
command: filebeat setup


name: start filebeat service
command: sudo service filebeat start

MetricBEAT
name: installing and launching Metricbeat
hosts: webservers
become: yes
tasks:


name: download Metricbeat deb
command: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.6.1-amd64.deb


name: install metricbeat deb
command: dpkg -i metricbeat-7.6.1-amd64.deb


name: drop in Metricbeat.yml
copy:
src: /etc/ansible/files/metricbeat-config.yml
dest: /etc/metricbeat/metricbeat.yml


name: enable and configure system module
command: metricbeat modules enable docker


name: setup metricbeat
command: sudo metricbeat setup


name: start metricbeat service
command: sudo service metricbeat start

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly redundant_____, in addition to restricting access_____ to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_
- Load balancers help tp protect against dDos attacksmake sure assets are deployed across multiple machines. Lets the ansible configure deployments across multiple VM's at once using one Playbook.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _dvwa____ and system resources_____.
Filebeat recieves system logs from all VM's hosting web assets.
Metricbeat monitors system resources across all VM's hosting web assets

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.
| NAME      | FUNCTION  | IP adress | Operating Sys      |   |
|-----------|-----------|-----------|--------------------|---|
| Jump box  | Gateway   | 10.0.0.1  | Linux              |   |
| web 1     | DVWA Host | 10.0.0.5  | Linux Ubuntu 18.04 |   |
| web 2     | DVWa host | 10.0.0.6  | Linux Ubuntu 18.04 |   |
| elk stack | dvwa host | 10.1.0.5  | Linux Ubuntu 18.04 |   |
|           |           |           |                    |   |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the _jumpbox privisioner____ machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _TODO: 99.171.189.32

Machines within the network can only be accessed by _jump box provisioner____.
- _TODO: Jumpbox. 10.0.0.4

A summary of the access policies in place can be found in the table below.

| NAME      | yes | allowed IPs  |   |
|-----------|-----|--------------|---|
| Jump box  | no  | 23.99.70.28  |   |
| web 1     | no  | 10.0.0.5     |   |
| web 2     | no  | 10.0.0.6     |   |
| elk stack | yes | 40.76.92.212 |   |
|           |     |              |   |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: because automation saves time and to make sure that no steps are missed in the configuration process.

The playbook implements the following tasks:
- _TODO: 
- Install Python3
- Install Docker Python3 container
- Instal Elk server container
- Launch new Docker container
- set max memory 

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance. < https://ibb.co/bWDbYJV >

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: 10.0.0.5 and 10.0.0.6

We have installed the following Beats on these machines:
filebeat andmetricbeat

These Beats allow us to collect the following information from each machine:
Filebeat collects event logs to monitor activity 
Metricbeat collects all statuses of systems  and virtual hardware used to monitor CPU, RAM Memory and Network activity.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the __playbook___ file to ip adresses_ ____.
- Update the _hosts____ file to include private IPS
- Run the playbook, and navigate to _kibana___ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?
- Playbooks are designated by a file extension of .yml -- copy them into your ansible container.
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?
- You update the hosts file to designate which machines the playbook will install software. In the hosts file you have to designate server section and a seperate server section with the appropriate IP's. 

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
http://40.76.92.212:5601/apps/kibana
