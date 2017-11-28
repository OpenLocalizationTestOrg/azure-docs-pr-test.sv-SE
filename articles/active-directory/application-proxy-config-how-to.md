---
title: aaaHow tooconfigure ett program med Application Proxy | Microsoft Docs
description: "Lär dig hur toocreate en konfigurera ett program för APplication Proxy i några enkla steg"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: c64019098fc124e4fe10b8288830bcd2b7239d3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-an-application-proxy-application"></a><span data-ttu-id="c5ff4-103">Hur tooconfigure ett program med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="c5ff4-103">How tooconfigure an Application Proxy application</span></span>

<span data-ttu-id="c5ff4-104">Den här artikeln hjälper dig toounderstand hur tooconfigure ett Application Proxy-program i Azure AD tooexpose din lokala program toohello cloud.</span><span class="sxs-lookup"><span data-stu-id="c5ff4-104">This article help you toounderstand how tooconfigure an Application Proxy application within Azure AD tooexpose your on-premises applications toohello cloud.</span></span>

## <a name="recommended-documents"></a><span data-ttu-id="c5ff4-105">Rekommenderade dokument</span><span class="sxs-lookup"><span data-stu-id="c5ff4-105">Recommended documents</span></span> 

<span data-ttu-id="c5ff4-106">toolearn om hello inledande konfiguration och skapande av ett program för Application Proxy via hello Admin Portal följer hello [publicera program med Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="c5ff4-106">toolearn about hello initial configurations and creation of an Application Proxy application through hello Admin Portal, follow hello [Publish applications using Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span></span>

<span data-ttu-id="c5ff4-107">Mer information om hur du konfigurerar kopplingar finns [aktivera Application Proxy hello Azure-portalen](active-directory-application-proxy-enable.md).</span><span class="sxs-lookup"><span data-stu-id="c5ff4-107">For details on configuring Connectors, see [Enable Application Proxy in hello Azure portal](active-directory-application-proxy-enable.md).</span></span>

<span data-ttu-id="c5ff4-108">Information om överföring av certifikat och använder anpassade domäner finns i [arbeta med anpassade domäner i Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span><span class="sxs-lookup"><span data-stu-id="c5ff4-108">For information on uploading certificates and using custom domains, see [Working with custom domains in Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span></span>

## <a name="create-hello-applicationsetting-hello-urls"></a><span data-ttu-id="c5ff4-109">Skapa hello programinställning/hello URL: er</span><span class="sxs-lookup"><span data-stu-id="c5ff4-109">Create hello Application/Setting hello URLs</span></span>

<span data-ttu-id="c5ff4-110">Om du följer hello stegen i hello [publicera program med Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal) dokumentation och kan hämta ett fel när programmet hello, se hello felinformationen och förslag på hur toofix hello program.</span><span class="sxs-lookup"><span data-stu-id="c5ff4-110">If you are following hello steps in hello [Publish applications using Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal) documentation and are getting an error creating hello application, see hello error details for information and suggestions for how toofix hello application.</span></span> <span data-ttu-id="c5ff4-111">De vanligaste felmeddelanden som innehåller en föreslagen åtgärd.</span><span class="sxs-lookup"><span data-stu-id="c5ff4-111">Most error messages include a suggested fix.</span></span> <span data-ttu-id="c5ff4-112">tooavoid vanliga fel, kontrollera:</span><span class="sxs-lookup"><span data-stu-id="c5ff4-112">tooavoid common errors, verify:</span></span>

-   <span data-ttu-id="c5ff4-113">Du är administratör med behörigheten toocreate ett program med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="c5ff4-113">You are an administrator with permission toocreate an Application Proxy application</span></span>

-   <span data-ttu-id="c5ff4-114">hello Intern URL är unikt</span><span class="sxs-lookup"><span data-stu-id="c5ff4-114">hello internal URL is unique</span></span>

-   <span data-ttu-id="c5ff4-115">hello externa URL: en är unikt</span><span class="sxs-lookup"><span data-stu-id="c5ff4-115">hello external URL is unique</span></span>

-   <span data-ttu-id="c5ff4-116">Hej URL: er starta med http eller https och sluta med en ”/”</span><span class="sxs-lookup"><span data-stu-id="c5ff4-116">hello URLs start with http or https, and end with a “/”</span></span>

-   <span data-ttu-id="c5ff4-117">hello Webbadress måste vara ett domännamn, inte en IP-adress</span><span class="sxs-lookup"><span data-stu-id="c5ff4-117">hello URL should be a domain name, not an IP address</span></span>

<span data-ttu-id="c5ff4-118">hello felmeddelande ska visas i hello övre högra hörnet när du skapar hello program.</span><span class="sxs-lookup"><span data-stu-id="c5ff4-118">hello error message should display in hello top right corner when you create hello application.</span></span> <span data-ttu-id="c5ff4-119">Du kan också välja hello ikonen toosee hello felmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="c5ff4-119">You can also select hello notification icon toosee hello error messages.</span></span>

   ![Meddelande-fråga](./media/application-proxy-config-how-to/error-message.png)

## <a name="configure-connectorsconnector-groups"></a><span data-ttu-id="c5ff4-121">Konfigurera grupper kopplingar-koppling</span><span class="sxs-lookup"><span data-stu-id="c5ff4-121">Configure connectors/connector groups</span></span>

<span data-ttu-id="c5ff4-122">Om du har problem med hur du konfigurerar programmet på grund av varning om hello kopplingar och grupper för anslutningstjänsten finns instruktioner för information om hur du aktiverar Application Proxy toodownload kopplingar.</span><span class="sxs-lookup"><span data-stu-id="c5ff4-122">If you are having difficulty configuring your application because of warning about hello connectors and connector groups, see instructions on enabling Application Proxy for details on how toodownload connectors.</span></span> <span data-ttu-id="c5ff4-123">Om du vill toolearn mer om kopplingar finns hello [kopplingar dokumentationen](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).</span><span class="sxs-lookup"><span data-stu-id="c5ff4-123">If you want toolearn more about connectors, see hello [connectors documentation](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).</span></span>

<span data-ttu-id="c5ff4-124">Om din kopplingar är inaktiva, betyder det att de är tooreach hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c5ff4-124">If your connectors are inactive, this means that they are unable tooreach hello service.</span></span> <span data-ttu-id="c5ff4-125">Detta är ofta eftersom inte alla hello krävs portar är öppna.</span><span class="sxs-lookup"><span data-stu-id="c5ff4-125">This is often because all hello required ports are not open.</span></span> <span data-ttu-id="c5ff4-126">toosee en lista över portar som krävs i avsnittet hello förutsättningar för hello aktivera Application Proxy-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="c5ff4-126">toosee a list of required ports, see hello pre-requisites section of hello enabling Application Proxy documentation.</span></span>

## <a name="upload-certificates-for-custom-domains"></a><span data-ttu-id="c5ff4-127">Överför certifikat för anpassade domäner</span><span class="sxs-lookup"><span data-stu-id="c5ff4-127">Upload certificates for custom domains</span></span>

<span data-ttu-id="c5ff4-128">Anpassade domäner kan du toospecify hello domänen för de externa URL: er.</span><span class="sxs-lookup"><span data-stu-id="c5ff4-128">Custom Domains allow you toospecify hello domain of your external URLs.</span></span> <span data-ttu-id="c5ff4-129">toouse anpassade domäner, behöver du tooupload hello certifikat för domänen.</span><span class="sxs-lookup"><span data-stu-id="c5ff4-129">toouse custom domains, you need tooupload hello certificate for that domain.</span></span> <span data-ttu-id="c5ff4-130">Information om hur du använder anpassade domäner och certifikat finns i [arbeta med anpassade domäner i Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span><span class="sxs-lookup"><span data-stu-id="c5ff4-130">For information on using custom domains and certificates, see [Working with custom domains in Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span></span> 

<span data-ttu-id="c5ff4-131">Om du upptäcker problem överföra ditt certifikat, leta efter hello felmeddelanden i hello portal för ytterligare information om hello problem med hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="c5ff4-131">If you are encountering issues uploading your certificate, look for hello error messages in hello portal for additional information on hello problem with hello certificate.</span></span> <span data-ttu-id="c5ff4-132">Vanliga problem med säkerhetscertifikat inkluderar:</span><span class="sxs-lookup"><span data-stu-id="c5ff4-132">Common certificate problems include:</span></span>

-   <span data-ttu-id="c5ff4-133">Certifikatet har upphört att gälla</span><span class="sxs-lookup"><span data-stu-id="c5ff4-133">Expired certificate</span></span>

-   <span data-ttu-id="c5ff4-134">Certifikatet är självsignerat</span><span class="sxs-lookup"><span data-stu-id="c5ff4-134">Certificate is self-signed</span></span>

-   <span data-ttu-id="c5ff4-135">Certifikatet saknar privat nyckel för hello</span><span class="sxs-lookup"><span data-stu-id="c5ff4-135">Certificate is missing hello private key</span></span>

<span data-ttu-id="c5ff4-136">hello felmeddelande visas i hello övre högra hörnet försök tooupload hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="c5ff4-136">hello error message display in hello top right corner as you try tooupload hello certificate.</span></span> <span data-ttu-id="c5ff4-137">Du kan också välja hello ikonen toosee hello felmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="c5ff4-137">You can also select hello notification icon toosee hello error messages.</span></span>

   ![Meddelande-fråga](./media/application-proxy-config-how-to/error-message2.png)

## <a name="next-steps"></a><span data-ttu-id="c5ff4-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c5ff4-139">Next steps</span></span>
[<span data-ttu-id="c5ff4-140">Publicera program med Azure AD Application Proxy</span><span class="sxs-lookup"><span data-stu-id="c5ff4-140">Publish applications using Azure AD Application Proxy</span></span>](application-proxy-publish-azure-portal.md)
