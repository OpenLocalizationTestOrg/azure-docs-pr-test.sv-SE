---
title: "aaaUpgrading toohello Azure Söktjänsts-REST API version 2016-09-01 | Microsoft Docs"
description: "Uppgradera toohello Azure Söktjänsts-REST API version 2016-09-01"
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 6183fa6c-48bb-4af7-adae-4be3bc43c3ed
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: brjohnst
ms.openlocfilehash: d0276b9cc52996a59f9aa726c27e62c6082eb908
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrading-toohello-azure-search-service-rest-api-version-2016-09-01"></a>Uppgradera toohello Azure Söktjänsts-REST API version 2016-09-01
Om du använder version 2015-02-28 eller 2015-02-28-Preview av hello [Azure Söktjänsts-REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx), den här artikeln hjälper dig att uppgradera din programmet toouse hello nästa allmänt tillgänglig API-version, 2016-09-01.

Version 2016-09-01 av hello REST API innehåller vissa ändringar från tidigare versioner. Dessa är främst bakåtkompatibla så ändrar koden kräver endast minsta möjliga ansträngning, beroende på vilken version du använde före. Se [steg tooupgrade](#UpgradeSteps) anvisningar för hur toochange din kod toouse hello nya API-version.

> [!NOTE]
> Din Azure Search-tjänstinstansen stöder flera REST API-versioner, inklusive hello senaste. Du kan fortsätta toouse en version när den inte längre hello senaste, men vi rekommenderar att du migrerar din kod toouse hello senaste versionen.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2016-09-01"></a>Vad är nytt i version 2016-09-01
Version 2016-09-01 är hello andra allmänt tillgänglig version av hello Azure Söktjänsts-REST API. Nya funktioner i den här API-versionen är:

* [Anpassade analyzers](https://aka.ms/customanalyzers), vilket gör att du tootake kontroll över hello att konvertera text till indexeras och sökbara token.
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) och [Azure Table Storage](search-howto-indexing-azure-tables.md) indexerare som gör att du tooeasily importera data från Azure-lagring till Azure Search i ett schema eller på begäran.
* [Fältet mappningar](search-indexer-field-mappings.md), vilket gör att du toocustomize hur indexerare importerar data till Azure Search.
* ETags som gör att du tooupdate hello definitioner av index och indexerare datakällor på ett samtidighet-säkert sätt. 

<a name="UpgradeSteps"></a>

## <a name="steps-tooupgrade"></a>Steg tooupgrade
Om du uppgraderar från version 2015-02-28, har du förmodligen inte toomake eventuella ändringar tooyour kod, än toochange hello-versionsnumret. hello endast situationer där du kanske behöver toochange koden är när:

* Det går inte att din kod när okända egenskaper returneras som en API-svar. Ditt program bör Ignorera egenskaper som inte förstår som standard.
* Koden kvarstår API-begäranden och försöker tooresend dem toohello nya API-versionen. T.ex, detta kan inträffa om tillämpningsprogrammet kvarstår fortsättning token som returnerades från hello Sök-API (leta efter mer information `@search.nextPageParameters` i hello [Sök API-referens](https://msdn.microsoft.com/library/azure/dn798927.aspx#Anchor_1)).

Om någon av dessa situationer gäller tooyou kan måste toochange koden i enlighet med detta. I annat fall inga ändringar bör vara nödvändigt om du inte vill använda hello toostart [nya funktioner](#WhatsNew) version 2016-09-01.

Om du uppgraderar från version 2015-02-28-Preview hello ovan gäller också, men du måste också vara medveten om att vissa funktioner inte är tillgängliga i version 2016 09 01:

* Azure Blob Storage indexeraren-stöd för CSV-filer och blobbar som innehåller JSON-matriser.
* Synonymer
* ”Mer så här” frågor

Om din kod använder dessa funktioner, kommer inte att kunna tooupgrade too2016-09-01 utan att ta bort din användning av dessa.

> [!IMPORTANT]
> Kontrollera komma ihåg, förhandsgranska API: er är avsedd för testning och utvärdering och ska inte användas i produktionsmiljöer.
> 
> 

## <a name="conclusion"></a>Slutsats
Om du vill ha mer information om hur du använder hello Azure Söktjänsts-REST API finns hello nyligen uppdaterat [API-referens](https://msdn.microsoft.com/library/azure/dn798935.aspx) på MSDN.

Vi uppskattar din feedback på Azure Search. Om du får problem känna sig fria tooask oss hjälp om hello [Azure Search MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch) eller [StackOverflow](http://stackoverflow.com/). Om begär du en fråga om Azure söka på StackOverflow, se till att tootag med `azure-search`.

Tack för att använda Azure Search!

