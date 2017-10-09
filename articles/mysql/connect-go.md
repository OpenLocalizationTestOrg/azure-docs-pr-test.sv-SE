---
title: "Ansluta tooAzure databas för MySQL med gå | Microsoft Docs"
description: "Denna Snabbstart innehåller flera gå kodexempel du kan använda tooconnect och fråga efter data från Azure-databas för MySQL."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.devlang: go
ms.topic: hero-article
ms.date: 07/18/2017
ms.openlocfilehash: e8067b807ee729e04850c5325f476806bcd54983
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-go-language-tooconnect-and-query-data"></a><span data-ttu-id="6cde4-103">Azure-databas för MySQL: använda gå språk tooconnect och fråga data</span><span class="sxs-lookup"><span data-stu-id="6cde4-103">Azure Database for MySQL: Use Go language tooconnect and query data</span></span>
<span data-ttu-id="6cde4-104">Den här snabbstarten visar hur tooconnect tooan Azure Database för att använda MySQL code skriven i hello [Gå](https://golang.org/) språk från Windows-, Ubuntu Linux- och Apple macOS-plattformar.</span><span class="sxs-lookup"><span data-stu-id="6cde4-104">This quickstart demonstrates how tooconnect tooan Azure Database for MySQL using code written in hello [Go](https://golang.org/) language from Windows, Ubuntu Linux, and Apple macOS platforms.</span></span> <span data-ttu-id="6cde4-105">Den visar hur toouse SQL-instruktioner tooquery infoga, uppdatera och ta bort data i hello-databas.</span><span class="sxs-lookup"><span data-stu-id="6cde4-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="6cde4-106">Den här artikeln förutsätter att du är bekant med utveckling med hjälp av gå, men som du är ny tooworking med Azure-databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="6cde4-106">This article assumes you are familiar with development using Go, but that you are new tooworking with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6cde4-107">Krav</span><span class="sxs-lookup"><span data-stu-id="6cde4-107">Prerequisites</span></span>
<span data-ttu-id="6cde4-108">Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="6cde4-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="6cde4-109">Skapa en Azure Database för MySQL med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6cde4-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="6cde4-110">Skapa en Azure Database för MySQL-server med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6cde4-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-go-and-mysql-connector"></a><span data-ttu-id="6cde4-111">Installera en anslutningsapp för Go och MySQL</span><span class="sxs-lookup"><span data-stu-id="6cde4-111">Install Go and MySQL connector</span></span>
<span data-ttu-id="6cde4-112">Installera [Gå](https://golang.org/doc/install) och hello [gå-sql-drivrutin för MySQL](https://github.com/go-sql-driver/mysql#installation) på din egen dator.</span><span class="sxs-lookup"><span data-stu-id="6cde4-112">Install [Go](https://golang.org/doc/install) and hello [go-sql-driver for MySQL](https://github.com/go-sql-driver/mysql#installation) on your own machine.</span></span> <span data-ttu-id="6cde4-113">Beroende på din plattform gör hello:</span><span class="sxs-lookup"><span data-stu-id="6cde4-113">Depending on your platform, follow hello steps:</span></span>

### <a name="windows"></a><span data-ttu-id="6cde4-114">Windows</span><span class="sxs-lookup"><span data-stu-id="6cde4-114">Windows</span></span>
1. <span data-ttu-id="6cde4-115">[Hämta](https://golang.org/dl/) och installera gå för Microsoft Windows enligt toohello [Installationsinstruktioner](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="6cde4-115">[Download](https://golang.org/dl/) and install Go for Microsoft Windows according toohello [installation instructions](https://golang.org/doc/install).</span></span>
2. <span data-ttu-id="6cde4-116">Starta hello kommandotolk från hello start-menyn.</span><span class="sxs-lookup"><span data-stu-id="6cde4-116">Launch hello command prompt from hello start menu.</span></span>
3. <span data-ttu-id="6cde4-117">Skapa en mapp för ditt projekt, till exempel.</span><span class="sxs-lookup"><span data-stu-id="6cde4-117">Make a folder for your project such.</span></span> <span data-ttu-id="6cde4-118">`mkdir  %USERPROFILE%\go\src\mysqlgo`.</span><span class="sxs-lookup"><span data-stu-id="6cde4-118">`mkdir  %USERPROFILE%\go\src\mysqlgo`.</span></span>
4. <span data-ttu-id="6cde4-119">Ändra katalogen till hello projektmappen som `cd %USERPROFILE%\go\src\mysqlgo`.</span><span class="sxs-lookup"><span data-stu-id="6cde4-119">Change directory into hello project folder, such as `cd %USERPROFILE%\go\src\mysqlgo`.</span></span>
5. <span data-ttu-id="6cde4-120">Ange hello miljövariabeln efter koden för GOPATH toopoint toohello källkatalogen.</span><span class="sxs-lookup"><span data-stu-id="6cde4-120">Set hello environment variable for GOPATH toopoint toohello source code directory.</span></span> <span data-ttu-id="6cde4-121">`set GOPATH=%USERPROFILE%\go`.</span><span class="sxs-lookup"><span data-stu-id="6cde4-121">`set GOPATH=%USERPROFILE%\go`.</span></span>
6. <span data-ttu-id="6cde4-122">Installera hello [gå-sql-drivrutin för mysql](https://github.com/go-sql-driver/mysql#installation) genom att köra hello `go get github.com/go-sql-driver/mysql` kommando.</span><span class="sxs-lookup"><span data-stu-id="6cde4-122">Install hello [go-sql-driver for mysql](https://github.com/go-sql-driver/mysql#installation) by running hello `go get github.com/go-sql-driver/mysql` command.</span></span>

   <span data-ttu-id="6cde4-123">Installera gå i sammanfattningen, och sedan köra dessa kommandon i Kommandotolken för hello:</span><span class="sxs-lookup"><span data-stu-id="6cde4-123">In summary, install Go, then run these commands in hello command prompt:</span></span>
   ```cmd
   mkdir  %USERPROFILE%\go\src\mysqlgo
   cd %USERPROFILE%\go\src\mysqlgo
   set GOPATH=%USERPROFILE%\go
   go get github.com/go-sql-driver/mysql
   ```

### <a name="linux-ubuntu"></a><span data-ttu-id="6cde4-124">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="6cde4-124">Linux (Ubuntu)</span></span>
1. <span data-ttu-id="6cde4-125">Starta hello Bash-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="6cde4-125">Launch hello Bash shell.</span></span> 
2. <span data-ttu-id="6cde4-126">Installera Go genom att köra `sudo apt-get install golang-go`.</span><span class="sxs-lookup"><span data-stu-id="6cde4-126">Install Go by running `sudo apt-get install golang-go`.</span></span>
3. <span data-ttu-id="6cde4-127">Skapa en mapp för ditt projekt i arbetskatalogen, t.ex `mkdir -p ~/go/src/mysqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="6cde4-127">Make a folder for your project in your home directory, such as `mkdir -p ~/go/src/mysqlgo/`.</span></span>
4. <span data-ttu-id="6cde4-128">Ändra katalogen till hello mapp som `cd ~/go/src/mysqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="6cde4-128">Change directory into hello folder, such as `cd ~/go/src/mysqlgo/`.</span></span>
5. <span data-ttu-id="6cde4-129">Ange hello GOPATH miljö variabeln toopoint tooa giltig källa katalog, t.ex ditt aktuella hem katalogens gå mapp.</span><span class="sxs-lookup"><span data-stu-id="6cde4-129">Set hello GOPATH environment variable toopoint tooa valid source directory, such as your current home directory's go folder.</span></span> <span data-ttu-id="6cde4-130">Kör vid hello bash shell `export GOPATH=~/go` tooadd hello går directory som hello GOPATH för hello shell-sessionen.</span><span class="sxs-lookup"><span data-stu-id="6cde4-130">At hello bash shell, run `export GOPATH=~/go` tooadd hello go directory as hello GOPATH for hello current shell session.</span></span>
6. <span data-ttu-id="6cde4-131">Installera hello [gå-sql-drivrutin för mysql](https://github.com/go-sql-driver/mysql#installation) genom att köra hello `go get github.com/go-sql-driver/mysql` kommando.</span><span class="sxs-lookup"><span data-stu-id="6cde4-131">Install hello [go-sql-driver for mysql](https://github.com/go-sql-driver/mysql#installation) by running hello `go get github.com/go-sql-driver/mysql` command.</span></span>

   <span data-ttu-id="6cde4-132">Sammanfattningsvis ska du köra dessa bash-kommandon:</span><span class="sxs-lookup"><span data-stu-id="6cde4-132">In summary, run these bash commands:</span></span>
   ```bash
   sudo apt-get install golang-go
   mkdir -p ~/go/src/mysqlgo/
   cd ~/go/src/mysqlgo/
   export GOPATH=~/go/
   go get github.com/go-sql-driver/mysql
   ```

### <a name="apple-macos"></a><span data-ttu-id="6cde4-133">Apple macOS</span><span class="sxs-lookup"><span data-stu-id="6cde4-133">Apple macOS</span></span>
1. <span data-ttu-id="6cde4-134">Hämta och installera gå enligt toohello [Installationsinstruktioner](https://golang.org/doc/install) matchar din plattform.</span><span class="sxs-lookup"><span data-stu-id="6cde4-134">Download and install Go according toohello [installation instructions](https://golang.org/doc/install)  matching your platform.</span></span> 
2. <span data-ttu-id="6cde4-135">Starta hello Bash-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="6cde4-135">Launch hello Bash shell.</span></span> 
3. <span data-ttu-id="6cde4-136">Skapa en mapp för ditt projekt i arbetskatalogen, t.ex `mkdir -p ~/go/src/mysqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="6cde4-136">Make a folder for your project in your home directory, such as `mkdir -p ~/go/src/mysqlgo/`.</span></span>
4. <span data-ttu-id="6cde4-137">Ändra katalogen till hello mapp som `cd ~/go/src/mysqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="6cde4-137">Change directory into hello folder, such as `cd ~/go/src/mysqlgo/`.</span></span>
5. <span data-ttu-id="6cde4-138">Ange hello GOPATH miljö variabeln toopoint tooa giltig källa katalog, t.ex ditt aktuella hem katalogens gå mapp.</span><span class="sxs-lookup"><span data-stu-id="6cde4-138">Set hello GOPATH environment variable toopoint tooa valid source directory, such as your current home directory's go folder.</span></span> <span data-ttu-id="6cde4-139">Kör vid hello bash shell `export GOPATH=~/go` tooadd hello går directory som hello GOPATH för hello shell-sessionen.</span><span class="sxs-lookup"><span data-stu-id="6cde4-139">At hello bash shell, run `export GOPATH=~/go` tooadd hello go directory as hello GOPATH for hello current shell session.</span></span>
6. <span data-ttu-id="6cde4-140">Installera hello [gå-sql-drivrutin för mysql](https://github.com/go-sql-driver/mysql#installation) genom att köra hello `go get github.com/go-sql-driver/mysql` kommando.</span><span class="sxs-lookup"><span data-stu-id="6cde4-140">Install hello [go-sql-driver for mysql](https://github.com/go-sql-driver/mysql#installation) by running hello `go get github.com/go-sql-driver/mysql` command.</span></span>

   <span data-ttu-id="6cde4-141">Sammanfattningsvis ska du installera Go och sedan köra dessa bash-kommandon:</span><span class="sxs-lookup"><span data-stu-id="6cde4-141">In summary, install Go, then run these bash commands:</span></span>
   ```bash
   mkdir -p ~/go/src/mysqlgo/
   cd ~/go/src/mysqlgo/
   export GOPATH=~/go/
   go get github.com/go-sql-driver/mysql
   ```

## <a name="get-connection-information"></a><span data-ttu-id="6cde4-142">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="6cde4-142">Get connection information</span></span>
<span data-ttu-id="6cde4-143">Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="6cde4-143">Get hello connection information needed tooconnect toohello Azure Database for MySQL.</span></span> <span data-ttu-id="6cde4-144">Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.</span><span class="sxs-lookup"><span data-stu-id="6cde4-144">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="6cde4-145">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="6cde4-145">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="6cde4-146">Hello vänstra menyn i Azure-portalen klickar du på **alla resurser** och Sök efter hello-server som du har creased, som **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="6cde4-146">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have creased, such as **myserver4demo**.</span></span>
3. <span data-ttu-id="6cde4-147">Klicka på servernamnet för hello **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="6cde4-147">Click hello server name **myserver4demo**.</span></span>
4. <span data-ttu-id="6cde4-148">Välj hello server **egenskaper** sidan.</span><span class="sxs-lookup"><span data-stu-id="6cde4-148">Select hello server's **Properties** page.</span></span> <span data-ttu-id="6cde4-149">Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.</span><span class="sxs-lookup"><span data-stu-id="6cde4-149">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="6cde4-150">![Azure Database för MySQL – inloggning för serveradministratör](./media/connect-go/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="6cde4-150">![Azure Database for MySQL - Server Admin Login](./media/connect-go/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="6cde4-151">Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet och, om nödvändigt återställa hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="6cde4-151">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>
   

## <a name="build-and-run-go-code"></a><span data-ttu-id="6cde4-152">Skapa och köra Go-kod</span><span class="sxs-lookup"><span data-stu-id="6cde4-152">Build and run Go code</span></span> 
1. <span data-ttu-id="6cde4-153">toowrite Golang kod som du kan använda en enkel textredigerare, till exempel Anteckningar i Microsoft Windows [vi](http://manpages.ubuntu.com/manpages/xenial/man1/nvi.1.html#contenttoc5) eller [Nano](https://www.nano-editor.org/) i Ubuntu eller TextEdit i macOS.</span><span class="sxs-lookup"><span data-stu-id="6cde4-153">toowrite Golang code, you can use a simple text editor, such as Notepad in Microsoft Windows, [vi](http://manpages.ubuntu.com/manpages/xenial/man1/nvi.1.html#contenttoc5) or [Nano](https://www.nano-editor.org/) in Ubuntu, or TextEdit in macOS.</span></span> <span data-ttu-id="6cde4-154">Om du föredrar en mer omfattande IDE (Interactive Development Environment) kan du prova [Gogland](https://www.jetbrains.com/go/) från Jetbrains, [Visual Studio Code](https://code.visualstudio.com/) från Microsoft eller [Atom](https://atom.io/).</span><span class="sxs-lookup"><span data-stu-id="6cde4-154">If you prefer a richer Interactive Development Environment (IDE) try [Gogland](https://www.jetbrains.com/go/) by Jetbrains, [Visual Studio Code](https://code.visualstudio.com/) by Microsoft, or [Atom](https://atom.io/).</span></span>
2. <span data-ttu-id="6cde4-155">Klistra in hello gå kod från hello avsnitt nedan i textfiler och spara i projektmappen med filnamnstillägget \*.go, till exempel Windows sökväg `%USERPROFILE%\go\src\mysqlgo\createtable.go` eller Linux-sökvägen `~/go/src/mysqlgo/createtable.go`.</span><span class="sxs-lookup"><span data-stu-id="6cde4-155">Paste hello Go code from hello sections below into text files, and save into your project folder with file extension \*.go, such as Windows path `%USERPROFILE%\go\src\mysqlgo\createtable.go` or Linux path `~/go/src/mysqlgo/createtable.go`.</span></span>
3. <span data-ttu-id="6cde4-156">Leta upp hello `HOST`, `DATABASE`, `USER`, och `PASSWORD` konstanter i hello kod och Ersätt hello exempelvärden med egna värden.</span><span class="sxs-lookup"><span data-stu-id="6cde4-156">Locate hello `HOST`, `DATABASE`, `USER`, and `PASSWORD` constants in hello code, and replace hello example values with your own values.</span></span> 
4. <span data-ttu-id="6cde4-157">Starta Kommandotolken hello eller bash shell.</span><span class="sxs-lookup"><span data-stu-id="6cde4-157">Launch hello command prompt or bash shell.</span></span> <span data-ttu-id="6cde4-158">Ändra katalog till din projektmapp.</span><span class="sxs-lookup"><span data-stu-id="6cde4-158">Change directory into your project folder.</span></span> <span data-ttu-id="6cde4-159">I Windows kan du till exempel använda `cd %USERPROFILE%\go\src\mysqlgo\`.</span><span class="sxs-lookup"><span data-stu-id="6cde4-159">For example, on Windows `cd %USERPROFILE%\go\src\mysqlgo\`.</span></span> <span data-ttu-id="6cde4-160">I Linux kan du använda `cd ~/go/src/mysqlgo/`.</span><span class="sxs-lookup"><span data-stu-id="6cde4-160">On Linux `cd ~/go/src/mysqlgo/`.</span></span>  <span data-ttu-id="6cde4-161">Vissa av hello IDE redigeringsprogram nämns erbjuder funktioner för felsökning och körning utan shell-kommandon.</span><span class="sxs-lookup"><span data-stu-id="6cde4-161">Some of hello IDE editors mentioned offer debug and runtime capabilities without requiring shell commands.</span></span>
5. <span data-ttu-id="6cde4-162">Köra hello kod genom att ange hello kommando `go run createtable.go` toocompile hello programmet och kör den.</span><span class="sxs-lookup"><span data-stu-id="6cde4-162">Run hello code by typing hello command `go run createtable.go` toocompile hello application and run it.</span></span> 
6. <span data-ttu-id="6cde4-163">Alternativt toobuild hello kod i en programspecifika `go build createtable.go`, starta `createtable.exe` toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="6cde4-163">Alternatively, toobuild hello code into a native application, `go build createtable.go`, then launch `createtable.exe` toorun hello application.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="6cde4-164">Ansluta, skapa tabell och infoga data</span><span class="sxs-lookup"><span data-stu-id="6cde4-164">Connect, create table, and insert data</span></span>
<span data-ttu-id="6cde4-165">Använd hello följande code tooconnect toohello server, skapa en tabell och läsa in hello data med hjälp av en **infoga** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="6cde4-165">Use hello following code tooconnect toohello server, create a table, and load hello data using an **INSERT** SQL statement.</span></span> 

<span data-ttu-id="6cde4-166">hello kod importerar tre paket: hello [sql paketet](https://golang.org/pkg/database/sql/), hello [gå sql-drivrutin för mysql](https://github.com/go-sql-driver/mysql#installation) som en drivrutin toocommunicate med hello Azure-databas för MySQL och hello [fmt paketet](https://golang.org/pkg/fmt/)för utskrivna indata och utdata på hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="6cde4-166">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [go sql driver for mysql](https://github.com/go-sql-driver/mysql#installation) as a driver toocommunicate with hello Azure Database for MySQL, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="6cde4-167">hello koden anropar metoden [sql. Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure databas för MySQL och kontrollerar hello anslutning med hjälp av metoden [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="6cde4-167">hello code calls method [sql.Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure Database for MySQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="6cde4-168">En [referensen](https://golang.org/pkg/database/sql/#DB) används i hela, hålla hello anslutningspoolen för hello databasserver.</span><span class="sxs-lookup"><span data-stu-id="6cde4-168">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="6cde4-169">hello koden anropar hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metoden flera gånger toorun flera DDL-kommandon.</span><span class="sxs-lookup"><span data-stu-id="6cde4-169">hello code calls hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method several times toorun several DDL commands.</span></span> <span data-ttu-id="6cde4-170">hello koden används även hello [Prepare()](http://go-database-sql.org/prepared.html) och Exec() toorun förberedda instruktioner med olika parametrar tooinsert tre rader.</span><span class="sxs-lookup"><span data-stu-id="6cde4-170">hello code also uses hello [Prepare()](http://go-database-sql.org/prepared.html) and Exec() toorun prepared statements with different parameters tooinsert three rows.</span></span> <span data-ttu-id="6cde4-171">Varje gång en anpassad checkError() metod är används toocheck om ett fel uppstod och oroa dig tooexit.</span><span class="sxs-lookup"><span data-stu-id="6cde4-171">Each time a custom checkError() method is used toocheck if an error occurred and panic tooexit.</span></span>

<span data-ttu-id="6cde4-172">Ersätt hello `host`, `database`, `user`, och `password` konstanter med egna värden.</span><span class="sxs-lookup"><span data-stu-id="6cde4-172">Replace hello `host`, `database`, `user`, and `password` constants with your own values.</span></span> 

```Go
package main

import (
    "database/sql"
    "fmt"

    _ "github.com/go-sql-driver/mysql"
)

const (
    host     = "myserver4demo.mysql.database.azure.com"
    database = "quickstartdb"
    user     = "myadmin@myserver4demo"
    password = "yourpassword"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true", user, password, host, database)

    // Initialize connection object.
    db, err := sql.Open("mysql", connectionString)
    checkError(err)
    defer db.Close()

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection toodatabase.")

    // Drop previous table of same name if one exists.
    _, err = db.Exec("DROP TABLE IF EXISTS inventory;")
    checkError(err)
    fmt.Println("Finished dropping table (if existed).")

    // Create table.
    _, err = db.Exec("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
    checkError(err)
    fmt.Println("Finished creating table.")

    // Insert some data into table.
    sqlStatement, err := db.Prepare("INSERT INTO inventory (name, quantity) VALUES (?, ?);")
    res, err := sqlStatement.Exec("banana", 150)
    checkError(err)
    rowCount, err := res.RowsAffected()
    fmt.Printf("Inserted %d row(s) of data.\n", rowCount)

    res, err = sqlStatement.Exec("orange", 154)
    checkError(err)
    rowCount, err = res.RowsAffected()
    fmt.Printf("Inserted %d row(s) of data.\n", rowCount)

    res, err = sqlStatement.Exec("apple", 100)
    checkError(err)
    rowCount, err = res.RowsAffected()
    fmt.Printf("Inserted %d row(s) of data.\n", rowCount)
    fmt.Println("Done.")
}

```

## <a name="read-data"></a><span data-ttu-id="6cde4-173">Läsa data</span><span class="sxs-lookup"><span data-stu-id="6cde4-173">Read data</span></span>
<span data-ttu-id="6cde4-174">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **Välj** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="6cde4-174">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="6cde4-175">hello kod importerar tre paket: hello [sql paketet](https://golang.org/pkg/database/sql/), hello [gå sql-drivrutin för mysql](https://github.com/go-sql-driver/mysql#installation) som en drivrutin toocommunicate med hello Azure-databas för MySQL och hello [fmt paketet](https://golang.org/pkg/fmt/)för utskrivna indata och utdata på hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="6cde4-175">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [go sql driver for mysql](https://github.com/go-sql-driver/mysql#installation) as a driver toocommunicate with hello Azure Database for MySQL, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="6cde4-176">hello koden anropar metoden [sql. Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure databas för MySQL och kontrollerar hello anslutning med hjälp av metoden [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="6cde4-176">hello code calls method [sql.Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure Database for MySQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="6cde4-177">En [referensen](https://golang.org/pkg/database/sql/#DB) används i hela, hålla hello anslutningspoolen för hello databasserver.</span><span class="sxs-lookup"><span data-stu-id="6cde4-177">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="6cde4-178">hello koden anropar hello [Query()](https://golang.org/pkg/database/sql/#DB.Query) metoden toorun hello select-kommandot.</span><span class="sxs-lookup"><span data-stu-id="6cde4-178">hello code calls hello [Query()](https://golang.org/pkg/database/sql/#DB.Query) method toorun hello select command.</span></span> <span data-ttu-id="6cde4-179">Sedan körs [efter](https://golang.org/pkg/database/sql/#Rows.Next) tooiterate genom hello resultatuppsättning och [Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) tooparse hello kolumnvärdena, spara hello-värdet till variabler.</span><span class="sxs-lookup"><span data-stu-id="6cde4-179">Then it runs [Next()](https://golang.org/pkg/database/sql/#Rows.Next) tooiterate through hello result set and [Scan()](https://golang.org/pkg/database/sql/#Rows.Scan) tooparse hello column values, saving hello value into variables.</span></span> <span data-ttu-id="6cde4-180">Varje gång en anpassad checkError() metod är används toocheck om ett fel uppstod och oroa dig tooexit.</span><span class="sxs-lookup"><span data-stu-id="6cde4-180">Each time a custom checkError() method is used toocheck if an error occurred and panic tooexit.</span></span>

<span data-ttu-id="6cde4-181">Ersätt hello `host`, `database`, `user`, och `password` konstanter med egna värden.</span><span class="sxs-lookup"><span data-stu-id="6cde4-181">Replace hello `host`, `database`, `user`, and `password` constants with your own values.</span></span> 

```Go
package main

import (
    "database/sql"
    "fmt"

    _ "github.com/go-sql-driver/mysql"
)

const (
    host     = "myserver4demo.mysql.database.azure.com"
    database = "quickstartdb"
    user     = "myadmin@myserver4demo"
    password = "yourpassword"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true", user, password, host, database)

    // Initialize connection object.
    db, err := sql.Open("mysql", connectionString)
    checkError(err)
    defer db.Close()

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection toodatabase.")

    // Variables for printing column data when scanned.
    var (
        id       int
        name     string
        quantity int
    )

    // Read some data from hello table.
    rows, err := db.Query("SELECT id, name, quantity from inventory;")
    checkError(err)
    defer rows.Close()
    fmt.Println("Reading data:")
    for rows.Next() {
        err := rows.Scan(&id, &name, &quantity)
        checkError(err)
        fmt.Printf("Data row = (%d, %s, %d)\n", id, name, quantity)
    }
    err = rows.Err()
    checkError(err)
    fmt.Println("Done.")
}
```

## <a name="update-data"></a><span data-ttu-id="6cde4-182">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="6cde4-182">Update data</span></span>
<span data-ttu-id="6cde4-183">Använd hello följande kod tooconnect och uppdatera hello data med hjälp av en **uppdatera** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="6cde4-183">Use hello following code tooconnect and update hello data using a **UPDATE** SQL statement.</span></span> 

<span data-ttu-id="6cde4-184">hello kod importerar tre paket: hello [sql paketet](https://golang.org/pkg/database/sql/), hello [gå sql-drivrutin för mysql](https://github.com/go-sql-driver/mysql#installation) som en drivrutin toocommunicate med hello Azure-databas för MySQL och hello [fmt paketet](https://golang.org/pkg/fmt/)för utskrivna indata och utdata på hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="6cde4-184">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [go sql driver for mysql](https://github.com/go-sql-driver/mysql#installation) as a driver toocommunicate with hello Azure Database for MySQL, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="6cde4-185">hello koden anropar metoden [sql. Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure databas för MySQL och kontrollerar hello anslutning med hjälp av metoden [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="6cde4-185">hello code calls method [sql.Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure Database for MySQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="6cde4-186">En [referensen](https://golang.org/pkg/database/sql/#DB) används i hela, hålla hello anslutningspoolen för hello databasserver.</span><span class="sxs-lookup"><span data-stu-id="6cde4-186">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="6cde4-187">hello koden anropar hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metoden toorun hello update-kommandot.</span><span class="sxs-lookup"><span data-stu-id="6cde4-187">hello code calls hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method toorun hello update command.</span></span> <span data-ttu-id="6cde4-188">Varje gång en anpassad checkError() metod är används toocheck om ett fel uppstod och oroa dig tooexit.</span><span class="sxs-lookup"><span data-stu-id="6cde4-188">Each time a custom checkError() method is used toocheck if an error occurred and panic tooexit.</span></span>

<span data-ttu-id="6cde4-189">Ersätt hello `host`, `database`, `user`, och `password` konstanter med egna värden.</span><span class="sxs-lookup"><span data-stu-id="6cde4-189">Replace hello `host`, `database`, `user`, and `password` constants with your own values.</span></span> 

```Go
package main

import (
    "database/sql"
    "fmt"

    _ "github.com/go-sql-driver/mysql"
)

const (
    host     = "myserver4demo.mysql.database.azure.com"
    database = "quickstartdb"
    user     = "myadmin@myserver4demo"
    password = "yourpassword"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true", user, password, host, database)

    // Initialize connection object.
    db, err := sql.Open("mysql", connectionString)
    checkError(err)
    defer db.Close()

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection toodatabase.")

    // Modify some data in table.
    rows, err := db.Exec("UPDATE inventory SET quantity = ? WHERE name = ?", 200, "banana")
    checkError(err)
    rowCount, err := rows.RowsAffected()
    fmt.Printf("Deleted %d row(s) of data.\n", rowCount)
    fmt.Println("Done.")
}
```

## <a name="delete-data"></a><span data-ttu-id="6cde4-190">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="6cde4-190">Delete data</span></span>
<span data-ttu-id="6cde4-191">Använd hello följande kod tooconnect och ta bort data med hjälp av en **ta bort** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="6cde4-191">Use hello following code tooconnect and remove data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="6cde4-192">hello kod importerar tre paket: hello [sql paketet](https://golang.org/pkg/database/sql/), hello [gå sql-drivrutin för mysql](https://github.com/go-sql-driver/mysql#installation) som en drivrutin toocommunicate med hello Azure-databas för MySQL och hello [fmt paketet](https://golang.org/pkg/fmt/)för utskrivna indata och utdata på hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="6cde4-192">hello code imports three packages: hello [sql package](https://golang.org/pkg/database/sql/), hello [go sql driver for mysql](https://github.com/go-sql-driver/mysql#installation) as a driver toocommunicate with hello Azure Database for MySQL, and hello [fmt package](https://golang.org/pkg/fmt/) for printed input and output on hello command line.</span></span>

<span data-ttu-id="6cde4-193">hello koden anropar metoden [sql. Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure databas för MySQL och kontrollerar hello anslutning med hjälp av metoden [db. Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span><span class="sxs-lookup"><span data-stu-id="6cde4-193">hello code calls method [sql.Open()](http://go-database-sql.org/accessing.html) tooconnect tooAzure Database for MySQL, and checks hello connection using method [db.Ping()](https://golang.org/pkg/database/sql/#DB.Ping).</span></span> <span data-ttu-id="6cde4-194">En [referensen](https://golang.org/pkg/database/sql/#DB) används i hela, hålla hello anslutningspoolen för hello databasserver.</span><span class="sxs-lookup"><span data-stu-id="6cde4-194">A [database handle](https://golang.org/pkg/database/sql/#DB) is used throughout, holding hello connection pool for hello database server.</span></span> <span data-ttu-id="6cde4-195">hello koden anropar hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) metoden toorun hello ta bort kommando.</span><span class="sxs-lookup"><span data-stu-id="6cde4-195">hello code calls hello [Exec()](https://golang.org/pkg/database/sql/#DB.Exec) method toorun hello delete command.</span></span> <span data-ttu-id="6cde4-196">Varje gång en anpassad checkError() metod är används toocheck om ett fel uppstod och oroa dig tooexit.</span><span class="sxs-lookup"><span data-stu-id="6cde4-196">Each time a custom checkError() method is used toocheck if an error occurred and panic tooexit.</span></span>

<span data-ttu-id="6cde4-197">Ersätt hello `host`, `database`, `user`, och `password` konstanter med egna värden.</span><span class="sxs-lookup"><span data-stu-id="6cde4-197">Replace hello `host`, `database`, `user`, and `password` constants with your own values.</span></span> 

```Go
package main

import (
    "database/sql"
    "fmt"
    _ "github.com/go-sql-driver/mysql"
)

const (
    host     = "myserver4demo.mysql.database.azure.com"
    database = "quickstartdb"
    user     = "myadmin@myserver4demo"
    password = "yourpassword"
)

func checkError(err error) {
    if err != nil {
        panic(err)
    }
}

func main() {

    // Initialize connection string.
    var connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true", user, password, host, database)

    // Initialize connection object.
    db, err := sql.Open("mysql", connectionString)
    checkError(err)
    defer db.Close()

    err = db.Ping()
    checkError(err)
    fmt.Println("Successfully created connection toodatabase.")

    // Modify some data in table.
    rows, err := db.Exec("DELETE FROM inventory WHERE name = ?", "orange")
    checkError(err)
    rowCount, err := rows.RowsAffected()
    fmt.Printf("Deleted %d row(s) of data.\n", rowCount)
    fmt.Println("Done.")
}
```

## <a name="next-steps"></a><span data-ttu-id="6cde4-198">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6cde4-198">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="6cde4-199">Migrera din databas med Exportera och importera</span><span class="sxs-lookup"><span data-stu-id="6cde4-199">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
