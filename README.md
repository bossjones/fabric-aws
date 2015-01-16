[![Build Status](https://travis-ci.org/EverythingMe/fabric-aws.svg?branch=master)](https://travis-ci.org/EverythingMe/fabric-aws)

# Fabric AWS integration

Ever used pychef's fabric integration and loved it?  
This project aims to integrate AWS with fabric. You can decorate your fabric tasks to run on an Autoscaling group (which can also be a part of a Cloudformation stack)

## Installation
* `pip install fabric-aws`
* [Configure boto](http://boto.readthedocs.org/en/latest/boto_config_tut.html)

## Example fabfile

```python
from fabric.api import *
from fabric_aws import *


@autoscaling_group('us-east-1', 'my-autoscaling-group')
@task
def uptime_asg():
    run('uptime')

@cloudformation_autoscaling_group('us-east-1', 'my-cloudformation',
                                  'AutoScalingGroup')
@task
def uptime_cfn():
    run('uptime')

@ec2('us-east-1', instance_ids=['i-01010101', 'i-10101010'])
@task
def uptime_ec2_instance_ids():
    run('uptime')

@ec2('us-east-1', filters={'tag:Name': 'jenkins'})
@task
def uptime_jenkins():
    run('uptime')

@ec2('us-east-1', filters={'tag:Name': 'my-vpc-instance'},
     hostname_attribute='private_ip_address')
@task
def uptime_vpc():
    run('uptime')
```
