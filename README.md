# Running Jenkins system

### 1. Start ***Jenkins system*** by docker image

<code> docker run -d **--name=jenkins-system** **-p 8080:8080** -p 50000:50000 -v ~/Jenkins/jenkins_mount_folder:/var/jenkins_home **jenkins/jenkins:lts** </code>

##### Refer guide: 
- https://www.jenkins.io/doc/book/installing/docker/
- https://hub.docker.com/r/jenkins/jenkins


### 2. Start ***Jenkins Docker Agent Node*** by docker image

- 2.1. Create Jenkins network and join the Jenkins system's container:

    + 2.1.1. Create jenkins network: 
    <code> docker network create jenkins </code> 

    + 2.1.2. Join Jenkins System to jenkins network: 
    <code> docker network connect jenkins jenkins-system </code> 

    + 2.1.3. Check status of connection: 
    <code> docker network inspect jenkins</code> 

- 2.2. Execute the jenkins-system container and create pair of key to connect with agent node:

    + 2.2.1. Execute to jenkins-system container with non-root user (user: jenkins):
    <code> docker exec -it jenkins-system /bin/bash </code>

    + 2.2.2. Generate key pair to authenticate Jenkins system with another:
    <code> ssh-keygen -f ~/{jenkins-home-path}/.ssh/jenkins_agent_key </code>

    + 2.2.3. Assign generated private key as a system credential in Jenkins UI System. (Refer: https://www.jenkins.io/doc/book/using/using-agents/#create-a-jenkins-ssh-credential)

- 2.3. Starting Docker Agent: 

    + 2.3.1. Create Docker Agent with Jenkins generated public key: 
    <code> docker run -d --name=agent-docker -p 22:22 -e "JENKINS_AGENT_SSH_PUBKEY={Jenkins’s generated public key}" -v "/var/run/docker.sock:/var/run/docker.sock:rw" lamlam13/jenkins-ssh-agent-docker:alpine-jdk21 </code> 

    + 2.3.2. Join Docker Agent container to jenkins network:
    <code> docker network jenkins agent-docker </code>

    + 2.3.3. Check status of connection:
    <code> docker network inspect jenkins</code> 

- 2.4. Setup Agent on Jenkins UI:
    + Refer https://www.jenkins.io/doc/book/using/using-agents/#setup-up-the-agent1-on-jenkins
    + This connection's criteia should be follow this guide:
        <p>Host: Defined agent-docker host on step 2.3.3.</p>
        <p>Credentials: Credential which added on step 2.2.3. </p> 

##### Refer guide: 
- https://www.jenkins.io/doc/book/installing/docker/
- https://www.jenkins.io/doc/book/using/using-agents/


