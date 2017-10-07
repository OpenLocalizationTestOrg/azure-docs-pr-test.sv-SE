---
title: "Ansluta tooAzure databas för MySQL från Python | Microsoft Docs"
description: "Denna Snabbstart innehåller flera Python kodexempel som du kan använda tooconnect och fråga data från Azure-databas för MySQL."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.devlang: python
ms.topic: hero-article
ms.date: 07/12/2017
ms.openlocfilehash: 9df5211adcab886a502fd138347aed8fb587cd5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-python-tooconnect-and-query-data"></a><span data-ttu-id="55d79-103">Azure-databas för MySQL: Använd Python tooconnect och fråga data</span><span class="sxs-lookup"><span data-stu-id="55d79-103">Azure Database for MySQL: Use Python tooconnect and query data</span></span>
<span data-ttu-id="55d79-104">Den här snabbstarten visar hur toouse [Python](https://python.org) tooconnect tooan Azure för MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="55d79-104">This quickstart demonstrates how toouse [Python](https://python.org) tooconnect tooan Azure Database for MySQL.</span></span> <span data-ttu-id="55d79-105">SQL instruktioner tooquery, infoga, uppdatera och ta bort data används i hello databasen från Mac OS x, Ubuntu Linux och Windows-plattformar.</span><span class="sxs-lookup"><span data-stu-id="55d79-105">It uses SQL statements tooquery, insert, update, and delete data in hello database from Mac OS, Ubuntu Linux, and Windows platforms.</span></span> <span data-ttu-id="55d79-106">hello stegen i den här artikeln förutsätter att du är bekant med att utveckla använder Python och nya tooworking med Azure-databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="55d79-106">hello steps in this article assume that you are familiar with developing using Python and are new tooworking with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="55d79-107">Krav</span><span class="sxs-lookup"><span data-stu-id="55d79-107">Prerequisites</span></span>
<span data-ttu-id="55d79-108">Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="55d79-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="55d79-109">Skapa en Azure Database för MySQL med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="55d79-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="55d79-110">Skapa en Azure Database för MySQL-server med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="55d79-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-python-and-hello-mysql-connector"></a><span data-ttu-id="55d79-111">Installera Python och hello MySQL-koppling</span><span class="sxs-lookup"><span data-stu-id="55d79-111">Install Python and hello MySQL connector</span></span>
<span data-ttu-id="55d79-112">Installera [Python](https://www.python.org/downloads/) och hello [MySQL-koppling för Python](https://dev.mysql.com/downloads/connector/python/) på din egen dator.</span><span class="sxs-lookup"><span data-stu-id="55d79-112">Install [Python](https://www.python.org/downloads/) and hello [MySQL connector for Python](https://dev.mysql.com/downloads/connector/python/) on your own machine.</span></span> <span data-ttu-id="55d79-113">Beroende på din plattform gör hello:</span><span class="sxs-lookup"><span data-stu-id="55d79-113">Depending on your platform, follow hello steps:</span></span>

### <a name="windows"></a><span data-ttu-id="55d79-114">Windows</span><span class="sxs-lookup"><span data-stu-id="55d79-114">Windows</span></span>
1. <span data-ttu-id="55d79-115">Hämta och installera Python 2.7 från [python.org](https://www.python.org/downloads/windows/).</span><span class="sxs-lookup"><span data-stu-id="55d79-115">Download and Install Python 2.7 from [python.org](https://www.python.org/downloads/windows/).</span></span> 
2. <span data-ttu-id="55d79-116">Kontrollera hello Python-installation genom att starta hello kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="55d79-116">Check hello Python installation by launching hello command prompt.</span></span> <span data-ttu-id="55d79-117">Kör kommandot hello `C:\python27\python.exe -V` med hello versaler V växeln toosee hello versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="55d79-117">Run hello command `C:\python27\python.exe -V` using hello uppercase V switch toosee hello version number.</span></span>
3. <span data-ttu-id="55d79-118">Installera hello Python connector för MySQL från [mysql.com](https://dev.mysql.com/downloads/connector/python/) motsvarande tooyour version av Python.</span><span class="sxs-lookup"><span data-stu-id="55d79-118">Install hello Python connector for MySQL from [mysql.com](https://dev.mysql.com/downloads/connector/python/) corresponding tooyour version of Python.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="55d79-119">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="55d79-119">Linux (Ubuntu)</span></span>
1. <span data-ttu-id="55d79-120">Linux (Ubuntu) installeras Python vanligtvis som en del av hello standardinstallation.</span><span class="sxs-lookup"><span data-stu-id="55d79-120">In Linux (Ubuntu), Python is typically installed as part of hello default installation.</span></span>
2. <span data-ttu-id="55d79-121">Kontrollera hello Python-installation genom att starta hello bash-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="55d79-121">Check hello Python installation by launching hello bash shell.</span></span> <span data-ttu-id="55d79-122">Kör kommandot hello `python -V` med hello versaler V växeln toosee hello versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="55d79-122">Run hello command `python -V` using hello uppercase V switch toosee hello version number.</span></span>
3. <span data-ttu-id="55d79-123">Kontrollera hello PIP installationen genom att köra hello `pip show pip -V` kommandot toosee hello-versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="55d79-123">Check hello PIP installation by running hello `pip show pip -V` command toosee hello version number.</span></span> 
4. <span data-ttu-id="55d79-124">PIP kan ingå i vissa versioner av Python.</span><span class="sxs-lookup"><span data-stu-id="55d79-124">PIP may be included in some versions of Python.</span></span> <span data-ttu-id="55d79-125">Om PIP inte har installerats kan du installera hello [PIP] (https://pip.pypa.io/en/stable/installing/) paketet genom att köra kommandot `sudo apt-get install python-pip`.</span><span class="sxs-lookup"><span data-stu-id="55d79-125">If PIP is not installed, you may install hello [PIP] (https://pip.pypa.io/en/stable/installing/) package, by running command `sudo apt-get install python-pip`.</span></span>
5. <span data-ttu-id="55d79-126">Uppdatera PIP toohello senaste versionen genom att köra hello `pip install -U pip` kommando.</span><span class="sxs-lookup"><span data-stu-id="55d79-126">Update PIP toohello latest version, by running hello `pip install -U pip` command.</span></span>
6. <span data-ttu-id="55d79-127">Installera hello MySQL connector för Python och dess beroenden med hello PIP-kommando:</span><span class="sxs-lookup"><span data-stu-id="55d79-127">Install hello MySQL connector for Python, and its dependencies by using hello PIP command:</span></span>

   ```bash
   sudo pip install mysql-connector-python-rf
   ```
 
### <a name="macos"></a><span data-ttu-id="55d79-128">MacOS</span><span class="sxs-lookup"><span data-stu-id="55d79-128">MacOS</span></span>
1. <span data-ttu-id="55d79-129">Mac OS x installeras Python vanligtvis som en del av hello standard OS-installationen.</span><span class="sxs-lookup"><span data-stu-id="55d79-129">In Mac OS, Python is typically installed as part of hello default OS installation.</span></span>
2. <span data-ttu-id="55d79-130">Kontrollera hello Python-installation genom att starta hello bash-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="55d79-130">Check hello Python installation by launching hello bash shell.</span></span> <span data-ttu-id="55d79-131">Kör kommandot hello `python -V` med hello versaler V växeln toosee hello versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="55d79-131">Run hello command `python -V` using hello uppercase V switch toosee hello version number.</span></span>
3. <span data-ttu-id="55d79-132">Kontrollera hello PIP installationen genom att köra hello `pip show pip -V` kommandot toosee hello-versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="55d79-132">Check hello PIP installation by running hello `pip show pip -V` command toosee hello version number.</span></span>
4. <span data-ttu-id="55d79-133">PIP kan ingå i vissa versioner av Python.</span><span class="sxs-lookup"><span data-stu-id="55d79-133">PIP may be included in some versions of Python.</span></span> <span data-ttu-id="55d79-134">Om PIP inte har installerats kan du installera hello [PIP](https://pip.pypa.io/en/stable/installing/) paketet.</span><span class="sxs-lookup"><span data-stu-id="55d79-134">If PIP is not installed, you may install hello [PIP](https://pip.pypa.io/en/stable/installing/) package.</span></span>
5. <span data-ttu-id="55d79-135">Uppdatera PIP toohello senaste versionen genom att köra hello `pip install -U pip` kommando.</span><span class="sxs-lookup"><span data-stu-id="55d79-135">Update PIP toohello latest version, by running hello `pip install -U pip` command.</span></span>
6. <span data-ttu-id="55d79-136">Installera hello MySQL connector för Python och dess beroenden med hello PIP-kommando:</span><span class="sxs-lookup"><span data-stu-id="55d79-136">Install hello MySQL connector for Python, and its dependencies by using hello PIP command:</span></span>

   ```bash
   pip install mysql-connector-python-rf
   ```

## <a name="get-connection-information"></a><span data-ttu-id="55d79-137">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="55d79-137">Get connection information</span></span>
<span data-ttu-id="55d79-138">Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="55d79-138">Get hello connection information needed tooconnect toohello Azure Database for MySQL.</span></span> <span data-ttu-id="55d79-139">Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.</span><span class="sxs-lookup"><span data-stu-id="55d79-139">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="55d79-140">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="55d79-140">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="55d79-141">Hello vänstra menyn i Azure-portalen klickar du på **alla resurser** och Sök efter hello-server som du har creased, som **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="55d79-141">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have creased, such as **myserver4demo**.</span></span>
3. <span data-ttu-id="55d79-142">Klicka på servernamnet för hello **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="55d79-142">Click hello server name **myserver4demo**.</span></span>
4. <span data-ttu-id="55d79-143">Välj hello server **egenskaper** sidan.</span><span class="sxs-lookup"><span data-stu-id="55d79-143">Select hello server's **Properties** page.</span></span> <span data-ttu-id="55d79-144">Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.</span><span class="sxs-lookup"><span data-stu-id="55d79-144">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="55d79-145">![Azure Database för MySQL – inloggning för serveradministratör](./media/connect-python/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="55d79-145">![Azure Database for MySQL - Server Admin Login](./media/connect-python/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="55d79-146">Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet och, om nödvändigt återställa hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="55d79-146">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>
   

## <a name="run-python-code"></a><span data-ttu-id="55d79-147">Köra Python-kod</span><span class="sxs-lookup"><span data-stu-id="55d79-147">Run Python Code</span></span>
- <span data-ttu-id="55d79-148">Klistra in hello kod i en textfil och spara hello-filen till en projektmapp med filen tillägget .py, till exempel C:\pythonmysql\createtable.py eller /home/username/pythonmysql/createtable.py</span><span class="sxs-lookup"><span data-stu-id="55d79-148">Paste hello code into a text file, and save hello file into a project folder with file extension .py, such as C:\pythonmysql\createtable.py or /home/username/pythonmysql/createtable.py</span></span>
- <span data-ttu-id="55d79-149">toorun hello kod, starta Kommandotolken hello eller bash shell.</span><span class="sxs-lookup"><span data-stu-id="55d79-149">toorun hello code, launch hello command prompt or bash shell.</span></span> <span data-ttu-id="55d79-150">Ändra katalog till din projektmapp, till exempel `cd pythonmysql`.</span><span class="sxs-lookup"><span data-stu-id="55d79-150">Change directory into your project folder `cd pythonmysql`.</span></span> <span data-ttu-id="55d79-151">Skriv hello python kommando följt av namnet på filen hello `python createtable.py` toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="55d79-151">Then type hello python command followed by hello file name `python createtable.py` toorun hello application.</span></span> <span data-ttu-id="55d79-152">Om python.exe inte hittas på hello Windows-Operativsystemet, kan du ange hello fullständig sökväg toohello körbara eller Lägg till hello Python sökväg till hello path-miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="55d79-152">On hello Windows OS, if python.exe is not found, you may provide hello full path toohello executable, or add hello Python path into hello path environment variable.</span></span> `C:\python27\python.exe createtable.py`

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="55d79-153">Ansluta, skapa tabell och infoga data</span><span class="sxs-lookup"><span data-stu-id="55d79-153">Connect, create table, and insert data</span></span>
<span data-ttu-id="55d79-154">Använd hello följande code tooconnect toohello server, skapa en tabell och läsa in hello data med hjälp av en **infoga** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="55d79-154">Use hello following code tooconnect toohello server, create a table, and load hello data using an **INSERT** SQL statement.</span></span> 

<span data-ttu-id="55d79-155">I hello kod har hello mysql.connector biblioteket importerats.</span><span class="sxs-lookup"><span data-stu-id="55d79-155">In hello code, hello mysql.connector library is imported.</span></span> <span data-ttu-id="55d79-156">Hej [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) funktionen är används tooconnect tooAzure databasen för MySQL med hello [anslutningsargument](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) i hello config samling.</span><span class="sxs-lookup"><span data-stu-id="55d79-156">hello [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) function is used tooconnect tooAzure Database for MySQL using hello [connection arguments](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in hello config collection.</span></span> <span data-ttu-id="55d79-157">hello koden använder en markör på hello-anslutning och [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metoden Kör hello SQL-fråga mot MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="55d79-157">hello code uses a cursor on hello connection, and [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) method executes hello SQL query against MySQL database.</span></span> 

<span data-ttu-id="55d79-158">Ersätt hello `host`, `user`, `password`, och `database` parametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="55d79-158">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from hello portal
config = {
  'host':'myserver4demo.mysql.database.azure.com',
  'user':'myadmin@myserver4demo',
  'password':'yourpassword',
  'database':'quickstartdb'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with hello user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Drop previous table of same name if one exists
  cursor.execute("DROP TABLE IF EXISTS inventory;")
  print("Finished dropping table (if existed).")

  # Create table
  cursor.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
  print("Finished creating table.")

  # Insert some data into table
  cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("banana", 150))
  print("Inserted",cursor.rowcount,"row(s) of data.")
  cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("orange", 154))
  print("Inserted",cursor.rowcount,"row(s) of data.")
  cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("apple", 100))
  print("Inserted",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="read-data"></a><span data-ttu-id="55d79-159">Läsa data</span><span class="sxs-lookup"><span data-stu-id="55d79-159">Read data</span></span>
<span data-ttu-id="55d79-160">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **Välj** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="55d79-160">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="55d79-161">I hello kod har hello mysql.connector biblioteket importerats.</span><span class="sxs-lookup"><span data-stu-id="55d79-161">In hello code, hello mysql.connector library is imported.</span></span> <span data-ttu-id="55d79-162">Hej [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) funktionen är används tooconnect tooAzure databasen för MySQL med hello [anslutningsargument](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) i hello config samling.</span><span class="sxs-lookup"><span data-stu-id="55d79-162">hello [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) function is used tooconnect tooAzure Database for MySQL using hello [connection arguments](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in hello config collection.</span></span> <span data-ttu-id="55d79-163">hello koden använder en markör på hello-anslutning och [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metoden Kör hello SQL-instruktion mot MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="55d79-163">hello code uses a cursor on hello connection, and [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) method executes hello SQL statement against MySQL database.</span></span> <span data-ttu-id="55d79-164">hello datarader läses med hello [fetchall()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-fetchall.html) metod.</span><span class="sxs-lookup"><span data-stu-id="55d79-164">hello data rows are read using hello [fetchall()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-fetchall.html) method.</span></span> <span data-ttu-id="55d79-165">hello resultatet sparas i en samling rad och en för iterator är används tooloop över hello rader.</span><span class="sxs-lookup"><span data-stu-id="55d79-165">hello result set is kept in a collection row and a for iterator is used tooloop over hello rows.</span></span>

<span data-ttu-id="55d79-166">Ersätt hello `host`, `user`, `password`, och `database` parametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="55d79-166">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from hello portal
config = {
  'host':'myserver4demo.mysql.database.azure.com',
  'user':'myadmin@myserver4demo',
  'password':'yourpassword',
  'database':'quickstartdb'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with hello user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Read data
  cursor.execute("SELECT * FROM inventory;")
  rows = cursor.fetchall()
  print("Read",cursor.rowcount,"row(s) of data.")

  # Print all rows
  for row in rows:
    print("Data row = (%s, %s, %s)" %(str(row[0]), str(row[1]), str(row[2])))

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="update-data"></a><span data-ttu-id="55d79-167">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="55d79-167">Update data</span></span>
<span data-ttu-id="55d79-168">Använd hello följande kod tooconnect och uppdatera hello data med hjälp av en **uppdatera** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="55d79-168">Use hello following code tooconnect and update hello data using a **UPDATE** SQL statement.</span></span> 

<span data-ttu-id="55d79-169">I hello kod har hello mysql.connector biblioteket importerats.</span><span class="sxs-lookup"><span data-stu-id="55d79-169">In hello code, hello mysql.connector library is imported.</span></span>  <span data-ttu-id="55d79-170">Hej [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) funktionen är används tooconnect tooAzure databasen för MySQL med hello [anslutningsargument](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) i hello config samling.</span><span class="sxs-lookup"><span data-stu-id="55d79-170">hello [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) function is used tooconnect tooAzure Database for MySQL using hello [connection arguments](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in hello config collection.</span></span> <span data-ttu-id="55d79-171">hello koden använder en markör på hello-anslutning och [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metoden Kör hello SQL-instruktion mot MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="55d79-171">hello code uses a cursor on hello connection, and [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) method executes hello SQL statement against MySQL database.</span></span> 

<span data-ttu-id="55d79-172">Ersätt hello `host`, `user`, `password`, och `database` parametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="55d79-172">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from hello portal
config = {
  'host':'myserver4demo.mysql.database.azure.com',
  'user':'myadmin@myserver4demo',
  'password':'yourpassword',
  'database':'quickstartdb'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with hello user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Update a data row in hello table
  cursor.execute("UPDATE inventory SET quantity = %s WHERE name = %s;", (200, "banana"))
  print("Updated",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="delete-data"></a><span data-ttu-id="55d79-173">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="55d79-173">Delete data</span></span>
<span data-ttu-id="55d79-174">Använd hello följande kod tooconnect och ta bort data med hjälp av en **ta bort** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="55d79-174">Use hello following code tooconnect and remove data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="55d79-175">I hello kod har hello mysql.connector biblioteket importerats.</span><span class="sxs-lookup"><span data-stu-id="55d79-175">In hello code, hello mysql.connector library is imported.</span></span>  <span data-ttu-id="55d79-176">Hej [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) funktionen är används tooconnect tooAzure databasen för MySQL med hello [anslutningsargument](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) i hello config samling.</span><span class="sxs-lookup"><span data-stu-id="55d79-176">hello [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) function is used tooconnect tooAzure Database for MySQL using hello [connection arguments](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in hello config collection.</span></span> <span data-ttu-id="55d79-177">hello koden använder en markör på hello-anslutning och [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metoden Kör hello SQL-fråga mot MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="55d79-177">hello code uses a cursor on hello connection, and [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) method executes hello SQL query against MySQL database.</span></span> 

<span data-ttu-id="55d79-178">Ersätt hello `host`, `user`, `password`, och `database` parametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="55d79-178">Replace hello `host`, `user`, `password`, and `database` parameters with hello values that you specified when you created hello server and database.</span></span>

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from hello portal
config = {
  'host':'myserver4demo.mysql.database.azure.com',
  'user':'myadmin@myserver4demo',
  'password':'yourpassword',
  'database':'quickstartdb'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established.")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with hello user name or password.")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist.")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Delete a data row in hello table
  cursor.execute("DELETE FROM inventory WHERE name=%(param1)s;", {'param1':"orange"})
  print("Deleted",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="next-steps"></a><span data-ttu-id="55d79-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="55d79-179">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="55d79-180">Migrera din databas med Exportera och importera</span><span class="sxs-lookup"><span data-stu-id="55d79-180">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
