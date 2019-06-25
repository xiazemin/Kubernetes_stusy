```
docker run \
    --volume=/:/rootfs:ro \
    --volume=/sys:/sys:ro \
    --volume=/dev:/dev \
    --volume=/var/lib/docker/:/var/lib/docker:ro \
    --volume=/var/lib/kubelet/:/var/lib/kubelet:rw \
    --volume=/var/run:/var/run:rw \
    --net=host \
    --pid=host \
    --privileged=true \
    -d \
    gcr.io/google_containers/hyperkube:v1.0.1 \
    /hyperkube kubelet --containerized --hostname-override="127.0.0.1" --address="0.0.0.0" --api-servers=http://localhost:8080 --config=/etc/kubernetes/manifests
```

问题

```
c643c3c43a2d2567035acdd48fe84c1516ab3800c41eaf28e837b0e7e730642e
docker: Error response from daemon: Mounts denied:
The path /var/lib/kubelet/
is not shared from OS X and is not known to Docker.
You can configure shared paths from Docker -> Preferences... -> File Sharing.
See https://docs.docker.com/docker-for-mac/osxfs/#namespaces for more info.
..
```

```
docker ps -a
CONTAINER ID        IMAGE                                       COMMAND                  CREATED             STATUS              PORTS               NAMES
c643c3c43a2d        gcr.io/google_containers/hyperkube:v1.0.1   "/hyperkube kubele..."   2 minutes ago       Created                                 kind_engelbart
bfc68c4b23ff        gcr.io/google_containers/hyperkube:v1.0.1   "/hyperkube kubele..."   6 minutes ago       Created                                 modest_hopper
c06761c40125        gcr.io/google_containers/etcd:2.0.12        "/usr/local/bin/et..."   12 minutes ago      Up 12 minutes                           romantic_bardeen

15:42:08-didi@localhost:~$docker rm c643c3c43a2d
c643c3c43a2d
15:42:48-didi@localhost:~$docker rm bfc68c4b23ff
bfc68c4b23ff

$docker images
REPOSITORY                                             TAG                 IMAGE ID            CREATED             SIZE
hub.c.163.com/mrjucn/centos6.5-mysql5.1-php5.7-nginx   latest              726cb1dfd4b7        2 years ago         2.78 GB
gcr.io/google_containers/hyperkube                     v1.0.1              1572ebab2007        3 years ago         217 MB
gcr.io/google_containers/etcd                          2.0.12              8c32a2c9996f        4 years ago         15.3 MB
15:43:10-didi@localhost:~$docker rmi 1572ebab2007
Untagged: gcr.io/google_containers/hyperkube:v1.0.1
Untagged: gcr.io/google_containers/hyperkube@sha256:555762fdbe88eb6d731aaed02d53123d7761585d8241b15fe63b5ebfa4c0c5dd
Deleted: sha256:1572ebab200708878dc0831c4746bdebe1ca54fcb47dc4b5cb07fa0ec2803f09
Deleted: sha256:475c83e77b36f42a35413803efa2de07bb608dad8fa377d992f30c9991d27d0c
Deleted: sha256:99fe3b31b89a3b5cfac03f0f305db98c0496cccc9cb42a204732467efca6cf5a
Deleted: sha256:d5138d1c9663cdbd047b4800410859528207eda08b08ccf17d386f6562e7b538
Deleted: sha256:337e49cb45458c8f3790e1813108f8c693ddbf0e3e3f09c8d4ab901228951e99
Deleted: sha256:266c830c17143220fa51b56e41c4731679f004cca124419a78d0f961c699146a
Deleted: sha256:017eb1d9fb5cb13633d0d6d7a0fc6e285d29fbc6b3a63ac1b47e53f0bf10e16c
Deleted: sha256:70e94b0f3e48ec9ae822d19fb9d65708c603760bdd811e39316d04e13de94eeb
Deleted: sha256:0a630ebfbac8f201ec71d389eeb7b930c94dcd7cbceedb2309a1e43f833752f4
Deleted: sha256:0dfae534a4825d9cd5c9ce8d4177d58d39226c6805a4f7e476ab231a3259b088
Deleted: sha256:170b376f64fb30995c140276be3d71dfb256b308d86183ca3b22aa93a79ad548
```

清理干净后重新安装

```

```



