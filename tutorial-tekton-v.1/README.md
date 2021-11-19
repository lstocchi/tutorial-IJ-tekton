This folder contains all resources needed to follow what is presented in this video <url>

The source code of our app can be found at https://github.com/lstocchi/react-web-app

# Set up

To follow this tutorial, make sure you have access to a Kubernetes/OpenShift cluster with Tekton installed (version greater than 0.26+).
Also, make sure to enable the tekton alpha mode to play with the debug feature.

```
kubectl patch configmap/feature-flags \
  -n <namespace> \
  --type merge \
  -p '{"data":{"enable-api-fields":"alpha"}}'
```

# Authorization

To create all things needed to authorize your pipeline, edit the `<username>` and `<password>` field inside the `authorization.yaml` and deploy it

`kubectl create -f authorization.yaml`

It will create the service account which will be used by the pipeline. 
Once it is created you can create the role 

```
kubectl create role hub-pipeline --resource=deployment,services,pvc,job,namespaces,ConfigMap,routes --verb=create,get,list,delete,patch

kubectl create rolebinding hub-pipeline --serviceaccount=<namespace>:<serviceAccountName> --role=hub-pipeline

oc adm policy add-scc-to-user privileged system:serviceaccount:<namespace>:<serviceAccountName>
```

where `<serviceAccountName>` is `quay-login` if you don't change the name of the service account.

# Other resources

All other steps can be followed by deploying the other resources in this repo.