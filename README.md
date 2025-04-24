# Guide on how to create an Ubuntu 22.04 Docker image with ROS2 Humble

Here you can find everything you need to install Docker on a Raspberry Pi 5 running Raspberry Pi OS (64 bit version) and then making an Ubuntu 22.04 container.

---

## Download Docker
Copy the following commands to the terminal:
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

6. Verify that you can run ```docker``` command without the need to get superuser privileges (```sudo```)
   ```bash
   docker run hello-world
   ```
   The command found above downloads a test image and runs inside a container. If everything has been done right it should run without any error.
