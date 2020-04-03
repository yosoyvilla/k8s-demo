K8's Demo
=========

A simple distributed application running across multiple Docker containers. (Working with K8's locally using minikube)

Getting started
---------------

This demo is based on [dockersamples/example-voting-app](https://github.com/dockersamples/example-voting-app)

## Lets Start
-------------------------

First create the vote namespace

```
$ kubectl create namespace vote
```

Run the following command to create the deployments objects:
```
$ kubectl create -f deployments/
deployment "db" created
deployment "redis" created
deployment "result" created
deployment "vote" created
deployment "worker" created
```

Run the following command to create the services objects:
```
$ kubectl create -f services/
service "db" created
service "redis" created
service "result" created
service "vote" created
```

Run the following command to enable the Load Balancer (Ingress-Nginx):
```
$ minikube addons enable ingress
```

Run the following command to create the Load Balancer (Ingress-Nginx):
```
$ kubectl apply -f demo-ingress.yaml
ingress.networking.k8s.io/demo-ingress created
```

Run the following command to verify the ingress address set:
```
$ kubectl --namespace=vote get ingress
NAME              HOSTS                  ADDRESS          PORTS     AGE
demo-ingress      demo-kubernetes.info   <your-address>   80        38s
```

Add the following line to the bottom of the /etc/hosts file.
```
<your-address> demo-kubernetes.info
```

If you want to delete all your infrastructure (all the previously created) you should run:
```
$ kubectl delete namespace vote
```

You can visit demo-kubernetes.info from your browser now.

To use this on a cloud you should add an [Ingress Controller](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/).

Architecture
-----

![Architecture diagram]()

Note
----

The voting application only accepts one vote per client. It does not register votes if a vote has already been submitted from a client.