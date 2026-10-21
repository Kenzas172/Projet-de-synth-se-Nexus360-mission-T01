# Installation de SRV-DC01

## 1. Présentation

SRV-DC01 est le serveur principal de l'infrastructure Nexus360.
Il héberge les services nécessaires à la gestion centralisée
des utilisateurs et des ordinateurs du domaine.

## 2. Environnement technique

- Système : Windows Server 2025 Standard Evaluation
- Mode d'installation : Desktop Experience
- Hyperviseur : VMware Workstation
- Nom du serveur : SRV-DC01
- Adresse IPv4 : 10.36.11.10/24
- Domaine Active Directory : ad.nexus360.test

## 3. Configuration réalisée

- Installation de Windows Server 2025.
- Configuration du nom du serveur.
- Configuration de l'adresse IPv4 statique.
- Installation du rôle Active Directory Domain Services (AD DS).
- Promotion du serveur en contrôleur de domaine.
- Création du domaine ad.nexus360.test.
- Vérification du domaine avec la console
  Utilisateurs et ordinateurs Active Directory.

## 4. Services prévus

- Active Directory : gestion des comptes et des ordinateurs.
- DNS : résolution des noms du domaine.
- DHCP : attribution automatique des adresses IP.

## 5. État d'avancement

Le domaine Active Directory a été créé.
La configuration et les tests des services DNS et DHCP
restent à documenter.

## 6. Prochaines étapes

- Vérifier la zone DNS du domaine.
- Tester la résolution DNS.
- Configurer et tester le service DHCP.
- Préparer l'intégration du serveur SRV-DC02.
