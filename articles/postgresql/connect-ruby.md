---
title: "aaaConnect tooAzure databas för PostgreSQL med Ruby | Microsoft Docs"
description: "Denna Snabbstart innehåller en Ruby kodexempel som du kan använda tooconnect och fråga efter data från Azure-databas för PostgreSQL."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: ruby
ms.topic: quickstart
ms.date: 06/30/2017
ms.openlocfilehash: 7a0c8c92023452b40ca19d76fa659744f3e9a236
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-use-ruby-tooconnect-and-query-data"></a><span data-ttu-id="2b6af-103">Azure-databas för PostgreSQL: Använd Ruby tooconnect och fråga data</span><span class="sxs-lookup"><span data-stu-id="2b6af-103">Azure Database for PostgreSQL: Use Ruby tooconnect and query data</span></span>
<span data-ttu-id="2b6af-104">Den här snabbstarten visar hur tooconnect tooan Azure-databas för PostgreSQL med hjälp av en [Ruby](https://www.ruby-lang.org) program.</span><span class="sxs-lookup"><span data-stu-id="2b6af-104">This quickstart demonstrates how tooconnect tooan Azure Database for PostgreSQL using a [Ruby](https://www.ruby-lang.org) application.</span></span> <span data-ttu-id="2b6af-105">Den visar hur toouse SQL-instruktioner tooquery infoga, uppdatera och ta bort data i hello-databas.</span><span class="sxs-lookup"><span data-stu-id="2b6af-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="2b6af-106">Den här artikeln förutsätter att du är bekant med utveckling med hjälp av Ruby, men som du är ny tooworking med Azure-databas för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="2b6af-106">This article assumes you are familiar with development using Ruby, but that you are new tooworking with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2b6af-107">Krav</span><span class="sxs-lookup"><span data-stu-id="2b6af-107">Prerequisites</span></span>
<span data-ttu-id="2b6af-108">Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="2b6af-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="2b6af-109">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="2b6af-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="2b6af-110">Skapa DB – Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2b6af-110">Create DB - Azure CLI</span></span>](quickstart-create-server-database-azure-cli.md)

## <a name="install-ruby"></a><span data-ttu-id="2b6af-111">Installera Ruby</span><span class="sxs-lookup"><span data-stu-id="2b6af-111">Install Ruby</span></span>
<span data-ttu-id="2b6af-112">Installera Ruby på din egen dator.</span><span class="sxs-lookup"><span data-stu-id="2b6af-112">Install Ruby on your own machine.</span></span> 

### <a name="windows"></a><span data-ttu-id="2b6af-113">Windows</span><span class="sxs-lookup"><span data-stu-id="2b6af-113">Windows</span></span>
- <span data-ttu-id="2b6af-114">Hämta och installera hello senaste versionen av [Ruby](http://rubyinstaller.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="2b6af-114">Download and Install hello latest version of [Ruby](http://rubyinstaller.org/downloads/).</span></span>
- <span data-ttu-id="2b6af-115">Hello på skärmen hello MSI Installer är klar, kryssrutan hello som säger ”kör ridk installera tooinstall MSYS2 och utveckling toolchain”.</span><span class="sxs-lookup"><span data-stu-id="2b6af-115">On hello finish screen of hello MSI installer, check hello box that says "Run 'ridk install' tooinstall MSYS2 and development toolchain."</span></span> <span data-ttu-id="2b6af-116">Klicka på **Slutför** toolaunch hello nästa installer.</span><span class="sxs-lookup"><span data-stu-id="2b6af-116">Then click **Finish** toolaunch hello next installer.</span></span>
- <span data-ttu-id="2b6af-117">Hej RubyInstaller2 för Windows installer startar.</span><span class="sxs-lookup"><span data-stu-id="2b6af-117">hello RubyInstaller2 for Windows installer launches.</span></span> <span data-ttu-id="2b6af-118">Skriv 2 tooinstall hello MSYS2 databasen update.</span><span class="sxs-lookup"><span data-stu-id="2b6af-118">Type 2 tooinstall hello MSYS2 repository update.</span></span> <span data-ttu-id="2b6af-119">När den är klar och returnerar uppmaning om att installera toohello Stäng hello kommandofönstret.</span><span class="sxs-lookup"><span data-stu-id="2b6af-119">After it finishes and returns toohello installation prompt, close hello command window.</span></span>
- <span data-ttu-id="2b6af-120">Starta en ny kommandotolk (cmd) hello Start-menyn.</span><span class="sxs-lookup"><span data-stu-id="2b6af-120">Launch a new command prompt (cmd) from hello Start menu.</span></span>
- <span data-ttu-id="2b6af-121">Testa hello Ruby installation `ruby -v` toosee hello versionen installerad.</span><span class="sxs-lookup"><span data-stu-id="2b6af-121">Test hello Ruby installation `ruby -v` toosee hello version installed.</span></span>
- <span data-ttu-id="2b6af-122">Testa installationen av hello symbolen `gem -v` toosee hello versionen installerad.</span><span class="sxs-lookup"><span data-stu-id="2b6af-122">Test hello Gem installation `gem -v` toosee hello version installed.</span></span>
- <span data-ttu-id="2b6af-123">Skapa hello PostgreSQL-modul för Ruby med symbolen genom att köra kommandot hello `gem install pg`.</span><span class="sxs-lookup"><span data-stu-id="2b6af-123">Build hello PostgreSQL module for Ruby using Gem by running hello command `gem install pg`.</span></span>

### <a name="macos"></a><span data-ttu-id="2b6af-124">MacOS</span><span class="sxs-lookup"><span data-stu-id="2b6af-124">MacOS</span></span>
- <span data-ttu-id="2b6af-125">Installera Ruby med Homebrew genom att köra kommandot hello `brew install ruby`.</span><span class="sxs-lookup"><span data-stu-id="2b6af-125">Install Ruby using Homebrew by running hello command `brew install ruby`.</span></span> <span data-ttu-id="2b6af-126">Flera installationsalternativ finns hello Ruby [dokumentationen](https://www.ruby-lang.org/en/documentation/installation/#homebrew)</span><span class="sxs-lookup"><span data-stu-id="2b6af-126">For more installation options, see hello Ruby [installation documentation](https://www.ruby-lang.org/en/documentation/installation/#homebrew)</span></span>
- <span data-ttu-id="2b6af-127">Testa hello Ruby installation `ruby -v` toosee hello versionen installerad.</span><span class="sxs-lookup"><span data-stu-id="2b6af-127">Test hello Ruby installation `ruby -v` toosee hello version installed.</span></span>
- <span data-ttu-id="2b6af-128">Testa installationen av hello symbolen `gem -v` toosee hello versionen installerad.</span><span class="sxs-lookup"><span data-stu-id="2b6af-128">Test hello Gem installation `gem -v` toosee hello version installed.</span></span>
- <span data-ttu-id="2b6af-129">Skapa hello PostgreSQL-modul för Ruby med symbolen genom att köra kommandot hello `gem install pg`.</span><span class="sxs-lookup"><span data-stu-id="2b6af-129">Build hello PostgreSQL module for Ruby using Gem by running hello command `gem install pg`.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="2b6af-130">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="2b6af-130">Linux (Ubuntu)</span></span>
- <span data-ttu-id="2b6af-131">Installera Ruby genom att köra kommandot hello `sudo apt-get install ruby-full`.</span><span class="sxs-lookup"><span data-stu-id="2b6af-131">Install Ruby by running hello command `sudo apt-get install ruby-full`.</span></span> <span data-ttu-id="2b6af-132">Flera installationsalternativ finns hello Ruby [dokumentationen](https://www.ruby-lang.org/en/documentation/installation/).</span><span class="sxs-lookup"><span data-stu-id="2b6af-132">For more installation options, see hello Ruby [installation documentation](https://www.ruby-lang.org/en/documentation/installation/).</span></span>
- <span data-ttu-id="2b6af-133">Testa hello Ruby installation `ruby -v` toosee hello versionen installerad.</span><span class="sxs-lookup"><span data-stu-id="2b6af-133">Test hello Ruby installation `ruby -v` toosee hello version installed.</span></span>
- <span data-ttu-id="2b6af-134">Installera hello senaste uppdateringarna för symbolen genom att köra kommandot hello `sudo gem update --system`.</span><span class="sxs-lookup"><span data-stu-id="2b6af-134">Install hello latest updates for Gem by running hello command `sudo gem update --system`.</span></span>
- <span data-ttu-id="2b6af-135">Testa installationen av hello symbolen `gem -v` toosee hello versionen installerad.</span><span class="sxs-lookup"><span data-stu-id="2b6af-135">Test hello Gem installation `gem -v` toosee hello version installed.</span></span>
- <span data-ttu-id="2b6af-136">Installera hello gcc, märke och andra verktyg genom att köra kommandot hello `sudo apt-get install build-essential`.</span><span class="sxs-lookup"><span data-stu-id="2b6af-136">Install hello gcc, make, and other build tools by running hello command `sudo apt-get install build-essential`.</span></span>
- <span data-ttu-id="2b6af-137">Installera hello PostgreSQL bibliotek genom att köra kommandot hello `sudo apt-get install libpq-dev`.</span><span class="sxs-lookup"><span data-stu-id="2b6af-137">Install hello PostgreSQL libraries by running hello command `sudo apt-get install libpq-dev`.</span></span>
- <span data-ttu-id="2b6af-138">Skapa hello Ruby pg modul med symbolen genom att köra kommandot hello `sudo gem install pg`.</span><span class="sxs-lookup"><span data-stu-id="2b6af-138">Build hello Ruby pg module using Gem by running hello command `sudo gem install pg`.</span></span>

## <a name="run-ruby-code"></a><span data-ttu-id="2b6af-139">Köra Ruby-kod</span><span class="sxs-lookup"><span data-stu-id="2b6af-139">Run Ruby code</span></span> 
- <span data-ttu-id="2b6af-140">Spara hello kod i en textfil och spara hello-filen till en projektmapp med filen tillägget .rb som `C:\rubypostgres\read.rb` eller`/home/username/rubypostgres/read.rb`</span><span class="sxs-lookup"><span data-stu-id="2b6af-140">Save hello code into a text file, and save hello file into a project folder with file extension .rb, such as `C:\rubypostgres\read.rb` or `/home/username/rubypostgres/read.rb`</span></span>
- <span data-ttu-id="2b6af-141">toorun hello kod, starta Kommandotolken hello eller bash shell.</span><span class="sxs-lookup"><span data-stu-id="2b6af-141">toorun hello code, launch hello command prompt or bash shell.</span></span> <span data-ttu-id="2b6af-142">Ändra katalog till din projektmapp `cd rubypostgres`, Skriv hello kommando `ruby read.rb` toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="2b6af-142">Change directory into your project folder `cd rubypostgres`, then type hello command `ruby read.rb` toorun hello application.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="2b6af-143">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="2b6af-143">Get connection information</span></span>
<span data-ttu-id="2b6af-144">Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="2b6af-144">Get hello connection information needed tooconnect toohello Azure Database for PostgreSQL.</span></span> <span data-ttu-id="2b6af-145">Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.</span><span class="sxs-lookup"><span data-stu-id="2b6af-145">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="2b6af-146">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="2b6af-146">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="2b6af-147">Hello vänstra menyn i Azure-portalen klickar du på **alla resurser** och Sök efter hello-server som du har skapat, exempelvis **mypgserver 20170401**.</span><span class="sxs-lookup"><span data-stu-id="2b6af-147">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have created, such as **mypgserver-20170401**.</span></span>
3. <span data-ttu-id="2b6af-148">Klicka på servernamnet för hello **mypgserver 20170401**.</span><span class="sxs-lookup"><span data-stu-id="2b6af-148">Click hello server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="2b6af-149">Välj hello server **översikt** sidan.</span><span class="sxs-lookup"><span data-stu-id="2b6af-149">Select hello server's **Overview** page.</span></span> <span data-ttu-id="2b6af-150">Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.</span><span class="sxs-lookup"><span data-stu-id="2b6af-150">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="2b6af-151">![Azure Database för PostgreSQL – inloggning för serveradministratör](./media/connect-ruby/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="2b6af-151">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-ruby/1-connection-string.png)</span></span>
5. <span data-ttu-id="2b6af-152">Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet.</span><span class="sxs-lookup"><span data-stu-id="2b6af-152">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name.</span></span> <span data-ttu-id="2b6af-153">Om det behövs, Återställ hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="2b6af-153">If necessary, reset hello password.</span></span>

## <a name="connect-and-create-a-table"></a><span data-ttu-id="2b6af-154">Ansluta och skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="2b6af-154">Connect and create a table</span></span>
<span data-ttu-id="2b6af-155">Använd hello följande kod tooconnect och skapa en tabell med hjälp av **CREATE TABLE** SQL-instruktionen, följt av **INSERT INTO** SQL-instruktioner tooadd rader i hello-tabellen.</span><span class="sxs-lookup"><span data-stu-id="2b6af-155">Use hello following code tooconnect and create a table using **CREATE TABLE** SQL statement, followed by **INSERT INTO** SQL statements tooadd rows into hello table.</span></span>

<span data-ttu-id="2b6af-156">hello koden använder en [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) objekt med konstruktorn [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure för PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="2b6af-156">hello code uses a  [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) object with constructor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure Database for PostgreSQL.</span></span> <span data-ttu-id="2b6af-157">Sedan anropas metoden [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello släpp CREATE TABLE och INSERT INTO-kommandon.</span><span class="sxs-lookup"><span data-stu-id="2b6af-157">Then it calls method [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello DROP, CREATE TABLE, and INSERT INTO commands.</span></span> <span data-ttu-id="2b6af-158">hello kod söker efter fel med hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) klass.</span><span class="sxs-lookup"><span data-stu-id="2b6af-158">hello code checks for errors using hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) class.</span></span> <span data-ttu-id="2b6af-159">Sedan anropas metoden [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello anslutningen innan du avslutar.</span><span class="sxs-lookup"><span data-stu-id="2b6af-159">Then it calls method [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="2b6af-160">Ersätt hello `host`, `database`, `user`, och `password` strängar med egna värden.</span><span class="sxs-lookup"><span data-stu-id="2b6af-160">Replace hello `host`, `database`, `user`, and `password` strings with your own values.</span></span> 
```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection toodatabase'

    # Drop previous table of same name if one exists
    connection.exec('DROP TABLE IF EXISTS inventory;')
    puts 'Finished dropping table (if existed).'

    # Drop previous table of same name if one exists.
    connection.exec('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);')
    puts 'Finished creating table.'

    # Insert some data into table.
    connection.exec("INSERT INTO inventory VALUES(1, 'banana', 150)")
    connection.exec("INSERT INTO inventory VALUES(2, 'orange', 154)")
    connection.exec("INSERT INTO inventory VALUES(3, 'apple', 100)")
    puts 'Inserted 3 rows of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="read-data"></a><span data-ttu-id="2b6af-161">Läsa data</span><span class="sxs-lookup"><span data-stu-id="2b6af-161">Read data</span></span>
<span data-ttu-id="2b6af-162">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **Välj** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="2b6af-162">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="2b6af-163">hello koden använder en [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) objekt med konstruktorn [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure för PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="2b6af-163">hello code uses a  [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) object with constructor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure Database for PostgreSQL.</span></span> <span data-ttu-id="2b6af-164">Sedan anropas metoden [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello SELECT-kommandot, hålla hello resultat i en resultatuppsättning.</span><span class="sxs-lookup"><span data-stu-id="2b6af-164">Then it calls method [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello SELECT command, keeping hello results in a result set.</span></span> <span data-ttu-id="2b6af-165">hello resultatet set samlingen är hävdade jämfört med hello `resultSet.each do` loop behålla hello aktuella radvärden i hello `row` variabeln.</span><span class="sxs-lookup"><span data-stu-id="2b6af-165">hello result set collection is iterated over using hello `resultSet.each do` loop, keeping hello current row values in hello `row` variable.</span></span> <span data-ttu-id="2b6af-166">hello kod söker efter fel med hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) klass.</span><span class="sxs-lookup"><span data-stu-id="2b6af-166">hello code checks for errors using hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) class.</span></span> <span data-ttu-id="2b6af-167">Sedan anropas metoden [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello anslutningen innan du avslutar.</span><span class="sxs-lookup"><span data-stu-id="2b6af-167">Then it calls method [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="2b6af-168">Ersätt hello `host`, `database`, `user`, och `password` strängar med egna värden.</span><span class="sxs-lookup"><span data-stu-id="2b6af-168">Replace hello `host`, `database`, `user`, and `password` strings with your own values.</span></span> 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :database => dbname, :port => '5432', :password => password)
    puts 'Successfully created connection toodatabase.'

    resultSet = connection.exec('SELECT * from inventory;')
    resultSet.each do |row|
        puts 'Data row = (%s, %s, %s)' % [row['id'], row['name'], row['quantity']]
    end

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="update-data"></a><span data-ttu-id="2b6af-169">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="2b6af-169">Update data</span></span>
<span data-ttu-id="2b6af-170">Använd hello följande kod tooconnect och uppdatera hello data med hjälp av en **uppdatera** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="2b6af-170">Use hello following code tooconnect and update hello data using a **UPDATE** SQL statement.</span></span>

<span data-ttu-id="2b6af-171">hello koden använder en [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) objekt med konstruktorn [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure för PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="2b6af-171">hello code uses a  [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) object with constructor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure Database for PostgreSQL.</span></span> <span data-ttu-id="2b6af-172">Sedan anropas metoden [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello uppdateringskommando.</span><span class="sxs-lookup"><span data-stu-id="2b6af-172">Then it calls method [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello UPDATE command.</span></span> <span data-ttu-id="2b6af-173">hello kod söker efter fel med hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) klass.</span><span class="sxs-lookup"><span data-stu-id="2b6af-173">hello code checks for errors using hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) class.</span></span> <span data-ttu-id="2b6af-174">Sedan anropas metoden [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello anslutningen innan du avslutar.</span><span class="sxs-lookup"><span data-stu-id="2b6af-174">Then it calls method [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="2b6af-175">Ersätt hello `host`, `database`, `user`, och `password` strängar med egna värden.</span><span class="sxs-lookup"><span data-stu-id="2b6af-175">Replace hello `host`, `database`, `user`, and `password` strings with your own values.</span></span> 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection toodatabase.'

    # Modify some data in table.
    connection.exec('UPDATE inventory SET quantity = %d WHERE name = %s;' % [200, '\'banana\''])
    puts 'Updated 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```


## <a name="delete-data"></a><span data-ttu-id="2b6af-176">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="2b6af-176">Delete data</span></span>
<span data-ttu-id="2b6af-177">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **ta bort** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="2b6af-177">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="2b6af-178">hello koden använder en [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) objekt med konstruktorn [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure för PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="2b6af-178">hello code uses a  [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) object with constructor [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure Database for PostgreSQL.</span></span> <span data-ttu-id="2b6af-179">Sedan anropas metoden [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello uppdateringskommando.</span><span class="sxs-lookup"><span data-stu-id="2b6af-179">Then it calls method [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello UPDATE command.</span></span> <span data-ttu-id="2b6af-180">hello kod söker efter fel med hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) klass.</span><span class="sxs-lookup"><span data-stu-id="2b6af-180">hello code checks for errors using hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) class.</span></span> <span data-ttu-id="2b6af-181">Sedan anropas metoden [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello anslutningen innan du avslutar.</span><span class="sxs-lookup"><span data-stu-id="2b6af-181">Then it calls method [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="2b6af-182">Ersätt hello `host`, `database`, `user`, och `password` strängar med egna värden.</span><span class="sxs-lookup"><span data-stu-id="2b6af-182">Replace hello `host`, `database`, `user`, and `password` strings with your own values.</span></span> 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection toodatabase.'

    # Modify some data in table.
    connection.exec('DELETE FROM inventory WHERE name = %s;' % ['\'orange\''])
    puts 'Deleted 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="next-steps"></a><span data-ttu-id="2b6af-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2b6af-183">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="2b6af-184">Migrera din databas med Exportera och importera</span><span class="sxs-lookup"><span data-stu-id="2b6af-184">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
