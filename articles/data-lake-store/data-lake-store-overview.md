---
title: "aaaOverview för Azure Data Lake Store | Microsoft Docs"
description: "Förstå vad är Azure Data Lake Store och hello värde över andra dataarkiv"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: b3475057-9427-4492-a3af-25a802a23a79
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 5a60a6b86a51c44647cf4ee168fb333d1c37b1b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-data-lake-store"></a>Översikt över Azure Data Lake Store
Azure Data Lake Store är en företagsomfattande storskalig lagringsplats för analytiska arbetsbelastningar för stordata. Azure Data Lake kan du toocapture data för alla storlek, typ och införandet hastigheten på en enda plats för drifts- och undersökande analyser.

> [!TIP]
> Använd hello [Data Lake Stores Utbildningsväg](https://azure.microsoft.com/documentation/learning-paths/data-lake-store-self-guided-training/) toostart utforska hello Azure Data Lake Store-tjänsten.
> 
> 

Azure Data Lake Store kan nås från Hadoop (tillgänglig med HDInsight-kluster) med hjälp av hello WebHDFS-kompatibelt REST API: er. Det är särskilt utformad tooenable analyser på hello lagrade data och är inställd för prestanda för dataanalyser. Out of box hello, innehåller alla funktioner och hello företagsklass, säkerhet, hanterbarhet, skalbarhet, tillförlitlighet och tillgänglighet – grundläggande för verkliga företagsfall.

![Azure Data Lake](./media/data-lake-store-overview/data-lake-store-concept.png)

Hello viktiga funktioner för hello Azure Data Lake bland hello följande.

### <a name="built-for-hadoop"></a>Byggt för Hadoop
hello Azure Data Lake store är ett Apache Hadoop-filsystem som kompatibelt med Hadoop Distributed File System (HDFS) och fungerar med hello Hadoop-ekosystemet.  Dina befintliga HDInsight-program eller tjänster som använder hello WebHDFS API kan enkelt integreras med Data Lake Store. Data Lake Store har också ett WebHDFS-kompatibelt REST-gränssnitt för program

Data som lagras i Data Lake Store kan enkelt analyseras med hjälp av analytiska Hadoop-ramar, till exempel MapReduce eller Hive. Microsoft Azure HDInsight-kluster kan etableras och konfigurerats toodirectly komma åt data som lagras i Data Lake Store.

### <a name="unlimited-storage-petabyte-files"></a>Obegränsad lagring, petabytefiler
Azure Data Lake Store ger obegränsad lagring och lämpar sig för att lagra en mängd olika data för analyser. Det medför inte några gränser för kontostorlekar, filstorlekar eller hello mängden data som kan lagras i en datasjö. Enskilda filer kan variera mellan kilobyte toopetabytes i storlek vilket gör det en bra val toostore alla typer av data. Data lagras varaktigt genom att göra flera kopior och det finns ingen gräns på hello tidsperiod för vilken hello data kan lagras i hello data lake.

### <a name="performance-tuned-for-big-data-analytics"></a>Prestandajusterad för analyser av stordata
Azure Data Lake Store är utformat för att köra storskaliga analytiska system som kräver omfattande genomströmning tooquery och analysera stora mängder data. Hej datasjön sprider delar av en fil under ett antal enskilda lagringsservrar. Detta förbättrar hello läsa dataflöde vid läsning av hello filen parallellt för att utföra dataanalys.

### <a name="enterprise-ready-highly-available-and-secure"></a>Företagsredo: Hög tillgänglighet och säkert
Azure Data Lake Store ger branschstandard för tillgänglighet och pålitlighet. Dina datatillgångar lagras varaktigt genom att göra redundanta kopior tooguard mot oväntade fel. Företag kan använda Azure Data Lake i sina lösningar som en viktig del av deras befintliga dataplattform.

Data Lake Store innehåller också säkerhet i företagsklass för hello lagrade data. Mer information finns i [Skydda data i Azure Data Lake Store](#DataLakeStoreSecurity).

### <a name="all-data"></a>Alla data
Azure Data Lake Store kan lagra data i deras ursprungliga format, så som de är, utan några tidigare transformationer. Data Lake Store kräver ett schema toobe definierats innan hello data har lästs in, lämnar in toohello enskilda analytiska ramverket toointerpret hello data inte och definiera ett schema för när hello hello analys. Som kan toostore filer av godtycklig storlek och format gör det möjligt för Data Lake Store toohandle strukturerade, delvis strukturerade och Ostrukturerade data.

Azure Data Lake Store-behållare för data är i stort sett mappar och filer. Du använder hello lagrade data med hjälp av SDK: er, Azure-portalen och Azure Powershell. Så länge du placerar dina data i hello store med hjälp av dessa gränssnitt och använder hello lämpliga behållare kan du lagra alla typer av data. Data Lake Store utför inte någon särskild hantering av data baserat på hello typ av data som lagras.

## <a name="DataLakeStoreSecurity"></a>Skydda data i Azure Data Lake Store
Azure Data Lake Store använder Azure Active Directory för autentisering och åtkomstkontrollistor (ACL) toomanage åtkomst tooyour data.

| Funktion | Beskrivning |
| --- | --- |
| Autentisering |Azure Data Lake Store integreras med Azure Active Directory (AAD) för identitets- och åtkomsthantering för alla hello data som lagras i Azure Data Lake Store. På grund av hello integration, Azure Data Lake fördelar från alla AAD-funktioner, inklusive multifaktorautentisering, villkorlig åtkomst, rollbaserad åtkomstkontroll, övervakning av programanvändning, säkerhetsövervakning och-avisering, osv. Azure Data Lake Store stöder hello OAuth 2.0-protokollet för autentisering i hello REST-gränssnittet. |
| Åtkomstkontroll |Azure Data Lake Store ger åtkomstkontroll genom att stödja POSIX-typ behörigheter som exponeras av hello WebHDFS-protokollet. I hello Data Lake Store Public Preview (hello aktuella versionen), kan du aktivera ACL: er hello rotmappen för undermappar och på enskilda filer. Mer information om hur åtkomstkontrollposter fungerar i kontexten för Data Lake Store finns [Åtkomstkontroll i Data Lake Store](data-lake-store-access-control.md). |
| Kryptering |Data Lake Store ger också kryptering för data som lagras i hello-konto. Du kan ange hello krypteringsinställningar när du skapar ett Data Lake Store-konto. Du valde toohave data krypteras eller välja utan kryptering. Mer information om hur tooprovide krypteringsrelaterade konfiguration, se [Kom igång med Azure Data Lake Store med hjälp av hello Azure Portal](data-lake-store-get-started-portal.md). |

Vill du toolearn mer om hur du skyddar data i Data Lake Store. Följ hello länkarna nedan.

* Anvisningar för hur toosecure data i Data Lake Store finns i [skydda data i Azure Data Lake Store](data-lake-store-secure-data.md).
* Föredrar du video? [Det här videoklippet](https://mix.office.com/watch/1q2mgzh9nn5lx) på hur toosecure data lagras i Data Lake Store.

## <a name="applications-compatible-with-azure-data-lake-store"></a>Program som är kompatibla med Azure Data Lake Store
Azure Data Lake Store är kompatibelt med de flesta öppna källkomponenter i hello Hadoop-ekosystemet. Det är även snyggt integrerat med andra Azure-tjänster. På så sätt blir Data Lake Store ett perfekt alternativ för dina datalagringsbehov. Följ hello länkarna nedan toolearn mer om hur Data Lake Store kan användas både med öppna källkodskomponenter samt andra Azure-tjänster.

* Se [Program och tjänster som är kompatibla med Azure Data Lake Store](data-lake-store-compatible-oss-other-applications.md) för en lista med öppna källkodsprogram som är kompatibla med Data Lake Store.
* Se [integrering med andra Azure-tjänster](data-lake-store-integrate-with-other-services.md) toounderstand hur Data Lake Store kan användas med andra Azure services tooenable ett brett spektrum av scenarier.
* Se [scenarier för användning av Data Lake Store](data-lake-store-data-scenarios.md) toolearn hur toouse Data Lake lagra i scenarier, till exempel vill föra in data, bearbetning av data, hämta data och visualisera data.

## <a name="what-is-azure-data-lake-store-file-system-adl"></a>Vad är Azure Data Lake Store-filsystem (adl://)?
Data Lake Store kan nås via hello nya filsystemet, hello AzureDataLakeFilesystem (adl: / /), i Hadoop-miljöer (tillgängligt med HDInsight-kluster). Program och tjänster som använder adl: / / kan tootake nytta av ytterligare prestandaoptimering som inte är tillgänglig i WebHDFS. Därför Data Lake Store ger du hello flexibilitet tooeither utnyttja hello bästa prestanda med hello rekommenderat alternativ för att använda adl: / / eller hantera befintlig kod genom fortsätter toouse hello WebHDFS API direkt. Azure HDInsight utnyttjar fullt hello AzureDataLakeFilesystem tooprovide hello bästa prestanda på Data Lake Store.

Du kan komma åt dina data i hello Data Lake Store med hjälp av `adl://<data_lake_store_name>.azuredatalakestore.net`. Mer information om hur tooaccess hello data i hello Data Lake Store finns [visa egenskaper för hello lagrade data](data-lake-store-get-started-portal.md#properties)

## <a name="how-do-i-start-using-azure-data-lake-store"></a>Hur börjar jag använda Azure Data Lake Store?
Se [Kom igång med Data Lake Store med hjälp av hello Azure Portal](data-lake-store-get-started-portal.md), på hur tooprovision ett Data Lake Store med hjälp av hello Azure-portalen. När du har etablerat Azure Data Lake kan du lära dig hur toouse erbjudanden för stordata, till exempel Azure Data Lake Analytics eller Azure HDInsight med Data Lake Store. Du kan också skapa toocreate en .NET-program ett Azure Data Lake Store-konto och utföra åtgärder som överföringsdata, hämta data osv.

* [Kom igång med Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Använd Azure HDInsight med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Kom igång med Azure Data Lake Store med hjälp av .NET SDK](data-lake-store-get-started-net-sdk.md)

## <a name="data-lake-store-videos"></a>Data Lake Store-videor
Om du vill titta på videor toolearn, ger Data Lake Store videor på en mängd funktioner.

* [Skapa ett Azure Data Lake Store-konto](https://mix.office.com/watch/1k1cycy4l4gen)
* [Använd hello Data Explorer tooManage Data i Azure Data Lake Store](https://mix.office.com/watch/icletrxrh6pc)
* [Anslut Azure Data Lake Analytics tooAzure Data Lake Store](https://mix.office.com/watch/qwji0dc9rx9k)
* [Åtkomst till Azure Data Lake Store via Data Lake Analytics](https://mix.office.com/watch/1n0s45up381a8)
* [Anslut Azure HDInsight tooAzure Data Lake Store](https://mix.office.com/watch/l93xri2yhtp2)
* [Åtkomst till Azure Data Lake Store via Hive och Pig](https://mix.office.com/watch/1n9g5w0fiqv1q)
* [Använd DistCp (Hadoop-distribuerad kopia) toocopy data tooand från Azure Data Lake Store](https://mix.office.com/watch/1liuojvdx6sie)
* [Använd Apache Sqoop toomove data mellan relationella källor och Azure Data Lake Store](https://mix.office.com/watch/1butcdjxmu114)
* [Data Orchestration med hjälp av Azure Data Factory för Azure Data Lake Store](https://mix.office.com/watch/1oa7le7t2u4ka)
* [Skydda Data i hello Azure Data Lake Store](https://mix.office.com/watch/1q2mgzh9nn5lx)

