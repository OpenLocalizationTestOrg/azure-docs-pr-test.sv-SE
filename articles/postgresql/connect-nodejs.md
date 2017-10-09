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
# <a name="azure-database-for-postgresql-use-nodejs-tooconnect-and-query-data"></a>Azure-databas för PostgreSQL: Använd Node.js tooconnect och fråga data
Den här snabbstarten visar hur tooconnect tooan Azure-databas för att använda PostgreSQL [Node.js](https://nodejs.org/). Den visar hur toouse SQL-instruktioner tooquery infoga, uppdatera och ta bort data i hello-databas. hello förutsätter stegen i den här artikeln att du är bekant med att utveckla med Node.js och att du är ny tooworking med Azure-databas för PostgreSQL.

## <a name="prerequisites"></a>Krav
Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:
- [Skapa DB – Portal](quickstart-create-server-database-portal.md)
- [Skapa DB – CLI](quickstart-create-server-database-azure-cli.md)

Du måste också:
- Installera [Node.js](https://nodejs.org)

## <a name="install-pg-client"></a>Installera pg-klienten
Installera [pg](https://www.npmjs.com/package/pg), vilket är en PostgreSQL-klient för Node.js.

toodo så kör hello nod Pakethanteraren (npm) för JavaScript från kommandoraden tooinstall hello pg klienten.
```bash
npm install pg
```

Kontrollera hello installationen genom att ange hello-paket installeras.
```bash
npm list
```

## <a name="get-connection-information"></a>Hämta anslutningsinformation
Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för PostgreSQL. Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Hello vänstra menyn i Azure-portalen klickar du på **alla resurser** och Sök efter hello-server som du nyss skapade.
3. Klicka på hello servernamn.
4. Välj hello server **översikt** sidan. Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.
 ![Azure Database för PostgreSQL – inloggning för serveradministratör](./media/connect-nodejs/1-connection-string.png)
5. Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet och, om nödvändigt återställa hello lösenord.

## <a name="running-hello-javascript-code-in-nodejs"></a>Kör hello JavaScript-kod i Node.js
Du kan köra Node.js från hello bash-gränssnitt eller windows kommandotolk genom att skriva `node`, köras interaktivt hello exempel JavaScript-kod genom att kopiera och klistra in hello-Kommandotolken. Du kan också spara hello JavaScript-kod i en textfil och starta `node filename.js` med hello filnamn som en parameter toorun den.

## <a name="connect-create-table-and-insert-data"></a>Ansluta, skapa tabell och infoga data
Använd hello följande kod tooconnect och läsa in hello data med hjälp av **CREATE TABLE** och **INSERT INTO** SQL-instruktioner.
Hej [pg. Klienten](https://github.com/brianc/node-postgres/wiki/Client) objektet är används toointerface med hello PostgreSQL-servern. Hej [pg. Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) funktionen är används tooestablish hello anslutning toohello server. Hej [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) funktionen är används tooexecute hello SQL-fråga mot PostgreSQL-databas. 

Ersätt hello värden, dbname, användare och lösenordsparametrar med hello värden som du angav när du skapade hello-server och databas.

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

## <a name="read-data"></a>Läsa data
Använd hello följande kod tooconnect och läsa hello data med hjälp av en **Välj** SQL-instruktionen. Hej [pg. Klienten](https://github.com/brianc/node-postgres/wiki/Client) objektet är används toointerface med hello PostgreSQL-servern. Hej [pg. Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) funktionen är används tooestablish hello anslutning toohello server. Hej [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) funktionen är används tooexecute hello SQL-fråga mot PostgreSQL-databas. 

Ersätt hello värden, dbname, användare och lösenordsparametrar med hello värden som du angav när du skapade hello-server och databas. 

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

## <a name="update-data"></a>Uppdatera data
Använd hello följande kod tooconnect och läsa hello data med hjälp av en **uppdatering** SQL-instruktionen. Hej [pg. Klienten](https://github.com/brianc/node-postgres/wiki/Client) objektet är används toointerface med hello PostgreSQL-servern. Hej [pg. Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) funktionen är används tooestablish hello anslutning toohello server. Hej [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) funktionen är används tooexecute hello SQL-fråga mot PostgreSQL-databas. 

Ersätt hello värden, dbname, användare och lösenordsparametrar med hello värden som du angav när du skapade hello-server och databas. 

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

## <a name="delete-data"></a>Ta bort data
Använd hello följande kod tooconnect och läsa hello data med hjälp av en **ta bort** SQL-instruktionen. Hej [pg. Klienten](https://github.com/brianc/node-postgres/wiki/Client) objektet är används toointerface med hello PostgreSQL-servern. Hej [pg. Client.connect()](https://github.com/brianc/node-postgres/wiki/Client#method-connect) funktionen är används tooestablish hello anslutning toohello server. Hej [pg. Client.Query()](https://github.com/brianc/node-postgres/wiki/Query) funktionen är används tooexecute hello SQL-fråga mot PostgreSQL-databas. 

Ersätt hello värden, dbname, användare och lösenordsparametrar med hello värden som du angav när du skapade hello-server och databas. 

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

## <a name="next-steps"></a>Nästa steg
> [!div class="nextstepaction"]
> [Migrera din databas med Exportera och importera](./howto-migrate-using-export-and-import.md)
