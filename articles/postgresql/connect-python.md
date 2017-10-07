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
# <a name="azure-database-for-postgresql-use-python-tooconnect-and-query-data"></a>Azure-databas för PostgreSQL: Använd Python tooconnect och fråga data
Den här snabbstarten visar hur toouse [Python](https://python.org) tooconnect tooan Azure-databas för PostgreSQL. Här visas också hur toouse SQL-instruktioner tooquery infoga, uppdatera och ta bort data i databasen hello i macOS, Ubuntu Linux och Windows-plattformar. hello stegen i den här artikeln förutsätter att du är bekant med att utveckla använder Python och nya tooworking med Azure-databas för PostgreSQL.

## <a name="prerequisites"></a>Krav
Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:
- [Skapa DB – Portal](quickstart-create-server-database-portal.md)
- [Skapa DB – CLI](quickstart-create-server-database-azure-cli.md)

Du behöver också:
- [Python](https://www.python.org/downloads/) installerad
- installera [PIP](https://pip.pypa.io/en/stable/installing/)-paketet (pip har redan installerats om du använder Python 2 > = 2.7.9 eller Python 3 > = 3-4-binärfiler som hämtats från [python.org](https://python.org)).

## <a name="install-hello-python-connection-libraries-for-postgresql"></a>Installera hello Python anslutningsbibliotek för PostgreSQL
Installera hello [psycopg2](http://initd.org/psycopg/docs/install.html) paketet, vilket gör att du tooconnect och fråga hello-databasen. psycopg2 är [på PyPI](https://pypi.python.org/pypi/psycopg2/) i hello form av [hjul](http://pythonwheels.com/) paket för de vanligaste hello-plattformar (Windows, Linux, OS x,). Använd pip installerar tooget hello binära versionen av hello modul, inklusive alla hello-beroenden.

1. Starta ett kommandoradsgränssnitt på din dator:
    - På Linux, för att starta hello Bash-gränssnitt.
    - I macOS, starta hello Terminal.
    - Starta hello kommandotolk från hello Start-menyn i Windows.
2. Se till att du använder hello mest aktuella versionen av pip genom att köra ett kommando som:
    ```cmd
    pip install -U pip
    ```

3. Kör följande kommando tooinstall hello psycopg2 paketet hello:
    ```cmd
    pip install psycopg2
    ```

## <a name="get-connection-information"></a>Hämta anslutningsinformation
Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för PostgreSQL. Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Hello vänstra menyn i Azure-portalen klickar du på **alla resurser** och Sök efter **mypgserver 20170401** (hello server som du just har skapat).
3. Klicka på servernamnet för hello **mypgserver 20170401**.
4. Välj hello server **översikt** sidan och anteckna sedan en hello **servernamn** och **serverinloggningsnamnet för admin**.
 ![Azure Database för PostgreSQL – inloggning för serveradministratör](./media/connect-python/1-connection-string.png)
5. Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet och, om nödvändigt återställa hello lösenord.

## <a name="how-toorun-python-code"></a>Hur toorun Python-kod
Det här ämnet innehåller totalt fyra kodexempel som vart och ett utför en viss funktion. hello visar följande anvisningar hur toocreate en textfil, infoga ett kodblock och spara hello-filen så att du kan köra den senare. Vara säker på att toocreate fyra separata filer, en för varje kodblock.

- Skapa en ny fil i valfri textredigerare.
- Kopiera och klistra in en hello kodexempel i följande avsnitt i hello textfil hello. Ersätt hello **värden**, **dbname**, **användare**, och **lösenord** parametrar med hello värden som du angav när du skapade hello Server och databas.
- Spara hello-filen med hello .py tillägg (till exempel postgres.py) i projektmappen. Om du kör hello Windows-Operativsystemet kan vara att tooselect UTF-8-kodning när hello fil sparas. 
- Starta hello kommandotolk eller Bash-gränssnitt och ändra hello directory tooyour projektmappen, till exempel `cd postgres`.
-  toorun hello kod, typen hello Python kommandot följt av hello filnamn, till exempel `Python postgres.py`.

> [!NOTE]
> Från och med Python version 3, kan du se hello fel `SyntaxError: Missing parentheses in call too'print'` när du kör hello följande kodblock. Om detta händer Ersätt varje anrop toohello kommando `print "string"` med ett funktionsanrop med parenteser, till exempel `print("string")`.

## <a name="connect-create-table-and-insert-data"></a>Ansluta, skapa tabell och infoga data
Använd hello följande kod tooconnect och läsa in hello data med hjälp av [psycopg2.connect](http://initd.org/psycopg/docs/connection.html) fungerar med **infoga** SQL-instruktionen. Hej [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) funktionen är används tooexecute hello SQL-fråga mot PostgreSQL-databas. Ersätt hello värden, dbname, användare och lösenordsparametrar med hello värden som du angav när du skapade hello-server och databas.

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

När hello koden har körts, hello utdata visas på följande sätt:

![Utdata i kommandorad](media/connect-python/2-example-python-output.png)

## <a name="read-data"></a>Läsa data
Använd hello följande kod tooread hello data infogas med [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) fungerar med **Välj** SQL-instruktionen. Den här funktionen accepterar en fråga och returnerar en resultatuppsättning som kan vara hävdade över med hello [cursor.fetchall()](http://initd.org/psycopg/docs/cursor.html#cursor.fetchall). Ersätt hello värden, dbname, användare och lösenordsparametrar med hello värden som du angav när du skapade hello-server och databas.

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

## <a name="update-data"></a>Uppdatera data
Använd hello följande kod tooupdate hello inventering raden som du tidigare infogade med [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) fungerar med **uppdatering** SQL-instruktionen. Ersätt hello värden, dbname, användare och lösenordsparametrar med hello värden som du angav när du skapade hello-server och databas.

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

## <a name="delete-data"></a>Ta bort data
Använd hello följande kod toodelete ett lagerobjekt som du tidigare infogade med [cursor.execute](http://initd.org/psycopg/docs/cursor.html#execute) fungerar med **ta bort** SQL-instruktionen. Ersätt hello värden, dbname, användare och lösenordsparametrar med hello värden som du angav när du skapade hello-server och databas.

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

## <a name="next-steps"></a>Nästa steg
> [!div class="nextstepaction"]
> [Migrera din databas med Exportera och importera](./howto-migrate-using-export-and-import.md)
