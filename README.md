# core-os-quay-on-ocp
Understand enterprise container registry quay in 21 Minutes 


Arch

Deploy 


### Pull image
docker login -u="coreos+rhcp" -p="L6ZXXVHD9XLQ7PR7HBNRW2FAIZQNJYHREISFGCUBIB45C43WCWYU3DZ0FHJH2AY5/AKIA2DJSYPDNB5FYHCR3/LRp4ML41e9ph3Hm/Kv3cM8sLbIA99n/BWDZttsOX" quay.io
AKIA2DJSYPDNAZNZV52U///0WAEhz+APy6mXRO+WHRFi3pBSCkW4Ryn7tslRItT
docker pull quay.io/coreos/quay:v2.9.2
oc create secret generic coreos-pull-secret --from-file=.dockerconfigjson=/root/.docker/config.json --type='kubernetes.io/dockerconfigjson' -n quay-enterprise

PG : 9.4 



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

