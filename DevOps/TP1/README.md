# Étape 1

1. Quel est le rôle de VM-K8s dans l’architecture mise en place ? Quelle est la différence entre cette machine et les nœuds workers ?

VM-K8s joue le rôle de Master (Control Plane). Elle gère le cluster Kubernetes : elle orchestre les applications, planifie les Pods sur les workers et surveille l'état du cluster. Les workers, quant à eux, exécutent concrètement les applications conteneurisées et les Pods qui leur sont attribués.

2. Pourquoi le Docker Registry et le master Kubernetes sont-ils installés sur deux VMs différentes ?

Ils sont séparés pour isoler leurs rôles et leurs ressources. Le Registry stocke et distribue les images Docker, tandis que le Master gère le cluster Kubernetes. Cette séparation évite qu'une charge importante sur le Registry perturbe le fonctionnement du Master et facilite la maintenance.

3. Le script d’installation vous a permis d’installer Kubernetes sans exécuter manuellement toutes les commandes nécessaires. Citez les opérations importantes réalisées par le script master et expliquez très brièvement en quoi elles sont nécessaires au fonctionnement du cluster.

Le script install_k8s_master.sh réalise 7 étapes :

- Désactiver le swap : Kubernetes nécessite que le swap soit désactivé pour le bon fonctionnement de kubelet. La modification de /etc/fstab permet de conserver cette désactivation après un redémarrage.

- Configuration des modules réseau : charge les modules réseau nécessaires et active le routage IP ainsi que le filtrage du trafic réseau.

- Installation de containerd : installe le moteur d'exécution qui permet de lancer les conteneurs.

- Installation de Kubernetes :
    - kubelet : gère les Pods sur la machine.
    - kubeadm : permet d'initialiser le cluster.
    - kubectl : permet d'administrer le cluster.

- Initialisation du cluster : initialise le Master, démarre les composants du Control Plane et configure le réseau prévu pour les Pods.

- Configuration de kubectl : permet à l'utilisateur d'administrer le cluster sans utiliser sudo à chaque commande.

- Génération de la commande join : génère la commande permettant aux machines workers de rejoindre le cluster.

Le réseau entre les Pods est ensuite activé séparément grâce à l'installation de Calico.

# Étape 2

1. Quel est le rôle d’un Deployment dans Kubernetes ? Pourquoi ne déploie-t-on pas directement un Pod ?

Un Deployment permet de gérer le déploiement et le cycle de vie des Pods dans Kubernetes. Il définit notamment le nombre de réplicas souhaités, l'image à utiliser et la configuration des conteneurs. On ne déploie pas directement un Pod car celui-ci n'est pas automatiquement recréé s'il est supprimé ou s'il tombe en panne. Le Deployment permet de maintenir en permanence le nombre de Pods souhaité grâce à un mécanisme de surveillance et de réconciliation.

2. Quelle est la différence entre une image Docker, un Pod et un Deployment ? Comment les 3 sont-ils liés ?

- Une image Docker contient l'application ainsi que toutes les dépendances nécessaires à son exécution.

- Un Pod est la plus petite unité déployable dans Kubernetes. Il héberge un ou plusieurs conteneurs qui utilisent des images Docker.

- Un Deployment est une ressource Kubernetes qui permet de gérer les Pods, notamment leur nombre et leur disponibilité.

3. Expliquez le principe de réconciliation. Quelle composante Kubernetes est responsable de ce comportement ?

Le principe de réconciliation consiste à comparer en permanence l'état réel du cluster avec l'état souhaité défini dans les ressources Kubernetes. Lorsqu'une différence est détectée, Kubernetes effectue automatiquement les actions nécessaires pour retrouver l'état souhaité. Par exemple, si un Pod est supprimé alors que le Deployment demande toujours un réplica, un nouveau Pod est automatiquement créé. Ce comportement est assuré par les contrôleurs Kubernetes, notamment le Deployment Controller, qui fait partie du kube-controller-manager.

# Étape 3

1. Pourquoi ne peut-on pas simplement utiliser l’adresse IP du Pod calculator-calc pour  permettre à un utilisateur d’accéder à l’application ?

Un Pod possède une adresse IP qui peut changer lorsqu’il est supprimé et recréé : il serait donc problématique pour un utilisateur de communiquer directement avec l’adresse IP du Pod. Le Service Kubernetes fournit un point d’accès stable vers un ou plusieurs Pods.

2. Comment Kubernetes sait-il que le Service calculator-calc doit transmettre les requêtes au Pod de calculator-calc ?

Kubernetes utilise un système de labels (dans le fichier Deployment) et de selectors (dans le fichier de configuration du service) pour associer un Service aux Pods correspondants. Le Pod possède le label "calculator-calc", tandis que le Service utilise le même nom dans son selector. Kubernetes identifie ainsi automatiquement les Pods correspondants et leur transmet les requêtes reçues par le Service.

3. Quelle est la différence entre le port exposé, le port cible et le NodePort utilisés dans votre architecture ?

- Le port exposé (port 80) est le port sur lequel le Service reçoit les requêtes à l'intérieur du cluster.
- Le port cible (targetPort 80) correspond au port sur lequel le conteneur du Pod écoute et reçoit les requêtes.
- Le NodePort (dans notre cas: 31328) est un port qui permet d'accéder au Service depuis l'extérieur du cluster.

# Étape 4

1. Pourquoi les quatre microservices utilisent-ils un port de type ClusterIP, tandis que calc-service utilise un port de type NodePort ?

Les quatre microservices utilisent le type ClusterIP car ils doivent uniquement communiquer entre eux à l’intérieur du cluster Kubernetes. Ce type de Service permet un accès stable depuis les autres Pods sans exposer les services à l’extérieur.

En revanche, calc-service utilise le type NodePort car il représente le frontend de l’application et doit être accessible depuis l’extérieur du cluster. Le NodePort permet aux utilisateurs d'accéder à l’application via l’adresse IP d’un noeud Kubernetes et un port spécifique.

2. Comment Kubernetes permet-il au frontend de contacter le backend sans connaître l’adresse IP du Pod ?

Kubernetes utilise le système de DNS interne pour permettre la communication entre les microservices. Chaque Service possède un nom DNS accessible depuis les autres Pods du cluster. Le frontend peut donc contacter le backend en utilisant directement le nom des Services et le port associé. Kubernetes se charge ensuite de rediriger automatiquement les requêtes vers les Pods correspondants.

3. On propose de configurer le frontend avec l’adresse IP actuelle des Pods backend (sum, sub...), par exemple "http ://10.x.x.x :<PORT>". Pourquoi cette solution n’est-elle pas adaptée à Kubernetes ?

Cette solution n’est pas adaptée car les adresses IP des Pods sont dynamiques. Lorsqu’un Pod est supprimé et recréé, il peut obtenir une nouvelle adresse IP. Le frontend risque alors de continuer à utiliser une ancienne adresse IP et de ne plus pouvoir communiquer avec le backend. L’utilisation des Services Kubernetes permet de résoudre ce problème en fournissant une adresse stable (nom DNS et port), indépendamment des changements d’adresse IP ou du remplacement des Pods.
