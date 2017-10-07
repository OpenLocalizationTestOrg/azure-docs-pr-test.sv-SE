---
title: aaaManage Azure CDN cachelagringsprincipen i Azure Media Services | Microsoft Docs
description: "Lär dig hur toomanage Azure CDN cachelagringsprincipen i Azure Media Services."
services: media-services,cdn
documentationcenter: .NET
author: juliako
manager: erikre
editor: 
ms.assetid: be33aecc-6dbe-43d7-a056-10ba911e0e94
ms.service: media-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/04/2017
ms.author: juliako
ms.openlocfilehash: 4c7ee2922fcbb5727e10fbe44fc6e61b79f6917c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-cdn-caching-policy-in-azure-media-services"></a>Hantera Azure CDN cachelagring principen i Azure Media Services
Azure Media Services innehåller HTTP-baserade anpassningsbar strömning och progressiv hämtning. HTTP-baserade direktuppspelning är mycket skalbart med fördelarna med cachelagring i proxy- och CDN lager samt cachelagring på klientsidan. Strömningsslutpunkter innehåller allmänna strömmande funktioner och konfiguration för cache-HTTP-huvuden. Strömningsslutpunkter anger HTTP Cache-Control: maximal ålder och Expires-huvuden. Du kan få mer information om HTTP-huvuden cache från [W3.org](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).

## <a name="default-caching-headers"></a>Standard cachelagring rubriker
Som standard gäller strömmande slutpunkter 3 dag cache-huvuden för strömning på begäran-data (faktiska media fragment/segment) och manifest(playlist). För direktsänd strömning strömningsslutpunkter gäller 3 dag cache-huvuden för data (faktiska media fragment/segment) och 2 sekunder cache-huvud för manifest(playlist) begäranden. När levande programmet stängs tooon-begäran (live Arkiv) tillämpas på begäran strömmande cache-huvuden.

## <a name="azure-cdn-integration"></a>Azure CDN-integrering
Azure Media Services innehåller [integrerad CDN](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) för strömmande slutpunkter. Cache-control huvuden gäller i hello samma sätt som strömmande slutpunkter tooCDN aktiverat strömningsslutpunkter. Azure CDN använder strömmande slutpunkt som konfigurerats cache värden toodefine hello livslängden för hello internt cachelagrade objekt och även använder det här värdet tooset hello leverans cache-huvuden. När CDN aktiverat strömningsslutpunkter rekommenderas inte tooset små cache-värden. Små inställningsvärden försämra hello prestandan och minska hello fördelen CDN. Det är inte tillåtet tooset cache-huvuden som är mindre än 600 sekunder för CDN aktiverat strömningsslutpunkter.

> [!IMPORTANT]
>Azure Media Services har fullständig integrering med Azure CDN. Du kan integrera alla hello tillgängliga Azure CDN-providers (Akamai och Verizon) tooyour strömningsslutpunkt inklusive CDN Standard och Premium produkter med en enda klickning. Mer information finns i det här [meddelande](https://azure.microsoft.com/blog/standardstreamingendpoint/).
> 
> Avgifter för data från strömmande slutpunkt tooCDN hämtar endast inaktiverade om hello CDN aktiveras via strömningsslutpunkt API: er eller med Azure-hanteringsportalen strömmande slutpunkt. Manuell integrering eller direkt skapa en CDN-slutpunkten med hjälp av CDN-API: er eller portal avsnittet inaktiveras inte hello data avgifter.

## <a name="configuring-cache-headers-with-azure-media-services"></a>Konfigurera cache-huvuden med Azure Media Services
Du kan använda Azure Management portal eller huvudvärden för Azure Media Services API: erna tooconfigure cache.

1. tooconfigure cache-huvuden med management portal finns för[hur tooManage Strömningsslutpunkter](../media-services/media-services-portal-manage-streaming-endpoints.md) avsnittet Konfigurera hello Streaming slutpunkt.
2. Azure Media Services REST API [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx#StreamingEndpointCacheControl).
3. Azure Media Services .NET SDK [StreamingEndpointCacheControl egenskaper](http://go.microsoft.com/fwlink/?LinkId=615302).

## <a name="cache-configuration-precedence-order"></a>Prioritetsordningen för cache-konfiguration
1. Azure Media Services konfigurerats cache värdet åsidosätter standardvärdet.
2. Om det finns ingen manuell konfiguration, används standardvärden.
3. 2 sekunder cache huvuden gäller toolive strömning manifest(playlist) oavsett Azure Media eller Azure Storage-konfiguration och åsidosättning av det här värdet är inte tillgängligt som standard.

