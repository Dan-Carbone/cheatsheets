# Surveillance du stockage dans Zabbix

## 1. Création d'un item et déclencheur pour le stockage

### **Item pour surveiller l'espace disque**
1. **Accéder à** :
   `Configuration` → `Hôtes` → Sélectionner l'hôte cible → `Items` → `Créer un item`.

2. **Paramètres de l'item** :
   | Champ               | Valeur                                                                 |
   |---------------------|-----------------------------------------------------------------------|
   | **Nom**             | `Espace disque utilisé sur {HOST.NAME} (/) `                         |
   | **Clé**             | `vfs.fs.size[/,pused]` (pourcentage utilisé) ou `vfs.fs.size[/,free]` (espace libre) |
   | **Type**            | `Agent Zabbix`                                                         |
   | **Type d'information** | `Numérique (float)`                                                   |
   | **Unités**          | `%` (si `pused`) ou `B` (si `free`)                                    |
   | **Intervalle**      | `60s` (ajustable)                                                     |
   | **Application**     | `Disque` (ou créer une nouvelle application comme `Stockage`)         |

3. **Tester l'item** :
   Utiliser le bouton `Tester` pour vérifier que la métrique est récupérée (ex: retour `42.5` pour 42.5% utilisé).

---

### **Déclencheur pour alerte à >90%**
1. **Accéder à** :
   `Configuration` → `Hôtes` → Sélectionner l'hôte → `Déclencheurs` → `Créer un déclencheur`.

2. **Paramètres du déclencheur** :
   | Champ               | Valeur                                                                 |
   |---------------------|-----------------------------------------------------------------------|
   | **Nom**             | `Espace disque critique sur {HOST.NAME} (/) > 90%`                  |
   | **Expression**      | `{nom_de_l_hote:vfs.fs.size[/,pused].last()} > 90`                   |
   | **Sévérité**        | `Haute` ou `Désastre`                                                 |
   | **Description**     | `L'espace disque racine (/) dépasse 90% d'utilisation.`              |
   | **Solution**        | `Nettoyer les fichiers temporaires ou étendre la partition.`         |

3. **Macro pour seuil dynamique** (optionnel) :
   - **Créer une macro** :
     `Administration` → `Général` → `Macros` → Ajouter une macro globale (ex: `{$DISK_CRIT}` avec valeur `90`).
   - **Modifier l'expression** :
     `{nom_de_l_hote:vfs.fs.size[/,pused].last()} > {$DISK_CRIT}`

---

## 2. Création d'un template réutilisable

### **Étapes pour le template**
1. **Créer le template** :
   `Configuration` → `Templates` → `Créer un template`.
   - **Nom** : `Template_Stockage_Linux` (exemple).
   - **Groupe** : `Templates/OS/Linux` (ou créer un nouveau groupe).

2. **Ajouter les items** :
   - Dupliquer l'item créé précédemment (adapter le chemin `/` si besoin pour `/home`, `/var`, etc.).
   - Exemple d'items supplémentaires :
     | Chemin  | Clé Zabbix               | Description                     |
     |---------|--------------------------|---------------------------------|
     | `/`     | `vfs.fs.size[/,pused]`   | Pourcentage utilisé (racine)    |
     | `/home` | `vfs.fs.size[/home,pused]` | Pourcentage utilisé (/home)     |

3. **Ajouter les déclencheurs** :
   - Réutiliser le déclencheur précédent en remplaçant `{nom_de_l_hote}` par `{HOST.NAME}`.
   - **Astuce** : Utiliser des macros dans le template pour personnaliser les seuils par hôte :
     ```plaintext
     {\$DISK_ROOT_CRIT} = 90  (valeur par défaut)
     {\$DISK_HOME_CRIT} = 85
     ```

4. **Lier le template à un hôte** :
   - Dans la configuration de l'hôte, ajouter le template via l'onglet `Templates`.

---

### **Bonnes pratiques**
- **Découverte automatique** :
  Utiliser la règle de découverte `vfs.fs.discovery` pour détecter dynamiquement les points de montage.
- **Actions associées** :
  Configurer une action dans `Configuration` → `Actions` pour envoyer des notifications (email, Slack) lors des alertes.
- **Tableau de bord** :
  Créer un graphique dans le template avec les items `vfs.fs.size[*,pused]` pour visualiser l'évolution.

---
