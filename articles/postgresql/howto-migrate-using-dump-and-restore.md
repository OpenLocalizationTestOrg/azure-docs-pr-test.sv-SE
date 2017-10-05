---
title: "Dump och återställa i Azure-databas för PostgreSQL | Microsoft Docs"
description: "Beskriver hur du extrahera en PostgreSQL-databas till en dumpfil och återställa PostgreSQL-databas från en fil skapas av pg_dump i Azure-databas för PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/14/2017
ms.openlocfilehash: 190373c4980b67e16b58700e4b7e65658545c615
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a><span data-ttu-id="f93cd-103">Migrera din PostgreSQL-databas med hjälp av dump och återställning</span><span class="sxs-lookup"><span data-stu-id="f93cd-103">Migrate your PostgreSQL database using dump and restore</span></span>
<span data-ttu-id="f93cd-104">Du kan använda [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) att extrahera en PostgreSQL-databas till en dumpfil och [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) att återställa PostgreSQL-databas från en fil skapas av pg_dump.</span><span class="sxs-lookup"><span data-stu-id="f93cd-104">You can use [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) to extract a PostgreSQL database into a dump file and [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) to restore the PostgreSQL database from an archive file created by pg_dump.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f93cd-105">Krav</span><span class="sxs-lookup"><span data-stu-id="f93cd-105">Prerequisites</span></span>
<span data-ttu-id="f93cd-106">Du behöver följande för att gå igenom den här instruktioner:</span><span class="sxs-lookup"><span data-stu-id="f93cd-106">To step through this how-to guide, you need:</span></span>
- <span data-ttu-id="f93cd-107">En [Azure-databas för PostgreSQL server](quickstart-create-server-database-portal.md) med brandväggsregler som tillåter åtkomst och databasen under den.</span><span class="sxs-lookup"><span data-stu-id="f93cd-107">An [Azure Database for PostgreSQL server](quickstart-create-server-database-portal.md) with firewall rules to allow access and database under it.</span></span>
- <span data-ttu-id="f93cd-108">[pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) och [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) kommandoradsverktyg som installerats</span><span class="sxs-lookup"><span data-stu-id="f93cd-108">[pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) and [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) command-line utilities installed</span></span>

<span data-ttu-id="f93cd-109">Följ dessa steg för att dumpa och återställa PostgreSQL-databas:</span><span class="sxs-lookup"><span data-stu-id="f93cd-109">Follow these steps to dump and restore your PostgreSQL database:</span></span>

## <a name="create-a-dump-file-using-pgdump-that-contains-the-data-to-be-loaded"></a><span data-ttu-id="f93cd-110">Skapa en dumpfil med pg_dump som innehåller data som ska hämtas</span><span class="sxs-lookup"><span data-stu-id="f93cd-110">Create a dump file using pg_dump that contains the data to be loaded</span></span>
<span data-ttu-id="f93cd-111">Om du vill säkerhetskopiera en befintlig PostgreSQL-databas lokalt eller i en virtuell dator, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f93cd-111">To back up an existing PostgreSQL database on-premises or in a VM, run the following command:</span></span>
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> > <database>.dump
```
<span data-ttu-id="f93cd-112">Till exempel om du har en lokal server och en databas som heter **testdb** i den</span><span class="sxs-lookup"><span data-stu-id="f93cd-112">For example, if you have a local server and a database called **testdb** in it</span></span>
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb > testdb.dump
```

## <a name="restore-the-data-into-the-target-azure-database-for-postrgesql-using-pgrestore"></a><span data-ttu-id="f93cd-113">Återställa data till mål-Azure-databas för PostrgeSQL med pg_restore</span><span class="sxs-lookup"><span data-stu-id="f93cd-113">Restore the data into the target Azure Database for PostrgeSQL using pg_restore</span></span>
<span data-ttu-id="f93cd-114">När du har skapat måldatabasen, kan du använda kommandot pg_restore och -d,--dbname-parametern för att återställa data i måldatabasen från dumpfilen.</span><span class="sxs-lookup"><span data-stu-id="f93cd-114">Once you have created the target database, you can use the pg_restore command and the -d, --dbname parameter to restore the data into the target database from the dump file.</span></span>
```bash
pg_restore -v –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
<span data-ttu-id="f93cd-115">I det här exemplet kan du återställa data från dumpfilen **testdb.dump** i databasen **mypgsqldb** på målservern **mypgserver 20170401.postgres.database.azure.com**.</span><span class="sxs-lookup"><span data-stu-id="f93cd-115">In this example, restore the data from the dump file **testdb.dump** into the database **mypgsqldb** on target server **mypgserver-20170401.postgres.database.azure.com**.</span></span>
```bash
pg_restore -v --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a><span data-ttu-id="f93cd-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f93cd-116">Next steps</span></span>
- <span data-ttu-id="f93cd-117">Om du vill migrera en PostgreSQL-databas med hjälp av export och import finns [migrera din PostgreSQL-databas med hjälp av export och import](howto-migrate-using-export-and-import.md)</span><span class="sxs-lookup"><span data-stu-id="f93cd-117">To migrate a PostgreSQL database using export and import, see [Migrate your PostgreSQL database using export and import](howto-migrate-using-export-and-import.md)</span></span>