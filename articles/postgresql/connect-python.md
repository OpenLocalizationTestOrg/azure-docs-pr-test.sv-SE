---
title: "Ansluta tooAzure databas för PostgreSQL från Python | Microsoft Docs"
description: "Denna Snabbstart innehåller en Python-kodexempel som du kan använda tooconnect och fråga efter data från Azure-databas för PostgreSQL."
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
ms.openlocfilehash: 7d6d9f5424fb39ad8837999d4788b4363c818887
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-use-python-tooconnect-and-query-data"></a><span data-ttu-id="d64d4-103">Azure-databas för PostgreSQL: Använd Python tooconnect och fråga data</span><span class="sxs-lookup"><span data-stu-id="d64d4-103">Azure Database for PostgreSQL: Use Python tooconnect and query data</span></span>
<span data-ttu-id="d64d4-104">Den här snabbstarten visar hur toouse [Python](https://python.org) tooconnect tooan Azure-databas för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="d64d4-104">This quickstart demonstrates how toouse [Python](https://python.org) tooconnect tooan Azure Database for PostgreSQL.</span></span> <span data-ttu-id="d64d4-105">Här visas också hur toouse SQL-instruktioner tooquery infoga, uppdatera och ta bort data i databasen hello i macOS, Ubuntu Linux och Windows-plattformar.</span><span class="sxs-lookup"><span data-stu-id="d64d4-105">It also demonstrates how toouse SQL statements tooquery, insert, update, and delete data in hello database from macOS, Ubuntu Linux, and Windows platforms.</span></span> <span data-ttu-id="d64d4-106">hello stegen i den här artikeln förutsätter att du är bekant med att utveckla använder Python och nya tooworking med Azure-databas för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="d64d4-106">hello steps in this article assume that you are familiar with developing using Python and are new tooworking with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d64d4-107">Krav</span><span class="sxs-lookup"><span data-stu-id="d64d4-107">Prerequisites</span></span>
<span data-ttu-id="d64d4-108">Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="d64d4-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="d64d4-109">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="d64d4-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="d64d4-110">Skapa DB – CLI</span><span class="sxs-lookup"><span data-stu-id="d64d4-110">Create DB - CLI</span></span>](quickstart-create-server-database-azure-cli.md)

<span data-ttu-id="d64d4-111">Du behöver också:</span><span class="sxs-lookup"><span data-stu-id="d64d4-111">You also need:</span></span>
- <span data-ttu-id="d64d4-112">[Python](https://www.python.org/downloads/) installerad</span><span class="sxs-lookup"><span data-stu-id="d64d4-112">[python](https://www.python.org/downloads/) installed</span></span>
- <span data-ttu-id="d64d4-113">installera [PIP](https://pip.pypa.io/en/stable/installing/)-paketet (pip har redan installerats om du använder Python 2 > = 2.7.9 eller Python 3 > = 3-4-binärfiler som hämtats från [python.org](https://python.org)).</span><span class="sxs-lookup"><span data-stu-id="d64d4-113">[pip](https://pip.pypa.io/en/stable/installing/) package installed (pip is already installed if you're working with Python 2 >=2.7.9 or Python 3 >=3.4 binaries downloaded from [python.org](https://python.org).</span></span>

## <a name="install-hello-python-connection-libraries-for-postgresql"></a><span data-ttu-id="d64d4-114">Installera hello Python anslutningsbibliotek för PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="d64d4-114">Install hello Python connection libraries for PostgreSQL</span></span>
<span data-ttu-id="d64d4-115">Installera hello [psycopg2](http://initd.org/psycopg/docs/install.html) paketet, vilket gör att du tooconnect och fråga hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="d64d4-115">Install hello [psycopg2](http://initd.org/psycopg/docs/install.html) package, which enables you tooconnect and query hello database.</span></span> <span data-ttu-id="d64d4-116">psycopg2 är [på PyPI](https://pypi.python.org/pypi/psycopg2/) i hello form av [hjul](http://pythonwheels.com/) paket för de vanligaste hello-plattformar (Windows, Linux, OS x,).</span><span class="sxs-lookup"><span data-stu-id="d64d4-116">psycopg2 is [available on PyPI](https://pypi.python.org/pypi/psycopg2/) in hello form of [wheel](http://pythonwheels.com/) packages for hello most common platforms (Linux, OSX, Windows).</span></span> <span data-ttu-id="d64d4-117">Använd pip installerar tooget hello binära versionen av hello modul, inklusive alla hello-beroenden.</span><span class="sxs-lookup"><span data-stu-id="d64d4-117">Use pip install tooget hello binary version of hello module including all hello dependencies.</span></span>

1. <span data-ttu-id="d64d4-118">Starta ett kommandoradsgränssnitt på din dator:</span><span class="sxs-lookup"><span data-stu-id="d64d4-118">On your own computer, launch a command-line interface:</span></span>
    - <span data-ttu-id="d64d4-119">På Linux, för att starta hello Bash-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="d64d4-119">On Linux, launch hello Bash shell.</span></span>
    - <span data-ttu-id="d64d4-120">I macOS, starta hello Terminal.</span><span class="sxs-lookup"><span data-stu-id="d64d4-120">On macOS, launch hello Terminal.</span></span>
    - <span data-ttu-id="d64d4-121">Starta hello kommandotolk från hello Start-menyn i Windows.</span><span class="sxs-lookup"><span data-stu-id="d64d4-121">On Windows, launch hello Command Prompt from hello Start Menu.</span></span>
2. <span data-ttu-id="d64d4-122">Se till att du använder hello mest aktuella versionen av pip genom att köra ett kommando som:</span><span class="sxs-lookup"><span data-stu-id="d64d4-122">Ensure that you are using hello most current version of pip by running a command such as:</span></span>
    ```cmd
    pip install -U pip
    ```

3. <span data-ttu-id="d64d4-123">Kör följande kommando tooinstall hello psycopg2 paketet hello:</span><span class="sxs-lookup"><span data-stu-id="d64d4-123">Run hello following command tooinstall hello psycopg2 package:</span></span>
    ```cmd
    pip install psycopg2
    ```

## <a name="get-connection-information"></a><span data-ttu-id="d64d4-124">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="d64d4-124">Get connection information</span></span>
<span data-ttu-id="d64d4-125">Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="d64d4-125">Get hello connection information needed tooconnect toohello Azure Database for PostgreSQL.</span></span> <span data-ttu-id="d64d4-126">Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.</span><span class="sxs-lookup"><span data-stu-id="d64d4-126">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="d64d4-127">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d64d4-127">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="d64d4-128">Hello vänstra menyn i Azure-portalen klickar du på **alla resurser** och Sök efter **mypgserver 20170401** (hello server som du just har skapat).</span><span class="sxs-lookup"><span data-stu-id="d64d4-128">From hello left-hand menu in Azure portal, click **All resources** and search for **mypgserver-20170401** (hello server you just created).</span></span>
3. <span data-ttu-id="d64d4-129">Klicka på servernamnet för hello **mypgserver 20170401**.</span><span class="sxs-lookup"><span data-stu-id="d64d4-129">Click hello server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="d64d4-130">Välj hello server **översikt** sidan och anteckna sedan en hello **servernamn** och **serverinloggningsnamnet för admin**.</span><span class="sxs-lookup"><span data-stu-id="d64d4-130">Select hello server's **Overview** page, and then make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="d64d4-131">![Azure Database för PostgreSQL – inloggning för serveradministratör](./media/connect-python/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="d64d4-131">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-python/1-connection-string.png)</span></span>
5. <span data-ttu-id="d64d4-132">Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet och, om nödvändigt återställa hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="d64d4-132">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="how-toorun-python-code"></a><span data-ttu-id="d64d4-133">Hur toorun Python-kod</span><span class="sxs-lookup"><span data-stu-id="d64d4-133">How toorun Python code</span></span>
<span data-ttu-id="d64d4-134">Det här ämnet innehåller totalt fyra kodexempel som vart och ett utför en viss funktion.</span><span class="sxs-lookup"><span data-stu-id="d64d4-134">This topic contains a total of four code samples, each of which performs a specific function.</span></span> <span data-ttu-id="d64d4-135">hello visar följande anvisningar hur toocreate en textfil, infoga ett kodblock och spara hello-filen så att du kan köra den senare.</span><span class="sxs-lookup"><span data-stu-id="d64d4-135">hello following instructions indicate how toocreate a text file, insert a code block, and then save hello file so that you can run it later.</span></span> <span data-ttu-id="d64d4-136">Vara säker på att toocreate fyra separata filer, en för varje kodblock.</span><span class="sxs-lookup"><span data-stu-id="d64d4-136">Be sure toocreate four separate files, one for each code block.</span></span>

- <span data-ttu-id="d64d4-137">Skapa en ny fil i valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="d64d4-137">Using your favorite text editor, create a new file.</span></span>
- <span data-ttu-id="d64d4-138">Kopiera och klistra in en hello kodexempel i följande avsnitt i hello textfil hello.</span><span class="sxs-lookup"><span data-stu-id="d64d4-138">Copy and paste one of hello code samples in hello following sections into hello text file.</span></span> <span data-ttu-id="d64d4-139">Ersätt hello **värden**, **dbname**, **användare**, och **lösenord** parametrar med hello värden som du angav när du skapade hello Server och databas.</span><span class="sxs-lookup"><span data-stu-id="d64d4-139">Replace hello **host**, **dbname**, **user**, and **password** parameters with hello values that you specified when you created hello server and database.</span></span>
- <span data-ttu-id="d64d4-140">Spara hello-filen med hello .py tillägg (till exempel postgres.py) i projektmappen.</span><span class="sxs-lookup"><span data-stu-id="d64d4-140">Save hello file with hello .py extension (for example postgres.py) into your project folder.</span></span> <span data-ttu-id="d64d4-141">Om du kör hello Windows-Operativsystemet kan vara att tooselect UTF-8-kodning när hello fil sparas.</span><span class="sxs-lookup"><span data-stu-id="d64d4-141">If you are running hello Windows OS, be sure tooselect UTF-8 encoding when saving hello file.</span></span> 
- <span data-ttu-id="d64d4-142">Starta hello kommandotolk eller Bash-gränssnitt och ändra hello directory tooyour projektmappen, till exempel `cd postgres`.</span><span class="sxs-lookup"><span data-stu-id="d64d4-142">Launch hello Command Prompt or Bash shell and then change hello directory tooyour project folder, for example `cd postgres`.</span></span>
-  <span data-ttu-id="d64d4-143">toorun hello kod, typen hello Python kommandot följt av hello filnamn, till exempel `Python postgres.py`.</span><span class="sxs-lookup"><span data-stu-id="d64d4-143">toorun hello code, type hello Python command followed by hello file name, for example `Python postgres.py`.</span></span>

> [!NOTE]
> <span data-ttu-id="d64d4-144">Från och med Python version 3, kan du se hello fel `SyntaxError: Missing parentheses in call too'print'` när du kör hello följande kodblock.</span><span class="sxs-lookup"><span data-stu-id="d64d4-144">Starting in Python version 3, you may see hello error `SyntaxError: Missing parentheses in call too'print'` when running hello following code blocks.</span></span> <span data-ttu-id="d64d4-145">Om detta händer Ersätt varje anrop toohello kommando `print "string"` med ett funktionsanrop med parenteser, till exempel `print("string")`.</span><span class="sxs-lookup"><span data-stu-id="d64d4-145">If that happens, replace each call toohello command `print "string"` with a function call using parenthesis, such as `print("string")`.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="d64d4-146">Ansluta, skapa tabell och infoga data</span><span class="sxs-lookup"><span data-stu-id="d64d4-146">Connect, create table, and insert data</span></span>
<span data-ttu-id="d64d4-147">Använd hello följande kod tooconnect och läsa in hello data med hjälp av [psycopg2.connect](http://initd.org/psycopg/docs/connection.html) fungerar med **infoga** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="d64d4-147">Use hello following code tooconnect and load hello data using [psycopg2.connect](http://initd.org/psycopg/docs/connection.html) function with **INSERT** SQL statement.</span></span> <span data-ttu-id="d64d4-148">Hej [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) funktionen är används tooexecute hello SQL-fråga mot PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="d64d4-148">hello [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) function is used tooexecute hello SQL query against PostgreSQL database.</span></span> <span data-ttu-id="d64d4-149">Ersätt hello värden, dbname, användare och lösenordsparametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="d64d4-149">Replace hello host, dbname, user, and password parameters with hello values that you specified when you created hello server and database.</span></span>

```Python
import psycopg2

# Update connection string information obtained from hello portal
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

<span data-ttu-id="d64d4-150">När hello koden har körts, hello utdata visas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="d64d4-150">After hello code runs successfully, hello output appears as follows:</span></span>

![Utdata i kommandorad](media/connect-python/2-example-python-output.png)

## <a name="read-data"></a><span data-ttu-id="d64d4-152">Läsa data</span><span class="sxs-lookup"><span data-stu-id="d64d4-152">Read data</span></span>
<span data-ttu-id="d64d4-153">Använd hello följande kod tooread hello data infogas med [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) fungerar med **Välj** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="d64d4-153">Use hello following code tooread hello data inserted using [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) function with **SELECT** SQL statement.</span></span> <span data-ttu-id="d64d4-154">Den här funktionen accepterar en fråga och returnerar en resultatuppsättning som kan vara hävdade över med hello [cursor.fetchall()](http://initd.org/psycopg/docs/cursor.html#cursor.fetchall).</span><span class="sxs-lookup"><span data-stu-id="d64d4-154">This function accepts a query and returns a result set that can be iterated over with hello use of [cursor.fetchall()](http://initd.org/psycopg/docs/cursor.html#cursor.fetchall).</span></span> <span data-ttu-id="d64d4-155">Ersätt hello värden, dbname, användare och lösenordsparametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="d64d4-155">Replace hello host, dbname, user, and password parameters with hello values that you specified when you created hello server and database.</span></span>

```Python
import psycopg2

# Update connection string information obtained from hello portal
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

## <a name="update-data"></a><span data-ttu-id="d64d4-156">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="d64d4-156">Update data</span></span>
<span data-ttu-id="d64d4-157">Använd hello följande kod tooupdate hello inventering raden som du tidigare infogade med [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) fungerar med **uppdatering** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="d64d4-157">Use hello following code tooupdate hello inventory row that you previously inserted using [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) function with **UPDATE** SQL statement.</span></span> <span data-ttu-id="d64d4-158">Ersätt hello värden, dbname, användare och lösenordsparametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="d64d4-158">Replace hello host, dbname, user, and password parameters with hello values that you specified when you created hello server and database.</span></span>

```Python
import psycopg2

# Update connection string information obtained from hello portal
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

# Update a data row in hello table
cursor.execute("UPDATE inventory SET quantity = %s WHERE name = %s;", (200, "banana"))
print "Updated 1 row of data"

# Cleanup
conn.commit()
cursor.close()
conn.close()
```

## <a name="delete-data"></a><span data-ttu-id="d64d4-159">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="d64d4-159">Delete data</span></span>
<span data-ttu-id="d64d4-160">Använd hello följande kod toodelete ett lagerobjekt som du tidigare infogade med [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) fungerar med **ta bort** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="d64d4-160">Use hello following code toodelete an inventory item that you previously inserted using [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) function with **DELETE** SQL statement.</span></span> <span data-ttu-id="d64d4-161">Ersätt hello värden, dbname, användare och lösenordsparametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="d64d4-161">Replace hello host, dbname, user, and password parameters with hello values that you specified when you created hello server and database.</span></span>

```Python
import psycopg2

# Update connection string information obtained from hello portal
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

## <a name="next-steps"></a><span data-ttu-id="d64d4-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d64d4-162">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="d64d4-163">Migrera din databas med Exportera och importera</span><span class="sxs-lookup"><span data-stu-id="d64d4-163">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
