---
title: aaaInstall Endpoint Protection i Azure Security Center | Microsoft Docs
description: "Det här dokumentet beskrivs hur tooimplement hello Azure Security Center rekommendation ** installera Endpoint Protection **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 1599ad5f-d810-421d-aafc-892e831b403f
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: fea891810e042e4d4f6e55094c0cd4de013ba68a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-endpoint-protection-in-azure-security-center"></a><span data-ttu-id="ba878-103">Installera Endpoint Protection i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="ba878-103">Install Endpoint Protection in Azure Security Center</span></span>
<span data-ttu-id="ba878-104">Azure Security Center rekommenderar att du installerar endpoint protection på din virtuella Azure-datorer (VM) om endpoint protection inte är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="ba878-104">Azure Security Center recommends that you install endpoint protection on your Azure virtual machines (VMs) if endpoint protection is not already enabled.</span></span> <span data-ttu-id="ba878-105">Den här rekommendationen gäller tooWindows virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ba878-105">This recommendation applies tooWindows VMs only.</span></span>

> [!NOTE]
> <span data-ttu-id="ba878-106">Den här exempeldistributionen använder Microsoft Antimalware.</span><span class="sxs-lookup"><span data-stu-id="ba878-106">This example deployment uses Microsoft Antimalware.</span></span> <span data-ttu-id="ba878-107">Se [Partner integrering i Azure Security Center](security-center-partner-integration.md#partners-that-integrate-with-security-center) för en lista med partners som är integrerad med Security Center.</span><span class="sxs-lookup"><span data-stu-id="ba878-107">See [Partner Integration in Azure Security Center](security-center-partner-integration.md#partners-that-integrate-with-security-center) for a list of partners integrated with Security Center.</span></span>  
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="ba878-108">Implementera hello rekommendation</span><span class="sxs-lookup"><span data-stu-id="ba878-108">Implement hello recommendation</span></span>

> [!NOTE]
> <span data-ttu-id="ba878-109">Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution.</span><span class="sxs-lookup"><span data-stu-id="ba878-109">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="ba878-110">Det här dokumentet är inte en stegvis guide.</span><span class="sxs-lookup"><span data-stu-id="ba878-110">This document is not a step-by-step guide.</span></span>
>
>

1. <span data-ttu-id="ba878-111">I hello **rekommendationer** bladet väljer **installera Endpoint Protection**.</span><span class="sxs-lookup"><span data-stu-id="ba878-111">In hello **Recommendations** blade, select **Install Endpoint Protection**.</span></span>
   <span data-ttu-id="ba878-112">![Välj installera Endpoint Protection][1]</span><span class="sxs-lookup"><span data-stu-id="ba878-112">![Select Install Endpoint Protection][1]</span></span>
2. <span data-ttu-id="ba878-113">Hej **installera Endpoint Protection** öppnas ett blad med en lista över virtuella datorer utan endpoint protection.</span><span class="sxs-lookup"><span data-stu-id="ba878-113">hello **Install Endpoint Protection** blade opens displaying a list of VMs without endpoint protection.</span></span> <span data-ttu-id="ba878-114">Välj från hello listan hello virtuella datorer som du vill att tooinstall endpoint protection på och klicka på **installera på virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="ba878-114">Select from hello list hello VMs that you want tooinstall endpoint protection on and click **Install on VMs**.</span></span>
   <span data-ttu-id="ba878-115">![Välj virtuella datorer tooinstall Endpoint Protection på][2]</span><span class="sxs-lookup"><span data-stu-id="ba878-115">![Select VMs tooinstall Endpoint Protection on][2]</span></span>
3. <span data-ttu-id="ba878-116">Hej **Välj Endpoint Protection** blad öppnas tooallow du tooselect hello slutpunktsskydd du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="ba878-116">hello **Select Endpoint Protection** blade opens tooallow you tooselect hello endpoint protection solution you want toouse.</span></span> <span data-ttu-id="ba878-117">I det här exemplet ska vi väljer **Microsoft Antimalware**.</span><span class="sxs-lookup"><span data-stu-id="ba878-117">In this example, let's select **Microsoft Antimalware**.</span></span>
   <span data-ttu-id="ba878-118">![Välj Endpoint Protection][3]</span><span class="sxs-lookup"><span data-stu-id="ba878-118">![Select Endpoint Protection][3]</span></span>
4. <span data-ttu-id="ba878-119">Mer information om hello slutpunktsskydd visas.</span><span class="sxs-lookup"><span data-stu-id="ba878-119">Additional information about hello endpoint protection solution is displayed.</span></span> <span data-ttu-id="ba878-120">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ba878-120">Select **Create**.</span></span>
   <span data-ttu-id="ba878-121">![Skapa program mot skadlig kod][4]</span><span class="sxs-lookup"><span data-stu-id="ba878-121">![Create antimalware solution][4]</span></span>
5. <span data-ttu-id="ba878-122">Ange konfigurationsinställningar för hello krävs på hello **lägga till tillägget** bladet och väljer sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="ba878-122">Enter hello required configuration settings on hello **Add Extension** blade, and then select **OK**.</span></span> <span data-ttu-id="ba878-123">toolearn mer om hello konfigurationsinställningar, se [standard och anpassad konfiguration för program mot skadlig kod](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration).</span><span class="sxs-lookup"><span data-stu-id="ba878-123">toolearn more about hello configuration settings, see [Default and Custom Antimalware Configuration](../security/azure-security-antimalware.md#default-and-custom-antimalware-configuration).</span></span>

<span data-ttu-id="ba878-124">[Microsoft Antimalware](../security/azure-security-antimalware.md) nu aktiva vid hello valda virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ba878-124">[Microsoft Antimalware](../security/azure-security-antimalware.md) is now active on hello selected VMs.</span></span>

## <a name="see-also"></a><span data-ttu-id="ba878-125">Se även</span><span class="sxs-lookup"><span data-stu-id="ba878-125">See also</span></span>
<span data-ttu-id="ba878-126">Den här artikeln visar hur hello tooimplement Security Center rekommendation ”installera Endpoint Protection”.</span><span class="sxs-lookup"><span data-stu-id="ba878-126">This article showed you how tooimplement hello Security Center recommendation "Install Endpoint Protection."</span></span> <span data-ttu-id="ba878-127">toolearn mer om hur du aktiverar Microsoft Antimalware i Azure, se hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="ba878-127">toolearn more about enabling Microsoft Antimalware in Azure, see hello following document:</span></span>

* <span data-ttu-id="ba878-128">[Microsoft Antimalware för molntjänster och virtuella datorer](../security/azure-security-antimalware.md) – Lär dig hur toodeploy Microsoft Antimalware.</span><span class="sxs-lookup"><span data-stu-id="ba878-128">[Microsoft Antimalware for Cloud Services and Virtual Machines](../security/azure-security-antimalware.md) -- Learn how toodeploy Microsoft Antimalware.</span></span>

<span data-ttu-id="ba878-129">toolearn mer om Security Center finns hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="ba878-129">toolearn more about Security Center, see hello following documents:</span></span>

* <span data-ttu-id="ba878-130">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper.</span><span class="sxs-lookup"><span data-stu-id="ba878-130">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies.</span></span>
* <span data-ttu-id="ba878-131">[Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="ba878-131">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="ba878-132">[Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa.</span><span class="sxs-lookup"><span data-stu-id="ba878-132">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="ba878-133">[Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="ba878-133">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="ba878-134">[Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.</span><span class="sxs-lookup"><span data-stu-id="ba878-134">[Monitoring partner solutions with Azure Security Center](security-center-partner-solutions.md) -- Learn how toomonitor hello health status of your partner solutions.</span></span>
* <span data-ttu-id="ba878-135">[Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ba878-135">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="ba878-136">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) --hittar du blogginlägg om säkerhet och Azure kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="ba878-136">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/select-install-endpoint-protection.png
[2]:./media/security-center-install-endpoint-protection/install-endpoint-protection-blade.png
[3]:./media/security-center-install-endpoint-protection/select-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/create-antimalware-solution.png
