# Readme
project 
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](Images/diagram_filename.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.

  ---
- name: Configure Elk VM with Docker
  hosts: elk
  remote_user: azadmin
  become: true
  tasks:
    # Use apt module
    - name: Install docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present
      # Use apt module
    - name: Install pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present
      # Use pip module
    - name: Install Docker python module
      pip:
        name: docker
        state: present
      # Use sysctl module
    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes
      # Use docker_container module
    - name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        published_ports:
          - 5601:5601
          - 9200:9200
          - 5044:5044
      # Use systemd module
    - name: Enable service docker on boot
      systemd:
        name: docker
        enabled: yes

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly PROTECTED, in addition to restricting TRAFFIC to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_ The load balancer model protects servers from getting too much traffic and breaking down, if one breaks the load balancer redirects to remaining online servers.
Jumpbox prevents Azure vms being exposed to the public, need special log in information to obtain useable

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the LOGS and system TRAFFIC.
- _TODO: What does Filebeat watch for?_
Filebeat watches and monitors the log files and locations that users specify, collects logs and sends them to Elasticsearch or Logstash for indexing.
- _TODO: What does Metricbeat record?_
records all the time collecting metrics from the operating system and from services running on the server and takes metric and statistics that it collects and sends them to an output user specified

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web-1    | Server   | 10.0.0.5   | Linux            |
| web-2    | Server   | 10.0.0.7   | Linux            |
| web-3    | Server   | 10.0.0.6  | Linux            |
| elk      | Server   | 10.2.0.5   | Linux            |
### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JUMP BOX PROVISIONER machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _TODO: Add whitelisted IP addresses_
5601 KIBANA PORT


Machines within the network can only be accessed by JUMPBOX PROVISIONER .
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_
JUMPBOX PROVISONER
10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes              | my ip address           |
| web-1    | no               |   10.0.0.4              |
| web-2    | no               |   10.0.0.4              |
| web-3    | no               |   10.0.0.4              |
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it was sealed from vulnerabilities 
- _TODO: What is the main advantage of automating configuration with Ansible?_
This free open source tool takes no special coding needed but spaces are very important when making an ansible playbook. Users are able to model even highly complex IT worksflows, the enitre application can be deployed from any environment. no extra software needed


The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- .install docker.io
- .install pip3
- .install Docker python module
- .increase virtual memory 
- .download and launch a docker
- ...

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_
10.0.0.5 web-1
10.0.0.7 web-2 

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_
-install metricbeat
-install filebeat
These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._
filebeat monitors the log files or locations that are specified, it collects log events and fowwards them to either Elasticsearch or logstash 
metricbeat takes the metric and stats that it collects and ships them to the specified output
in the winlogbeat you see the logging information, users and count of log ins, event levels and pretty pretty colors



### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the yaml file to /etc/ansible
- Update the /etc/ansible/host file to include... PRIVATE IDS AND ansible_python_interpreter=/usr/bin/python3
- Run the playbook, and navigate to curl localhost/setup.php to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
/etc/ansible/file/filebeat-configuration.yml

- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
edit /etc/ansible/host file to add websever/elk ip address



- _Which URL do you navigate to in order to check that the ELK server is running?
http://104.45.211.56:5601/app/kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

ansible-playbook filebeat.yml
