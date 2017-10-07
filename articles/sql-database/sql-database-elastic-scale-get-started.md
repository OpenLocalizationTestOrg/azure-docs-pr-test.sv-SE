---
title: "aaaGet igång med elastiska Databasverktyg | Microsoft Docs"
description: "Grundläggande förklaring av funktionen hello elastisk databas verktyg i Azure SQL Database, inklusive ett enkelt och kör sample-appen."
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
ms.openlocfilehash: a84e05c39dffbaef440538602f898acee6e0483a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-elastic-database-tools"></a>Kom igång med elastiska Databasverktyg
Det här dokumentet introducerar toohello developer upplevelse genom att hjälpa dig toorun hello sample-appen. hello exemplet skapar ett enkelt delat program och utforskar viktiga funktioner för elastiska Databasverktyg. hello exemplet visar funktioner för hello [klientbibliotek för elastisk databas](sql-database-elastic-database-client-library.md).

tooinstall hello-biblioteket, gå för[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). hello biblioteket har installerats med hello sample-appen som beskrivs i följande avsnitt hello.

## <a name="prerequisites"></a>Krav
* Visual Studio 2012 eller senare med C#. Hämta en kostnadsfri version på [Visual Studio-hämtningar](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
* NuGet 2.7 eller senare. tooget hello senaste versionen, se [installera NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).

## <a name="download-and-run-hello-sample-app"></a>Hämta och köra hello sample-appen
Hej **elastisk DB-verktyg för Azure SQL - komma igång** exempelprogrammet visar hello viktigaste aspekterna i hello utvecklingsarbetet för delat program som använder elastisk Databasverktyg. Den fokuserar på viktiga användningsområden för [Fragmentera kartan management](sql-database-elastic-scale-shard-map-management.md), [data beroende routning](sql-database-elastic-scale-data-dependent-routing.md), och [flera Fragmentera frågar](sql-database-elastic-scale-multishard-querying.md). toodownload och kör hello exemplet gör du följande: 

1. Hämta hello [elastisk DB-verktyg för Azure SQL - komma igång exempel](https://code.msdn.microsoft.com/windowsapps/Elastic-Scale-with-Azure-a80d8dc6) på MSDN. Packa upp hello exempel tooa plats som du väljer.

2. toocreate ett projekt, öppna hello **ElasticScaleStarterKit.sln** lösningar från hello **C#** directory.

3. Öppna hello i hello-lösning för hello exempelprojektet **app.config** fil. Följ sedan instruktionerna för hello i hello filen tooadd din Azure SQL Database-servernamn och din inloggningsinformation (användarnamn och lösenord).

4. Skapa och köra programmet hello. När du uppmanas, aktivera Visual Studio toorestore hello NuGet-paket hello lösning. Hello senaste versionen av hello klientbibliotek för elastisk databas laddas ned från NuGet.

5. Experimentera med hello olika alternativ toolearn mer om hello klientfunktioner för biblioteket. Observera hello steg hello programmet tar i hello konsolens utdata och känna sig fria tooexplore hello kod hello bakgrunden.
   
    ![Pågår][4]

Grattis--har du skapat och köra ditt första delat program med elastiska Databasverktyg på SQL-databas. Använd Visual Studio eller SQL Server Management Studio tooconnect tooyour SQL-databas och ta en titt på hello delar som hello exemplet skapas. Du kommer att märka nya exempel Fragmentera databaser och Fragmentera kartan manager-databasen hello urvalet har skapats.

> [!IMPORTANT]
> Vi rekommenderar att du alltid använder hello senaste versionen av Management Studio så att du hålla synkroniserade med uppdateringar tooAzure och SQL-databas. [Uppdatera SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

### <a name="key-pieces-of-hello-code-sample"></a>Nyckelinformation hello-kodexempel
* **Hantera delar och Fragmentera**: hello kod visar hur toowork med shards, intervall och avbildningar i hello filen **ShardManagementUtils.cs**. Mer information finns i [skala ut databaser med hello Fragmentera kartan manager](http://go.microsoft.com/?linkid=9862595).  

* **Data dependent routning**: routning av transaktioner toohello rätt Fragmentera visas i **DataDependentRoutingSample.cs**. Mer information finns i [Data beroende routning](http://go.microsoft.com/?linkid=9862596). 

* **Fråga över flera delar**: frågar över shards illustreras i hello filen **MultiShardQuerySample.cs**. Mer information finns i [flera Fragmentera frågar](http://go.microsoft.com/?linkid=9862597).

* **Lägga till tomma shards**: hello iterativ tillägg av ny tom shards utförs av hello koden i hello filen **CreateShardSample.cs**. Mer information finns i [skala ut databaser med hello Fragmentera kartan manager](http://go.microsoft.com/?linkid=9862595).

### <a name="other-elastic-scale-operations"></a>Andra åtgärder elastisk skalbarhet
* **Dela en befintlig Fragmentera**: hello kapaciteten toosplit shards tillhandahålls av hello **för delade sökvägssammanslagning**. Mer information finns i [flytta data mellan databaser som skalats ut molnet](sql-database-elastic-scale-overview-split-and-merge.md).

* **Sammanslagning av befintlig shards**: Fragmentera sammanslagningar görs också med hjälp av hello **för delade sökvägssammanslagning**. Mer information finns i [flytta data mellan databaser som skalats ut molnet](sql-database-elastic-scale-overview-split-and-merge.md).   

## <a name="cost"></a>Kostnad
hello elastisk Databasverktyg är kostnadsfri. När du använder elastisk Databasverktyg kan får du inte ytterligare avgifter ovanpå hello kostnaden för din användning av Azure. 

Till exempel skapas hello exempelprogrammet nya databaser. hello kostnaden för detta beror på hello SQL Database edition som du väljer och hello Azure användning av programmet.

Information om priser finns [SQL-databas prisinformation](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Nästa steg
Mer information om elastisk Databasverktyg finns hello följande sidor:

* Kodexempel: 
  * [Elastisk DB verktyg för Azure SQL - komma igång](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE)
  * [Elastisk DB verktyg för Azure SQL - Entity Framework-integrering](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
  * [Fragmentera elasticitet på Script Center](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
* Blogg: [elastisk skalbarhet meddelande](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
* Microsoft Virtual Academy: [implementera skalbar med hjälp av delning med hello elastisk databas klienten biblioteket Video](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554?l=lWyQhF1fC_6306218965) 
* Channel 9: [elastisk skalbarhet översikten](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
* Diskussionsforum: [Azure SQL Database-forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
* toomeasure prestanda: [prestandaräknare för Fragmentera kartan manager](sql-database-elastic-database-client-library.md)

<!--Anchors-->
[hello Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run hello Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png

