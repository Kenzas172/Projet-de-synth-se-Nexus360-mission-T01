# Installation et configuration d'Active Directory — Nexus360

## 1. Présentation du projet

Dans le cadre du projet Nexus360, j'ai mis en place un
serveur Windows Server 2025 destiné à devenir le contrôleur
de domaine principal de l'infrastructure.

L'objectif est de centraliser la gestion des utilisateurs,
des ordinateurs et des ressources du réseau grâce à
Active Directory.

## 2. Environnement technique

- Hyperviseur : VMware Workstation
- Système d'exploitation : Windows Server 2025 Standard Evaluation
- Interface : Desktop Experience
- Nom du serveur : SRV-DC01
- Adresse IP : 10.36.11.10
- Masque de sous-réseau : 255.255.255.0
- Nom de domaine : ad.nexus360.test

## 3. Configuration réseau

Avant l'installation d'Active Directory, j'ai configuré
le réseau virtuel du serveur dans VMware Workstation.

Le serveur SRV-DC01 utilise le réseau VMnet2, configuré
en mode Host-only.

Une adresse IPv4 statique a été attribuée au serveur :

- Adresse IP : 10.36.11.10
- Masque : 255.255.255.0

Cette configuration permet de conserver une adresse IP
stable pour le contrôleur de domaine.

## 4. Installation d'Active Directory

J'ai installé le rôle Active Directory Domain Services
(AD DS) depuis le Gestionnaire de serveur de Windows Server.

Après l'installation du rôle, j'ai lancé l'assistant de
configuration afin de promouvoir le serveur en contrôleur
de domaine.

J'ai ensuite créé un nouveau domaine :

**ad.nexus360.test**

Une fois la configuration terminée, le serveur SRV-DC01
est devenu le premier contrôleur de domaine de
l'infrastructure Nexus360.

## 5. Difficultés rencontrées

Lors de la configuration d'Active Directory, j'ai rencontré
une erreur pendant la vérification des prérequis.

Le problème semblait lié à la gestion des mots de passe
du compte Administrateur et du mode de restauration des
services d'annuaire (DSRM).

J'ai dû reprendre certaines étapes de configuration
avant de réussir à terminer la création du domaine.


## 6. Vérification du domaine

Après la création du domaine, j'ai ouvert la console
« Utilisateurs et ordinateurs Active Directory ».

J'ai vérifié que le domaine suivant apparaissait dans
la console :

**ad.nexus360.test**

Cette vérification confirme que le domaine a été créé
et qu'il est visible dans les outils d'administration
Active Directory.

## 7. Rôle de SRV-DC01 dans l'infrastructure

SRV-DC01 est destiné à assurer plusieurs fonctions
dans l'infrastructure Nexus360 :

- Gestion centralisée des comptes utilisateurs.
- Gestion des comptes ordinateurs.
- Organisation des utilisateurs et des groupes.
- Application de stratégies de groupe (GPO).
- Intégration des postes clients au domaine.
- Hébergement du service DNS.
- Hébergement du service DHCP, après sa configuration.

## 8. Prochaines étapes

Les prochaines étapes du projet sont :

- Vérifier la configuration du service DNS.
- Tester la résolution des noms du domaine.
- Configurer et tester le service DHCP.
- Préparer l'intégration de SRV-DC02.
- Organiser les utilisateurs et les groupes Active Directory.
- Étudier la mise en place de stratégies de groupe (GPO).

## 9. Bilan

Cette première étape m'a permis d'installer Windows
Server 2025, de configurer le réseau du serveur et de
créer le domaine Active Directory de Nexus360.

Malgré les difficultés rencontrées pendant la configuration,
j'ai réussi à créer le domaine et à le vérifier dans la
console d'administration.

La suite du projet consistera à configurer les services
réseau et à intégrer progressivement les autres machines
à l'infrastructure.
