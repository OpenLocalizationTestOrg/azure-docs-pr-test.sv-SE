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
# <a name="how-tooauthorize-developer-accounts-using-oauth-20-in-azure-api-management"></a><span data-ttu-id="2556e-103">Hur tooauthorize developer användarkonton med OAuth 2.0 i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="2556e-103">How tooauthorize developer accounts using OAuth 2.0 in Azure API Management</span></span>
<span data-ttu-id="2556e-104">Stöd för många API: er [OAuth 2.0](http://oauth.net/2/) toosecure hello API och se till att endast giltiga användare har åtkomst och de kan bara komma åt resurser toowhich de är rätt.</span><span class="sxs-lookup"><span data-stu-id="2556e-104">Many APIs support [OAuth 2.0](http://oauth.net/2/) toosecure hello API and ensure that only valid users have access, and they can only access resources toowhich they're entitled.</span></span> <span data-ttu-id="2556e-105">I ordning toouse Azure API Managements interaktiva Developer-konsolen med dessa API: er hello tjänsten kan du tooconfigure toowork din service-instans med OAuth 2.0 aktiverat API.</span><span class="sxs-lookup"><span data-stu-id="2556e-105">In order toouse Azure API Management's interactive Developer Console with such APIs, hello service allows you tooconfigure your service instance toowork with your OAuth 2.0 enabled API.</span></span>

## <span data-ttu-id="2556e-106"><a name="prerequisites"></a>Krav</span><span class="sxs-lookup"><span data-stu-id="2556e-106"><a name="prerequisites"> </a>Prerequisites</span></span>
<span data-ttu-id="2556e-107">Den här guiden visar hur tooconfigure din API Management service-instans toouse OAuth 2.0-auktorisering för utvecklare konton, men visar inte hur tooconfigure en OAuth 2.0-providern.</span><span class="sxs-lookup"><span data-stu-id="2556e-107">This guide shows you how tooconfigure your API Management service instance toouse OAuth 2.0 authorization for developer accounts, but does not show you how tooconfigure an OAuth 2.0 provider.</span></span> <span data-ttu-id="2556e-108">hello-konfigurationen för varje OAuth 2.0-providern är olika, även om hello stegen är ungefär och hello krävs typer av information som används när du konfigurerar OAuth 2.0 i din API Management service-instans är hello samma.</span><span class="sxs-lookup"><span data-stu-id="2556e-108">hello configuration for each OAuth 2.0 provider is different, although hello steps are similar, and hello required pieces of information used in configuring OAuth 2.0 in your API Management service instance are hello same.</span></span> <span data-ttu-id="2556e-109">Det här avsnittet visas exempel som använder Azure Active Directory som en OAuth 2.0-provider.</span><span class="sxs-lookup"><span data-stu-id="2556e-109">This topic shows examples using Azure Active Directory as an OAuth 2.0 provider.</span></span>

> [!NOTE]
> <span data-ttu-id="2556e-110">Mer information om hur du konfigurerar OAuth 2.0 med Azure Active Directory finns hello [WebApp-GraphAPI-DotNet] [ WebApp-GraphAPI-DotNet] exempel.</span><span class="sxs-lookup"><span data-stu-id="2556e-110">For more information on configuring OAuth 2.0 using Azure Active Directory, see hello [WebApp-GraphAPI-DotNet][WebApp-GraphAPI-DotNet] sample.</span></span>
> 
> 

## <span data-ttu-id="2556e-111"><a name="step1"></a>Konfigurera en server för OAuth 2.0-auktorisering i API Management</span><span class="sxs-lookup"><span data-stu-id="2556e-111"><a name="step1"> </a>Configure an OAuth 2.0 authorization server in API Management</span></span>
<span data-ttu-id="2556e-112">tooget har startats klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2556e-112">tooget started, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span>

![Utgivarportalen][api-management-management-console]

> [!NOTE]
> <span data-ttu-id="2556e-114">Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [Kom igång med Azure API Management] [ Get started with Azure API Management] kursen.</span><span class="sxs-lookup"><span data-stu-id="2556e-114">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="2556e-115">Klicka på **säkerhet** från hello **API Management** menyn hello vänster, klicka på **OAuth 2.0**, och klicka sedan på **Lägg till auktorisering server**.</span><span class="sxs-lookup"><span data-stu-id="2556e-115">Click **Security** from hello **API Management** menu on hello left, click **OAuth 2.0**, and then click **Add authorization server**.</span></span>

![OAuth 2.0][api-management-oauth2]

<span data-ttu-id="2556e-117">När du klickar på **Lägg till auktorisering server**, hello nya auktorisering server formuläret visas.</span><span class="sxs-lookup"><span data-stu-id="2556e-117">After clicking **Add authorization server**, hello new authorization server form is displayed.</span></span>

![Ny server][api-management-oauth2-server-1]

<span data-ttu-id="2556e-119">Ange ett namn och en valfri beskrivning i hello **namn** och **beskrivning** fält.</span><span class="sxs-lookup"><span data-stu-id="2556e-119">Enter a name and an optional description in hello **Name** and **Description** fields.</span></span> 

> [!NOTE]
> <span data-ttu-id="2556e-120">Dessa fält är används tooidentify hello OAuth 2.0 auktorisering server inom hello aktuella API Management service-instans och deras värden kommer inte från hello OAuth 2.0-server.</span><span class="sxs-lookup"><span data-stu-id="2556e-120">These fields are used tooidentify hello OAuth 2.0 authorization server within hello current API Management service instance and their values do not come from hello OAuth 2.0 server.</span></span>
> 
> 

<span data-ttu-id="2556e-121">Ange hello **klienten URL för registrering**.</span><span class="sxs-lookup"><span data-stu-id="2556e-121">Enter hello **Client registration page URL**.</span></span> <span data-ttu-id="2556e-122">Den här sidan är där användare kan skapa och hantera sina konton och varierar beroende på hello OAuth 2.0-providern som används.</span><span class="sxs-lookup"><span data-stu-id="2556e-122">This page is where users can create and manage their accounts, and varies depending on hello OAuth 2.0 provider used.</span></span> <span data-ttu-id="2556e-123">Hej **klienten URL för registrering** toohello sidan som användarna kan använda toocreate och konfigurera sina egna konton för OAuth 2.0-leverantörer som stöder Användarhantering av konton.</span><span class="sxs-lookup"><span data-stu-id="2556e-123">hello **Client registration page URL** points toohello page that users can use toocreate and configure their own accounts for OAuth 2.0 providers that support user management of accounts.</span></span> <span data-ttu-id="2556e-124">Vissa organisationer konfigurera eller använda den här funktionen, även om detta hello OAuth 2.0-providern stöds inte.</span><span class="sxs-lookup"><span data-stu-id="2556e-124">Some organizations do not configure or use this functionality even if hello OAuth 2.0 provider supports it.</span></span> <span data-ttu-id="2556e-125">Om OAuth 2.0-providern inte har användarhantering för konton som har konfigurerats, ange en platshållare URL här, till exempel hello URL för ditt företag eller en URL som `https://placeholder.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="2556e-125">If your OAuth 2.0 provider does not have user management of accounts configured, enter a placeholder URL here such as hello URL of your company, or a URL such as `https://placeholder.contoso.com`.</span></span>

<span data-ttu-id="2556e-126">hello nästa avsnitt av hello formuläret innehåller hello **Auktoriseringskoden bevilja typer**, **slutpunkts-URL-auktorisering**, och **begäran auktoriseringsmetod** inställningar.</span><span class="sxs-lookup"><span data-stu-id="2556e-126">hello next section of hello form contains hello **Authorization code grant types**, **Authorization endpoint URL**, and **Authorization request method** settings.</span></span>

![Ny server][api-management-oauth2-server-2]

<span data-ttu-id="2556e-128">Ange hello **Auktoriseringskoden bevilja typer** genom att kontrollera hello önskad typer.</span><span class="sxs-lookup"><span data-stu-id="2556e-128">Specify hello **Authorization code grant types** by checking hello desired types.</span></span> <span data-ttu-id="2556e-129">**Auktoriseringskoden** har angetts som standard.</span><span class="sxs-lookup"><span data-stu-id="2556e-129">**Authorization code** is specified by default.</span></span>

<span data-ttu-id="2556e-130">Ange hello **slutpunkts-URL-auktorisering**.</span><span class="sxs-lookup"><span data-stu-id="2556e-130">Enter hello **Authorization endpoint URL**.</span></span> <span data-ttu-id="2556e-131">Azure Active Directory, URL: en kommer att vara liknande toohello följande URL, där `<client_id>` ersätts med hello klient-id som identifierar toohello OAuth 2.0-programservern.</span><span class="sxs-lookup"><span data-stu-id="2556e-131">For Azure Active Directory, this URL will be similar toohello following URL, where `<client_id>` is replaced with hello client id that identifies your application toohello OAuth 2.0 server.</span></span>

`https://login.microsoftonline.com/<client_id>/oauth2/authorize`

<span data-ttu-id="2556e-132">Hej **begäran auktoriseringsmetod** anger hur hello auktoriseringsbegäran skickas toohello OAuth 2.0-server.</span><span class="sxs-lookup"><span data-stu-id="2556e-132">hello **Authorization request method** specifies how hello authorization request is sent toohello OAuth 2.0 server.</span></span> <span data-ttu-id="2556e-133">Som standard **hämta** är markerad.</span><span class="sxs-lookup"><span data-stu-id="2556e-133">By default **GET** is selected.</span></span>

<span data-ttu-id="2556e-134">hello nästa avsnitt är där hello **Token slutpunkts-URL**, **klienten autentiseringsmetoder**, **åtkomsttoken skickar metoden**, och **standard scope** har angetts.</span><span class="sxs-lookup"><span data-stu-id="2556e-134">hello next section is where hello **Token endpoint URL**, **Client authentication methods**, **Access token sending method**, and **Default scope** are specified.</span></span>

![Ny server][api-management-oauth2-server-3]

<span data-ttu-id="2556e-136">En Azure Active Directory OAuth 2.0-server hello **Token slutpunkts-URL** har hello följande format, där `<APPID>` har hello `yourapp.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="2556e-136">For an Azure Active Directory OAuth 2.0 server, hello **Token endpoint URL** will have hello following format, where `<APPID>`  has hello format of `yourapp.onmicrosoft.com`.</span></span>

`https://login.microsoftonline.com/<APPID>/oauth2/token`

<span data-ttu-id="2556e-137">Hej standardinställningen för **klienten autentiseringsmetoder** är **grundläggande**, och **åtkomsttoken skickar metoden** är **Authorization-huvud**.</span><span class="sxs-lookup"><span data-stu-id="2556e-137">hello default setting for **Client authentication methods** is **Basic**, and  **Access token sending method** is **Authorization header**.</span></span> <span data-ttu-id="2556e-138">Dessa värden har konfigurerats på den här delen av hello formuläret, tillsammans med hello **standard scope**.</span><span class="sxs-lookup"><span data-stu-id="2556e-138">These values are configured on this section of hello form, along with hello **Default scope**.</span></span>

<span data-ttu-id="2556e-139">Hej **klientautentiseringsuppgifterna** avsnitt innehåller hello **klient-ID** och **klienthemlighet**, som erhålls under hello skapande och konfiguration av OAuth 2.0 Server.</span><span class="sxs-lookup"><span data-stu-id="2556e-139">hello **Client credentials** section contains hello **Client ID** and **Client secret**, which are obtained during hello creation and configuration process of your OAuth 2.0 server.</span></span> <span data-ttu-id="2556e-140">En gång hello **klient-ID** och **klienthemlighet** anges, hello **redirect_uri** för hello **Auktoriseringskoden** genereras.</span><span class="sxs-lookup"><span data-stu-id="2556e-140">Once hello **Client ID** and **Client secret** are specified, hello **redirect_uri** for hello **authorization code** is generated.</span></span> <span data-ttu-id="2556e-141">Den här URI är används tooconfigure hello reply-URL i serverkonfigurationen OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="2556e-141">This URI is used tooconfigure hello reply URL in your OAuth 2.0 server configuration.</span></span>

![Ny server][api-management-oauth2-server-4]

<span data-ttu-id="2556e-143">Om **Auktoriseringskoden bevilja typer** har angetts för**resurs ägarlösenord**, hello **lösenordsinformation för resurs-ägare** avsnittet är används toospecify som de tilldelats. Annars kan du lämna det tomt.</span><span class="sxs-lookup"><span data-stu-id="2556e-143">If **Authorization code grant types** is set too**Resource owner password**, hello **Resource owner password credentials** section is used toospecify those credentials; otherwise you can leave it blank.</span></span>

![Ny server][api-management-oauth2-server-5]

<span data-ttu-id="2556e-145">När hello formuläret är klar klickar du på **spara** toosave hello API Management OAuth 2.0-auktorisering serverkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="2556e-145">Once hello form is complete, click **Save** toosave hello API Management OAuth 2.0 authorization server configuration.</span></span> <span data-ttu-id="2556e-146">När du har sparat hello serverkonfiguration kan du konfigurera API: er toouse den här konfigurationen enligt hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2556e-146">Once hello server configuration is saved, you can configure APIs toouse this configuration, as shown in hello next section.</span></span>

## <span data-ttu-id="2556e-147"><a name="step2"></a>Konfigurera en API-toouse OAuth 2.0 användarautentiseringen</span><span class="sxs-lookup"><span data-stu-id="2556e-147"><a name="step2"> </a>Configure an API toouse OAuth 2.0 user authorization</span></span>
<span data-ttu-id="2556e-148">Klicka på **API: er** från hello **API Management** menyn på hello vänster, klicka på hello namnet på hello önskad API, **säkerhet**, och sedan kryssrutan hello för **OAuth 2.0**.</span><span class="sxs-lookup"><span data-stu-id="2556e-148">Click **APIs** from hello **API Management** menu on hello left, click hello name of hello desired API, click **Security**, and then check hello box for **OAuth 2.0**.</span></span>

![Auktoriseringen av användaren][api-management-user-authorization]

<span data-ttu-id="2556e-150">Välj hello önskad **auktorisering server** hello listrutan och klickar på **spara**.</span><span class="sxs-lookup"><span data-stu-id="2556e-150">Select hello desired **Authorization server** from hello drop-down list, and click **Save**.</span></span>

![Auktoriseringen av användaren][api-management-user-authorization-save]

## <span data-ttu-id="2556e-152"><a name="step3"></a>Testa hello OAuth 2.0 användarautentiseringen i hello Developer-portalen</span><span class="sxs-lookup"><span data-stu-id="2556e-152"><a name="step3"> </a>Test hello OAuth 2.0 user authorization in hello Developer Portal</span></span>
<span data-ttu-id="2556e-153">När du har konfigurerat OAuth 2.0 auktorisering servern och konfigurerat API-toouse servern, kan du testa den genom att gå toohello Developer-portalen och anropar en API.</span><span class="sxs-lookup"><span data-stu-id="2556e-153">Once you have configured your OAuth 2.0 authorization server and configured your API toouse that server, you can test it by going toohello Developer Portal and calling an API.</span></span>  <span data-ttu-id="2556e-154">Klicka på **utvecklarportalen** i hello övre högra menyn.</span><span class="sxs-lookup"><span data-stu-id="2556e-154">Click **Developer portal** in hello top right menu.</span></span>

![Utvecklarportalen][api-management-developer-portal-menu]

<span data-ttu-id="2556e-156">Klicka på **API: er** i hello översta menyn och välj **Echo API**.</span><span class="sxs-lookup"><span data-stu-id="2556e-156">Click **APIs** in hello top menu and select **Echo API**.</span></span>

![Echo API][api-management-apis-echo-api]

> [!NOTE]
> <span data-ttu-id="2556e-158">Om du har bara en API som konfigurerats eller synliga tooyour konto, klicka på API: er kommer du direkt toohello åtgärder för att API.</span><span class="sxs-lookup"><span data-stu-id="2556e-158">If you have only one API configured or visible tooyour account, then clicking APIs takes you directly toohello operations for that API.</span></span>
> 
> 

<span data-ttu-id="2556e-159">Välj hello **hämta resurs** åtgärden, klickar du på **öppna konsolen**, och välj sedan **Auktoriseringskoden** hello i listrutan.</span><span class="sxs-lookup"><span data-stu-id="2556e-159">Select hello **GET Resource** operation, click **Open Console**, and then select **Authorization code** from hello drop-down.</span></span>

![Öppna konsolen][api-management-open-console]

<span data-ttu-id="2556e-161">När **Auktoriseringskoden** är markerat visas ett popup-fönster med hello inloggning form av hello OAuth 2.0-providern.</span><span class="sxs-lookup"><span data-stu-id="2556e-161">When **Authorization code** is selected, a pop-up window is displayed with hello sign-in form of hello OAuth 2.0 provider.</span></span> <span data-ttu-id="2556e-162">I det här exemplet tillhandahålls hello inloggning formuläret av Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2556e-162">In this example hello sign-in form is provided by Azure Active Directory.</span></span>

> [!NOTE]
> <span data-ttu-id="2556e-163">Om du har popup-fönster har inaktiverats kommer du att uppmanas tooenable dem hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="2556e-163">If you have pop-ups disabled you will be prompted tooenable them by hello browser.</span></span> <span data-ttu-id="2556e-164">När du har aktiverat dem, Välj **Auktoriseringskoden** igen och hello inloggning formuläret visas.</span><span class="sxs-lookup"><span data-stu-id="2556e-164">After you enable them, select **Authorization code** again and hello sign-in form will be displayed.</span></span>
> 
> 

![Logga in][api-management-oauth2-signin]

<span data-ttu-id="2556e-166">När du har loggat in hello **Begäransrubriker** fylls i med en `Authorization : Bearer` huvud som tillåter hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="2556e-166">Once you have signed in, hello **Request headers** are populated with an `Authorization : Bearer` header that authorizes hello request.</span></span>

![Token för anslutningsbegäran sidhuvud][api-management-request-header-token]

<span data-ttu-id="2556e-168">Du kan nu konfigurera hello önskade värden för hello återstående parametrar och skicka begäran om hello.</span><span class="sxs-lookup"><span data-stu-id="2556e-168">At this point you can configure hello desired values for hello remaining parameters, and submit hello request.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="2556e-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2556e-169">Next steps</span></span>
<span data-ttu-id="2556e-170">Mer information om att använda OAuth 2.0 och API-hantering finns hello följande video och tillhörande [artikel](api-management-howto-protect-backend-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="2556e-170">For more information about using OAuth 2.0 and API Management, see hello following video and accompanying [article](api-management-howto-protect-backend-with-aad.md).</span></span>

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

