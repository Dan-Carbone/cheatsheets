# Désactiver l'auto-login par Active Directory (AD) dans Grafana

Pour désactiver l’**auto-login par Active Directory (AD)** dans Grafana, suivez les étapes ci-dessous selon votre configuration.

---

## 1. Localiser les fichiers de configuration

| Système      | Chemin du fichier principal         | Fichier LDAP/AD (si utilisé)       |
|--------------|-------------------------------------|------------------------------------|
| Linux        | `/etc/grafana/grafana.ini`          | `/etc/grafana/ldap.toml`           |
| Windows      | `C:\Program Files\GrafanaLabs\grafana\conf\grafana.ini` | `C:\Program Files\GrafanaLabs\grafana\conf\ldap.toml` |

---

## 2. Désactiver l’auto-login

### Via LDAP/AD
1. Ouvrez le fichier `ldap.toml`.
2. Recherchez la section `[servers.attributes]` et modifiez :
   ```ini
   auto_login = false
   ```

### Via Anonymous Auth
1. Ouvrez le fichier `grafana.ini`.
2. Dans la section `[auth.anonymous]`, assurez-vous que :
   ```ini
   enabled = false
   ```

---

## 3. Redémarrer Grafana

Appliquez les modifications en redémarrage le service Grafana :

**Linux :**
```bash
sudo systemctl restart grafana-server
```

**Windows :**
```powershell
Restart-Service grafana
```

---

## 4. Vérifications supplémentaires

- **Interface Grafana** : Allez dans **Configuration > Plugins > LDAP** pour vérifier qu’aucune option d’auto-login n’est activée.
- **Reverse Proxy** : Si vous utilisez Apache/Nginx, vérifiez leur configuration pour éviter tout auto-login forcé.

---

## 5. Documentation officielle
Pour plus de détails, consultez la [documentation Grafana](https://grafana.com/docs/grafana/latest/setup-grafana/configure-grafana/).

---