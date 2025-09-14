# Configuration d'un Item et Trigger pour la Charge CPU dans Zabbix

## 1. Créer l'Item pour la Charge CPU

1. **Accéder à la section des éléments** :
   Allez dans **Configuration** → **Hôtes** → Sélectionnez votre hôte → **Éléments** → **Créer un élément**.

2. **Paramètres de l'item** :
   - **Nom** : `Charge CPU moyenne (1 minute)`
   - **Type** : `Agent Zabbix`
   - **Clé** : `system.cpu.load[percpu,avg1]`
   - **Type d’information** : `Numérique (float)`
   - **Unités** : `%`
   - **Intervalle de mise à jour** : `60s`

3. **Enregistrer** l'item.

---

## 2. Créer le Trigger pour une Charge CPU > 90% pendant 10 Minutes

1. **Accéder à la section des triggers** :
   Allez dans **Configuration** → **Hôtes** → Sélectionnez votre hôte → **Triggers** → **Créer un trigger**.

2. **Paramètres du trigger** :
   - **Nom** : `Charge CPU > 90% pendant 10 minutes`
   - **Expression** :
     ```plaintext
     {Nom_de_votre_hôte\:system.cpu.load[percpu,avg1].avg(10m)} > 90
     ```
   - **Sévérité** : `Haute`
   - **Description** (optionnelle) :
     ```plaintext
     La charge CPU moyenne dépasse 90% depuis 10 minutes.
     Valeur actuelle : {ITEM.LASTVALUE1}
     ```

3. **Enregistrer** le trigger.

---

## 3. Utilisation d'une Macro pour le Seuil (Optionnel)

1. **Créer une macro** pour l’hôte ou le template :
   - **Nom** : `{$CPU_LOAD_MAX}`
   - **Valeur** : `90`

2. **Mettre à jour l'expression du trigger** :
   ```plaintext
   {Nom_de_votre_hôte\:system.cpu.load[percpu,avg1].avg(10m)} > {\$CPU_LOAD_MAX}
