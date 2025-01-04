# Port

A **port** is a logical endpoint in a computer's networking system. It is used to identify specific processes or network services within a system. Ports are part of the **TCP/IP protocol suite**, which allows computers to communicate over a network.

When we say "MySQL listens on port 3306," or "HTTP listens on port 80," it means the application or service is waiting for incoming network connections on that specific port.

---

### **What is a Port?**

- **Definition:** A port is a 16-bit number used to identify a specific process or service on a computer. Ports range from 0 to 65535.

- **Purpose:** Ports allow multiple services to run simultaneously on the same IP address. For example:

  - MySQL on port 3306.
  - HTTP (web server) on port 80.
  - SMTP (email server) on port 25.

- **How It Works:** When a program or service "binds" to a port, it tells the operating system, "I'm responsible for any communication on this port." If another service tries to use the same port, a conflict occurs.

---

### **How Does a Port Look Like?**

A port itself is just a number, but it is associated with an IP address. Together, they form a **socket**:

#### **Example:**

1.  **IP Address:** `192.168.1.10`
2.  **Port:** `3306` (for MySQL)

The complete **socket address** looks like:

makefile

Copy code

`192.168.1.10:3306`

This combination uniquely identifies the service running on a particular machine in a network.

---

### **Ports in Action**

When you access a service over the network:

1.  Your client (e.g., browser or application) sends a request to a specific port on a server.
2.  The server listens on the port and responds to the request.

#### **Example with HTTP (Port 80):**

- When you visit `http://example.com`:
  - Your browser sends an HTTP request to port 80 of the server hosting `example.com`.
  - The web server (e.g., Apache or Nginx) listening on port 80 receives the request and sends back the website's data.

---

### **Types of Ports**

Ports are categorized into three groups:

1.  **Well-Known Ports (0--1023):**

    - Reserved for common services:
      - HTTP: 80
      - HTTPS: 443
      - FTP: 21
      - SMTP: 25

2.  **Registered Ports (1024--49151):**

    - Used by less common services:
      - MySQL: 3306
      - PostgreSQL: 5432

3.  **Dynamic/Private Ports (49152--65535):**

    - Temporary ports assigned by the operating system for client-side connections.

---

### **Visualization of Ports**

You can think of a port as a door to a building:

- The **building** is the IP address (your machine).
- The **door** (port) allows specific services to enter or leave the building.

If MySQL is on port 3306, it's like saying, "This door is specifically for the database service."
