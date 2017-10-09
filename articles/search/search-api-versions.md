---
title: aaaAPI versioner av Azure Search | Microsoft Docs
description: "Princip för programversion för Azure Search REST API: er och hello klientbiblioteket i hello .NET SDK."
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 0458053a-164e-4682-a802-00097ecde981
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/11/2017
ms.author: brjohnst
ms.openlocfilehash: 4fa722fad5577c6b254be7fa673eb240fff316a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="api-versions-in-azure-search"></a>API-versioner i Azure Search
Azure Search samlar funktionen uppdateras regelbundet. Ibland, men inte alltid dessa uppdateringar som kräver att oss toopublish en ny version av våra API toopreserve bakåtkompatibilitet. En ny version kan du toocontrol när och hur du integrerar tjänstuppdateringar för sökning i koden.

Som regel försök vi toopublish nya versioner bara när det behövs, eftersom den kan omfatta vissa arbete tooupgrade din kod toouse ett nytt API-version. Vi bara publicerar en ny version om vi behöver toochange någon aspekt av hello API på ett sätt som bryter bakåtkompatibilitet. Detta kan inträffa på grund av korrigeringar tooexisting funktioner eller på grund av nya funktioner som ändrar befintliga API-ytan.

Vi följer hello samma regel för SDK-uppdateringar. hello Azure Search SDK följer hello [semantiska versionshantering](http://semver.org/) regler, vilket innebär att dess version består av tre delar: högre och lägre build-nummer (till exempel 1.1.0). Vi kommer att släppa en ny högre version av hello SDK endast vid ändringar som bryter bakåtkompatibilitet. Vi kommer att öka hello lägre version för hårt funktionsuppdateringar och för felkorrigeringar vi bara ökar hello-versionen.

> [!NOTE]
> Din Azure Search-tjänstinstansen stöder flera REST API-versioner, inklusive hello senaste. Du kan fortsätta toouse en version när den inte längre hello senaste, men vi rekommenderar att du migrerar din kod toouse hello senaste versionen. När du använder hello REST-API, måste du ange hello API-versionen i varje begäran via parametern för hello api-version. När du använder hello .NET SDK avgör hello version av hello SDK som du använder hello motsvarande version av hello REST API. Om du använder en äldre SDK kan du fortsätta toorun koden utan ändringar även om hello tjänsten uppgraderade toosupport nyare API-version.

## <a name="snapshot-of-current-versions"></a>Ögonblicksbilden av aktuella versioner
Nedan visas en ögonblicksbild av hello aktuella versioner av alla programming interfaces tooAzure sökning.

| Gränssnitt | Senaste huvudversion | Status |
| --- | --- | --- |
| [.NET SDK](https://aka.ms/search-sdk) |3.0 |Allmänt tillgänglig, publicerat November 2016 |
| [Förhandsversion av .NET SDK](https://aka.ms/search-sdk-preview) |2.0-preview |Förhandsgranskning, publicerat augusti 2016 |
| [Tjänsten REST API](https://docs.microsoft.com/rest/api/searchservice/) |2016-09-01 |Allmänt tillgänglig |
| [Tjänsten REST API-förhandsgranskning](search-api-2015-02-28-preview.md) |2015-02-28-preview |Förhandsversion |
| [Hantering av .NET SDK](https://aka.ms/search-mgmt-sdk) |2015-08-19 |Allmänt tillgänglig |
| [REST API för hantering](https://docs.microsoft.com/rest/api/searchmanagement/) |2015-08-19 |Allmänt tillgänglig |

För hello REST API: er, inklusive hello `api-version` på varje anrop krävs. Detta gör det enkelt tootarget en viss version, till exempel en förhandsgranskning API. hello följande exempel visar hur hello `api-version` parameter har angetts:

    GET https://adventure-works.search.windows.net/indexes/bikes?api-version=2016-09-01

> [!NOTE]
> Även om varje begäran har en `api-version`, rekommenderar vi att du använder hello samma version för alla API-begäranden. Detta gäller särskilt när nya API-versioner införa attribut eller funktioner som inte känns igen av tidigare versioner. Blanda API-versioner kan ha oönskade konsekvenser och bör undvikas.
>
> hello Service REST-API och Management REST API är en ny version oberoende av varandra. Alla likheter i versionsnummer är sammanhanget.

Allmänt tillgänglig (eller GA) API: er kan användas i produktion och ämne tooAzure servicenivåavtal. Förhandsversioner har experiment funktioner som inte är alltid migrerade tooa GA-version. **Vi starkt rekommenderar mot att använda Förhandsgranska API: er i program i produktion.**

## <a name="about-preview-and-generally-available-versions"></a>Om förhandsversionen och allmänt tillgängliga versioner
Azure Search Frigör alltid före experiment funktioner via hello REST API först, sedan via förhandsversioner av hello .NET SDK.

Förhandsgranskningsfunktioner är inte garanterat toobe migreras tooa GA-versionen. Funktioner i en GA-versionen är anses sannolikt inte kommer toochange med hello undantag av små bakåtkompatibla korrigeringar och förbättringar, är förhandsgranskningsfunktioner tillgängliga för testning och experiment med hello målet för att samla in feedback om funktionen designen och implementeringen.

Men eftersom förhandsgranskningsfunktioner ämne toochange, rekommenderar vi mot skriva kod som tar ett beroende på förhandsversioner. Om du använder en äldre version av preview rekommenderar vi migrering toohello allmänt tillgänglig (GA) version.

För hello .NET SDK: vägledning för migrering av kod finns på [uppgradera hello .NET SDK](search-dotnet-sdk-migration.md).

Allmän tillgänglighet innebär att Azure Search är nu under hello servicenivåavtal (SLA). hello SLA finns på [serviceavtal för Azure Search](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
