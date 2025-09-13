# Cheat Sheet Apache (Linux)

---

## 1. Installation et Démarrage

<custom-element data-json="%7B%22type%22%3A%22table-metadata%22%2C%22attributes%22%3A%7B%22title%22%3A%22Installation%20et%20gestion%20du%20service%20Apache%22%7D%7D" />

| Commande (Debian/Ubuntu)       | Commande (CentOS/RHEL)         | Description                     |
|--------------------------------|-------------------------------|---------------------------------|
| `sudo apt update`              | `sudo yum update`             | Met à jour les paquets.         |
| `sudo apt install apache2`     | `sudo yum install httpd`      | Installe Apache.                |
| `sudo systemctl start apache2` | `sudo systemctl start httpd`  | Démarre Apache.                 |
| `sudo systemctl stop apache2`  | `sudo systemctl stop httpd`   | Arrête Apache.                  |
| `sudo systemctl restart apache2` | `sudo systemctl restart httpd` | Redémarre Apache.               |
| `sudo systemctl enable apache2` | `sudo systemctl enable httpd` | Active Apache au démarrage.     |
| `sudo systemctl status apache2` | `sudo systemctl status httpd` | Vérifie le statut d'Apache.     |

---

## 2. Fichiers et Répertoires Clés

<custom-element data-json="%7B%22type%22%3A%22table-metadata%22%2C%22attributes%22%3A%7B%22title%22%3A%22Emplacements%20des%20fichiers%20Apache%22%7D%7D" />

| Fichier/répertoire               | Description                                      |
|----------------------------------|--------------------------------------------------|
| `/etc/apache2/`                  | Répertoire de configuration (Debian/Ubuntu).     |
| `/etc/httpd/`                    | Répertoire de configuration (CentOS/RHEL).       |
| `/etc/apache2/apache2.conf`      | Configuration principale (Debian/Ubuntu).        |
| `/etc/httpd/conf/httpd.conf`     | Configuration principale (CentOS/RHEL).          |
| `/etc/apache2/ports.conf`        | Configuration des ports.                         |
| `/etc/apache2/sites-available/`  | Configurations des sites (Debian/Ubuntu).         |
| `/etc/apache2/sites-enabled/`    | Sites activés (liens symboliques, Debian/Ubuntu).|
| `/etc/httpd/conf.d/`             | Configurations supplémentaires (CentOS/RHEL).    |
| `/var/www/html/`                 | Répertoire racine des fichiers web.              |
| `/var/log/apache2/`              | Logs (Debian/Ubuntu).                             |
| `/var/log/httpd/`                | Logs (CentOS/RHEL).                               |

---

## 3. Gestion des Sites Web

### Activer/Désactiver un Site (Debian/Ubuntu)
```bash
sudo a2ensite nom_du_site.conf   # Active un site
sudo a2dissite nom_du_site.conf  # Désactive un site
sudo systemctl reload apache2    # Recharge Apache
