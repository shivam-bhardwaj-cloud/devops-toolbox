# Docker Commands
### Check Docker Version

``` bash
docker --version
```
### System Info

``` bash
docker info
```
## 2. Working with Images

### Pull an Image
``` bash
docker pull nginx
```

### List Images
``` bash
docker images
```
### Build an Image
``` bash
docker build -t <image-name>:tag .
```

### Remove an Image
``` bash
docker rmi <image_id_or_name>
```

## 3. Container Management

### Run a Container
``` bash
docker run -d -p 8080:80 --name my-container nginx
```
### List Running Containers
``` bash
docker ps
```
### List All Containers
``` bash
docker ps -a
```
### Stop a Container
``` bash
docker stop <container_id_or_name>
```
### Start a Container
``` bash
docker start <container_id_or_name>
```

### Remove a Container

``` bash
docker rm <container_id_or_name>
```
### Force Remove
``` bash
docker rm -f <container_id_or_name>
```
## 4. Debugging & Interaction
### View Logs
``` bash
docker logs <container_id_or_name>
```
### Logs in real-time:
``` bash
docker logs -f <container_id_or_name>
```
### Access a Container
``` bash
docker exec -it <container_id_or_name> /bash
```
**Note**: Use `/bin` `/sh` if `/bash` is not installed in the image

### Inspect Container
``` bash
docker inspect <container_id_or_name>
```
## 5. Docker Network & Volumes
### List Networks
``` bash
docker network ls
```
### Create a Network
``` bash
docker network create <network-name>
```
### List Volumes
``` bash
docker volume ls
```
### Create a Volume
``` bash
docker volume create <volume-name>
```
## 6. System Cleanup
### Prune System
``` bash
docker system prune
```

## 7. Docker Compose
### Start Services

```bash
docker-compose up
```
### Restart Services
``` bash
docker-compose restart <service_name>
```

### Stop Services
``` bash
docker-compose down
```

### Start and Rebuild
``` bash
docker-compose up --build
```

### View Compose Logs
``` bash
docker-compose logs -f
```

### List Compose Containers
``` bash
docker-compose ps
```

### Execute Command
``` bash
docker-compose exec <service_name> /bin/bash
```

## 8. Useful Flags
### Interactive Mode
``` bash
-i
```
### TTY Mode
``` bash
-t
```

### Remove Container Automatically
``` bash
--rm
```
## 9. Advanced Volume Management
Create a Volume
``` bash
docker volume create <volume-name>
```
### List Volumes
``` bash
docker volume ls
```

### Inspect a Volume
```bash
docker volume inspect <volume-name>
```

### Remove a Volume

``` bash
docker volume rm <volume-name>
```
### Prune Volumes
``` bash
docker volume prune
```
### Mount Volume to Container
``` bash
docker run --name my-db -v <volume-name>:/var/lib/mysql mysql:latest
```
### Bind Mount (Host Directory)
``` bash
docker run --name dev-container -v /path/on/host:/path/in/container nginx
```

## 10. Advanced Networking
### List Networks
```bash
docker network ls
```

### Create a Network
``` bash
docker network create <network-name>
```

### Inspect a Network
``` bash
docker network inspect <network-name>
```

### Run Container on Network
``` bash
docker run -d --name web-server --network <network-name> nginx
```

### Connect Container to Network
``` bash
docker network connect <network-name> <container_name>
```

### Disconnect Container
``` bash
docker network disconnect <network-name> <container_name>
```

### Remove a Network
``` bash
docker network rm <network-name>
```
### Prune Networks
``` bash

docker network prune
```
