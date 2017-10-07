---
title: "aaaPublish native client-appar – Azure AD | Microsoft Docs"
description: "Beskriver hur tooenable native client appar toocommunicate med Azure AD Application Proxy Connector tooprovide säker fjärråtkomst tooyour lokala appar."
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
ms.openlocfilehash: 0ed2be217bf992f034d8321d5e66569b4cace24f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooenable-native-client-apps-toointeract-with-proxy-applications"></a><span data-ttu-id="b0c6c-103">Hur tooenable native client appar toointeract med proxy program</span><span class="sxs-lookup"><span data-stu-id="b0c6c-103">How tooenable native client apps toointeract with proxy Applications</span></span>

<span data-ttu-id="b0c6c-104">Azure Active Directory Application Proxy kan också vara används toopublish native client appar i tillägg tooweb program.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-104">In addition tooweb applications, Azure Active Directory Application Proxy can also be used toopublish native client apps.</span></span> <span data-ttu-id="b0c6c-105">Native client-appar skiljer sig från webbprogram eftersom de är installerade på en enhet, medan webbappar kan nås via en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-105">Native client apps differ from web apps because they are installed on a device, while web apps are accessed through a browser.</span></span> 

<span data-ttu-id="b0c6c-106">Application Proxy stöder interna klientprogram av accepterar Azure AD som utfärdade token som skickas i standard godkänner HTTP-huvuden.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-106">Application Proxy supports native client apps by accepting Azure AD issued tokens that are sent in standard Authorize HTTP headers.</span></span>

![Förhållandet mellan användare och Azure Active Directory publicerade program](./media/active-directory-application-proxy-native-client/richclientflow.png)

<span data-ttu-id="b0c6c-108">Använd hello Azure AD Authentication Library, som tar hand om autentisering och stöder många klienten miljöer, toopublish interna program.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-108">Use hello Azure AD Authentication Library, which takes care of authentication and supports many client environments, toopublish native applications.</span></span> <span data-ttu-id="b0c6c-109">Programproxy passar in i hello [programspecifika tooWeb API scenariot](develop/active-directory-authentication-scenarios.md#native-application-to-web-api).</span><span class="sxs-lookup"><span data-stu-id="b0c6c-109">Application Proxy fits into hello [Native Application tooWeb API scenario](develop/active-directory-authentication-scenarios.md#native-application-to-web-api).</span></span> <span data-ttu-id="b0c6c-110">Den här artikeln vägleder dig genom hello fyra steg toopublish programspecifika med Application Proxy och hello Azure AD-Autentiseringsbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-110">This article walks you through hello four steps toopublish a native application with Application Proxy and hello Azure AD Authentication Library.</span></span> 

## <a name="step-1-publish-your-application"></a><span data-ttu-id="b0c6c-111">Steg 1: Publicera programmet</span><span class="sxs-lookup"><span data-stu-id="b0c6c-111">Step 1: Publish your application</span></span>
<span data-ttu-id="b0c6c-112">Publicera programmet proxy precis som alla andra program och tilldela användare tooaccess ditt program.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-112">Publish your proxy application as you would any other application and assign users tooaccess your application.</span></span> <span data-ttu-id="b0c6c-113">Mer information finns i [publicera program med programproxy](active-directory-application-proxy-publish.md).</span><span class="sxs-lookup"><span data-stu-id="b0c6c-113">For more information, see [Publish applications with Application Proxy](active-directory-application-proxy-publish.md).</span></span>

## <a name="step-2-configure-your-application"></a><span data-ttu-id="b0c6c-114">Steg 2: Konfigurera ditt program</span><span class="sxs-lookup"><span data-stu-id="b0c6c-114">Step 2: Configure your application</span></span>
<span data-ttu-id="b0c6c-115">Konfigurera ditt interna program på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b0c6c-115">Configure your native application as follows:</span></span>

1. <span data-ttu-id="b0c6c-116">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b0c6c-116">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="b0c6c-117">Navigera för**Azure Active Directory** > **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-117">Navigate too**Azure Active Directory** > **App registrations**.</span></span>
3. <span data-ttu-id="b0c6c-118">Välj **nya appregistrering**.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-118">Select **New application registration**.</span></span>
4. <span data-ttu-id="b0c6c-119">Ange ett namn för ditt program, Välj **interna** som hello programtyp, och ange hello omdirigerings-URI för ditt program.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-119">Specify a name for your application, select **Native** as hello application type, and provide hello Redirect URI for your application.</span></span> 

   ![Skapa en ny appregistrering](./media/active-directory-application-proxy-native-client/create.png)
5. <span data-ttu-id="b0c6c-121">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-121">Select **Create**.</span></span>

<span data-ttu-id="b0c6c-122">Mer detaljerad information om hur du skapar en ny appregistrering finns [integrera program med Azure Active Directory](.//develop/active-directory-integrating-applications.md).</span><span class="sxs-lookup"><span data-stu-id="b0c6c-122">For more detailed information about creating a new app registration, see [Integrating applications with Azure Active Directory](.//develop/active-directory-integrating-applications.md).</span></span>


## <a name="step-3-grant-access-tooother-applications"></a><span data-ttu-id="b0c6c-123">Steg 3: Ge åtkomst tooother program</span><span class="sxs-lookup"><span data-stu-id="b0c6c-123">Step 3: Grant access tooother applications</span></span>
<span data-ttu-id="b0c6c-124">Aktivera det ursprungliga programmet hello toobe exponeras tooother program i din katalog:</span><span class="sxs-lookup"><span data-stu-id="b0c6c-124">Enable hello native application toobe exposed tooother applications in your directory:</span></span>

1. <span data-ttu-id="b0c6c-125">Fortfarande i **App registreringar**, Välj hello nytt interna program som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-125">Still in **App registrations**, select hello new native application that you just created.</span></span>
2. <span data-ttu-id="b0c6c-126">Välj **nödvändiga behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-126">Select **Required permissions**.</span></span>
3. <span data-ttu-id="b0c6c-127">Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-127">Select **Add**.</span></span>
4. <span data-ttu-id="b0c6c-128">Öppna hello första steg bör **väljer en API**.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-128">Open hello first step, **Select an API**.</span></span>
5. <span data-ttu-id="b0c6c-129">Använda hello Sök-fältet toofind hello Application Proxy app som du har publicerat i hello första avsnittet.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-129">Use hello search bar toofind hello Application Proxy app that you published in hello first section.</span></span> <span data-ttu-id="b0c6c-130">Välj appen och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-130">Choose that app, then click **Select**.</span></span> 

   ![Sök efter hello proxy app](./media/active-directory-application-proxy-native-client/select_api.png)
6. <span data-ttu-id="b0c6c-132">Öppna hello andra steget **Välj behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-132">Open hello second step, **Select permissions**.</span></span>
7. <span data-ttu-id="b0c6c-133">Använda hello kryssrutan toogrant programspecifika åtkomst tooyour proxy programmet och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-133">Use hello checkbox toogrant your native application access tooyour proxy application, then click **Select**.</span></span>

   ![Bevilja åtkomst tooproxy app](./media/active-directory-application-proxy-native-client/select_perms.png)
8. <span data-ttu-id="b0c6c-135">Välj **klar**.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-135">Select **Done**.</span></span>


## <a name="step-4-edit-hello-active-directory-authentication-library"></a><span data-ttu-id="b0c6c-136">Steg 4: Redigera hello Active Directory Authentication Library</span><span class="sxs-lookup"><span data-stu-id="b0c6c-136">Step 4: Edit hello Active Directory Authentication Library</span></span>
<span data-ttu-id="b0c6c-137">Redigera hello interna programkod hello autentisering gäller hello Active Directory Authentication Library (ADAL) tooinclude hello följande text:</span><span class="sxs-lookup"><span data-stu-id="b0c6c-137">Edit hello native application code in hello authentication context of hello Active Directory Authentication Library (ADAL) tooinclude hello following text:</span></span>

```
// Acquire Access Token from AAD for Proxy Application
AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<Tenant ID>");
AuthenticationResult result = authContext.AcquireToken("< External Url of Proxy App >",
        "<App ID of hello Native app>",
        new Uri("<Redirect Uri of hello Native App>"),
        PromptBehavior.Never);

//Use hello Access Token tooaccess hello Proxy Application
HttpClient httpClient = new HttpClient();
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");
```

<span data-ttu-id="b0c6c-138">hello variabler i hello exempelkod ska ersättas med följande:</span><span class="sxs-lookup"><span data-stu-id="b0c6c-138">hello variables in hello sample code should be replaced as follows:</span></span>

* <span data-ttu-id="b0c6c-139">**Klient-ID** hittar du i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-139">**Tenant ID** can be found in hello Azure portal.</span></span> <span data-ttu-id="b0c6c-140">Navigera för**Azure Active Directory** > **egenskaper** och kopiera hello Directory-ID.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-140">Navigate too**Azure Active Directory** > **Properties** and copy hello Directory ID.</span></span> 
* <span data-ttu-id="b0c6c-141">**Externa URL: en** är hello frontend-URL som du angav i hello Proxy-programmet.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-141">**External URL** is hello front-end URL you entered in hello Proxy Application.</span></span> <span data-ttu-id="b0c6c-142">toofind detta värde, navigera toohello **programproxy** avsnittet hello proxy-appen.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-142">toofind this value, navigate toohello **Application proxy** section of hello proxy app.</span></span>
* <span data-ttu-id="b0c6c-143">**App-ID** av hello inbyggda appen kan hittas på hello **egenskaper** sidan hello det ursprungliga programmet.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-143">**App ID** of hello native app can be found on hello **Properties** page of hello native application.</span></span>
* <span data-ttu-id="b0c6c-144">**Omdirigerings-URI för hello inbyggda appen** kan hittas på hello **omdirigerings-URI: er** sidan hello det ursprungliga programmet.</span><span class="sxs-lookup"><span data-stu-id="b0c6c-144">**Redirect URI of hello native app** can be found on hello **Redirect URIs** page of hello native application.</span></span>


## <a name="see-also"></a><span data-ttu-id="b0c6c-145">Se även</span><span class="sxs-lookup"><span data-stu-id="b0c6c-145">See also</span></span>

<span data-ttu-id="b0c6c-146">Läs mer om hello programspecifika flödet [programspecifika tooweb API](develop/active-directory-authentication-scenarios.md#native-application-to-web-api)</span><span class="sxs-lookup"><span data-stu-id="b0c6c-146">For more information about hello native application flow, see [Native application tooweb API](develop/active-directory-authentication-scenarios.md#native-application-to-web-api)</span></span>

<span data-ttu-id="b0c6c-147">Hello senaste nyheterna och uppdateringarna finns på hello [bloggen om Application Proxy](http://blogs.technet.com/b/applicationproxyblog/)</span><span class="sxs-lookup"><span data-stu-id="b0c6c-147">For hello latest news and updates, check out hello [Application Proxy blog](http://blogs.technet.com/b/applicationproxyblog/)</span></span>
