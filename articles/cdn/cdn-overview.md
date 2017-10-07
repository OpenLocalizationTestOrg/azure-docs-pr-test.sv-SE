---
title: "aaaAzure CDN-översikt | Microsoft Docs"
description: "Lär dig vilka hello Azure innehåll innehållsleveransnätverk (CDN) är och hur toouse den toodeliver hög bandbredd innehåll genom cachelagring av blobbar och statiskt innehåll."
services: cdn
documentationcenter: 
author: smcevoy
manager: akucer
editor: 
ms.assetid: 866e0c30-1f33-43a5-91f0-d22f033b16c6
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/08/2017
ms.author: v-semcev
ms.openlocfilehash: e0230a6e107969b845985f2f4d357bf93cd40d42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-hello-azure-content-delivery-network-cdn"></a>Översikt över hello Azure Content Delivery Network (CDN)
> [!NOTE]
> Det här dokumentet beskriver vilka hello Azure Content Delivery Network (CDN) är hur det fungerar och hello funktioner i varje Azure CDN-produkten.  Om du vill att tooskip informationen och gå raka tooa självstudier om hur toocreate en CDN-slutpunkt finns [med hjälp av Azure CDN](cdn-create-new-endpoint.md).  Om du vill toosee en lista över aktuella CDN-nodplatser [Azure CDN POP platser](cdn-pop-locations.md).
> 
> 

hello Azure innehåll innehållsleveransnätverk (CDN) cachelagrar statiska webbinnehåll på strategiskt monterade platser tooprovide maximalt dataflöde för att leverera innehåll toousers.  hello CDN erbjuder utvecklare en global lösning för att leverera innehåll med hög bandbredd genom cachelagring av över hello world hello innehållet på fysiska noder. 

hello fördelarna med att använda hello CDN toocache Webbplatstillgångar är:

* Bättre prestanda och användaren upplevelse för slutanvändare, särskilt när du använder program där flera turer är krävs tooload innehåll.
* Hög skalning toobetter hantera omedelbar hög belastning som Starta händelsen hello början av en produkt.
* Genom att distribuera användarförfrågningar och betjänar innehåll från kant servrar, skickas mindre trafik toohello ursprung.

## <a name="how-it-works"></a>Hur det fungerar
![Översikt över CDN](./media/cdn-overview/cdn-overview.png)

1. En användare (Alice) begär en fil (även kallad tillgång eller resurs) med hjälp av en URL med ett särskilt domännamn, t.ex. `<endpointname>.azureedge.net`.  DNS dirigerar hello begäran toohello bäst prestanda punkt förekomst (POP) plats.  Detta är vanligtvis hello POP som geografiskt närmaste toohello-användare.
2. Om hello edge servrar i hello POP inte har hello-filen i sin cache, begär hello gränsservern hello-fil från hello ursprung.  hello ursprung kan vara en Webbapp i Azure, Azure Cloud Service, Azure Storage-konto eller valfri offentligt tillgänglig webbserver.
3. hello ursprung returnerar hello toohello kant filserver, inklusive valfria HTTP-huvuden som beskriver hello filen Time-to-Live (TTL).
4. Hej gränsservern cachelagrar hello-filen och returnerar hello filen toohello ursprungliga begärande (Alice).  hello filen förblir cachelagrade på gränsservern hello tills hello TTL upphör att gälla.  Om hello ursprung inte ange ett TTL-värde, hello standard TTL-värdet är sju dagar.
5. Ytterligare användare kan sedan begäran hello samma fil med samma URL: en och kan också vara riktad toothat samma POP.
6. Om hello TTL-värde för hello-filen inte har upphört att gälla, returnerar hello gränsservern hello filen från hello cache.  Detta resulterar i en snabbare och mer responsiv användarupplevelse.

## <a name="azure-cdn-features"></a>Funktioner i Azure CDN
Det finns tre Azure CDN-produkter:  **Azure CDN Standard från Akamai**, **Azure CDN Standard från Verizon** och **Azure CDN Premium från Verizon**.  hello visar följande tabell hello-funktioner som är tillgängliga med varje produkt.

|  | Standard Akamai | Standard Verizon | Premium Verizon |
| --- | --- | --- | --- |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; __Prestanda och optimering__ |
| [Acceleration av dynamisk webbplats](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration) | **&#x2713;**  | **&#x2713;** | **&#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Acceleration av dynamisk webbplats – adaptiv bildkomprimering](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#adaptive-image-compression-akamai-only) | **&#x2713;**  |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Acceleration av dynamisk webbplats – förhämtning av objekt](https://docs.microsoft.com/azure/cdn/cdn-dynamic-site-acceleration#object-prefetch-akamai-only) | **&#x2713;**  |  |  |
| [Optimering av videoströmning](https://docs.microsoft.com/azure/cdn/cdn-media-streaming-optimization) | **&#x2713;**  | \* |  \* |
| [Optimering av stora filer](https://docs.microsoft.com/azure/cdn/cdn-large-file-optimization) | **&#x2713;**  | \* |  \* |
| [Global serverbelastningsutjämning (GSLB)](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-load-balancing-azure) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Snabbrensning](cdn-purge-endpoint.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Inläsning av tillgångar i förväg](cdn-preload-endpoint.md) | |**&#x2713;** |**&#x2713;** |
| [Cachelagring av frågesträngar](cdn-query-string.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| Dual stack-IPv4/IPv6 |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Stöd för HTTP/2](cdn-http2.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; __Säkerhet__ |
| Stöd för HTTPS med CDN-slutpunkt |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Egen domän-HTTPS](cdn-custom-ssl.md) | |**&#x2713;** |**&#x2713;** |
| [Stöd för anpassade domännamn](cdn-map-content-to-custom-domain.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Geo-filtrering](cdn-restrict-access-by-country.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Tokenautentisering](cdn-token-auth.md)|  |  |**&#x2713;**| 
| [DDOS-skydd](https://www.us-cert.gov/ncas/tips/ST04-015) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; __Analyser och rapporter__ |
| [Grundläggande analys](cdn-analyze-usage-patterns.md) | **&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Avancerade HTTP-rapporter](cdn-advanced-http-reports.md) | | |**&#x2713;** |
| [Realtidsstatistik](cdn-real-time-stats.md) | | |**&#x2713;** |
| [Realtidsaviseringar](cdn-real-time-alerts.md) | | |**&#x2713;** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; __Användarvänlighet__ |
| Enkel integrering med Azure-tjänster som [Storage](cdn-create-a-storage-account-with-cdn.md), [Cloud Services](cdn-cloud-service-with-cdn.md), [Web Apps](../app-service-web/app-service-web-tutorial-content-delivery-network.md) och [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md) |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| Hantering via [REST API](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](cdn-app-dev-net.md), [Node.js](cdn-app-dev-node.md) eller [PowerShell](cdn-manage-powershell.md). |**&#x2713;** |**&#x2713;** |**&#x2713;** |
| [Anpassningsbar, regelbaserad motor för innehållsleverans](cdn-rules-engine.md) | | |**&#x2713;** |
| Inställningar för cache/huvud (med hjälp av [regelmotor](cdn-rules-engine.md)) | | |**&#x2713;** |
| Omdirigering/omarbetning för URL (med hjälp av [regelmotor](cdn-rules-engine.md)) | | |**&#x2713;** |
| Regler för mobil enhet (med hjälp av [regelmotor](cdn-rules-engine.md)) | | |**&#x2713;** |

\* Verizon har stöd för leverans av stora filer och multimedia direkt via Allmän webbleverans.


> [!TIP]
> Finns det en funktion som du vill att toosee i Azure CDN?  [Lämna feedback](https://feedback.azure.com/forums/169397-cdn)! 
> 
> 

## <a name="next-steps"></a>Nästa steg
tooget igång med CDN, se [med hjälp av Azure CDN](cdn-create-new-endpoint.md).

Om du är en befintlig CDN-kund kan du nu hantera dina CDN-slutpunkter via hello [Microsoft Azure-portalen](https://portal.azure.com) eller med [PowerShell](cdn-manage-powershell.md).

toosee Hej CDN i praktiken, kolla hello [video av våra Build 2016 session](https://azure.microsoft.com/documentation/videos/build-2016-leveraging-the-new-azure-cdn-apis-to-build-wicked-fast-applications/).

Lär dig hur tooautomate Azure CDN med [.NET](cdn-app-dev-net.md) eller [Node.js](cdn-app-dev-node.md).

Information om priser finns i avsnittet om [CDN-priser](https://azure.microsoft.com/pricing/details/cdn/).

