---
title: "aaaLoad data från SQL Server till Azure SQL Data Warehouse (SSIS) | Microsoft Docs"
description: "Visar hur toocreate SQL Server Integration Services (SSIS) toomove paketdata från en mängd olika data datakällor tooSQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: e2c252e9-0828-47c2-a808-e3bea46c134a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 03/30/2017
ms.author: cakarst;douglasl;barbkess
ms.openlocfilehash: bb28a08807a5b07832b85f2f074c2acf912c1dc3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-ssis"></a>Läs in data från SQL Server till Azure SQL Data Warehouse (SSIS)
> [!div class="op_single_selector"]
> * [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [bcp](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

Skapa ett SQL Server Integration Services (SSIS) paketet tooload data från SQL Server till Azure SQL Data Warehouse. Du kan alternativt omstrukturera, transformera och rensa hello data när det överförs via hello SSIS-dataflöde.

I den här kursen ska du:

* Skapa ett nytt Integration Services-projekt i Visual Studio.
* Ansluta toodata källor, till exempel SQL Server (som en källa) och SQL Data Warehouse (som mål).
* Utforma ett SSIS-paket som läser in data från hello källa i hello mål.
* Kör hello SSIS-paket tooload hello data.

Den här kursen använder SQL Server som hello datakälla. SQL Server kan köras lokalt eller i en virtuell Azure-dator.

## <a name="basic-concepts"></a>Grundläggande begrepp
hello-paket är hello arbetsenheten i SSIS. Relaterade paket grupperas i projekt. Du skapar projekt och design paket i Visual Studio med SQL Server Data Tools. hello design processen är en visuell process där du drar och släpper komponenter från hello verktygslådan toohello designytan, ansluter dem och ange deras egenskaper. När du har ditt paket kan distribuera du om du vill den tooSQL Server för omfattande hantering, övervakning och säkerhet.

## <a name="options-for-loading-data-with-ssis"></a>Alternativ för att läsa in data med SSIS
SQL Server Integration Services (SSIS) är en flexibel uppsättning verktyg som ger en mängd olika alternativ för att ansluta till och läsa in data i SQL Data Warehouse.

1. Använd en ADO NET mål tooconnect tooSQL Data Warehouse. Den här kursen använder ett ADO NET mål eftersom den har hello minst antal konfigurationsalternativ.
2. Använd en OLE DB mål tooconnect tooSQL Data Warehouse. Det här alternativet kan ge något bättre prestanda än hello ADO NET mål.
3. Använd hello Azure Blob överför toostage hello aktivitetsdata i Azure Blob Storage. Använd sedan hello SSIS kör SQL-uppgiften toolaunch en Polybase-skript som läses in hello data i SQL Data Warehouse. Det här alternativet ger hello bästa prestanda av hello tre alternativ som visas här. tooget hello Azure Blob-överför aktiviteten hämta hello [Microsoft SQL Server 2016 Integration Services Feature Pack för Azure][Microsoft SQL Server 2016 Integration Services Feature Pack for Azure]. toolearn mer om Polybase, se [PolyBase-guiden][PolyBase Guide].

## <a name="before-you-start"></a>Innan du börjar
toostep igenom den här kursen behöver du:

1. **SQL Server Integration Services (SSIS)**. SSIS är en komponent i SQL Server och kräver en utvärderingsversion eller en licensierad version av SQL Server. tooget en utvärderingsversion av SQL Server 2016 Preview finns [SQL Server-utvärderingar][SQL Server Evaluations].
2. **Visual Studio**. tooget Hej kostnadsfri Visual Studio Community Edition, se [Visual Studio Community][Visual Studio Community].
3. **SQL Server Data Tools för Visual Studio (SSDT)**. tooget SQL Server Data Tools för Visual Studio finns [Hämta SQL Server Data Tools (SSDT)][Download SQL Server Data Tools (SSDT)].
4. **Exempeldata**. Den här kursen använder exempeldata som lagras i SQL Server i hello exempeldatabasen AdventureWorks som hello källa data toobe läses in i SQL Data Warehouse. tooget hello exempeldatabasen AdventureWorks finns [AdventureWorks exempeldatabas för 2014][AdventureWorks 2014 Sample Databases].
5. **En SQL Data Warehouse-databas och behörigheter**. Den här självstudiekursen ansluter tooa SQL Data Warehouse-instans och läser in data i den. Du har toohave behörigheter toocreate en tabell och tooload data.
6. **En brandväggsregel**. Du har toocreate en brandväggsregel på SQL Data Warehouse med hello IP-adressen för den lokala datorn innan du kan ladda upp data toohello SQL Data Warehouse.

## <a name="step-1-create-a-new-integration-services-project"></a>Steg 1: Skapa ett nytt Integration Services-projekt
1. Starta Visual Studio.
2. På hello **filen** väljer du **ny | Projektet**.
3. Navigera toohello **installerad | Mallar | Business Intelligence | Integration Services** projekttyper.
4. Välj **Integration Services-projekt**. Ange värden för **namn** och **plats**, och välj sedan **OK**.

Visual Studio öppnas och skapar ett nytt projekt för Integration Services (SSIS). Visual Studio öppnas hello designer för hello enda nya SSIS-paket (Package.dtsx) i hello-projekt. Du ser hello följande områden:

* Hello vänster hello **verktygslådan** SSIS-komponenter.
* Hello mitten hello designytan med flera flikar. Normalt använder du minst hello **Kontrollflöde** och hello **dataflöde** flikar.
* På rätt hello, hello **Solution Explorer** och hello **egenskaper** fönster.
  
    ![][01]

## <a name="step-2-create-hello-basic-data-flow"></a>Steg 2: Skapa hello grundläggande dataflöde
1. Dra en Data flödar aktivitet från hello verktygslådan toohello mittpunkt hello designytan (på hello **Kontrollflöde** fliken).
   
    ![][02]
2. Dubbelklicka på fliken för hello Data flödar aktivitet tooswitch toohello dataflöde.
3. Dra en ADO.NET källa toohello designytan hello andra källor listan i hello verktygslådan. Med hello källa kort fortfarande markerat, ändra namnet för**SQL Server-datakälla** i hello **egenskaper** fönstret.
4. Dra en ADO.NET mål toohello designytan under hello ADO.NET källa hello andra mål listan i hello verktygslådan. Hej målnätverkskort fortfarande markerat, ändra dess namn för**SQL DW mål** i hello **egenskaper** fönstret.
   
    ![][09]

## <a name="step-3-configure-hello-source-adapter"></a>Steg 3: Konfigurera hello källa nätverkskort
1. Dubbelklicka på hello källa kortet tooopen hello **ADO.NET källa Editor**.
   
    ![][03]
2. På hello **Connection Manager** för hello **ADO.NET källa Editor**, klicka på hello **ny** knappen Nästa toohello **ADO.NET Anslutningshanteraren**lista tooopen hello **konfigurera ADO.NET Anslutningshanteraren** dialogrutan och skapa anslutningsinställningar för hello SQL Server-databas som den här kursen läser in data.
   
    ![][04]
3. I hello **konfigurera ADO.NET Anslutningshanteraren** dialogrutan klickar du på hello **ny** knappen tooopen hello **Connection Manager** dialogrutan och skapa en ny anslutning.
   
    ![][05]
4. I hello **Connection Manager** dialogrutan rutan, hello följande saker.
   
   1. För **Provider**, Välj hello SqlClient-dataprovidern.
   2. För **servernamn**, ange hello SQL Server-namn.
   3. I hello **inloggning toohello server** väljer eller ange autentiseringsinformation.
   4. I hello **Anslut tooa databasen** väljer hello exempeldatabasen AdventureWorks.
   5. Klicka på **Anslutningstestet**.
      
       ![][06]
   6. Klicka på dialogrutan för hello som rapporterar hello resultatet av anslutningstestet hello, **OK** tooreturn toohello **Connection Manager** dialogrutan.
   7. I hello **Connection Manager** dialogrutan klickar du på **OK** tooreturn toohello **konfigurera ADO.NET Anslutningshanteraren** dialogrutan.
5. I hello **konfigurera ADO.NET Anslutningshanteraren** dialogrutan klickar du på **OK** tooreturn toohello **ADO.NET källa Editor**.
6. I hello **ADO.NET källa Editor**, i hello **namnet på hello tabell eller vy hello** listan, Välj hello **Sales.SalesOrderDetail** tabell.
   
    ![][07]
7. Klicka på **Preview** toosee hello först 200 rader med data i hello källtabellen i hello **Preview frågeresultat** dialogrutan.
   
    ![][08]
8. I hello **Preview frågeresultat** dialogrutan klickar du på **Stäng** tooreturn toohello **ADO.NET källa Editor**.
9. I hello **ADO.NET källa Editor**, klickar du på **OK** toofinish konfigurera hello-datakällan.

## <a name="step-4-connect-hello-source-adapter-toohello-destination-adapter"></a>Steg 4: Anslut hello källa kortet toohello målnätverkskort
1. Välj hello käll-kortet på hello designytan.
2. Välj hello blå pil som sträcker sig från hello källa nätverkskort och dra toohello mål editor tills den har fästs på plats.
   
    ![][10]
   
    I en typisk SSIS-paket, använda ett antal andra komponenter från hello SSIS verktygslådan between hello källa och hello mål toorestructure, transformering och rensa data som överförs via hello SSIS-dataflöde. tookeep det här exemplet är så enkel som möjligt, vi ansluter hello datakälla direkt toohello mål.

## <a name="step-5-configure-hello-destination-adapter"></a>Steg 5: Konfigurera hello-målnätverkskort
1. Dubbelklicka på hello målet nätverkskort tooopen hello **ADO.NET mål Editor**.
   
    ![][11]
2. På hello **Connection Manager** för hello **ADO.NET mål Editor**, klicka på hello **ny** knappen Nästa toohello **Anslutningshanteraren**lista tooopen hello **konfigurera ADO.NET Anslutningshanteraren** dialogrutan och skapa anslutningsinställningar för hello Azure SQL Data Warehouse-databas som den här kursen läser in data.
3. I hello **konfigurera ADO.NET Anslutningshanteraren** dialogrutan klickar du på hello **ny** knappen tooopen hello **Connection Manager** dialogrutan och skapa en ny anslutning.
4. I hello **Connection Manager** dialogrutan rutan, hello följande saker.
   1. För **Provider**, Välj hello SqlClient-dataprovidern.
   2. För **servernamn**, ange hello SQL Data Warehouse namn.
   3. I hello **inloggning toohello server** väljer **används SQL Server-autentisering** och ange autentiseringsinformation.
   4. I hello **Anslut tooa databasen** väljer du en befintlig SQL Data Warehouse-databas.
   5. Klicka på **Anslutningstestet**.
   6. Klicka på dialogrutan för hello som rapporterar hello resultatet av anslutningstestet hello, **OK** tooreturn toohello **Connection Manager** dialogrutan.
   7. I hello **Connection Manager** dialogrutan klickar du på **OK** tooreturn toohello **konfigurera ADO.NET Anslutningshanteraren** dialogrutan.
5. I hello **konfigurera ADO.NET Anslutningshanteraren** dialogrutan klickar du på **OK** tooreturn toohello **ADO.NET mål Editor**.
6. I hello **ADO.NET mål Editor**, klickar du på **ny** nästa toohello **använder en tabell eller vy** lista tooopen hello **Create Table** dialogrutan toocreate en ny måltabell med en kolumnlista som matchar hello källtabellen.
   
    ![][12a]
7. I hello **Create Table** dialogrutan rutan, hello följande saker.
   
   1. Ändra hello hello måltabellen för**SalesOrderDetail**.
   2. Ta bort hello **rowguid** kolumn. Hej **uniqueidentifier** datatyp stöds inte i SQL Data Warehouse.
   3. Ändra hello datatyp hello **LineTotal** kolumn för**pengar**. Hej **decimal** datatyp stöds inte i SQL Data Warehouse. Information om vilka datatyper finns [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)].
      
       ![][12b]
   4. Klicka på **OK** toocreate hello tabellen och returnerar toohello **ADO.NET mål Editor**.
8. I hello **ADO.NET mål Editor**väljer hello **mappningar** fliken toosee hur kolumner i hello källan mappas toocolumns i hello målet.
   
    ![][13]
9. Klicka på **OK** toofinish konfigurera hello-datakällan.

## <a name="step-6-run-hello-package-tooload-hello-data"></a>Steg 6: Kör hello paketdata tooload hello
Kör hello paketet genom att klicka på hello **starta** knappen hello verktygsfältet eller välja ett hello **kör** alternativ på hello **felsöka** menyn.

Hello paketet börjar toorun, se gul snurrande hjul tooindicate som hello antalet bearbetade hittills rader.

![][14]

När hello paketet har körts du finns grön bockmarkering tooindicate lyckade samt hello Totalt antal rader med data som har lästs in från hello källa toohello mål.

![][15]

Grattis! Du har använt SQL Server Integration Services tooload data till Azure SQL Data Warehouse.

## <a name="next-steps"></a>Nästa steg
* Läs mer om hello SSIS-dataflöde. Börja här: [dataflöde][Data Flow].
* Lär dig hur toodebug och felsöka dina paket direkt i hello design-miljön. Börja här: [felsökningsverktyg för paketet utveckling][Troubleshooting Tools for Package Development].
* Lär dig hur toodeploy din SSIS projekt och paketerar tooIntegration Services-Server eller tooanother lagringsplats. Börja här: [projekt för distribution av och paket][Deployment of Projects and Packages].

<!-- Image references -->
[01]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-designer-01.png
[02]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-data-flow-task-02.png
[03]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-03.png
[04]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-manager-04.png
[05]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-05.png
[06]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/test-connection-06.png
[07]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-07.png
[08]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/preview-data-08.png
[09]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/source-destination-09.png
[10]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/connect-source-destination-10.png
[11]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-destination-11.png
[12a]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-before-12a.png
[12b]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-after-12b.png
[13]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/column-mapping-13.png
[14]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-running-14.png
[15]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[PolyBase Guide]: https://msdn.microsoft.com/library/mt143171.aspx
[Download SQL Server Data Tools (SSDT)]: https://msdn.microsoft.com/library/mt204009.aspx
[CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)]: https://msdn.microsoft.com/library/mt203953.aspx
[Data Flow]: https://msdn.microsoft.com/library/ms140080.aspx
[Troubleshooting Tools for Package Development]: https://msdn.microsoft.com/library/ms137625.aspx
[Deployment of Projects and Packages]: https://msdn.microsoft.com/library/hh213290.aspx

<!--Other Web references-->
[Microsoft SQL Server 2016 Integration Services Feature Pack for Azure]: http://go.microsoft.com/fwlink/?LinkID=626967
[SQL Server Evaluations]: https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016
[Visual Studio Community]: https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx
[AdventureWorks 2014 Sample Databases]: https://msftdbprodsamples.codeplex.com/releases/view/125550
