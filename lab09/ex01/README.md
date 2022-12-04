# Instructions
Activez le plugin ingress
$ minikube -p kubelabs addons enable ingress

$ kubectl apply -f ns-lab8.yaml

$ kubectl apply -f app1.yaml -n ns-lab09
$ kubectl apply -f app2.yaml -n ns-lab09

$ kubectl apply -f app-ingress.yaml -n ns-lab09
$ kubectl describe ingress -n ns-lab09 moningressfanout


