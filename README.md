### Dockerfile
```
FROM       ubuntu:18.04

RUN apt-get update

RUN apt-get install -y openssh-server
RUN mkdir /var/run/sshd

RUN echo 'root:root' |chpasswd

RUN sed -ri 's/^#?PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config

RUN mkdir /root/.ssh

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

EXPOSE 22

CMD    ["/usr/sbin/sshd", "-D"]
```

### Build image
```
$ docker build -t ubuntu_sshd .
```

### Run container
```
$ docker run -d -P --name test_sshd ubuntu_sshd
```

### Find out what host port the container’s port 22 is mapped to

```
$ docker port test_sshd 22
```

### Connect the ubuntu
```
$ ssh root@x.x.x.x -p xxxx
```

### Build Image and enable new ports with new image
```
$ docker stop your_container_id
$ docker commit you_container_id your_new_image_name
$ docker run -td -p 32679:22 -p 10080:80 -p 10443:443 your_new_image_name
```

### Export image and load the image
```
$ docker save  your_new_image_name > your_new_image_name.tar
$ docker load your_new_image_name.tar
```
