---
title: "Ansluta tooAzure databas för MySQL från Node.js | Microsoft Docs"
description: "Denna Snabbstart innehåller flera Node.js-kodexempel du kan använda tooconnect och fråga efter data från Azure-databas för MySQL."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 07/17/2017
ms.openlocfilehash: ed9a39b5ab003e8216cef1c0f6a95a75c3db8458
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-nodejs-tooconnect-and-query-data"></a>Azure-databas för MySQL: Använd Node.js tooconnect och fråga data
Den här snabbstarten visar hur tooconnect tooan Azure-databas för att använda MySQL [Node.js](https://nodejs.org/) från Windows, Ubuntu Linux och Mac-plattformar. Den visar hur toouse SQL-instruktioner tooquery infoga, uppdatera och ta bort data i hello-databas. hello förutsätter stegen i den här artikeln att du är bekant med att utveckla med Node.js och att du är ny tooworking med Azure-databas för MySQL.

## <a name="prerequisites"></a>Krav
Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:
- [Skapa en Azure Database för MySQL med Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Skapa en Azure Database för MySQL-server med Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

Du måste också:
- Installera hello [Node.js](https://nodejs.org) runtime.
- Installera [mysql2](https://www.npmjs.com/package/mysql2) paketet tooconnect tooMySQL från hello Node.js-program. 

## <a name="install-nodejs-and-hello-mysql-connector"></a>Installera Node.js och hello MySQL-koppling
Följ hello lämpliga instruktioner tooinstall Node.js beroende på din plattform. Använd npm tooinstall hello mysql2 paket och dess beroenden i projektmappen.

### <a name="windows"></a>**Windows**
1. Besök hello [Node.js hämtar sidan](https://nodejs.org/en/download/) och välj din önskade alternativ för Windows installer.
2. Skapa en lokal projektmapp som till exempel `nodejsmysql`. 
3. Starta CD-skivan och hello Kommandotolken i hello projektmappen som`cd c:\nodejsmysql\`
4. Kör hello NPM tooinstall hello mysql2 biblioteket i hello projektmappen.

   ```cmd
   cd c:\nodejsmysql\
   "C:\Program Files\nodejs\npm" install mysql2
   "C:\Program Files\nodejs\npm" list
   ```

5. Kontrollera installationen av hello hello `npm list` utdata text för `mysql2@1.3.5`.

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
1. Kör hello följande kommandon tooinstall **Node.js** och **npm** hello package manager för Node.js.

   ```bash
   sudo apt-get install -y nodejs npm
   ```

2. Kör följande kommandon toomake hello en projektmapp `mysqlnodejs` och installera hello mysql2 paketet till mappen.

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql2
   npm list
   ```
3. Kontrollera hello installationen genom att kontrollera npm lista resultattexten för `mysql2@1.3.5`.

### <a name="mac-os"></a>**Mac OS**
1. Ange följande kommandon tooinstall hello **brew**, ett enkelt att använda package manager för Mac OS X och **Node.js**.

   ```bash
   ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   brew install node
   ```
2. Kör följande kommandon toomake hello en projektmapp `mysqlnodejs` och installera hello mysql2 paketet till mappen.

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql2
   npm list
   ```

3. Kontrollera installationen av hello hello `npm list` utdata text för `mysql2@1.3.6`. hello kan versionsnumret variera när nya korrigeringsfiler blir tillgängliga.

## <a name="get-connection-information"></a>Hämta anslutningsinformation
Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för MySQL. Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Hello vänster klickar du på **alla resurser**, och sök sedan efter hello-server som du har skapat (till exempel **myserver4demo**).
3. Klicka på servernamnet för hello **myserver4demo**.
4. Välj hello server **egenskaper** sidan. Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.
 ![Azure Database för MySQL – inloggning för serveradministratör](./media/connect-nodejs/1_server-properties-name-login.png)
5. Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet och, om nödvändigt återställa hello lösenord.

## <a name="running-hello-javascript-code-in-nodejs"></a>Kör hello JavaScript-kod i Node.js
1. Klistra in hello JavaScript-kod i textfiler och spara i en projektmapp med filen tillägget .js, till exempel C:\nodejsmysql\createtable.js eller /home/username/nodejsmysql/createtable.js
2. Starta Kommandotolken hello eller bash shell. Ändra katalog till din projektmapp, till exempel `cd nodejsmysql`.
3. toorun hello programmet skriver hello nod kommandot följt av hello filnamn, som `node createtable.js`.
4. I Windows, om hello nod programmet inte finns i din miljö variabeln sökväg måste toouse hello fullständig sökväg toolaunch hello noden program som`"C:\Program Files\nodejs\node.exe" createtable.js`

## <a name="connect-create-table-and-insert-data"></a>Ansluta, skapa tabell och infoga data
Använd hello följande kod tooconnect och läsa in hello data med hjälp av **CREATE TABLE** och **INSERT INTO** SQL-instruktioner.

Hej [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metod är att använda toointerface med hello MySQL-servern. Hej [connect()](https://github.com/mysqljs/mysql#establishing-connections) funktionen är används tooestablish hello anslutning toohello server. Hej [query()](https://github.com/mysqljs/mysql#performing-queries) funktionen är används tooexecute hello SQL-fråga mot MySQL-databas. 

Ersätt hello `host`, `user`, `password`, och `database` parametrar med hello värden som du angav när du skapade hello-server och databas.

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
    if (err) { 
        console.log("!!! Cannot connect !!! Error:");
        throw err;
    }
    else
    {
       console.log("Connection established.");
           queryDatabase();
    }   
});

function queryDatabase(){
       conn.query('DROP TABLE IF EXISTS inventory;', function (err, results, fields) { 
            if (err) throw err; 
            console.log('Dropped inventory table if existed.');
        })
       conn.query('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);', 
            function (err, results, fields) {
                if (err) throw err;
            console.log('Created inventory table.');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['banana', 150], 
            function (err, results, fields) {
                if (err) throw err;
            else console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['orange', 154], 
            function (err, results, fields) {
                if (err) throw err;
            console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['apple', 100], 
        function (err, results, fields) {
                if (err) throw err;
            console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.end(function (err) { 
        if (err) throw err;
        else  console.log('Done.') 
        });
};
```

## <a name="read-data"></a>Läsa data
Använd hello följande kod tooconnect och läsa hello data med hjälp av en **Välj** SQL-instruktionen. 

Hej [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metod är att använda toointerface med hello MySQL-servern. Hej [connect()](https://github.com/mysqljs/mysql#establishing-connections) metoden är används tooestablish hello anslutning toohello server. Hej [query()](https://github.com/mysqljs/mysql#performing-queries) metod är att använda tooexecute hello SQL-fråga mot MySQL-databas. hello resultat matrisen är används toohold hello resultaten av hello-frågan.

Ersätt hello `host`, `user`, `password`, och `database` parametrar med hello värden som du angav när du skapade hello-server och databas.

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            readData();
        }   
    });

function readData(){
        conn.query('SELECT * FROM inventory', 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Selected ' + results.length + ' row(s).');
                for (i = 0; i < results.length; i++) {
                    console.log('Row: ' + JSON.stringify(results[i]));
                }
                console.log('Done.');
            })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Closing connection.') 
        });
};
```

## <a name="update-data"></a>Uppdatera data
Använd hello följande kod tooconnect och läsa hello data med hjälp av en **uppdatering** SQL-instruktionen. 

Hej [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metod är att använda toointerface med hello MySQL-servern. Hej [connect()](https://github.com/mysqljs/mysql#establishing-connections) metoden är används tooestablish hello anslutning toohello server. Hej [query()](https://github.com/mysqljs/mysql#performing-queries) metod är att använda tooexecute hello SQL-fråga mot MySQL-databas. 

Ersätt hello `host`, `user`, `password`, och `database` parametrar med hello värden som du angav när du skapade hello-server och databas.

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            updateData();
        }   
    });

function updateData(){
       conn.query('UPDATE inventory SET quantity = ? WHERE name = ?', [200, 'banana'], 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Updated ' + results.affectedRows + ' row(s).');
        })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Done.') 
        });
};
```

## <a name="delete-data"></a>Ta bort data
Använd hello följande kod tooconnect och läsa hello data med hjälp av en **ta bort** SQL-instruktionen. 

Hej [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metod är att använda toointerface med hello MySQL-servern. Hej [connect()](https://github.com/mysqljs/mysql#establishing-connections) metoden är används tooestablish hello anslutning toohello server. Hej [query()](https://github.com/mysqljs/mysql#performing-queries) metod är att använda tooexecute hello SQL-fråga mot MySQL-databas. 

Ersätt hello `host`, `user`, `password`, och `database` parametrar med hello värden som du angav när du skapade hello-server och databas.

```javascript
const mysql = require('mysql2');

var config =
{
    host: 'myserver4demo.mysql.database.azure.com',
    user: 'myadmin@myserver4demo',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            deleteData();
        }   
    });

function deleteData(){
       conn.query('DELETE FROM inventory WHERE name = ?', ['orange'], 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Deleted ' + results.affectedRows + ' row(s).');
        })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Done.') 
        });
};
```

## <a name="next-steps"></a>Nästa steg
> [!div class="nextstepaction"]
> [Migrera din databas med Exportera och importera](./concepts-migrate-import-export.md)
