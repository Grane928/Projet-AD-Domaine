# Projet-AD

**1. Vérification de l'IP disponible avant attribution**

Avant de pouvoir attribuer une adresse IP statique à ma machine virtuelle, j'avais besoin de m'assurer que l'adresse IP 192.168.1.150 était disponible sur le réseau. Pour cela, j'ai utilisé l'outil nmap, un scanner réseau qui me permet de voir quelles adresses IP sont déjà utilisées dans mon sous-réseau local.

Tout d'abord, j'ai créé un fichier vide où j'allais stocker les résultats de mon scan avec la commande touch. Cela m'a permis de garder une trace de toutes les machines actives sur le réseau :

`touch ip_machine`

Ensuite, j'ai exécuté la commande suivante pour effectuer un scan réseau sur le sous-réseau 192.168.1.0/24 (qui couvre toutes les adresses de 192.168.1.1 à 192.168.1.254) et j'ai redirigé les résultats dans le fichier ip_machine :

`sudo nmap -sn -T4 192.168.1.0/24 > ip_machine`

Cette commande scanne les machines actives sans tester les ports, ce qui me permettait juste de savoir quelles adresses IP étaient en ligne.

Enfin, pour vérifier si 192.168.1.150 était déjà prise, j'ai utilisé grep pour rechercher cette adresse dans le fichier ip_machine. Si l'adresse était déjà utilisée par une autre machine, je n'aurais pas pu l'assigner à ma VM :

`grep "192.168.1.150" ip_machine`

Grâce à cela, je pouvais être sûr que l'IP était disponible avant de l'attribuer à ma machine virtuelle, ce qui m'a permis d'éviter un conflit d'adresses sur le réseau.

<img width="1277" height="805" alt="Capture d’écran 2025-08-24 153704" src="https://github.com/user-attachments/assets/9ea34d3e-e6b7-405b-a2c0-b82c01e51a88" />

**2. Configuration de l'adresse IP statique sur Windows Server**

Etape 01 : Accès aux paramètres de la carte réseau

Une fois que j'ai vérifié la disponibilité de l'IP, j'ai passé à l'étape suivante, qui consiste à configurer une adresse IP statique pour mon serveur Windows.
Pour commencer, j'ai ouvert les paramètres de la carte réseau de la machine virtuelle. Dans le Panneau de configuration de Windows Server, j'ai cliqué sur Ethernet pour accéder à ses propriétés. Ensuite, j'ai sélectionné Propriétés pour configurer l'adresse IP.

<img width="1024" height="764" alt="Capture d’écran 2025-08-24 153846" src="https://github.com/user-attachments/assets/a8f4578a-3ab0-48a1-afa4-79a129dbbdc0" />

Etape 02 : Attribution de l'adresse IP statique

Dans l'onglet Propriétés, j'ai sélectionné Protocole Internet Version 4 (TCP/IPv4) et configuré l'adresse IP manuellement. J'ai entré l'adresse 192.168.1.150, la masque de sous-réseau 255.255.255.0, et la passerelle par défaut 192.168.1.254. Cela a permis de définir une adresse statique pour mon serveur, ce qui est essentiel pour que celui-ci soit stable dans le réseau.

<img width="1023" height="765" alt="Capture d’écran 2025-08-24 154836" src="https://github.com/user-attachments/assets/f833aeb7-3ddf-4a94-ae35-6e9faccfc4ce" />

Etape 03 : Vérification de l'adresse IP

Après avoir configuré l'adresse IP, j'ai ouvert une fenêtre de commande (CMD) et utilisé la commande `ipconfig` pour vérifier que l'adresse IP avait bien été attribuée au serveur. La sortie m'a montré l'IP 192.168.1.150, confirmant que la configuration était correcte et que le serveur était prêt à être utilisé dans le réseau.

<img width="1020" height="768" alt="Capture d’écran 2025-08-24 155756" src="https://github.com/user-attachments/assets/32b4456f-9ab2-4858-a3f3-6ca36c77d9eb" />

**3. Lancement de l'assistant d'ajout de rôles et de fonctionnalités**

<img width="1019" height="766" alt="Capture d’écran 2025-08-24 160145" src="https://github.com/user-attachments/assets/48c2ced4-b19c-4d6f-ad72-929e384de577" />

Etape 01 : Sélection du type d'installation

Dans cette première étape, j'ouvre le gestionnaire de serveur sur Windows Server, puis je commence l'assistant "Add Roles and Features". Cette fenêtre me guide pour l'installation de rôles et de fonctionnalités sur le serveur.

<img width="1023" height="766" alt="Capture d’écran 2025-08-24 160559" src="https://github.com/user-attachments/assets/55e14e38-5e6c-476d-91ac-dd7c729dea31" />

Etape 02 : Sélection du serveur de destination

Ensuite, je choisis l'option "Role-based or feature-based installation". Cela permet de configurer un seul serveur en y ajoutant des rôles et fonctionnalités. Cette étape est cruciale pour la configuration du serveur avec les rôles nécessaires.

<img width="1021" height="770" alt="Capture d’écran 2025-08-24 160618" src="https://github.com/user-attachments/assets/3d17e2c9-226f-42d7-bcdf-f86e7dc3e685" />

Etape 03 : Sélection des rôles serveur

Après avoir choisi le type d'installation, je sélectionne le serveur de destination dans le serveur pool. Je m'assure que le serveur qui doit recevoir les rôles et fonctionnalités est bien sélectionné, ici, le serveur avec l'adresse IP 192.168.1.150.

<img width="1019" height="765" alt="Capture d’écran 2025-08-24 160636" src="https://github.com/user-attachments/assets/bf6a1a5f-0b09-48f2-8867-fead7d83dfd5" />

Etape 04 : Sélection des fonctionnalités supplémentaires

Dans cette étape, je sélectionne les rôles à ajouter à mon serveur. Ici, j'active le rôle "Active Directory Domain Services" qui permet de transformer le serveur en un contrôleur de domaine, une étape essentielle pour la gestion des utilisateurs et des ressources sur le réseau.

<img width="1023" height="767" alt="Capture d’écran 2025-08-24 160658" src="https://github.com/user-attachments/assets/be02b8bc-a748-4eaa-ae2a-2c95662ad2f0" />

Etape 05 : Configuration d'Active Directory Domain Services (AD DS)

Ensuite, je sélectionne les fonctionnalités additionnelles. Dans ce cas, "Group Policy Management" est ajouté, ce qui est indispensable pour gérer les politiques de groupe au sein du domaine.

<img width="1019" height="766" alt="Capture d’écran 2025-08-24 160723" src="https://github.com/user-attachments/assets/3acd4db6-6f9c-4e58-9250-45c5bb0fac9b" />

Etape 06 : Confirmation des sélections

Une fois les rôles et fonctionnalités installés, je configure Active Directory Domain Services. Cela inclut la création du domaine et la configuration des contrôleurs de domaine pour assurer le bon fonctionnement de l'annuaire des utilisateurs.

<img width="1020" height="766" alt="Capture d’écran 2025-08-24 160740" src="https://github.com/user-attachments/assets/2b1c04cc-4b00-47b2-b93f-4bc4af67994f" />

Etape 07 : Progression de l'installation

Avant d'installer les rôles et fonctionnalités, je confirme toutes les sélections que j'ai faites : rôles, fonctionnalités et configurations supplémentaires. C'est ici que je peux revoir mes choix et valider avant l'installation finale.

L'installation commence, et l'assistant affiche l'état de la progression. Il est important de vérifier que toutes les étapes sont correctement suivies avant d'aller plus loin dans la configuration du domaine.

<img width="1019" height="767" alt="Capture d’écran 2025-08-24 161036" src="https://github.com/user-attachments/assets/b1096031-3a6b-4eb4-b2af-a1641e1d4e7f" />

**4. Configuration post-déploiement**

Etape 01 : Création du domaine

Une fois l'installation terminée, une alerte m'a indiqué de promouvoir le serveur en contrôleur de domaine. J'ai cliqué sur l'alerte pour démarrer cette étape essentielle.
J'ai choisi l'option pour ajouter un nouveau domaine dans une nouvelle forêt, puis défini mydomaine.local comme nom de domaine.

<img width="1025" height="767" alt="Capture d’écran 2025-08-24 161639" src="https://github.com/user-attachments/assets/c8b1e1ea-ff84-4cc6-aac7-6f4e00437ffc" />

Etape 02 : Options du contrôleur de domaine

J'ai sélectionné Windows Server 2016 pour les niveaux fonctionnels et activé les services DNS et le catalogue global. J'ai aussi défini un mot de passe pour le mode de récupération.

<img width="1019" height="764" alt="Capture d’écran 2025-08-24 162006" src="https://github.com/user-attachments/assets/6a81eccb-c62e-4942-9a90-8d372620c7f0" />

Etape 03 : Vérification du nom NetBIOS

J'ai vérifié et laissé par défaut le nom NetBIOS mydomaine, adapté pour mon réseau local.

<img width="1020" height="764" alt="Capture d’écran 2025-08-24 162328" src="https://github.com/user-attachments/assets/b8dbee45-f568-4bca-b398-b301bd79b854" />

Etape 04 : Choix des chemins d'installation

Les chemins par défaut pour les bases de données AD DS et SYSVOL étaient suffisants, donc je les ai laissés tels quels.

<img width="1019" height="764" alt="Capture d’écran 2025-08-24 162538" src="https://github.com/user-attachments/assets/08fa536a-3877-4066-ab62-be694008b261" />

Etape 05 : Révision des paramètres

J'ai revu et validé mes configurations avant de continuer l'installation.

<img width="1021" height="766" alt="Capture d’écran 2025-08-24 162622" src="https://github.com/user-attachments/assets/8c7cfd76-1430-4391-884c-a9a002979572" />

Etape 06 : Vérification des prérequis

Les prérequis ont été validés avec succès, ce qui a permis de poursuivre l'installation.

<img width="1019" height="767" alt="Capture d’écran 2025-08-24 163204" src="https://github.com/user-attachments/assets/11d19514-c2c7-4818-bf78-7ecec1e98296" />

Etape 07 : Vérification finale

Une fois l'installation terminée, le gestionnaire de serveur a confirmé que les rôles AD DS et DNS étaient installés et actifs.

<img width="1020" height="764" alt="Capture d’écran 2025-08-24 164642" src="https://github.com/user-attachments/assets/832ca7d5-ff74-48ca-97cf-02399f73a152" />









