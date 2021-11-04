## PROJECT 5
 ### CLIENT-SERVER ARCHITECTURE WITH MYSQL
This project is all about Knowing the workings of a Client-Server Architecture, it basically entails showing the ability to be able to connect

to the Db-server of the linux ubuntu 20.04 distribution of EC2_instance. Its obvious as I continue to make progress in the field of IT 

I shall see that most concepts are interlink with each other. One of such likely concepts is that of Client-Server implies to an architectural design

in which two or more computers are linked over a network to send and receive requests between one another.

We shall see in the link up communication between each other the sender of request shall be called “client” while the machine that usually

respond(serving) with data requested is the “Server”

A simple diagram below depicts a Web Client-Server architecture 

![image](https://user-images.githubusercontent.com/55473846/140402696-4d9c56d6-d715-4fbd-bc53-1865a562001d.png)

In this scenario, the web Server has a part of a "Client" that links and reads/writes to/from a Database (DB) Server (MySQL),

and the communication between them happens over a Local Network (it can also be Internet connection, but it is a common practice

to place clcint_server and DB Server close to each other in local network).

As shown above the setup on the diagram is a standard generic Web-Stack architecture close to what i have previously executed in earlier projects (LAMP, LEMP, MEAN, MERN),

this type of architecture demonstrated or built with many other technologies – various Web and DB servers.

As previously demonstrated and can be seen now the LAMP website server(s) can be situated anywhere in the world and connected from any part of the world using the internet.

In this project i demonstrated the power of Client-Server communication. For this project i created two Aws Ec2_instances of Ubuntu version 20.04 distribution

with all the configurations for free tier set up. I named one of the ec2 instance Db-server while the other EC2 instance I named the client.

For the db-Server i connected to the instance using ssh connection using Gitbash

![image](https://user-images.githubusercontent.com/55473846/140403052-c2e54be9-4cb7-4a55-97fd-91bfb5d8aa76.png)

I enabled mysql by running the command below

$sudo systemctl enable mysql-server

![image](https://user-images.githubusercontent.com/55473846/140403161-fdc55ea6-dc2c-4d6b-be33-d4c2d0ff58b4.png)


The next thing i did was to install the client, i connected to the client instance on my ec2_ instance called the “client” connected to it using my Gitbash and using

ssh connection. On the Db-server, i connected to the instance and clicked on security that took me to security group where i edited the inbound rules.

I now added another inbound rule selecting mysql-Aurora which has a default port:3306. Now on the source i didn’t open the source for everyone by

inserting 0.0.0.0 like i have been doing in other projects but i rather inserted the ip_address of the client machine only.

The command below show the ip_address of the client while i connected to that ec2 instance

$ ip addr show

![image](https://user-images.githubusercontent.com/55473846/140403662-7e4a3d2a-575d-4f20-95af-1725e89a6a81.png)

Having inserted the inserted ip address shown below in the source of the inbound rule 172.31.16.139/24 into the inbound rule source for the

db-server as the only ip address that can enter that server under security group as shown below. However even when we run hostname -i on the same client ec2_ instance

It will still give us same thing

172.31.16.139/

![image](https://user-images.githubusercontent.com/55473846/140403826-017708c2-b49b-4623-863a-bad26be3020b.png)

Having done the above i now have to connect to the db-server instance of the Ec2_instance, first I needed to install the db-server instance data

base engine and secure the installation by running the command below 

$ sudo mysql_secure installation 

I entered any letter “s” except yes when prompted for validation and entered y all the way for other p


![image](https://user-images.githubusercontent.com/55473846/140403982-ee6f006d-2373-4b98-80e5-30d3ebbf8185.png)

I now ran 

$ sudo mysql

![image](https://user-images.githubusercontent.com/55473846/140404087-f62d36fd-9759-4d4d-b8f1-e67716f6c661.png)

Now because i am the administrative user using sudo it allows connection not waiting for me to add any password

Now that i have configured both the db-server and the client we now need to have data into the database of the db_server.

I created user “remote_user”, created the database “test_db”, grant all access to the “remote_user” with grant OPTION. While finally granting all on test_db. 

This is as shown below:

![image](https://user-images.githubusercontent.com/55473846/140404237-916af2c7-0547-4faf-a1fd-5d6ddcb9ef4d.png)

Having done the above i now needed to configure MySQL server to make available connections from remote hosts using the command below

sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf

I bind the address of the db-server by changing the bind_address from 127.0.0.1 to 0.0.0.0  as shown below:

![image](https://user-images.githubusercontent.com/55473846/140404372-dba47f53-e586-4fe9-b831-8bad6251491f.png)

Having done this i now restarted mysql by running the command

$sudo systemctl restart mysql

Once the above was done i now needed to connect Client to my db_server since i already have the mysql-client software installed,

now we can test the set up by connecting to connecting to my client instance whose ip_address can be gotten by passing the either of the command below:

Ip addr show

![image](https://user-images.githubusercontent.com/55473846/140404651-e4d8cc27-4cc8-434d-8e73-eca4382bbebd.png)

Or 

hostname -i

![image](https://user-images.githubusercontent.com/55473846/140404810-7ff08c44-4b1a-4c11-be3e-b8f9bfb311ac.png)

Now I can now log to the db-server from my client using the command below

$sudo mysql -u remote_user -h <ip_adress_db-server> -p

$sudo mysql -u remote_user -h 172.31.22.165 -p

Enter password:

And i was effectively able to login to the db-server as shown below

![image](https://user-images.githubusercontent.com/55473846/140405181-c24bd28e-5cb2-4b83-8775-781fb9cb2c05.png)

and i was able to show the database with command below

show databases.


![image](https://user-images.githubusercontent.com/55473846/140405380-97151912-e5f9-44ac-bf39-7d637fd8567f.png)

The above shows i could connect to my db-server from my client but am restricted to perform certain functions like 

CREATE DATABASE mytest2_db from the remote client may be because I don’t have the right privileges


![image](https://user-images.githubusercontent.com/55473846/140405735-cb50ac46-c953-452e-8204-2284eb4e8b9d.png)




