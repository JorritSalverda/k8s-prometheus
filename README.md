To make sure you can execute the RBAC roles and bindings make sure your own account is admin in the Kubernetes cluster.

```
ACCOUNT=$(gcloud info --format='value(config.account)')
kubectl create clusterrolebinding owner-cluster-admin-binding \
    --clusterrole cluster-admin \
    --user $ACCOUNT
```

To run Prometheus at it's cheapest - yes with possible downtime - execute the following command:

```
cat kubernetes.yaml | HOSTNAME=prometheus.myserver.com GCS_BUCKET_NAME=mythanosbucket GCS_CREDENTIALS=*** envsubst \$HOSTNAME,\$GCS_BUCKET_NAME,\$GCS_CREDENTIALS | kubectl apply -f -
```

Check if everything's up and running with:

```
watch -n 2 kubectl get svc,ing,deploy,po,cm,secret,pdb,hpa,ep,pvc -l app=prometheus -n prometheus
```

Access the service via port-forwarding:

```
kubectl port-forward svc/prometheus -n prometheus 9090:80
```

And open `http://127.0.0.1:9090/` to see your prometheus instance.