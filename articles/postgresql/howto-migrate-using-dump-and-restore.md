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
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a><span data-ttu-id="f22b2-103">Migrera din PostgreSQL-databas med hjälp av dump och återställning</span><span class="sxs-lookup"><span data-stu-id="f22b2-103">Migrate your PostgreSQL database using dump and restore</span></span>
<span data-ttu-id="f22b2-104">Du kan använda [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) tooextract en PostgreSQL-databas till en dumpfil och [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) toorestore hello PostgreSQL-databas från en fil skapas av pg_dump.</span><span class="sxs-lookup"><span data-stu-id="f22b2-104">You can use [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) tooextract a PostgreSQL database into a dump file and [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) toorestore hello PostgreSQL database from an archive file created by pg_dump.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f22b2-105">Krav</span><span class="sxs-lookup"><span data-stu-id="f22b2-105">Prerequisites</span></span>
<span data-ttu-id="f22b2-106">toostep via den här hur tooguide behöver du:</span><span class="sxs-lookup"><span data-stu-id="f22b2-106">toostep through this how-tooguide, you need:</span></span>
- <span data-ttu-id="f22b2-107">En [Azure-databas för PostgreSQL server](quickstart-create-server-database-portal.md) med brandväggen regler tooallow åtkomst och databasen under den.</span><span class="sxs-lookup"><span data-stu-id="f22b2-107">An [Azure Database for PostgreSQL server](quickstart-create-server-database-portal.md) with firewall rules tooallow access and database under it.</span></span>
- <span data-ttu-id="f22b2-108">[pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) och [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) kommandoradsverktyg som installerats</span><span class="sxs-lookup"><span data-stu-id="f22b2-108">[pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) and [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) command-line utilities installed</span></span>

<span data-ttu-id="f22b2-109">Följ dessa steg toodump och Återställ PostgreSQL-databasen:</span><span class="sxs-lookup"><span data-stu-id="f22b2-109">Follow these steps toodump and restore your PostgreSQL database:</span></span>

## <a name="create-a-dump-file-using-pgdump-that-contains-hello-data-toobe-loaded"></a><span data-ttu-id="f22b2-110">Skapa en dumpfil med pg_dump som innehåller hello data toobe lästes in</span><span class="sxs-lookup"><span data-stu-id="f22b2-110">Create a dump file using pg_dump that contains hello data toobe loaded</span></span>
<span data-ttu-id="f22b2-111">tooback in en befintlig PostgreSQL-databas lokalt eller på en virtuell dator kör du följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="f22b2-111">tooback up an existing PostgreSQL database on-premises or in a VM, run hello following command:</span></span>
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> > <database>.dump
```
<span data-ttu-id="f22b2-112">Till exempel om du har en lokal server och en databas som heter **testdb** i den</span><span class="sxs-lookup"><span data-stu-id="f22b2-112">For example, if you have a local server and a database called **testdb** in it</span></span>
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb > testdb.dump
```

## <a name="restore-hello-data-into-hello-target-azure-database-for-postrgesql-using-pgrestore"></a><span data-ttu-id="f22b2-113">Återställa hello data till målet hello Azure-databas för PostrgeSQL med pg_restore</span><span class="sxs-lookup"><span data-stu-id="f22b2-113">Restore hello data into hello target Azure Database for PostrgeSQL using pg_restore</span></span>
<span data-ttu-id="f22b2-114">Du kan använda hello pg_restore kommandot och hello -d,--dbname parametern toorestore hello data till hello måldatabasen från hello dumpfilen när du har skapat hello måldatabasen.</span><span class="sxs-lookup"><span data-stu-id="f22b2-114">Once you have created hello target database, you can use hello pg_restore command and hello -d, --dbname parameter toorestore hello data into hello target database from hello dump file.</span></span>
```bash
pg_restore -v –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
<span data-ttu-id="f22b2-115">I det här exemplet återställa hello data från hello dumpfilen **testdb.dump** till hello databasen **mypgsqldb** på målservern **mypgserver 20170401.postgres.database.azure.com**.</span><span class="sxs-lookup"><span data-stu-id="f22b2-115">In this example, restore hello data from hello dump file **testdb.dump** into hello database **mypgsqldb** on target server **mypgserver-20170401.postgres.database.azure.com**.</span></span>
```bash
pg_restore -v --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a><span data-ttu-id="f22b2-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f22b2-116">Next steps</span></span>
- <span data-ttu-id="f22b2-117">toomigrate en PostgreSQL-databas med hjälp av export och import, se [migrera din PostgreSQL-databas med hjälp av export och import](howto-migrate-using-export-and-import.md)</span><span class="sxs-lookup"><span data-stu-id="f22b2-117">toomigrate a PostgreSQL database using export and import, see [Migrate your PostgreSQL database using export and import](howto-migrate-using-export-and-import.md)</span></span>
