# istio_working_GCP



jaeger dashboard
================
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=jaeger -o jsonpath='{.items[0].metadata.name}') 15032:16686

http://localhost:15032


grafana dashboard
================
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000 &

http://localhost:3000/dashboard/db/istio-mesh-dashboard


kiali dashboard
================
$ kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=kiali -o jsonpath='{.items[0].metadata.name}') 20001:20001

https://localhost:20001/kiali/console 





Create a new cluster.
====================

Retrieve your credentials for kubectl.
=====================================

gcloud container clusters get-credentials vnv	 \
    --zone us-central1-a \
    --project refined-magpie-232210


Grant cluster administrator (admin) permissions to the current user. To create the necessary RBAC rules for Istio, the current user requires admin permissions.
======

kubectl create clusterrolebinding cluster-admin-binding \
    --clusterrole=cluster-admin \
    --user=$(gcloud config get-value core/account)

Install Istio

for i in install/kubernetes/helm/istio-init/files/crd*yaml; do kubectl apply -f $i; done

kubectl apply -f install/kubernetes/istio-demo.yaml


enable istion side car 
=====================
kubectl label namespace default istio-injection=enabled

gcloud container clusters resize istio-cluster --size=0 --zone=asia-south1-a
