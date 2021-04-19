## Kubectl cheatsheet

### List Services
kubectl -n magma get svc

### Get Logs
kubectl logs orc8r-controller-7b6bcb454-6hlrd -n magma
kubectl -n magma get pods

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


