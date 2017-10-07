---
title: 'aaaHow toocreate API: er i Azure API Management'
description: "Lär dig hur toocreate och konfigurera API: er i Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 14c20da4-f29f-4b28-bec7-3d4c50b734da
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 48ed8d93947253aa1e67ad995927ed6101cac072
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-apis-in-azure-api-management"></a>Hur toocreate API: er i Azure API Management
En API i API Management representerar en uppsättning åtgärder som kan anropas av klientprogram. Nya API: er skapas i hello publisher portal och sedan hello önskad operations har lagts till. När hello operations läggs hello API läggs tooa produkten och publiceras. När en API som har publicerats kan vara det prenumererar på tooand som används av utvecklare.

Den här guiden visar hello första steget i processen för hello: hur toocreate och konfigurera ett nytt API i API-hantering. Mer information om att lägga till åtgärder och publicering av en produkt finns [hur tooadd operations tooan API] [ How tooadd operations tooan API] och [hur toocreate och publicera en produkt] [ How toocreate and publish a product].

## <a name="create-new-api"></a>Skapa ett nytt API
API: er skapas och konfigureras i hello publisher portal. tooaccess hello publisher klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten.

![Utgivarportalen][api-management-management-console]

> Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.
> 
> 

Klicka på **API: er** från hello **API Management** menyn på hello vänster och klicka sedan på **lägga till API**.

![Skapa API][api-management-create-api]

Använd hello **lägga till nya API** fönstret tooconfigure hello nya API.

![Lägga till ett nytt API][api-management-add-new-api]

hello följande fält är används tooconfigure hello nya API.

* **Webb-API-namnet** ger ett unikt och beskrivande namn för hello API. Den visas i hello utvecklare och utgivare portaler.
* **Webbtjänstens URL** referenser hello HTTP-tjänsten implementerar hello API. API-hantering vidarebefordrar begäranden toothis adress.
* **Web API-URL-suffix** är tillagda toohello bas-URL för hello API management-tjänsten. hello bas-URL är gemensamma för alla API: er som en instans för API Management-tjänsten är värd för. API Management särskiljer API: er av deras suffix och därför hello suffix måste vara unikt för alla API: et för en viss utgivare.
* **Web API-URL-schema** anger vilka protokoll som kan vara används tooaccess hello API. HTTPs har angetts som standard.
* toooptionally Lägg till den här nya API tooa produkten, markera hello **produkter (valfritt)** listrutan och väljer en produkt. Det här steget kan vara upprepas flera gånger tooadd hello API toomultiple produkter.

När hello önskade värden har konfigurerats, klickar du på **spara**. När du har skapat hello nya API visas hello sammanfattningssidan hello-API: t i hello publisher-portalen.

![API-sammanfattning][api-management-api-summary]

## <a name="configure-api-settings"></a>Konfigurera API-inställningar
Du kan använda hello **inställningar** fliken tooverify och redigera hello konfigurationen för en API. **Webb-API-namnet**, **-webbtjänstens URL**, och **URL för Web API-suffix** från början anges när hello API skapas och kan ändras här. **Beskrivning** ger en valfri beskrivning och **Web API-URL-schema** anger vilka protokoll som kan vara används tooaccess hello API.

![API-inställningar][api-management-api-settings]

tooconfigure gateway-autentisering för hello backend tjänsten implementerar hello API, Välj hello **säkerhet** fliken hello **med autentiseringsuppgifter** listrutan kan vara används tooconfigure **HTTP grundläggande** eller **klientcertifikat** autentisering. toouse HTTP grundläggande autentisering, anger du bara hello önskad autentiseringsuppgifter. Information om hur du använder certifikat för klientautentisering finns [hur toosecure backend-tjänster som använder klienten certifikatautentisering i Azure API Management][How toosecure back-end services using client certificate authentication in Azure API Management].

Hej **säkerhet** flik kan också vara används tooconfigure **användarautentiseringen** med hjälp av OAuth 2.0. Mer information finns i [hur tooauthorize developer användarkonton med OAuth 2.0 i Azure API Management][How tooauthorize developer accounts using OAuth 2.0 in Azure API Management].

![Inställningar för grundläggande autentisering][api-management-api-settings-credentials]

Klicka på **spara** toosave alla ändringar du gör toohello API-inställningarna.

## <a name="next-steps"> </a>Nästa steg
När en API som har skapats och hello-inställningarna, hello nästa steg är tooadd hello operations toohello API, lägga till hello API tooa produkt och publicera den så att den är tillgänglig för utvecklare. Mer information finns i följande artiklar hello.

* [Hur tooadd operations tooan API][How tooadd operations tooan API]
* [Hur toocreate och publicera en produkt][How toocreate and publish a product]

[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[What is an API?]: #what-is-api
[Create a new API]: #create-new-api
[Configure API settings]: #configure-api-settings
[Configure API operations]: #configure-api-operations
[Next steps]: #next-steps

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How toocreate and publish a product]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[How toosecure back-end services using client certificate authentication in Azure API Management]: api-management-howto-mutual-certificates.md
[How tooauthorize developer accounts using OAuth 2.0 in Azure API Management]: api-management-howto-oauth2.md
