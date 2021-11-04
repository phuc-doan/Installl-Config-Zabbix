# **`==== Zabbix manual installation ====`**

**Author:** *Doan Van Phuc* 😎😎

**Date of issue**: *Nov 3th 2021*🎞🎞

## Step 1: `Preparing:`🔑🔑
```
Zabbix Version:
    4.0 LTS 
	  OS
		CenOS 7 
	  DataBase:
    MySQL
```
The First, Run this command to stop **firewalld & Selinux**
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
## Step 2: When server reboot has done, We'll load some packet install **`Zabbbix version 4.0`**

- Repo setup:
- To load file, use **`rpm`**
```
  rpm -Uvh https://repo.zabbix.com/zabbix/4.0/rhel/7/x86_64/zabbix-release-4.0-2.el7.noarch.rpm
	yum search zabbix
	yum install zabbix-get zabbix-agent zabbix-server-mysql zabbix-web-mysql mariadb-server -y
```

![1 (2)](https://user-images.githubusercontent.com/83824403/140336458-8d748f8b-fdc7-453a-b67b-bdf5b633398a.png)

 
 
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


![3 (2)](https://user-images.githubusercontent.com/83824403/140336625-2cdb277d-51d2-49cc-a23d-135b0d85009c.png)


```
cd /usr/share/doc/zabbix-server-mysql-4.0.35/
ls
```

![4 (2)](https://user-images.githubusercontent.com/83824403/140336734-98f0277a-0094-46b3-aec0-8f4ca460a917.png)



### Express file **`create.sql.gz`**
zcat create.sql.gz | mysql zabbix                   #with zabbix = user 
```
mysql
	use zabbix;
	show tables;
	exit;
```
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
```
systemctl start zabbix-server	
systemctl enable zabbix-server
less /var/log/zabbix/zabbix-server.log
```

![Zabbix success (2)](https://user-images.githubusercontent.com/83824403/140336851-207c185f-8dd3-450e-9164-9baf5c315bb4.png)


### We're done, Now enjoy it 😍😍


