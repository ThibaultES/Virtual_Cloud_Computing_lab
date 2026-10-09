# Étape 1

1. Expliquez, avec vos propres mots, ce qu’est une machine virtuelle.

Une machine virtuelle est une instance d'une machine physique existante sur une machine hôte.

2. Citez deux avantages de la virtualisation dans un environnement professionnel.

- Permet d'utiliser des machines différentes sur une même machine physique donc de mutualiser les ressources et de les exploiter au maximum.
- Permet d'avoir un environnement isolé pour le développement ou les tests par exemple.

3. Quelle différence existe-t-il entre travailler directement sur votre ordinateur et travailler dans une machine virtuelle ?

Une machine virtuelle sera plus lente car le contrôle passe par un hyperviseur géré par l'OS hôte à la différence de notre PC où le contrôle passe directement à l'OS sans la couche hyperviseur.

# Étape 2

1. Expliquez, avec vos propres mots, ce qu’est un conteneur Docker.

Un conteneur Docker est un environnement virtuel isolé qui s'execute sur une machine hôte et qui possède ses propres librairies et dépendances et auquel on alloue des ressources.

2. Quelle différence fondamentale existe entre une machine virtuelle et un conteneur ?

Une machine virtuelle possède son propre OS tandis qu'un conteneur utilise l'OS d'une machine (virtuelle ou physique).

3. Pourquoi les conteneurs sont-ils particulièrement adaptés au déploiement d’applications dans
le Cloud ?

Ils sont reproductibles, plus légers que les VM, isolés, et permettent d'avoir les ressources (librairies, dépendances, codes) nécessaires au déploiement d'un service. Plusieurs services dans plusieurs conteneurs permettent de déployer facilement une application dans le Cloud et de permettre l'utilisation de chacun de ces services dans d'autres applications.

# Étape 3

1. Pourquoi un Dockerfile est-il préférable à la configuration manuelle d’un conteneur ?

Un Dockerfile sert à centraliser toutes les informations de configuration du conteneur et à lancer en peu d'étapes sa création.

2. Quelle différence existe entre une image Docker et un conteneur Docker ?

L'image Docker contient les informations nécessaires à la création d'un conteneur Docker.

# Étape 4

1. Pourquoi Docker Compose est-il préférable au lancement manuel de plusieurs conteneurs ?

Docker Compose permet, en un unique fichier, de décrire la configuration de plusieurs conteneurs à la fois.

2. Quel est le rôle du fichier "docker-compose.yml" ?

Ce fichier permet d'indiquer l'architecture de l'application avec les différents services qu'elle utilise (ports des conteneurs), ainsi que le port de la VM permettant d'appeller l'application (redirigeant vers le port du conteneur principal de l'application).

3. Dans quels cas Docker Compose pourrait-il trouver ses limites ?

Docker Compose est sensible à l'arrêt d'un conteneur; si un conteneur s'arrête, toute l'application s'arrête. Aussi, il ne peut pas déployer et gérer des conteneurs en dehors de la machine où il s'execute.