# Multi Cloud Manager (MCM) Edge cluster deployments

Deploying a Guestbook application using MCM manager hub cluster and multiply Edge clusters

## Pre

1. MCM Hub cluster with 2 or more Worker nodes, Management node and Master/Proxy nodes.
2. Multi Edge Clusters with one Master/Proxy node and on Worker node.
The Edge cluster could be on the cloud providers or on standalone vmware environments.
3. Network conactivity between the Hub cluster and the Edge Clusters, with the appropate firewalls ports open.
4. Guestbook Application
 


## Usage
0. Clone the repo ```git clone ....``` 
1. Package Guestbook 'app' into a charts with ```helm package gbapp```
2. log in MCM Hub Cluster ```cloudctl login -a <https:x.x.x.x:8443>```
1. Load application charts into ICP with ```cloudctl catalog load-chart --archive gbapp-0.1.2.tgz```
2. Install application chart with GUI ```Catalog (top right), search for gbapp   ```
3. or CLI ```helm install gbapp -n <release-name> --tls ```
3. Update placement related values to redeploy application
4. Delete helm release to deregister application ```helm delete <release-name> --purge --tls```


![View of the Guestbook Application](https://github.com/antonyp63/Guestbook/blob/master/Guestbook_app.jpg)



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


