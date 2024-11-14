# docker-nginx-application
Building a custom docker image using Ubuntu as a base docker image and running the nginx application. This docker image is built using a Dockerfile.

Step 1: Install Docker

```
#!/bin/bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

# To install the latest version, run:
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Verify that the Docker Engine installation is successful by running the hello-world image.
sudo docker run hello-world

```
Start and Enable Docker:

Start the Docker service and enable it to run on system startup.
```
sudo systemctl start docker
sudo systemctl enable docker
```

Verify Docker Installation:

Run the following command to check if Docker was installed correctly.
```
docker --version
```

Step 2: Set Up Project Directory and Dockerfile
Create a Project Directory:
```
mkdir nginx-docker
cd nginx-docker
```
Create a Dockerfile:
```
touch Dockerfile
```
Step 3: Write the Dockerfile
Open the Dockerfile and add the following content:
```
# Use Ubuntu as the base image
FROM ubuntu:latest

# Update package list and install NGINX
RUN apt-get update && \
    apt-get install -y nginx && \
    rm -rf /var/lib/apt/lists/*

# Expose port 80 for HTTP traffic
EXPOSE 80

# Start NGINX when the container starts
CMD ["nginx", "-g", "daemon off;"]

```
Step 4: Build the Docker Image
Run the following command to build the Docker image from the Dockerfile:
The -t flag tags the image as custom-nginx for easy reference.
```
docker build -t custom-nginx .
```

Step 5: Run the Docker Container with Host Network Mode
Run the container with the host network to make it accessible on the host’s public IP:
1. -d refers o run in detached mode. 
2. The --network host option tells Docker to use the host network mode for the container.
```
docker run -d --name my-nginx-container --network host custom-nginx
```
Step 6: Test Access on Public IP
Get the Host's Public IP
Access NGINX:
Open a browser(incognito mode preferred) and go to your public IP (http://<your-public-ip>) to view the default NGINX welcome page.
![Uploading image.png…]()


Notes:
1. Run the following command to check if your NGINX container is up and running.
   ```
   docker ps
   ```
2. If the container is not running, try to start it:
   ```
   docker start <my-nginx-container>
   ```
3. Check the logs of the NGINX container for any errors that might have occurred.
   ```
   docker logs <my-nginx-container>
   ```
4. To see if the image is created
   ```
   docker images
   ```
   
