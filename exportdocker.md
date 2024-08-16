

cara export container 

```bash
docker export -o my-ubuntu-container.tar my-ubuntu-container
```


cara import docker container 

```bash
docker import my-ubuntu-container.tar my-ubuntu-image
```


cara menjalankan docker nya 
```bash
docker run -d -p 3306:3306 -p 8080:80 --name my-new-container my-ubuntu-image /bin/bash -c "service apache2 start && service mysql start && tail -f /dev/null"
```


output program nya 
```bash

❯ docker import my-ubuntu-container.tar my-ubuntu-image
sha256:a1f76640c7afb9032b5b60c519da87d612fdd2361a5244e748769fb9592a3595

❯ docker run -d -p 3306:3306 -p 8080:80 --name my-new-container my-ubuntu-image /bin/bash -c "service apache2 start && service mysql start && tail -f /dev/null"
26bdd611383d455967d5923fc8792d09be5cd819414ca836df681f5ae9e0cd83

❯ docker ps
CONTAINER ID   IMAGE             COMMAND                  CREATED         STATUS         PORTS                                                                              NAMES
26bdd611383d   my-ubuntu-image   "/bin/bash -c 'servi…"   5 seconds ago   Up 5 seconds   0.0.0.0:3306->3306/tcp, :::3306->3306/tcp, 0.0.0.0:8080->80/tcp, :::8080->80/tcp   my-new-container
~/github/dockerdone            

```


[[cara menjalankan docker ubuntu]]
