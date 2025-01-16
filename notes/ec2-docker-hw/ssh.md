# SSH and Its Usage with AWS

## What is SSH?

- **Definition:**  
  SSH (Secure Shell) is a cryptographic network protocol that encrypts all data exchanged between a client and a server.
- **Purpose:**  
  Provides secure access to a remote server through the terminal.
- **Key Features:**
  - Encryption ensures data security during transmission.
  - Commonly used to access and manage remote servers.

## Components Required for SSH Connection

- **SSH Server:**
  - Installed on the remote machine.
  - Accepts incoming SSH connections.
  - Pre-installed on rented servers like AWS EC2 instances.
- **SSH Client:**
  - Software used to initiate a connection to the SSH server.
  - Pre-installed on most operating systems, including Linux, macOS, and Windows.

## Establishing an SSH Connection

- **Basic Command:**
  ```bash
  ssh <username>@<server-address>
  ```
  Example:
  ```bash
  ssh ec2-user@ec2-123-45-67-89.compute-1.amazonaws.com
  ```
- **Using a Custom Port:**  
  If the server runs on a port other than the default (22):
  ```bash
  ssh -p <port> <username>@<server-address>
  ```
  Example:
  ```bash
  ssh -p 2222 user@server.com
  ```

## Authentication Mechanisms

- **Password Authentication:**
  - Requires entering the server's password.
  - Less secure; prone to brute-force attacks.
- **Public-Private Key Authentication:**
  - **Private Key:** Secret key stored securely on your machine.
  - **Public Key:** Shared with the server for authentication.
  - More secure than password authentication.

## Generating an SSH Key Pair

- **Command:**
  ```bash
  ssh-keygen -t ed25519
  ```
  - `ed25519` is a modern, secure cryptographic algorithm.
- **Key Pair Details:**
  - Private Key: Stored securely on your machine, never shared.
  - Public Key: Can be shared with others, used to authenticate the private key.
- **File Location:**
  - Typically stored in the `.ssh` folder in the user's home directory:
    - Private Key: `~/.ssh/id_ed25519`
    - Public Key: `~/.ssh/id_ed25519.pub`
- **Optional Passphrase:**
  - Adds an extra layer of encryption to the private key.

## Using the Generated Key Pair

- The public key is added to the remote server's authorized keys file:
  - Location: `~/.ssh/authorized_keys` on the server.
- Private key remains on the client machine and is used during the connection process.

## Default SSH Port (22) and Security Considerations

- **Default Behavior:**  
  Most SSH servers run on port 22.
- **Security Measures:**
  - Change the SSH port to reduce brute-force attacks.
  - Example of custom port usage:
    ```bash
    ssh -p 2222 <username>@<server-address>
    ```

## Preparing for AWS EC2 Setup

- **Requirements:**
  - Generate an SSH key pair before launching an EC2 instance.
  - Public key is added to the EC2 instance during creation.
- **Authentication:**  
  AWS enforces public-private key authentication for EC2 instances for enhanced security.

## Next Steps

- **Registering for AWS:**  
  Set up an AWS account to launch EC2 instances.
- **Exploring the AWS Dashboard:**  
  Familiarize yourself with EC2 services and instance configuration options.
