---
title: "aaaAzure Strömningsslutpunkt för Media Services översikt | Microsoft Docs"
description: "Det här avsnittet ger en översikt över Azure Media Services strömningsslutpunkter."
services: media-services
documentationcenter: 
author: Juliako
writer: juliako
manager: cfowler
editor: 
ms.assetid: 097ab5e5-24e1-4e8e-b112-be74172c2701
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: f27f590175dcc945d1d3299fc0cae5a68cfbf4e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="streaming-endpoints-overview"></a>Översikt över strömmande slutpunkter 

##<a name="overview"></a>Översikt

I Microsoft Azure Media Services (AMS), en **Strömningsslutpunkt** representerar en strömmande tjänst som kan leverera innehåll direkt tooa player klientprogrammet eller tooa innehåll innehållsleveransnätverk (CDN) för vidare distribution. Media Services tillhandahåller också sömlös integration av Azure CDN. hello utgående ström från en StreamingEndpoint-tjänst kan vara en direktsänd dataström, en video på begäran eller progressiv hämtning av dina tillgångar i Media Services-kontot. Varje Azure Media Services-konto innehåller standard StreamingEndpoint. Du kan skapa ytterligare Strömningsslutpunkter hello kontot. Det finns två versioner av Strömningsslutpunkter 1.0 och 2.0. Från och med januari 10 2017 alla konton som nyligen skapade AMS innehåller version 2.0 **standard** StreamingEndpoint. Ytterligare strömningsslutpunkter som du lägger till toothis kontot också vara version 2.0. Den här ändringen påverkar inte befintliga konton för hello; befintliga Strömningsslutpunkter blir version 1.0 och kan vara uppgraderade tooversion 2.0. Med den här ändringen kommer det ske beteende, fakturering och funktionen förändringar (Mer information finns i hello **strömning typer och versioner** avsnitt beskrivs nedan).

Från och med hello 2.15 version (som publicerades i januari 2017), Azure Media Services dessutom lagts hello följande egenskaper toohello Strömningsslutpunkt entitet: **CdnProvider**, **CdnProfile**,  **FreeTrialEndTime**, **StreamingEndpointVersion**. Detaljerad översikt över de här egenskaperna finns [detta](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). 

När du skapar ett Azure Media Services-konto ett standardvärde som standard strömmande slutpunkt skapas i hello **stoppad** tillstånd. Du kan inte ta bort hello standard strömmande slutpunkten. Beroende på hello Azure CDN tillgänglighet i hello riktade region, som standard nyskapad standard strömningsslutpunkt även ”StandardVerizon” CDN provider-integration. 

>[!NOTE]
>Azure CDN-integrering kan inaktiveras innan du startar hello strömmande slutpunkten.

Det här avsnittet ger en översikt över hello huvudsakliga funktionerna som tillhandahålls av strömningsslutpunkter.

## <a name="streaming-types-and-versions"></a>Strömning typer och versioner

### <a name="standardpremium-types-version-20"></a>Standard/Premium typer (version 2.0)

Från och med hello januari 2017 versionen av Media Services kan du ha två strömmande typer: **Standard** och **Premium**. Dessa typer är en del av hello Streaming endpoint version ”2.0”.

Typ|Beskrivning
---|---
**Standard**|Det här är standardalternativet för hello som skulle fungera för hello merparten av hello scenarier.<br/>Med det här alternativet får du fast/begränsad SLA, första 15 dagar efter att du startar hello strömmande slutpunkten är ledig.<br/>Om du skapar fler än en strömningsslutpunkter endast är hello först en gratis för hello första 15 dagar hello andra debiteras när du startar. <br/>Observera att kostnadsfria utvärderingsversionen bara gäller toonewly skapat media services-konton och standard strömmande slutpunkten. Befintliga strömningsslutpunkter och dessutom skapade strömningsslutpunkter inte innehåller kostnadsfri utvärderingsversion period även de uppgraderade tooversion 2.0 eller de skapas som version 2.0.
**Premium**|Det här alternativet är lämpligt för professional scenarier som kräver högre skala eller kontroll.<br/>Variabeln SLA som är baserad på premium strömmande enhet (SU) kapacitet köpt dedikerade strömningsslutpunkter live i isolerad miljö och konkurrerar inte om resurser.

Mer information finns i hello **jämför strömning typer** efter avsnittet.

### <a name="classic-type-version-10"></a>Klassisk typ (version 1.0)

För användare som skapade AMS konton tidigare toohello januari 10 2017 versionen som du har en **klassiska** typ av en strömmande slutpunkt. Den här typen är en del av hello strömmande slutpunkten version ”1.0”.

Om din **version ”1.0”** strömningsslutpunkt har > = 1 premium strömningsenheter (SU), den kommer att vara premium strömmande slutpunkt och ger alla AMS-funktioner (precis som hello **Standard/Premium** typ) utan några ytterligare konfigurationssteg.

>[!NOTE]
>**Klassiska** strömningsslutpunkter (version ”1.0” och 0 SU), tillhandahåller begränsade funktioner och inte innehåller ett SERVICENIVÅAVTAL. Det rekommenderas toomigrate för**Standard** skriver en bättre upplevelse och toouse funktioner som dynamisk paketering eller kryptering och andra funktioner som medföljer hello tooget **Standard** typen. toomigrate toohello **Standard** skriver finns toohello [Azure-portalen](https://portal.azure.com/) och välj **Opt-in tooStandard**. Mer information om migrering finns hello [migrering](#migration-between-types) avsnitt.
>
>Tänk på att den här åtgärden inte kan återställas och påverkar priset.
>
 
## <a name="comparing-streaming-types"></a>Jämföra strömmande typer

### <a name="versions"></a>Versioner

|Typ|StreamingEndpointVersion|ScaleUnits|CDN|Fakturering|SLA| 
|--------------|----------|-----------------|-----------------|-----------------|-----------------|    
|Klassisk|1.0|0|Ej tillämpligt|Kostnadsfri|Ej tillämpligt|
|Standard-slutpunkt för direktuppspelning|2.0|0|Ja|Avgiftsbelagt|Ja|
|Premium-enheter för direktuppspelning|1.0|>0|Ja|Avgiftsbelagt|Ja|
|Premium-enheter för direktuppspelning|2.0|>0|Ja|Avgiftsbelagt|Ja|

### <a name="features"></a>Funktioner

Funktion|Standard|Premium
---|---|---
Ledigt första 15 dagar| Ja |Nej
Dataflöde |Konfigurera too600 Mbps när Azure CDN inte används. Skalas med CDN.|200 Mbit/s per streaming unit (SU). Skalas med CDN.
SLA | 99.9|99,9 (200 Mbit/s per SU).
CDN|Azure CDN tredjeparts CDN eller inga CDN.|Azure CDN tredjeparts CDN eller inga CDN.
Fakturering är linjärt| Dagligen|Dagligen
Dynamisk kryptering|Ja|Ja
Dynamisk paketering|Ja|Ja
Skala|Automatisk skalas upp toohello riktade dataflöde.|Ytterligare enheter för strömning
IP-filtrering/G20/Custom värd|Ja|Ja
Progressiv hämtning|Ja|Ja
Rekommenderade användning |Vi rekommenderar för hello majoriteten av strömning scenarier.|Professional användning.<br/>Om du tror att kan du ha behov utöver Standard. Kontakta oss (amsstreaming på microsoft.com) om du tror att en samtidiga målgruppen storlek är större än 50 000 användare.


## <a name="migration-between-types"></a>Migrering mellan typer

Från | för| Åtgärd
---|---|---
Klassisk|Standard|Behovet av tooopt i
Klassisk|Premium| Skala (ytterligare enheter för strömning)
Standard/Premium|Klassisk|Inte tillgänglig (om den strömmande slutpunkten version är 1.0. Det är tillåtet toochange tooclassic med att ange scaleunits för ”0”)
Standard (med eller utan ett CDN)|Premium med hello samma konfigurationer|Tillåtet i hello **igång** tillstånd. (via Azure portal)
Premium (med eller utan ett CDN)|Standard med hello samma konfigurationer|Tillåtet i hello **igång** tillstånd (via Azure portal)
Standard (med eller utan ett CDN)|Premium med olika konfig|Tillåtet i hello **stoppats** tillstånd (via Azure portal). Inte tillåtet i hello körs.
Premium (med eller utan ett CDN)|Standard med olika konfig|Tillåtet i hello **stoppats** tillstånd (via Azure portal). Inte tillåtet i hello körs.
Version 1.0 med SU > = 1 CDN|Standard/Premium inga CDN|Tillåtet i hello **stoppats** tillstånd. Tillåts inte i hello **igång** tillstånd.
Version 1.0 med SU > = 1 CDN|Standard med eller utan ett CDN|Tillåtet i hello **stoppats** tillstånd. Tillåts inte i hello **igång** tillstånd. Version 1.0 CDN kommer att tas bort och nya som skapas och igång.
Version 1.0 med SU > = 1 CDN|Premium med eller utan ett CDN|Tillåtet i hello **stoppats** tillstånd. Tillåts inte i hello **igång** tillstånd. Klassiska CDN kommer att tas bort och nya som skapas och igång.

## <a name="next-steps"></a>Nästa steg
Granska sökvägarna för Media Services-utbildning.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

