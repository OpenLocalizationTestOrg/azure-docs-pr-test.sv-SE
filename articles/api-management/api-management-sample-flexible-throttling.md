---
title: "aaaAdvanced begäran begränsning med Azure API Management"
description: "Lär dig hur toocreate och tillämpa flexibla kvoten och begränsa principer med Azure API Management hastighet."
services: api-management
documentationcenter: 
author: darrelmiller
manager: erikre
editor: 
ms.assetid: fc813a65-7793-4c17-8bb9-e387838193ae
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: ac87f83118a37bd587fddf044e5c2d6fc2af9031
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-request-throttling-with-azure-api-management"></a>Avancerade begäran begränsning med Azure API Management
Att kunna toothrottle inkommande begäranden är en viktig roll i Azure API Management. Antingen genom att kontrollera hello andel begäranden eller hello Totalt antal begäranden/data överförs API Management gör API providers tooprotect sina API: er från missbruk och skapa värde för olika nivåer för API-produkten.

## <a name="product-based-throttling"></a>Produkten baserat begränsning
toodate hello hastighet kapacitet för begränsning har begränsad toobeing omfång tooa viss produkt prenumeration (i praktiken en nyckel), definieras i hello API Management publisher-portalen. Detta är användbart för hello API providern tooapply gränser för hello utvecklare som har registrerat dig toouse sitt API, men det hjälper inte, till exempel i begränsning enskilda slutanvändare av hello API. Det är möjligt att för enskild användare av hello developer program tooconsume hello hela kvoten och sedan förhindra andra hello developer-kunder kan toouse hello program. Flera kunder som kan generera en stor mängd begäranden kan också begränsa åtkomst toooccasional användare.

## <a name="custom-key-based-throttling"></a>Anpassade nyckelbaserad begränsning
hello nya [hastighet gränsen av nyckeln](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) och [kvoten av nyckeln](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) principerna ger ett betydligt mer flexibel lösning tootraffic kontroll. Dessa nya principer gör toodefine uttryck tooidentify hello nycklar som kommer att använda tootrack trafik användning. Hej hur detta fungerar illustreras easiest med ett exempel. 

## <a name="ip-address-throttling"></a>Begränsning av IP-adress
hello följande principer begränsa en enskild klient IP-adress tooonly 10 anrop i minuten, med totalt 1 000 000 samtal och 10 000 kB bandbredd per månad. 

```xml
<rate-limit-by-key  calls="10"
          renewal-period="60"
          counter-key="@(context.Request.IpAddress)" />

<quota-by-key calls="1000000"
          bandwidth="10000"
          renewal-period="2629800"
          counter-key="@(context.Request.IpAddress)" />
```

Om alla klienter i hello Internet används en unik IP-adress, kan det vara ett effektivt sätt för att begränsa användningen av användaren. Det är mycket troligt att flera användare att dela en offentlig IP-adress på grund av toothem använder hello Internet via en NAT-enhet. Trots detta för API: er som tillåter obehörig åtkomst hello `IpAddress` kanske hello bästa alternativet.

## <a name="user-identity-throttling"></a>Begränsning av identitet
Om en användare autentiseras och sedan en bandbreddsbegränsning nyckel kan genereras baserat på information som unikt identifierar en som användaren.

```xml
<rate-limit-by-key calls="10"
    renewal-period="60"
    counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />
```

I det här exemplet vi extrahera hello Authorization-huvud, konvertera den för`JWT` objekt och använda hello ämnet för hello token tooidentify hello användare och använda det som hello hastighet begränsa nyckel. Om hello användaridentitet lagras i hello `JWT` som en hello andra anspråk sedan att värdet kan användas i dess ställe.

## <a name="combined-policies"></a>Kombinerade principer
Hello nya principer för begränsning av ger mer kontroll än hello befintliga bandbreddsbegränsning principer, finns det fortfarande värde kombinera båda funktioner. Begränsning av prenumeration produktnyckel ([gränsen anropet frekvensen av prenumerationen](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) och [kvot för uppsättningen av prenumerationen](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) är ett bra sätt tooenable monetizing för en API genom att dra baserat på användning nivåer. hello avancerat hjälpmedel metataggkontroll kontroll av att kunna toothrottle av användare kompletterar och förhindrar att en användarbeteende försämring hello upplevelse av en annan. 

## <a name="client-driven-throttling"></a>Klienten drivs begränsning
När hello begränsning nyckel definieras med hjälp av en [principuttryck](https://msdn.microsoft.com/library/azure/dn910913.aspx), så är det hello API-leverantör som väljer hur hello begränsning är begränsad. Dock vill kanske en utvecklare toocontrol hur de betygsätta begränsa sina kunder. Detta kan aktiveras av hello API-providern genom att introducera en anpassad rubrik tooallow hello utvecklarens klienten programmet toocommunicate hello viktiga toohello API.

```xml
<rate-limit-by-key calls="100"
          renewal-period="60"
          counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>
```

Detta gör att hello developer klienten programmet toochoose hur de vill toocreate hello hastighet begränsa nyckel. Med lite uppfinningsrikedom kunde klienten utvecklare skapa sina egna hastighet nivåer genom att allokera uppsättningar av nycklar toousers och rotera hello nyckelanvändning.

## <a name="summary"></a>Sammanfattning
Azure API Management ger hastighet och citattecken begränsning tooboth skydda och lägga till värdet tooyour API-tjänsten. hello begränsning nya principer med anpassade reglerna kan du bättre kornat kontroll över dessa principer tooenable dina kunder toobuild ännu bättre program. hello exemplen i den här artikeln visar hello användningen av dessa nya principer med tillverkning som begränsar nycklar med klienternas IP-adresser, användar-ID och klienten genererade värden. Det finns emellertid många andra delar av hello-meddelande som kan användas som användaragent, URL-sökväg fragment, meddelandestorlek.

## <a name="next-steps"></a>Nästa steg
Ge oss din feedback i Disqus-tråden hello för det här avsnittet. Det är bra toohear om andra potentiella nyckelvärden som har ett logiskt val i dina scenarier.

## <a name="watch-a-video-overview-of-these-policies"></a>Titta på en videoöversikt över dessa principer
Mer information om hello [hastighet gränsen av nyckeln](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) och [kvoten av nyckeln](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) principer som beskrivs i den här artikeln får du titta på hello följande video.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Advanced-Request-Throttling-with-Azure-API-Management/player]
> 
> 

