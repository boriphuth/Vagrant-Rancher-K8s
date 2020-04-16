# Vagrant-Rancher-K8s
docker run -d --restart=unless-stopped -p 8000:80 -p 4433:443 rancher/rancher

# play-rancher

https://www.linuxsysadmins.com/install-docker-on-rhel-and-centos-linux/
# Installing Docker on Red Hat Enterprise Linux and CentOS Linux 7
```bash
# Step 1: Resolve Dependencies for Docker Container
$ yum install -y yum-utils device-mapper-persistent-data lvm2

# Step 2: Enable repositories for Docker Container
$ cd /etc/yum.repos.d/; curl -O https://download.docker.com/linux/centos/docker-ce.repo

# Step 3: Install Docker Package
$ yum repolist
$ yum install docker-ce docker-ce-cli containerd.io -y

# Step 4: Start and enable the service persistently
$ sudo systemctl start docker
$ sudo systemctl enable docker

# Step 5: Grant non-root user to run privileged commands.
$ sudo usermod -aG docker m6080042

# Step 6: Run a few basic commands
$ docker -v
$ docker info

```

https://www.linuxsysadmins.com/setup-kubernetes-cluster-with-rancher
```bash
# Operating System and Docker Setup
$ cat /etc/redhat-release
$ docker -v

# Start with Setting up
$ sudo docker run --name Rancher_K8s -d --restart=unless-stopped -p 80:80 -p 443:443 rancher/rancher
$ docker ps -a

sudo swapoff -a
sudo sed -i '/ swap / s/^/#/' /etc/fstab

https://rancher.com/docs/rancher/v2.x/en/installation/other-installation-methods/single-node-docker/proxy/

docker run -d --restart=unless-stopped \
-p 80:80 -p 443:443 \
-e HTTP_PROXY="http://webproxy.pln.corp.services:80" \
-e HTTPS_PROXY="http://webproxy.pln.corp.services:80" \
-e NO_PROXY="localhost,127.0.0.1,0.0.0.0,192.168.10.0/24,10.*" \
rancher/rancher:latest

```
https://gist.github.com/superseb/2cf186726807a012af59a027cb41270d
https://stackoverflow.com/questions/48290551/how-to-fix-openshift-pod-start-failed-with-nodeunderdiskpressure

$ curl https://gist.githubusercontent.com/superseb/2cf186726807a012af59a027cb41270d/raw/eaa2d235e7693c2d1c5a2a916349410274bb95a9/cleanup.sh > ./cleanup.sh && chmod a+x ./cleanup.sh && sudo ./cleanup.sh

$ sudo systemctl stop docker && sudo rm -rf /var/lib/docker && sudo systemctl start docker && sudo systemctl status docker

$ docker rm -vf $(docker ps -a -q) && docker rmi -f $(docker images -a -q)


## an error no space left on device
https://success.docker.com/article/no-space-left-on-device-error
```bash
du -d1 -h /var/lib/docker/containers | sort -h
du -d1 -h /var/lib/docker/overlay2   | sort -h

#to remove all log files
find /var/lib/docker/containers/ -type f -name "*.log" -delete

find /var/lib/docker/overlay2/ -type f -name "*.log" -delete

cat /dev/null > /var/lib/docker/containers/container_id/container_log_name

sudo sh -c "cat /dev/null > /var/lib/docker/containers/container_id/container_log_name" 
```

## https://stackoverflow.com/questions/37955942/vagrant-up-vboxmanage-exe-error-vt-x-is-not-available-verr-vmx-no-vmx-code
## Stderr: VBoxManage.exe: error: The native API dll was not found (C:\Windows\system32\WinHvPlatform.dll) (VERR_NEM_NOT_AVAILABLE).

$ bcdedit /set hypervisorlaunchtype off
and reboot your computer. To turn it back on again, run:

$ bcdedit /set hypervisorlaunchtype on
If you receive "The integer data is not valid as specified", try:

$ bcdedit /set hypervisorlaunchtype auto