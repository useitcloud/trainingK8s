$ minikube addons enable dashboard
$ minikube dashboard --url &
$ kubectl proxy --address='0.0.0.0' --disable-filter=true &
$ minikube addons enable metrics-server

$ arkade get k9s
$ sudo mv /home/ubuntu/.arkade/bin/k9s /usr/local/bin/
$ k9s

$ minikube addons disable dashboard