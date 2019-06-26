ConfigMap用于保存配置数据的键值对，可以用来保存单个属性，也可以用来保存配置文件。ConfigMap跟secret很类似，但它可以更方便地处理不包含敏感信息的字符串。



ConfigMap创建

可以使用kubectl create configmap从文件、目录或者key-value字符串创建等创建ConfigMap。



\# 从key-value字符串创建ConfigMap

$ kubectl create configmap special-config --from-literal=special.how=very

configmap "special-config" created



```
$ kubectl get configmap special-config -o go-template='{{.data}}'
```



map\[special.how:very\]



\# 从env文件创建

$ echo -e "a=b\nc=d" \| tee config.env

a=b

c=d

$ kubectl create configmap special-config --from-env-file=config.env

configmap "special-config" created



```
$ kubectl get configmap special-config -o go-template='{{.data}}'
```



map\[a:b c:d\]



\# 从目录创建

$ mkdir config

$ echo a&gt;config/a

$ echo b&gt;config/b

$ kubectl create configmap special-config --from-file=config/

configmap "special-config" created



```
$ kubectl get configmap special-config -o go-template='{{.data}}'
```



map\[a:a

 b:b

\]

ConfigMap使用

ConfigMap可以通过多种方式在Pod中使用，比如设置环境变量、设置容器命令行参数、在Volume中创建配置文件等。



注意



ConfigMap必须在Pod引用它之前创建

使用envFrom时，将会自动忽略无效的键

Pod只能使用同一个命名空间内的ConfigMap

用作环境变量

首先创建ConfigMap：



$ kubectl create configmap special-config --from-literal=special.how=very --from-literal=special.type=charm

$ kubectl create configmap env-config --from-literal=log\_level=INFO

然后以环境变量方式引用



apiVersion: v1

kind: Pod

metadata:

  name: test-pod

spec:

  containers:

    - name: test-container

      image: gcr.io/google\_containers/busybox

      command: \[ "/bin/sh", "-c", "env" \]

      env:

        - name: SPECIAL\_LEVEL\_KEY

          valueFrom:

            configMapKeyRef:

              name: special-config

              key: special.how

        - name: SPECIAL\_TYPE\_KEY

          valueFrom:

            configMapKeyRef:

              name: special-config

              key: special.type

      envFrom:

        - configMapRef:

            name: env-config

  restartPolicy: Never

当pod运行结束后，它的输出会包括



SPECIAL\_LEVEL\_KEY=very

SPECIAL\_TYPE\_KEY=charm

log\_level=INFO

用作命令行参数

将ConfigMap用作命令行参数时，需要先把ConfigMap的数据保存在环境变量中，然后通过$\(VAR\_NAME\)的方式引用环境变量.



apiVersion: v1

kind: Pod

metadata:

  name: dapi-test-pod

spec:

  containers:

    - name: test-container

      image: gcr.io/google\_containers/busybox

      command: \[ "/bin/sh", "-c", "echo $\(SPECIAL\_LEVEL\_KEY\) $\(SPECIAL\_TYPE\_KEY\)" \]

      env:

        - name: SPECIAL\_LEVEL\_KEY

          valueFrom:

            configMapKeyRef:

              name: special-config

              key: special.how

        - name: SPECIAL\_TYPE\_KEY

          valueFrom:

            configMapKeyRef:

              name: special-config

              key: special.type

  restartPolicy: Never

当Pod结束后会输出



very charm

使用volume将ConfigMap作为文件或目录直接挂载

将创建的ConfigMap直接挂载至Pod的/etc/config目录下，其中每一个key-value键值对都会生成一个文件，key为文件名，value为内容



apiVersion: v1

kind: Pod

metadata:

  name: vol-test-pod

spec:

  containers:

    - name: test-container

      image: gcr.io/google\_containers/busybox

      command: \[ "/bin/sh", "-c", "cat /etc/config/special.how" \]

      volumeMounts:

      - name: config-volume

        mountPath: /etc/config

  volumes:

    - name: config-volume

      configMap:

        name: special-config

  restartPolicy: Never

当Pod结束后会输出



very

将创建的ConfigMap中special.how这个key挂载到/etc/config目录下的一个相对路径/keys/special.level。如果存在同名文件，直接覆盖。其他的key不挂载



apiVersion: v1

kind: Pod

metadata:

  name: dapi-test-pod

spec:

  containers:

    - name: test-container

      image: gcr.io/google\_containers/busybox

      command: \[ "/bin/sh","-c","cat /etc/config/keys/special.level" \]

      volumeMounts:

      - name: config-volume

        mountPath: /etc/config

  volumes:

    - name: config-volume

      configMap:

        name: special-config

        items:

        - key: special.how

          path: keys/special.level

  restartPolicy: Never

当Pod结束后会输出



very

参考：https://feisky.gitbooks.io/kubernetes/concepts/configmap.html

