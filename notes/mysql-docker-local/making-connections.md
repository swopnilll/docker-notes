# Connecting to a MySQL database instance running inside a Docker container

### **Key Concepts**

1.  **Docker and MySQL Containers**

    - Docker containers can be used to host different versions of MySQL databases.
    - Examples: MySQL 5.7 (`mysql:5.7`) and the latest version (`mysql:latest`).

2.  **Connecting to MySQL Containers**\
    There are two main ways to connect to a MySQL instance running in a Docker container:

    - **Command Line Interface (CLI)**
    - **MySQL Workbench (Graphical Interface)**

3.  **Understanding Ports and Mapping**

    - MySQL uses the default port `3306` for communication.
    - Docker allows mapping container ports to host machine ports using the `-p` option to make the service accessible externally.

### **Detailed Explanation**

#### **Method 1: Connecting via Command Line**

**Scenario:**\
You're using the terminal and want to interact with the MySQL database directly.

**Steps:**

1.  **Run the Docker Command:**\
    Use the `docker exec` command to interact with the running container.

```bash
docker exec -it <container_name> mysql -u root -p
```

- `-it`: Interactive terminal mode.
- `<container_name>`: Name of the running MySQL container.
- `mysql -u root -p`: Launches the MySQL client inside the container using the root user. You'll be prompted for the root password.

**Outcome:**\
 This connects you directly to the MySQL database instance running inside the container.

#### **Method 2: Connecting via MySQL Workbench**

**Scenario:**\
You want to connect to the MySQL instance using a graphical interface (e.g., MySQL Workbench) installed on your host machine.

**Steps:**

1.  **Expose the Container Port:**\
    When running the Docker container, map the container's internal MySQL port (`3306`) to an external port (e.g., `3309`) on the host machine.

    `docker run -d -p 3309:3306 --name <container_name> mysql:latest`

    - `-p 3309:3306`: Maps port `3306` of the container to port `3309` on the host.
    - `-d`: Runs the container in detached mode.

2.  **Connect Using MySQL Workbench:**

    - **Hostname:** `localhost` or the IP address of your machine.
    - **Port:** `3309` (as mapped above).
    - **Username:** `root` (or any other user you set up).
    - **Password:** The password for the MySQL user.

3.  **Outcome:**\
    MySQL Workbench establishes a connection to the MySQL instance running inside the container via the mapped port.

### **Example Workflow**

1.  **Start a MySQL Container:**

    `docker run -d -p 3309:3306 --name mysql_container mysql:latest`

2.  **Verify the Container is Running:**

    `docker ps`

    This shows all running containers. Confirm that the MySQL container is listed.

3.  **Command Line Connection:**

    `docker exec -it mysql_container mysql -u root -p`

    Enter the password and access the MySQL prompt.

4.  **MySQL Workbench Connection:**\
    Open MySQL Workbench and use the following details:

    - **Hostname:** `localhost`
    - **Port:** `3309`
    - **Username:** `root`
    - **Password:** Your configured MySQL password.

5.  **Test the Connection:**\
    Once connected, you can execute queries and manage the database using the Workbench interface.

### **Key Points to Remember**

1.  **Default MySQL Port:** The default port for MySQL is `3306`.

    - Containers need explicit port mapping to make them accessible from the host system.

2.  **Docker Command Syntax:**

    - `docker exec -it`: Used to interactively access a container.
    - `docker run -p`: Maps container ports to host ports.

3.  **MySQL Workbench Integration:**

    - Requires the container's MySQL port to be published/mapped to the host system.
    - Use the mapped port (e.g., `3309`) to connect from external tools.
