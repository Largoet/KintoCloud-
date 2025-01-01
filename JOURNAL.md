### Journal de bord : Kinto-Cloud

---

#### **1. Installation initiale du serveur**

- **Installation de Debian** :
  - Version installée : Debian GNU/Linux 12 (Bookworm).
  - Commande pour vérifier la version :
    ```bash
    lsb_release -a
    ```

- **Mise à jour des paquets** :
  - Commande :
    ```bash
    sudo apt update && sudo apt upgrade -y
    ```

- **Ajout de `rsyslog` pour la journalisation** :
  - Installation :
    ```bash
    sudo apt install rsyslog
    ```
  - Vérification de son état :
    ```bash
    sudo systemctl status rsyslog
    ```

---

#### **2. Configuration RAID**

- **Création du RAID 1 avec `mdadm`** :
  - Commande :
    ```bash
    sudo mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/sdb1 /dev/sdc1
    ```
  - Vérification de l'état :
    ```bash
    cat /proc/mdstat
    sudo mdadm --detail /dev/md0
    ```

- **Formatage du RAID** :
  - Formatage en `ext4` :
    ```bash
    sudo mkfs.ext4 /dev/md0
    ```

- **Montage du RAID** :
  - Création du point de montage :
    ```bash
    sudo mkdir -p /mnt/raid
    ```
  - Montage temporaire :
    ```bash
    sudo mount /dev/md0 /mnt/raid
    ```
  - Vérification :
    ```bash
    df -h
    ```

- **Ajout au fichier `fstab` pour montage automatique** :
  - Ajout de la ligne suivante dans `/etc/fstab` :
    ```
    UUID=<UUID_du_RAID> /mnt/raid ext4 defaults 0 2
    ```
  - Remplacement de `<UUID_du_RAID>` par la valeur obtenue avec :
    ```bash
    sudo blkid
    ```

---

#### **3. Installation de Docker et Nextcloud**

- **Installation de Docker** :
  - Commandes :
    ```bash
    sudo apt install docker.io docker-compose
    ```
  - Vérification de l'installation :
    ```bash
    sudo docker --version
    sudo docker-compose --version
    ```

- **Configuration de Nextcloud avec Docker Compose** :
  - Fichier `docker-compose.yml` :
    ```yaml
    version: '3.3'

    services:
      db:
        image: mariadb
        container_name: nextcloud-db
        restart: always
        volumes:
          - db_data:/var/lib/mysql
        environment:
          MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
          MYSQL_PASSWORD: ${MYSQL_PASSWORD}
          MYSQL_DATABASE: ${MYSQL_DATABASE}
          MYSQL_USER: ${MYSQL_USER}

      app:
        image: nextcloud
        container_name: nextcloud-app
        ports:
          - "8080:80"
        restart: always
        links:
          - db
        volumes:
          - /mnt/raid/nextcloud_data:/var/www/html
        environment:
          MYSQL_DATABASE: ${MYSQL_DATABASE}
          MYSQL_USER: ${MYSQL_USER}
          MYSQL_PASSWORD: ${MYSQL_PASSWORD}
          MYSQL_HOST: db

    volumes:
      db_data:
    ```

  - Fichier `.env` :
    ```env
    MYSQL_ROOT_PASSWORD=your_root_password
    MYSQL_PASSWORD=your_db_password
    MYSQL_DATABASE=nextcloud
    MYSQL_USER=nextcloud
    ```

- **Démarrage des containers** :
  - Commandes :
    ```bash
    sudo docker-compose up -d
    ```
  - Vérification des containers :
    ```bash
    sudo docker ps
    ```

- **Connexion à l'interface Nextcloud** :
  - Accès à l'interface via : `http://<IP_DU_SERVEUR>:8080`.
  - Création du compte administrateur et configuration de la base de données.

---

#### **4. Intégration des données sur le RAID**

- **Vérification des données** :
  - Les fichiers téléversés sur Nextcloud sont enregistrés dans `/mnt/raid/nextcloud_data`.
  - Commandes :
    ```bash
    sudo ls -l /mnt/raid/nextcloud_data
    sudo ls -l /mnt/raid/nextcloud_data/<user>/files
    ```

- **Modification des permissions** :
  - Pour s'assurer que `www-data` a les droits nécessaires :
    ```bash
    sudo chown -R www-data:www-data /mnt/raid/nextcloud_data
    ```

---

#### **5. Prochaines étapes**

- **Mise en place de la sécurité** :
  - Installation et configuration d'un VPN WireGuard pour un accès sécurisé au serveur.
  - Renforcement de la configuration SSH (changement de port, authentification par clé uniquement).

- **Automatisation avec Ansible** :
  - Exploration des cas d'usage et création de playbooks pour automatiser les tâches répétitives.

---


