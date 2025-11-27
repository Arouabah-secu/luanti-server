🌍 Installation de Minetest sur des Conteneurs LXC avec un Dashboard de Gestion 📊

Ce projet vous guide à travers l'installation de serveurs Minetest sur des conteneurs LXC sous Debian ou Ubuntu, ainsi que la mise en place d'un dashboard interactif pour gérer facilement vos serveurs.

🚀 Prérequis

Avant de commencer, assurez-vous de disposer de :

Des containeurs LXC configurés avec des adresses IP distinctes.

Un système Debian ou Ubuntu installé sur les conteneurs.

⚙️ Étapes d'Installation de Minetest
1. 🖥️ Installation de Minetest sur les conteneurs LXC

Pour installer Minetest sur vos conteneurs LXC, procédez comme suit :

Mettre à jour les dépôts et installer le serveur Minetest :

sudo apt update
sudo apt install minetest-server


Configurer les cartes : Déplacez les fichiers de configuration de vos mondes Minetest dans le répertoire /etc/minetest/ :

minetest.conf : Fichier de configuration général du serveur.

world.mt : Configuration spécifique de chaque monde.

Attribuer les bonnes permissions aux fichiers de configuration pour permettre à Minetest de les utiliser :

sudo chown -R Debian-minetest:games /etc/minetest


Redémarrer le serveur Minetest pour appliquer la configuration :

sudo systemctl restart minetest-server

2. 🌐 Rendre les cartes accessibles depuis l'extérieur

Pour que les joueurs puissent accéder à chaque carte depuis l'extérieur, configurez des règles DNAT via iptables sur votre serveur principal pour rediriger les ports vers chaque conteneur.

Par exemple, pour rediriger le port 30000 vers un conteneur avec l'adresse IP 10.0.3.10 :

sudo iptables -A PREROUTING -t nat -p udp -m udp --dport 30000 -j DNAT --to-destination 10.0.3.10:30000


Adaptez l'IP et le port en fonction de votre configuration.

📑 Installation du Dashboard Web

Le dashboard vous permet de gérer facilement les serveurs Minetest via une interface Web. Voici comment l'installer.

1. 🛠️ Installer les dépendances nécessaires

Sur le serveur principal (hôte), installez Apache2 et PHP pour faire fonctionner le dashboard :

sudo apt update
sudo apt install apache2 php php-cli php-common libapache2-mod-php

2. ⚙️ Configurer le Dashboard

Créer le répertoire pour le dashboard :

sudo mkdir -p /var/www/minetest


Déplacer le fichier index.php dans ce répertoire :

sudo mv index.php /var/www/minetest/


Configurer Apache pour pointer vers ce répertoire :

Modifiez le fichier de configuration Apache dans /etc/apache2/sites-available/000-default.conf pour définir DocumentRoot :

DocumentRoot /var/www/minetest


Attribuer les bonnes permissions au répertoire du dashboard pour qu'Apache puisse y accéder :

sudo chown -R www-data:www-data /var/www/minetest


Redémarrer Apache pour appliquer les changements :

sudo systemctl restart apache2

3. 🖌️ Personnaliser le Dashboard

Vous pouvez modifier le fichier index.php selon vos besoins pour personnaliser l'interface ou ajouter des fonctionnalités spécifiques.

📝 Installation des Scripts

Déplacez les scripts .sh dans le répertoire /usr/bin/ pour qu'ils soient accessibles partout :

sudo mv script.sh /usr/bin/


Déplacez les fichiers de service .service dans /etc/systemd/system/ pour pouvoir les gérer avec systemd :

sudo mv myservice.service /etc/systemd/system/


Rechargez systemd pour qu'il prenne en compte les nouveaux services :

sudo systemctl daemon-reload


Assurez-vous que l'utilisateur www-data a les permissions d'exécution sur ces fichiers.

📚 Informations Importantes

Ce projet est compatible avec les environnements suivants :

LXC (Linux Containers) pour l'isolation des serveurs.

Apache2 pour servir le dashboard.

PHP pour exécuter les scripts du dashboard.

🤝 Contribuer au Projet

Les contributions sont bienvenues ! Si vous souhaitez :

Améliorer le code ou ajouter des fonctionnalités,

Signaler des problèmes,

Proposer des corrections ou soumettre une pull request,

n'hésitez pas à le faire sur GitHub !
