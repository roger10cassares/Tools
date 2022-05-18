## Docker
*From: https://docs.docker.com/engine/install/ubuntu/*

### Uninstall and remove old stuffs

At first, to completely uninstall any Docker in a Debian based system. Follow the steps as bellow:

- Uninstall the Docker Engine, CLI, Containerd, and Docker Compose packages:

    ```bash
    sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-compose-plugin
    ```


- Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:

  ```bash
   sudo rm -rf /var/lib/docker
   sudo rm -rf /var/lib/containerd
  ```

  At this pont, all Docker stuffs should been completely removed from the system.
### Docker install

Please, install the same Docker stuffs version in all machines that should contemplate the cluster

- Set up the repository and update the apt package index and install packages to allow apt to use a repository over HTTPS:

    ```bash
    sudo apt-get update
    sudo apt-get install \
     ca-certificates \
     curl \
     gnupg \
     lsb-release 
    ```

- Add Docker’s official GPG key:

    ```bash
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    ```

- Use the following command to set up the stable repository. To add the nightly or test repository, add the word nightly or test (or both) after the word stable in the commands below. Learn about nightly and test channels.

  ```bash
  echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  ```

- Update the apt package to enable the latest releases for the Docker Engine and containerd:

    ```bash
     sudo apt update
    ```

- To install a specific version of `docker-ce`, `docker-ce-cli`,  `containerd.io`, and `docker-compose-plugin`, list the available versions in the repo, then select and install:

    - List the versions available in the repo:

        ```bash
        apt-cache madison docker-ce
        ----
         docker-ce | 5:20.10.16~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
         docker-ce | 5:20.10.15~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
         docker-ce | 5:20.10.14~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
         docker-ce | 5:20.10.13~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
        ----
        
        apt-cache madison docker-ce-cli 
        ----
        docker-ce-cli | 5:20.10.16~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
        docker-ce-cli | 5:20.10.15~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
        docker-ce-cli | 5:20.10.14~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
        docker-ce-cli | 5:20.10.13~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
        ----
        
        apt-cache madison containerd.io
        ----
        containerd.io |    1.6.4-1 | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
        containerd.io |   1.5.11-1 | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
        containerd.io |   1.5.10-1 | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
        ----
        
        apt-cache madison docker-compose-plugin
        ----
        docker-compose-plugin | 2.5.0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
        docker-compose-plugin | 2.3.3~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
        ```

    - Install a specific version using the version string from the second column, for example,
       `5:20.10.14~3-0~ubuntu-jammy`

       ```bash
       sudo apt-get install docker-ce=5:20.10.16~3-0~ubuntu-jammy docker-ce-cli=5:20.10.16~3-0~ubuntu-jammy containerd.io=1.6.4-1 docker-compose-plugin=2.5.0~ubuntu-jammy
       ```

       

- To run docker without `sudo`,  create the docker `group` and add your `user`. Create the docker `group`:

    ```bash
    sudo groupadd docker
    ```

- Add your user to the docker group.

    ```bash
    sudo usermod -aG docker $USER
    ```

- After that, reboot the system to apply this changes

    

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

       
