## Docker
*From: https://docs.docker.com/engine/install/ubuntu/*

### Uninstall and remove old stuffs

At first, to completely uninstall any Docker in a Debian based system:

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```


- Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:

  ```bash
   sudo rm -rf /var/lib/docker
   sudo rm -rf /var/lib/containerd
  ```

  At this pont, all Docker stuffs should been completely removed from the system.
### Docker install



### Install using the repository

Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

#### Set up the repository

1. Update the `apt` package index and install packages to allow `apt` to use a repository over HTTPS:

   ```bash
   sudo apt-get update
   
   sudo apt-get install \
       ca-certificates \
       curl \
       gnupg \
       lsb-release
   ```

2. Add Docker’s official GPG key:

   ```bash
   sudo mkdir -p /etc/apt/keyrings
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
   ```

3. Use the following command to set up the repository:

   ```bash
   echo \
     "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
     $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

#### Install Docker Engine

1. Update the `apt` package index:

   ```
   sudo apt-get update
   ```

2. Install Specific version of Docker Engine, containerd, and Docker Compose.

   To install a specific version of Docker Engine, start by list the available versions in the repository:

   ```bash
   # List the available versions:
   apt-cache madison docker-ce | awk '{ print $3 }'
   ----
   5:20.10.22~3-0~ubuntu-jammy
   5:20.10.21~3-0~ubuntu-jammy
   5:20.10.20~3-0~ubuntu-jammy
   5:20.10.19~3-0~ubuntu-jammy
   5:20.10.18~3-0~ubuntu-jammy
   5:20.10.17~3-0~ubuntu-jammy
   5:20.10.16~3-0~ubuntu-jammy
   5:20.10.15~3-0~ubuntu-jammy
   5:20.10.14~3-0~ubuntu-jammy
   5:20.10.13~3-0~ubuntu-jammy
   ----
   
   apt-cache madison docker-compose-plugin | awk '{ print $3 }'
   ----
   2.12.2~ubuntu-jammy
   2.12.0~ubuntu-jammy
   2.11.2~ubuntu-jammy
   2.10.2~ubuntu-jammy
   2.6.0~ubuntu-jammy
   2.5.0~ubuntu-jammy
   2.3.3~ubuntu-jammy
   ----
   
   apt-cache madison containerd.io | awk '{ print $3 }'
   ----
   1.6.10-1
   1.6.9-1
   1.6.8-1
   1.6.7-1
   1.6.6-1
   1.6.4-1
   1.5.11-1
   1.5.10-1
   ----
   
   
   ```

   ```bash
   
   ```

   

   ```bash
   
   ```

   

   

   Select the desired version and install:

   

   ```BASH
   DOCKER_VERSION_STRING=5:20.10.22~3-0~ubuntu-jammy
   DOCKER_COMPOSE_VERSION_STRING=2.14.1~ubuntu-jammy
   CONTEINERD_VERSION_STRING=1.6.10-1
   
   sudo apt-get install docker-ce=$DOCKER_VERSION_STRING docker-ce-cli=$DOCKER_VERSION_STRING containerd.io=$CONTEINERD_VERSION_STRING docker-compose-plugin=$DOCKER_COMPOSE_VERSION_STRING
   ```

   ------



Hold the versions with

```bash
sudo apt-mark hold docker-ce docker-ce-cli containerd.io docker-compose-plugin
```





1. Verify that the Docker Engine installation is successful by running the `hello-world` image:

   

   ```
   sudo docker run hello-world
   ```

   This command downloads a test image and runs it in a container. When the container runs, it prints a confirmation message and exits.

You have now successfully installed and started Docker Engine. The `docker` user group exists but contains no users, which is why you’re required to use `sudo` to run Docker commands. Continue to [Linux post-install](https://docs.docker.com/engine/install/linux-postinstall/) to allow non-privileged users to run Docker commands and for other optional configuration steps.



- To run docker without `sudo`,  create the docker `group` and add your `user`. Create the docker `group`:

    ```bash
    sudo groupadd docker
    ```

- Add your user to the docker group.

    ```bash
    sudo usermod -aG docker $USER
    ```

- After that, reboot the system to apply this changes

    ​    

- Then, verify that Docker Engine is installed correctly by running the hello-world image.

    ```bash
    sudo docker run hello-world
     ----
     Unable to find image 'hello-world:latest' locally
    latest: Pulling from library/hello-world
    2db29710123e: Pull complete 
    Digest: sha256:cc15c5b292d8525effc0f89cb299f1804f3a725c8d05e158653a563f15e4f685
    Status: Downloaded newer image for hello-world:latest
    
    Hello from Docker!
    This message shows that your installation appears to be working correctly.
    
    To generate this message, Docker took the following steps:
    
      1. The Docker client contacted the Docker daemon.
      2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
         (amd64)
      3. The Docker daemon created a new container from that image which runs the
         executable that produces the output you are currently reading.
      4. The Docker daemon streamed that output to the Docker client, which sent it
         to your terminal.
    
    To try something more ambitious, you can run an Ubuntu container with:
     $ docker run -it ubuntu bash
    
    Share images, automate workflows, and more with a free Docker ID:
     https://hub.docker.com/
    
    For more examples and ideas, visit:
    
     https://docs.docker.com/get-started/
    -------------------
    ```

- Enable the docker to run as a service from the boot time

    ```bash
    sudo systemctl enable docker
    ```

- Verify the running status of the Docker service running

    ```bash
    sudo systemctl status docker.service
    ```

       
