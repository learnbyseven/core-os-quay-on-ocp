# core-os-quay
Understand enterprise container registry quay in 21 Minutes 

Single server Arch deployment
=============================

Perquisites
-----------

1. Single VM 4 vCPU 4 GB RAM 

2. OS RHEL7

3. APPLICATION IMAGES 
   - quay.io/coreos/quay:v2.9.5
   - registry.access.redhat.com/rhscl/postgresql-96-rhel7
   - registry.access.redhat.com/rhscl/redis-32-rhel7


Application Architecture
------------------------

- Quay
- Redis
- Postgress (preferred) for Clair / MariaDB 

Deployment Steps 
----------------

1. Install Docker
2. Pull Quay image from docker
- docker login -u="coreos+rhcp" -p="L6ZXXVHD9XLQ7PR7HBNRW2FAIZQNJYHREISFGCUBIB45C43WCWYU3DZ0FHJH2AY5" quay.io
- docker pull quay.io/coreos/quay:v2.9.2
3. Create containers in sequence  
   - DB 
   - Redis 
   - Quay 
   


Hight Availability Quay Deployment 
==================================

ADDONS 
------

Clair 
-----

Container Image
quay.io/coreos/clair-jwt:v2.0.0

oc create secret generic coreos-pull-secret --from-file=.dockerconfigjson=/root/.docker/config.json --type='kubernetes.io/dockerconfigjson' -n quay-enterprise

PG : 9.4 

# core-os-quay-on-ocp

STEPs:
Download script 
## Quay namespace creation
oc create -f quay-enterprise-namespace.yml
## config secret 
oc create -f quay-enterprise-config-secret.yml
## Pull secret
oc create secret generic coreos-pull-secret \
     --from-file=".dockerconfigjson=$HOME/.docker/config.json" \
     --type='kubernetes.io/dockerconfigjson' -n quay-enterprise
     
## Role & Role Bindings 
oc create -f quay-servicetoken-role-k8s1-6.yaml && oc create -f quay-servicetoken-role-binding-k8s1-6.yaml

## Add Privilege 
oc adm policy add-scc-to-user anyuid system:serviceaccount:quay-enterprise:default

## Redis deployment 
oc create -f quay-enterprise-redis.yml

## Quay service LB or NP 
## LB 
oc create -f quay-enterprise-app-rc.yml -f quay-enterprise-service-loadbalancer.yml
## NP
oc create -f quay-enterprise-app-rc.yml -f quay-enterprise-service-nodeport.yml

## DB deployment prefer 
./deploy-db-postgres-local
./deploy-db-postgres-cloud-storage-aws 

## Verification 
oc get pods -n quay-enterprise
sudo docker ps | grep postgres
oc get services -n quay-enterprise



/AKIA2DJSYPDNB5FYHCR3/LRp4ML41e9ph3Hm/Kv3cM8sLbIA99n/BWDZttsOX

AKIA2DJSYPDNAZNZV52U///0WAEhz+APy6mXRO+WHRFi3pBSCkW4Ryn7tslRItT

Security

f266f756e0a2435ba18c4248795b670f09e6d8ba7563d21d1b053f4d7614d3da


-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAusw64f0CRMKw6GvloDNhbhQbVWu3Nl+Ne3rls5GCEj6y/crb
wq6c3GlxQmumapqIlfeiihYo36th1z1rBV+/v5hZYolNXMVuwBpivhWMr+8Yuxhh
WGEDE+p5VIarcnFbNntHtgNiGJ3Ch/0y/lS2uGb+2C2qFHYXMDWDFJpKfgACSlzk
tNQnzxIqj6CCLrL8vMkG5hjAHAwahXEOSnqhRoB95mR4+fV44UCD7NAAqIaIh2g3
imslQIscU3PwOk5RvCqSDv4SYryN3RKns7rSa2XdOOS58lX2HEztsAAS+dijHRtE
Iibviyd3FxumLHkfbcC5XjEXfRJVbLN6xceNowIDAQABAoIBABbgEcnrq6PIr9TB
Ty0vfZv3DGMz6LYuPMDwC5cnl4/XwSBWqlOMrJr1XVsD8deWNb8fxKDia9Olmjq7
Q72JvI1LATnm6eQVgtA1qv1E3aYMmAZkIEUBbwNjsG4LJo3CQf2fBT4jQJfBIg8j
O7kXngoLsDkVsSwGS3qqOEjanYq7t09tTTzsuTQdoyi9jLgIbVrJ5sxE/aQGGw7i
OSCypEUBgmU/RlBSdTCaHv3Kle78FjU+klDjafX2YjBm2QW2ZromabzGpLoCMiWB
eZx/RmfpGGP0Yiv+IDGpN9V36YSP5q43RLjZbDA9oJ3Yi+gcyb4XOakNkGa/j0gJ
n0F6VtkCgYEA1leuedMp46gBIFCToXaQCDnhCVp7/kleuIPLm9AaIAtEt6SjPWo5
0QZqB1SOgAUAGAHkhbz6X3IFpOBeig6qFXu6QrexprMPT8JlAf+cTdt4aA/F7tOn
8czAfjStU/Cxrqc+2MNa6EBu50lK5ROCCbOyZ+7XeoApd/E9zoq6jM0CgYEA3xoZ
Ep0LOGtdUeBa/bz69uAiiSqmcjziINm5Ar44U09beoH29ls8nLs06q/fpjTnRlIt
RsrdjGvkuARrNihB7GI2v7XfojMmlkSq7qeIrSQIn/c34dnTELnJWtfjQPNPZ2/l
1MnPHFgtT81aYRto5n1+ckJo42srovvyTxCmhC8CgYEAyHInEHaRbfznULkJ1q1x
9L9r27tqyVsD4boe5w+t3tmq4bJtljmI6Bj/futsd/w1Ij5i307jNe8DqDTLNICS
PpT+kvYGhMZfQ0+f9kZ8fbMI3wghKj91h4LbYSsSDLXC9HojI8NNeHUJQfIgwCmG
KlyKMvgBOuYv4aMREd5apuECgYEAjUj6vdXkSCt94p8BIJUwHW2dkV4IIGo8De+z
gXAzPVcRKIjre+IVhW/st/7+1EPGfrsF30ITgZzGMF7kAl0GOouL/mZQJGjeM+Vy
lkZUgMlECQHHSujmCD6PrE7xpK0xCOFNHC9dUKbqsxHp/XsdOHIaxIMX54V0EfgZ
4EY8HZMCgYBqm0PmoHvaZizt6sHv5UcTE2MQSRvSsCXADdcyvCsSycQtq5zEDW21
Iz+9C5cYH8tvmXqiQKIy590cqRu5fB0oZ+/AszSBoIjw8seOG1pHVdVZiJFafGxC
uRAKltXoUU1E/hK0pHc04SZriDtsKXxVzI7ZMvF7WAbl24zBOT0Haw==
-----END RSA PRIVATE KEY-----
