```
docker run -d --net=host --privileged gcr.io/google_containers/hyperkube:v1.0.1 /hyperkube proxy --master=http://127.0.0.1:8080 --v=2
```

更改端口

```
docker run -d --net=host --privileged gcr.io/google_containers/hyperkube:v1.0.1 \
/hyperkube proxy --master=http://127.0.0.1:8089 --v=2
```

```
$docker run -d --net=host --privileged gcr.io/google_containers/hyperkube:v1.0.1 /hyperkube proxy --master=http://127.0.0.1:8089 --v=2
cc59343a389890370bfb351a48790656be43ef0f21735047030b1d8b8245bb75
```



