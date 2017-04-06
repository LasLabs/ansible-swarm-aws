[![License: Apache 2.0](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)

[![Build Status](https://travis-ci.org/LasLabs/ansible-aws-swarm.svg?branch=master)](https://travis-ci.org/LasLabs/ansible-aws-swarm)

Ansible Swarm AWS
=================

This role will allow you launch and provision a set of nodes in AWS EC2 as
Docker Swarm Node(s). It does pretty much all the heavy lifting including creating
the SSH keys for you and launching the Swarm Cluster.

Usage
=====

Role Variables
--------------
| Name | Description |
| :--: | :---------: |
| aws_ssh_key | The name of the new SSH key we create for the swarm nodes |
| aws_security_group_name | The name of the Security Group we create for the swarm nodes |
| aws_region | The AWS region to spool the new nodes in |
| aws_ami | The AMI to use as the base for the nodes |
| aws_vpc_subnet_id | The Subnet in which to launch the instance. This will dictate which VPC you're launching within |
| aws_instance_tags_manager | Tags for the manager instance being launched |
| aws_instance_tags_worker | Tags for the instance worker instances being launched |
| aws_instance_tags | Tags for all the instances that are launched |
| aws_instance_type | The instance size/type to use for the nodes |
| swarm_node_count | The number of worker nodes to launch |
| swarm_iface | The ethernet interface to use for the Swarm. eth0 is default |

First, create a playbook like so to launch the instances:

Example Playbook
----------------

```
    - hosts: localhost
      connection: local
      vars:
        aws_ssh_key: ec2_docker_swarm_key
        aws_security_group_name: ec2_docker_swarm_security_group
        aws_region: us-east-1
        aws_ami: ami-e2b033f4
        aws_vpc_subnet_id: subnet-5a660898
        aws_instance_type: t2.small
        aws_instance_tags:
          Name: Docker Node
        aws_instance_tags_manager:
          Docker: Swarm Manager
        aws_instance_tags_worker:
          Docker: Swarm Worker
        swarm_node_count: 2
      roles:
         - ansible-swarm-aws
```

Then, launch docker swarm on them with this command:
```
ansible-playbook -i inventory ansible-swarm-aws/tasks/docker-swarm.yml
```

Credits
=======

Inspiration
-----------
* https://github.com/skippbox/ansible-swarm
* https://github.com/nextrevision/ansible-swarm-playbook

Contributors
------------

* Ted Salmon <tsalmon@laslabs.com>

Maintainer
----------

[![LasLabs Inc.](https://laslabs.com/logo.png)](https://laslabs.com)

This module is maintained by [LasLabs Inc.](https://laslabs.com)

* https://github.com/laslabs/ansible-aws-swarm
