# Nexus360 – Infrastructure systèmes et réseaux multi-sites

## 1. Présentation du projet

Nexus360 est un projet de mise en place d'une infrastructure informatique multi-sites dans un environnement virtualisé.

L'objectif est de construire une infrastructure représentant le système d'information d'une entreprise, avec plusieurs sites interconnectés, des serveurs Windows et Linux, des postes clients, des services d'annuaire, ainsi qu'une solution de supervision.

Le projet s'appuie principalement sur VMware pour la virtualisation et Tailscale pour la connectivité entre les différentes machines.

## 2. Objectifs

Les principaux objectifs du projet sont :

- Déployer une infrastructure composée de plusieurs sites.
- Mettre en place un plan d'adressage IP cohérent.
- Centraliser l'authentification avec Active Directory.
- Configurer les services DNS et DHCP.
- Déployer des serveurs Windows et Linux.
- Mettre en place des services de fichiers et de gestion.
- Déployer des services Web et applicatifs.
- Automatiser certaines tâches d'administration avec Ansible.
- Mettre en place une solution de supervision et d'alertes.
- Assurer la communication entre les différents sites avec Tailscale.

## 3. Architecture réseau

L'infrastructure utilise la plage d'adresses privée :

`10.36.0.0/16`

Chaque site dispose de son propre sous-réseau en `/24`.

| Site | Sous-réseau | Rôle principal |
|---|---|---|
| Site 11 | `10.36.11.0/24` | Contrôleur de domaine principal |
| Site 12 | `10.36.12.0/24` | Second site Active Directory |
| Site 13 | `10.36.13.0/24` | Serveurs Linux |
| Site 14 | `10.36.14.0/24` | Postes clients Windows 11 |
| Site 15 | `10.36.15.0/24` | Serveur applicatif Linux |

Les machines virtuelles sont hébergées avec VMware. Les réseaux internes permettent de séparer les différents sites.

Tailscale est utilisé pour établir une connectivité privée entre les machines participantes, même lorsqu'elles sont hébergées sur des ordinateurs physiques différents.

Les passerelles Linux dédiées ont été envisagées pendant le projet, mais ne sont pas retenues dans l'architecture finale présentée.

## 4. Répartition du travail

Le projet est réparti entre cinq membres.

### Personne 1 – Jade : Active Directory et services réseau Windows

Machines principales :
- `SRV-DC01`
- `SRV-DC02`

Missions :
- Installation et configuration d'Active Directory Domain Services.
- Mise en place du domaine.
- Configuration du DNS.
- Configuration du DHCP.
- Gestion des contrôleurs de domaine.
- Préparation de l'authentification centralisée des clients.

Domaine utilisé :

`ad.nexus360.test`

Nom NetBIOS :

`NEXUS360`

### Personne 2 – Alex : Administration et services Windows

Machines principales :
- `SRV-FS01`
- `SRV-MGT01`

Missions :
- Mise en place des services de fichiers.
- Gestion des partages réseau et des permissions.
- Administration des serveurs Windows.
- Gestion des mises à jour avec WSUS.
- Centralisation et collecte des événements Windows.
- Administration à distance avec les outils adaptés.

### Personne 3 – Ronan : Serveurs Linux et automatisation

Machines principales :
- `LNX-WEB01`
- `LNX-APP01`
- `LNX-OPS01`

Missions :
- Déploiement du serveur Web de l'intranet avec Nginx.
- Mise en place du serveur applicatif.
- Administration des serveurs Linux.
- Automatisation des tâches avec Ansible.
- Préparation des opérations de stockage et de sauvegarde.
- Recherche d'une configuration reproductible et idempotente.

Machines identifiées :
- `LNX-WEB01` : `10.36.13.21`
- `LNX-APP01` : `10.36.15.24`

### Personne 4 – Gloria : Supervision et suivi de l'infrastructure

Machine principale :
- `LNX-MON01`

Missions :
- Mise en place de la supervision avec Prometheus.
- Visualisation des métriques avec Grafana.
- Configuration des alertes.
- Suivi de l'état des machines et des services.
- Centralisation et exploitation des journaux.
- Suivi des sauvegardes.
- Participation à l'intégration continue et au suivi du dépôt Git.

### Personne 5 – Kenza : Réseau et postes clients

Machines principales :
- `CLI-W11-01`
- `CLI-W11-02`

Missions :
- Préparation des postes clients Windows 11.
- Configuration des interfaces réseau VMware.
- Mise en place du réseau interne du Site 14.
- Configuration de l'adressage IP.
- Installation et configuration de Tailscale.
- Vérification de la connectivité entre les postes et les serveurs.
- Préparation de l'intégration des clients au domaine Active Directory.
- Réalisation des tests réseau.

## 5. Configuration des postes clients – Site 14

Le Site 14 utilise le réseau :

`10.36.14.0/24`

Le segment réseau VMware utilisé est :

`SITE14-LAN`

| Machine | Fonction | Configuration IP |
|---|---|---|
| `CLI-W11-01` | Comptabilité | `10.36.14.101` – réservation DHCP |
| `CLI-W11-02` | Direction | DHCP dynamique |

Les deux postes possèdent deux interfaces réseau :
- Une interface connectée au réseau interne `SITE14-LAN`.
- Une interface NAT permettant l'accès à Internet via VMware.

La configuration NAT observée pendant les tests utilise le réseau `192.168.222.0/24`, avec la passerelle `192.168.222.2`.

## 6. Services Active Directory et DNS

Le contrôleur de domaine principal est :

`SRV-DC01`

Son adresse sur le réseau du Site 11 est :

`10.36.11.10`

Son adresse Tailscale observée pendant les tests est :

`100.94.94.52`

Le domaine Active Directory est :

`ad.nexus360.test`

Les vérifications réalisées sur DC01 ont confirmé :
- La présence de la zone DNS `ad.nexus360.test`.
- La présence de la zone `_msdcs.ad.nexus360.test`.
- L'intégration de la zone DNS à Active Directory.
- Le fonctionnement du service Active Directory (NTDS).
- Le fonctionnement du service DNS.
- Le fonctionnement du service Netlogon.
- La résolution de l'enregistrement SRV LDAP du domaine.

## 7. Configuration DHCP

Une étendue DHCP a été créée pour le Site 14.

| Paramètre | Valeur |
|---|---|
| Réseau | `10.36.14.0/24` |
| Début de plage | `10.36.14.100` |
| Fin de plage | `10.36.14.200` |
| W11-01 | Réservation `10.36.14.101` |
| W11-02 | Attribution dynamique prévue |

La réservation de W11-01 est présente sur le serveur DHCP et son adresse apparaît dans les baux.

L'attribution dynamique à W11-02 reste à valider. Une communication DHCP entre sites nécessite un relais DHCP ou un serveur accessible sur le réseau local concerné : les requêtes DHCP classiques ne traversent pas directement le réseau Tailscale.

## 8. Connectivité avec Tailscale

Tailscale permet de relier les machines du projet à travers un réseau privé.

Les deux postes Windows 11 ont été installés sur le tailnet du groupe.

Tests réalisés :

- Communication de W11-02 vers W11-01 : réussie.
- Communication de W11-02 vers DC01 : réussie.
- Présence de DC01 dans le réseau Tailscale : confirmée.

Les commandes suivantes permettent de vérifier la connectivité :

```powershell
tailscale status
tailscale ip -4
```

Pour tester la communication avec une autre machine :

```powershell
tailscale ping ADRESSE_TAILSCALE
```

## 9. Tests et validation

Plusieurs vérifications ont été effectuées pendant le déploiement.

| Test | Résultat |
|---|---|
| Installation des deux clients Windows 11 | Réalisée |
| Configuration des interfaces VMware | Réalisée |
| Connexion des deux clients à Tailscale | Réalisée |
| Communication W11-02 vers W11-01 | Réussie |
| Communication W11-02 vers DC01 | Réussie |
| Vérification des services AD et DNS de DC01 | Réalisée |
| Vérification de la zone DNS du domaine | Réussie |
| Étendue DHCP du Site 14 | Créée |
| Réservation DHCP de W11-01 | Présente |
| Attribution DHCP dynamique de W11-02 | À finaliser |
| Intégration de W11-01 au domaine | À finaliser |
| Intégration de W11-02 au domaine | À finaliser |

## 10. Points restant à finaliser

Les points suivants restent à traiter ou à valider :

- Finaliser l'attribution DHCP dynamique de W11-02 sur le réseau Site 14.
- Vérifier le mécanisme de relais DHCP si nécessaire.
- Finaliser la configuration DNS des deux postes clients.
- Vérifier que les services Active Directory de DC01 sont accessibles depuis les clients.
- Valider les enregistrements DNS et les adresses publiées par DC01.
- Intégrer `CLI-W11-01` au domaine `ad.nexus360.test`.
- Intégrer `CLI-W11-02` au domaine `ad.nexus360.test`.
- Vérifier la disponibilité de DC02 et la réplication Active Directory.
- Effectuer les tests globaux entre les différents services et sites.
- Finaliser la documentation technique et les captures de validation.

## 11. Environnement et technologies

| Technologie | Utilisation |
|---|---|
| VMware | Virtualisation des machines |
| Windows Server | Services d'infrastructure et administration |
| Windows 11 | Postes clients |
| Active Directory | Gestion centralisée des identités |
| DNS | Résolution des noms |
| DHCP | Attribution automatique des adresses IP |
| Tailscale | Connectivité privée intersites |
| Rocky Linux | Hébergement de certains services Linux |
| Ubuntu Server | Serveur applicatif |
| Nginx | Serveur Web |
| Ansible | Automatisation et configuration |
| Prometheus | Collecte des métriques |
| Grafana | Visualisation et tableaux de bord |
| Git | Gestion du code et de la documentation |

## 12. Conclusion

Le projet Nexus360 a permis de mettre en place une infrastructure virtualisée multi-sites et de travailler sur différents aspects de l'administration systèmes et réseaux.

Chaque membre a contribué à une partie de l'infrastructure : services Windows, Active Directory, serveurs Linux, automatisation, supervision et connectivité réseau.

Les premières communications entre les postes clients et les serveurs ont été validées. Les dernières étapes concernent principalement la finalisation du DHCP, du DNS et de l'intégration des postes Windows 11 au domaine Active Directory.

Ce projet met en pratique les compétences de déploiement, d'administration, de configuration réseau, de supervision et de documentation d'une infrastructure informatique.

---

**Projet :** Nexus360  
**Type :** Infrastructure systèmes et réseaux multi-sites  
**Virtualisation :** VMware  
**Dépôt :** Git
