## Container > Container Registry > User Guide

## Prerequisites 
### Install Docker 
The Container Registry service is provided to save and deploy Docker container images. To enable the service, Docker must be installed on user's environment. 

#### Windows
On Docker Hub, download and install [Docker Desktop for Windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows).

#### macOS
On Docker Hub, download and install [Docker Desktop for Mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac).

#### Linux
Each Linux distribution requires different installation procedure. If you're not using CentOS or Ubuntu, check [Install Docker Engine](https://docs.docker.com/engine/install).

* CentOS
```
// Install package required for docker installation
$ sudo yum install -y yum-utils device-mapper-persistent-data lvm2

// Add Docker registry 
$ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

// Install Docker 
$ sudo yum install -y docker-ce docker-ce-cli containerd.io

// Start Docker 
$ sudo systemctl start docker
```

* Ubuntu
```
// Update apt index 
$ sudo apt-get update

// Install package to enable repository over HTTPS 
$ sudo apt-get install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common

// Add GPG key and confirm 
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo apt-key fingerprint 0EBFCD88

pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]

// Add repository and update apt index 
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
$ sudo apt-get update

// Install Docker
$ sudo apt-get install docker-ce docker-ce-cli containerd.io

// Start Docker 
$ sudo systemctl start docker
```

### Check Appkey 
To login to user registry by using Docker command line (CLI), service appkey or integrated project appkey is required. To check your service appkey, go to **Container > Container Registry** and click **URL & Appkey**. You may create an integrated project appkey from **API Security Setting on the Project Setting page**. 

## How to Use Container Registry 

> [Note]
> Member users are not allowed to save or delete container images.

### Check Address of User Registry
To check the address of user registry, go to **Container > Container Registry** and click **Registry Address**.

### Login to User Registry 
To save or import container images to a certain environment, Docker command line is required. Login first to access user registry by using Docker CLI; login requires email address of TOAST user account, as well as service appkey, or integrated appkey of a project in which the Container Registry service is activated.   

```
$ docker login {User registry address}
Username: {Email address of TOAST user account}
Password: {Appkey}
Login Succeeded
```

### Create Tags
To save container images at a user registry, name of a repository including user registry address, and tags are required. To create a tag, use the **tag** command of Docker CLI. 

```
$ docker tag {Repository name}:{tag} {User registry address}/{Repository name}:{tag}
```

* Example 
```
$ docker tag ubuntu:18.04 example-kr1-registry.container.cloud.toast.com/ubuntu:18.04
```

> [Note]
> Name of a container image ([Repository name]:[Tag name])allows small-case letters, numbers, and some special characters (-, .) only. A name cannot be longer than 255, including registry address, and a tag cannot be longer than 129 characters. A name is recommended not to be too long, though.   

### Save (Push) Container Images  
Use the **push** command of Docker CLI to save container images at a user registry.  

```
$ docker push {User registry address}/{Repository name}:{Tag name}
```

* Example 
```
$ docker push example-kr1-registry.container.cloud.toast.com/ubuntu:18.04
The push refers to repository [example-kr1-registry.container.cloud.toast.com/ubuntu]
16542a8fc3be: Pushed
6597da2e2e52: Pushed
977183d4e999: Pushed
c8be1b8f4d60: Pushed
18.04: digest: sha256:e5dd9dbb37df5b731a6688fa49f4003359f6f126958c9c928f937bec69836320 size: 1152
```

### Query Container Images 
Saved container images are available on web console. 

* List of Repositories 
Go to **Container > Container Registry** and find the list of image repositories.  

* List of Tags
Click a repository from the list to query the list of tags saved in the selected repository. Select a tag of choice and find its details. With given addresses, tags can be deployed to an environment in need.      

### Pull Container Images 
Pull images with the **pull** command of Docker CLI. Confirm the tag address of the image to pull from web console. 

```
$ docker pull {Tag address}
```

* Example 
```
$ docker pull example-kr1-registry.container.cloud.toast.com/ubuntu:18.04
18.04: Pulling from ubuntu
5bed26d33875: Pull complete
f11b29a9c730: Pull complete
930bda195c84: Pull complete
78bf9a5ad49e: Pull complete
Digest: sha256:e5dd9dbb37df5b731a6688fa49f4003359f6f126958c9c928f937bec69836320
Status: Downloaded newer image for example-kr1-registry.container.cloud.toast.com/ubuntu:18.04
example-kr1-registry.container.cloud.toast.com/ubuntu:18.04

$ docker images
REPOSITORY                                              TAG     IMAGE ID        CREATED         SIZE
example-kr1-registry.container.cloud.toast.com/ubuntu   18.04   4e5021d210f6    12 days ago     64.2MB
```

### Delete Container Images
Select a tag from the list and click **Delete Tags** and delete it. 
