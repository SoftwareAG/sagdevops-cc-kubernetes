# Setup Command Central on Kubernetes cluster

> WORK IN PROGRESS !!!

Use this document to setup Command Central on one of the following Kubernetes environments:

1. Local Kubernetes cluster provided by Docker for Mac or Windows
2. Managed EKS cluster on AWS

## Running Command Central on local Kubernetes

You need Docker for Mac or Docker for Windows installed on your box.
Enable Kubernetes using Docker Preferences.

Verify Kubernetes is running:

```bash
$ kubectrl get nodes
NAME                 STATUS    ROLES     AGE       VERSION
docker-for-desktop   Ready     master    2d        v1.10.3
```

### Subscribe to Command Central images on Docker Store

* Login to [Docker Store](https://store.docker.com) with your Docker ID
* Open [Command Central product](https://store.docker.com/images/softwareag-commandcentral)
* `Checkout`, accept the license agreement to get access to Command Central images

Create Docker Store credentials to allow Kubernetes to pull
images from Docker Store:

```bash
$ kubectl create secret docker-registry regcred \
    --docker-server=https://index.docker.io/v1/ \
    --docker-username=<your-docker-login> \
    --docker-password=<your-docker-password> \
    --docker-email=<your-docker-email>
secret "regcred" created
```

### Launch Command Central service

Create Command Central deployment and service:

```bash
kubectl create -f sag-cc.yaml
```

Verify deployments:

```bash
$ kubectl get deployments
NAME                  DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
cc-deployment         1         1         1            1           7h
```

Verify services:

```bash
$ kubectl get svc
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
cc           ClusterIP   10.110.71.120   <none>        8091/TCP   7h
kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP    2d
```

Verify pods:

```bash
$ kubectl get pods
NAME                                   READY     STATUS    RESTARTS   AGE
cc-deployment-56f6d448c6-gswcj         1/1       Running   0          7h
```

Forward the traffic to the Command Central pod:

> IMPORTANT: the name of the pod will be different. Use the value from the previous command output!

```bash
$ kubectl port-forward cc-deployment-56f6d448c6-gswcj 8091:8091
Forwarding from 127.0.0.1:8091 -> 8091
```

Open [Command Central Web UI](https://127.0.0.1:8091) and
login as Administrator/manage

You have Command Central running on Kubernetes!

## Running Command Central on AWS EKS

TODO
