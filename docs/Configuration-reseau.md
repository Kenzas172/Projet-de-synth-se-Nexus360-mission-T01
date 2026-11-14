# Configuration réseau — SRV-DC01

## 1. Cartes réseau

J'ai dû installer et configurer deux cartes réseau sur le serveur.

- Une carte pour le réseau interne du projet Nexus360.
- Une carte pour accéder à Internet grâce au réseau NAT de VMware.

## 2. Configuration IP

Pour le réseau interne, j'ai configuré une adresse IP fixe :

- Adresse IP : 10.36.11.10
- Masque : 255.255.255.0
- Passerelle : 10.36.11.1

## 3. Objectif

Les deux cartes réseau permettent au serveur de communiquer avec le réseau interne et d'accéder à Internet.

## 4. Bilan

La configuration des deux cartes réseau permet à SRV-DC01 de participer au réseau Nexus360 tout en disposant d'un accès Internet.
