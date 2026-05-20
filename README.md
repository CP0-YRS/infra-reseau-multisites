# Infrastructure Réseau Multi-Sites

Conception et déploiement de l'infrastructure réseau d'une entreprise avec un siège et une agence distante, simulée sur Cisco Packet Tracer.

## Architecture

Le siège dispose de 4 services (Direction, Comptabilité, Développement, RH) segmentés par des VLANs distincts. Deux routeurs assurent la redondance de la passerelle par défaut via le protocole HSRP. Le routage dynamique OSPF multi-area assure l'interconnexion entre le siège et l'agence distante à travers un lien WAN série.

## Technologies et Protocoles

| Protocole / Norme | Rôle Technique |
| :--- | :--- |
| **802.1Q** | Tagging et encapsulation des VLANs sur les liaisons trunks |
| **STP (Spanning Tree Protocol)** | Prévention des boucles de commutation de couche 2 |
| **OSPF (Open Shortest Path First)** | Routage dynamique intra-domaine entre les sites (Area 0 + Area 1) |
| **HSRP (Hot Standby Router Protocol)** | Redondance de la passerelle par défaut et basculement automatique (Failover) |
| **DHCP** | Attribution dynamique et centralisée des configurations IP clients |
| **ACL (Access Control Lists)** | Filtrage de sécurité et contrôle du trafic inter-VLAN |

## Plan d'Adressage IP

| Site | VLAN | Service / Liaison | Sous-Réseau | Passerelle Virtuelle (HSRP) |
| :--- | :--- | :--- | :--- | :--- |
| **Siège** | 10 | Direction | 172.16.10.0/24 | 172.16.10.1 |
| **Siège** | 20 | Comptabilité | 172.16.20.0/24 | 172.16.20.1 |
| **Siège** | 30 | Développement | 172.16.30.0/24 | 172.16.30.1 |
| **Siège** | 40 | RH | 172.16.40.0/24 | 172.16.40.1 |
| **Agence** | 50 | Commercial | 172.16.50.0/24 | 172.16.50.1 |
| **Agence** | 60 | Support | 172.16.60.0/24 | 172.16.60.1 |
| **WAN** | — | Liaison Série R1-R3 | 10.0.0.0/30 | — |

## Équipements Rapprochés

* **Routeurs :** 3x Cisco 2911 (R1, R2, R3)
* **Commutateur de Cœur :** 1x Cisco 3560 L3 (SW-CORE)
* **Commutateurs d'Accès :** 3x Cisco 2960 (SW-ACCES1, SW-ACCES2, SW-AGENCE)
* **Terminaux :** 8x Stations de travail (PCs) réparties dans les différents VLANs

## Validation et Tests Opérationnels

* **Routage Inter-VLAN :** Validation des flux via requêtes ICMP (Direction vers Comptabilité)
* **Routage Inter-Sites :** Convergence de la table de routage OSPF validée par l'interconnexion Siège vers Agence
* **Haute Disponibilité :** Test de basculement HSRP fonctionnel (Coupure de R1, reprise transparente des flux par R2)
* **Services Réseau :** Attribution correcte des baux DHCP pour l'intégralité des terminaux

## Incidents Identifiés et Résolutions

* **Port-Security :** Verrouillage intempestif d'une interface en état `err-disabled` suite à une erreur de brassage. Résolution par réalignement de la configuration et réinitialisation de l'interface (`shutdown` / `no shutdown`).
* **Encapsulation Trunk (L3 Switch) :** Rejet initial de la commande `switchport mode trunk` sur le commutateur Cisco 3560. Résolu en spécifiant explicitement le protocole de liaison au préalable via `switchport trunk encapsulation dot1q`.
* **Assignation des Ports :** Défaut d'attribution IP DHCP sur certaines stations dû à un positionnement incorrect du port en mode `trunk` au lieu de `access`.

## Compétences Technologiques Acquises

* Conception et optimisation d'un plan d'adressage IP VLSM.
* Configuration avancée des VLANs, trunks 802.1Q et architectures de routage inter-VLAN (Router-on-a-Stick et Switch L3).
* Implémentation du routage dynamique hiérarchique OSPF Multi-Area.
* Mise en œuvre de mécanismes de haute disponibilité et de redondance de premier niveau (HSRP).
* Diagnostic de pannes réseaux de niveaux 2 et 3 via l'analyse des tables de routage, d'adjacence et des états d'interfaces.
