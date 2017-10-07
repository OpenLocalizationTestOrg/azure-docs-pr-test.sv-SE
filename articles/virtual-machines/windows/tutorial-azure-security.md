---
title: aaaAzure Security Center och Windows-datorer i Azure | Microsoft Docs
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
ms.openlocfilehash: 238bf4e266a24a536d35dd647db6056ab39a1f1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-virtual-machine-security-by-using-azure-security-center"></a><span data-ttu-id="78e30-103">Övervaka virtuella säkerhet med hjälp av Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="78e30-103">Monitor virtual machine security by using Azure Security Center</span></span>

<span data-ttu-id="78e30-104">Azure Security Center kan hjälpa dig få insyn i Azure-resurs säkerhetsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="78e30-104">Azure Security Center can help you gain visibility into your Azure resource security practices.</span></span> <span data-ttu-id="78e30-105">Security Center ger övervakning av integrerad säkerhet.</span><span class="sxs-lookup"><span data-stu-id="78e30-105">Security Center offers integrated security monitoring.</span></span> <span data-ttu-id="78e30-106">Det kan upptäcka hot som annars kan förbli oupptäckta.</span><span class="sxs-lookup"><span data-stu-id="78e30-106">It can detect threats that otherwise might go unnoticed.</span></span> <span data-ttu-id="78e30-107">I kursen får du lära dig om Azure Security Center och hur du:</span><span class="sxs-lookup"><span data-stu-id="78e30-107">In this tutorial, you learn about Azure Security Center, and how to:</span></span>
 
> [!div class="checklist"]
> * <span data-ttu-id="78e30-108">Konfigurera datainsamling</span><span class="sxs-lookup"><span data-stu-id="78e30-108">Set up data collection</span></span>
> * <span data-ttu-id="78e30-109">Ställa in säkerhetsprinciper</span><span class="sxs-lookup"><span data-stu-id="78e30-109">Set up security policies</span></span>
> * <span data-ttu-id="78e30-110">Visa och lösa konfigurationsproblem hälsa</span><span class="sxs-lookup"><span data-stu-id="78e30-110">View and fix configuration health issues</span></span>
> * <span data-ttu-id="78e30-111">Granska identifierade hot</span><span class="sxs-lookup"><span data-stu-id="78e30-111">Review detected threats</span></span>  

## <a name="security-center-overview"></a><span data-ttu-id="78e30-112">Översikt över Security Center</span><span class="sxs-lookup"><span data-stu-id="78e30-112">Security Center overview</span></span>

<span data-ttu-id="78e30-113">Security Center identifierar potentiella konfigurationsproblem för virtuell dator (VM) och mål säkerhetshot.</span><span class="sxs-lookup"><span data-stu-id="78e30-113">Security Center identifies potential virtual machine (VM) configuration issues and targeted security threats.</span></span> <span data-ttu-id="78e30-114">Dessa kan innehålla virtuella datorer som saknar nätverkssäkerhetsgrupper och okrypterade diskar brute force-attacker för Remote Desktop Protocol (RDP).</span><span class="sxs-lookup"><span data-stu-id="78e30-114">These might include VMs that are missing network security groups, unencrypted disks, and brute-force Remote Desktop Protocol (RDP) attacks.</span></span> <span data-ttu-id="78e30-115">hello information visas på instrumentpanelen som hello Security Center enkelt att läsa diagram.</span><span class="sxs-lookup"><span data-stu-id="78e30-115">hello information is shown on hello Security Center dashboard in easy-to-read graphs.</span></span>

<span data-ttu-id="78e30-116">tooaccess hello instrumentpanelen i Security Center, i hello Azure-portalen på hello-menyn och väljer **Security Center**.</span><span class="sxs-lookup"><span data-stu-id="78e30-116">tooaccess hello Security Center dashboard, in hello Azure portal, on hello menu, select  **Security Center**.</span></span> <span data-ttu-id="78e30-117">På instrumentpanelen hello du finns hello säkerhetshälsa i Azure-miljön, hitta en uppräkning av aktuella rekommendationer och visa hello aktuell status för hot aviseringar.</span><span class="sxs-lookup"><span data-stu-id="78e30-117">On hello dashboard, you can see hello security health of your Azure environment, find a count of current recommendations, and view hello current state of threat alerts.</span></span> <span data-ttu-id="78e30-118">Du kan expandera varje övergripande diagram toosee detalj.</span><span class="sxs-lookup"><span data-stu-id="78e30-118">You can expand each high-level chart toosee more detail.</span></span>

![Instrumentpanelen Security Center](./media/tutorial-azure-security/asc-dash.png)

<span data-ttu-id="78e30-120">Security Center är mer omfattande än data identifiering tooprovide rekommendationer för problem som upptäcks.</span><span class="sxs-lookup"><span data-stu-id="78e30-120">Security Center goes beyond data discovery tooprovide recommendations for issues that it detects.</span></span> <span data-ttu-id="78e30-121">Om en virtuell dator har distribuerats utan ett anslutet nätverkssäkerhetsgrupp, visar Säkerhetscenter en rekommendation med steg du kan vidta.</span><span class="sxs-lookup"><span data-stu-id="78e30-121">For example, if a VM was deployed without an attached network security group, Security Center displays a recommendation, with remediation steps you can take.</span></span> <span data-ttu-id="78e30-122">Du kan hämta automatiska reparationer utan att lämna hello kontexten för Security Center.</span><span class="sxs-lookup"><span data-stu-id="78e30-122">You get automated remediation without leaving hello context of Security Center.</span></span>  

![Rekommendationer](./media/tutorial-azure-security/recommendations.png)

## <a name="set-up-data-collection"></a><span data-ttu-id="78e30-124">Konfigurera datainsamling</span><span class="sxs-lookup"><span data-stu-id="78e30-124">Set up data collection</span></span>

<span data-ttu-id="78e30-125">Innan du kan få en överblick över VM säkerhetskonfigurationer, måste tooset in insamling av Security Center.</span><span class="sxs-lookup"><span data-stu-id="78e30-125">Before you can get visibility into VM security configurations, you need tooset up Security Center data collection.</span></span> <span data-ttu-id="78e30-126">Detta innebär att aktivera insamling av data och skapa ett Azure storage-konto toohold insamlade data.</span><span class="sxs-lookup"><span data-stu-id="78e30-126">This involves turning on data collection and creating an Azure storage account toohold collected data.</span></span> 

1. <span data-ttu-id="78e30-127">Klicka på instrumentpanelen för hello Security Center **säkerhetsprincip**, och sedan välja din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="78e30-127">On hello Security Center dashboard, click **Security policy**, and then select your subscription.</span></span> 
2. <span data-ttu-id="78e30-128">För **datainsamling**väljer **på**.</span><span class="sxs-lookup"><span data-stu-id="78e30-128">For **Data collection**, select **On**.</span></span>
3. <span data-ttu-id="78e30-129">Välj toocreate ett lagringskonto **väljer något lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="78e30-129">toocreate a storage account, select **Choose a storage account**.</span></span> <span data-ttu-id="78e30-130">Markera **OK**.</span><span class="sxs-lookup"><span data-stu-id="78e30-130">Then, select **OK**.</span></span>
4. <span data-ttu-id="78e30-131">På hello **säkerhetsprincip** bladet väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="78e30-131">On hello **Security Policy** blade, select **Save**.</span></span> 

<span data-ttu-id="78e30-132">hello Säkerhetscenter data collection agent installeras på alla virtuella datorer och datainsamlingen påbörjas.</span><span class="sxs-lookup"><span data-stu-id="78e30-132">hello Security Center data collection agent is then installed on all VMs, and data collection begins.</span></span> 

## <a name="set-up-a-security-policy"></a><span data-ttu-id="78e30-133">Ställ in en säkerhetsprincip</span><span class="sxs-lookup"><span data-stu-id="78e30-133">Set up a security policy</span></span>

<span data-ttu-id="78e30-134">Säkerhetsprinciper finns används toodefine hello objekt som Security Center samlar in data och ger rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="78e30-134">Security policies are used toodefine hello items for which Security Center collects data and makes recommendations.</span></span> <span data-ttu-id="78e30-135">Du kan tillämpa olika principer toodifferent uppsättningar med Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="78e30-135">You can apply different security policies toodifferent sets of Azure resources.</span></span> <span data-ttu-id="78e30-136">Även om Azure-resurser som standard ska utvärderas mot alla principobjekt kan du inaktivera enskilda principobjekt för alla Azure-resurser eller för en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="78e30-136">Although by default Azure resources are evaluated against all policy items, you can turn off individual policy items for all Azure resources or for a resource group.</span></span> <span data-ttu-id="78e30-137">Detaljerad information om säkerhetsprinciper från security Center finns [ställa in säkerhetsprinciper i Azure Security Center](../../security-center/security-center-policies.md).</span><span class="sxs-lookup"><span data-stu-id="78e30-137">For in-depth information about Security Center security policies, see [Set security policies in Azure Security Center](../../security-center/security-center-policies.md).</span></span> 

<span data-ttu-id="78e30-138">tooset in en säkerhetsprincip för alla Azure-resurser:</span><span class="sxs-lookup"><span data-stu-id="78e30-138">tooset up a security policy for all Azure resources:</span></span>

1. <span data-ttu-id="78e30-139">Välj på instrumentpanelen för hello Security Center **säkerhetsprincip**, och sedan välja din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="78e30-139">On hello Security Center dashboard, select **Security policy**, and then select your subscription.</span></span>
2. <span data-ttu-id="78e30-140">Välj **skyddsprincip**.</span><span class="sxs-lookup"><span data-stu-id="78e30-140">Select **Prevention policy**.</span></span>
3. <span data-ttu-id="78e30-141">Aktivera eller inaktivera principobjekt som du vill tooapply tooall Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="78e30-141">Turn on or turn off policy items that you want tooapply tooall Azure resources.</span></span>
4. <span data-ttu-id="78e30-142">När du är klar med att välja inställningarna väljer **OK**.</span><span class="sxs-lookup"><span data-stu-id="78e30-142">When you're finished selecting your settings, select **OK**.</span></span>
5. <span data-ttu-id="78e30-143">På hello **säkerhetsprincip** bladet väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="78e30-143">On hello **Security policy** blade, select **Save**.</span></span> 

<span data-ttu-id="78e30-144">tooset en princip för en viss resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="78e30-144">tooset up a policy for a specific resource group:</span></span>

1. <span data-ttu-id="78e30-145">Välj på instrumentpanelen för hello Security Center **säkerhetsprincip**, och välj sedan en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="78e30-145">On hello Security Center dashboard, select **Security policy**, and then select a resource group.</span></span>
2. <span data-ttu-id="78e30-146">Välj **skyddsprincip**.</span><span class="sxs-lookup"><span data-stu-id="78e30-146">Select **Prevention policy**.</span></span>
3. <span data-ttu-id="78e30-147">Aktivera eller inaktivera principobjekt som du vill tooapply toohello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="78e30-147">Turn on or turn off policy items that you want tooapply toohello resource group.</span></span>
4. <span data-ttu-id="78e30-148">Under **arv**väljer **unik**.</span><span class="sxs-lookup"><span data-stu-id="78e30-148">Under **INHERITANCE**, select **Unique**.</span></span>
5. <span data-ttu-id="78e30-149">När du är klar med att välja inställningarna väljer **OK**.</span><span class="sxs-lookup"><span data-stu-id="78e30-149">When you're finished selecting your settings, select **OK**.</span></span>
6. <span data-ttu-id="78e30-150">På hello **säkerhetsprincip** bladet väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="78e30-150">On hello **Security policy** blade, select **Save**.</span></span>  

<span data-ttu-id="78e30-151">Du kan också stänga av insamling av data för en viss resursgrupp på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="78e30-151">You also can turn off data collection for a specific resource group on this page.</span></span>

<span data-ttu-id="78e30-152">I följande exempel hello, en unik princip har skapats för en resursgrupp med namnet *myResoureGroup*.</span><span class="sxs-lookup"><span data-stu-id="78e30-152">In hello following example, a unique policy has been created for a resource group named *myResoureGroup*.</span></span> <span data-ttu-id="78e30-153">Disk kryptering och web application brandväggen rekommendationer är inaktiverade i den här principen.</span><span class="sxs-lookup"><span data-stu-id="78e30-153">In this policy, disk encryption and web application firewall recommendations are turned off.</span></span>

![Unik princip](./media/tutorial-azure-security/unique-policy.png)

## <a name="view-vm-configuration-health"></a><span data-ttu-id="78e30-155">Visa konfigurationshälsa för VM</span><span class="sxs-lookup"><span data-stu-id="78e30-155">View VM configuration health</span></span>

<span data-ttu-id="78e30-156">När du har aktiverat datainsamling och ställa in en säkerhetsprincip, börjar Security Center tooprovide aviseringar och rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="78e30-156">After you've turned on data collection and set a security policy, Security Center begins tooprovide alerts and recommendations.</span></span> <span data-ttu-id="78e30-157">Eftersom virtuella datorer distribueras är hello data collection agenten installerad.</span><span class="sxs-lookup"><span data-stu-id="78e30-157">As VMs are deployed, hello data collection agent is installed.</span></span> <span data-ttu-id="78e30-158">Security Center består av data för hello nya virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="78e30-158">Security Center is then populated with data for hello new VMs.</span></span> <span data-ttu-id="78e30-159">Detaljerad information om hälsa för VM-konfiguration, se [skydda dina virtuella datorer i Security Center](../../security-center/security-center-virtual-machine-recommendations.md).</span><span class="sxs-lookup"><span data-stu-id="78e30-159">For in-depth information about VM configuration health, see [Protect your VMs in Security Center](../../security-center/security-center-virtual-machine-recommendations.md).</span></span> 

<span data-ttu-id="78e30-160">När data har samlats in sammanställs hello resurshälsa för varje virtuell dator och relaterade Azure-resurs.</span><span class="sxs-lookup"><span data-stu-id="78e30-160">As data is collected, hello resource health for each VM and related Azure resource is aggregated.</span></span> <span data-ttu-id="78e30-161">hello information visas i ett enkelt att läsa diagram.</span><span class="sxs-lookup"><span data-stu-id="78e30-161">hello information is shown in an easy-to-read chart.</span></span> 

<span data-ttu-id="78e30-162">tooview resource health:</span><span class="sxs-lookup"><span data-stu-id="78e30-162">tooview resource health:</span></span>

1.  <span data-ttu-id="78e30-163">På instrumentpanelen för hello Security Center under **resurssäkerhetshälsa**väljer **Compute**.</span><span class="sxs-lookup"><span data-stu-id="78e30-163">On hello Security Center dashboard, under **Resource security health**, select **Compute**.</span></span> 
2.  <span data-ttu-id="78e30-164">På hello **Compute** bladet väljer **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="78e30-164">On hello **Compute** blade, select **Virtual machines**.</span></span> <span data-ttu-id="78e30-165">Den här vyn visar en sammanfattning av hello Konfigurationsstatus för din virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="78e30-165">This view provides a summary of hello configuration status for all your VMs.</span></span>

![Beräkna hälsa](./media/tutorial-azure-security/compute-health.png)

<span data-ttu-id="78e30-167">toosee alla rekommendationer för en virtuell dator, Välj hello VM.</span><span class="sxs-lookup"><span data-stu-id="78e30-167">toosee all recommendations for a VM, select hello VM.</span></span> <span data-ttu-id="78e30-168">Rekommendationer och reparation beskrivs i detalj i hello nästa avsnitt i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="78e30-168">Recommendations and remediation are covered in more detail in hello next section of this tutorial.</span></span>

## <a name="remediate-configuration-issues"></a><span data-ttu-id="78e30-169">Åtgärda problem med konfiguration</span><span class="sxs-lookup"><span data-stu-id="78e30-169">Remediate configuration issues</span></span>

<span data-ttu-id="78e30-170">När Security Center startar toopopulate med konfigurationsdata görs rekommendationer baserat på hello säkerhetsprincip som du konfigurerar.</span><span class="sxs-lookup"><span data-stu-id="78e30-170">After Security Center begins toopopulate with configuration data, recommendations are made based on hello security policy you set up.</span></span> <span data-ttu-id="78e30-171">Till exempel om en virtuell dator har konfigurerats utan en nätverkssäkerhetsgrupp, görs en rekommendation toocreate en.</span><span class="sxs-lookup"><span data-stu-id="78e30-171">For instance, if a VM was set up without an associated network security group, a recommendation is made toocreate one.</span></span> 

<span data-ttu-id="78e30-172">toosee en lista över alla rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="78e30-172">toosee a list of all recommendations:</span></span> 

1. <span data-ttu-id="78e30-173">Välj på instrumentpanelen för hello Security Center **rekommendationer**.</span><span class="sxs-lookup"><span data-stu-id="78e30-173">On hello Security Center dashboard, select **Recommendations**.</span></span>
2. <span data-ttu-id="78e30-174">Välj en specifik rekommendation.</span><span class="sxs-lookup"><span data-stu-id="78e30-174">Select a specific recommendation.</span></span> <span data-ttu-id="78e30-175">En lista över alla resurser som gäller hello rekommendation visas.</span><span class="sxs-lookup"><span data-stu-id="78e30-175">A list of all resources for which hello recommendation applies appears.</span></span>
3. <span data-ttu-id="78e30-176">tooapply en rekommendation, välja en specifik resurs.</span><span class="sxs-lookup"><span data-stu-id="78e30-176">tooapply a recommendation, select a specific resource.</span></span> 
4. <span data-ttu-id="78e30-177">Följ instruktionerna för hello för steg.</span><span class="sxs-lookup"><span data-stu-id="78e30-177">Follow hello instructions for remediation steps.</span></span> 

<span data-ttu-id="78e30-178">I många fall Security Center innehåller tillämplig steg du kan vidta tooaddress en rekommendation utan att lämna Security Center.</span><span class="sxs-lookup"><span data-stu-id="78e30-178">In many cases, Security Center provides actionable steps you can take tooaddress a recommendation without leaving Security Center.</span></span> <span data-ttu-id="78e30-179">I följande exempel hello, upptäcks Security Center en nätverkssäkerhetsgrupp som har en obegränsad inkommande regel.</span><span class="sxs-lookup"><span data-stu-id="78e30-179">In hello following example, Security Center detects a network security group that has an unrestricted inbound rule.</span></span> <span data-ttu-id="78e30-180">Hello rekommendation på sidan kan du välja hello **redigera regler för inkommande trafik** knappen.</span><span class="sxs-lookup"><span data-stu-id="78e30-180">On hello recommendation page, you can select hello **Edit inbound rules** button.</span></span> <span data-ttu-id="78e30-181">hello-användargränssnitt som är nödvändiga toomodify hello regel visas.</span><span class="sxs-lookup"><span data-stu-id="78e30-181">hello UI that is needed toomodify hello rule appears.</span></span> 

![Rekommendationer](./media/tutorial-azure-security/remediation.png)

<span data-ttu-id="78e30-183">De är märkta som löst som rekommendationer har åtgärdats.</span><span class="sxs-lookup"><span data-stu-id="78e30-183">As recommendations are remediated, they are marked as resolved.</span></span> 

## <a name="view-detected-threats"></a><span data-ttu-id="78e30-184">Visa identifierade hot</span><span class="sxs-lookup"><span data-stu-id="78e30-184">View detected threats</span></span>

<span data-ttu-id="78e30-185">Dessutom visar tooresource rekommendationer, Security Center hotidentifieringsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="78e30-185">In addition tooresource configuration recommendations, Security Center displays threat detection alerts.</span></span> <span data-ttu-id="78e30-186">hello aviseringar säkerhetsfunktion sammanställer data som samlas in från varje VM, Azure nätverk loggar och anslutna partner solutions toodetect säkerhetshot mot Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="78e30-186">hello security alerts feature aggregates data collected from each VM, Azure networking logs, and connected partner solutions toodetect security threats against Azure resources.</span></span> <span data-ttu-id="78e30-187">För detaljerad information om funktionerna i Security Center threat detection Se [identifieringsfunktionerna i Azure Security Center](../../security-center/security-center-detection-capabilities.md).</span><span class="sxs-lookup"><span data-stu-id="78e30-187">For in-depth information about Security Center threat detection capabilities, see [Azure Security Center detection capabilities](../../security-center/security-center-detection-capabilities.md).</span></span>

<span data-ttu-id="78e30-188">hello säkerhetsfunktion aviseringar kräver hello Security Center priser nivå toobe ökade från *lediga* för*Standard*.</span><span class="sxs-lookup"><span data-stu-id="78e30-188">hello security alerts feature requires hello Security Center pricing tier toobe increased from *Free* too*Standard*.</span></span> <span data-ttu-id="78e30-189">En 30-dagars **kostnadsfri utvärderingsversion** är tillgänglig när du flyttar toothis högre prisnivå.</span><span class="sxs-lookup"><span data-stu-id="78e30-189">A 30-day **free trial** is available when you move toothis higher pricing tier.</span></span> 

<span data-ttu-id="78e30-190">toochange hello prisnivån:</span><span class="sxs-lookup"><span data-stu-id="78e30-190">toochange hello pricing tier:</span></span>  

1. <span data-ttu-id="78e30-191">Klicka på instrumentpanelen för hello Security Center **säkerhetsprincip**, och sedan välja din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="78e30-191">On hello Security Center dashboard, click **Security policy**, and then select your subscription.</span></span>
2. <span data-ttu-id="78e30-192">Välj **prisnivå**.</span><span class="sxs-lookup"><span data-stu-id="78e30-192">Select **Pricing tier**.</span></span>
3. <span data-ttu-id="78e30-193">Välj hello ny nivå och välj sedan **Välj**.</span><span class="sxs-lookup"><span data-stu-id="78e30-193">Select hello new tier, and then select **Select**.</span></span>
4. <span data-ttu-id="78e30-194">På hello **säkerhetsprincip** bladet väljer **spara**.</span><span class="sxs-lookup"><span data-stu-id="78e30-194">On hello **Security policy** blade, select **Save**.</span></span> 

<span data-ttu-id="78e30-195">När du har ändrat hello prisnivån börjar hello säkerhet aviseringar diagram toopopulate som säkerhetshot identifieras.</span><span class="sxs-lookup"><span data-stu-id="78e30-195">After you've changed hello pricing tier, hello security alerts graph begins toopopulate as security threats are detected.</span></span>

![Säkerhetsaviseringar](./media/tutorial-azure-security/security-alerts.png)

<span data-ttu-id="78e30-197">Välj en avisering tooview information.</span><span class="sxs-lookup"><span data-stu-id="78e30-197">Select an alert tooview information.</span></span> <span data-ttu-id="78e30-198">Du kan till exempel finns en beskrivning av hello hot, hello identifieringstiden alla hot försök och hello rekommenderade åtgärderna.</span><span class="sxs-lookup"><span data-stu-id="78e30-198">For example, you can see a description of hello threat, hello detection time, all threat attempts, and hello recommended remediation.</span></span> <span data-ttu-id="78e30-199">I följande exempel hello, upptäcktes en RDP brute force-attacker med 294 RDP lösenordsförsök.</span><span class="sxs-lookup"><span data-stu-id="78e30-199">In hello following example, an RDP brute-force attack was detected, with 294 failed RDP attempts.</span></span> <span data-ttu-id="78e30-200">En rekommenderad lösning på problemet.</span><span class="sxs-lookup"><span data-stu-id="78e30-200">A recommended resolution is provided.</span></span>

![RDP-attack](./media/tutorial-azure-security/rdp-attack.png)

## <a name="next-steps"></a><span data-ttu-id="78e30-202">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="78e30-202">Next steps</span></span>
<span data-ttu-id="78e30-203">I den här självstudiekursen, Ställ in Azure Security Center och granskas virtuella datorer i Security Center.</span><span class="sxs-lookup"><span data-stu-id="78e30-203">In this tutorial, you set up Azure Security Center, and then reviewed VMs in Security Center.</span></span> <span data-ttu-id="78e30-204">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="78e30-204">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="78e30-205">Konfigurera datainsamling</span><span class="sxs-lookup"><span data-stu-id="78e30-205">Set up data collection</span></span>
> * <span data-ttu-id="78e30-206">Ställa in säkerhetsprinciper</span><span class="sxs-lookup"><span data-stu-id="78e30-206">Set up security policies</span></span>
> * <span data-ttu-id="78e30-207">Visa och lösa konfigurationsproblem hälsa</span><span class="sxs-lookup"><span data-stu-id="78e30-207">View and fix configuration health issues</span></span>
> * <span data-ttu-id="78e30-208">Granska identifierade hot</span><span class="sxs-lookup"><span data-stu-id="78e30-208">Review detected threats</span></span>

<span data-ttu-id="78e30-209">Avancera toohello nästa självstudiekurs toolearn hur toocreate CI/CD-pipeline med Visual Studio Team Services och en Windows virtuell dator som kör IIS.</span><span class="sxs-lookup"><span data-stu-id="78e30-209">Advance toohello next tutorial toolearn how toocreate a CI/CD pipeline with Visual Studio Team Services and a Windows VM running IIS.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="78e30-210">Visual Studio Team Services CI/CD-pipeline</span><span class="sxs-lookup"><span data-stu-id="78e30-210">Visual Studio Team Services CI/CD pipeline</span></span>](./tutorial-vsts-iis-cicd.md)
