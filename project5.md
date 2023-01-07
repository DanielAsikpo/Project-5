# **IMPLEMENTING A CLIENT-SERVER ARCHITECTURE USING A MYSQL RELATIONAL DATABASE MANAGEMENT SYSTEM**


- ## **Step 1** 

Launch two ubuntu EC2 instances in AWS and name them: `mysql server` and `mysql client` respectively.

![Image](./Images/Screenshot_1.png)

- ## **Step 2** 

Install MySQL Server software on mysql server.

Before we install the server, run `sudo apt update` and `sudo apt upgrade`

Then run the following command to install the MySQL Server:

`sudo apt install mysql-server -y`

![Image](./Images/Screenshot_2.png)

- ## **Step 3** 

On mysql client, install MySQL Client software: 

`sudo apt install mysql-client -y`

![Image](./Images/Screenshot_3.png)

- ## **Step 4**

Before you can receive mysql client traffic on your mysql server, you need make sure your security group settings on AWS allows port 3306 traffic - the default MySQL server TCP port.  **For extra security, do not allow all IP addresses to reach your mysql server.**


- ## **Step 5** 

For mysql client to gain remote access to mysql server, we need to create and database and a user on mysql server.

Open the MySQL console by typing:

`sudo mysql`

Create the remote user with this following command: 

`CREATE USER 'remote_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';`

Create database with:

`CREATE DATABASE myproject_db;`

Then grant privileges to remote_user: 

`GRANT ALL ON myproject_db.* TO 'remote_user'@'%' WITH GRANT OPTION;`

Finally, flush privileges and exit mysql:

`FLUSH PRIVILEGES;`

This is a good practice that will free up any memory that the server cached as a result of the preceding CREATE USER and GRANT statements.

Then exit with the `exit` command

![Image](./Images/Screenshot_4.png)

- ## **Step 6**

After creating the user and database, configure MySQL server to allow connections from remote hosts. 

Use the command below: 

`sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf`

By default, this value is set to 127.0.0.1, meaning that the server will only look for local connections. In the text editor, **replace the old bind-address from ‘127.0.0.1’ to ‘0.0.0.0’** then save and exit. **(CTRL + X, Y, then ENTER)**. This gives access to an external IP address.

![Image](./Images/Screenshot_5.png)

- ## **Step 7** 

Restart mysql server with: `sudo systemctl restart mysql`

- ## **Step 8** 

Then from msql client, connect remotely to the mysql server Database Engine without using SSH. **You must use mysql utility to perform this action.** 

Type: `sudo mysql -u remote_user -h 172.31.95.168 -p` and enter password for the user password.

- ## **Step 9** 

Check that you have successfully connected to a remote MySQL server and can perform SQL queries:

`Show databases;`

![Image](./Images/Screenshot_6.png)

*Congratulations!!! You have deployed a fully functional MySQL Client-Server set up.*