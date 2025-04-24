# How to create an Ubuntu 22.04 Docker image with ROS2 Humble

Here you can find a guide on how to install Docker on a Raspberry Pi 5 and creating an Ubuntu 22.04 container with ROS2 Humble.
In the next steps I will assume that you are operating from the Raspberry Pi 5 running Raspberry Pi OS (64 bit version), which was the environment I used. If you are struggling to install the OS take a look at this [page](https://www.raspberrypi.com/software/).

---

## Download Docker
Execute the following commands to the terminal:
1. Update the system
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. Set up Docker's ```apt``` repository
   ```bash
   sudo apt-get update
   sudo apt-get install ca-certificates curl
   sudo install -m 0755 -d /etc/apt/keyrings
   sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
   sudo chmod a+r /etc/apt/keyrings/docker.asc
   echo \
     "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
     $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
     sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   sudo apt-get update
   ```

3. Install the latest version of ```docker```
   ```bash
   sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```

4. Add the user to the ```docker``` group
   ```bash
   sudo groupadd docker && \
   sudo usermod -aG docker $USER && \
   newgrp docker
   ```

5. Reboot the system to apply the changes
   ```bash
   reboot
   ```

6. Verify that you can run ```docker``` command without the need to get superuser privileges
   ```bash
   docker run hello-world
   ```
   The above command downloads a test image and runs it inside a container. If everything has been done right it should finish without any error.

---

## Build the Docker image
Follow the next steps to build the Ubuntu container:

1. Clone this repo (but before check if there are some upgrades and install ```git```)
   ```bash
   sudo apt update && \
   sudo apt upgrade -y && \
   sudo apt install git && \
   git clone https://github.com/Soyadrul/ros2_humble_docker.git && \
   cd ros2_humble_docker/
   ```

2. Make the ```entrypoint.sh``` file executable for the current user
   ```bash
   chmod u+x humble/entrypoint.sh
   ```

3. Build the Docker image (this will take approximately ~42 minutes)
   ```bash
   docker compose -f 'compose.yaml' up -d --build
   ```

4. Install VSCode
   ```bash
   sudo apt install code
   ```

5. Start VSCode and install the ```Remote Development``` extension (press ```CTRL+SHIFT+X``` to see the extension panel)

