# 🚀 Migration SQL Server 2016 → 2022
## Méthode : AlwaysOn Availability Group
## Downtime cible : < 30 secondes
## Edition recommandée : Enterprise

---

# 🎯 Architecture

VM1 (2016)  →  Availability Group  →  VM2 (2022)

Listener DNS unique.

---

# 1️⃣ Préparation Windows

Sur les deux serveurs :

- [ ] Installer Failover Clustering Feature
- [ ] Même domaine Active Directory
- [ ] Même collation SQL
- [ ] Ports ouverts (5022)

---

# 2️⃣ Activer AlwaysOn

Dans SQL Configuration Manager :
- Activer AlwaysOn Availability Groups
- Redémarrer service

---

# 3️⃣ Créer Endpoint HADR

```sql
CREATE ENDPOINT [Hadr_endpoint]
STATE=STARTED
AS TCP (LISTENER_PORT = 5022)
FOR DATA_MIRRORING (
ROLE=ALL
);
```

---

# 4️⃣ Préparer Base

Sur 2016 :

```sql
ALTER DATABASE MaBase SET RECOVERY FULL;
BACKUP DATABASE MaBase TO DISK='\\Share\full.bak';
BACKUP LOG MaBase TO DISK='\\Share\log.trn';
```

---

# 5️⃣ Restore sur 2022

```sql
RESTORE DATABASE MaBase
FROM DISK='\\Share\full.bak'
WITH NORECOVERY,
MOVE 'MaBase' TO 'D:\SQLData\MaBase.mdf',
MOVE 'MaBase_log' TO 'L:\SQLLogs\MaBase_log.ldf';

RESTORE LOG MaBase
FROM DISK='\\Share\log.trn'
WITH NORECOVERY;
```

---

# 6️⃣ Créer Availability Group

```sql
CREATE AVAILABILITY GROUP AG_Migration
FOR DATABASE MaBase
REPLICA ON
'SQL2016' WITH (
ENDPOINT_URL = 'TCP://SQL2016:5022',
FAILOVER_MODE = MANUAL,
AVAILABILITY_MODE = SYNCHRONOUS_COMMIT
),
'SQL2022' WITH (
ENDPOINT_URL = 'TCP://SQL2022:5022',
FAILOVER_MODE = MANUAL,
AVAILABILITY_MODE = SYNCHRONOUS_COMMIT
);
```

---

# 7️⃣ Ajouter Listener

```sql
ALTER AVAILABILITY GROUP AG_Migration
ADD LISTENER 'SQLListener'
(
WITH IP ((N'10.0.0.50', N'255.255.255.0')),
PORT=1433
);
```

---

# 🔥 8️⃣ Bascule Zero Downtime

```sql
ALTER AVAILABILITY GROUP AG_Migration
FAILOVER;
```

Downtime réel : quelques secondes.

---

# 🔁 Rollback

```sql
ALTER AVAILABILITY GROUP AG_Migration
FAILOVER;
```

Retour immédiat.

---

# 9️⃣ Finalisation

- Retirer 2016 du cluster
- Monter compatibilité :

```sql
ALTER DATABASE MaBase SET COMPATIBILITY_LEVEL = 160;
```

---

# ✅ Résultat

- Downtime : < 30 secondes
- Rollback instantané
- Haute disponibilité intégrée
- Idéal production critique
