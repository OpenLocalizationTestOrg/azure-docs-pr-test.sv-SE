---
title: "Så här konfigurerar du en tjänst i molnet (klassiska portal) | Microsoft Docs"
description: "Lär dig hur du konfigurerar molntjänster i Azure. Lär dig att uppdatera tjänstkonfigurationen för molnet och konfigurera fjärråtkomst till rollinstanser."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 4902f79d-ea91-41ca-89a4-7c818180ee5f
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 39bb294c96ce0c12d91cf8b3488ac3e1a7b2f7b2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-cloud-services"></a><span data-ttu-id="0a1e9-104">Så här konfigurerar du molntjänster</span><span class="sxs-lookup"><span data-stu-id="0a1e9-104">How to Configure Cloud Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0a1e9-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0a1e9-105">Azure portal</span></span>](cloud-services-how-to-configure-portal.md)
> * [<span data-ttu-id="0a1e9-106">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="0a1e9-106">Azure classic portal</span></span>](cloud-services-how-to-configure.md)
> 
> 

<span data-ttu-id="0a1e9-107">Du kan konfigurera de vanligaste inställningarna för en molntjänst i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-107">You can configure the most commonly used settings for a cloud service in the Azure classic portal.</span></span> <span data-ttu-id="0a1e9-108">Du kan också uppdatera konfigurationsfilerna direkt, hämta en tjänstkonfigurationsfil för uppdatering och sedan överföra den uppdaterade filen och uppdatera molntjänsten med konfigurationsändringarna.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-108">Or, if you like to update your configuration files directly, download a service configuration file to update, and then upload the updated file and update the cloud service with the configuration changes.</span></span> <span data-ttu-id="0a1e9-109">Vilket sätt du än väljer kommer konfigurationsuppdateringarna att skickas ut till alla rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-109">Either way, the configuration updates are pushed out to all role instances.</span></span>

<span data-ttu-id="0a1e9-110">Den klassiska Azure-portalen kan du också [aktivera anslutning till fjärrskrivbord för en roll i Azure Cloud Services](cloud-services-role-enable-remote-desktop.md)</span><span class="sxs-lookup"><span data-stu-id="0a1e9-110">The Azure classic portal also allows you to [enable Remote Desktop Connection for a Role in Azure Cloud Services](cloud-services-role-enable-remote-desktop.md)</span></span>

<span data-ttu-id="0a1e9-111">Azure kan bara garantera 99,95% tjänsten under configuration uppdateringar om du har minst två rollinstanser för varje roll.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-111">Azure can only ensure 99.95 percent service availability during the configuration updates if you have at least two role instances for every role.</span></span> <span data-ttu-id="0a1e9-112">Som gör det möjligt för en virtuell dator att bearbeta klientbegäranden medan den andra uppdateras.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-112">That enables one virtual machine to process client requests while the other is being updated.</span></span> <span data-ttu-id="0a1e9-113">Mer information finns i [serviceavtal](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="0a1e9-113">For more information, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>

## <a name="change-a-cloud-service"></a><span data-ttu-id="0a1e9-114">Ändra en tjänst i molnet</span><span class="sxs-lookup"><span data-stu-id="0a1e9-114">Change a cloud service</span></span>
1. <span data-ttu-id="0a1e9-115">I den [klassiska Azure-portalen](http://manage.windowsazure.com/), klickar du på **molntjänster**, klicka på namnet på Molntjänsten och klicka sedan på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-115">In the [Azure classic portal](http://manage.windowsazure.com/), click **Cloud Services**, click the name of the cloud service, and then click **Configure**.</span></span>
   
    ![Konfigurationssidan](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage1.png)
   
    <span data-ttu-id="0a1e9-117">På den **konfigurera** kan du konfigurera övervakning, uppdatering rollinställningar och välj gästoperativsystemet och familj för rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-117">On the **Configure** page, you can configure monitoring, update role settings, and choose the guest operating system and family for role instances.</span></span> 
2. <span data-ttu-id="0a1e9-118">I **övervakning**, ställa in övervakning på utförlig eller minimalt och konfigurera diagnostik-anslutningssträngar som krävs för övervakning av utförlig.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-118">In **monitoring**, set the monitoring level to Verbose or Minimal, and configure the diagnostics connection strings that are required for verbose monitoring.</span></span>
3. <span data-ttu-id="0a1e9-119">Du kan uppdatera följande inställningar för tjänsten roller (grupperade efter roll):</span><span class="sxs-lookup"><span data-stu-id="0a1e9-119">For service roles (grouped by role), you can update the following settings:</span></span>
   
    * <span data-ttu-id="0a1e9-120">**Inställningar för** ändra värden för diverse konfigurationsinställningar som anges i den *ConfigurationSettings* element i tjänstekonfigurationsfilen (.cscfg).</span><span class="sxs-lookup"><span data-stu-id="0a1e9-120">**Settings** Modify the values of miscellaneous configuration settings that are specified in the *ConfigurationSettings* elements of the service configuration (.cscfg) file.</span></span>

    * <span data-ttu-id="0a1e9-121">**Certifikat**</span><span class="sxs-lookup"><span data-stu-id="0a1e9-121">**Certificates**</span></span>  
        <span data-ttu-id="0a1e9-122">Ändra tumavtrycket för certifikatet som används i SSL-kryptering för en roll.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-122">Change the certificate thumbprint that's being used in SSL encryption for a role.</span></span> <span data-ttu-id="0a1e9-123">Om du vill ändra ett certifikat, måste du först ladda upp det nya certifikatet (på den **certifikat** sidan).</span><span class="sxs-lookup"><span data-stu-id="0a1e9-123">To change a certificate, you must first upload the new certificate (on the **Certificates** page).</span></span> <span data-ttu-id="0a1e9-124">Uppdatera tumavtrycket i certifikatet strängen som visas i rollinställningarna för.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-124">Then update the thumbprint in the certificate string displayed in the role settings.</span></span>
4. <span data-ttu-id="0a1e9-125">I **operativsystemet**, kan du ändra operativsystemsfamilj eller version för rollinstanser, eller välj **automatisk** att aktivera Automatiska uppdateringar av den aktuella versionen av operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-125">In **operating system**, you can change the operating system family or version for role instances, or choose **Automatic** to enable automatic updates of the current operating system version.</span></span> <span data-ttu-id="0a1e9-126">Inställningar för operativsystemet gäller webb- och arbetsroller, men påverkar inte virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-126">The operating system settings apply to web roles and worker roles, but do not affect Virtual Machines.</span></span>
   
    <span data-ttu-id="0a1e9-127">Den senaste versionen av operativsystemet är installerat på alla rollinstanser under distributionen och operativsystem som uppdateras automatiskt som standard.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-127">During deployment, the most recent operating system version is installed on all role instances, and the operating systems are updated automatically by default.</span></span> 
   
    <span data-ttu-id="0a1e9-128">Om du behöver för din molntjänst som körs på en annan operativsystemversion på grund av kompatibilitetskraven i koden, väljer du en operativsystemsfamilj och version.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-128">If you need for your cloud service to run on a different operating system version because of compatibility requirements in your code, you can choose an operating system family and version.</span></span> <span data-ttu-id="0a1e9-129">När du väljer en specifik operativsystemsversion har automatisk operativsystemuppdateringar för Molntjänsten avbrutits.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-129">When you choose a specific operating system version, automatic operating system updates for the cloud service are suspended.</span></span> <span data-ttu-id="0a1e9-130">Du måste se till att operativsystem som tar emot uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-130">You will need to ensure the operating systems receive updates.</span></span>
   
    <span data-ttu-id="0a1e9-131">Om du har löst alla problem med programkompatibilitet som dina appar har med den senaste versionen av operativsystemet kan du aktivera automatisk operativsystemuppdateringar genom att ange operativsystemets version **automatisk**.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-131">If you resolve all compatibility issues that your apps have with the most recent operating system version, you can enable automatic operating system updates by setting the operating system version to **Automatic**.</span></span> 
   
    ![OS-inställningar](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage_OSSettings.png)
5. <span data-ttu-id="0a1e9-133">Om du vill spara inställningarna och push-installera dem rollinstanserna, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-133">To save your configuration settings, and push them to the role instances, click **Save**.</span></span> <span data-ttu-id="0a1e9-134">(Klicka på **Ignorera** att avbryta ändringarna.) **Spara** och **Ignorera** läggs till i kommandofältet när du ändrar en inställning.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-134">(Click **Discard** to cancel the changes.) **Save** and **Discard** are added to the command bar after you change a setting.</span></span>

## <a name="update-a-cloud-service-configuration-file"></a><span data-ttu-id="0a1e9-135">Uppdatera en cloud service-konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="0a1e9-135">Update a cloud service configuration file</span></span>
1. <span data-ttu-id="0a1e9-136">Hämta en cloud service konfigurationsfilen (.cscfg) med den aktuella konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-136">Download a cloud service configuration file (.cscfg) with the current configuration.</span></span> <span data-ttu-id="0a1e9-137">På den **konfigurera** för Molntjänsten, klickar du på **hämta**.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-137">On the **Configure** page for the cloud service, click **Download**.</span></span> <span data-ttu-id="0a1e9-138">Klicka på **spara**, eller klicka på **Spara som** att spara filen.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-138">Then click **Save**, or click **Save As** to save the file.</span></span>
2. <span data-ttu-id="0a1e9-139">När du har uppdaterat tjänstkonfigurationsfilen överföra och tillämpa konfigurationsuppdateringarna:</span><span class="sxs-lookup"><span data-stu-id="0a1e9-139">After you update the service configuration file, upload and apply the configuration updates:</span></span>
   
   1. <span data-ttu-id="0a1e9-140">På den **konfigurera** klickar du på **överför**.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-140">On the **Configure** page, click **Upload**.</span></span>
      
       ![Överför konfiguration](./media/cloud-services-how-to-configure/CloudServices_UploadConfigFile.png)
   2. <span data-ttu-id="0a1e9-142">I **konfigurationsfilen**, använda **Bläddra** att välja uppdaterade .cscfg-filen.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-142">In **Configuration file**, use **Browse** to select the updated .cscfg file.</span></span>
   3. <span data-ttu-id="0a1e9-143">Om din molntjänst innehåller de roller som har en enda instans, Välj den **tillämpa konfigurationen även om en eller flera roller innehåller en enda instans** kryssrutan för att aktivera configuration uppdateringar för rollerna som ska fortsätta.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-143">If your cloud service contains any roles that have only one instance, select the **Apply configuration even if one or more roles contain a single instance** check box to enable the configuration updates for the roles to proceed.</span></span>
      
       <span data-ttu-id="0a1e9-144">Om du inte definierar minst två instanser av varje roll, Azure kan inte garantera till minst 99,95% tillgänglighet för din molntjänst under service configuration uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="0a1e9-144">Unless you define at least two instances of every role, Azure cannot guarantee at least 99.95 percent availability of your cloud service during service configuration updates.</span></span> <span data-ttu-id="0a1e9-145">Mer information finns i [serviceavtal](https://azure.microsoft.com/support/legal/sla/).</span><span class="sxs-lookup"><span data-stu-id="0a1e9-145">For more information, see [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).</span></span>
   4. <span data-ttu-id="0a1e9-146">Klicka på **OK** (markering).</span><span class="sxs-lookup"><span data-stu-id="0a1e9-146">Click **OK** (checkmark).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="0a1e9-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0a1e9-147">Next steps</span></span>
* <span data-ttu-id="0a1e9-148">Lär dig hur du [distribuera en tjänst i molnet](cloud-services-how-to-create-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="0a1e9-148">Learn how to [deploy a cloud service](cloud-services-how-to-create-deploy.md).</span></span>
* <span data-ttu-id="0a1e9-149">Konfigurera en [domännamn](cloud-services-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="0a1e9-149">Configure a [custom domain name](cloud-services-custom-domain-name.md).</span></span>
* <span data-ttu-id="0a1e9-150">[Hantera din molntjänst](cloud-services-how-to-manage.md).</span><span class="sxs-lookup"><span data-stu-id="0a1e9-150">[Manage your cloud service](cloud-services-how-to-manage.md).</span></span>
* [<span data-ttu-id="0a1e9-151">Aktivera anslutning till fjärrskrivbord för en roll i Azure-molntjänster</span><span class="sxs-lookup"><span data-stu-id="0a1e9-151">Enable Remote Desktop Connection for a Role in Azure Cloud Services</span></span>](cloud-services-role-enable-remote-desktop.md)
* <span data-ttu-id="0a1e9-152">Konfigurera [ssl-certifikat](cloud-services-configure-ssl-certificate.md).</span><span class="sxs-lookup"><span data-stu-id="0a1e9-152">Configure [ssl certificates](cloud-services-configure-ssl-certificate.md).</span></span>

