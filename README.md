# Jenkins Deployment
### Deployment on CentOS 7
Note: I'm using virtual box 
#### Pre-requistes
1. Install CentOS 7
2. Configure forwarding port to access jenkins and ssh from host machine.

```sh
sudo yum -y update
sudo yum -y install nano
sudo yum -y install wget
```
##### Check firewall state
```sh
sudo firewall-cmd --state
```
##### Enable remote ssh
```sh
sudo nano /etc/ssh/sshd_config
```
uncomment PermitRootLogin Yes
##### Install network tools
```sh
sudo yum -y install net-tools
nmcli d
nmtui edit enp0s8
```
##### Install and config Java 11 SDK
```sh
sudo yum install java-11-openjdk
java -version
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-11.0.20.0.8-1.el7_9.x86_64/jre
```

#### Install and configure Jenkins
```sh
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo

sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key

sudo yum install jenkins

sudo systemctl enable jenkins

sudo systemctl start jenkins

sudo systemctl status jenkins
```
Error: Public key for jenkins-2.414.1-1.1.noarch.rpm is not installed

Solution:
```sh
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
```

##### Configure firewall
```sh
[root@localhost /] YOURPORT=8080
[root@localhost /] PERM="--permanent"
[root@localhost /] SERV="$PERM --service=jenkins"

firewall-cmd $SERV --set-short="Jenkins ports"
firewall-cmd $SERV --set-description="Jenkins port exceptions"
firewall-cmd $SERV --add-port=$YOURPORT/tcp
firewall-cmd $PERM --add-service=jenkins
firewall-cmd --zone=public --add-service=http --permanent
firewall-cmd --reload
```

##### To get jenkins initial password
```sh
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```