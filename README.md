# Guestbook
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