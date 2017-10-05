---
title: Azure Security Center och Windows-datorer i Azure | Microsoft Docs
description: "Läs mer om säkerheten för din Windows Azure-dator med Azure Security Center."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/01/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: adb00e28b0b204858a763f83836ee2ac96f8f9e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-virtual-machine-security-by-using-azure-security-center"></a><span data-ttu-id="a6e65-103">Övervaka virtuella säkerhet med hjälp av Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="a6e65-103">Monitor virtual machine security by using Azure Security Center</span></span>

<span data-ttu-id="a6e65-104">Azure Security Center kan hjälpa dig få insyn i Azure-resurs säkerhetsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="a6e65-104">Azure Security Center can help you gain visibility into your Azure resource security practices.</span></span> <span data-ttu-id="a6e65-105">Security Center ger övervakning av integrerad säkerhet.</span><span class="sxs-lookup"><span data-stu-id="a6e65-105">Security Center offers integrated security monitoring.</span></span> <span data-ttu-id="a6e65-106">Det kan upptäcka hot som annars kan förbli oupptäckta.</span><span class="sxs-lookup"><span data-stu-id="a6e65-106">It can detect threats that otherwise might go unnoticed.</span></span> <span data-ttu-id="a6e65-107">I kursen får du lära dig om Azure Security Center och hur du:</span><span class="sxs-lookup"><span data-stu-id="a6e65-107">In this tutorial, you learn about Azure Security Center, and how to:</span></span>
 
> [!div class="checklist"]
> * <span data-ttu-id="a6e65-108">Konfigurera datainsamling</span><span class="sxs-lookup"><span data-stu-id="a6e65-108">Set up data collection</span></span>
> * <span data-ttu-id="a6e65-109">Ställa in säkerhetsprinciper</span><span class="sxs-lookup"><span data-stu-id="a6e65-109">Set up security policies</span></span>
> * <span data-ttu-id="a6e65-110">Visa och lösa konfigurationsproblem hälsa</span><span class="sxs-lookup"><span data-stu-id="a6e65-110">View and fix configuration health issues</span></span>
> * <span data-ttu-id="a6e65-111">Granska identifierade hot</span><span class="sxs-lookup"><span data-stu-id="a6e65-111">Review detected threats</span></span>  

## <a name="security-center-overview"></a><span data-ttu-id="a6e65-112">Översikt över Security Center</span><span class="sxs-lookup"><span data-stu-id="a6e65-112">Security Center overview</span></span>

<span data-ttu-id="a6e65-113">Security Center identifierar potentiella konfigurationsproblem för virtuell dator (VM) och mål säkerhetshot.</span><span class="sxs-lookup"><span data-stu-id="a6e65-113">Security Center identifies potential virtual machine (VM) configuration issues and targeted security threats.</span></span> <span data-ttu-id="a6e65-114">Dessa kan innehålla virtuella datorer som saknar nätverkssäkerhetsgrupper och okrypterade diskar brute force-attacker för Remote Desktop Protocol (RDP).</span><span class="sxs-lookup"><span data-stu-id="a6e65-114">These might include VMs that are missing network security groups, unencrypted disks, and brute-force Remote Desktop Protocol (RDP) attacks.</span></span> <span data-ttu-id="a6e65-115">Informationen visas på instrumentpanelen i Security Center i lätt att läsa diagram.</span><span class="sxs-lookup"><span data-stu-id="a6e65-115">The information is shown on the Security Center dashboard in easy-to-read graphs.</span></span>

<span data-ttu-id="a6e65-116">För att komma åt instrumentpanelen i Security Center i Azure-portalen på menyn, Välj **Security Center**.</span><span class="sxs-lookup"><span data-stu-id="a6e65-116">To access the Security Center dashboard, in the Azure portal, on the menu, select  **Security Center**.</span></span> <span data-ttu-id="a6e65-117">På instrumentpanelen, se säkerhetshälsa för Azure-miljön, hitta antalet aktuella rekommendationer och visa aktuell status för hot aviseringar.</span><span class="sxs-lookup"><span data-stu-id="a6e65-117">On the dashboard, you can see the security health of your Azure environment, find a count of current recommendations, and view the current state of threat alerts.</span></span> <span data-ttu-id="a6e65-118">Du kan expandera varje övergripande diagram om du vill se mer information.</span><span class="sxs-lookup"><span data-stu-id="a6e65-118">You can expand each high-level chart to see more detail.</span></span>

![Instrumentpanelen Security Center](./media/tutorial-azure-security/asc-dash.png)

<span data-ttu-id="a6e65-120">Security Center är mer omfattande än identifiering av data för att ge rekommendationer för problem som upptäcks.</span><span class="sxs-lookup"><span data-stu-id="a6e65-120">Security Center goes beyond data discovery to provide recommendations for issues that it detects.</span></span> <span data-ttu-id="a6e65-121">Om en virtuell dator har distribuerats utan ett anslutet nätverkssäkerhetsgrupp, visar Säkerhetscenter en rekommendation med steg du kan vidta.</span><span class="sxs-lookup"><span data-stu-id="a6e65-121">For example, if a VM was deployed without an attached network security group, Security Center displays a recommendation, with remediation steps you can take.</span></span> <span data-ttu-id="a6e65-122">Du kan hämta automatiska reparationer utan att lämna samband med Security Center.</span><span class="sxs-lookup"><span data-stu-id="a6e65-122">You get automated remediation without leaving the context of Security Center.</span></span>  

![Rekommendationer](./media/tutorial-azure-security/recommendations.png)

## <a name="set-up-data-collection"></a><span data-ttu-id="a6e65-124">Konfigurera datainsamling</span><span class="sxs-lookup"><span data-stu-id="a6e65-124">Set up data collection</span></span>

<span data-ttu-id="a6e65-125">Innan du kan få en överblick över VM säkerhetskonfigurationer, måste du ställer in insamling av Security Center.</span><span class="sxs-lookup"><span data-stu-id="a6e65-125">Before you can get visibility into VM security configurations, you need to set up Security Center data collection.</span></span> <span data-ttu-id="a6e65-126">Detta innebär att aktivera insamling av data och skapa ett Azure storage-konto för att lagra data som samlats in.</span><span class="sxs-lookup"><span data-stu-id="a6e65-126">This involves turning on data collection and creating an Azure storage account to hold collected data.</span></span> 

1. <span data-ttu-id="a6e65-127">Klicka på instrumentpanelen i Security Center **säkerhetsprincip**, och sedan välja din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a6e65-127">On the Security Center dashboard, click **Security policy**, and then select your subscription.</span></span> 
2. <span data-ttu-id="a6e65-128">För **datainsamling**väljer **på**.</span><span class="sxs-lookup"><span data-stu-id="a6e65-128">For **Data collection**, select **On**.</span></span>
3. <span data-ttu-id="a6e65-129">Om du vill skapa ett lagringskonto **väljer något lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="a6e65-129">To create a storage account, select **Choose a storage account**.</span></span> <span data-ttu-id="a6e65-130">Markera **OK**.</span><span class="sxs-lookup"><span data-stu-id="a6e65-130">Then, select **OK**.</span></span>
4. <span data-ttu-id="a6e65-131">På den **säkerhetsprincip** bladet väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="a6e65-131">On the **Security Policy** blade, select **Save**.</span></span> 

<span data-ttu-id="a6e65-132">Security Center data collection agent installeras på alla virtuella datorer och datainsamlingen påbörjas.</span><span class="sxs-lookup"><span data-stu-id="a6e65-132">The Security Center data collection agent is then installed on all VMs, and data collection begins.</span></span> 

## <a name="set-up-a-security-policy"></a><span data-ttu-id="a6e65-133">Ställ in en säkerhetsprincip</span><span class="sxs-lookup"><span data-stu-id="a6e65-133">Set up a security policy</span></span>

<span data-ttu-id="a6e65-134">IPSec-principer används för att definiera objekten som Säkerhetscenter samlar in data och ger rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="a6e65-134">Security policies are used to define the items for which Security Center collects data and makes recommendations.</span></span> <span data-ttu-id="a6e65-135">Du kan använda olika principer för olika uppsättningar av Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="a6e65-135">You can apply different security policies to different sets of Azure resources.</span></span> <span data-ttu-id="a6e65-136">Även om Azure-resurser som standard ska utvärderas mot alla principobjekt kan du inaktivera enskilda principobjekt för alla Azure-resurser eller för en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a6e65-136">Although by default Azure resources are evaluated against all policy items, you can turn off individual policy items for all Azure resources or for a resource group.</span></span> <span data-ttu-id="a6e65-137">Detaljerad information om säkerhetsprinciper från security Center finns [ställa in säkerhetsprinciper i Azure Security Center](../../security-center/security-center-policies.md).</span><span class="sxs-lookup"><span data-stu-id="a6e65-137">For in-depth information about Security Center security policies, see [Set security policies in Azure Security Center](../../security-center/security-center-policies.md).</span></span> 

<span data-ttu-id="a6e65-138">Att ställa in en säkerhetsprincip för alla Azure-resurser:</span><span class="sxs-lookup"><span data-stu-id="a6e65-138">To set up a security policy for all Azure resources:</span></span>

1. <span data-ttu-id="a6e65-139">Välj på instrumentpanelen i Security Center **säkerhetsprincip**, och sedan välja din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a6e65-139">On the Security Center dashboard, select **Security policy**, and then select your subscription.</span></span>
2. <span data-ttu-id="a6e65-140">Välj **skyddsprincip**.</span><span class="sxs-lookup"><span data-stu-id="a6e65-140">Select **Prevention policy**.</span></span>
3. <span data-ttu-id="a6e65-141">Aktivera eller inaktivera principobjekt som du vill koppla till alla Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="a6e65-141">Turn on or turn off policy items that you want to apply to all Azure resources.</span></span>
4. <span data-ttu-id="a6e65-142">När du är klar med att välja inställningarna väljer **OK**.</span><span class="sxs-lookup"><span data-stu-id="a6e65-142">When you're finished selecting your settings, select **OK**.</span></span>
5. <span data-ttu-id="a6e65-143">På den **säkerhetsprincip** bladet väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="a6e65-143">On the **Security policy** blade, select **Save**.</span></span> 

<span data-ttu-id="a6e65-144">Att konfigurera en princip för en viss resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="a6e65-144">To set up a policy for a specific resource group:</span></span>

1. <span data-ttu-id="a6e65-145">Välj på instrumentpanelen i Security Center **säkerhetsprincip**, och välj sedan en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a6e65-145">On the Security Center dashboard, select **Security policy**, and then select a resource group.</span></span>
2. <span data-ttu-id="a6e65-146">Välj **skyddsprincip**.</span><span class="sxs-lookup"><span data-stu-id="a6e65-146">Select **Prevention policy**.</span></span>
3. <span data-ttu-id="a6e65-147">Aktivera eller inaktivera principobjekt som du vill koppla till resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="a6e65-147">Turn on or turn off policy items that you want to apply to the resource group.</span></span>
4. <span data-ttu-id="a6e65-148">Under **arv**väljer **unik**.</span><span class="sxs-lookup"><span data-stu-id="a6e65-148">Under **INHERITANCE**, select **Unique**.</span></span>
5. <span data-ttu-id="a6e65-149">När du är klar med att välja inställningarna väljer **OK**.</span><span class="sxs-lookup"><span data-stu-id="a6e65-149">When you're finished selecting your settings, select **OK**.</span></span>
6. <span data-ttu-id="a6e65-150">På den **säkerhetsprincip** bladet väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="a6e65-150">On the **Security policy** blade, select **Save**.</span></span>  

<span data-ttu-id="a6e65-151">Du kan också stänga av insamling av data för en viss resursgrupp på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="a6e65-151">You also can turn off data collection for a specific resource group on this page.</span></span>

<span data-ttu-id="a6e65-152">I följande exempel skapas en unik princip för en resursgrupp med namnet *myResoureGroup*.</span><span class="sxs-lookup"><span data-stu-id="a6e65-152">In the following example, a unique policy has been created for a resource group named *myResoureGroup*.</span></span> <span data-ttu-id="a6e65-153">Disk kryptering och web application brandväggen rekommendationer är inaktiverade i den här principen.</span><span class="sxs-lookup"><span data-stu-id="a6e65-153">In this policy, disk encryption and web application firewall recommendations are turned off.</span></span>

![Unik princip](./media/tutorial-azure-security/unique-policy.png)

## <a name="view-vm-configuration-health"></a><span data-ttu-id="a6e65-155">Visa konfigurationshälsa för VM</span><span class="sxs-lookup"><span data-stu-id="a6e65-155">View VM configuration health</span></span>

<span data-ttu-id="a6e65-156">När du har aktiverat datainsamling och ange en säkerhetsprincip för, börjar Security Center att ge aviseringar och rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="a6e65-156">After you've turned on data collection and set a security policy, Security Center begins to provide alerts and recommendations.</span></span> <span data-ttu-id="a6e65-157">Eftersom virtuella datorer distribueras är data collection agenten installerad.</span><span class="sxs-lookup"><span data-stu-id="a6e65-157">As VMs are deployed, the data collection agent is installed.</span></span> <span data-ttu-id="a6e65-158">Security Center består av data för de nya virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="a6e65-158">Security Center is then populated with data for the new VMs.</span></span> <span data-ttu-id="a6e65-159">Detaljerad information om hälsa för VM-konfiguration, se [skydda dina virtuella datorer i Security Center](../../security-center/security-center-virtual-machine-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="a6e65-159">For in-depth information about VM configuration health, see [Protect your VMs in Security Center](../../security-center/security-center-virtual-machine-recommendations.md).</span></span> 

<span data-ttu-id="a6e65-160">När data har samlats in sammanställs resurshälsa för varje virtuell dator och relaterade Azure-resurs.</span><span class="sxs-lookup"><span data-stu-id="a6e65-160">As data is collected, the resource health for each VM and related Azure resource is aggregated.</span></span> <span data-ttu-id="a6e65-161">Informationen visas i ett enkelt att läsa diagram.</span><span class="sxs-lookup"><span data-stu-id="a6e65-161">The information is shown in an easy-to-read chart.</span></span> 

<span data-ttu-id="a6e65-162">Visa resurshälsa:</span><span class="sxs-lookup"><span data-stu-id="a6e65-162">To view resource health:</span></span>

1.  <span data-ttu-id="a6e65-163">På Security Center instrumentpanelen under **resurssäkerhetshälsa**väljer **Compute**.</span><span class="sxs-lookup"><span data-stu-id="a6e65-163">On the Security Center dashboard, under **Resource security health**, select **Compute**.</span></span> 
2.  <span data-ttu-id="a6e65-164">På den **Compute** bladet väljer **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="a6e65-164">On the **Compute** blade, select **Virtual machines**.</span></span> <span data-ttu-id="a6e65-165">Den här vyn visar en sammanfattning av status för konfiguration för dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a6e65-165">This view provides a summary of the configuration status for all your VMs.</span></span>

![Beräkna hälsa](./media/tutorial-azure-security/compute-health.png)

<span data-ttu-id="a6e65-167">Om du vill se alla rekommendationer för en virtuell dator, väljer du den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a6e65-167">To see all recommendations for a VM, select the VM.</span></span> <span data-ttu-id="a6e65-168">Rekommendationer och reparation beskrivs närmare i nästa avsnitt i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="a6e65-168">Recommendations and remediation are covered in more detail in the next section of this tutorial.</span></span>

## <a name="remediate-configuration-issues"></a><span data-ttu-id="a6e65-169">Åtgärda problem med konfiguration</span><span class="sxs-lookup"><span data-stu-id="a6e65-169">Remediate configuration issues</span></span>

<span data-ttu-id="a6e65-170">När Security Center börjar fylla med konfigurationsdata, görs rekommendationer baserat på den säkerhetsprincip som du konfigurerar.</span><span class="sxs-lookup"><span data-stu-id="a6e65-170">After Security Center begins to populate with configuration data, recommendations are made based on the security policy you set up.</span></span> <span data-ttu-id="a6e65-171">Till exempel om en virtuell dator har konfigurerats utan en nätverkssäkerhetsgrupp görs en rekommendation för att skapa en.</span><span class="sxs-lookup"><span data-stu-id="a6e65-171">For instance, if a VM was set up without an associated network security group, a recommendation is made to create one.</span></span> 

<span data-ttu-id="a6e65-172">Visa en lista över alla rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a6e65-172">To see a list of all recommendations:</span></span> 

1. <span data-ttu-id="a6e65-173">Välj på instrumentpanelen i Security Center **rekommendationer**.</span><span class="sxs-lookup"><span data-stu-id="a6e65-173">On the Security Center dashboard, select **Recommendations**.</span></span>
2. <span data-ttu-id="a6e65-174">Välj en specifik rekommendation.</span><span class="sxs-lookup"><span data-stu-id="a6e65-174">Select a specific recommendation.</span></span> <span data-ttu-id="a6e65-175">En lista över alla resurser som rekommendationen gäller visas.</span><span class="sxs-lookup"><span data-stu-id="a6e65-175">A list of all resources for which the recommendation applies appears.</span></span>
3. <span data-ttu-id="a6e65-176">Välj en specifik resurs för att tillämpa en rekommendation.</span><span class="sxs-lookup"><span data-stu-id="a6e65-176">To apply a recommendation, select a specific resource.</span></span> 
4. <span data-ttu-id="a6e65-177">Följ anvisningarna för steg.</span><span class="sxs-lookup"><span data-stu-id="a6e65-177">Follow the instructions for remediation steps.</span></span> 

<span data-ttu-id="a6e65-178">I många fall innehåller Security Center tillämplig steg som du kan vidta en rekommendation utan att lämna Security Center.</span><span class="sxs-lookup"><span data-stu-id="a6e65-178">In many cases, Security Center provides actionable steps you can take to address a recommendation without leaving Security Center.</span></span> <span data-ttu-id="a6e65-179">I följande exempel identifierar Security Center en nätverkssäkerhetsgrupp som har en obegränsad inkommande regel.</span><span class="sxs-lookup"><span data-stu-id="a6e65-179">In the following example, Security Center detects a network security group that has an unrestricted inbound rule.</span></span> <span data-ttu-id="a6e65-180">På sidan rekommendation kan du välja den **redigera regler för inkommande trafik** knappen.</span><span class="sxs-lookup"><span data-stu-id="a6e65-180">On the recommendation page, you can select the **Edit inbound rules** button.</span></span> <span data-ttu-id="a6e65-181">Användargränssnittet som behövs för att ändra regeln visas.</span><span class="sxs-lookup"><span data-stu-id="a6e65-181">The UI that is needed to modify the rule appears.</span></span> 

![Rekommendationer](./media/tutorial-azure-security/remediation.png)

<span data-ttu-id="a6e65-183">De är märkta som löst som rekommendationer har åtgärdats.</span><span class="sxs-lookup"><span data-stu-id="a6e65-183">As recommendations are remediated, they are marked as resolved.</span></span> 

## <a name="view-detected-threats"></a><span data-ttu-id="a6e65-184">Visa identifierade hot</span><span class="sxs-lookup"><span data-stu-id="a6e65-184">View detected threats</span></span>

<span data-ttu-id="a6e65-185">Förutom resurs konfigurationsrekommendationer visas hotidentifieringsaviseringar i Security Center.</span><span class="sxs-lookup"><span data-stu-id="a6e65-185">In addition to resource configuration recommendations, Security Center displays threat detection alerts.</span></span> <span data-ttu-id="a6e65-186">Aviseringar säkerhetsfunktion sammanställer data som samlas in från varje VM, Azure nätverk loggar och anslutna partnerlösningar att upptäcka säkerhetshot mot Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="a6e65-186">The security alerts feature aggregates data collected from each VM, Azure networking logs, and connected partner solutions to detect security threats against Azure resources.</span></span> <span data-ttu-id="a6e65-187">För detaljerad information om funktionerna i Security Center threat detection Se [identifieringsfunktionerna i Azure Security Center](../../security-center/security-center-detection-capabilities.md).</span><span class="sxs-lookup"><span data-stu-id="a6e65-187">For in-depth information about Security Center threat detection capabilities, see [Azure Security Center detection capabilities](../../security-center/security-center-detection-capabilities.md).</span></span>

<span data-ttu-id="a6e65-188">Aviseringar säkerhetsfunktion kräver Security Center prisnivån ökas från *lediga* till *Standard*.</span><span class="sxs-lookup"><span data-stu-id="a6e65-188">The security alerts feature requires the Security Center pricing tier to be increased from *Free* to *Standard*.</span></span> <span data-ttu-id="a6e65-189">En 30-dagars **kostnadsfri utvärderingsversion** är tillgänglig när du flyttar till den här högre prisnivå.</span><span class="sxs-lookup"><span data-stu-id="a6e65-189">A 30-day **free trial** is available when you move to this higher pricing tier.</span></span> 

<span data-ttu-id="a6e65-190">Ändra prisnivån:</span><span class="sxs-lookup"><span data-stu-id="a6e65-190">To change the pricing tier:</span></span>  

1. <span data-ttu-id="a6e65-191">Klicka på instrumentpanelen i Security Center **säkerhetsprincip**, och sedan välja din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a6e65-191">On the Security Center dashboard, click **Security policy**, and then select your subscription.</span></span>
2. <span data-ttu-id="a6e65-192">Välj **prisnivå**.</span><span class="sxs-lookup"><span data-stu-id="a6e65-192">Select **Pricing tier**.</span></span>
3. <span data-ttu-id="a6e65-193">Välj ny nivå och välj sedan **Välj**.</span><span class="sxs-lookup"><span data-stu-id="a6e65-193">Select the new tier, and then select **Select**.</span></span>
4. <span data-ttu-id="a6e65-194">På den **säkerhetsprincip** bladet väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="a6e65-194">On the **Security policy** blade, select **Save**.</span></span> 

<span data-ttu-id="a6e65-195">När du har ändrat prisnivån börjar säkerhet aviseringar diagrammet fylla som säkerhet hot upptäcks.</span><span class="sxs-lookup"><span data-stu-id="a6e65-195">After you've changed the pricing tier, the security alerts graph begins to populate as security threats are detected.</span></span>

![Säkerhetsaviseringar](./media/tutorial-azure-security/security-alerts.png)

<span data-ttu-id="a6e65-197">Välj en avisering för att visa information.</span><span class="sxs-lookup"><span data-stu-id="a6e65-197">Select an alert to view information.</span></span> <span data-ttu-id="a6e65-198">Du kan till exempel se en beskrivning av hotet, identifieringstid, alla hot försök och de rekommenderade åtgärderna.</span><span class="sxs-lookup"><span data-stu-id="a6e65-198">For example, you can see a description of the threat, the detection time, all threat attempts, and the recommended remediation.</span></span> <span data-ttu-id="a6e65-199">I följande exempel upptäcktes en RDP brute force-attacker med 294 RDP lösenordsförsök.</span><span class="sxs-lookup"><span data-stu-id="a6e65-199">In the following example, an RDP brute-force attack was detected, with 294 failed RDP attempts.</span></span> <span data-ttu-id="a6e65-200">En rekommenderad lösning på problemet.</span><span class="sxs-lookup"><span data-stu-id="a6e65-200">A recommended resolution is provided.</span></span>

![RDP-attack](./media/tutorial-azure-security/rdp-attack.png)

## <a name="next-steps"></a><span data-ttu-id="a6e65-202">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a6e65-202">Next steps</span></span>
<span data-ttu-id="a6e65-203">I den här självstudiekursen, Ställ in Azure Security Center och granskas virtuella datorer i Security Center.</span><span class="sxs-lookup"><span data-stu-id="a6e65-203">In this tutorial, you set up Azure Security Center, and then reviewed VMs in Security Center.</span></span> <span data-ttu-id="a6e65-204">Du har lärt dig hur till:</span><span class="sxs-lookup"><span data-stu-id="a6e65-204">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a6e65-205">Konfigurera datainsamling</span><span class="sxs-lookup"><span data-stu-id="a6e65-205">Set up data collection</span></span>
> * <span data-ttu-id="a6e65-206">Ställa in säkerhetsprinciper</span><span class="sxs-lookup"><span data-stu-id="a6e65-206">Set up security policies</span></span>
> * <span data-ttu-id="a6e65-207">Visa och lösa konfigurationsproblem hälsa</span><span class="sxs-lookup"><span data-stu-id="a6e65-207">View and fix configuration health issues</span></span>
> * <span data-ttu-id="a6e65-208">Granska identifierade hot</span><span class="sxs-lookup"><span data-stu-id="a6e65-208">Review detected threats</span></span>

<span data-ttu-id="a6e65-209">Gå vidare till nästa kurs att lära dig hur du skapar en CI/CD-pipeline med Visual Studio Team Services och en Windows virtuell dator som kör IIS.</span><span class="sxs-lookup"><span data-stu-id="a6e65-209">Advance to the next tutorial to learn how to create a CI/CD pipeline with Visual Studio Team Services and a Windows VM running IIS.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="a6e65-210">Visual Studio Team Services CI/CD-pipeline</span><span class="sxs-lookup"><span data-stu-id="a6e65-210">Visual Studio Team Services CI/CD pipeline</span></span>](./tutorial-vsts-iis-cicd.md)
