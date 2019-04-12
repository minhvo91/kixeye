# Deploying KIXEYE Test Application using Ansible and Docker-compose


This is the Ansible playbook and Docker-compose file will be used for:
- Deploying KIXEYE Test Application to remote server automatically
- Dockerizing KIXEYE Test Application on remote server

The requirement of application is: https://github.com/Kixeye/testapp/blob/master/README.md

## Requirement to run this Ansible playbook:
- Remote server is CentOS 7
- SSH key of local machine was added to Remote server
- On local machine:
  - Configure remote server in hosts file of ansible configuration
  - Change variable *hosts* to the hostname of your remote server in testapp.yml file

```
---
 - hosts: vagrant-centos7

```

## How to use

1. **Clone this repo to your machine**
```
[minhvo@localhost ~]$ git clone https://github.com/minhvo91/kixeye.git
```
2. **Running testapp.yml playbook on your machine**
```
[minhvo@localhost ~]$ cd kixeye
[minhvo@localhost ~/kixeye]$ ansible-playbook testapp.yml                                                     ✔  4196  23:00:10 

PLAY [vagrant-centos7] **************************************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************************************
ok: [vagrant-centos7]

TASK [Install EPEL Repository] ******************************************************************************************************************
ok: [vagrant-centos7]

TASK [Install pip] ******************************************************************************************************************************
ok: [vagrant-centos7]

TASK [Installing Docker Prerequisite packages] **************************************************************************************************
[DEPRECATION WARNING]: Invoking "yum" only once while using a loop via squash_actions is deprecated. Instead of using a loop to supply multiple 
items and specifying `name: "{{ item }}"`, please use `name: ['yum-utils', 'device-mapper-persistent-data', 'lvm2', 'python-pip']` and remove 
the loop. This feature will be removed in version 2.11. Deprecation warnings can be disabled by setting deprecation_warnings=False in 
ansible.cfg.
ok: [vagrant-centos7] => (item=[u'yum-utils', u'device-mapper-persistent-data', u'lvm2', u'python-pip'])

TASK [Configuring docker-ce repo] ***************************************************************************************************************
ok: [vagrant-centos7]

TASK [Installing Docker latest version] *********************************************************************************************************
[DEPRECATION WARNING]: Invoking "yum" only once while using a loop via squash_actions is deprecated. Instead of using a loop to supply multiple 
items and specifying `name: "{{ item }}"`, please use `name: ['docker-ce', 'docker-ce-cli', 'containerd.io']` and remove the loop. This feature 
will be removed in version 2.11. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
ok: [vagrant-centos7] => (item=[u'docker-ce', u'docker-ce-cli', u'containerd.io'])

TASK [Starting and Enabling Docker service] *****************************************************************************************************
ok: [vagrant-centos7]

TASK [Install docker-compose] *******************************************************************************************************************
ok: [vagrant-centos7]

TASK [Copy docker project to remote host] *******************************************************************************************************
ok: [vagrant-centos7]

TASK [Creating testapp by Docker] ***************************************************************************************************************
changed: [vagrant-centos7]

TASK [Logging TestApp Docker Container] *********************************************************************************************************
changed: [vagrant-centos7]

PLAY RECAP **************************************************************************************************************************************
vagrant-centos7            : ok=11   changed=2    unreachable=0    failed=0   


```

3. **Check the app if it is up and running correctly (On remote server)**
```
[vagrant@vagrant-centos7 ~]$ curl http://localhost:8080/leaderboard
[{"userId":3,"score":-1236509442},{"userId":4,"score":-1164950331}]
```

OR You can check by URL: http://<remote-server-ip>:8080/leaderboard

4. **Checking the log (On remote server)**
```
[vagrant@vagrant-centos7 ~]$ tail -f /var/log/testapp.log 
redis.local_1  | 2019-04-12T16:06:53.288142818Z 1:M 12 Apr 2019 16:06:53.287 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
redis.local_1  | 2019-04-12T16:06:53.288146392Z 1:M 12 Apr 2019 16:06:53.287 * Ready to accept connections
app_1          | 2019-04-12T16:06:58.652830384Z 16:06:58 [leaderboard-akka.actor.default-dispatcher-2] INFO  akka.event.slf4j.Slf4jLogger - Slf4jLogger started
app_1          | 2019-04-12T16:06:59.124679408Z 16:06:59 [main] INFO  kixeye.testapp.Redis$ - generated scores: List((5.155487E7,1), (-2.92478344E8,2), (-1.236509442E9,3), (-1.164950331E9,4))
app_1          | 2019-04-12T16:06:59.569606592Z 16:06:59 [leaderboard-akka.actor.default-dispatcher-6] INFO  kixeye.testapp.Main$ - Loaded data into Redis
app_1          | 2019-04-12T16:07:00.018328966Z 16:07:00 [Main-akka.actor.default-dispatcher-5] INFO  akka.event.slf4j.Slf4jLogger - Slf4jLogger started
app_1          | 2019-04-12T16:08:35.198729977Z 16:08:35 [Main-akka.actor.default-dispatcher-13] TRACE kixeye.testapp.routes.Leaderboard - HTTP request for leaderboard
app_1          | 2019-04-12T16:08:35.247560722Z 16:08:35 [leaderboard-akka.actor.default-dispatcher-10] DEBUG kixeye.testapp.RequestHandler - getting leaderboard from redis
app_1          | 2019-04-12T16:08:35.331944487Z 16:08:35 [scala-execution-context-global-39] DEBUG kixeye.testapp.RequestHandler - got leaderboard: List(LeaderboardEntry(3,-1236509442), LeaderboardEntry(4,-1164950331))
app_1          | 2019-04-12T16:08:35.331969953Z 16:08:35 [Main-akka.actor.default-dispatcher-13] DEBUG kixeye.testapp.routes.Leaderboard - Got status response List(LeaderboardEntry(3,-1236509442), LeaderboardEntry(4,-1164950331)) for HTTP leaderboard request

```



