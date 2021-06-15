## Kubectl cheatsheet

### List Services
kubectl -n magma get svc

### Get Logs
kubectl logs orc8r-controller-7b6bcb454-6hlrd -n magma
kubectl -n magma get pods
kubectl logs <pod_name> --previous # previous logs 


### Set Port Forwarding
kubectl port-forward --address 0.0.0.0 svc/orc8r-prometheus 9090:9090

### Get Deployment Information
Kubectl deployment
> kubectl get deployment

### Edit Deployment
kubectl edit deploy/orc8r-controller

### Restart Pod
*All Pods*

kubectl -n {NAMESPACE} rollout restart deploy
 
*Specific Pod*
```
ksubraveti@ksubraveti-mbp helm % kubectl -n orc8r rollout restart deployment/orc8r-analytics
error: the server doesn't have a resource type "orc8r-analytics"

ksubraveti@ksubraveti-mbp helm % kubectl -n orc8r get deployment
NAME                            READY   UP-TO-DATE   AVAILABLE   AGE
fluentd                         2/2     2            2           393d
nms-magmalte                    1/1     1            1           1	2d
nms-nginx-proxy                 1/1     1            1           12d
openvpn                         1/1     1            1           294d
orc8r-accessd                   2/2     2            2           11d
orc8r-alertmanager              1/1     1            1           12d
orc8r-alertmanager-configurer   1/1     1            1           12d
orc8r-analytics                 2/2     2            2           11d
orc8r-bootstrapper              2/2     2            2           11d
orc8r-certifier                 2/2     2            2           11d
orc8r-configurator              2/2     2            2           11d
orc8r-cwf                       2/2     2            2           11d
orc8r-device                    2/2     2            2           11d
orc8r-directoryd                2/2     2            2           11d
orc8r-dispatcher                2/2     2            2           11d
orc8r-download                  2/2     2            2           11d
orc8r-fbinternal                2/2     2            2           11d
orc8r-feg                       2/2     2            2           11d
orc8r-feg-relay                 2/2     2            2           11d
orc8r-ha                        2/2     2            2           11d
orc8r-health                    2/2     2            2           11d
orc8r-lte                       2/2     2            2           11d
orc8r-metricsd                  2/2     2            2           11d
orc8r-nginx                     2/2     2            2           12d
orc8r-obsidian                  2/2     2            2           11d
orc8r-orchestrator              2/2     2            2           11d
orc8r-policydb                  2/2     2            2           11d
orc8r-prometheus                1/1     1            1           11d
orc8r-prometheus-cache          1/1     1            1           12d
orc8r-prometheus-configurer     1/1     1            1           12d
orc8r-service-registry          2/2     2            2           11d
orc8r-smsd                      2/2     2            2           11d
orc8r-state                     2/2     2            2           11d
orc8r-streamer                  2/2     2            2           11d
orc8r-subscriberdb              2/2     2            2           11d
orc8r-tenants                   2/2     2            2           11d
orc8r-testcontroller            2/2     2            2           11d
orc8r-thanos-compact            1/1     1            1           11d
orc8r-thanos-query              1/1     1            1           11d
orc8r-thanos-store-0            1/1     1            1           11d
orc8r-user-grafana              1/1     1            1           12d
orc8r-vpn                       2/2     2            2           11d
orc8r-wifi                      2/2     2            2           11d
```

ksubraveti@ksubraveti-mbp helm % kubectl -n orc8r rollout restart deployment/orc8r-analytics
deployment.apps/orc8r-analytics restarted

### Label vs Annotation
Labels can be used to select objects and to find collections of objects that satisfy certain conditions. 
In contrast, annotations are not used to identify and select objects. The metadata in an annotation can 
be small or large, structured or unstructured, and can include characters not permitted by labels

### Get Kubectl Annotation
kubectl get pods -n orc8r -o jsonpath='{.items[*].metadata.annotations}'

### Dump the entire cluster information
kubectl cluster-info dump

### Kubernetes image pull policy 
imagePullPolicy - IfNotPresent, Always etc. Modify to Always to always pull from registry

Describe and delete secret
root@39a8f782c90c:~/scripts/testlib# kubectl -n orc8r delete secret sh.helm.release.v1.orc8r.v1
secret "sh.helm.release.v1.orc8r.v1" deleted
root@39a8f782c90c:~/scripts/testlib# kubectl -n orc8r delete secret sh.helm.release.v1.lte-orc8r.v1
secret "sh.helm.release.v1.lte-orc8r.v1" deleted
root@39a8f782c90c:~/scripts/testlib# terr^C
root@39a8f782c90c:~/scripts/testlib# kubectl -n orc8r describe secret

### Dump PVC information
root@fbb460a95152:~/project# kubectl -n orc8r get pvc -o wide
NAME                 STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE   VOLUMEMODE
grafanadashboards    Bound    pvc-6e38f986-a290-4f2a-a697-ddc4b89daf7a   2Gi        RWX            efs            38h   Filesystem
grafanadata          Bound    pvc-af21df00-24e8-45cb-89ff-92fe3bedd651   2Gi        RWX            efs            38h   Filesystem
grafanadatasources   Bound    pvc-1a80a13f-2479-487e-8d81-e923b05e9aae   100M       RWX            efs            38h   Filesystem
grafanaproviders     Bound    pvc-111f39c2-eacc-4551-b88e-35fe5263a1d1   100M       RWX            efs            38h   Filesystem
openvpn              Bound    pvc-4c00c48f-99aa-4f31-a62f-f3554f69dc03   2M         RWO            efs            38h   Filesystem
promcfg              Bound    pvc-e6c32c69-795d-eklhhjudivdtevrdceijtelttllnndhf4378-8fb2-62a4bc4a4ab1   1Gi        RWX            efs            38h   Filesystem
promdata             Bound    pvc-769b7ecc-b67c-4085-b785-68349e064bd4   64Gi       RWO            efs            38h   Filesystem

### Describe disk usage
kubectl describe pv

### DELETE PERSISTENT VOLUME CLAIM
kubectl -n orc8r delete  pvc/promdata


### Adding kubernetes metrics
Deploying Kubernetes Metric Server.
https://docs.aws.amazon.com/eks/latest/userguide/metrics-server.html

1. kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
2. kubectl get deployment metrics-server -n kube-system

### Install Kibana on your kubernetes cluster
helm upgrade -n orc8r orc8r-kibana3 --version 7.7.1 elastic/kibana --set elasticsearchHosts=https://vpc-orc8r-es-uqztpgpinvr7y36d5suq2gg7i4.us-west-2.es.amazonaws.com:443 --set image=docker.elastic.co/kibana/kibana-oss

### Helm Node Selector
Use node selector to pin a pod to a specific node.
