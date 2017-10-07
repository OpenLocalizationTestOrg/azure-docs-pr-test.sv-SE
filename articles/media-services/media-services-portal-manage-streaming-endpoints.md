---
title: "aaaManage strömningsslutpunkter med hello Azure-portalen | Microsoft Docs"
description: "Det här avsnittet visar hur toomanage strömningsslutpunkter med hello Azure-portalen."
services: media-services
documentationcenter: 
author: Juliako
writer: juliako
manager: cfowler
editor: 
ms.assetid: bb1aca25-d23a-4520-8c45-44ef3ecd5371
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: dfa9352894d37edb317a6334d7f109419deb362b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-streaming-endpoints-with-hello-azure-portal"></a>Hantera strömningsslutpunkter med hello Azure-portalen

Det här avsnittet visar hur toouse hello Azure portal toomanage strömningsslutpunkter. 

>[!NOTE]
>Se till att tooreview hello [översikt](media-services-streaming-endpoints-overview.md) avsnittet. 

Information om hur tooscale hello strömmande slutpunkten finns [detta](media-services-portal-scale-streaming-endpoints.md) avsnittet.

## <a name="start-managing-streaming-endpoints"></a>Börja hantera strömningsslutpunkter 

toostart hantera strömningsslutpunkter för ditt konto hello följande.

1. I hello [Azure-portalen](https://portal.azure.com/), Välj Azure Media Services-konto.
2. I hello **inställningar** bladet väljer **Strömningsslutpunkter**.
   
    ![Strömmande slutpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

> [!NOTE]
> Du debiteras endast när din Strömningsslutpunkt är i körningstillstånd.

## <a name="adddelete-a-streaming-endpoint"></a>Lägg till/ta bort en strömmande slutpunkt

>[!NOTE]
>hello standard strömmande slutpunkten kan inte tas bort.

tooadd/ta bort strömmande slutpunkten med hello Azure-portalen, hello följande:

1. tooadd strömningsslutpunkt, klicka på hello **+ Endpoint** hello överst på hello sidan. 

    Du kanske vill flera Strömningsslutpunkter om du planerar toohave olika CDN-nät eller en CDN och direktåtkomst.

2. toodelete strömningsslutpunkt, tryck på **ta bort** knappen.      
3. Klicka på hello **starta** knappen toostart hello strömmande slutpunkten.
   
    ![Strömmande slutpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)


## <a id="configure_streaming_endpoints"></a>Konfigurera hello Streaming slutpunkt
Strömmande slutpunkten kan du tooconfigure hello följande egenskaper:

* Åtkomstkontroll
* Cache-kontroll
* Mellan åtkomstprinciper för platsen

Detaljerad information om de här egenskaperna finns [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).

Du kan konfigurera strömmande slutpunkt hello följande:

1. Välj hello strömningsslutpunkt som du vill tooconfigure.
2. Klicka på **inställningar**.

En kort beskrivning av hello fält följer.

![Strömmande slutpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)

1. Maximal cachestorlek princip: använda tooconfigure cache-livstid för tillgångar hanteras via den här strömmande slutpunkten. Om inget värde anges används standard-hello. hello standardvärdena kan också definieras direkt i Azure-lagring. Om Azure CDN har aktiverats för hello strömmande slutpunkten, bör du inte ange hello cache princip värdet tooless än 600 sekunder.  
2. Tillåtna IP-adresser: används toospecify IP-adresser som skulle vara tillåten tooconnect toohello publicerade strömmande slutpunkt. Om inga angivna IP-adresser är IP-adresser kan tooconnect. IP-adresser kan anges som en IP-adress (t.ex, 10.0.0.1), en IP-intervall med IP-adress och nätmask CIDR (till exempel 10.0.0.1/22) eller en IP-intervall med IP-adress och en prickad decimal nätmask (till exempel ”10.0.0.1 () 255.255.255.0)').
3. Konfiguration för autentisering med Akamai signatur-huvud: används toospecify hur signatur sidhuvud autentiseringsbegäran från Akamai servrar har konfigurerats. Förfallodatum är i UTC.

## <a name="scale-your-premium-streaming-endpoint"></a>Skala din Premium strömmande slutpunkten

Mer information finns i [detta](media-services-portal-scale-streaming-endpoints.md) avsnitt.

## <a id="enable_cdn"></a>Aktivera Azure CDN-integrering

När du skapar ett nytt konto är standard strömmande slutpunkten Azure CDN-integrering aktiverad som standard.

Om du senare vill toodisable/aktivera hello CDN strömmande slutpunkten måste vara i hello **stoppats** tillstånd. Det kan ta upp too2 timmar för hello Azure CDN integration tooget aktiverad och hello ändringar toobe active över alla hello CDN POP. Dock kan starta strömmande slutpunkt och dataströmmen utan avbrott från hello strömmande slutpunkten och när hello integrationen är slutförd, hello dataströmmen levereras från hello CDN. Under hello etablering period inte din strömmande slutpunkt i **startar** tillstånd och du kan se degredad prestanda.

CDN-integrering är aktiverat i alla hello Azure data Datacenter execpt Kina och Federal Government regioner.

När den har aktiverats hello **åtkomstkontroll**, **anpassad hostname** och **autentisering med Akamai signatur** konfigurationen hämtar inaktiverad.
 
> [!IMPORTANT]
> Azure Media Services-integrering med Azure CDN implementeras på **Azure CDN från Verizon** standard strömningsslutpunkter. Premium strömningsslutpunkter kan konfigureras med hjälp av alla **Azure CDN priser nivåer och providers**. Mer information om Azure CDN funktioner finns hello [CDN-översikt](../cdn/cdn-overview.md).
 
### <a name="additional-considerations"></a>Annat som är bra att tänka på

* När CDN är aktiverad för en strömmande slutpunkten, det går inte att klienter begär innehåll direkt från hello ursprung. Om du behöver hello möjlighet tootest ditt innehåll med eller utan CDN kan skapa du en annan strömningsslutpunkt som inte är aktiverat CDN.
* Din strömmande slutpunkten hostname förblir hello samma när du har aktiverat CDN. Du behöver inte toomake eventuella ändringar tooyour media services-arbetsflödet efter CDN har aktiverats. Om din strömmande slutpunktens värdnamn är strasbourg.streaming.mediaservices.windows.net, efter att aktivera CDN, används till exempel hello exakt samma värdnamn.
* För nya strömningsslutpunkter kan du aktivera CDN genom att skapa en ny slutpunkt; Du måste toofirst stoppa hello slutpunkt och sedan aktivera/inaktivera hello CDN för befintliga strömningsslutpunkter.
* Standard strömmande slutpunkten kan bara konfigureras med hjälp av **Verizon Standard CDN-providern** med hjälp av Azure-hanteringsportalen. Du kan dock aktivera andra leverantörer av Azure CDN med hjälp av REST API: er.

## <a name="configure-cdn-profile"></a>Konfigurera CDN-profilen

Du kan konfigurera hello CDN-profilen genom att välja hello **hantera CDN** knappen hello uppifrån.

![Strömmande slutpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints6.png)

## <a name="next-steps"></a>Nästa steg
Granska sökvägarna för Media Services-utbildning.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

