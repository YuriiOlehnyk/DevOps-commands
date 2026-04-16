# Introduction to Docker by Denis Astahov


### Install Docker on Ubuntu 18.04

>Installation
```bash
sudo apt update
sudo apt install apt-transport-https
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt update
sudo apt install docker-ce
sudo systemctl status docker
sudo usermod -aG docker $USER

>>>logout/login<<<

docker run hello-world
```

### First Commands
>Check status
```bash
docker ps   #running containers
docker ps -a  #all containers
docker images   # images
```
### Run container 
>Run container
```bash
docker search tomcat
docker pull tomcat
docker run -it -p 1234:8080 tomcat      # run (on command line)
docker run -it -p 8888:80 nginx  
docker run -d -p 8888:80 nginx      # run on background
docker run --rm <container>         - run container and automatically remove after it stops

docker build -t denis .         
docker images

docker run -it  -p 1234:80  denis:latest
docker run -d -p  1234:80  denis:latest

docker  ps     # list containers
docker  ps -a  # list all containers

docker tag denis_ubuntu denis_ubuntu-PROD       # change tag
docker tag denis_ubuntu denis_ubuntu-PROD:v2
```
### STOP AND DELETE
```bash
docker stop "ID" # stop container
docker rm   # delete container
docker rmi  # delete image
```

### UPDATE IMAGE
```bash
docker run -d -p 7777:80 denis_ubuntu4
docker exec -it 5267e21d140 /bin/bash       # get acces to image
echo "V2" >> /var/www/html/index.html       # write something
exit
docker commit 5267e21d140 denis_v2:latest   #update image
```

### Export/Import Docker Image to file
```bash
docker save image:tag > arch_name.tar
docker load -i arch_name.tar
```

### Import/Export Docker Image to AWS ECR

```bash
docker build -t denis:v1 .
aws ecr get-login --no-include-email --region=ca-central-1 
docker tag  denis:v1  12345678.dkr.ecr.ca-central-1.amazonaws.com/myrepo:latest
docker push 12345678.dkr.ecr.ca-central-1.amazonaws.com/myrepo:lastest

docker pull 12345678.dkr.ecr.ca-central-1.amazonaws.com/myrepo:latest
```



### Kill and Delete Containers and Images
>Remove containers and images
```bash
`docker rm -f $(docker ps -aq)`        # Delete all Containers
`docker rmi -f $(docker images -q)  `  # Delete all Images
```
