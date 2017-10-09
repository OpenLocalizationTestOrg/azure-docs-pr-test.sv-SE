---
title: "aaaHow tooDump och återställa i Azure-databas för PostgreSQL | Microsoft Docs"
description: "Beskriver hur tooextract en PostgreSQL-databas till en dumpfil och Återställ hello PostgreSQL-databas från en fil skapas av pg_dump i Azure-databas för PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/14/2017
ms.openlocfilehash: 9ad28e9dec3927b0f80b5e6bab6423cc944f5156
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a>Migrera din PostgreSQL-databas med hjälp av dump och återställning
Du kan använda [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) tooextract en PostgreSQL-databas till en dumpfil och [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) toorestore hello PostgreSQL-databas från en fil skapas av pg_dump.

## <a name="prerequisites"></a>Krav
toostep via den här hur tooguide behöver du:
- En [Azure-databas för PostgreSQL server](quickstart-create-server-database-portal.md) med brandväggen regler tooallow åtkomst och databasen under den.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) och [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) kommandoradsverktyg som installerats

Följ dessa steg toodump och Återställ PostgreSQL-databasen:

## <a name="create-a-dump-file-using-pgdump-that-contains-hello-data-toobe-loaded"></a>Skapa en dumpfil med pg_dump som innehåller hello data toobe lästes in
tooback in en befintlig PostgreSQL-databas lokalt eller på en virtuell dator kör du följande kommando hello:
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> > <database>.dump
```
Till exempel om du har en lokal server och en databas som heter **testdb** i den
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb > testdb.dump
```

## <a name="restore-hello-data-into-hello-target-azure-database-for-postrgesql-using-pgrestore"></a>Återställa hello data till målet hello Azure-databas för PostrgeSQL med pg_restore
Du kan använda hello pg_restore kommandot och hello -d,--dbname parametern toorestore hello data till hello måldatabasen från hello dumpfilen när du har skapat hello måldatabasen.
```bash
pg_restore -v –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
I det här exemplet återställa hello data från hello dumpfilen **testdb.dump** till hello databasen **mypgsqldb** på målservern **mypgserver 20170401.postgres.database.azure.com**.
```bash
pg_restore -v --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a>Nästa steg
- toomigrate en PostgreSQL-databas med hjälp av export och import, se [migrera din PostgreSQL-databas med hjälp av export och import](howto-migrate-using-export-and-import.md)
