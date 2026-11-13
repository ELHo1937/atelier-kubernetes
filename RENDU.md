# Rendu — Atelier Kubernetes

> Complétez ce fichier au fil de l'atelier, puis faites un commit et un push (voir la section « Rendu » du README).

**Nom / Prénom :** Rahim Hocine
**Groupe :**

---

Exercice 1 — Cluster

1. Combien de nœuds affiche `kubectl get nodes` ? Quel est le rôle de chacun ?
   > 3 nœuds au total. k3d-lab-server-0 : rôle control-plane (master). k3d-lab-agent-0 et k3d-lab-agent-1 : rôle <none> (workers).
2. Que montre `docker ps` ? Qu'est-ce qu'un « nœud » dans k3d, concrètement ?
   > La commande affiche 4 conteneurs Docker en cours d'exécution. Concrètement, avec k3d, un "nœud" Kubernetes est en réalité un conteneur Docker.

## Exercice 2 — Premier déploiement

3. Quels objets Kubernetes ont été créés par `kubectl create deployment` ? (indice : `kubectl get all`)
   > Un Deployment, un ReplicaSet et un Pod.
4. Vous avez supprimé le pod `web-...` : que s'est-il passé, et pourquoi ?
   > Un nouveau pod a été recréé immédiatement. Le Deployment (via son ReplicaSet) maintient le nombre de pods désiré actif en permanence.

## Exercice 3 — Modifier un conteneur en direct

5. Après suppression du pod, votre message « Bienvenue... » a-t-il survécu ? Pourquoi ?
   > Non, le message a disparu car les conteneurs sont éphémères. Le nouveau pod est recréé à partir de l'image Nginx d'origine.
6. Pourquoi le `port-forward` a-t-il cessé de fonctionner après la suppression du pod ?
   > Le tunnel était attaché spécifiquement à l'ancien pod. Quand il a été supprimé, la connexion a été rompue.

## Exercice 4 — Déclaratif

7. Quelle différence entre `kubectl create` et `kubectl apply` ?
   > `create` est une commande impérative (exécution manuelle), tandis que `apply` est déclaratif (on fournit un fichier décrivant l'état attendu, et K8s se débrouille).
8. À quoi sert le `selector` d'un Service ? Que se passe-t-il s'il ne correspond à aucun pod ?
   > Il sert à cibler les bons pods grâce à leurs labels (ex: app=site). S'il ne correspond à aucun pod, le trafic n'est envoyé nulle part.

## Exercice 5 — Scaling & mises à jour

9. Sur quels nœuds se trouvent vos 3 réplicas ? Collez la sortie de `kubectl get pods -l app=site -o wide`.
   ```text
   NAME                    READY   STATUS    RESTARTS   AGE    IP           NODE              
   site-666897848d-8f2zj   1/1     Running   0          80s    172.18.0.3   k3d-lab-agent-0
   site-666897848d-bqn5c   1/1     Running   0          103s   172.18.0.4   k3d-lab-agent-1
   site-666897848d-tzwvl   1/1     Running   0          87s    172.18.0.3   k3d-lab-agent-0