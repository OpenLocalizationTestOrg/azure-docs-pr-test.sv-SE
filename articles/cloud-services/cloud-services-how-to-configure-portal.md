---
title: "Så här konfigurerar du en tjänst i molnet (portal) | Microsoft Docs"
description: "Lär dig hur du konfigurerar molntjänster i Azure. Lär dig att uppdatera tjänstkonfigurationen för molnet och konfigurera fjärråtkomst till rollinstanser. De här exemplen använder Azure-portalen."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 7308f3c0-825e-499d-bfa5-c60f86371921
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2016
ms.author: adegeo
ms.openlocfilehash: a7e891d05ffe4cc2b4f68dce072a81499cc6de80
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-configure-cloud-services"></a><span data-ttu-id="a07aa-105">Så här konfigurerar du molntjänster</span><span class="sxs-lookup"><span data-stu-id="a07aa-105">How to Configure Cloud Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a07aa-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a07aa-106">Azure portal</span></span>](cloud-services-how-to-configure-portal.md)
> * [<span data-ttu-id="a07aa-107">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="a07aa-107">Azure classic portal</span></span>](cloud-services-how-to-configure.md)
>
>

<span data-ttu-id="a07aa-108">Du kan konfigurera de vanligaste inställningarna för en molnbaserad tjänst i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a07aa-108">You can configure the most commonly used settings for a cloud service in the Azure portal.</span></span> <span data-ttu-id="a07aa-109">Du kan också uppdatera konfigurationsfilerna direkt, hämta en tjänstkonfigurationsfil för uppdatering och sedan överföra den uppdaterade filen och uppdatera molntjänsten med konfigurationsändringarna.</span><span class="sxs-lookup"><span data-stu-id="a07aa-109">Or, if you like to update your configuration files directly, download a service configuration file to update, and then upload the updated file and update the cloud service with the configuration changes.</span></span> <span data-ttu-id="a07aa-110">Vilket sätt du än väljer kommer konfigurationsuppdateringarna att skickas ut till alla rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="a07aa-110">Either way, the configuration updates are pushed out to all role instances.</span></span>

<span data-ttu-id="a07aa-111">Du kan också hantera instanser av dina molntjänstroller eller fjärrskrivbord i dem.</span><span class="sxs-lookup"><span data-stu-id="a07aa-111">You can also manage the instances of your cloud service roles, or remote desktop into them.</span></span>

<span data-ttu-id="a07aa-112">Azure kan bara garantera 99,95% tjänsten under configuration uppdateringar om du har minst två rollinstanser för varje roll.</span><span class="sxs-lookup"><span data-stu-id="a07aa-112">Azure can only ensure 99.95 percent service availability during the configuration updates if you have at least two role instances for every role.</span></span> <span data-ttu-id="a07aa-113">Som gör det möjligt för en virtuell dator att bearbeta klientbegäranden medan den andra uppdateras.</span><span class="sxs-lookup"><span data-stu-id="a07aa-113">That enables one virtual machine to process client requests while the other is being updated.</span></span> <span data-ttu-id="a07aa-114">Mer information finns i [serviceavtal](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="a07aa-114">For more information, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>

## <a name="change-a-cloud-service"></a><span data-ttu-id="a07aa-115">Ändra en tjänst i molnet</span><span class="sxs-lookup"><span data-stu-id="a07aa-115">Change a cloud service</span></span>
<span data-ttu-id="a07aa-116">Efter att den [Azure-portalen](https://portal.azure.com/), navigera till Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="a07aa-116">After opening the [Azure portal](https://portal.azure.com/), navigate to your cloud service.</span></span> <span data-ttu-id="a07aa-117">Härifrån kan hantera du många aspekter av den.</span><span class="sxs-lookup"><span data-stu-id="a07aa-117">From here, you manage many aspects of it.</span></span>

![Inställningssidan](./media/cloud-services-how-to-configure-portal/cloud-service.png)

<span data-ttu-id="a07aa-119">Den **inställningar** eller **alla inställningar** länkar öppnas den **inställningar** bladet där du kan ändra den **egenskaper**, ändra den **Configuration**, hantera den **certifikat**, installationsprogrammet **Varna regler**, och hantera den **användare** som har åtkomst till den här Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="a07aa-119">The **Settings** or **All settings** links will open up the **Settings** blade where you can change the **Properties**, change the **Configuration**, manage the **Certificates**, setup **Alert rules**, and manage the **Users** who have access to this cloud service.</span></span>

![Azure cloud service inställningsbladet](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

### <a name="manage-guest-os-version"></a><span data-ttu-id="a07aa-121">Hantera gäst-OS-version</span><span class="sxs-lookup"><span data-stu-id="a07aa-121">Manage Guest OS version</span></span>

<span data-ttu-id="a07aa-122">Som standard uppdaterar Azure regelbundet dina gästoperativsystemet till den senaste stödda bilden i OS-familjen som du har angett i tjänstkonfigurationen (.cscfg), till exempel Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="a07aa-122">By default, Azure periodically updates your guest OS to the latest supported image within the OS family that you've specified in your service configuration (.cscfg), such as Windows Server 2016.</span></span>

<span data-ttu-id="a07aa-123">Om du behöver använder en viss OS-version du kan ange i den **Configuration** bladet.</span><span class="sxs-lookup"><span data-stu-id="a07aa-123">If you need to target a specific OS version, you can set it in the **Configuration** blade.</span></span>

![Ange OS-version](./media/cloud-services-how-to-configure-portal/cs-settings-config-guestosversion.png)


>[!IMPORTANT]
> <span data-ttu-id="a07aa-125">Om du väljer en viss OS-version inaktiverar automatisk OS uppdateringar och gör korrigering ditt ansvar.</span><span class="sxs-lookup"><span data-stu-id="a07aa-125">Choosing a specific OS version disables automatic OS updates and makes patching your responsibility.</span></span> <span data-ttu-id="a07aa-126">Du måste se till att dina rollinstanser tar emot uppdateringar eller du kan visa ditt program till säkerhetsproblem.</span><span class="sxs-lookup"><span data-stu-id="a07aa-126">You must ensure that your role instances are receiving updates or you may expose your application to security vulnerabilities.</span></span>

## <a name="monitoring"></a><span data-ttu-id="a07aa-127">Övervakning</span><span class="sxs-lookup"><span data-stu-id="a07aa-127">Monitoring</span></span>
<span data-ttu-id="a07aa-128">Du kan lägga till aviseringar Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="a07aa-128">You can add alerts to your cloud service.</span></span> <span data-ttu-id="a07aa-129">Klicka på **inställningar** > **avisering regler** > **Lägg till avisering**.</span><span class="sxs-lookup"><span data-stu-id="a07aa-129">Click **Settings** > **Alert Rules** > **Add alert**.</span></span>

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

<span data-ttu-id="a07aa-130">Härifrån kan konfigurera du en avisering.</span><span class="sxs-lookup"><span data-stu-id="a07aa-130">From here, you can setup an alert.</span></span> <span data-ttu-id="a07aa-131">Med den **mått** listrutan, kan du konfigurera en avisering för följande typer av data.</span><span class="sxs-lookup"><span data-stu-id="a07aa-131">With the **Metric** drop down box, you can setup an alert for the following types of data.</span></span>

* <span data-ttu-id="a07aa-132">Disk-lästa</span><span class="sxs-lookup"><span data-stu-id="a07aa-132">Disk read</span></span>
* <span data-ttu-id="a07aa-133">Skrivning till disk</span><span class="sxs-lookup"><span data-stu-id="a07aa-133">Disk write</span></span>
* <span data-ttu-id="a07aa-134">Nätverk i</span><span class="sxs-lookup"><span data-stu-id="a07aa-134">Network in</span></span>
* <span data-ttu-id="a07aa-135">Nätverk ut</span><span class="sxs-lookup"><span data-stu-id="a07aa-135">Network out</span></span>
* <span data-ttu-id="a07aa-136">CPU-procent</span><span class="sxs-lookup"><span data-stu-id="a07aa-136">CPU percentage</span></span>

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a><span data-ttu-id="a07aa-137">Konfigurera övervakning från en mått panel</span><span class="sxs-lookup"><span data-stu-id="a07aa-137">Configure monitoring from a metric tile</span></span>
<span data-ttu-id="a07aa-138">Istället för att använda **inställningar** > **Varningsregler**, kan du klicka på någon av panelerna mått i den **övervakning** avsnitt i den **Molntjänsten** bladet.</span><span class="sxs-lookup"><span data-stu-id="a07aa-138">Instead of using **Settings** > **Alert Rules**, you can click on one of the metric tiles in the **Monitoring** section of the **Cloud service** blade.</span></span>

![Molntjänsten övervakning](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

<span data-ttu-id="a07aa-140">Härifrån kan du anpassa diagrammet används med panelen eller lägga till en varningsregel.</span><span class="sxs-lookup"><span data-stu-id="a07aa-140">From here you can customize the chart used with the tile, or add an alert rule.</span></span>

## <a name="reboot-reimage-or-remote-desktop"></a><span data-ttu-id="a07aa-141">Omstart eller avbildningsåterställning fjärrskrivbord</span><span class="sxs-lookup"><span data-stu-id="a07aa-141">Reboot, reimage, or remote desktop</span></span>
<span data-ttu-id="a07aa-142">Just nu kan du konfigurera remote desktop med hjälp av den **Azure-portalen**.</span><span class="sxs-lookup"><span data-stu-id="a07aa-142">At this time you cannot configure remote desktop using the **Azure portal**.</span></span> <span data-ttu-id="a07aa-143">Men du kan konfigurera det via den [klassiska Azure-portalen](cloud-services-role-enable-remote-desktop.md), [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), eller via [Visual Studio](../vs-azure-tools-remote-desktop-roles.md).</span><span class="sxs-lookup"><span data-stu-id="a07aa-143">However, you can set it up through the [Azure classic portal](cloud-services-role-enable-remote-desktop.md), [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), or through [Visual Studio](../vs-azure-tools-remote-desktop-roles.md).</span></span>

<span data-ttu-id="a07aa-144">Klicka först på cloud service-instans.</span><span class="sxs-lookup"><span data-stu-id="a07aa-144">First, click on the cloud service instance.</span></span>

![Cloud Service-instans](./media/cloud-services-how-to-configure-portal/cs-instance.png)

<span data-ttu-id="a07aa-146">I bladet som öppnas initiera en fjärrskrivbordsanslutning, starta från en fjärrdator om instansen eller via fjärranslutning återavbilda (startar med en ny avbildning) instansen.</span><span class="sxs-lookup"><span data-stu-id="a07aa-146">From the blade that opens you can initiate a remote desktop connection, remotely reboot the instance, or remotely reimage (start with a fresh image) the instance.</span></span>

![Cloud Service-instans knappar](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)

## <a name="reconfigure-your-cscfg"></a><span data-ttu-id="a07aa-148">Konfigurera om din .cscfg</span><span class="sxs-lookup"><span data-stu-id="a07aa-148">Reconfigure your .cscfg</span></span>
<span data-ttu-id="a07aa-149">Du kan behöva konfigurera om Molntjänsten via den [service config (cscfg)](cloud-services-model-and-package.md#cscfg) fil.</span><span class="sxs-lookup"><span data-stu-id="a07aa-149">You may need to reconfigure your cloud service through the [service config (cscfg)](cloud-services-model-and-package.md#cscfg) file.</span></span> <span data-ttu-id="a07aa-150">Du måste först hämta .cscfg-filen och ändra den sedan ladda upp den.</span><span class="sxs-lookup"><span data-stu-id="a07aa-150">First you need to download your .cscfg file, modify it, then upload it.</span></span>

1. <span data-ttu-id="a07aa-151">Klicka på den **inställningar** ikon eller **alla inställningar** länken för att öppna den **inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="a07aa-151">Click on the **Settings** icon or the **All settings** link to open up the **Settings** blade.</span></span>

    ![Inställningssidan](./media/cloud-services-how-to-configure-portal/cloud-service.png)
2. <span data-ttu-id="a07aa-153">Klicka på den **Configuration** objekt.</span><span class="sxs-lookup"><span data-stu-id="a07aa-153">Click on the **Configuration** item.</span></span>

    ![Konfiguration av bladet](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)
3. <span data-ttu-id="a07aa-155">Klicka på den **hämta** knappen.</span><span class="sxs-lookup"><span data-stu-id="a07aa-155">Click on the **Download** button.</span></span>

    ![Ladda ned](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)
4. <span data-ttu-id="a07aa-157">När du har uppdaterat tjänstkonfigurationsfilen överföra och tillämpa konfigurationsuppdateringarna:</span><span class="sxs-lookup"><span data-stu-id="a07aa-157">After you update the service configuration file, upload and apply the configuration updates:</span></span>

    ![Ladda upp](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png)
5. <span data-ttu-id="a07aa-159">Välj .cscfg-filen och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="a07aa-159">Select the .cscfg file and click **OK**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a07aa-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a07aa-160">Next steps</span></span>
* <span data-ttu-id="a07aa-161">Lär dig hur du [distribuera en tjänst i molnet](cloud-services-how-to-create-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a07aa-161">Learn how to [deploy a cloud service](cloud-services-how-to-create-deploy-portal.md).</span></span>
* <span data-ttu-id="a07aa-162">Konfigurera en [domännamn](cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a07aa-162">Configure a [custom domain name](cloud-services-custom-domain-name-portal.md).</span></span>
* <span data-ttu-id="a07aa-163">[Hantera din molntjänst](cloud-services-how-to-manage-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a07aa-163">[Manage your cloud service](cloud-services-how-to-manage-portal.md).</span></span>
* <span data-ttu-id="a07aa-164">Konfigurera [ssl-certifikat](cloud-services-configure-ssl-certificate-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a07aa-164">Configure [ssl certificates](cloud-services-configure-ssl-certificate-portal.md).</span></span>
