# Multi Cloud Manager (MCM) Edge cluster deployments

Deploying a Guestbook application using MCM manager hub cluster and multiply Edge clusters

### Pre

MCM Hub cluster with 2 or more Worker nodes, Management node and Master/Proxy nodes

Multi Edge Clusters with one Master/Proxy node and on Worker node.

The Edge cluster could be on the cloud providers or on standalone vmware environments

Network conactivity between the Hub cluster and the Edge Clusters, with the appropate firewalls ports open.

Guestbook Application
 


```
git clone 

helm package gbapp

cloudctl login -a <https:x.x.x.x:8443>

cloudctl catalog load-chart -a gbapp-0.1.2.tgz -r local-charts
```
Application directory and files

```

gbapp/Chart.yaml

gbapp/values.yaml

gbapp/template

NOTES.txt
_helpers.tpl
appfrontrel.yaml
application.yaml
appmasterrel.yaml
frontenddeployable.yaml
masterdeployable.yaml
placement.yaml
placementbinding.yaml
slavedeployable.yaml
slaverelationship.yaml


```
example of slacedeployable.yaml

```

apiVersion: mcm.ibm.com/v1alpha1
kind: Deployable
metadata:
  name: {{ template "guestbookapplication.fullname" . }}-redisslave
  labels:
    app: {{ template "guestbookapplication.name" . }}
    chart: {{ .Chart.Name }}-{{ .Chart.Version | replace "+" "_" }}
    release: {{ .Release.Name }}
    heritage: {{ .Release.Service }}
    name: {{ template "guestbookapplication.fullname" . }}-redisslave
    servicekind: CacheService
spec:
  deployer:
    kind: helm
    helm:
      chartURL: https://raw.githubusercontent.com/antonyp63/Guestbook/master/gbrs-0.1.1.tgz
      namespace: {{ .Values.appInClusterNamespace }}
      
      
```


