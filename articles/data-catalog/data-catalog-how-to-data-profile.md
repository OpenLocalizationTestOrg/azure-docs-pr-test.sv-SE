---
title: "aaaHow tooData profil datakällor"
description: "Hur-tooarticle syntaxmarkering hur tooinclude tabell - och på kolumnnivå data profiler när du registrerar datakällor i Azure Data Catalog och hur data toouse profiler toounderstand datakällor."
services: data-catalog
documentationcenter: 
author: spelluru
manager: NA
editor: 
tags: 
ms.assetid: 94a8274b-5c9c-4962-a4b1-2fed38a3d919
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/03/2017
ms.author: spelluru
ms.openlocfilehash: 12c9f38501cdaee903d0dcbbdd0b82395f35a187
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-profile-data-sources"></a>Datakällor för dataprofil
## <a name="introduction"></a>Introduktion
**Microsoft Azure Data Catalog** är en helt hanterad molntjänst som fungerar som ett system för registrering och identifieringssystem för företagets datakällor. Med andra ord **Azure Data Catalog** är allt om hjälpa personer identifiera, förstå och använda datakällor och hjälpa organisationer tooget mer värde från deras befintliga data. När en datakälla har registrerats med **Azure Data Catalog**, dess metadata kopieras och indexeras av hello service, men hello artikeln inte det avslutas.

Hej **Data profilering** funktion i **Azure Data Catalog** undersöker hello data från datakällor som stöds i katalogen och samlar in information om dessa data och statistik. Det är enkelt tooinclude en profil för dina datatillgångar. När du registrerar en datatillgång, välja **inkludera Data profil** i hello registreringsverktyget för datakällor.

## <a name="what-is-data-profiling"></a>Vad är profilering av Data
Data profilering undersöker hello data i hello datakällan som registreras och samlar in information om dessa data och statistik. Datakällsidentifiering, kan statistiken hjälpa dig att avgöra hello lämplighet hello data toosolve sina affärsproblem.

<!-- In [How toodiscover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How tooinclude a data profile when registering a data source](#howto). -->

hello stöder följande datakällor profilering av data:

* SQL Server (inklusive Azure SQL DB och Azure SQL Data Warehouse) tabeller och vyer
* Oracle-tabeller och vyer
* Teradata-tabeller och vyer
* Hive-tabeller

Inklusive profiler data när du registrerar datatillgångar hjälper användarna att besvara frågor om datakällor, inklusive:

* Kan det vara används toosolve mitt företag problem?
* Överensstämmer hello data tooparticular standarder eller mönster?
* Vilka är några av hello avvikelser i hello-datakällan?
* Vad är möjliga utmaningarna med att integrera informationen i mitt program?

> [!NOTE]
> Du kan också lägga till dokumentationen tooan tillgången toodescribe hur data kan integreras i ett program. Se [hur toodocument datakällor](data-catalog-how-to-documentation.md).
>
>

<a name="howto"/>

## <a name="how-tooinclude-a-data-profile-when-registering-a-data-source"></a>Hur tooinclude data profilen när du registrerar en datakälla
Det är enkelt tooinclude en profil av datakällan. När du registrerar en datakälla i hello **objekt toobe registrerade** panelen av hello registrering av datakälla verktyget, välja **inkludera Data profil**.

![](media/data-catalog-data-profile/data-catalog-register-profile.png)

toolearn mer om hur tooregister datakällor, se [hur tooregister datakällor](data-catalog-how-to-register.md) och [Kom igång med Azure Data Catalog](data-catalog-get-started.md).

## <a name="filtering-on-data-assets-that-include-data-profiles"></a>Filtrering på datatillgångar som innehåller data profiler
toodiscover datatillgångar som innehåller en data-profil, som du kan inkludera `has:tableDataProfiles` eller `has:columnsDataProfiles` som en av dina sökvillkor.

> [!NOTE]
> Att välja **inkludera Data profil** i hello data registreringsverktyget för datakällor innehåller både tabell och på kolumnnivå profilinformation. Hello kan Data Catalog-API: et dock data tillgångar toobe registrerats med bara en uppsättning profilinformation som ingår.
>
>

## <a name="viewing-data-profile-information"></a>Visa information om profilen
När du har hittat en lämplig datakälla med en profil kan visa du hello information om data. tooview hello data profil, Välj en datatillgång och välj **Data profil** i portalen hello Data Catalog-fönstret.

![](media/data-catalog-data-profile/data-catalog-view.png)

En data-profil i **Azure Data Catalog** visar tabellen och kolumnen profil information, bland annat:

### <a name="object-data-profile"></a>Objektet data profil
* Antal rader
* Tabellens storlek
* När hello objekt uppdaterades senast

### <a name="column-data-profile"></a>Kolumnen data profil
* Kolumnens datatyp
* Antalet distinkta värden
* Antal rader med NULL-värden
* Lägsta, högsta, medelvärde och standardavvikelsen för kolumnvärden

## <a name="summary"></a>Sammanfattning
Profilering av data innehåller statistik och information om registrerade data tillgångar toohelp du fastställa hello lämplighet hello data toosolve affärsproblem. Tillsammans med kommentera och dokumentera datakällor, kan data-profiler ge användare en bättre förståelse av dina data.

## <a name="see-also"></a>Se även
* [Hur tooregister datakällor](data-catalog-how-to-register.md)
* [Kom igång med Azure Data Catalog](data-catalog-get-started.md)
