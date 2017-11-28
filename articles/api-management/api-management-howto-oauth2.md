---
title: "Auktorisera developer konton med hjälp av OAuth 2.0 i Azure API Management | Microsoft Docs"
description: "Lär dig mer om att auktorisera användare som använder OAuth 2.0 i API-hantering."
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
ms.openlocfilehash: a19c453bb3271374b587f3d0b35adad55863b490
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-authorize-developer-accounts-using-oauth-20-in-azure-api-management"></a><span data-ttu-id="a4dd0-103">Så här auktoriserar developer konton med hjälp av OAuth 2.0 i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="a4dd0-103">How to authorize developer accounts using OAuth 2.0 in Azure API Management</span></span>
<span data-ttu-id="a4dd0-104">Stöd för många API: er [OAuth 2.0](http://oauth.net/2/) att skydda API: et och se till att endast giltigt användare har åtkomst och de kan bara komma åt resurser som de är rätt.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-104">Many APIs support [OAuth 2.0](http://oauth.net/2/) to secure the API and ensure that only valid users have access, and they can only access resources to which they're entitled.</span></span> <span data-ttu-id="a4dd0-105">För att kunna använda Azure API Management interaktiva Utvecklarkonsolen med dessa API: er, kan tjänsten du konfigurera service-instans för att fungera med din OAuth 2.0 aktiverade API.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-105">In order to use Azure API Management's interactive Developer Console with such APIs, the service allows you to configure your service instance to work with your OAuth 2.0 enabled API.</span></span>

## <span data-ttu-id="a4dd0-106"><a name="prerequisites"></a>Krav</span><span class="sxs-lookup"><span data-stu-id="a4dd0-106"><a name="prerequisites"> </a>Prerequisites</span></span>
<span data-ttu-id="a4dd0-107">Den här guiden visar hur du konfigurerar API Management service-instans för att använda OAuth 2.0-auktorisering för utvecklare konton, men visar inte hur du konfigurerar en OAuth 2.0-provider.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-107">This guide shows you how to configure your API Management service instance to use OAuth 2.0 authorization for developer accounts, but does not show you how to configure an OAuth 2.0 provider.</span></span> <span data-ttu-id="a4dd0-108">Konfigurationen för varje OAuth 2.0-provider är olika, men stegen är liknande och nödvändiga typer av information som används när du konfigurerar OAuth 2.0 i din API Management service-instans är samma.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-108">The configuration for each OAuth 2.0 provider is different, although the steps are similar, and the required pieces of information used in configuring OAuth 2.0 in your API Management service instance are the same.</span></span> <span data-ttu-id="a4dd0-109">Det här avsnittet visas exempel som använder Azure Active Directory som en OAuth 2.0-provider.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-109">This topic shows examples using Azure Active Directory as an OAuth 2.0 provider.</span></span>

> [!NOTE]
> <span data-ttu-id="a4dd0-110">Mer information om hur du konfigurerar OAuth 2.0 med Azure Active Directory finns i [WebApp-GraphAPI-DotNet] [ WebApp-GraphAPI-DotNet] exempel.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-110">For more information on configuring OAuth 2.0 using Azure Active Directory, see the [WebApp-GraphAPI-DotNet][WebApp-GraphAPI-DotNet] sample.</span></span>
> 
> 

## <span data-ttu-id="a4dd0-111"><a name="step1"></a>Konfigurera en server för OAuth 2.0-auktorisering i API Management</span><span class="sxs-lookup"><span data-stu-id="a4dd0-111"><a name="step1"> </a>Configure an OAuth 2.0 authorization server in API Management</span></span>
<span data-ttu-id="a4dd0-112">Börja genom att klicka på **Publisher-portal** på Azure Portal för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-112">To get started, click **Publisher portal** in the Azure Portal for your API Management service.</span></span>

![Utgivarportalen][api-management-management-console]

> [!NOTE]
> <span data-ttu-id="a4dd0-114">Om du inte har skapat en API Management-tjänstinstans än läser du [Skapa en API Management-tjänstinstans][Create an API Management service instance] i självstudiekursen [Komma igång med Azure API Management][Get started with Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="a4dd0-114">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in the [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="a4dd0-115">Klicka på **säkerhet** från den **API Management** menyn till vänster, klickar på **OAuth 2.0**, och klicka sedan på **Lägg till auktorisering server**.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-115">Click **Security** from the **API Management** menu on the left, click **OAuth 2.0**, and then click **Add authorization server**.</span></span>

![OAuth 2.0][api-management-oauth2]

<span data-ttu-id="a4dd0-117">När du klickar på **Lägg till auktorisering server**, nya auktorisering server formuläret visas.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-117">After clicking **Add authorization server**, the new authorization server form is displayed.</span></span>

![Ny server][api-management-oauth2-server-1]

<span data-ttu-id="a4dd0-119">Ange ett namn och en valfri beskrivning i den **namn** och **beskrivning** fält.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-119">Enter a name and an optional description in the **Name** and **Description** fields.</span></span> 

> [!NOTE]
> <span data-ttu-id="a4dd0-120">Fälten som används för att identifiera OAuth 2.0 auktorisering server inom den aktuella tjänstinstansen för API Management och deras värden kommer inte från OAuth 2.0-server.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-120">These fields are used to identify the OAuth 2.0 authorization server within the current API Management service instance and their values do not come from the OAuth 2.0 server.</span></span>
> 
> 

<span data-ttu-id="a4dd0-121">Ange den **klienten URL för registrering**.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-121">Enter the **Client registration page URL**.</span></span> <span data-ttu-id="a4dd0-122">Den här sidan är där användare kan skapa och hantera sina konton och varierar beroende på OAuth 2.0-providern som används.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-122">This page is where users can create and manage their accounts, and varies depending on the OAuth 2.0 provider used.</span></span> <span data-ttu-id="a4dd0-123">Den **klienten URL för registrering** pekar på den sida som användare kan använda för att skapa och konfigurera sina egna konton för OAuth 2.0-leverantörer som stöder Användarhantering av konton.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-123">The **Client registration page URL** points to the page that users can use to create and configure their own accounts for OAuth 2.0 providers that support user management of accounts.</span></span> <span data-ttu-id="a4dd0-124">Vissa organisationer konfigurera eller använda den här funktionen även om OAuth 2.0-providern stöds inte.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-124">Some organizations do not configure or use this functionality even if the OAuth 2.0 provider supports it.</span></span> <span data-ttu-id="a4dd0-125">Om OAuth 2.0-providern inte har användarhantering för konton som har konfigurerats, ange en platshållare URL här som URL för ditt företag eller en URL som `https://placeholder.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-125">If your OAuth 2.0 provider does not have user management of accounts configured, enter a placeholder URL here such as the URL of your company, or a URL such as `https://placeholder.contoso.com`.</span></span>

<span data-ttu-id="a4dd0-126">Nästa avsnitt i formuläret innehåller den **Auktoriseringskoden bevilja typer**, **slutpunkts-URL-auktorisering**, och **begäran auktoriseringsmetod** inställningar.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-126">The next section of the form contains the **Authorization code grant types**, **Authorization endpoint URL**, and **Authorization request method** settings.</span></span>

![Ny server][api-management-oauth2-server-2]

<span data-ttu-id="a4dd0-128">Ange den **Auktoriseringskoden bevilja typer** genom att markera de önskade typerna.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-128">Specify the **Authorization code grant types** by checking the desired types.</span></span> <span data-ttu-id="a4dd0-129">**Auktoriseringskoden** har angetts som standard.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-129">**Authorization code** is specified by default.</span></span>

<span data-ttu-id="a4dd0-130">Ange den **slutpunkts-URL-auktorisering**.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-130">Enter the **Authorization endpoint URL**.</span></span> <span data-ttu-id="a4dd0-131">URL: en för Azure Active Directory, kommer att likna följande URL, där `<client_id>` ersätts med klient-id som identifierar ditt program till OAuth 2.0-servern.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-131">For Azure Active Directory, this URL will be similar to the following URL, where `<client_id>` is replaced with the client id that identifies your application to the OAuth 2.0 server.</span></span>

`https://login.microsoftonline.com/<client_id>/oauth2/authorize`

<span data-ttu-id="a4dd0-132">Den **begäran auktoriseringsmetod** anger hur auktoriseringsbegäran skickas till OAuth 2.0-server.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-132">The **Authorization request method** specifies how the authorization request is sent to the OAuth 2.0 server.</span></span> <span data-ttu-id="a4dd0-133">Som standard **hämta** är markerad.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-133">By default **GET** is selected.</span></span>

<span data-ttu-id="a4dd0-134">I nästa avsnitt är var den **Token slutpunkts-URL**, **klienten autentiseringsmetoder**, **åtkomsttoken skickar metoden**, och **standard scope**har angetts.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-134">The next section is where the **Token endpoint URL**, **Client authentication methods**, **Access token sending method**, and **Default scope** are specified.</span></span>

![Ny server][api-management-oauth2-server-3]

<span data-ttu-id="a4dd0-136">För en Azure Active Directory OAuth 2.0-server i **Token slutpunkts-URL** har följande format, där `<APPID>` har formatet för `yourapp.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-136">For an Azure Active Directory OAuth 2.0 server, the **Token endpoint URL** will have the following format, where `<APPID>`  has the format of `yourapp.onmicrosoft.com`.</span></span>

`https://login.microsoftonline.com/<APPID>/oauth2/token`

<span data-ttu-id="a4dd0-137">Standardinställningen för **klienten autentiseringsmetoder** är **grundläggande**, och **åtkomsttoken skickar metoden** är **Authorization-huvud**.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-137">The default setting for **Client authentication methods** is **Basic**, and  **Access token sending method** is **Authorization header**.</span></span> <span data-ttu-id="a4dd0-138">Dessa värden är konfigurerade på det här avsnittet av formuläret, tillsammans med den **standard scope**.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-138">These values are configured on this section of the form, along with the **Default scope**.</span></span>

<span data-ttu-id="a4dd0-139">Den **klientautentiseringsuppgifterna** avsnittet innehåller den **klient-ID** och **klienthemlighet**, som erhålls under skapande och konfiguration av OAuth 2.0-server .</span><span class="sxs-lookup"><span data-stu-id="a4dd0-139">The **Client credentials** section contains the **Client ID** and **Client secret**, which are obtained during the creation and configuration process of your OAuth 2.0 server.</span></span> <span data-ttu-id="a4dd0-140">En gång i **klient-ID** och **klienthemlighet** har angetts i **redirect_uri** för den **Auktoriseringskoden** genereras.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-140">Once the **Client ID** and **Client secret** are specified, the **redirect_uri** for the **authorization code** is generated.</span></span> <span data-ttu-id="a4dd0-141">URI: N används för att konfigurera reply-URL i serverkonfigurationen OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-141">This URI is used to configure the reply URL in your OAuth 2.0 server configuration.</span></span>

![Ny server][api-management-oauth2-server-4]

<span data-ttu-id="a4dd0-143">Om **Auktoriseringskoden bevilja typer** är inställd på **resurs ägarlösenord**, **lösenordsinformation för resurs-ägare** avsnitt används för att ange autentiseringsuppgifterna; Annars kan du lämna det tomt.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-143">If **Authorization code grant types** is set to **Resource owner password**, the **Resource owner password credentials** section is used to specify those credentials; otherwise you can leave it blank.</span></span>

![Ny server][api-management-oauth2-server-5]

<span data-ttu-id="a4dd0-145">När formuläret är klar klickar du på **spara** spara serverkonfiguration för API Management OAuth 2.0-auktorisering.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-145">Once the form is complete, click **Save** to save the API Management OAuth 2.0 authorization server configuration.</span></span> <span data-ttu-id="a4dd0-146">När serverkonfigurationen sparas kan konfigurera du API: er för att använda den här konfigurationen som visas i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-146">Once the server configuration is saved, you can configure APIs to use this configuration, as shown in the next section.</span></span>

## <span data-ttu-id="a4dd0-147"><a name="step2"></a>Konfigurera en API för att använda OAuth 2.0 användarautentiseringen</span><span class="sxs-lookup"><span data-stu-id="a4dd0-147"><a name="step2"> </a>Configure an API to use OAuth 2.0 user authorization</span></span>
<span data-ttu-id="a4dd0-148">Klicka på **API: er** från den **API Management** Klicka på menyn till vänster, klickar på namnet på den önskade API **säkerhet**, och sedan markera kryssrutan för **OAuth 2.0**.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-148">Click **APIs** from the **API Management** menu on the left, click the name of the desired API, click **Security**, and then check the box for **OAuth 2.0**.</span></span>

![Auktoriseringen av användaren][api-management-user-authorization]

<span data-ttu-id="a4dd0-150">Välj den önskade **auktorisering server** i listrutan och klickar på **spara**.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-150">Select the desired **Authorization server** from the drop-down list, and click **Save**.</span></span>

![Auktoriseringen av användaren][api-management-user-authorization-save]

## <span data-ttu-id="a4dd0-152"><a name="step3"></a>Testa användarautentiseringen OAuth 2.0 i Developer-portalen</span><span class="sxs-lookup"><span data-stu-id="a4dd0-152"><a name="step3"> </a>Test the OAuth 2.0 user authorization in the Developer Portal</span></span>
<span data-ttu-id="a4dd0-153">När du har konfigurerat OAuth 2.0 auktorisering servern och konfigurerat ditt API för att använda den servern kan testa du den genom att gå till Developer-portalen och anropar en API.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-153">Once you have configured your OAuth 2.0 authorization server and configured your API to use that server, you can test it by going to the Developer Portal and calling an API.</span></span>  <span data-ttu-id="a4dd0-154">Klicka på **Utvecklarportal** på menyn längst upp till höger.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-154">Click **Developer portal** in the top right menu.</span></span>

![Utvecklarportalen][api-management-developer-portal-menu]

<span data-ttu-id="a4dd0-156">Klicka på **API: er** i den översta menyn och välj **Echo API**.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-156">Click **APIs** in the top menu and select **Echo API**.</span></span>

![Echo API][api-management-apis-echo-api]

> [!NOTE]
> <span data-ttu-id="a4dd0-158">Om du bara har ett API som är konfigurerat eller som visas för ditt konto och du klickar på API:er så kommer du direkt till åtgärderna för det API:et.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-158">If you have only one API configured or visible to your account, then clicking APIs takes you directly to the operations for that API.</span></span>
> 
> 

<span data-ttu-id="a4dd0-159">Välj den **hämta resurs** åtgärden, klickar du på **öppna konsolen**, och välj sedan **Auktoriseringskoden** från listrutan.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-159">Select the **GET Resource** operation, click **Open Console**, and then select **Authorization code** from the drop-down.</span></span>

![Öppna konsolen][api-management-open-console]

<span data-ttu-id="a4dd0-161">När **Auktoriseringskoden** är markerat visas ett popup-fönster med formuläret inloggning av OAuth 2.0-providern.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-161">When **Authorization code** is selected, a pop-up window is displayed with the sign-in form of the OAuth 2.0 provider.</span></span> <span data-ttu-id="a4dd0-162">I det här exemplet tillhandahålls formuläret inloggning av Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-162">In this example the sign-in form is provided by Azure Active Directory.</span></span>

> [!NOTE]
> <span data-ttu-id="a4dd0-163">Om du har inaktiverat popup-fönster uppmanas du att aktivera dem i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-163">If you have pop-ups disabled you will be prompted to enable them by the browser.</span></span> <span data-ttu-id="a4dd0-164">När du har aktiverat dem, Välj **Auktoriseringskoden** igen och logga in formuläret visas.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-164">After you enable them, select **Authorization code** again and the sign-in form will be displayed.</span></span>
> 
> 

![Logga in][api-management-oauth2-signin]

<span data-ttu-id="a4dd0-166">När du har loggat in, den **Begäransrubriker** fylls i med en `Authorization : Bearer` huvud som godkänner begäran.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-166">Once you have signed in, the **Request headers** are populated with an `Authorization : Bearer` header that authorizes the request.</span></span>

![Token för anslutningsbegäran sidhuvud][api-management-request-header-token]

<span data-ttu-id="a4dd0-168">Du kan nu konfigurera önskade värden för parametrarna återstående och skicka din begäran.</span><span class="sxs-lookup"><span data-stu-id="a4dd0-168">At this point you can configure the desired values for the remaining parameters, and submit the request.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="a4dd0-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a4dd0-169">Next steps</span></span>
<span data-ttu-id="a4dd0-170">Mer information om att använda OAuth 2.0 och API-hantering finns i följande video och tillhörande [artikel](api-management-howto-protect-backend-with-aad.md).</span><span class="sxs-lookup"><span data-stu-id="a4dd0-170">For more information about using OAuth 2.0 and API Management, see the following video and accompanying [article](api-management-howto-protect-backend-with-aad.md).</span></span>

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


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

