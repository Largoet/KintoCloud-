# Kinto-Cloud

(English below)

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

# Kinto-Cloud

**Kinto-Cloud** is a self-hosted cloud solution that allows you to securely store and access your files (such as photos) on your personal server, with a user-friendly interface through Nextcloud. The server is built using Debian, and Docker is used to manage services like Nextcloud for easy deployment and maintenance.

## Project Overview

This project aims to set up a personal server that serves as a cloud storage solution. It involves:

- Setting up a RAID for storage
- Installing Docker to host Nextcloud
- Storing files (mainly photos) securely on the RAID
- Configuring Nextcloud to provide an accessible interface for viewing and managing your files remotely
- Ensuring server security by using best practices and automated configuration with Ansible
- Optionally integrating a VPN for added security

## Prerequisites

- A dedicated server running **Debian** (or your preferred Linux distribution)
- Basic knowledge of Linux commands and system administration
- Docker installed on your server
- Access to the internet for setting up remote access to Nextcloud
- A local network with a router configured to allow remote access if needed

## Setup Instructions

1. **Install Debian on your server**:
   - Follow the official Debian installation guide to set up your server. A minimal installation is recommended to keep the system lightweight.
   
2. **Configure RAID for Storage**:
   - Use the `mdadm` tool to configure RAID for storing your files. Set up a RAID 1 or RAID 5 based on your preference for redundancy and performance.

3. **Install Docker**:
   - Install Docker on your server to easily deploy Nextcloud and other services. You can follow Docker's official installation guide for Debian.

4. **Set Up Nextcloud with Docker**:
   - Use Docker Compose to set up Nextcloud, a popular self-hosted file sync and share application.
   - Pull the Nextcloud Docker image and configure it to mount the RAID storage for your files.

5. **Secure Your Server**:
   - Before exposing your server to the internet, ensure that all necessary security measures are in place.
   - Use **UFW** to configure firewall rules to restrict access.
   - Set up **fail2ban** to protect against brute-force attacks.
   - Use **SSH keys** for remote server access instead of passwords.
   - You can use **Ansible** to automate security settings and keep them consistent across server reboots and updates.

6. **(Optional) Set Up a VPN**:
   - Integrate a VPN on your server for a more secure connection to access your Nextcloud remotely. OpenVPN or WireGuard are good options for this.

7. **Access Nextcloud Remotely**:
   - After securing your server, open the necessary ports on your router to allow remote access to Nextcloud.
   - Use HTTPS for secure access by setting up SSL certificates with **Let’s Encrypt**.

## Security Considerations

It is essential to prioritize security when making your server accessible to the internet. Follow these steps to ensure your system is protected:

- Use a VPN for added security when accessing your server remotely.
- Regularly update your system and Docker images.
- Always use SSH keys for remote access and disable password authentication.

## Technologies Used

- **Operating System**: Debian (or your preferred Linux distribution)
- **File Storage**: RAID (mdadm)
- **Containerization**: Docker, Docker Compose
- **Cloud Storage**: Nextcloud
- **Security Tools**: UFW, fail2ban, SSH, Ansible, OpenVPN/WireGuard (optional)

## License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for more details.

