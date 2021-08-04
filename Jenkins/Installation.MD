# Install Jenkins on AWS EC2
Jenkins is a self-contained Java-based program, ready to run out-of-the-box, with packages for Windows, Mac OS X and other Unix-like operating systems. As an extensible automation server, Jenkins can be used as a simple CI server or turned into the continuous delivery hub for any project.

### Follow this article in **[Youtube](https://youtu.be/M32O4Yv0ANc)**

### Prerequisites
1. EC2 RHEL 7.x Instance [Get help here](https://www.youtube.com/watch?v=KDtS6BzJo3A)
   - With Internet Access
   - Security Group with Port `8080` open for internet
1. Java v1.8.x 

## Install Java
We will be using open java for our demo, Get latest version from http://openjdk.java.net/install/
```sh
yum install java-1.8*
#yum -y install java-1.8.0-openjdk
```

### Confirm Java Version
Lets install java and set the java home
```sh
java -version
find /usr/lib/jvm/java-1.8* | head -n 3
#JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.191.b12-1.el7_6.x86_64
export JAVA_HOME
PATH=$PATH:$JAVA_HOME
#After setting java path

[root@ip-172-31-29-97 ~]# cat ~/.bash_profile
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.292.b10-1.el7_9.x86_64
export JAVA_HOME
PATH=$PATH:$JAVA_HOME:$HOME

export PATH
[root@ip-172-31-29-97 ~]#

# To set it permanently update your .bash_profile
source ~/.bash_profile
```
_The output should be something like this,_
```
[root@~]# java -version
openjdk version "1.8.0_151"
OpenJDK Runtime Environment (build 1.8.0_151-b12)
OpenJDK 64-Bit Server VM (build 25.151-b12, mixed mode)
```

## Install Jenkins
You can install jenkins using the rpm or by setting up the repo. We will setup the repo so that we can update it easily in future.
Get latest version of jenkins from https://pkg.jenkins.io/redhat-stable/
```sh
yum -y install wget
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
 sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
yum -y install jenkins
```

### Start Jenkins
```sh
# Start jenkins service
systemctl start jenkins

# Setup Jenkins to start at boot,
systemctl enable jenkins
```

#### Accessing Jenkins
By default jenkins runs at port `8080`, You can access jenkins at
```sh
http://YOUR-SERVER-PUBLIC-IP:8080
```
#### Configure Jenkins
- The default Username is `admin`
- Grab the default password 
  - Password Location:`/var/lib/jenkins/secrets/initialAdminPassword`
- `Skip` Plugin Installation; _We can do it later_
- Change admin password
  - `Admin` > `Configure` > `Password`
- Configure `java` path
  - `Manage Jenkins` > `Global Tool Configuration` > `JDK`  
- Create another admin user id

### Test Jenkins Jobs
1. Create “new item”
1. Enter an item name – `My-First-Project`
   - Chose `Freestyle` project
1. Under Build section
	Execute shell : echo "Welcome to Jenkins Demo"
1. Save your job 
1. Build job
1. Check "console output"

### Next Step
- [x] [Configure Users & Groups in Jenkins](https://youtu.be/jZOqcB32dYM)
- [x] [Secure your Jenkins Server](https://youtu.be/19FmJumnkDc)
- [x] [Jenkins Plugin Installation](https://youtu.be/p_PqPBbjaZ4)
- [x] [Jenkins Master-Slave Configuration](https://youtu.be/hwrYURP4O2k)
- [ ] Setup Jenkins to run inside Tomcat Server

#  Install & configure Maven build tool on Jenkins
Maven is a code build tool which used to convert your code to artifact. this is widely used plugin to build in continuous integration

#### Follow this artical in **[YouTube](https://www.youtube.com/watch?v=wgfsVmHnAiM)**

#### Prerequisites
1. Jenkins server **[Get Help Here](https://www.youtube.com/watch?v=M32O4Yv0ANc)

#### Install Maven on Jenkins
Download maven packages https://maven.apache.org/download.cgi onto Jenkins server. In this case I am using /opt/maven as my installation directory
	- Link : https://maven.apache.org/download.cgi
```sh
  # Creating maven directory under /opt
  mkdir /opt/maven
  cd /opt/maven
  # downloading maven version 3.6.0
  wget https://mirrors.estointernet.in/apache/maven/maven-3/3.8.1/binaries/apache-maven-3.8.1-bin.tar.gz
  tar zvxf apache-maven-3.8.1-bin.tar.gz
  cd apache-maven-3.8.1
  ls
  cd bin
  ./mvn -version
  [root@ip-172-31-29-97 bin]# ./mvn -version
Apache Maven 3.8.1 (05c21c65bdfed0f71a2f2ada8b84da59348c4c5d)
Maven home: /opt/maven/apache-maven-3.8.1
Java version: 1.8.0_292, vendor: Red Hat, Inc., runtime: /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.292.b10-1.el7_9.x86_64/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "3.10.0-1062.12.1.el7.x86_64", arch: "amd64", family: "unix"
env
export PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin:/opt/maven/apache-maven-3.8.1/bin
echo $PATH
mvn -version

  
 ```
