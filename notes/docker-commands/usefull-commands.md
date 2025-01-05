| **Docker Command**          |                      **Command Explanation**                      |                        **Examples** |
| :-------------------------- | :---------------------------------------------------------------: | ----------------------------------: |
| `docker run`                |          Runs a container from a specified Docker image           |            `docker run hello-world` |
| `docker ps`                 |                     Lists running containers                      |                         `docker ps` |
| `docker build`              |              Builds a Docker image from a Dockerfile              |        `docker build -t my-image .` |
| `docker stop`               |               Stops one or more running containers                |          `docker stop container_id` |
| `docker rm`                 |                    Removes stopped containers                     |            `docker rm container_id` |
| `docker rmi`                |                    Removes one or more images                     |               `docker rmi image_id` |
| `docker exec`               |             Runs a command inside a running container             | `docker exec -it container_id bash` |
| `docker logs`               |                    Fetches logs of a container                    |          `docker logs container_id` |
| `docker-compose up`         |       Starts services defined in a docker-compose.yml file        |              `docker-compose up -d` |
| `docker-compose down`       |       Stops and removes services defined in docker-compose        |               `docker-compose down` |
| `docker container prune -f` | Removes all stopped containers. The `-f` flag skips confirmation. |         `docker container prune -f` |
