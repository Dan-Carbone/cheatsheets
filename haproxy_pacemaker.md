# Cheatsheet : HAProxy avec Pacemaker

## 1. Statut du cluster et des ressources
| Commande                          | Description                                      |
|-----------------------------------|--------------------------------------------------|
| `sudo crm status`                 | Affiche l'état global du cluster.                |
| `sudo crm resource status`        | Liste l'état de toutes les ressources.           |
| `sudo crm resource status haproxy`| Affiche l'état spécifique de HAProxy.            |
| `sudo crm configure show`         | Affiche la configuration actuelle du cluster.    |

## 2. Gestion de la ressource HAProxy
| Commande                                      | Description                                      |
|-----------------------------------------------|--------------------------------------------------|
| `sudo crm resource start haproxy`             | Démarre la ressource HAProxy.                    |
| `sudo crm resource stop haproxy`              | Arrête la ressource HAProxy.                     |
| `sudo crm resource restart haproxy`           | Redémarre la ressource HAProxy.                  |
| `sudo crm resource migrate haproxy <node>`    | Migre HAProxy vers un nœud spécifique.            |
| `sudo crm resource unmanage haproxy`          | Désactive la gestion automatique de HAProxy.     |
| `sudo crm resource manage haproxy`            | Réactive la gestion automatique de HAProxy.      |
| `sudo crm resource cleanup haproxy`           | Nettoie les erreurs de la ressource HAProxy.     |

## 3. Configuration du cluster
| Commande                          | Description                                      |
|-----------------------------------|--------------------------------------------------|
| `sudo crm configure edit`         | Édite la configuration du cluster.               |
| `sudo crm configure load update <fichier>` | Charge une configuration depuis un fichier. |

## 4. Vérification des logs
| Commande                          | Description                                      |
|-----------------------------------|--------------------------------------------------|
| `sudo journalctl -u pacemaker -f`| Suivi en temps réel des logs Pacemaker.          |
| `sudo tail -f /var/log/pacemaker.log` | Affiche les logs Pacemaker.                |
| `sudo systemctl status haproxy`   | Vérifie le statut du service HAProxy localement. |

## 5. Commandes HAProxy locales
| Commande                                      | Description                                      |
|-----------------------------------------------|--------------------------------------------------|
| `sudo systemctl restart haproxy`              | Redémarre HAProxy sur le nœud local.             |
| `haproxy -c -f /etc/haproxy/haproxy.cfg`      | Vérifie la syntaxe du fichier de configuration.  |
| `echo "show stat" \| socat /var/run/haproxy.stat stdio` | Affiche les statistiques HAProxy. |

---

### Notes :
- Remplacez `haproxy` par le nom exact de votre ressource si différent.
- Pour migrer une ressource, remplacez `<node>` par le nom du nœud cible.
- Après toute modification, validez avec `sudo crm configure commit`.
