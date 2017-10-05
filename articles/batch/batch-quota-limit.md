---
title: "Tjänsten kvoter och gränser för Azure Batch | Microsoft Docs"
description: "Lär dig mer om standard Azure Batch-kvoter, gränser och begränsningar och hur du begär kvoten ökar"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f3f69ed8d3a985afe07e648e7512a88b25278ced
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="batch-service-quotas-and-limits"></a><span data-ttu-id="1c33c-103">Kvoter och begränsningar för Batch-tjänsten</span><span class="sxs-lookup"><span data-stu-id="1c33c-103">Batch service quotas and limits</span></span>

<span data-ttu-id="1c33c-104">Som med andra Azure-tjänster, det finns begränsningar på vissa resurser som är associerade med Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1c33c-104">As with other Azure services, there are limits on certain resources associated with the Batch service.</span></span> <span data-ttu-id="1c33c-105">Många av dessa gränser är standard kvoterna som används av Azure-prenumeration eller kontonivå.</span><span class="sxs-lookup"><span data-stu-id="1c33c-105">Many of these limits are default quotas applied by Azure at the subscription or account level.</span></span> <span data-ttu-id="1c33c-106">Den här artikeln beskrivs dessa standardinställningar och hur du kan begära kvoten ökar.</span><span class="sxs-lookup"><span data-stu-id="1c33c-106">This article discusses those defaults, and how you can request quota increases.</span></span>

<span data-ttu-id="1c33c-107">Tänk på dessa kvoter när du utformar och skalar upp dina Batch-arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="1c33c-107">Keep these quotas in mind as you are designing and scaling up your Batch workloads.</span></span> <span data-ttu-id="1c33c-108">Till exempel om din pool inte når målet antalet compute-noder som du har angett, kanske du har nått core kvotgränsen för Batch-kontot eller en regionala Virtuella kärnor kvot för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1c33c-108">For example, if your pool isn't reaching the target number of compute nodes you've specified, you might have reached the core quota limit for your Batch account, or a regional VM cores quota for your subscription.</span></span>

<span data-ttu-id="1c33c-109">Du kan köra flera Batch-arbetsbelastningar i samma Batch-konto eller distribuera dina arbetsbelastningar mellan Batch-konton som är i samma prenumeration, men i olika Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="1c33c-109">You can run multiple Batch workloads in a single Batch account, or distribute your workloads among Batch accounts that are in the same subscription, but in different Azure regions.</span></span>

<span data-ttu-id="1c33c-110">Om du planerar att köra produktionsarbetsbelastningar i Batch kan du behöva öka en eller flera av kvoter ovan standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="1c33c-110">If you plan to run production workloads in Batch, you may need to increase one or more of the quotas above the default.</span></span> <span data-ttu-id="1c33c-111">Om du vill generera en kvot kan du öppna en online [kundsupport](#increase-a-quota) utan kostnad.</span><span class="sxs-lookup"><span data-stu-id="1c33c-111">If you want to raise a quota, you can open an online [customer support request](#increase-a-quota) at no charge.</span></span>

> [!NOTE]
> <span data-ttu-id="1c33c-112">En kvot är en kreditgräns, inte en kapacitet garanti.</span><span class="sxs-lookup"><span data-stu-id="1c33c-112">A quota is a credit limit, not a capacity guarantee.</span></span> <span data-ttu-id="1c33c-113">Kontakta Azure-supporten om du har stora kapacitetsbehov.</span><span class="sxs-lookup"><span data-stu-id="1c33c-113">If you have large-scale capacity needs, please contact Azure support.</span></span>
> 
> 

## <a name="resource-quotas"></a><span data-ttu-id="1c33c-114">Resurskvoter</span><span class="sxs-lookup"><span data-stu-id="1c33c-114">Resource quotas</span></span>
[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="quotas-in-user-subscription-mode"></a><span data-ttu-id="1c33c-115">Kvoter i användarläge för prenumeration</span><span class="sxs-lookup"><span data-stu-id="1c33c-115">Quotas in user subscription mode</span></span>

<span data-ttu-id="1c33c-116">För Batch-kontot med poolen allokering inställd på **användarens prenumeration**, Batch-virtuella datorer och andra resurser, till exempel storage-konton, skapas direkt i din prenumeration när poolen har skapats.</span><span class="sxs-lookup"><span data-stu-id="1c33c-116">For a Batch account with pool allocation mode set to **user subscription**, Batch VMs and other resources, such as storage accounts, are created directly in your subscription when a pool is created.</span></span> <span data-ttu-id="1c33c-117">Azure Batch kärnor kvoten gäller inte för ett konto som har skapats i det här läget.</span><span class="sxs-lookup"><span data-stu-id="1c33c-117">The Azure Batch cores quota does not apply to an account created in this mode.</span></span> <span data-ttu-id="1c33c-118">I stället kvoter i din prenumeration för regional compute kärnor och andra resurser används.</span><span class="sxs-lookup"><span data-stu-id="1c33c-118">Instead, the quotas in your subscription for regional compute cores and other resources are applied.</span></span> <span data-ttu-id="1c33c-119">Mer information om dessa kvoter i [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="1c33c-119">Learn more about these quotas in [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).</span></span>

<span data-ttu-id="1c33c-120">När du planerar resursanvändningen för ett konto som har skapats i användarläge för prenumerationen kan du Observera följande Batch-resurser (förutom beräkning kärnor) krävs för varje 40 virtuella Linux-datorer eller 20 virtuella Windows-datorer:</span><span class="sxs-lookup"><span data-stu-id="1c33c-120">When planning resource usage for an account created in user subscription mode, note the following Batch resources (in addition to compute cores) are required for every 40 Linux VMs, or 20 Windows VMs:</span></span>

| <span data-ttu-id="1c33c-121">Resurs</span><span class="sxs-lookup"><span data-stu-id="1c33c-121">Resource</span></span> | <span data-ttu-id="1c33c-122">Kvot</span><span class="sxs-lookup"><span data-stu-id="1c33c-122">Quota</span></span> | <span data-ttu-id="1c33c-123">Leverantör</span><span class="sxs-lookup"><span data-stu-id="1c33c-123">Provider</span></span> |
| --- | ---| --- |
| <span data-ttu-id="1c33c-124">Ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="1c33c-124">One storage account</span></span> | <span data-ttu-id="1c33c-125">Lagringskonton</span><span class="sxs-lookup"><span data-stu-id="1c33c-125">Storage Accounts</span></span> | <span data-ttu-id="1c33c-126">Microsoft.Storage</span><span class="sxs-lookup"><span data-stu-id="1c33c-126">Microsoft.Storage</span></span> |
| <span data-ttu-id="1c33c-127">En offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="1c33c-127">One public IP address</span></span> | <span data-ttu-id="1c33c-128">Offentliga IP-adresser</span><span class="sxs-lookup"><span data-stu-id="1c33c-128">Public IP Addresses</span></span> | <span data-ttu-id="1c33c-129">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="1c33c-129">Microsoft.Network</span></span> | 
| <span data-ttu-id="1c33c-130">Ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="1c33c-130">One virtual network</span></span> | <span data-ttu-id="1c33c-131">Virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="1c33c-131">Virtual Networks</span></span> | <span data-ttu-id="1c33c-132">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="1c33c-132">Microsoft.Network</span></span> | 
| <span data-ttu-id="1c33c-133">En nätverkssäkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="1c33c-133">One network security group</span></span> | <span data-ttu-id="1c33c-134">Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="1c33c-134">Network Security Groups</span></span> | <span data-ttu-id="1c33c-135">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="1c33c-135">Microsoft.Network</span></span> | 
| <span data-ttu-id="1c33c-136">En virtuella datorns skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="1c33c-136">One virtual machine scale set</span></span> | <span data-ttu-id="1c33c-137">Skalningsuppsättningar för Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="1c33c-137">Virtual Machine Scale Sets</span></span> | <span data-ttu-id="1c33c-138">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="1c33c-138">Microsoft.Compute</span></span> | 
| <span data-ttu-id="1c33c-139">En belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="1c33c-139">One load balancer</span></span> | <span data-ttu-id="1c33c-140">Belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="1c33c-140">Load Balancers</span></span> | <span data-ttu-id="1c33c-141">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="1c33c-141">Microsoft.Network</span></span> | 

<span data-ttu-id="1c33c-142">Kärnor kvoten på regional nivå eller per VM familj ska anges enligt VM-storlek som krävs för din Batch-pool eller pooler:</span><span class="sxs-lookup"><span data-stu-id="1c33c-142">The cores quota at a regional level or per VM family should be set according to the VM size required for your Batch pool or pools:</span></span>

| <span data-ttu-id="1c33c-143">Kvot</span><span class="sxs-lookup"><span data-stu-id="1c33c-143">Quota</span></span> | <span data-ttu-id="1c33c-144">Leverantör</span><span class="sxs-lookup"><span data-stu-id="1c33c-144">Provider</span></span> |
| --- | ---- |
| <span data-ttu-id="1c33c-145">Totalt antal regionala kärnor</span><span class="sxs-lookup"><span data-stu-id="1c33c-145">Total Regional Cores</span></span> | <span data-ttu-id="1c33c-146">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="1c33c-146">Microsoft.Compute</span></span> |
| <span data-ttu-id="1c33c-147">…</span><span class="sxs-lookup"><span data-stu-id="1c33c-147">…</span></span> <span data-ttu-id="1c33c-148">Family kärnor</span><span class="sxs-lookup"><span data-stu-id="1c33c-148">Family Cores</span></span> | <span data-ttu-id="1c33c-149">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="1c33c-149">Microsoft.Compute</span></span> |



## <a name="other-limits"></a><span data-ttu-id="1c33c-150">Övriga begränsningar</span><span class="sxs-lookup"><span data-stu-id="1c33c-150">Other limits</span></span>
| <span data-ttu-id="1c33c-151">**Resurs**</span><span class="sxs-lookup"><span data-stu-id="1c33c-151">**Resource**</span></span> | <span data-ttu-id="1c33c-152">**Övre gräns**</span><span class="sxs-lookup"><span data-stu-id="1c33c-152">**Maximum Limit**</span></span> |
| --- | --- |
| <span data-ttu-id="1c33c-153">[Samtidiga uppgifter](batch-parallel-node-tasks.md) per compute-nod</span><span class="sxs-lookup"><span data-stu-id="1c33c-153">[Concurrent tasks](batch-parallel-node-tasks.md) per compute node</span></span> |<span data-ttu-id="1c33c-154">4 x antal nod kärnor</span><span class="sxs-lookup"><span data-stu-id="1c33c-154">4 x number of node cores</span></span> |
| <span data-ttu-id="1c33c-155">[Program](batch-application-packages.md) per Batch-kontot</span><span class="sxs-lookup"><span data-stu-id="1c33c-155">[Applications](batch-application-packages.md) per Batch account</span></span> |<span data-ttu-id="1c33c-156">20</span><span class="sxs-lookup"><span data-stu-id="1c33c-156">20</span></span> |
| <span data-ttu-id="1c33c-157">Programpaket per program</span><span class="sxs-lookup"><span data-stu-id="1c33c-157">Application packages per application</span></span> |<span data-ttu-id="1c33c-158">40</span><span class="sxs-lookup"><span data-stu-id="1c33c-158">40</span></span> |
| <span data-ttu-id="1c33c-159">Storlek för paketet (alla)</span><span class="sxs-lookup"><span data-stu-id="1c33c-159">Application package size (each)</span></span> |<span data-ttu-id="1c33c-160">Uppskattat 195GB<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="1c33c-160">Approx. 195GB<sup>1</sup></span></span> |
| <span data-ttu-id="1c33c-161">Maximal startstorlek för aktiviteten</span><span class="sxs-lookup"><span data-stu-id="1c33c-161">Maximum start task size</span></span> | <span data-ttu-id="1c33c-162">32768 tecken<sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="1c33c-162">32768 characters<sup>2</sup></span></span> |

<span data-ttu-id="1c33c-163"><sup>1</sup> azure Storage-gränsen för högsta blob-blockstorlek</span><span class="sxs-lookup"><span data-stu-id="1c33c-163"><sup>1</sup> Azure Storage limit for maximum block blob size</span></span><br /><span data-ttu-id="1c33c-164">
<sup>2</sup> innehåller resursfiler och miljövariabler</span><span class="sxs-lookup"><span data-stu-id="1c33c-164">
<sup>2</sup> Includes resource files and environment variables</span></span>

## <a name="view-batch-quotas"></a><span data-ttu-id="1c33c-165">Visa Batch-kvoter</span><span class="sxs-lookup"><span data-stu-id="1c33c-165">View Batch quotas</span></span>
<span data-ttu-id="1c33c-166">Visa kvoterna Batch-konto i den [Azure-portalen][portal].</span><span class="sxs-lookup"><span data-stu-id="1c33c-166">View your Batch account quotas in the [Azure portal][portal].</span></span>

1. <span data-ttu-id="1c33c-167">Välj **Batch-konton** i portalen, välj sedan Batch-kontot som du är intresserad av.</span><span class="sxs-lookup"><span data-stu-id="1c33c-167">Select **Batch accounts** in the portal, then select the Batch account you're interested in.</span></span>
2. <span data-ttu-id="1c33c-168">Välj **egenskaper** på menyn bladet för Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="1c33c-168">Select **Properties** on the Batch account's menu blade.</span></span>
3. <span data-ttu-id="1c33c-169">Egenskapsbladet visar den **kvoter** för närvarande används för Batch-kontot</span><span class="sxs-lookup"><span data-stu-id="1c33c-169">The Properties blade displays the **quotas** currently applied to the Batch account</span></span>
   
    ![Kvoter för batch-konto][account_quotas]

<span data-ttu-id="1c33c-171">Visa kvoter för relaterade prenumeration i Azure Portal för ett Batch-konto som skapats i användarläge för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="1c33c-171">For a Batch account created in user subscription mode, view the related subscription quotas in the Azure Portal.</span></span>

1. <span data-ttu-id="1c33c-172">Välj **prenumerationer**, och välj den prenumeration som du använder för Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="1c33c-172">Select **Subscriptions**, and select the subscription you are using for the Batch account.</span></span>

2. <span data-ttu-id="1c33c-173">På den **prenumeration** bladet väljer **användning + kvoter**.</span><span class="sxs-lookup"><span data-stu-id="1c33c-173">On the **Subscription** blade, select **Usage + quotas**.</span></span>



## <a name="increase-a-quota"></a><span data-ttu-id="1c33c-174">Öka en kvot</span><span class="sxs-lookup"><span data-stu-id="1c33c-174">Increase a quota</span></span>
<span data-ttu-id="1c33c-175">Följ dessa steg för att begära en kvot öka för Batch-kontot eller din prenumeration med hjälp av den [Azure-portalen][portal].</span><span class="sxs-lookup"><span data-stu-id="1c33c-175">Follow these steps to request a quota increase for your Batch account or your subscription using the [Azure portal][portal].</span></span> <span data-ttu-id="1c33c-176">Typ av databaskvot beror på poolen allokering läget för Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="1c33c-176">The type of quota increase depends on the pool allocation mode of your Batch account.</span></span>

### <a name="increase-a-batch-cores-quota"></a><span data-ttu-id="1c33c-177">Öka kvoten för kärnor en Batch</span><span class="sxs-lookup"><span data-stu-id="1c33c-177">Increase a Batch cores quota</span></span> 

<span data-ttu-id="1c33c-178">Om Batch-kontot har skapats i **Batch-tjänsten** läge, Följ dessa steg för att begära en Batch kärnor kvot öka:</span><span class="sxs-lookup"><span data-stu-id="1c33c-178">If your Batch account was created in **Batch service** mode, follow these steps to request a Batch cores quota increase:</span></span>

1. <span data-ttu-id="1c33c-179">Välj den **hjälp + support** panelen på instrumentpanelen i portalen eller frågetecken (**?**) i övre högra hörnet av portalen.</span><span class="sxs-lookup"><span data-stu-id="1c33c-179">Select the **Help + support** tile on your portal dashboard, or the question mark (**?**) in the upper-right corner of the portal.</span></span>
2. <span data-ttu-id="1c33c-180">Välj **ny supportbegäran** > **grunderna**.</span><span class="sxs-lookup"><span data-stu-id="1c33c-180">Select **New support request** > **Basics**.</span></span>
3. <span data-ttu-id="1c33c-181">På den **grunderna** bladet:</span><span class="sxs-lookup"><span data-stu-id="1c33c-181">On the **Basics** blade:</span></span>
   
    <span data-ttu-id="1c33c-182">a.</span><span class="sxs-lookup"><span data-stu-id="1c33c-182">a.</span></span> <span data-ttu-id="1c33c-183">**Utfärda typ** > **kvot**</span><span class="sxs-lookup"><span data-stu-id="1c33c-183">**Issue Type** > **Quota**</span></span>
   
    <span data-ttu-id="1c33c-184">b.</span><span class="sxs-lookup"><span data-stu-id="1c33c-184">b.</span></span> <span data-ttu-id="1c33c-185">Välj din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1c33c-185">Select your subscription.</span></span>
   
    <span data-ttu-id="1c33c-186">c.</span><span class="sxs-lookup"><span data-stu-id="1c33c-186">c.</span></span> <span data-ttu-id="1c33c-187">**Typ av kvot** > **Batch**</span><span class="sxs-lookup"><span data-stu-id="1c33c-187">**Quota type** > **Batch**</span></span>
   
    <span data-ttu-id="1c33c-188">d.</span><span class="sxs-lookup"><span data-stu-id="1c33c-188">d.</span></span> <span data-ttu-id="1c33c-189">**Supportplan** > **kvot support - ingår**</span><span class="sxs-lookup"><span data-stu-id="1c33c-189">**Support plan** > **Quota support - Included**</span></span>
   
    <span data-ttu-id="1c33c-190">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="1c33c-190">Click **Next**.</span></span>
4. <span data-ttu-id="1c33c-191">På den **problemet** bladet:</span><span class="sxs-lookup"><span data-stu-id="1c33c-191">On the **Problem** blade:</span></span>
   
    <span data-ttu-id="1c33c-192">a.</span><span class="sxs-lookup"><span data-stu-id="1c33c-192">a.</span></span> <span data-ttu-id="1c33c-193">Välj en **allvarlighetsgrad** enligt din [inverkan på verksamheten][support_sev].</span><span class="sxs-lookup"><span data-stu-id="1c33c-193">Select a **Severity** according to your [business impact][support_sev].</span></span>
   
    <span data-ttu-id="1c33c-194">b.</span><span class="sxs-lookup"><span data-stu-id="1c33c-194">b.</span></span> <span data-ttu-id="1c33c-195">I **information**, ange varje kvot som du vill ändra, Batch-kontonamnet och den nya gränsen.</span><span class="sxs-lookup"><span data-stu-id="1c33c-195">In **Details**, specify each quota you want to change, the Batch account name, and the new limit.</span></span>
   
    <span data-ttu-id="1c33c-196">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="1c33c-196">Click **Next**.</span></span>
5. <span data-ttu-id="1c33c-197">På den **kontaktinformation** bladet:</span><span class="sxs-lookup"><span data-stu-id="1c33c-197">On the **Contact information** blade:</span></span>
   
    <span data-ttu-id="1c33c-198">a.</span><span class="sxs-lookup"><span data-stu-id="1c33c-198">a.</span></span> <span data-ttu-id="1c33c-199">Välj en **önskad kontaktmetod**.</span><span class="sxs-lookup"><span data-stu-id="1c33c-199">Select a **Preferred contact method**.</span></span>
   
    <span data-ttu-id="1c33c-200">b.</span><span class="sxs-lookup"><span data-stu-id="1c33c-200">b.</span></span> <span data-ttu-id="1c33c-201">Verifiera och ange nödvändiga kontaktinformation.</span><span class="sxs-lookup"><span data-stu-id="1c33c-201">Verify and enter the required contact details.</span></span>
   
    <span data-ttu-id="1c33c-202">Klicka på **skapa** för att skicka in supportbegäran.</span><span class="sxs-lookup"><span data-stu-id="1c33c-202">Click **Create** to submit the support request.</span></span>

<span data-ttu-id="1c33c-203">När du har skickat supportförfrågan Azure-supporten kommer att kontakta dig.</span><span class="sxs-lookup"><span data-stu-id="1c33c-203">Once you've submitted your support request, Azure support will contact you.</span></span> <span data-ttu-id="1c33c-204">Observera att slutföra begäran kan ta upp till 2 arbetsdagar.</span><span class="sxs-lookup"><span data-stu-id="1c33c-204">Note that completing the request can take up to 2 business days.</span></span>

### <a name="increase-a-subscription-cores-quota"></a><span data-ttu-id="1c33c-205">Öka kvoten för kärnor en prenumeration</span><span class="sxs-lookup"><span data-stu-id="1c33c-205">Increase a subscription cores quota</span></span>

<span data-ttu-id="1c33c-206">Om Batch-kontot har skapats i **användarens prenumeration** läge och du behöver ytterligare regionala eller VM family kärnor, begäran en kvot ökar i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1c33c-206">If your Batch account was created in **user subscription** mode and you need additional regional or VM family cores, request a quota increase in your subscription.</span></span> <span data-ttu-id="1c33c-207">Anvisningar finns [Resource Manager kärnkvot öka begäranden](../azure-supportability/resource-manager-core-quotas-request.md).</span><span class="sxs-lookup"><span data-stu-id="1c33c-207">For steps, see [Resource Manager core quota increase requests](../azure-supportability/resource-manager-core-quotas-request.md).</span></span>



## <a name="related-topics"></a><span data-ttu-id="1c33c-208">Relaterade ämnen</span><span class="sxs-lookup"><span data-stu-id="1c33c-208">Related topics</span></span>
* [<span data-ttu-id="1c33c-209">Skapa ett Azure Batch-konto med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1c33c-209">Create an Azure Batch account using the Azure portal</span></span>](batch-account-create-portal.md)
* [<span data-ttu-id="1c33c-210">Översikt av Azure Batch-funktion</span><span class="sxs-lookup"><span data-stu-id="1c33c-210">Azure Batch feature overview</span></span>](batch-api-basics.md)
* [<span data-ttu-id="1c33c-211">Azure-prenumeration och tjänsten gränser, kvoter och begränsningar</span><span class="sxs-lookup"><span data-stu-id="1c33c-211">Azure subscription and service limits, quotas, and constraints</span></span>](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
