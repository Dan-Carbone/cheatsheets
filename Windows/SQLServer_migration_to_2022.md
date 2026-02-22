# Migration SQL Server 2016 → 2022 (Zero Downtime)

## 🧭 Contexte
- Hyperviseur : VMware
- Source : SQL Server 2016
- Cible : SQL Server 2022
- Objectif : Séparation DATA / LOG / SYSTEM / TEMPDB
- Downtime : < 5 minutes (bascule finale)

---

## 1️⃣ Préparation Infrastructure

- [ ] Snapshot VMware (source)
- [ ] Provision VM cible (Windows à jour)
- [ ] Ajouter disques :
  - [ ] D:\SQLData
  - [ ] L:\SQLLogs
  - [ ] S:\SQLSystem
  - [ ] T:\SQLTempDB
- [ ] Format NTFS 64K
- [ ] Activer Instant File Initialization
- [ ] Installer SQL Server 2022
- [ ] Installer même CU que recommandé
- [ ] Configurer Max Memory
- [ ] Configurer MaxDOP

---

## 2️⃣ Préparation Sécurité

- [ ] Sauvegarde clés TDE
- [ ] Script logins (sp_help_revlogin)
- [ ] Script SQL Agent Jobs
- [ ] Script Linked Servers
- [ ] Script Credentials / Proxies
- [ ] Script SSIS (si présent)

---

## 3️⃣ Mise en place Synchronisation (Log Shipping)

- [ ] Full backup
- [ ] Restore WITH NORECOVERY
- [ ] Configurer Log Shipping
- [ ] Vérifier synchronisation
- [ ] Tester restauration logs

---

## 4️⃣ Tests Pré-Production

- [ ] Test applicatif en lecture seule
- [ ] Vérifier temps de réponse
- [ ] Vérifier fragmentation
- [ ] Vérifier compatibilité (niveau 130 → 160)

---

## 5️⃣ Bascule Finale

- [ ] Stop application
- [ ] Backup log final
- [ ] Restore log final
- [ ] Recovery base
- [ ] Changer DNS / string connexion
- [ ] Validation applicative
- [ ] Monitoring 24h

---

## 6️⃣ Post-Migration

- [ ] Activer Query Store
- [ ] Passer compatibilité 160
- [ ] Rebuild index si nécessaire
- [ ] Supprimer ancien log shipping
- [ ] Supprimer snapshot VMware

---

## ✅ Migration terminée
