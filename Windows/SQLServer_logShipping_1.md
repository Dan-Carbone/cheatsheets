# 🚀 Migration SQL Server 2016 → 2022
## Méthode : Log Shipping Transitionnel
## Downtime cible : 1–3 minutes
## Compatible : Standard & Enterprise

---

# 🎯 Architecture

Production (2016)  →  Log Shipping  →  Nouvelle VM (2022)

---

# 1️⃣ Préparation VM cible

- [ ] VM Windows patchée
- [ ] SQL Server 2022 installé
- [ ] Même collation
- [ ] Même mode d’authentification
- [ ] Disques séparés :

| Disque | Usage |
|--------|-------|
| D:\SQLData | Data |
| L:\SQLLogs | Logs |
| S:\SQLSystem | System |
| T:\SQLTempDB | TempDB |

Format NTFS 64K.

---

# 2️⃣ Migration Sécurité

## Logins

```sql
EXEC sp_help_revlogin;
```

Exécuter sur 2022.

---

# 3️⃣ Initialisation Base

## Full Backup (2016)

```sql
BACKUP DATABASE MaBase
TO DISK = '\\Share\MaBase_full.bak'
WITH INIT, COMPRESSION;
```

## Restore (2022)

```sql
RESTORE DATABASE MaBase
FROM DISK = '\\Share\MaBase_full.bak'
WITH 
MOVE 'MaBase' TO 'D:\SQLData\MaBase.mdf',
MOVE 'MaBase_log' TO 'L:\SQLLogs\MaBase_log.ldf',
NORECOVERY,
REPLACE;
```

---

# 4️⃣ Log Shipping Continu

## Job Backup Log (2016)

```sql
BACKUP LOG MaBase
TO DISK = '\\Share\Logs\MaBase.trn'
WITH INIT, COMPRESSION;
```

Fréquence : toutes les 2 minutes.

## Job Restore (2022)

```sql
RESTORE LOG MaBase
FROM DISK = '\\Share\Logs\MaBase.trn'
WITH NORECOVERY;
```

---

# 5️⃣ Bascule Finale

1. Stop application
2. Backup log final :

```sql
BACKUP LOG MaBase
TO DISK = '\\Share\MaBase_final.trn'
WITH INIT, COMPRESSION;
```

3. Restore final :

```sql
RESTORE LOG MaBase
FROM DISK = '\\Share\MaBase_final.trn'
WITH RECOVERY;
```

4. Modifier DNS / connection string

---

# 🔁 Rollback

Repointer application vers 2016.
Aucune donnée perdue.

---

# ✅ Résultat

- Downtime : 1–3 min
- Risque : faible
- Compatible Standard
