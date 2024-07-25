1. Connect to AWS EC2 instance
   Make Key Pair only accessible to the owner
```sh
  chmod 400 DevOps-Key-Pair.pem
```
```sh
ssh -i "DevOps-Key-Pair.pem" ec2-user@ec2-13-211-141-140.ap-southeast-2.compute.amazonaws.com
```
```shell
sudo su -
``` 

2. Install Docker
```sh
yum install docker
```
```sh
service docker status
service docker start
```

3. Start Tomcat
```sh
docker pull tomcat
docker run -d --name tomcat-container -p 8081:8080 tomcat
```
3-1. Login to tomcat container
```sh
docker exec -it tomcat-container /bin/bash
```
```sh
cd /usr/local/tomcat/webapps.dist
cp -R * ../webapps/
```

4. Tomcat Dockerfile
```sh
vi Dockerfile
```
```sh
FROM centos:latest 
RUN cd /etc/yum.repos.d/
RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
RUN yum -y install java
RUN mkdir /opt/tomcat/
WORKDIR /opt/tomcat
ADD https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.91/bin/apache-tomcat-9.0.91.tar.gz /opt/tomcat
RUN tar xvfz apache*.tar.gz
RUN mv apache-tomcat-9.0.91/* /opt/tomcat 
EXPOSE 8080
CMD ["/opt/tomcat/bin/catalina.sh", "run"]
```
```sh
docker build -t mytomcat .
docker run -d --name mytomcat-server -p 8083:8080 mytomcat
```

5. Customized Tomcat Dockerfile
```sh
vi Dockerfile
```
```sh
FROM tomcat:latest
RUN cp -R /usr/local/tomcat/webapps.dist/* /usr/local/tomcat/webapps/
```
```sh
docker build -t demotomcat .
docker run -d --name demotomcat-container -p 8085:8080 demotomcat 
```