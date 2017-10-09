---
title: aaaConnect tooAzure SQL Data Warehouse | Microsoft Docs
description: "Hur toofind hello servernamn och anslutningssträng för din tooAzure SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: e52872ca-ae74-4e25-9c56-d49c85c8d0f0
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.openlocfilehash: f15e098026afb7c5efbbbfaf62b681e8cd7936bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooazure-sql-data-warehouse"></a>Ansluta tooAzure SQL Data Warehouse
Den här artikeln hjälper dig att hämta anslutna tooSQL datalagret för hello första gången.

## <a name="find-your-server-name"></a>Hitta servernamnet
Hej första steg tooconnecting tooSQL datalagret är viktigt att veta hur toofind namnet på servern.  Hello servernamnet i följande exempel hello är till exempel sample.database.windows.net. toofind hello fullständigt kvalificerade servernamn:

1. Gå toohello [Azure-portalen][Azure portal].
2. Klicka på **SQL-databaser** 
3. Klicka på hello-databas som du vill tooconnect till.
4. Leta upp hello fullständiga servernamnet.
   
    ![Fullständigt servernamn][1]

## <a name="supported-drivers-and-connection-strings"></a>Drivrutiner och anslutningssträngar som stöds
Azure SQL Data Warehouse stöder [ADO.NET][ADO.NET], [ODBC][ODBC], [PHP][PHP] och [JDBC][JDBC]. Klicka på någon av hello föregående drivrutiner toofind hello senaste versionen och dokumentation. tooautomatically generera hello anslutningssträngen för hello-drivrutin som du använder från hello Azure portal, kan du klicka på hello **visa databasanslutningssträngar** från föregående exempel hello.  Nedan visas några exempel på hur en anslutningssträng kan se ut för respektive drivrutin.

> [!NOTE]
> Överväg att ange hello anslutning timeout too300 sekunder tooallow din anslutning toosurvive korta perioder för tillfället.
> 
> 

### <a name="adonet-connection-string-example"></a>Exempel på ADO.NET-anslutningssträng
```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### <a name="odbc-connection-string-example"></a>Exempel på ODBC-anslutningssträng
```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### <a name="php-connection-string-example"></a>Exempel på PHP-anslutningssträng
```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting tooSQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### <a name="jdbc-connection-string-example"></a>Exempel på JDBC-anslutningssträng
```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="connection-settings"></a>Inställningar för anslutning
SQL Data Warehouse standardiserar några inställningar under anslutning och objektskapande. Dessa inställningar kan inte åsidosättas. Det gäller exempelvis:

| Databasinställning | Värde |
|:--- |:--- |
| [ANSI_NULLS][ANSI_NULLS] |ON |
| [QUOTED_IDENTIFIERS][QUOTED_IDENTIFIERS] |ON |
| [DATEFORMAT][DATEFORMAT] |mdy |
| [DATEFIRST][DATEFIRST] |7 |

## <a name="next-steps"></a>Nästa steg
tooconnect och fråga med Visual Studio finns [fråga med Visual Studio][Query with Visual Studio]. toolearn mer information om alternativ för användarautentisering finns [autentisering tooAzure SQL Data Warehouse][Authentication tooAzure SQL Data Warehouse].

<!--Articles-->
[Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[Authentication tooAzure SQL Data Warehouse]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx
[ANSI_NULLS]: https://msdn.microsoft.com/library/ms188048.aspx
[QUOTED_IDENTIFIERS]: https://msdn.microsoft.com/library/ms174393.aspx
[DATEFORMAT]: https://msdn.microsoft.com/library/ms189491.aspx
[DATEFIRST]: https://msdn.microsoft.com/library/ms181598.aspx

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->
[1]: media/sql-data-warehouse-connect-overview/get-server-name.png


