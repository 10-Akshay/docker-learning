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


# Docker communication between 2 containers

ubuntu@ip-172-31-29-42:~/docker-learning-for-devops$ docker network create mynetwork
7a23f40f98e0374101dc1982834c78523e4679d9491f1a4a5fff46e361ea111a
ubuntu@ip-172-31-29-42:~/docker-learning-for-devops$ docker network ls
NETWORK ID     NAME        DRIVER    SCOPE
fe443b9f51e5   bridge      bridge    local
c62fab8db5af   host        host      local
7a23f40f98e0   mynetwork   bridge    local
15896a163b44   none        null      local
ubuntu@ip-172-31-29-42:~/docker-learning-for-devops$ docker run -d -it container1 --network mynetwork ubuntu
Unable to find image 'container1:latest' locally
docker: Error response from daemon: pull access denied for container1, repository does not exist or may require 'docker login'

Run 'docker run --help' for more information
ubuntu@ip-172-31-29-42:~/docker-learning-for-devops$ docker run -d -it --name container1:latest --network mynetwork ubuntu
docker: Error response from daemon: Invalid container name (container1:latest), only [a-zA-Z0-9][a-zA-Z0-9_.-] are allowed

Run 'docker run --help' for more information
ubuntu@ip-172-31-29-42:~/docker-learning-for-devops$ docker run -d -it --name container:latest --network mynetwork ubuntu
docker: Error response from daemon: Invalid container name (container:latest), only [a-zA-Z0-9][a-zA-Z0-9_.-] are allowed

Run 'docker run --help' for more information
ubuntu@ip-172-31-29-42:~/docker-learning-for-devops$ docker run -d -it --name container1:latest --network mynetwork ubuntu
docker: Error response from daemon: Invalid container name (container1:latest), only [a-zA-Z0-9][a-zA-Z0-9_.-] are allowed

Run 'docker run --help' for more information
ubuntu@ip-172-31-29-42:~/docker-learning-for-devops$ docker run -dit --name container1 --network mynetwork ubuntu
5cc6c2a46ece9a39fd9e9f28d8816ff7b3494364fcc6f9300441bdc0068cc3f2
ubuntu@ip-172-31-29-42:~/docker-learning-for-devops$ docker run -dit --name container2 --network mynetwork ubuntu
c9f1ef9af42bb978fc7b0653e6ed71c665185dca9cfae5c7387b60ee0152d344
ubuntu@ip-172-31-29-42:~/docker-learning-for-devops$ docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED              STATUS              PORTS     NAMES
c9f1ef9af42b   ubuntu    "/bin/bash"   45 seconds ago       Up 45 seconds                 container2
5cc6c2a46ece   ubuntu    "/bin/bash"   About a minute ago   Up About a minute             container1
ubuntu@ip-172-31-29-42:~/docker-learning-for-devops$ docker exec -it container1 bash
root@5cc6c2a46ece:/# apt update
Get:1 http://archive.ubuntu.com/ubuntu resolute InRelease [136 kB]
Get:2 http://security.ubuntu.com/ubuntu resolute-security InRelease [137 kB]
Get:3 http://security.ubuntu.com/ubuntu resolute-security/restricted amd64 Packages [297 kB]
Get:4 http://security.ubuntu.com/ubuntu resolute-security/main amd64 Packages [325 kB]
Get:5 http://security.ubuntu.com/ubuntu resolute-security/universe amd64 Packages [157 kB]
Get:6 http://archive.ubuntu.com/ubuntu resolute-updates InRelease [137 kB]
Get:7 http://archive.ubuntu.com/ubuntu resolute-backports InRelease [136 kB]
Get:8 http://archive.ubuntu.com/ubuntu resolute/main amd64 Packages [1874 kB]
Get:9 http://archive.ubuntu.com/ubuntu resolute/universe amd64 Packages [20.1 MB]
Get:10 http://archive.ubuntu.com/ubuntu resolute/multiverse amd64 Packages [352 kB]
Get:11 http://archive.ubuntu.com/ubuntu resolute/restricted amd64 Packages [189 kB]
Get:12 http://archive.ubuntu.com/ubuntu resolute-updates/multiverse amd64 Packages [3584 B]
Get:13 http://archive.ubuntu.com/ubuntu resolute-updates/main amd64 Packages [372 kB]
Get:14 http://archive.ubuntu.com/ubuntu resolute-updates/restricted amd64 Packages [299 kB]
Get:15 http://archive.ubuntu.com/ubuntu resolute-updates/universe amd64 Packages [236 kB]
Fetched 24.8 MB in 2s (11.9 MB/s)
3 packages can be upgraded. Run 'apt list --upgradable' to see them.
root@5cc6c2a46ece:/# date
Sat Jul  4 17:43:43 UTC 2026
root@5cc6c2a46ece:/# uname
Linux
root@5cc6c2a46ece:/# whoami
root
root@5cc6c2a46ece:/# exit
exit
ubuntu@ip-172-31-29-42:~/docker-learning-for-devops$ docker exec -it container1 bash
root@5cc6c2a46ece:/# apt install iputils-ping -y
Installing:
  iputils-ping

Installing dependencies:
  libcap2  libidn2-0  libunistring5  linux-sysctl-defaults

Summary:
  Upgrading: 0, Installing: 5, Removing: 0, Not Upgrading: 3
  Download size: 761 kB
  Space needed: 2677 kB / 1517 MB available

Get:1 http://archive.ubuntu.com/ubuntu resolute/main amd64 libcap2 amd64 1:2.75-10ubuntu2 [29.3 kB]
Get:2 http://archive.ubuntu.com/ubuntu resolute/main amd64 libunistring5 amd64 1.3-2build1 [610 kB]
Get:3 http://archive.ubuntu.com/ubuntu resolute/main amd64 libidn2-0 amd64 2.3.8-4build1 [67.6 kB]
Get:4 http://archive.ubuntu.com/ubuntu resolute/main amd64 iputils-ping amd64 3:20250605-1ubuntu1 [47.1 kB]
Get:5 http://archive.ubuntu.com/ubuntu resolute/main amd64 linux-sysctl-defaults all 4.15ubuntu5 [6208 B]
Fetched 761 kB in 0s (8338 kB/s)
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 79, <STDIN> line 5.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC entries checked: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.40.1 /usr/local/share/perl/5.40.1 /usr/lib/x86_64-linux-gnu/perl5/5.40 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl-base /usr/lib/x86_64-linux-gnu/perl/5.40 /usr/share/perl/5.40 /usr/local/lib/site_perl) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 8, <STDIN> line 5.)
debconf: falling back to frontend: Teletype
Selecting previously unselected package libcap2:amd64.
(Reading database ... 7724 files and directories currently installed.)
Preparing to unpack .../libcap2_1%3a2.75-10ubuntu2_amd64.deb ...
Unpacking libcap2:amd64 (1:2.75-10ubuntu2) ...
Selecting previously unselected package libunistring5:amd64.
Preparing to unpack .../libunistring5_1.3-2build1_amd64.deb ...
Unpacking libunistring5:amd64 (1.3-2build1) ...
Selecting previously unselected package libidn2-0:amd64.
Preparing to unpack .../libidn2-0_2.3.8-4build1_amd64.deb ...
Unpacking libidn2-0:amd64 (2.3.8-4build1) ...
Selecting previously unselected package iputils-ping.
Preparing to unpack .../iputils-ping_3%3a20250605-1ubuntu1_amd64.deb ...
Unpacking iputils-ping (3:20250605-1ubuntu1) ...
Selecting previously unselected package linux-sysctl-defaults.
Preparing to unpack .../linux-sysctl-defaults_4.15ubuntu5_all.deb ...
Unpacking linux-sysctl-defaults (4.15ubuntu5) ...
Setting up libcap2:amd64 (1:2.75-10ubuntu2) ...
Setting up linux-sysctl-defaults (4.15ubuntu5) ...
Setting up libunistring5:amd64 (1.3-2build1) ...
Setting up libidn2-0:amd64 (2.3.8-4build1) ...
Setting up iputils-ping (3:20250605-1ubuntu1) ...
/var/lib/dpkg/info/iputils-ping.postinst: 8: setcap: not found
Processing triggers for procps (2:4.0.4-9ubuntu1) ...
procps: Applying updated sysctl configuration
sysctl: setting key "kernel.sysrq", ignoring: Read-only file system
sysctl: setting key "kernel.core_uses_pid", ignoring: Read-only file system
sysctl: setting key "net.ipv4.conf.default.rp_filter", ignoring: Read-only file system
sysctl: setting key "net.ipv4.conf.eth0.rp_filter", ignoring: Read-only file system
sysctl: setting key "net.ipv4.conf.lo.rp_filter", ignoring: Read-only file system
sysctl: setting key "net.ipv4.conf.default.accept_source_route", ignoring: Read-only file system
sysctl: setting key "net.ipv4.conf.eth0.accept_source_route", ignoring: Read-only file system
sysctl: setting key "net.ipv4.conf.lo.accept_source_route", ignoring: Read-only file system
sysctl: setting key "net.ipv4.conf.default.promote_secondaries", ignoring: Read-only file system
sysctl: setting key "net.ipv4.conf.eth0.promote_secondaries", ignoring: Read-only file system
sysctl: setting key "net.ipv4.conf.lo.promote_secondaries", ignoring: Read-only file system
sysctl: setting key "net.ipv4.ping_group_range", ignoring: Read-only file system
sysctl: setting key "fs.protected_hardlinks", ignoring: Read-only file system
sysctl: setting key "fs.protected_symlinks", ignoring: Read-only file system
sysctl: setting key "fs.protected_regular", ignoring: Read-only file system
sysctl: setting key "fs.protected_fifos", ignoring: Read-only file system
sysctl: setting key "vm.max_map_count", ignoring: Read-only file system
sysctl: setting key "kernel.printk", ignoring: Read-only file system
sysctl: setting key "net.ipv6.conf.all.use_tempaddr", ignoring: Read-only file system
sysctl: setting key "net.ipv6.conf.default.use_tempaddr", ignoring: Read-only file system
sysctl: setting key "kernel.kptr_restrict", ignoring: Read-only file system
sysctl: setting key "kernel.sysrq", ignoring: Read-only file system
sysctl: setting key "vm.max_map_count", ignoring: Read-only file system
sysctl: setting key "net.ipv4.conf.default.rp_filter", ignoring: Read-only file system
sysctl: setting key "net.ipv4.conf.all.rp_filter", ignoring: Read-only file system
sysctl: setting key "kernel.yama.ptrace_scope", ignoring: Read-only file system
sysctl: setting key "vm.mmap_min_addr", ignoring: Read-only file system
Processing triggers for libc-bin (2.43-2ubuntu2) ...
root@5cc6c2a46ece:/# exit
exit
ubuntu@ip-172-31-29-42:~/docker-learning-for-devops$ docker exec -it container2 bash
root@c9f1ef9af42b:/# ping container1
bash: ping: command not found
root@c9f1ef9af42b:/# apt update
Get:1 http://security.ubuntu.com/ubuntu resolute-security InRelease [137 kB]
Get:2 http://security.ubuntu.com/ubuntu resolute-security/universe amd64 Packages [157 kB]
Get:3 http://archive.ubuntu.com/ubuntu resolute InRelease [136 kB]
Get:4 http://security.ubuntu.com/ubuntu resolute-security/restricted amd64 Packages [297 kB]
Get:5 http://security.ubuntu.com/ubuntu resolute-security/main amd64 Packages [325 kB]
Get:6 http://archive.ubuntu.com/ubuntu resolute-updates InRelease [137 kB]
Get:7 http://archive.ubuntu.com/ubuntu resolute-backports InRelease [136 kB]
Get:8 http://archive.ubuntu.com/ubuntu resolute/multiverse amd64 Packages [352 kB]
Get:9 http://archive.ubuntu.com/ubuntu resolute/universe amd64 Packages [20.1 MB]
Get:10 http://archive.ubuntu.com/ubuntu resolute/restricted amd64 Packages [189 kB]
Get:11 http://archive.ubuntu.com/ubuntu resolute/main amd64 Packages [1874 kB]
Get:12 http://archive.ubuntu.com/ubuntu resolute-updates/main amd64 Packages [372 kB]
Get:13 http://archive.ubuntu.com/ubuntu resolute-updates/universe amd64 Packages [236 kB]
Get:14 http://archive.ubuntu.com/ubuntu resolute-updates/multiverse amd64 Packages [3584 B]
Get:15 http://archive.ubuntu.com/ubuntu resolute-updates/restricted amd64 Packages [299 kB]
Fetched 24.8 MB in 3s (9599 kB/s)
3 packages can be upgraded. Run 'apt list --upgradable' to see them.
root@c9f1ef9af42b:/# apt install iputils-ping -y
Installing:
  iputils-ping

Installing dependencies:
  libcap2  libidn2-0  libunistring5  linux-sysctl-defaults

Summary:
  Upgrading: 0, Installing: 5, Removing: 0, Not Upgrading: 3
  Download size: 761 kB
  Space needed: 2677 kB / 1473 MB available

Get:1 http://archive.ubuntu.com/ubuntu resolute/main amd64 libcap2 amd64 1:2.75-10ubuntu2 [29.3 kB]
Get:2 http://archive.ubuntu.com/ubuntu resolute/main amd64 libunistring5 amd64 1.3-2build1 [610 kB]
Get:3 http://archive.ubuntu.com/ubuntu resolute/main amd64 libidn2-0 amd64 2.3.8-4build1 [67.6 kB]
Get:4 http://archive.ubuntu.com/ubuntu resolute/main amd64 iputils-ping amd64 3:20250605-1ubuntu1 [47.1 kB]
Get:5 http://archive.ubuntu.com/ubuntu resolute/main amd64 linux-sysctl-defaults all 4.15ubuntu5 [6208 B]
Fetched 761 kB in 0s (6376 kB/s)
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 79, <STDIN> line 5.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC entries checked: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.40.1 /usr/local/share/perl/5.40.1 /usr/lib/x86_64-linux-gnu/perl5/5.40 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl-base /usr/lib/x86_64-linux-gnu/perl/5.40 /usr/share/perl/5.40 /usr/local/lib/site_perl) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 8, <STDIN> line 5.)
debconf: falling back to frontend: Teletype
Selecting previously unselected package libcap2:amd64.
(Reading database ... 7724 files and directories currently installed.)
Preparing to unpack .../libcap2_1%3a2.75-10ubuntu2_amd64.deb ...
Unpacking libcap2:amd64 (1:2.75-10ubuntu2) ...
Selecting previously unselected package libunistring5:amd64.
Preparing to unpack .../libunistring5_1.3-2build1_amd64.deb ...
Unpacking libunistring5:amd64 (1.3-2build1) ...
Selecting previously unselected package libidn2-0:amd64.
Preparing to unpack .../libidn2-0_2.3.8-4build1_amd64.deb ...
Unpacking libidn2-0:amd64 (2.3.8-4build1) ...
Selecting previously unselected package iputils-ping.
Preparing to unpack .../iputils-ping_3%3a20250605-1ubuntu1_amd64.deb ...
Unpacking iputils-ping (3:20250605-1ubuntu1) ...
Selecting previously unselected package linux-sysctl-defaults.
Preparing to unpack .../linux-sysctl-defaults_4.15ubuntu5_all.deb ...
Unpacking linux-sysctl-defaults (4.15ubuntu5) ...
Setting up libcap2:amd64 (1:2.75-10ubuntu2) ...
Setting up linux-sysctl-defaults (4.15ubuntu5) ...
Setting up libunistring5:amd64 (1.3-2build1) ...
Setting up libidn2-0:amd64 (2.3.8-4build1) ...
Setting up iputils-ping (3:20250605-1ubuntu1) ...
/var/lib/dpkg/info/iputils-ping.postinst: 8: setcap: not found
Processing triggers for procps (2:4.0.4-9ubuntu1) ...
procps: Applying updated sysctl configuration
sysctl: setting key "kernel.sysrq", ignoring: Read-only file system
sysctl: setting key "kernel.core_uses_pid", ignoring: Read-only file system
sysctl: setting key "net.ipv4.conf.default.rp_filter", ignoring: Read-only file system
sysctl: setting key "net.ipv4.conf.eth0.rp_filter", ignoring: Read-only file system
sysctl: setting key "net.ipv4.conf.lo.rp_filter", ignoring: Read-only file system
sysctl: setting key "net.ipv4.conf.default.accept_source_route", ignoring: Read-only file system
sysctl: setting key "net.ipv4.conf.eth0.accept_source_route", ignoring: Read-only file system
sysctl: setting key "net.ipv4.conf.lo.accept_source_route", ignoring: Read-only file system
sysctl: setting key "net.ipv4.conf.default.promote_secondaries", ignoring: Read-only file system
sysctl: setting key "net.ipv4.conf.eth0.promote_secondaries", ignoring: Read-only file system
sysctl: setting key "net.ipv4.conf.lo.promote_secondaries", ignoring: Read-only file system
sysctl: setting key "net.ipv4.ping_group_range", ignoring: Read-only file system
sysctl: setting key "fs.protected_hardlinks", ignoring: Read-only file system
sysctl: setting key "fs.protected_symlinks", ignoring: Read-only file system
sysctl: setting key "fs.protected_regular", ignoring: Read-only file system
sysctl: setting key "fs.protected_fifos", ignoring: Read-only file system
sysctl: setting key "vm.max_map_count", ignoring: Read-only file system
sysctl: setting key "kernel.printk", ignoring: Read-only file system
sysctl: setting key "net.ipv6.conf.all.use_tempaddr", ignoring: Read-only file system
sysctl: setting key "net.ipv6.conf.default.use_tempaddr", ignoring: Read-only file system
sysctl: setting key "kernel.kptr_restrict", ignoring: Read-only file system
sysctl: setting key "kernel.sysrq", ignoring: Read-only file system
sysctl: setting key "vm.max_map_count", ignoring: Read-only file system
sysctl: setting key "net.ipv4.conf.default.rp_filter", ignoring: Read-only file system
sysctl: setting key "net.ipv4.conf.all.rp_filter", ignoring: Read-only file system
sysctl: setting key "kernel.yama.ptrace_scope", ignoring: Read-only file system
sysctl: setting key "vm.mmap_min_addr", ignoring: Read-only file system
Processing triggers for libc-bin (2.43-2ubuntu2) ...
root@c9f1ef9af42b:/# ping container1
PING container1 (172.18.0.2) 56(84) bytes of data.
64 bytes from container1.mynetwork (172.18.0.2): icmp_seq=1 ttl=64 time=0.067 ms
64 bytes from container1.mynetwork (172.18.0.2): icmp_seq=2 ttl=64 time=0.051 ms
64 bytes from container1.mynetwork (172.18.0.2): icmp_seq=3 ttl=64 time=0.049 ms
64 bytes from container1.mynetwork (172.18.0.2): icmp_seq=4 ttl=64 time=0.049 ms
64 bytes from container1.mynetwork (172.18.0.2): icmp_seq=5 ttl=64 time=0.048 ms
64 bytes from container1.mynetwork (172.18.0.2): icmp_seq=6 ttl=64 time=0.050 ms
64 bytes from container1.mynetwork (172.18.0.2): icmp_seq=7 ttl=64 time=0.044 ms
64 bytes from container1.mynetwork (172.18.0.2): icmp_seq=8 ttl=64 time=0.063 ms
64 bytes from container1.mynetwork (172.18.0.2): icmp_seq=9 ttl=64 time=0.051 ms
64 bytes from container1.mynetwork (172.18.0.2): icmp_seq=10 ttl=64 time=0.047 ms
64 bytes from container1.mynetwork (172.18.0.2): icmp_seq=11 ttl=64 time=0.051 ms
64 bytes from container1.mynetwork (172.18.0.2): icmp_seq=12 ttl=64 time=0.051 ms
64 bytes from container1.mynetwork (172.18.0.2): icmp_seq=13 ttl=64 time=0.055 ms
64 bytes from container1.mynetwork (172.18.0.2): icmp_seq=14 ttl=64 time=0.048 ms
64 bytes from container1.mynetwork (172.18.0.2): icmp_seq=15 ttl=64 time=0.051 ms
64 bytes from container1.mynetwork (172.18.0.2): icmp_seq=16 ttl=64 time=0.046 ms
64 bytes from container1.mynetwork (172.18.0.2): icmp_seq=17 ttl=64 time=0.058 ms
64 bytes from container1.mynetwork (172.18.0.2): icmp_seq=18 ttl=64 time=0.050 ms
64 bytes from container1.mynetwork (172.18.0.2): icmp_seq=19 ttl=64 time=0.051 ms
^C
--- container1 ping statistics ---
19 packets transmitted, 19 received, 0% packet loss, time 18394ms
rtt min/avg/max/mdev = 0.044/0.051/0.067/0.005 ms
root@c9f1ef9af42b:/#

