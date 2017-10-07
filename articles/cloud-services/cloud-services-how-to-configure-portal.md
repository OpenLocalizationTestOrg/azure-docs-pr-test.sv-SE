---
title: "aaaHow tooconfigure en tjänst i molnet (portal) | Microsoft Docs"
description: "Lär dig hur tooconfigure cloud services i Azure. Lär dig tjänstkonfigurationen för tooupdate hello molnet och konfigurera fjärråtkomst toorole instanser. Dessa exempel används hello Azure-portalen."
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
ms.openlocfilehash: 969a08558473e8c79153192942bfda587eb5ada5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-cloud-services"></a><span data-ttu-id="78edc-105">Hur tooConfigure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="78edc-105">How tooConfigure Cloud Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="78edc-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="78edc-106">Azure portal</span></span>](cloud-services-how-to-configure-portal.md)
> * [<span data-ttu-id="78edc-107">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="78edc-107">Azure classic portal</span></span>](cloud-services-how-to-configure.md)
>
>

<span data-ttu-id="78edc-108">Du kan konfigurera hello de vanligaste inställningarna för en tjänst i molnet i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="78edc-108">You can configure hello most commonly used settings for a cloud service in hello Azure portal.</span></span> <span data-ttu-id="78edc-109">Eller, om du vill tooupdate konfigurationsfilerna direkt, hämta en service configuration file tooupdate och överför hello uppdateras filen och uppdatera hello Molntjänsten hello konfigurationsändringar.</span><span class="sxs-lookup"><span data-stu-id="78edc-109">Or, if you like tooupdate your configuration files directly, download a service configuration file tooupdate, and then upload hello updated file and update hello cloud service with hello configuration changes.</span></span> <span data-ttu-id="78edc-110">Oavsett hur push hello konfigurationsuppdateringar-installeras tooall rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="78edc-110">Either way, hello configuration updates are pushed out tooall role instances.</span></span>

<span data-ttu-id="78edc-111">Du kan också hantera hello instanser av dina molntjänstroller eller fjärrskrivbord i dem.</span><span class="sxs-lookup"><span data-stu-id="78edc-111">You can also manage hello instances of your cloud service roles, or remote desktop into them.</span></span>

<span data-ttu-id="78edc-112">Azure kan bara kontrollera 99,95% tjänsttillgänglighet under hello konfigurationsuppdateringar om du har minst två rollinstanser för varje roll.</span><span class="sxs-lookup"><span data-stu-id="78edc-112">Azure can only ensure 99.95 percent service availability during hello configuration updates if you have at least two role instances for every role.</span></span> <span data-ttu-id="78edc-113">Som gör att en virtuell dator tooprocess klientbegäranden medan hello andra uppdateras.</span><span class="sxs-lookup"><span data-stu-id="78edc-113">That enables one virtual machine tooprocess client requests while hello other is being updated.</span></span> <span data-ttu-id="78edc-114">Mer information finns i [serviceavtal](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="78edc-114">For more information, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>

## <a name="change-a-cloud-service"></a><span data-ttu-id="78edc-115">Ändra en tjänst i molnet</span><span class="sxs-lookup"><span data-stu-id="78edc-115">Change a cloud service</span></span>
<span data-ttu-id="78edc-116">När du öppnar hello [Azure-portalen](https://portal.azure.com/), navigera tooyour Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="78edc-116">After opening hello [Azure portal](https://portal.azure.com/), navigate tooyour cloud service.</span></span> <span data-ttu-id="78edc-117">Härifrån kan hantera du många aspekter av den.</span><span class="sxs-lookup"><span data-stu-id="78edc-117">From here, you manage many aspects of it.</span></span>

![Inställningssidan](./media/cloud-services-how-to-configure-portal/cloud-service.png)

<span data-ttu-id="78edc-119">Hej **inställningar** eller **alla inställningar** länkar öppnas hello **inställningar** bladet där du kan ändra hello **egenskaper**, ändra hello **Configuration**, hantera hello **certifikat**, installationsprogrammet **Varna regler**, och hantera hello **användare** som har åtkomst toothis Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="78edc-119">hello **Settings** or **All settings** links will open up hello **Settings** blade where you can change hello **Properties**, change hello **Configuration**, manage hello **Certificates**, setup **Alert rules**, and manage hello **Users** who have access toothis cloud service.</span></span>

![Azure cloud service inställningsbladet](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

### <a name="manage-guest-os-version"></a><span data-ttu-id="78edc-121">Hantera gäst-OS-version</span><span class="sxs-lookup"><span data-stu-id="78edc-121">Manage Guest OS version</span></span>

<span data-ttu-id="78edc-122">Som standard uppdaterar Azure regelbundet gäst OS toohello senaste stöds bilden i hello OS-familj som du har angett i tjänstkonfigurationen (.cscfg), till exempel Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="78edc-122">By default, Azure periodically updates your guest OS toohello latest supported image within hello OS family that you've specified in your service configuration (.cscfg), such as Windows Server 2016.</span></span>

<span data-ttu-id="78edc-123">Om du behöver tootarget en viss OS-version, du kan ange i hello **Configuration** bladet.</span><span class="sxs-lookup"><span data-stu-id="78edc-123">If you need tootarget a specific OS version, you can set it in hello **Configuration** blade.</span></span>

![Ange OS-version](./media/cloud-services-how-to-configure-portal/cs-settings-config-guestosversion.png)


>[!IMPORTANT]
> <span data-ttu-id="78edc-125">Om du väljer en viss OS-version inaktiverar automatisk OS uppdateringar och gör korrigering ditt ansvar.</span><span class="sxs-lookup"><span data-stu-id="78edc-125">Choosing a specific OS version disables automatic OS updates and makes patching your responsibility.</span></span> <span data-ttu-id="78edc-126">Du måste se till att dina rollinstanser tar emot uppdateringar eller du kan visa dina program toosecurity säkerhetsproblem.</span><span class="sxs-lookup"><span data-stu-id="78edc-126">You must ensure that your role instances are receiving updates or you may expose your application toosecurity vulnerabilities.</span></span>

## <a name="monitoring"></a><span data-ttu-id="78edc-127">Övervakning</span><span class="sxs-lookup"><span data-stu-id="78edc-127">Monitoring</span></span>
<span data-ttu-id="78edc-128">Du kan lägga till aviseringar tooyour Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="78edc-128">You can add alerts tooyour cloud service.</span></span> <span data-ttu-id="78edc-129">Klicka på **inställningar** > **avisering regler** > **Lägg till avisering**.</span><span class="sxs-lookup"><span data-stu-id="78edc-129">Click **Settings** > **Alert Rules** > **Add alert**.</span></span>

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

<span data-ttu-id="78edc-130">Härifrån kan konfigurera du en avisering.</span><span class="sxs-lookup"><span data-stu-id="78edc-130">From here, you can setup an alert.</span></span> <span data-ttu-id="78edc-131">Med hello **mått** listrutan, kan du konfigurera en avisering om hello följande typer av data.</span><span class="sxs-lookup"><span data-stu-id="78edc-131">With hello **Metric** drop down box, you can setup an alert for hello following types of data.</span></span>

* <span data-ttu-id="78edc-132">Disk-lästa</span><span class="sxs-lookup"><span data-stu-id="78edc-132">Disk read</span></span>
* <span data-ttu-id="78edc-133">Skrivning till disk</span><span class="sxs-lookup"><span data-stu-id="78edc-133">Disk write</span></span>
* <span data-ttu-id="78edc-134">Nätverk i</span><span class="sxs-lookup"><span data-stu-id="78edc-134">Network in</span></span>
* <span data-ttu-id="78edc-135">Nätverk ut</span><span class="sxs-lookup"><span data-stu-id="78edc-135">Network out</span></span>
* <span data-ttu-id="78edc-136">CPU-procent</span><span class="sxs-lookup"><span data-stu-id="78edc-136">CPU percentage</span></span>

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a><span data-ttu-id="78edc-137">Konfigurera övervakning från en mått panel</span><span class="sxs-lookup"><span data-stu-id="78edc-137">Configure monitoring from a metric tile</span></span>
<span data-ttu-id="78edc-138">Istället för att använda **inställningar** > **Varningsregler**, kan du klicka på någon av hello mått paneler i hello **övervakning** avsnitt i hello **moln tjänsten** bladet.</span><span class="sxs-lookup"><span data-stu-id="78edc-138">Instead of using **Settings** > **Alert Rules**, you can click on one of hello metric tiles in hello **Monitoring** section of hello **Cloud service** blade.</span></span>

![Molntjänsten övervakning](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

<span data-ttu-id="78edc-140">Härifrån kan anpassa hello diagram används med hello panelen eller lägga till en varningsregel.</span><span class="sxs-lookup"><span data-stu-id="78edc-140">From here you can customize hello chart used with hello tile, or add an alert rule.</span></span>

## <a name="reboot-reimage-or-remote-desktop"></a><span data-ttu-id="78edc-141">Omstart eller avbildningsåterställning fjärrskrivbord</span><span class="sxs-lookup"><span data-stu-id="78edc-141">Reboot, reimage, or remote desktop</span></span>
<span data-ttu-id="78edc-142">Du kan inte konfigurera Fjärrskrivbord med hello just nu **Azure-portalen**.</span><span class="sxs-lookup"><span data-stu-id="78edc-142">At this time you cannot configure remote desktop using hello **Azure portal**.</span></span> <span data-ttu-id="78edc-143">Men du kan konfigurera det via hello [klassiska Azure-portalen](cloud-services-role-enable-remote-desktop.md), [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), eller via [Visual Studio](../vs-azure-tools-remote-desktop-roles.md).</span><span class="sxs-lookup"><span data-stu-id="78edc-143">However, you can set it up through hello [Azure classic portal](cloud-services-role-enable-remote-desktop.md), [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), or through [Visual Studio](../vs-azure-tools-remote-desktop-roles.md).</span></span>

<span data-ttu-id="78edc-144">Klicka först på hello cloud service-instans.</span><span class="sxs-lookup"><span data-stu-id="78edc-144">First, click on hello cloud service instance.</span></span>

![Cloud Service-instans](./media/cloud-services-how-to-configure-portal/cs-instance.png)

<span data-ttu-id="78edc-146">Från hello bladet som öppnar du initiera en fjärrskrivbordsanslutning, starta om hello instans, eller via fjärranslutning avbildningsåterställning (starta med en ny avbildning) hello via fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="78edc-146">From hello blade that opens you can initiate a remote desktop connection, remotely reboot hello instance, or remotely reimage (start with a fresh image) hello instance.</span></span>

![Cloud Service-instans knappar](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)

## <a name="reconfigure-your-cscfg"></a><span data-ttu-id="78edc-148">Konfigurera om din .cscfg</span><span class="sxs-lookup"><span data-stu-id="78edc-148">Reconfigure your .cscfg</span></span>
<span data-ttu-id="78edc-149">Du kanske måste tooreconfigure Molntjänsten via hello [service config (cscfg)](cloud-services-model-and-package.md#cscfg) fil.</span><span class="sxs-lookup"><span data-stu-id="78edc-149">You may need tooreconfigure your cloud service through hello [service config (cscfg)](cloud-services-model-and-package.md#cscfg) file.</span></span> <span data-ttu-id="78edc-150">Du måste först toodownload din .cscfg-filen, ändra den och sedan ladda upp den.</span><span class="sxs-lookup"><span data-stu-id="78edc-150">First you need toodownload your .cscfg file, modify it, then upload it.</span></span>

1. <span data-ttu-id="78edc-151">Klicka på hello **inställningar** ikonen eller hello **alla inställningar** länka tooopen in hello **inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="78edc-151">Click on hello **Settings** icon or hello **All settings** link tooopen up hello **Settings** blade.</span></span>

    ![Inställningssidan](./media/cloud-services-how-to-configure-portal/cloud-service.png)
2. <span data-ttu-id="78edc-153">Klicka på hello **Configuration** objekt.</span><span class="sxs-lookup"><span data-stu-id="78edc-153">Click on hello **Configuration** item.</span></span>

    ![Konfiguration av bladet](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)
3. <span data-ttu-id="78edc-155">Klicka på hello **hämta** knappen.</span><span class="sxs-lookup"><span data-stu-id="78edc-155">Click on hello **Download** button.</span></span>

    ![Ladda ned](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)
4. <span data-ttu-id="78edc-157">När du har uppdaterat hello-tjänstkonfigurationsfil ladda upp och tillämpa hello configuration uppdateringar:</span><span class="sxs-lookup"><span data-stu-id="78edc-157">After you update hello service configuration file, upload and apply hello configuration updates:</span></span>

    ![Ladda upp](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png)
5. <span data-ttu-id="78edc-159">Välj hello .cscfg-filen och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="78edc-159">Select hello .cscfg file and click **OK**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="78edc-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="78edc-160">Next steps</span></span>
* <span data-ttu-id="78edc-161">Lär dig hur för[distribuera en tjänst i molnet](cloud-services-how-to-create-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="78edc-161">Learn how too[deploy a cloud service](cloud-services-how-to-create-deploy-portal.md).</span></span>
* <span data-ttu-id="78edc-162">Konfigurera en [domännamn](cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="78edc-162">Configure a [custom domain name](cloud-services-custom-domain-name-portal.md).</span></span>
* <span data-ttu-id="78edc-163">[Hantera din molntjänst](cloud-services-how-to-manage-portal.md).</span><span class="sxs-lookup"><span data-stu-id="78edc-163">[Manage your cloud service](cloud-services-how-to-manage-portal.md).</span></span>
* <span data-ttu-id="78edc-164">Konfigurera [ssl-certifikat](cloud-services-configure-ssl-certificate-portal.md).</span><span class="sxs-lookup"><span data-stu-id="78edc-164">Configure [ssl certificates](cloud-services-configure-ssl-certificate-portal.md).</span></span>
