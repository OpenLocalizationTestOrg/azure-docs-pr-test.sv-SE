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
# <a name="azure-database-for-mysql-use-nodejs-tooconnect-and-query-data"></a><span data-ttu-id="7db83-103">Azure-databas för MySQL: Använd Node.js tooconnect och fråga data</span><span class="sxs-lookup"><span data-stu-id="7db83-103">Azure Database for MySQL: Use Node.js tooconnect and query data</span></span>
<span data-ttu-id="7db83-104">Den här snabbstarten visar hur tooconnect tooan Azure-databas för att använda MySQL [Node.js](https://nodejs.org/) från Windows, Ubuntu Linux och Mac-plattformar.</span><span class="sxs-lookup"><span data-stu-id="7db83-104">This quickstart demonstrates how tooconnect tooan Azure Database for MySQL using [Node.js](https://nodejs.org/) from Windows, Ubuntu Linux, and Mac platforms.</span></span> <span data-ttu-id="7db83-105">Den visar hur toouse SQL-instruktioner tooquery infoga, uppdatera och ta bort data i hello-databas.</span><span class="sxs-lookup"><span data-stu-id="7db83-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="7db83-106">hello förutsätter stegen i den här artikeln att du är bekant med att utveckla med Node.js och att du är ny tooworking med Azure-databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="7db83-106">hello steps in this article assume that you are familiar with developing using Node.js, and that you are new tooworking with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7db83-107">Krav</span><span class="sxs-lookup"><span data-stu-id="7db83-107">Prerequisites</span></span>
<span data-ttu-id="7db83-108">Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="7db83-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="7db83-109">Skapa en Azure Database för MySQL med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7db83-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="7db83-110">Skapa en Azure Database för MySQL-server med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7db83-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

<span data-ttu-id="7db83-111">Du måste också:</span><span class="sxs-lookup"><span data-stu-id="7db83-111">You also need to:</span></span>
- <span data-ttu-id="7db83-112">Installera hello [Node.js](https://nodejs.org) runtime.</span><span class="sxs-lookup"><span data-stu-id="7db83-112">Install hello [Node.js](https://nodejs.org) runtime.</span></span>
- <span data-ttu-id="7db83-113">Installera [mysql2](https://www.npmjs.com/package/mysql2) paketet tooconnect tooMySQL från hello Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="7db83-113">Install [mysql2](https://www.npmjs.com/package/mysql2) package tooconnect tooMySQL from hello Node.js application.</span></span> 

## <a name="install-nodejs-and-hello-mysql-connector"></a><span data-ttu-id="7db83-114">Installera Node.js och hello MySQL-koppling</span><span class="sxs-lookup"><span data-stu-id="7db83-114">Install Node.js and hello MySQL connector</span></span>
<span data-ttu-id="7db83-115">Följ hello lämpliga instruktioner tooinstall Node.js beroende på din plattform.</span><span class="sxs-lookup"><span data-stu-id="7db83-115">Depending on your platform, follow hello appropriate instructions tooinstall Node.js.</span></span> <span data-ttu-id="7db83-116">Använd npm tooinstall hello mysql2 paket och dess beroenden i projektmappen.</span><span class="sxs-lookup"><span data-stu-id="7db83-116">Use npm tooinstall hello mysql2 package and its dependencies into your project folder.</span></span>

### <a name="windows"></a><span data-ttu-id="7db83-117">**Windows**</span><span class="sxs-lookup"><span data-stu-id="7db83-117">**Windows**</span></span>
1. <span data-ttu-id="7db83-118">Besök hello [Node.js hämtar sidan](https://nodejs.org/en/download/) och välj din önskade alternativ för Windows installer.</span><span class="sxs-lookup"><span data-stu-id="7db83-118">Visit hello [Node.js downloads page](https://nodejs.org/en/download/) and select your desired Windows installer option.</span></span>
2. <span data-ttu-id="7db83-119">Skapa en lokal projektmapp som till exempel `nodejsmysql`.</span><span class="sxs-lookup"><span data-stu-id="7db83-119">Make a local project folder such as `nodejsmysql`.</span></span> 
3. <span data-ttu-id="7db83-120">Starta CD-skivan och hello Kommandotolken i hello projektmappen som`cd c:\nodejsmysql\`</span><span class="sxs-lookup"><span data-stu-id="7db83-120">Launch hello command prompt and cd into hello project folder, such as `cd c:\nodejsmysql\`</span></span>
4. <span data-ttu-id="7db83-121">Kör hello NPM tooinstall hello mysql2 biblioteket i hello projektmappen.</span><span class="sxs-lookup"><span data-stu-id="7db83-121">Run hello NPM tool tooinstall hello mysql2 library into hello project folder.</span></span>

   ```cmd
   cd c:\nodejsmysql\
   "C:\Program Files\nodejs\npm" install mysql2
   "C:\Program Files\nodejs\npm" list
   ```

5. <span data-ttu-id="7db83-122">Kontrollera installationen av hello hello `npm list` utdata text för `mysql2@1.3.5`.</span><span class="sxs-lookup"><span data-stu-id="7db83-122">Verify hello installation by checking hello `npm list` output text for `mysql2@1.3.5`.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="7db83-123">**Linux (Ubuntu)**</span><span class="sxs-lookup"><span data-stu-id="7db83-123">**Linux (Ubuntu)**</span></span>
1. <span data-ttu-id="7db83-124">Kör hello följande kommandon tooinstall **Node.js** och **npm** hello package manager för Node.js.</span><span class="sxs-lookup"><span data-stu-id="7db83-124">Run hello following commands tooinstall **Node.js** and **npm** hello package manager for Node.js.</span></span>

   ```bash
   sudo apt-get install -y nodejs npm
   ```

2. <span data-ttu-id="7db83-125">Kör följande kommandon toomake hello en projektmapp `mysqlnodejs` och installera hello mysql2 paketet till mappen.</span><span class="sxs-lookup"><span data-stu-id="7db83-125">Run hello following commands toomake a project folder `mysqlnodejs` and install hello mysql2 package into that folder.</span></span>

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql2
   npm list
   ```
3. <span data-ttu-id="7db83-126">Kontrollera hello installationen genom att kontrollera npm lista resultattexten för `mysql2@1.3.5`.</span><span class="sxs-lookup"><span data-stu-id="7db83-126">Verify hello installation by checking npm list output text for `mysql2@1.3.5`.</span></span>

### <a name="mac-os"></a><span data-ttu-id="7db83-127">**Mac OS**</span><span class="sxs-lookup"><span data-stu-id="7db83-127">**Mac OS**</span></span>
1. <span data-ttu-id="7db83-128">Ange följande kommandon tooinstall hello **brew**, ett enkelt att använda package manager för Mac OS X och **Node.js**.</span><span class="sxs-lookup"><span data-stu-id="7db83-128">Enter hello following commands tooinstall **brew**, an easy-to-use package manager for Mac OS X and **Node.js**.</span></span>

   ```bash
   ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   brew install node
   ```
2. <span data-ttu-id="7db83-129">Kör följande kommandon toomake hello en projektmapp `mysqlnodejs` och installera hello mysql2 paketet till mappen.</span><span class="sxs-lookup"><span data-stu-id="7db83-129">Run hello following commands toomake a project folder `mysqlnodejs` and install hello mysql2 package into that folder.</span></span>

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql2
   npm list
   ```

3. <span data-ttu-id="7db83-130">Kontrollera installationen av hello hello `npm list` utdata text för `mysql2@1.3.6`.</span><span class="sxs-lookup"><span data-stu-id="7db83-130">Verify hello installation by checking hello `npm list` output text for `mysql2@1.3.6`.</span></span> <span data-ttu-id="7db83-131">hello kan versionsnumret variera när nya korrigeringsfiler blir tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="7db83-131">hello version number may vary as new patches are released.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="7db83-132">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="7db83-132">Get connection information</span></span>
<span data-ttu-id="7db83-133">Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="7db83-133">Get hello connection information needed tooconnect toohello Azure Database for MySQL.</span></span> <span data-ttu-id="7db83-134">Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.</span><span class="sxs-lookup"><span data-stu-id="7db83-134">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="7db83-135">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="7db83-135">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="7db83-136">Hello vänster klickar du på **alla resurser**, och sök sedan efter hello-server som du har skapat (till exempel **myserver4demo**).</span><span class="sxs-lookup"><span data-stu-id="7db83-136">In hello left pane, click **All resources**, and then search for hello server you have created (for example, **myserver4demo**).</span></span>
3. <span data-ttu-id="7db83-137">Klicka på servernamnet för hello **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="7db83-137">Click hello server name **myserver4demo**.</span></span>
4. <span data-ttu-id="7db83-138">Välj hello server **egenskaper** sidan.</span><span class="sxs-lookup"><span data-stu-id="7db83-138">Select hello server's **Properties** page.</span></span> <span data-ttu-id="7db83-139">Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.</span><span class="sxs-lookup"><span data-stu-id="7db83-139">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="7db83-140">![Azure Database för MySQL – inloggning för serveradministratör](./media/connect-nodejs/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="7db83-140">![Azure Database for MySQL - Server Admin Login](./media/connect-nodejs/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="7db83-141">Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet och, om nödvändigt återställa hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="7db83-141">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="running-hello-javascript-code-in-nodejs"></a><span data-ttu-id="7db83-142">Kör hello JavaScript-kod i Node.js</span><span class="sxs-lookup"><span data-stu-id="7db83-142">Running hello JavaScript code in Node.js</span></span>
1. <span data-ttu-id="7db83-143">Klistra in hello JavaScript-kod i textfiler och spara i en projektmapp med filen tillägget .js, till exempel C:\nodejsmysql\createtable.js eller /home/username/nodejsmysql/createtable.js</span><span class="sxs-lookup"><span data-stu-id="7db83-143">Paste hello JavaScript code into text files, and save into a project folder with file extension .js, such as C:\nodejsmysql\createtable.js or /home/username/nodejsmysql/createtable.js</span></span>
2. <span data-ttu-id="7db83-144">Starta Kommandotolken hello eller bash shell.</span><span class="sxs-lookup"><span data-stu-id="7db83-144">Launch hello command prompt or bash shell.</span></span> <span data-ttu-id="7db83-145">Ändra katalog till din projektmapp, till exempel `cd nodejsmysql`.</span><span class="sxs-lookup"><span data-stu-id="7db83-145">Change directory into your project folder `cd nodejsmysql`.</span></span>
3. <span data-ttu-id="7db83-146">toorun hello programmet skriver hello nod kommandot följt av hello filnamn, som `node createtable.js`.</span><span class="sxs-lookup"><span data-stu-id="7db83-146">toorun hello application, type hello node command followed by hello file name, such as `node createtable.js`.</span></span>
4. <span data-ttu-id="7db83-147">I Windows, om hello nod programmet inte finns i din miljö variabeln sökväg måste toouse hello fullständig sökväg toolaunch hello noden program som`"C:\Program Files\nodejs\node.exe" createtable.js`</span><span class="sxs-lookup"><span data-stu-id="7db83-147">On Windows, if hello node application is not in your environment variable path, you may need toouse hello full path toolaunch hello node application, such as `"C:\Program Files\nodejs\node.exe" createtable.js`</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="7db83-148">Ansluta, skapa tabell och infoga data</span><span class="sxs-lookup"><span data-stu-id="7db83-148">Connect, create table, and insert data</span></span>
<span data-ttu-id="7db83-149">Använd hello följande kod tooconnect och läsa in hello data med hjälp av **CREATE TABLE** och **INSERT INTO** SQL-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="7db83-149">Use hello following code tooconnect and load hello data using **CREATE TABLE** and  **INSERT INTO** SQL statements.</span></span>

<span data-ttu-id="7db83-150">Hej [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metod är att använda toointerface med hello MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="7db83-150">hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) method is used toointerface with hello MySQL server.</span></span> <span data-ttu-id="7db83-151">Hej [connect()](https://github.com/mysqljs/mysql#establishing-connections) funktionen är används tooestablish hello anslutning toohello server.</span><span class="sxs-lookup"><span data-stu-id="7db83-151">hello [connect()](https://github.com/mysqljs/mysql#establishing-connections) function is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="7db83-152">Hej [query()](https://github.com/mysqljs/mysql#performing-queries) funktionen är används tooexecute hello SQL-fråga mot MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="7db83-152">hello [query()](https://github.com/mysqljs/mysql#performing-queries) function is used tooexecute hello SQL query against MySQL database.</span></span> 

<span data-ttu-id="7db83-153">Ersätt hello `host`, `user`, `password`, och `database` parametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="7db83-153">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

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

## <a name="read-data"></a><span data-ttu-id="7db83-154">Läsa data</span><span class="sxs-lookup"><span data-stu-id="7db83-154">Read data</span></span>
<span data-ttu-id="7db83-155">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **Välj** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="7db83-155">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="7db83-156">Hej [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metod är att använda toointerface med hello MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="7db83-156">hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) method is used toointerface with hello MySQL server.</span></span> <span data-ttu-id="7db83-157">Hej [connect()](https://github.com/mysqljs/mysql#establishing-connections) metoden är används tooestablish hello anslutning toohello server.</span><span class="sxs-lookup"><span data-stu-id="7db83-157">hello [connect()](https://github.com/mysqljs/mysql#establishing-connections) method is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="7db83-158">Hej [query()](https://github.com/mysqljs/mysql#performing-queries) metod är att använda tooexecute hello SQL-fråga mot MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="7db83-158">hello [query()](https://github.com/mysqljs/mysql#performing-queries) method is used tooexecute hello SQL query against MySQL database.</span></span> <span data-ttu-id="7db83-159">hello resultat matrisen är används toohold hello resultaten av hello-frågan.</span><span class="sxs-lookup"><span data-stu-id="7db83-159">hello results array is used toohold hello results of hello query.</span></span>

<span data-ttu-id="7db83-160">Ersätt hello `host`, `user`, `password`, och `database` parametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="7db83-160">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

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

## <a name="update-data"></a><span data-ttu-id="7db83-161">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="7db83-161">Update data</span></span>
<span data-ttu-id="7db83-162">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **uppdatering** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="7db83-162">Use hello following code tooconnect and read hello data using a **UPDATE** SQL statement.</span></span> 

<span data-ttu-id="7db83-163">Hej [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metod är att använda toointerface med hello MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="7db83-163">hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) method is used toointerface with hello MySQL server.</span></span> <span data-ttu-id="7db83-164">Hej [connect()](https://github.com/mysqljs/mysql#establishing-connections) metoden är används tooestablish hello anslutning toohello server.</span><span class="sxs-lookup"><span data-stu-id="7db83-164">hello [connect()](https://github.com/mysqljs/mysql#establishing-connections) method is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="7db83-165">Hej [query()](https://github.com/mysqljs/mysql#performing-queries) metod är att använda tooexecute hello SQL-fråga mot MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="7db83-165">hello [query()](https://github.com/mysqljs/mysql#performing-queries) method is used tooexecute hello SQL query against MySQL database.</span></span> 

<span data-ttu-id="7db83-166">Ersätt hello `host`, `user`, `password`, och `database` parametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="7db83-166">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

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

## <a name="delete-data"></a><span data-ttu-id="7db83-167">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="7db83-167">Delete data</span></span>
<span data-ttu-id="7db83-168">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **ta bort** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="7db83-168">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="7db83-169">Hej [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) metod är att använda toointerface med hello MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="7db83-169">hello [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) method is used toointerface with hello MySQL server.</span></span> <span data-ttu-id="7db83-170">Hej [connect()](https://github.com/mysqljs/mysql#establishing-connections) metoden är används tooestablish hello anslutning toohello server.</span><span class="sxs-lookup"><span data-stu-id="7db83-170">hello [connect()](https://github.com/mysqljs/mysql#establishing-connections) method is used tooestablish hello connection toohello server.</span></span> <span data-ttu-id="7db83-171">Hej [query()](https://github.com/mysqljs/mysql#performing-queries) metod är att använda tooexecute hello SQL-fråga mot MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="7db83-171">hello [query()](https://github.com/mysqljs/mysql#performing-queries) method is used tooexecute hello SQL query against MySQL database.</span></span> 

<span data-ttu-id="7db83-172">Ersätt hello `host`, `user`, `password`, och `database` parametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="7db83-172">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="7db83-173">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7db83-173">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="7db83-174">Migrera din databas med Exportera och importera</span><span class="sxs-lookup"><span data-stu-id="7db83-174">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
