# État d’un container
$ kubectl apply -f kuard-pod.yaml

Aprés avoir induit le pod en echec au vérifucation d'etat "livenessProbe" Kubernetes redémarrera le pod. À ce stade, l'interface se réinitialise. Les détails du redémarrage peuvent être trouvés en exécutant la commande
$ kubectl describe pod/kuard


# Observabilité
Activez la récolte des « métriques » de surveillance avec la commande suivante :
$ minikube addons enable metrics-server
$ kubectl top nodes
$ kubectl top pod kuard
$ kubectl top pods -A
$ kubectl top pods -A --sort-by=memory | head -5
$ kubectl top pods -A --sort-by=cpu | head -5


$ kubectl delete -f kuard-pod.yaml