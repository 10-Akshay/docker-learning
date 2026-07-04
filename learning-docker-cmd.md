# Running docker containers :
ubuntu@ip-172-31-29-42:~/docker-learning-for-devops$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

# Docker images :
ubuntu@ip-172-31-29-42:~/docker-learning-for-devops$ docker images
                                                                                           i Info →   U  In Use
IMAGE           ID             DISK USAGE   CONTENT SIZE   EXTRA
nginx:latest    ec4ed8b5299e        241MB           66MB    U
python:latest   09b29c360b84       1.63GB          432MB    U

# Pull new image :
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

# Login to docker hub

ubuntu@ip-172-31-29-42:~/docker-learning-for-devops$ docker login
Authenticating with existing credentials... [Username: akshaysutar10]

i Info → To login with a different account, run 'docker logout' followed by 'docker login'


Login Succeeded

# Running containers

ubuntu@ip-172-31-29-42:~/docker-learning-for-devops$ docker ps -a
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS                      PORTS     NAMES
454454093f79   ubuntu        "/bin/bash"              28 minutes ago   Exited (0) 27 minutes ago             vibrant_curie
8205565f9ea7   nginx         "/docker-entrypoint.…"   34 minutes ago   Exited (0) 34 minutes ago             affectionate_tesla
c64323364be9   hello-world   "/hello"                 46 minutes ago   Exited (0) 46 minutes ago             keen_pascal
1bf4c414ee64   ubuntu        "/bin/bash"              2 hours ago      Exited (0) 2 hours ago                sad_driscoll
326eb7805526   python        "python3"                17 hours ago     Exited (0) 17 hours ago               vigorous_kapitsa
e1d319c1214f   nginx         "/docker-entrypoint.…"   18 hours ago     Exited (0) 16 hours ago               silly_darwin






# Docker network 

ubuntu@ip-172-31-29-42:~$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
fe443b9f51e5   bridge    bridge    local
c62fab8db5af   host      host      local
15896a163b44   none      null      local


ubuntu@ip-172-31-29-42:~$ docker network create mynetwork1
fa91eab5dbc37a8fe304a0c810a9788485a3aaa24b72a1ccc607157357cb8c4a

ubuntu@ip-172-31-29-42:~$ docker network ls
NETWORK ID     NAME         DRIVER    SCOPE
fe443b9f51e5   bridge       bridge    local
c62fab8db5af   host         host      local
fa91eab5dbc3   mynetwork1   bridge    local
15896a163b44   none         null      local


ubuntu@ip-172-31-29-42:~$ docker network create mynetwork2 -d host
Error response from daemon: only one instance of "host" network is allowed

ubuntu@ip-172-31-29-42:~$ docker network create mynetwork2 -d bridge
f459036e542aeb6bd553ec53815f85d96fa46c867b9ee0a137c5c6a8d30bfa0e


ubuntu@ip-172-31-29-42:~$ docker network ls

NETWORK ID     NAME         DRIVER    SCOPE
fe443b9f51e5   bridge       bridge    local
c62fab8db5af   host         host      local
fa91eab5dbc3   mynetwork1   bridge    local
f459036e542a   mynetwork2   bridge    local
15896a163b44   none         null      local

