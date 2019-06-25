# Multi Cloud Manager (MCM) Edge cluster deployments

Deploying a Guestbook application using MCM manager hub cluster and multiply Edge clusters

## Pre-rec

1. MCM Hub cluster with 2 or more Worker nodes, Management node and Master/Proxy nodes.
2. Multi Edge Clusters with one Master/Proxy node and on Worker node.
The Edge cluster could be on the cloud providers or on standalone vmware environments.
3. Network conactivity between the Hub cluster and the Edge Clusters, with the appropate firewalls ports open.
4. Guestbook Application
 


## Usage
0. Clone the repo ```git clone ....``` or ```kubectl apply -f https://raw.githubusercontent.com/google/metallb/v0.7.3/manifests/metallb.yaml```
1. Package Guestbook 'app' into a charts with ```helm package gbapp```
2. log in MCM Hub Cluster ```cloudctl login -a <https:x.x.x.x:8443>```
1. Load application charts into ICP with ```cloudctl catalog load-chart --archive gbapp-0.1.x.tgz```

At this point you have upload the Guestbook Application helm to the Hub Cluster.

You will need to repair you Hub cluster to ensure you have a successful deployment.


2. Install application chart with GUI ```Catalog (top right), search for gbapp   ```
3. or CLI ```helm install gbapp -n <release-name> --tls ```
3. Update placement related values to redeploy application
4. Delete helm release to deregister application ```helm delete <release-name> --purge --tls```
5. Example of the Guestbook Application:


![View of the Guestbook Application](https://raw.githubusercontent.com/antonyp63/Guestbook/master/Guestbook_app.jpg)



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
## Applications fail to install during Helm deployment

```
kubectl get clusterimagepolicy

kubectl edit clusterimagepolicy ibmcloud-default-cluster-image-policy

- name: gcr.io/google-samples/*
- name: gcr.io/kubernetes-e2e-test-images/*
```

