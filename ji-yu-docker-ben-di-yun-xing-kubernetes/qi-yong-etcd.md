```
$   docker run --net=host -d gcr.io/google_containers/etcd:2.0.12 /usr/local/bin/etcd --addr=127.0.0.1:4001 --bind-addr=0.0.0.0:4001 --data-dir=/var/etcd/data
Unable to find image 'gcr.io/google_containers/etcd:2.0.12' locally
2.0.12: Pulling from google_containers/etcd
a3ed95caeb02: Pull complete
35c8bf5fd6cd: Pull complete
a7e0d6960478: Pull complete
3109a5487eac: Pull complete
Digest: sha256:24cf1202eea3953f9a8c44b0930d03666019ff8c277a0f6cd6190645eb1f7ba5
Status: Downloaded newer image for gcr.io/google_containers/etcd:2.0.12
c06761c401256525c7daba5eb6d9d56c292c6f2f324dd865c8e8338f1fd6496b
```

```
$docker ps
CONTAINER ID        IMAGE                                  COMMAND                  CREATED             STATUS              PORTS               NAMES
c06761c40125        gcr.io/google_containers/etcd:2.0.12   "/usr/local/bin/et..."   18 seconds ago      Up 17 seconds                           romantic_bardeen
```

```
$docker images
REPOSITORY                                             TAG                 IMAGE ID            CREATED             SIZE
hub.c.163.com/mrjucn/centos6.5-mysql5.1-php5.7-nginx   latest              726cb1dfd4b7        2 years ago         2.78 GB
gcr.io/google_containers/etcd                          2.0.12              8c32a2c9996f        4 years ago         15.3 MB
```



