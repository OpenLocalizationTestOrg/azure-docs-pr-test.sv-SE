---
title: aaaUse Case - kund profilering
description: "Läs mer om hur Azure Data Factory används toocreate en datadrivna arbetsflödet (pipeline) tooprofile spel kunder."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: e07d55cf-8051-4203-9966-bdfa1035104b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: 47f5e77242366c80cce2a2db65e3c696505b3e1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-case---customer-profiling"></a>Användningsfall - Kundprofilering
Azure Data Factory är en av många tjänster som används tooimplement hello Cortana Intelligence Suite solution Accelerator-verktyg.  Mer information om Cortana Intelligence finns [Cortana Intelligence Suite](http://www.microsoft.com/cortanaanalytics). I det här dokumentet beskriver vi en enkel användning case toohelp du komma igång med att förstå hur Azure Data Factory kan lösa vanliga problem med analytics.

## <a name="scenario"></a>Scenario
Contoso är ett spel företag som skapar spel för flera plattformar: konsoler, hand hålls enheter och datorer (datorer). När spelare spelar dessa spel, produceras stora volymer av loggdata som spårar hello användningsmönster, spel-format och inställningar för hello användare.  När den kombineras med demografisk, regionala och produktdata Contoso kan utföra analyser tooguide dem om hur tooenhance spelare upplevelse och rikta dem för uppgraderingar och i spelet inköp. 

Contosos målet är tooidentify upp-säljer/cross-säljer affärsmöjligheter baserat på hello spel historik över dess spelare och Lägg till tilltalande funktioner toodrive tillväxt och ger en bättre upplevelse toocustomers. För den här användningsfall använder vi ett spel företag som ett exempel på ett företag. hello företaget vill ha toooptimize dess spel baserat på spelare beteende. Dessa principer tillämpas tooany företag som vill tooengage sina kunder runt dess varor och tjänster och förbättra sina kunder.

I den här lösningen vill Contoso tooevaluate hello effektivitet med en marknadsföringskampanj den nyligen har startats. Vi börjar med hello rådata spel loggar, process utöka dem med geolokalisering data, ansluta med reklam referensdata och slutligen kopierar du dem till en Azure SQL Database tooanalyze hello kampanjens påverkan.

## <a name="deploy-solution"></a>Distribuera lösningen
Du behöver tooaccess och testa den här enkla användningsfall är en [Azure-prenumeration](https://azure.microsoft.com/pricing/free-trial/), en [Azure Blob storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account), och en [Azure SQL Database](../sql-database/sql-database-get-started.md). Du distribuerar hello kunden profilering pipeline från hello **exempel pipelines** panelen på hello startsida i din data factory.

1. Skapa en datafabrik eller öppna en befintlig datafabrik. Se [kopiera data från Blob Storage tooSQL databasen med hjälp av Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) för steg toocreate en datafabrik.
2. I hello **DATAFABRIKEN** bladet för hello data factory klickar du på hello **exempel pipelines** panelen.

    ![Exempel pipelines sida vid sida](./media/data-factory-samples/SamplePipelinesTile.png)
3. I hello **exempel pipelines** bladet, klickar du på hello **kunden profilering** som du vill toodeploy.

    ![Exempel pipelines bladet](./media/data-factory-samples/SampleTile.png)
4. Ange konfigurationsinställningar för hello exemplet. Till exempel din Azure storage-kontonamnet och nyckeln, Azure SQL-servernamnet, databas, användar-ID och lösenord.

    ![Exempel-bladet](./media/data-factory-samples/SampleBlade.png)
5. När du är klar med att ange hello konfigurationsinställningar, klickar du på **skapa** toocreate/distribuera hello exempel pipelines och länkade tjänster/tabeller som används av hello pipelines.
6. Du ser hello status för distributionen på hello exempel panelen som du tidigare klickade på hello **exempel pipelines** bladet.

    ![Status för distribution](./media/data-factory-samples/DeploymentStatus.png)
7. När du ser hello **distributionen lyckades** meddelandet på hello ikonen för hello exemplet, Stäng hello **exempel pipelines** bladet.  
8. På **DATA FACTORY** bladet som du ser att länkade tjänster, datauppsättningar och pipelines läggs tooyour data factory.  

    ![Bladet Datafabrik](./media/data-factory-samples/DataFactoryBladeAfter.png)

## <a name="solution-overview"></a>Lösning: översikt
Den här enkla användningsfall kan användas som ett exempel på hur du kan använda Azure Data Factory tooingest, förbereda, transformera, analysera och publicera data.

![Arbetsflödet slutpunkt till slutpunkt](./media/data-factory-customer-profiling-usecase/EndToEndWorkflow.png)

Denna bild visar hur hello data pipelines visas i hello Azure-portalen när de har distribuerats.

1. Hej **PartitionGameLogsPipeline** läser hello rådata spel händelser från blob storage och skapar partitioner baserat på år, månad och dag.
2. Hej **EnrichGameLogsPipeline** ansluter partitionerade spel händelser med geo kod referensdata och förbättra hello data genom att mappa IP-adresser toohello motsvarande geoplatser.
3. Hej **AnalyzeMarketingCampaignPipeline** pipeline använder hello utökat data och bearbetar med hello annonserar data toocreate hello slutversionen som innehåller marknadsföring kampanj effektivitet.

I det här exemplet är Data Factory används tooorchestrate aktiviteter som kopiera indata, transformering och bearbeta hello data och utdata hello slutliga data tooan Azure SQL Database.  Du kan också visualisera data pipelines hello nätverk, hantera dem och övervaka deras status från hello Användargränssnittet.

## <a name="benefits"></a>Fördelar
Optimera sina analyser och justera med affärsmål spel företagets är tooquickly kan samla in användningsmönster och analysera hello effektivitet med dess marknadsföringskampanjer.

