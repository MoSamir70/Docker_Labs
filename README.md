# -------------------------- Problem 1 --------------------------------------

## Run hello-world

```bash
docker run hello-world
```

## Check container status

```bash
docker ps -a
```

## Start stopped container

```bash
docker start CONTAINER_ID
```

## Remove container

```bash
docker rm CONTAINER_ID
```

## Remove image 
##### --> hit the "i" in rmi for image

```bash
docker rmi hello-world
```

# -------------------------- Problem 2 --------------------------------------cd

### Problem 2 Concept -> Persistence data inside container

## Run Ubuntu Interactive Container

```bash
docker run -it ubuntu   
```
--> root@3c2613b70cd8:/#   +  Writ Ur command here

## Run echo command 

```bash
echo docker
```

--> output = docker

## Create file

```bash
touch hello-docker
```

### check it is exists

```bash
ls
```
output ---> bin  boot  dev  etc  hello-docker  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

## Exit container

```bash
exit or Ctrl + D
```

## Show containers

```bash
docker ps -a
```
Status --> Exited

## Restart container

```bash
docker start CONTAINER_ID
docker exec -it CONTAINER_ID bash
```
--> File or data is still existed not deleted

## Stop container

```bash
docker stop CONTAINER_ID
```

## Remove container

```bash
docker rm CONTAINER_ID
```

## Remove all stopped containers

```bash
docker container prune
```

## Note

The file hello-docker exists as long as the container exists.
If the container is removed, the file is deleted too.

hint: Must Stop Container before Remove or it will show error 
U can use do 2 commands in one command using Force

```bash
docker rm -f mycontainer
```
