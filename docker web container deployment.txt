# Running Ubuntu EC2 instance. SSH into your instance
# install Docker

curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh

# Check if Docker is installed
docker version

# Run this command to avoid any permission related errors from Docker socket. With this command, you are giving full permissions to owner/group/others to the docker socket.
sudo chmod 666 /var/run/docker.sock

#WARNING
#The Docker daemon binds to a Unix socket, not a TCP port. By default, it’s the root user that owns the Unix socket, and other users can only access it using sudo. The Docker daemon always runs as the root user.

# Alternative is If you don’t want to preface the docker command with sudo,or give full permissions to all the users, create a Unix group called docker and add users to it (whoever you need to give access). When the Docker daemon starts, it creates a Unix socket accessible by members of the docker group.

# Create a docker group.
sudo groupadd docker

# Add your user to the docker group.
sudo usermod -aG docker $USER

# Log out and log back in so that your group membership is re-evaluated. If you’re running Linux in a virtual machine, it may be necessary to restart the virtual machine for changes to take effect.
newgrp docker

# Check that Swarm is not active right now.
docker info


# Configure the advertise address for manager node
docker swarm init --advertise-addr <MANAGER-IP>

# The output will return a token to join swarm as a worker node

# Run this command on all nodes you want to create as workers and join swarm
docker swarm join --token <token> <ip-address:2377>

# To show the status of all nodes
docker node ls

# Create docker service to run NGINX container and publish on port 80
docker service create --name nginx_web -p 80:80 nginx:latest

# To list all services
docker service ls

# To check what node our service is running on
docker service ps <service name>

# Create multiple replicas of existing service
docker service update <service name> -- replicas 3

# Remove the service with all replicas
docker service rm <service name>




























