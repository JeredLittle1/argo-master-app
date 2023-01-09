## Description
Creates the `ingress-nginx` controller.


## Local Setup

### Rancher Desktop & K3S


If running locally using Rancher, set `nginx.kindCluster=false`, edit the `/etc/hosts` file and add the following lines:

```
 EXTERNAL_IP argocd.test.org
 EXTERNAL_IP argo-workflows.test.org
 EXTERNAL_IP airflow.test.org
 EXTERNAL_IP jupyterhub.test.org
```

***WARNING*** Disable `Traefik` via `Preferences`. Otherwise, the `nginx-controller` will never obtain an `EXTERNAL-IP`.

*Note*: The domain `test.org` may change based on the `values.yaml` config value `ingress.domainName`. In this case, `ingress.domainName=test.org`

The `EXTERNAL_IP` field can be found by running:

```
kubectl describe service/nginx-ingress-nginx-controller -n ingress-nginx
```

Which outputs something like:

```
Name:                     nginx-ingress-nginx-controller
Namespace:                ingress-nginx
Labels:                   app.kubernetes.io/component=controller
                          app.kubernetes.io/instance=nginx
                          app.kubernetes.io/managed-by=Helm
                          app.kubernetes.io/name=ingress-nginx
                          app.kubernetes.io/part-of=ingress-nginx
                          app.kubernetes.io/version=1.5.1
                          argocd.argoproj.io/instance=nginx
                          helm.sh/chart=ingress-nginx-4.4.2
Annotations:              <none>
Selector:                 app.kubernetes.io/component=controller,app.kubernetes.io/instance=nginx,app.kubernetes.io/name=ingress-nginx
Type:                     LoadBalancer
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.43.77.104
IPs:                      10.43.77.104
LoadBalancer Ingress:     172.21.134.137
Port:                     http  80/TCP
TargetPort:               http/TCP
NodePort:                 http  31750/TCP
Endpoints:                10.42.0.116:80
Port:                     https  443/TCP
TargetPort:               https/TCP
NodePort:                 https  30368/TCP
Endpoints:                10.42.0.116:443
Session Affinity:         None
External Traffic Policy:  Cluster
Events:
  Type    Reason                Age    From                Message
  ----    ------                ----   ----                -------
  Normal  EnsuringLoadBalancer  6m46s  service-controller  Ensuring load balancer
  Normal  AppliedDaemonSet      6m46s  service-controller  Applied LoadBalancer DaemonSet kube-system/svclb-nginx-ingress-nginx-controller-6f4ea5ad
  Normal  UpdatedLoadBalancer   6m44s  service-controller  Updated LoadBalancer with new IPs: [] -> [172.21.134.137]
```

The `LoadBalancer Ingress` IP address is the one to add to `/etc/hosts` above.

### Kind
If using `kind` to create your cluster, set `nginx.kindCluster=true`. Then, modify your `/etc/hosts` file & add:

```
 127.0.0.1 argocd.test.org
 127.0.0.1 argo-workflows.test.org
 127.0.0.1 airflow.test.org
 127.0.0.1 jupyterhub.test.org
 127.0.0.1 prometheus.test.org
 127.0.0.1 grafana.test.org
```

*Note*: The domain `test.org` may change based on the `values.yaml` config value `ingress.domainName`. In this case, `ingress.domainName=test.org`