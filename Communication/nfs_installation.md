# Configuration d'un partage NFS entre deux machines Linux

Ce guide explique comment configurer un serveur NFS sur **VM1_Linux** et monter le partage sur **VM2_Linux**, afin que le compte de service **svc-tutu** puisse accéder aux données partagées dans `/srv/partage`.

---

## 1. Installer le serveur NFS sur VM1_Linux

### Installation

```bash
sudo apt update
sudo apt install nfs-kernel-server -y  # Debian/Ubuntu
# ou
sudo yum install nfs-utils -y         # CentOS/RHEL
```

### Créer le répertoire de partage

```bash
sudo mkdir -p /srv/partage
sudo chown nobody\:nogroup /srv/partage
sudo chmod 777 /srv/partage  # À ajuster selon vos besoins
```

### Configurer `/etc/exports`

Éditez le fichier :

```bash
sudo nano /etc/exports
```

Ajoutez la ligne suivante (remplacez `IP_VM2` par l'IP de **VM2_Linux**) :

```plaintext
/srv/partage   IP_VM2(rw,sync,no_subtree_check,no_root_squash)
```

### Exporter et démarrer le service

```bash
sudo exportfs -a
sudo systemctl restart nfs-kernel-server
sudo systemctl enable nfs-kernel-server
```

Vérifiez :

```bash
sudo exportfs -v
```

---

## 2. Configurer le client NFS sur VM2_Linux

### Installer les outils NFS

```bash
sudo apt update
sudo apt install nfs-common -y  # Debian/Ubuntu
# ou
sudo yum install nfs-utils -y   # CentOS/RHEL
```

### Monter le partage

```bash
sudo mkdir -p /mnt/vm1_partage
sudo mount -t nfs IP_VM1:/srv/partage /mnt/vm1_partage
```

### Monter automatiquement au démarrage

Ajoutez à `/etc/fstab` :

```plaintext
IP_VM1:/srv/partage  /mnt/vm1_partage  nfs  defaults  0  0
```

Testez :

```bash
sudo mount -a
```

---

## 3. Vérifier les permissions pour svc-tutu

Sur **VM1_Linux** :

```bash
sudo chown svc-tutu\:svc-tutu /srv/partage
sudo chmod 750 /srv/partage
```

Sur **VM2_Linux** :

```bash
sudo su - svc-tutu
touch /mnt/vm1_partage/test_file
ls -l /mnt/vm1_partage
```

---
s