---
title: aaaAzure Active Directory PoC Playbook introduktion | Microsoft Docs
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
ms.openlocfilehash: 655524e9692de46e831fc68e1636e6c20d41958f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-introduction"></a><span data-ttu-id="a4b16-104">Azure Active Directory bevis på koncept Playbook: introduktion</span><span class="sxs-lookup"><span data-stu-id="a4b16-104">Azure Active Directory Proof of Concept Playbook: Introduction</span></span>

<span data-ttu-id="a4b16-105">Den här artikeln innehåller riktlinjer tooexplore olika Azure AD-funktioner i ett konceptbevis (PoC).</span><span class="sxs-lookup"><span data-stu-id="a4b16-105">This article provides guidelines tooexplore different Azure AD capabilities in a Proof of Concept (PoC).</span></span> <span data-ttu-id="a4b16-106">hello avsedd målgruppen för det här dokumentet är identitet arkitekter, IT-proffs och systemintegrerare.</span><span class="sxs-lookup"><span data-stu-id="a4b16-106">hello intended audience of this document is Identity Architects, IT Professionals, and System Integrators</span></span>

## <a name="how-toouse-this-playbook"></a><span data-ttu-id="a4b16-107">Hur toouse denna Playbook</span><span class="sxs-lookup"><span data-stu-id="a4b16-107">How toouse this Playbook</span></span>

1. <span data-ttu-id="a4b16-108">Använd hello [tema](active-directory-playbook-ingredients.md#theme) avsnittet och välj hello områden av intresse baserat på dina behov.</span><span class="sxs-lookup"><span data-stu-id="a4b16-108">Use hello [Theme](active-directory-playbook-ingredients.md#theme) section and pick hello area(s) of interest based on your needs.</span></span>  
2. <span data-ttu-id="a4b16-109">Omfång hello PoC genom att välja hello-scenarier som överensstämmer med dina affärsmål.</span><span class="sxs-lookup"><span data-stu-id="a4b16-109">Scope hello PoC by choosing hello scenarios that align with your business goals.</span></span> <span data-ttu-id="a4b16-110">hello kortare hello bättre.</span><span class="sxs-lookup"><span data-stu-id="a4b16-110">hello shorter hello better.</span></span> <span data-ttu-id="a4b16-111">Vi rekommenderar att du gör som kort och koncis som möjligt tooconvey hello värde toohello intressenter och minimerar hello komplexitet toorealize den.</span><span class="sxs-lookup"><span data-stu-id="a4b16-111">We recommend doing it as short and concise as possible tooconvey hello value toohello stakeholders while minimizing hello complexity toorealize it.</span></span>  
3. <span data-ttu-id="a4b16-112">Använd hello [implementering](active-directory-playbook-implementation.md) avsnittet toounderstand hello scenarier och vad betyder de för din miljö.</span><span class="sxs-lookup"><span data-stu-id="a4b16-112">Use hello [Implementation](active-directory-playbook-implementation.md) section toounderstand hello scenarios, and what would they mean for your environment.</span></span> <span data-ttu-id="a4b16-113">I varje scenario beskrivs hur tooset den (vi kallar [byggblock](active-directory-playbook-building-blocks.md)), och hur toonavigate hello scenarier.</span><span class="sxs-lookup"><span data-stu-id="a4b16-113">In each scenario, we describe how tooset it up (what we call [Building Blocks](active-directory-playbook-building-blocks.md)), and how toonavigate hello scenarios.</span></span> 
4. <span data-ttu-id="a4b16-114">Varje byggblock förklarar hello förutsättningar, samt en ungefärliga tid toocomplete.</span><span class="sxs-lookup"><span data-stu-id="a4b16-114">Each building block explains hello pre-requisites needed, as well as an approximate time toocomplete.</span></span> <span data-ttu-id="a4b16-115">Detta kan hjälpa dig under hello planeringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="a4b16-115">This can help you during hello planning process.</span></span> 
5. <span data-ttu-id="a4b16-116">Baserat på 1 till 3 ovan, definiera hello [miljö](active-directory-playbook-ingredients.md#environment) i vilka tooexecute.</span><span class="sxs-lookup"><span data-stu-id="a4b16-116">Based on 1-3 Above, define hello [Environment](active-directory-playbook-ingredients.md#environment) in which tooexecute.</span></span> <span data-ttu-id="a4b16-117">Vi rekommenderar att toostrive för en produktion miljö tooget en bra känsla hello upplevelse för användarna.</span><span class="sxs-lookup"><span data-stu-id="a4b16-117">We encourage toostrive for a production environment tooget a good feel of hello experience for your users.</span></span> 
6. <span data-ttu-id="a4b16-118">När med motstridiga krav, använder du matrisen användbara förhållandet</span><span class="sxs-lookup"><span data-stu-id="a4b16-118">When having conflicting requirements, use this helpful tradeoff matrix</span></span> 
   * <span data-ttu-id="a4b16-119">Tema till Central visar värde</span><span class="sxs-lookup"><span data-stu-id="a4b16-119">Theme-centric showing of value</span></span>  
   * <span data-ttu-id="a4b16-120">Jämn tooprepare tooset upp och tooexecute hello-scenarier</span><span class="sxs-lookup"><span data-stu-id="a4b16-120">Smoothness tooprepare, tooset up, and tooexecute hello scenarios</span></span> 
   * <span data-ttu-id="a4b16-121">Minimal tid tooexecute hello målscenarier</span><span class="sxs-lookup"><span data-stu-id="a4b16-121">Minimal time tooexecute hello target scenarios</span></span> 
   * <span data-ttu-id="a4b16-122">Som nära tooproduction som möjligt i din begränsningar</span><span class="sxs-lookup"><span data-stu-id="a4b16-122">As close tooproduction as feasible within your constraints</span></span> 

>[!NOTE]
> <span data-ttu-id="a4b16-123">I den här artikeln visas vissa specifika tredjepartsprogram och produkter som exempel i informationssyfte.</span><span class="sxs-lookup"><span data-stu-id="a4b16-123">Throughout this article, you will see some specific third party applications and products mentioned as examples for convenience.</span></span> <span data-ttu-id="a4b16-124">Azure AD stöder tusentals program i vår [programgalleriet](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) som du kan använda baserat på dina behov och miljö.</span><span class="sxs-lookup"><span data-stu-id="a4b16-124">Azure AD supports thousands of applications in our [application gallery](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) that you can use based on your needs and environment.</span></span> 



[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]