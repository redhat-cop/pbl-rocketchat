# RocketChat on OpenShift

Thie repo contains templates and tooling for taking Rocket Chat to Production on OpenShift Container Platform.

## Dev Deployment

```
oc new-project dev
oc process -f mongodb-statefulset-replication.yaml | oc apply -f-
oc process -f rocketchat.yaml -p HOSTNAME_HTTP=chat-dev.apps.example.com -p ACCOUNT_DNS_DOMAIN_CHECK=false | oc apply -f-
```
