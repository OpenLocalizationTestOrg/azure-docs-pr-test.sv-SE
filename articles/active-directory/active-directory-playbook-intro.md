---
title: Azure Active Directory PoC Playbook introduktion | Microsoft Docs
description: "Utforska och snabbt implementera scenarier för identitets- och åtkomsthantering"
services: active-directory
keywords: Azure active directory, playbook, konceptbevis, PoC
documentationcenter: 
author: dstefanMSFT
manager: asuthar
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/12/2017
ms.author: dstefan
ms.openlocfilehash: 567f3373594bc53435e8c0bd0a3445dd318af1cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-introduction"></a><span data-ttu-id="1c9ab-104">Azure Active Directory bevis på koncept Playbook: introduktion</span><span class="sxs-lookup"><span data-stu-id="1c9ab-104">Azure Active Directory Proof of Concept Playbook: Introduction</span></span>

<span data-ttu-id="1c9ab-105">Den här artikeln innehåller riktlinjer för att utforska olika Azure AD-funktioner i ett konceptbevis (PoC).</span><span class="sxs-lookup"><span data-stu-id="1c9ab-105">This article provides guidelines to explore different Azure AD capabilities in a Proof of Concept (PoC).</span></span> <span data-ttu-id="1c9ab-106">Målgruppen för det här dokumentet är identitet arkitekter, IT-proffs och systemintegrerare.</span><span class="sxs-lookup"><span data-stu-id="1c9ab-106">The intended audience of this document is Identity Architects, IT Professionals, and System Integrators</span></span>

## <a name="how-to-use-this-playbook"></a><span data-ttu-id="1c9ab-107">Hur du använder den här Playbook</span><span class="sxs-lookup"><span data-stu-id="1c9ab-107">How to use this Playbook</span></span>

1. <span data-ttu-id="1c9ab-108">Använd den [tema](active-directory-playbook-ingredients.md#theme) avsnittet och välj områdena av intresse baserat på dina behov.</span><span class="sxs-lookup"><span data-stu-id="1c9ab-108">Use the [Theme](active-directory-playbook-ingredients.md#theme) section and pick the area(s) of interest based on your needs.</span></span>  
2. <span data-ttu-id="1c9ab-109">Omfattningen PoC genom att välja de scenarier som överensstämmer med dina affärsmål.</span><span class="sxs-lookup"><span data-stu-id="1c9ab-109">Scope the PoC by choosing the scenarios that align with your business goals.</span></span> <span data-ttu-id="1c9ab-110">Ju kortare bättre.</span><span class="sxs-lookup"><span data-stu-id="1c9ab-110">The shorter the better.</span></span> <span data-ttu-id="1c9ab-111">Vi rekommenderar att du gör det som kort och koncis som möjligt för att förmedla värdet till de berörda parter och minimerar komplexitet för att använda den.</span><span class="sxs-lookup"><span data-stu-id="1c9ab-111">We recommend doing it as short and concise as possible to convey the value to the stakeholders while minimizing the complexity to realize it.</span></span>  
3. <span data-ttu-id="1c9ab-112">Använd den [implementering](active-directory-playbook-implementation.md) avsnitt för att förstå scenarierna och vad de betyder för din miljö.</span><span class="sxs-lookup"><span data-stu-id="1c9ab-112">Use the [Implementation](active-directory-playbook-implementation.md) section to understand the scenarios, and what would they mean for your environment.</span></span> <span data-ttu-id="1c9ab-113">I varje scenario beskrivs hur du ställer in (vi kallar [byggblock](active-directory-playbook-building-blocks.md)), och hur du navigerar scenarier.</span><span class="sxs-lookup"><span data-stu-id="1c9ab-113">In each scenario, we describe how to set it up (what we call [Building Blocks](active-directory-playbook-building-blocks.md)), and how to navigate the scenarios.</span></span> 
4. <span data-ttu-id="1c9ab-114">Varje byggblock beskrivs kraven behövs, samt en ungefärlig tid att slutföra.</span><span class="sxs-lookup"><span data-stu-id="1c9ab-114">Each building block explains the pre-requisites needed, as well as an approximate time to complete.</span></span> <span data-ttu-id="1c9ab-115">Detta kan hjälpa dig under planeringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="1c9ab-115">This can help you during the planning process.</span></span> 
5. <span data-ttu-id="1c9ab-116">Baserat på 1 till 3 ovan, definierar den [miljö](active-directory-playbook-ingredients.md#environment) där du vill köra.</span><span class="sxs-lookup"><span data-stu-id="1c9ab-116">Based on 1-3 Above, define the [Environment](active-directory-playbook-ingredients.md#environment) in which to execute.</span></span> <span data-ttu-id="1c9ab-117">Vi rekommenderar för att sträva efter en produktionsmiljö att få en bra känsla av för dina användare.</span><span class="sxs-lookup"><span data-stu-id="1c9ab-117">We encourage to strive for a production environment to get a good feel of the experience for your users.</span></span> 
6. <span data-ttu-id="1c9ab-118">När med motstridiga krav, använder du matrisen användbara förhållandet</span><span class="sxs-lookup"><span data-stu-id="1c9ab-118">When having conflicting requirements, use this helpful tradeoff matrix</span></span> 
   * <span data-ttu-id="1c9ab-119">Tema till Central visar värde</span><span class="sxs-lookup"><span data-stu-id="1c9ab-119">Theme-centric showing of value</span></span>  
   * <span data-ttu-id="1c9ab-120">Jämn förbereda, konfigurera och köra scenarier</span><span class="sxs-lookup"><span data-stu-id="1c9ab-120">Smoothness to prepare, to set up, and to execute the scenarios</span></span> 
   * <span data-ttu-id="1c9ab-121">Minimal tid att köra målscenarier</span><span class="sxs-lookup"><span data-stu-id="1c9ab-121">Minimal time to execute the target scenarios</span></span> 
   * <span data-ttu-id="1c9ab-122">Så nära produktion som möjligt i din begränsningar</span><span class="sxs-lookup"><span data-stu-id="1c9ab-122">As close to production as feasible within your constraints</span></span> 

>[!NOTE]
> <span data-ttu-id="1c9ab-123">I den här artikeln visas vissa specifika tredjepartsprogram och produkter som exempel i informationssyfte.</span><span class="sxs-lookup"><span data-stu-id="1c9ab-123">Throughout this article, you will see some specific third party applications and products mentioned as examples for convenience.</span></span> <span data-ttu-id="1c9ab-124">Azure AD stöder tusentals program i vår [programgalleriet](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) som du kan använda baserat på dina behov och miljö.</span><span class="sxs-lookup"><span data-stu-id="1c9ab-124">Azure AD supports thousands of applications in our [application gallery](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) that you can use based on your needs and environment.</span></span> 



[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]