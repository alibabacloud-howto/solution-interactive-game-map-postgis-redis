# Game of Thrones

## Overview

### A Game of Maps

An interactive "Game of Thrones" map powered by Leaflet, PostGIS, and Redis.

## Index

- Step 1: Use Terraform to create resources
- Step 2: RDS Postgresql database
- Step 3: Install Nodejs
- Step 4: install the sample project

## 1. Use Terraform to create resources

### 1.1 Install Terraform

Run the following command to update the apt installation source. (This experiment uses the Ubuntu 20.04 system)

```
apt update
```

![image desc](https://labex.io/upload/Y/D/L/rztUc4BBLFXM.png)

Run the following command to install the unpacking tool:

```
apt install -y unzip zip
```

![image desc](https://labex.io/upload/S/F/D/3MrxW06YQ113.png)

Run the following command to download the Terraform installation package:

```
wget http://labex-ali-data.oss-us-west-1.aliyuncs.com/terraform/terraform_0.14.6_linux_amd64.zip
```

![image desc](https://labex.io/upload/O/E/T/h9fs71HhZYat.png)

Run the following command to unpack the Terraform installation package to /usr/local/bin:

```
unzip terraform_0.14.6_linux_amd64.zip -d /usr/local/bin/
```

![image desc](https://labex.io/upload/O/H/H/iGN8qs6pi0tm.png)

### 1.2 Create Resources

Refer back to the user's home directory as shown below, click AccessKey Management.

![image desc](https://labex.io/upload/G/I/B/BUGx6Dcu78BY.jpg)

Click on Create AccessKey. After AccessKey has been created successfully, AccessKeyID and AccessKeySecret are displayed. AccessKeySecret is only displayed once. Click Download CSV FIle to save the AccessKeySecret.

![image desc](https://labex.io/upload/H/F/B/8Y6UiUGsRpR3.jpg)

Back to the command line, enter the following command to create the "terraform" directory.

```
mkdir -p terraform && cd terraform
```

![image desc](https://labex.io/upload/P/O/G/INb3OihYE6QP.jpg)

Enter the command ``vim main.tf``, copy the following content to the file, please pay attention to replace with the user's own **access-key, secret-key**.

```

```

![image desc](https://labex.io/upload/X/A/M/EFG5HN0bppkF.jpg)

Enter the following command to initialize the project.

```
terraform init
```

![image desc](https://labex.io/upload/V/N/X/D3zun1aZFGTE.jpg)


Enter the following command to view the execution plan.

```
terraform plan
```

![image desc](https://labex.io/upload/D/L/Q/GWfNMfOYiaji.jpg)

Enter the following command to start creating resources, enter "yes".

```
terraform apply
```

![image desc](https://labex.io/upload/C/M/B/kgY36KGahSyv.jpg)

![image desc](https://labex.io/upload/E/X/R/v8kVcbBeVN6E.jpg)

## 2. RDS Postgresql database

Click  Elastic Compute Service, as shown in the following figure.

![image desc](https://labex.io/upload/Q/C/E/vWl7sSgr7lBx.jpg)

We can see one running ECS instance in China(Hongkong) region. 

![image desc](https://labex.io/upload/I/J/O/DeOtcINee1k2.jpg)

Copy this ECS instance's Internet IP address and remotely log on to this ECS (Ubuntu system) instance. For details of remote login, refer to  [login](https://labex.io/questions/150)。

> The default account name and password of the ECS instance:
> 
> Account name: root
> 
> Password: nkYHG890..

After logging in successfully, enter the following command to download the pre-prepared data script.

```
wget https://cdn.patricktriest.com/atlas-of-thrones/atlas_of_thrones.sql
```

![image desc](https://labex.io/upload/L/B/Q/WK8oIDuG3GAd.jpg)

Enter the following command to install the PostgreSQL client.

```
apt update && apt -y install postgresql-client
```

![image desc](https://labex.io/upload/Y/Q/T/B5F9zCAmDqp3.jpg)

Enter the following command to connect to the PostgreSQL instance. ***Please pay attention to replace YOUR-POSTGRESQL-ADDRESS with the connection address of the user's own PostgreSQL instance***

```
psql -h YOUR-POSTGRESQL-ADDRESS -p 5432 -U patrick -d atlas_of_thrones 
```

![image desc](https://labex.io/upload/S/U/C/EaQZPQ31rp0n.jpg)

```
Password: the_best_passsword
```

Enter the following command to install the Postgis extension. ***If the prompt already exists, it doesn’t matter, just proceed directly to the following operations***

```
CREATE EXTENSION postgis;
```

![image desc](https://labex.io/upload/R/W/J/y3olOucQIGoU.jpg)

Enter the following command to exit the client.

```
\q
```

![image desc](https://labex.io/upload/M/E/M/CuXnrVk0KdBG.jpg)

Enter the following command, Load the downloaded SQL dump into your newly created database. Please pay attention to replace YOUR-POSTGRESQL-ADDRESS with the connection address of your own PostgreSQL instance

```
psql -h YOUR-POSTGRESQL-ADDRESS  -p 5432 -U patrick -d atlas_of_thrones < atlas_of_thrones.sql
```

![image desc](https://labex.io/upload/F/X/E/2FdNCp0Lm2VN.jpg)

```
Password: the_best_passsword
```

Enter the following command to log in to the "atlas_of_thrones" database. ***Please pay attention to replace YOUR-POSTGRESQL-ADDRESS with the connection address of the user's own PostgreSQL instance***

```
psql -h YOUR-POSTGRESQL-ADDRESS  -p 5432 -U patrick -d atlas_of_thrones
```

![image desc](https://labex.io/upload/G/W/G/jsp9FRE18zlV.jpg)

```
Password: the_best_passsword
```

Enter the following command, we can get a list of available tables.

```
\dt
```

![image desc](https://labex.io/upload/X/N/K/4FYaJwSjgCtJ.jpg)

Enter the following command, we can inspect the schema of an individual table by running

```
\d kingdoms
```

![image desc](https://labex.io/upload/M/F/D/RjApKTuXcKVb.jpg)

Enter the following command to query data.

```
SELECT name, claimedby, gid FROM kingdoms;
```

![image desc](https://labex.io/upload/A/U/D/klB43sLfIbJc.jpg)

Enter the following command to exit the client.

```
\q
```

![image desc](https://labex.io/upload/M/E/M/CuXnrVk0KdBG.jpg)


## 3. Install Nodejs

Enter the following command to download the Nodejs installation package.

```
wget http://labex-ali-data.oss-us-west-1.aliyuncs.com/nodejs/node-v12.13.0-linux-x64.tar.xz
```

![image desc](https://labex.io/upload/E/V/W/fyDG1ABVrZfQ.jpg)

Enter the following command to decompress the installation package.

```
tar -xf node-v12.13.0-linux-x64.tar.xz
```

![image desc](https://labex.io/upload/G/U/Q/LWonPK545rgt.jpg)

Enter the following command to move the unzipped directory to the "/usr/local" directory.

```
mv node-v12.13.0-linux-x64 /usr/local/node
```

![image desc](https://labex.io/upload/J/E/T/Nnadk3kLYL7h.jpg)

Enter the command: ``vim /etc/profile``, add the following content to the file.

```
export NODE_HOME=/usr/local/node
export PATH=$PATH:$NODE_HOME/bin
```

![image desc](https://labex.io/upload/A/V/A/ZVkFMDG9mk7N.jpg)

Enter the following command to make the modification effective.

```
source /etc/profile
```

![image desc](https://labex.io/upload/C/F/G/01ruzk50EBCg.jpg)

Enter the following command to view the installed version, indicating that the installation is complete.

```
node -v

npm -v
```

![image desc](https://labex.io/upload/D/J/P/r7fxWNVpP9cw.jpg)

## 4. Install the sample project

Enter the following command to install the git tool.

```
apt -y install git
```

![image desc](https://labex.io/upload/X/U/C/HL5vUW5vevTQ.jpg)

Enter the following command to download the project.

```
git clone -b labex https://github.com/andi1991/Atlas-Of-Thrones.git
```

![image desc](https://labex.io/upload/I/M/K/3mP3gKFiBrwd.jpg)

Enter the following command to enter the project directory.

```
cd Atlas-Of-Thrones
```

![image desc](https://labex.io/upload/A/X/E/h4ehMNlOMVkJ.jpg)

Enter the command: ``vim .env``, copy the following content to the file, ***please pay attention to replace YOUR-POSTGRESQL-ADDRESS 、YOUR-REDIS-ADDRESS with the connection address of your own instance***

```
PORT=5000
DATABASE_URL=postgres://patrick:the_best_passsword@YOUR-POSTGRESQL-ADDRESS:5432/atlas_of_thrones?ssl=false
REDIS_HOST=YOUR-REDIS-ADDRESS
REDIS_PORT=6379
CORS_ORIGIN=http://localhost:8080
```

![image desc](https://labex.io/upload/I/B/A/PpyAb3CozFSm.jpg)

Enter the following command to install related dependencies.

```
npm install
```

![image desc](https://labex.io/upload/B/S/X/HEqZLj8IgzKY.jpg)

Enter the following command to install the dependent package that reported the error separately.

```
npm cache clean --force

npm i --unsafe-perm node-sass@4.14.1
```

![image desc](https://labex.io/upload/B/P/Y/3K3CzByLazs6.jpg)

Enter the following command to start the service.

```
npm run dev
```

![image desc](https://labex.io/upload/P/X/I/U4vJ0O69SKs5.jpg)

Access the service in the browser, enter the link below, ***please pay attention to replace YOUR-ECS-PUBLIC-IP with the user's own ECS public IP address***

```
YOUR-ECS-PUBLIC-IP:8080

47.243.23.173:8080
```

![image desc](https://labex.io/upload/Q/U/H/lQmb9C32O2cn.jpg)

Service deployment completed
