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

### Problem 3: MySQL+ background containers + environment variables For Password

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
```text
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
```

## Exit container

```bash
exit  
```
---> from mysql> then bash-5.1# 

```bash
docker stop app-database
docker rm app-database
```
Hint: Keep mysql Image As it so large "1.29 GB" No need to download it  again


# -------------------------- Problem 4 --------------------------

# Problem 4: Web Server + Static Files + Docker commit 


```text
--> Create mkdir problem-4 then cd problem-4
--> manually craete html.index 
--> add content inside it 
```

```text
<!DOCTYPE html>
<html>
<head>
    <title>Docker Lab </title>
</head>
<body>
    <h1>Hello From Docker Nginx -> Problem 4 </h1>
</body>
</html>
```

## Run Nginx Container

```bash
docker run -d \
--name nginx-container \
-p 8080:80 \
nginx
```

```text
| Part | Meaning                     |
| ---- | --------------------------- |
| 8080 | Port on Windows             |
| 80   | Port inside Nginx container |
| -p   | Publish/Expose Link port from HOST_PORT 8080 to CONTAINER_PORT 80 |
```
```text
-p --> p small for developer customize ports
-P --> P Capital Docker automatic choose ports
```

## Test Public Link

```text
http://localhost:8080 
```
--output--> Welcome to nginx!


## Copy HTML file

```bash
docker cp index.html nginx-container:/usr/share/nginx/html/index.html
```
output--> Successfully copied + then Refresh Browser to see html content "Hello From Docker Nginx-> Problem 4"

## Commit Container As New Image

```bash
docker commit nginx-container my-nginx:v1
```
```text
--> docker commit nginx-container IMAGE_NAME : Tag
--> For best Practice in Production use Dockerfile instead of docker commit
```

## Check Images

```bash
docker images
```
--> my-nginx Must be existed

## Remove old Container 

```bash
docker stop nginx-container
docker rm nginx-container
```

```text
--> Remove old Container if old & new has same name otherwise no need for this step
--> For best practice remove old to prevent using -p 8080:80 twise -> so U will get an error
```

## Test New Image / Run committed image 

```bash
docker run -d \
--name test-nginx \
-p 8080:80 \
my-nginx:v1
```

## Remove Test Container 

```bash
docker stop test-nginx
docker rm test-nginx
```

# -------------------------- Problem 5 --------------------------


# Problem 5: Python app + Docker file + Multi-stage build + Push to docker hub

```text
--> mkdir problem-5 then cd problem-5
--> manually craete  app.py   
--> add content inside it --> print("Hello From Docker Python App")
--> Test Locally --> python app.py and shoud print Hello From Docker Python App
--> Hint Python Must be installed 

-->  manually craete Dockerfile without extention 
```

## Dockerfile Content

```text
FROM python:3.12

WORKDIR /app

COPY app.py .

CMD ["python", "app.py"]
```

```text
| Command | Meaning           |
| ------- | ----------------- |
| FROM    | base image        |
| WORKDIR | working directory |
| COPY    | copy files        |
| CMD     | startup command   |
```

## Build Image

```bash
docker build -t python-app .
```

```text
| Part       | Meaning        |
| ---------- | -------------- |
| -t         | image tag      |
| python-app | image name     |
| .          | current folder |
```

### check images
```bash
docker images
```
--> python-app  1.6 GB !! -> Very Large Size

## Run Container

```bash
docker run python-app
```
output --> Hello From Docker Python App

### Bonus — Smaller Image Using Multi-Stage

#### Update Docker file

```dockerfile
FROM python:3.12-slim AS builder

WORKDIR /app

COPY app.py .

FROM python:3.12-alpine

WORKDIR /app

COPY --from=builder /app/app.py .

CMD ["python", "app.py"]
```

```text
| Part       | Meaning        |
| ---------- | -------------- |
| Slim       | small Python image|
| Alpine     | Linux minimal |
```

## Build Smaller Image

```bash
docker build -t python-app-small .
```

```text
docker images --> check images & Compare Sizes 
```
--> From 1.6 GB  to 74.3 MB

## Login & Push To Docker Hub

```bash
docker login

docker tag python-app-small USERNAME/python-app:v1

docker push USERNAME/python-app:v1
```


```bash
USERNAME --> samir079  

Hint: You can easily sign up from:[Docker Hub Official Website](https://hub.docker.com)  

So no need to enter username & password in docker login step.

Already pushed image, so if any other device needs to use it: docker run samir079/python-app:v1
```


## Docker File Content 


```text
# =========================
# Version 2  = Small Size + Multi-stage build
# =========================

FROM python:3.12-slim AS builder

WORKDIR /app

COPY app.py .

FROM python:3.12-alpine

WORKDIR /app

COPY --from=builder /app/app.py .

CMD ["python", "app.py"]


# =========================
# Version 1 = Large Size + Hint: There is no Multi line option in docker file
# =========================
#
# FROM python:3.12
#
# WORKDIR /app
#
# COPY app.py .
#
# CMD ["python", "app.py"]
```

