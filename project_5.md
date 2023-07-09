**PROJECT 5:  Implement a Client/Server Architecture Using MySQL Relational Database Management System**


## INSTALLING MYSQL SERVER SOFTWARE AND MYSQL CLIENT SOFTWARE

1. On your AWS console create two Ubuntu 20.04 LTS instances and name them "mysql server" and "mysql client"


2. SSH into the mysql server instance from one shell, and into the mysql client instance from another


3. Update and upgrade Ubuntu apt package with the command:

    `sudo apt update`
    ` sudo apt upgrade`


4. On **mysql server** :

    - install mysql server: `sudo apt-get install mysql-server`

    - confirm installation: `mysql --version` or `mysql -v`

    - run the security script to set up a password for the root user:

        - login to MySQL with ` sudo mysql` 

        - run the SQL query:  ` ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '<password>';` to set password for the root user 

        - exit MySQL with `exit`

        - run `sudo mysql_secure_installation` to start the security script and secure your password

        - verify that MySQL is running with `sudo systemctl status mysql`


5. On **mysql client** install MySQL Client Software with `sudo apt-get install default-mysql-client`


## ESTABLISHING A CONNECTION BETWEEN MySQL CLIENT AND MySQL SERVER VIA IP ADDRESS

1. Open TCP port 3306 in mysql server so that MySQL can connect to client 

    ![port](./images/add_inbound_rule.png)


2. Change the MySQL configuration settings on **mysql server** so that it allows remote connections

- open the file mysqld.cnf `sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf`

- where it says "bind-address" change the value from "127.0.0.1" to "0.0.0.0"


3. Create user with privileges

- login to MySQL server from the server terminal with `sudo mysql`

- use the `CREATE USER` statement to create a new user:

    mysql> `CREATE USER 'username'@'localhost' IDENTIFIED WITH authentication_plugin BY 'password';`
    
    Substitute fields accordingly. Here the `username` will be the user that is trying to access from the client, in this case the root user of **mysql client**. The `localhost` is mysql client IP address. And enter a strong password where it says `password`

    You can also allow the new user to access the database from anywhere (using IP address 0.0.0.0 as localhost) but for extra security, use only the specific local IP address of your "mysql client".
    

- grant permissions to the new user with the statement: 

    `GRANT 'permission' ON 'database' . 'table' TO 'user'@'localhost';`


- use `FLUSH PRIVILEGES` to make changes effective


4. On **mysql client** terminal run ` mysql --host='ip_address' --user='username' --password`
substituting to ip_address and username the correct values

    - when prompted enter the password you chose for the new user 

    - then check to have you have successfully connected to remote MySQL server by running some SQL queries such as  `SHOW DATABASES;`


