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
# <a name="azure-database-for-mysql-use-python-tooconnect-and-query-data"></a>Azure-databas för MySQL: Använd Python tooconnect och fråga data
Den här snabbstarten visar hur toouse [Python](https://python.org) tooconnect tooan Azure för MySQL-databas. SQL instruktioner tooquery, infoga, uppdatera och ta bort data används i hello databasen från Mac OS x, Ubuntu Linux och Windows-plattformar. hello stegen i den här artikeln förutsätter att du är bekant med att utveckla använder Python och nya tooworking med Azure-databas för MySQL.

## <a name="prerequisites"></a>Krav
Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:
- [Skapa en Azure Database för MySQL med Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Skapa en Azure Database för MySQL-server med Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-python-and-hello-mysql-connector"></a>Installera Python och hello MySQL-koppling
Installera [Python](https://www.python.org/downloads/) och hello [MySQL-koppling för Python](https://dev.mysql.com/downloads/connector/python/) på din egen dator. Beroende på din plattform gör hello:

### <a name="windows"></a>Windows
1. Hämta och installera Python 2.7 från [python.org](https://www.python.org/downloads/windows/). 
2. Kontrollera hello Python-installation genom att starta hello kommandotolk. Kör kommandot hello `C:\python27\python.exe -V` med hello versaler V växeln toosee hello versionsnumret.
3. Installera hello Python connector för MySQL från [mysql.com](https://dev.mysql.com/downloads/connector/python/) motsvarande tooyour version av Python.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
1. Linux (Ubuntu) installeras Python vanligtvis som en del av hello standardinstallation.
2. Kontrollera hello Python-installation genom att starta hello bash-gränssnitt. Kör kommandot hello `python -V` med hello versaler V växeln toosee hello versionsnumret.
3. Kontrollera hello PIP installationen genom att köra hello `pip show pip -V` kommandot toosee hello-versionsnumret. 
4. PIP kan ingå i vissa versioner av Python. Om PIP inte har installerats kan du installera hello [PIP] (https://pip.pypa.io/en/stable/installing/) paketet genom att köra kommandot `sudo apt-get install python-pip`.
5. Uppdatera PIP toohello senaste versionen genom att köra hello `pip install -U pip` kommando.
6. Installera hello MySQL connector för Python och dess beroenden med hello PIP-kommando:

   ```bash
   sudo pip install mysql-connector-python-rf
   ```
 
### <a name="macos"></a>MacOS
1. Mac OS x installeras Python vanligtvis som en del av hello standard OS-installationen.
2. Kontrollera hello Python-installation genom att starta hello bash-gränssnitt. Kör kommandot hello `python -V` med hello versaler V växeln toosee hello versionsnumret.
3. Kontrollera hello PIP installationen genom att köra hello `pip show pip -V` kommandot toosee hello-versionsnumret.
4. PIP kan ingå i vissa versioner av Python. Om PIP inte har installerats kan du installera hello [PIP](https://pip.pypa.io/en/stable/installing/) paketet.
5. Uppdatera PIP toohello senaste versionen genom att köra hello `pip install -U pip` kommando.
6. Installera hello MySQL connector för Python och dess beroenden med hello PIP-kommando:

   ```bash
   pip install mysql-connector-python-rf
   ```

## <a name="get-connection-information"></a>Hämta anslutningsinformation
Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för MySQL. Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Hello vänstra menyn i Azure-portalen klickar du på **alla resurser** och Sök efter hello-server som du har creased, som **myserver4demo**.
3. Klicka på servernamnet för hello **myserver4demo**.
4. Välj hello server **egenskaper** sidan. Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.
 ![Azure Database för MySQL – inloggning för serveradministratör](./media/connect-python/1_server-properties-name-login.png)
5. Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet och, om nödvändigt återställa hello lösenord.
   

## <a name="run-python-code"></a>Köra Python-kod
- Klistra in hello kod i en textfil och spara hello-filen till en projektmapp med filen tillägget .py, till exempel C:\pythonmysql\createtable.py eller /home/username/pythonmysql/createtable.py
- toorun hello kod, starta Kommandotolken hello eller bash shell. Ändra katalog till din projektmapp, till exempel `cd pythonmysql`. Skriv hello python kommando följt av namnet på filen hello `python createtable.py` toorun hello program. Om python.exe inte hittas på hello Windows-Operativsystemet, kan du ange hello fullständig sökväg toohello körbara eller Lägg till hello Python sökväg till hello path-miljövariabeln. `C:\python27\python.exe createtable.py`

## <a name="connect-create-table-and-insert-data"></a>Ansluta, skapa tabell och infoga data
Använd hello följande code tooconnect toohello server, skapa en tabell och läsa in hello data med hjälp av en **infoga** SQL-instruktionen. 

I hello kod har hello mysql.connector biblioteket importerats. Hej [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) funktionen är används tooconnect tooAzure databasen för MySQL med hello [anslutningsargument](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) i hello config samling. hello koden använder en markör på hello-anslutning och [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metoden Kör hello SQL-fråga mot MySQL-databas. 

Ersätt hello `host`, `user`, `password`, och `database` parametrar med hello värden som du angav när du skapade hello-server och databas.

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

## <a name="read-data"></a>Läsa data
Använd hello följande kod tooconnect och läsa hello data med hjälp av en **Välj** SQL-instruktionen. 

I hello kod har hello mysql.connector biblioteket importerats. Hej [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) funktionen är används tooconnect tooAzure databasen för MySQL med hello [anslutningsargument](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) i hello config samling. hello koden använder en markör på hello-anslutning och [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metoden Kör hello SQL-instruktion mot MySQL-databas. hello datarader läses med hello [fetchall()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-fetchall.html) metod. hello resultatet sparas i en samling rad och en för iterator är används tooloop över hello rader.

Ersätt hello `host`, `user`, `password`, och `database` parametrar med hello värden som du angav när du skapade hello-server och databas.

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

## <a name="update-data"></a>Uppdatera data
Använd hello följande kod tooconnect och uppdatera hello data med hjälp av en **uppdatera** SQL-instruktionen. 

I hello kod har hello mysql.connector biblioteket importerats.  Hej [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) funktionen är används tooconnect tooAzure databasen för MySQL med hello [anslutningsargument](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) i hello config samling. hello koden använder en markör på hello-anslutning och [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metoden Kör hello SQL-instruktion mot MySQL-databas. 

Ersätt hello `host`, `user`, `password`, och `database` parametrar med hello värden som du angav när du skapade hello-server och databas.

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

## <a name="delete-data"></a>Ta bort data
Använd hello följande kod tooconnect och ta bort data med hjälp av en **ta bort** SQL-instruktionen. 

I hello kod har hello mysql.connector biblioteket importerats.  Hej [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) funktionen är används tooconnect tooAzure databasen för MySQL med hello [anslutningsargument](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) i hello config samling. hello koden använder en markör på hello-anslutning och [cursor.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) metoden Kör hello SQL-fråga mot MySQL-databas. 

Ersätt hello `host`, `user`, `password`, och `database` parametrar med hello värden som du angav när du skapade hello-server och databas.

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

## <a name="next-steps"></a>Nästa steg
> [!div class="nextstepaction"]
> [Migrera din databas med Exportera och importera](./concepts-migrate-import-export.md)
