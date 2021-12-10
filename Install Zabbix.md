# **`==== Zabbix manual installation ====`**

**Zabbix Server and Zabbix Agent**

**Author:** *Doan Van Phuc* 😎😎

**Date of issue**: *Nov 3th 2021*🎞🎞


# INSTALL ZABBIX SERVER

## Step 1: `Preparing:`🔑🔑
```
Zabbix Version:
    5.0 LTS 
	  OS
		CenOS 7 
	  DataBase:
    MySQL
```
The First,Run this command to stop **firewalld & Selinux**
```

systemctl stop firewalld
systemctl disable firewalld
```
And then, we have to edit Selinux file config, most important we need take note this is change **setenforce** to **disable**

- To check, use command:

```
sestatus

vi /etc/selinux/config

reboot
```

## Step 2: When server reboot has done, We'll load some packet install **`Zabbbix version 5.0`**

- Repo setup:
- To load file, use **`rpm`**
```
 # rpm -Uvh https://repo.zabbix.com/zabbix/5.0/rhel/7/x86_64/zabbix-release-5.0-1.el7.noarch.rpm
# yum clean all
	yum install zabbix-get zabbix-agent zabbix-server-mysql zabbix-web-mysql mariadb-server -y
```

![1 (2)](https://user-images.githubusercontent.com/83824403/140336458-8d748f8b-fdc7-453a-b67b-bdf5b633398a.png)

 
  ## Install Zabbix frontend
Documentation
Enable Red Hat Software Collections
```
# yum install centos-release-scl
```
 
-  Edit file /etc/yum.repos.d/zabbix.repo and enable zabbix-frontend repository.
```
[zabbix-frontend]
...
enabled=1
...
```

- Install Zabbix frontend packages.

```
# yum install zabbix-web-mysql-scl zabbix-apache-conf-scl
```

- Install Zabbix frontend


```
# yum install centos-release-scl
```
## Step 3🛠: When all done, we restart and enable *`Mariadb`*
```
systemctl start mariadb
systemctl enable mariadb
```
## Step 4: Check Mysql
```
mysql
	create database zabbix character set utf8 collate utf8_bin;
	grant all privileges on zabbix.* to zabbix@localhost identified by '1';
show databases;
```

 ![2 (2)](https://user-images.githubusercontent.com/83824403/140336563-87f35ff5-e04d-4f44-b8af-397ebfe5d899.png)

 
 
- Show database is command helpfull, it can show all database we installed 
- Save **`PassWord`**, In next step we need it 🤣🤣



- On Zabbix server host import initial schema and data. You will be prompted to enter **`your newly created password`**.

```
# zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -uzabbix -p zabbix
```

![4 (2)](https://user-images.githubusercontent.com/83824403/140336734-98f0277a-0094-46b3-aec0-8f4ca460a917.png)



## Step 5: Go to file Zabbix Config, And take note some point 🤗🤗
```
vi /etc/zabbix/zabbix_server.conf
	search:
		DBName=zabbix
		DBUser=zabbix
	insert:
		DBPassword=1
```


![6 (2)](https://user-images.githubusercontent.com/83824403/140336792-ef04fd6c-873e-4cb4-8dd1-2e7938705983.png)





### We havve to replace timezone to **`Asia/Ho_Chi_Minh`**

- By the way, We can access file Zabbix.conf in to link **/etc/httpd/conf.d/zabbix.conf**
```
vi /etc/httpd/conf.d/zabbix.conf
	Asia/Ho_Chi_Minh
```
## Last Step, Restart some service as Zabbix, Httpd, or check log's Zabbix

- Start Zabbix server and agent processes
- Start Zabbix server and agent processes and make it start at system boot.
```
# systemctl restart zabbix-server zabbix-agent httpd rh-php72-php-fpm
# systemctl enable zabbix-server zabbix-agent httpd rh-php72-php-fpm
```

![Zabbix success (2)](https://user-images.githubusercontent.com/83824403/140336851-207c185f-8dd3-450e-9164-9baf5c315bb4.png)




# INSTALL ZABBIX AGENT


#### ` in this instance, i create two agent: Linux centos 7 and Windows. We can use zabbix server to monitor agents `



## 1) Install zabbix agent in centos 7:



 *below I will guide to install Zabbix-Agent version 4.4 on CentOS 7 platform. To practice this tutorial, we are required to successfully install Zabbix Server version 4.4*



## Step 1: Install zabbix agent with rpm:



```
rpm -Uvh https://repo.zabbix.com/zabbix/4.4/rhel/7/x86_64/zabbix-agent-4.4.0-1.el7.x86_64.rpm
```


## Step 2: Install Zabbix-agent: 
🚗🚗

```
yum install zabbix-agent -y
```


## Step 3: Fix some Items in Zabbix-Agent:

 - We config this file conf, the parameters below /etc/zabbix/zabbix_agentd.conf 


```
Server=<IP_ZABBIX_SERVER>
ServerActive=<IP_ZABBIX_SERVER>
Hostname=<ZABBIX_SERVER_HOSTNAME>
```

- *According to my post, the specific parameters are the agent's ip address, please correct them according to the parameters that the server can see it to connect*

```
Server=192.168.1.67
ServerActive=192.168.1.67
Hostname=zabbix-server-lab
```


![Untitled (7)](https://user-images.githubusercontent.com/83824403/141446894-2ff8fcbe-6ccb-4b73-b261-ee7296f239c9.png)


## Step 4: Config firewalld 🔑🔑
 
 **`Use this command`**
 
```
firewall-cmd --zone=public --add-port=10050/tcp --permanent
firewall-cmd --reload
```


## Step 5: Restart service

```
systemctl enable zabbix-agent
systemctl restart zabbix-agent
```


![Untitled (6)](https://user-images.githubusercontent.com/83824403/141446612-5f35cf0c-3757-49e7-a97d-f91d48829497.png)


- *And done, Re-testing is absolutely necessary for the system to function properly*


```
zabbix_get -s <ZABBIX_AGENT_IP> -k agent.version
```
- follow my this zabbix agent ✔✔

```
zabbix_get -s 192.168.1.67 -k agent.version
```
-  if the output shows what is below it is almost successful

``
4.4.0
``

## Install zabbix agent Windowns

**Similar with zabbix agent in linux, or you can refer this link :https://www.zabbix.com/documentation/1.8/manual/processes/zabbix_agentd_win***

- *if same it bellow, that means we have successfully installed it. But must type correctly server's address in zabbix agent box ^ ^*🌹🌹


![Untitled (8)](https://user-images.githubusercontent.com/83824403/141447107-a2c4536a-48bf-41c0-9c6d-b9615f5eaf39.png)



## Lastest Step: Access to Server and Add agents into server

😎😎

- go to the server select configuration section, from there you can add the monitoring features you want

-  for example monitoring the in and out of the agent's network card or setting up alert. 
  
-  But before we do that we need to note that: the address of the agent and the server must always be entered correctly. ❤❤
 
-  When the words ZBX appear green, the agent and server have successfully 🤝🤝 shaken hands







**like this picture:**




![Untitled (9)](https://user-images.githubusercontent.com/83824403/141448361-05d36078-6e96-41a1-9dd3-41d13e7f9c94.png)








**And we monitor some zabbix-agent'parameters**







![Untitled (10)](https://user-images.githubusercontent.com/83824403/141448814-10542ff4-9472-4a17-8a26-3c5f5ab9d7c1.png)



### We're done, Now enjoy it 😍😍

*`And in the next post in my repository, I will do a lab that connects zabbix and grafana for easy monitoring, if possible I will add Promethus`*
