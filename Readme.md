Command to install docker on amazon linux:
sudo yum install docker -y

Command to start docker:
sudo systemctl start docker

Command to check docker status:
sudo systemctl status docker

Command to stop docker:
sudo systemctl stop docker

Command to add ec2-user to the docker group:
sudo usermod -aG docker ec2-user

Command to check running containers:
docker ps

Command to check all containers:
docker ps -a

Command to run a container from the image in interactive mode:
docker run -it --name <containername> <imagename> /bin/bash

Command to run a container from the image in detached mode:
docker run -td --name <containername> <imagename>

Command to connect to a running container:
docker exec -it <containername> /bin/bash

Command to stop a running container:
docker stop <containerid>

Command to start a running container:
docker start <containerid>

Command to forecefully stop the container:
docker kill <containerid>

Command to create an image from a container:
docker commit <containername> <dockerhubusername>/<imagename>

command to bind host directory with container dir
docker run -td --name container001 -v <host dir>:<container dir> ubuntu

Command to create docker volume:
docker volume create <volume name>

Command to Delete a volume:
docker volume rm <volume name>

Command to mount the volume to a container:
docker run -td --name container001 -v <volumename>:<container dir> ubuntu

Command to mount a volume in read-only mode:
docker run -it -v <volumename>:<container dir>:ro <imagename>

start a new container with the same volumes as an existing container
docker run -d --name <newcontainer> --volumes-from <existing-container> <imagename>



simple docker file:

FROM ubuntu
WORKDIR /tmp
RUN echo "Good day" > /tmp/one.txt
ENV project DEVOPS
COPY hello.txt /tmp


apache2 dockerfile:

FROM ubuntu
RUN apt-get update
RUN apt-get install -y iputils-ping
RUN apt-get install net-tools
ENV owner puneet
RUN apt-get install -y apache2
ADD index.html /var/www/html
EXPOSE 80
WORKDIR /usr/sbin
CMD ["apachectl", "-D", "FOREGROUND"]




DOCKER NETWORKING:

docker network ls


default bridge network:

docker run -d --name c1 myimage01

docker run -d --name c2 myimage01

docker container inspect c1 | grep IPAdd

docker container inspect c2 | grep IPAdd

docker exec c1 ping 172.17.0.3

docker exec c2 ping 172.17.0.2


custom bridge network:


docker network create mynetwork --driver bridge --subnet 20.21.0.0/16 --gateway 20.21.0.1

docker network ls

docker run -d --name c3 --network mynetwork myimage01

docker run -d --name c4 --network mynetwork myimage01

docker container inspect c3 | grep IPAdd

docker container inspect c4 | grep IPAdd

docker exec c3 ping 20.21.0.3

docker exec c4 ping 20.21.0.2

docker exec c4 ping 172.17.0.2



none:

docker run -d --name c5 --network none myimage01
docker container inspect c5 | grep IPAdd
docker exec c5 ifconfig
docker network ls
docker network inspect none;

docker network disconnect mynetwork c4

host:

docker run -d --name c6 --network host myimage01
docker container inspect c6 | grep IPAdd
docker ps
docker exec c6 hostname -i
