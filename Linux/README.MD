## Package Management (Linux)

```bash
sudo apt update && sudo apt upgrade -y        # Debian/Ubuntu
sudo yum update -y                            # RHEL/CentOS
sudo dnf update -y                            # Fedora/Amazon Linux 2023
```
## System Monitoring

``` bash
top                                            # CPU/RAM live usage
htop                                           # Better interactive top (if installed)
df -h                                          # Disk usage
du -sh *                                       # Directory sizes
free -h                                        # Memory usage
uptime                                         # Load averages
journalctl -xe                                 # Recent system logs
```
## Network Debug

``` bash
ping <host>
curl -I <url>
traceroute <host>
netstat -tulpn                                 # Listening ports and processes
ss -tulpn                                      # Same as netstat but modern
```

## File Permission

``` bash 
chmod +x <file>                                # Make executable
chown <user>:<group> <file>                    # Change owner
tar -czvf archive.tar.gz <folder>              # Compress folder
tar -xzvf archive.tar.gz                       # Extract
```
## Git Basic

``` bash
git clone <repo-url>
git pull
git branch -a
git checkout -b <new-branch>
git merge <branch>
git log --oneline --graph --decorate
```

## Docker Essentials

``` bash
docker ps                                      # Running containers
docker ps -a                                   # All containers
docker images                                  # Images
docker run -it <image> /bin/bash               # Interactive shell
docker build -t <tag> .
docker logs -f <container>
docker exec -it <container> /bin/bash
docker stop <container>
docker rm <container>
docker rmi <image>
```
## Kubernetes Basic

``` bash
kubectl get nodes
kubectl get pods -A
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl exec -it <pod-name> -- sh
kubectl apply -f <file>.yaml
kubectl delete -f <file>.yaml
```
## AWS CLI
``` bash
aws configure
aws ec2 describe-instances
aws s3 ls
aws s3 cp <local-file> s3://<bucket>/
aws s3 cp s3://<bucket>/<file> .
aws cloudformation describe-stacks
aws eks update-kubeconfig --name <cluster>
```

## Terraform Basics
``` bash
terraform init
terraform plan
terraform apply -auto-approve
terraform destroy -auto-approve
terraform fmt
terraform validate
```
## Ansible Basics
``` bash
ansible all -m ping -i inventory.ini
ansible-playbook -i inventory.ini playbook.yaml
```
