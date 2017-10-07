---
title: "aaaService kvoter och gränser för Azure Batch | Microsoft Docs"
description: "Lär dig mer om standard Azure Batch-kvoter gränser och begränsningar och hur toorequest kvot ökar"
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
ms.openlocfilehash: 6035d1c7618cfe97ebca3780e02a4ee34f54e534
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="batch-service-quotas-and-limits"></a><span data-ttu-id="09270-103">Kvoter och begränsningar för Batch-tjänsten</span><span class="sxs-lookup"><span data-stu-id="09270-103">Batch service quotas and limits</span></span>

<span data-ttu-id="09270-104">Som med andra Azure-tjänster, det finns begränsningar på vissa resurser som är associerade med hello Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="09270-104">As with other Azure services, there are limits on certain resources associated with hello Batch service.</span></span> <span data-ttu-id="09270-105">Många av dessa gränser är standard kvoterna som används av Azure på hello prenumeration eller konto.</span><span class="sxs-lookup"><span data-stu-id="09270-105">Many of these limits are default quotas applied by Azure at hello subscription or account level.</span></span> <span data-ttu-id="09270-106">Den här artikeln beskrivs dessa standardinställningar och hur du kan begära kvoten ökar.</span><span class="sxs-lookup"><span data-stu-id="09270-106">This article discusses those defaults, and how you can request quota increases.</span></span>

<span data-ttu-id="09270-107">Tänk på dessa kvoter när du utformar och skalar upp dina Batch-arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="09270-107">Keep these quotas in mind as you are designing and scaling up your Batch workloads.</span></span> <span data-ttu-id="09270-108">Till exempel om din pool inte når hello mål antalet compute-noder som du har angett, kanske du har nått hello core kvotgränsen för Batch-kontot eller en regionala Virtuella kärnor kvot för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="09270-108">For example, if your pool isn't reaching hello target number of compute nodes you've specified, you might have reached hello core quota limit for your Batch account, or a regional VM cores quota for your subscription.</span></span>

<span data-ttu-id="09270-109">Du kan köra flera Batch arbetsbelastningar i en enskild Batch-kontot eller distribuera dina arbetsbelastningar bland Batch-konton som finns i hello samma prenumeration, men i olika Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="09270-109">You can run multiple Batch workloads in a single Batch account, or distribute your workloads among Batch accounts that are in hello same subscription, but in different Azure regions.</span></span>

<span data-ttu-id="09270-110">Om du planerar toorun produktionsarbetsbelastningar i Batch behöva tooincrease en eller flera av hello kvoter ovan hello standard.</span><span class="sxs-lookup"><span data-stu-id="09270-110">If you plan toorun production workloads in Batch, you may need tooincrease one or more of hello quotas above hello default.</span></span> <span data-ttu-id="09270-111">Om du vill tooraise en kvot kan du öppna en online [kundsupport](#increase-a-quota) utan kostnad.</span><span class="sxs-lookup"><span data-stu-id="09270-111">If you want tooraise a quota, you can open an online [customer support request](#increase-a-quota) at no charge.</span></span>

> [!NOTE]
> <span data-ttu-id="09270-112">En kvot är en kreditgräns, inte en kapacitet garanti.</span><span class="sxs-lookup"><span data-stu-id="09270-112">A quota is a credit limit, not a capacity guarantee.</span></span> <span data-ttu-id="09270-113">Kontakta Azure-supporten om du har stora kapacitetsbehov.</span><span class="sxs-lookup"><span data-stu-id="09270-113">If you have large-scale capacity needs, please contact Azure support.</span></span>
> 
> 

## <a name="resource-quotas"></a><span data-ttu-id="09270-114">Resurskvoter</span><span class="sxs-lookup"><span data-stu-id="09270-114">Resource quotas</span></span>
[!INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="quotas-in-user-subscription-mode"></a><span data-ttu-id="09270-115">Kvoter i användarläge för prenumeration</span><span class="sxs-lookup"><span data-stu-id="09270-115">Quotas in user subscription mode</span></span>

<span data-ttu-id="09270-116">För Batch-kontot med poolen allokering inställd för**användarens prenumeration**, Batch-virtuella datorer och andra resurser, till exempel storage-konton, skapas direkt i din prenumeration när poolen har skapats.</span><span class="sxs-lookup"><span data-stu-id="09270-116">For a Batch account with pool allocation mode set too**user subscription**, Batch VMs and other resources, such as storage accounts, are created directly in your subscription when a pool is created.</span></span> <span data-ttu-id="09270-117">hello Azure Batch kärnor kvoten gäller inte tooan konto som har skapats i det här läget.</span><span class="sxs-lookup"><span data-stu-id="09270-117">hello Azure Batch cores quota does not apply tooan account created in this mode.</span></span> <span data-ttu-id="09270-118">I stället tillämpas hello kvoter i din prenumeration för regional beräkning kärnor och andra resurser.</span><span class="sxs-lookup"><span data-stu-id="09270-118">Instead, hello quotas in your subscription for regional compute cores and other resources are applied.</span></span> <span data-ttu-id="09270-119">Mer information om dessa kvoter i [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="09270-119">Learn more about these quotas in [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).</span></span>

<span data-ttu-id="09270-120">När du planerar resursanvändningen för ett konto som har skapats i användarläge för prenumerationen Obs hello följande Batch resurser (tillägg toocompute kärnor) krävs för varje 40 virtuella Linux-datorer eller 20 virtuella Windows-datorer:</span><span class="sxs-lookup"><span data-stu-id="09270-120">When planning resource usage for an account created in user subscription mode, note hello following Batch resources (in addition toocompute cores) are required for every 40 Linux VMs, or 20 Windows VMs:</span></span>

| <span data-ttu-id="09270-121">Resurs</span><span class="sxs-lookup"><span data-stu-id="09270-121">Resource</span></span> | <span data-ttu-id="09270-122">Kvot</span><span class="sxs-lookup"><span data-stu-id="09270-122">Quota</span></span> | <span data-ttu-id="09270-123">Leverantör</span><span class="sxs-lookup"><span data-stu-id="09270-123">Provider</span></span> |
| --- | ---| --- |
| <span data-ttu-id="09270-124">Ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="09270-124">One storage account</span></span> | <span data-ttu-id="09270-125">Lagringskonton</span><span class="sxs-lookup"><span data-stu-id="09270-125">Storage Accounts</span></span> | <span data-ttu-id="09270-126">Microsoft.Storage</span><span class="sxs-lookup"><span data-stu-id="09270-126">Microsoft.Storage</span></span> |
| <span data-ttu-id="09270-127">En offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="09270-127">One public IP address</span></span> | <span data-ttu-id="09270-128">Offentliga IP-adresser</span><span class="sxs-lookup"><span data-stu-id="09270-128">Public IP Addresses</span></span> | <span data-ttu-id="09270-129">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="09270-129">Microsoft.Network</span></span> | 
| <span data-ttu-id="09270-130">Ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="09270-130">One virtual network</span></span> | <span data-ttu-id="09270-131">Virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="09270-131">Virtual Networks</span></span> | <span data-ttu-id="09270-132">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="09270-132">Microsoft.Network</span></span> | 
| <span data-ttu-id="09270-133">En nätverkssäkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="09270-133">One network security group</span></span> | <span data-ttu-id="09270-134">Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="09270-134">Network Security Groups</span></span> | <span data-ttu-id="09270-135">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="09270-135">Microsoft.Network</span></span> | 
| <span data-ttu-id="09270-136">En virtuella datorns skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="09270-136">One virtual machine scale set</span></span> | <span data-ttu-id="09270-137">Skalningsuppsättningar för Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="09270-137">Virtual Machine Scale Sets</span></span> | <span data-ttu-id="09270-138">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="09270-138">Microsoft.Compute</span></span> | 
| <span data-ttu-id="09270-139">En belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="09270-139">One load balancer</span></span> | <span data-ttu-id="09270-140">Belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="09270-140">Load Balancers</span></span> | <span data-ttu-id="09270-141">Microsoft.Network</span><span class="sxs-lookup"><span data-stu-id="09270-141">Microsoft.Network</span></span> | 

<span data-ttu-id="09270-142">hello kärnor kvot på regional nivå eller per VM familj ska ange bl.a toohello VM-storlek krävs för Batch-pool eller pooler:</span><span class="sxs-lookup"><span data-stu-id="09270-142">hello cores quota at a regional level or per VM family should be set according toohello VM size required for your Batch pool or pools:</span></span>

| <span data-ttu-id="09270-143">Kvot</span><span class="sxs-lookup"><span data-stu-id="09270-143">Quota</span></span> | <span data-ttu-id="09270-144">Leverantör</span><span class="sxs-lookup"><span data-stu-id="09270-144">Provider</span></span> |
| --- | ---- |
| <span data-ttu-id="09270-145">Totalt antal regionala kärnor</span><span class="sxs-lookup"><span data-stu-id="09270-145">Total Regional Cores</span></span> | <span data-ttu-id="09270-146">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="09270-146">Microsoft.Compute</span></span> |
| <span data-ttu-id="09270-147">…</span><span class="sxs-lookup"><span data-stu-id="09270-147">…</span></span> <span data-ttu-id="09270-148">Family kärnor</span><span class="sxs-lookup"><span data-stu-id="09270-148">Family Cores</span></span> | <span data-ttu-id="09270-149">Microsoft.Compute</span><span class="sxs-lookup"><span data-stu-id="09270-149">Microsoft.Compute</span></span> |



## <a name="other-limits"></a><span data-ttu-id="09270-150">Övriga begränsningar</span><span class="sxs-lookup"><span data-stu-id="09270-150">Other limits</span></span>
| <span data-ttu-id="09270-151">**Resurs**</span><span class="sxs-lookup"><span data-stu-id="09270-151">**Resource**</span></span> | <span data-ttu-id="09270-152">**Övre gräns**</span><span class="sxs-lookup"><span data-stu-id="09270-152">**Maximum Limit**</span></span> |
| --- | --- |
| <span data-ttu-id="09270-153">[Samtidiga uppgifter](batch-parallel-node-tasks.md) per compute-nod</span><span class="sxs-lookup"><span data-stu-id="09270-153">[Concurrent tasks](batch-parallel-node-tasks.md) per compute node</span></span> |<span data-ttu-id="09270-154">4 x antal nod kärnor</span><span class="sxs-lookup"><span data-stu-id="09270-154">4 x number of node cores</span></span> |
| <span data-ttu-id="09270-155">[Program](batch-application-packages.md) per Batch-kontot</span><span class="sxs-lookup"><span data-stu-id="09270-155">[Applications](batch-application-packages.md) per Batch account</span></span> |<span data-ttu-id="09270-156">20</span><span class="sxs-lookup"><span data-stu-id="09270-156">20</span></span> |
| <span data-ttu-id="09270-157">Programpaket per program</span><span class="sxs-lookup"><span data-stu-id="09270-157">Application packages per application</span></span> |<span data-ttu-id="09270-158">40</span><span class="sxs-lookup"><span data-stu-id="09270-158">40</span></span> |
| <span data-ttu-id="09270-159">Storlek för paketet (alla)</span><span class="sxs-lookup"><span data-stu-id="09270-159">Application package size (each)</span></span> |<span data-ttu-id="09270-160">Uppskattat 195GB<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="09270-160">Approx. 195GB<sup>1</sup></span></span> |
| <span data-ttu-id="09270-161">Maximal startstorlek för aktiviteten</span><span class="sxs-lookup"><span data-stu-id="09270-161">Maximum start task size</span></span> | <span data-ttu-id="09270-162">32768 tecken<sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="09270-162">32768 characters<sup>2</sup></span></span> |

<span data-ttu-id="09270-163"><sup>1</sup> azure Storage-gränsen för högsta blob-blockstorlek</span><span class="sxs-lookup"><span data-stu-id="09270-163"><sup>1</sup> Azure Storage limit for maximum block blob size</span></span><br /><span data-ttu-id="09270-164">
<sup>2</sup> innehåller resursfiler och miljövariabler</span><span class="sxs-lookup"><span data-stu-id="09270-164">
<sup>2</sup> Includes resource files and environment variables</span></span>

## <a name="view-batch-quotas"></a><span data-ttu-id="09270-165">Visa Batch-kvoter</span><span class="sxs-lookup"><span data-stu-id="09270-165">View Batch quotas</span></span>
<span data-ttu-id="09270-166">Visa dina kvoter för Batch-kontot i hello [Azure-portalen][portal].</span><span class="sxs-lookup"><span data-stu-id="09270-166">View your Batch account quotas in hello [Azure portal][portal].</span></span>

1. <span data-ttu-id="09270-167">Välj **Batch-konton** i hello portal, välj sedan hello Batch-kontot du är intresserad av.</span><span class="sxs-lookup"><span data-stu-id="09270-167">Select **Batch accounts** in hello portal, then select hello Batch account you're interested in.</span></span>
2. <span data-ttu-id="09270-168">Välj **egenskaper** hello batchkonto menyn bladet.</span><span class="sxs-lookup"><span data-stu-id="09270-168">Select **Properties** on hello Batch account's menu blade.</span></span>
3. <span data-ttu-id="09270-169">hello egenskapsbladet visar hello **kvoter** toohello Batch-kontot som för närvarande används</span><span class="sxs-lookup"><span data-stu-id="09270-169">hello Properties blade displays hello **quotas** currently applied toohello Batch account</span></span>
   
    ![Kvoter för batch-konto][account_quotas]

<span data-ttu-id="09270-171">Visa hello relaterade prenumeration kvoter i hello Azure-portalen för en Batch-kontot som skapats i användarläge för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="09270-171">For a Batch account created in user subscription mode, view hello related subscription quotas in hello Azure Portal.</span></span>

1. <span data-ttu-id="09270-172">Välj **prenumerationer**, och välj hello-prenumeration som du använder för hello Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="09270-172">Select **Subscriptions**, and select hello subscription you are using for hello Batch account.</span></span>

2. <span data-ttu-id="09270-173">På hello **prenumeration** bladet väljer **användning + kvoter**.</span><span class="sxs-lookup"><span data-stu-id="09270-173">On hello **Subscription** blade, select **Usage + quotas**.</span></span>



## <a name="increase-a-quota"></a><span data-ttu-id="09270-174">Öka en kvot</span><span class="sxs-lookup"><span data-stu-id="09270-174">Increase a quota</span></span>
<span data-ttu-id="09270-175">Följ dessa steg toorequest en kvot öka för Batch-kontot eller med hjälp av hello [Azure-portalen][portal].</span><span class="sxs-lookup"><span data-stu-id="09270-175">Follow these steps toorequest a quota increase for your Batch account or your subscription using hello [Azure portal][portal].</span></span> <span data-ttu-id="09270-176">hello beror databaskvot på hello poolen allokering läge av Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="09270-176">hello type of quota increase depends on hello pool allocation mode of your Batch account.</span></span>

### <a name="increase-a-batch-cores-quota"></a><span data-ttu-id="09270-177">Öka kvoten för kärnor en Batch</span><span class="sxs-lookup"><span data-stu-id="09270-177">Increase a Batch cores quota</span></span> 

<span data-ttu-id="09270-178">Om Batch-kontot har skapats i **Batch-tjänsten** läge, Följ dessa steg toorequest databaskvot en Batch kärnor:</span><span class="sxs-lookup"><span data-stu-id="09270-178">If your Batch account was created in **Batch service** mode, follow these steps toorequest a Batch cores quota increase:</span></span>

1. <span data-ttu-id="09270-179">Välj hello **hjälp + support** panelen på instrumentpanelen i portalen eller hello frågetecken (**?**) i hello övre högra hörnet av hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="09270-179">Select hello **Help + support** tile on your portal dashboard, or hello question mark (**?**) in hello upper-right corner of hello portal.</span></span>
2. <span data-ttu-id="09270-180">Välj **ny supportbegäran** > **grunderna**.</span><span class="sxs-lookup"><span data-stu-id="09270-180">Select **New support request** > **Basics**.</span></span>
3. <span data-ttu-id="09270-181">På hello **grunderna** bladet:</span><span class="sxs-lookup"><span data-stu-id="09270-181">On hello **Basics** blade:</span></span>
   
    <span data-ttu-id="09270-182">a.</span><span class="sxs-lookup"><span data-stu-id="09270-182">a.</span></span> <span data-ttu-id="09270-183">**Utfärda typ** > **kvot**</span><span class="sxs-lookup"><span data-stu-id="09270-183">**Issue Type** > **Quota**</span></span>
   
    <span data-ttu-id="09270-184">b.</span><span class="sxs-lookup"><span data-stu-id="09270-184">b.</span></span> <span data-ttu-id="09270-185">Välj din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="09270-185">Select your subscription.</span></span>
   
    <span data-ttu-id="09270-186">c.</span><span class="sxs-lookup"><span data-stu-id="09270-186">c.</span></span> <span data-ttu-id="09270-187">**Typ av kvot** > **Batch**</span><span class="sxs-lookup"><span data-stu-id="09270-187">**Quota type** > **Batch**</span></span>
   
    <span data-ttu-id="09270-188">d.</span><span class="sxs-lookup"><span data-stu-id="09270-188">d.</span></span> <span data-ttu-id="09270-189">**Supportplan** > **kvot support - ingår**</span><span class="sxs-lookup"><span data-stu-id="09270-189">**Support plan** > **Quota support - Included**</span></span>
   
    <span data-ttu-id="09270-190">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="09270-190">Click **Next**.</span></span>
4. <span data-ttu-id="09270-191">På hello **problemet** bladet:</span><span class="sxs-lookup"><span data-stu-id="09270-191">On hello **Problem** blade:</span></span>
   
    <span data-ttu-id="09270-192">a.</span><span class="sxs-lookup"><span data-stu-id="09270-192">a.</span></span> <span data-ttu-id="09270-193">Välj en **allvarlighetsgrad** enligt tooyour [inverkan på verksamheten][support_sev].</span><span class="sxs-lookup"><span data-stu-id="09270-193">Select a **Severity** according tooyour [business impact][support_sev].</span></span>
   
    <span data-ttu-id="09270-194">b.</span><span class="sxs-lookup"><span data-stu-id="09270-194">b.</span></span> <span data-ttu-id="09270-195">I **information**, ange varje kvot som du vill toochange hello Batch-kontonamn och hello ny gräns.</span><span class="sxs-lookup"><span data-stu-id="09270-195">In **Details**, specify each quota you want toochange, hello Batch account name, and hello new limit.</span></span>
   
    <span data-ttu-id="09270-196">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="09270-196">Click **Next**.</span></span>
5. <span data-ttu-id="09270-197">På hello **kontaktinformation** bladet:</span><span class="sxs-lookup"><span data-stu-id="09270-197">On hello **Contact information** blade:</span></span>
   
    <span data-ttu-id="09270-198">a.</span><span class="sxs-lookup"><span data-stu-id="09270-198">a.</span></span> <span data-ttu-id="09270-199">Välj en **önskad kontaktmetod**.</span><span class="sxs-lookup"><span data-stu-id="09270-199">Select a **Preferred contact method**.</span></span>
   
    <span data-ttu-id="09270-200">b.</span><span class="sxs-lookup"><span data-stu-id="09270-200">b.</span></span> <span data-ttu-id="09270-201">Verifiera och ange hello krävs kontaktinformation.</span><span class="sxs-lookup"><span data-stu-id="09270-201">Verify and enter hello required contact details.</span></span>
   
    <span data-ttu-id="09270-202">Klicka på **skapa** toosubmit hello supportbegäran.</span><span class="sxs-lookup"><span data-stu-id="09270-202">Click **Create** toosubmit hello support request.</span></span>

<span data-ttu-id="09270-203">När du har skickat supportförfrågan Azure-supporten kommer att kontakta dig.</span><span class="sxs-lookup"><span data-stu-id="09270-203">Once you've submitted your support request, Azure support will contact you.</span></span> <span data-ttu-id="09270-204">Observera att slutföra hello begäran kan ta upp too2 arbetsdagar.</span><span class="sxs-lookup"><span data-stu-id="09270-204">Note that completing hello request can take up too2 business days.</span></span>

### <a name="increase-a-subscription-cores-quota"></a><span data-ttu-id="09270-205">Öka kvoten för kärnor en prenumeration</span><span class="sxs-lookup"><span data-stu-id="09270-205">Increase a subscription cores quota</span></span>

<span data-ttu-id="09270-206">Om Batch-kontot har skapats i **användarens prenumeration** läge och du behöver ytterligare regionala eller VM family kärnor, begäran en kvot ökar i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="09270-206">If your Batch account was created in **user subscription** mode and you need additional regional or VM family cores, request a quota increase in your subscription.</span></span> <span data-ttu-id="09270-207">Anvisningar finns [Resource Manager kärnkvot öka begäranden](../azure-supportability/resource-manager-core-quotas-request.md).</span><span class="sxs-lookup"><span data-stu-id="09270-207">For steps, see [Resource Manager core quota increase requests](../azure-supportability/resource-manager-core-quotas-request.md).</span></span>



## <a name="related-topics"></a><span data-ttu-id="09270-208">Relaterade ämnen</span><span class="sxs-lookup"><span data-stu-id="09270-208">Related topics</span></span>
* [<span data-ttu-id="09270-209">Skapa ett Azure Batch-konto med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="09270-209">Create an Azure Batch account using hello Azure portal</span></span>](batch-account-create-portal.md)
* [<span data-ttu-id="09270-210">Översikt av Azure Batch-funktion</span><span class="sxs-lookup"><span data-stu-id="09270-210">Azure Batch feature overview</span></span>](batch-api-basics.md)
* [<span data-ttu-id="09270-211">Azure-prenumeration och tjänsten gränser, kvoter och begränsningar</span><span class="sxs-lookup"><span data-stu-id="09270-211">Azure subscription and service limits, quotas, and constraints</span></span>](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
