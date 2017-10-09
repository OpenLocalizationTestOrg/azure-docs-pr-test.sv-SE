---
title: "aaaData Factory användningsfall - produktrekommendationer"
description: "Läs mer om ett användningsfall som implementeras med hjälp av Azure Data Factory tillsammans med andra tjänster."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 6f1523c7-46c3-4b8d-9ed6-b847ae5ec4ae
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: d7912965fe4762d64e8ca3c28381ea6187f36631
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-case---product-recommendations"></a>Användningsfall - Produktrekommendationer
Azure Data Factory är en av många tjänster som används tooimplement hello Cortana Intelligence Suite solution Accelerator-verktyg.  Se [Cortana Intelligence Suite](http://www.microsoft.com/cortanaanalytics) sidan för ytterligare information om det här paketet. I det här dokumentet beskrivs vanliga användningsfall som Azure användare redan har löst och implementeras med hjälp av Azure Data Factory och andra Cortana Intelligence component services.

## <a name="scenario"></a>Scenario
Online återförsäljare vill ofta tooentice sina kunder toopurchase produkter genom att presentera med produkter som de mest sannolika toobe intresserad och därför troligen toobuy. tooaccomplish, online återförsäljare behöver toocustomize deras online användarupplevelsen genom att använda anpassade produktrekommendationer för den specifika användaren. Dessa anpassade är nyligen införda toobe gjorts baserat på deras aktuella och historiska shopping beteende data, produktinformation, varumärken och produkt- och segmentering data.  Dessutom kan de ger hello användaren produktrekommendationer för analys av den övergripande användning beteende från sina användare kombineras.

hello målet med dessa återförsäljare är toooptimize för användaren klicka till försäljning konverteringar och få högre försäljningsintäkter.  De uppnå den här konverteringen genom att leverera kontextuella, beteende-baserade produktrekommendationer baserat på kundens intressen och åtgärder. För den här användningsfall använder vi online återförsäljare som ett exempel på företag som vill ha toooptimize för sina kunder. Dessa principer tillämpas tooany företag som vill tooengage sina kunder runt dess varor och tjänster och förbättra sina kunder köpa med anpassade produktrekommendationer.

## <a name="challenges"></a>Utmaningar
Det finns många UTMANINGAR den online återförsäljare sida vid tooimplement den här typen av användningsfall. 

Data av olika storlekar och former måste först vara inhämtas från flera datakällor, både lokalt och i hello molnet. Dessa data innehåller produktdata och historiska beteende kundinformation användardata hello användaren bläddrar hello online retail-platsen. 

Andra, anpassade produktrekommendationer måste rimligen och korrekt beräknas och förutsade. Tooproduct, varumärken och beteende och webbläsare kundinformation, online återförsäljare måste dessutom också tooinclude kundfeedback på senaste inköp toofactor i hello bestämning av produkten hello rekommendationer för hello användare. 

Det tredje måste hello rekommendationer omedelbart leverans toohello användaren tooprovide sömlös surfning och köpa upplevelse, och ger hello senaste och mest relevanta rekommendationer. 

Slutligen återförsäljare måste toomeasure hello effektivitet med sina metoden genom att spåra övergripande upp säljer mellan säljer Klicka till konvertering försäljning lyckade och justera tootheir framtida rekommendationer.

## <a name="solution-overview"></a>Lösning: översikt
Det här exemplet användningsfall har lösas och implementeras av verkliga Azure-användare med hjälp av Azure Data Factory och andra Cortana Intelligence component services, inklusive [HDInsight](https://azure.microsoft.com/services/hdinsight/) och [Power BI](https://powerbi.microsoft.com/).

hello online återförsäljare använder ett Azure Blob-lagring, en lokal SQLServer, Azure SQL DB och en relationella datamart som deras alternativ för lagring av data i hela hello arbetsflöde.  hello blobstore innehåller kundinformation, beteende kunddata och produkten information data. hello produktinformation innehåller information produktinformation märke och en produktkatalogen lagras lokalt i ett SQL data warehouse. 

Alla hello data kombineras och skickas till en produkt rekommendation system toodeliver personliga rekommendationer baserat på kundens intressen och åtgärder, medan hello användaren bläddrar produkter i hello-katalogen på hello-webbplatsen. hello kunder finns också produkter som är relaterade toohello produkten de tittar på baserat på totala användningsmönster för webbplatsen som inte är relaterade tooany en användare.

![Använd case diagram](./media/data-factory-product-reco-usecase/diagram-1.png)

Gigabyte rådata web loggfiler skapas varje dag från hello online återförsäljare webbplats som halvstrukturerade filer. Hej rådata web loggfiler och hello kund- och kataloginformation inhämtas regelbundet till ett Azure Blob storage med hjälp av Data Factory globalt distribuerade data flyttas som en tjänst. hello rådata loggfiler för hello dag partitioneras (per år och månad) i blob storage för långsiktig lagring.  [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) är används toopartition hello rådata loggfiler i hello blob store och processen hello inhämtas loggar i stor skala med hjälp av både Hive och Pig-skript. Hej partitionerade webbloggar data sedan bearbetade tooextract hello behövs indata för machine learning rekommendation system toogenerate hello personliga produktrekommendationer.

hello rekommendation system som används för hello machine learning i det här exemplet är en öppen källkod machine learning rekommendation plattform från [Apache Mahout](http://mahout.apache.org/).  Alla [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) eller anpassade modell kan vara tillämpade toohello scenario.  Hej Mahout modellen är används toopredict hello likheten mellan objekten på hello webbplats baserat på totala användningsmönster och toogenerate hello personliga rekommendationer baserat på hello enskilda användare.

Slutligen är hello resultatuppsättningen för anpassade produktrekommendationer flyttade tooa relationella datamarten för användning av hello återförsäljare webbplats.  hello resultatuppsättningen kan även nås direkt från blob storage med ett annat program eller flyttas tooadditional lagrar för andra användare och användningsfall.

## <a name="benefits"></a>Fördelar
Genom att optimera sina produkten rekommendation strategi och justera med affärsmål uppfyllt hello lösning hello online återförsäljare försäljning och marknadsföring mål. Dessutom kan de kan toooperationalize och hantera hello produkten rekommendation arbetsflödet på ett effektivt, tillförlitligt och kostnadseffektivt sätt. hello metoden gjort det enkelt för dem tooupdate sina modell och finjustera effektivitet baserat på hello mått i försäljning Klicka till konvertering lyckade försök. Med hjälp av Azure Data Factory, de kan tooabandon sina tidskrävande och kostsamt manuell moln resurshantering och flytta tooon begäran molnet resurshantering. Därför kan de kan toosave tid, pengar, och minska sina toosolution distributionen. Övriga datavyer och operativa tjänstens hälsa blev enkelt toovisualize och felsöka med hello intuitiva Data Factory-övervakning och hantering användargränssnitt som är tillgängliga från hello Azure-portalen. Lösningen kan nu schemalagda och hanteras så att klar data på ett tillförlitligt sätt produceras och levereras toousers och data och bearbetning beroenden hanteras automatiskt utan mänsklig inblandning.

Genom att tillhandahålla anpassade shopping upplevelsen hello online återförsäljare skapas en mer konkurrenskraftiga, spännande kund får och därför att öka försäljning och övergripande nöjda.

