---
title: "Kom igång med elastiska Databasverktyg | Microsoft Docs"
description: "Grundläggande förklaring av funktionen för elastisk databas verktyg i Azure SQL Database, inklusive ett enkelt och kör sample-appen."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: CarlRabeler
ms.assetid: b6911f8d-2bae-4d04-9fa8-f79a3db7129d
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: ddove
ms.openlocfilehash: 637463399593f4bc9ff5bfcbf67bf93b816efc7f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-elastic-database-tools"></a>Kom igång med elastiska Databasverktyg
Det här dokumentet presenteras developer-upplevelsen genom att hjälpa dig att köra sample-appen. Exemplet skapar ett enkelt delat program och utforskar viktiga funktioner för elastiska Databasverktyg. Exemplet visar funktionerna i den [klientbibliotek för elastisk databas](sql-database-elastic-database-client-library.md).

Om du vill installera biblioteket, gå till [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Biblioteket har installerats med exempelappen som beskrivs i följande avsnitt.

## <a name="prerequisites"></a>Krav
* Visual Studio 2012 eller senare med C#. Hämta en kostnadsfri version på [Visual Studio-hämtningar](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
* NuGet 2.7 eller senare. Om du vill hämta den senaste versionen finns [installera NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).

## <a name="download-and-run-the-sample-app"></a>Hämta och kör exempelappen
Den **elastisk DB-verktyg för Azure SQL - komma igång** exempelprogrammet visar de viktigaste aspekterna i utvecklingsarbetet för delat program som använder elastisk Databasverktyg. Den fokuserar på viktiga användningsområden för [Fragmentera kartan management](sql-database-elastic-scale-shard-map-management.md), [data beroende routning](sql-database-elastic-scale-data-dependent-routing.md), och [flera Fragmentera frågar](sql-database-elastic-scale-multishard-querying.md). Följ dessa steg om du vill hämta och köra exemplet: 

1. Hämta den [elastisk DB-verktyg för Azure SQL - komma igång exempel](https://code.msdn.microsoft.com/windowsapps/Elastic-Scale-with-Azure-a80d8dc6) på MSDN. Packa upp exemplet på en plats som du väljer.

2. Om du vill skapa ett projekt, öppna den **ElasticScaleStarterKit.sln** lösningar från den **C#** directory.

3. Öppna i lösningen för exempelprojektet den **app.config** fil. Följ sedan instruktionerna i filen för att lägga till din Azure SQL Database-servernamn och din inloggningsinformation (användarnamn och lösenord).

4. Skapa och köra programmet. När du uppmanas, aktivera Visual Studio för att återställa NuGet-paket för lösningen. Då hämtas den senaste versionen av klientbiblioteket för elastisk databas från NuGet.

5. Experimentera med olika alternativ för mer information om funktioner för klient-biblioteket. Observera hur programmet tar i konsolen utdata och gärna att utforska koden i bakgrunden.
   
    ![Pågår][4]

Grattis--har du skapat och köra ditt första delat program med elastiska Databasverktyg på SQL-databas. Använd Visual Studio eller SQL Server Management Studio för att ansluta till SQL-databasen och ta en titt på shards som exemplet skapas. Du ser nya exempeldatabaserna Fragmentera och en Fragmentera kartan manager-databas som har skapats av exemplet.

> [!IMPORTANT]
> Vi rekommenderar att du alltid använder den senaste versionen av Management Studio så att du vara synkroniserat med uppdateringar till Azure och SQL-databas. [Uppdatera SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

### <a name="key-pieces-of-the-code-sample"></a>Nyckelinformation kodexemplet
* **Hantera delar och Fragmentera**: koden illustrerar hur du arbetar med shards, intervall och avbildningar i filen **ShardManagementUtils.cs**. Mer information finns i [skala ut databaser med Fragmentera kartan manager](http://go.microsoft.com/?linkid=9862595).  

* **Data dependent routning**: routning av transaktioner till rätt Fragmentera visas i **DataDependentRoutingSample.cs**. Mer information finns i [Data beroende routning](http://go.microsoft.com/?linkid=9862596). 

* **Fråga över flera delar**: frågar över shards illustreras i filen **MultiShardQuerySample.cs**. Mer information finns i [flera Fragmentera frågar](http://go.microsoft.com/?linkid=9862597).

* **Lägga till tomma shards**: iterativ tillägg av ny tom shards utförs av koden i filen **CreateShardSample.cs**. Mer information finns i [skala ut databaser med Fragmentera kartan manager](http://go.microsoft.com/?linkid=9862595).

### <a name="other-elastic-scale-operations"></a>Andra åtgärder elastisk skalbarhet
* **Dela en befintlig Fragmentera**: möjlighet att dela shards tillhandahålls av den **för delade sökvägssammanslagning**. Mer information finns i [flytta data mellan databaser som skalats ut molnet](sql-database-elastic-scale-overview-split-and-merge.md).

* **Sammanslagning av befintlig shards**: Fragmentera sammanslagningar görs också med hjälp av den **för delade sökvägssammanslagning**. Mer information finns i [flytta data mellan databaser som skalats ut molnet](sql-database-elastic-scale-overview-split-and-merge.md).   

## <a name="cost"></a>Kostnad
Elastisk Databasverktyg är kostnadsfri. När du använder elastisk Databasverktyg kan får du inte ytterligare avgifter ovanpå kostnaden för din användning av Azure. 

Till exempel skapas exempelprogrammet nya databaser. Kostnaden för den här beror på SQL-databas-utgåva som du väljer och Azure användningen av ditt program.

Information om priser finns [SQL-databas prisinformation](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Nästa steg
Mer information om elastisk Databasverktyg finns på följande sidor:

* Kodexempel: 
  * [Elastisk DB verktyg för Azure SQL - komma igång](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE)
  * [Elastisk DB verktyg för Azure SQL - Entity Framework-integrering](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
  * [Fragmentera elasticitet på Script Center](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
* Blogg: [elastisk skalbarhet meddelande](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
* Microsoft Virtual Academy: [implementera skalbar med hjälp av delning med elastisk databas klienten biblioteket Video](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554?l=lWyQhF1fC_6306218965) 
* Channel 9: [elastisk skalbarhet översikten](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
* Diskussionsforum: [Azure SQL Database-forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
* Att mäta prestanda: [prestandaräknare för Fragmentera kartan manager](sql-database-elastic-database-client-library.md)

<!--Anchors-->
[The Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run the Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png

