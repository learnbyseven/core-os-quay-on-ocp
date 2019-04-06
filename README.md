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
======

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


