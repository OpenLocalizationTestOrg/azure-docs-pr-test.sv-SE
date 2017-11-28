---
title: aaaApply systemuppdateringar i Azure Security Center | Microsoft Docs
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendationer ** gäller system uppdateringar ** och ** startas om efter system uppdateringar **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: e5bd7f55-38fd-4ebb-84ab-32bd60e9fa7a
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2017
ms.author: terrylan
ms.openlocfilehash: 02024f1558b4758c09141fe1934c2e1a9845cc96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="apply-system-updates-in-azure-security-center"></a><span data-ttu-id="4b381-103">Tillämpa uppdateringar i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="4b381-103">Apply system updates in Azure Security Center</span></span>
<span data-ttu-id="4b381-104">Azure Security Center övervakar dagligen Windows och Linux virtuella datorer (VM) efter saknade uppdateringar av operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="4b381-104">Azure Security Center monitors daily Windows and  Linux virtual machines (VMs) for missing operating system updates.</span></span> <span data-ttu-id="4b381-105">Security Center hämtar en lista med tillgängliga säkerhetsuppdateringar och viktiga uppdateringar från Windows Update eller Windows Server Update Services (WSUS), beroende på vilken tjänsten har konfigurerats på en Windows-VM.</span><span class="sxs-lookup"><span data-stu-id="4b381-105">Security Center retrieves a list of available security and critical updates from Windows Update or Windows Server Update Services (WSUS), depending on which service is configured on a Windows VM.</span></span>  <span data-ttu-id="4b381-106">Säkerhetscenter kontrollerar också hello senaste uppdateringarna i Linux-system.</span><span class="sxs-lookup"><span data-stu-id="4b381-106">Security Center also checks for hello latest updates in Linux systems.</span></span> <span data-ttu-id="4b381-107">Om den virtuella datorn saknas för en uppdatering av system, rekommenderar Security Center att du installerar uppdateringar</span><span class="sxs-lookup"><span data-stu-id="4b381-107">If your VM is missing a system update, Security Center will recommend that you apply system updates</span></span>

> [!NOTE]
> <span data-ttu-id="4b381-108">Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.</span><span class="sxs-lookup"><span data-stu-id="4b381-108">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="4b381-109">Det är alltså inte en steg-för-steg-guide.</span><span class="sxs-lookup"><span data-stu-id="4b381-109">This is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="4b381-110">Implementera hello rekommendation</span><span class="sxs-lookup"><span data-stu-id="4b381-110">Implement hello recommendation</span></span>
1. <span data-ttu-id="4b381-111">I hello **rekommendationer** bladet väljer **gäller systemuppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="4b381-111">In hello **Recommendations** blade, select **Apply system updates**.</span></span>

   ![Tillämpa systemuppdateringar][1]
2. <span data-ttu-id="4b381-113">Hej **gäller systemuppdateringar** öppnas ett blad med en lista över virtuella datorer som saknar systemuppdateringar.</span><span class="sxs-lookup"><span data-stu-id="4b381-113">hello **Apply system updates** blade opens displaying a list of VMs missing system updates.</span></span> <span data-ttu-id="4b381-114">Välj en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="4b381-114">Select a VM.</span></span>

   ![Välj en virtuell dator][2]
3. <span data-ttu-id="4b381-116">Då öppnas ett blad med en lista över saknade uppdateringar för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="4b381-116">A blade opens displaying a list of missing updates for that VM.</span></span> <span data-ttu-id="4b381-117">Välj en uppdatering av system.</span><span class="sxs-lookup"><span data-stu-id="4b381-117">Select a system update.</span></span> <span data-ttu-id="4b381-118">I det här exemplet väljer vi KB3156016.</span><span class="sxs-lookup"><span data-stu-id="4b381-118">In this example, let’s select KB3156016.</span></span>

   ![Säkerhetsuppdateringar som saknas][3]

4. <span data-ttu-id="4b381-120">Åtgärderna i hello hello **säkerhetsuppdatering** bladet tooapply hello saknad uppdatering.</span><span class="sxs-lookup"><span data-stu-id="4b381-120">Follow hello steps in hello **Security Update** blade tooapply hello missing update.</span></span>

   ![Säkerhetsuppdatering][4]

## <a name="reboot-after-system-updates"></a><span data-ttu-id="4b381-122">Starta om datorn efter uppdateringarna</span><span class="sxs-lookup"><span data-stu-id="4b381-122">Reboot after system updates</span></span>
1. <span data-ttu-id="4b381-123">Returnera toohello **rekommendationer** bladet.</span><span class="sxs-lookup"><span data-stu-id="4b381-123">Return toohello **Recommendations** blade.</span></span> <span data-ttu-id="4b381-124">En ny post skapades när du använt systemuppdateringar kallas **startas om efter systemuppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="4b381-124">A new entry was generated after you applied system updates, called **Reboot after system updates**.</span></span> <span data-ttu-id="4b381-125">Den här posten kan du vet att du måste tooreboot hello VM toocomplete hello process för tillämpning av uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="4b381-125">This entry lets you know that you need tooreboot hello VM toocomplete hello process of applying system updates.</span></span>

   ![Starta om datorn efter uppdateringarna][5]
2. <span data-ttu-id="4b381-127">Välj **startas om efter systemuppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="4b381-127">Select **Reboot after system updates**.</span></span> <span data-ttu-id="4b381-128">Då öppnas **omstart är väntande toocomplete systemuppdateringar** bladet visas en lista över virtuella datorer som du behöver toorestart toocomplete hello gäller system uppdateringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="4b381-128">This opens **A restart is pending toocomplete system updates** blade displaying a list of VMs that you need toorestart toocomplete hello apply system updates process.</span></span>

   ![Väntande omstart][6]

<span data-ttu-id="4b381-130">Starta om hello VM från Azure toocomplete hello processen.</span><span class="sxs-lookup"><span data-stu-id="4b381-130">Restart hello VM from Azure toocomplete hello process.</span></span>

## <a name="see-also"></a><span data-ttu-id="4b381-131">Se även</span><span class="sxs-lookup"><span data-stu-id="4b381-131">See also</span></span>
<span data-ttu-id="4b381-132">toolearn mer om Security Center finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="4b381-132">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="4b381-133">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="4b381-133">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="4b381-134">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="4b381-134">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="4b381-135">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.</span><span class="sxs-lookup"><span data-stu-id="4b381-135">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="4b381-136">[Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="4b381-136">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="4b381-137">[Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="4b381-137">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="4b381-138">[Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4b381-138">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="4b381-139">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --hittar du blogginlägg om säkerhet och Azure kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="4b381-139">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/recommendation.png
[2]:./media/security-center-apply-system-updates/select-vm.png
[3]: ./media/security-center-apply-system-updates/missing-security-updates.png
[4]: ./media/security-center-apply-system-updates/security-update.png
[5]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[6]: ./media/security-center-apply-system-updates/restart-pending.png
