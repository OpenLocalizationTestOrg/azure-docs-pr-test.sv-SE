---
title: "aaaConnect tooAzure databas för PostgreSQL med hjälp av gå språk | Microsoft Docs"
description: "Denna Snabbstart ger ett gå programming språk exempel du kan använda tooconnect och fråga efter data från Azure-databas för PostgreSQL."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: go
ms.topic: quickstart
ms.date: 06/29/2017
ms.openlocfilehash: aa3c93da03116b8fcb54557494dccfad558e5f1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-use-go-language-tooconnect-and-query-data"></a>Azure-databas för PostgreSQL: använda gå språk tooconnect och fråga data
Den här snabbstarten visar hur tooconnect tooan Azure Database för att använda PostgreSQL code skriven i hello [Gå](https://golang.org/) språk (golang). Den visar hur toouse SQL-instruktioner tooquery infoga, uppdatera och ta bort data i hello-databas. Den här artikeln förutsätter att du är bekant med utveckling med hjälp av gå, men som du är ny tooworking med Azure-databas för PostgreSQL.

## <a name="prerequisites"></a>Krav
Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:
- [Skapa DB – Portal](quickstart-create-server-database-portal.md)
- [Skapa DB – Azure CLI](quickstart-create-server-database-azure-cli.md)

## <a name="install-go-and-pq-connector"></a>Installera Go och pq connector
Installera [Gå](https://golang.org/doc/install) och hello [ren gå Postgres drivrutin (pq)](https://github.com/lib/pq) på din egen dator. Beroende på din plattform gör hello:

### <a name="windows"></a>Windows
1. [Hämta](https://golang.org/dl/) och installera gå för Microsoft Windows enligt toohello [Installationsinstruktioner](https://golang.org/doc/install).
2. Starta hello kommandotolk från hello start-menyn.
3. Skapa en mapp för ditt projekt, till exempel. `mkdir  %USERPROFILE%\go\src\postgresqlgo`.
4. Ändra katalogen till hello projektmappen som `cd %USERPROFILE%\go\src\postgresqlgo`.
5. Ange hello miljövariabeln efter koden för GOPATH toopoint toohello källkatalogen. `set GOPATH=%USERPROFILE%\go`.
6. Installera hello [ren gå Postgres drivrutin (pq)](https://github.com/lib/pq) genom att köra hello `go get github.com/lib/pq` kommando.

   Installera gå i sammanfattningen, och sedan köra dessa kommandon i Kommandotolken för hello:
   ```cmd
   mkdir  %USERPROFILE%\go\src\postgresqlgo
   cd %USERPROFILE%\go\src\postgresqlgo
   set GOPATH=%USERPROFILE%\go
   go get github.com/lib/pq
   ```

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
1. Starta hello Bash-gränssnitt. 
2. Installera Go genom att köra `sudo apt-get install golang-go`.
3. Skapa en mapp för ditt projekt i arbetskatalogen, t.ex `mkdir -p ~/go/src/postgresqlgo/`.
4. Ändra katalogen till hello mapp som `cd ~/go/src/postgresqlgo/`.
5. Ange hello GOPATH miljö variabeln toopoint tooa giltig källa katalog, t.ex ditt aktuella hem katalogens gå mapp. Kör vid hello bash shell `export GOPATH=~/go` tooadd hello går directory som hello GOPATH för hello shell-sessionen.
6. Installera hello [ren gå Postgres drivrutin (pq)](https://github.com/lib/pq) genom att köra hello `go get github.com/lib/pq` kommando.

   Sammanfattningsvis ska du köra dessa bash-kommandon:
   ```bash
   sudo apt-get install golang-go
   mkdir -p ~/go/src/postgresqlgo/
   cd ~/go/src/postgresqlgo/
   export GOPATH=~/go/
   go get github.com/lib/pq
   ```

### <a name="apple-macos"></a>Apple macOS
1. Hämta och installera gå enligt toohello [Installationsinstruktioner](https://golang.org/doc/install) matchar din plattform. 
2. Starta hello Bash-gränssnitt. 
3. Skapa en mapp för ditt projekt i arbetskatalogen, t.ex `mkdir -p ~/go/src/postgresqlgo/`.
4. Ändra katalogen till hello mapp som `cd ~/go/src/postgresqlgo/`.
5. Ange hello GOPATH miljö variabeln toopoint tooa giltig källa katalog, t.ex ditt aktuella hem katalogens gå mapp. Kör vid hello bash shell `export GOPATH=~/go` tooadd hello går directory som hello GOPATH för hello shell-sessionen.
6. Installera hello [ren gå Postgres drivrutin (pq)](https://github.com/lib/pq) genom att köra hello `go get github.com/lib/pq` kommando.

   Sammanfattningsvis ska du installera Go och sedan köra dessa bash-kommandon:
   ```bash
   mkdir -p ~/go/src/postgresqlgo/
   cd ~/go/src/postgresqlgo/
   export GOPATH=~/go/
   go get github.com/lib/pq
   ```

## <a name="get-connection-information"></a>Hämta anslutningsinformation
Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för PostgreSQL. Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Hello vänstra menyn i Azure-portalen klickar du på **alla resurser** och Sök efter hello-server som du har skapat, exempelvis **mypgserver 20170401**.
3. Klicka på servernamnet för hello **mypgserver 20170401**.
4. Välj hello server **översikt** sidan. Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.
 ![Azure Database för PostgreSQL – inloggning för serveradministratör](./media/connect-go/1-connection-string.png)
5. Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sida och visa hello admin serverinloggningsnamnet. Om det behövs, Återställ hello lösenord.

## <a name="build-and-run-go-code"></a>Skapa och köra Go-kod 
1. toowrite Golang kod som du kan använda en enkel textredigerare, till exempel Anteckningar i Microsoft Windows [vi](http://manpages.ubuntu.com/manpages/xenial/man1/nvi.1.html#contenttoc5) eller [Nano](https://www.nano-editor.org/) i Ubuntu eller TextEdit i macOS. Om du föredrar en mer omfattande IDE (Interactive Development Environment) kan du prova [Gogland](https://www.jetbrains.com/go/) från Jetbrains, [Visual Studio Code](https://code.visualstudio.com/) från Microsoft eller [Atom](https://atom.io/).
2. Klistra in hello Golang kod från hello avsnitt nedan i textfiler och spara i projektmappen med filnamnstillägget \*.go, till exempel Windows sökväg `%USERPROFILE%\go\src\postgresqlgo\createtable.go` eller Linux-sökvägen `~/go/src/postgresqlgo/createtable.go`.
3. Leta upp hello `HOST`, `DATABASE`, `USER`, och `PASSWORD` konstanter i hello kod och Ersätt hello exempelvärden med egna värden.  
4. Starta Kommandotolken hello eller bash shell. Ändra katalog till din projektmapp. I Windows kan du till exempel använda `cd %USERPROFILE%\go\src\postgresqlgo\`. I Linux kan du använda `cd ~/go/src/postgresqlgo/`. Vissa av hello IDE miljöer nämns erbjuder funktioner för felsökning och körning utan shell-kommandon.
5. Köra hello kod genom att ange hello kommando `go run createtable.go` toocompile hello programmet och kör den. 
6. Alternativt toobuild hello kod i en programspecifika `go build createtable.go`, starta `createtable.exe` toorun hello program.

## <a name="connect-and-create-a-table"></a>Ansluta och skapa en tabell
Använd hello följande kod tooconnect och skapa en tabell med hjälp av **CREATE TABLE** SQL-instruktionen, följt av **INSERT INTO** SQL-instruktioner tooadd rader i hello-tabellen.

hello kod importerar tre paket: hello [sql paketet](https://golang.org/pkg/database/sql/), hello [pq paketet](http://godoc.org/github.com/lib/pq) som en drivrutin toocommunicate med hello Postgres server och hello [fmt paketet](https://golang.org/pkg/fmt/) för ut inkommande och utgående på hello-kommandoraden.

hello koden anropar metoden [sql. Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure databas för PostgreSQL och kontrollerar hello anslutning med hjälp av metoden [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping). En [referensen](https://golang.org/pkg/database/sql/#DB) används i hela, hålla hello anslutningspoolen för hello databasserver. hello koden anropar hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metoden flera gånger toorun flera SQL-kommandon. Varje gång en anpassad checkError() metoden toocheck om ett fel uppstod och oroa dig tooexit om ett fel inträffar.

Ersätt hello `HOST`, `DATABASE`, `USER`, och `PASSWORD` parametrar med egna värden. 

```go
package main

import (
    "database/sql"
    "fmt"
    _ "github.com/lib/pq"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {
    // Initialize connection string.
    var connectionString string = fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection toodatabase")

    // Drop previous table of same name if one exists.
    _, err = db.Exec("DROP TABLE IF EXISTS inventory;")
    checkError(err)
    fmt.Println("Finished dropping table (if existed)")

    // Create table.
    _, err = db.Exec("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
    checkError(err)
    fmt.Println("Finished creating table")

    // Insert some data into table.
    sql_statement := "INSERT INTO inventory (name, quantity) VALUES ($1, $2);"
    _, err = db.Exec(sql_statement, "banana", 150)
    checkError(err)
    _, err = db.Exec(sql_statement, "orange", 154)
    checkError(err)
    _, err = db.Exec(sql_statement, "apple", 100)
    checkError(err)
    fmt.Println("Inserted 3 rows of data")
}
```

## <a name="read-data"></a>Läsa data
Använd hello följande kod tooconnect och läsa hello data med hjälp av en **Välj** SQL-instruktionen. 

hello kod importerar tre paket: hello [sql paketet](https://golang.org/pkg/database/sql/), hello [pq paketet](http://godoc.org/github.com/lib/pq) som en drivrutin toocommunicate med hello Postgres server och hello [fmt paketet](https://golang.org/pkg/fmt/) för ut inkommande och utgående på hello-kommandoraden.

hello koden anropar metoden [sql. Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure databas för PostgreSQL och kontrollerar hello anslutning med hjälp av metoden [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping). En [referensen](https://golang.org/pkg/database/sql/#DB) används i hela, hålla hello anslutningspoolen för hello databasserver. hello select-frågan körs genom att anropa metoden [db. Query()](https://golang.org/pkg/database/sql/#DB.Query), och hålls hello resulterande rader i en variabel av typen [rader](https://golang.org/pkg/database/sql/#Rows). hello koden läser hello data kolumnvärdena hello aktuell rad med metoden [rader. Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) och slingor över hello rader med hello iterator [rader. Efter](https://golang.org/pkg/database/sql/#Rows.Next) tills det finns inga fler rader. Varje rad kolumnvärdena är tryckt toohello konsolen ut. Varje gång en anpassad checkError() metoden toocheck om ett fel uppstod och oroa dig tooexit om ett fel inträffar.

Ersätt hello `HOST`, `DATABASE`, `USER`, och `PASSWORD` parametrar med egna värden. 

```go
package main

import (
    "database/sql"
    "fmt"
    _ "github.com/lib/pq"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString string = fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection toodatabase")

    // Read rows from table.
    var id int
    var name string
    var quantity int

    sql_statement := "SELECT * from inventory;"
    rows, err := db.Query(sql_statement)
    checkError(err)

    for rows.Next() {
        switch err := rows.Scan(&id, &name, &quantity); err {
        case sql.ErrNoRows:
            fmt.Println("No rows were returned")
        case nil:
            fmt.Printf("Data row = (%d, %s, %d)\n", id, name, quantity)
        default:
            checkError(err)
        }
    }
}
```

## <a name="update-data"></a>Uppdatera data
Använd hello följande kod tooconnect och uppdatera hello data med hjälp av en **uppdatera** SQL-instruktionen.

hello kod importerar tre paket: hello [sql paketet](https://golang.org/pkg/database/sql/), hello [pq paketet](http://godoc.org/github.com/lib/pq) som en drivrutin toocommunicate med hello Postgres server och hello [fmt paketet](https://golang.org/pkg/fmt/) för ut inkommande och utgående på hello-kommandoraden.

hello koden anropar metoden [sql. Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure databas för PostgreSQL och kontrollerar hello anslutning med hjälp av metoden [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping). En [referensen](https://golang.org/pkg/database/sql/#DB) används i hela, hålla hello anslutningspoolen för hello databasserver. hello koden anropar hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metoden toorun hello SQL-uttryck som uppdaterar hello tabell. En anpassad checkError() metoden toocheck inträffar om ett fel uppstod och oroa dig tooexit om ett fel.

Ersätt hello `HOST`, `DATABASE`, `USER`, och `PASSWORD` parametrar med egna värden. 
```go
package main

import (
  "database/sql"
  _ "github.com/lib/pq"
  "fmt"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {
    
    // Initialize connection string.
    var connectionString string = 
        fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection toodatabase")

    // Modify some data in table.
    sql_statement := "UPDATE inventory SET quantity = $2 WHERE name = $1;"
    _, err = db.Exec(sql_statement, "banana", 200)
    checkError(err)
    fmt.Println("Updated 1 row of data")
}
```

## <a name="delete-data"></a>Ta bort data
Använd hello följande kod tooconnect och läsa hello data med hjälp av en **ta bort** SQL-instruktionen. 

hello kod importerar tre paket: hello [sql paketet](https://golang.org/pkg/database/sql/), hello [pq paketet](http://godoc.org/github.com/lib/pq) som en drivrutin toocommunicate med hello Postgres server och hello [fmt paketet](https://golang.org/pkg/fmt/) för ut inkommande och utgående på hello-kommandoraden.

hello koden anropar metoden [sql. Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure databas för PostgreSQL och kontrollerar hello anslutning med hjälp av metoden [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping). En [referensen](https://golang.org/pkg/database/sql/#DB) används i hela, hålla hello anslutningspoolen för hello databasserver. hello koden anropar hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metoden toorun hello SQL-uttryck som uppdaterar hello tabell. En anpassad checkError() metoden toocheck inträffar om ett fel uppstod och oroa dig tooexit om ett fel.

Ersätt hello `HOST`, `DATABASE`, `USER`, och `PASSWORD` parametrar med egna värden. 
```go
package main

import (
  "database/sql"
  _ "github.com/lib/pq"
  "fmt"
)

const (
    // Initialize connection constants.
    HOST     = "mypgserver-20170401.postgres.database.azure.com"
    DATABASE = "mypgsqldb"
    USER     = "mylogin@mypgserver-20170401"
    PASSWORD = "<server_admin_password>"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {
    
    // Initialize connection string.
    var connectionString string = 
        fmt.Sprintf("host=%s user=%s password=%s dbname=%s sslmode=require", HOST, USER, PASSWORD, DATABASE)

    // Initialize connection object.
    db, err := sql.Open("postgres", connectionString)
    checkError(err)

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection toodatabase")

    // Delete some data from table.
    sql_statement := "DELETE FROM inventory WHERE name = $1;"
    _, err = db.Exec(sql_statement, "orange")
    checkError(err)
    fmt.Println("Deleted 1 row of data")
}
```

## <a name="next-steps"></a>Nästa steg
> [!div class="nextstepaction"]
> [Migrera din databas med Exportera och importera](./howto-migrate-using-export-and-import.md)
