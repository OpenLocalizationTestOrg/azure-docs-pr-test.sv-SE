---
title: "aaaClaims-kompatibla appar – Azure AD App Proxy | Microsoft Docs"
description: "Hur toopublish lokalt ASP.NET-program som accepterar AD FS-anspråk för säker fjärråtkomst av användare."
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
ms.openlocfilehash: 7be633225de700226c7c94815eb91b3de2b61cb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-claims-aware-apps-in-application-proxy"></a><span data-ttu-id="c9a2f-103">Arbeta med anspråksmedvetna appar i Application Proxy</span><span class="sxs-lookup"><span data-stu-id="c9a2f-103">Working with claims-aware apps in Application Proxy</span></span>
<span data-ttu-id="c9a2f-104">[Anspråksmedvetna appar](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) utför en omdirigering toohello säkerhet säkerhetstokentjänst (STS).</span><span class="sxs-lookup"><span data-stu-id="c9a2f-104">[Claims-aware apps](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) perform a redirection toohello Security Token Service (STS).</span></span> <span data-ttu-id="c9a2f-105">hello STS begär autentiseringsuppgifter från hello användare mot en token och omdirigerar hello toohello användarprogram.</span><span class="sxs-lookup"><span data-stu-id="c9a2f-105">hello STS requests credentials from hello user in exchange for a token and then redirects hello user toohello application.</span></span> <span data-ttu-id="c9a2f-106">Det finns ett par sätt tooenable Application Proxy toowork med dessa omdirigeringar.</span><span class="sxs-lookup"><span data-stu-id="c9a2f-106">There are a few ways tooenable Application Proxy toowork with these redirects.</span></span> <span data-ttu-id="c9a2f-107">Använd den här artikeln tooconfigure distributionen för anspråksmedvetna program.</span><span class="sxs-lookup"><span data-stu-id="c9a2f-107">Use this article tooconfigure your deployment for claims-aware apps.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="c9a2f-108">Krav</span><span class="sxs-lookup"><span data-stu-id="c9a2f-108">Prerequisites</span></span>
<span data-ttu-id="c9a2f-109">Kontrollera att hello STS som hello anspråksmedvetna app omdirigerar toois tillgängligt utanför ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="c9a2f-109">Make sure that hello STS that hello claims-aware app redirects toois available outside of your on-premises network.</span></span> <span data-ttu-id="c9a2f-110">Du kan göra hello STS tillgängliga genom att exponera den via en proxyserver eller genom att låta utanför anslutningar.</span><span class="sxs-lookup"><span data-stu-id="c9a2f-110">You can make hello STS available by exposing it through a proxy or by allowing outside connections.</span></span> 

## <a name="publish-your-application"></a><span data-ttu-id="c9a2f-111">Publicera programmet</span><span class="sxs-lookup"><span data-stu-id="c9a2f-111">Publish your application</span></span>

1. <span data-ttu-id="c9a2f-112">Publicera programmet enligt instruktionerna i toohello [publicera program med programproxy](application-proxy-publish-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c9a2f-112">Publish your application according toohello instructions described in [Publish applications with Application Proxy](application-proxy-publish-azure-portal.md).</span></span>
2. <span data-ttu-id="c9a2f-113">Navigera toohello program sida i hello portalen och väljer **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c9a2f-113">Navigate toohello application page in hello portal and select **Single sign-on**.</span></span>
3. <span data-ttu-id="c9a2f-114">Om du väljer **Azure Active Directory** som din **förautentisering metoden**väljer **Azure AD enkel inloggning inaktiverat** som din **internt Autentiseringsmetod**.</span><span class="sxs-lookup"><span data-stu-id="c9a2f-114">If you chose **Azure Active Directory** as your **Preauthentication Method**, select **Azure AD single sign-on disabled** as your **Internal Authentication Method**.</span></span> <span data-ttu-id="c9a2f-115">Om du väljer **Passthrough** som din **förautentisering metoden**, behöver du inte toochange något annat.</span><span class="sxs-lookup"><span data-stu-id="c9a2f-115">If you chose **Passthrough** as your **Preauthentication Method**, you don't need toochange anything.</span></span>

## <a name="configure-adfs"></a><span data-ttu-id="c9a2f-116">Konfigurera AD FS</span><span class="sxs-lookup"><span data-stu-id="c9a2f-116">Configure ADFS</span></span>

<span data-ttu-id="c9a2f-117">Du kan konfigurera AD FS för anspråksmedvetna program i ett av två sätt.</span><span class="sxs-lookup"><span data-stu-id="c9a2f-117">You can configure ADFS for claims-aware apps in one of two ways.</span></span> <span data-ttu-id="c9a2f-118">hello är först med hjälp av anpassade domäner.</span><span class="sxs-lookup"><span data-stu-id="c9a2f-118">hello first is by using custom domains.</span></span> <span data-ttu-id="c9a2f-119">hello är andra med WS-Federation.</span><span class="sxs-lookup"><span data-stu-id="c9a2f-119">hello second is with WS-Federation.</span></span> 

### <a name="option-1-custom-domains"></a><span data-ttu-id="c9a2f-120">Alternativ 1: Anpassade domäner</span><span class="sxs-lookup"><span data-stu-id="c9a2f-120">Option 1: Custom domains</span></span>

<span data-ttu-id="c9a2f-121">Om alla hello interna URL: er för dina program är fullständigt kvalificerade domännamn (FQDN), så du kan konfigurera [anpassade domäner](active-directory-application-proxy-custom-domains.md) för dina program.</span><span class="sxs-lookup"><span data-stu-id="c9a2f-121">If all hello internal URLs for your applications are fully qualified domain names (FQDNs), then you can configure [custom domains](active-directory-application-proxy-custom-domains.md) for your applications.</span></span> <span data-ttu-id="c9a2f-122">Använd hello anpassade domäner toocreate externa URL: er som är hello samma som hello interna URL: er.</span><span class="sxs-lookup"><span data-stu-id="c9a2f-122">Use hello custom domains toocreate external URLs that are hello same as hello internal URLs.</span></span> <span data-ttu-id="c9a2f-123">När de externa URL: er matchar din interna URL: er, fungerar hello STS omdirigeringar om användarna är lokala eller fjärranslutna.</span><span class="sxs-lookup"><span data-stu-id="c9a2f-123">When your external URLs match your internal URLs, then hello STS redirections work whether your users are on-premises or remote.</span></span> 

### <a name="option-2-ws-federation"></a><span data-ttu-id="c9a2f-124">Alternativ 2: WS-Federation</span><span class="sxs-lookup"><span data-stu-id="c9a2f-124">Option 2: WS-Federation</span></span>

1. <span data-ttu-id="c9a2f-125">Öppna AD FS-hantering.</span><span class="sxs-lookup"><span data-stu-id="c9a2f-125">Open ADFS Management.</span></span>
2. <span data-ttu-id="c9a2f-126">Gå för**förtroende för förlitande part**, högerklicka på hello-app som du publicerar med Application Proxy och välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="c9a2f-126">Go too**Relying Party Trusts**, right-click on hello app you are publishing with Application Proxy, and choose **Properties**.</span></span>  

   ![Förtroenden för förlitande part högerklickar du på appens namn - skärmbild](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  

3. <span data-ttu-id="c9a2f-128">På hello **slutpunkter** fliken, under **slutpunktstyp**väljer **WS-Federation**.</span><span class="sxs-lookup"><span data-stu-id="c9a2f-128">On hello **Endpoints** tab, under **Endpoint type**, select **WS-Federation**.</span></span>
4. <span data-ttu-id="c9a2f-129">Under **betrodda URL**, ange hello-URL som du angav i hello Application Proxy under **externa URL: en** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c9a2f-129">Under **Trusted URL**, enter hello URL you entered in hello Application Proxy under **External URL** and click **OK**.</span></span>  

   ![Lägga till en slutpunkt - värdet betrodda URL – skärmbild](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="next-steps"></a><span data-ttu-id="c9a2f-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c9a2f-131">Next steps</span></span>
* <span data-ttu-id="c9a2f-132">[Aktivera enkel inloggning på](application-proxy-sso-overview.md) för program som inte är anspråksmedvetna</span><span class="sxs-lookup"><span data-stu-id="c9a2f-132">[Enable single-sign on](application-proxy-sso-overview.md) for applications that aren't claims-aware</span></span>
* [<span data-ttu-id="c9a2f-133">Aktivera ursprunglig klient appar toointeract med proxy-program</span><span class="sxs-lookup"><span data-stu-id="c9a2f-133">Enable native client apps toointeract with proxy applications</span></span>](active-directory-application-proxy-native-client.md)


