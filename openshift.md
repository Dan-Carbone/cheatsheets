---

## **OpenShift / `oc` Cheatsheet**

### **1. Connexion et Configuration**
| Commande | Description |
|----------|-------------|
| `oc login <url> -u <user> -p <password>` | Se connecter à un cluster |
| `oc logout` | Se déconnecter |
| `oc whoami` | Afficher l’utilisateur courant |
| `oc project <nom>` | Changer de projet |
| `oc projects` | Lister tous les projets |
| `oc config view` | Voir la configuration actuelle |

---

### **2. Gestion des Projets**
| Commande | Description |
|----------|-------------|
| `oc new-project <nom>` | Créer un nouveau projet |
| `oc delete project <nom>` | Supprimer un projet |
| `oc get projects` | Lister les projets |

---

### **3. Déploiement d’Applications**
| Commande | Description |
|----------|-------------|
| `oc new-app <image>` | Déployer une image Docker |
| `oc new-app <repo-git>` | Déployer à partir d’un dépôt Git (S2I) |
| `oc status` | Voir l’état des ressources dans le projet |
| `oc get pods` | Lister les pods |
| `oc get svc` | Lister les services |
| `oc get routes` | Lister les routes |
| `oc expose svc/<nom>` | Exposer un service via une route |
| `oc rollout status dc/<nom>` | Voir l’état d’un déploiement |
| `oc rollout latest dc/<nom>` | Redéployer la dernière version |
| `oc rollout undo dc/<nom>` | Annuler un déploiement |

---

### **4. Gestion des Pods et Conteneurs**
| Commande | Description |
|----------|-------------|
| `oc get pods` | Lister les pods |
| `oc describe pod <nom>` | Détails d’un pod |
| `oc logs <pod>` | Voir les logs d’un pod |
| `oc logs -f <pod>` | Suivre les logs en temps réel |
| `oc exec -it <pod> -- /bin/bash` | Accéder à un shell dans un pod |
| `oc port-forward <pod> <local-port>:<pod-port>` | Forwarder un port local vers un pod |
| `oc delete pod <nom>` | Supprimer un pod |
| `oc scale dc/<nom> --replicas=<n>` | Mettre à l’échelle un déploiement |

---

### **5. Gestion des Ressources**
| Commande | Description |
|----------|-------------|
| `oc get all` | Lister toutes les ressources |
| `oc get rc,svc,routes,pods` | Lister plusieurs types de ressources |
| `oc create -f <fichier.yaml>` | Créer une ressource à partir d’un fichier YAML |
| `oc apply -f <fichier.yaml>` | Appliquer une configuration |
| `oc delete -f <fichier.yaml>` | Supprimer une ressource |
| `oc edit <ressource>/<nom>` | Modifier une ressource (ouvre l’éditeur par défaut) |

---

### **6. Stockage**
| Commande | Description |
|----------|-------------|
| `oc get pvc` | Lister les PersistentVolumeClaims |
| `oc get pv` | Lister les PersistentVolumes |
| `oc set volume dc/<nom> --add --name=<nom-volume> --type=pvc --claim-name=<pvc>` | Ajouter un volume à un déploiement |

---

### **7. Réseau**
| Commande | Description |
|----------|-------------|
| `oc get svc` | Lister les services |
| `oc expose svc/<nom> --name=<route>` | Créer une route pour un service |
| `oc get routes` | Lister les routes |
| `oc describe route <nom>` | Détails d’une route |

---

### **8. Builds et Images**
| Commande | Description |
|----------|-------------|
| `oc start-build <build-config>` | Lancer un build |
| `oc logs bc/<nom>` | Voir les logs d’un build |
| `oc import-image <nom> --from=<registry>/<image> --confirm` | Importer une image dans le registry interne |

---

### **9. Sécurité et RBAC**
| Commande | Description |
|----------|-------------|
| `oc policy add-role-to-user <role> <user> -n <projet>` | Ajouter un rôle à un utilisateur |
| `oc create secret generic <nom> --from-literal=key=value` | Créer un secret |
| `oc secrets link <service-account> <secret>` | Lier un secret à un ServiceAccount |

---

### **10. Monitoring et Dépannage**
| Commande | Description |
|----------|-------------|
| `oc get events` | Voir les événements du projet |
| `oc debug dc/<nom>` | Démarrer un pod de debug pour un déploiement |
| `oc rsync <local-path> <pod>:<remote-path>` | Copier des fichiers vers/un pod |
| `oc adm top pods` | Voir l’utilisation CPU/mémoire des pods |

---

### **11. Auto-scaling**
| Commande | Description |
|----------|-------------|
| `oc autoscale dc/<nom> --min=<n> --max=<m> --cpu-percent=<p>` | Configurer l’auto-scaling |

---

### **12. CI/CD (Pipelines)**
| Commande | Description |
|----------|-------------|
| `oc start-pipeline <nom>` | Lancer un pipeline Tekton/Openshift Pipelines |

---

## **Où trouver des cheatsheets complets ?**
- **[Cheatsheet officiel Red Hat (PDF)](https://www.openshift.com/hubfs/cheat-sheets/openshift-cli-cheatsheet.pdf)**
- **[Kubernetes Cheatsheet](https://kubernetes.io/fr/docs/reference/kubectl/cheatsheet/)** (beaucoup de commandes sont similaires à `oc`)
- **[OpenShift CLI Reference](https://docs.openshift.com/container-platform/4.12/cli_reference/openshift_cli/developer-cli-commands.html)**

---

### **Astuce**
- La plupart des commandes `oc` sont compatibles avec `kubectl`. Vous pouvez souvent remplacer `oc` par `kubectl` (et vice versa) pour les commandes de base.
- Pour une aide contextuelle :
  ```bash
  oc <commande> --help
  ```

---