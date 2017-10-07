---
title: "aaa ”Azure Analysis Services Adventure Works kursen | Microsoft Docs ”"
description: "Introducerar hello Adventure Works självstudiekurs för Azure Analysis Services"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 06/01/2017
ms.author: owend
ms.openlocfilehash: 2df8b3ab4e8c4ffbe0086418d60fd2e2abd35e7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-analysis-services---adventure-works-tutorial"></a>Azure Analysis Services – Självstudiekurs för Adventure Works

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Den här självstudiekursen innehåller erfarenheter toocreate och distribuera en tabellmodell på hello 1400 kompatibilitetsnivå med hjälp av [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).  

Om du är den nya tooAnalysis tjänster och tabular modellering kan den här kursen är hello snabbaste sättet toolearn hur toocreate och distribuera en grundläggande tabellmodell. När du har hello krav på plats bör ta mellan två toothree timmar toocomplete.  
  
## <a name="what-you-learn"></a>Detta får du får lära dig   
  
-   Hur toocreate en ny tabellmodell projektet på hello **1400 kompatibilitetsnivå** i SSDT.
  
-   Hur tooimport data från en relationsdatabas i en tabellmodell-projekt.  
  
-   Hur toocreate och hantera relationer mellan tabeller i hello modellen.  
  
-   Hur beräknas toocreate kolumner, mått och prestationsindikatorer som hjälper användarna att analysera viktiga affärsmått.  
  
-   Hur toocreate och hantera perspektiv och hierarkier som hjälper användarna att enkelt bläddra modelldata genom att tillhandahålla business och programspecifika synpunkter.  
  
-   Hur toocreate partitioner som delar upp tabelldata i mindre logiska delar som kan bearbetas oberoende av andra partitioner.  
  
-   Hur modellen toosecure objekt och data genom att skapa roller med användaren medlemmar.  
  
-   Hur toodeploy en tabellmodell tooan **Azure Analysis Services** servern eller en lokal SQL Server 2017 Analysis Services-server.  
  
## <a name="prerequisites"></a>Krav  
toocomplete den här kursen behöver du:  
  
-   En Azure Analysis Services eller SQL Server 2017 Analysis Services-instansen toodeploy din modell till. Registrera dig för en kostnadsfri [utvärderingsversion av Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) och [skapa en server](../analysis-services-create-server.md). Eller registrera dig och ladda ned [SQL Server 2017 Community Technology Preview](https://www.microsoft.com/evalcenter/evaluate-sql-server-vnext-ctp). 

-   Ett informationslager för SQL Server eller Azure SQL Data Warehouse med hello [AdventureWorksDW2014 exempeldatabasen](http://go.microsoft.com/fwlink/?LinkID=335807). Den här exempeldatabasen innehåller hello data nödvändiga toocomplete den här kursen. Ladda ned [kostnadsfria versioner av SQL Server Data Tools](https://www.microsoft.com/sql-server/sql-server-downloads). Eller registrera dig för en kostnadsfri [Azure SQL Database-utvärderingsversion](https://azure.microsoft.com/services/sql-database/). 

    **Viktigt:** om du installerar hello exempeldatabasen på en lokal SQL Server och du distribuerar din modell tooan Azure Analysis Services-server, en [lokala datagateway](../analysis-services-gateway.md) krävs.

-   hello senaste versionen av [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx).

-   hello senaste versionen av [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).    

-   Ett klientprogram som till exempel [Power BI Desktop](https://powerbi.microsoft.com/desktop/) eller Excel. 

## <a name="scenario"></a>Scenario  
Den här självstudien är baserad på Adventure Works Cycles, ett fiktivt företag. Adventure Works är en stor, flerspråkig tillverkningsföretag som producerar och distribuerar cyklar, delar och tillbehör toocommercial marknader i Nordamerika, Europa och Asien. hello företaget använder 500 anställda. Dessutom har Adventure Works flera regionala försäljningsteam inom sitt marknadsområde. Projektet är toocreate en tabellmodell för försäljning och marknadsföring användare tooanalyze Internet försäljningsinformation i hello AdventureWorksDW-databasen.  
  
Du måste slutföra olika erfarenheter toocomplete hello kursen. Varje lektion innehåller olika uppgifter. Varje uppgift i ordning är nödvändigt för att slutföra hello lektionen. En viss lektion kan innehålla flera uppgifter som leder till samma resultat, men tillvägagångssättet för att slutföra uppgifterna skiljer sig åt. Den här metoden visar är det ofta mer än ett sätt toocomplete en aktivitet och toochallenge du med hjälp av kunskaper som du har lärt dig i tidigare erfarenheter och uppgifter.  
  
hello syftar hello erfarenheter tooguide dig genom att redigera en grundläggande tabellmodell genom att använda många av funktionerna hello ingår i SSDT. Eftersom varje lektionen bygger på hello föregående lektionen, bör du genomföra hello erfarenheter i ordning.
  
Den här kursen ger inte några erfarenheter om att hantera en server i Azure-portalen, hantera en server eller databas med hjälp av SSMS eller med hjälp av en klient programdata toobrowse modell. 


## <a name="lessons"></a>Lektioner  
Vägledningen innehåller följande erfarenheter hello:  
  
|Lektion|Beräknad tid toocomplete|  
|----------|------------------------------|  
|[1 - Skapa ett nytt projekt för tabellmodeller](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md)|10 minuter|  
|[2 - Hämta data](../tutorials/aas-lesson-2-get-data.md)|10 minuter|  
|[3 - Markera som datumtabell](../tutorials/aas-lesson-3-mark-as-date-table.md)|3 minuter|  
|[4 - Skapa relationer](../tutorials/aas-lesson-4-create-relationships.md)|10 minuter|  
|[5 - Skapa beräknade kolumner](../tutorials/aas-lesson-5-create-calculated-columns.md)|15 minuter|
|[6 - Skapa mått](../tutorials/aas-lesson-6-create-measures.md)|30 minuter|  
|[7 - Skapa KPI:er](../tutorials/aas-lesson-7-create-key-performance-indicators.md)|15 minuter|  
|[8 - Skapa perspektiv](../tutorials/aas-lesson-8-create-perspectives.md)|5 minuter|  
|[9 - Skapa hierarkier](../tutorials/aas-lesson-9-create-hierarchies.md)|20 minuter|  
|[10 - Skapa partitioner](../tutorials/aas-lesson-10-create-partitions.md)|15 minuter|  
|[11 - Skapa roller](../tutorials/aas-lesson-11-create-roles.md)|15 minuter|  
|[12 - Analysera i Excel](../tutorials/aas-lesson-12-analyze-in-excel.md)|5 minuter| 
|[13 - Distribuera](../tutorials/aas-lesson-13-deploy.md)|5 minuter|  
  
## <a name="supplemental-lessons"></a>Kompletterande lektioner  
Dessa erfarenheter är inte obligatoriska toocomplete hello kursen, men kan vara användbart i bättre förstå avancerade tabellmodell funktioner för redigering.  
  
|Lektion|Beräknad tid toocomplete|  
|----------|------------------------------|  
|[Detaljrader](../tutorials/aas-supplemental-lesson-detail-rows.md)|10 minuter|
|[Dynamisk säkerhet](../tutorials/aas-supplemental-lesson-dynamic-security.md)|30 minuter|
|[Ojämna hierarkier](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)|20 minuter| 

  
## <a name="next-steps"></a>Nästa steg  
tooget igång, se [lektionen 1: skapa en ny modell Tabellprojekt](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).  
  
  
  

