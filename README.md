# Kinto-Cloud

Kinto-Cloud est un projet de serveur domestique sur **Debian**, permettant de stocker et de gérer des photos de manière sécurisée via **Nextcloud** hébergé dans un container **Docker**. Ce projet utilise des outils comme **RAID** pour une gestion de stockage avancée et **WireGuard** pour sécuriser l'accès au serveur via un VPN local.

## Objectif

L'objectif est de configurer un serveur à domicile pour stocker des données, principalement des photos, et y accéder depuis n'importe quel appareil à distance tout en garantissant une sécurité maximale grâce à un VPN et un environnement Docker. Le tout est géré via une configuration automatisée avec **Ansible** pour simplifier les mises à jour et la gestion de la sécurité.

## Prérequis

- Un serveur physique ou virtuel avec **Debian** installé.
- Connexion Internet.
- Quelques disques durs pour configurer le **RAID** (au moins 2 disques).
- Accès SSH pour configurer le serveur (recommandation de sécurisation via un VPN).

## Structure du projet

1. **Installation de Debian** : Configuration de Debian sur un serveur pour y héberger les services nécessaires.
2. **RAID pour stockage** : Mise en place d'un système de stockage RAID pour séparer les données personnelles (photos) et professionnelles.
3. **Sécurisation du serveur** : Mise en place d'une sécurisation précoce du serveur avant l'installation de Nextcloud.
   - Sécurisation de l'accès SSH.
   - Configuration du firewall (UFW).
   - Installation d'un VPN avec **WireGuard** pour sécuriser l'accès au serveur.
4. **Installation de Docker** : Utilisation de Docker pour héberger des services comme **Nextcloud**.
5. **Nextcloud** : Installation de Nextcloud dans un container Docker pour gérer et accéder aux photos de manière sécurisée et pratique.
6. **Ansible** : Utilisation d'Ansible pour automatiser les tâches de sécurité et de maintenance du serveur.

## Fonctionnalités

- **Accès à Nextcloud** : Une fois connecté au VPN, vous pouvez accéder à vos photos via une interface simple et sécurisée (Nextcloud) depuis n'importe quel appareil (téléphone, PC, etc.).
- **Sécurisation avec VPN** : Le VPN (WireGuard) permet de chiffrer l'accès à votre serveur et à vos données sans exposer les services à Internet.
- **Automatisation avec Ansible** : Ansible permet de gérer et mettre à jour le serveur de manière sécurisée et rapide, facilitant la maintenance et la gestion de la sécurité du serveur.

## Sécurisation

1. **Sécurisation SSH** : Avant d'installer Nextcloud, configurer l'accès SSH avec des clés et désactiver l'accès par mot de passe.
2. **Firewall** : Mise en place d'un pare-feu avec **UFW** pour restreindre l'accès aux services et ouvrir uniquement les ports nécessaires.
3. **WireGuard VPN** : Installation et configuration de WireGuard pour établir un tunnel VPN local sécurisé. Ce VPN garantit que l'accès à votre serveur est uniquement possible via une connexion sécurisée.
4. **Ansible** : Utilisation d'Ansible pour appliquer des configurations de sécurité sur le serveur de manière cohérente et fiable, notamment pour la gestion du pare-feu et des mises à jour de sécurité.

## Étapes de mise en place

1. **Installation de Debian** :
   - Installer Debian sur le serveur et effectuer les configurations de base.

2. **Configurer le RAID** :
   - Configurer un RAID pour séparer les photos et les projets professionnels.

3. **Sécuriser le serveur** :
   - Sécuriser l'accès SSH : Désactiver l'authentification par mot de passe, configurer les clés SSH.
   - Configurer le pare-feu avec **UFW** pour restreindre l'accès aux services.
   - Installer et configurer **WireGuard** pour créer un tunnel VPN local sécurisé.

4. **Installer Docker et Nextcloud** :
   - Utiliser Docker pour installer Nextcloud et y configurer le stockage des photos.
   
5. **Utiliser Ansible pour la gestion de la sécurité** :
   - Ansible est utilisé pour automatiser la gestion du serveur et appliquer les meilleures pratiques de sécurité.

## Documentation supplémentaire

Pour chaque étape de mise en place, des instructions détaillées seront fournies pour vous aider à installer et configurer les services sur votre serveur.

---

### Contributions

Les contributions sont les bienvenues ! Si vous avez des idées pour améliorer Kinto-Cloud ou si vous souhaitez aider à la documentation ou au code, n'hésitez pas à ouvrir une **issue** ou une **pull request**.

---

### Licence

Ce projet est sous la licence **MIT**. Consultez le fichier **LICENSE** pour plus de détails.
