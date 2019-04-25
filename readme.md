# AWS_DOCKER_SWARM_OVERLAYNETWORK_SNIPEIT_MYSQL

## Description
This repo will deploy and set up Snipe-it Inventory App running in a docker container which is connected to a mysql database again running on a docker container on different hosts. This was achieved using docker swarm and overlay network. This repo consists of five playbooks of which four needs to be executed in the order stated in the instruction section to have successful deployment. Here is small breif description of what each playbook does:

### db_machine.yml
- Creates security group for the database instance
- Creates EC2 instance for database
- Adds the created instance to the host group
- Saves database instance IP address as a variable
- Adds tag to the instance

### db_provisioning.yml
- Installs python and docker on the db instance
- Pulls MYSQL docker image from the docker hub
- Copies app environmental variables to remote machine
- Initialises docker swarm
- Generates and saves worker token as variable
- Creates a overlay network
- Creates a MYSQL docker container and attaches to the overlay network

### app_machine.yml
- Creates security group for the app instance
- Creates EC2 instance for the app
- Adds the created instance to the host group
- Saves app instance IP address as a variable
- Adds tag to the instance
- Creates a elastic load balancer
- Attaches the app to the load balancer

### app_provisioning.yml
- Installs python and docker on the app instance
- Pulls Snipe-it docker image from the docker hub
- Copies app environmental variables to remote machine
- Joins docker swarm as worker node
- Creates a Snipe-it docker container and attaches to the overlay network

### ec2_terminate.yml
- Gather facts from other hosts
- Terminates ec2 instances by tags
- Removes lines generated and added during deployment

## Prerequisites
Following packages needs to be installed on the localhost machine
- Ansible
- Python =>2.7

## Personalisation
Following files can be edited to make the deployment to your preferences
- ansible.cfg
- app.env
- aws_vars.yml
- aws_keys.yml

## Instruction
Run the following commands in the order stated for a successful deployment

``ansible-playbook -i hosts db_machine.yml``

``ansible-playbook -i hosts db_provisioning.yml -K``

``ansible-playbook -i hosts app_machine.yml``

``ansible-playbook -i hosts app_provisioning.yml -K``

## Termination

Run the following command to terminate the aws instances and revert the repo back to its original form

``ansible-playbook -i hosts ec2_terminate.yml``
