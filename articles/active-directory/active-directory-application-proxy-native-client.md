---
title: Publicera native client - appar i Azure AD | Microsoft Docs
description: "Beskriver hur du aktiverar inbyggd klientprogram att kommunicera med Azure AD Application Proxy Connector att tillhandahålla säker fjärråtkomst till lokala appar."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: f0cae145-e346-4126-948f-3f699747b96e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: bdaa5af6ff5331bc310499586615b48a864c3c5e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a><span data-ttu-id="a7d11-103">Så här aktiverar du native client appar kan interagera med proxy program</span><span class="sxs-lookup"><span data-stu-id="a7d11-103">How to enable native client apps to interact with proxy Applications</span></span>

<span data-ttu-id="a7d11-104">Förutom webbprogram, kan Azure Active Directory Application Proxy också användas att publicera appar-klienten.</span><span class="sxs-lookup"><span data-stu-id="a7d11-104">In addition to web applications, Azure Active Directory Application Proxy can also be used to publish native client apps.</span></span> <span data-ttu-id="a7d11-105">Native client-appar skiljer sig från webbprogram eftersom de är installerade på en enhet, medan webbappar kan nås via en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="a7d11-105">Native client apps differ from web apps because they are installed on a device, while web apps are accessed through a browser.</span></span> 

<span data-ttu-id="a7d11-106">Application Proxy stöder interna klientprogram av accepterar Azure AD som utfärdade token som skickas i standard godkänner HTTP-huvuden.</span><span class="sxs-lookup"><span data-stu-id="a7d11-106">Application Proxy supports native client apps by accepting Azure AD issued tokens that are sent in standard Authorize HTTP headers.</span></span>

![Förhållandet mellan användare och Azure Active Directory publicerade program](./media/active-directory-application-proxy-native-client/richclientflow.png)

<span data-ttu-id="a7d11-108">Använda Azure AD Authentication Library, som tar hand om autentisering och stöder många klienten miljöer, publicering av interna program.</span><span class="sxs-lookup"><span data-stu-id="a7d11-108">Use the Azure AD Authentication Library, which takes care of authentication and supports many client environments, to publish native applications.</span></span> <span data-ttu-id="a7d11-109">Programproxy passar in i den [programspecifika Web API-scenariot](develop/active-directory-authentication-scenarios.md#native-application-to-web-api).</span><span class="sxs-lookup"><span data-stu-id="a7d11-109">Application Proxy fits into the [Native Application to Web API scenario](develop/active-directory-authentication-scenarios.md#native-application-to-web-api).</span></span> <span data-ttu-id="a7d11-110">Den här artikeln vägleder dig genom fyra stegen för att publicera en interna program med Application Proxy och Azure AD-Autentiseringsbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="a7d11-110">This article walks you through the four steps to publish a native application with Application Proxy and the Azure AD Authentication Library.</span></span> 

## <a name="step-1-publish-your-application"></a><span data-ttu-id="a7d11-111">Steg 1: Publicera programmet</span><span class="sxs-lookup"><span data-stu-id="a7d11-111">Step 1: Publish your application</span></span>
<span data-ttu-id="a7d11-112">Publicera programmet proxy precis som alla andra program och tilldela användare åtkomst till ditt program.</span><span class="sxs-lookup"><span data-stu-id="a7d11-112">Publish your proxy application as you would any other application and assign users to access your application.</span></span> <span data-ttu-id="a7d11-113">Mer information finns i [publicera program med programproxy](active-directory-application-proxy-publish.md).</span><span class="sxs-lookup"><span data-stu-id="a7d11-113">For more information, see [Publish applications with Application Proxy](active-directory-application-proxy-publish.md).</span></span>

## <a name="step-2-configure-your-application"></a><span data-ttu-id="a7d11-114">Steg 2: Konfigurera ditt program</span><span class="sxs-lookup"><span data-stu-id="a7d11-114">Step 2: Configure your application</span></span>
<span data-ttu-id="a7d11-115">Konfigurera ditt interna program på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a7d11-115">Configure your native application as follows:</span></span>

1. <span data-ttu-id="a7d11-116">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a7d11-116">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="a7d11-117">Gå till **Azure Active Directory** > **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="a7d11-117">Navigate to **Azure Active Directory** > **App registrations**.</span></span>
3. <span data-ttu-id="a7d11-118">Välj **nya appregistrering**.</span><span class="sxs-lookup"><span data-stu-id="a7d11-118">Select **New application registration**.</span></span>
4. <span data-ttu-id="a7d11-119">Ange ett namn för ditt program, Välj **interna** som programtyp, och ange omdirigerings-URI för ditt program.</span><span class="sxs-lookup"><span data-stu-id="a7d11-119">Specify a name for your application, select **Native** as the application type, and provide the Redirect URI for your application.</span></span> 

   ![Skapa en ny appregistrering](./media/active-directory-application-proxy-native-client/create.png)
5. <span data-ttu-id="a7d11-121">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a7d11-121">Select **Create**.</span></span>

<span data-ttu-id="a7d11-122">Mer detaljerad information om hur du skapar en ny appregistrering finns [integrera program med Azure Active Directory](.//develop/active-directory-integrating-applications.md).</span><span class="sxs-lookup"><span data-stu-id="a7d11-122">For more detailed information about creating a new app registration, see [Integrating applications with Azure Active Directory](.//develop/active-directory-integrating-applications.md).</span></span>


## <a name="step-3-grant-access-to-other-applications"></a><span data-ttu-id="a7d11-123">Steg 3: Ge tillgång till andra program</span><span class="sxs-lookup"><span data-stu-id="a7d11-123">Step 3: Grant access to other applications</span></span>
<span data-ttu-id="a7d11-124">Aktivera det ursprungliga programmet kan exponeras för andra program i din katalog:</span><span class="sxs-lookup"><span data-stu-id="a7d11-124">Enable the native application to be exposed to other applications in your directory:</span></span>

1. <span data-ttu-id="a7d11-125">Fortfarande i **App registreringar**, Välj ny interna program som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="a7d11-125">Still in **App registrations**, select the new native application that you just created.</span></span>
2. <span data-ttu-id="a7d11-126">Välj **nödvändiga behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="a7d11-126">Select **Required permissions**.</span></span>
3. <span data-ttu-id="a7d11-127">Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a7d11-127">Select **Add**.</span></span>
4. <span data-ttu-id="a7d11-128">Öppna det första steget **väljer en API**.</span><span class="sxs-lookup"><span data-stu-id="a7d11-128">Open the first step, **Select an API**.</span></span>
5. <span data-ttu-id="a7d11-129">Använd sökfältet för att hitta Application Proxy-appen som du har publicerat i det första avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a7d11-129">Use the search bar to find the Application Proxy app that you published in the first section.</span></span> <span data-ttu-id="a7d11-130">Välj appen och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="a7d11-130">Choose that app, then click **Select**.</span></span> 

   ![Sök efter den proxy-app](./media/active-directory-application-proxy-native-client/select_api.png)
6. <span data-ttu-id="a7d11-132">Öppna det andra steget **Välj behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="a7d11-132">Open the second step, **Select permissions**.</span></span>
7. <span data-ttu-id="a7d11-133">Använd kryssrutan för att ge din ursprungliga programmet åtkomst till ditt program för proxy och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="a7d11-133">Use the checkbox to grant your native application access to your proxy application, then click **Select**.</span></span>

   ![Bevilja åtkomst till proxy-app](./media/active-directory-application-proxy-native-client/select_perms.png)
8. <span data-ttu-id="a7d11-135">Välj **klar**.</span><span class="sxs-lookup"><span data-stu-id="a7d11-135">Select **Done**.</span></span>


## <a name="step-4-edit-the-active-directory-authentication-library"></a><span data-ttu-id="a7d11-136">Steg 4: Redigera Active Directory Authentication Library</span><span class="sxs-lookup"><span data-stu-id="a7d11-136">Step 4: Edit the Active Directory Authentication Library</span></span>
<span data-ttu-id="a7d11-137">Redigera det ursprungliga programmet koden i kontexten för autentisering av den Active Directory Authentication Library (ADAL) att inkludera följande text:</span><span class="sxs-lookup"><span data-stu-id="a7d11-137">Edit the native application code in the authentication context of the Active Directory Authentication Library (ADAL) to include the following text:</span></span>

```
// Acquire Access Token from AAD for Proxy Application
AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<Tenant ID>");
AuthenticationResult result = authContext.AcquireToken("< External Url of Proxy App >",
        "<App ID of the Native app>",
        new Uri("<Redirect Uri of the Native App>"),
        PromptBehavior.Never);

//Use the Access Token to access the Proxy Application
HttpClient httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");
```

<span data-ttu-id="a7d11-138">Variabler i exempelkoden ska ersättas med följande:</span><span class="sxs-lookup"><span data-stu-id="a7d11-138">The variables in the sample code should be replaced as follows:</span></span>

* <span data-ttu-id="a7d11-139">**Klient-ID** kan hittas i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a7d11-139">**Tenant ID** can be found in the Azure portal.</span></span> <span data-ttu-id="a7d11-140">Gå till **Azure Active Directory** > **egenskaper** och kopiera Directory-ID.</span><span class="sxs-lookup"><span data-stu-id="a7d11-140">Navigate to **Azure Active Directory** > **Properties** and copy the Directory ID.</span></span> 
* <span data-ttu-id="a7d11-141">**Externa URL: en** är frontend-URL du angav i programmets Proxy.</span><span class="sxs-lookup"><span data-stu-id="a7d11-141">**External URL** is the front-end URL you entered in the Proxy Application.</span></span> <span data-ttu-id="a7d11-142">Du hittar det här värdet genom att navigera till den **programproxy** avsnitt av proxy-app.</span><span class="sxs-lookup"><span data-stu-id="a7d11-142">To find this value, navigate to the **Application proxy** section of the proxy app.</span></span>
* <span data-ttu-id="a7d11-143">**App-ID** av den inbyggda appen kan hittas på den **egenskaper** sidan i det ursprungliga programmet.</span><span class="sxs-lookup"><span data-stu-id="a7d11-143">**App ID** of the native app can be found on the **Properties** page of the native application.</span></span>
* <span data-ttu-id="a7d11-144">**Omdirigerings-URI för den inbyggda appen** kan hittas på den **omdirigerings-URI: er** sidan i det ursprungliga programmet.</span><span class="sxs-lookup"><span data-stu-id="a7d11-144">**Redirect URI of the native app** can be found on the **Redirect URIs** page of the native application.</span></span>


## <a name="see-also"></a><span data-ttu-id="a7d11-145">Se även</span><span class="sxs-lookup"><span data-stu-id="a7d11-145">See also</span></span>

<span data-ttu-id="a7d11-146">Mer information om det ursprungliga programmet flödet finns [programspecifika i webb-API](develop/active-directory-authentication-scenarios.md#native-application-to-web-api)</span><span class="sxs-lookup"><span data-stu-id="a7d11-146">For more information about the native application flow, see [Native application to web API](develop/active-directory-authentication-scenarios.md#native-application-to-web-api)</span></span>

<span data-ttu-id="a7d11-147">Läs mer om de senaste nyheterna och uppdateringarna i [bloggen om Application Proxy](http://blogs.technet.com/b/applicationproxyblog/)</span><span class="sxs-lookup"><span data-stu-id="a7d11-147">For the latest news and updates, check out the [Application Proxy blog](http://blogs.technet.com/b/applicationproxyblog/)</span></span>
