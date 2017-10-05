---
title: "Ansluta till Azure Database för MySQL från Python | Microsoft Docs"
description: "I den här snabbstarten finns flera kodexempel i Python som du kan använda för att ansluta till och fråga efter data från Azure Database för MySQL."
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
ms.openlocfilehash: 4c3a2e65b83fab6fe5b8b7778782a747bb5e9cf9
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="azure-database-for-mysql-use-python-to-connect-and-query-data"></a><span data-ttu-id="c520c-103">Azure Database för MySQL: Använda Python för att ansluta och fråga efter data</span><span class="sxs-lookup"><span data-stu-id="c520c-103">Azure Database for MySQL: Use Python to connect and query data</span></span>
<span data-ttu-id="c520c-104">Den här snabbstarten visar hur du använder [Python](https://python.org) för att ansluta till en Azure Database för MySQL.</span><span class="sxs-lookup"><span data-stu-id="c520c-104">This quickstart demonstrates how to use [Python](https://python.org) to connect to an Azure Database for MySQL.</span></span> <span data-ttu-id="c520c-105">SQL-instruktioner används för att fråga, infoga, uppdatera och ta bort data i databasen i Mac OS, Ubuntu Linux och Windows-plattformar.</span><span class="sxs-lookup"><span data-stu-id="c520c-105">It uses SQL statements to query, insert, update, and delete data in the database from Mac OS, Ubuntu Linux, and Windows platforms.</span></span> <span data-ttu-id="c520c-106">I den här artikeln förutsätter vi att du har kunskaper om Python och att du inte har arbetat med Azure Database för MySQL tidigare.</span><span class="sxs-lookup"><span data-stu-id="c520c-106">The steps in this article assume that you are familiar with developing using Python and are new to working with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c520c-107">Krav</span><span class="sxs-lookup"><span data-stu-id="c520c-107">Prerequisites</span></span>
<span data-ttu-id="c520c-108">I den här snabbstarten används de resurser som skapades i någon av följande guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="c520c-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="c520c-109">Skapa en Azure Database för MySQL med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c520c-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="c520c-110">Skapa en Azure Database för MySQL-server med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c520c-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-python-and-the-mysql-connector"></a><span data-ttu-id="c520c-111">Installera Python och MySQL Connector</span><span class="sxs-lookup"><span data-stu-id="c520c-111">Install Python and the MySQL connector</span></span>
<span data-ttu-id="c520c-112">Installera [Python](https://www.python.org/downloads/) och [MySQL Connector för Python](https://dev.mysql.com/downloads/connector/python/) på din egen dator.</span><span class="sxs-lookup"><span data-stu-id="c520c-112">Install [Python](https://www.python.org/downloads/) and the [MySQL connector for Python](https://dev.mysql.com/downloads/connector/python/) on your own machine.</span></span> <span data-ttu-id="c520c-113">Följ instruktionerna för din plattform:</span><span class="sxs-lookup"><span data-stu-id="c520c-113">Depending on your platform, follow the steps:</span></span>

### <a name="windows"></a><span data-ttu-id="c520c-114">Windows</span><span class="sxs-lookup"><span data-stu-id="c520c-114">Windows</span></span>
1. <span data-ttu-id="c520c-115">Hämta och installera Python 2.7 från [python.org](https://www.python.org/downloads/windows/).</span><span class="sxs-lookup"><span data-stu-id="c520c-115">Download and Install Python 2.7 from [python.org](https://www.python.org/downloads/windows/).</span></span> 
2. <span data-ttu-id="c520c-116">Kontrollera Python-installationen genom att starta Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="c520c-116">Check the Python installation by launching the command prompt.</span></span> <span data-ttu-id="c520c-117">Kör kommandot `C:\python27\python.exe -V` med -V (versal) för att visa versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="c520c-117">Run the command `C:\python27\python.exe -V` using the uppercase V switch to see the version number.</span></span>
3. <span data-ttu-id="c520c-118">Installera den Python Connector för MySQL som motsvarar din version av Python från [mysql.com](https://dev.mysql.com/downloads/connector/python/).</span><span class="sxs-lookup"><span data-stu-id="c520c-118">Install the Python connector for MySQL from [mysql.com](https://dev.mysql.com/downloads/connector/python/) corresponding to your version of Python.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="c520c-119">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="c520c-119">Linux (Ubuntu)</span></span>
1. <span data-ttu-id="c520c-120">I Linux (Ubuntu) installeras vanligtvis Python som en del av standardinstallationen.</span><span class="sxs-lookup"><span data-stu-id="c520c-120">In Linux (Ubuntu), Python is typically installed as part of the default installation.</span></span>
2. <span data-ttu-id="c520c-121">Kontrollera Python-installationen genom att starta Bash-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="c520c-121">Check the Python installation by launching the bash shell.</span></span> <span data-ttu-id="c520c-122">Kör kommandot `python -V` med -V (versal) för att visa versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="c520c-122">Run the command `python -V` using the uppercase V switch to see the version number.</span></span>
3. <span data-ttu-id="c520c-123">Kontrollera PIP-installationen genom att köra kommandot `pip show pip -V` och se versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="c520c-123">Check the PIP installation by running the `pip show pip -V` command to see the version number.</span></span> 
4. <span data-ttu-id="c520c-124">PIP kan ingå i vissa versioner av Python.</span><span class="sxs-lookup"><span data-stu-id="c520c-124">PIP may be included in some versions of Python.</span></span> <span data-ttu-id="c520c-125">Om PIP inte har installerats kan du installera [PIP]-paketet (https://pip.pypa.io/en/stable/installing/) genom att köra kommandot `sudo apt-get install python-pip`.</span><span class="sxs-lookup"><span data-stu-id="c520c-125">If PIP is not installed, you may install the [PIP] (https://pip.pypa.io/en/stable/installing/) package, by running command `sudo apt-get install python-pip`.</span></span>
5. <span data-ttu-id="c520c-126">Uppdatera PIP till den senaste versionen genom att köra kommandot `pip install -U pip`.</span><span class="sxs-lookup"><span data-stu-id="c520c-126">Update PIP to the latest version, by running the `pip install -U pip` command.</span></span>
6. <span data-ttu-id="c520c-127">Installera MySQL Connector för Python och dess beroenden med hjälp av PIP-kommandot:</span><span class="sxs-lookup"><span data-stu-id="c520c-127">Install the MySQL connector for Python, and its dependencies by using the PIP command:</span></span>

   ```bash
   sudo pip install mysql-connector-python-rf
   ```
 
### <a name="macos"></a><span data-ttu-id="c520c-128">MacOS</span><span class="sxs-lookup"><span data-stu-id="c520c-128">MacOS</span></span>
1. <span data-ttu-id="c520c-129">I Mac OS installeras vanligtvis Python som en del av standardinstallationen av operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="c520c-129">In Mac OS, Python is typically installed as part of the default OS installation.</span></span>
2. <span data-ttu-id="c520c-130">Kontrollera Python-installationen genom att starta Bash-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="c520c-130">Check the Python installation by launching the bash shell.</span></span> <span data-ttu-id="c520c-131">Kör kommandot `python -V` med -V (versal) för att visa versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="c520c-131">Run the command `python -V` using the uppercase V switch to see the version number.</span></span>
3. <span data-ttu-id="c520c-132">Kontrollera PIP-installationen genom att köra kommandot `pip show pip -V` och se versionsnumret.</span><span class="sxs-lookup"><span data-stu-id="c520c-132">Check the PIP installation by running the `pip show pip -V` command to see the version number.</span></span>
4. <span data-ttu-id="c520c-133">PIP kan ingå i vissa versioner av Python.</span><span class="sxs-lookup"><span data-stu-id="c520c-133">PIP may be included in some versions of Python.</span></span> <span data-ttu-id="c520c-134">Om PIP inte har installerats kan du installera [PIP](https://pip.pypa.io/en/stable/installing/)-paketet.</span><span class="sxs-lookup"><span data-stu-id="c520c-134">If PIP is not installed, you may install the [PIP](https://pip.pypa.io/en/stable/installing/) package.</span></span>
5. <span data-ttu-id="c520c-135">Uppdatera PIP till den senaste versionen genom att köra kommandot `pip install -U pip`.</span><span class="sxs-lookup"><span data-stu-id="c520c-135">Update PIP to the latest version, by running the `pip install -U pip` command.</span></span>
6. <span data-ttu-id="c520c-136">Installera MySQL Connector för Python och dess beroenden med hjälp av PIP-kommandot:</span><span class="sxs-lookup"><span data-stu-id="c520c-136">Install the MySQL connector for Python, and its dependencies by using the PIP command:</span></span>

   ```bash
   pip install mysql-connector-python-rf
   ```

## <a name="get-connection-information"></a><span data-ttu-id="c520c-137">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="c520c-137">Get connection information</span></span>
<span data-ttu-id="c520c-138">Skaffa den information som du behöver för att ansluta till Azure Database för MySQL.</span><span class="sxs-lookup"><span data-stu-id="c520c-138">Get the connection information needed to connect to the Azure Database for MySQL.</span></span> <span data-ttu-id="c520c-139">Du behöver det fullständiga servernamnet och inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="c520c-139">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="c520c-140">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c520c-140">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="c520c-141">På den vänstra menyn i Azure Portal klickar du på **Alla resurser** och söker efter den server som du nyss skapade, till exempel **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="c520c-141">From the left-hand menu in Azure portal, click **All resources** and search for the server you have creased, such as **myserver4demo**.</span></span>
3. <span data-ttu-id="c520c-142">Klicka på servernamnet **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="c520c-142">Click the server name **myserver4demo**.</span></span>
4. <span data-ttu-id="c520c-143">Välj sidan **Egenskaper** för servern.</span><span class="sxs-lookup"><span data-stu-id="c520c-143">Select the server's **Properties** page.</span></span> <span data-ttu-id="c520c-144">Anteckna **servernamn** och **inloggningsnamnet för serveradministratören**.</span><span class="sxs-lookup"><span data-stu-id="c520c-144">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="c520c-145">![Azure Database för MySQL – inloggning för serveradministratör](./media/connect-python/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="c520c-145">![Azure Database for MySQL - Server Admin Login](./media/connect-python/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="c520c-146">Om du glömmer inloggningsinformationen för servern öppnar du sidan **Översikt** för att se inloggningsnamnet för serveradministratören. Om det behövs kan du återställa lösenordet.</span><span class="sxs-lookup"><span data-stu-id="c520c-146">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>
   

## <a name="run-python-code"></a><span data-ttu-id="c520c-147">Köra Python-kod</span><span class="sxs-lookup"><span data-stu-id="c520c-147">Run Python Code</span></span>
- <span data-ttu-id="c520c-148">Klistra in koden i en textfil och spara filen till en projektmapp med filnamnstillägget .py, till exempel C:\pythonmysql\createtable.py eller /home/username/pythonmysql/createtable.py</span><span class="sxs-lookup"><span data-stu-id="c520c-148">Paste the code into a text file, and save the file into a project folder with file extension .py, such as C:\pythonmysql\createtable.py or /home/username/pythonmysql/createtable.py</span></span>
- <span data-ttu-id="c520c-149">Om du vill köra koden startar du kommandotolken eller bash-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="c520c-149">To run the code, launch the command prompt or bash shell.</span></span> <span data-ttu-id="c520c-150">Ändra katalog till din projektmapp, till exempel `cd pythonmysql`.</span><span class="sxs-lookup"><span data-stu-id="c520c-150">Change directory into your project folder `cd pythonmysql`.</span></span> <span data-ttu-id="c520c-151">Skriv Python-kommandot följt av filnamnet `python createtable.py` för att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="c520c-151">Then type the python command followed by the file name `python createtable.py` to run the application.</span></span> <span data-ttu-id="c520c-152">Om python.exe inte hittas i Windows kan du ange den fullständiga sökvägen till den körbara filen eller lägga till Python-sökvägen i Path-miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="c520c-152">On the Windows OS, if python.exe is not found, you may provide the full path to the executable, or add the Python path into the path environment variable.</span></span> `C:\python27\python.exe createtable.py`

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="c520c-153">Ansluta, skapa tabell och infoga data</span><span class="sxs-lookup"><span data-stu-id="c520c-153">Connect, create table, and insert data</span></span>
<span data-ttu-id="c520c-154">Använd följande kod för att ansluta till servern, skapa en tabell och läsa in data med hjälp av SQL-instruktionen **INSERT**.</span><span class="sxs-lookup"><span data-stu-id="c520c-154">Use the following code to connect to the server, create a table, and load the data using an **INSERT** SQL statement.</span></span> 

<span data-ttu-id="c520c-155">Biblioteket mysql.connector importeras i koden.</span><span class="sxs-lookup"><span data-stu-id="c520c-155">In the code, the mysql.connector library is imported.</span></span> <span data-ttu-id="c520c-156">Funktionen [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) används för att ansluta till Azure Database för MySQL med [anslutningsargumenten](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) i config-insamlingen.</span><span class="sxs-lookup"><span data-stu-id="c520c-156">The [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) function is used to connect to Azure Database for MySQL using the [connection arguments](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in the config collection.</span></span> <span data-ttu-id="c520c-157">I koden används en cursor i anslutningen, och metoden [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) kör SQL-frågan mot MySQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="c520c-157">The code uses a cursor on the connection, and [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) method executes the SQL query against MySQL database.</span></span> 

<span data-ttu-id="c520c-158">Ersätt parametrarna `host`, `user`, `password` och `database` med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="c520c-158">Replace the `host`, `user`, `password`, and `database` parameters with the values that you specified when you created the server and database.</span></span>

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
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
    print("Something is wrong with the user name or password")
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

## <a name="read-data"></a><span data-ttu-id="c520c-159">Läsa data</span><span class="sxs-lookup"><span data-stu-id="c520c-159">Read data</span></span>
<span data-ttu-id="c520c-160">Använd följande kod för att ansluta och läsa data med en **SELECT**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="c520c-160">Use the following code to connect and read the data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="c520c-161">Biblioteket mysql.connector importeras i koden.</span><span class="sxs-lookup"><span data-stu-id="c520c-161">In the code, the mysql.connector library is imported.</span></span> <span data-ttu-id="c520c-162">Funktionen [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) används för att ansluta till Azure Database för MySQL med [anslutningsargumenten](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) i config-insamlingen.</span><span class="sxs-lookup"><span data-stu-id="c520c-162">The [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) function is used to connect to Azure Database for MySQL using the [connection arguments](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in the config collection.</span></span> <span data-ttu-id="c520c-163">I koden används en cursor i anslutningen, och metoden [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) kör SQL-instruktionen mot MySQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="c520c-163">The code uses a cursor on the connection, and [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) method executes the SQL statement against MySQL database.</span></span> <span data-ttu-id="c520c-164">Datarader läses med hjälp av metoden [fetchall()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-fetchall.html).</span><span class="sxs-lookup"><span data-stu-id="c520c-164">The data rows are read using the [fetchall()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-fetchall.html) method.</span></span> <span data-ttu-id="c520c-165">Resultatuppsättningen sparas i en samlingsrad och en for-iterator används för att loopa igenom raderna.</span><span class="sxs-lookup"><span data-stu-id="c520c-165">The result set is kept in a collection row and a for iterator is used to loop over the rows.</span></span>

<span data-ttu-id="c520c-166">Ersätt parametrarna `host`, `user`, `password` och `database` med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="c520c-166">Replace the `host`, `user`, `password`, and `database` parameters with the values that you specified when you created the server and database.</span></span>

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
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
    print("Something is wrong with the user name or password")
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

## <a name="update-data"></a><span data-ttu-id="c520c-167">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="c520c-167">Update data</span></span>
<span data-ttu-id="c520c-168">Använd följande kod för att ansluta och uppdatera data med en **UPDATE**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="c520c-168">Use the following code to connect and update the data using a **UPDATE** SQL statement.</span></span> 

<span data-ttu-id="c520c-169">Biblioteket mysql.connector importeras i koden.</span><span class="sxs-lookup"><span data-stu-id="c520c-169">In the code, the mysql.connector library is imported.</span></span>  <span data-ttu-id="c520c-170">Funktionen [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) används för att ansluta till Azure Database för MySQL med [anslutningsargumenten](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) i config-insamlingen.</span><span class="sxs-lookup"><span data-stu-id="c520c-170">The [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) function is used to connect to Azure Database for MySQL using the [connection arguments](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in the config collection.</span></span> <span data-ttu-id="c520c-171">I koden används en cursor i anslutningen, och metoden [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) kör SQL-instruktionen mot MySQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="c520c-171">The code uses a cursor on the connection, and [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) method executes the SQL statement against MySQL database.</span></span> 

<span data-ttu-id="c520c-172">Ersätt parametrarna `host`, `user`, `password` och `database` med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="c520c-172">Replace the `host`, `user`, `password`, and `database` parameters with the values that you specified when you created the server and database.</span></span>

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
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
    print("Something is wrong with the user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Update a data row in the table
  cursor.execute("UPDATE inventory SET quantity = %s WHERE name = %s;", (200, "banana"))
  print("Updated",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="delete-data"></a><span data-ttu-id="c520c-173">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="c520c-173">Delete data</span></span>
<span data-ttu-id="c520c-174">Använd följande kod för att ansluta och ta bort data med en **DELETE**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="c520c-174">Use the following code to connect and remove data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="c520c-175">Biblioteket mysql.connector importeras i koden.</span><span class="sxs-lookup"><span data-stu-id="c520c-175">In the code, the mysql.connector library is imported.</span></span>  <span data-ttu-id="c520c-176">Funktionen [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) används för att ansluta till Azure Database för MySQL med [anslutningsargumenten](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) i config-insamlingen.</span><span class="sxs-lookup"><span data-stu-id="c520c-176">The [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) function is used to connect to Azure Database for MySQL using the [connection arguments](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) in the config collection.</span></span> <span data-ttu-id="c520c-177">I koden används en cursor i anslutningen, och metoden [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) kör SQL-frågan mot MySQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="c520c-177">The code uses a cursor on the connection, and [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) method executes the SQL query against MySQL database.</span></span> 

<span data-ttu-id="c520c-178">Ersätt parametrarna `host`, `user`, `password` och `database` med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="c520c-178">Replace the `host`, `user`, `password`, and `database` parameters with the values that you specified when you created the server and database.</span></span>

```Python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
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
    print("Something is wrong with the user name or password.")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist.")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Delete a data row in the table
  cursor.execute("DELETE FROM inventory WHERE name=%(param1)s;", {'param1':"orange"})
  print("Deleted",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="next-steps"></a><span data-ttu-id="c520c-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c520c-179">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="c520c-180">Migrera din databas med Exportera och importera</span><span class="sxs-lookup"><span data-stu-id="c520c-180">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
