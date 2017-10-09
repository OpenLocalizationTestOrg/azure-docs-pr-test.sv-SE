---
title: "aaaHow toowork med 'stordata' datakällor | Microsoft Docs"
description: "Hur tooarticle färgmarkera mönster för att använda Azure Data Catalog med 'stordata' datakällor, inklusive Azure Blob Storage, Azure Data Lake och Hadoop HDFS."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 626d1568-0780-4726-bad1-9c5000c6b31a
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: e478f71f26744975a7d7e1784b74bf50b424cf65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toowork-with-big-data-sources-in-azure-data-catalog"></a>Hur toowork med stordata datakällor i Azure Data Catalog
## <a name="introduction"></a>Introduktion
**Microsoft Azure Data Catalog** är en helt hanterad molntjänst som fungerar som ett system för registrering och identifieringssystem för företagets datakällor. Allt handlar om hjälper människor identifiera, förstå och använda datakällor och hjälpa organisationer tooget mer värde från deras befintliga datakällor, inklusive stordata.

**Azure Data Catalog** stöder hello registrering av Azure-bloggen Storage-blobbar och samt Hadoop HDFS filer och kataloger. hello halvstrukturerade arten av de här datakällorna ger stor flexibilitet. Dock tooget hello största möjliga värde från att registrera dem med **Azure Data Catalog**, användare måste ta hänsyn till hur hello datakällor är ordnade.

## <a name="directories-as-logical-data-sets"></a>Kataloger som logiska datauppsättningar
Vanliga mönstret för att ordna stora datakällor är tootreat kataloger som logiska datauppsättningar. Översta kataloger är används toodefine en datamängd, medan undermappar definiera partitioner och hello-filer som de innehåller datalagring hello sig själv.

Ett exempel på det här mönstret kan vara:

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

I det här exemplet representerar vehicle_maintenance_events och location_tracking_events logiska datauppsättningar. Var och en av mapparna innehåller filer som är ordnad efter år och månad i undermappar. Var och en av dessa mappar kan innehålla hundratals eller tusentals filer.

I det här mönstret registrering av enskilda filer med **Azure Data Catalog** förmodligen inte vara meningsfullt. I stället registrera hello kataloger som representerar hello datauppsättningar som vara meningsfullt toohello användare arbetar med hello data.

## <a name="reference-data-files"></a>Referens-datafiler
En kompletterande mönstret är toostore referensdatamängder som enskilda filer. Dessa datauppsättningar kan betraktas som ”liten” hello-sida av stordata och är ofta liknande toodimensions i en modell med analysdata. Referens-datafiler innehåller poster som används tooprovide kontext för hello huvuddelen av hello datafiler lagrade på andra ställen i hello stordataarkiv.

Ett exempel på det här mönstret kan vara:

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

När en analytiker eller data forskare fungerar med hello uppgifterna i hello större katalogstrukturer hello data i dessa filer kan det vara används tooprovide mer detaljerad information för enheter som kallas tooonly efter namn eller ID i hello större data Ange.

I det här mönstret är det klokt tooregister hello referens för enskilda filer med **Azure Data Catalog**. Representerar en datauppsättning för varje fil och var och en kan kommenterats och identifierade individuellt.

## <a name="alternate-patterns"></a>Alternativa mönster
hello mönster som beskrivs i föregående avsnitt hello finns två möjliga sätt en stordataarkiv kan ordnas, men varje implementering är olika. Oavsett hur dina datakällor är strukturerade, när du registrerar ett stort datakällor med **Azure Data Catalog**, fokusera på registrera hello filer och kataloger som representerar hello datauppsättningar av värdet tooothers inom din organisation. Registrerar alla filer och kataloger kan göra hello-katalogen, vilket gör det svårare för användare toofind de behöver.

## <a name="summary"></a>Sammanfattning
Registrera datakällor med **Azure Data Catalog** gör dem lättare toodiscover och förstå. Genom att registrera och kommentera hello stordata filer och kataloger som representerar logiska datauppsättningar, kan du hjälpa användarna att hitta och använda hello stort datakällor de behöver.
