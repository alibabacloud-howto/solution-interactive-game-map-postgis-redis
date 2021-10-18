# Creating a Web Game Map App Using Leaflet, PostgreSQL/PostGIS and Redis on Alibaba Cloud

You can access the tutorial artifact including deployment script (Terraform), related source code, sample data and instruction guidance from the github project:
[https://github.com/alibabacloud-howto/solution-interactive-game-map-postgis-redis](https://github.com/alibabacloud-howto/solution-interactive-game-map-postgis-redis)

More tutorial around Alibaba Cloud Database, please refer to:
[https://github.com/alibabacloud-howto/database](https://github.com/alibabacloud-howto/database)

---
### Overview

This is demo of building an interactive "Game of Thrones" ([https://github.com/andi1991/Atlas-Of-Thrones](https://github.com/andi1991/Atlas-Of-Thrones)) map powered by Leaflet, PostgreSQL/PostGIS and Redis on Alibaba Cloud.

Deployment architecture:

![image.png](https://github.com/alibabacloud-howto/solution-interactive-game-map-postgis-redis/raw/main/images/archi.png)

---
### Index

- [Step 1. Use Terraform to provision ECS, PostgreSQL database and Redis on Alibaba Cloud](https://github.com/alibabacloud-howto/solution-interactive-game-map-postgis-redis#step-1-use-terraform-to-provision-ecs-postgresql-database-and-redis-on-alibaba-cloud)
- [Step 2. Setup sample data in RDS PostgreSQL database](https://github.com/alibabacloud-howto/solution-interactive-game-map-postgis-redis#2-setup-sample-data-in-rds-postgresql-database)
- [Step 3. Install NodeJS](https://github.com/alibabacloud-howto/solution-interactive-game-map-postgis-redis#3-install-nodejs)
- [Step 4. Deploy and run the demo game map project](https://github.com/alibabacloud-howto/solution-interactive-game-map-postgis-redis#4-deploy-and-run-the-demo-game-map-project)

---
### Step 1. Use Terraform to provision ECS, PostgreSQL database and Redis on Alibaba Cloud

Run the [terraform script](https://github.com/alibabacloud-howto/solution-interactive-game-map-postgis-redis/blob/main/deployment/terraform/main.tf). Please specify the necessary information and region to deploy.

![image.png](https://github.com/alibabacloud-howto/solution-interactive-game-map-postgis-redis/raw/main/images/tf-parms.png)

After the Terraform script execution finished, the ECS instance information are listed as below.

![image.png](https://github.com/alibabacloud-howto/solution-interactive-game-map-postgis-redis/raw/main/images/tf-done.png)

- ``eip_ecs``: The public EIP of the ECS for gamp map application host
- ``rds_pg_url``: The connection endpoint URL of the RDS PostgreSQL database
- ``rds_pg_port``: The connection endpoint port of the RDS PostgreSQL database
- ``redis_url``: The connection endpoint URL of the Redis

---
### 2. Setup sample data in RDS PostgreSQL database

Click ``Elastic Compute Service``, as shown in the following figure.

![image desc](https://labex.io/upload/Q/C/E/vWl7sSgr7lBx.jpg)

We can see one running ECS instance in China(Hongkong) region. 

![image desc](https://labex.io/upload/I/J/O/DeOtcINee1k2.jpg)

Copy this ECS instance's Internet IP address and remotely log on to this ECS (Ubuntu system) instance. For details of remote login, refer to  [login](https://labex.io/questions/150)。

> The default account name and password of the ECS instance:
> 
> Account name: root
>
> Password: Aliyun-test

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

---
### 3. Install NodeJS

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

---
### 4. Deploy and run the demo game map project

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
```

![image desc](https://labex.io/upload/Q/U/H/lQmb9C32O2cn.jpg)

Game map service deployment is completed.

---
Author of this tutorial on Alibaba Cloud:
- [@andi1991](https://github.com/andi1991)
- [@javainthinking](https://github.com/javainthinking)