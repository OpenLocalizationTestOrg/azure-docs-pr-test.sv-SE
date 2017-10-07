---
title: aaaAzure Mobile Engagement Android SDK-Integration
description: "Senaste uppdateringarna och procedurer för Android SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 585341f9-3f39-459a-af42-864e400a0128
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 16b098198674c49567d720d0c01d984cb763ed8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="release-notes"></a>Viktig information

## <a name="431-07172017"></a>4.3.1 (07/17/2017)
* Åtgärda en krasch sällan kan inträffa när du anropar `EngagementAgentUtils.isInDedicatedEngagementProcess`, som också används av hello `EngagementApplication` klass.

## <a name="430-06272017"></a>4.3.0 (06/27/2017)
* 8 stöd för Android (tidigare versioner av hello SDK inte fungerar på Android 8).
* Inget mer beroende stödbibliotek.
* Ta bort `EngagementFragmentActivity` klass.
* Förfallodatum för[bakgrund körning gränser](https://developer.android.com/preview/features/background.html) på Android 8 loggar i bakgrunden kan fördröjas tills hello användaren interagerar med hello enhet, detta kan påverka Push-kampanj **levererade** och **Systemmeddelande visas** statistik att försenas om hello enheten viloläge (hello-meddelande fortfarande visas, kommer ring och vibrerar i realtid utan problem).
* Förfallodatum för[bakgrund plats gränser](https://developer.android.com/preview/features/background-location-limits.html), hello realtid platsen i bakgrunden kommer inte att uppdateras ofta på Android 8.

## <a name="424-03302017"></a>4.2.4 (03/30/2017)
* Åtgärda meddelande i appen färger på Android 7 toobe hello samma som äldre versioner av Android.

## <a name="423-08102016"></a>4.2.3 (08/10/2016)
* Ingen mer WIFI-Lås.
* Åtgärda ett dödläge vid anrop av getDeviceId innan init (bugg introducerades i 4.2.0).

## <a name="422-05172016"></a>4.2.2 (05/17/2016)
* Stabilitetsförbättringar.

## <a name="421-05102016"></a>4.2.1 (05/10/2016)
* Säkerhet: inaktivera webbåtkomst visa lokal fil.
* Säkerhet: ta bort `EngagementPreferenceActivity` klass som utökar föråldrat och osäkra `PreferenceActivity` klass.
* Säkerhet: reach aktiviteter är nu dokumenterat toouse `exported="false"`, den här flaggan kan också användas i tidigare versioner av SDK.

## <a name="420-03112016"></a>4.2.0 (03/11/2016)
* hello SDK är nu licensierad under MIT.
* Tillåt att ange en anpassad enhetsidentifierare till Initieringstid SDK.

## <a name="415-02012016"></a>4.1.5 (02/01/2016)
* Stabilitetsförbättringar.

## <a name="414-01262016"></a>4.1.4 (01/26/2016)
* Stabilitetsförbättringar.

## <a name="413-1292015"></a>4.1.3 (12/9/2015)
* Stabilitetsförbättringar.

## <a name="412-11252015"></a>4.1.2 (11/25/2015)
* Stabilitetsförbättringar.

## <a name="411-11042015"></a>4.1.1 (11/04/2015)
* Stabilitetsförbättringar.

## <a name="410-08252015"></a>4.1.0 (08/25/2015)
* Hantera nya behörighetsmodellen för Android M.
* Kan nu konfigurera funktioner för plats vid körning i stället för `AndroidManifest.xml`.
* Åtgärda ett programfel behörighet: Om du använder `ACCESS_FINE_LOCATION`, sedan `ACCESS_COARSE_LOCATION` behövs inte längre.
* Stabilitetsförbättringar.

## <a name="400-07062015"></a>4.0.0 (07/06/2015)
* Interna protokollet ändras toomake analyser och push mer tillförlitlig.
* Intern push (GCM/ADM) nu används också för i appen meddelanden så att du måste konfigurera hello native push-autentiseringsuppgifter för alla typer av push-kampanj.
* Åtgärda helheten meddelande: de var 10-endast tal visas efter att pushas.
* Åtgärda ett programfel i webbvyn: Klicka på en länk kördes också hello standard åtgärds-URL.
* Åtgärda en sällsynt krasch relaterade toolocal lagringshantering.
* Åtgärda dynamisk sträng konfigurationshantering.
* Uppdatera LICENSAVTALET.

## <a name="300-02172015"></a>3.0.0 (02/17/2015)
* Första versionen av Azure Mobile Engagement
* appId configuration ersätts av en sträng anslutningskonfiguration.
* Ta bort API toosend och ta emot godtycklig XMPP meddelanden från valfri XMPP entiteter.
* Ta bort API toosend och ta emot meddelanden mellan enheter.
* Förbättringar av säkerhet.
* Spårning av Google Play och SmartAd tas bort.

