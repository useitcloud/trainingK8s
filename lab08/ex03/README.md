# Installation avec git
$ git clone https://github.com/prometheus-operator/kube-prometheus
$ cd kube-prometheus

$ kubectl apply --server-side -f manifests/setup
$ kubectl wait \
--for condition=Established \
--all CustomResourceDefinition \
--namespace=monitoring
$ kubectl apply -f manifests/

$ kubectl create -f manifests/setup
$ kubectl create -f manifests/
$ cd ..

$ kubectl --namespace monitoring get all

$ kubectl --namespace monitoring get svc
$ kubectl --namespace monitoring port-forward --address 0.0.0.0 services/prometheus-k8s 9090 &
$ kubectl --namespace monitoring port-forward --address 0.0.0.0  svc/alertmanager-main 9093 &
$ kubectl --namespace monitoring port-forward --address 0.0.0.0 svc/grafana 3000 &

$ kubectl apply -f pod.yaml
$ kubectl apply -f pod-monitor.yaml

$ kubectl delete -f pod-monitor.yaml
$ kubectl delete -f pod.yaml
$ cd kube-prometheus
$ kubectl delete --ignore-not-found=true -f manifests/ -f manifests/setup


# Installation avec helm
$ helm repo add prometheus-community \
https://prometheus-community.github.io/helm-charts

$ helm upgrade --install prometheus \
prometheus-community/kube-prometheus-stack \
--namespace monitoring \
--create-namespace

$ kubectl --namespace monitoring get all -l "release=prometheus"

$ kubectl --namespace monitoring get svc -l "release=prometheus"

$ kubectl -n monitoring get servicemonitor
$ kubectl -n monitoring get servicemonitor prometheus-kube-prometheus-operator -o yaml

$ kubectl --namespace monitoring port-forward --address 0.0.0.0 svc/prometheus-kube-prometheus-prometheus 9090 &
$ kubectl --namespace monitoring port-forward --address 0.0.0.0 svc/prometheus-kube-prometheus-alertmanager 9093 &

$ kubectl -n monitoring get prometheusrule
$ kubectl -n monitoring get PrometheusRule prometheus-kube-prometheus-prometheus  -o yaml


$ kubectl -n monitoring get configmap -l grafana_datasource=1
$ kubectl -n monitoring get configmap -l grafana_dashboard=1
$ kubectl --namespace monitoring port-forward --address 0.0.0.0 svc/prometheus-grafana 8081:80 &

$ kubectl get secret -n monitoring prometheus-grafana -o yaml
$ echo YOUR_USERNAME | base64 --decode
$ echo YOUR_PASSWORD | base64 --decode

$ helm delete prometheus --namespace monitoring
$ kubectl delete crd podmonitors.monitoring.coreos.com \
probes.monitoring.coreos.com \
prometheuses.monitoring.coreos.com \
prometheusrules.monitoring.coreos.com \
alertmanagerconfigs.monitoring.coreos.com \
alertmanagers.monitoring.coreos.com \
servicemonitors.monitoring.coreos.com \
thanosrulers.monitoring.coreos.com