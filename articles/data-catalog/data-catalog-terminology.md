---
title: "aaaAzure terminologi för Data Catalog | Microsoft Docs"
description: "Den här artikeln innehåller en introduktion tooconcepts och termer som används i Azure Data Catalog-dokumentationen."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 6fec74d9-4a3c-4b4b-88ba-cad5ad143331
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: b5f071db4f62c914d2c1cdef9aa686b18d5297d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-catalog-terminology"></a>Terminologi för Azure Data Catalog
## <a name="catalog"></a>Katalogen
hello Azure Data Catalog är en molnbaserad metadata-databasen där källor och data tillgångar kan registreras. hello catalog fungerar som en central lagringsplats för strukturella metadata som extraherats från datakällorna och beskrivande metadata som lagts till av användare.

## <a name="data-source"></a>Datakälla
En datakälla är ett system eller en behållare som hanterar datatillgångar. Exempel: SQL Server-databaser, Oracle-databaser, SQL Server Analysis Services-databaser (tabell eller flerdimensionella) och SQL Server Reporting Services-servrar.

## <a name="data-asset"></a>Datatillgång
Datatillgångar är objekt inom datakällor som kan registreras med hello katalog. Exempel är SQL Server-tabeller och vyer, Oracle-tabeller och vyer, SQL Server Analysis Services åtgärder, mått och KPI: er och SQL Server Reporting Services-rapporter.

## <a name="data-asset-location"></a>Plats för tillgångsinformation
hello katalogen lagrar hello platsen för en datakälla eller en datatillgång, vilket kan vara används tooconnect toohello datakällan med ett klientprogram. hello format och information om hello plats beror på hello typ av datakälla. Till exempel framgår en SQL Server-tabell av dess fyra delnamn – servernamnet, databasnamnet, schemanamnet, objektnamn – när en SQL Server Reporting Services-rapport kan identifieras av dess URL.

## <a name="structural-metadata"></a>Strukturella metadata
Strukturella metadata är hello metadata som extraherats från en datakälla som beskriver hello strukturen i en datatillgång. Detta inkluderar hello tillgångar plats dess namn och typ och ytterligare typspecifika egenskaper. Hello strukturella metadata för tabeller och vyer innehåller till exempel hello namn och datatyper för hello-objektets kolumner.

## <a name="descriptive-metadata"></a>Beskrivande metadata
Beskrivande metadata är metadata som beskriver hello syfte eller syftet med en datatillgång. Vanligtvis beskrivande metadata läggs till av katalogens användare med hello Azure Data Catalog-portalen, men den även kan extraheras från hello datakällan under registreringen. Till exempel hello Azure Data Catalog registreringsverktyget extrahera beskrivningar från hello beskrivningsegenskapen i SQL Server Analysis Services och SQL Server Reporting Services och hello [ms_description utökad egenskap](https://technet.microsoft.com/library/ms190243.aspx)i SQL Server-databaser, om de här egenskaperna är ifyllda med värden.

## <a name="request-access"></a>Begär åtkomst
En datatillgång beskrivande metadata kan innehålla information om hur toorequest åt toohello data tillgång eller datakälla. Den här informationen visas hello Dataplats tillgången och kan innehålla en eller flera av följande alternativ för hello:

* hello e-postadress för hello användare eller grupp som ansvarar för att bevilja åtkomst toohello datakälla.
* hello-URL för hello dokumenterade processen att användarna måste följa toogain access toohello-datakälla.
* hello-URL för ett identitets- och hanteringsverktyg (till exempel Microsoft Identity Manager) som kan använda toogain access toohello-datakälla.
* En fritext-post som beskriver hur användarna kan få åtkomst till toohello datakälla.

## <a name="preview"></a>Förhandsversion
En förhandsgranskning i Azure Data Catalog är en ögonblicksbild av upp too20 poster som kan hämtas från hello datakällan under registreringen och lagras i katalogen hello med hello data tillgångens metadata. hello preview hjälper användare som identifierar en datatillgång bättre förstå dess funktionen och syftet. Med andra ord ser exempeldata kan vara mer användbart än ser bara hello kolumnnamn och datatyper.
Förhandsversioner måste stöds endast för tabeller och vyer, och du uttryckligen av hello användare under registreringen.

## <a name="data-profile"></a>Data-profil
En profil för data i Azure Data Catalog är en ögonblicksbild av tabellen nivå och på kolumnnivå metadata om en registrerade datatillgångar som kan extraheras från hello datakälla under registreringen och lagras i katalogen hello med hello data tillgångens metadata. hello data profil hjälper användare som identifierar en datatillgång bättre förstå dess funktionen och syftet. Liknande toopreviews, data-profiler måste väljas uttryckligen genom hello användaren under registreringen.

> [!NOTE]
> Extrahera en data-profil kan vara en kostsam åtgärd för stora tabeller och vyer och kan avsevärt öka hello tooregister tid som krävs för en datakälla.
>
>

## <a name="user-perspective"></a>Användarperspektiv
I Azure Data Catalog kan alla användare ange beskrivande metadata för en registrerad datatillgång. Varje användare har olika perspektiv hello data och dess användning. Hej administratör ansvarar för en server kan exempelvis ange hello information om dess servicenivåavtal (SLA) eller windows säkerhetskopiering. en data-steward kan innehålla länkar toodocumentation för hello företag bearbetar hello data stöder; och en analytiker kan ange en beskrivning i hello villkor som är mest relevant tooother analytiker och som kan vara mest värdefulla toothose användare som behöver toodiscover och förstå hello data.

Var och en av dessa perspektiv är natur värdefulla och med Azure Data Catalog kan varje användare ge hello information som är meningsfullt toothem medan alla användare kan använda denna information toounderstand hello data och dess syfte.

## <a name="expert"></a>Expert
En expert är en användare som har identifierats som har ett välgrundat ”expert” perspektiv för en datatillgång. Alla användare kan lägga till sig själva eller en annan användare som en expert för en tillgång. Som listats som en expert inte förmedla ytterligare behörighet i Azure Data Catalog. Det gör att användare tooeasily Leta upp de perspektiv som är mest sannolika toobe som är användbar när du granskar beskrivande metadata för en tillgång.

## <a name="owner"></a>Ägare
En användare som har ytterligare behörigheter för att hantera en datatillgång i Azure Data Catalog är ägaren. Användare kan bli ägare till registrerade datatillgångar och ägare kan lägga till andra användare som delägare. Mer information finns i [hur toomanage datatillgångar](data-catalog-how-to-manage.md)  

> [!NOTE]
> Ägarskap och Datorhantering är endast tillgänglig i hello Standard Edition av Azure Data Catalog.
>
>

## <a name="registration"></a>Registrering
Registreringen är hello act extrahera data tillgångens metadata från en datakälla och kopiera den toohello Azure Data Catalog-tjänsten. Datatillgångar som har registrerats kan sedan kommenterats och identifieras.

## <a name="see-also"></a>Se även
* [Vad är Azure Data Catalog?](data-catalog-what-is-data-catalog.md) -Den här artikeln innehåller en översikt över hello Azure Data Catalog service, hello värde och hello scenarier som stöds.
* [Kom igång med Azure Data Catalog](data-catalog-get-started.md) -den här artikeln innehåller en vägledning för slutpunkt-till-slutpunkt som visar hur toouse Azure Data Catalog för upptäckt av datakälla.  
