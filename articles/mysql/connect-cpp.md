---
title: "Ansluta till Azure Database för MySQL från C++ | Microsoft Docs"
description: "Den här snabbstarten innehåller ett kodexempel i C++ som du kan använda för att ansluta till och fråga efter data från Azure Database för MySQL."
services: mysql
author: seanli1988
ms.author: seal
manager: janders
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.devlang: C++
ms.topic: hero-article
ms.date: 08/03/2017
ms.openlocfilehash: 63388b83b913d95136140fa4c56af0dbebbdad81
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-database-for-mysql-use-connectorc-to-connect-and-query-data"></a><span data-ttu-id="4eafb-103">Azure Database för MySQL: Använda Connector/C++ för att ansluta och fråga efter data</span><span class="sxs-lookup"><span data-stu-id="4eafb-103">Azure Database for MySQL: Use Connector/C++ to connect and query data</span></span>
<span data-ttu-id="4eafb-104">Den här snabbstarten visar hur du ansluter till en Azure Database för MySQL med hjälp av ett C++-program.</span><span class="sxs-lookup"><span data-stu-id="4eafb-104">This quickstart demonstrates how to connect to an Azure Database for MySQL using a C++ application.</span></span> <span data-ttu-id="4eafb-105">Den visar hur du använder SQL-instruktioner för att fråga, infoga, uppdatera och ta bort data i databasen.</span><span class="sxs-lookup"><span data-stu-id="4eafb-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="4eafb-106">I den här artikeln förutsätter vi att du har kunskaper om C++ och att du inte har arbetat med Azure Database för MySQL tidigare.</span><span class="sxs-lookup"><span data-stu-id="4eafb-106">The steps in this article assume that you are familiar with developing using C++, and that you are new to working with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4eafb-107">Krav</span><span class="sxs-lookup"><span data-stu-id="4eafb-107">Prerequisites</span></span>
<span data-ttu-id="4eafb-108">I den här snabbstarten används de resurser som skapades i någon av följande guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="4eafb-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="4eafb-109">Skapa en Azure Database för MySQL med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4eafb-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="4eafb-110">Skapa en Azure Database för MySQL-server med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="4eafb-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

<span data-ttu-id="4eafb-111">Du måste också:</span><span class="sxs-lookup"><span data-stu-id="4eafb-111">You also need to:</span></span>
- <span data-ttu-id="4eafb-112">Installera [.NET Framework](https://www.microsoft.com/net/download)</span><span class="sxs-lookup"><span data-stu-id="4eafb-112">Install [.NET Framework](https://www.microsoft.com/net/download)</span></span>
- <span data-ttu-id="4eafb-113">Installera [Visual Studio](https://www.visualstudio.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="4eafb-113">Install [Visual Studio](https://www.visualstudio.com/downloads/)</span></span>
- <span data-ttu-id="4eafb-114">Installera [MySQL Connector/C++](https://dev.mysql.com/downloads/connector/cpp/)</span><span class="sxs-lookup"><span data-stu-id="4eafb-114">Install [MySQL Connector/C++](https://dev.mysql.com/downloads/connector/cpp/)</span></span> 
- <span data-ttu-id="4eafb-115">Installera [Boost](http://www.boost.org/)</span><span class="sxs-lookup"><span data-stu-id="4eafb-115">Install [Boost](http://www.boost.org/)</span></span>

## <a name="install-visual-studio-and-net"></a><span data-ttu-id="4eafb-116">Installera Visual Studio och .NET</span><span class="sxs-lookup"><span data-stu-id="4eafb-116">Install Visual Studio and .NET</span></span>
<span data-ttu-id="4eafb-117">I stegen i det här avsnittet förutsätter vi att du har erfarenhet av att utveckla med .NET.</span><span class="sxs-lookup"><span data-stu-id="4eafb-117">The steps in this section assume that you are familiar with developing using .NET.</span></span>

### <a name="windows"></a><span data-ttu-id="4eafb-118">**Windows**</span><span class="sxs-lookup"><span data-stu-id="4eafb-118">**Windows**</span></span>
1. <span data-ttu-id="4eafb-119">Installera Visual Studio 2017 Community, en komplett, utbyggbar och kostnadsfri IDE som du kan använda för att skapa moderna program för Android, iOS, Windows, samt webb- och databasprogram och molntjänster.</span><span class="sxs-lookup"><span data-stu-id="4eafb-119">Install Visual Studio 2017 Community, which is a full featured, extensible, free IDE for creating modern applications for Android, iOS, Windows, as well as web & database applications and cloud services.</span></span> <span data-ttu-id="4eafb-120">Du kan installera den fullständiga versionen av .NET Framework eller bara.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4eafb-120">You can install either the full .NET Framework or just .NET Core.</span></span> <span data-ttu-id="4eafb-121">Kodsnuttarna i snabbstarten fungerar för båda.</span><span class="sxs-lookup"><span data-stu-id="4eafb-121">The code snippets in the Quickstart work with either.</span></span> <span data-ttu-id="4eafb-122">Om du redan har Visual Studio installerat på datorn kan du hoppa över de kommande två stegen.</span><span class="sxs-lookup"><span data-stu-id="4eafb-122">If you already have Visual Studio installed on your machine, skip the next two steps.</span></span>
   - <span data-ttu-id="4eafb-123">Ladda ned [installationsprogrammet för Visual Studio 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15).</span><span class="sxs-lookup"><span data-stu-id="4eafb-123">Download the [Visual Studio 2017 installer](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15).</span></span> 
   - <span data-ttu-id="4eafb-124">Kör installationsprogrammet och följ anvisningarna för att slutföra installationen.</span><span class="sxs-lookup"><span data-stu-id="4eafb-124">Run the installer and follow the installation prompts to complete the installation.</span></span>

### <a name="configure-visual-studio"></a><span data-ttu-id="4eafb-125">**Konfigurera Visual Studio**</span><span class="sxs-lookup"><span data-stu-id="4eafb-125">**Configure Visual Studio**</span></span>
1. <span data-ttu-id="4eafb-126">Från Visual Studio, projektegenskap > konfigurationsegenskaper > C/C++ > länkare > allmänt > ytterligare bibliotekskataloger, lägg till katalogen lib\opt (t.ex.: C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\lib\opt) av c++ connector.</span><span class="sxs-lookup"><span data-stu-id="4eafb-126">From Visual Studio, project property > configuration properties > C/C++ > linker > general > additional library directories, add the lib\opt directory (i.e.: C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\lib\opt) of the c++ connector.</span></span>
2. <span data-ttu-id="4eafb-127">Från Visual Studio, projektegenskap > konfigurationsegenskaper > C/C++ > allmänt > ytterligare inkluderingskataloger</span><span class="sxs-lookup"><span data-stu-id="4eafb-127">From Visual Studio, project property > configuration properties > C/C++ > general > additional include directories</span></span>
   - <span data-ttu-id="4eafb-128">Lägg till katalog include/ för c++ connector (t.ex.: C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\include\)</span><span class="sxs-lookup"><span data-stu-id="4eafb-128">Add include/ directory of c++ connector (i.e.: C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\include\)</span></span>
   - <span data-ttu-id="4eafb-129">Lägg till Boost-biblioteksrotkatalog (t.ex.: C:\boost_1_64_0\)</span><span class="sxs-lookup"><span data-stu-id="4eafb-129">Add Boost library's root directory (i.e.: C:\boost_1_64_0\)</span></span>
3. <span data-ttu-id="4eafb-130">Från Visual Studio, projektegenskap > konfigurationsegenskaper > C/C++ > länkare > Indata> Ytterligare beroenden, lägger du till mysqlcppconn.lib i textfältet</span><span class="sxs-lookup"><span data-stu-id="4eafb-130">From Visual Studio, project property > configuration properties > C/C++ > linker > Input > Additional Dependencies, add mysqlcppconn.lib into the text field</span></span>
4. <span data-ttu-id="4eafb-131">Antingen kopierar du mysqlcppconn.dll från c++ connector-biblioteksmappen i steg 3 till samma katalog som den körbara programfilen eller lägger till den i miljövariabeln så att programmet kan hitta den.</span><span class="sxs-lookup"><span data-stu-id="4eafb-131">Either copy mysqlcppconn.dll from the c++ connector library folder in step 3 to the same directory as the application executable or add it to the environment variable so your application can find it.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="4eafb-132">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="4eafb-132">Get connection information</span></span>
<span data-ttu-id="4eafb-133">Skaffa den information som du behöver för att ansluta till Azure Database för MySQL.</span><span class="sxs-lookup"><span data-stu-id="4eafb-133">Get the connection information needed to connect to the Azure Database for MySQL.</span></span> <span data-ttu-id="4eafb-134">Du behöver det fullständiga servernamnet och inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="4eafb-134">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="4eafb-135">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="4eafb-135">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="4eafb-136">På den vänstra menyn i Azure Portal klickar du på **Alla resurser** och söker efter den server som du nyss skapade, till exempel **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="4eafb-136">From the left-hand menu in Azure portal, click **All resources** and search for the server you have created, such as **myserver4demo**.</span></span>
3. <span data-ttu-id="4eafb-137">Klicka på servernamnet.</span><span class="sxs-lookup"><span data-stu-id="4eafb-137">Click the server name.</span></span>
4. <span data-ttu-id="4eafb-138">Välj sidan **Egenskaper** för servern.</span><span class="sxs-lookup"><span data-stu-id="4eafb-138">Select the server's **Properties** page.</span></span> <span data-ttu-id="4eafb-139">Anteckna **servernamn** och **inloggningsnamnet för serveradministratören**.</span><span class="sxs-lookup"><span data-stu-id="4eafb-139">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="4eafb-140">![Azure Database för MySQL-servernamn](./media/connect-cpp/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="4eafb-140">![Azure Database for MySQL server name](./media/connect-cpp/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="4eafb-141">Om du glömmer inloggningsinformationen för servern öppnar du sidan **Översikt** för att se inloggningsnamnet för serveradministratören. Om det behövs kan du återställa lösenordet.</span><span class="sxs-lookup"><span data-stu-id="4eafb-141">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="4eafb-142">Ansluta, skapa tabell och infoga data</span><span class="sxs-lookup"><span data-stu-id="4eafb-142">Connect, create table, and insert data</span></span>
<span data-ttu-id="4eafb-143">Använd följande kod för att ansluta och läsa in data med hjälp av SQL-instruktionerna **CREATE TABLE** och **INSERT**.</span><span class="sxs-lookup"><span data-stu-id="4eafb-143">Use the following code to connect and load the data using **CREATE TABLE** and  **INSERT INTO** SQL statements.</span></span> <span data-ttu-id="4eafb-144">Koden använder klassen sql::Driver med metoden connect() för att upprätta en anslutning till MySQL.</span><span class="sxs-lookup"><span data-stu-id="4eafb-144">The code uses sql::Driver class with the connect() method to establish a connection to MySQL.</span></span> <span data-ttu-id="4eafb-145">Sedan används metoden createStatement() och execute() för att köra databaskommandona.</span><span class="sxs-lookup"><span data-stu-id="4eafb-145">Then the code uses method createStatement() and execute() to run the database commands.</span></span> 

<span data-ttu-id="4eafb-146">Ersätt parametrarna Host, DBName, User och Password med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="4eafb-146">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

```c++
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::Statement *stmt;
    sql::PreparedStatement *pstmt;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }

    stmt = con>createStatement();
    stmt>execute("DROP TABLE IF EXISTS inventory");
    cout << "Finished dropping table (if existed)" << endl;
    stmt>execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);");
    cout << "Finished creating table" << endl;
    delete stmt;

    pstmt = con>prepareStatement("INSERT INTO inventory(name, quantity) VALUES(?,?)");
    pstmt>setString(1, "banana");
    pstmt>setInt(2, 150);
    pstmt>execute();
    cout << "One row inserted." << endl;

    pstmt>setString(1, "orange");
    pstmt>setInt(2, 154);
    pstmt>execute();
    cout << "One row inserted." << endl;

    pstmt>setString(1, "apple");
    pstmt>setInt(2, 100);
    pstmt>execute();
    cout << "One row inserted." << endl;
    
    delete pstmt;   
    delete con;
    system("pause");
    return 0;

```

## <a name="read-data"></a><span data-ttu-id="4eafb-147">Läsa data</span><span class="sxs-lookup"><span data-stu-id="4eafb-147">Read data</span></span>

<span data-ttu-id="4eafb-148">Använd följande kod för att ansluta och läsa data med en **SELECT**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="4eafb-148">Use the following code to connect and read the data using a **SELECT** SQL statement.</span></span> <span data-ttu-id="4eafb-149">Koden använder klassen sql::Driver med metoden connect() för att upprätta en anslutning till MySQL.</span><span class="sxs-lookup"><span data-stu-id="4eafb-149">The code uses sql::Driver class with the connect() method to establish a connection to MySQL.</span></span> <span data-ttu-id="4eafb-150">Sedan används metoden prepareStatement() och executeQuery() för att köra de valda kommandona.</span><span class="sxs-lookup"><span data-stu-id="4eafb-150">Then the code uses method prepareStatement() and executeQuery() to run the select commands.</span></span> <span data-ttu-id="4eafb-151">Slutligen används metoden next () för att gå vidare till posterna i resultaten.</span><span class="sxs-lookup"><span data-stu-id="4eafb-151">Finally the code uses next() to advance to the records in the results.</span></span> <span data-ttu-id="4eafb-152">Koden använder sedan getInt() och getString() för att parsa värdena i posten.</span><span class="sxs-lookup"><span data-stu-id="4eafb-152">Then the code uses getInt() and getString() to parse the values in the record.</span></span>

<span data-ttu-id="4eafb-153">Ersätt parametrarna Host, DBName, User och Password med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="4eafb-153">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/resultset.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;
    sql::ResultSet *result;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }   

//  select  
    pstmt = con>prepareStatement("SELECT * FROM inventory;");
    result = pstmt>executeQuery();  
    
    while (result>next())
        printf("Reading from table=(%d, %s, %d)\n", result>getInt(1), result>getString(2).c_str(), result>getInt(3));   
    
    delete result;
    delete pstmt;   
    delete con;
    system("pause");
    return 0;
}
```

## <a name="update-data"></a><span data-ttu-id="4eafb-154">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="4eafb-154">Update data</span></span>
<span data-ttu-id="4eafb-155">Använd följande kod för att ansluta och läsa data med en **UPDATE**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="4eafb-155">Use the following code to connect and read the data using a **UPDATE** SQL statement.</span></span> <span data-ttu-id="4eafb-156">Koden använder klassen sql::Driver med metoden connect() för att upprätta en anslutning till MySQL.</span><span class="sxs-lookup"><span data-stu-id="4eafb-156">The code uses sql::Driver class with the connect() method to establish a connection to MySQL.</span></span> <span data-ttu-id="4eafb-157">Sedan används metoden prepareStatement() och executeQuery() för att köra uppdateringskommandona.</span><span class="sxs-lookup"><span data-stu-id="4eafb-157">Then the code uses method prepareStatement() and executeQuery() to run the update commands.</span></span> 

<span data-ttu-id="4eafb-158">Ersätt parametrarna Host, DBName, User och Password med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="4eafb-158">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }   

    //update
    pstmt = con>prepareStatement("UPDATE inventory SET quantity = ? WHERE name = ?");
    pstmt>setInt(1, 200);
    pstmt>setString(2, "banana");
    pstmt>executeQuery();
    printf("Row updated\n");
    
    delete con;
    delete pstmt;
    system("pause");
    return 0;
}
```


## <a name="delete-data"></a><span data-ttu-id="4eafb-159">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="4eafb-159">Delete data</span></span>
<span data-ttu-id="4eafb-160">Använd följande kod för att ansluta och läsa data med en **DELETE**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="4eafb-160">Use the following code to connect and read the data using a **DELETE** SQL statement.</span></span> <span data-ttu-id="4eafb-161">Koden använder klassen sql::Driver med metoden connect() för att upprätta en anslutning till MySQL.</span><span class="sxs-lookup"><span data-stu-id="4eafb-161">The code uses sql::Driver class with the connect() method to establish a connection to MySQL.</span></span> <span data-ttu-id="4eafb-162">Sedan används metoden prepareStatement() och executeQuery() för att köra borttagningskommandona.</span><span class="sxs-lookup"><span data-stu-id="4eafb-162">Then the code uses method prepareStatement() and executeQuery() to run the delete commands.</span></span>

<span data-ttu-id="4eafb-163">Ersätt parametrarna Host, DBName, User och Password med de värden som du angav när du skapade servern och databasen.</span><span class="sxs-lookup"><span data-stu-id="4eafb-163">Replace the Host, DBName, User, and Password parameters with the values that you specified when you created the server and database.</span></span> 

```csharp
#include <stdlib.h>
#include <iostream>
#include "stdafx.h"

#include "mysql_connection.h"
#include <cppconn/driver.h>
#include <cppconn/exception.h>
#include <cppconn/resultset.h>
#include <cppconn/prepared_statement.h>
using namespace std;

int main()
{
    sql::Driver *driver;
    sql::Connection *con;
    sql::PreparedStatement *pstmt;
    sql::ResultSet *result;

    try
    {
        driver = get_driver_instance();
        //for demonstration only. never save password in the code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect to database. Error message: " << e.what() << endl;
        system("pause");
        exit(1);
    }
        
    //delete
    pstmt = con>prepareStatement("DELETE FROM inventory WHERE name = ?");
    pstmt>setString(1, "orange");
    result = pstmt>executeQuery();
    printf("Row deleted\n");    
    
    delete pstmt;
    delete con;
    delete result;
    system("pause");
    return 0;
}
```

## <a name="next-steps"></a><span data-ttu-id="4eafb-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4eafb-164">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="4eafb-165">Migrera MySQL-databasen till Azure Database för MySQL med säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="4eafb-165">Migrate your MySQL database to Azure Database for MySQL using dump and restore</span></span>](concepts-migrate-dump-restore.md)
