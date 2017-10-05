---
title: "Cloud App Discovery-registerinställningar för Proxy-tjänster | Microsoft Docs"
description: "Syftet med det här avsnittet är att förse dig med de steg som du behöver utföra för att ställa in den begärda porten på datorer som kör Cloud App Discovery-agenten."
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
ms.openlocfilehash: ea15dc9a9f20a296e622c8fb1011f7ee99de3e99
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a><span data-ttu-id="e794e-103">Cloud App Discovery-registerinställningar för Proxy-tjänster</span><span class="sxs-lookup"><span data-stu-id="e794e-103">Cloud App Discovery Registry Settings for Proxy Services</span></span>
<span data-ttu-id="e794e-104">Cloud App Discovery-agenten har konfigurerats för att använda endast portarna 80 eller 443 som standard.</span><span class="sxs-lookup"><span data-stu-id="e794e-104">By default, the Cloud App Discovery agent is configured to use only the ports 80 or 443.</span></span> <span data-ttu-id="e794e-105">Om du planerar att installera Cloud App Discovery i en miljö med en proxyserver som använder en anpassad port (varken 80 eller 443), måste du konfigurera dina agenter för att använda den här porten.</span><span class="sxs-lookup"><span data-stu-id="e794e-105">If you are planning on installing Cloud App Discovery in an environment with a proxy server that is using a custom port (neither 80 nor 443), you need to configure your agents to use this port.</span></span> <span data-ttu-id="e794e-106">Konfigurationen är baserad på en registernyckel.</span><span class="sxs-lookup"><span data-stu-id="e794e-106">The configuration is based on a registry key.</span></span>

<span data-ttu-id="e794e-107">Syftet med det här avsnittet är att förse dig med de steg som du behöver utföra för att ställa in den begärda porten på datorer som kör Cloud App Discovery-agenten.</span><span class="sxs-lookup"><span data-stu-id="e794e-107">The objective of this topic is to provide you with the steps you need to perform to set the required port on the computers running the Cloud App Discovery agent.</span></span>

<span data-ttu-id="e794e-108">**Utför följande steg om du vill ändra den port som används av datorn som kör Cloud App Discovery-agenten:**</span><span class="sxs-lookup"><span data-stu-id="e794e-108">**To modify the port used by the computer running the Cloud App Discovery agent, perform the following steps:**</span></span>

1. <span data-ttu-id="e794e-109">Starta Registereditorn.</span><span class="sxs-lookup"><span data-stu-id="e794e-109">Start the registry editor.</span></span> <br> ![Kör](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)
2. <span data-ttu-id="e794e-111">Navigera till eller skapa följande registernyckel:</span><span class="sxs-lookup"><span data-stu-id="e794e-111">Navigate to or create the following registry key:</span></span> <br> <span data-ttu-id="e794e-112">**HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud App Discovery\Endpoint**</span><span class="sxs-lookup"><span data-stu-id="e794e-112">**HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud App Discovery\Endpoint**</span></span> 
3. <span data-ttu-id="e794e-113">Skapa en ny **multisträng** värde med namnet **portar**.</span><span class="sxs-lookup"><span data-stu-id="e794e-113">Create a new **multi-string** value called **Ports**.</span></span> <span data-ttu-id="e794e-114">![Ny](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)</span><span class="sxs-lookup"><span data-stu-id="e794e-114">![New](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)</span></span>
4. <span data-ttu-id="e794e-115">Öppna den **redigera multisträng** dialogrutan, dubbelklicka på värdet portar.</span><span class="sxs-lookup"><span data-stu-id="e794e-115">To open the **Edit Multi-String** dialog, double-click the Ports value.</span></span>
5. <span data-ttu-id="e794e-116">Ange följande värden i textrutan värde data och Lägg till alla anpassade portar som används av din organisation:</span><span class="sxs-lookup"><span data-stu-id="e794e-116">In the Value data textbox, type the following values and add all custom ports that are used by your organization:</span></span> <br><br><span data-ttu-id="e794e-117">
   **80**</span><span class="sxs-lookup"><span data-stu-id="e794e-117">
   **80**</span></span> <br><span data-ttu-id="e794e-118">
   **8080**</span><span class="sxs-lookup"><span data-stu-id="e794e-118">
   **8080**</span></span> <br><span data-ttu-id="e794e-119">
   **8118**</span><span class="sxs-lookup"><span data-stu-id="e794e-119">
   **8118**</span></span> <br><span data-ttu-id="e794e-120">
   **8888**</span><span class="sxs-lookup"><span data-stu-id="e794e-120">
   **8888**</span></span> <br><span data-ttu-id="e794e-121">
   **81**</span><span class="sxs-lookup"><span data-stu-id="e794e-121">
   **81**</span></span> <br><span data-ttu-id="e794e-122">
   **12080**</span><span class="sxs-lookup"><span data-stu-id="e794e-122">
   **12080**</span></span> <br><span data-ttu-id="e794e-123">
   **6999**</span><span class="sxs-lookup"><span data-stu-id="e794e-123">
**6999**</span></span> <br><span data-ttu-id="e794e-124">
**30606**</span><span class="sxs-lookup"><span data-stu-id="e794e-124">
**30606**</span></span> <br><span data-ttu-id="e794e-125">
**31595**</span><span class="sxs-lookup"><span data-stu-id="e794e-125">
**31595**</span></span> <br><span data-ttu-id="e794e-126">
**4080**</span><span class="sxs-lookup"><span data-stu-id="e794e-126">
**4080**</span></span> <br><span data-ttu-id="e794e-127">
**443**</span><span class="sxs-lookup"><span data-stu-id="e794e-127">
**443**</span></span> <br><span data-ttu-id="e794e-128">
**1110**</span><span class="sxs-lookup"><span data-stu-id="e794e-128">
**1110**</span></span> <br><br><span data-ttu-id="e794e-129">
![Redigera multisträng](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)</span><span class="sxs-lookup"><span data-stu-id="e794e-129">
![Edit Multi-String](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)</span></span>
6. <span data-ttu-id="e794e-130">Klicka på **OK** att stänga den **redigera multisträng** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e794e-130">Click **OK** to close the **Edit Multi-String** dialog.</span></span>

<span data-ttu-id="e794e-131">**Ytterligare resurser**</span><span class="sxs-lookup"><span data-stu-id="e794e-131">**Additional Resources**</span></span>

* [<span data-ttu-id="e794e-132">Hur kan identifiera ej sanktionerade molnappar som används inom organisationen</span><span class="sxs-lookup"><span data-stu-id="e794e-132">How can I discover unsanctioned cloud apps that are used within my organization</span></span>](active-directory-cloudappdiscovery-whatis.md) 

