---
title: "Ansluta till Azure Database för PostgreSQL med Node.js | Microsoft Docs"
description: "I den här snabbstarten finns ett kodexempel i Node.js som du kan använda för att ansluta till och fråga efter data från Azure Database för PostgreSQL."
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
ms.openlocfilehash: f6c98833c73b70bcf1f8ca53596a34f09807b276
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-database-for-postgresql-use-nodejs-to-connect-and-query-data"></a><span data-ttu-id="b21c7-103">Azure Database för PostgreSQL: Använda Node.js för att ansluta och fråga efter data</span><span class="sxs-lookup"><span data-stu-id="b21c7-103">Azure Database for PostgreSQL: Use Node.js to connect and query data</span></span>
<span data-ttu-id="b21c7-104">Den här snabbstarten visar hur du ansluter till en Azure-databas för att använda PostgreSQL [Node.js](https://nodejs.org/).</span><span class="sxs-lookup"><span data-stu-id="b21c7-104">This quickstart demonstrates how to connect to an Azure Database for PostgreSQL using [Node.js](https://nodejs.org/).</span></span> <span data-ttu-id="b21c7-105">Den visar hur du använder SQL-instruktioner för att fråga, infoga, uppdatera och ta bort data i databasen.</span><span class="sxs-lookup"><span data-stu-id="b21c7-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="b21c7-106">I den här artikeln förutsätter vi att du har kunskaper om Node.js och att du inte har arbetat med Azure Database för PostgreSQL tidigare.</span><span class="sxs-lookup"><span data-stu-id="b21c7-106">The steps in this article assume that you are familiar with developing using Node.js, and that you are new to working with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b21c7-107">Krav</span><span class="sxs-lookup"><span data-stu-id="b21c7-107">Prerequisites</span></span>
<span data-ttu-id="b21c7-108">I den här snabbstarten används de resurser som skapades i någon av följande guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="b21c7-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="b21c7-109">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="b21c7-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="b21c7-110">Skapa DB – CLI</span><span class="sxs-lookup"><span data-stu-id="b21c7-110">Create DB - CLI</span></span>](quickstart-create-server-database-azure-cli.md)

<span data-ttu-id="b21c7-111">Du måste också:</span><span class="sxs-lookup"><span data-stu-id="b21c7-111">You also need to:</span></span>
- <span data-ttu-id="b21c7-112">Installera [Node.js](https://nodejs.org)</span><span class="sxs-lookup"><span data-stu-id="b21c7-112">Install [Node.js](https://nodejs.org)</span></span>

## <a name="install-pg-client"></a><span data-ttu-id="b21c7-113">Installera pg-klienten</span><span class="sxs-lookup"><span data-stu-id="b21c7-113">Install pg client</span></span>
<span data-ttu-id="b21c7-114">Installera [pg](https://www.npmjs.com/package/pg), vilket är en PostgreSQL-klient för Node.js.</span><span class="sxs-lookup"><span data-stu-id="b21c7-114">Install [pg](https://www.npmjs.com/package/pg), which is a PostgreSQL client for Node.js.</span></span>

<span data-ttu-id="b21c7-115">Kör du node package manager (npm) för JavaScript från kommandoraden och installera pg-klienten.</span><span class="sxs-lookup"><span data-stu-id="b21c7-115">To do so, run the node package manager (npm) for JavaScript from your command line to install the pg client.</span></span>
```bash
npm install pg
```

<span data-ttu-id="b21c7-116">Verifiera installationen genom att visa en lista över de paket som har installerats.</span><span class="sxs-lookup"><span data-stu-id="b21c7-116">Verify the installation by listing the packages installed.</span></span>
```bash
npm list
```

## <a name="get-connection-information"></a><span data-ttu-id="b21c7-117">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="b21c7-117">Get connection information</span></span>
<span data-ttu-id="b21c7-118">Hämta den information som du behöver för att ansluta till Azure Database för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="b21c7-118">Get the connection information needed to connect to the Azure Database for PostgreSQL.</span></span> <span data-ttu-id="b21c7-119">Du behöver det fullständiga servernamnet och inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="b21c7-119">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="b21c7-120">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b21c7-120">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="b21c7-121">I den vänstra menyn i Azure-portalen klickar du på **alla resurser** och Sök efter den server som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="b21c7-121">From the left-hand menu in Azure portal, click **All resources** and search for the server you just created.</span></span>
3. <span data-ttu-id="b21c7-122">Klicka på servernamnet.</span><span class="sxs-lookup"><span data-stu-id="b21c7-122">Click the server name.</span></span>
4. <span data-ttu-id="b21c7-123">Välj serverns **översikt**-sida.</span><span class="sxs-lookup"><span data-stu-id="b21c7-123">Select the server's **Overview** page.</span></span> <span data-ttu-id="b21c7-124">Anteckna **servernamn** och **inloggningsnamnet för serveradministratören**.</span><span class="sxs-lookup"><span data-stu-id="b21c7-124">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="b21c7-125">![Azure Database för PostgreSQL – inloggning för serveradministratör](./media/connect-nodejs/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="b21c7-125">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-nodejs/1-connection-string.png)</span></span>
5. <span data-ttu-id="b21c7-126">Om du glömmer inloggningsinformationen för servern öppnar du sidan **Översikt** för att se inloggningsnamnet för serveradministratören. Om det behövs kan du återställa lösenordet.</span><span class="sxs-lookup"><span data-stu-id="b21c7-126">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="running-the-javascript-code-in-nodejs"></a><span data-ttu-id="b21c7-127">Kör JavaScript-kod i Node.js</span><span class="sxs-lookup"><span data-stu-id="b21c7-127">Running the JavaScript code in Node.js</span></span>
<span data-ttu-id="b21c7-128">Du kan köra Node.js från bash-gränssnitt eller windows kommandotolk genom att skriva `node`, köra exemplet JavaScript-kod interaktivt genom att kopiera och klistra den i kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="b21c7-128">You may launch Node.js from the bash shell or windows command prompt by typing `node`, then run the example JavaScript code interactively by copy and pasting it onto the prompt.</span></span> <span data-ttu-id="b21c7-129">Du kan också spara JavaScript-koden i en textfil och starta `node filename.js` med namnet som en parameter för att köra den.</span><span class="sxs-lookup"><span data-stu-id="b21c7-129">Alternatively, you may save the JavaScript code into a text file and launch `node filename.js` with the file name as a parameter to run it.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="b21c7-130">Ansluta, skapa tabell och infoga data</span><span class="sxs-lookup"><span data-stu-id="b21c7-130">Connect, create table, and insert data</span></span>
<span data-ttu-id="b21c7-131">Använd följande kod för att ansluta och läsa in data med hjälp av SQL-instruktionerna **CREATE TABLE** och **INSERT**.</span><span class="sxs-lookup"><span data-stu-id="b21c7-131">Use the following code to connect and load the data using **CREATE TABLE** and  **INSERT INTO** SQL statements.</span></span>
<span data-ttu-id="b21c7-132">Objektet [pg. Klienten](https://github.com/brianc/node-postgres/wiki/Client) används för gränssnittet med PostgreSQL-server.</span><span class="sxs-lookup"><span data-stu-id="b21c7-132">The [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) object is used to interface with the PostgreSQL server.</span></span> <span data-ttu-id="b21c7-133">Funktionen [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) används för att etablera anslutningen till servern.</span><span class="sxs-lookup"><span data-stu-id="b21c7-133">The [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) function is used to establish the connection to the server.</span></span> <span data-ttu-id="b21c7-134">Funktionen [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) används för att köra SQL-frågor mot en PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b21c7-134">The [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) function is used to execute the SQL query against PostgreSQL database.</span></span> 

<span data-ttu-id="b21c7-135">Ersätt parametrarna host, dbname,user och password med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="b21c7-135">Replace the host, dbname, user, and password parameters with the values that you specified when you created the server and database.</span></span>

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

## <a name="read-data"></a><span data-ttu-id="b21c7-136">Läsa data</span><span class="sxs-lookup"><span data-stu-id="b21c7-136">Read data</span></span>
<span data-ttu-id="b21c7-137">Använd följande kod för att ansluta och läsa data med en **SELECT**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="b21c7-137">Use the following code to connect and read the data using a **SELECT** SQL statement.</span></span> <span data-ttu-id="b21c7-138">Objektet [pg. Klienten](https://github.com/brianc/node-postgres/wiki/Client) används för gränssnittet med PostgreSQL-server.</span><span class="sxs-lookup"><span data-stu-id="b21c7-138">The [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) object is used to interface with the PostgreSQL server.</span></span> <span data-ttu-id="b21c7-139">Funktionen [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) används för att etablera anslutningen till servern.</span><span class="sxs-lookup"><span data-stu-id="b21c7-139">The [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) function is used to establish the connection to the server.</span></span> <span data-ttu-id="b21c7-140">Funktionen [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) används för att köra SQL-frågor mot en PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b21c7-140">The [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) function is used to execute the SQL query against PostgreSQL database.</span></span> 

<span data-ttu-id="b21c7-141">Ersätt parametrarna host, dbname,user och password med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="b21c7-141">Replace the host, dbname, user, and password parameters with the values that you specified when you created the server and database.</span></span> 

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
  
    console.log(`Running query to PostgreSQL server: ${config.host}`);

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

## <a name="update-data"></a><span data-ttu-id="b21c7-142">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="b21c7-142">Update data</span></span>
<span data-ttu-id="b21c7-143">Använd följande kod för att ansluta och läsa data med en **UPDATE**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="b21c7-143">Use the following code to connect and read the data using a **UPDATE** SQL statement.</span></span> <span data-ttu-id="b21c7-144">Objektet [pg. Klienten](https://github.com/brianc/node-postgres/wiki/Client) används för gränssnittet med PostgreSQL-server.</span><span class="sxs-lookup"><span data-stu-id="b21c7-144">The [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) object is used to interface with the PostgreSQL server.</span></span> <span data-ttu-id="b21c7-145">Funktionen [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) används för att etablera anslutningen till servern.</span><span class="sxs-lookup"><span data-stu-id="b21c7-145">The [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) function is used to establish the connection to the server.</span></span> <span data-ttu-id="b21c7-146">Funktionen [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) används för att köra SQL-frågor mot en PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b21c7-146">The [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) function is used to execute the SQL query against PostgreSQL database.</span></span> 

<span data-ttu-id="b21c7-147">Ersätt parametrarna host, dbname,user och password med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="b21c7-147">Replace the host, dbname, user, and password parameters with the values that you specified when you created the server and database.</span></span> 

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

## <a name="delete-data"></a><span data-ttu-id="b21c7-148">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="b21c7-148">Delete data</span></span>
<span data-ttu-id="b21c7-149">Använd följande kod för att ansluta och läsa data med en **DELETE**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="b21c7-149">Use the following code to connect and read the data using a **DELETE** SQL statement.</span></span> <span data-ttu-id="b21c7-150">Objektet [pg. Klienten](https://github.com/brianc/node-postgres/wiki/Client) används för gränssnittet med PostgreSQL-server.</span><span class="sxs-lookup"><span data-stu-id="b21c7-150">The [pg.Client](https://github.com/brianc/node-postgres/wiki/Client) object is used to interface with the PostgreSQL server.</span></span> <span data-ttu-id="b21c7-151">Funktionen [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) används för att etablera anslutningen till servern.</span><span class="sxs-lookup"><span data-stu-id="b21c7-151">The [pg.Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) function is used to establish the connection to the server.</span></span> <span data-ttu-id="b21c7-152">Funktionen [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) används för att köra SQL-frågor mot en PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b21c7-152">The [pg.Client.query()](https://github.com/brianc/node-postgres/wiki/Query) function is used to execute the SQL query against PostgreSQL database.</span></span> 

<span data-ttu-id="b21c7-153">Ersätt parametrarna host, dbname,user och password med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="b21c7-153">Replace the host, dbname, user, and password parameters with the values that you specified when you created the server and database.</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="b21c7-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b21c7-154">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="b21c7-155">Migrera din databas med Exportera och importera</span><span class="sxs-lookup"><span data-stu-id="b21c7-155">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
