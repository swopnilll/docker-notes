```yaml
FROM nginx:1.15

COPY index.html /usr/share/nginx/html
```

This process is faster and easier than starting from a minimal base image (e.g., Debian) and manually installing and configuring NGINX yourself. Here's the detailed explanation of the entire process:

---

### 1\. **Understanding the Goal**

We want to:

1.  Include an HTML file (`index.html`) in our Docker image.
2.  Use an NGINX web server to serve this file over HTTP.
3.  Build and run a container that serves this file to users via a browser.

### 3\. **Choosing the Base Image**

- **Why NGINX?**
  - NGINX is a lightweight, high-performance web server commonly used to serve static files, proxy requests, or handle HTTP traffic.
  - The official **NGINX image** on Docker Hub is already configured to serve files placed in the `/usr/share/nginx/html` directory.
- By using the official NGINX image, we skip the need to:
  - Start with a minimal base image like Debian.
  - Install NGINX manually using package managers.
  - Configure NGINX settings to serve files.
- **Result:** Saves time, effort, and ensures stability with a well-tested image.

### 4\. **Creating the Dockerfile**

- A `Dockerfile` is used to define the custom image. Here's what it looks like:

  ```yaml
  FROM nginx:latest
  COPY index.html /usr/share/nginx/html/index.html
  ```

- **Explanation of the Dockerfile instructions:**
  1.  **`FROM nginx:latest`**:
      - This sets the base image as the latest official NGINX image from Docker Hub.
      - The base image already includes NGINX pre-installed and configured to serve files from `/usr/share/nginx/html`.
  2.  **`COPY index.html /usr/share/nginx/html/index.html`**:
      - This instruction copies the local `index.html` file into the `/usr/share/nginx/html/` directory inside the container.
      - NGINX is configured to look for files in this directory, so placing the `index.html` here ensures it will be served when the container runs.

### 5\. **Building the Image**

- Use the `docker build` command to build the custom image:

  `docker build -t my-nginx .`

- **Explanation:**
  1.  **`docker build`**:
      - This command reads the instructions in the `Dockerfile` to create a Docker image.
  2.  **`-t my-nginx`**:
      - Tags the image with the name `my-nginx`.
      - This makes it easier to reference the image later (e.g., when running containers).
  3.  **`.` (dot)**:
      - Specifies the current directory as the build context.
      - Docker looks for the `Dockerfile` and other files (e.g., `index.html`) in this directory.

### 6\. **Running the Container**

- Use the `docker run` command to start a container from the custom image:

  `docker run -d -p 8080:80 my-nginx`

- **Explanation:**
  1.  **`docker run`**:
      - Creates and starts a container from the specified image (`my-nginx`).
  2.  **`-d`**:
      - Runs the container in detached mode (in the background).
  3.  **`-p 8080:80`**:
      - Maps port `8080` on the host machine to port `80` inside the container.
      - NGINX listens on port `80` by default, and this mapping allows you to access the web server at `http://localhost:8080` on the host.
  4.  **`my-nginx`**:
      - Specifies the name of the image to use for the container.

### 7\. **Accessing the Web Page**

- Open a web browser and navigate to `http://localhost:8080`.
- You should see the contents of your `index.html` file displayed

### 8\. **Key Advantages of This Approach**

1.  **Reusability**: The custom Docker image (`my-nginx`) can be shared, reused, or pushed to a container registry.
2.  **Efficiency**: By leveraging the prebuilt NGINX image, you save time and effort in setting up and configuring the web server.
3.  **Portability**: The image includes everything needed to serve the `index.html` file, ensuring it works consistently across any environment where Docker is installed.
4.  **Simplicity**: Minimal instructions in the `Dockerfile` reduce complexity while achieving the desired result.

### **Summary**

- This process demonstrates how to include an HTML file in a Docker image and serve it using NGINX.
- By using the official NGINX image, we simplify the process and leverage a preconfigured, well-tested setup.
- The resulting custom image is efficient, portable, and easy to use, showcasing the power of Docker in real-world scenarios.

# **Including Files in a Docker Image**

## **1. The Concept of Build Context**

### **Definition**

- The **build context** is the directory provided to the `docker build` command.
- The files in this directory are accessible during the image build process but only for the instructions in the Dockerfile.

### **Key Points**

- Files from the build context are **not automatically included** in the resulting image or the containers created from that image.
- To explicitly include files, you need to use the **`COPY` instruction** in the Dockerfile.

---

## **2. The `COPY` Instruction**

### **Purpose**

- The `COPY` instruction copies files from the build context to a specified location inside the image during the build process.

### **Syntax**

```dockerfile
COPY <source> <destination>
```

- `<source>`: The file or directory from the build context.
- `<destination>`: The location inside the image where the file(s) will be copied.

### **Example**

```dockerfile
COPY index.html /usr/share/nginx/html/index.html
```

- This copies the local `index.html` file into the directory `/usr/share/nginx/html` inside the image.
- Since NGINX is preconfigured to serve files from `/usr/share/nginx/html`, the `index.html` file will automatically be served when the container runs.

---

## **3. Importance of Build Context**

### **Availability**

- The contents of the build context are available only during the image build process.
- They can only be accessed by Dockerfile instructions (e.g., `COPY` or `ADD`).

### **Exclusion**

- If a file in the build context is not explicitly copied into the image (using `COPY`), it will not be included in the final image or the resulting containers.

---

## **4. Default Behavior of Base Images**

### **Base Images May Contain Predefined Instructions**

- Many official base images, like `nginx:1.15`, already include preconfigured behavior and default instructions (e.g., a `CMD` instruction to start NGINX).

### **CMD Instruction in Base Image**

- The **CMD instruction** specifies the default executable that runs when a container is started.
- For example, the `nginx:1.15` base image already has a CMD instruction that runs the NGINX server:
  ```dockerfile
  CMD ["nginx", "-g", "daemon off;"]
  ```
- This eliminates the need to include your own `CMD` instruction unless you need to modify the behavior.

---

## **5. Why No CMD Instruction in This Dockerfile**

### **Reason**

- The Dockerfile in this scenario does not include a `CMD` instruction because the base image (`nginx:1.15`) already contains a CMD instruction to start the NGINX server.
- Adding another `CMD` instruction is unnecessary unless you want to override the existing one.

### **Implications**

- When the container starts, it will automatically execute the default command (`nginx`) and serve files from the `/usr/share/nginx/html` directory.

---

## **6. Summary of the Process**

### **1. Prepare the Build Context**

- Place all necessary files (e.g., `index.html`) in the directory you plan to use as the build context.

### **2. Create a Dockerfile**

- Use a prebuilt base image (e.g., `nginx`) to simplify the setup.
- Use the `COPY` instruction to include specific files from the build context into the image.

### **3. Build the Image**

- Use the `docker build` command to create an image:
  ```bash
  docker build -t my-nginx .
  ```
- The build context (`.`) makes the local files available during the build.

### **4. Run the Container**

- Start a container from the built image:
  ```bash
  docker run -d -p 8080:80 my-nginx
  ```
- The container runs NGINX, serving files from `/usr/share/nginx/html`.

### **5. Access the Application**

- Open a browser and navigate to `http://localhost:8080` to view the content served from the `index.html` file.

---

## **Key Takeaways**

1. The **build context** provides files during the image build process but does not automatically include them in the final image or container.
2. The **`COPY` instruction** explicitly includes files from the build context into the image.
3. Prebuilt base images like `nginx` often include default behavior (e.g., a CMD instruction to start the server).
4. Leveraging prebuilt images and the build context simplifies creating efficient, custom Docker images.
