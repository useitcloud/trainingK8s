# Description
Dans cette partie on va déployer une base de donnée "mariaDB" avec le manifeste "mariadb-deployment.yaml"
Observez et decrivez les objets à déployer
## Déploiement
Lancez le déployement
$ kubectl apply -f mariadb-deployment.yaml

Affichez les volume et pods
$ kubectl get pvc, pods

Consultez l’état du pod de MariaDB à l’aide de la commande suivante
$ kubectl get pods -l app=mariadb

Connexion au container
Afin de tester le fonctionnement de la base de données, vous allez devoir lancer un shell dans le container de base de données :
$ kubectl exec -it deployment/mariadb -- bash

Afin de vous assurer que tout fonctionne, lancez la commande mysqladmin suivie des options suivantes :
- Le mot-clé status.
- L’option -p suivie de la variable d’environnement contenant le mot de passe (-p$MYSQL_ROOT_PASSWORD).
Ci-dessous la commande correspondante :
$ mysqladmin status -p$MYSQL_ROOT_PASSWORD

Sortez de pods

## Changement de stratégie de déploiment
Par défaut, si rien n’est spécifié, Kubernetes utilise un déploiement par roulement. Le principe de ce mécanisme est le suivant :
Un nouveau pod est démarré en parallèle de l’ancien.
Lorsque le nouveau pod est prêt, l’ancien pod est supprimé.
Dans le cas où plusieurs pods seraient démarrés, Kubernetes réalisera l’opération pod par pod jusqu’à remplacer complètement les anciens pods.
La plupart du temps, ce type de comportement est le bon puisque les temps d’indisponibilité sont ainsi réduits. Dans le cas présent, cette stratégie constitue un problème.
Le problème vient du fait qu’une base de données ne peut pas partager ses fichiers.
Le changement de la cinématique de déploiement se fait en modifiant le champ deployment --> spec --> strategy --> type.
Ci-dessous les valeurs possibles :
- RollingUpdate : remplacement par vague des pods existants (mécanisme par défaut).
- Recreate : suppression des anciens pods avant recréation.

faite un path sur le deploiment pour changer cette stratégie vers "recreate"

$ kubectl patch deployment mariadb --type strategic --patch-file patch-file-strategy.yaml
ref: https://kubernetes.io/docs/tasks/manage-kubernetes-objects/update-api-object-kubectl-patch/

vérifiez que la stratégie est prise en compte
$ kubectl get deployment mariadb --output yaml

Consultez l’état du déploiement dans autre terminal avec la commande suivante :
$ kubectl get pods -l app=mariadb --watch

Ensuite simuler un crash avec la commande
$ kubectl delete pods -l app=mariadb

Scalez le deploiement avec 
$ kubectl scale deployment mariadb --replicas=2

Ce mécanisme de déploiement avec replicaset n’est pas adapté pour gérer un ensemble de pods de base de données. Pour cela, Kubernetes offre un autre mécanisme : les objets de type StatefulSet.
Un objet StatefulSet (ou son raccourci sts) dispose des caractéristiques suivantes :
- Prédictibilité du nom des pods.
- Assignation du stockage persistant pod par pod.
- Création des pods dans un ordre donné.
- Mise à jour des pods dans un ordre donné.

Supprimez 
$ kubectl delete -f mariadb-deployment.yaml

Appliquez
$ kubectl apply -f  mariadb-sts.yaml

Consultez l’état des pods associés au StatefulSet de MariaDB :
$ kubectl get pods -l app=mariadb

Tout comme pour un déploiement, Kubernetes peut augmenter le nombre de pods associés à un objet StatefulSet.
La commande kubectl a besoin pour cela des options suivantes :
- Le verbe scale.
- Le type d’objet (statefulset ou le raccourci sts).
- Le nom de l’objet StatefulSet (mariadb).
- Le nombre de réplicats à obtenir (avec l’option -replicas).

 Afin de démarrer deux pods dans le ReplicaSet de MariaDB, lancez la commande suivante :
$ kubectl scale sts mariadb --replicas=2

Pour consulter les pods et les PVC portant le label app=mariadb en même temps, utilisez la commande suivante :
$ kubectl get pods,pvc -l app=mariadb

La diminution du nombre de pods d’un objet StatefulSet se fait avec l’option scale de la commande kubectl. La seule chose à changer sera la valeur de l’option --replicas.
Pour passer de deux pods à un seul sur le StatefulSet de MariaDB, lancez la commande suivante :

$ kubectl scale sts mariadb --replicas=1

Lancez la commande de consultation précédente afin de connaître l’état des pods et des demandes de volumes persistants.
Le pod mariadb-1 a disparu. En revanche, la demande de volume persistant associée est toujours présente.
$ kubectl get pods,pvc -l app=mariadb

Ce comportement est intéressant puisqu’il permettra de conserver les données d’un pod faisant partie d’un ensemble StatefulSet.
Attention toutefois lors du démarrage d’un pod, il sera peut-être nécessaire de resynchroniser le contenu des bases avant de mettre à disposition le pod.

Nettoyage
