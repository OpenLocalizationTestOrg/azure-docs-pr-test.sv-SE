---
title: "Ansluta till Azure Database för PostgreSQL med Python | Microsoft Docs"
description: "I den här snabbstarten finns ett kodexempel i Python som du kan använda för att ansluta till och fråga efter data från Azure Database för PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: python
ms.topic: quickstart
ms.date: 08/15/2017
ms.openlocfilehash: d682d94143fb9fd5e2c2a578c3cb0dcfa101462c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-database-for-postgresql-use-python-to-connect-and-query-data"></a><span data-ttu-id="b4d5c-103">Azure Database för PostgreSQL: Använda Python för att ansluta och fråga efter data</span><span class="sxs-lookup"><span data-stu-id="b4d5c-103">Azure Database for PostgreSQL: Use Python to connect and query data</span></span>
<span data-ttu-id="b4d5c-104">Den här snabbstarten visar hur du använder [Python](https://python.org) för att ansluta till en Azure Database för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-104">This quickstart demonstrates how to use [Python](https://python.org) to connect to an Azure Database for PostgreSQL.</span></span> <span data-ttu-id="b4d5c-105">Den visar också hur SQL-instruktioner används för att fråga, infoga, uppdatera och ta bort data i databasen i macOS-, Ubuntu Linux- och Windows-plattformar.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-105">It also demonstrates how to use SQL statements to query, insert, update, and delete data in the database from macOS, Ubuntu Linux, and Windows platforms.</span></span> <span data-ttu-id="b4d5c-106">I den här artikeln förutsätter vi att du har kunskaper om Python och att du inte har arbetat med Azure Database för PostgreSQL tidigare.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-106">The steps in this article assume that you are familiar with developing using Python and are new to working with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b4d5c-107">Krav</span><span class="sxs-lookup"><span data-stu-id="b4d5c-107">Prerequisites</span></span>
<span data-ttu-id="b4d5c-108">I den här snabbstarten används de resurser som skapades i någon av följande guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="b4d5c-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="b4d5c-109">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="b4d5c-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="b4d5c-110">Skapa DB – CLI</span><span class="sxs-lookup"><span data-stu-id="b4d5c-110">Create DB - CLI</span></span>](quickstart-create-server-database-azure-cli.md)

<span data-ttu-id="b4d5c-111">Du behöver också:</span><span class="sxs-lookup"><span data-stu-id="b4d5c-111">You also need:</span></span>
- <span data-ttu-id="b4d5c-112">[Python](https://www.python.org/downloads/) installerad</span><span class="sxs-lookup"><span data-stu-id="b4d5c-112">[python](https://www.python.org/downloads/) installed</span></span>
- <span data-ttu-id="b4d5c-113">installera [PIP](https://pip.pypa.io/en/stable/installing/)-paketet (pip har redan installerats om du använder Python 2 > = 2.7.9 eller Python 3 > = 3-4-binärfiler som hämtats från [python.org](https://python.org)).</span><span class="sxs-lookup"><span data-stu-id="b4d5c-113">[pip](https://pip.pypa.io/en/stable/installing/) package installed (pip is already installed if you're working with Python 2 >=2.7.9 or Python 3 >=3.4 binaries downloaded from [python.org](https://python.org).</span></span>

## <a name="install-the-python-connection-libraries-for-postgresql"></a><span data-ttu-id="b4d5c-114">Installera Python-anslutningsbibliotek för PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="b4d5c-114">Install the Python connection libraries for PostgreSQL</span></span>
<span data-ttu-id="b4d5c-115">Installera paketet [psycopg2](http://initd.org/psycopg/docs/install.html) för att ansluta till och fråga databasen.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-115">Install the [psycopg2](http://initd.org/psycopg/docs/install.html) package, which enables you to connect and query the database.</span></span> <span data-ttu-id="b4d5c-116">psycopg2 finns [i PyPI](https://pypi.python.org/pypi/psycopg2/) i form av [WHL](http://pythonwheels.com/)-paket för de flesta vanliga plattformar (Linux, OSX, Windows).</span><span class="sxs-lookup"><span data-stu-id="b4d5c-116">psycopg2 is [available on PyPI](https://pypi.python.org/pypi/psycopg2/) in the form of [wheel](http://pythonwheels.com/) packages for the most common platforms (Linux, OSX, Windows).</span></span> <span data-ttu-id="b4d5c-117">Använd pip-installation för att få den binära versionen av modulen, inklusive alla beroenden.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-117">Use pip install to get the binary version of the module including all the dependencies.</span></span>

1. <span data-ttu-id="b4d5c-118">Starta ett kommandoradsgränssnitt på din dator:</span><span class="sxs-lookup"><span data-stu-id="b4d5c-118">On your own computer, launch a command-line interface:</span></span>
    - <span data-ttu-id="b4d5c-119">I Linux, starta Bash-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-119">On Linux, launch the Bash shell.</span></span>
    - <span data-ttu-id="b4d5c-120">I macOS, starta Terminal.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-120">On macOS, launch the Terminal.</span></span>
    - <span data-ttu-id="b4d5c-121">I Windows, starta kommandotolken från Start-menyn.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-121">On Windows, launch the Command Prompt from the Start Menu.</span></span>
2. <span data-ttu-id="b4d5c-122">Se till att du använder den senaste versionen av pip genom att köra ett kommando som:</span><span class="sxs-lookup"><span data-stu-id="b4d5c-122">Ensure that you are using the most current version of pip by running a command such as:</span></span>
    ```cmd
    pip install -U pip
    ```

3. <span data-ttu-id="b4d5c-123">Kör följande kommando för att installera psycopg2-paketet:</span><span class="sxs-lookup"><span data-stu-id="b4d5c-123">Run the following command to install the psycopg2 package:</span></span>
    ```cmd
    pip install psycopg2
    ```

## <a name="get-connection-information"></a><span data-ttu-id="b4d5c-124">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="b4d5c-124">Get connection information</span></span>
<span data-ttu-id="b4d5c-125">Hämta den information som du behöver för att ansluta till Azure Database för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-125">Get the connection information needed to connect to the Azure Database for PostgreSQL.</span></span> <span data-ttu-id="b4d5c-126">Du behöver det fullständiga servernamnet och inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-126">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="b4d5c-127">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b4d5c-127">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="b4d5c-128">I den vänstra menyn i Azure Portal klickar du på **Alla resurser** och söker efter **mypgserver-20170401** (den server som du nyss skapade).</span><span class="sxs-lookup"><span data-stu-id="b4d5c-128">From the left-hand menu in Azure portal, click **All resources** and search for **mypgserver-20170401** (the server you just created).</span></span>
3. <span data-ttu-id="b4d5c-129">Klicka på servernamnet **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-129">Click the server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="b4d5c-130">Välj serverns sida **Översikt** och notera **Servernamn** och **Inloggningsnamn för serveradministratören**.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-130">Select the server's **Overview** page, and then make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="b4d5c-131">![Azure Database för PostgreSQL – inloggning för serveradministratör](./media/connect-python/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="b4d5c-131">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-python/1-connection-string.png)</span></span>
5. <span data-ttu-id="b4d5c-132">Om du glömmer inloggningsinformationen för servern öppnar du sidan **Översikt** för att se inloggningsnamnet för serveradministratören. Om det behövs kan du återställa lösenordet.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-132">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="how-to-run-python-code"></a><span data-ttu-id="b4d5c-133">Så här kör du Python-kod</span><span class="sxs-lookup"><span data-stu-id="b4d5c-133">How to run Python code</span></span>
<span data-ttu-id="b4d5c-134">Det här ämnet innehåller totalt fyra kodexempel som vart och ett utför en viss funktion.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-134">This topic contains a total of four code samples, each of which performs a specific function.</span></span> <span data-ttu-id="b4d5c-135">Följande anvisningar beskriver hur du skapar en textfil, infogar ett kodblock och sedan sparar filen så att du kan köra den senare.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-135">The following instructions indicate how to create a text file, insert a code block, and then save the file so that you can run it later.</span></span> <span data-ttu-id="b4d5c-136">Se till att du skapar fyra olika filer, en för varje kodblock.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-136">Be sure to create four separate files, one for each code block.</span></span>

- <span data-ttu-id="b4d5c-137">Skapa en ny fil i valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-137">Using your favorite text editor, create a new file.</span></span>
- <span data-ttu-id="b4d5c-138">Kopiera och klistra in ett av kodexemplen i följande avsnitt i textfilen.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-138">Copy and paste one of the code samples in the following sections into the text file.</span></span> <span data-ttu-id="b4d5c-139">Ersätt parametrarna **host**, **dbname**, **user** och **password** med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-139">Replace the **host**, **dbname**, **user**, and **password** parameters with the values that you specified when you created the server and database.</span></span>
- <span data-ttu-id="b4d5c-140">Spara filen med filnamnstillägget .py (till exempel postgres.py) i projektmappen.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-140">Save the file with the .py extension (for example postgres.py) into your project folder.</span></span> <span data-ttu-id="b4d5c-141">Om du kör Windows-operativsystemet ska du se till att välja UTF-8-kodning när du sparar filen.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-141">If you are running the Windows OS, be sure to select UTF-8 encoding when saving the file.</span></span> 
- <span data-ttu-id="b4d5c-142">Starta kommandotolken eller Bash-gränssnittet och byt katalog till din projektmapp, till exempel `cd postgres`.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-142">Launch the Command Prompt or Bash shell and then change the directory to your project folder, for example `cd postgres`.</span></span>
-  <span data-ttu-id="b4d5c-143">Skriv Python-kommandot följt av filnamnet, till exempel `Python postgres.py`, för att köra koden.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-143">To run the code, type the Python command followed by the file name, for example `Python postgres.py`.</span></span>

> [!NOTE]
> <span data-ttu-id="b4d5c-144">Från och med Python version 3 kanske du ser felet `SyntaxError: Missing parentheses in call to 'print'` när du kör kodblocken nedan.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-144">Starting in Python version 3, you may see the error `SyntaxError: Missing parentheses in call to 'print'` when running the following code blocks.</span></span> <span data-ttu-id="b4d5c-145">Om detta händer, ersätter du varje anrop till kommandot `print "string"` med ett funktionsanrop med parenteser, till exempel `print("string")`.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-145">If that happens, replace each call to the command `print "string"` with a function call using parenthesis, such as `print("string")`.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="b4d5c-146">Ansluta, skapa tabell och infoga data</span><span class="sxs-lookup"><span data-stu-id="b4d5c-146">Connect, create table, and insert data</span></span>
<span data-ttu-id="b4d5c-147">Använd följande kod för att ansluta och läsa in data med hjälp av funktionen [psycopg2.connect](http://initd.org/psycopg/docs/connection.html) med en **INSERT**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-147">Use the following code to connect and load the data using [psycopg2.connect](http://initd.org/psycopg/docs/connection.html) function with **INSERT** SQL statement.</span></span> <span data-ttu-id="b4d5c-148">Funktionen [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) används för att köra SQL-frågor mot en PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-148">The [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) function is used to execute the SQL query against PostgreSQL database.</span></span> <span data-ttu-id="b4d5c-149">Ersätt parametrarna host, dbname,user och password med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-149">Replace the host, dbname, user, and password parameters with the values that you specified when you created the server and database.</span></span>

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Drop previous table of same name if one exists
cursor.execute("DROP TABLE IF EXISTS inventory;")
print "Finished dropping table (if existed)"

# Create table
cursor.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
print "Finished creating table"

# Insert some data into table
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("banana", 150))
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("orange", 154))
cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("apple", 100))
print "Inserted 3 rows of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

<span data-ttu-id="b4d5c-150">När koden har körts visas utdata på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b4d5c-150">After the code runs successfully, the output appears as follows:</span></span>

![Utdata i kommandorad](media/connect-python/2-example-python-output.png)

## <a name="read-data"></a><span data-ttu-id="b4d5c-152">Läsa data</span><span class="sxs-lookup"><span data-stu-id="b4d5c-152">Read data</span></span>
<span data-ttu-id="b4d5c-153">Använd följande kod för att läsa data som infogats med funktionen [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) med **SELECT** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-153">Use the following code to read the data inserted using [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) function with **SELECT** SQL statement.</span></span> <span data-ttu-id="b4d5c-154">Den här funktionen accepterar en fråga och returnerar en resultatuppsättning som kan upprepas med hjälp av [cursor.fetchall()](http://initd.org/psycopg/docs/cursor.html#cursor.fetchall).</span><span class="sxs-lookup"><span data-stu-id="b4d5c-154">This function accepts a query and returns a result set that can be iterated over with the use of [cursor.fetchall()](http://initd.org/psycopg/docs/cursor.html#cursor.fetchall).</span></span> <span data-ttu-id="b4d5c-155">Ersätt parametrarna host, dbname,user och password med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-155">Replace the host, dbname, user, and password parameters with the values that you specified when you created the server and database.</span></span>

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Fetch all rows from table
cursor.execute("SELECT * FROM inventory;")
rows = cursor.fetchall()

# Print all rows
for row in rows:
    print "Data row = (%s, %s, %s)" %(str(row[0]), str(row[1]), str(row[2]))

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="update-data"></a><span data-ttu-id="b4d5c-156">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="b4d5c-156">Update data</span></span>
<span data-ttu-id="b4d5c-157">Med följande kod uppdaterar du inventarieraden du tidigare lade till med funktionen [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) och SQL-instruktionen **UPDATE** .</span><span class="sxs-lookup"><span data-stu-id="b4d5c-157">Use the following code to update the inventory row that you previously inserted using [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) function with **UPDATE** SQL statement.</span></span> <span data-ttu-id="b4d5c-158">Ersätt parametrarna host, dbname,user och password med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-158">Replace the host, dbname, user, and password parameters with the values that you specified when you created the server and database.</span></span>

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Update a data row in the table
cursor.execute("UPDATE inventory SET quantity = %s WHERE name = %s;", (200, "banana"))
print "Updated 1 row of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="delete-data"></a><span data-ttu-id="b4d5c-159">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="b4d5c-159">Delete data</span></span>
<span data-ttu-id="b4d5c-160">Med följande kod raderar du inventarieobjektet du tidigare lade till med funktionen [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) och SQL-instruktionen **DELETE** .</span><span class="sxs-lookup"><span data-stu-id="b4d5c-160">Use the following code to delete an inventory item that you previously inserted using [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) function with **DELETE** SQL statement.</span></span> <span data-ttu-id="b4d5c-161">Ersätt parametrarna host, dbname,user och password med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="b4d5c-161">Replace the host, dbname, user, and password parameters with the values that you specified when you created the server and database.</span></span>

```Python
import psycopg2

# Update connection string information obtained from the portal
host = "mypgserver-20170401.postgres.database.azure.com"
user = "mylogin@mypgserver-20170401"
dbname = "mypgsqldb"
password = "<server_admin_password>"
sslmode = "require"

# Construct connection string
conn_string = "host={0} user={1} dbname={2} password={3} sslmode={4}".format(host, user, dbname, password, sslmode)
conn = psycopg2.connect(conn_string) 
print "Connection established"

cursor = conn.cursor()

# Delete data row from table
cursor.execute("DELETE FROM inventory WHERE name = %s;", ("orange",))
print "Deleted 1 row of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="next-steps"></a><span data-ttu-id="b4d5c-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b4d5c-162">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="b4d5c-163">Migrera din databas med Exportera och importera</span><span class="sxs-lookup"><span data-stu-id="b4d5c-163">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
