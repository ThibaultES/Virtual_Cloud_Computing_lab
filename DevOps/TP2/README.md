# Étape 1

1. Pourquoi est-il nécessaire que Jenkins puisse communiquer avec VM-K8s et VM-Registry ?

Jenkins doit communiquer avec VM-Registry pour pouvoir publier les images Docker construites automatiquement et les rendre accessibles au cluster Kubernetes.

Il doit également communiquer avec VM-K8s pour accéder à l'API Kubernetes et pouvoir déployer ou mettre à jour l'application directement sur le cluster.

2. Quel est le rôle de Jenkins dans la chaîne CI/CD ?

Jenkins est un outil d'automatisation qui permet de mettre en place une chaîne CI/CD. Il récupère le code source depuis Git, automatise la construction et les tests de l'application, construit les images Docker, les publie sur le Registry et automatise le déploiement sur Kubernetes. Il permet ainsi de réduire les interventions manuelles et de rendre les déploiements plus rapides et fiables.

(La suite a été faite en autonomie)

# Étape 2

1. Quelles opérations réalisées manuellement dans le TP1 ont été automatisées par Jenkins ?

Jenkins automatise plusieurs opérations réalisées manuellement dans le TP1 :
- La récupération du code source depuis le dépôt Git.
- La construction des cinq images Docker correspondant aux microservices.
- La préparation des images pour leur utilisation dans l'infrastructure.

Avec l'ajout des étapes de publication et de déploiement, Jenkins peut également automatiser l'envoi des images vers le Registry et le déploiement sur Kubernetes.

2. Votre pipeline comporte plusieurs étapes. Que se passe-t-il si la construction de l’image calculator-sub échoue ?

Si la construction de l'image calculator-sub échoue, le pipeline Jenkins s'arrête et passe à l'état « FAILURE ». Les étapes suivantes ne sont pas exécutées. Cela permet d'éviter de déployer une application dont la construction est incomplète ou incorrecte.

3. Le Webhook GitHub est la méthode la plus utilisée en entreprise dans une chaîne CI. Quelles opérations devriez-vous faire pour la mettre en place dans votre architecture ?

Pour mettre en place un Webhook GitHub, il faudrait :
- Rendre le serveur Jenkins accessible depuis Internet avec une adresse IP publique ou un nom de domaine.
- Configurer le pare-feu et les règles de sécurité pour autoriser les connexions entrantes vers Jenkins.
- Activer le déclenchement par webhook GitHub dans la configuration du job Jenkins.
- Configurer un Webhook dans les paramètres du dépôt GitHub en indiquant l'URL de Jenkins.
- Configurer Jenkins pour recevoir et traiter les notifications envoyées par GitHub.

Ainsi, à chaque nouveau commit poussé sur GitHub, celui-ci envoie une notification à Jenkins qui déclenche automatiquement le pipeline.

# Étape 3

1. Pourquoi Jenkins doit-il disposer d’un accès à l’API Kubernetes ?

Jenkins doit disposer d'un accès à l'API Kubernetes pour pouvoir communiquer avec le cluster et gérer ses ressources. Cet accès lui permet d'exécuter automatiquement les commandes kubectl apply afin de créer ou mettre à jour les Deployments et les Services. Il peut ainsi déployer la nouvelle version de l'application sans intervention manuelle.

2. Quelles étapes du job Jenkins correspondent à la CI, et lesquelles correspondent à la CD ?

La CI correspond aux étapes :
- Récupération du code source depuis Git.
- Construction des images Docker.
- Vérification et tests éventuels de l'application.

La CD correspond aux étapes :
- Publication des images Docker sur le Registry.
- Déploiement de l'application sur Kubernetes à l'aide de kubectl apply.

La CI permet de vérifier et de préparer l'application, tandis que la CD permet de la rendre disponible sur l'infrastructure.

3. Quel est le parcours d’une modification du code source depuis le "git push" jusqu’à l’application Calculator ? Pour chaque étape, indiquez l’outil ou la technologie qui intervient.

- Modification du code source : le développeur modifie le code de l'application sur son poste de travail.
- Git push : le développeur envoie ses modifications sur le dépôt GitHub.
- Détection du changement : Jenkins détecte le nouveau commit grâce au Polling SCM ou à un Webhook GitHub.
- Récupération du code : Jenkins récupère la nouvelle version du code source depuis GitHub.
- Construction des images : Jenkins utilise Docker et les Dockerfiles pour construire les images des cinq microservices.
- Publication des images : Jenkins envoie les images Docker vers le Docker Registry à l'aide de la commande docker push.
- Déploiement : Jenkins utilise kubectl apply et l'API Kubernetes pour appliquer les manifestes Kubernetes et mettre à jour les ressources du cluster.
- Exécution de l'application : Kubernetes récupère les nouvelles images depuis le Registry et met à jour les Pods des différents microservices. L'application Calculator est alors disponible avec sa nouvelle version.
