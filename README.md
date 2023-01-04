# README

## Goal

Deploy a python application (IFotos) to EC2 instances using Ansible

## How does it work ?

- Ansible playbook creates a set of EC2 instances with its associated SG and ELB
- Python application (Flask) files are copied to EC2 instances
- The container is built and started using docker-compose
- A Nginx process is started to redirect incoming traffic from port 80 to the docker container
- - using port 3000 directly on the ELB - so far
- Instances are then attached to the ELB

## Prerequisite

- Working ansible installation
- AWS credentials (access & secret key, ec2 ssh key)

## Setup


### Review settings

- Infrastructure settings can be edited in the `vars.yml` file
  - AWS region, instance type, ...

## Run

- There are two playbook available :
  - `flask.yml` will provision infrastructure and deploy a containerized Python Flask application
  - `clean.yml` will cleanup all infrastructure
- To run theses playbooks, you can use make

```
make deploy
make clean
```

## What could be done better

- Python application container should be built outside the target instances and pushed to a registry
- - which is what I did: https://github.com/ifriedman7/IFotos.git
- Flask built-in application server is not production grade, should be replaced it with a compliant WSGI server
- - also did
