# Docker Commands
### Check Docker Version

```shell
docker --version
```
### System Info

```shell
docker info
```
## 2. Working with Images

### Pull an Image
```shell
docker pull nginx
```

### List Images
```shell
docker images
```
### Build an Image
```shell
docker build -t <image-name>:tag .
```

### Remove an Image
```shell
docker rmi <image_id_or_name>
```

## 3. Container Management

### Run a Container
```shell
docker run -d -p 8080:80 --name my-container nginx
```
### List Running Containers
```shell
docker ps
```
### List All Containers
```shell
docker ps -a
```
### Stop a Container
```shell
docker stop <container_id_or_name>
```
### Start a Container
```shell
docker start <container_id_or_name>
```

### Remove a Container

```shell
docker rm <container_id_or_name>
```
### Force Remove
```shell
docker rm -f <container_id_or_name>
```
## 4. Debugging & Interaction
### View Logs
```shell
docker logs <container_id_or_name>
```
### Logs in real-time:
```shell
docker logs -f <container_id_or_name>
```
### Access a Container
```shell
docker exec -it <container_id_or_name> /bash
```
**Note**: Use `/bin` `/sh` if `/bash` is not installed in the image

### Inspect Container
```shell
docker inspect <container_id_or_name>
```
## 5. Docker Network & Volumes
### List Networks
```shell
docker network ls
```
### Create a Network
```shell
docker network create <network-name>
```
### List Volumes
```shell
docker volume ls
```
### Create a Volume
```shell
docker volume create <volume-name>
```
## 6. System Cleanup
### Prune System
```shell
docker system prune
```

## 7. Docker Compose
### Start Services

```shell
docker-compose up
```
### Restart Services
```shell
docker-compose restart <service_name>
```

### Stop Services
```shell
docker-compose down
```

### Start and Rebuild
```shell
docker-compose up --build
```

### View Compose Logs
```shell
docker-compose logs -f
```

### List Compose Containers
```shell
docker-compose ps
```

### Execute Command
```shell
docker-compose exec <service_name> /bin/bash
```

## 8. Useful Flags
### Interactive Mode
```shell
-i
```
### TTY Mode
```shell
-t
```

### Remove Container Automatically
```shell
--rm
```
## 9. Advanced Volume Management
Create a Volume
```shell
docker volume create <volume-name>
```
### List Volumes
```shell
docker volume ls
```

### Inspect a Volume
```shell
docker volume inspect <volume-name>
```

### Remove a Volume

```shell
docker volume rm <volume-name>
```
### Prune Volumes
```shell
docker volume prune
```
### Mount Volume to Container
```shell
docker run --name my-db -v <volume-name>:/var/lib/mysql mysql:latest
```
### Bind Mount (Host Directory)
```shell
docker run --name dev-container -v /path/on/host:/path/in/container nginx
```

## 10. Advanced Networking
### List Networks
```shell
docker network ls
```

### Create a Network
```shell
docker network create <network-name>
```

### Inspect a Network
```shell
docker network inspect <network-name>
```

### Run Container on Network
```shell
docker run -d --name web-server --network <network-name> nginx
```

### Connect Container to Network
```shell
docker network connect <network-name> <container_name>
```

### Disconnect Container
```shell
docker network disconnect <network-name> <container_name>
```

### Remove a Network
```shell
docker network rm <network-name>
```
### Prune Networks
```shell
docker network prune
```
