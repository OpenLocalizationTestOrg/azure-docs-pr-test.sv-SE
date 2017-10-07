---
title: "aaaUsing hello Azure CDN tooaccess blobbar med anpassade domäner via HTTPS"
description: "Lär dig hur toointegrate hello Azure CDN med blob storage tooaccess BLOB-objekt med anpassade domäner via HTTPS"
services: storage
documentationcenter: 
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: mihauss
ms.openlocfilehash: 678e24a7dde5cb2f8feea177bb47c92f61035e66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cdn-tooaccess-blobs-with-custom-domains-over-https"></a>Med anpassade domäner via HTTPS hello Azure CDN tooaccess blobbar

Azure Content Delivery Network (CDN) har nu stöd för HTTPS för anpassade domännamn.
Du kan använda den här funktionen tooaccess storage-blobbar med hjälp av den anpassade domänen via HTTPS. toodo så behöver du tooenable Azure CDN på ditt blob-slutpunkten och mappa hello CDN tooa domännamn. När du har gjort är aktivera HTTPS för den anpassade domänen förenklad via en enda klickning aktivering, fullständig certifikathantering och med utan extra kostnad toonormal CDN-prisnivå.

Den här möjligheten är viktigt eftersom det låter du tooprotect hello sekretess och dataintegritet känsliga web application data under överföringen. Hello SSL-protokollet tooserve-trafik via HTTPS garanterar att data krypteras när de skickas över hello internet. HTTPS ger förtroende och autentisering och skyddar ditt webbprogram från attacker.

> [!NOTE]
> Dessutom kan tooproviding SSL-stöd för anpassade domännamn, hello Azure CDN hjälp du skala ditt programinnehåll toodeliver hög bandbredd hello världen.
> toolearn fler kolla [översikt över hello Azure CDN](../cdn/cdn-overview.md).
>
>

## <a name="quick-start"></a>Snabbstart

Dessa är hello steg krävs tooenable HTTPS för anpassade blob storage slutpunkten:

1.  [Integrera Azure storage-konto med Azure CDN](../cdn/cdn-create-a-storage-account-with-cdn.md).
    Den här artikeln vägleder dig genom att skapa ett lagringskonto i hello Azure Portal om du inte har gjort det redan.
2.  [Anpassad domän för kartan Azure CDN innehåll tooa](../cdn/cdn-map-content-to-custom-domain.md).
3.  [Aktivera HTTPS på en anpassad domän för Azure CDN](../cdn/cdn-custom-ssl.md).

## <a name="shared-access-signatures"></a>Signaturer för delad åtkomst

Om din slutpunkt för blob storage är konfigurerade toodisallow anonym läsbehörighet, behöver du tooprovide en [delade signatur åtkomst (SAS)](storage-dotnet-shared-access-signature-part-1.md) token för varje begäran du tooyour anpassade domäner. Som standard inte blob storage slutpunkter tillåta anonym läsbehörighet. Se [Hantera anonym läsbehörighet till behållare och blobbar](storage-manage-access-to-resources.md) mer information om signaturer för delad åtkomst.

Azure CDN gäller inte några begränsningar tillagda toohello SAS-token. Alla SAS-token har till exempel en förfallotid. Detta innebär att innehåll kan fortfarande nås med ett utgånget SAS tills innehållet tas bort från hello CDN edge noder. Du kan styra hur länge data cachelagras på hello CDN genom att ange hello cache-Svarsrubrik. Se [hantera förfallodatum för Azure Storage-blobbar i Azure CDN](../cdn/cdn-manage-expiration-of-blob-content.md) anvisningar.

Om du skapar flera SAS-URL: er för hello samma slutpunkt för blob, rekommenderar vi aktivera cachelagring av frågan frågesträngar för Azure CDN. Detta är tooensure varje URL behandlas som en unik entitet. Se [styra Azure CDN cachelagring av frågesträngar med frågesträngar](../cdn/cdn-query-string.md) för mer information.

## <a name="http-toohttps-redirection"></a>TooHTTPS HTTP-omdirigering

Du kan välja tooredirect HTTP-trafik tooHTTPS. Detta kräver användning av hello Azure CDN premium erbjudande från Verizon. Du behöver för[åsidosätta HTTP beteende med hjälp av Azure CDN regelmotor](../cdn/cdn-rules-engine.md) med följande regel:

![](./media/storage-https-custom-domain-cdn/redirect-to-https.png)

”Cdn-slutpunkt-name” refererar toohello namn som du har konfigurerat för CDN-slutpunkten. Du kan välja det här värdet hello listrutan. ”Ursprungssökväg” syftar på hello sökväg i ditt ursprung storage-konto där din statiskt innehåll finns.
Om du är värd för alla statiska innehållet i en enskild behållare, ersätter du ”ursprungssökväg” med hello namnet för behållaren.

En mer grundlig genomgång i regler finns hello [Azure CDN regler motorn funktioner](../cdn/cdn-rules-engine-reference-features.md).

## <a name="pricing-and-billing"></a>Priser och fakturering

När du har åtkomst till blobbar via en Azure CDN du betalar [Blob storage-priser](https://azure.microsoft.com/pricing/details/storage/blobs/) för trafik mellan hello edge noder och hello ursprung (Blob storage) och [CDN priser](https://azure.microsoft.com/pricing/details/cdn/) för data som nås från hello edge noder.

Anta exempelvis att du har ett lagringskonto i USA, västra som används med en Azure CDN. Om någon i hello Storbritannien försöker tooaccess en hello-blobbar i det lagringskontot via hello CDN kontrollerar Azure först hello kantnod som är närmast hello Storbritannien för blobben. Om att hitta den ansluter till kopian av hello blob och använder CDN prissättning, eftersom den används på hello CDN. Om inte hittas, Azure kopierar hello blob toohello kantnod, vilket leder till utgång och transaktionskostnader som anges i hello Blob storage-priser och komma åt hello-filen på hello kantnod, vilket leder till CDN fakturering.

När du tittar på hello [CDN sida med priser](https://azure.microsoft.com/pricing/details/cdn/), Observera att HTTPS-stöd för anpassade domännamn är endast tillgängligt för Azure CDN från Verizon produkter (Standard och Premium).

## <a name="next-steps"></a>Nästa steg

[Konfigurera ett anpassat domännamn för din slutpunkt för Blob-lagring](storage-custom-domain-name.md)
