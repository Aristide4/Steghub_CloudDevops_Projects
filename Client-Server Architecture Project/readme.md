# Task: Implement a Client-Server Architecture with MySQL Database Management System (DBMS)

## Instructions Followed:

### Step 1: Create and Configure Two Linux-Based Virtual Servers (EC2 Instances on AWS)

- **mysql server**
- **mysql client**

<img width="1561" height="171" alt="create sql clients and server" src="https://github.com/user-attachments/assets/a7f79e25-806a-4d50-a6bb-bf5d0e367041" />


### Step 2: Install MySQL Server on the `mysql server` Instance

   Install MySQL by executing the following:
   ```bash
   sudo apt install mysql-server -y
   ```
  <img width="1405" height="183" alt="image" src="https://github.com/user-attachments/assets/5a225a3d-ddcb-477b-983d-1c40487c8d13" />
  

### Step 3: Install MySQL Client on the `mysql client` Instance

3. **Install MySQL Client **

   Install the MySQL client on this instance:

   ```bash
   sudo apt install mysql-client -y
   ```

   <img width="1110" height="172" alt="install sql client " src="https://github.com/user-attachments/assets/55c39514-ed63-4cae-8046-e7c24dfd0221" />


3. **Create a User and Database on the MySQL Server**

   A user named `Aris`  created on the `mysql server`:

   ```sql
   CREATE USER 'Aris'@'%' IDENTIFIED WITH mysql_native_password BY 'CloudDevops12!';

   FLUSH PRIVILEGES;
   ```

   <img width="995" height="131" alt="create user " src="https://github.com/user-attachments/assets/159808d5-109e-43fc-8977-4595981a8ae7" />


4. **Allow Remote Connections to the MySQL Server**

   To allow the MySQL server to accept remote connections, the bind address was modified in the MySQL configuration file. The default address `127.0.0.1` was replaced with `0.0.0.0` to enable connections from any host:

   ```bash
   sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
   ```

   Locate the following line:
   ```bash
   bind-address = 127.0.0.1
   ```

   Change it to:
   ```bash
   bind-address = 0.0.0.0
   ```

 <img width="811" height="589" alt="image" src="https://github.com/user-attachments/assets/e0098cf8-c887-4f6f-8ebe-0261abf1c1f5" />

---

### Step 6: Connect Remotely to the MySQL Server from the `mysql client` Instance

Using the MySQL utility, connect remotely to the `mysql server` database engine without using SSH. The following command is used from the `mysql client`:

```bash
sudo mysql -u Aris -h 3.91.100.123 -p
```

Replace `172.31.41.170` with the internal IP address of the `mysql server`.

<img width="1058" height="303" alt="connect to mysql from client" src="https://github.com/user-attachments/assets/1dfa97ba-86b8-46a8-aeac-0804e236c9bc" />

---

### Step 7: Verify the Connection and Perform SQL Queries

Once connected, verify the connection by listing the databases:

```bash
SHOW DATABASES;
```

<img width="1181" height="512" alt="playing with databases" src="https://github.com/user-attachments/assets/7e40804c-5838-439a-85ee-0db215d938c5" />

### Conclusion

At this point, the project is successfully completed with a fully functional MySQL client-server setup. This demonstrates the deployment and connection between a MySQL client and a remote MySQL server on separate EC2 instances.

