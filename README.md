
# Docker installation on different environments


## Features

- Ubuntu 18.04 and above
- AWS account


## Installation on Ubuntu 18.04 and above...

The Docker installation package available in the official Ubuntu repository may not be the latest version.
To ensure we get the latest version, we’ll install Docker from the official Docker repository.
To do that, we’ll add a new package source, add the GPG key from Docker to ensure the downloads are valid, and then install the package.

First, update your existing list of packages:

```bash
  sudo apt update
```
Next, install a few prerequisite packages which let apt use packages over HTTPS:

```bash
  sudo apt install apt-transport-https ca-certificates curl software-properties-common
```  
Then add the GPG key for the official Docker repository to your system:

```bash
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
``` 
Add the Docker repository to APT sources:

```bash
  sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
```
Next, update the package database with the Docker packages from the newly added repo:

```bash
  sudo apt update
```
Make sure you are about to install from the Docker repo instead of the default Ubuntu repo:

```bash
  apt-cache policy docker-ce
```
You’ll see output like this, although the version number for Docker may be different:

```bash
                            Output of apt-cache policy docker-ce

docker-ce:
  Installed: (none)
  Candidate: 18.03.1~ce~3-0~ubuntu
  Version table:
     18.03.1~ce~3-0~ubuntu 500
        500 https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
```
Notice that docker-ce is not installed, but the candidate for installation is from the Docker repository for Ubuntu 18.04 (bionic).

Finally, install Docker:

```bash
  sudo apt install docker-ce
```

Docker should now be installed, the daemon started, and the process enabled to start on boot. Check that it’s running:

```bash
  sudo systemctl status docker
```
The output should be similar to the following, showing that the service is active and running:

```bash
Output

● docker.service - Docker Application Container Engine
   Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2021-08-09 19:42:32 UTC; 33s ago
     Docs: https://docs.docker.com
 Main PID: 5231 (dockerd)
    Tasks: 7
   CGroup: /system.slice/docker.service
           └─5231 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
```
Installing Docker now gives you not just the Docker service (daemon) but also the docker command line utility, or the Docker client. 
## Insatllation on AWS EC2
If you don't need a local development environment and you prefer to use an Amazon EC2 instance to use Docker, here are the steps to launch an Amazon EC2 instance and install Docker Engine and the Docker CLI.

### To install Docker on an Amazon EC2 instance
  1. Launch an instance with the Amazon Linux 2 or Amazon Linux AMI. For more information, see Launching an instance in the Amazon EC2 User Guide for Linux Instances.

  2. Connect to your instance. For more information, see Connect to your Linux instance in the Amazon EC2 User Guide for Linux Instances.

  3. Update the installed packages and package cache on your instance.

  ```bash
  sudo yum update -y
  ```
  4. Install the most recent Docker Engine package.

Amazon Linux 2

```bash
sudo amazon-linux-extras install docker
```
Amazon Linux 

```bash
sudo yum install docker
```
  5. Start the Docker service.

```bash
sudo service docker start
```
  6. Add the ec2-user to the docker group so you can execute Docker commands without using sudo.

  ```bash
  sudo usermod -a -G docker ec2-user
  ```
  7. Log out and log back in again to pick up the new docker group permissions. You can accomplish this by closing your current SSH terminal window and reconnecting to your instance in a new one. Your new SSH session will have the appropriate docker group permissions.

  8. Verify that the ec2-user can run Docker commands without sudo.
  ```bash
  docker info
  ```

### Note
In some cases, you may need to reboot your instance to provide permissions for the ec2-user to access the Docker daemon. Try rebooting your instance if you see the following error:
```bash
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
```
