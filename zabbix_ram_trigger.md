# Surveillance de la RAM et du Stockage dans Zabbix

---

## 1. Surveillance de la RAM Disponible

### 1.1. Créer l'Item pour la RAM Disponible

1. **Accéder à la section des éléments** :
   Allez dans **Configuration** → **Hôtes** → Sélectionnez votre hôte → **Éléments** → **Créer un élément**.

2. **Paramètres de l'item** :
   - **Nom** : `Mémoire disponible (en %)`
   - **Type** : `Agent Zabbix`
   - **Clé** : `vm.memory.size[pavailable]`
   - **Type d’information** : `Numérique (float)`
   - **Unités** : `%`
   - **Intervalle de mise à jour** : `60s`

3. **Enregistrer** l'item.

---

### 1.2. Créer le Trigger pour une RAM Disponible < 10% pendant 10 Minutes

1. **Accéder à la section des triggers** :
   Allez dans **Configuration** → **Hôtes** → Sélectionnez votre hôte → **Triggers** → **Créer un trigger**.

2. **Paramètres du trigger** :
   - **Nom** : `RAM disponible < 10% pendant 10 minutes`
   - **Expression** :
     ```plaintext
     {Nom_de_votre_hôte\:vm.memory.size[pavailable].avg(10m)} < 10
     ```
   - **Sévérité** : `Haute`
   - **Description** (optionnelle) :
     ```plaintext
     La mémoire disponible est inférieure à 10% depuis 10 minutes.
     Valeur actuelle : {ITEM.LASTVALUE1} %
     ```

3. **Enregistrer** le trigger.

---

### 1.3. Utilisation d'une Macro pour le Seuil (Optionnel)

1. **Créer une macro** pour l’hôte ou le template :
   - **Nom** : `{$RAM_MIN_AVAILABLE}`
   - **Valeur** : `10`

2. **Mettre à jour l'expression du trigger** :
   ```plaintext
   {Nom_de_votre_hôte\:vm.memory.size[pavailable].avg(10m)} < {\$RAM_MIN_AVAILABLE}
