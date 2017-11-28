---
title: "Ansluta tooAzure databas för MySQL med Ruby | Microsoft Docs"
description: "Denna Snabbstart innehåller flera Ruby kodexempel du kan använda tooconnect och fråga efter data från Azure-databas för MySQL."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.devlang: ruby
ms.topic: hero-article
ms.date: 07/13/2017
ms.openlocfilehash: ff0880dcc24e96f467c9092bc663ce3dc4c2637a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-ruby-tooconnect-and-query-data"></a><span data-ttu-id="6861c-103">Azure-databas för MySQL: Använd Ruby tooconnect och fråga data</span><span class="sxs-lookup"><span data-stu-id="6861c-103">Azure Database for MySQL: Use Ruby tooconnect and query data</span></span>
<span data-ttu-id="6861c-104">Den här snabbstarten visar hur tooconnect tooan Azure-databas för MySQL med hjälp av en [Ruby](https://www.ruby-lang.org) program och hello [mysql2](https://rubygems.org/gems/mysql2) symbolen från Windows, Ubuntu Linux och Mac-plattformar.</span><span class="sxs-lookup"><span data-stu-id="6861c-104">This quickstart demonstrates how tooconnect tooan Azure Database for MySQL using a [Ruby](https://www.ruby-lang.org) application and hello [mysql2](https://rubygems.org/gems/mysql2) gem from Windows, Ubuntu Linux, and Mac platforms.</span></span> <span data-ttu-id="6861c-105">Den visar hur toouse SQL-instruktioner tooquery infoga, uppdatera och ta bort data i hello-databas.</span><span class="sxs-lookup"><span data-stu-id="6861c-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="6861c-106">Den här artikeln förutsätter att du är bekant med utveckling med hjälp av Ruby, men som du är ny tooworking med Azure-databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="6861c-106">This article assumes you are familiar with development using Ruby, but that you are new tooworking with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6861c-107">Krav</span><span class="sxs-lookup"><span data-stu-id="6861c-107">Prerequisites</span></span>
<span data-ttu-id="6861c-108">Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="6861c-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="6861c-109">Skapa en Azure Database för MySQL med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6861c-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="6861c-110">Skapa en Azure Database för MySQL-server med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6861c-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-ruby"></a><span data-ttu-id="6861c-111">Installera Ruby</span><span class="sxs-lookup"><span data-stu-id="6861c-111">Install Ruby</span></span>
<span data-ttu-id="6861c-112">Installera Ruby och symbolen hello MySQL2 bibliotek på din egen dator.</span><span class="sxs-lookup"><span data-stu-id="6861c-112">Install Ruby, Gem, and hello MySQL2 library on your own machine.</span></span> 

### <a name="windows"></a><span data-ttu-id="6861c-113">Windows</span><span class="sxs-lookup"><span data-stu-id="6861c-113">Windows</span></span>
1. <span data-ttu-id="6861c-114">Ladda ned och installerar hello 2.3 versionen av [Ruby](http://rubyinstaller.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="6861c-114">Download and Install hello 2.3 version of [Ruby](http://rubyinstaller.org/downloads/).</span></span>
2. <span data-ttu-id="6861c-115">Starta en ny kommandotolk (cmd) hello Start-menyn.</span><span class="sxs-lookup"><span data-stu-id="6861c-115">Launch a new command prompt (cmd) from hello Start menu.</span></span>
3. <span data-ttu-id="6861c-116">Ändra katalogen till hello Ruby katalog för version 2.3.</span><span class="sxs-lookup"><span data-stu-id="6861c-116">Change directory into hello Ruby directory for version 2.3.</span></span> `cd c:\Ruby23-x64\bin`
4. <span data-ttu-id="6861c-117">Testa hello Ruby installationen genom att köra kommandot hello `ruby -v` toosee hello versionen installerad.</span><span class="sxs-lookup"><span data-stu-id="6861c-117">Test hello Ruby installation by running hello command `ruby -v` toosee hello version installed.</span></span>
5. <span data-ttu-id="6861c-118">Testa hello symbolen installationen genom att köra kommandot hello `gem -v` toosee hello versionen installerad.</span><span class="sxs-lookup"><span data-stu-id="6861c-118">Test hello Gem installation by running hello command `gem -v` toosee hello version installed.</span></span>
6. <span data-ttu-id="6861c-119">Skapa hello Mysql2-modul för Ruby med symbolen genom att köra kommandot hello `gem install mysql2`.</span><span class="sxs-lookup"><span data-stu-id="6861c-119">Build hello Mysql2 module for Ruby using Gem by running hello command `gem install mysql2`.</span></span>

### <a name="macos"></a><span data-ttu-id="6861c-120">MacOS</span><span class="sxs-lookup"><span data-stu-id="6861c-120">MacOS</span></span>
1. <span data-ttu-id="6861c-121">Installera Ruby med Homebrew genom att köra kommandot hello `brew install ruby`.</span><span class="sxs-lookup"><span data-stu-id="6861c-121">Install Ruby using Homebrew by running hello command `brew install ruby`.</span></span> <span data-ttu-id="6861c-122">Flera installationsalternativ finns hello Ruby [dokumentationen](https://www.ruby-lang.org/en/documentation/installation/#homebrew).</span><span class="sxs-lookup"><span data-stu-id="6861c-122">For more installation options, see hello Ruby [installation documentation](https://www.ruby-lang.org/en/documentation/installation/#homebrew).</span></span>
2. <span data-ttu-id="6861c-123">Testa hello Ruby installationen genom att köra kommandot hello `ruby -v` toosee hello versionen installerad.</span><span class="sxs-lookup"><span data-stu-id="6861c-123">Test hello Ruby installation by running hello command `ruby -v` toosee hello version installed.</span></span>
3. <span data-ttu-id="6861c-124">Testa hello symbolen installationen genom att köra kommandot hello `gem -v` toosee hello versionen installerad.</span><span class="sxs-lookup"><span data-stu-id="6861c-124">Test hello Gem installation by running hello command `gem -v` toosee hello version installed.</span></span>
4. <span data-ttu-id="6861c-125">Skapa hello Mysql2-modul för Ruby med symbolen genom att köra kommandot hello `gem install mysql2`.</span><span class="sxs-lookup"><span data-stu-id="6861c-125">Build hello Mysql2 module for Ruby using Gem by running hello command `gem install mysql2`.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="6861c-126">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="6861c-126">Linux (Ubuntu)</span></span>
1. <span data-ttu-id="6861c-127">Installera Ruby genom att köra kommandot hello `sudo apt-get install ruby-full`.</span><span class="sxs-lookup"><span data-stu-id="6861c-127">Install Ruby by running hello command `sudo apt-get install ruby-full`.</span></span> <span data-ttu-id="6861c-128">Flera installationsalternativ finns hello Ruby [dokumentationen](https://www.ruby-lang.org/en/documentation/installation/).</span><span class="sxs-lookup"><span data-stu-id="6861c-128">For more installation options, see hello Ruby [installation documentation](https://www.ruby-lang.org/en/documentation/installation/).</span></span>
2. <span data-ttu-id="6861c-129">Testa hello Ruby installationen genom att köra kommandot hello `ruby -v` toosee hello versionen installerad.</span><span class="sxs-lookup"><span data-stu-id="6861c-129">Test hello Ruby installation by running hello command `ruby -v` toosee hello version installed.</span></span>
3. <span data-ttu-id="6861c-130">Installera hello senaste uppdateringarna för symbolen genom att köra kommandot hello `sudo gem update --system`.</span><span class="sxs-lookup"><span data-stu-id="6861c-130">Install hello latest updates for Gem by running hello command `sudo gem update --system`.</span></span>
4. <span data-ttu-id="6861c-131">Testa hello symbolen installationen genom att köra kommandot hello `gem -v` toosee hello versionen installerad.</span><span class="sxs-lookup"><span data-stu-id="6861c-131">Test hello Gem installation by running hello command `gem -v` toosee hello version installed.</span></span>
5. <span data-ttu-id="6861c-132">Installera hello gcc, märke och andra verktyg genom att köra kommandot hello `sudo apt-get install build-essential`.</span><span class="sxs-lookup"><span data-stu-id="6861c-132">Install hello gcc, make, and other build tools by running hello command `sudo apt-get install build-essential`.</span></span>
6. <span data-ttu-id="6861c-133">Installera hello MySQL-klientbibliotek developer genom att köra kommandot hello `sudo apt-get install libmysqlclient-dev`.</span><span class="sxs-lookup"><span data-stu-id="6861c-133">Install hello MySQL client developer libraries by running hello command `sudo apt-get install libmysqlclient-dev`.</span></span>
7. <span data-ttu-id="6861c-134">Skapa hello mysql2-modul för Ruby med symbolen genom att köra kommandot hello `sudo gem install mysql2`.</span><span class="sxs-lookup"><span data-stu-id="6861c-134">Build hello mysql2 module for Ruby using Gem by running hello command `sudo gem install mysql2`.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="6861c-135">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="6861c-135">Get connection information</span></span>
<span data-ttu-id="6861c-136">Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="6861c-136">Get hello connection information needed tooconnect toohello Azure Database for MySQL.</span></span> <span data-ttu-id="6861c-137">Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.</span><span class="sxs-lookup"><span data-stu-id="6861c-137">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="6861c-138">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="6861c-138">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="6861c-139">Hello vänstra menyn i Azure-portalen klickar du på **alla resurser** och Sök efter hello-server som du har creased, som **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="6861c-139">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have creased, such as **myserver4demo**.</span></span>
3. <span data-ttu-id="6861c-140">Klicka på servernamnet för hello **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="6861c-140">Click hello server name **myserver4demo**.</span></span>
4. <span data-ttu-id="6861c-141">Välj hello server **egenskaper** sidan.</span><span class="sxs-lookup"><span data-stu-id="6861c-141">Select hello server's **Properties** page.</span></span> <span data-ttu-id="6861c-142">Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.</span><span class="sxs-lookup"><span data-stu-id="6861c-142">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="6861c-143">![Azure Database för MySQL – inloggning för serveradministratör](./media/connect-ruby/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="6861c-143">![Azure Database for MySQL - Server Admin Login](./media/connect-ruby/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="6861c-144">Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet och, om nödvändigt återställa hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="6861c-144">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="run-ruby-code"></a><span data-ttu-id="6861c-145">Köra Ruby-kod</span><span class="sxs-lookup"><span data-stu-id="6861c-145">Run Ruby code</span></span> 
1. <span data-ttu-id="6861c-146">Klistra in hello Ruby kod från hello avsnitt nedan i textfiler och spara hello filer till en projektmapp med filen tillägget .rb som `C:\rubymysql\createtable.rb` eller `/home/username/rubymysql/createtable.rb`.</span><span class="sxs-lookup"><span data-stu-id="6861c-146">Paste hello Ruby code from hello sections below into text files, and save hello files into a project folder with file extension .rb, such as `C:\rubymysql\createtable.rb` or `/home/username/rubymysql/createtable.rb`.</span></span>
2. <span data-ttu-id="6861c-147">toorun hello kod, starta Kommandotolken hello eller bash shell.</span><span class="sxs-lookup"><span data-stu-id="6861c-147">toorun hello code, launch hello command prompt or bash shell.</span></span> <span data-ttu-id="6861c-148">Ändra katalog till din projektmapp, till exempel `cd rubymysql`</span><span class="sxs-lookup"><span data-stu-id="6861c-148">Change directory into your project folder `cd rubymysql`</span></span>
3. <span data-ttu-id="6861c-149">Skriv hello ruby kommando följt av hello filnamn som `ruby createtable.rb` toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="6861c-149">Then type hello ruby command followed by hello file name, such as `ruby createtable.rb` toorun hello application.</span></span>
4. <span data-ttu-id="6861c-150">På hello Windows-Operativsystemet, om hello ruby programmet inte finns i din path-miljövariabeln måste toouse hello fullständig sökväg toolaunch hello noden program som`"c:\Ruby23-x64\bin\ruby.exe" createtable.rb`</span><span class="sxs-lookup"><span data-stu-id="6861c-150">On hello Windows OS, if hello ruby application is not in your path environment variable, you may need toouse hello full path toolaunch hello node application, such as `"c:\Ruby23-x64\bin\ruby.exe" createtable.rb`</span></span>

## <a name="connect-and-create-a-table"></a><span data-ttu-id="6861c-151">Ansluta och skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="6861c-151">Connect and create a table</span></span>
<span data-ttu-id="6861c-152">Använd hello följande kod tooconnect och skapa en tabell med hjälp av **CREATE TABLE** SQL-instruktionen, följt av **INSERT INTO** SQL-instruktioner tooadd rader i hello-tabellen.</span><span class="sxs-lookup"><span data-stu-id="6861c-152">Use hello following code tooconnect and create a table using **CREATE TABLE** SQL statement, followed by **INSERT INTO** SQL statements tooadd rows into hello table.</span></span>

<span data-ttu-id="6861c-153">hello koden använder en [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) klassen .new() metoden tooconnect tooAzure databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="6861c-153">hello code uses a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) class .new() method tooconnect tooAzure Database for MySQL.</span></span> <span data-ttu-id="6861c-154">Sedan anropas metoden [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) flera gånger toorun hello släpp CREATE TABLE kommandona, och INSERT INTO.</span><span class="sxs-lookup"><span data-stu-id="6861c-154">Then it calls method [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) several times toorun hello DROP, CREATE TABLE, and INSERT INTO commands.</span></span> <span data-ttu-id="6861c-155">Sedan anropas metoden [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello anslutningen innan du avslutar.</span><span class="sxs-lookup"><span data-stu-id="6861c-155">Then it calls method [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="6861c-156">Ersätt hello `host`, `database`, `username`, och `password` strängar med egna värden.</span><span class="sxs-lookup"><span data-stu-id="6861c-156">Replace hello `host`, `database`, `username`, and `password` strings with your own values.</span></span> 
```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection toodatabase.'

    # Drop previous table of same name if one exists
    client.query('DROP TABLE IF EXISTS inventory;')
    puts 'Finished dropping table (if existed).'

    # Drop previous table of same name if one exists.
    client.query('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);')
    puts 'Finished creating table.'

    # Insert some data into table.
    client.query("INSERT INTO inventory VALUES(1, 'banana', 150)")
    client.query("INSERT INTO inventory VALUES(2, 'orange', 154)")
    client.query("INSERT INTO inventory VALUES(3, 'apple', 100)")
    puts 'Inserted 3 rows of data.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="read-data"></a><span data-ttu-id="6861c-157">Läsa data</span><span class="sxs-lookup"><span data-stu-id="6861c-157">Read data</span></span>
<span data-ttu-id="6861c-158">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **Välj** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="6861c-158">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="6861c-159">hello koden använder en [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) klassen .new() metoden tooconnect tooAzure databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="6861c-159">hello code uses a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) class .new() method tooconnect tooAzure Database for MySQL.</span></span> <span data-ttu-id="6861c-160">Sedan anropas metoden [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello väljer kommandon.</span><span class="sxs-lookup"><span data-stu-id="6861c-160">Then it calls method [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello SELECT commands.</span></span> <span data-ttu-id="6861c-161">Sedan anropas metoden [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello anslutningen innan du avslutar.</span><span class="sxs-lookup"><span data-stu-id="6861c-161">Then it calls method [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="6861c-162">Ersätt hello `host`, `database`, `username`, och `password` strängar med egna värden.</span><span class="sxs-lookup"><span data-stu-id="6861c-162">Replace hello `host`, `database`, `username`, and `password` strings with your own values.</span></span> 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection toodatabase.'

    # Read data
    resultSet = client.query('SELECT * from inventory;')
    resultSet.each do |row|
        puts 'Data row = (%s, %s, %s)' % [row['id'], row['name'], row['quantity']]
    end
    puts 'Read ' + resultSet.count.to_s + ' row(s).'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="update-data"></a><span data-ttu-id="6861c-163">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="6861c-163">Update data</span></span>
<span data-ttu-id="6861c-164">Använd hello följande kod tooconnect och uppdatera hello data med hjälp av en **uppdatera** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="6861c-164">Use hello following code tooconnect and update hello data using a **UPDATE** SQL statement.</span></span>

<span data-ttu-id="6861c-165">hello koden använder en [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) klassen .new() metoden tooconnect tooAzure databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="6861c-165">hello code uses a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) class .new() method tooconnect tooAzure Database for MySQL.</span></span> <span data-ttu-id="6861c-166">Sedan anropas metoden [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello UPDATE kommandon.</span><span class="sxs-lookup"><span data-stu-id="6861c-166">Then it calls method [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello UPDATE commands.</span></span> <span data-ttu-id="6861c-167">Sedan anropas metoden [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello anslutningen innan du avslutar.</span><span class="sxs-lookup"><span data-stu-id="6861c-167">Then it calls method [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="6861c-168">Ersätt hello `host`, `database`, `username`, och `password` strängar med egna värden.</span><span class="sxs-lookup"><span data-stu-id="6861c-168">Replace hello `host`, `database`, `username`, and `password` strings with your own values.</span></span> 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection toodatabase.'

    # Update data
   client.query('UPDATE inventory SET quantity = %d WHERE name = %s;' % [200, '\'banana\''])
   puts 'Updated 1 row of data.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```


## <a name="delete-data"></a><span data-ttu-id="6861c-169">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="6861c-169">Delete data</span></span>
<span data-ttu-id="6861c-170">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **ta bort** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="6861c-170">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="6861c-171">hello koden använder en [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) klassen .new() metoden tooconnect tooAzure databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="6861c-171">hello code uses a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) class .new() method tooconnect tooAzure Database for MySQL.</span></span> <span data-ttu-id="6861c-172">Sedan anropas metoden [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello DELETE-kommandon.</span><span class="sxs-lookup"><span data-stu-id="6861c-172">Then it calls method [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello DELETE commands.</span></span> <span data-ttu-id="6861c-173">Sedan anropas metoden [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello anslutningen innan du avslutar.</span><span class="sxs-lookup"><span data-stu-id="6861c-173">Then it calls method [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello connection before terminating.</span></span>

<span data-ttu-id="6861c-174">Ersätt hello `host`, `database`, `username`, och `password` strängar med egna värden.</span><span class="sxs-lookup"><span data-stu-id="6861c-174">Replace hello `host`, `database`, `username`, and `password` strings with your own values.</span></span> 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection toodatabase.'

    # Delete data
    resultSet = client.query('DELETE FROM inventory WHERE name = %s;' % ['\'orange\''])
    puts 'Deleted 1 row.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="next-steps"></a><span data-ttu-id="6861c-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6861c-175">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="6861c-176">Migrera din databas med Exportera och importera</span><span class="sxs-lookup"><span data-stu-id="6861c-176">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
