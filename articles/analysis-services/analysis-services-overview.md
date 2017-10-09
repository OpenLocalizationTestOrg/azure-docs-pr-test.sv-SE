---
title: "aaaWhat är Azure Analysis Services | Microsoft Docs"
description: "Hämta hello översikt av Analysis Services i Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 83d7a29c-57ae-4aa0-8327-72dd8f00247d
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/01/2017
ms.author: owend
ms.openlocfilehash: 48830a86f47a8ddc7770e6c44dd56c29927fe582
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-analysis-services"></a>Vad är Azure Analysis Services?
![Azure Analysis Services](./media/analysis-services-overview/aas-overview-aas-icon.png)

Azure Analysis Services innehåller företagsklass datamodeller i hello molnet. Det här är en fullständigt hanterad plattform som en tjänst (PaaS), integrerad med Azure-dataplattformstjänsterna. 

Med Analysis Services kan du blanda och kombinera data från flera källor, definiera mätvärden och skydda dina data i en enda tillförlitlig semantisk datamodell. hello datamodellen innehåller ett enklare och snabbare sätt för dina användare toobrowse enorma mängder data med klientprogram som Power BI, Excel, Reporting Services, appar från tredje part och anpassade.

![Datakällor](./media/analysis-services-overview/aas-overview-data-sources.png)

Checka ut [den här videon](https://sec.ch9.ms/ch9/d6dd/a1cda46b-ef03-4cea-8f11-68da23c5d6dd/AzureASoverview_high.mp4) toolearn hur Azure Analysis Services passa Microsoft datorns övergripande BI-funktionerna och hur du kan dra nytta av komma din datamodeller i hello moln.

## <a name="built-on-sql-server-analysis-services"></a>Bygger på SQL Server Analysis Services
Azure Analysis Services är kompatibelt med många fantastiska funktioner som redan finns i SQL Server Analysis Services Enterprise. Azure Analysis Services har stöd för tabellmodeller på hello 1200 och 1400 [kompatibilitetsnivåer](analysis-services-compat-level.md). Partitioner, säkerhet på radnivå, dubbelriktade relationer och översättningar stöds. Minnes- och DirectQuery-lägena innebär att du blixtsnabbt kan skicka frågor via omfattande och komplexa datauppsättningar.

Tabellmodeller ger snabb utveckling och är mycket anpassningsbara. För utvecklare innehåller tabellmodeller hello Tabular Object Model (TOM) toodescribe modellobjekt. TOM exponeras i JSON via hello [Tabular modellen Scripting språk (TMSL)](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference) och hello AMO (data definition language) via hello [Microsoft.AnalysisServices.Tabular](https://msdn.microsoft.com/library/microsoft.analysisservices.tabular.aspx) namnområde.

## <a name="better-with-azure"></a>Bättre med Azure
Azure Analysis Services kan integreras med många Azure-tjänster som gör att du toobuild sofistikerade Analyslösningar. Integrering med [Azure Active Directory](../active-directory/active-directory-whatis.md) ger säker och rollbaserad åtkomst tooyour viktiga data. Integrera med [Azure Data Factory](../data-factory/data-factory-introduction.md) pipelines genom att inkludera en aktivitet som läser in data i hello modellen. [Azure Automation](../automation/automation-intro.md) och [Azure Functions](../azure-functions/functions-overview.md) kan användas för att utföra enkel orkestrering av modeller med anpassad kod.

## <a name="get-up-and-running-quickly"></a>Kom igång snabbt
På Azure Portal kan du [skapa en server](analysis-services-create-server.md) på några minuter. Och du kan etablera servrar med hjälp av en deklarativ mall med Azure Resource Manager-[mallar](../azure-resource-manager/resource-manager-create-first-template.md) och PowerShell. Du kan distribuera flera tjänster tillsammans med andra Azure-komponenter, till exempel lagringskonton och Azure Functions, med en enda mall. 

När du har skapat en server kan skapa du en tabellmodell direkt i Azure Portal. Med nya hello (förhandsgranskning) [Web designer funktionen](analysis-services-create-model-portal.md), du kan ansluta tooan Azure SQL Database, Azure SQL Data Warehouse-datakälla eller importera en Power BI Desktop .PBX-fil. Relationer mellan tabeller skapas automatiskt och du kan skapa mått eller redigera hello model.bim i json-format rätt från din webbläsare.

## <a name="scale-tooyour-needs"></a>Skala tooyour behov
Azure Analysis Services är tillgängligt på nivåerna Developer, Basic och Standard. Inom varje nivå variera planerade kostnader bl.a tooprocessing ström, QPUs och minne storlek. När du skapar en server kan välja du en plan inom en nivå. Du kan ändra planer eller ned inom hello samma nivå eller uppgradera tooa högre nivå, men det går inte att nedgradera från en högre nivå tooa lägre nivå.

Skala upp, ned eller pausa din server. Använd hello Azure-portalen eller har fullständig kontroll på direkt med hjälp av PowerShell. Betala endast för det du använder. toolearn mer om hello olika planer och nivåer och Använd hello Kalkylatorn toodetermine hello rätt prisavtal för dig, se [priser för Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="keep-your-data-close"></a>Förvara dina data nära
Azure Analysis Services-servrar kan skapas i hello följande [Azure-regioner](https://azure.microsoft.com/regions/):

| Nord- och Sydamerika | Europa | Asien och stillahavsområdet |
|----------|--------|--------------|
|  Södra Brasilien<br> Centrala Kanada<br> Östra USA 2<br> Norra centrala USA<br> Södra centrala USA<br> Västra centrala USA<br> Västra USA | Norra Europa<br> Storbritannien, södra<br> Västra Europa |   Sydöstra Australien<br> Östra Japan<br> Sydostasien<br> Indien, västra  |

Nya områden läggs alla hello tid så att den här listan kan vara ofullständiga. Du kan välja en plats när du skapar en server i Azure Portal eller med hjälp av Azure Resource Manager-mallar. tooget hello bästa prestanda, Välj en plats närmast största användarbasen. Garantera [hög tillgänglighet](analysis-services-bcdr.md) genom att distribuera modellerna på redundanta servrar i flera områden.

## <a name="migrate-your-existing-tabular-models"></a>Migrera dina befintliga tabellmodeller
Om du redan har befintliga lokala SQL Server Analysis Services-modell lösningar kan du migrera tooAzure Analysis Services utan betydande förändringar. toomigrate, kan du använda SSDT toodeploy modellen tooyour servern. I SSMS kan du även använda säkerhetskopiering och återställning eller TMSL.

Om du har lokala datakällor kan du behöver tooinstall och konfigurera en [lokala datagateway](analysis-services-gateway.md). Om du har roller och medlemmar i rollen redan konfigurerats migrera dina roller, men du har tooreadd rollmedlemmar med hjälp av SSMS eller PowerShell.

## <a name="connect-toopopular-data-sources"></a>Anslut toopopular datakällor
Azure Analysis Services stöder [ansluta toodata källor](analysis-services-datasource.md) lokalt i din organisation och i hello molnet. Du kan kombinera data från datakällor som finns både lokalt och i molnet. På så sätt får du en hybridlösning. 

Nya tabellmodeller 1400 funktionen hello moderna hämta Data i SSDT, baserat på hello M formeln frågespråk. Med hämta Data du har flera Dataomvandling av och funktioner i mashup hello möjlighet toocreate och redigera egna avancerade M formeln språk frågor. Med 1400-tabellmodeller kan du till exempel modellera på datafiler i Azure Blob Storage.

## <a name="use-hello-tools-you-already-know"></a>Använd hello verktyg som du redan känner till

![BI-utvecklarverktyg](./media/analysis-services-overview/aas-overview-dev-tools.png)

#### <a name="sql-server-data-tools-ssdt-for-visual-studio"></a>SQL Server Data Tools (SSDT) för Visual Studio
Utveckla och distribuera modeller med hello ledigt [SQL Server Data Tools (SSDT) för Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx). SSDT innehåller Analysis Services-projektmallar som du kommer igång med snabbare. SSDT nu innehåller hello moderna hämta Data datasource fråge- och mashup funktioner för 1400 tabellmodeller. Om du är bekant med hämta Data i Power BI Desktop- och Excel 2016 vet du redan hur lätt det är toocreate hög anpassade källan för datafrågor.

#### <a name="sql-server-management-studio"></a>SQL Server Management Studio
Hantera dina servrar och modelldatabaser med hjälp av [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). Ansluta tooyour servrar i hello molnet. Köra TMSL skript direkt från hello XMLA frågefönstret och automatisera uppgifter med hjälp av TMSL skript. Nya funktioner och funktionaliteter införs snabbt och SSMS uppdateras varje månad.

#### <a name="powershell"></a>PowerShell
Hanteringsaktiviteter för server-resurs som skapa servrar, pausa eller återuppta åtgärder eller ändra hello-servicenivå (nivån) använda cmdlets för Azure Resource Manager (AzureRM). Andra uppgifter för hantering av databaser, till exempel att lägga till eller ta bort medlemmar i rollen bearbetning eller skriptkörning TMSL använda cmdlets i hello SqlServer modul. Både AzureRM och SQLServer-moduler är tillgängliga i hello [PowerShell-galleriet](https://www.powershellgallery.com/).


## <a name="your-data-is-secure"></a>Dina data är skyddade
![Datavisualiseringar](./media/analysis-services-overview/aas-overview-secure.png)

#### <a name="authentication"></a>Autentisering
Användarautentisering för Azure Analysis Services hanteras av [Azure Active Directory (AAD)](../active-directory/active-directory-whatis.md). När du försöker toolog i tooan Azure Analysis Services-databasen, användare, Använd en organisation kontoidentitet med toohello access-databas de försöker tooaccess. Dessa användaridentiteter måste vara medlemmar i standard hello Azure Active Directory för hello prenumeration där hello Azure Analysis Services-servern finns. Det finns fler toolearn [autentisering och användarbehörigheter](analysis-services-manage-users.md).

#### <a name="data-security"></a>Datasäkerhet
Azure Analysis Services använder Azure Blob storage toopersist storage och metadata för Analysis Services-databaser. Datafiler i Blob krypteras med Azure Blob Server Side Encryption (SSE). När du använder läget Direct Query lagras endast metadata. hello faktiska data hämtas från hello-datakällan när databasfrågan.

#### <a name="on-premises-data-sources"></a>Lokala datakällor
Säker åtkomst toodata som finns lokalt i din organisation uppnås genom att installera och konfigurera en [lokala datagateway](analysis-services-gateway.md). Gateways ge åtkomst toodata för både direktfrågan och InMemory-läge. När en modell för Azure Analysis Services ansluter tooan lokala datakällan är skapas en fråga tillsammans med hello krypterade autentiseringsuppgifter för datakälla för hello lokalt. hello gateway-Molntjänsten analyserar hello frågan och skickar hello begäran tooan Azure Service Bus. hello lokala gateway avsöker hello Azure Service Bus för väntande begäranden. hello gateway sedan hämtar hello frågan dekrypterar hello autentiseringsuppgifter och ansluter toohello datakälla för körning. hello resultaten sedan skickas från hello datakälla tillbaka toohello gateway och sedan på toohello Azure Analysis Services-databas.

Azure Analysis Services styrs av hello [licensvillkor för Microsoft Online Services](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) och hello [sekretesspolicy för Microsoft Online Services](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx).
toolearn mer om Azure-säkerhet finns hello [Microsoft Trust Center](https://www.microsoft.com/trustcenter/Security/AzureSecurity).

## <a name="supports-hello-latest-client-tools"></a>Stöder hello senaste klientverktyg
![Datavisualiseringar](./media/analysis-services-overview/aas-overview-clients.png)

Moderna datautforsknings- och visualiseringsverktyg som Power BI, Excel och verktyg från tredje part ger användarna interaktiva och högvisuella insikter om dina modelldata.

Klienter använder MSOLAP, AMO eller ADOMD [klientbiblioteken](analysis-services-data-providers.md) tooconnect tooAnalysis Services-servrar. Microsoft-klientprogram som Power BI Desktop och Excel installerar samtliga tre klientbibliotek. Men kom ihåg, beroende på hello version eller ofta uppdateras kanske inte klientbibliotek hello senaste versionerna som krävs av Azure Analysis Services. hello gäller samma toocustom program eller andra gränssnitt som AsCmd, TOM, ADOMD.NET. Dessa program kräver vanligtvis manuellt installera hello bibliotek som en del av ett paket.


## <a name="get-help"></a>Få hjälp

#### <a name="documentation"></a>Dokumentation
Azure Analysis Services är enkla tooset upp och toomanage. Du hittar alla hello information du behöver toocreate och hantera din servertjänster. När du skapar en modell toodeploy tooyour datalagring, har den mycket hello samma som för att skapa en datamodell som du distribuerar tooan lokal server. Det finns ett omfattande bibliotek med idéskisser, procedurer, självstudiekurser och referensartiklar på [Analysis Services](https://docs.microsoft.com/sql/analysis-services/analysis-services).

#### <a name="videos"></a>Videoklipp
Kolla in de användbara videoklippen på [Azure Analysis Services på Channel 9](https://channel9.msdn.com/series/Azure-Analysis-Services).

#### <a name="blogs"></a>Bloggar
Saker och ting ändras snabbt. Du får alltid hello senaste informationen om hello [Analysis Services-teambloggen](https://blogs.msdn.microsoft.com/analysisservices/) och [Azure blogg](https://azure.microsoft.com/blog/).

#### <a name="community"></a>Community
Analysis Services har ett levande användarforum. Hello konversationen på [Azure Analysis Services-forumet](https://aka.ms/azureanalysisservicesforum).

## <a name="feedback"></a>Feedback
Har du förslag eller saknar du någon funktion? Vara säker på att tooleave kommentarer på [Azure Analysis Services Feedback](https://aka.ms/azureanalysisservicesfeedback).

Har förslag om hello-dokumentation? Du kan lägga till kommentarer med Livefyre längst ned hello i varje artikel.

## <a name="next-steps"></a>Nästa steg
Nu när du vet mer om Azure Analysis Services är det dags tooget igång. Lär dig hur för[skapar du en server](analysis-services-create-server.md) i Azure. När servern är klar, stega igenom hello [Adventure Works kursen](tutorials/aas-adventure-works-tutorial.md) toolearn hur toocreate en fullt fungerande tabellmodell och distribuera den tooyour server.
