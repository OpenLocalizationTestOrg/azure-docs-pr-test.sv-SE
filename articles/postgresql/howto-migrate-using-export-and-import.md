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
# <a name="migrate-your-postgresql-database-using-export-and-import"></a><span data-ttu-id="e0fcb-103">Migrera din PostgreSQL-databas med hjälp av export och import</span><span class="sxs-lookup"><span data-stu-id="e0fcb-103">Migrate your PostgreSQL database using export and import</span></span>
<span data-ttu-id="e0fcb-104">Du kan använda [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) tooextract en PostgreSQL-databas i en skriptfil och [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) tooimport hello data till hello måldatabasen från filen.</span><span class="sxs-lookup"><span data-stu-id="e0fcb-104">You can use [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) tooextract a PostgreSQL database into a script file and [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) tooimport hello data into hello target database from that file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e0fcb-105">Krav</span><span class="sxs-lookup"><span data-stu-id="e0fcb-105">Prerequisites</span></span>
<span data-ttu-id="e0fcb-106">toostep via den här hur tooguide behöver du:</span><span class="sxs-lookup"><span data-stu-id="e0fcb-106">toostep through this how-tooguide, you need:</span></span>
- <span data-ttu-id="e0fcb-107">En [Azure-databas för PostgreSQL server](quickstart-create-server-database-portal.md) med brandväggen regler tooallow åtkomst och databasen under den.</span><span class="sxs-lookup"><span data-stu-id="e0fcb-107">An [Azure Database for PostgreSQL server](quickstart-create-server-database-portal.md) with firewall rules tooallow access and database under it.</span></span>
- <span data-ttu-id="e0fcb-108">[pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) kommandoradsverktyget installerad</span><span class="sxs-lookup"><span data-stu-id="e0fcb-108">[pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) command-line utility installed</span></span>
- <span data-ttu-id="e0fcb-109">[psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) kommandoradsverktyget installerad</span><span class="sxs-lookup"><span data-stu-id="e0fcb-109">[psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) command-line utility installed</span></span>

<span data-ttu-id="e0fcb-110">Följ dessa steg tooexport och importera PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="e0fcb-110">Follow these steps tooexport and import your PostgreSQL database.</span></span>

## <a name="create-a-script-file-using-pgdump-that-contains-hello-data-toobe-loaded"></a><span data-ttu-id="e0fcb-111">Skapa en skriptfil med pg_dump som innehåller hello data toobe lästes in</span><span class="sxs-lookup"><span data-stu-id="e0fcb-111">Create a script file using pg_dump that contains hello data toobe loaded</span></span>
<span data-ttu-id="e0fcb-112">tooexport din befintliga PostgreSQL-databas lokalt eller i en VM tooa sql-skriptfilen, kör du följande kommando i din befintliga miljö hello:</span><span class="sxs-lookup"><span data-stu-id="e0fcb-112">tooexport your existing PostgreSQL database on-premises or in a VM tooa sql script file, run hello following command in your existing environment:</span></span>
```bash
pg_dump –-host=<host> --username=<name> --dbname=<database name> --file=<database>.sql
```
<span data-ttu-id="e0fcb-113">Till exempel om du har en lokal server och en databas som heter **testdb** i den</span><span class="sxs-lookup"><span data-stu-id="e0fcb-113">For example, if you have a local server and a database called **testdb** in it</span></span>
```bash
pg_dump --host=localhost --username=masterlogin --dbname=testdb --file=testdb.sql
```

## <a name="import-hello-data-on-target-azure-database-for-postrgesql"></a><span data-ttu-id="e0fcb-114">Importera hello data på målet Azure-databas för PostrgeSQL</span><span class="sxs-lookup"><span data-stu-id="e0fcb-114">Import hello data on target Azure Database for PostrgeSQL</span></span>
<span data-ttu-id="e0fcb-115">Du kan använda kommandoraden för hello psql och hello -d,--dbname parametern tooimport hello data till Azure-databas för PostrgeSQL vi skapade och Läs in data från hello sql-filen.</span><span class="sxs-lookup"><span data-stu-id="e0fcb-115">You can use hello psql command line and hello -d, --dbname parameter tooimport hello data into Azure Database for PostrgeSQL we created and load data from hello sql file.</span></span>
```bash
psql --file=<database>.sql --host=<server name> --port=5432 --username=<user@servername> --dbname=<target database name>
```
<span data-ttu-id="e0fcb-116">Det här exemplet används psql verktyget och en skriptfil med namnet **testdb.sql** från föregående steg tooimport data till databasen hello **mypgsqldb** på målservern  **mypgserver 20170401.postgres.database.azure.com**.</span><span class="sxs-lookup"><span data-stu-id="e0fcb-116">This example uses psql utility and a script file named **testdb.sql** from previous step tooimport data into hello database **mypgsqldb** on target server **mypgserver-20170401.postgres.database.azure.com**.</span></span>
```bash
psql --file=testdb.sql --host=mypgserver-20170401.database.windows.net --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb
```

## <a name="next-steps"></a><span data-ttu-id="e0fcb-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e0fcb-117">Next steps</span></span>
- <span data-ttu-id="e0fcb-118">toomigrate en PostgreSQL-databas med hjälp av dump och återställning, se [migrera din PostgreSQL-databas med hjälp av dump och återställning](howto-migrate-using-dump-and-restore.md)</span><span class="sxs-lookup"><span data-stu-id="e0fcb-118">toomigrate a PostgreSQL database using dump and restore, see [Migrate your PostgreSQL database using dump and restore](howto-migrate-using-dump-and-restore.md)</span></span>
