# Migration de SAMBA vers NFS entre VM1_Linux et VM2_Linux

Ce guide décrit les étapes pour désinstaller SAMBA de **VM1_Linux** et configurer un serveur NFS pour partager `/srv/partage` avec **VM2_Linux**, permettant au compte de service **svc-tutu** d'accéder aux données.

---

## 1. Désinstaller SAMBA sur VM1_Linux

Arrêtez et désinstallez SAMBA :

```bash
sudo systemctl stop smbd nmbd winbind
sudo systemctl disable smbd nmbd winbind
sudo apt purge samba samba-common -y  # Pour Debian/Ubuntu
# ou
sudo yum remove samba samba-common -y # Pour CentOS/RHEL
sudo rm -rf /etc/samba/
```

Vérifiez que les services sont bien désactivés :

```bash
sudo systemctl status smbd nmbd winbind
```

---

## 2. Installer et configurer le serveur NFS sur VM1_Linux

### Installation du serveur NFS

```bash
sudo apt update
sudo apt install nfs-kernel-server -y  # Pour Debian/Ubuntu
# ou
sudo yum install nfs-utils -y         # Pour CentOS/RHEL
```

### Créer le répertoire de partage

```bash
sudo mkdir -p /srv/partage
sudo chown nobody\:nogroup /srv/partage
sudo chmod 777 /srv/partage  # À ajuster selon vos besoins de sécurité
```

### Configurer le partage NFS

Éditez le fichier `/etc/exports` :

```bash
sudo nano /etc/exports
```

Ajoutez la ligne suivante (remplacez `IP_VM2` par l'adresse IP de **VM2_Linux**) :

```plaintext
/srv/partage   IP_VM2(rw,sync,no_subtree_check,no_root_squash)
```

### Exporter le partage et démarrer le service NFS

```bash
sudo exportfs -a
sudo systemctl restart nfs-kernel-server
sudo systemctl enable nfs-kernel-server
```

Vérifiez que le partage est bien exporté :

```bash
sudo exportfs -v
```

---

## 3. Configurer le client NFS sur VM2_Linux

### Installer les outils NFS

```bash
sudo apt update
sudo apt install nfs-common -y  # Pour Debian/Ubuntu
# ou
sudo yum install nfs-utils -y   # Pour CentOS/RHEL
```

### Créer un point de montage

```bash
sudo mkdir -p /mnt/vm1_partage
```

### Monter le partage NFS

```bash
sudo mount -t nfs IP_VM1:/srv/partage /mnt/vm1_partage
```
(Remplacez `IP_VM1` par l'adresse IP de **VM1_Linux**.)

### Monter automatiquement au démarrage

Ajoutez la ligne suivante à `/etc/fstab` :

```plaintext
IP_VM1:/srv/partage  /mnt/vm1_partage  nfs  defaults  0  0
```

Testez la configuration :

```bash
sudo mount -a
```

---

## 4. Vérifier les permissions pour le compte svc-tutu

Sur **VM1_Linux**, ajustez les permissions si nécessaire :

```bash
sudo chown svc-tutu\:svc-tutu /srv/partage
sudo chmod 750 /srv/partage  # Exemple : lecture/écriture pour svc-tutu, lecture pour les autres
```

Sur **VM2_Linux**, testez l'accès en tant que **svc-tutu** :

```bash
sudo su - svc-tutu
touch /mnt/vm1_partage/test_file
ls -l /mnt/vm1_partage
```

---

## 5. Résumé des commandes clés

| Étape | Commande |
|-------|----------|
| Désinstaller SAMBA | `sudo apt purge samba samba-common -y` |
| Installer NFS (serveur) | `sudo apt install nfs-kernel-server -y` |
| Configurer `/etc/exports` | `/srv/partage IP_VM2(rw,sync,no_subtree_check)` |
| Exporter le partage | `sudo exportfs -a` |
| Installer NFS (client) | `sudo apt install nfs-common -y` |
| Monter le partage | `sudo mount -t nfs IP_VM1:/srv/partage /mnt/vm1_partage` |

---

## Remarques importantes

- **Sécurité** : Limitez l'accès au partage NFS aux adresses IP nécessaires.
- **Pare-feu** : Assurez-vous que le port NFS (2049) est ouvert entre les VMs.
- **Sauvegarde** : Sauvegardez les données avant toute manipulation.

---
