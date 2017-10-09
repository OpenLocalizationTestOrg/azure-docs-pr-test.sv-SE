---
title: "aaaAuthorize developer konton med hjälp av OAuth 2.0 i Azure API Management | Microsoft Docs"
description: "Lär dig hur tooauthorize användare med hjälp av OAuth 2.0 i API-hantering."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 78c48247-64f0-4708-b2d0-98b61a821283
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 934901dd6df399470a3257bf7a3a9b9fb5f40d5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooauthorize-developer-accounts-using-oauth-20-in-azure-api-management"></a>Hur tooauthorize developer användarkonton med OAuth 2.0 i Azure API Management
Stöd för många API: er [OAuth 2.0](http://oauth.net/2/) toosecure hello API och se till att endast giltiga användare har åtkomst och de kan bara komma åt resurser toowhich de är rätt. I ordning toouse Azure API Managements interaktiva Developer-konsolen med dessa API: er hello tjänsten kan du tooconfigure toowork din service-instans med OAuth 2.0 aktiverat API.

## <a name="prerequisites"></a>Krav
Den här guiden visar hur tooconfigure din API Management service-instans toouse OAuth 2.0-auktorisering för utvecklare konton, men visar inte hur tooconfigure en OAuth 2.0-providern. hello-konfigurationen för varje OAuth 2.0-providern är olika, även om hello stegen är ungefär och hello krävs typer av information som används när du konfigurerar OAuth 2.0 i din API Management service-instans är hello samma. Det här avsnittet visas exempel som använder Azure Active Directory som en OAuth 2.0-provider.

> [!NOTE]
> Mer information om hur du konfigurerar OAuth 2.0 med Azure Active Directory finns hello [WebApp-GraphAPI-DotNet] [ WebApp-GraphAPI-DotNet] exempel.
> 
> 

## <a name="step1"></a>Konfigurera en server för OAuth 2.0-auktorisering i API Management
tooget har startats klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten.

![Utgivarportalen][api-management-management-console]

> [!NOTE]
> Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.
> 
> 

Klicka på **säkerhet** från hello **API Management** menyn hello vänster, klicka på **OAuth 2.0**, och klicka sedan på **Lägg till auktorisering server**.

![OAuth 2.0][api-management-oauth2]

När du klickar på **Lägg till auktorisering server**, hello nya auktorisering server formuläret visas.

![Ny server][api-management-oauth2-server-1]

Ange ett namn och en valfri beskrivning i hello **namn** och **beskrivning** fält. 

> [!NOTE]
> Dessa fält är används tooidentify hello OAuth 2.0 auktorisering server inom hello aktuella API Management service-instans och deras värden kommer inte från hello OAuth 2.0-server.
> 
> 

Ange hello **klienten URL för registrering**. Den här sidan är där användare kan skapa och hantera sina konton och varierar beroende på hello OAuth 2.0-providern som används. Hej **klienten URL för registrering** toohello sidan som användarna kan använda toocreate och konfigurera sina egna konton för OAuth 2.0-leverantörer som stöder Användarhantering av konton. Vissa organisationer konfigurera eller använda den här funktionen, även om detta hello OAuth 2.0-providern stöds inte. Om OAuth 2.0-providern inte har användarhantering för konton som har konfigurerats, ange en platshållare URL här, till exempel hello URL för ditt företag eller en URL som `https://placeholder.contoso.com`.

hello nästa avsnitt av hello formuläret innehåller hello **Auktoriseringskoden bevilja typer**, **slutpunkts-URL-auktorisering**, och **begäran auktoriseringsmetod** inställningar.

![Ny server][api-management-oauth2-server-2]

Ange hello **Auktoriseringskoden bevilja typer** genom att kontrollera hello önskad typer. **Auktoriseringskoden** har angetts som standard.

Ange hello **slutpunkts-URL-auktorisering**. Azure Active Directory, URL: en kommer att vara liknande toohello följande URL, där `<client_id>` ersätts med hello klient-id som identifierar toohello OAuth 2.0-programservern.

`https://login.microsoftonline.com/<client_id>/oauth2/authorize`

Hej **begäran auktoriseringsmetod** anger hur hello auktoriseringsbegäran skickas toohello OAuth 2.0-server. Som standard **hämta** är markerad.

hello nästa avsnitt är där hello **Token slutpunkts-URL**, **klienten autentiseringsmetoder**, **åtkomsttoken skickar metoden**, och **standard scope** har angetts.

![Ny server][api-management-oauth2-server-3]

En Azure Active Directory OAuth 2.0-server hello **Token slutpunkts-URL** har hello följande format, där `<APPID>` har hello `yourapp.onmicrosoft.com`.

`https://login.microsoftonline.com/<APPID>/oauth2/token`

Hej standardinställningen för **klienten autentiseringsmetoder** är **grundläggande**, och **åtkomsttoken skickar metoden** är **Authorization-huvud**. Dessa värden har konfigurerats på den här delen av hello formuläret, tillsammans med hello **standard scope**.

Hej **klientautentiseringsuppgifterna** avsnitt innehåller hello **klient-ID** och **klienthemlighet**, som erhålls under hello skapande och konfiguration av OAuth 2.0 Server. En gång hello **klient-ID** och **klienthemlighet** anges, hello **redirect_uri** för hello **Auktoriseringskoden** genereras. Den här URI är används tooconfigure hello reply-URL i serverkonfigurationen OAuth 2.0.

![Ny server][api-management-oauth2-server-4]

Om **Auktoriseringskoden bevilja typer** har angetts för**resurs ägarlösenord**, hello **lösenordsinformation för resurs-ägare** avsnittet är används toospecify som de tilldelats. Annars kan du lämna det tomt.

![Ny server][api-management-oauth2-server-5]

När hello formuläret är klar klickar du på **spara** toosave hello API Management OAuth 2.0-auktorisering serverkonfiguration. När du har sparat hello serverkonfiguration kan du konfigurera API: er toouse den här konfigurationen enligt hello nästa avsnitt.

## <a name="step2"></a>Konfigurera en API-toouse OAuth 2.0 användarautentiseringen
Klicka på **API: er** från hello **API Management** menyn på hello vänster, klicka på hello namnet på hello önskad API, **säkerhet**, och sedan kryssrutan hello för **OAuth 2.0**.

![Auktoriseringen av användaren][api-management-user-authorization]

Välj hello önskad **auktorisering server** hello listrutan och klickar på **spara**.

![Auktoriseringen av användaren][api-management-user-authorization-save]

## <a name="step3"></a>Testa hello OAuth 2.0 användarautentiseringen i hello Developer-portalen
När du har konfigurerat OAuth 2.0 auktorisering servern och konfigurerat API-toouse servern, kan du testa den genom att gå toohello Developer-portalen och anropar en API.  Klicka på **utvecklarportalen** i hello övre högra menyn.

![Utvecklarportalen][api-management-developer-portal-menu]

Klicka på **API: er** i hello översta menyn och välj **Echo API**.

![Echo API][api-management-apis-echo-api]

> [!NOTE]
> Om du har bara en API som konfigurerats eller synliga tooyour konto, klicka på API: er kommer du direkt toohello åtgärder för att API.
> 
> 

Välj hello **hämta resurs** åtgärden, klickar du på **öppna konsolen**, och välj sedan **Auktoriseringskoden** hello i listrutan.

![Öppna konsolen][api-management-open-console]

När **Auktoriseringskoden** är markerat visas ett popup-fönster med hello inloggning form av hello OAuth 2.0-providern. I det här exemplet tillhandahålls hello inloggning formuläret av Azure Active Directory.

> [!NOTE]
> Om du har popup-fönster har inaktiverats kommer du att uppmanas tooenable dem hello webbläsare. När du har aktiverat dem, Välj **Auktoriseringskoden** igen och hello inloggning formuläret visas.
> 
> 

![Logga in][api-management-oauth2-signin]

När du har loggat in hello **Begäransrubriker** fylls i med en `Authorization : Bearer` huvud som tillåter hello-begäran.

![Token för anslutningsbegäran sidhuvud][api-management-request-header-token]

Du kan nu konfigurera hello önskade värden för hello återstående parametrar och skicka begäran om hello. 

## <a name="next-steps"></a>Nästa steg
Mer information om att använda OAuth 2.0 och API-hantering finns hello följande video och tillhörande [artikel](api-management-howto-protect-backend-with-aad.md).

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Protecting-Web-API-Backend-with-Azure-Active-Directory-and-API-Management/player]
> 
> 

[api-management-management-console]: ./media/api-management-howto-oauth2/api-management-management-console.png
[api-management-oauth2]: ./media/api-management-howto-oauth2/api-management-oauth2.png
[api-management-user-authorization]: ./media/api-management-howto-oauth2/api-management-user-authorization.png
[api-management-user-authorization-save]: ./media/api-management-howto-oauth2/api-management-user-authorization-save.png
[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-oauth2-server-1]: ./media/api-management-howto-oauth2/api-management-oauth2-server-1.png
[api-management-oauth2-server-2]: ./media/api-management-howto-oauth2/api-management-oauth2-server-2.png
[api-management-oauth2-server-3]: ./media/api-management-howto-oauth2/api-management-oauth2-server-3.png
[api-management-oauth2-server-4]: ./media/api-management-howto-oauth2/api-management-oauth2-server-4.png
[api-management-oauth2-server-5]: ./media/api-management-howto-oauth2/api-management-oauth2-server-5.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png


[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How tooadd and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs tooa product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API toouse OAuth 2.0 user authorization]: #step2
[Test hello OAuth 2.0 user authorization in hello Developer Portal]: #step3
[Next steps]: #next-steps

