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

Un Dockerfile permet de centraliser toutes les instructions nécessaires à la configuration d'un conteneur. Il permet d'automatiser sa création et de reproduire exactement le même environnement sur différentes machines, sans avoir à refaire manuellement toute la configuration.

2. Quelle différence existe entre une image Docker et un conteneur Docker ?

L'image Docker contient les informations nécessaires à la création d'un conteneur Docker.

# Étape 4

1. Pourquoi Docker Compose est-il préférable au lancement manuel de plusieurs conteneurs ?

Docker Compose permet, en un unique fichier, de décrire la configuration de plusieurs conteneurs à la fois.

2. Quel est le rôle du fichier "docker-compose.yml" ?

Le Docker Compose permet de décrire l'architecture d'une application composée de plusieurs services. Il précise notamment les images à utiliser ou les Dockerfiles à construire, les ports à exposer, les volumes, les variables d'environnement et les réseaux nécessaires à la communication entre les conteneurs.

3. Dans quels cas Docker Compose pourrait-il trouver ses limites ?

Docker Compose présente des limites lorsqu'il faut gérer une infrastructure importante composée de nombreux conteneurs répartis sur plusieurs machines. Il est principalement conçu pour orchestrer des conteneurs sur une seule machine.