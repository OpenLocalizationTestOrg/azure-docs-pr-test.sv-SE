---
title: "aaaSecuring Azure CDN tillgångar med tokenautentisering | Microsoft Docs"
description: "Med hjälp av tokenautentisering toosecure åtkomst tooyour Azure CDN tillgångar."
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: 
ms.assetid: 837018e3-03e6-4f9c-a23e-4b63d5707a64
ms.service: cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/11/2016
ms.author: mezha
ms.openlocfilehash: 5865bcb8eed7ced834970d52d30136252039265f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="securing-azure-cdn-assets-with-token-authentication"></a>Att säkra Azure CDN tillgångar med tokenautentisering

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

##<a name="overview"></a>Översikt

Token-autentisering är en mekanism som gör att du tooprevent Azure CDN från betjäna tillgångar toounauthorized klienter.  Detta görs vanligtvis tooprevent ”hotlinking” av innehåll, där en annan webbplats, ofta anslagstavla använder dina tillgångar utan behörighet.  Detta kan påverka dina kostnader för leverans av innehåll. Genom att aktivera den här funktionen på CDN autentiseras begäranden genom kant CDN POP innan du levererar hello innehåll. 

## <a name="how-it-works"></a>Hur det fungerar

Tokenautentisering verifierar begäranden som genereras av en tillförlitlig plats genom att kräva begäranden toocontain token-värde som innehåller kodad information om hello beställaren. Innehållet endast hanteras toorequester hello kodad information uppfyller hello krav, annars nekas begäran. Du kan ställa in hello krav med hjälp av en eller flera parametrar.

- Land: Tillåt eller neka förfrågningar som kommer från angivna länder.  [Lista över giltiga landskoder.](https://msdn.microsoft.com/library/mt761717.aspx) 
- URL: Tillåt endast angivna tillgång eller sökvägen toorequest.  
- Värden: Tillåt eller neka begäranden med angivna värdar i hello huvudet i begäran.
- Referent: Tillåt eller neka angivna referent toorequest.
- IP-adress: endast tillåta begäranden som kommer från särskilda IP-adress eller IP-undernät.
- Protokoll: Tillåt eller blockera förfrågningar baserat på hello protokollet toorequest hello innehållet har använts.
- Förfallotid: tilldela ett datum och tid period tooensure att en länk endast förblir giltig för en begränsad tidsperiod.

Visa detaljerade Konfigurationsexempel på varje parameter.

## <a name="reference-architecture"></a>Referensarkitektur

Nedan följer en Referensarkitektur för att skapa token autentisering på CDN toowork med ditt webbprogram.

![CDN-profilbladet hantera knappen](./media/cdn-token-auth/cdn-token-auth-workflow2.png)

## <a name="token-validation-logic-on-cdn-endpoint"></a>Token valideringslogik på CDN-slutpunkten
    
Det här diagrammet beskrivs hur Azure CDN verifierar klientens begäran om tokenautentisering är konfigurerat på CDN-slutpunkten.

![CDN-profilbladet hantera knappen](./media/cdn-token-auth/cdn-token-auth-validation-logic.png)

## <a name="setting-up-token-authentication"></a>Konfigurera tokenautentisering

1. Från hello [Azure-portalen](https://portal.azure.com)Bläddra tooyour CDN-profilen och klicka sedan på hello **hantera** knappen toolaunch hello kompletterande portalen.

    ![CDN-profilbladet hantera knappen](./media/cdn-rules-engine/cdn-manage-btn.png)

2. Hovra över **HTTP stora**, och klicka sedan på **Token Auth** i hello utfällbar. Du kan ställa in krypteringsnyckeln och kryptering parametrar i den här fliken.

    1. Ange en unik krypteringsnyckel för **primärnyckel**.  Ange en annan för **säkerhetskopiering nyckel**

        ![Konfigurationsnyckel för CDN-token auth](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
    2. Ställ in kryptering parametrar med krypteringsverktyget (Tillåt eller neka förfrågningar baserat på förfallotid, land, referent, protokoll, klientens IP-Adressen. Du kan använda valfri kombination.)

        ![CDN-profilbladet hantera knappen](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

        - EC-gälla: tilldelar en förfallotid för en token efter en angiven tidsperiod. Begäranden skickas efter hello förfallotid kommer att nekas. Den här parametern används Unix tidsstämpel (baserat på sekunder sedan standard epok av 1/1/1970 00:00:00 GMT. Du kan använda online verktyg tooprovide konvertering mellan standard och Unix tid.)  Till exempel om du vill att tooset hello token toobe upphört att gälla på 2016/12/31 12:00:00 GMT, använda hello Unix tid: 1483185600 enligt nedan:
    
        ![CDN-profilbladet hantera knappen](./media/cdn-token-auth/cdn-token-auth-expire2.png)
    
        - EC-url-Tillåt: gör att du tootailor token tooa viss tillgång eller sökväg. Den begränsar åtkomst toorequests vars URL börjar med en viss relativ sökväg. Du kan ange flera sökvägar Avgränsa varje sökväg med ett kommatecken. URL: er är skiftlägeskänsliga. Du kan ställa in olika värdet tooprovide olika åtkomstnivå beroende på hello krav. Nedan visas ett par scenarier:
        
            Om du har en URL: http://www.mydomain.com/pictures/city/strasbourg.png. Se indatavärdet ”” och dess åtkomst nivå därefter

            1. Ange värdet ”/”: alla begäranden som tillåts
            2. Ange värdet ”/ bilder”: alla hello efter begäranden kan
            
                - http://www.mydomain.com/Pictures.PNG
                - http://www.mydomain.com/Pictures/City/Strasbourg.PNG
                - http://www.mydomain.com/picturesnew/City/strasbourgh.PNG
            3. Ange värdet ”/ bilder /”: endast begäran om /pictures/ ska tillåtas
            4. Ange värdet ”/ pictures/city/strasbourg.png”: endast tillåter att begäran för den här tillgången
    
        ![CDN-profilbladet hantera knappen](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
    
        - EC land tillåter: endast tillåta begäranden som kommer från en eller flera angivna länder. Att kommer nekas förfrågningar som kommer från andra länder. Använd land koden tooset hello parametrar och avgränsa varje landskod med kommatecken. Till exempel om du vill tooallow från USA och Frankrike, ange oss FR i hello kolumnen som nedan.  
        
        ![CDN-profilbladet hantera knappen](./media/cdn-token-auth/cdn-token-auth-country-allow.png)

        - EC land neka: nekar förfrågningar som kommer från en eller flera angivna länder. Förfrågningar som kommer från andra länder tillåts. Använd land koden tooset hello parametrar och avgränsa varje landskod med kommatecken. Till exempel om du vill toodeny från USA och Frankrike, ange oss FR i hello-kolumn.
    
        - EC-ref-Tillåt: endast tillåta begäranden från angivna referent. En referent identifierar hello Webbadressen till hello webbsida som länkade toohello resurs som begärs. hello referent parametervärdet får inte innehålla hello-protokollet. Du kan ange ett värdnamn och/eller en sökväg på det värdnamnet. Du kan också lägga till flera referenter inom en enda parameter avgränsa dem med kommatecken. Om du har angett ett värde för referent, men hello referent information skickas inte i hello begäran på grund av toosome webbläsarkonfiguration, nekas dessa begäranden som standard. Du kan tilldela ”saknas” eller ett tomt värde i hello parametern tooallow dessa begäranden med referent information saknas. Du kan också använda ”*. consoto.com” tooallow alla underdomäner i consoto.com.  Till exempel om du vill tooallow för begäranden från www.consoto.com, alla underordnade domäner under consoto2.com och erquests med reffers saknas eller är tomt, ange värdet nedan:
        
        ![CDN-profilbladet hantera knappen](./media/cdn-token-auth/cdn-token-auth-referrer-allow2.png)
    
        - EC ref neka: nekar förfrågningar från angivna referent. Läs toodetails och exempel i ”ec-ref-Tillåt”-parametern.
         
        - EC proto Tillåt: endast tillåta begäranden från angivna protokollet. Till exempel http eller https.
        
        ![CDN-profilbladet hantera knappen](./media/cdn-token-auth/cdn-token-auth-url-allow4.png)
            
        - EC proto neka: nekar förfrågningar från angivna protokollet. Till exempel http eller https.
    
        - EC clientip: begränsar åtkomst toospecified beställaren IP-adress. Både IPV4 och IPV6 stöds. Du kan ange enskild begäran IP-adress eller IP-undernät.
            
        ![CDN-profilbladet hantera knappen](./media/cdn-token-auth/cdn-token-auth-clientip.png)
        
    3. Du kan testa din token med hello beskrivning-verktyget.

    4. Du kan också anpassa hello typ av svar som ska returneras toouser när begäran nekas. Som standard använder vi 403.

3. Klicka på **regelmotor** fliken **HTTP stora**. Du ska använda den här fliken toodefine sökvägar tooapply hello funktionen, aktivera hello tokenautentisering funktionen och aktivera ytterligare tokenautentisering relaterade funktioner.

    - Använda ”om” kolumnen toodefine tillgång eller sökväg som du vill tooapply tokenautentisering. 
    - Klicka på tooadd ”Token Auth” från hello funktionen listrutan tooenable tokenautentisering.
        
    ![CDN-profilbladet hantera knappen](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. I hello **regelmotor** fliken finns det några ytterligare funktioner som du kan aktivera.
    
    - Token auth DOS-kod: Anger hello typ av svar som ska returneras toouser när en begäran nekas. Regler som ställts in här åsidosätter hello DOS-inställningen i hello token auth-fliken.
    - Ignorera token auth: Anger om URL: en används toovalidate token ska vara skiftlägeskänslig.
    - Token auth-parameter: Byt namn på strängparametern visar hello begärda URL: en hello token auth frågan. 
        
    ![CDN-profilbladet hantera knappen](./media/cdn-token-auth/cdn-rules-engine2.png)

5. Du kan anpassa din token som är ett program som genererar token för tokenbaserad autentiseringsfunktioner. Källkoden kan användas här i [GitHub](https://github.com/VerizonDigital/ectoken).
Tillgängliga språk är:
    
    - C
    - C#
    - PHP
    - Perl
    - Java
    - Python    


## <a name="azure-cdn-features-and-provider-pricing"></a>Azure CDN funktioner och providern priser

Se hello [CDN-översikt](cdn-overview.md) avsnittet.
