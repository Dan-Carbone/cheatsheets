# Exemple de configuration complète pour surveiller le port 9252 (GitLab Runner)

## Item

| Paramètre          | Valeur                     |
|--------------------|----------------------------|
| Nom                | GitLab Runner - Port 9252   |
| Clé                | `net.tcp.port[9252]`       |
| Type               | Zabbix agent               |
| Type d'information | Numérique (unsigned)       |
| Intervalle         | 60s                        |

---

## Trigger

| Paramètre          | Valeur                                                                 |
|--------------------|------------------------------------------------------------------------|
| Nom                | GitLab Runner - Port 9252 fermé sur {HOST.NAME}                       |
| Expression         | `{Template_App_GitLab_Runner:net.tcp.port[9252].last(0)}=0`           |
| Description        | Le port 9252 (GitLab Runner) est fermé. Vérifiez le service.          |
| Sévérité           | Haute (ou selon votre politique)                                      |

---