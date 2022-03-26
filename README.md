## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Elk Network Diagram](/Images/Cloud_Network_diag_Redteam_Jeremy_Bird.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yml file may be used to install only certain pieces of it, such as Filebeat.

Config Files >

-[ansible.cfg ](/Ansible/Ansible.cfg)

-[install_elk.yml ](/Ansible/install-elk.yml)

-[filebeat-config.yml ](/Ansible/files/filebeat-config.yml)

-[metricbeat-config.yml ](/Ansible/files/metricbeat-config.yml)

Playbook Files >

-[filebeat-config.yml ](/Ansible/role/filebeat-playbook.yml)

-[metric-beat.yml ](/Ansible/files/metricbeat-playbook.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting in bound traffic to the network.

- What aspect of security do load balancers protect? 
Load balancer can seemless distribute traffic to a website and spread the load over multiple severs ensuring high availability. The load balancer sits offers security by sitting in between the client and the server and you can apply various controls and rules on the load balancer. This adds an addtional layer of security.

- What is the advantage of a jump box?
A jump box is a secure computer that has resticted access to connect to it. This is the computer admins would typically log into before making any administration to any of the other machines in the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
- What does Filebeat watch for?
	Filebeat watchs for system logs and collects log events.

- What does Metricbeat record?
Metricbeat records meticts and statistics from the systems and services running on a machine.

The configuration details of each machine may be found below.
#_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name          | Function          | IP Address                               | Operating System |
|---------------|-------------------|------------------------------------------|------------------|
| Jump Box      | Gateway           | Public: 138.91.156.193 Private:10.0.0.4  | Linux            |
| Web-1         | Web Server        | Private: 10.0.0.5                        | Linux            |
| Web-2         | Web Server        | Private: 10.0.0.8                        | Linux            |
| Elk-Stack     | Server Monitoring | Public: 22.228.212.129 Private: 10.1.0.4 | Linux            |
| Load Balancer | Load Balancing    | Public: 40.80.152.74                     | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:<br>
- Home IP 69.47.228.112

Machines within the network can only be accessed by Jump Box.<br>
- Home IP 69.47.228.112

A summary of the access policies in place can be found in the table below.

|      Name     | Publicly Accessible |       Allowed IP Addresses      |
|:-------------:|:-------------------:|:-------------------------------:|
| Jump Box      | Yes                 | 69.47.228.112 using SSH         |
| Web-1         | No                  | 10.0.0.4 using SSH              |
| Web-2         | No                  | 10.0.0.4 using SSH              |
| ELK-Stack     | No                  | 69.47.228.112 using TCP 5601    |
| Load Balancer | No                  | 69.47.22.112 using HTTP port 80 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allows admins a way to automate configurations.  This ensures that when Ansible is run on addtinoal machines they are configure in the same way. You can also use Ansible to update installed packages agian ensuring the configurations are uniform and repeatable.

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- Installed docker.io
- Installed python3 
- Increased the virtual memory
- Picked which docker contianer to use and specified what ports that the ELK runs on <br>
		Published_ports:<br>
			  -  5601:5601<br>
			  -  9200:9200<br>
			  -  5044:5044<br>
		  
- Ensured Docker service wsa enabled on reboots of the instance.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Elk docker PS](/Images/docker_ps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:<br>
- 10.0.0.5
- 10.0.0.8

We have installed the following Beats on these machines:<br>
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat - collected the log infromation from the web servers Web-1 and Web-2 <br>
- Metricbeat - collect infromation of ssystem and services running on the machine. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the Playbook files to the Ansible control node. Copied the files into /etc/asnible/roles<br>
-[filebeat-config.yml ](/Ansible/role/filebeat-playbook.yml)
-[metric-beat.yml ](/Ansible/files/metricbeat-playbook.yml)

- Update the hosts file to include to include the machines in scope:<BR>

[webservers]<br>
10.0.0.5 ansible_python_interpreter=/usr/bin/python3<BR>
10.0.0.8 ansible_python_interpreter=/usr/bin/python3<BR>

Also update the two config files to include any settings: <br> 
-[filebeat-config.yml ](/Ansible/files/filebeat-config.yml)<br>

-[metricbeat-config.yml ](/Ansible/files/metricbeat-config.yml)<br>

If you are needing to run on specific servers be sure to specify in teh config what section of the host file you want run it on. ex webservers or elk

- Run the playbook, and navigate to Kibana to check that the installation worked as expected. You can do this by navigating to the http://20.228.212.129:5601/app/kibana#/home

From there you can see if the beats are running by going to the following: <br>
-Filebeat Kibana Home > Add log data button > System logs> system logs dashboard. <br>
-Metricbeat Kibana Home > Add metic data button > docker metrics > Docker metics dashboard.  <br>

With both the config and playbook files ready to go you can execute the commands by running > <BR>
	-Filebeat > ansible-playbook fbeat-playbook.yml
	-Metricbeat > ansible-playbook metricbeat-playbook.yml

If updates are neede changes to the config file will need to be run. 


