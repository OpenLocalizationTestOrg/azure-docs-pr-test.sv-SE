---
title: "aaaAzure Media Services översikt | Microsoft Docs"
description: "Det här avsnittet ger en översikt över Azure Media Services"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 7a5e9723-c379-446b-b4d6-d0e41bd7d31f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/04/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 81f9f4d9ff75effea30c10fd09449e9d2025f377
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-overview"></a>Översikt över Azure Media Services 

Microsoft Azure Media Services är en utökningsbar molnbaserad plattform som gör att utvecklare toobuild skalbara media hanterings- och program. Media Services baseras på REST API: er som gör att du toosecurely överföra, lagra, koda och paketera video-eller ljudinnehåll för både på begäran och live strömmande leverans toovarious klienter (till exempel TV, datorer och mobila enheter).

Du kan bygga arbetsflöden för slutpunkt till slutpunkt bara med Media Services. Du kan också välja toouse komponenter från tredje part för vissa delar av arbetsflödet. Koda till exempel med hjälp av en kodare från tredje part. Därefter kan du överföra, skydda, paketera och leverera med Media Services.

Du kan välja toostream innehållet live eller leverera innehåll på begäran. hello innehåller också länkar tooother relevanta avsnitt.

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
Du kan visa sökvägar för AMS-utbildning här:

* [Arbetsflöde för AMS-liveuppspelning](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
* [Arbetsflöde för AMS-strömning på begäran](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

## <a name="prerequisites"></a>Krav

toostart med Azure Media Services bör du ha hello följande:

* Ett Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information finns i [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com).
* Ett Azure Media Services-konto. Mer information finns i [Skapa konto](media-services-portal-create-account.md).
* (Valfritt) Konfigurera utvecklingsmiljön. Välj .NET eller REST API för din utvecklingsmiljö. Mer information finns i [Ställa in miljön](media-services-dotnet-how-to-use.md).

    Lär dig också hur för[ansluta programmässigt tooAMS API](media-services-use-aad-auth-to-access-ams-api.md).
* En standard- eller premiumslutpunkt för direktuppspelning med tillståndet Startad.  Mer information finns i [Hantera strömningsslutpunkter](media-services-portal-manage-streaming-endpoints.md).

## <a name="sdks-and-tools"></a>SDK:er och verktyg

toobuild Media Services-lösningar som du kan använda:

* [Media Services REST API](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference)
* En av hello tillgängliga klient-SDK:
    * [Azure Media Services SDK för .NET](https://github.com/Azure/azure-sdk-for-media-services),
    * [Azure SDK för Java](https://github.com/Azure/azure-sdk-for-java),
    * [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php),
    * [Azure Media Services för Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (detta är en icke-Microsoft-version av en Node.js SDK. Den underhålls av en grupp och är inte en 100% täckning för hello AMS APIs).
* Befintliga verktyg:
    * [Azure Portal](https://portal.azure.com/)
    * [Azure-Media Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer (AMSE) är ett Winforms/C#-program för Windows)

## <a name="concepts-and-overview"></a>Koncept och översikt
Azure Media Services-koncepten finns i [Koncept](media-services-concepts.md).

En hur-tooseries som introducerar tooall hello huvudkomponenterna i Azure Media Services, se [Azure Media Services stegvisa självstudier](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series). Den här serien ger en bra översikt över koncepten och använder hello AMSE-verktyget toodemonstrate AMS uppgifter. AMSE-verktyget är ett verktyg i Windows. Det här verktyget stöder de flesta av hello uppgifter som du kan göra programmässigt med [AMS SDK för .NET](https://github.com/Azure/azure-sdk-for-media-services), [Azure SDK för Java](https://github.com/Azure/azure-sdk-for-java), eller [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php).

## <a name="supported-scenarios-and-availability-of-media-services-across-data-centers"></a>Scenarier som stöds och tillgänglighet för Media Services över datacenter

Detaljerad information finns i [AMS-scenarier och tillgänglighet för funktioner och tjänster över datacenter](scenarios-and-availability.md).

## <a name="service-level-agreement-sla"></a>Serviceavtal (SLA)

* Vi garanterar 99,9 % tillgänglighet för REST API-transaktioner för Media Services Encoding.
* För Streaming svarar vi på serviceförfrågningar med 99,9 % tillgänglighet för befintligt medieinnehåll när en Standard- eller Premium-slutpunkt för direktuppspelning har köpts.
* För Live-kanaler garanterar vi att köra kanaler ska ha extern anslutning minst 99,9% av hello tid.
* Content Protection garanterar vi att vi ska uppfyller viktiga förfrågningar minst 99,9% av hello tid.
* Indexer ska vi utföra service för indexeringsuppgifter som bearbetas med en Encoding-reserverad enhet 99,9% av hello tid.

Mer information finns i [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/).

Information om tillgänglighet i datacenter finns hello [Avaiability](scenarios-and-availability.md#availability) avsnitt.

## <a name="support"></a>Support

[Azure-supporten](https://azure.microsoft.com/support/options/) tillhandahåller support för Azure, inklusive Media Services.

## <a name="provide-feedback"></a>Ge feedback

[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
