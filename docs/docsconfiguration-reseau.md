# Configuration réseau — Nexus360

## 1. Objectif

Cette étape consiste à préparer les réseaux virtuels
nécessaires au fonctionnement de l'infrastructure Nexus360.

La virtualisation est réalisée avec VMware Workstation
sur un poste hôte Windows 11.

## 2. Création des réseaux virtuels

Deux réseaux virtuels ont été créés dans VMware Workstation.

| Réseau virtuel | Sous-réseau | Masque | Type |
|---|---|---|---|
| VMnet2 | 10.36.11.0/24 | 255.255.255.0 | Host-only |
| VMnet3 | 10.36.12.0/24 | 255.255.255.0 | Host-only |

Le DHCP intégré à VMware a été désactivé sur VMnet3.

## 3. Configuration de SRV-DC01

| Paramètre | Valeur |
|---|---|
| Nom du serveur | SRV-DC01 |
| Adresse IPv4 | 10.36.11.10 |
| Masque | 255.255.255.0 |
| Réseau virtuel | VMnet2 |

Une adresse IPv4 statique a été configurée afin
de conserver une adresse stable pour le contrôleur
de domaine principal.

## 4. Configuration de SRV-DC02

| Paramètre | Valeur |
|---|---|
| Nom du serveur | SRV-DC02 |
| Adresse IPv4 | 10.36.12.11 |
| Masque | 255.255.255.0 |
| Réseau virtuel | VMnet3 |

Le serveur utilise Windows Server 2025 en mode Server Core.

## 5. Points à vérifier

- Vérifier la configuration IPv4 de chaque serveur.
- Vérifier la résolution DNS.
- Mettre en place le routage entre les réseaux si nécessaire.
- Vérifier la connectivité entre les serveurs.

## 6. État d'avancement

Les réseaux virtuels VMnet2 et VMnet3 ont été créés
et les adresses IP des deux serveurs ont été configurées.

Les tests de connectivité et la configuration du routage
restent à effectuer.

La vérification du service DNS sera documentée séparément.
