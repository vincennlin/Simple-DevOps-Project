1. Create user
````sh
    cat /etc/passwd
    cat /etc/group
    useradd dockeradmin
    passwd dockeradmin
````
new password: dockerpassw0rd
```sh
    usermod -aG docker dockeradmin
    id dockeradmin
```

2. Enable password authentication in sshd_config
````sh
    vi /etc/ssh/sshd_config
    PasswordAuthentication yes
    #PasswordAuthentication no
    service sshd reload
````
````shell
    whoami
    sudo su - dockeradmin
````
Generate key
````sh
    ssh-keygen
    ls -l ~/.ssh
    cat ~/.ssh/id_rsa.pub
````

3. Create directory for Docker
Login as root first
````sh
    sudo su -
    cd /opt
    mkdir docker
    ll
    chown -R dockeradmin:dockeradmin docker
    ll
````
````shell
    cd /root
    mv Dockerfile /opt/docker
    cd /opt/docker
    chown -R dockeradmin:dockeradmin Dockerfile
````

4. Then update in Jenkins: 
Configuration -> Post-build Actions -> Send build artifacts over SSH -> Transfers -> Transfer set -> Remote directory: `//opt//docker`
````shell
    vi Dockerfile 
````
````Dockerfile
FROM tomcat:latest
RUN cp -R /usr/local/tomcat/webapps.dist/* /usr/local/tomcat/webapps/
COPY ./*.war /usr/local/tomcat/webapps
````
````shell
    docker build -t tomcat:v1 .
    docker images
    docker run -d --name tomcatv1 -p 8086:8080 tomcat:v1
    docker ps
````

5. "Exec command" field in Configure in Jenkins Job
````sh
    cd /opt/docker;
    docker build -t regapp:v1 .;
    docker stop registerapp;
    docker rm registerapp;
    docker run -d --name registerapp -p 8087:8080 regapp:v1;
````