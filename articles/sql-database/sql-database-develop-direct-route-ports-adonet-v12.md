---
title: "aaaPorts utöver 1433 för SQL-databas | Microsoft Docs"
description: "Klientanslutningar från ADO.NET tooAzure SQL-databas ibland kringgå hello proxy och interagera direkt med hello-databasen. Andra portar än 1433 blir viktiga."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
ms.assetid: 3f17106a-92fd-4aa4-b6a9-1daa29421f64
ms.service: sql-database
ms.custom: develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: sstein
ms.openlocfilehash: a35ff2d827ae3fa29b3ea855dbb7ed78583c82eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="ports-beyond-1433-for-adonet-45"></a>Portar utöver 1433 för ADO.NET 4.5
Det här avsnittet beskriver hello Azure SQL Database anslutningsbeteendet för klienter som använder ADO.NET 4.5 eller senare. 

> [!IMPORTANT]
> Information om anslutningen arkitektur finns [Azure SQL Database connectivity arkitektur](sql-database-connectivity-architecture.md).
>

## <a name="outside-vs-inside"></a>Utanför eller innanför
För anslutningar tooAzure SQL-databas måste först ber vi om klientprogrammet körs *utanför* eller *inuti* hello Azure-molnet gräns. hello underavsnitt beskrivs två vanliga scenarier.

#### <a name="outside-client-runs-on-your-desktop-computer"></a>*Utanför:* klienten körs på datorn
Port 1433 är endast hello-port som måste vara öppna på den stationära datorn som är värd för ditt SQL Database-klientprogram.

#### <a name="inside-client-runs-on-azure"></a>*Inre:* klienten körs på Azure
När klienten körs hello Azure-molnet gräns, använder vi kan anropa en *vägen* toointeract med hello SQL Database-server. När en anslutning har upprättats kan innebära ytterligare interaktion mellan hello-klienten och databasen ingen mellanprogram proxy.

hello-sekvensen är följande:

1. ADO.NET 4.5 (eller senare) initierar en kort interaktion med hello Azure-molnet och tar emot ett dynamiskt identifierade portnummer.
   
   * hello dynamiskt identifieras portnumret är i intervallet hello 11000 11999 eller 14000 14999.
2. ADO.NET sedan ansluter toohello SQL Database-server direkt, utan mellanprogram mellan.
3. Frågor skickas direkt toohello databasen och resultat returneras direkt toohello klienten.

Se till att hello-port intervall med 11000 11999 och 14000 14999 på Azure klientdatorn lämnas tillgänglig för ADO.NET 4.5 klienten interaktioner med SQL-databas.

* I synnerhet måste portar i hello intervallet vara fria från andra utgående blockeringar.
* På den virtuella datorn i Azure, hello **Windows-brandväggen med avancerad säkerhet** kontroller hello portinställningar.
  
  * Du kan använda hello [brandväggens användargränssnittet](http://msdn.microsoft.com/library/cc646023.aspx) tooadd en regel som du anger hello **TCP** protokollet tillsammans med ett portintervall med hello syntax som **11000 11999**.

## <a name="version-clarifications"></a>Version förtydliganden
Det här avsnittet förklarar hello monikrar som refererar tooproduct versioner. Dessutom visas vissa pairings versioner mellan produkter.

#### <a name="adonet"></a>ADO.NET
* ADO.NET 4.0 stöder hello 7.3 TDS-protokollet, men inte 7.4.
* ADO.NET 4.5 och senare stöder hello 7.4 TDS-protokollet.

## <a name="related-links"></a>Relaterade länkar
* ADO.NET 4.6 gavs ut 20 juli 2015. En blogg meddelande från hello .NET-teamet finns [här](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).
* ADO.NET 4.5 gavs ut 15 augusti 2012. En blogg meddelande från hello .NET-teamet finns [här](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).
  
  * Det finns ett blogginlägg om ADO.NET 4.5.1 [här](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).
* [TDS-protokollet versionslista](http://www.freetds.org/userguide/tdshistory.htm)
* [Översikt över SQL Database-utveckling](sql-database-develop-overview.md)
* [Azure SQL Database-brandvägg](sql-database-firewall-configure.md)
* [Så här: konfigurera brandväggsinställningar på SQL-databas](sql-database-configure-firewall-settings.md)

