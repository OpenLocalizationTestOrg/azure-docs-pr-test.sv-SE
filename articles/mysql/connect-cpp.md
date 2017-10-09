---
title: "Ansluta tooAzure databas för MySQL från C++ | Microsoft Docs"
description: "Denna Snabbstart innehåller en C++-kodexempel som du kan använda tooconnect och fråga efter data från Azure-databas för MySQL."
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
ms.openlocfilehash: d027597bf02b1eacab9b8808957399f6e54e63cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-connectorc-tooconnect-and-query-data"></a><span data-ttu-id="eda8a-103">Azure-databas för MySQL: Använd Connector/C++ tooconnect och fråga data</span><span class="sxs-lookup"><span data-stu-id="eda8a-103">Azure Database for MySQL: Use Connector/C++ tooconnect and query data</span></span>
<span data-ttu-id="eda8a-104">Den här snabbstarten visar hur tooconnect tooan Azure-databas för MySQL med C++-program.</span><span class="sxs-lookup"><span data-stu-id="eda8a-104">This quickstart demonstrates how tooconnect tooan Azure Database for MySQL using a C++ application.</span></span> <span data-ttu-id="eda8a-105">Den visar hur toouse SQL-instruktioner tooquery infoga, uppdatera och ta bort data i hello-databas.</span><span class="sxs-lookup"><span data-stu-id="eda8a-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="eda8a-106">hello förutsätter stegen i den här artikeln att du är bekant med att utveckla med C++ och att du är ny tooworking med Azure-databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="eda8a-106">hello steps in this article assume that you are familiar with developing using C++, and that you are new tooworking with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eda8a-107">Krav</span><span class="sxs-lookup"><span data-stu-id="eda8a-107">Prerequisites</span></span>
<span data-ttu-id="eda8a-108">Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="eda8a-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="eda8a-109">Skapa en Azure Database för MySQL med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="eda8a-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="eda8a-110">Skapa en Azure Database för MySQL-server med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="eda8a-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

<span data-ttu-id="eda8a-111">Du måste också:</span><span class="sxs-lookup"><span data-stu-id="eda8a-111">You also need to:</span></span>
- <span data-ttu-id="eda8a-112">Installera [.NET Framework](https://www.microsoft.com/net/download)</span><span class="sxs-lookup"><span data-stu-id="eda8a-112">Install [.NET Framework](https://www.microsoft.com/net/download)</span></span>
- <span data-ttu-id="eda8a-113">Installera [Visual Studio](https://www.visualstudio.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="eda8a-113">Install [Visual Studio](https://www.visualstudio.com/downloads/)</span></span>
- <span data-ttu-id="eda8a-114">Installera [MySQL Connector/C++](https://dev.mysql.com/downloads/connector/cpp/)</span><span class="sxs-lookup"><span data-stu-id="eda8a-114">Install [MySQL Connector/C++](https://dev.mysql.com/downloads/connector/cpp/)</span></span> 
- <span data-ttu-id="eda8a-115">Installera [Boost](http://www.boost.org/)</span><span class="sxs-lookup"><span data-stu-id="eda8a-115">Install [Boost](http://www.boost.org/)</span></span>

## <a name="install-visual-studio-and-net"></a><span data-ttu-id="eda8a-116">Installera Visual Studio och .NET</span><span class="sxs-lookup"><span data-stu-id="eda8a-116">Install Visual Studio and .NET</span></span>
<span data-ttu-id="eda8a-117">hello stegen i det här avsnittet förutsätter att du är bekant med att utveckla med hjälp av .NET.</span><span class="sxs-lookup"><span data-stu-id="eda8a-117">hello steps in this section assume that you are familiar with developing using .NET.</span></span>

### <a name="windows"></a><span data-ttu-id="eda8a-118">**Windows**</span><span class="sxs-lookup"><span data-stu-id="eda8a-118">**Windows**</span></span>
1. <span data-ttu-id="eda8a-119">Installera Visual Studio 2017 Community, en komplett, utbyggbar och kostnadsfri IDE som du kan använda för att skapa moderna program för Android, iOS, Windows, samt webb- och databasprogram och molntjänster.</span><span class="sxs-lookup"><span data-stu-id="eda8a-119">Install Visual Studio 2017 Community, which is a full featured, extensible, free IDE for creating modern applications for Android, iOS, Windows, as well as web & database applications and cloud services.</span></span> <span data-ttu-id="eda8a-120">Du kan installera antingen hello fullständig .NET Framework eller bara .NET Core.</span><span class="sxs-lookup"><span data-stu-id="eda8a-120">You can install either hello full .NET Framework or just .NET Core.</span></span> <span data-ttu-id="eda8a-121">Hej kodfragment i hello Quickstart arbeta med antingen.</span><span class="sxs-lookup"><span data-stu-id="eda8a-121">hello code snippets in hello Quickstart work with either.</span></span> <span data-ttu-id="eda8a-122">Hoppa över hello nästa två steg om du redan har Visual Studio installerat på datorn.</span><span class="sxs-lookup"><span data-stu-id="eda8a-122">If you already have Visual Studio installed on your machine, skip hello next two steps.</span></span>
   - <span data-ttu-id="eda8a-123">Hämta hello [Visual Studio 2017 installer](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15).</span><span class="sxs-lookup"><span data-stu-id="eda8a-123">Download hello [Visual Studio 2017 installer](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15).</span></span> 
   - <span data-ttu-id="eda8a-124">Kör installationsprogrammet för hello och följ hello installationen prompter toocomplete hello installationen.</span><span class="sxs-lookup"><span data-stu-id="eda8a-124">Run hello installer and follow hello installation prompts toocomplete hello installation.</span></span>

### <a name="configure-visual-studio"></a><span data-ttu-id="eda8a-125">**Konfigurera Visual Studio**</span><span class="sxs-lookup"><span data-stu-id="eda8a-125">**Configure Visual Studio**</span></span>
1. <span data-ttu-id="eda8a-126">Projektet egenskapen från Visual Studio > konfigurationsegenskaper > C/C++ > linker > Allmänt > ytterligare bibliotek kataloger, lägga till hello lib\opt katalog (dvs: C:\Program Files (x86) \MySQL\MySQL Connector C++ 1.1.9\lib\opt) av hello c ++ koppling.</span><span class="sxs-lookup"><span data-stu-id="eda8a-126">From Visual Studio, project property > configuration properties > C/C++ > linker > general > additional library directories, add hello lib\opt directory (i.e.: C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\lib\opt) of hello c++ connector.</span></span>
2. <span data-ttu-id="eda8a-127">Från Visual Studio, projektegenskap > konfigurationsegenskaper > C/C++ > allmänt > ytterligare inkluderingskataloger</span><span class="sxs-lookup"><span data-stu-id="eda8a-127">From Visual Studio, project property > configuration properties > C/C++ > general > additional include directories</span></span>
   - <span data-ttu-id="eda8a-128">Lägg till katalog include/ för c++ connector (t.ex.: C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\include\)</span><span class="sxs-lookup"><span data-stu-id="eda8a-128">Add include/ directory of c++ connector (i.e.: C:\Program Files (x86)\MySQL\MySQL Connector C++ 1.1.9\include\)</span></span>
   - <span data-ttu-id="eda8a-129">Lägg till Boost-biblioteksrotkatalog (t.ex.: C:\boost_1_64_0\)</span><span class="sxs-lookup"><span data-stu-id="eda8a-129">Add Boost library's root directory (i.e.: C:\boost_1_64_0\)</span></span>
3. <span data-ttu-id="eda8a-130">Projektet egenskapen från Visual Studio > konfigurationsegenskaper > C/C++ > linker > indata > ytterligare beroenden, Lägg till mysqlcppconn.lib hello textfält</span><span class="sxs-lookup"><span data-stu-id="eda8a-130">From Visual Studio, project property > configuration properties > C/C++ > linker > Input > Additional Dependencies, add mysqlcppconn.lib into hello text field</span></span>
4. <span data-ttu-id="eda8a-131">Antingen kopiera mysqlcppconn.dll från hello c ++ connector biblioteksmapp i steg 3 toohello samma katalog som hello körbar Programdel eller lägga till den toohello miljövariabeln så att programmet kan hitta den.</span><span class="sxs-lookup"><span data-stu-id="eda8a-131">Either copy mysqlcppconn.dll from hello c++ connector library folder in step 3 toohello same directory as hello application executable or add it toohello environment variable so your application can find it.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="eda8a-132">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="eda8a-132">Get connection information</span></span>
<span data-ttu-id="eda8a-133">Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="eda8a-133">Get hello connection information needed tooconnect toohello Azure Database for MySQL.</span></span> <span data-ttu-id="eda8a-134">Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.</span><span class="sxs-lookup"><span data-stu-id="eda8a-134">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="eda8a-135">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="eda8a-135">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="eda8a-136">Hello vänstra menyn i Azure-portalen klickar du på **alla resurser** och Sök efter hello-server som du har skapat, exempelvis **myserver4demo**.</span><span class="sxs-lookup"><span data-stu-id="eda8a-136">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have created, such as **myserver4demo**.</span></span>
3. <span data-ttu-id="eda8a-137">Klicka på hello servernamn.</span><span class="sxs-lookup"><span data-stu-id="eda8a-137">Click hello server name.</span></span>
4. <span data-ttu-id="eda8a-138">Välj hello server **egenskaper** sidan.</span><span class="sxs-lookup"><span data-stu-id="eda8a-138">Select hello server's **Properties** page.</span></span> <span data-ttu-id="eda8a-139">Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.</span><span class="sxs-lookup"><span data-stu-id="eda8a-139">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="eda8a-140">![Azure Database för MySQL-servernamn](./media/connect-cpp/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="eda8a-140">![Azure Database for MySQL server name](./media/connect-cpp/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="eda8a-141">Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet och, om nödvändigt återställa hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="eda8a-141">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="connect-create-table-and-insert-data"></a><span data-ttu-id="eda8a-142">Ansluta, skapa tabell och infoga data</span><span class="sxs-lookup"><span data-stu-id="eda8a-142">Connect, create table, and insert data</span></span>
<span data-ttu-id="eda8a-143">Använd hello följande kod tooconnect och läsa in hello data med hjälp av **CREATE TABLE** och **INSERT INTO** SQL-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="eda8a-143">Use hello following code tooconnect and load hello data using **CREATE TABLE** and  **INSERT INTO** SQL statements.</span></span> <span data-ttu-id="eda8a-144">hello koden använder sql::Driver klass med hello connect() metoden tooestablish tooMySQL en anslutning.</span><span class="sxs-lookup"><span data-stu-id="eda8a-144">hello code uses sql::Driver class with hello connect() method tooestablish a connection tooMySQL.</span></span> <span data-ttu-id="eda8a-145">Hello koden använder sedan metoden createStatement() och execute() toorun hello databasen kommandon.</span><span class="sxs-lookup"><span data-stu-id="eda8a-145">Then hello code uses method createStatement() and execute() toorun hello database commands.</span></span> 

<span data-ttu-id="eda8a-146">Ersätt hello värden, DBName, användare och lösenord parametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="eda8a-146">Replace hello Host, DBName, User, and Password parameters with hello values that you specified when you created hello server and database.</span></span> 

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
        //for demonstration only. never save password in hello code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect toodatabase. Error message: " << e.what() << endl;
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

## <a name="read-data"></a><span data-ttu-id="eda8a-147">Läsa data</span><span class="sxs-lookup"><span data-stu-id="eda8a-147">Read data</span></span>

<span data-ttu-id="eda8a-148">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **Välj** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="eda8a-148">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> <span data-ttu-id="eda8a-149">hello koden använder sql::Driver klass med hello connect() metoden tooestablish tooMySQL en anslutning.</span><span class="sxs-lookup"><span data-stu-id="eda8a-149">hello code uses sql::Driver class with hello connect() method tooestablish a connection tooMySQL.</span></span> <span data-ttu-id="eda8a-150">Sedan hello koden använder metoden prepareStatement() och executeQuery() toorun hello Välj kommandon.</span><span class="sxs-lookup"><span data-stu-id="eda8a-150">Then hello code uses method prepareStatement() and executeQuery() toorun hello select commands.</span></span> <span data-ttu-id="eda8a-151">Slutligen använder hello kod efter tooadvance toohello poster i hello resultat.</span><span class="sxs-lookup"><span data-stu-id="eda8a-151">Finally hello code uses next() tooadvance toohello records in hello results.</span></span> <span data-ttu-id="eda8a-152">Hello koden använder sedan getInt() och getString() tooparse hello värden i hello-post.</span><span class="sxs-lookup"><span data-stu-id="eda8a-152">Then hello code uses getInt() and getString() tooparse hello values in hello record.</span></span>

<span data-ttu-id="eda8a-153">Ersätt hello värden, DBName, användare och lösenord parametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="eda8a-153">Replace hello Host, DBName, User, and Password parameters with hello values that you specified when you created hello server and database.</span></span> 

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
        //for demonstration only. never save password in hello code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect toodatabase. Error message: " << e.what() << endl;
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

## <a name="update-data"></a><span data-ttu-id="eda8a-154">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="eda8a-154">Update data</span></span>
<span data-ttu-id="eda8a-155">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **uppdatering** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="eda8a-155">Use hello following code tooconnect and read hello data using a **UPDATE** SQL statement.</span></span> <span data-ttu-id="eda8a-156">hello koden använder sql::Driver klass med hello connect() metoden tooestablish tooMySQL en anslutning.</span><span class="sxs-lookup"><span data-stu-id="eda8a-156">hello code uses sql::Driver class with hello connect() method tooestablish a connection tooMySQL.</span></span> <span data-ttu-id="eda8a-157">Hello koden använder sedan metoden prepareStatement() och executeQuery() toorun hello update-kommandon.</span><span class="sxs-lookup"><span data-stu-id="eda8a-157">Then hello code uses method prepareStatement() and executeQuery() toorun hello update commands.</span></span> 

<span data-ttu-id="eda8a-158">Ersätt hello värden, DBName, användare och lösenord parametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="eda8a-158">Replace hello Host, DBName, User, and Password parameters with hello values that you specified when you created hello server and database.</span></span> 

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
        //for demonstration only. never save password in hello code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect toodatabase. Error message: " << e.what() << endl;
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


## <a name="delete-data"></a><span data-ttu-id="eda8a-159">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="eda8a-159">Delete data</span></span>
<span data-ttu-id="eda8a-160">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **ta bort** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="eda8a-160">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> <span data-ttu-id="eda8a-161">hello koden använder sql::Driver klass med hello connect() metoden tooestablish tooMySQL en anslutning.</span><span class="sxs-lookup"><span data-stu-id="eda8a-161">hello code uses sql::Driver class with hello connect() method tooestablish a connection tooMySQL.</span></span> <span data-ttu-id="eda8a-162">Hello koden använder sedan metoden prepareStatement() och executeQuery() toorun hello ta bort kommandon.</span><span class="sxs-lookup"><span data-stu-id="eda8a-162">Then hello code uses method prepareStatement() and executeQuery() toorun hello delete commands.</span></span>

<span data-ttu-id="eda8a-163">Ersätt hello värden, DBName, användare och lösenord parametrar med hello värden som du angav när du skapade hello-server och databas.</span><span class="sxs-lookup"><span data-stu-id="eda8a-163">Replace hello Host, DBName, User, and Password parameters with hello values that you specified when you created hello server and database.</span></span> 

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
        //for demonstration only. never save password in hello code!
        con = driver>connect("tcp://myserver4demo.mysql.database.azure.com:3306/quickstartdb", "myadmin@myserver4demo", "server_admin_password");
    }
    catch (sql::SQLException e)
    {
        cout << "Could not connect toodatabase. Error message: " << e.what() << endl;
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

## <a name="next-steps"></a><span data-ttu-id="eda8a-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eda8a-164">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="eda8a-165">Migrera din MySQL-databas tooAzure databas för MySQL med dump och återställning</span><span class="sxs-lookup"><span data-stu-id="eda8a-165">Migrate your MySQL database tooAzure Database for MySQL using dump and restore</span></span>](concepts-migrate-dump-restore.md)
