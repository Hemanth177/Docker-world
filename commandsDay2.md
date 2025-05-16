 1  # Add Docker's official GPG key:
    2  sudo apt-get update
    3  sudo apt-get install ca-certificates curl
    4  sudo install -m 0755 -d /etc/apt/keyrings
    5  sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
    6  sudo chmod a+r /etc/apt/keyrings/docker.asc
    7  # Add the repository to Apt sources:
    8  echo   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
    9    $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" |   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   10  sudo apt-get update
   11  sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   12  sudo usermod -aG docker ubuntu
   13  docker --version
   14  docker ps
   15  docker images
   16  docker run -td --name c1 nginx
   17  docker images
   18  docker ps
   19  docker stop c1
   20  docker ps
   21  docker ps -a
   22  docker start c1
   23  docker ps
   24  docker exec -it c1 ls
   25  docker exec -it c1 df -h
   26  docker exec -it c1 touch hello.txt
   27  docker exec -it c1 mkdir testfolder
   28  docker exec -it c1 /bin/bash
   29  docker run -td --name c2 nginx
   30  docker ps
   31  docker exec -it c1 /bin/bash
   32  docker ps
   33  docker commit c1 myimage001
   34  docker images
   35  docker run -td --name mycontainer myimage001
   36  docker ps
   37  docker exec -it mycontainer /bin/bash
   38  docker ps
   39  docker rm c2
   40  docker stop c2
   41  docker ps
   42  docker ps -a
   43  docker rm c2
   44  docker ps -a
   45  docker images
   46  docker rmi nginx
   47  docker images
   48  docker tag myimage001 puneetgavri/may2025
   49  docker images
   50  docker push puneetgavri/may2025
   51  docker login -u puneetgavri
   52  docker push puneetgavri/may2025
   53  docker commit c1 puneetgavri/may2025:v1
   54  docker images
   55  docker push puneetgavri/may2025:v1
   56  docker ps
   57  clear
   58  docker ps
   59  docker run -td --name applecontainer -p 8080:80  myimage001
   60  docker ps
   61  ls
   62  ls -lstr
   63  vi Dockerfile
   64  docker build customimage01 .
   65  docker build -t customimage01 .
   66  ll
   67  pwd
   68  vi Dockerfile
   69  docker build -t customimage01 .
   70  docker images
   71  docker run -td --name customc1 customimage01
   72  docker
   73  docker ps
   74  docker exec -it customc1 /bin/bash
   75  history
