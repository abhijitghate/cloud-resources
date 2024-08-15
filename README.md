# cloud-resources


### Docker

`docker run <name of the image>`

This commands tuns a container form the given image.

`docker ps`


`docker ps -a`


`docker stop`

`docker rm <container name>`  removes a container permanently 


`docker images` list all images

`docker rmi <image name>`   delete image

`docker pull <image name>` 


Note - if we run `docker run ubuntu` it won't spin up a contianer since there is no application or process running inside that container. Ubuntu is an operating system that is used as a base image for different application and since there is no other application running inside that container, it will simply exit.



`docker exec <coontainer name> <command>` this will execute the command inside the specified container


Attached and detached mode - 

`docker run <container name>` will run the contianer in attached mode meaning it will run in stdout. For making it run in backgeound we can run it in detached mode using `-d` such as `docker run -d <container name>`. You can attahc a container running in detached mode later too - by using `docker attach <container id>`


For data to persist in a dockerised application, we will have to mount the container with a volume. This volume will reside inside the the docker host but outside the docker container and will persist the data there. This way, even if the container is destroyed, the data is persisted inthe docker host.

for this, we can use the `-v` option with `run` command.

e.g. `docker run -v /opt/datadir/volume_to_be_mounted:/var/lib/mysql mysql`


`docker inspect <container name>` for finding info on a container.




`docker log <container name>` for logs



`kubectl get pods`

`kubectl describe pods <pod name>`

`kubectl create -f <yaml file to create pods with>`

`kubectl get replicaset`

`kubectl delete replicaset <name of the replicaset, myapp-replicaset>` also delees all the underlying pods

`kubectl replace -f <yaml file for replicaset>` -> in the event of updating the number of replicas etc

`kubectl scale replicas=6 -f replicaset-definition.yaml` --> to update the number of replicas via command line. This won't change the value of replicas in the definition file.

In case of replica set, we always need to provide the `template` part in `spec`. We need to do it in order to allow the creating of a new pod in the event of failure of it. 
### ----

`kubectl run <name of the pod> --image=<image name> --dry-run=client -o yaml` --> outputs a yaml file (thanks to `-o` flag in `yaml` format which can be used to edit and create new `yaml` file for creation of new pods.  

### ----

Replication controllers are used to ensure high availability. We can use replication controller even if we are running a single container. It also helps in load balancing and scaling. Although, it is an older way of habdling these issues. The newer way to do it is Replica Set.





(Optional) Additional information about ETCDCTL UtilityETCDCTL is the CLI tool used to interact with ETCD.ETCDCTL can interact with ETCD Server using 2 API versions – Version 2 and Version 3.  By default it’s set to use Version 2. Each version has different sets of commands.

For example, ETCDCTL version 2 supports the following commands:

`etcdctl backup`

`etcdctl cluster-health`

`etcdctl mk`

`etcdctl mkdir`

`etcdctl set`

Whereas the commands are different in version 3


`etcdctl snapshot save`

`etcdctl endpoint health`

`etcdctl get`

`etcdctl put`

To set the right version of API set the environment variable ETCDCTL_API command

export ETCDCTL_API=3

When the API version is not set, it is assumed to be set to version 2. And version 3 commands listed above don’t work. When API version is set to version 3, version 2 commands listed above don’t work.

Apart from that, you must also specify the path to certificate files so that ETCDCTL can authenticate to the ETCD API Server. The certificate files are available in the etcd-master at the following path. We discuss more about certificates in the security section of this course. So don’t worry if this looks complex:

`--cacert /etc/kubernetes/pki/etcd/ca.crt`

`--cert /etc/kubernetes/pki/etcd/server.crt`

`--key /etc/kubernetes/pki/etcd/server.key`

So for the commands, I showed in the previous video to work you must specify the ETCDCTL API version and path to certificate files. Below is the final form:

`kubectl exec etcd-controlplane -n kube-system -- sh -c "ETCDCTL_API=3 etcdctl get / --prefix --keys-only --limit=10 --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt --key /etc/kubernetes/pki/etcd/server.key"`









While you would be working mostly the declarative way – using definition files, imperative commands can help in getting one-time tasks done quickly, as well as generate a definition template easily. This would help save a considerable amount of time during your exams.

Before we begin, familiarize yourself with the two options that can come in handy while working with the below commands:

--dry-run By default as soon as the command is run, the resource will be created. If you simply want to test your command, use the --dry-run=client option. This will not create the resource; instead, it tells you whether the resource can be created and if your command is right.

-o yaml This will output the resource definition in YAML format on the screen.

Use the above two in combination to generate a resource definition file quickly that you can then modify and create resources as required instead of creating the files from scratch.

POD
Create an NGINX Pod

kubectl run nginx --image=nginx
Generate POD Manifest YAML file (-o yaml). Don’t create it(–dry-run)

kubectl run nginx --image=nginx --dry-run=client -o yaml
Deployment
Create a deployment

kubectl create deployment --image=nginx nginx
Generate Deployment YAML file (-o yaml). Don’t create it(–dry-run)

kubectl create deployment --image=nginx nginx --dry-run=client -o yaml
Generate Deployment with 4 Replicas

kubectl create deployment nginx --image=nginx --replicas=4
You can also scale a deployment using the

kubectl scale
command.

kubectl scale deployment nginx--replicas=4
Another way to do this is to save the YAML definition to a file and modify

kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > nginx-deployment.yaml
You can then update the YAML file with the replicas or any other field before creating the deployment.

Service
Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379

kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml
(This will automatically use the pod’s labels as selectors)

Or

kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml 
(This will not use the pods labels as selectors, instead, it will assume selectors as app=redis.

You cannot pass in selectors as an option.

So, it does not work very well if your pod has a different label set. So, generate the file and modify the selectors before creating the service)

Create a Service named nginx of type NodePort to expose pod nginx’s port 80 on port 30080 on the nodes:

kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service --dry-run=client -o yaml
(This will automatically use the pod’s labels as selectors, but you cannot specify the node port. You have to generate a definition file and then add the node port manually before creating the service with the pod.)

Or

kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
(This will not use the pod labels as selectors.)

Both the above commands have their own challenges. While one of them cannot accept a selector, the other cannot accept a node port. I would recommend going with the

kubectl expose
command. If you need to specify a node port, generate a definition file using the same command and manually input the nodeport before creating the service.

