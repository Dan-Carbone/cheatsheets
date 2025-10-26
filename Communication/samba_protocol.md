# Configuration d'un partage SAMBA entre une machine Linux et une machine Windows

Ce guide explique comment configurer un serveur SAMBA sur **VM1_Linux** pour partager `/srv/partage` avec **VM3_Windows**, et permettre à un compte de service d'y accéder.

---

## 1. Installer SAMBA sur VM1_Linux

### Installation

```bash
sudo apt update
sudo apt install samba -y  # Debian/Ubuntu
# ou
sudo yum install samba -y  # CentOS/RHEL
```

### Créer le répertoire de partage

```bash
sudo mkdir -p /srv/partage
sudo chmod 777 /srv/partage  # À ajuster selon vos besoins
```

### Configurer `/etc/samba/smb.conf`

Éditez le fichier :

```bash
sudo nano /etc/samba/smb.conf
```

Ajoutez à la fin :

```ini
[partage]
   path = /srv/partage
   browsable = yes
   read only = no
   guest ok = no
   create mask = 0775
   directory mask = 0775
   valid users = svc-tutu
```

### Créer un utilisateur SAMBA pour svc-tutu

```bash
sudo smbpasswd -a svc-tutu
sudo systemctl restart smbd nmbd
sudo systemctl enable smbd nmbd
```

---

## 2. Accéder au partage depuis VM3_Windows

1. Ouvrez l'explorateur de fichiers.
2. Dans la barre d'adresse, entrez : `\\IP_VM1\partage`
3. Connectez-vous avec le nom d'utilisateur **svc-tutu** et son mot de passe SAMBA.

---

## 3. Vérifier les permissions

Sur **VM1_Linux** :

```bash
sudo chown svc-tutu\:svc-tutu /srv/partage
sudo chmod 750 /srv/partage
```

---
