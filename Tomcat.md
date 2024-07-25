1. Connect to AWS EC2 instance
   Make Key Pair only accessible to the owner
```sh
  chmod 400 DevOps-Key-Pair.pem
```
```sh
ssh -i "DevOps-Key-Pair.pem" ec2-user@ec2-13-236-86-229.ap-southeast-2.compute.amazonaws.com
```
```shell
sudo su -
```

2. Install Java 11
```sh
amazon-linux-extras install java-openjdk11
java -version
```
3. Download Tomcat
```sh
cd /opt
wget https://downloads.apache.org/tomcat/tomcat-9/v9.0.91/bin/apache-tomcat-9.0.91.tar.gz
tar -xvzf apache-tomcat-9.0.91.tar.gz
mv apache-tomcat-9.0.91 tomcat
```

4. Start Tomcat
```sh
cd /opt/tomcat/bin
./startup.sh
```

5. Access Tomcat
```sh
http://<Public-IP>:8080
```
```sh
vi /opt/tomcat/webapps/host-manager/META-INF/context.xml
vi /opt/tomcat/webapps/manager/META-INF/context.xml
```
```sh
  <!--  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
  allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> -->
```
```sh
cd /opt/tomcat/bin
./shutdown.sh
./startup.sh
```
```sh
vi /opt/tomcat/conf/tomcat-users.xml
```
```sh
	<role rolename="manager-gui"/>
	<role rolename="manager-script"/>
	<role rolename="manager-jmx"/>
	<role rolename="manager-status"/>
	<user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status"/>
	<user username="deployer" password="deployer" roles="manager-script"/>
	<user username="tomcat" password="s3cret" roles="manager-gui"/>
```
```sh
ln -s /opt/tomcat/bin/startup.sh /usr/local/bin/tomcatup
ln -s /opt/tomcat/bin/shutdown.sh /usr/local/bin/tomcatdown
tomcatdown
tomcatup
```
```sh
	<user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status"/>
	<user username="deployer" password="deployer" roles="manager-script"/>
	<user username="tomcat" password="s3cret" roles="manager-gui"/>
```