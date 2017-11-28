---
title: "aaaCommon moln Tjänstehantering | Microsoft Docs"
description: "Lär dig hur toomanage cloud services i hello Azure-portalen. Dessa exempel används hello Azure-portalen."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: cb218ad9-77d4-4149-83db-71159c00767e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: ade8a18a7754edbaae4903230c26c009fef63ed7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-cloud-services"></a><span data-ttu-id="f97ab-104">Hur tooManage Cloud Services</span><span class="sxs-lookup"><span data-stu-id="f97ab-104">How tooManage Cloud Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f97ab-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f97ab-105">Azure portal</span></span>](cloud-services-how-to-manage-portal.md)
> * [<span data-ttu-id="f97ab-106">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="f97ab-106">Azure classic portal</span></span>](cloud-services-how-to-manage.md)
>
>

<span data-ttu-id="f97ab-107">I hello **molntjänster (klassisk)** område i hello Azure portal, du kan uppdatera en rolltjänst eller en distribution, befordra en stegvis distribution tooproduction, länka resurser tooyour molntjänst så att du kan se hello resurs beroenden och skala hello resurser tillsammans, och ta bort en molnbaserad tjänst eller en distribution.</span><span class="sxs-lookup"><span data-stu-id="f97ab-107">In hello **Cloud Services (classic)** area of hello Azure portal, you can update a service role or a deployment, promote a staged deployment tooproduction, link resources tooyour cloud service so that you can see hello resource dependencies and scale hello resources together, and delete a cloud service or a deployment.</span></span>

<span data-ttu-id="f97ab-108">Mer information om hur tooscale Molntjänsten är tillgänglig [här](cloud-services-how-to-scale-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f97ab-108">More information about how tooscale your cloud service is available [here](cloud-services-how-to-scale-portal.md).</span></span>

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a><span data-ttu-id="f97ab-109">Så här: uppdatera en rolltjänst för molnet eller distribution</span><span class="sxs-lookup"><span data-stu-id="f97ab-109">How to: Update a cloud service role or deployment</span></span>
<span data-ttu-id="f97ab-110">Om du behöver tooupdate hello programkod för din molntjänst använder **uppdatering** på hello cloud service-bladet.</span><span class="sxs-lookup"><span data-stu-id="f97ab-110">If you need tooupdate hello application code for your cloud service, use **Update** on hello cloud service blade.</span></span> <span data-ttu-id="f97ab-111">Du kan uppdatera en roll eller alla roller.</span><span class="sxs-lookup"><span data-stu-id="f97ab-111">You can update a single role or all roles.</span></span> <span data-ttu-id="f97ab-112">tooupdate, kan du överföra ett nytt tjänstpaket eller tjänstkonfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="f97ab-112">tooupdate, you can upload a new service package or service configuration file.</span></span>

1. <span data-ttu-id="f97ab-113">I hello [Azure-portalen][Azure portal], Välj hello molntjänst som du vill tooupdate.</span><span class="sxs-lookup"><span data-stu-id="f97ab-113">In hello [Azure portal][Azure portal], select hello cloud service you want tooupdate.</span></span> <span data-ttu-id="f97ab-114">Det här steget öppnas hello cloud service-instans bladet.</span><span class="sxs-lookup"><span data-stu-id="f97ab-114">This step opens hello cloud service instance blade.</span></span>
2. <span data-ttu-id="f97ab-115">Hello-bladet klickar du på hello **uppdatering** knappen.</span><span class="sxs-lookup"><span data-stu-id="f97ab-115">In hello blade, click hello **Update** button.</span></span>

    ![Uppdateringsknappen](./media/cloud-services-how-to-manage-portal/update-button.png)

3. <span data-ttu-id="f97ab-117">Uppdatera hello distribution med en ny servicepaketfil (.cspkg) och tjänstekonfigurationsfilen (.cscfg).</span><span class="sxs-lookup"><span data-stu-id="f97ab-117">Update hello deployment with a new service package file (.cspkg) and service configuration file (.cscfg).</span></span>

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. <span data-ttu-id="f97ab-119">**Du kan också** uppdatera hello distributionsetiketten och hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="f97ab-119">**Optionally** update hello deployment label and hello storage account.</span></span>
5. <span data-ttu-id="f97ab-120">Om några roller har endast en rollinstans väljer hello **distribuera även om en eller flera roller innehåller en enda instans** tooenable hello uppgradera tooproceed.</span><span class="sxs-lookup"><span data-stu-id="f97ab-120">If any roles have only one role instance, select hello **Deploy even if one or more roles contain a single instance** tooenable hello upgrade tooproceed.</span></span>

    <span data-ttu-id="f97ab-121">Azure kan bara garantera tjänsttillgänglighet 99,95% under en molnet tjänstuppdatering om rollerna har minst två rollinstanser (virtuella datorer).</span><span class="sxs-lookup"><span data-stu-id="f97ab-121">Azure can only guarantee 99.95 percent service availability during a cloud service update if each role has at least two role instances (virtual machines).</span></span> <span data-ttu-id="f97ab-122">Med två rollinstanser bearbetar en virtuell dator klientbegäranden medan hello andra uppdateras.</span><span class="sxs-lookup"><span data-stu-id="f97ab-122">With two role instances, one virtual machine processes client requests while hello other is updated.</span></span>

6. <span data-ttu-id="f97ab-123">Kontrollera **starta distribution** toohave hello uppdateringen när hello uppladdningen av hello paketet är klar.</span><span class="sxs-lookup"><span data-stu-id="f97ab-123">Check **Start deployment** toohave hello update applied after hello upload of hello package has finished.</span></span>
7. <span data-ttu-id="f97ab-124">Klicka på **OK** toobegin uppdatera hello-tjänst.</span><span class="sxs-lookup"><span data-stu-id="f97ab-124">Click **OK** toobegin updating hello service.</span></span>

## <a name="how-to-swap-deployments-toopromote-a-staged-deployment-tooproduction"></a><span data-ttu-id="f97ab-125">Så här: växla distributioner toopromote tooproduction en stegvis distribution</span><span class="sxs-lookup"><span data-stu-id="f97ab-125">How to: Swap deployments toopromote a staged deployment tooproduction</span></span>
<span data-ttu-id="f97ab-126">När du bestämmer dig toodeploy en ny version av en tjänst i molnet, steg och testa den nya versionen i din fristående molntjänstmiljö.</span><span class="sxs-lookup"><span data-stu-id="f97ab-126">When you decide toodeploy a new release of a cloud service, stage and test your new release in your cloud service staging environment.</span></span> <span data-ttu-id="f97ab-127">Använd **växla** tooswitch hello URL: er av vilka hello två distributioner behandlas och befordra en ny version tooproduction.</span><span class="sxs-lookup"><span data-stu-id="f97ab-127">Use **Swap** tooswitch hello URLs by which hello two deployments are addressed and promote a new release tooproduction.</span></span>

<span data-ttu-id="f97ab-128">Du kan växla distributioner från hello **molntjänster** sida eller hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="f97ab-128">You can swap deployments from hello **Cloud Services** page or hello dashboard.</span></span>

1. <span data-ttu-id="f97ab-129">I hello [Azure-portalen][Azure portal], Välj hello molntjänst som du vill tooupdate.</span><span class="sxs-lookup"><span data-stu-id="f97ab-129">In hello [Azure portal][Azure portal], select hello cloud service you want tooupdate.</span></span> <span data-ttu-id="f97ab-130">Det här steget öppnas hello cloud service-instans bladet.</span><span class="sxs-lookup"><span data-stu-id="f97ab-130">This step opens hello cloud service instance blade.</span></span>
2. <span data-ttu-id="f97ab-131">Hello-bladet klickar du på hello **växla** knappen.</span><span class="sxs-lookup"><span data-stu-id="f97ab-131">In hello blade, click hello **Swap** button.</span></span>

    ![Cloud Services växlingsutrymme](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. <span data-ttu-id="f97ab-133">hello öppnas följande bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="f97ab-133">hello following confirmation prompt opens.</span></span>

    ![Cloud Services växlingsutrymme](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. <span data-ttu-id="f97ab-135">När du har kontrollerat hello distributionsinformation klickar du på **OK** tooswap hello distributioner.</span><span class="sxs-lookup"><span data-stu-id="f97ab-135">After you verify hello deployment information, click **OK** tooswap hello deployments.</span></span>

    <span data-ttu-id="f97ab-136">hello distribution växlingen sker snabbt eftersom hello enda som ändras är hello virtuella IP-adresser (VIP) för hello-distributioner.</span><span class="sxs-lookup"><span data-stu-id="f97ab-136">hello deployment swap happens quickly because hello only thing that changes is hello virtual IP addresses (VIPs) for hello deployments.</span></span>

    <span data-ttu-id="f97ab-137">toosave beräkning kostnader, kan du ta bort hello mellanlagring av distribution när du har kontrollerat att din Produktionsdistribution fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="f97ab-137">toosave compute costs, you can delete hello staging deployment after you verify that your production deployment is working as expected.</span></span>

### <a name="common-questions-about-swapping-deployments"></a><span data-ttu-id="f97ab-138">Vanliga frågor om växling distributioner</span><span class="sxs-lookup"><span data-stu-id="f97ab-138">Common questions about swapping deployments</span></span>

<span data-ttu-id="f97ab-139">**Vad är hello förutsättningarna för att byta distributioner?**</span><span class="sxs-lookup"><span data-stu-id="f97ab-139">**What are hello prerequisites for swapping deployments?**</span></span>

<span data-ttu-id="f97ab-140">Det finns två viktiga krav för en lyckad distribution växling:</span><span class="sxs-lookup"><span data-stu-id="f97ab-140">There are two key prerequisites for a successful deployment swap:</span></span>

- <span data-ttu-id="f97ab-141">Om du vill att toouse en statisk IP-adress för din produktionsplatsen, måste du reservera en för din mellanlagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="f97ab-141">If you would like toouse a static IP address for your production slot, you must reserve one for your staging slot as well.</span></span> <span data-ttu-id="f97ab-142">Annars misslyckas hello växlingen.</span><span class="sxs-lookup"><span data-stu-id="f97ab-142">Otherwise, hello swap fails.</span></span>

- <span data-ttu-id="f97ab-143">Alla instanser av dina roller måste köras innan du kan utföra hello växlingen.</span><span class="sxs-lookup"><span data-stu-id="f97ab-143">All instances of your roles must be running before you can perform hello swap.</span></span> <span data-ttu-id="f97ab-144">Du kan kontrollera hello status för dina instanser i bladet för hello översikt av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f97ab-144">You can check hello status of your instances in hello overview blade of hello Azure portal.</span></span> <span data-ttu-id="f97ab-145">Du kan också använda hello [Get-AzureRole](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0) i Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f97ab-145">Alternatively, you can use hello [Get-AzureRole](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0) command in Windows PowerShell.</span></span>

<span data-ttu-id="f97ab-146">Observera att Gästoperativsystem uppdateringar och service återställning åtgärder kan också orsakas av distributionen växlingar toofail.</span><span class="sxs-lookup"><span data-stu-id="f97ab-146">Note that Guest OS updates and service healing operations can also cause deployment swaps toofail.</span></span> <span data-ttu-id="f97ab-147">Mer information finns i [distribution felsöka cloud service](cloud-services-troubleshoot-deployment-problems.md).</span><span class="sxs-lookup"><span data-stu-id="f97ab-147">For more information, see [Troubleshoot cloud service deployment problems](cloud-services-troubleshoot-deployment-problems.md).</span></span>

<span data-ttu-id="f97ab-148">**Medför en växling driftstopp för Mina program? Hur får jag hantera den?**</span><span class="sxs-lookup"><span data-stu-id="f97ab-148">**Does a swap incur downtime for my application? How should I handle it?**</span></span>

<span data-ttu-id="f97ab-149">Enligt beskrivningen i hello sista avsnittet är en distribution växling vanligtvis snabbt eftersom det är bara en konfigurationsändring i hello Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="f97ab-149">As described in hello last section, a deployment swap is typically fast since it is just a configuration change in hello Azure load balancer.</span></span> <span data-ttu-id="f97ab-150">I vissa fall kan det dock ta tio eller fler sekunder och resultera i tillfälliga anslutningsfel.</span><span class="sxs-lookup"><span data-stu-id="f97ab-150">In some cases, however, it can take ten or more seconds and result in transient connection failures.</span></span> <span data-ttu-id="f97ab-151">toolimit påverkan tooyour kunder, Överväg att implementera [klienten logik](../best-practices-retry-general.md).</span><span class="sxs-lookup"><span data-stu-id="f97ab-151">toolimit impact tooyour customers, consider implementing [client retry logic](../best-practices-retry-general.md).</span></span>

## <a name="how-to-link-a-resource-tooa-cloud-service"></a><span data-ttu-id="f97ab-152">Så här: länka en resurs tooa molntjänst</span><span class="sxs-lookup"><span data-stu-id="f97ab-152">How to: Link a resource tooa cloud service</span></span>
<span data-ttu-id="f97ab-153">hello Azure-portalen länkar inte resurser tillsammans som har hello aktuella klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f97ab-153">hello Azure portal does not link resources together like hello current Azure classic portal does.</span></span> <span data-ttu-id="f97ab-154">Distribuera ytterligare resurser toohello i stället samma resursgrupp som används av hello tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="f97ab-154">Instead, deploy additional resources toohello same resource group being used by hello Cloud Service.</span></span>

## <a name="how-to-delete-deployments-and-a-cloud-service"></a><span data-ttu-id="f97ab-155">Så här: ta bort distributioner och en tjänst i molnet</span><span class="sxs-lookup"><span data-stu-id="f97ab-155">How to: Delete deployments and a cloud service</span></span>
<span data-ttu-id="f97ab-156">Innan du kan ta bort en molnbaserad tjänst, måste du ta bort alla befintliga distributionen.</span><span class="sxs-lookup"><span data-stu-id="f97ab-156">Before you can delete a cloud service, you must delete each existing deployment.</span></span>

<span data-ttu-id="f97ab-157">toosave beräkning kostnader, kan du ta bort hello mellanlagring av distribution när du har kontrollerat att din Produktionsdistribution fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="f97ab-157">toosave compute costs, you can delete hello staging deployment after you verify that your production deployment is working as expected.</span></span> <span data-ttu-id="f97ab-158">Du debiteras för beräkning kostnader för distribuerade rollinstanser som har stoppats.</span><span class="sxs-lookup"><span data-stu-id="f97ab-158">You are billed for compute costs for deployed role instances that are stopped.</span></span>

<span data-ttu-id="f97ab-159">Använd följande procedur toodelete hello en distribution eller Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="f97ab-159">Use hello following procedure toodelete a deployment or your cloud service.</span></span>

1. <span data-ttu-id="f97ab-160">I hello [Azure-portalen][Azure portal], Välj hello molntjänst som du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="f97ab-160">In hello [Azure portal][Azure portal], select hello cloud service you want toodelete.</span></span> <span data-ttu-id="f97ab-161">Det här steget öppnas hello cloud service-instans bladet.</span><span class="sxs-lookup"><span data-stu-id="f97ab-161">This step opens hello cloud service instance blade.</span></span>
2. <span data-ttu-id="f97ab-162">Hello-bladet klickar du på hello **ta bort** knappen.</span><span class="sxs-lookup"><span data-stu-id="f97ab-162">In hello blade, click hello **Delete** button.</span></span>

    ![Cloud Services växlingsutrymme](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. <span data-ttu-id="f97ab-164">Du kan ta bort hello hela molntjänst genom att kontrollera **Cloud service och dess distributioner** eller välj antingen hello **Produktionsdistribution** eller hello **mellanlagring av distribution**.</span><span class="sxs-lookup"><span data-stu-id="f97ab-164">You can delete hello entire cloud service by checking **Cloud service and its deployments** or choose either hello **Production deployment** or hello **Staging deployment**.</span></span>

    ![Cloud Services växlingsutrymme](./media/cloud-services-how-to-manage-portal/delete-blade.png)

4. <span data-ttu-id="f97ab-166">Klicka på hello **ta bort** knappen längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="f97ab-166">Click hello **Delete** button at hello bottom.</span></span>
5. <span data-ttu-id="f97ab-167">toodelete hello-Molntjänsten, klicka på **ta bort Molntjänsten**.</span><span class="sxs-lookup"><span data-stu-id="f97ab-167">toodelete hello cloud service, click **Delete cloud service**.</span></span> <span data-ttu-id="f97ab-168">Klicka sedan på hello bekräftelse på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="f97ab-168">Then, at hello confirmation prompt, click **Yes**.</span></span>

> [!NOTE]
> <span data-ttu-id="f97ab-169">När en tjänst i molnet har tagits bort och utförlig övervakning är konfigurerad, måste du ta bort hello data manuellt från ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f97ab-169">When a cloud service is deleted, and verbose monitoring is configured, you must delete hello data manually from your storage account.</span></span> <span data-ttu-id="f97ab-170">Information om där toofind hello mått tabeller finns [detta](cloud-services-how-to-monitor.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="f97ab-170">For information about where toofind hello metrics tables, see [this](cloud-services-how-to-monitor.md) article.</span></span>


## <a name="how-to-find-more-information-about-failed-deployments"></a><span data-ttu-id="f97ab-171">Så här: Mer information om misslyckade installationer</span><span class="sxs-lookup"><span data-stu-id="f97ab-171">How to: Find more information about failed deployments</span></span>
<span data-ttu-id="f97ab-172">Hej **översikt** bladet har ett statusfält hello överst.</span><span class="sxs-lookup"><span data-stu-id="f97ab-172">hello **Overview** blade has a status bar at hello top.</span></span> <span data-ttu-id="f97ab-173">När du klickar på hello fältet ett nytt blad öppnas och visar den felinformation.</span><span class="sxs-lookup"><span data-stu-id="f97ab-173">When you click hello bar, a new blade opens and displays any error information.</span></span> <span data-ttu-id="f97ab-174">Om hello distributionen inte innehåller några fel, är hello information bladet tomt.</span><span class="sxs-lookup"><span data-stu-id="f97ab-174">If hello deployment does not contain any errors, hello information blade is blank.</span></span>

![Cloud Services växlingsutrymme](./media/cloud-services-how-to-manage-portal/status-info.png)



[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a><span data-ttu-id="f97ab-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f97ab-176">Next steps</span></span>
* <span data-ttu-id="f97ab-177">[Allmän konfiguration av Molntjänsten](cloud-services-how-to-configure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f97ab-177">[General configuration of your cloud service](cloud-services-how-to-configure-portal.md).</span></span>
* <span data-ttu-id="f97ab-178">Lär dig hur för[distribuera en tjänst i molnet](cloud-services-how-to-create-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f97ab-178">Learn how too[deploy a cloud service](cloud-services-how-to-create-deploy-portal.md).</span></span>
* <span data-ttu-id="f97ab-179">Konfigurera en [domännamn](cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f97ab-179">Configure a [custom domain name](cloud-services-custom-domain-name-portal.md).</span></span>
* <span data-ttu-id="f97ab-180">Konfigurera [ssl-certifikat](cloud-services-configure-ssl-certificate-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f97ab-180">Configure [ssl certificates](cloud-services-configure-ssl-certificate-portal.md).</span></span>
