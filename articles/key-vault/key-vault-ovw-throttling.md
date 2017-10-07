---
ms.assetid: 
title: "aaaAzure Key Vault bandbreddsbegränsning vägledning | Microsoft Docs"
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 06/21/2017
ms.openlocfilehash: a75cf96bc6503e51f14378bee598bad57e85be82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-throttling-guidance"></a>Azure Key Vault begränsning vägledning

Begränsning är en process som du har initierat som begränsar hello antal samtidiga anrop toohello Azure-tjänsten tooprevent felaktig användning av resurser. Azure Key Vault (AKV) är utformad toohandle en stor mängd begäranden. Om det inträffar ett överväldigande antal begäranden, upprätthåller begränsning din klientbegäranden optimala prestanda och tillförlitlighet för hello AKV-tjänsten.

Bandbreddsbegränsning gränser variera beroende på hello scenario. Till exempel om du utför ett stort antal skrivningar hello möjlighet för begränsning är högre än om du bara utför läsningar.

## <a name="how-does-key-vault-handle-its-limits"></a>Hur hanterar Key Vault gränsen?

Tjänstbegränsningarna i Key Vault finns det tooprevent missbruk av resurser och kontrollera Tjänstkvalitet för alla Key Vault-klienter. När ett tröskelvärde för tjänsten har överskridits begränsar Key Vault ytterligare förfrågningar från klienten för en viss tidsperiod. När det händer gör Key Vault returnerar HTTP-statuskod 429 (för många begäranden), och hello begär misslyckas. Dessutom misslyckade begäranden som 429 antal mot hello begränsning gränser spåras av Key Vault. 

Om du har ett giltigt företag ärende för högre begränsning gränser, kontaktar du oss.


## <a name="how-toothrottle-your-app-in-response-tooservice-limits"></a>Hur toothrottle din app i svaret tooservice begränsar

hello följande är **metodtips** för begränsning av din app:
- Minska hello antal åtgärder per begäran.
- Minska hello frekvensen för begäranden.
- Undvik omedelbara försök. 
    - Alla begäranden som görs mot gränserna för Resursanvändning.

När du implementerar din app felhantering måste använda hello HTTP felkod 429 toodetect hello för klientsidan begränsning. Om hello begäran misslyckas igen med en HTTP-429 felkod, det fortfarande uppstår en gräns för Azure-tjänsten. Fortsätt toouse hello rekommenderad klientsidan bandbreddsbegränsning metod, som du försöker hello begäran tills den lyckas.

### <a name="recommended-client-side-throttling-method"></a>Rekommenderad metod för klientsidan bandbreddsbegränsning

HTTP-felkod: 429 börja begränsning klienten med en exponentiell backoff-metoden:

1. Vänta 1 sekund, försök igen
2. Om du fortfarande begränsas vänta 2 sekunder, försök igen med begäran
3. Om du fortfarande begränsas vänta 4 sekunder, försök igen med begäran
4. Om du fortfarande begränsas vänta 8 sekunder, försök igen med begäran
5. Om du fortfarande begränsas vänta 16 sekunder, försök igen med begäran

Nu bör du inte att hämta HTTP 429 svarskoder.

## <a name="see-also"></a>Se även

En djupare orientering med begränsningen på hello Microsoft Cloud finns [begränsning mönster](https://docs.microsoft.com/azure/architecture/patterns/throttling).

