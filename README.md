# Running Jenkins system

### 1. Start ***Jenkins system*** by docker image

<code> docker run -d **--name=jenkins-system** **-p 8080:8080** -p 50000:50000 -v ~/Jenkins/jenkins_mount_folder:/var/jenkins_home **jenkins/jenkins:lts** </code>

##### Refer guide: 
- https://www.jenkins.io/doc/book/installing/docker/
- https://hub.docker.com/r/jenkins/jenkins


### 2. Start ***Jenkins Docker Agent Node*** by docker image

Step by step:
- Create Jenkins network and join the Jenkins system's container:

    + Create jenkins network: 
    <code> docker network create jenkins </code> 

    + Join Jenkins System to jenkins network: 
    <code> docker network connect jenkins jenkins-system </code> 

    + Check status of jenkins network: 
    <code> docker network inspect jenkins</code> 

- Execute the jenkins-system container and create pair of key to connect with agent node:

##### Refer guide: 
- https://www.jenkins.io/doc/book/installing/docker/


