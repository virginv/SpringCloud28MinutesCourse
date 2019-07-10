# spring-cloud-config-server

## URL services
```
http://localhost:8888/limits-service/dev 

http://localhost:8080/limits 

http://localhost:8000/currency-exchange/from/USD/to/BS 

http://localhost:8001/currency-exchange/from/USD/to/MNX 
```
## URL Database H2 Console
```
http://localhost:8000/h2-console 
```
## URL without ribbon
```
http://localhost:8100/currency-converter/from/USD/to/MNX/quantity/10
```
## URL with ribbon
```
http://localhost:8100/currency-converter-feign/from/USD/to/MNX/quantity/10 
```
## URL Eureka 
```
http://localhost:8761
```

## URLs throught Zuul --> http://localhost:8765/{application-name}/uri
```
http://localhost:8765/currency-exchange-service/currency-exchange/from/USD/to/BS
http://localhost:8765/currency-conversion-service/currency-converter-feign/from/USD/to/MNX/quantity/10
```

## URL rabbitmq
```
http://localhost:15672
```

## URL zipkin
```
http://localhost:9411/zipkin/
```
** Add Eureka in Zipkin**

In order to trace Eureka naming server in Zipkin, add below zipkin dependencies in Eureka naming server microservice in pom.xml 
and run all the instances, currency-exchange-service, currency-conversion-service, Eurekanamingserver, ZuulApiGateway and ALSO RabbitMQ and Zipkin jar.

```
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-sleuth</artifactId>
</dependency>
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-zipkin</artifactId>
</dependency>
```


** library FOR RABBIN **

```
<dependency>
	<groupId>org.springframework.amqp</groupId>
	<artifactId>spring-rabbit</artifactId>
</dependency>
```


## Actuator Refresh
** By default, all endpoints except for shutdown are enabled Include  all endpoints **

```
management.endpoints.web.exposure.include=*
http://localhost:8080/actuator/refresh
http://localhost:8080/actuator/bus-refresh
```

# HOW TO INSTALL RabbitMQ (UBUNTU 18.04)

RabbitMQ requires Erlang to be installed first before it can run. 

## Install Erlang:

Erlang is a programming language used to build massively scalable soft real-time systems with requirements on high availability

* **Step 1** – Adding Repository

```
wget https://packages.erlang-solutions.com/erlang-solutions_1.0_all.deb
sudo dpkg -i erlang-solutions_1.0_all.deb
```
* **Step 2** – Install Erlang on Ubuntu

```
sudo apt-get update
sudo apt-get install erlang
```

## Install RabbitMQ:

* **Step 1** – Adding Repository

```
wget -O- https://dl.bintray.com/rabbitmq/Keys/rabbitmq-release-signing-key.asc | sudo apt-key add -
wget -O- https://www.rabbitmq.com/rabbitmq-release-signing-key.asc | sudo apt-key add -
echo "deb https://dl.bintray.com/rabbitmq/debian $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/rabbitmq.list
```

* **Step 2**: Install RabbitMQ Server

```
sudo apt update
sudo apt -y install rabbitmq-server
```

** After installation, RabbitMQ service is started and enabled to start on boot. To check the status, run:**

```
sudo systemctl status  rabbitmq-server.service 
```

** Check status:**

```
systemctl is-enabled rabbitmq-server.service
```

** Enable service:**

```
sudo systemctl enable rabbitmq-server
```

* **Step 3**: Enable the RabbitMQ Management Dashboard (Optional): The Web service should be listening on TCP port 15672

```
sudo rabbitmq-plugins enable rabbitmq_management
```

** If you have an active UFW firewall, open both ports 5672 and 15672**

```
sudo ufw allow proto tcp from any to any port 5672,15672
```

**By default, the guest user exists and can connect only from localhost. You can login with this user locally with the password “guest”
To be able to login on the network, create an admin user like below:**

```
sudo rabbitmqctl add_user admin StrongPassword
sudo rabbitmqctl set_user_tags admin administrator
```

** RabbitMQ User Management Commands**

```
rabbitmqctl delete_user user
rabbitmqctl change_password user strongpassword
```

## HOW TO INSTALL Zipkin (UBUNTU 18.04)

* **Step 1**: Fetch the latest release of Zipkin as a self-contained executable jar

```
curl -sSL https://zipkin.apache.org/quickstart.sh | bash -s
```

* **Step 2**: To run Zipkin, just execute the command:

```
java -jar zipkin.jar
```

** Start Zipkin with rabbitmq (/home/virginia/zipkin.jar)**

```
RABBIT_URI=amqp://localhost java -jar zipkin.jar    
```

* **Step 3**: Configure Systemd
Running Zipkin with the java -jar command will not persist system reboots. If your system has support for systemd, you can create service for it.

** Move jar file to /opt directory.**

```
sudo mkdir /opt/zipkin
sudo mv zipkin.jar /opt/zipkin
ls /opt/zipkin
```
** Start by creating a system group for the user:**

```
sudo groupadd -r zipkin
sudo useradd -r -s /bin/false -g zipkin zipkin
sudo chown -R zipkin:zipkin /opt/zipkin
```
** Create Systemd Service**

```
sudo vim /etc/systemd/system/zipkin.service
```

** Memory limits can be set where -Xms128m and -Xmx256m are used to set the minimum and maximum memory that the application can use.**

```
ExecStart=/bin/java -Xms128m -Xmx256m -jar zipkin.jar
```

** Inform systemd about the new service addition.**

```
sudo systemctl daemon-reload
```
** Once reloaded, start the service**

```
sudo systemctl start zipkin.service
```
** Check the status:**

```
sudo systemctl status zipkin.service
```

# git

git remote add origin https://github.com/virginv/spring-cloud.git

git@github.com:virginv/spring-cloud.git

https://github.com/virginv/spring-cloud

git remote add origin https://github.com/virginv/spring-cloud-config-server.git

git remote add origin https://github.com/virginv/git-localconfig-repo.git

git push -u origin master
