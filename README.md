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

### IMPORTANT

`kubectl run <name of the pod> --image=<image name> --dry-run=client -o yaml` --> outputs a yaml file (thanks to `-o` flag in `yaml` format which can be used to edit and create new `yaml` file for creation of new pods.  


