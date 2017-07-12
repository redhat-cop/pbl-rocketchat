# RocketChat on OpenShift

This repository contains templates and tooling for taking Rocket Chat to Production on OpenShift Container Platform.

## Prerequisites

Mongo is deployed as a StatefulSet and requires access to at least 3 PersistentVolumes for its backing storage.

##  Deployment

Create a new project

```
oc new-project rocketchat
```

Deploy the MongoDB StatefulSet using the included template

```
oc process -f mongodb-statefulset-replication.yaml | oc apply -f-
```

The RocketChat image is stored in the [Red Hat Container Catalog](https://registry.access.redhat.com) (RHCC). A valid Red Hat subscription is required in order to retrieve the image.

Create a new secret called _rhcc_ containing your credentials to the Red Hat Customer Portal

```
oc secrets new-dockercfg external-registry \
    --docker-username=<username> \
    --docker-password=<password> \
    --docker-email=<email> \
    --docker-server=registry.connect.redhat.com
```

Add the secret to the default service account

```
oc secrets add serviceaccount/default secrets/rhcc --for=pull
```

Deploy the RocketChat template. Be sure to include the hostname of the application as a template parameter. 

```
oc process -f rocketchat.yaml -p HOSTNAME_HTTP=chat-dev.apps.example.com -p ACCOUNT_DNS_DOMAIN_CHECK=false | oc apply -f-
```

Once deployed, the application will be available at the provided hostname.

