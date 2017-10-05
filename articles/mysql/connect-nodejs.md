---
title: "Ansluta till Azure Database för MySQL med Node.js | Microsoft Docs"
description: "I den här snabbstarten finns flera kodexempel i Node.js som du kan använda för att ansluta till och fråga efter data från Azure Database för MySQL."
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
ms.openlocfilehash: 0c0bd4b707c114d2991e5f0473a4bfbe9e463e3c
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="azure-database-for-mysql-use-nodejs-to-connect-and-query-data"></a><span data-ttu-id="b30eb-103">Azure Database för MySQL: Använda Node.js för att ansluta och fråga efter data</span><span class="sxs-lookup"><span data-stu-id="b30eb-103">Azure Database for MySQL: Use Node.js to connect and query data</span></span>
<span data-ttu-id="b30eb-104">Den här snabbstarten visar hur du ansluter till en Azure Database för MySQL med hjälp av ett [Node.js](https://nodejs.org/)-program från plattformar med Windows, Ubuntu Linux och Mac.</span><span class="sxs-lookup"><span data-stu-id="b30eb-104">This quickstart demonstrates how to connect to an Azure Database for MySQL using [Node.js](https://nodejs.org/) from Windows, Ubuntu Linux, and Mac platforms.</span></span> <span data-ttu-id="b30eb-105">Den visar hur du använder SQL-instruktioner för att fråga, infoga, uppdatera och ta bort data i databasen.</span><span class="sxs-lookup"><span data-stu-id="b30eb-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="b30eb-106">I den här artikeln förutsätter vi att du har kunskaper om Node.js och att du inte har arbetat med Azure Database för MySQL tidigare.</span><span class="sxs-lookup"><span data-stu-id="b30eb-106">The steps in this article assume that you are familiar with developing using Node.js, and that you are new to working with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b30eb-107">Krav</span><span class="sxs-lookup"><span data-stu-id="b30eb-107">Prerequisites</span></span>
<span data-ttu-id="b30eb-108">I den här snabbstarten används de resurser som skapades i någon av följande guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="b30eb-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="b30eb-109">Skapa en Azure Database för MySQL med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b30eb-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="b30eb-110">Skapa en Azure Database för MySQL-server med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b30eb-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

<span data-ttu-id="b30eb-111">Du måste också:</span><span class="sxs-lookup"><span data-stu-id="b30eb-111">You also need to:</span></span>
- <span data-ttu-id="b30eb-112">Installera [Node.js](https://nodejs.org) runtime.</span><span class="sxs-lookup"><span data-stu-id="b30eb-112">Install the [Node.js](https://nodejs.org) runtime.</span></span>
- <span data-ttu-id="b30eb-113">Installera [mysql2](https://www.npmjs.com/package/mysql2)-paketet för att ansluta till MySQL från Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="b30eb-113">Install [mysql2](https://www.npmjs.com/package/mysql2) package to connect to MySQL from the Node.js application.</span></span> 

## <a name="install-nodejs-and-the-mysql-connector"></a><span data-ttu-id="b30eb-114">Installera Node.js och MySQL Connector</span><span class="sxs-lookup"><span data-stu-id="b30eb-114">Install Node.js and the MySQL connector</span></span>
<span data-ttu-id="b30eb-115">Följ instruktionerna för att installera Node.js beroende på din plattform.</span><span class="sxs-lookup"><span data-stu-id="b30eb-115">Depending on your platform, follow the appropriate instructions to install Node.js.</span></span> <span data-ttu-id="b30eb-116">Använd npm för att installera paketet mysql2 och dess beroenden i projektmappen.</span><span class="sxs-lookup"><span data-stu-id="b30eb-116">Use npm to install the mysql2 package and its dependencies into your project folder.</span></span>

### <a name="windows"></a><span data-ttu-id="b30eb-117">**Windows**</span><span class="sxs-lookup"><span data-stu-id="b30eb-117">**Windows**</span></span>
1. <span data-ttu-id="b30eb-118">Besök [hämtningssidan för Node.js ](https://nodejs.org/en/download/) och välj önskat alternativ för Windows installeringsverktyg.</span><span class="sxs-lookup"><span data-stu-id="b30eb-118">Visit the [Node.js downloads page](https://nodejs.org/en/download/) and select your desired Windows installer option.</span></span>
2. <span data-ttu-id="b30eb-119">Skapa en lokal projektmapp som till exempel `nodejsmysql`.</span><span class="sxs-lookup"><span data-stu-id="b30eb-119">Make a local project folder such as `nodejsmysql`.</span></span> 
3. <span data-ttu-id="b30eb-120">Starta Kommandotolken och cd i projektmappen, till exempel `cd c:\nodejsmysql\`</span><span class="sxs-lookup"><span data-stu-id="b30eb-120">Launch the command prompt and cd into the project folder, such as `cd c:\nodejsmysql\`</span></span>
4. <span data-ttu-id="b30eb-121">Kör verktyget NPM om du vill installera mysql2-biblioteket i projektmappen.</span><span class="sxs-lookup"><span data-stu-id="b30eb-121">Run the NPM tool to install the mysql2 library into the project folder.</span></span>

   ```cmd
   cd c:\nodejsmysql\
   "C:\Program Files\nodejs\npm" install mysql2
   "C:\Program Files\nodejs\npm" list
   ```

5. <span data-ttu-id="b30eb-122">Verifiera installationen genom att kontrollera `npm list`-utdatatexten för `mysql2@1.3.5`.</span><span class="sxs-lookup"><span data-stu-id="b30eb-122">Verify the installation by checking the `npm list` output text for `mysql2@1.3.5`.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="b30eb-123">**Linux (Ubuntu)**</span><span class="sxs-lookup"><span data-stu-id="b30eb-123">**Linux (Ubuntu)**</span></span>
1. <span data-ttu-id="b30eb-124">Ange följande kommandon för att installera **npm** och **Node.js**, en användarvänlig pakethanterare för Node.js.</span><span class="sxs-lookup"><span data-stu-id="b30eb-124">Run the following commands to install **Node.js** and **npm** the package manager for Node.js.</span></span>

   ```bash
   sudo apt-get install -y nodejs npm
   ```

2. <span data-ttu-id="b30eb-125">Kör följande kommandon för att skapa en projektmapp `mysqlnodejs` och installera mysql2-paketet till mappen.</span><span class="sxs-lookup"><span data-stu-id="b30eb-125">Run the following commands to make a project folder `mysqlnodejs` and install the mysql2 package into that folder.</span></span>

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql2
   npm list
   ```
3. <span data-ttu-id="b30eb-126">Verifiera installationen genom att kontrollera npm-listans utdatatext för `mysql2@1.3.5`.</span><span class="sxs-lookup"><span data-stu-id="b30eb-126">Verify the installation by checking npm list output text for `mysql2@1.3.5`.</span></span>

### <a name="mac-os"></a><span data-ttu-id="b30eb-127">**Mac OS**</span><span class="sxs-lookup"><span data-stu-id="b30eb-127">**Mac OS**</span></span>
1. <span data-ttu-id="b30eb-128">Ange följande kommandon för att installera **brew**, en användarvänlig pakethanterare för Mac OS X och **Node.js**.</span><span class="sxs-lookup"><span data-stu-id="b30eb-128">Enter the following commands to install **brew**, an easy-to-use package manager for Mac OS X and **Node.js**.</span></span>

   ```bash
   ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   brew install node
   ```
2. <span data-ttu-id="b30eb-129">Kör följande kommandon för att skapa en projektmapp `mysqlnodejs` och installera mysql2-paketet till mappen.</span><span class="sxs-lookup"><span data-stu-id="b30eb-129">Run the following commands to make a project folder `mysqlnodejs` and install the mysql2 package into that folder.</span></span>

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql2
   npm list
   ```

3. <span data-ttu-id="b30eb-130">Verifiera installationen genom att kontrollera `npm list`-utdatatexten för `mysql2@1.3.6`.</span><span class="sxs-lookup"><span data-stu-id="b30eb-130">Verify the installation by checking the `npm list` output text for `mysql2@1.3.6`.</span></span> <span data-ttu-id="b30eb-131">Versionsnumret kan variera när nya korrigeringsfiler blir tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="b30eb-131">The version number may vary as new patches are released.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="b30eb-132">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="b30eb-132">Get connection information</span></span>
<span data-ttu-id="b30eb-133">Skaffa den information som du behöver för att ansluta till Azure Database för MySQL.</span><span class="sxs-lookup"><span data-stu-id="b30eb-133">Get the connection information needed to connect to the Azure Database for MySQL.</span></span> <span data-ttu-id="b30eb-134">Du behöver det fullständiga servernamnet och inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="b30eb-134">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="b30eb-135">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b30eb-135">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="b30eb-136">I den vänstra rutan klickar du på **alla resurser** och söker sedan efter servern som du skapade (till exempel **myserver4demo**).</span><span class="sxs-lookup"><span data-stu-id="b30eb-136">In the left pane, click **All resources**, and then search for the server you have created (for example, **myserver4demo**).</span></span>
3. <span data-ttu-id="b30eb-137">Klicka på servernamnet **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="b30eb-137">Click the server name **myserver4demo**.</span></span>
4. <span data-ttu-id="b30eb-138">Välj sidan **Egenskaper** för servern.</span><span class="sxs-lookup"><span data-stu-id="b30eb-138">Select the server's **Properties** page.</span></span> <span data-ttu-id="b30eb-139">Anteckna **servernamn** och **inloggningsnamnet för serveradministratören**.</span><span class="sxs-lookup"><span data-stu-id="b30eb-139">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="b30eb-140">![Azure Database för MySQL – inloggning för serveradministratör](./media/connect-nodejs/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="b30eb-140">![Azure Database for MySQL - Server Admin Login](./media/connect-nodejs/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="b30eb-141">Om du glömmer inloggningsinformationen för servern öppnar du sidan **Översikt** för att se inloggningsnamnet för serveradministratören. Om det behövs kan du återställa lösenordet.</span><span class="sxs-lookup"><span data-stu-id="b30eb-141">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="running-the-javascript-code-in-nodejs"></a><span data-ttu-id="b30eb-142">Kör JavaScript-kod i Node.js</span><span class="sxs-lookup"><span data-stu-id="b30eb-142">Running the JavaScript code in Node.js</span></span>
1. <span data-ttu-id="b30eb-143">Klistra in JavaScript-koden i en textfil och spara filen till en projektmapp med filnamnstillägget .js, till exempel C:\nodejsmysql\createtable.js eller /home/username/nodejsmysql/createtable.js</span><span class="sxs-lookup"><span data-stu-id="b30eb-143">Paste the JavaScript code into text files, and save into a project folder with file extension .js, such as C:\nodejsmysql\createtable.js or /home/username/nodejsmysql/createtable.js</span></span>
2. <span data-ttu-id="b30eb-144">Om du vill köra koden startar du kommandotolken eller bash-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="b30eb-144">Launch the command prompt or bash shell.</span></span> <span data-ttu-id="b30eb-145">Ändra katalog till din projektmapp, till exempel `cd nodejsmysql`.</span><span class="sxs-lookup"><span data-stu-id="b30eb-145">Change directory into your project folder `cd nodejsmysql`.</span></span>
3. <span data-ttu-id="b30eb-146">Skriv node-kommandot följt av filnamnet, till exempel `node createtable.js`, för att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="b30eb-146">To run the application, type the node command followed by the file name, such as `node createtable.js`.</span></span>
4. <span data-ttu-id="b30eb-147">I Windows, om node-programmet inte finns på sökvägen för miljövariabeln, kan du behöva använda den fullständiga sökvägen för att starta nodprogrammet, till exempel `"C:\Program Files\nodejs\node.exe" createtable.js`</span><span class="sxs-lookup"><span data-stu-id="b30eb-147">On Windows, if the node application is not in your environment variable path, you may need to use the full path to launch the node application, such as `"C:\Program Files\nodejs\node.exe" createtable.js`</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="b30eb-148">Ansluta, skapa tabell och infoga data</span><span class="sxs-lookup"><span data-stu-id="b30eb-148">Connect, create table, and insert data</span></span>
<span data-ttu-id="b30eb-149">Använd följande kod för att ansluta och läsa in data med hjälp av SQL-instruktionerna **CREATE TABLE** och **INSERT**.</span><span class="sxs-lookup"><span data-stu-id="b30eb-149">Use the following code to connect and load the data using **CREATE TABLE** and  **INSERT INTO** SQL statements.</span></span>

<span data-ttu-id="b30eb-150">Metoden [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) används som gränssnitt med MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="b30eb-150">The [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) method is used to interface with the MySQL server.</span></span> <span data-ttu-id="b30eb-151">Funktionen [connect()](https://github.com/mysqljs/mysql#establishing-connections) används för att etablera anslutningen till servern.</span><span class="sxs-lookup"><span data-stu-id="b30eb-151">The [connect()](https://github.com/mysqljs/mysql#establishing-connections) function is used to establish the connection to the server.</span></span> <span data-ttu-id="b30eb-152">Funktionen [query()](https://github.com/mysqljs/mysql#performing-queries) används för att köra SQL-frågor mot en MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b30eb-152">The [query()](https://github.com/mysqljs/mysql#performing-queries) function is used to execute the SQL query against MySQL database.</span></span> 

<span data-ttu-id="b30eb-153">Ersätt parametrarna `host`, `user`, `password` och `database` med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="b30eb-153">Replace the `host`, `user`, `password`, and `database` parameters with the values that you specified when you created the server and database.</span></span>

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

## <a name="read-data"></a><span data-ttu-id="b30eb-154">Läsa data</span><span class="sxs-lookup"><span data-stu-id="b30eb-154">Read data</span></span>
<span data-ttu-id="b30eb-155">Använd följande kod för att ansluta och läsa data med en **SELECT**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="b30eb-155">Use the following code to connect and read the data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="b30eb-156">Metoden [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) används som gränssnitt med MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="b30eb-156">The [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) method is used to interface with the MySQL server.</span></span> <span data-ttu-id="b30eb-157">Metoden [connect()](https://github.com/mysqljs/mysql#establishing-connections) används för att etablera anslutningen till servern.</span><span class="sxs-lookup"><span data-stu-id="b30eb-157">The [connect()](https://github.com/mysqljs/mysql#establishing-connections) method is used to establish the connection to the server.</span></span> <span data-ttu-id="b30eb-158">Metoden [query()](https://github.com/mysqljs/mysql#performing-queries) används för att köra SQL-frågor mot en MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b30eb-158">The [query()](https://github.com/mysqljs/mysql#performing-queries) method is used to execute the SQL query against MySQL database.</span></span> <span data-ttu-id="b30eb-159">Resultatmatrisen används för resultat från frågan.</span><span class="sxs-lookup"><span data-stu-id="b30eb-159">The results array is used to hold the results of the query.</span></span>

<span data-ttu-id="b30eb-160">Ersätt parametrarna `host`, `user`, `password` och `database` med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="b30eb-160">Replace the `host`, `user`, `password`, and `database` parameters with the values that you specified when you created the server and database.</span></span>

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

## <a name="update-data"></a><span data-ttu-id="b30eb-161">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="b30eb-161">Update data</span></span>
<span data-ttu-id="b30eb-162">Använd följande kod för att ansluta och läsa data med en **UPDATE**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="b30eb-162">Use the following code to connect and read the data using a **UPDATE** SQL statement.</span></span> 

<span data-ttu-id="b30eb-163">Metoden [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) används som gränssnitt med MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="b30eb-163">The [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) method is used to interface with the MySQL server.</span></span> <span data-ttu-id="b30eb-164">Metoden [connect()](https://github.com/mysqljs/mysql#establishing-connections) används för att etablera anslutningen till servern.</span><span class="sxs-lookup"><span data-stu-id="b30eb-164">The [connect()](https://github.com/mysqljs/mysql#establishing-connections) method is used to establish the connection to the server.</span></span> <span data-ttu-id="b30eb-165">Metoden [query()](https://github.com/mysqljs/mysql#performing-queries) används för att köra SQL-frågor mot en MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b30eb-165">The [query()](https://github.com/mysqljs/mysql#performing-queries) method is used to execute the SQL query against MySQL database.</span></span> 

<span data-ttu-id="b30eb-166">Ersätt parametrarna `host`, `user`, `password` och `database` med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="b30eb-166">Replace the `host`, `user`, `password`, and `database` parameters with the values that you specified when you created the server and database.</span></span>

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

## <a name="delete-data"></a><span data-ttu-id="b30eb-167">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="b30eb-167">Delete data</span></span>
<span data-ttu-id="b30eb-168">Använd följande kod för att ansluta och läsa data med en **DELETE**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="b30eb-168">Use the following code to connect and read the data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="b30eb-169">Metoden [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) används som gränssnitt med MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="b30eb-169">The [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) method is used to interface with the MySQL server.</span></span> <span data-ttu-id="b30eb-170">Metoden [connect()](https://github.com/mysqljs/mysql#establishing-connections) används för att etablera anslutningen till servern.</span><span class="sxs-lookup"><span data-stu-id="b30eb-170">The [connect()](https://github.com/mysqljs/mysql#establishing-connections) method is used to establish the connection to the server.</span></span> <span data-ttu-id="b30eb-171">Metoden [query()](https://github.com/mysqljs/mysql#performing-queries) används för att köra SQL-frågor mot en MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b30eb-171">The [query()](https://github.com/mysqljs/mysql#performing-queries) method is used to execute the SQL query against MySQL database.</span></span> 

<span data-ttu-id="b30eb-172">Ersätt parametrarna `host`, `user`, `password` och `database` med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="b30eb-172">Replace the `host`, `user`, `password`, and `database` parameters with the values that you specified when you created the server and database.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="b30eb-173">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b30eb-173">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="b30eb-174">Migrera din databas med Exportera och importera</span><span class="sxs-lookup"><span data-stu-id="b30eb-174">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
