### Justification des Choix Techniques : Kinto-Cloud

---

#### **1. RAID 1 avec ext4**

- **Pourquoi RAID 1 ?**

  - **Redondance des données** : RAID 1 permet une duplication des données sur deux disques. Si un disque tombe en panne, les données restent accessibles sur l'autre. En comparaison, RAID 5 répartit les données et la parité sur plusieurs disques, offrant une tolérance aux pannes avec une meilleure utilisation de l'espace disque, mais il est plus complexe à reconstruire. RAID 10 combine redondance et performance en associant RAID 1 et RAID 0, mais nécessite un nombre de disques plus élevé.
  - **Simplicité de récupération** : En cas de défaillance, la récupération des données est plus directe que d'autres configurations RAID.

- **Pourquoi ext4 ?**

  - **Fiabilité** : ext4 est un système de fichiers mature et stable, largement utilisé dans l'environnement Linux. En comparaison, XFS est souvent privilégié pour les systèmes exigeant de hautes performances en I/O. ext4 a été choisi ici pour son équilibre entre fiabilité, performance, et compatibilité avec Debian.
  - **Performance** : ext4 offre un bon équilibre entre performance et sécurité des données.
  - **Compatibilité** : ext4 est pris en charge nativement par Debian, ce qui facilite sa gestion.

---

#### **2. Choix de Debian**

- **Stabilité** : Debian est réputée pour sa stabilité, ce qui en fait un excellent choix pour un serveur. En comparaison, Ubuntu Server, bien qu'également stable, est parfois considéré comme plus convivial pour les débutants grâce à sa documentation abondante. CentOS, de son côté, est souvent préféré dans des environnements professionnels pour sa version basée sur Red Hat, mais Debian se distingue par son indépendance et sa légèreté, particulièrement utile pour des configurations plus modestes.
- **Sécurité** : Les mises à jour de sécurité de Debian sont régulières et fiables.
- **Légèreté** : Comparé à d'autres distributions, Debian est économe en ressources, ce qui optimise les performances sur des configurations modérées.
- **Large Écosystème** : Une abondance de documentation et une communauté active facilitent le support.

---

#### **3. Docker et Nextcloud**

- **Pourquoi Docker ?**

  - **Isolation** : Chaque service (comme Nextcloud) tourne dans son propre conteneur, évitant les conflits.
  - **Portabilité** : Docker facilite le déploiement sur d’autres machines si nécessaire.
  - **Simplicité des mises à jour** : Les applications dans Docker peuvent être mises à jour en quelques commandes sans affecter le système principal.

- **Pourquoi Nextcloud ?**

  - **Auto-hébergement** : Nextcloud permet de garder le contrôle total sur ses données.
  - **Fonctionnalités riches** : Synchronisation des fichiers, calendriers, contacts, et bien plus.
  - **Communauté active** : Une large communauté garantit des extensions et des corrections régulières.

---

#### **4. WireGuard pour le VPN**

- **Pourquoi WireGuard ?**
  - **Performance** : WireGuard est extrêmement rapide et économe en ressources. En comparaison, OpenVPN offre une compatibilité étendue et une flexibilité grâce à ses multiples protocoles supportés (TCP/UDP), mais il peut être plus lent et gourmand en ressources. IPSec, quant à lui, est souvent utilisé pour les configurations professionnelles et offre une sécurité robuste, mais sa configuration est généralement plus complexe et son déploiement moins intuitif qu'avec WireGuard.
  - **Simplicité de configuration** : Sa configuration est intuitive par rapport à d'autres solutions VPN.
  - **Sécurité** : WireGuard utilise des protocoles cryptographiques modernes et éprouvés.
  - **Adapté à un usage personnel** : Parfait pour fournir un accès externe sécurisé à un serveur domestique.

---

#### **5. Choix de Cloudflare comme DNS**

- **Confidentialité** : Cloudflare DNS promet de ne pas conserver les logs utilisateurs, garantissant une meilleure protection des données. Par rapport à d'autres solutions comme Google DNS ou Quad9, Cloudflare offre un bon compromis entre confidentialité et performances.
- **Performance** : Cloudflare offre des temps de résolution DNS rapides.
- **Disponibilité** : Cloudflare DNS est fiable, avec une haute disponibilité.
- **Alternative à AdGuard** : Bien qu’AdGuard propose un blocage de publicité, Cloudflare est plus adapté pour éviter une trop grande collecte de données.

---

#### **Conclusion**

Ces choix techniques ont été faits en fonction des besoins de fiabilité, de performance, et de contrôle des données pour un serveur personnel et familial. Ce journal évoluera au fil du temps en fonction des ajustements et des améliorations apportées au projet.

