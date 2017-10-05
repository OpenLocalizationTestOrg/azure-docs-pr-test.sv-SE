---
title: "Anspråksmedvetna appar – Azure AD App Proxy | Microsoft Docs"
description: "Så här publicerar du en lokal ASP.NET-program som accepterar AD FS-anspråk för säker fjärråtkomst av användare."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: harshja
ms.assetid: 91e6211b-fe6a-42c6-bdb3-1fff0312db15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: kgremban
ms.openlocfilehash: 5784222608b01509fc4ff84b1a8792cbcfea89e6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="working-with-claims-aware-apps-in-application-proxy"></a><span data-ttu-id="edeac-103">Arbeta med anspråksmedvetna appar i Application Proxy</span><span class="sxs-lookup"><span data-stu-id="edeac-103">Working with claims-aware apps in Application Proxy</span></span>
<span data-ttu-id="edeac-104">[Anspråksmedvetna appar](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) utför en omdirigering till den säkerhet säkerhetstokentjänst (STS).</span><span class="sxs-lookup"><span data-stu-id="edeac-104">[Claims-aware apps](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) perform a redirection to the Security Token Service (STS).</span></span> <span data-ttu-id="edeac-105">STS begär autentiseringsuppgifter från användare mot en token och omdirigeras användaren till programmet.</span><span class="sxs-lookup"><span data-stu-id="edeac-105">The STS requests credentials from the user in exchange for a token and then redirects the user to the application.</span></span> <span data-ttu-id="edeac-106">Det finns några sätt att aktivera Application Proxy ska fungera med dessa omdirigeringar.</span><span class="sxs-lookup"><span data-stu-id="edeac-106">There are a few ways to enable Application Proxy to work with these redirects.</span></span> <span data-ttu-id="edeac-107">Använd den här artikeln för att konfigurera distributionen för anspråksmedvetna program.</span><span class="sxs-lookup"><span data-stu-id="edeac-107">Use this article to configure your deployment for claims-aware apps.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="edeac-108">Krav</span><span class="sxs-lookup"><span data-stu-id="edeac-108">Prerequisites</span></span>
<span data-ttu-id="edeac-109">Se till att STS som appen anspråksmedvetna omdirigerar till är tillgängligt utanför ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="edeac-109">Make sure that the STS that the claims-aware app redirects to is available outside of your on-premises network.</span></span> <span data-ttu-id="edeac-110">Du kan göra STS tillgängliga genom att exponera den via en proxyserver eller genom att tillåta externa anslutningar.</span><span class="sxs-lookup"><span data-stu-id="edeac-110">You can make the STS available by exposing it through a proxy or by allowing outside connections.</span></span> 

## <a name="publish-your-application"></a><span data-ttu-id="edeac-111">Publicera programmet</span><span class="sxs-lookup"><span data-stu-id="edeac-111">Publish your application</span></span>

1. <span data-ttu-id="edeac-112">Publicera programmet enligt instruktionerna i [publicera program med programproxy](application-proxy-publish-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="edeac-112">Publish your application according to the instructions described in [Publish applications with Application Proxy](application-proxy-publish-azure-portal.md).</span></span>
2. <span data-ttu-id="edeac-113">Gå till sidan program i portalen och välj **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="edeac-113">Navigate to the application page in the portal and select **Single sign-on**.</span></span>
3. <span data-ttu-id="edeac-114">Om du väljer **Azure Active Directory** som din **förautentisering metoden**väljer **Azure AD enkel inloggning inaktiverat** som din **internt Autentiseringsmetod**.</span><span class="sxs-lookup"><span data-stu-id="edeac-114">If you chose **Azure Active Directory** as your **Preauthentication Method**, select **Azure AD single sign-on disabled** as your **Internal Authentication Method**.</span></span> <span data-ttu-id="edeac-115">Om du väljer **Passthrough** som din **förautentisering metoden**, behöver du inte ändra något.</span><span class="sxs-lookup"><span data-stu-id="edeac-115">If you chose **Passthrough** as your **Preauthentication Method**, you don't need to change anything.</span></span>

## <a name="configure-adfs"></a><span data-ttu-id="edeac-116">Konfigurera AD FS</span><span class="sxs-lookup"><span data-stu-id="edeac-116">Configure ADFS</span></span>

<span data-ttu-id="edeac-117">Du kan konfigurera AD FS för anspråksmedvetna program i ett av två sätt.</span><span class="sxs-lookup"><span data-stu-id="edeac-117">You can configure ADFS for claims-aware apps in one of two ways.</span></span> <span data-ttu-id="edeac-118">Först är att använda anpassade domäner.</span><span class="sxs-lookup"><span data-stu-id="edeac-118">The first is by using custom domains.</span></span> <span data-ttu-id="edeac-119">Andra är med WS-Federation.</span><span class="sxs-lookup"><span data-stu-id="edeac-119">The second is with WS-Federation.</span></span> 

### <a name="option-1-custom-domains"></a><span data-ttu-id="edeac-120">Alternativ 1: Anpassade domäner</span><span class="sxs-lookup"><span data-stu-id="edeac-120">Option 1: Custom domains</span></span>

<span data-ttu-id="edeac-121">Om alla interna URL: er för dina program är fullständigt kvalificerade domännamn (FQDN), så du kan konfigurera [anpassade domäner](active-directory-application-proxy-custom-domains.md) för dina program.</span><span class="sxs-lookup"><span data-stu-id="edeac-121">If all the internal URLs for your applications are fully qualified domain names (FQDNs), then you can configure [custom domains](active-directory-application-proxy-custom-domains.md) for your applications.</span></span> <span data-ttu-id="edeac-122">Använd anpassade domäner för att skapa externa URL: er som är samma som de interna URL: er.</span><span class="sxs-lookup"><span data-stu-id="edeac-122">Use the custom domains to create external URLs that are the same as the internal URLs.</span></span> <span data-ttu-id="edeac-123">När de externa URL: er matchar din interna URL: er, fungerar STS omdirigeringar om användarna är lokala eller fjärranslutna.</span><span class="sxs-lookup"><span data-stu-id="edeac-123">When your external URLs match your internal URLs, then the STS redirections work whether your users are on-premises or remote.</span></span> 

### <a name="option-2-ws-federation"></a><span data-ttu-id="edeac-124">Alternativ 2: WS-Federation</span><span class="sxs-lookup"><span data-stu-id="edeac-124">Option 2: WS-Federation</span></span>

1. <span data-ttu-id="edeac-125">Öppna AD FS-hantering.</span><span class="sxs-lookup"><span data-stu-id="edeac-125">Open ADFS Management.</span></span>
2. <span data-ttu-id="edeac-126">Gå till **förtroende för förlitande part**, högerklicka på appen som du publicerar med Application Proxy och välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="edeac-126">Go to **Relying Party Trusts**, right-click on the app you are publishing with Application Proxy, and choose **Properties**.</span></span>  

   ![Förtroenden för förlitande part högerklickar du på appens namn - skärmbild](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  

3. <span data-ttu-id="edeac-128">På den **slutpunkter** fliken, under **slutpunktstyp**väljer **WS-Federation**.</span><span class="sxs-lookup"><span data-stu-id="edeac-128">On the **Endpoints** tab, under **Endpoint type**, select **WS-Federation**.</span></span>
4. <span data-ttu-id="edeac-129">Under **betrodda URL**, ange den URL som du angav i programproxy under **externa URL: en** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="edeac-129">Under **Trusted URL**, enter the URL you entered in the Application Proxy under **External URL** and click **OK**.</span></span>  

   ![Lägga till en slutpunkt - värdet betrodda URL – skärmbild](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="next-steps"></a><span data-ttu-id="edeac-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="edeac-131">Next steps</span></span>
* <span data-ttu-id="edeac-132">[Aktivera enkel inloggning på](application-proxy-sso-overview.md) för program som inte är anspråksmedvetna</span><span class="sxs-lookup"><span data-stu-id="edeac-132">[Enable single-sign on](application-proxy-sso-overview.md) for applications that aren't claims-aware</span></span>
* [<span data-ttu-id="edeac-133">Aktivera ursprunglig klientappar att interagera med proxy-program</span><span class="sxs-lookup"><span data-stu-id="edeac-133">Enable native client apps to interact with proxy applications</span></span>](active-directory-application-proxy-native-client.md)


