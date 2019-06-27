$ kubectl run my-nginx --image=nginx --replicas=2 --port=80

deployment "my-nginx" created

To expose your service to the public internet, run:



$ kubectl expose deployment my-nginx --target-port=80 --type=LoadBalancer

service "my-nginx" exposed

You can see that they are running by:



$ kubectl get po

NAME                                READY     STATUS    RESTARTS   AGE

my-nginx-3800858182-h9v8d           1/1       Running   0          1m

my-nginx-3800858182-wqafx           1/1       Running   0          1m

Kubernetes will ensure that your application keeps running, by automatically restarting containers that fail, spreading containers across nodes, and recreating containers on new nodes when nodes fail.



To find the public IP address assigned to your application, execute:



$ kubectl get service my-nginx

NAME         CLUSTER\_IP       EXTERNAL\_IP       PORT\(S\)                AGE

my-nginx     10.179.240.1     25.1.2.3          80/TCP                 8s

You may need to wait for a minute or two for the external ip address to be provisioned.



In order to access your nginx landing page, you also have to make sure that traffic from external IPs is allowed. Do this by opening a firewall to allow traffic on port 80.



If you’re running on AWS, Kubernetes creates an ELB for you. ELBs use host names, not IPs, so you will have to do kubectl describe service/my-nginx and look for the LoadBalancer Ingress host name. Traffic from external IPs is allowed automatically.



Killing the application

To kill the application and delete its containers and public IP address, do:



$ kubectl delete deployment,service my-nginx

deployment "my-nginx" deleted

service "my-nginx" deleted

