---
title: "aaaHow toodelegate användarens registrering och produkten prenumeration"
description: "Lär dig hur toodelegate användaren registrering och produkten prenumeration tooa tredjeparts i Azure API Management."
services: api-management
documentationcenter: 
author: antonba
manager: erikre
editor: 
ms.assetid: 8b7ad5ee-a873-4966-a400-7e508bbbe158
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 406648db2d2f37c4dcda466294726d331cc0551b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodelegate-user-registration-and-product-subscription"></a>Hur toodelegate användarens registrering och produkt-prenumeration
Delegering kan du toouse din befintliga webbplats för att hantera developer logga-i/sign-upp och prenumeration tooproducts som motsats toousing hello inbyggda funktioner i hello developer-portalen. Detta aktiverar webbplats tooown hello användardata och utföra hello verifiering av stegen i ett anpassat sätt.

## <a name="delegate-signin-up"></a>Delegera developer inloggning och registrering
toodelegate developer inloggning och registrering tooyour befintlig webbplats måste toocreate en slutpunkt för särskilda delegering på webbplatsen som fungerar som hello startpunkt för en sådan begäran som initieras från hello API Management developer-portalen.

hello slutliga arbetsflödet är följande:

1. Utvecklare som klickar på hello logga in eller registrera länk på hello API Management developer-portalen
2. Webbläsaren är omdirigerade toohello delegering slutpunkt
3. Delegering slutpunkt i RETUR omdirigerar tooor anger Användargränssnittet frågar användaren toosign i eller registrera
4. Om åtgärden lyckades kan är hello användaren omdirigerade tillbaka toohello API Management developer Portalsida de startas från

toobegin, låt oss först ställa in API-hantering tooroute begäranden via din slutpunkt för delegering. I hello API Management publisher-portalen klickar du på **säkerhet** och klicka sedan på hello **delegering** fliken. Klicka på hello kryssrutan tooenable ombud inloggning & registrering.

![Delegering sida][api-management-delegation-signin-up]

* Bestäm vad hello URL för din slutpunkt för särskilda delegering tas och ange den i hello **delegering slutpunkts-URL** fältet. 
* Inom hello **delegering autentiseringsnyckel** ange en hemlighet som kommer att använda toocompute en signatur som tooyou för verifiering tooensure som hello begäran verkligen kommer från Azure API Management. Du kan klicka på hello **generera** knappen toohave API Managemnet slumpmässigt Generera en nyckel för dig.

Nu måste toocreate hello **delegering endpoint**. Tooperform har ett antal åtgärder:

1. Ta emot en begäran i hello följande format:
   
   > *http://www.yourwebsite.com/apimdelegation?operation=Signin&returnUrl= {URL för datakälla på sidan} & salt = {sträng} & sig = {sträng}*
   > 
   > 
   
    Frågeparametrar för hello inloggning / registrering fall:
   
   * **åtgärden**: identifierar vilken typ av begäran om delegering är det - vara **SignIn** i det här fallet
   * **returnUrl**: hello URL till hello sida där hello användaren har klickat på en länk för inloggning eller registrering
   * **salt**: en särskild salt sträng som används för att beräkna en hash för säkerhet
   * **sig**: en beräknad säkerhet hash toobe som används för jämförelse tooyour egna beräknat hash-värde
2. Kontrollera att hello förfrågan kommer från Azure API Management (valfritt, men rekommenderas för säkerhet)
   
   * Beräkna en SHA512 HMAC hash för en sträng baserat på hello **returnUrl** och **salt** fråga parametrar ([exempelkoden nedan]):
     
     > HMAC (**salt** + ”\n” + **returnUrl**)
     > 
     > 
   * Jämför hello ovan beräknad toohello hashvärde hello **sig** Frågeparametern. Om hello två hash-värdena matchar vidare toohello nästa steg, annars neka hello-begäran.
3. Kontrollera att du tar emot en begäran om inloggning-i/logga-up: hello **åtgärden** frågeparameter anges för ”**SignIn**”.
4. Aktuell hello användare med användargränssnitt toosign i eller registrera
5. Om hello användaren har registrerat dig har du toocreate motsvarande konto för dem i API-hantering. [Skapa en användare] med hello API Management REST API. När du gör det, se till att du hello användar-ID toohello samma i din användararkivet eller tooan-ID som du kan hålla reda på.
6. När hello användaren kan autentiseras:
   
   * [begär en enkel inloggning (SSO) token] via hello API Management REST API
   * Lägg till en returnUrl fråga parametern toohello SSO-URL som du har fått från hello API-anrop ovan:
     
     > t.ex. https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url 
     > 
     > 
   * omdirigera hello användaren toohello ovan producerade URL

I tillägg toohello **SignIn** igen, du kan också utföra kontohantering genom att följa stegen i hello och med någon av följande åtgärder hello.

* **ChangePassword**
* **ChangeProfile**
* **CloseAccount**

Du måste överföra hello följande frågeparametrar för kontohanteringsåtgärder.

* **åtgärden**: identifierar vilken typ av begäran om delegering (ChangePassword, ChangeProfile eller CloseAccount)
* **användar-ID**: hello användar-id för hello konto toomanage
* **salt**: en särskild salt sträng som används för att beräkna en hash för säkerhet
* **sig**: en beräknad säkerhet hash toobe som används för jämförelse tooyour egna beräknat hash-värde

## <a name="delegate-product-subscription"></a>Delegera produkten prenumeration
Delegera produkten prenumeration fungerar på samma sätt toodelegating användaren logga in/ut. hello slutliga arbetsflöde skulle vara följande:

1. Utvecklare väljer en produkt i hello API Management developer-portalen och klickar på knappen för hello prenumerera
2. Webbläsaren är omdirigerade toohello delegering slutpunkt
3. Delegering endpoint utför nödvändiga produkten prenumeration steg – detta är tooyou och kan medföra att tooanother sidan toorequest faktureringsinformation, frågar andra frågor eller bara lagra hello information och inte kräver någon åtgärd från användarens sida

tooenable hello funktionalitet på hello **delegering** klickar **delegera produkten prenumeration**.

Kontrollera sedan hello delegering endpoint utför hello följande åtgärder:

1. Ta emot en begäran i hello följande format:
   
   > *http://www.yourwebsite.com/apimdelegation?operation= {åtgärd} & productId = {produkten toosubscribe till} & userId = {användare som gjort begäran} & salt = {sträng} & sig = {sträng}*
   > 
   > 
   
    Frågeparametrar Hej produkten prenumeration fall:
   
   * **åtgärden**: identifierar vilken typ av begäran om delegering. För produkten prenumeration är begäranden hello giltiga alternativ:
     * ”Prenumerera”: en begäran toosubscribe hello användaren tooa angivna produkt med angett ID (se nedan)
     * ”Avbryta prenumerationen”: en begäran toounsubscribe en användare från en produkt
     * ”Förnya”: en requst toorenew en prenumeration (t.ex. Det kan ut)
   * **productId**: hello-ID för hello produkten hello användare begärde toosubscribe till
   * **användar-ID**: hello-ID för hello användare för vilka hello begäran har gjorts
   * **salt**: en särskild salt sträng som används för att beräkna en hash för säkerhet
   * **sig**: en beräknad säkerhet hash toobe som används för jämförelse tooyour egna beräknat hash-värde
2. Kontrollera att hello förfrågan kommer från Azure API Management (valfritt, men rekommenderas för säkerhet)
   
   * Beräkna en HMAC SHA512 av en sträng som baseras på hello **productId**, **userId** och **salt** fråga parametrar:
     
     > HMAC (**salt** + ”\n” + **productId** + ”\n” + **userId**)
     > 
     > 
   * Jämför hello ovan beräknad toohello hashvärde hello **sig** Frågeparametern. Om hello två hash-värdena matchar vidare toohello nästa steg, annars neka hello-begäran.
3. Utföra någon prenumeration bearbetning utifrån hello typ av åtgärd som angavs i **åtgärden** -t.ex. fakturering, ytterligare frågor, osv.
4. Prenumerera på har prenumerera hello användaren toohello produkten på din sida, hello användaren toohello API Management produkten av [anropa hello REST API för produkten prenumeration].

## <a name="delegate-example-code"></a> Exempelkod
Dessa code exempel visar hur tootake hello *delegering valideringsnyckel*, som har angetts i hello delegering skärmen i hello publisher portal, toocreate en HMAC som sedan används toovalidate hello signatur, vilket bevisar hello giltighet hello skickade returnUrl. hello samma kod fungerar i hello productId och användar-ID med små ändringar.

**C#-kod toogenerate-hash för returnUrl**

```c#
using System.Security.Cryptography;

string key = "delegation validation key";
string returnUrl = "returnUrl query parameter";
string salt = "salt query parameter";
string signature;
using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
{
    signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
    // change too(salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature toosig query parameter
}
```

**NodeJS kod toogenerate hash för returnUrl**

```
var crypto = require('crypto');

var key = 'delegation validation key'; 
var returnUrl = 'returnUrl query parameter';
var salt = 'salt query parameter';

var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
var digest = hmac.update(salt + '\n' + returnUrl).digest();
// change too(salt + "\n" + productId + "\n" + userId) when delegating product subscription
// compare signature toosig query parameter

var signature = digest.toString('base64');
```

## <a name="next-steps"></a>Nästa steg
Mer information om delegering finns i följande video hello.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Delegating-User-Authentication-and-Product-Subscription-to-a-3rd-Party-Site/player]
> 
> 

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[begär en enkel inloggning (SSO) token]: http://go.microsoft.com/fwlink/?LinkId=507409
[Skapa en användare]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser
[anropa hello REST API för produkten prenumeration]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO
[Next steps]: #next-steps
[exempelkoden nedan]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 
