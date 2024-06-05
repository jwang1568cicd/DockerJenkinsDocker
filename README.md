This project will focus on the Jenkins Docker pipeline within the docker container.

1. Dockerfile is based on jenkins/jenkins:lts with required plugins for both jenkins and docker.
FROM jenkins/jenkins:lts
USER root
RUN apt-get update -qq \
    && apt-get install -qqy apt-transport-https ca-certificates curl gnupg2 software-properties-common
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
RUN add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/debian \
   $(lsb_release -cs) \
   stable"
RUN apt-get update  -qq \
    && apt-get -y install docker-ce
RUN usermod -aG docker jenkins

2. The Jenkinsfile is an example for docker image build and verified with 'docker ps -a' command
pipeline {
    agent { dockerfile true }
    stages {
        stage('Test') {
            steps {
                echo 'docker images build and run within a docker container!'
		sh 'docker ps -a'
            }
        }
    }
}

3. The console.oupt is the capture of the Jenkins pipeline console log as a reference.

4. Here is are the CLI for verification with the docker container.

vagrant@jenkins:~$ sudo -i
root@jenkins:~#
root@jenkins:~#
root@jenkins:~#
root@jenkins:~# ls -la
total 52
drwx------  4 root root 4096 Jun  5 16:25 .
drwxr-xr-x 21 root root 4096 Jun  4 23:45 ..
-rw-------  1 root root 6258 Jun  5 16:42 .bash_history
-rw-r--r--  1 root root 3106 Dec  5  2019 .bashrc
drwx------  3 root root 4096 Jun  4 22:55 .docker
drwxr-xr-x  2 root root 4096 Jun  5 16:25 dockerjenkinsdocker
-rwxr-xr-x  1 root root  694 Jun  4 20:21 install_docker.sh
-rw-r--r--  1 root root  161 Dec  5  2019 .profile
-rwxr-xr-x  1 root root  531 Jun  4 18:18 setup_jenkins.sh
-rw-------  1 root root 5993 Jun  5 16:25 .viminfo
-rw-r--r--  1 root root  164 Jun  4 18:22 .wget-hsts
root@jenkins:~# cd dockerjenkinsdocker/
root@jenkins:~/dockerjenkinsdocker# ls -la
total 12
drwxr-xr-x 2 root root 4096 Jun  5 16:25 .
drwx------ 4 root root 4096 Jun  5 16:25 ..
-rw-r--r-- 1 root root  456 Jun  5 16:25 Dockerfile
root@jenkins:~/dockerjenkinsdocker# docker ps
CONTAINER ID   IMAGE                   COMMAND                  CREATED         STATUS         PORTS                                                                                      NAMES
bf4ce8a4f605   docker-jenkins-docker   "/usr/bin/tini -- /u…"   5 minutes ago   Up 5 minutes   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp, 0.0.0.0:50000->50000/tcp, :::50000->50000/tcp   optimistic_fermat
root@jenkins:~/dockerjenkinsdocker# docker logs bf4ce8a4f605
Running from: /usr/share/jenkins/jenkins.war
webroot: /var/jenkins_home/war
2024-06-05 16:37:08.922+0000 [id=1]     INFO    winstone.Logger#logInternal: Beginning extraction from war file
2024-06-05 16:37:10.242+0000 [id=1]     WARNING o.e.j.s.handler.ContextHandler#setContextPath: Empty contextPath
2024-06-05 16:37:10.296+0000 [id=1]     INFO    org.eclipse.jetty.server.Server#doStart: jetty-10.0.20; built: 2024-01-29T20:46:45.278Z; git: 3a745c71c23682146f262b99f4ddc4c1bc41630c; jvm 17.0.11+9
2024-06-05 16:37:10.589+0000 [id=1]     INFO    o.e.j.w.StandardDescriptorProcessor#visitServlet: NO JSP Support for /, did not find org.eclipse.jetty.jsp.JettyJspServlet
2024-06-05 16:37:10.636+0000 [id=1]     INFO    o.e.j.s.s.DefaultSessionIdManager#doStart: Session workerName=node0
2024-06-05 16:37:11.174+0000 [id=1]     INFO    hudson.WebAppMain#contextInitialized: Jenkins home directory: /var/jenkins_home found at: EnvVars.masterEnvVars.get("JENKINS_HOME")
2024-06-05 16:37:11.299+0000 [id=1]     INFO    o.e.j.s.handler.ContextHandler#doStart: Started w.@161f6623{Jenkins v2.452.1,/,file:///var/jenkins_home/war/,AVAILABLE}{/var/jenkins_home/war}
2024-06-05 16:37:11.320+0000 [id=1]     INFO    o.e.j.server.AbstractConnector#doStart: Started ServerConnector@3a1dd365{HTTP/1.1, (http/1.1)}{0.0.0.0:8080}
2024-06-05 16:37:11.328+0000 [id=1]     INFO    org.eclipse.jetty.server.Server#doStart: Started Server@f001896{STARTING}[10.0.20,sto=0] @2951ms
2024-06-05 16:37:11.339+0000 [id=25]    INFO    winstone.Logger#logInternal: Winstone Servlet Engine running: controlPort=disabled
2024-06-05 16:37:11.634+0000 [id=34]    INFO    jenkins.InitReactorRunner$1#onAttained: Started initialization
2024-06-05 16:37:11.649+0000 [id=35]    INFO    jenkins.InitReactorRunner$1#onAttained: Listed all plugins
2024-06-05 16:37:12.554+0000 [id=32]    INFO    jenkins.InitReactorRunner$1#onAttained: Prepared all plugins
2024-06-05 16:37:12.557+0000 [id=34]    INFO    jenkins.InitReactorRunner$1#onAttained: Started all plugins
2024-06-05 16:37:12.567+0000 [id=34]    INFO    jenkins.InitReactorRunner$1#onAttained: Augmented all extensions
2024-06-05 16:37:12.816+0000 [id=33]    INFO    jenkins.InitReactorRunner$1#onAttained: System config loaded
2024-06-05 16:37:12.820+0000 [id=33]    INFO    jenkins.InitReactorRunner$1#onAttained: System config adapted
2024-06-05 16:37:12.822+0000 [id=33]    INFO    jenkins.InitReactorRunner$1#onAttained: Loaded all jobs
2024-06-05 16:37:12.825+0000 [id=34]    INFO    jenkins.InitReactorRunner$1#onAttained: Configuration for all jobs updated
2024-06-05 16:37:13.031+0000 [id=48]    INFO    hudson.util.Retrier#start: Attempt #1 to do the action check updates server
2024-06-05 16:37:13.657+0000 [id=35]    INFO    jenkins.install.SetupWizard#init:

*************************************************************
*************************************************************
*************************************************************

Jenkins initial setup is required. An admin user has been created and a password generated.
Please use the following password to proceed to installation:

9647678aaedc467ab8e7b0d6fb673b57

This may also be found at: /var/jenkins_home/secrets/initialAdminPassword

*************************************************************
*************************************************************
*************************************************************

2024-06-05 16:37:32.911+0000 [id=35]    INFO    jenkins.InitReactorRunner$1#onAttained: Completed initialization
2024-06-05 16:37:32.979+0000 [id=24]    INFO    hudson.lifecycle.Lifecycle#onReady: Jenkins is fully up and running
2024-06-05 16:37:33.348+0000 [id=48]    INFO    h.m.DownloadService$Downloadable#load: Obtained the updated data file for hudson.tasks.Maven.MavenInstaller
2024-06-05 16:37:33.349+0000 [id=48]    INFO    hudson.util.Retrier#start: Performed the action check updates server successfully at the attempt #1
root@jenkins:~/dockerjenkinsdocker# docker update --restart unless-stopped $(docker ps -q)
bf4ce8a4f605
root@jenkins:~/dockerjenkinsdocker#
root@jenkins:~/dockerjenkinsdocker#
root@jenkins:~/dockerjenkinsdocker# docker ps
CONTAINER ID   IMAGE                   COMMAND                  CREATED         STATUS         PORTS                                                                                      NAMES
bf4ce8a4f605   docker-jenkins-docker   "/usr/bin/tini -- /u…"   8 minutes ago   Up 8 minutes   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp, 0.0.0.0:50000->50000/tcp, :::50000->50000/tcp   optimistic_fermat
root@jenkins:~/dockerjenkinsdocker# docker exec -it bf4ce8a4f605 bash
root@bf4ce8a4f605:/# ps -a
    PID TTY          TIME CMD
      7 pts/0    00:01:22 java
     14 pts/0    00:00:00 jenkins.sh
    311 pts/1    00:00:00 ps
root@bf4ce8a4f605:/# docker ps -a
CONTAINER ID   IMAGE                   COMMAND                  CREATED         STATUS         PORTS                                                                                      NAMES
bf4ce8a4f605   docker-jenkins-docker   "/usr/bin/tini -- /u…"   9 minutes ago   Up 9 minutes   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp, 0.0.0.0:50000->50000/tcp, :::50000->50000/tcp   optimistic_fermat
root@bf4ce8a4f605:/#
root@bf4ce8a4f605:/#
root@bf4ce8a4f605:/#
root@bf4ce8a4f605:/#
root@bf4ce8a4f605:/#
root@bf4ce8a4f605:/#
exit
root@jenkins:~/dockerjenkinsdocker# docker ps
CONTAINER ID   IMAGE                   COMMAND                  CREATED          STATUS          PORTS                                                                                      NAMES
bf4ce8a4f605   docker-jenkins-docker   "/usr/bin/tini -- /u…"   11 minutes ago   Up 11 minutes   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp, 0.0.0.0:50000->50000/tcp, :::50000->50000/tcp   optimistic_fermat
root@jenkins:~/dockerjenkinsdocker# docker ps
CONTAINER ID   IMAGE                   COMMAND                  CREATED          STATUS                  PORTS                                                                                      NAMES
bf4ce8a4f605   docker-jenkins-docker   "/usr/bin/tini -- /u…"   11 minutes ago   Up Less than a second   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp, 0.0.0.0:50000->50000/tcp, :::50000->50000/tcp   optimistic_fermat
root@jenkins:~/dockerjenkinsdocker# docker ps
CONTAINER ID   IMAGE                   COMMAND                  CREATED          STATUS          PORTS                                                                                      NAMES
bf4ce8a4f605   docker-jenkins-docker   "/usr/bin/tini -- /u…"   11 minutes ago   Up 14 seconds   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp, 0.0.0.0:50000->50000/tcp, :::50000->50000/tcp   optimistic_fermat
root@jenkins:~/dockerjenkinsdocker# docker ps
CONTAINER ID   IMAGE                   COMMAND                  CREATED          STATUS          PORTS                                                                                      NAMES
bf4ce8a4f605   docker-jenkins-docker   "/usr/bin/tini -- /u…"   11 minutes ago   Up 16 seconds   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp, 0.0.0.0:50000->50000/tcp, :::50000->50000/tcp   optimistic_fermat
root@jenkins:~/dockerjenkinsdocker#
root@jenkins:~/dockerjenkinsdocker# history
    1  vi setup_jenkins.sh
    2  chmod +x setup_jenkins.sh
    3  ./setup_jenkins.sh
    4  clear
    5  ip addr
    6  cat /var/lib/jenkins/secrets/initialAdminPassword
    7  cat /etc/os-release
    8  pwd
    9  ls -la /usr/lib/jvm/
   10  ls -la /usr/lib/jvm/java-11-openjdk-amd64/
   11  docker
   12  vi install_docker.sh
   13  chmod +x install_docker.sh
   14  ./install_docker.sh
   15  systemctl status docker
   16  docker -V
   17  docker -v
   18  sudo usermod -a -G root jenkins
   19  sudo service jenkins restart
   20  systemctl status jenkins
   21  sudo mkdir /Users/jenkins
   22  grep docker /etc/group
   23  sudo usermod -a -G docker jenkins
   24  sudo service jenkins restart
   25  history
   26  docker images
   27  curl http://localhost:4243/version
   28  systemctl status docker
   29  cat /lib/systemd/system/docker.service
   30  grep -i port /lib/systemd/system/docker.service
   31  node --version
   32  cd ..
   33  mkdir web-server
   34  cd web-server/
   35  vi Dockerfile
   36  vi deploy.sh
   37  chmod +x deploy.sh
   38  cat deploy.sh
   39  ./deploy.sh
   40  ls -la
   41  cat deploy.sh
   42  docker ps
   43  ifconfig
   44  cat deploy.sh
   45  docker images
   46  docker ps -a
   47  docker ps
   48  docker stop web2108
   53  docker remove hello-world
   54  docker remove d2c94e258dcb
   55  docker images
   56  docker remove hello-world
   57  docker rmi 19fd3afc77b1
   58  docker ps -a
   59  docker rm efd610c3cbab
   60  docker images
   61  docker rmi 19fd3afc77b1
   62  docker images
   63  docker rm 1ab4ac7b06e0c3963800582407bcd1678d45bda7
   64  docker rmi d2c94e258dcb
   65  docker rmi ae7f6c370720
   66  docker images
   67  docker rmi 19fd3afc77b1
   68  docke stop 1ab4ac7b06e0c3963800582407bcd1678d45bda7
   69  docker stop 1ab4ac7b06e0c3963800582407bcd1678d45bda7
   70  docker images
   71  docker ps -a
   72  docker rm we
   73  docker images -a
   74  docker rmi -f 19fd3afc77b1
   75  docker images
   76  docker ps -a
   77  docker images
   78  docker pull jenkins/jenkins
  107  docker pull jenkins/jenkins
  108  docker run -p 8080:8080 -p 50000:50000 jenkins/jenkins:latest
  109  docker images
  110  docker ps
  111  docker ps -a
  112  docker stop focused_turing
  113  docker rm 9cc41e666f61
  114  docker run -p 9988:9988 -p 50000:50000 jenkins/jenkins:latest
  115  docker r  ps -a
  116  docker ps
  117  docker ps -a
  118  docker stop charming_torvalds
  119  docker rm 3936279682f2
  120  docker run -p 9988:8080 -p 50000:50000 jenkins/jenkins:latest
  121  docker ps -a
  122  docker stop dazzling_albattani
  123  docker rm 1e43075b1ae3
  124  docker run -d -p 8080:8080 -p 50000:50000 jenkins/jenkins:latest
  125  docker ps
  126  docker ps -a
  127  docker log cf83212ef716
  128  docker logs cf83212ef716
  129  docker ps
  130  docker ps -a
  131  docker stop angry_carver
  132  docker rm cf83212ef7
  133  docker run -d -p 8080:8080 -p 50000:50000 jenkins/jenkins:latest
  134  docker ps
  135  docker logs 0aa94730c97f
  136  docker ps
  137  docker run -d -p 8080:8080 -p 50000:50000 jenkins/jenkins:latest
  138  docker ps
  139  docker ps -a
  140  docker logs a801176
  141  docker ps -a
  142  docker ps
  143  docker ps -a
  144  docker run -d -p 8080:8080 -p 50000:50000 jenkins/jenkins:latest
  145  docker ps
  146  docker logs 21c5913e6a77
  147  docker ps
  148  docker run -d -p 8080:8080 -p 50000:50000 jenkins/jenkins:latest
  149  docker ps
  150  docker ps
  151  docker logs
  152  docker logs ec59914d31cd
  153  docker update --restart unless-stopped $(docker ps -q)
  154  docker ps
  155  docker ps -a
  156  docker stop agitated_rhodes
  157  docker stop peaceful_leavitt
  158  docker stop sweet_spence
  159  docker stop nifty_jones
  160  docker ps
  161  docker ps -a
  162  docker ps
  163  docker update --restart unless-stopped $(docker ps -q)
  164  docker run -d -p 8080:8080 -p 50000:50000 jenkins/jenkins:latest
  165  docker ps
  166  docker logs 5d7c99bcea76
  167  docker ps
  168  docker update --restart unless-stopped $(docker ps -q)
  169  docker run -d -p 8080:8080 -p 50000:50000 jenkins/jenkins:latest
  170  docker update --restart unless-stopped $(docker ps -q)
  171  docker ps
  172  docker logs e1d8cfc717f4
  173  docker update --restart unless-stopped $(docker ps -q)
  174  docker logs e1d8cfc717f4
  175  clear
  176  docker ps
  177  docker logs e1d8cfc717f4
  178  docker ps
  201  docker images
  212  docker -v
  213  docker exec -it 8cf020221b00 docker -v
  214  docker exec -it 8cf020221b00 whereis ps
  215  docker exec -it 8cf020221b00 whereis docker
  216  docker exec -it 8cf020221b00 which docker
  217  docker exec -it 8cf020221b00 which ps
  218  docker exec -it 8cf020221b00 cat /etc/os-release
  219  docker exec -it 8cf020221b00 apt-get update
  220  docker exec -it 8cf020221b00 sudo apt-get update
  221  docker images
  224  docker run -it --entrypoint /bin/bash ls
  225  docker run -it --entrypoint /bin/bash
  226  docker ps
  227  docker exec -it 8cf020221b00 /bin/bash
  228  pwd
  229  ls -ls
  234  docker ps
  235  docker stop stupefied_hugle
  236  docker rm 8cf020221b00
  237  cat Dockerfile
  238  docker run -d -it -p 8080:8080 -p 50000:50000 docker-jenkins-docker
  239  docker update --restart unless-stopped $(docker ps -q)
  240  docker exec -it 60ba6dffba51 bash
  241  docker exec -it 60ba6dffba51 ps -a | grep docker
  242  docker exec -it 60ba6dffba51 ps -a
  243  docker ps
  244  docker stop nice_nightingale
  245  docker rm 60ba6dffba51
  246  docker run -d -it -p 8080:8080 -p 50000:50000 -v /var/run/docker.sock:/var/run/docker.sock docker-jenkins-docker
  247  docker update --restart unless-stopped $(docker ps -q)
  248  docker exec -it bf4ce8a4f605 bash
  249  docker ps
  250  cat /var/jenkins_home/secrets/initialAdminPassword
  251  ls -la
  252  cd dockerjenkinsdocker/
  253  ls -la
  254  docker ps
  255  docker logs bf4ce8a4f605
  256  docker update --restart unless-stopped $(docker ps -q)
  257  docker ps
  258  docker exec -it bf4ce8a4f605 bash
  259  docker ps
  260  history
root@jenkins:~/dockerjenkinsdocker#

