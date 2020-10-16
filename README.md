[![Build Status](https://travis-ci.com/emanuele232/Vagrant-DockerSwarm-setup.svg?branch=main)](https://travis-ci.com/emanuele232/Vagrant-DockerSwarm-setup)

# Vagrant-DockerSwarm-setup
A Vagrant configurations that deploys a Docker swarm using Ansible

## Tasklist

- Deploy two CentOs
- Configure the VMs with 50 GB of space
- Deploy Docker on the VMs
  - Expose REST API 
  - configure the API to use tls
- Configure a Docker Swarm, one vm will be the master the other will be the Worker

The VMs are configured using Ansible
The code on github uses travis + ansible-lint for CI
It is nice to test using Molecule

## Resources used

- Vagrant configuration: [six ansible pratices](https://max.engineer/six-ansible-practices)
- Docker installation: [ansible-role-docker](https://github.com/geerlingguy/ansible-role-docker)
- Protect the Docker sockert: [docker documentation](https://docs.docker.com/engine/security/https/) or the better-formatted [secure docker API](https://blog.eduonix.com/software-development/learn-secure-docker-api-using-ssltls/)
- Configure the Docker swarm: [Docker swarm doc.](https://docs.docker.com/engine/swarm/) 

# Using this Tool
*quick how-to*
## requirements
- ansible
- vagrant 
  - vagrant plugins *vagrant-disksize* (vagrant hostupdater is reccomended)

## ssh config
I used a config file for ssh to ease the connection to the vms

    #For vagrant virtual machines
    Host 192.168.33.* *.myapp.dev
    StrictHostKeyChecking no
    UserKnownHostsFile=/dev/null
    User root
    LogLevel ERROR

This config file will ensure the use of root when connecting throught ssh and ignore ssh host check


## Usage
The usage consist in creating the virtual machines with

    vagrant up

and running the ansible playbook with

    ansible-playbook -i hosts  playbook.yml 

## Connecting to the swarm from your local machine
To connect to the swarm using the protected REST API you have to install *docker-ce* and fire the command:

    docker --tlsverify --tlscacert=ca.crt --tlscert=/client.crt --tlskey=/client-key.pem -H={{ HOST }}:2376 node ls

Where the variable HOST is one of the VMs.

*the command node ls will return the nodes connected to the swarm*



