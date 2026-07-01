# Running docker containers :
ubuntu@ip-172-31-29-42:~/docker-learning-for-devops$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

# Docker images :
ubuntu@ip-172-31-29-42:~/docker-learning-for-devops$ docker images
                                                                                           i Info →   U  In Use
IMAGE           ID             DISK USAGE   CONTENT SIZE   EXTRA
nginx:latest    ec4ed8b5299e        241MB           66MB    U
python:latest   09b29c360b84       1.63GB          432MB    U

#Pull new image :
ubuntu@ip-172-31-29-42:~/docker-learning-for-devops$ sudo docker  pull ubuntu
Using default tag: latest
latest: Pulling from library/ubuntu
d1f56e4c7f2f: Pull complete
81e2f2053c8f: Pull complete
107e4f1717f2: Download complete
Digest: sha256:53958ec7b67c2c9355df922dd08dbf0360611f8c3cdb656875e81873db9ffdba
Status: Downloaded newer image for ubuntu:latest
docker.io/library/ubuntu:latest

ubuntu@ip-172-31-29-42:~/docker-learning-for-devops$ docker images
                                                                                           i Info →   U  In Use
IMAGE           ID             DISK USAGE   CONTENT SIZE   EXTRA
nginx:latest    ec4ed8b5299e        241MB           66MB    U
python:latest   09b29c360b84       1.63GB          432MB    U
ubuntu:latest   53958ec7b67c        160MB         45.3MB

