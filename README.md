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

