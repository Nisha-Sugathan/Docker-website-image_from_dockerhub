# Docker-website-image_from_dockerhub
Docker Hub registry contains images, we can also manage the images on docker hub. In this project we create a custom image on local machine and push that image to docker hub. On another instance we pull that custom image and create container for a website by pulling that image from docker hub.

### Prerequisites
Need to install docker on your machine
Add your username to docker group

### Docker Installation

```
sudo yum install docker -y &> /dev/null
sudo systemctl restart docker.service
sudo systemctl enable docker.service
sudo usermod -a -G docker ec2-user
```
![docker_installation](https://github.com/Nisha-Sugathan/Docker-Bind_mounting/assets/134600837/ba7797c4-9a73-4ce6-b593-2befa5850e0d)

###  Sitefiles version 1 in a project directory 

```
 wget https://www.tooplate.com/zip-templates/2134_gotto_job.zip
 unzip  2134_gotto_job.zip
 mv 2134_gotto_job html
 ```
 
 ####  Dockerfile 
 
```
FROM httpd:alpine
COPY ./html/ /usr/local/apache2/htdocs/
CMD ["httpd-foreground"]

```
 ### Building docker image 
 
 ```
 docker build -t jobapp:1 .
 ```
 ### Docker login
  ```
 $ docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: nishasugathan2022
Password: 
WARNING! Your password will be stored unencrypted in /home/ec2-user/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded

 ```
 ### Create tag and then push image using resource tag
 ```
 docker image tag jobapp:1 nishasugathan2022/jobapp:1
 docker image push nishasugathan2022/jobapp:1 
 
  docker image tag nishasugathan2022/jobapp:1 nishasugathan2022/jobapp:latest
  docker image push nishasugathan2022/jobapp:latest
 ```
 ### Pulling image on client machine
 We need to SSH in to another client machine, and install docker and its coreesponding settings, then pull the image using the docker pull command below.
 
 ```
  docker pull nishasugathan2022/jobapp:latest
  ```
  
  ### Version1 webserver container
  ```
  docker container run -d -p 8081:80 --name webserver1 nishasugathan2022/jobapp:latest
 ```
 
 ###   Site version 2 release 
 We need to build, tag and push the image of version2 to git hub
   ```
    docker image build -t nishasugathan2022/jobapp:2 .
    docker image tag nishasugathan2022/jobapp:2 nishasugathan2022/jobapp:latest
    docker image  push nishasugathan2022/jobapp:latest
   ```
 ### Pull the version2 latest image and create container on client 
 
 ```
  docker pull nishasugathan2022/jobapp``
  docker container run -d -p 8082:80 --name webserver2 nishasugathan2022/jobapp:latest
 ```
 ###   Site version 3 release 
 We need to build, tag and push the image of version3 to git hub
 
 ```
 docker image build -t nishasugathan2022/jobapp:3 
 docker image tag nishasugathan2022/jobapp:3 nishasugathan2022/jobapp:latest
 docker image push nishasugathan2022/jobapp:latest
 ```
  ### Pull the version2 latest image and create container on client 
  
 ```
 docker image pull nishasugathan2022/jobapp
 docker container run -d -p 8083:80 --name webserver3 nishasugathan2022/jobapp:latest
 
 ```
 
 Screenshot of list of containers in client machine
 
 
 ![image](https://github.com/Nisha-Sugathan/Docker-website-image_from_dockerhub/assets/134600837/44e65122-1659-4dc9-b2d5-50d3dc7df242)

  
  ### Output links
  
  ```
  http://ec2-65-0-89-112.ap-south-1.compute.amazonaws.com:8081/
  http://ec2-65-0-89-112.ap-south-1.compute.amazonaws.com:8082/
  http://ec2-65-0-89-112.ap-south-1.compute.amazonaws.com:8083/
  
  ```

 
