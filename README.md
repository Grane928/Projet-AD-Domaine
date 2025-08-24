# Projet-AD

Étape 1 : Vérification de l'IP disponible avant attribution

Avant de pouvoir attribuer une adresse IP statique à ma machine virtuelle, j'avais besoin de m'assurer que l'adresse IP 192.168.1.150 était disponible sur le réseau. Pour cela, j'ai utilisé l'outil nmap, un scanner réseau qui me permet de voir quelles adresses IP sont déjà utilisées dans mon sous-réseau local.

Tout d'abord, j'ai créé un fichier vide où j'allais stocker les résultats de mon scan avec la commande touch. Cela m'a permis de garder une trace de toutes les machines actives sur le réseau :

`touch ip_machine`

Ensuite, j'ai exécuté la commande suivante pour effectuer un scan réseau sur le sous-réseau 192.168.1.0/24 (qui couvre toutes les adresses de 192.168.1.1 à 192.168.1.254) et j'ai redirigé les résultats dans le fichier ip_machine :

`sudo nmap -sn -T4 192.168.1.0/24 > ip_machine`

Cette commande scanne les machines actives sans tester les ports, ce qui me permettait juste de savoir quelles adresses IP étaient en ligne.

Enfin, pour vérifier si 192.168.1.150 était déjà prise, j'ai utilisé grep pour rechercher cette adresse dans le fichier ip_machine. Si l'adresse était déjà utilisée par une autre machine, je n'aurais pas pu l'assigner à ma VM :

`grep "192.168.1.150" ip_machine`

Grâce à cela, je pouvais être sûr que l'IP était disponible avant de l'attribuer à ma machine virtuelle, ce qui m'a permis d'éviter un conflit d'adresses sur le réseau.

![Description de l'image](https://github.com/Grane928/Projet-AD/blob/main/Capture%20d%E2%80%99%C3%A9cran%202025-08-24%20153704.png?raw=true)

Étape 2 : Configuration de l'adresse IP statique sur Windows Server

Une fois que j'ai vérifié la disponibilité de l'IP, j'ai passé à l'étape suivante, qui consiste à configurer une adresse IP statique pour mon serveur Windows.

1. Accès aux paramètres de la carte réseau

Pour commencer, j'ai ouvert les paramètres de la carte réseau de la machine virtuelle. Dans le Panneau de configuration de Windows Server, j'ai cliqué sur Ethernet pour accéder à ses propriétés. Ensuite, j'ai sélectionné Propriétés pour configurer l'adresse IP.

![Description de l'image](https://raw.githubusercontent.com/Grane928/Projet-AD/ea528575f94ddd5497574107a3539b1c4eab5e00/Capture%20d%E2%80%99%C3%A9cran%202025-08-24%20153846.png?token=BSPLBDJGOXRBBHQU7C6SA73IVM2I2)

2. Attribution de l'adresse IP statique

Dans l'onglet Propriétés, j'ai sélectionné Protocole Internet Version 4 (TCP/IPv4) et configuré l'adresse IP manuellement. J'ai entré l'adresse 192.168.1.150, la masque de sous-réseau 255.255.255.0, et la passerelle par défaut 192.168.1.254. Cela a permis de définir une adresse statique pour mon serveur, ce qui est essentiel pour que celui-ci soit stable dans le réseau.

![Description de l'image](https://raw.githubusercontent.com/Grane928/Projet-AD/ea528575f94ddd5497574107a3539b1c4eab5e00/Capture%20d%E2%80%99%C3%A9cran%202025-08-24%20154836.png?token=BSPLBDNCNRIYEFUC7CSATS3IVM2I2)

3. Vérification de l'adresse IP

Après avoir configuré l'adresse IP, j'ai ouvert une fenêtre de commande (CMD) et utilisé la commande `ipconfig` pour vérifier que l'adresse IP avait bien été attribuée au serveur. La sortie m'a montré l'IP 192.168.1.150, confirmant que la configuration était correcte et que le serveur était prêt à être utilisé dans le réseau.

![Description de l'image](https://raw.githubusercontent.com/Grane928/Projet-AD/ea528575f94ddd5497574107a3539b1c4eab5e00/Capture%20d%E2%80%99%C3%A9cran%202025-08-24%20155756.png?token=BSPLBDPOHJMTFQ5XQQKZTBLIVM2I2)

Étape 1 : Lancement de l'assistant d'ajout de rôles et de fonctionnalités

Image 1 :


Dans cette première étape, j'ouvre le gestionnaire de serveur sur Windows Server, puis je commence l'assistant "Add Roles and Features". Cette fenêtre me guide pour l'installation de rôles et de fonctionnalités sur le serveur.

Étape 2 : Sélection du type d'installation

Image 2 :


Ensuite, je choisis l'option "Role-based or feature-based installation". Cela permet de configurer un seul serveur en y ajoutant des rôles et fonctionnalités. Cette étape est cruciale pour la configuration du serveur avec les rôles nécessaires.

Étape 3 : Sélection du serveur de destination

Image 3 :


Après avoir choisi le type d'installation, je sélectionne le serveur de destination dans le serveur pool. Je m'assure que le serveur qui doit recevoir les rôles et fonctionnalités est bien sélectionné, ici, le serveur avec l'adresse IP 192.168.1.150.

Étape 4 : Sélection des rôles serveur

Image 4 :


Dans cette étape, je sélectionne les rôles à ajouter à mon serveur. Ici, j'active le rôle "Active Directory Domain Services" qui permet de transformer le serveur en un contrôleur de domaine, une étape essentielle pour la gestion des utilisateurs et des ressources sur le réseau.

Étape 5 : Sélection des fonctionnalités supplémentaires

Image 5 :


Ensuite, je sélectionne les fonctionnalités additionnelles. Dans ce cas, "Group Policy Management" est ajouté, ce qui est indispensable pour gérer les politiques de groupe au sein du domaine.

Étape 6 : Configuration d'Active Directory Domain Services (AD DS)

Image 6 :


Une fois les rôles et fonctionnalités installés, je configure Active Directory Domain Services. Cela inclut la création du domaine et la configuration des contrôleurs de domaine pour assurer le bon fonctionnement de l'annuaire des utilisateurs.

Étape 7 : Confirmation des sélections

Image 7 :


Avant d'installer les rôles et fonctionnalités, je confirme toutes les sélections que j'ai faites : rôles, fonctionnalités et configurations supplémentaires. C'est ici que je peux revoir mes choix et valider avant l'installation finale.

Étape 8 : Progression de l'installation

Image 8 :


L'installation commence, et l'assistant affiche l'état de la progression. Il est important de vérifier que toutes les étapes sont correctement suivies avant d'aller plus loin dans la configuration du domaine.




