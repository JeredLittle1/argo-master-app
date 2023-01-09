## Description
If you can't see the metrics for pods (CPU/Memory show 0), make sure to install the metric server locally with:

```
helm repo add metrics-server https://kubernetes-sigs.github.io/metrics-server/

helm repo update

helm upgrade --install --set args={--kubelet-insecure-tls} metrics-server metrics-server/metrics-server --namespace kube-system
```

Give the installation a few minutes before running:

```
kubectl top pod -A
```

You should now be able to see the pod metrics:

```
NAMESPACE            NAME                                         CPU(cores)   MEMORY(bytes)   
kube-system          coredns-565d847f94-kf2sq                     3m           15Mi            
kube-system          coredns-565d847f94-sjg7t                     3m           15Mi            
kube-system          etcd-kind-control-plane                      28m          32Mi            
kube-system          kindnet-jb42d                                1m           8Mi             
kube-system          kindnet-mm9td                                1m           9Mi             
kube-system          kube-apiserver-kind-control-plane            44m          311Mi           
kube-system          kube-controller-manager-kind-control-plane   24m          47Mi            
kube-system          kube-proxy-gws6v                             2m           14Mi            
kube-system          kube-proxy-nb646                             2m           15Mi            
kube-system          kube-scheduler-kind-control-plane            4m           21Mi            
kube-system          metrics-server-54c4d8c9df-9dxt5              3m           18Mi            
local-path-storage   local-path-provisioner-684f458cdd-szrxl      2m           7Mi 
```


It is recommended to install k9s in addition to using this stack: https://www.google.com/search?client=firefox-b-1-d&q=k9s