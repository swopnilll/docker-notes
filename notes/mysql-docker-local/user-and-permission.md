# User and Permissions in MySQL: System-Level and Database-Level

MySQL operates at two levels of user and permissions management:

1. **System-Level (Operating System) Users and Permissions**
   - Controls how MySQL interacts with the system files and directories.
2. **Database-Level (MySQL) Users and Permissions**
   - Defines access to databases, tables, and other objects within MySQL.

---

## System-Level (OS) Users and Permissions

### Why MySQL Needs a System User

MySQL runs as a **service/daemon** on the OS. For security and isolation:

- It runs under a **dedicated user account** (commonly `mysql`).
- This ensures MySQL processes cannot interfere with other system processes or access unauthorized files.

### What Happens During Installation

1. **Creation of the `mysql` System User and Group**

   - During installation, MySQL typically creates:
     - A **system user** named `mysql`.
     - A **group** named `mysql`.
   - Example on Linux:
     ```bash
     sudo groupadd mysql       # Creates the mysql group
     sudo useradd -r -g mysql mysql # Creates the mysql system user with no login shell
     ```

2. **Assignment of File Ownership**
   - Directories like the data directory (`/var/lib/mysql`) and log files are owned by the `mysql` user and group.
   - Example:
     ```bash
     sudo chown -R mysql:mysql /var/lib/mysql
     sudo chown mysql:mysql /var/log/mysql.log
     ```

### System User Restrictions

The `mysql` system user is a **non-login** account:

- It cannot log in to the operating system.
- This minimizes the risk of misuse or security breaches.

### Permissions on MySQL Files

MySQL needs read and write access to its data files, logs, and configuration files. The `mysql` user is given appropriate permissions:

- **Data Directory**: `/var/lib/mysql`

  - **Permissions:**
    ```bash
    drwx------  mysql mysql /var/lib/mysql
    ```
  - Explanation: Only the `mysql` user can read/write here for security.

- **Log Files**: `/var/log/mysql.log`
  - **Permissions:**
    ```bash
    -rw-r----- mysql mysql /var/log/mysql.log
    ```
  - Explanation: Only `mysql` can write, but members of the `mysql` group can read.

### Example of System Permissions Issues

#### Problem: MySQL Cannot Access the Data Directory

If MySQL is unable to access `/var/lib/mysql`, you might see an error like:

```plaintext
[ERROR] InnoDB: The MySQL server failed to start because it could not access the data directory.
```

#### Solution: Ensure Correct Ownership and Permissions

```bash
sudo chown -R mysql:mysql /var/lib/mysql
sudo chmod -R 700 /var/lib/mysql
```

---

## Database-Level (MySQL) Users and Permissions

Inside MySQL, users are defined independently of the system's users. These **database users** authenticate and authorize access to MySQL objects like databases and tables.

### 1. MySQL User Accounts

MySQL user accounts are defined in the `mysql.user` table and managed using SQL commands like `CREATE USER`, `GRANT`, and `REVOKE`.

### 2. Permissions/Privileges

MySQL uses privileges to control what a user can do. These include:

- **Global Privileges**: Apply to all databases (e.g., `CREATE USER`).
- **Database Privileges**: Apply to specific databases (e.g., `SELECT`).
- **Table Privileges**: Apply to specific tables within a database.
- **Column Privileges**: Apply to specific columns within a table.

---

### Examples of Database-Level User and Permissions

#### Creating a MySQL User

```sql
CREATE USER 'user1'@'localhost' IDENTIFIED BY 'securepassword';
```

- `user1`: The username.
- `localhost`: Limits the user to log in only from the local machine.
- `securepassword`: The user's password.

#### Granting Privileges

```sql
GRANT SELECT, INSERT, UPDATE ON mydb.* TO 'user1'@'localhost';
```

- Grants `SELECT`, `INSERT`, and `UPDATE` privileges on all tables in the `mydb` database.

#### Revoking Privileges

```sql
REVOKE INSERT ON mydb.* FROM 'user1'@'localhost';
```

- Revokes the `INSERT` privilege.

#### Viewing User Privileges

To check privileges:

```sql
SHOW GRANTS FOR 'user1'@'localhost';
```

---

## Combining System and Database Permissions

Both levels of permissions must align for MySQL to function correctly:

1. The `mysql` system user must have the necessary file permissions to access data and logs.
2. The MySQL database user must have sufficient privileges to access or modify database objects.

---

### Common Issues and Resolutions

#### 1. MySQL Service Fails to Start

**Cause:** Incorrect ownership or permissions on the data directory.
**Fix:**

```bash
sudo chown -R mysql:mysql /var/lib/mysql
sudo chmod -R 700 /var/lib/mysql
```

#### 2. Access Denied for a Database User

**Cause:** The database user lacks required privileges or the wrong host is specified.
**Fix:**

```sql
GRANT ALL PRIVILEGES ON mydb.* TO 'user1'@'localhost';
FLUSH PRIVILEGES;
```

#### 3. File Permission Denied in Logs

**Cause:** The `mysql` system user cannot write to the log file.
**Fix:**

```bash
sudo chown mysql:mysql /var/log/mysql/error.log
sudo chmod 640 /var/log/mysql/error.log
```

---

This document provides a detailed overview of how MySQL manages users and permissions both at the system and database levels. Proper configuration of these aspects ensures a secure and smoothly operating database environment.
