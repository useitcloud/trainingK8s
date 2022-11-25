# Instructions
$ kubectl get nodes
$ kubectl drain kubelabs-m02 --ignore-daemonsets
$ kubectl uncordon kubelabs-m02

$ minikube node list
$ minikube node delete kubelabs-m02
$ kubectl get nodes

$ arkade get helm
$ sudo mv /home/ubuntu/.arkade/bin/helm /usr/local/bin/
$ helm version

$ helm repo add bitnami https://charts.bitnami.com/bitnami
$ helm repo update
$ helm repo list

$ helm search repo wordpress

$ helm show values bitnami/wordpress
$ helm uninstall wpblog
$ kubectl get pvc
$ kubectl delete pvc data-wpblog-mariadb-0

$ helm install wpblog bitnami/wordpress

$ helm ls

$ helm uninstall wpblog

$ helm install wpblog bitnami/wordpress \
--namespace intranet \
--create-namespace
--set mariadb.primary.persistence.size=4Gi,persistence.size=6Gi

$ kubens intranet

$ kubectl get all -l "app.kubernetes.io/instance=wpblog"

$ helm upgrade wpblog bitnami/wordpress --set service.type=NodePort

$ helm get values wpblog

$ kubectl get services

$ kubectl port-forward  --address 0.0.0.0  services/wpblog-wordpress 8081:80

$ helm uninstall wpblog
$ kubectl get pvc
$ kubectl delete pvc data-wpblog-mariadb-0

$ helm install wpblog bitnami/wordpress --values=values.yaml

$ helm template wpblog bitnami/wordpress --values=values.yaml >> wpblog-fullresources.yaml