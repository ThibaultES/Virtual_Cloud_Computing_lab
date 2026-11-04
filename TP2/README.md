# Étape 1

1. Quel est le rôle d’un réseau privé dans une infrastructure Cloud ? D’un routeur externe ?

Un réseau privé sert à communiquer entre les VM de manière sécurisée en les isolant du réseau public.
Le routeur externe sert à communiquer entre les machines du réseau privé et l'extérieur.

2. Pourquoi un fournisseur Cloud propose-t-il autant de catégories de ressources différentes ?

Un fournisseur Cloud propose différentes catégories de ressources pour répondre aux besoins variés des utilisateurs. Selon les applications, certains auront besoin de davantage de CPU, de mémoire vive ou de stockage.

1. En observant la topologie générée par OpenStack, expliquez le chemin emprunté par un paquet réseau envoyé depuis l’extérieur vers une machine virtuelle sur net-discovery.

Un paquet envoyé depuis l'extérieur arrive sur l'adresse IP flottante associée à la machine virtuelle. Il passe ensuite par le routeur externe qui effectue la redirection vers l'adresse IP privée de la machine virtuelle sur le réseau net-discovery.

# Étape 2

1. Quelles sont les étapes nécessaires pour pouvoir se connecter en SSH à une nouvelle machine virtuelle OpenStack depuis la machine hôte ?

- Créer une paire de clés SSH lors de la création de la VM ou utiliser une paire existante.
- Télécharger et enregistrer la clé privée au format PEM dans le dossier .ssh de la machine hôte.
- Vérifier les permissions de la clé privée.
- Associer une adresse IP flottante à la VM.
- Autoriser le trafic SSH (port 22) dans le groupe de sécurité.
- Se connecter avec la commande : ssh -i ~/.ssh/vm-discovery-key.pem ubuntu@<IP_FLOTTANTE>

2. Quel est le rôle de l’adresse IP privée, de l’adresse IP flottante et des Groupes de Sécurité dans cette connexion ?

- L'adresse IP privée permet à la machine virtuelle de communiquer avec les autres machines du réseau privé.
- L'adresse IP flottante est une adresse publique associée à la VM, qui permet de communiquer avec elle depuis l'extérieur.
- Les groupes de sécurité définissent les règles de trafic entrant et sortant autorisées, notamment l'accès SSH sur le port 22.

3. Pourquoi OpenStack distingue-t-il une adresse IP privée d’une IP flottante publique, plutôt que d’attribuer directement une adresse publique à chaque machine virtuelle ?

L'utilisation d'adresses IP privées permet de conserver les machines virtuelles isolées d'Internet et de limiter leur exposition. Les adresses IP flottantes sont attribuées uniquement aux machines qui doivent être accessibles depuis l'extérieur. Cela permet d'économiser les adresses IP publiques et d'améliorer la sécurité de l'infrastructure.

# Étape 3

1. Pourquoi les Security Groups sont-ils indispensables même lorsqu’une machine virtuelle possède une adresse IP publique ?

Par défaut, OpenStack bloque les connexions entrantes vers les machines virtuelles. Les security groups permettent de définir quelles entrées et sorties sont autorisées.

2. Décrivez le chemin parcouru par une requête HTTP envoyée depuis votre ordinateur jusqu’à l’application hello-api exécutée sur votre machine virtuelle.

La requête HTTP envoyée depuis notre ordinateur passe par Internet et arrive sur l'adresse IP flottante de la machine virtuelle. Le routeur redirige ensuite la requête vers l'adresse IP privée de la VM. Le groupe de sécurité vérifie que le port HTTP utilisé est autorisé. Enfin, la requête atteint le service Docker qui redirige le trafic vers le port du conteneur exécutant l'application hello-api.

# Étape 4

1. Quelles différences existe-il entre une image officielle et un snapshot ?

Une image officielle existe et est partagée pour que quiconque puisse s'en servir. Un snapshot est une copie d'une instance existante (donc qui peut être créée par nous-même) et réutilisée comme image pour une nouvelle VM.

2. Quels avantages apporte le redimensionnement d’une machine virtuelle dans un environnement Cloud ?

Le redimensionnement permet d'ajuster l'allocation des ressources à une VM sans la recréer du début et d'optimiser les coûts en évitant d'allouer trop de ressources.

3. Dans quels contextes un administrateur système préférera-t-il créer une nouvelle machine à partir d’un snapshot plutôt que de repartir d’une image vierge ?

Quand le snapshot apporte beaucoup de spécificité, il est préférable de partir d'un snapshot plutôt que d'une image existante et tout reconfigurer.