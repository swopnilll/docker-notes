### Comprehensive Notes on Docker Images and Containers

---

#### **Docker Images**

1.  **Definition**:

    - A Docker Image is a **read-only template** with instructions for creating a Docker container.
    - It is a collection of instructions to set up an environment (e.g., OS, applications).

2.  **Key Points**:

    - **Images are static**: They are just a set of instructions and do not run until instantiated as a container.
    - **Layered Architecture**:
      - Each instruction in the Dockerfile creates a new layer in the image.
      - Example:
        - Base layer: Ubuntu OS.
        - Additional layers: Apache web server, application.
    - **Layer Reusability**:
      - Only modified layers are rebuilt during updates, improving efficiency.

3.  **Customization**:

    - An image can be based on another image with added customizations.
      - Example: A MySQL image layered on top of an Ubuntu image.
    - Users can:
      - Use pre-built images from repositories like Docker Hub.
      - Create and publish custom images.

4.  **Dockerfile**:

    - Used to define the instructions for building an image.
    - Contains step-by-step commands (e.g., installing software, copying files).

5.  **Advantages**:

    - Lightweight and fast due to shared OS resources and layered structure.
    - Supports incremental updates, minimizing rebuild efforts.

---

#### **Docker Containers**

1.  **Definition**:

    - A **runnable instance of a Docker image**.
    - Containers take the static instructions from an image and execute them as a process.

2.  **Key Points**:

    - **Lifecycle Management**:
      - Commands like `docker start`, `docker stop`, `docker rm` are used to control containers.
    - **Statefulness**:
      - Containers are **stateless by default**.
      - Changes made within a container (e.g., database updates) are lost if the container is deleted.
      - Persistent storage must be attached for data retention.

3.  **Isolation**:

    - Containers are isolated from:
      - Each other.
      - The host machine.
    - Achieved using Linux **namespaces** (isolation of processes) and **control groups** (resource allocation).

4.  **Reusability**:

    - The current state of a container can be saved as a new image, allowing reuse elsewhere.

5.  **Networking**:

    - Containers can be connected to one or more networks.
    - They can share or isolate resources as needed.

6.  **Persistent Storage**:

    - Essential for retaining data beyond the container's lifecycle.
    - Without persistent storage, data within the container is lost upon deletion.

7.  **Use Case Example**:

    - Base Image: Ubuntu → Add Layer: Apache → Add Layer: Application.
    - Result: A container runs this stack and can serve web pages or process data.

---

#### **Comparison of Docker Images vs. Containers**

| Aspect          | Docker Image                        | Docker Container                      |
| --------------- | ----------------------------------- | ------------------------------------- |
| **Definition**  | A template with setup instructions. | A running instance of an image.       |
| **State**       | Static and read-only.               | Dynamic and mutable during runtime.   |
| **Purpose**     | Acts as a blueprint.                | Executes processes as defined.        |
| **Persistence** | Does not retain data.               | Requires persistent storage for data. |
