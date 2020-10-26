---
title: "Pod"
date: 2020-10-26T16:54:28+05:30
---

{{% notice info %}}
A pod is the smallest object you can create in kubernetes. It is the single instance of application
{{% /notice %}}

{{% notice note %}}
Kubernetes uses yaml file as input. A kubernetes definition file always contains apiVersion, kind, metadata and spec
{{% /notice %}}

**apiVersion:** Based on what we trying to creation, apiVersion will vary

**kind:**  Type of object which we trying to create

**metadata:** Data about object

**spec:** Specification of object. Details about container to create

Create the pod definition file say `pod.yml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: prometheus-pod
  labels:
    app: prometheus
    type: monitoring
spec:
  containers:
    - name: prometheus
      image: prom/prometheus
```

Run the command `kubectl create -f pod.yml --dry-run=client` or `kubectl apply -f pod.yml --dry-run=client` to do dry run this will help to catch issues if any

```go
slashpai@pai  ~/github/myrepo/kube_manifests  (⎈ |minikube:default) kubectl create -f pod.yml --dry-run=client
pod/prometheus-pod created (dry run)
```

Run the command `kubectl apply -f pod.yml` or `kubectl create -f pod.yml` to create pod

Run the command to see the pods `kubectl get pods`. Since we haven't specified any particular namespace it will create pod in default namespace

```go
slashpai@pai  ~/github/myrepo/kube_manifests  (⎈ |minikube:default) kubectl get pods
NAME             READY   STATUS    RESTARTS   AGE
prometheus-pod   1/1     Running   0          119s
slashpai@pai  ~/github/myrepo/kube_manifests  (⎈ |minikube:default)
```

Run the command `kubectl describe pod prometheus-pod` to see details about pod particularly the events section

```go
slashpai@pai  ~/github/myrepo/kube_manifests  (⎈ |minikube:default) kubectl describe pod prometheus-pod
Name:         prometheus-pod
Namespace:    default
Priority:     0
Node:         minikube/192.168.49.2
Start Time:   Mon, 26 Oct 2020 17:10:24 +0530
Labels:       app=prometheus
              type=monitoring
Annotations:  <none>
Status:       Running
IP:           172.17.0.3
IPs:
  IP:  172.17.0.3
Containers:
  prometheus:
    Container ID:   docker://75779b15aa1a36ab02305ddce8c3578ee8cb38f18b9c3424754903852cb4aa85
    Image:          prom/prometheus
    Image ID:       docker-pullable://prom/prometheus@sha256:60190123eb28250f9e013df55b7d58e04e476011911219f5cedac3c73a8b74e6
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Mon, 26 Oct 2020 17:10:38 +0530
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-gkff4 (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  default-token-gkff4:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-gkff4
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                 node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  25s                      Successfully assigned default/prometheus-pod to minikube
  Normal  Pulling    25s   kubelet, minikube  Pulling image "prom/prometheus"
  Normal  Pulled     13s   kubelet, minikube  Successfully pulled image "prom/prometheus" in 12.169051807s
  Normal  Created    12s   kubelet, minikube  Created container prometheus
  Normal  Started    12s   kubelet, minikube  Started container prometheus
 slashpai@pai  ~/github/myrepo/kube_manifests  (⎈ |minikube:default) 
```

Run the command `kubectl logs prometheus-pod` to view the logs of pod we just created. To stream logs `kubectl logs -f prometheus-pod`
