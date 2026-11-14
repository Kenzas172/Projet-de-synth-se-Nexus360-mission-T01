# Installation et configuration d'Active Directory — Nexus360

## 1. Présentation

Pour le projet Nexus360, j'ai installé Windows Server 2025 afin de créer le serveur principal du domaine.

Active Directory permet de gérer les utilisateurs et les ordinateurs du réseau.

## 2. Informations du serveur

- Nom : SRV-DC01
- Système : Windows Server 2025
- Logiciel : VMware Workstation
- Adresse IP : 10.36.11.10
- Domaine : ad.nexus360.test

## 3. Configuration réseau

J'ai configuré le réseau du serveur dans VMware Workstation.

J'ai ensuite attribué une adresse IP fixe à SRV-DC01 pour qu'il garde toujours la même adresse sur le réseau.

## 4. Installation d'Active Directory

J'ai installé le rôle Active Directory Domain Services (AD DS).

J'ai ensuite configuré le serveur pour créer le domaine :

**ad.nexus360.test**

SRV-DC01 est devenu le premier contrôleur de domaine du projet.

## 5. Difficultés rencontrées

Lors de l'installation d'Active Directory, je n'arrivais pas à créer le domaine.

Le problème venait du compte Administrateur de Windows Server 2025, car je n'avais pas défini son mot de passe lors de l'installation du système.

J'ai corrigé ce problème en définissant un mot de passe pour le compte Administrateur.

J'ai également défini un mot de passe pour le mode de restauration DSRM pendant la configuration d'Active Directory.

Après ces corrections, j'ai pu terminer la création du domaine `ad.nexus360.test`.

## 6. Vérification

J'ai ouvert la console « Utilisateurs et ordinateurs Active Directory ».

J'ai vérifié que le domaine `ad.nexus360.test` apparaissait bien.

## 7. Rôle du serveur

SRV-DC01 permet de :

- Gérer les utilisateurs.
- Gérer les ordinateurs.
- Gérer les groupes.
- Gérer les connexions au domaine.
- Fournir les services DNS et DHCP (configuration DHCP à vérifier).

## 8. Prochaines étapes

- Vérifier le DNS.
- Configurer le DHCP.
- Ajouter le deuxième contrôleur de domaine SRV-DC02.
- Créer les utilisateurs et les groupes.
- Configurer les règles de sécurité.

## 9. Bilan

J'ai réussi à installer Active Directory et à créer le domaine Nexus360.

Cette étape permet de commencer la gestion centralisée du réseau.

La suite sera de configurer les autres services et d'ajouter les autres machines au domaine.
