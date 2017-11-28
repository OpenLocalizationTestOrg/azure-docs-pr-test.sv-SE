---
title: aaaClassic portal Azure AD App Proxy kopplingar | Microsoft Docs
description: Omfattar hur toocreate och hantera grupper av kopplingar i Azure AD Application Proxy.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: b283796a-9679-4c79-b703-802bb850f65d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 43559b0f4ffc3c7dbbf00901e89ac276d01796e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a><span data-ttu-id="02763-103">Publicera program på separata nätverk och platser med hjälp av connector-grupper</span><span class="sxs-lookup"><span data-stu-id="02763-103">Publish applications on separate networks and locations using connector groups</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="02763-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="02763-104">Azure portal</span></span>](active-directory-application-proxy-connectors-azure-portal.md)
> * [<span data-ttu-id="02763-105">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="02763-105">Azure classic portal</span></span>](active-directory-application-proxy-connectors.md)
>
>

<span data-ttu-id="02763-106">Kopplingen grupper är användbara för olika scenarier, inklusive:</span><span class="sxs-lookup"><span data-stu-id="02763-106">Connector groups are useful for various scenarios, including:</span></span>

* <span data-ttu-id="02763-107">Webbplatser med flera sammankopplade datacenter.</span><span class="sxs-lookup"><span data-stu-id="02763-107">Sites with multiple interconnected datacenters.</span></span> <span data-ttu-id="02763-108">I detta fall vill du tookeep så mycket trafik i hello datacenter som möjligt eftersom länkar mellan datacenter är dyrt och långsamt.</span><span class="sxs-lookup"><span data-stu-id="02763-108">In this case, you want tookeep as much traffic within hello datacenter as possible because cross-datacenter links are expensive and slow.</span></span> <span data-ttu-id="02763-109">Du kan distribuera kopplingar i varje datacenter tooserve endast hello program som finns inom hello datacenter.</span><span class="sxs-lookup"><span data-stu-id="02763-109">You can deploy connectors in each datacenter tooserve only hello applications that reside within hello datacenter.</span></span> <span data-ttu-id="02763-110">Den här metoden minimerar länkar mellan datacenter och ger en helt transparent upplevelse tooyour användare.</span><span class="sxs-lookup"><span data-stu-id="02763-110">This approach minimizes cross-datacenter links and provides an entirely transparent experience tooyour users.</span></span>
* <span data-ttu-id="02763-111">Hantera program på isolerade nätverk som inte är del av hello huvudsakliga företagsnätverket.</span><span class="sxs-lookup"><span data-stu-id="02763-111">Managing applications installed on isolated networks that are not part of hello main corporate network.</span></span> <span data-ttu-id="02763-112">Du kan använda anslutningstjänsten grupper tooinstall dedikerad kopplingar på isolerade nätverk tooalso isolera program toohello nätverk.</span><span class="sxs-lookup"><span data-stu-id="02763-112">You can use connector groups tooinstall dedicated connectors on isolated networks tooalso isolate applications toohello network.</span></span>
* <span data-ttu-id="02763-113">För program som installerats på IaaS för molnåtkomst anger connector grupper du en gemensam toosecure hello åtkomst tooall hello-appar.</span><span class="sxs-lookup"><span data-stu-id="02763-113">For applications installed on IaaS for cloud access, connector groups provide a common service toosecure hello access tooall hello apps.</span></span> <span data-ttu-id="02763-114">Kopplingen grupper inte skapa ytterligare beroende på företagets nätverk eller Fragmentera hello app upplevelse.</span><span class="sxs-lookup"><span data-stu-id="02763-114">Connector groups don't create additional dependency on your corporate network, or fragment hello app experience.</span></span> <span data-ttu-id="02763-115">Kopplingar kan installeras på varje molndatacenter och hantera program som finns i det här nätverket.</span><span class="sxs-lookup"><span data-stu-id="02763-115">Connectors can be installed on every cloud datacenter and serve only applications that reside in this network.</span></span> <span data-ttu-id="02763-116">Du kan installera flera kopplingar tooachieve hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="02763-116">You can install several connectors tooachieve high availability.</span></span>
* <span data-ttu-id="02763-117">Stöd för miljöer med flera skogar där specifika kopplingar kan distribueras per skog och ange tooserve specifika program.</span><span class="sxs-lookup"><span data-stu-id="02763-117">Support for multi-forest environments in which specific connectors can be deployed per forest and set tooserve specific applications.</span></span>
* <span data-ttu-id="02763-118">Kopplingen grupper kan användas i Disaster Recovery platser tooeither identifiera växling vid fel eller hello huvudsakliga som säkerhetskopia för platsen.</span><span class="sxs-lookup"><span data-stu-id="02763-118">Connector groups can be used in Disaster Recovery sites tooeither detect failover or as backup for hello main site.</span></span>
* <span data-ttu-id="02763-119">Kopplingen grupper kan också vara används tooserve flera företag från en enskild klient.</span><span class="sxs-lookup"><span data-stu-id="02763-119">Connector groups can also be used tooserve multiple companies from a single tenant.</span></span>

## <a name="prerequisite-create-your-connectors"></a><span data-ttu-id="02763-120">Förutsättning: Skapa kopplingar</span><span class="sxs-lookup"><span data-stu-id="02763-120">Prerequisite: Create your connectors</span></span>
<span data-ttu-id="02763-121">toogroup din kopplingar [installera flera kopplingar](active-directory-application-proxy-enable.md), namnge och gruppera dem.</span><span class="sxs-lookup"><span data-stu-id="02763-121">toogroup your connectors, [install multiple connectors](active-directory-application-proxy-enable.md), then name and group them.</span></span> <span data-ttu-id="02763-122">Slutligen du har tooassign dem toospecific appar.</span><span class="sxs-lookup"><span data-stu-id="02763-122">Finally you have tooassign them toospecific apps.</span></span>

## <a name="step-1-create-connector-groups"></a><span data-ttu-id="02763-123">Steg 1: Skapa grupper för anslutningstjänsten</span><span class="sxs-lookup"><span data-stu-id="02763-123">Step 1: Create connector groups</span></span>
<span data-ttu-id="02763-124">Du kan skapa så många connector-grupper som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="02763-124">You can create as many connector groups as you want.</span></span> <span data-ttu-id="02763-125">Skapa en koppling sker i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="02763-125">Connector group creation is accomplished in hello Azure classic portal.</span></span>

1. <span data-ttu-id="02763-126">Välj din katalog och klickar på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="02763-126">Select your directory and click **Configure**.</span></span>  
    <span data-ttu-id="02763-127">![Programproxy, konfigurera skärmbild - Klicka på Hantera connector grupper](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)</span><span class="sxs-lookup"><span data-stu-id="02763-127">![Application proxy, configure screenshot - click manage connector groups](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)</span></span>
2. <span data-ttu-id="02763-128">Klicka på under Application Proxy **hantera Connector grupper** och skapa en koppling grupp genom att ge hello gruppen ett namn.</span><span class="sxs-lookup"><span data-stu-id="02763-128">Under Application Proxy, click **Manage Connector Groups** and create a connector group by giving hello group a name.</span></span>  
    <span data-ttu-id="02763-129">![Application proxy connector grupper skärmbild - namn på ny grupp](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)</span><span class="sxs-lookup"><span data-stu-id="02763-129">![Application proxy connector groups screenshot - name new group](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)</span></span>

## <a name="step-2-assign-connectors-tooyour-groups"></a><span data-ttu-id="02763-130">Steg 2: Tilldela kopplingar tooyour grupper</span><span class="sxs-lookup"><span data-stu-id="02763-130">Step 2: Assign connectors tooyour groups</span></span>
<span data-ttu-id="02763-131">När hello connector grupper skapas bör du flytta hello kopplingar toohello lämplig grupp.</span><span class="sxs-lookup"><span data-stu-id="02763-131">Once hello connector groups are created, move hello connectors toohello appropriate group.</span></span>

1. <span data-ttu-id="02763-132">Under **Application Proxy**, klickar du på **hantera kopplingar**.</span><span class="sxs-lookup"><span data-stu-id="02763-132">Under **Application Proxy**, click **Manage Connectors**.</span></span>
2. <span data-ttu-id="02763-133">Under **grupp**väljer hello-grupp som du vill använda för varje koppling.</span><span class="sxs-lookup"><span data-stu-id="02763-133">Under **Group**, select hello group you want for each connector.</span></span> <span data-ttu-id="02763-134">Det kan ta hello kopplingar in too10 minuter toobecome aktiv i hello ny grupp.</span><span class="sxs-lookup"><span data-stu-id="02763-134">It might take hello connectors up too10 minutes toobecome active in hello new group.</span></span>  
    <span data-ttu-id="02763-135">![Application proxy kopplingar skärmbild – Välj grupp från listrutan](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)</span><span class="sxs-lookup"><span data-stu-id="02763-135">![Application proxy connectors screenshot - select group from dropdown menu](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)</span></span>

## <a name="step-3-assign-applications-tooyour-connector-groups"></a><span data-ttu-id="02763-136">Steg 3: Tilldela program tooyour connector grupper</span><span class="sxs-lookup"><span data-stu-id="02763-136">Step 3: Assign applications tooyour connector groups</span></span>
<span data-ttu-id="02763-137">hello sista steget är tooset varje toohello connector programgrupp som hanterar den.</span><span class="sxs-lookup"><span data-stu-id="02763-137">hello last step is tooset each application toohello connector group that serves it.</span></span>

1. <span data-ttu-id="02763-138">I hello klassiska Azure-portalen i din katalog väljer hello program du vill tooassign toohello gruppen och klicka på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="02763-138">In hello Azure classic portal, in your directory, select hello Application you want tooassign toohello group and click **Configure**.</span></span>
2. <span data-ttu-id="02763-139">Under **Connector grupp**väljer hello-grupp som du vill hello programmet toouse.</span><span class="sxs-lookup"><span data-stu-id="02763-139">Under **Connector group**, select hello group you want hello application toouse.</span></span> <span data-ttu-id="02763-140">Den här ändringen tillämpas omedelbart.</span><span class="sxs-lookup"><span data-stu-id="02763-140">This change is immediately applied.</span></span>  
    <span data-ttu-id="02763-141">![Application proxy connector grupp skärmbild - Välj grupp från listrutan](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)</span><span class="sxs-lookup"><span data-stu-id="02763-141">![Application proxy connector group screenshot - select group from dropdown menu](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)</span></span>

## <a name="see-also"></a><span data-ttu-id="02763-142">Se även</span><span class="sxs-lookup"><span data-stu-id="02763-142">See also</span></span>
* [<span data-ttu-id="02763-143">Aktivera Application Proxy</span><span class="sxs-lookup"><span data-stu-id="02763-143">Enable Application Proxy</span></span>](active-directory-application-proxy-enable.md)
* [<span data-ttu-id="02763-144">Aktivera enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="02763-144">Enable single-sign on</span></span>](active-directory-application-proxy-sso-using-kcd.md)
* [<span data-ttu-id="02763-145">Aktivera villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="02763-145">Enable conditional access</span></span>](active-directory-application-proxy-conditional-access.md)
* [<span data-ttu-id="02763-146">Felsökning av problem med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="02763-146">Troubleshoot issues you're having with Application Proxy</span></span>](active-directory-application-proxy-troubleshoot.md)

<span data-ttu-id="02763-147">Hello senaste nyheterna och uppdateringarna finns på hello [bloggen om Application Proxy](http://blogs.technet.com/b/applicationproxyblog/)</span><span class="sxs-lookup"><span data-stu-id="02763-147">For hello latest news and updates, check out hello [Application Proxy blog](http://blogs.technet.com/b/applicationproxyblog/)</span></span>
