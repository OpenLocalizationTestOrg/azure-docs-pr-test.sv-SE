---
title: "Felsökning av vanliga problem vid skapandet av VHD | Microsoft Docs"
description: "Svar på vanliga felsökning frågor och problem under skapande av virtuell Hårddisk."
services: Azure Marketplace
documentationcenter: 
author: HannibalSII
manager: 
editor: 
ms.assetid: e39563d8-8646-4cb7-b078-8b10ac35b494
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 09/26/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: c4e88a9fbb15dd90d619b159ae1065dfacc1907f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-troubleshoot-common-issues-encountered-during-vhd-creation"></a><span data-ttu-id="b00cf-103">Felsökning av vanliga problem som påträffas under skapande av virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="b00cf-103">How to troubleshoot common issues encountered during VHD creation</span></span>
<span data-ttu-id="b00cf-104">Den här artikeln för att hjälpa en Azure Marketplace utgivaren och/eller medadministratör som kan ha ett problem eller har frågor när du publicerar eller hantera sina virtuella solution(s).</span><span class="sxs-lookup"><span data-stu-id="b00cf-104">This article is provided to help an Azure Marketplace publisher and/or co-administrator that may experience an issue or have common questions while publishing or managing their virtual machine solution(s).</span></span>

1. <span data-ttu-id="b00cf-105">Hur ändrar namnet på värddatorn?</span><span class="sxs-lookup"><span data-stu-id="b00cf-105">How do I change the name of the host?</span></span>
   
    <span data-ttu-id="b00cf-106">När du har skapat VM kan inte användare uppdatera namnet på värden.</span><span class="sxs-lookup"><span data-stu-id="b00cf-106">Once VM is created, users can’t update the name of the host.</span></span>
2. <span data-ttu-id="b00cf-107">Så här återställer du tjänsten Remote Desktop eller dess inloggningslösenord?</span><span class="sxs-lookup"><span data-stu-id="b00cf-107">How to reset the Remote Desktop service or its login password?</span></span>
   
   * [<span data-ttu-id="b00cf-108">Referens för Windows VM</span><span class="sxs-lookup"><span data-stu-id="b00cf-108">Reference for Windows VM</span></span>](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-reset-rdp/)
   * [<span data-ttu-id="b00cf-109">Referens för Linux VM</span><span class="sxs-lookup"><span data-stu-id="b00cf-109">Reference for Linux VM</span></span>](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)
3. <span data-ttu-id="b00cf-110">Hur du skapar nya ssh certifikat?</span><span class="sxs-lookup"><span data-stu-id="b00cf-110">How to generate new ssh certificates?</span></span>
   
   <span data-ttu-id="b00cf-111">Mer information finns på länken: [https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)</span><span class="sxs-lookup"><span data-stu-id="b00cf-111">Please refer to the link: [https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)</span></span>
4. <span data-ttu-id="b00cf-112">Så här konfigurerar du en öppen VPN-certifikat?</span><span class="sxs-lookup"><span data-stu-id="b00cf-112">How to configure an open VPN certificate?</span></span>
   
   <span data-ttu-id="b00cf-113">Mer information finns på länken: [https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/](https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/)</span><span class="sxs-lookup"><span data-stu-id="b00cf-113">Please refer to the link: [https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/](https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/)</span></span>
5. <span data-ttu-id="b00cf-114">Vad är Supportpolicy för att köra Microsoft server-program i virtuella Microsoft Azure-miljön (infrastructure-as-a-service)?</span><span class="sxs-lookup"><span data-stu-id="b00cf-114">What is the support policy for running Microsoft server software in the Microsoft Azure virtual machine environment (infrastructure-as-a-service)?</span></span>
   
   <span data-ttu-id="b00cf-115">Mer information finns på länken: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)</span><span class="sxs-lookup"><span data-stu-id="b00cf-115">Please refer to the link: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)</span></span>
6. <span data-ttu-id="b00cf-116">Har ingen unik identifierare i virtuella datorer?</span><span class="sxs-lookup"><span data-stu-id="b00cf-116">Do Virtual Machines have any unique identifier?</span></span>
   
   <span data-ttu-id="b00cf-117">Azure kodar Azure VM unika ID på varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b00cf-117">Azure encodes Azure VM Unique ID in every VM.</span></span> <span data-ttu-id="b00cf-118">Mer information finns i den här bloggen och dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="b00cf-118">See details in this blog and documentation.</span></span>
7. <span data-ttu-id="b00cf-119">På en virtuell dator hur kan jag hantera tillägg för anpassat skript i Start-uppgiften?</span><span class="sxs-lookup"><span data-stu-id="b00cf-119">In a VM how can I manage the custom script extension in the startup task?</span></span>
   
   <span data-ttu-id="b00cf-120">Mer information finns på länken: [https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/)</span><span class="sxs-lookup"><span data-stu-id="b00cf-120">Please refer to the link: [https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/)</span></span>
8. <span data-ttu-id="b00cf-121">Så här skapar du en virtuell dator från Azure-portalen med den virtuella Hårddisken som har överförts till premium-lagring?</span><span class="sxs-lookup"><span data-stu-id="b00cf-121">How to create a VM from the Azure portal using the VHD that is uploaded to premium storage?</span></span>
   
   <span data-ttu-id="b00cf-122">Vi stöder inte den här funktionen ännu.</span><span class="sxs-lookup"><span data-stu-id="b00cf-122">We do not support this feature yet.</span></span>
9. <span data-ttu-id="b00cf-123">Är en 32-bitars-app som stöds i Azure Marketplace?</span><span class="sxs-lookup"><span data-stu-id="b00cf-123">Is a 32-bit app supported in the Azure Marketplace?</span></span>
   
   <span data-ttu-id="b00cf-124">Hittar du på länken för information om stöd för principen: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)</span><span class="sxs-lookup"><span data-stu-id="b00cf-124">Please refer to the link for details on the support policy: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)</span></span>
10. <span data-ttu-id="b00cf-125">När jag försöker skapa en avbildning från min virtuella hårddiskar, visas felmeddelandet ”. Virtuell Hårddisk är redan registrerad med avbildningslagringsplats som resursen ”i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b00cf-125">Every time I am trying to create an image from my VHDs, I get the error “.VHD is already registered with image repository as the resource” in PowerShell.</span></span> <span data-ttu-id="b00cf-126">Skapade inte någon bild innan eller hittade I en bild med det här namnet i Azure.</span><span class="sxs-lookup"><span data-stu-id="b00cf-126">I did not create any image before nor did I find any image with this name in Azure.</span></span> <span data-ttu-id="b00cf-127">Hur lösa problemet?</span><span class="sxs-lookup"><span data-stu-id="b00cf-127">How do I resolve this?</span></span>
    
    <span data-ttu-id="b00cf-128">Vanligtvis inträffa om användaren etableras en virtuell dator från den här virtuella Hårddisken och det finns ett lås på den virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="b00cf-128">This usually happen if the user provisioned a VM from this VHD and there is a lock on that VHD.</span></span> <span data-ttu-id="b00cf-129">Kontrollera att det finns ingen VM allokerats från den här virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="b00cf-129">Please check that there is no VM allocated from this VHD.</span></span> <span data-ttu-id="b00cf-130">Om felet kvarstår fortfarande sedan du höja ett supportärende med hjälp av den här länken eller från publicering portal rörande detta (information ges i svaret för frågan 11).</span><span class="sxs-lookup"><span data-stu-id="b00cf-130">If the error still persist , then please raise a support ticket using this link or from the Publishing portal regarding this (details are given in the answer of question 11).</span></span>