# -------------------------- Problem 1 --------------------------

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

# -------------------------- Problem 2 --------------------------

# Problem 2 Concept -> Persistence data inside container

### Run Ubuntu Interactive Container

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

1- The file hello-docker exists as long as the container exists.
If the container is removed, the file is deleted too.

2- Must Stop Container before Remove or it will show error 
U can use do 2 commands in one command using Force

```bash
docker rm -f mycontainer
```


# -------------------------- Problem 3 --------------------------

# Problem 3 : MySQL + background containers + environment variables

## Run MySQL Container

================================ Multi Line Problem
#### for bash

```bash
docker run -d \
--name app-database \
-p 3306:3306 \
-e MYSQL_ROOT_PASSWORD=P4sSw0rd0! \
mysql:latest
```

#### for Powershell

```bash
docker run -d `
--name app-database `
-e MYSQL_ROOT_PASSWORD=P4sSw0rd0! `
mysql:latest
```

#### for CMD

```bash
docker run -d `
--name app-database `
-e MYSQL_ROOT_PASSWORD=P4sSw0rd0! `
mysql:latest
```

### best Solution = One Line 

```bash
docker run -d --name app-database -e MYSQL_ROOT_PASSWORD=P4sSw0rd0! mysql:latest
```

```text
-d = Detached mode to work in background 
--name => Container name
-e for Environment Variable
```

================================= end of Multi Line Problem


## Check running containers

```bash
docker ps
```
Output --> Status = Up

## Show logs

```bash
docker logs app-database
```

## Enter container

```bash
docker exec -it app-database bash
```

## Login to MySQL

```bash
mysql -u root -p
```
bash-5.1# + mysql -u root -p

Enter Password:

```text
P4sSw0rd0!
```

```text
hint: Passward in Hidden just Paste it then hit enter --- output --> Welcome to the MySQL monitor.
```

## Show databases

```sql
SHOW DATABASES;
```
output ->
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+

## Exit container

```bash
exit  from mysql> then bash-5.1# 
```

```bash
docker stop app-database
docker rm app-database
```

Hint: Keep mysql Image As it so large "1.29 GB" No need to download it  again