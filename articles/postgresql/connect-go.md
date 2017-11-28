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
# <a name="azure-database-for-postgresql-use-go-language-tooconnect-and-query-data"></a><span data-ttu-id="0479e-103">Azure-databas för PostgreSQL: använda gå språk tooconnect och fråga data</span><span class="sxs-lookup"><span data-stu-id="0479e-103">Azure Database for PostgreSQL: Use Go language tooconnect and query data</span></span>
<span data-ttu-id="0479e-104">Den här snabbstarten visar hur tooconnect tooan Azure Database för att använda PostgreSQL code skriven i hello [Gå](https://golang.org/) språk (golang).</span><span class="sxs-lookup"><span data-stu-id="0479e-104">This quickstart demonstrates how tooconnect tooan Azure Database for PostgreSQL using code written in hello [Go](https://golang.org/) language (golang).</span></span> <span data-ttu-id="0479e-105">Den visar hur toouse SQL-instruktioner tooquery infoga, uppdatera och ta bort data i hello-databas.</span><span class="sxs-lookup"><span data-stu-id="0479e-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="0479e-106">Den här artikeln förutsätter att du är bekant med utveckling med hjälp av gå, men som du är ny tooworking med Azure-databas för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="0479e-106">This article assumes you are familiar with development using Go, but that you are new tooworking with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0479e-107">Krav</span><span class="sxs-lookup"><span data-stu-id="0479e-107">Prerequisites</span></span>
<span data-ttu-id="0479e-108">Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="0479e-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="0479e-109">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="0479e-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="0479e-110">Skapa DB – Azure CLI</span><span class="sxs-lookup"><span data-stu-id="0479e-110">Create DB - Azure CLI</span></span>](quickstart-create-server-database-azure-cli.md)

## <a name="install-go-and-pq-connector"></a><span data-ttu-id="0479e-111">Installera Go och pq connector</span><span class="sxs-lookup"><span data-stu-id="0479e-111">Install Go and pq connector</span></span>
<span data-ttu-id="0479e-112">Installera [Gå](https://golang.org/doc/install) och hello [ren gå Postgres drivrutin (pq)](https://github.com/lib/pq) på din egen dator.</span><span class="sxs-lookup"><span data-stu-id="0479e-112">Install [Go](https://golang.org/doc/install) and hello [Pure Go Postgres driver (pq)](https://github.com/lib/pq) on your own machine.</span></span> <span data-ttu-id="0479e-113">Beroende på din plattform gör hello:</span><span class="sxs-lookup"><span data-stu-id="0479e-113">Depending on your platform, follow hello steps:</span></span>

### <a name="windows"></a><span data-ttu-id="0479e-114">Windows</span><span class="sxs-lookup"><span data-stu-id="0479e-114">Windows</span></span>
1. <span data-ttu-id="0479e-115">[Hämta](https://golang.org/dl/) och installera gå för Microsoft Windows enligt toohello [Installationsinstruktioner](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="0479e-115">[Download](https://golang.org/dl/) and install Go for Microsoft Windows according toohello [installation instructions](https://golang.org/doc/install).</span></span>
2. <span data-ttu-id="0479e-116">Starta hello kommandotolk från hello start-menyn.</span><span class="sxs-lookup"><span data-stu-id="0479e-116">Launch hello command prompt from hello start menu.</span></span>
3. <span data-ttu-id="0479e-117">Skapa en mapp för ditt projekt, till exempel.</span><span class="sxs-lookup"><span data-stu-id="0479e-117">Make a folder for your project such.</span></span> <span data-ttu-id="0479e-118">`mkdir  %USERPROFILE%\go\src\postgresqlgo`.</span><span class="sxs-lookup"><span data-stu-id="0479e-118">`mkdir  %USERPROFILE%\go\src\postgresqlgo`.</span></span>
4. <span data-ttu-id="0479e-119">Ändra katalogen till hello projektmappen som `cd %USERPROFILE%\go\src\postgresqlgo`.</span><span class="sxs-lookup"><span data-stu-id="0479e-119">Change directory into hello project folder, such as `cd %USERPROFILE%\go\src\postgresqlgo`.</span></span>
5. <span data-ttu-id="0479e-120">Ange hello miljövariabeln efter koden för GOPATH toopoint toohello källkatalogen.</span><span class="sxs-lookup"><span data-stu-id="0479e-120">Set hello environment variable for GOPATH toopoint toohello source code directory.</span></span> <span data-ttu-id="0479e-121">`set GOPATH=%USERPROFILE%\go`.</span><span class="sxs-lookup"><span data-stu-id="0479e-121">`set GOPATH=%USERPROFILE%\go`.</span></span>
6. <span data-ttu-id="0479e-122">Installera hello [ren gå Postgres drivrutin (pq)](https://github.com/lib/pq) genom att köra hello `go get github.com/lib/pq` kommando.</span><span class="sxs-lookup"><span data-stu-id="0479e-122">Install hello [Pure Go Postgres driver (pq)](https://github.com/lib/pq) by running hello `go get github.com/lib/pq` command.</span></span>

   <span data-ttu-id="0479e-123">Installera gå i sammanfattningen, och sedan köra dessa kommandon i Kommandotolken för hello:</span><span class="sxs-lookup"><span data-stu-id="0479e-123">In summary, install Go, then run these commands in hello command prompt:</span></span>
   ```cmd
   mkdir  %USERPROFILE%\go\src\postgresqlgo
   cd %USERPROFILE%\go\src\postgresqlgo
   set GOPATH=%USERPROFILE%\go
   go get github.com/lib/pq
   ```

### <a name="linux-ubuntu"></a><span data-ttu-id="0479e-124">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="0479e-124">Linux (Ubuntu)</span></span>
1. <span data-ttu-id="0479e-125">Starta hello Bash-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="0479e-125">Launch hello Bash shell.</span></span> 
2. <span data-ttu-id="0479e-126">Installera Go genom att köra `sudo apt-get install golang-go`.</span><span class="sxs-lookup"><span data-stu-id="0479e-126">Install Go by running `sudo apt-get install golang-go`.</span></span>
3. <span data-ttu-id="0479e-127">Skapa en mapp för ditt projekt i arbetskatalogen, t.ex `mkdir -p ~/go/src/postgresqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="0479e-127">Make a folder for your project in your home directory, such as `mkdir -p ~/go/src/postgresqlgo/`.</span></span>
4. <span data-ttu-id="0479e-128">Ändra katalogen till hello mapp som `cd ~/go/src/postgresqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="0479e-128">Change directory into hello folder, such as `cd ~/go/src/postgresqlgo/`.</span></span>
5. <span data-ttu-id="0479e-129">Ange hello GOPATH miljö variabeln toopoint tooa giltig källa katalog, t.ex ditt aktuella hem katalogens gå mapp.</span><span class="sxs-lookup"><span data-stu-id="0479e-129">Set hello GOPATH environment variable toopoint tooa valid source directory, such as your current home directory's go folder.</span></span> <span data-ttu-id="0479e-130">Kör vid hello bash shell `export GOPATH=~/go` tooadd hello går directory som hello GOPATH för hello shell-sessionen.</span><span class="sxs-lookup"><span data-stu-id="0479e-130">At hello bash shell, run `export GOPATH=~/go` tooadd hello go directory as hello GOPATH for hello current shell session.</span></span>
6. <span data-ttu-id="0479e-131">Installera hello [ren gå Postgres drivrutin (pq)](https://github.com/lib/pq) genom att köra hello `go get github.com/lib/pq` kommando.</span><span class="sxs-lookup"><span data-stu-id="0479e-131">Install hello [Pure Go Postgres driver (pq)](https://github.com/lib/pq) by running hello `go get github.com/lib/pq` command.</span></span>

   <span data-ttu-id="0479e-132">Sammanfattningsvis ska du köra dessa bash-kommandon:</span><span class="sxs-lookup"><span data-stu-id="0479e-132">In summary, run these bash commands:</span></span>
   ```bash
   sudo apt-get install golang-go
   mkdir -p ~/go/src/postgresqlgo/
   cd ~/go/src/postgresqlgo/
   export GOPATH=~/go/
   go get github.com/lib/pq
   ```

### <a name="apple-macos"></a><span data-ttu-id="0479e-133">Apple macOS</span><span class="sxs-lookup"><span data-stu-id="0479e-133">Apple macOS</span></span>
1. <span data-ttu-id="0479e-134">Hämta och installera gå enligt toohello [Installationsinstruktioner](https://golang.org/doc/install) matchar din plattform.</span><span class="sxs-lookup"><span data-stu-id="0479e-134">Download and install Go according toohello [installation instructions](https://golang.org/doc/install)  matching your platform.</span></span> 
2. <span data-ttu-id="0479e-135">Starta hello Bash-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="0479e-135">Launch hello Bash shell.</span></span> 
3. <span data-ttu-id="0479e-136">Skapa en mapp för ditt projekt i arbetskatalogen, t.ex `mkdir -p ~/go/src/postgresqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="0479e-136">Make a folder for your project in your home directory, such as `mkdir -p ~/go/src/postgresqlgo/`.</span></span>
4. <span data-ttu-id="0479e-137">Ändra katalogen till hello mapp som `cd ~/go/src/postgresqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="0479e-137">Change directory into hello folder, such as `cd ~/go/src/postgresqlgo/`.</span></span>
5. <span data-ttu-id="0479e-138">Ange hello GOPATH miljö variabeln toopoint tooa giltig källa katalog, t.ex ditt aktuella hem katalogens gå mapp.</span><span class="sxs-lookup"><span data-stu-id="0479e-138">Set hello GOPATH environment variable toopoint tooa valid source directory, such as your current home directory's go folder.</span></span> <span data-ttu-id="0479e-139">Kör vid hello bash shell `export GOPATH=~/go` tooadd hello går directory som hello GOPATH för hello shell-sessionen.</span><span class="sxs-lookup"><span data-stu-id="0479e-139">At hello bash shell, run `export GOPATH=~/go` tooadd hello go directory as hello GOPATH for hello current shell session.</span></span>
6. <span data-ttu-id="0479e-140">Installera hello [ren gå Postgres drivrutin (pq)](https://github.com/lib/pq) genom att köra hello `go get github.com/lib/pq` kommando.</span><span class="sxs-lookup"><span data-stu-id="0479e-140">Install hello [Pure Go Postgres driver (pq)](https://github.com/lib/pq) by running hello `go get github.com/lib/pq` command.</span></span>

   <span data-ttu-id="0479e-141">Sammanfattningsvis ska du installera Go och sedan köra dessa bash-kommandon:</span><span class="sxs-lookup"><span data-stu-id="0479e-141">In summary, install Go, then run these bash commands:</span></span>
   ```bash
   mkdir -p ~/go/src/postgresqlgo/
   cd ~/go/src/postgresqlgo/
   export GOPATH=~/go/
   go get github.com/lib/pq
   ```

## <a name="get-connection-information"></a><span data-ttu-id="0479e-142">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="0479e-142">Get connection information</span></span>
<span data-ttu-id="0479e-143">Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="0479e-143">Get hello connection information needed tooconnect toohello Azure Database for PostgreSQL.</span></span> <span data-ttu-id="0479e-144">Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.</span><span class="sxs-lookup"><span data-stu-id="0479e-144">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="0479e-145">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="0479e-145">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="0479e-146">Hello vänstra menyn i Azure-portalen klickar du på **alla resurser** och Sök efter hello-server som du har skapat, exempelvis **mypgserver 20170401**.</span><span class="sxs-lookup"><span data-stu-id="0479e-146">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have created, such as **mypgserver-20170401**.</span></span>
3. <span data-ttu-id="0479e-147">Klicka på servernamnet för hello **mypgserver 20170401**.</span><span class="sxs-lookup"><span data-stu-id="0479e-147">Click hello server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="0479e-148">Välj hello server **översikt** sidan.</span><span class="sxs-lookup"><span data-stu-id="0479e-148">Select hello server's **Overview** page.</span></span> <span data-ttu-id="0479e-149">Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.</span><span class="sxs-lookup"><span data-stu-id="0479e-149">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="0479e-150">![Azure Database för PostgreSQL – inloggning för serveradministratör](./media/connect-go/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="0479e-150">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-go/1-connection-string.png)</span></span>
5. <span data-ttu-id="0479e-151">Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sida och visa hello admin serverinloggningsnamnet.</span><span class="sxs-lookup"><span data-stu-id="0479e-151">If you forget your server login information, navigate toohello **Overview** page, and view hello Server admin login name.</span></span> <span data-ttu-id="0479e-152">Om det behövs, Återställ hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="0479e-152">If necessary, reset hello password.</span></span>

## <a name="build-and-run-go-code"></a><span data-ttu-id="0479e-153">Skapa och köra Go-kod</span><span class="sxs-lookup"><span data-stu-id="0479e-153">Build and run Go code</span></span> 
1. <span data-ttu-id="0479e-154">toowrite Golang kod som du kan använda en enkel textredigerare, till exempel Anteckningar i Microsoft Windows [vi](http://manpages.ubuntu.com/manpages/xenial/man1/nvi.1.html#contenttoc5) eller [Nano](https://www.nano-editor.org/) i Ubuntu eller TextEdit i macOS.</span><span class="sxs-lookup"><span data-stu-id="0479e-154">toowrite Golang code, you can use a simple text editor, such as Notepad in Microsoft Windows, [vi](http://manpages.ubuntu.com/manpages/xenial/man1/nvi.1.html#contenttoc5) or [Nano](https://www.nano-editor.org/) in Ubuntu, or TextEdit in macOS.</span></span> <span data-ttu-id="0479e-155">Om du föredrar en mer omfattande IDE (Interactive Development Environment) kan du prova [Gogland](https://www.jetbrains.com/go/) från Jetbrains, [Visual Studio Code](https://code.visualstudio.com/) från Microsoft eller [Atom](https://atom.io/).</span><span class="sxs-lookup"><span data-stu-id="0479e-155">If you prefer a richer Interactive Development Environment (IDE) try [Gogland](https://www.jetbrains.com/go/) by Jetbrains, [Visual Studio Code](https://code.visualstudio.com/) by Microsoft, or [Atom](https://atom.io/).</span></span>
2. <span data-ttu-id="0479e-156">Klistra in hello Golang kod från hello avsnitt nedan i textfiler och spara i projektmappen med filnamnstillägget \*.go, till exempel Windows sökväg `%USERPROFILE%\go\src\postgresqlgo\createtable.go` eller Linux-sökvägen `~/go/src/postgresqlgo/createtable.go`.</span><span class="sxs-lookup"><span data-stu-id="0479e-156">Paste hello Golang code from hello sections below into text files, and save into your project folder with file extension \*.go, such as Windows path `%USERPROFILE%\go\src\postgresqlgo\createtable.go` or Linux path `~/go/src/postgresqlgo/createtable.go`.</span></span>
3. <span data-ttu-id="0479e-157">Leta upp hello `HOST`, `DATABASE`, `USER`, och `PASSWORD` konstanter i hello kod och Ersätt hello exempelvärden med egna värden.</span><span class="sxs-lookup"><span data-stu-id="0479e-157">Locate hello `HOST`, `DATABASE`, `USER`, and `PASSWORD` constants in hello code, and replace hello example values with your own values.</span></span>  
4. <span data-ttu-id="0479e-158">Starta Kommandotolken hello eller bash shell.</span><span class="sxs-lookup"><span data-stu-id="0479e-158">Launch hello command prompt or bash shell.</span></span> <span data-ttu-id="0479e-159">Ändra katalog till din projektmapp.</span><span class="sxs-lookup"><span data-stu-id="0479e-159">Change directory into your project folder.</span></span> <span data-ttu-id="0479e-160">I Windows kan du till exempel använda `cd %USERPROFILE%\go\src\postgresqlgo\`.</span><span class="sxs-lookup"><span data-stu-id="0479e-160">For example, on Windows `cd %USERPROFILE%\go\src\postgresqlgo\`.</span></span> <span data-ttu-id="0479e-161">I Linux kan du använda `cd ~/go/src/postgresqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="0479e-161">On Linux `cd ~/go/src/postgresqlgo/`.</span></span> <span data-ttu-id="0479e-162">Vissa av hello IDE miljöer nämns erbjuder funktioner för felsökning och körning utan shell-kommandon.</span><span class="sxs-lookup"><span data-stu-id="0479e-162">Some of hello IDE environments mentioned offer debug and runtime capabilities without requiring shell commands.</span></span>
5. <span data-ttu-id="0479e-163">Köra hello kod genom att ange hello kommando `go run createtable.go` toocompile hello programmet och kör den.</span><span class="sxs-lookup"><span data-stu-id="0479e-163">Run hello code by typing hello command `go run createtable.go` toocompile hello application and run it.</span></span> 
6. <span data-ttu-id="0479e-164">Alternativt toobuild hello kod i en programspecifika `go build createtable.go`, starta `createtable.exe` toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="0479e-164">Alternatively, toobuild hello code into a native application, `go build createtable.go`, then launch `createtable.exe` toorun hello application.</span></span>

## <a name="connect-and-create-a-table"></a><span data-ttu-id="0479e-165">Ansluta och skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="0479e-165">Connect and create a table</span></span>
<span data-ttu-id="0479e-166">Använd hello följande kod tooconnect och skapa en tabell med hjälp av **CREATE TABLE** SQL-instruktionen, följt av **INSERT INTO** SQL-instruktioner tooadd rader i hello-tabellen.</span><span class="sxs-lookup"><span data-stu-id="0479e-166">Use hello following code tooconnect and create a table using **CREATE TABLE** SQL statement, followed by **INSERT INTO** SQL statements tooadd rows into hello table.</span></span>

<span data-ttu-id="0479e-167">hello kod importerar tre paket: hello [sql paketet](https://golang.org/pkg/database/sql/), hello [pq paketet](http://godoc.org/github.com/lib/pq) som en drivrutin toocommunicate med hello Postgres server och hello [fmt paketet](https://golang.org/pkg/fmt/) för ut inkommande och utgående på hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="0479e-167">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [pq package](http://godoc.org/github.com/lib/pq) as a driver toocommunicate with hello Postgres server, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="0479e-168">hello koden anropar metoden [sql. Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure databas för PostgreSQL och kontrollerar hello anslutning med hjälp av metoden [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="0479e-168">hello code calls method [sql.Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure Database for PostgreSQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="0479e-169">En [referensen](https://golang.org/pkg/database/sql/#DB) används i hela, hålla hello anslutningspoolen för hello databasserver.</span><span class="sxs-lookup"><span data-stu-id="0479e-169">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="0479e-170">hello koden anropar hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metoden flera gånger toorun flera SQL-kommandon.</span><span class="sxs-lookup"><span data-stu-id="0479e-170">hello code calls hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method several times toorun several SQL commands.</span></span> <span data-ttu-id="0479e-171">Varje gång en anpassad checkError() metoden toocheck om ett fel uppstod och oroa dig tooexit om ett fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="0479e-171">Each time a custom checkError() method toocheck if an error occurred and panic tooexit if an error occurs.</span></span>

<span data-ttu-id="0479e-172">Ersätt hello `HOST`, `DATABASE`, `USER`, och `PASSWORD` parametrar med egna värden.</span><span class="sxs-lookup"><span data-stu-id="0479e-172">Replace hello `HOST`, `DATABASE`, `USER`, and `PASSWORD` parameters with your own values.</span></span> 

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

## <a name="read-data"></a><span data-ttu-id="0479e-173">Läsa data</span><span class="sxs-lookup"><span data-stu-id="0479e-173">Read data</span></span>
<span data-ttu-id="0479e-174">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **Välj** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="0479e-174">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="0479e-175">hello kod importerar tre paket: hello [sql paketet](https://golang.org/pkg/database/sql/), hello [pq paketet](http://godoc.org/github.com/lib/pq) som en drivrutin toocommunicate med hello Postgres server och hello [fmt paketet](https://golang.org/pkg/fmt/) för ut inkommande och utgående på hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="0479e-175">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [pq package](http://godoc.org/github.com/lib/pq) as a driver toocommunicate with hello Postgres server, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="0479e-176">hello koden anropar metoden [sql. Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure databas för PostgreSQL och kontrollerar hello anslutning med hjälp av metoden [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="0479e-176">hello code calls method [sql.Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure Database for PostgreSQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="0479e-177">En [referensen](https://golang.org/pkg/database/sql/#DB) används i hela, hålla hello anslutningspoolen för hello databasserver.</span><span class="sxs-lookup"><span data-stu-id="0479e-177">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="0479e-178">hello select-frågan körs genom att anropa metoden [db. Query()](https://golang.org/pkg/database/sql/#DB.Query), och hålls hello resulterande rader i en variabel av typen [rader](https://golang.org/pkg/database/sql/#Rows).</span><span class="sxs-lookup"><span data-stu-id="0479e-178">hello select query is run by calling method [db.Query()](https://golang.org/pkg/database/sql/#DB.Query), and hello resulting rows is kept in a variable of type [rows](https://golang.org/pkg/database/sql/#Rows).</span></span> <span data-ttu-id="0479e-179">hello koden läser hello data kolumnvärdena hello aktuell rad med metoden [rader. Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) och slingor över hello rader med hello iterator [rader. Efter](https://golang.org/pkg/database/sql/#Rows.Next) tills det finns inga fler rader.</span><span class="sxs-lookup"><span data-stu-id="0479e-179">hello code reads hello column data values in hello current row using method [rows.Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) and loops over hello rows using hello iterator [rows.Next()](https://golang.org/pkg/database/sql/#Rows.Next) until no more rows exist.</span></span> <span data-ttu-id="0479e-180">Varje rad kolumnvärdena är tryckt toohello konsolen ut. Varje gång en anpassad checkError() metoden toocheck om ett fel uppstod och oroa dig tooexit om ett fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="0479e-180">Each row's column values are printed toohello console out. Each time a custom checkError() method toocheck if an error occurred and panic tooexit if an error occurs.</span></span>

<span data-ttu-id="0479e-181">Ersätt hello `HOST`, `DATABASE`, `USER`, och `PASSWORD` parametrar med egna värden.</span><span class="sxs-lookup"><span data-stu-id="0479e-181">Replace hello `HOST`, `DATABASE`, `USER`, and `PASSWORD` parameters with your own values.</span></span> 

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

## <a name="update-data"></a><span data-ttu-id="0479e-182">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="0479e-182">Update data</span></span>
<span data-ttu-id="0479e-183">Använd hello följande kod tooconnect och uppdatera hello data med hjälp av en **uppdatera** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="0479e-183">Use hello following code tooconnect and update hello data using a **UPDATE** SQL statement.</span></span>

<span data-ttu-id="0479e-184">hello kod importerar tre paket: hello [sql paketet](https://golang.org/pkg/database/sql/), hello [pq paketet](http://godoc.org/github.com/lib/pq) som en drivrutin toocommunicate med hello Postgres server och hello [fmt paketet](https://golang.org/pkg/fmt/) för ut inkommande och utgående på hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="0479e-184">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [pq package](http://godoc.org/github.com/lib/pq) as a driver toocommunicate with hello Postgres server, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="0479e-185">hello koden anropar metoden [sql. Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure databas för PostgreSQL och kontrollerar hello anslutning med hjälp av metoden [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="0479e-185">hello code calls method [sql.Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure Database for PostgreSQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="0479e-186">En [referensen](https://golang.org/pkg/database/sql/#DB) används i hela, hålla hello anslutningspoolen för hello databasserver.</span><span class="sxs-lookup"><span data-stu-id="0479e-186">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="0479e-187">hello koden anropar hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metoden toorun hello SQL-uttryck som uppdaterar hello tabell.</span><span class="sxs-lookup"><span data-stu-id="0479e-187">hello code calls hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method toorun hello SQL statement that updates hello table.</span></span> <span data-ttu-id="0479e-188">En anpassad checkError() metoden toocheck inträffar om ett fel uppstod och oroa dig tooexit om ett fel.</span><span class="sxs-lookup"><span data-stu-id="0479e-188">A custom checkError() method toocheck if an error occurred and panic tooexit if an error occurs.</span></span>

<span data-ttu-id="0479e-189">Ersätt hello `HOST`, `DATABASE`, `USER`, och `PASSWORD` parametrar med egna värden.</span><span class="sxs-lookup"><span data-stu-id="0479e-189">Replace hello `HOST`, `DATABASE`, `USER`, and `PASSWORD` parameters with your own values.</span></span> 
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

## <a name="delete-data"></a><span data-ttu-id="0479e-190">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="0479e-190">Delete data</span></span>
<span data-ttu-id="0479e-191">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **ta bort** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="0479e-191">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="0479e-192">hello kod importerar tre paket: hello [sql paketet](https://golang.org/pkg/database/sql/), hello [pq paketet](http://godoc.org/github.com/lib/pq) som en drivrutin toocommunicate med hello Postgres server och hello [fmt paketet](https://golang.org/pkg/fmt/) för ut inkommande och utgående på hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="0479e-192">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [pq package](http://godoc.org/github.com/lib/pq) as a driver toocommunicate with hello Postgres server, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="0479e-193">hello koden anropar metoden [sql. Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure databas för PostgreSQL och kontrollerar hello anslutning med hjälp av metoden [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="0479e-193">hello code calls method [sql.Open()](http://godoc.org/github.com/lib/pq#Open) tooconnect tooAzure Database for PostgreSQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="0479e-194">En [referensen](https://golang.org/pkg/database/sql/#DB) används i hela, hålla hello anslutningspoolen för hello databasserver.</span><span class="sxs-lookup"><span data-stu-id="0479e-194">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="0479e-195">hello koden anropar hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metoden toorun hello SQL-uttryck som uppdaterar hello tabell.</span><span class="sxs-lookup"><span data-stu-id="0479e-195">hello code calls hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method toorun hello SQL statement that updates hello table.</span></span> <span data-ttu-id="0479e-196">En anpassad checkError() metoden toocheck inträffar om ett fel uppstod och oroa dig tooexit om ett fel.</span><span class="sxs-lookup"><span data-stu-id="0479e-196">A custom checkError() method toocheck if an error occurred and panic tooexit if an error occurs.</span></span>

<span data-ttu-id="0479e-197">Ersätt hello `HOST`, `DATABASE`, `USER`, och `PASSWORD` parametrar med egna värden.</span><span class="sxs-lookup"><span data-stu-id="0479e-197">Replace hello `HOST`, `DATABASE`, `USER`, and `PASSWORD` parameters with your own values.</span></span> 
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

## <a name="next-steps"></a><span data-ttu-id="0479e-198">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0479e-198">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="0479e-199">Migrera din databas med Exportera och importera</span><span class="sxs-lookup"><span data-stu-id="0479e-199">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
