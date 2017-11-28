---
title: "aaaCloud App Discovery-registerinställningar för Proxy-tjänster | Microsoft Docs"
description: "hello syftet med det här avsnittet är tooprovide du med hello steg du behöver tooperform tooset hello krävs port på hello-datorer som kör hello Cloud App Discovery-agenten."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 8d78e925-e331-40ba-904a-e4ef14260cac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: bb1fe20016459160b4f67cb0125b1781a0260c4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a><span data-ttu-id="ccfbf-103">Cloud App Discovery-registerinställningar för Proxy-tjänster</span><span class="sxs-lookup"><span data-stu-id="ccfbf-103">Cloud App Discovery Registry Settings for Proxy Services</span></span>
<span data-ttu-id="ccfbf-104">Hello Cloud App Discovery-agenten är som standard konfigurerade toouse endast hello portarna 80 eller 443.</span><span class="sxs-lookup"><span data-stu-id="ccfbf-104">By default, hello Cloud App Discovery agent is configured toouse only hello ports 80 or 443.</span></span> <span data-ttu-id="ccfbf-105">Om du planerar att installera Cloud App Discovery i en miljö med en proxyserver som använder en anpassad port (varken 80 eller 443), måste tooconfigure agenter-toouse denna port.</span><span class="sxs-lookup"><span data-stu-id="ccfbf-105">If you are planning on installing Cloud App Discovery in an environment with a proxy server that is using a custom port (neither 80 nor 443), you need tooconfigure your agents toouse this port.</span></span> <span data-ttu-id="ccfbf-106">hello konfigurationen baseras på en registernyckel.</span><span class="sxs-lookup"><span data-stu-id="ccfbf-106">hello configuration is based on a registry key.</span></span>

<span data-ttu-id="ccfbf-107">hello syftet med det här avsnittet är tooprovide du med hello steg du behöver tooperform tooset hello krävs port på hello-datorer som kör hello Cloud App Discovery-agenten.</span><span class="sxs-lookup"><span data-stu-id="ccfbf-107">hello objective of this topic is tooprovide you with hello steps you need tooperform tooset hello required port on hello computers running hello Cloud App Discovery agent.</span></span>

<span data-ttu-id="ccfbf-108">**toomodify hello port som används av hello datorn som kör hello Cloud App Discovery-agenten, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ccfbf-108">**toomodify hello port used by hello computer running hello Cloud App Discovery agent, perform hello following steps:**</span></span>

1. <span data-ttu-id="ccfbf-109">Starta hello Registereditorn.</span><span class="sxs-lookup"><span data-stu-id="ccfbf-109">Start hello registry editor.</span></span> <br> ![Kör](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)
2. <span data-ttu-id="ccfbf-111">Navigera tooor skapa hello följande registernyckel:</span><span class="sxs-lookup"><span data-stu-id="ccfbf-111">Navigate tooor create hello following registry key:</span></span> <br> <span data-ttu-id="ccfbf-112">**HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud App Discovery\Endpoint**</span><span class="sxs-lookup"><span data-stu-id="ccfbf-112">**HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud App Discovery\Endpoint**</span></span> 
3. <span data-ttu-id="ccfbf-113">Skapa en ny **multisträng** värde med namnet **portar**.</span><span class="sxs-lookup"><span data-stu-id="ccfbf-113">Create a new **multi-string** value called **Ports**.</span></span> <span data-ttu-id="ccfbf-114">![Ny](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)</span><span class="sxs-lookup"><span data-stu-id="ccfbf-114">![New](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)</span></span>
4. <span data-ttu-id="ccfbf-115">tooopen hello **redigera multisträng** dialogrutan, dubbelklicka på värdet för hello-portar.</span><span class="sxs-lookup"><span data-stu-id="ccfbf-115">tooopen hello **Edit Multi-String** dialog, double-click hello Ports value.</span></span>
5. <span data-ttu-id="ccfbf-116">Skriv hello följande värden i hello värdet data textruta och lägga till alla anpassade portar som används av din organisation:</span><span class="sxs-lookup"><span data-stu-id="ccfbf-116">In hello Value data textbox, type hello following values and add all custom ports that are used by your organization:</span></span> <br><br><span data-ttu-id="ccfbf-117">
   **80**</span><span class="sxs-lookup"><span data-stu-id="ccfbf-117">
   **80**</span></span> <br><span data-ttu-id="ccfbf-118">
   **8080**</span><span class="sxs-lookup"><span data-stu-id="ccfbf-118">
   **8080**</span></span> <br><span data-ttu-id="ccfbf-119">
   **8118**</span><span class="sxs-lookup"><span data-stu-id="ccfbf-119">
   **8118**</span></span> <br><span data-ttu-id="ccfbf-120">
   **8888**</span><span class="sxs-lookup"><span data-stu-id="ccfbf-120">
   **8888**</span></span> <br><span data-ttu-id="ccfbf-121">
   **81**</span><span class="sxs-lookup"><span data-stu-id="ccfbf-121">
   **81**</span></span> <br><span data-ttu-id="ccfbf-122">
   **12080**</span><span class="sxs-lookup"><span data-stu-id="ccfbf-122">
   **12080**</span></span> <br><span data-ttu-id="ccfbf-123">
   **6999**</span><span class="sxs-lookup"><span data-stu-id="ccfbf-123">
**6999**</span></span> <br><span data-ttu-id="ccfbf-124">
**30606**</span><span class="sxs-lookup"><span data-stu-id="ccfbf-124">
**30606**</span></span> <br><span data-ttu-id="ccfbf-125">
**31595**</span><span class="sxs-lookup"><span data-stu-id="ccfbf-125">
**31595**</span></span> <br><span data-ttu-id="ccfbf-126">
**4080**</span><span class="sxs-lookup"><span data-stu-id="ccfbf-126">
**4080**</span></span> <br><span data-ttu-id="ccfbf-127">
**443**</span><span class="sxs-lookup"><span data-stu-id="ccfbf-127">
**443**</span></span> <br><span data-ttu-id="ccfbf-128">
**1110**</span><span class="sxs-lookup"><span data-stu-id="ccfbf-128">
**1110**</span></span> <br><br><span data-ttu-id="ccfbf-129">
![Redigera multisträng](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)</span><span class="sxs-lookup"><span data-stu-id="ccfbf-129">
![Edit Multi-String](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)</span></span>
6. <span data-ttu-id="ccfbf-130">Klicka på **OK** tooclose hello **redigera multisträng** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ccfbf-130">Click **OK** tooclose hello **Edit Multi-String** dialog.</span></span>

<span data-ttu-id="ccfbf-131">**Ytterligare resurser**</span><span class="sxs-lookup"><span data-stu-id="ccfbf-131">**Additional Resources**</span></span>

* [<span data-ttu-id="ccfbf-132">Hur kan identifiera ej sanktionerade molnappar som används inom organisationen</span><span class="sxs-lookup"><span data-stu-id="ccfbf-132">How can I discover unsanctioned cloud apps that are used within my organization</span></span>](active-directory-cloudappdiscovery-whatis.md) 

