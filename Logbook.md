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

#### **5. Configuration et sécurisation de SSH**

- **Modification du port SSH** :
  - Dans `/etc/ssh/sshd_config`, modification ou ajout de la ligne :
    ```
    Port 2222
    ```
  - Autres paramètres configurés :
    ```
    PermitRootLogin no
    PubkeyAuthentication yes
    PasswordAuthentication no
    ```
  - Redémarrage du service SSH :
    ```bash
    sudo systemctl restart ssh
    ```

- **Ajout des règles UFW** :
  - Installation de UFW :
    ```bash
    sudo apt install ufw
    ```
  - Ouverture des ports nécessaires :
    ```bash
    sudo ufw allow 2222/tcp
    sudo ufw allow 80/tcp
    sudo ufw allow 443/tcp
    ```
  - Activation du pare-feu :
    ```bash
    sudo ufw enable
    ```
  - Vérification des règles :
    ```bash
    sudo ufw status
    ```

- **Test de connexion** :
  - Connexion réussie via :
    ```bash
    ssh -p 2222 <user>@<IP_DU_SERVEUR>
    ```

---

#### **6. Configuration de WireGuard et VPN**

- **Installation de WireGuard** :
  - Commandes :
    ```bash
    sudo apt install wireguard
    ```
  - Création du répertoire :
    ```bash
    sudo mkdir -p /etc/wireguard
    sudo chmod 700 /etc/wireguard
    ```

- **Génération des clés** :
  - Clé privée et publique pour le serveur :
    ```bash
    wg genkey | sudo tee /etc/wireguard/private.key | wg pubkey | sudo tee /etc/wireguard/public.key
    ```
  - Clés pour les clients :
    ```bash
    wg genkey | sudo tee /etc/wireguard/client_private.key | wg pubkey | sudo tee /etc/wireguard/client_public.key
    ```

- **Configuration du serveur (`wg0.conf`)** :
  - Exemple de configuration :
    ```
    [Interface]
    Address = 10.0.0.1/24
    ListenPort = 51820
    PrivateKey = <SERVER_PRIVATE_KEY>
    PostUp = ufw route allow in on wg0 out on eno1
    PostDown = ufw route delete allow in on wg0 out on eno1
    DNS = 1.1.1.1

    [Peer]
    PublicKey = <CLIENT_PUBLIC_KEY>
    AllowedIPs = 10.0.0.2/32
    ```

- **Démarrage de WireGuard** :
  - Activation et démarrage :
    ```bash
    sudo systemctl enable wg-quick@wg0
    sudo systemctl start wg-quick@wg0
    ```
  - Vérification de l'état :
    ```bash
    sudo systemctl status wg-quick@wg0
    ```

- **Configuration des clients (Android)** :
  - **Problème rencontré** :
    Lors de la génération des QR codes pour simplifier la configuration des clients, une erreur de permission a été rencontrée. Le fichier de configuration (`android_sony.conf`) n'était pas accessible pour le générateur de QR code.

  - **Solution** :
    - Vérification et ajustement des permissions :
      ```bash
      sudo chown <user>:<group> /etc/wireguard/android_sony.conf
      sudo chmod 600 /etc/wireguard/android_sony.conf
      ```
    - Ajout de l'utilisateur au groupe WireGuard (si nécessaire) :
      ```bash
      sudo usermod -aG wireguard <user>
      ```
    - Génération réussie du QR code :
      ```bash
      qrencode -t ansiutf8 < /etc/wireguard/android_sony.conf
      ```

  - Exemple de configuration pour Android :
    ```
    [Interface]
    PrivateKey = <ANDROID_PRIVATE_KEY>
    Address = 10.0.0.2/32
    DNS = 1.1.1.1

    [Peer]
    PublicKey = <SERVER_PUBLIC_KEY>
    Endpoint = <SERVER_PUBLIC_IP>:51820
    AllowedIPs = 0.0.0.0/0, ::/0
    PersistentKeepalive = 25
    ```

---

#### **7. Prochaines étapes**

- **Renforcement de la sécurité** :
  - Audit des règles UFW et du VPN.
  - Configuration des certificats SSL/TLS pour Nextcloud.

- **Automatisation avec Ansible** :
  - Préparation de playbooks pour automatiser les tâches d’administration système.

---



