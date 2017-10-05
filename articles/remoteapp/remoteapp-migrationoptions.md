---
title: "Alternativ för att migrera utanför Azure RemoteApp | Microsoft Docs"
description: "Läs mer om alternativen för att migrera utanför Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: c4e0e5bc-5c13-4487-b1b6-ebf2a5edc1f0
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 9ab63124e2521ee1922d15c1e388c54d50eb8301
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="options-for-migrating-out-of-azure-remoteapp"></a><span data-ttu-id="bae78-103">Alternativ för att migrera utanför Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="bae78-103">Options for migrating out of Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="bae78-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="bae78-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="bae78-105">Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.</span><span class="sxs-lookup"><span data-stu-id="bae78-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>


<span data-ttu-id="bae78-106">Om du har slutat använda Azure RemoteApp eftersom den [pensionering meddelande](https://go.microsoft.com/fwlink/?linkid=821148) eller eftersom du är klar med utvärderingen, måste du migrera från Azure RemoteApp till en annan app service.</span><span class="sxs-lookup"><span data-stu-id="bae78-106">If you have stopped using Azure RemoteApp because of the [retirement announcement](https://go.microsoft.com/fwlink/?linkid=821148) or because you've finished your evaluation, you need to migrate off of Azure RemoteApp to another app service.</span></span> <span data-ttu-id="bae78-107">Det finns två olika metoder för att migrera: en självhanterad (kallas ofta infrastruktur som en tjänst [IaaS]) distribution eller en helt hanterad (ofta kallade plattform som en tjänst) eller programvara som en tjänst [PaaS/SaaS] erbjudande.</span><span class="sxs-lookup"><span data-stu-id="bae78-107">There are two different approaches for migrating: a self-managed (often called Infrastructure as a Service [IaaS]) deployment or a fully managed (often called Platform as a Service or Software as a Service [PaaS/SaaS]) offering.</span></span> 

<span data-ttu-id="bae78-108">Självbetjäning IaaS är ett själv distribution som är hanterade drivs och ägs av du distribuerats direkt på virtuella datorer (VM) eller fysiska system.</span><span class="sxs-lookup"><span data-stu-id="bae78-108">Self-service IaaS is a do-it-yourself deployment that is managed, operated, and owned by you, directly deployed on virtual machines (VMs) or physical systems.</span></span> <span data-ttu-id="bae78-109">I den andra änden en helt hanterad PaaS/SaaS erbjudande är mer som Azure RemoteApp - partner som tillhandahåller en tjänstnivå ovanpå en lösning för fjärrkommunikation som hanterar operativa och behandlingen när du som kund, göra vissa avbildningen och apphantering.</span><span class="sxs-lookup"><span data-stu-id="bae78-109">At the other end, a fully managed PaaS/SaaS offering is more like Azure RemoteApp - a partner provides a service layer on top of a remoting solution that handles operational and servicing, while you, as the customer, do some image and app management.</span></span>

<span data-ttu-id="bae78-110">[Visa Azure RemoteApp-Webbseminarier på migreringsalternativ](https://social.msdn.microsoft.com/Forums/azure/40557aaa-3e9f-403c-b221-ad3eac10dc56/migration-option-webinar-recordings?forum=AzureRemoteApp), eller Läs på för mer information (inklusive exempel på olika alternativ som värd).</span><span class="sxs-lookup"><span data-stu-id="bae78-110">[View the Azure RemoteApp webinars on migration options](https://social.msdn.microsoft.com/Forums/azure/40557aaa-3e9f-403c-b221-ad3eac10dc56/migration-option-webinar-recordings?forum=AzureRemoteApp), or read on for more information (including examples of the different hosting options).</span></span>

## <a name="self-managed-iaas-solutions"></a><span data-ttu-id="bae78-111">Fristående (IaaS) lösningar</span><span class="sxs-lookup"><span data-stu-id="bae78-111">Self-managed (IaaS) solutions</span></span>
### <a name="rds-on-iaas"></a><span data-ttu-id="bae78-112">**RDS på IaaS**</span><span class="sxs-lookup"><span data-stu-id="bae78-112">**RDS on IaaS**</span></span>
<span data-ttu-id="bae78-113">Du kan distribuera en intern sessionsbaserade Fjärrskrivbordstjänster (i Windows Server) distribution med RemoteApp eller skrivbord lokalt eller i en värdmiljö (t.ex. på virtuella Azure-datorer).</span><span class="sxs-lookup"><span data-stu-id="bae78-113">You can deploy a native session-based Remote Desktop Services (in Windows Server) deployment using either RemoteApp or desktops on-premises or in a hosted environment (like on Azure VMs).</span></span> <span data-ttu-id="bae78-114">RDS på IaaS-distributioner är bäst för kunder som redan är bekant med och som har befintliga teknisk expertis med RDS-distribution.</span><span class="sxs-lookup"><span data-stu-id="bae78-114">RDS on IaaS deployments are best for customers already familiar with and that have existing technical expertise with RDS deployments.</span></span> 

> [!NOTE]
> <span data-ttu-id="bae78-115">Du måste volymlicensiering med Software Assurance (SA) för klientåtkomstlicenser för Fjärrskrivbordstjänster använder det här distributionsalternativet.</span><span class="sxs-lookup"><span data-stu-id="bae78-115">You need Volume Licensing with Software Assurance (SA) for RDS client access licenses to use this deployment option.</span></span>

<span data-ttu-id="bae78-116">Distribution av Fjärrskrivbordstjänster på Azure Virtual Machines är enklare än någonsin när du använder distributions- och korrigering mallar (läsa ett [översikt](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/) och sedan [hämta dem](https://aka.ms/rdautomation)).</span><span class="sxs-lookup"><span data-stu-id="bae78-116">Deploying RDS on Azure VMs is easier than ever when you use deployment and patching templates (read an [overview](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/) and then [go get them](https://aka.ms/rdautomation)).</span></span> <span data-ttu-id="bae78-117">Du kan få samma elastiska skalning funktioner med Azure klassisk distribution modellen resurser (inte Resursmodell för Azure-resurser) i Azure RemoteApp med hjälp av den [automatisk skalning skriptet](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76), även om det finns fler anpassningar och konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="bae78-117">You can get the same elastic scaling capabilities with Azure classic deployment model resources (not Azure Resource Model resources) within Azure RemoteApp by using the [auto scaling script](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76), although there are more customizations and configurations.</span></span> <span data-ttu-id="bae78-118">När du distribuerar RDS på Azure Virtual Machines support tillhandahålls via [Azure stöder](https://azure.microsoft.com/support/plans/), samma supportpersonal som stöds av du med Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="bae78-118">When you deploy RDS on Azure VMs, support is provided through [Azure Support](https://azure.microsoft.com/support/plans/), the same support professionals that supported you with Azure RemoteApp.</span></span> <span data-ttu-id="bae78-119">Du kan hämta cost beräknar baserat på din användning av befintlig genom att kontakta [Azure-supporten](https://azure.microsoft.com/support/plans/), eller du kan utföra beräkningar själv med hjälp av en snart att vara publicerat kostnaden Kalkylatorn.</span><span class="sxs-lookup"><span data-stu-id="bae78-119">You can get cost estimates based on your existing usage by contacting [Azure Support](https://azure.microsoft.com/support/plans/), or you can perform calculations yourself using a soon to be released Cost Calculator.</span></span>  <span data-ttu-id="bae78-120">Dessutom N-serien virtuella datorer (för tillfället i privat förhandsvisning) kan du lägga till vGPU - höra mer om att lägga till vGPU och hur du [utnyttjar Fjärrskrivbordstjänster förbättringar i Windows Server 2016](https://myignite.microsoft.com/videos/2794) i vår Ignite-sessionen.</span><span class="sxs-lookup"><span data-stu-id="bae78-120">Also, with N-series VMs (currently in private preview) you can add vGPU - hear more about adding vGPU and about how to [harness RDS improvements in Windows Server 2016](https://myignite.microsoft.com/videos/2794) in our Ignite session.</span></span>   

<span data-ttu-id="bae78-121">Vi har steg för steg distributionsguider för [Windows Server 2012 R2](http://aka.ms/rdsonazure) och [Windows Server 2016](http://aka.ms/rdsonazure2016) att hjälpa dig med distributionen.</span><span class="sxs-lookup"><span data-stu-id="bae78-121">We have step by step deployment guides for [Windows Server 2012 R2](http://aka.ms/rdsonazure) and [Windows Server 2016](http://aka.ms/rdsonazure2016) to assist with your deployment.</span></span> <span data-ttu-id="bae78-122">Kolla in den [fjärrskrivbord blogg](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) för de senaste nyheterna.</span><span class="sxs-lookup"><span data-stu-id="bae78-122">Check out the [Remote Desktop blog](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) for the latest news.</span></span>

### <a name="citrix-on-iaas"></a><span data-ttu-id="bae78-123">**Citrix på IaaS**</span><span class="sxs-lookup"><span data-stu-id="bae78-123">**Citrix on IaaS**</span></span>
<span data-ttu-id="bae78-124">En intern Citrix sessionsbaserade XenApp eller XenDesktop kan vara distribueras lokalt eller i en värdmiljö (som på Azure Virtual Machines).</span><span class="sxs-lookup"><span data-stu-id="bae78-124">A native Citrix deployment of session-based XenApp or XenDesktop can be deployed on-premises or within a hosted environment (such as on Azure VMs).</span></span> 

<span data-ttu-id="bae78-125">Kolla in distributionsguiden stegvisa [Citrix XA 7.6 på Azure](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx), mer information.</span><span class="sxs-lookup"><span data-stu-id="bae78-125">Check out the step-by-step deployment guide, [Citrix XA 7.6 on Azure](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx), for more information.</span></span> <span data-ttu-id="bae78-126">Läs mer om [Citrix på Azure](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), inklusive ett pris Kalkylatorn.</span><span class="sxs-lookup"><span data-stu-id="bae78-126">Read more about [Citrix on Azure](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), including a price calculator.</span></span> <span data-ttu-id="bae78-127">Du kan också hitta en [Citrix Kontakta](http://citrix.com/English/contact/index.asp) att diskutera alternativen med.</span><span class="sxs-lookup"><span data-stu-id="bae78-127">You can also find a [Citrix contact](http://citrix.com/English/contact/index.asp) to discuss your options with.</span></span>

## <a name="fully-managed-paassaas-offerings"></a><span data-ttu-id="bae78-128">Helt hanterad PaaS/SaaS ()-erbjudanden</span><span class="sxs-lookup"><span data-stu-id="bae78-128">Fully managed (PaaS/SaaS) offerings</span></span>

### <a name="citrix-xenapp-essentials-released-april-2017"></a><span data-ttu-id="bae78-129">Citrix XenApp Essentials (utgiven April 2017)</span><span class="sxs-lookup"><span data-stu-id="bae78-129">Citrix XenApp Essentials (released April 2017)</span></span>
<span data-ttu-id="bae78-130">Nu tillgängligt på den [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Citrix.XenAppEssentials), Citrix XenApp Essentials är den nya application virtualization tjänsten kombinerar prestanda och flexibilitet för Citrix molnplattform med enkla, normativ och enkelt att använda Bild av Microsoft Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="bae78-130">Available now on the [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Citrix.XenAppEssentials), Citrix XenApp Essentials is the new application virtualization service, combining the power and flexibility of the Citrix Cloud platform with the simple, prescriptive, and easy-to-consume vision of Microsoft Azure RemoteApp.</span></span> 

<span data-ttu-id="bae78-131">Befintliga Azure RemoteApp-kunder kan [registrera dig för en kostnadsfri utvärderingsversion](https://www.citrix.com/products/citrix-cloud/form/xenapp-essentials-msft-trial/).</span><span class="sxs-lookup"><span data-stu-id="bae78-131">Existing Azure RemoteApp customers can [register for a free trial](https://www.citrix.com/products/citrix-cloud/form/xenapp-essentials-msft-trial/).</span></span>  <span data-ttu-id="bae78-132">Obs: Endast Citrix användaren avgift är ledig gäller Azure kostnader för beräkning och lagring</span><span class="sxs-lookup"><span data-stu-id="bae78-132">Note: Only Citrix user service charge is free, Azure compute and storage costs apply</span></span>

<span data-ttu-id="bae78-133">Lära sig mer:</span><span class="sxs-lookup"><span data-stu-id="bae78-133">Learn More:</span></span>
- [<span data-ttu-id="bae78-134">Migrera från Azure RemoteApp till Citrix XenApp Essentials</span><span class="sxs-lookup"><span data-stu-id="bae78-134">Migrate from Azure RemoteApp to Citrix XenApp Essentials</span></span>](remoteapp-migrate-citrix.md)
- [<span data-ttu-id="bae78-135">Citrix och Microsoft</span><span class="sxs-lookup"><span data-stu-id="bae78-135">Citrix and Microsoft</span></span>](https://www.citrix.com/global-partners/microsoft/remote-app.html)
- <span data-ttu-id="bae78-136">[Citrix XenApp Essentials presentation](https://www.youtube.com/watch?v=91Z7CCfQ-9k).</span><span class="sxs-lookup"><span data-stu-id="bae78-136">[Citrix XenApp Essentials presentation](https://www.youtube.com/watch?v=91Z7CCfQ-9k).</span></span>  

### <a name="citrix-cloud-xenapp-service-and-xendesktop-service"></a><span data-ttu-id="bae78-137">Citrix XenApp Molntjänsten och XenDesktop Service</span><span class="sxs-lookup"><span data-stu-id="bae78-137">Citrix Cloud XenApp Service and XenDesktop Service</span></span> 

<span data-ttu-id="bae78-138">[Citrix XenApp Molntjänsten och XenDesktop Service](https://www.citrix.com/products/citrix-cloud/services.html) är den bästa lösningen för leverans av både appar och stationära datorer, plus avancerad hantering och övervakning av funktioner.</span><span class="sxs-lookup"><span data-stu-id="bae78-138">[Citrix Cloud XenApp Service and XenDesktop Service](https://www.citrix.com/products/citrix-cloud/services.html) is the best solution for the delivery of both apps and desktops, plus advanced management and monitoring capabilities.</span></span> 

#### <a name="conexlink-platform-name-mycloudit"></a><span data-ttu-id="bae78-139">Conexlink (plattformsnamnet: MyCloudIT)</span><span class="sxs-lookup"><span data-stu-id="bae78-139">Conexlink (Platform name: MyCloudIT)</span></span>
<span data-ttu-id="bae78-140">[MyCloudIT](https://mycloudit.com) är en automatiseringsplattform för IT-företag att förenkla, optimera och skala migrering och leverans av fjärrskrivbord fjärrprogram och infrastrukturen i Microsoft Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="bae78-140">[MyCloudIT](https://mycloudit.com) is an automation platform for IT companies to simplify, optimize, and scale the migration and delivery of remote desktops, remote applications, and infrastructure in the Microsoft Azure Cloud.</span></span> 

<span data-ttu-id="bae78-141">Plattformens MyCloudIT minskar tidpunkten för distribution av 95%, Azure kostnaden med 30% och flyttar deras klient hela IT-infrastruktur till molnet i en fråga om några viktiga linjer.</span><span class="sxs-lookup"><span data-stu-id="bae78-141">The MyCloudIT platform reduces deployment time by 95%, Azure cost by 30%, and moves their client's entire IT infrastructure into the cloud in a matter of a few key strokes.</span></span> <span data-ttu-id="bae78-142">Partners kan nu hantera kunder från en global instrumentpanelen, service slutanvändare runtom i världen som aldrig förr och öka intäkterna utan att lägga till ytterligare kostnader eller omfattande Azure utbildning.</span><span class="sxs-lookup"><span data-stu-id="bae78-142">Partners can now manage customers from one global dashboard, service end users around the world like never before, and grow revenues without adding additional overhead or extensive Azure training.</span></span>  

> <span data-ttu-id="bae78-143">Primär plats: Dallas, TX, USA</span><span class="sxs-lookup"><span data-stu-id="bae78-143">Primary location: Dallas, TX, USA</span></span>
> 
> <span data-ttu-id="bae78-144">Åtgärden region: över hela världen</span><span class="sxs-lookup"><span data-stu-id="bae78-144">Operation region: Worldwide</span></span>
> 
> <span data-ttu-id="bae78-145">Samarbeta status: [guld](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)</span><span class="sxs-lookup"><span data-stu-id="bae78-145">Partner status: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)</span></span>
> 
> <span data-ttu-id="bae78-146">Microsoft Cloud-leverantör: Ja</span><span class="sxs-lookup"><span data-stu-id="bae78-146">Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="bae78-147">Sessionsbaserade RemoteApp- och fjärrskrivbordsanslutning lösningar: Ja, både</span><span class="sxs-lookup"><span data-stu-id="bae78-147">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="bae78-148">Lösningar för migrering till Azure RemoteApp: Ja, [Läs mer](https://mycloudit.com/remote-app-microsoft/)</span><span class="sxs-lookup"><span data-stu-id="bae78-148">Azure RemoteApp migration solutions: Yes, [learn more](https://mycloudit.com/remote-app-microsoft/)</span></span>
> 
> <span data-ttu-id="bae78-149">Brian Garoutte, VD för företag-utveckling</span><span class="sxs-lookup"><span data-stu-id="bae78-149">Brian Garoutte, VP of Business Development</span></span>
> 
> <span data-ttu-id="bae78-150">Telefon: 972-218-0741</span><span class="sxs-lookup"><span data-stu-id="bae78-150">Phone: 972-218-0741</span></span>
>   
> <span data-ttu-id="bae78-151">E-post:[brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)</span><span class="sxs-lookup"><span data-stu-id="bae78-151">Email: [brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)</span></span>

### <a name="frame"></a><span data-ttu-id="bae78-152">RAM</span><span class="sxs-lookup"><span data-stu-id="bae78-152">Frame</span></span>

<span data-ttu-id="bae78-153">IT-organisationer i enterprise och myndigheter, hanterade leverantörer och inledande programleverantörer välja ram för att skapa och hantera sina säker, programvarudefinierade arbetsytor i molnet.</span><span class="sxs-lookup"><span data-stu-id="bae78-153">IT organizations in enterprise and government, managed service providers, and leading software vendors choose Frame to create and manage their secure, software-defined workspaces in the cloud.</span></span> <span data-ttu-id="bae78-154">Från små och stora företag, ram enkelt otroligt så att användare kan komma åt Windows-program på en webbläsare från valfri enhet.</span><span class="sxs-lookup"><span data-stu-id="bae78-154">From small to large organizations, Frame makes it incredibly easy to let users access Windows applications on any browser from any device.</span></span> <span data-ttu-id="bae78-155">RAM-plattformen innehåller allt administratör behöver för att distribuera program från molnet, till exempel Azure-infrastrukturen och RDS-licenser (när Azure-konto och licenser är valfri).</span><span class="sxs-lookup"><span data-stu-id="bae78-155">The Frame platform includes everything an admin needs to deploy applications from the cloud including the Azure infrastructure and RDS licenses (bringing your own Azure account and licenses is optional).</span></span> 

<span data-ttu-id="bae78-156">Lär dig mer om [ram på Azure](https://www.fra.me/ara).</span><span class="sxs-lookup"><span data-stu-id="bae78-156">Learn more about [Frame on Azure](https://www.fra.me/ara).</span></span> 

> <span data-ttu-id="bae78-157">Primär plats: San Mateo, CA, USA</span><span class="sxs-lookup"><span data-stu-id="bae78-157">Primary location: San Mateo, CA, USA</span></span>
>
> <span data-ttu-id="bae78-158">Åtgärden region: över hela världen</span><span class="sxs-lookup"><span data-stu-id="bae78-158">Operation region: Worldwide</span></span>
>
> <span data-ttu-id="bae78-159">Microsoft-Partner: Ja</span><span class="sxs-lookup"><span data-stu-id="bae78-159">Microsoft Partner: Yes</span></span>
> 
> <span data-ttu-id="bae78-160">Telefon: 1-480-269-4668</span><span class="sxs-lookup"><span data-stu-id="bae78-160">Phone: 1-480-269-4668</span></span>

### <a name="awingu"></a><span data-ttu-id="bae78-161">Awingu</span><span class="sxs-lookup"><span data-stu-id="bae78-161">Awingu</span></span>
<span data-ttu-id="bae78-162">Awingu innehåller en enkel arbetsyta online-lösning som kör äldre appar, SaaS och dokument från en html5-webbläsare.</span><span class="sxs-lookup"><span data-stu-id="bae78-162">Awingu provides a simple online workspace solution running legacy apps, SaaS and documents from an html5 browser.</span></span> <span data-ttu-id="bae78-163">Därför gör alla program på ett säkert sätt tillgängligt på någon typ av enhet.</span><span class="sxs-lookup"><span data-stu-id="bae78-163">As such, making any applications securely available on any type of device.</span></span> <span data-ttu-id="bae78-164">En mängd op Single-Sign-On-alternativ är tillgänglig för SaaS-tjänster.</span><span class="sxs-lookup"><span data-stu-id="bae78-164">For SaaS services, a wide range op Single-Sign-On options is available.</span></span> <span data-ttu-id="bae78-165">Också kan olika (moln) filer system vara djupt integrerad på din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="bae78-165">Also diverse (cloud) files systems can be deeply integrated into your workspace.</span></span> <span data-ttu-id="bae78-166">Bredvid full mobilitet ger Awingus omfattande online arbetsytan optimala säkerhet med detaljerade kontroller (t.ex. Hämta/överföring), fullständig användning granskning, Multifaktorautentisering (t.ex. Azure MFA), session inspelning och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="bae78-166">Next to full mobility, Awingu's rich online workspace will give optimal security with granular controls (e.g. downloading/uploading), full usage auditing, Multi-Factor Authentication (e.g. Azure MFA), session recording and more.</span></span> <span data-ttu-id="bae78-167">Out-of-the-box, Awingu kan dokumentet och programdelning sessionen för optimerad och säkert samarbete.</span><span class="sxs-lookup"><span data-stu-id="bae78-167">Out-of-the-box, Awingu enables document and application session sharing for optimized and secure collaboration.</span></span>
<span data-ttu-id="bae78-168">Awingus lösning är flera innehavare, flera AD och öppna API.</span><span class="sxs-lookup"><span data-stu-id="bae78-168">Awingu's solution is multi-tenant, multi-AD and open API.</span></span> <span data-ttu-id="bae78-169">Den används av små och stora företag och leverantörer av molntjänster och [ISV: er](http://www.isv2saas.com).</span><span class="sxs-lookup"><span data-stu-id="bae78-169">It is used by small and large businesses, Cloud Service Providers and [ISVs](http://www.isv2saas.com).</span></span> <span data-ttu-id="bae78-170">Dessa kunder uppskattar särskilt enkelt för användning, enkel att installera och TCO.</span><span class="sxs-lookup"><span data-stu-id="bae78-170">These customers especially appreciate the easy-of-use, ease-to-install and low TCO.</span></span>

<span data-ttu-id="bae78-171">Allt-i-ett-Awingu är [tillgängliga i Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/awingu.awingu-arm) med 2 inbyggda samtidiga användare.</span><span class="sxs-lookup"><span data-stu-id="bae78-171">Awingu All-in-One is [available in the Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/awingu.awingu-arm) with 2 built-in concurrent users.</span></span> <span data-ttu-id="bae78-172">Ytterligare licenser är tillgängliga via en [mängd distributörer och återförsäljare](http://www.awingu.com/reseller).</span><span class="sxs-lookup"><span data-stu-id="bae78-172">Additional licenses are available through a [wide range of distributors and resellers](http://www.awingu.com/reseller).</span></span>

<span data-ttu-id="bae78-173">Lär dig mer om [Awingu på som ett alternativ till Azure RemoteApp](http://alternative-for-azure-remoteapp.awingu.com/).</span><span class="sxs-lookup"><span data-stu-id="bae78-173">Learn more about [Awingu on as alternative to Azure RemoteApp](http://alternative-for-azure-remoteapp.awingu.com/).</span></span>


> <span data-ttu-id="bae78-174">Primär plats: Belgien</span><span class="sxs-lookup"><span data-stu-id="bae78-174">Primary location: Belgium</span></span>
> 
> <span data-ttu-id="bae78-175">Operativsystemet regioner: EMEA, Nordamerika och Brasilien</span><span class="sxs-lookup"><span data-stu-id="bae78-175">Operating regions: EMEA, North America and Brazil</span></span>
> 
> <span data-ttu-id="bae78-176">Sessionsbaserade RemoteApp- och fjärrskrivbordsanslutning lösningar: Ja, både</span><span class="sxs-lookup"><span data-stu-id="bae78-176">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span> 
> 
> <span data-ttu-id="bae78-177">**Globala**:</span><span class="sxs-lookup"><span data-stu-id="bae78-177">**Global**:</span></span>
> 
> <span data-ttu-id="bae78-178">Arnaud Marlière, CMO</span><span class="sxs-lookup"><span data-stu-id="bae78-178">Arnaud Marlière, CMO</span></span>
> 
> <span data-ttu-id="bae78-179">E-post:[arnaud@awingu.com](mailto:arnaud@awingu.com)</span><span class="sxs-lookup"><span data-stu-id="bae78-179">Email: [arnaud@awingu.com](mailto:arnaud@awingu.com)</span></span>
> 
> <span data-ttu-id="bae78-180">Telefon: +1 646 583 3025</span><span class="sxs-lookup"><span data-stu-id="bae78-180">Phone: +1 646 583 3025</span></span>
> 
> <span data-ttu-id="bae78-181">**Belgien HQ**:</span><span class="sxs-lookup"><span data-stu-id="bae78-181">**Belgium HQ**:</span></span>
> 
> <span data-ttu-id="bae78-182">Ottergemsesteenweg Zuid 808 B44</span><span class="sxs-lookup"><span data-stu-id="bae78-182">Ottergemsesteenweg-Zuid 808 B44</span></span>
> 
> <span data-ttu-id="bae78-183">9000 Speditörs</span><span class="sxs-lookup"><span data-stu-id="bae78-183">9000 Gent</span></span>
> 
> <span data-ttu-id="bae78-184">E-post:[info@awingu.com](mailto:info@awingu.com)</span><span class="sxs-lookup"><span data-stu-id="bae78-184">Email: [info@awingu.com](mailto:info@awingu.com)</span></span> 
> 
> <span data-ttu-id="bae78-185">Telefon: + 32 9 296 40 11</span><span class="sxs-lookup"><span data-stu-id="bae78-185">Phone: +32 9 296 40 11</span></span>
> 
> <span data-ttu-id="bae78-186">**USA**:</span><span class="sxs-lookup"><span data-stu-id="bae78-186">**USA**:</span></span>
> 
> <span data-ttu-id="bae78-187">7 våning, 1177 Ave av Nord,</span><span class="sxs-lookup"><span data-stu-id="bae78-187">7th floor, 1177 Ave of the Americas,</span></span>
> 
> <span data-ttu-id="bae78-188">New York, NY 10036</span><span class="sxs-lookup"><span data-stu-id="bae78-188">New York, NY 10036</span></span>
> 
> <span data-ttu-id="bae78-189">E-post:[info.us@awingu.com](mailto:info.us@awingu.com)</span><span class="sxs-lookup"><span data-stu-id="bae78-189">Email: [info.us@awingu.com](mailto:info.us@awingu.com)</span></span>

### <a name="microsoft-hosted-service-provider"></a><span data-ttu-id="bae78-190">Microsoft som värd-leverantör</span><span class="sxs-lookup"><span data-stu-id="bae78-190">Microsoft Hosted Service Provider</span></span>
<span data-ttu-id="bae78-191">Värd partner har vanligtvis en helt hanterad värdbaserade Windows-skrivbordet och programtjänsten, vilket kan omfatta hantering av Azure-resurser, operativsystem, program och supportavdelningen med partnern licensavtal med Microsoft och andra programvaruleverantörer tillsammans med som en Tjänstleverantörslicensavtalet att återförsäljning av prenumeranten Access License (SAL).</span><span class="sxs-lookup"><span data-stu-id="bae78-191">Hosting partners typically offer a fully managed hosted Windows desktop and application service, which may include managing the Azure resources, operating systems, applications, and helpdesk using the partner's licensing agreements with Microsoft and other software providers along with being a Service Provider License Agreement to allow reselling of Subscriber Access License (SAL).</span></span> <span data-ttu-id="bae78-192">Följande information innehåller information och kontaktinformation för några av de värdar som specialisering bistå kunder med Azure RemoteApp-migreringen.</span><span class="sxs-lookup"><span data-stu-id="bae78-192">The following information provides details and contact information for some of the hosters that specialize in assisting customers with their Azure RemoteApp migration.</span></span> <span data-ttu-id="bae78-193">Checka ut [den aktuella listan över värdbaserade leverantörer](http://aka.ms/rdsonazurecertified) som slutförts RDS på IaaS sökväg och assessment.</span><span class="sxs-lookup"><span data-stu-id="bae78-193">Check out [the current list of Hosted Service Providers](http://aka.ms/rdsonazurecertified) that have completed the RDS on IaaS learning path and assessment.</span></span>  

### <a name="citrix-service-provider-program"></a><span data-ttu-id="bae78-194">Citrix Service Provider Program</span><span class="sxs-lookup"><span data-stu-id="bae78-194">Citrix Service Provider Program</span></span>
<span data-ttu-id="bae78-195">Citrix Service Provider Program gör det enkelt för leverantörer att leverera enkelhet av virtuella molntjänster till små och medelstora företag, erbjuda dem till de tjänster som de vill använda i en enkel, betalning per användning modell.</span><span class="sxs-lookup"><span data-stu-id="bae78-195">The Citrix Service Provider Program makes it easy for service providers to deliver the simplicity of virtual cloud computing to SMBs, offering them the services they want in an easy, pay-as-you-go model.</span></span> <span data-ttu-id="bae78-196">Citrix-leverantörer utöka sina Microsoft SPLA-företag och expandera deras RDS plattform investeringar med vilken enhet som helst, åtkomst överallt, bredaste programmets support, en innehållsrik, extra säkerhet och ökad skalbarhet.</span><span class="sxs-lookup"><span data-stu-id="bae78-196">Citrix Service Providers grow their Microsoft SPLA businesses and expand their RDS platform investments with any device, anywhere access, the broadest application support, a rich experience, added security and increased scalability.</span></span> <span data-ttu-id="bae78-197">I sin tur Citrix-leverantörer ger dig flera prenumeranter ökar nöjda och minska sina driftskostnader.</span><span class="sxs-lookup"><span data-stu-id="bae78-197">In turn, Citrix Service Providers attract more subscribers, increase customer satisfaction and reduce their operational costs.</span></span> <span data-ttu-id="bae78-198">[Lär dig mer](http://www.citrix.com/products/service-providers.html) eller [hitta en partner](https://www.citrix.com/buy/partnerlocator.html).</span><span class="sxs-lookup"><span data-stu-id="bae78-198">[Learn more](http://www.citrix.com/products/service-providers.html) or [find a partner](https://www.citrix.com/buy/partnerlocator.html).</span></span>

#### <a name="acuutech"></a><span data-ttu-id="bae78-199">Acuutech</span><span class="sxs-lookup"><span data-stu-id="bae78-199">Acuutech</span></span>
<span data-ttu-id="bae78-200">[Acuutech](http://www.acuutech.com) specialiserat sig på att tillhandahålla värdbaserad skrivbord lösningar som ger fullständig skrivbord och program för ISV upplevelser som bygger på Microsoft-teknik till en global klient grundläggande från Azure och egna datacenter.</span><span class="sxs-lookup"><span data-stu-id="bae78-200">[Acuutech](http://www.acuutech.com) specializes in providing hosted desktop solutions, delivering full desktop and ISV applications experiences built on Microsoft technology to a global client base from Azure and their own datacenters.</span></span>

> <span data-ttu-id="bae78-201">Primär plats: London, Storbritannien; Singapore; Houston, TX</span><span class="sxs-lookup"><span data-stu-id="bae78-201">Primary location: London, UK; Singapore; Houston, TX</span></span>
> 
> <span data-ttu-id="bae78-202">Åtgärden region: över hela världen</span><span class="sxs-lookup"><span data-stu-id="bae78-202">Operation region: Worldwide</span></span>
> 
> <span data-ttu-id="bae78-203">Samarbeta status: Guld</span><span class="sxs-lookup"><span data-stu-id="bae78-203">Partner status: Gold</span></span>
> 
> <span data-ttu-id="bae78-204">Microsoft Cloud-leverantör: Ja</span><span class="sxs-lookup"><span data-stu-id="bae78-204">Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="bae78-205">Sessionsbaserade RemoteApp- och fjärrskrivbordsanslutning lösningar: Ja, både</span><span class="sxs-lookup"><span data-stu-id="bae78-205">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="bae78-206">Lösningar för migrering till Azure RemoteApp: Ja, [Läs mer](http://www.acuutech.com/ara-migration/)</span><span class="sxs-lookup"><span data-stu-id="bae78-206">Azure RemoteApp migration solutions: Yes, [learn more](http://www.acuutech.com/ara-migration/)</span></span>
> 
> <span data-ttu-id="bae78-207">**Storbritannien**:</span><span class="sxs-lookup"><span data-stu-id="bae78-207">**United Kingdom**:</span></span>
>   
> <span data-ttu-id="bae78-208">5/6 York House, Langston väg</span><span class="sxs-lookup"><span data-stu-id="bae78-208">5/6 York House, Langston Road,</span></span>
>   
> <span data-ttu-id="bae78-209">Loughton, Essex IG10 3TQ</span><span class="sxs-lookup"><span data-stu-id="bae78-209">Loughton, Essex IG10 3TQ</span></span>
>   
> <span data-ttu-id="bae78-210">Telefon: + 44 (0) 20 8502 2155</span><span class="sxs-lookup"><span data-stu-id="bae78-210">Phone: +44 (0) 20 8502 2155</span></span>
> 
> <span data-ttu-id="bae78-211">**Singapore**:</span><span class="sxs-lookup"><span data-stu-id="bae78-211">**Singapore**:</span></span>
>   
> <span data-ttu-id="bae78-212">100 Cecil gata, #09-02</span><span class="sxs-lookup"><span data-stu-id="bae78-212">100 Cecil Street, #09-02,</span></span> 
>   
> <span data-ttu-id="bae78-213">Globalt Singapore 069532</span><span class="sxs-lookup"><span data-stu-id="bae78-213">The Globe, Singapore 069532</span></span>
> 
> <span data-ttu-id="bae78-214">Telefon: + 65 6709 4933</span><span class="sxs-lookup"><span data-stu-id="bae78-214">Phone: +65 6709 4933</span></span>
>   
> <span data-ttu-id="bae78-215">**Nordamerika**:</span><span class="sxs-lookup"><span data-stu-id="bae78-215">**North America**:</span></span>
>   
> <span data-ttu-id="bae78-216">3601 S. Sandman St.</span><span class="sxs-lookup"><span data-stu-id="bae78-216">3601 S. Sandman St.</span></span>
>   
> <span data-ttu-id="bae78-217">Suite 200, Houston, TX 77098</span><span class="sxs-lookup"><span data-stu-id="bae78-217">Suite 200, Houston, TX 77098</span></span>
>   
> <span data-ttu-id="bae78-218">Telefon: +1 713 691 0800</span><span class="sxs-lookup"><span data-stu-id="bae78-218">Phone: +1 713 691 0800</span></span>

#### <a name="aspex"></a><span data-ttu-id="bae78-219">ASPEX</span><span class="sxs-lookup"><span data-stu-id="bae78-219">ASPEX</span></span>
<span data-ttu-id="bae78-220">[ASPEX](http://www.aspex.be/en) specialiserat sig på ISV övergå till molnet och ISV-vill optimera deras aktuella inställningar för molnet.</span><span class="sxs-lookup"><span data-stu-id="bae78-220">[ASPEX](http://www.aspex.be/en) specializes in ISVs transitioning to the Cloud and ISV‘ looking to optimize their current cloud setups.</span></span> <span data-ttu-id="bae78-221">ASPEX erbjuder en mängd olika hanterade tjänster, devops och rådgivning.</span><span class="sxs-lookup"><span data-stu-id="bae78-221">ASPEX offers a wide range of managed services, devops, and consulting services.</span></span>  

> <span data-ttu-id="bae78-222">Primär plats: Antwerpen, Belgien</span><span class="sxs-lookup"><span data-stu-id="bae78-222">Primary location: Antwerp, Belgium</span></span>
> 
> <span data-ttu-id="bae78-223">Åtgärden region: västra Europa</span><span class="sxs-lookup"><span data-stu-id="bae78-223">Operation region: Western Europe</span></span>
> 
> <span data-ttu-id="bae78-224">Samarbeta status: [Silver](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)</span><span class="sxs-lookup"><span data-stu-id="bae78-224">Partner status: [Silver](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)</span></span>
> 
> <span data-ttu-id="bae78-225">Microsoft Cloud-leverantör: Ja</span><span class="sxs-lookup"><span data-stu-id="bae78-225">Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="bae78-226">Sessionsbaserade RemoteApp- och fjärrskrivbordsanslutning lösningar: Ja, både</span><span class="sxs-lookup"><span data-stu-id="bae78-226">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="bae78-227">Lösningar för migrering till Azure RemoteApp: Ja, [Läs mer](https://www.aspex.be/en/azure-remote-apps)</span><span class="sxs-lookup"><span data-stu-id="bae78-227">Azure RemoteApp migration solutions: Yes, [learn more](https://www.aspex.be/en/azure-remote-apps)</span></span>
> 
> <span data-ttu-id="bae78-228">Telefon: +3232202198</span><span class="sxs-lookup"><span data-stu-id="bae78-228">Phone: +3232202198</span></span>
> 
> <span data-ttu-id="bae78-229">E-post:[info@aspex.be](mailto:info@aspex.be)</span><span class="sxs-lookup"><span data-stu-id="bae78-229">Mail: [info@aspex.be](mailto:info@aspex.be)</span></span>
> 
> <span data-ttu-id="bae78-230">Web: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)</span><span class="sxs-lookup"><span data-stu-id="bae78-230">Web: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)</span></span>

#### <a name="caasecom"></a><span data-ttu-id="bae78-231">Caase.com</span><span class="sxs-lookup"><span data-stu-id="bae78-231">Caase.com</span></span>
<span data-ttu-id="bae78-232">[Caase.com](http://www.caase.com/) hjälper företag, lokala myndigheter, icke-statliga organ och sjukvården institutioner med transporten mot ett effektivare sätt för arbete i Microsoft Cloud.</span><span class="sxs-lookup"><span data-stu-id="bae78-232">[Caase.com](http://www.caase.com/) helps businesses, local governments, non-governmental bodies and healthcare institutions with their journey towards a smarter way of work in the Microsoft Cloud.</span></span> <span data-ttu-id="bae78-233">Ha produktiva och säker var som helst, med vilken enhet som helst och låg IT-kostnader.</span><span class="sxs-lookup"><span data-stu-id="bae78-233">Being productive and secure at any place, with any device and at low IT cost.</span></span> <span data-ttu-id="bae78-234">Caase.com är true specialist för Microsoft Office365, Azure, Enterprise Mobility och säkerhet och Windows.</span><span class="sxs-lookup"><span data-stu-id="bae78-234">Caase.com is a true specialist for Microsoft Office365, Azure, Enterprise Mobility and Security and Windows.</span></span> <span data-ttu-id="bae78-235">Med våra konsulter, migreringstjänster, införande program, träning skapar hantering och support Caase.com du en optimerad och säker plattform för samarbete för kunders anställda, partners och leverantörer.</span><span class="sxs-lookup"><span data-stu-id="bae78-235">With our consultancy, migration services, adoption programs, training, management and support Caase.com creates an optimized and secure platform for collaboration for both customers’ employees, partners and suppliers.</span></span>
<span data-ttu-id="bae78-236">Caase.com är mastermind arbetsytan Azure Remote (mobila arbetsyta) och digitala arbetsplatsen (sociala intranät).</span><span class="sxs-lookup"><span data-stu-id="bae78-236">Caase.com is the mastermind of the Azure Remote Workspace (mobile workplace) and the Digital Workplace (Social Intranet).</span></span> <span data-ttu-id="bae78-237">Båda lösningarna – åstadkommas införs – är grunden som användarna av dessa lösningar har mest angenäm, lyckad och effektiv upplevelsen i sina vägen till Microsoft Cloud.</span><span class="sxs-lookup"><span data-stu-id="bae78-237">Both solutions – accomplished with adoption – are the foundation which ensures that the users of these solutions have the most pleasant, successful and effective experience in their route to the Microsoft Cloud.</span></span>
<span data-ttu-id="bae78-238">Nederländska översättning ánd en stödjande film över här: http://caase.com/over-ons/</span><span class="sxs-lookup"><span data-stu-id="bae78-238">Dutch translation ánd a supporting movie over here: http://caase.com/over-ons/</span></span>

> <span data-ttu-id="bae78-239">Åtgärden region: nederländska baserat globalt omfattande</span><span class="sxs-lookup"><span data-stu-id="bae78-239">Operation region: Dutch based, global reach</span></span>
> 
> <span data-ttu-id="bae78-240">Samarbeta status: [guld](https://partnercenter.microsoft.com/pcv/solution-providers/caasecom_4295593260/51159_3)</span><span class="sxs-lookup"><span data-stu-id="bae78-240">Partner status: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/caasecom_4295593260/51159_3)</span></span>
> 
> <span data-ttu-id="bae78-241">Microsoft Cloud-leverantör: Ja</span><span class="sxs-lookup"><span data-stu-id="bae78-241">Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="bae78-242">Sessionsbaserade RemoteApp- och fjärrskrivbordsanslutning lösningar: Ja, både</span><span class="sxs-lookup"><span data-stu-id="bae78-242">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="bae78-243">Lösningar för migrering till Azure RemoteApp: Ja, [mer](http://caase.com/diensten/microsoft-azure/).</span><span class="sxs-lookup"><span data-stu-id="bae78-243">Azure RemoteApp migration solutions: Yes, [learn more](http://caase.com/diensten/microsoft-azure/).</span></span>
> 
> 
> <span data-ttu-id="bae78-244">Nederländerna:</span><span class="sxs-lookup"><span data-stu-id="bae78-244">Netherlands:</span></span>
> 
> <span data-ttu-id="bae78-245">Rigtersbleek Zandvoort 10 (Tyskland Spinnerij)</span><span class="sxs-lookup"><span data-stu-id="bae78-245">Rigtersbleek-Zandvoort 10 (De Spinnerij)</span></span>
> 
> <span data-ttu-id="bae78-246">7521 BE, Enschede</span><span class="sxs-lookup"><span data-stu-id="bae78-246">7521 BE, Enschede</span></span>
> 
> <span data-ttu-id="bae78-247">Telefon: +31 (0) 88 4320 000</span><span class="sxs-lookup"><span data-stu-id="bae78-247">Phone: +31 (0) 88 4320 000</span></span>


#### <a name="nerdio"></a><span data-ttu-id="bae78-248">Nerdio</span><span class="sxs-lookup"><span data-stu-id="bae78-248">Nerdio</span></span>
<span data-ttu-id="bae78-249">[Nerdio för Azure](http://getnerdio.com/nfa/) är en IT-automatiseringsplattform som levererar löjligt enkelt etablering, hantering och optimering av fullständig IT-miljöer i Microsoft-molnet.</span><span class="sxs-lookup"><span data-stu-id="bae78-249">[Nerdio for Azure](http://getnerdio.com/nfa/) is an IT automation platform that delivers ridiculously simple provisioning, management and optimization of complete IT environments in the Microsoft cloud.</span></span> <span data-ttu-id="bae78-250">Klara av virtuella skrivbord, fjärrprogram och servrar i ett par timmar.</span><span class="sxs-lookup"><span data-stu-id="bae78-250">Stand up virtual desktops, remote apps and servers in a couple of hours.</span></span> <span data-ttu-id="bae78-251">Administrera miljön i tre gånger eller mindre med Nerdio Admin Portal.</span><span class="sxs-lookup"><span data-stu-id="bae78-251">Administer the environment in three clicks or less with Nerdio Admin Portal.</span></span> <span data-ttu-id="bae78-252">Använd intelligent automatisk skalning och spara 40 till 60% i Azure IaaS-resurser.</span><span class="sxs-lookup"><span data-stu-id="bae78-252">Use intelligent auto-scaling and save 40 to 60% in Azure IaaS resources.</span></span>

> <span data-ttu-id="bae78-253">Primär plats: Chicago, IL åtgärden region: Worldwide Partner status: [guld](https://partnercenter.microsoft.com/en-us/pcv/solution-providers/adar-inc_341c9afa-f12c-46f5-8f7b-3f9ef59a66a5/3a7ae479-3ac2-42f6-84e2-d456dc7424e1) Microsoft Molntjänstleverantör: Ja</span><span class="sxs-lookup"><span data-stu-id="bae78-253">Primary location: Chicago, IL Operation region: Worldwide Partner status: [Gold](https://partnercenter.microsoft.com/en-us/pcv/solution-providers/adar-inc_341c9afa-f12c-46f5-8f7b-3f9ef59a66a5/3a7ae479-3ac2-42f6-84e2-d456dc7424e1) Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="bae78-254">Sessionsbaserade RemoteApp- och fjärrskrivbordsanslutning lösningar: Ja, både</span><span class="sxs-lookup"><span data-stu-id="bae78-254">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="bae78-255">Lösningar för migrering till Azure RemoteApp: Ja</span><span class="sxs-lookup"><span data-stu-id="bae78-255">Azure RemoteApp migration solutions: Yes</span></span>
> 
> 
> <span data-ttu-id="bae78-256">8001 Lincoln Ave</span><span class="sxs-lookup"><span data-stu-id="bae78-256">8001 Lincoln Ave</span></span>
> 
> <span data-ttu-id="bae78-257">Suite 212</span><span class="sxs-lookup"><span data-stu-id="bae78-257">Suite 212</span></span>
> 
> <span data-ttu-id="bae78-258">Skokie, IL 60077</span><span class="sxs-lookup"><span data-stu-id="bae78-258">Skokie, IL 60077</span></span>
> 
> <span data-ttu-id="bae78-259">USA</span><span class="sxs-lookup"><span data-stu-id="bae78-259">USA</span></span>
> 
> <span data-ttu-id="bae78-260">(844) 4NERDIO externt</span><span class="sxs-lookup"><span data-stu-id="bae78-260">(844) 4NERDIO ext.</span></span> <span data-ttu-id="bae78-261">6</span><span class="sxs-lookup"><span data-stu-id="bae78-261">6</span></span>
> 
> [sayhello@getnerdio.com](mailto:sayhello@getnerdio.com)

#### <a name="saasplaza"></a><span data-ttu-id="bae78-262">**SaaSplaza**</span><span class="sxs-lookup"><span data-stu-id="bae78-262">**SaaSplaza**</span></span>
<span data-ttu-id="bae78-263">[SaaSplaza](http://www.saasplaza.com/) erbjuder fullständigt Microsoft Dynamics portfölj (NAV, AX, GP, SL, CRM) privata och offentliga moln (Azure).</span><span class="sxs-lookup"><span data-stu-id="bae78-263">[SaaSplaza](http://www.saasplaza.com/) offers complete Microsoft Dynamics portfolio (NAV, AX, GP, SL, CRM) private and public cloud (Azure).</span></span>

> <span data-ttu-id="bae78-264">Primär plats: Nederländerna</span><span class="sxs-lookup"><span data-stu-id="bae78-264">Primary location: Netherlands</span></span>
> 
> <span data-ttu-id="bae78-265">Åtgärden Region: över hela världen</span><span class="sxs-lookup"><span data-stu-id="bae78-265">Operation Region: Worldwide</span></span>
> 
> <span data-ttu-id="bae78-266">Samarbeta status: [guld](https://partnercenter.microsoft.com/pcv/solution-providers/saasplaza_4295495801/791011_2?k=saasplaza)</span><span class="sxs-lookup"><span data-stu-id="bae78-266">Partner status: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/saasplaza_4295495801/791011_2?k=saasplaza)</span></span>
> 
> <span data-ttu-id="bae78-267">Microsoft Cloud-leverantör: Ja</span><span class="sxs-lookup"><span data-stu-id="bae78-267">Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="bae78-268">Sessionsbaserade RemoteApp- och fjärrskrivbordsanslutning lösningar: Ja, både</span><span class="sxs-lookup"><span data-stu-id="bae78-268">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="bae78-269">**EMEA**:</span><span class="sxs-lookup"><span data-stu-id="bae78-269">**EMEA**:</span></span>
> 
> <span data-ttu-id="bae78-270">Prins Mauritslaan 29 35</span><span class="sxs-lookup"><span data-stu-id="bae78-270">Prins Mauritslaan 29-35</span></span>
> 
> <span data-ttu-id="bae78-271">71 LP Badhoevedorp</span><span class="sxs-lookup"><span data-stu-id="bae78-271">71 LP Badhoevedorp</span></span>
> 
> <span data-ttu-id="bae78-272">Nederländerna</span><span class="sxs-lookup"><span data-stu-id="bae78-272">The Netherlands</span></span>
> 
> <span data-ttu-id="bae78-273">Telefon: +31 20 547 8060</span><span class="sxs-lookup"><span data-stu-id="bae78-273">Phone: +31 20 547 8060</span></span> 
> 
>  <span data-ttu-id="bae78-274">**Americas**:</span><span class="sxs-lookup"><span data-stu-id="bae78-274">**Americas**:</span></span>
> 
> <span data-ttu-id="bae78-275">171 NIEDERSACHSEN väg, Suite 105</span><span class="sxs-lookup"><span data-stu-id="bae78-275">171 Saxony Road, Suite 105</span></span>
> 
> <span data-ttu-id="bae78-276">Encinitas, CA 92024</span><span class="sxs-lookup"><span data-stu-id="bae78-276">Encinitas, CA 92024</span></span>
> 
> <span data-ttu-id="bae78-277">San Diego</span><span class="sxs-lookup"><span data-stu-id="bae78-277">San Diego</span></span>
> 
> <span data-ttu-id="bae78-278">USA</span><span class="sxs-lookup"><span data-stu-id="bae78-278">United States</span></span>
> 
> <span data-ttu-id="bae78-279">Telefon: +1 858 385 8900</span><span class="sxs-lookup"><span data-stu-id="bae78-279">Phone: +1 858 385 8900</span></span> 
> 
> <span data-ttu-id="bae78-280">**APAC**:</span><span class="sxs-lookup"><span data-stu-id="bae78-280">**APAC**:</span></span>
> 
> <span data-ttu-id="bae78-281">105 Cecil gata</span><span class="sxs-lookup"><span data-stu-id="bae78-281">105 Cecil Street</span></span>
>    
> <span data-ttu-id="bae78-282">\#11-08, Åttahörning</span><span class="sxs-lookup"><span data-stu-id="bae78-282">\#11-08, The Octagon</span></span>
> 
> <span data-ttu-id="bae78-283">Singapore 069534</span><span class="sxs-lookup"><span data-stu-id="bae78-283">Singapore 069534</span></span>
> 
> <span data-ttu-id="bae78-284">Singapore</span><span class="sxs-lookup"><span data-stu-id="bae78-284">Singapore</span></span>
>   
> <span data-ttu-id="bae78-285">Telefon - Singapore: + 65 6222 6591</span><span class="sxs-lookup"><span data-stu-id="bae78-285">Phone - Singapore: +65 6222 6591</span></span>
> 
> <span data-ttu-id="bae78-286">Telefon - Australien: +61 2 8310 5568</span><span class="sxs-lookup"><span data-stu-id="bae78-286">Phone - Australia: +61 2 8310 5568</span></span> 
>    
> <span data-ttu-id="bae78-287">Telefon - Nya Zeeland: +64 4 488 0321</span><span class="sxs-lookup"><span data-stu-id="bae78-287">Phone - New Zealand: +64 4 488 0321</span></span>
> 
## <a name="need-more-help"></a><span data-ttu-id="bae78-288">Behöver du mer hjälp?</span><span class="sxs-lookup"><span data-stu-id="bae78-288">Need more help?</span></span>
<span data-ttu-id="bae78-289">Fortfarande behöver hjälp att välja eller har fler frågor?</span><span class="sxs-lookup"><span data-stu-id="bae78-289">Still need help choosing or have further questions?</span></span> <span data-ttu-id="bae78-290">Använd någon av följande metoder för att få hjälp.</span><span class="sxs-lookup"><span data-stu-id="bae78-290">Use one of the following methods to get help.</span></span> 

1. <span data-ttu-id="bae78-291">Skicka e-post på [ arainfo@microsoft.com ](mailto:arainfo@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="bae78-291">Email us at [arainfo@microsoft.com](mailto:arainfo@microsoft.com).</span></span>
2. <span data-ttu-id="bae78-292">Kontakta [Azure-supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="bae78-292">Contact [Azure support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span> <span data-ttu-id="bae78-293">Starta genom att öppna en [Azure supportärende](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="bae78-293">Start by opening an [Azure support case](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span>
3. <span data-ttu-id="bae78-294">Ring oss.</span><span class="sxs-lookup"><span data-stu-id="bae78-294">Call us.</span></span> <span data-ttu-id="bae78-295">[Hitta ett lokalt försäljning nummer](https://azure.microsoft.com/overview/sales-number/).</span><span class="sxs-lookup"><span data-stu-id="bae78-295">[Find a local sales number](https://azure.microsoft.com/overview/sales-number/).</span></span>

