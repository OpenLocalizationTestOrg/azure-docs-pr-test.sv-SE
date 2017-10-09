---
title: "Ansluta tooAzure databas för PostgreSQL från Node.js | Microsoft Docs"
description: "Denna Snabbstart innehåller en Node.js-kodexempel som du kan använda tooconnect och fråga efter data från Azure-databas för PostgreSQL."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 06/23/2017
ms.openlocfilehash: 9b269d72068ecc24bcf3fb447a2efeda512c698c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-use-nodejs-tooconnect-and-query-data"></a><span data-ttu-id="290b9-103">Azure-databas för PostgreSQL: Använd Node.js tooconnect och fråga data</span><span class="sxs-lookup"><span data-stu-id="290b9-103">Azure Database for PostgreSQL: Use Node.js tooconnect and query data</span></span>
<span data-ttu-id="290b9-104">Den här snabbstarten visar hur tooconnect tooan Azure-databas för att använda PostgreSQL [Node.js](https://nodejs.org/).</span><span class="sxs-lookup"><span data-stu-id="290b9-104">This quickstart demonstrates how tooconnect tooan Azure Database for PostgreSQL using [Node.js](https://nodejs.org/).</span></span> <span data-ttu-id="290b9-105">Den visar hur toouse SQL-instruktioner tooquery infoga, uppdatera och ta bort data i hello-databas.</span><span class="sxs-lookup"><span data-stu-id="290b9-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="290b9-106">hello förutsätter stegen i den här artikeln att du är bekant med att utveckla med Node.js och att du är ny tooworking med Azure-databas för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="290b9-106">hello steps in this article assume that you are familiar with developing using Node.js, and that you are new tooworking with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="290b9-107">Krav</span><span class="sxs-lookup"><span data-stu-id="290b9-107">Prerequisites</span></span>
<span data-ttu-id="290b9-108">Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="290b9-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="290b9-109">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="290b9-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="290b9-110">Skapa DB – CLI</span><span class="sxs-lookup"><span data-stu-id="290b9-110">Create DB - CLI</span></span>](quickstart-create-server-database-azure-cli.md)

<span data-ttu-id="290b9-111">Du måste också:</span><span class="sxs-lookup"><span data-stu-id="290b9-111">You also need to:</span></span>
- <span data-ttu-id="290b9-112">Installera [Node.js](https://nodejs.org)</span><span class="sxs-lookup"><span data-stu-id="290b9-112">Install [Node.js](https://nodejs.org)</span></span>

## <a name="install-pg-client"></a><span data-ttu-id="290b9-113">Installera pg-klienten</span><span class="sxs-lookup"><span data-stu-id="290b9-113">Install pg client</span></span>
<span data-ttu-id="290b9-114">Installera [pg](https://www.npmjs.com/package/pg), vilket är en PostgreSQL-klient för Node.js.</span><span class="sxs-lookup"><span data-stu-id="290b9-114">Install [pg](https://www.npmjs.com/package/pg), which is a PostgreSQL client for Node.js.</span></span>

<span data-ttu-id="290b9-115">toodo så kör hello nod Pakethanteraren (npm) för JavaScript från kommandoraden tooinstall hello pg klienten.</span><span class="sxs-lookup"><span data-stu-id="290b9-115">toodo so, run hello node package manager (npm) for JavaScript from your command line tooinstall hello pg client.</span></span>
```bash
npm install pg
```

<span data-ttu-id="290b9-116">Kontrollera hello installationen genom att ange hello-paket installeras.</span><span class="sxs-lookup"><span data-stu-id="290b9-116">Verify hello installation by listing hello packages installed.</span></span>
```bash
npm list
```

## <a name="get-connection-information"></a><span data-ttu-id="290b9-117">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="290b9-117">Get connection information</span></span>
<span data-ttu-id="290b9-118">Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="290b9-118">Get hello connection information needed tooconnect toohello Azure Database for PostgreSQL.</span></span> <span data-ttu-id="290b9-119">Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.</span><span class="sxs-lookup"><span data-stu-id="290b9-119">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="290b9-120">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="290b9-120">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="290b9-121">Hello vänstra menyn i Azure-portalen klickar du på **alla resurser** och Sök efter hello-server som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="290b9-121">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you just created.</span></span>
3. <span data-ttu-id="290b9-122">Klicka på hello servernamn.</span><span class="sxs-lookup"><span data-stu-id="290b9-122">Click hello server name.</span></span>
4. <span data-ttu-id="290b9-123">Välj hello server **översikt** sidan.</span><span class="sxs-lookup"><span data-stu-id="290b9-123">Select hello server's **Overview** page.</span></span> <span data-ttu-id="290b9-124">Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.</span><span class="sxs-lookup"><span data-stu-id="290b9-124">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="290b9-125">![Azure Database för PostgreSQL – inloggning för serveradministratör](./media/connect-nodejs/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="290b9-125">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-nodejs/1-connection-string.png)</span></span>
5. <span data-ttu-id="290b9-126">Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet och, om nödvändigt återställa hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="290b9-126">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="running-hello-javascript-code-in-nodejs"></a><span data-ttu-id="290b9-127">Kör hello JavaScript-kod i Node.js</span><span class="sxs-lookup"><span data-stu-id="290b9-127">Running hello JavaScript code in Node.js</span></span>
<span data-ttu-id="290b9-128">Du kan köra Node.js från hello bash-gränssnitt eller windows kommandotolk genom att skriva `node`, köras interaktivt hello exempel JavaScript-kod genom att kopiera och klistra in hello-Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="290b9-128">You may launch Node.js from hello bash shell or windows command prompt by typing `node`, then run hello example JavaScript code interactively by copy and pasting it onto hello prompt.</span></span> <span data-ttu-id="290b9-129">Du kan också spara hello JavaScript-kod i en textfil och starta `node filename.js` med hello filnamn som en parameter toorun den.</span><span class="sxs-lookup"><span data-stu-id="290b9-129">Alternatively, you may save hello JavaScript code into a text file and launch `node filename.js` with hello file name as a parameter toorun it.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="290b9-130">Ansluta, skapa tabell och infoga data</span><span class="sxs-lookup"><span data-stu-id="290b9-130">Connect, create table, and insert data</span></span>
<span data-ttu-id="290b9-131">Använd hello följande kod tooconnect och läsa in hello data med hjälp av **CREATE TABLE** och **INSERT INTO** SQL-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="290b9-131">Use hello following code tooconnect and load hello data using **CREATE TABLE** and  **INSERT INTO** SQL statements.</span></span>
<span data-ttu-id="290b9-132">Hej [pg. Klienten](https://github.com/brianc/node-postgres/wiki/Client) objektet är används toointerface med hello PostgreSQL-servern.</span><span class="sxs-lookup"><span data-stu-id="290b9-132">hello [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) object is used toointerface with hello PostgreSQL server.</span></span> <span data-ttu-id="290b9-133">Hej [pg. Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) funktionen är används tooestablish hello anslutning toohello server.</span><span class="sxs-lookup"><span data-stu-id="290b9-133">hello [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) function is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="290b9-134">Hej [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) funktionen är används tooexecute hello SQL-fråga mot PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="290b9-134">hello [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) function is used tooexecute hello SQL query against PostgreSQL database.</span></span> 

<span data-ttu-id="290b9-135">Ersätt hello värden, dbname, användare och lösenordsparametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="290b9-135">Replace hello host, dbname, user, and password parameters with hello values that you specified when you created hello server and database.</span></span>

```javascript
const pg = require('pg');

const config = {
    host: '<your-db-server-name>.postgres.database.azure.com',
    // Do not hard code your username and password.
    // Consider using Node environment variables.
    user: '<your-db-username>',     
    password: '<your-password>',
    database: '<name-of-database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) throw err;
    else {
        queryDatabase();
    }
});

function queryDatabase() {
    const query = `
        DROP TABLE IF EXISTS inventory;
        CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);
        INSERT INTO inventory (name, quantity) VALUES ('banana', 150);
        INSERT INTO inventory (name, quantity) VALUES ('orange', 154);
        INSERT INTO inventory (name, quantity) VALUES ('apple', 100);
    `;

    client
        .query(query)
        .then(() => {
            console.log('Table created successfully!');
            client.end(console.log('Closed client connection'));
        })
        .catch(err => console.log(err))
        .then(() => {
            console.log('Finished execution, exiting now');
            process.exit();
        });
}
```

## <a name="read-data"></a><span data-ttu-id="290b9-136">Läsa data</span><span class="sxs-lookup"><span data-stu-id="290b9-136">Read data</span></span>
<span data-ttu-id="290b9-137">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **Välj** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="290b9-137">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> <span data-ttu-id="290b9-138">Hej [pg. Klienten](https://github.com/brianc/node-postgres/wiki/Client) objektet är används toointerface med hello PostgreSQL-servern.</span><span class="sxs-lookup"><span data-stu-id="290b9-138">hello [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) object is used toointerface with hello PostgreSQL server.</span></span> <span data-ttu-id="290b9-139">Hej [pg. Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) funktionen är används tooestablish hello anslutning toohello server.</span><span class="sxs-lookup"><span data-stu-id="290b9-139">hello [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) function is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="290b9-140">Hej [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) funktionen är används tooexecute hello SQL-fråga mot PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="290b9-140">hello [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) function is used tooexecute hello SQL query against PostgreSQL database.</span></span> 

<span data-ttu-id="290b9-141">Ersätt hello värden, dbname, användare och lösenordsparametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="290b9-141">Replace hello host, dbname, user, and password parameters with hello values that you specified when you created hello server and database.</span></span> 

```javascript
const pg = require('pg');

const config = {
    host: '<your-db-server-name>.postgres.database.azure.com',
    // Do not hard code your username and password.
    // Consider using Node environment variables.
    user: '<your-db-username>',     
    password: '<your-password>',
    database: '<name-of-database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) throw err;
    else { queryDatabase(); }
});

function queryDatabase() {
  
    console.log(`Running query tooPostgreSQL server: ${config.host}`);

    const query = 'SELECT * FROM inventory;';

    client.query(query)
        .then(res => {
            const rows = res.rows;

            rows.map(row => {
                console.log(`Read: ${JSON.stringify(row)}`);
            });

            process.exit();
        })
        .catch(err => {
            console.log(err);
        });
}
```

## <a name="update-data"></a><span data-ttu-id="290b9-142">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="290b9-142">Update data</span></span>
<span data-ttu-id="290b9-143">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **uppdatering** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="290b9-143">Use hello following code tooconnect and read hello data using a **UPDATE** SQL statement.</span></span> <span data-ttu-id="290b9-144">Hej [pg. Klienten](https://github.com/brianc/node-postgres/wiki/Client) objektet är används toointerface med hello PostgreSQL-servern.</span><span class="sxs-lookup"><span data-stu-id="290b9-144">hello [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) object is used toointerface with hello PostgreSQL server.</span></span> <span data-ttu-id="290b9-145">Hej [pg. Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) funktionen är används tooestablish hello anslutning toohello server.</span><span class="sxs-lookup"><span data-stu-id="290b9-145">hello [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) function is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="290b9-146">Hej [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) funktionen är används tooexecute hello SQL-fråga mot PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="290b9-146">hello [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) function is used tooexecute hello SQL query against PostgreSQL database.</span></span> 

<span data-ttu-id="290b9-147">Ersätt hello värden, dbname, användare och lösenordsparametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="290b9-147">Replace hello host, dbname, user, and password parameters with hello values that you specified when you created hello server and database.</span></span> 

```javascript
const pg = require('pg');

const config = {
    host: '<your-db-server-name>.postgres.database.azure.com',
    // Do not hard code your username and password.
    // Consider using Node environment variables.
    user: '<your-db-username>',     
    password: '<your-password>',
    database: '<name-of-database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) throw err;
    else {
        queryDatabase();
    }
});

function queryDatabase() {
    const query = `
        UPDATE inventory 
        SET quantity= 1000 WHERE name='banana';
    `;

    client
        .query(query)
        .then(result => {
            console.log('Update completed');
            console.log(`Rows affected: ${result.rowCount}`);
        })
        .catch(err => {
            console.log(err);
            throw err;
        });
}
```

## <a name="delete-data"></a><span data-ttu-id="290b9-148">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="290b9-148">Delete data</span></span>
<span data-ttu-id="290b9-149">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **ta bort** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="290b9-149">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> <span data-ttu-id="290b9-150">Hej [pg. Klienten](https://github.com/brianc/node-postgres/wiki/Client) objektet är används toointerface med hello PostgreSQL-servern.</span><span class="sxs-lookup"><span data-stu-id="290b9-150">hello [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) object is used toointerface with hello PostgreSQL server.</span></span> <span data-ttu-id="290b9-151">Hej [pg. Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) funktionen är används tooestablish hello anslutning toohello server.</span><span class="sxs-lookup"><span data-stu-id="290b9-151">hello [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) function is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="290b9-152">Hej [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) funktionen är används tooexecute hello SQL-fråga mot PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="290b9-152">hello [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) function is used tooexecute hello SQL query against PostgreSQL database.</span></span> 

<span data-ttu-id="290b9-153">Ersätt hello värden, dbname, användare och lösenordsparametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="290b9-153">Replace hello host, dbname, user, and password parameters with hello values that you specified when you created hello server and database.</span></span> 

```javascript
const pg = require('pg');

const config = {
    host: '<your-db-server-name>.postgres.database.azure.com',
    // Do not hard code your username and password.
    // Consider using Node environment variables.
    user: '<your-db-username>',     
    password: '<your-password>',
    database: '<name-of-database>',
    port: 5432,
    ssl: true
};

const client = new pg.Client(config);

client.connect(err => {
    if (err) {
        throw err;
    } else {
        queryDatabase();
    }
});

function queryDatabase() {
    const query = `
        DELETE FROM inventory 
        WHERE name = 'apple';
    `;

    client
        .query(query)
        .then(result => {
            console.log('Delete completed');
            console.log(`Rows affected: ${result.rowCount}`);
        })
        .catch(err => {
            console.log(err);
            throw err;
        });
}
```

## <a name="next-steps"></a><span data-ttu-id="290b9-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="290b9-154">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="290b9-155">Migrera din databas med Exportera och importera</span><span class="sxs-lookup"><span data-stu-id="290b9-155">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
