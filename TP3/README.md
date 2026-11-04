# Étape 1

[Schema de l'architecture](TP3_schema.pdf)

# Étape 2

1. Quels sont les différents rôles de la VM connectée à Internet dans votre architecture ?

Elle sert de point d'entrée pour les utilisateurs qui souhaitent accéder à l'application.
Elle héberge le service frontend (calc-service), qui reçoit les requêtes des utilisateurs.
Elle permet de communiquer avec les autres VM du réseau privé.
Elle peut également servir de passerelle SSH pour accéder aux VM privées depuis l'extérieur.

2. Quels avantages apporte ce déploiement par rapport à une exécution de tous les services sur une seule machine ?

Isoler les services dans des conteneurs permets de garantir plus de sécurité car ils sont isolé du réseau Internet et de permettre une maintenabilité plus facile. S'il faut toucher à un service pour l'améliorer ou le corriger, seul le service correspondant est touché et l'application tourne en continu.

# Étape 3

1. Quels flux réseau sont nécessaires au fonctionnement de votre application ?

Il faut que les VM puissent contacter en HTTP le conteneur calculatrice.
Il faut que les VM puissent échanger en TCP avec la VM du front-end sur le réseau privé.
Il faut pouvoir contacter la calculatrice en HTTP depuis Internet.

2. Pourquoi est-il préférable d’ouvrir uniquement les ports nécessaires plutôt que d’autoriser tout le trafic ?

Pour des raisons de sécurité. Autoriser tout le trafic pourrait permettre à des personnes malveillantes d'accéder à des services qui ne devraient pas être accessibles.

3. Pouviez-vous utiliser Docker Compose pour déployer les services sur toutes les VM en simultané ? Pourquoi ?

Non, car les VM sont isolées, on ne peut pas déployer les services sur toutes les VM en simultané. Docker Compose sert à orchestrer plusieurs conteneurs sur une même machine Docker.