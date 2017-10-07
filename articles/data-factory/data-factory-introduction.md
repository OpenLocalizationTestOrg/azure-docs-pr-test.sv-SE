---
title: "aaaIntroduction tooData fabriken, en tjänst för integrering av data | Microsoft Docs"
description: "Detta är vad Azure Data Factory är: en molnbaserad dataintegreringstjänst som samordnar och automatiserar förflyttning och transformering av data."
keywords: dataintegrering, molnbaserad dataintegrering, azure data factory
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: cec68cb5-ca0d-473b-8ae8-35de949a009e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: 4cc30515315efc938951057743ff8eb3701214ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-data-factory"></a>Introduktion tooAzure Data Factory 
## <a name="what-is-azure-data-factory"></a>Vad är Azure Data Factory?
I hälsningsmeddelande för stordata, hur befintliga data utnyttjas i företag? Är det möjligt tooenrich data som genereras i hello molnet med hjälp av referensdata från lokala datakällor eller andra olika datakällor? Till exempel ett spel företag samlar in många loggar som genereras av spel i hello molnet. Tooanalyze vill dessa loggar toogain insikter i toocustomer inställningar, demografi, användning beteende etc. tooidentify-upp säljer och mellan säljer affärsmöjligheter, utveckla nya intressant funktioner toodrive tillväxt och ge en bättre upplevelse toocustomers. 

tooanalyze dessa loggar hello företaget måste toouse hello referensdata, till exempel kundinformation, spel information marknadsföring kampanjinformation som är i ett lokalt datalager. Därför hello företaget vill ha tooingest loggdata från hello molnarkivet för data och referensdata från hello lokala datalager. Bearbeta hello data med Hadoop i hello molnet (Azure HDInsight) och sedan publicera hello resultatet i ett informationslager i molnet, till exempel Azure SQL Data Warehouse eller en lokala data datalager som SQL Server. Det vill det här arbetsflödet toorun varje vecka en gång. 

Vad som behövs är en plattform som gör att hello företagets toocreate ett arbetsflöde som kan mata in data från datalager för molnet och lokalt och omvandlings- eller data med hjälp av befintliga beräknings-tjänster, till exempel Hadoop och publicera hello resultat tooan lokalt eller moln datalager för BI program tooconsume. 

![Data Factory-översikt](media/data-factory-introduction/what-is-azure-data-factory.png) 

Azure Data Factory är hello plattform för den här typen av scenarier. Det är en **molnbaserade data integration service som gör att du toocreate datadrivna arbetsflöden i hello molnet för att samordna och automatisera dataflytt och Dataomvandling**. Med Azure Data Factory kan du skapa och schemalägga datadrivna arbetsflöden (kallas pipelines) som kan mata in data från olika datalager processen/transformering hello data med hjälp av beräkning tjänster, till exempel Azure HDInsight Hadoop, Spark, Azure Data Lake Analyser och Azure Machine Learning och publicera data toodata lagras till exempel Azure SQL Data Warehouse för business intelligence (BI) program tooconsume utdata.  

Det är snarare som en EL-plattform (Extract-and-Load) och sedan en TL-plattform (Transform-and-Load) än en traditionell ETL-plattform (Extract-Transform-and-Load). hello transformationer som utförs är tootransform/bearbetning av data med hjälp av beräkning services i stället för att tooperform transformationer som hello dem för att lägga till härledda kolumner, räkna antal rader, sortera data osv. 

Är för närvarande i Azure Data Factory hello data som används och produceras av arbetsflöden **tid-segmenterade data** (varje timme, varje dag, varje vecka, etc.). En pipeline kan till exempel läsa indata, bearbeta data och skapa utdata en gång om dagen. Du kan också köra ett arbetsflöde bara en gång.  
  

## <a name="how-does-it-work"></a>Hur fungerar det? 
hello pipelines (datadrivna arbetsflöden) i Azure Data Factory brukar utföra hello följande tre steg:

![Tre nivåer i Azure Data Factory](media/data-factory-introduction/three-information-production-stages.png)

### <a name="connect-and-collect"></a>Ansluta och samla in
Företag har olika typer av data lagrade i olika källor. hello första steget i att skapa ett informationssystem produktion är tooconnect tooall hello krävs datakällor och bearbetning, till exempel SaaS-tjänster filen resurser, FTP, webbtjänster och flytta hello data som behövs tooa centraliserad plats för efterföljande bearbeta.

Utan Data Factory företag skapa anpassade data movement komponenter eller skriva anpassade tjänster toointegrate dessa datakällor och bearbetning. Det är dyrt och svåra toointegrate och underhålla sådana system och det saknar ofta hello enterprise klass övervakning och avisering och hello-kontroller som kan erbjuda för en helt hanterad tjänst.

Du kan använda hello Kopieringsaktiviteten i data pipeline toomove data från både lokalt och molnet källa data lagras tooa centralisering datalager i hello molnet för ytterligare analys med Data Factory. Du kan till exempel samla data i ett Azure Data Lake Store och transformeringen hello data senare med hjälp av en tjänst för beräkning av Azure Data Lake Analytics. Eller så kan du samla data i en Azure Blob Storage och sedan omvandla data med ett Azure HDInsight Hadoop-kluster.

### <a name="transform-and-enrich"></a>Omvandla och berika
När data finns i ett centraliserad datalager i hello molnet, vill hello insamlade data toobe bearbetas eller omvandlas med hjälp av beräknings-tjänster, till exempel HDInsight Hadoop, Spark, Data Lake Analytics och Maskininlärning. Vill du tooreliably producerar omvandlas data på en hanterbar och kontrollerat schema toofeed produktionsmiljöer med betrodda data. 

### <a name="publish"></a>Publicera 
Leverera transformerade data från molnet hello tooon lokala källor som SQL Server eller behålla det i ditt moln lagring källor för förbrukning av business intelligence (BI) och verktyg för analys och andra program.

## <a name="key-components"></a>Nyckelkomponenter
En Azure-prenumeration kan ha en eller flera Azure Data Factory-instanser (eller datafabriker). Azure Data Factory består av fyra viktiga komponenter som arbetar tillsammans för tooprovide hello plattform som du kan skapa datadrivna arbetsflöden med steg toomove och transformera data. 

### <a name="pipeline"></a>Pipeline
En datafabrik kan ha en eller flera pipelines. En pipeline är en grupp av aktiviteter. Tillsammans utföra hello aktiviteter i en pipeline en uppgift. En pipeline kan exempelvis innehålla en grupp av aktiviteter som en data från en Azure blob och sedan köra en Hive-fråga på ett HDInsight-kluster toopartition hello data. hello fördelen med detta är att hello-pipeline kan du toomanage hello aktiviteter som en uppsättning i stället för var och en individuellt. Du kan distribuera och schemalägga hello pipeline, i stället för hello aktiviteter oberoende av varandra. 

### <a name="activity"></a>Aktivitet
En pipeline kan innehålla en eller flera aktiviteter. Aktiviteter definiera hello åtgärder tooperform på dina data. Du kan till exempel använda en kopia av toocopy aktivitetsdata från en butik tooanother data datalagring. På liknande sätt kan du använda en Hive-aktivitet som körs en Hive-fråga på en Azure HDInsight-kluster tootransform eller analysera dina data. Data Factory stöder två typer av aktiviteter: aktiviteter för dataförflyttning och datatransformering.

### <a name="data-movement-activities"></a>Dataförflyttningsaktiviteter
Kopieringsaktiviteten i Data Factory kopierar data från en källa data store tooa sink datalagret. Data Factory stöder hello följande datalager. Data från en källa kan skrivas tooany mottagare. Klicka på en data store toolearn hur toocopy data tooand från detta Arkiv.

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Mer information finns i artikeln [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md).

### <a name="data-transformation-activities"></a>Datatransformeringsaktiviteter
[!INCLUDE [data-factory-transformation-activities](../../includes/data-factory-transformation-activities.md)]

Mer information finns i artikeln [Datatransformeringsaktiviteter](data-factory-data-transformation-activities.md).

### <a name="custom-net-activities"></a>Anpassa .NET-aktiviteter
Om du behöver toomove data till/från ett dataarkiv som Kopieringsaktiviteten inte stöder, eller Transformera data med egna logik, skapa en **anpassad .NET-aktivitet**. Mer information om att skapa och använda en anpassad aktivitet finns i [Use custom activities in an Azure Data Factory pipeline (Använda anpassade aktiviteter i en Azure Data Factory-pipeline)](data-factory-use-custom-activities.md).

### <a name="datasets"></a>Datauppsättningar
En aktivitet har noll eller flera datauppsättningar som indata och en eller flera datauppsättningar som utdata. Datauppsättningar representerar datastrukturer inom hello datalager som helt enkelt peka eller refererade hello data som du vill toouse i dina aktiviteter som indata eller utdata. Azure-blobbdatauppsättning anger hello blob-behållaren och mappen i hello Azure Blob Storage från vilka hello pipeline bör du läsa hello data. Eller en datauppsättning för Azure SQL-tabellen anger hello tabell toowhich hello utdata skrivs av hello-aktivitet. 

### <a name="linked-services"></a>Länkade tjänster
Länkade tjänster liknar anslutningssträngar som definierar hello anslutningsinformation som behövs för Data Factory tooconnect tooexternal resurser. Sägas sätt – en länkad tjänst definierar hello anslutning toohello datakälla och en datauppsättning representerar hello datastrukturen hello. Till exempel anger en länkad Azure Storage-tjänst connection string tooconnect toohello Azure Storage-konto. Och Azure-blobbdatauppsättning anger hello blob-behållaren och hello-mapp som innehåller hello data.   

Länkade tjänster används för två syften i Data Factory:

* toorepresent en **datalagret** inklusive, men inte begränsat till en lokal SQL Server, Oracle-databas, filresurs eller ett Azure Blob Storage-konto. Se hello [Data movement aktiviteter](#data-movement-activities) avsnittet för en lista av stöds datalager.
* toorepresent en **resurser** som kan vara värd för hello körningen av en aktivitet. Till exempel körs hello HDInsightHive aktiviteten på ett HDInsight Hadoop-kluster. Se avsnittet [Datatransformeringsaktiviteter](#data-transformation-activities), för en lista över compute-miljöer som stöds.

### <a name="relationship-between-data-factory-entities"></a>Förhållande mellan Data Factory-enheter
![Diagram: Data Factory, en molnbaserad dataintegreringstjänst – nyckelbegrepp](./media/data-factory-introduction/data-integration-service-key-concepts.png)
**Figur 2.** Relationer mellan datauppsättning, aktivitet, pipeline och länkad tjänst

## <a name="supported-regions"></a>Regioner som stöds
För närvarande kan du skapa datafabriker i hello **västra USA**, **östra USA**, och **Nordeuropa** regioner. Dock en datafabrik kan komma åt datalager och beräkna tjänster i andra Azure-regioner toomove data mellan datalager eller bearbeta data med hjälp av compute-tjänster.

Azure Data Factory lagrar inte själv några data. Gör det möjligt att skapa datadrivna arbetsflöden tooorchestrate flytt av data mellan [stöds datalager](#data-movement-activities) och bearbetning av data med hjälp av [compute services](#data-transformation-activities) i andra regioner eller i ett lokalt miljö. Du kan också för[övervaka och hantera arbetsflöden](data-factory-monitor-manage-pipelines.md) använder både programmässiga och UI-metoder.

Även om Data Factory finns bara på **västra USA**, **östra USA**, och **Nordeuropa** regioner, hello-tjänsten startar hello dataflyttning i Data Factory är tillgänglig [globalt](data-factory-data-movement-activities.md#global) i flera regioner. Om ett dataarkiv som finns bakom en brandvägg och sedan en [Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) installerats i din lokala miljö flyttar hello data i stället.

Exempelvis kan vi anta att dina beräkningsmiljöer, som t.ex. Azure HDInsight-kluster och Azure Machine Learning, körs utanför Västeuropas region. Du kan skapa och använda en Azure Data Factory-instans i Norra Europa och använda den tooschedule jobb på dina beräknings-miljöer i västra Europa. Det tar några tid i millisekunder för Data Factory tootrigger hello jobb på beräkning miljön men hello tid för att köra jobbet hello på datormiljön ändras inte.

## <a name="get-started-with-creating-a-pipeline"></a>Kom igång med att skapa en pipeline
Du kan använda något av dessa verktyg eller API: er toocreate data pipelines i Azure Data Factory: 

- Azure Portal
- Visual Studio
- PowerShell
- .NET-API
- REST API
- Azure Resource Manager-mall. 

toolearn hur rörledningar toobuild datafabriker med data, följ instruktionerna i hello följande kurser:

| Självstudier | Beskrivning |
| --- | --- |
| [Flytta data mellan två molndatalager](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) |I kursen får du skapa en datafabrik med en rörledning som **flyttar data** från Blob storage tooSQL databas. |
| [Omvandla data med Hadoop-kluster](data-factory-build-your-first-pipeline.md) |I de här självstudierna skapar du din första Azure Data Factory med en datapipeline som **bearbetar data** genom att köra Hive-skriptet på ett Azure HDInsight-kluster (Hadoop). |
| [Flytta data mellan ett lokalt datalager och ett molndatalager med hjälp av Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) |I den här självstudiekursen skapar du en datafabrik med en rörledning som **flyttar data** från en **lokalt** SQL Server-databasen tooan Azure blob. Som en del av hello genomgången kan du installera och konfigurera hello Data Management Gateway på din dator. |
