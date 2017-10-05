---
title: "Drivrutiner för SQL Data Warehouse | Microsoft Docs"
description: "Anslutningssträngar och drivrutiner för SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 5c91f423-b550-4734-8094-c7f2c418ac8d
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.openlocfilehash: e71ea1d23f68ed41c03bbce88b08863d2831c1bd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="drivers-for-azure-sql-data-warehouse"></a><span data-ttu-id="55821-103">Drivrutiner för Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="55821-103">Drivers for Azure SQL Data Warehouse</span></span>
<span data-ttu-id="55821-104">Du kan ansluta till SQL Data Warehouse med flera olika programprotokoll såsom [ADO.NET][ADO.NET], [ODBC][ODBC], [PHP] [ PHP] och [JDBC][JDBC].</span><span class="sxs-lookup"><span data-stu-id="55821-104">You can connect to SQL Data Warehouse with several different application protocols such as, [ADO.NET][ADO.NET], [ODBC][ODBC], [PHP][PHP] and [JDBC][JDBC].</span></span> <span data-ttu-id="55821-105">Nedan följer några exempel på anslutningar strängar för varje protokoll.</span><span class="sxs-lookup"><span data-stu-id="55821-105">Below are some examples of connections strings for each protocol.</span></span>  <span data-ttu-id="55821-106">Du kan också använda Azure-portalen för att skapa anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="55821-106">You can also use the Azure portal to build your connection string.</span></span>  <span data-ttu-id="55821-107">För att skapa anslutningssträngen med hjälp av Azure portal, navigerar du till ditt databas-blad under *Essentials* klickar du på *visa databasanslutningssträngar*.</span><span class="sxs-lookup"><span data-stu-id="55821-107">To build your connection string using the Azure portal, navigate to your database blade, under *Essentials* click on *Show database connection strings*.</span></span>

## <a name="sample-adonet-connection-string"></a><span data-ttu-id="55821-108">Exempel ADO.NET-anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="55821-108">Sample ADO.NET connection string</span></span>
```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

## <a name="sample-odbc-connection-string"></a><span data-ttu-id="55821-109">Exempel ODBC-anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="55821-109">Sample ODBC connection string</span></span>
```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

## <a name="sample-php-connection-string"></a><span data-ttu-id="55821-110">Exempel PHP-anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="55821-110">Sample PHP connection string</span></span>
```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

## <a name="sample-jdbc-connection-string"></a><span data-ttu-id="55821-111">Exempel JDBC-anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="55821-111">Sample JDBC connection string</span></span>
```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

> [!NOTE]
> <span data-ttu-id="55821-112">Överväg att ange timeout för anslutningen med 300 sekunder för att tillåta anslutning till överleva korta tidsperioder för tillfället.</span><span class="sxs-lookup"><span data-stu-id="55821-112">Consider setting the connection timeout to 300 seconds in order to allow the connection to survive short periods of  unavailability.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="55821-113">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="55821-113">Next steps</span></span>
<span data-ttu-id="55821-114">Om du vill starta frågar ditt data warehouse med Visual Studio och andra program, se [fråga med Visual Studio][Query with Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="55821-114">To start querying your data warehouse with Visual Studio and other applications, see [Query with Visual Studio][Query with Visual Studio].</span></span>

<!--Image references-->

<!--Azure.com references-->
[Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md

<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx

<!--Other references-->
