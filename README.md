# Docker Local Installation Steps
Steps for installing docker and docker-compose without any internet connection on a machine

## Steps

1. Download the docker binary from https://download.docker.com/linux/static/stable/
2. Download the docker-compose binary from https://github.com/docker/compose/releases
2. Copy it to your local machine via rsync or filezilla
3. Execute the following commands to install docker and docker-compose



```
tar xzvf /path/to/<FILE>.tar.gz # Docker binary tar file
sudo cp docker/* /usr/bin/
sudo dockerd &
mv docker-compose-linux-version docker-compose
chmod +x docker-compose
sudo cp docker-compose /usr/bin
```
In order to have the docker images load in another path instead of the default path which is /var
we need to change our image installation directory. A sample docker.service file and containerd.service along
with their socket files have been provided in the repository.

Change the image save directory path in ExecStart variable in docker.service file.
It is specified as -g /data/docker-images in the docker.service file. Please note
that the path folder has to be created before specifying in the file.

Copy the service and socket files to the /etc/systemd/system/ directory using the following command
```
sudo cp file_name /etc/systemd/system/
```
Now since you have already run the dockerd command. It can cause issues in starting the service.
Run the following command first to get the pid of the dockerd
```
cat /run/docker/docker.pid
kill <process_id>
```
Now we can start the docker service through
```
sudo systemctl daemon-reload
sudo systemctl start docker
```
That's it. You're done!
