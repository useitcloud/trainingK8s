
$ helm create mailhog
$ tree mailhog/
mailhog/
├── Chart.yaml
├── charts
├── templates
│   ├── NOTES.txt
│   ├── _helpers.tpl
│   ├── deployment.yaml
│   ├── hpa.yaml
│   ├── ingress.yaml
│   ├── service.yaml
│   ├── serviceaccount.yaml
│   └── tests
│       └── test-connection.yaml
└── values.yaml

$ helm lint mailhog

$ helm install --dry-run mailhog mailhog/

$ helm install mailhog mailhog/

$ kubectl get all -l "app.kubernetes.io/name=mailhog"

$ kubectl port-forward --address 0.0.0.0 services/mailhog 8025

$ helm uninstall mailhog
