1. AWS EC2 console
Make Key Pair only accessible to the owner
```sh
  chmod 400 DevOps-Key-Pair.pem
```
```sh
  ssh -i "DevOps-Key-Pair.pem" ec2-user@ec2-54-252-226-192.ap-southeast-2.compute.amazonaws.com
  ```
````shell
  sudo su -
````
2. Install Jenkins on AWS EC2
```sh

  sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
  sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
```
```sh
  amazon-linux-extras
  amazon-linux-extras install epel
  amazon-linux-extras install java-openjdk11
```
```shell
  yum install fontconfig java-17-openjdk
  yum install jenkins
```
3. Start Jenkins
```sh
  service jenkins status
  service jenkins start
```
4. Open Jenkins in browser
```sh
  http://<Public-IP>:8080
```
5. Unlock Jenkins
```sh
  cat /var/lib/jenkins/secrets/initialAdminPassword
```
6. Install git
```sh
  yum install git
```
7. Install Maven
```sh
  cd /opt
  wget https://dlcdn.apache.org/maven/maven-3/3.9.8/binaries/apache-maven-3.9.8-bin.tar.gz
  tar -xvzf apache-maven-3.9.8-bin.tar.gz
  mv apache-maven-3.9.8 maven
```
8. Set Maven environment variables
```sh
    cd ~
    vi .bash_profile
```
type "i" to insert mode
```sh
    fi
    M2_HOME=/opt/maven
    M2=/opt/maven/bin
    JAVA_HOME=/usr/lib/jvm/java-11-openjdk-11.0.23.0.9-2.amzn2.0.1.x86_64
    # User specific environment and startup programs
    
    PATH=$PATH:$HOME/bin:$JAVA_HOME:$M2_HOME:$M2
    
    export PATH
```
type "esc + enter" to exit insert mode
```sh
  :wq
```
8-1. How to find the path of Java
```sh
  find / -name java-11*
```
10. Reload the .base_profile
```sh
  source .bash_profile
```
11. Restart Jenkins
```sh
  systemctl start jenkins
```