# Deploying KIXEYE Test Application using Ansible and Docker-compose


This is the Ansible playbook run on CentOS7 server to deploy KIXEYE Test Application

The requirement of application is: https://github.com/Kixeye/testapp/blob/master/README.md

## Requirement to run this Ansible:
- Remote server is CentOS 7
- Configure remote server in hosts file of ansible configuration
- Change variable *host* to the hostname of your remote server in testapp.yml file

```
---
 - hosts: vagrant-centos7

```

## How to use

1. Clone this repo to your machine
2. Running testapp.yml playbook
3. Check the app if it is up and running correctly
4. Checking the log 



