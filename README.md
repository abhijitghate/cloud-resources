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



