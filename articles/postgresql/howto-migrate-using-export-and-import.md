---
title: "aaaMigrate en databas med hjälp av importera och exportera i Azure-databas för PostgreSQL | Microsoft Docs"
description: "Beskriver hur extrahera en PostgreSQL-databas till en skriptfil och importera hello data till hello måldatabasen från filen."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/14/2017
ms.openlocfilehash: 5ca4650782b02e1067c5a8a3710f2dfbc8c76063
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-postgresql-database-using-export-and-import"></a>Migrera din PostgreSQL-databas med hjälp av export och import
Du kan använda [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) tooextract en PostgreSQL-databas i en skriptfil och [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) tooimport hello data till hello måldatabasen från filen.

## <a name="prerequisites"></a>Krav
toostep via den här hur tooguide behöver du:
- En [Azure-databas för PostgreSQL server](quickstart-create-server-database-portal.md) med brandväggen regler tooallow åtkomst och databasen under den.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) kommandoradsverktyget installerad
- [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) kommandoradsverktyget installerad

Följ dessa steg tooexport och importera PostgreSQL-databas.

## <a name="create-a-script-file-using-pgdump-that-contains-hello-data-toobe-loaded"></a>Skapa en skriptfil med pg_dump som innehåller hello data toobe lästes in
tooexport din befintliga PostgreSQL-databas lokalt eller i en VM tooa sql-skriptfilen, kör du följande kommando i din befintliga miljö hello:
```bash
pg_dump –-host=<host> --username=<name> --dbname=<database name> --file=<database>.sql
```
Till exempel om du har en lokal server och en databas som heter **testdb** i den
```bash
pg_dump --host=localhost --username=masterlogin --dbname=testdb --file=testdb.sql
```

## <a name="import-hello-data-on-target-azure-database-for-postrgesql"></a>Importera hello data på målet Azure-databas för PostrgeSQL
Du kan använda kommandoraden för hello psql och hello -d,--dbname parametern tooimport hello data till Azure-databas för PostrgeSQL vi skapade och Läs in data från hello sql-filen.
```bash
psql --file=<database>.sql --host=<server name> --port=5432 --username=<user@servername> --dbname=<target database name>
```
Det här exemplet används psql verktyget och en skriptfil med namnet **testdb.sql** från föregående steg tooimport data till databasen hello **mypgsqldb** på målservern  **mypgserver 20170401.postgres.database.azure.com**.
```bash
psql --file=testdb.sql --host=mypgserver-20170401.database.windows.net --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb
```

## <a name="next-steps"></a>Nästa steg
- toomigrate en PostgreSQL-databas med hjälp av dump och återställning, se [migrera din PostgreSQL-databas med hjälp av dump och återställning](howto-migrate-using-dump-and-restore.md)
