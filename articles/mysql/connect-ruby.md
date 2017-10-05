---
title: "Ansluta till Azure Database för MySQL med Ruby | Microsoft Docs"
description: "I den här snabbstarten finns ett kodexempel i Ruby som du kan använda för att ansluta till och fråga efter data från Azure Database för MySQL."
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
ms.openlocfilehash: e54f1dccbae060c52f48bfeb277c045b99a91715
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="azure-database-for-mysql-use-ruby-to-connect-and-query-data"></a><span data-ttu-id="babba-103">Azure Database för MySQL: Använda Ruby för att ansluta och fråga efter data</span><span class="sxs-lookup"><span data-stu-id="babba-103">Azure Database for MySQL: Use Ruby to connect and query data</span></span>
<span data-ttu-id="babba-104">Den här snabbstarten visar hur du ansluter till en Azure Database för MySQL med hjälp av ett [Ruby](https://www.ruby-lang.org)-program och en [mysql2](https://rubygems.org/gems/mysql2)-gem från plattformar med Windows, Ubuntu Linux och Mac.</span><span class="sxs-lookup"><span data-stu-id="babba-104">This quickstart demonstrates how to connect to an Azure Database for MySQL using a [Ruby](https://www.ruby-lang.org) application and the [mysql2](https://rubygems.org/gems/mysql2) gem from Windows, Ubuntu Linux, and Mac platforms.</span></span> <span data-ttu-id="babba-105">Den visar hur du använder SQL-instruktioner för att fråga, infoga, uppdatera och ta bort data i databasen.</span><span class="sxs-lookup"><span data-stu-id="babba-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="babba-106">Den här artikeln förutsätter att du är van att utveckla i Ruby, men saknar erfarenhet av Azure Database för MySQL.</span><span class="sxs-lookup"><span data-stu-id="babba-106">This article assumes you are familiar with development using Ruby, but that you are new to working with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="babba-107">Krav</span><span class="sxs-lookup"><span data-stu-id="babba-107">Prerequisites</span></span>
<span data-ttu-id="babba-108">I den här snabbstarten används de resurser som skapades i någon av följande guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="babba-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="babba-109">Skapa en Azure Database för MySQL med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="babba-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="babba-110">Skapa en Azure Database för MySQL-server med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="babba-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-ruby"></a><span data-ttu-id="babba-111">Installera Ruby</span><span class="sxs-lookup"><span data-stu-id="babba-111">Install Ruby</span></span>
<span data-ttu-id="babba-112">Installera Ruby, Gem och MySQL2-biblioteket på din egen dator.</span><span class="sxs-lookup"><span data-stu-id="babba-112">Install Ruby, Gem, and the MySQL2 library on your own machine.</span></span> 

### <a name="windows"></a><span data-ttu-id="babba-113">Windows</span><span class="sxs-lookup"><span data-stu-id="babba-113">Windows</span></span>
1. <span data-ttu-id="babba-114">Hämta och installera den version 2.3 av [Ruby](http://rubyinstaller.org/downloads/).</span><span class="sxs-lookup"><span data-stu-id="babba-114">Download and Install the 2.3 version of [Ruby](http://rubyinstaller.org/downloads/).</span></span>
2. <span data-ttu-id="babba-115">Starta en ny kommandotolk (cmd) från Start-menyn.</span><span class="sxs-lookup"><span data-stu-id="babba-115">Launch a new command prompt (cmd) from the Start menu.</span></span>
3. <span data-ttu-id="babba-116">Ändra katalog till katalogen Ruby för version 2.3.</span><span class="sxs-lookup"><span data-stu-id="babba-116">Change directory into the Ruby directory for version 2.3.</span></span> `cd c:\Ruby23-x64\bin`
4. <span data-ttu-id="babba-117">Kontrollera Ruby-installationen genom att köra kommandot `ruby -v` och se den installerade versionen.</span><span class="sxs-lookup"><span data-stu-id="babba-117">Test the Ruby installation by running the command `ruby -v` to see the version installed.</span></span>
5. <span data-ttu-id="babba-118">Kontrollera Gem-installationen genom att köra kommandot `gem -v` och se den installerade versionen.</span><span class="sxs-lookup"><span data-stu-id="babba-118">Test the Gem installation by running the command `gem -v` to see the version installed.</span></span>
6. <span data-ttu-id="babba-119">Skapa Mysql2-modulen för Ruby med Gem genom att köra kommandot `gem install mysql2`.</span><span class="sxs-lookup"><span data-stu-id="babba-119">Build the Mysql2 module for Ruby using Gem by running the command `gem install mysql2`.</span></span>

### <a name="macos"></a><span data-ttu-id="babba-120">MacOS</span><span class="sxs-lookup"><span data-stu-id="babba-120">MacOS</span></span>
1. <span data-ttu-id="babba-121">Installera Ruby med Homebrew genom att köra kommandot `brew install ruby`.</span><span class="sxs-lookup"><span data-stu-id="babba-121">Install Ruby using Homebrew by running the command `brew install ruby`.</span></span> <span data-ttu-id="babba-122">Fler installationsalternativ finns i [installationsdokumentationen](https://www.ruby-lang.org/en/documentation/installation/#homebrew) för Ruby.</span><span class="sxs-lookup"><span data-stu-id="babba-122">For more installation options, see the Ruby [installation documentation](https://www.ruby-lang.org/en/documentation/installation/#homebrew).</span></span>
2. <span data-ttu-id="babba-123">Kontrollera Ruby-installationen genom att köra kommandot `ruby -v` och se den installerade versionen.</span><span class="sxs-lookup"><span data-stu-id="babba-123">Test the Ruby installation by running the command `ruby -v` to see the version installed.</span></span>
3. <span data-ttu-id="babba-124">Kontrollera Gem-installationen genom att köra kommandot `gem -v` och se den installerade versionen.</span><span class="sxs-lookup"><span data-stu-id="babba-124">Test the Gem installation by running the command `gem -v` to see the version installed.</span></span>
4. <span data-ttu-id="babba-125">Skapa Mysql2-modulen för Ruby med Gem genom att köra kommandot `gem install mysql2`.</span><span class="sxs-lookup"><span data-stu-id="babba-125">Build the Mysql2 module for Ruby using Gem by running the command `gem install mysql2`.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="babba-126">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="babba-126">Linux (Ubuntu)</span></span>
1. <span data-ttu-id="babba-127">Installera Ruby genom att köra kommandot `sudo apt-get install ruby-full`.</span><span class="sxs-lookup"><span data-stu-id="babba-127">Install Ruby by running the command `sudo apt-get install ruby-full`.</span></span> <span data-ttu-id="babba-128">Fler installationsalternativ finns i [installationsdokumentationen](https://www.ruby-lang.org/en/documentation/installation/) för Ruby.</span><span class="sxs-lookup"><span data-stu-id="babba-128">For more installation options, see the Ruby [installation documentation](https://www.ruby-lang.org/en/documentation/installation/).</span></span>
2. <span data-ttu-id="babba-129">Kontrollera Ruby-installationen genom att köra kommandot `ruby -v` och se den installerade versionen.</span><span class="sxs-lookup"><span data-stu-id="babba-129">Test the Ruby installation by running the command `ruby -v` to see the version installed.</span></span>
3. <span data-ttu-id="babba-130">Installera de senaste uppdateringarna för Gem genom att köra kommandot `sudo gem update --system`.</span><span class="sxs-lookup"><span data-stu-id="babba-130">Install the latest updates for Gem by running the command `sudo gem update --system`.</span></span>
4. <span data-ttu-id="babba-131">Kontrollera Gem-installationen genom att köra kommandot `gem -v` och se den installerade versionen.</span><span class="sxs-lookup"><span data-stu-id="babba-131">Test the Gem installation by running the command `gem -v` to see the version installed.</span></span>
5. <span data-ttu-id="babba-132">Installera gcc, make och andra genereringsverktyg genom att köra kommandot `sudo apt-get install build-essential`.</span><span class="sxs-lookup"><span data-stu-id="babba-132">Install the gcc, make, and other build tools by running the command `sudo apt-get install build-essential`.</span></span>
6. <span data-ttu-id="babba-133">Installera MySQL-klientbibliotek för utvecklare genom att köra kommandot `sudo apt-get install libmysqlclient-dev`.</span><span class="sxs-lookup"><span data-stu-id="babba-133">Install the MySQL client developer libraries by running the command `sudo apt-get install libmysqlclient-dev`.</span></span>
7. <span data-ttu-id="babba-134">Skapa mysql2-modulen för Ruby med Gem genom att köra kommandot `sudo gem install mysql2`.</span><span class="sxs-lookup"><span data-stu-id="babba-134">Build the mysql2 module for Ruby using Gem by running the command `sudo gem install mysql2`.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="babba-135">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="babba-135">Get connection information</span></span>
<span data-ttu-id="babba-136">Skaffa den information som du behöver för att ansluta till Azure Database för MySQL.</span><span class="sxs-lookup"><span data-stu-id="babba-136">Get the connection information needed to connect to the Azure Database for MySQL.</span></span> <span data-ttu-id="babba-137">Du behöver det fullständiga servernamnet och inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="babba-137">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="babba-138">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="babba-138">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="babba-139">På den vänstra menyn i Azure Portal klickar du på **Alla resurser** och söker efter den server som du nyss skapade, till exempel **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="babba-139">From the left-hand menu in Azure portal, click **All resources** and search for the server you have creased, such as **myserver4demo**.</span></span>
3. <span data-ttu-id="babba-140">Klicka på servernamnet **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="babba-140">Click the server name **myserver4demo**.</span></span>
4. <span data-ttu-id="babba-141">Välj sidan **Egenskaper** för servern.</span><span class="sxs-lookup"><span data-stu-id="babba-141">Select the server's **Properties** page.</span></span> <span data-ttu-id="babba-142">Anteckna **servernamn** och **inloggningsnamnet för serveradministratören**.</span><span class="sxs-lookup"><span data-stu-id="babba-142">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="babba-143">![Azure Database för MySQL – inloggning för serveradministratör](./media/connect-ruby/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="babba-143">![Azure Database for MySQL - Server Admin Login](./media/connect-ruby/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="babba-144">Om du glömmer inloggningsinformationen för servern öppnar du sidan **Översikt** för att se inloggningsnamnet för serveradministratören. Om det behövs kan du återställa lösenordet.</span><span class="sxs-lookup"><span data-stu-id="babba-144">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="run-ruby-code"></a><span data-ttu-id="babba-145">Köra Ruby-kod</span><span class="sxs-lookup"><span data-stu-id="babba-145">Run Ruby code</span></span> 
1. <span data-ttu-id="babba-146">Klistra in Ruby-koden nedan i textfiler och spara filerna till en projektmapp med filtillägget .rb `C:\rubymysql\createtable.rb` eller `/home/username/rubymysql/createtable.rb`.</span><span class="sxs-lookup"><span data-stu-id="babba-146">Paste the Ruby code from the sections below into text files, and save the files into a project folder with file extension .rb, such as `C:\rubymysql\createtable.rb` or `/home/username/rubymysql/createtable.rb`.</span></span>
2. <span data-ttu-id="babba-147">Om du vill köra koden startar du kommandotolken eller bash-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="babba-147">To run the code, launch the command prompt or bash shell.</span></span> <span data-ttu-id="babba-148">Ändra katalog till din projektmapp, till exempel `cd rubymysql`</span><span class="sxs-lookup"><span data-stu-id="babba-148">Change directory into your project folder `cd rubymysql`</span></span>
3. <span data-ttu-id="babba-149">Skriv Ruby-kommandot följt av filnamnet, till exempel `ruby createtable.rb`, för att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="babba-149">Then type the ruby command followed by the file name, such as `ruby createtable.rb` to run the application.</span></span>
4. <span data-ttu-id="babba-150">I Windows, om Ruby-programmet inte finns på sökvägen för miljövariabeln, kan du behöva använda den fullständiga sökvägen för att starta nodprogrammet, till exempel `"c:\Ruby23-x64\bin\ruby.exe" createtable.rb`</span><span class="sxs-lookup"><span data-stu-id="babba-150">On the Windows OS, if the ruby application is not in your path environment variable, you may need to use the full path to launch the node application, such as `"c:\Ruby23-x64\bin\ruby.exe" createtable.rb`</span></span>

## <a name="connect-and-create-a-table"></a><span data-ttu-id="babba-151">Ansluta och skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="babba-151">Connect and create a table</span></span>
<span data-ttu-id="babba-152">Använd följande kod för att ansluta och skapa en tabell med hjälp av **CREATE TABLE**-SQL-instruktionen följt av **INSERT INTO**-SQL-instruktioner för att lägga till rader i tabellen.</span><span class="sxs-lookup"><span data-stu-id="babba-152">Use the following code to connect and create a table using **CREATE TABLE** SQL statement, followed by **INSERT INTO** SQL statements to add rows into the table.</span></span>

<span data-ttu-id="babba-153">Koden använder en [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8)-klass .new() metod för att ansluta till Azure-databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="babba-153">The code uses a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) class .new() method to connect to Azure Database for MySQL.</span></span> <span data-ttu-id="babba-154">Sedan anropas metoden [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) flera gånger för att köra kommandona DROP, CREATE TABLE och INSERT INTO.</span><span class="sxs-lookup"><span data-stu-id="babba-154">Then it calls method [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) several times to run the DROP, CREATE TABLE, and INSERT INTO commands.</span></span> <span data-ttu-id="babba-155">Sedan anropas metoden [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) för att stänga anslutningen.</span><span class="sxs-lookup"><span data-stu-id="babba-155">Then it calls method [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) to close the connection before terminating.</span></span>

<span data-ttu-id="babba-156">Ersätt strängarna `host`, `database`, `username` och `password` med egna värden.</span><span class="sxs-lookup"><span data-stu-id="babba-156">Replace the `host`, `database`, `username`, and `password` strings with your own values.</span></span> 
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
    puts 'Successfully created connection to database.'

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

## <a name="read-data"></a><span data-ttu-id="babba-157">Läsa data</span><span class="sxs-lookup"><span data-stu-id="babba-157">Read data</span></span>
<span data-ttu-id="babba-158">Använd följande kod för att ansluta och läsa data med en **SELECT**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="babba-158">Use the following code to connect and read the data using a **SELECT** SQL statement.</span></span> 

<span data-ttu-id="babba-159">Koden använder en [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8)-klass .new() metod för att ansluta till Azure-databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="babba-159">The code uses a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) class .new() method to connect to Azure Database for MySQL.</span></span> <span data-ttu-id="babba-160">Sedan anropas metoden [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) för att köra kommandot SELECT.</span><span class="sxs-lookup"><span data-stu-id="babba-160">Then it calls method [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) to run the SELECT commands.</span></span> <span data-ttu-id="babba-161">Sedan anropas metoden [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) för att stänga anslutningen.</span><span class="sxs-lookup"><span data-stu-id="babba-161">Then it calls method [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) to close the connection before terminating.</span></span>

<span data-ttu-id="babba-162">Ersätt strängarna `host`, `database`, `username` och `password` med egna värden.</span><span class="sxs-lookup"><span data-stu-id="babba-162">Replace the `host`, `database`, `username`, and `password` strings with your own values.</span></span> 

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
    puts 'Successfully created connection to database.'

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

## <a name="update-data"></a><span data-ttu-id="babba-163">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="babba-163">Update data</span></span>
<span data-ttu-id="babba-164">Använd följande kod för att ansluta och uppdatera data med en **UPDATE**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="babba-164">Use the following code to connect and update the data using a **UPDATE** SQL statement.</span></span>

<span data-ttu-id="babba-165">Koden använder en [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8)-klass .new() metod för att ansluta till Azure-databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="babba-165">The code uses a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) class .new() method to connect to Azure Database for MySQL.</span></span> <span data-ttu-id="babba-166">Sedan anropas metoden [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) för att köra kommandot UPDATE.</span><span class="sxs-lookup"><span data-stu-id="babba-166">Then it calls method [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) to run the UPDATE commands.</span></span> <span data-ttu-id="babba-167">Sedan anropas metoden [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) för att stänga anslutningen.</span><span class="sxs-lookup"><span data-stu-id="babba-167">Then it calls method [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) to close the connection before terminating.</span></span>

<span data-ttu-id="babba-168">Ersätt strängarna `host`, `database`, `username` och `password` med egna värden.</span><span class="sxs-lookup"><span data-stu-id="babba-168">Replace the `host`, `database`, `username`, and `password` strings with your own values.</span></span> 

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
    puts 'Successfully created connection to database.'

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


## <a name="delete-data"></a><span data-ttu-id="babba-169">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="babba-169">Delete data</span></span>
<span data-ttu-id="babba-170">Använd följande kod för att ansluta och läsa data med en **DELETE**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="babba-170">Use the following code to connect and read the data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="babba-171">Koden använder en [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8)-klass .new() metod för att ansluta till Azure-databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="babba-171">The code uses a [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) class .new() method to connect to Azure Database for MySQL.</span></span> <span data-ttu-id="babba-172">Sedan anropas metoden [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) för att köra kommandot DELETE.</span><span class="sxs-lookup"><span data-stu-id="babba-172">Then it calls method [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) to run the DELETE commands.</span></span> <span data-ttu-id="babba-173">Sedan anropas metoden [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) för att stänga anslutningen.</span><span class="sxs-lookup"><span data-stu-id="babba-173">Then it calls method [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) to close the connection before terminating.</span></span>

<span data-ttu-id="babba-174">Ersätt strängarna `host`, `database`, `username` och `password` med egna värden.</span><span class="sxs-lookup"><span data-stu-id="babba-174">Replace the `host`, `database`, `username`, and `password` strings with your own values.</span></span> 

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
    puts 'Successfully created connection to database.'

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

## <a name="next-steps"></a><span data-ttu-id="babba-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="babba-175">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="babba-176">Migrera din databas med Exportera och importera</span><span class="sxs-lookup"><span data-stu-id="babba-176">Migrate your database using Export and Import</span></span>](./concepts-migrate-import-export.md)
