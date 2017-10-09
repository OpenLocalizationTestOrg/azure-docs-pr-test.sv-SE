---
title: "aaaOptions för att migrera utanför Azure RemoteApp | Microsoft Docs"
description: "Läs mer om hello alternativ för att migrera utanför Azure RemoteApp."
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
ms.openlocfilehash: 75324597881520d0c75939983b728ae9bbd7f436
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="options-for-migrating-out-of-azure-remoteapp"></a><span data-ttu-id="f4a00-103">Alternativ för att migrera utanför Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="f4a00-103">Options for migrating out of Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f4a00-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="f4a00-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="f4a00-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="f4a00-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>


<span data-ttu-id="f4a00-106">Om du har slutat använda Azure RemoteApp på grund av hello [pensionering meddelande](https://go.microsoft.com/fwlink/?linkid=821148) eller eftersom du är klar med utvärderingen, måste toomigrate ut från Azure RemoteApp tooanother app service.</span><span class="sxs-lookup"><span data-stu-id="f4a00-106">If you have stopped using Azure RemoteApp because of hello [retirement announcement](https://go.microsoft.com/fwlink/?linkid=821148) or because you've finished your evaluation, you need toomigrate off of Azure RemoteApp tooanother app service.</span></span> <span data-ttu-id="f4a00-107">Det finns två olika metoder för att migrera: en självhanterad (kallas ofta infrastruktur som en tjänst [IaaS]) distribution eller en helt hanterad (ofta kallade plattform som en tjänst) eller programvara som en tjänst [PaaS/SaaS] erbjudande.</span><span class="sxs-lookup"><span data-stu-id="f4a00-107">There are two different approaches for migrating: a self-managed (often called Infrastructure as a Service [IaaS]) deployment or a fully managed (often called Platform as a Service or Software as a Service [PaaS/SaaS]) offering.</span></span> 

<span data-ttu-id="f4a00-108">Självbetjäning IaaS är ett själv distribution som är hanterade drivs och ägs av du distribuerats direkt på virtuella datorer (VM) eller fysiska system.</span><span class="sxs-lookup"><span data-stu-id="f4a00-108">Self-service IaaS is a do-it-yourself deployment that is managed, operated, and owned by you, directly deployed on virtual machines (VMs) or physical systems.</span></span> <span data-ttu-id="f4a00-109">Avsluta en helt hanterad PaaS/SaaS erbjudande är mer som Azure RemoteApp - partner som tillhandahåller en tjänstnivå ovanpå en lösning för fjärrkommunikation som hanterar operativa hello andra och underhåll medan du, som hello kund göra vissa avbildningen och apphantering.</span><span class="sxs-lookup"><span data-stu-id="f4a00-109">At hello other end, a fully managed PaaS/SaaS offering is more like Azure RemoteApp - a partner provides a service layer on top of a remoting solution that handles operational and servicing, while you, as hello customer, do some image and app management.</span></span>

<span data-ttu-id="f4a00-110">[Visa hello Azure RemoteApp Webbseminarier på migreringsalternativ](https://social.msdn.microsoft.com/Forums/azure/40557aaa-3e9f-403c-b221-ad3eac10dc56/migration-option-webinar-recordings?forum=AzureRemoteApp), eller Läs på för mer information (inklusive exempel på olika webbhotell hello).</span><span class="sxs-lookup"><span data-stu-id="f4a00-110">[View hello Azure RemoteApp webinars on migration options](https://social.msdn.microsoft.com/Forums/azure/40557aaa-3e9f-403c-b221-ad3eac10dc56/migration-option-webinar-recordings?forum=AzureRemoteApp), or read on for more information (including examples of hello different hosting options).</span></span>

## <a name="self-managed-iaas-solutions"></a><span data-ttu-id="f4a00-111">Fristående (IaaS) lösningar</span><span class="sxs-lookup"><span data-stu-id="f4a00-111">Self-managed (IaaS) solutions</span></span>
### <a name="rds-on-iaas"></a><span data-ttu-id="f4a00-112">**RDS på IaaS**</span><span class="sxs-lookup"><span data-stu-id="f4a00-112">**RDS on IaaS**</span></span>
<span data-ttu-id="f4a00-113">Du kan distribuera en intern sessionsbaserade Fjärrskrivbordstjänster (i Windows Server) distribution med RemoteApp eller skrivbord lokalt eller i en värdmiljö (t.ex. på virtuella Azure-datorer).</span><span class="sxs-lookup"><span data-stu-id="f4a00-113">You can deploy a native session-based Remote Desktop Services (in Windows Server) deployment using either RemoteApp or desktops on-premises or in a hosted environment (like on Azure VMs).</span></span> <span data-ttu-id="f4a00-114">RDS på IaaS-distributioner är bäst för kunder som redan är bekant med och som har befintliga teknisk expertis med RDS-distribution.</span><span class="sxs-lookup"><span data-stu-id="f4a00-114">RDS on IaaS deployments are best for customers already familiar with and that have existing technical expertise with RDS deployments.</span></span> 

> [!NOTE]
> <span data-ttu-id="f4a00-115">Du behöver volymlicensiering med Software Assurance (SA) för Fjärrskrivbordstjänster klienten åtkomst licenser toouse det här distributionsalternativet.</span><span class="sxs-lookup"><span data-stu-id="f4a00-115">You need Volume Licensing with Software Assurance (SA) for RDS client access licenses toouse this deployment option.</span></span>

<span data-ttu-id="f4a00-116">Distribution av Fjärrskrivbordstjänster på Azure Virtual Machines är enklare än någonsin när du använder distributions- och korrigering mallar (läsa ett [översikt](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/) och sedan [hämta dem](https://aka.ms/rdautomation)).</span><span class="sxs-lookup"><span data-stu-id="f4a00-116">Deploying RDS on Azure VMs is easier than ever when you use deployment and patching templates (read an [overview](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/) and then [go get them](https://aka.ms/rdautomation)).</span></span> <span data-ttu-id="f4a00-117">Du kan hämta hello samma funktioner för elastiska skalning med Azure klassiska modellen distributionsresurser (inte Resursmodell för Azure-resurser) i Azure RemoteApp med hello [automatisk skalning skriptet](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76), även om det finns flera anpassningar och konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="f4a00-117">You can get hello same elastic scaling capabilities with Azure classic deployment model resources (not Azure Resource Model resources) within Azure RemoteApp by using hello [auto scaling script](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76), although there are more customizations and configurations.</span></span> <span data-ttu-id="f4a00-118">När du distribuerar RDS på Azure Virtual Machines support tillhandahålls via [Azure stöder](https://azure.microsoft.com/support/plans/), hello samma supportpersonal som stöds av du med Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="f4a00-118">When you deploy RDS on Azure VMs, support is provided through [Azure Support](https://azure.microsoft.com/support/plans/), hello same support professionals that supported you with Azure RemoteApp.</span></span> <span data-ttu-id="f4a00-119">Du kan hämta cost beräknar baserat på din användning av befintlig genom att kontakta [Azure-supporten](https://azure.microsoft.com/support/plans/), eller du kan utföra beräkningar själv med hjälp av en snart toobe publicerat kostnaden Kalkylatorn.</span><span class="sxs-lookup"><span data-stu-id="f4a00-119">You can get cost estimates based on your existing usage by contacting [Azure Support](https://azure.microsoft.com/support/plans/), or you can perform calculations yourself using a soon toobe released Cost Calculator.</span></span>  <span data-ttu-id="f4a00-120">Dessutom N-serien virtuella datorer (för tillfället i privat förhandsvisning) kan du lägga till vGPU - höra mer om att lägga till vGPU och hur för[utnyttjar Fjärrskrivbordstjänster förbättringar i Windows Server 2016](https://myignite.microsoft.com/videos/2794) i vår Ignite-sessionen.</span><span class="sxs-lookup"><span data-stu-id="f4a00-120">Also, with N-series VMs (currently in private preview) you can add vGPU - hear more about adding vGPU and about how too[harness RDS improvements in Windows Server 2016](https://myignite.microsoft.com/videos/2794) in our Ignite session.</span></span>   

<span data-ttu-id="f4a00-121">Vi har steg för steg distributionsguider för [Windows Server 2012 R2](http://aka.ms/rdsonazure) och [Windows Server 2016](http://aka.ms/rdsonazure2016) tooassist med din distribution.</span><span class="sxs-lookup"><span data-stu-id="f4a00-121">We have step by step deployment guides for [Windows Server 2012 R2](http://aka.ms/rdsonazure) and [Windows Server 2016](http://aka.ms/rdsonazure2016) tooassist with your deployment.</span></span> <span data-ttu-id="f4a00-122">Kolla in hello [fjärrskrivbord blogg](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) för hello nyheter.</span><span class="sxs-lookup"><span data-stu-id="f4a00-122">Check out hello [Remote Desktop blog](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) for hello latest news.</span></span>

### <a name="citrix-on-iaas"></a><span data-ttu-id="f4a00-123">**Citrix på IaaS**</span><span class="sxs-lookup"><span data-stu-id="f4a00-123">**Citrix on IaaS**</span></span>
<span data-ttu-id="f4a00-124">En intern Citrix sessionsbaserade XenApp eller XenDesktop kan vara distribueras lokalt eller i en värdmiljö (som på Azure Virtual Machines).</span><span class="sxs-lookup"><span data-stu-id="f4a00-124">A native Citrix deployment of session-based XenApp or XenDesktop can be deployed on-premises or within a hosted environment (such as on Azure VMs).</span></span> 

<span data-ttu-id="f4a00-125">Kolla hello stegvisa Distributionsguide [Citrix XA 7.6 på Azure](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx), mer information.</span><span class="sxs-lookup"><span data-stu-id="f4a00-125">Check out hello step-by-step deployment guide, [Citrix XA 7.6 on Azure](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx), for more information.</span></span> <span data-ttu-id="f4a00-126">Läs mer om [Citrix på Azure](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), inklusive ett pris Kalkylatorn.</span><span class="sxs-lookup"><span data-stu-id="f4a00-126">Read more about [Citrix on Azure](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), including a price calculator.</span></span> <span data-ttu-id="f4a00-127">Du kan också hitta en [Citrix Kontakta](http://citrix.com/English/contact/index.asp) toodiscuss alternativ med.</span><span class="sxs-lookup"><span data-stu-id="f4a00-127">You can also find a [Citrix contact](http://citrix.com/English/contact/index.asp) toodiscuss your options with.</span></span>

## <a name="fully-managed-paassaas-offerings"></a><span data-ttu-id="f4a00-128">Helt hanterad PaaS/SaaS ()-erbjudanden</span><span class="sxs-lookup"><span data-stu-id="f4a00-128">Fully managed (PaaS/SaaS) offerings</span></span>

### <a name="citrix-xenapp-essentials-released-april-2017"></a><span data-ttu-id="f4a00-129">Citrix XenApp Essentials (utgiven April 2017)</span><span class="sxs-lookup"><span data-stu-id="f4a00-129">Citrix XenApp Essentials (released April 2017)</span></span>
<span data-ttu-id="f4a00-130">Nu tillgängligt på hello [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Citrix.XenAppEssentials), Citrix XenApp Essentials är hello nya program virtualiseringstjänsten, kombinera hello power och flexibilitet hello Citrix molnplattform med hello enkel, normativ, och enkelt att använda visionen för Microsoft Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="f4a00-130">Available now on hello [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Citrix.XenAppEssentials), Citrix XenApp Essentials is hello new application virtualization service, combining hello power and flexibility of hello Citrix Cloud platform with hello simple, prescriptive, and easy-to-consume vision of Microsoft Azure RemoteApp.</span></span> 

<span data-ttu-id="f4a00-131">Befintliga Azure RemoteApp-kunder kan [registrera dig för en kostnadsfri utvärderingsversion](https://www.citrix.com/products/citrix-cloud/form/xenapp-essentials-msft-trial/).</span><span class="sxs-lookup"><span data-stu-id="f4a00-131">Existing Azure RemoteApp customers can [register for a free trial](https://www.citrix.com/products/citrix-cloud/form/xenapp-essentials-msft-trial/).</span></span>  <span data-ttu-id="f4a00-132">Obs: Endast Citrix användaren avgift är ledig gäller Azure kostnader för beräkning och lagring</span><span class="sxs-lookup"><span data-stu-id="f4a00-132">Note: Only Citrix user service charge is free, Azure compute and storage costs apply</span></span>

<span data-ttu-id="f4a00-133">Lära sig mer:</span><span class="sxs-lookup"><span data-stu-id="f4a00-133">Learn More:</span></span>
- [<span data-ttu-id="f4a00-134">Migrera från Azure RemoteApp tooCitrix XenApp Essentials</span><span class="sxs-lookup"><span data-stu-id="f4a00-134">Migrate from Azure RemoteApp tooCitrix XenApp Essentials</span></span>](remoteapp-migrate-citrix.md)
- [<span data-ttu-id="f4a00-135">Citrix och Microsoft</span><span class="sxs-lookup"><span data-stu-id="f4a00-135">Citrix and Microsoft</span></span>](https://www.citrix.com/global-partners/microsoft/remote-app.html)
- <span data-ttu-id="f4a00-136">[Citrix XenApp Essentials presentation](https://www.youtube.com/watch?v=91Z7CCfQ-9k).</span><span class="sxs-lookup"><span data-stu-id="f4a00-136">[Citrix XenApp Essentials presentation](https://www.youtube.com/watch?v=91Z7CCfQ-9k).</span></span>  

### <a name="citrix-cloud-xenapp-service-and-xendesktop-service"></a><span data-ttu-id="f4a00-137">Citrix XenApp Molntjänsten och XenDesktop Service</span><span class="sxs-lookup"><span data-stu-id="f4a00-137">Citrix Cloud XenApp Service and XenDesktop Service</span></span> 

<span data-ttu-id="f4a00-138">[Citrix XenApp Molntjänsten och XenDesktop Service](https://www.citrix.com/products/citrix-cloud/services.html) är hello bästa lösningen för hello leverans av både appar och stationära datorer, plus avancerad hantering och övervakning av funktioner.</span><span class="sxs-lookup"><span data-stu-id="f4a00-138">[Citrix Cloud XenApp Service and XenDesktop Service](https://www.citrix.com/products/citrix-cloud/services.html) is hello best solution for hello delivery of both apps and desktops, plus advanced management and monitoring capabilities.</span></span> 

#### <a name="conexlink-platform-name-mycloudit"></a><span data-ttu-id="f4a00-139">Conexlink (plattformsnamnet: MyCloudIT)</span><span class="sxs-lookup"><span data-stu-id="f4a00-139">Conexlink (Platform name: MyCloudIT)</span></span>
<span data-ttu-id="f4a00-140">[MyCloudIT](https://mycloudit.com) är en automatiseringsplattform för IT-företag toosimplify, optimera och skala hello migrering och leverans av fjärrskrivbord fjärrprogram och infrastrukturen i hello Microsoft Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="f4a00-140">[MyCloudIT](https://mycloudit.com) is an automation platform for IT companies toosimplify, optimize, and scale hello migration and delivery of remote desktops, remote applications, and infrastructure in hello Microsoft Azure Cloud.</span></span> 

<span data-ttu-id="f4a00-141">Hej MyCloudIT plattform minskar tidpunkten för distribution av 95%, Azure kostnaden med 30% och flyttar deras klient hela IT-infrastruktur till molnet hello i en fråga om några viktiga linjer.</span><span class="sxs-lookup"><span data-stu-id="f4a00-141">hello MyCloudIT platform reduces deployment time by 95%, Azure cost by 30%, and moves their client's entire IT infrastructure into hello cloud in a matter of a few key strokes.</span></span> <span data-ttu-id="f4a00-142">Partners kan nu hantera kunder från en global instrumentpanelen, service slutanvändare runt hello world som aldrig förr och öka intäkterna utan att lägga till ytterligare kostnader eller omfattande Azure utbildning.</span><span class="sxs-lookup"><span data-stu-id="f4a00-142">Partners can now manage customers from one global dashboard, service end users around hello world like never before, and grow revenues without adding additional overhead or extensive Azure training.</span></span>  

> <span data-ttu-id="f4a00-143">Primär plats: Dallas, TX, USA</span><span class="sxs-lookup"><span data-stu-id="f4a00-143">Primary location: Dallas, TX, USA</span></span>
> 
> <span data-ttu-id="f4a00-144">Åtgärden region: över hela världen</span><span class="sxs-lookup"><span data-stu-id="f4a00-144">Operation region: Worldwide</span></span>
> 
> <span data-ttu-id="f4a00-145">Samarbeta status: [guld](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)</span><span class="sxs-lookup"><span data-stu-id="f4a00-145">Partner status: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)</span></span>
> 
> <span data-ttu-id="f4a00-146">Microsoft Cloud-leverantör: Ja</span><span class="sxs-lookup"><span data-stu-id="f4a00-146">Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="f4a00-147">Sessionsbaserade RemoteApp- och fjärrskrivbordsanslutning lösningar: Ja, både</span><span class="sxs-lookup"><span data-stu-id="f4a00-147">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="f4a00-148">Lösningar för migrering till Azure RemoteApp: Ja, [Läs mer](https://mycloudit.com/remote-app-microsoft/)</span><span class="sxs-lookup"><span data-stu-id="f4a00-148">Azure RemoteApp migration solutions: Yes, [learn more](https://mycloudit.com/remote-app-microsoft/)</span></span>
> 
> <span data-ttu-id="f4a00-149">Brian Garoutte, VD för företag-utveckling</span><span class="sxs-lookup"><span data-stu-id="f4a00-149">Brian Garoutte, VP of Business Development</span></span>
> 
> <span data-ttu-id="f4a00-150">Telefon: 972-218-0741</span><span class="sxs-lookup"><span data-stu-id="f4a00-150">Phone: 972-218-0741</span></span>
>   
> <span data-ttu-id="f4a00-151">E-post:[brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)</span><span class="sxs-lookup"><span data-stu-id="f4a00-151">Email: [brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)</span></span>

### <a name="frame"></a><span data-ttu-id="f4a00-152">RAM</span><span class="sxs-lookup"><span data-stu-id="f4a00-152">Frame</span></span>

<span data-ttu-id="f4a00-153">IT-organisationer i enterprise och myndigheter, hanterade leverantörer och inledande programleverantörer Välj ram toocreate och hantera sina säker, programvarudefinierade arbetsytor i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="f4a00-153">IT organizations in enterprise and government, managed service providers, and leading software vendors choose Frame toocreate and manage their secure, software-defined workspaces in hello cloud.</span></span> <span data-ttu-id="f4a00-154">Från små toolarge organisationer ram gör det otroligt enkelt toolet användare åtkomst till Windows-program på en webbläsare från valfri enhet.</span><span class="sxs-lookup"><span data-stu-id="f4a00-154">From small toolarge organizations, Frame makes it incredibly easy toolet users access Windows applications on any browser from any device.</span></span> <span data-ttu-id="f4a00-155">hello ram plattformen innehåller allt en administratör måste toodeploy program från hello molnet, till exempel hello Azure-infrastrukturen och RDS-licenser (när Azure-konto och licenser är valfri).</span><span class="sxs-lookup"><span data-stu-id="f4a00-155">hello Frame platform includes everything an admin needs toodeploy applications from hello cloud including hello Azure infrastructure and RDS licenses (bringing your own Azure account and licenses is optional).</span></span> 

<span data-ttu-id="f4a00-156">Lär dig mer om [ram på Azure](https://www.fra.me/ara).</span><span class="sxs-lookup"><span data-stu-id="f4a00-156">Learn more about [Frame on Azure](https://www.fra.me/ara).</span></span> 

> <span data-ttu-id="f4a00-157">Primär plats: San Mateo, CA, USA</span><span class="sxs-lookup"><span data-stu-id="f4a00-157">Primary location: San Mateo, CA, USA</span></span>
>
> <span data-ttu-id="f4a00-158">Åtgärden region: över hela världen</span><span class="sxs-lookup"><span data-stu-id="f4a00-158">Operation region: Worldwide</span></span>
>
> <span data-ttu-id="f4a00-159">Microsoft-Partner: Ja</span><span class="sxs-lookup"><span data-stu-id="f4a00-159">Microsoft Partner: Yes</span></span>
> 
> <span data-ttu-id="f4a00-160">Telefon: 1-480-269-4668</span><span class="sxs-lookup"><span data-stu-id="f4a00-160">Phone: 1-480-269-4668</span></span>

### <a name="awingu"></a><span data-ttu-id="f4a00-161">Awingu</span><span class="sxs-lookup"><span data-stu-id="f4a00-161">Awingu</span></span>
<span data-ttu-id="f4a00-162">Awingu innehåller en enkel arbetsyta online-lösning som kör äldre appar, SaaS och dokument från en html5-webbläsare.</span><span class="sxs-lookup"><span data-stu-id="f4a00-162">Awingu provides a simple online workspace solution running legacy apps, SaaS and documents from an html5 browser.</span></span> <span data-ttu-id="f4a00-163">Därför gör alla program på ett säkert sätt tillgängligt på någon typ av enhet.</span><span class="sxs-lookup"><span data-stu-id="f4a00-163">As such, making any applications securely available on any type of device.</span></span> <span data-ttu-id="f4a00-164">En mängd op Single-Sign-On-alternativ är tillgänglig för SaaS-tjänster.</span><span class="sxs-lookup"><span data-stu-id="f4a00-164">For SaaS services, a wide range op Single-Sign-On options is available.</span></span> <span data-ttu-id="f4a00-165">Också kan olika (moln) filer system vara djupt integrerad på din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="f4a00-165">Also diverse (cloud) files systems can be deeply integrated into your workspace.</span></span> <span data-ttu-id="f4a00-166">Nästa toofull mobility Awingus omfattande online arbetsytan ger optimala säkerhet med detaljerade kontroller (t.ex. Hämta/överföring), fullständig användning granskning, Multifaktorautentisering (t.ex. Azure MFA), session inspelning och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="f4a00-166">Next toofull mobility, Awingu's rich online workspace will give optimal security with granular controls (e.g. downloading/uploading), full usage auditing, Multi-Factor Authentication (e.g. Azure MFA), session recording and more.</span></span> <span data-ttu-id="f4a00-167">Out-of-the-box, Awingu kan dokumentet och programdelning sessionen för optimerad och säkert samarbete.</span><span class="sxs-lookup"><span data-stu-id="f4a00-167">Out-of-the-box, Awingu enables document and application session sharing for optimized and secure collaboration.</span></span>
<span data-ttu-id="f4a00-168">Awingus lösning är flera innehavare, flera AD och öppna API.</span><span class="sxs-lookup"><span data-stu-id="f4a00-168">Awingu's solution is multi-tenant, multi-AD and open API.</span></span> <span data-ttu-id="f4a00-169">Den används av små och stora företag och leverantörer av molntjänster och [ISV: er](http://www.isv2saas.com).</span><span class="sxs-lookup"><span data-stu-id="f4a00-169">It is used by small and large businesses, Cloud Service Providers and [ISVs](http://www.isv2saas.com).</span></span> <span data-ttu-id="f4a00-170">Dessa kunder uppskattar särskilt hello enkelt att använda, enkel att installera och låg TCO.</span><span class="sxs-lookup"><span data-stu-id="f4a00-170">These customers especially appreciate hello easy-of-use, ease-to-install and low TCO.</span></span>

<span data-ttu-id="f4a00-171">Allt-i-ett-Awingu är [tillgängliga i hello Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/awingu.awingu-arm) med 2 inbyggda samtidiga användare.</span><span class="sxs-lookup"><span data-stu-id="f4a00-171">Awingu All-in-One is [available in hello Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/awingu.awingu-arm) with 2 built-in concurrent users.</span></span> <span data-ttu-id="f4a00-172">Ytterligare licenser är tillgängliga via en [mängd distributörer och återförsäljare](http://www.awingu.com/reseller).</span><span class="sxs-lookup"><span data-stu-id="f4a00-172">Additional licenses are available through a [wide range of distributors and resellers](http://www.awingu.com/reseller).</span></span>

<span data-ttu-id="f4a00-173">Lär dig mer om [Awingu på som alternativa tooAzure RemoteApp](http://alternative-for-azure-remoteapp.awingu.com/).</span><span class="sxs-lookup"><span data-stu-id="f4a00-173">Learn more about [Awingu on as alternative tooAzure RemoteApp](http://alternative-for-azure-remoteapp.awingu.com/).</span></span>


> <span data-ttu-id="f4a00-174">Primär plats: Belgien</span><span class="sxs-lookup"><span data-stu-id="f4a00-174">Primary location: Belgium</span></span>
> 
> <span data-ttu-id="f4a00-175">Operativsystemet regioner: EMEA, Nordamerika och Brasilien</span><span class="sxs-lookup"><span data-stu-id="f4a00-175">Operating regions: EMEA, North America and Brazil</span></span>
> 
> <span data-ttu-id="f4a00-176">Sessionsbaserade RemoteApp- och fjärrskrivbordsanslutning lösningar: Ja, både</span><span class="sxs-lookup"><span data-stu-id="f4a00-176">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span> 
> 
> <span data-ttu-id="f4a00-177">**Globala**:</span><span class="sxs-lookup"><span data-stu-id="f4a00-177">**Global**:</span></span>
> 
> <span data-ttu-id="f4a00-178">Arnaud Marlière, CMO</span><span class="sxs-lookup"><span data-stu-id="f4a00-178">Arnaud Marlière, CMO</span></span>
> 
> <span data-ttu-id="f4a00-179">E-post:[arnaud@awingu.com](mailto:arnaud@awingu.com)</span><span class="sxs-lookup"><span data-stu-id="f4a00-179">Email: [arnaud@awingu.com](mailto:arnaud@awingu.com)</span></span>
> 
> <span data-ttu-id="f4a00-180">Telefon: +1 646 583 3025</span><span class="sxs-lookup"><span data-stu-id="f4a00-180">Phone: +1 646 583 3025</span></span>
> 
> <span data-ttu-id="f4a00-181">**Belgien HQ**:</span><span class="sxs-lookup"><span data-stu-id="f4a00-181">**Belgium HQ**:</span></span>
> 
> <span data-ttu-id="f4a00-182">Ottergemsesteenweg Zuid 808 B44</span><span class="sxs-lookup"><span data-stu-id="f4a00-182">Ottergemsesteenweg-Zuid 808 B44</span></span>
> 
> <span data-ttu-id="f4a00-183">9000 Speditörs</span><span class="sxs-lookup"><span data-stu-id="f4a00-183">9000 Gent</span></span>
> 
> <span data-ttu-id="f4a00-184">E-post:[info@awingu.com](mailto:info@awingu.com)</span><span class="sxs-lookup"><span data-stu-id="f4a00-184">Email: [info@awingu.com](mailto:info@awingu.com)</span></span> 
> 
> <span data-ttu-id="f4a00-185">Telefon: + 32 9 296 40 11</span><span class="sxs-lookup"><span data-stu-id="f4a00-185">Phone: +32 9 296 40 11</span></span>
> 
> <span data-ttu-id="f4a00-186">**USA**:</span><span class="sxs-lookup"><span data-stu-id="f4a00-186">**USA**:</span></span>
> 
> <span data-ttu-id="f4a00-187">7 våning, 1177 Ave av hello Nord,</span><span class="sxs-lookup"><span data-stu-id="f4a00-187">7th floor, 1177 Ave of hello Americas,</span></span>
> 
> <span data-ttu-id="f4a00-188">New York, NY 10036</span><span class="sxs-lookup"><span data-stu-id="f4a00-188">New York, NY 10036</span></span>
> 
> <span data-ttu-id="f4a00-189">E-post:[info.us@awingu.com](mailto:info.us@awingu.com)</span><span class="sxs-lookup"><span data-stu-id="f4a00-189">Email: [info.us@awingu.com](mailto:info.us@awingu.com)</span></span>

### <a name="microsoft-hosted-service-provider"></a><span data-ttu-id="f4a00-190">Microsoft som värd-leverantör</span><span class="sxs-lookup"><span data-stu-id="f4a00-190">Microsoft Hosted Service Provider</span></span>
<span data-ttu-id="f4a00-191">Värd partner har vanligtvis en helt hanterad värd för Windows-skrivbordet och programtjänsten, vilket kan omfatta hantering hello Azure-resurser, operativsystem, program och supportavdelningen med hello partner licensavtal med Microsoft och andra programvaruleverantörer tillsammans med som en Tjänstleverantörslicensavtalet tooallow återförsäljning av prenumeranten Access License (SAL).</span><span class="sxs-lookup"><span data-stu-id="f4a00-191">Hosting partners typically offer a fully managed hosted Windows desktop and application service, which may include managing hello Azure resources, operating systems, applications, and helpdesk using hello partner's licensing agreements with Microsoft and other software providers along with being a Service Provider License Agreement tooallow reselling of Subscriber Access License (SAL).</span></span> <span data-ttu-id="f4a00-192">hello innehåller följande information information och kontaktinformation för några av hello-värdar som specialisering bistå kunder med Azure RemoteApp-migreringen.</span><span class="sxs-lookup"><span data-stu-id="f4a00-192">hello following information provides details and contact information for some of hello hosters that specialize in assisting customers with their Azure RemoteApp migration.</span></span> <span data-ttu-id="f4a00-193">Checka ut [hello aktuell lista över värdbaserade leverantörer](http://aka.ms/rdsonazurecertified) som slutförts hello RDS på IaaS sökväg och assessment.</span><span class="sxs-lookup"><span data-stu-id="f4a00-193">Check out [hello current list of Hosted Service Providers](http://aka.ms/rdsonazurecertified) that have completed hello RDS on IaaS learning path and assessment.</span></span>  

### <a name="citrix-service-provider-program"></a><span data-ttu-id="f4a00-194">Citrix Service Provider Program</span><span class="sxs-lookup"><span data-stu-id="f4a00-194">Citrix Service Provider Program</span></span>
<span data-ttu-id="f4a00-195">hello Citrix Service Provider Program är det enkelt för service providers toodeliver hello enkelhet virtuella cloud computing tooSMBs, erbjuda dem hello-tjänster som de vill använda i en enkel, betalning per användning modell.</span><span class="sxs-lookup"><span data-stu-id="f4a00-195">hello Citrix Service Provider Program makes it easy for service providers toodeliver hello simplicity of virtual cloud computing tooSMBs, offering them hello services they want in an easy, pay-as-you-go model.</span></span> <span data-ttu-id="f4a00-196">Citrix-leverantörer utöka sina Microsoft SPLA-företag och expandera deras RDS plattform investeringar med vilken enhet som helst, åtkomst överallt, hello bredaste programstöd, en innehållsrik, extra säkerhet och ökad skalbarhet.</span><span class="sxs-lookup"><span data-stu-id="f4a00-196">Citrix Service Providers grow their Microsoft SPLA businesses and expand their RDS platform investments with any device, anywhere access, hello broadest application support, a rich experience, added security and increased scalability.</span></span> <span data-ttu-id="f4a00-197">I sin tur Citrix-leverantörer ger dig flera prenumeranter ökar nöjda och minska sina driftskostnader.</span><span class="sxs-lookup"><span data-stu-id="f4a00-197">In turn, Citrix Service Providers attract more subscribers, increase customer satisfaction and reduce their operational costs.</span></span> <span data-ttu-id="f4a00-198">[Lär dig mer](http://www.citrix.com/products/service-providers.html) eller [hitta en partner](https://www.citrix.com/buy/partnerlocator.html).</span><span class="sxs-lookup"><span data-stu-id="f4a00-198">[Learn more](http://www.citrix.com/products/service-providers.html) or [find a partner](https://www.citrix.com/buy/partnerlocator.html).</span></span>

#### <a name="acuutech"></a><span data-ttu-id="f4a00-199">Acuutech</span><span class="sxs-lookup"><span data-stu-id="f4a00-199">Acuutech</span></span>
<span data-ttu-id="f4a00-200">[Acuutech](http://www.acuutech.com) specialiserat sig på att tillhandahålla värdbaserad skrivbord lösningar som ger fullständig skrivbord och program för ISV upplevelser som bygger på Microsoft-teknik tooa globala client grundläggande från Azure och egna datacenter.</span><span class="sxs-lookup"><span data-stu-id="f4a00-200">[Acuutech](http://www.acuutech.com) specializes in providing hosted desktop solutions, delivering full desktop and ISV applications experiences built on Microsoft technology tooa global client base from Azure and their own datacenters.</span></span>

> <span data-ttu-id="f4a00-201">Primär plats: London, Storbritannien; Singapore; Houston, TX</span><span class="sxs-lookup"><span data-stu-id="f4a00-201">Primary location: London, UK; Singapore; Houston, TX</span></span>
> 
> <span data-ttu-id="f4a00-202">Åtgärden region: över hela världen</span><span class="sxs-lookup"><span data-stu-id="f4a00-202">Operation region: Worldwide</span></span>
> 
> <span data-ttu-id="f4a00-203">Samarbeta status: Guld</span><span class="sxs-lookup"><span data-stu-id="f4a00-203">Partner status: Gold</span></span>
> 
> <span data-ttu-id="f4a00-204">Microsoft Cloud-leverantör: Ja</span><span class="sxs-lookup"><span data-stu-id="f4a00-204">Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="f4a00-205">Sessionsbaserade RemoteApp- och fjärrskrivbordsanslutning lösningar: Ja, både</span><span class="sxs-lookup"><span data-stu-id="f4a00-205">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="f4a00-206">Lösningar för migrering till Azure RemoteApp: Ja, [Läs mer](http://www.acuutech.com/ara-migration/)</span><span class="sxs-lookup"><span data-stu-id="f4a00-206">Azure RemoteApp migration solutions: Yes, [learn more](http://www.acuutech.com/ara-migration/)</span></span>
> 
> <span data-ttu-id="f4a00-207">**Storbritannien**:</span><span class="sxs-lookup"><span data-stu-id="f4a00-207">**United Kingdom**:</span></span>
>   
> <span data-ttu-id="f4a00-208">5/6 York House, Langston väg</span><span class="sxs-lookup"><span data-stu-id="f4a00-208">5/6 York House, Langston Road,</span></span>
>   
> <span data-ttu-id="f4a00-209">Loughton, Essex IG10 3TQ</span><span class="sxs-lookup"><span data-stu-id="f4a00-209">Loughton, Essex IG10 3TQ</span></span>
>   
> <span data-ttu-id="f4a00-210">Telefon: + 44 (0) 20 8502 2155</span><span class="sxs-lookup"><span data-stu-id="f4a00-210">Phone: +44 (0) 20 8502 2155</span></span>
> 
> <span data-ttu-id="f4a00-211">**Singapore**:</span><span class="sxs-lookup"><span data-stu-id="f4a00-211">**Singapore**:</span></span>
>   
> <span data-ttu-id="f4a00-212">100 Cecil gata, #09-02</span><span class="sxs-lookup"><span data-stu-id="f4a00-212">100 Cecil Street, #09-02,</span></span> 
>   
> <span data-ttu-id="f4a00-213">Hej världen, Singapore 069532</span><span class="sxs-lookup"><span data-stu-id="f4a00-213">hello Globe, Singapore 069532</span></span>
> 
> <span data-ttu-id="f4a00-214">Telefon: + 65 6709 4933</span><span class="sxs-lookup"><span data-stu-id="f4a00-214">Phone: +65 6709 4933</span></span>
>   
> <span data-ttu-id="f4a00-215">**Nordamerika**:</span><span class="sxs-lookup"><span data-stu-id="f4a00-215">**North America**:</span></span>
>   
> <span data-ttu-id="f4a00-216">3601 S. Sandman St.</span><span class="sxs-lookup"><span data-stu-id="f4a00-216">3601 S. Sandman St.</span></span>
>   
> <span data-ttu-id="f4a00-217">Suite 200, Houston, TX 77098</span><span class="sxs-lookup"><span data-stu-id="f4a00-217">Suite 200, Houston, TX 77098</span></span>
>   
> <span data-ttu-id="f4a00-218">Telefon: +1 713 691 0800</span><span class="sxs-lookup"><span data-stu-id="f4a00-218">Phone: +1 713 691 0800</span></span>

#### <a name="aspex"></a><span data-ttu-id="f4a00-219">ASPEX</span><span class="sxs-lookup"><span data-stu-id="f4a00-219">ASPEX</span></span>
<span data-ttu-id="f4a00-220">[ASPEX](http://www.aspex.be/en) specialiserat sig på ISV övergång toohello molnet och ISV-söker toooptimize deras aktuella inställningar för molnet.</span><span class="sxs-lookup"><span data-stu-id="f4a00-220">[ASPEX](http://www.aspex.be/en) specializes in ISVs transitioning toohello Cloud and ISV‘ looking toooptimize their current cloud setups.</span></span> <span data-ttu-id="f4a00-221">ASPEX erbjuder en mängd olika hanterade tjänster, devops och rådgivning.</span><span class="sxs-lookup"><span data-stu-id="f4a00-221">ASPEX offers a wide range of managed services, devops, and consulting services.</span></span>  

> <span data-ttu-id="f4a00-222">Primär plats: Antwerpen, Belgien</span><span class="sxs-lookup"><span data-stu-id="f4a00-222">Primary location: Antwerp, Belgium</span></span>
> 
> <span data-ttu-id="f4a00-223">Åtgärden region: västra Europa</span><span class="sxs-lookup"><span data-stu-id="f4a00-223">Operation region: Western Europe</span></span>
> 
> <span data-ttu-id="f4a00-224">Samarbeta status: [Silver](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)</span><span class="sxs-lookup"><span data-stu-id="f4a00-224">Partner status: [Silver](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)</span></span>
> 
> <span data-ttu-id="f4a00-225">Microsoft Cloud-leverantör: Ja</span><span class="sxs-lookup"><span data-stu-id="f4a00-225">Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="f4a00-226">Sessionsbaserade RemoteApp- och fjärrskrivbordsanslutning lösningar: Ja, både</span><span class="sxs-lookup"><span data-stu-id="f4a00-226">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="f4a00-227">Lösningar för migrering till Azure RemoteApp: Ja, [Läs mer](https://www.aspex.be/en/azure-remote-apps)</span><span class="sxs-lookup"><span data-stu-id="f4a00-227">Azure RemoteApp migration solutions: Yes, [learn more](https://www.aspex.be/en/azure-remote-apps)</span></span>
> 
> <span data-ttu-id="f4a00-228">Telefon: +3232202198</span><span class="sxs-lookup"><span data-stu-id="f4a00-228">Phone: +3232202198</span></span>
> 
> <span data-ttu-id="f4a00-229">E-post:[info@aspex.be](mailto:info@aspex.be)</span><span class="sxs-lookup"><span data-stu-id="f4a00-229">Mail: [info@aspex.be](mailto:info@aspex.be)</span></span>
> 
> <span data-ttu-id="f4a00-230">Web: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)</span><span class="sxs-lookup"><span data-stu-id="f4a00-230">Web: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)</span></span>

#### <a name="caasecom"></a><span data-ttu-id="f4a00-231">Caase.com</span><span class="sxs-lookup"><span data-stu-id="f4a00-231">Caase.com</span></span>
<span data-ttu-id="f4a00-232">[Caase.com](http://www.caase.com/) hjälper företag, lokala myndigheter, icke-statliga organ och sjukvården institutioner med transporten mot ett effektivare sätt för pågående hello Microsoft Cloud.</span><span class="sxs-lookup"><span data-stu-id="f4a00-232">[Caase.com](http://www.caase.com/) helps businesses, local governments, non-governmental bodies and healthcare institutions with their journey towards a smarter way of work in hello Microsoft Cloud.</span></span> <span data-ttu-id="f4a00-233">Ha produktiva och säker var som helst, med vilken enhet som helst och låg IT-kostnader.</span><span class="sxs-lookup"><span data-stu-id="f4a00-233">Being productive and secure at any place, with any device and at low IT cost.</span></span> <span data-ttu-id="f4a00-234">Caase.com är true specialist för Microsoft Office365, Azure, Enterprise Mobility och säkerhet och Windows.</span><span class="sxs-lookup"><span data-stu-id="f4a00-234">Caase.com is a true specialist for Microsoft Office365, Azure, Enterprise Mobility and Security and Windows.</span></span> <span data-ttu-id="f4a00-235">Med våra konsulter, migreringstjänster, införande program, träning skapar hantering och support Caase.com du en optimerad och säker plattform för samarbete för kunders anställda, partners och leverantörer.</span><span class="sxs-lookup"><span data-stu-id="f4a00-235">With our consultancy, migration services, adoption programs, training, management and support Caase.com creates an optimized and secure platform for collaboration for both customers’ employees, partners and suppliers.</span></span>
<span data-ttu-id="f4a00-236">Caase.com är hello mastermind hello Azure Remote arbetsytan (mobila arbetsyta) och hello digitala arbetsplats (sociala intranät).</span><span class="sxs-lookup"><span data-stu-id="f4a00-236">Caase.com is hello mastermind of hello Azure Remote Workspace (mobile workplace) and hello Digital Workplace (Social Intranet).</span></span> <span data-ttu-id="f4a00-237">Båda lösningarna – åstadkommas införs – är hello foundation, vilket garanterar att hello användare av dessa lösningar har hello så angenäm, lyckad och effektivt erfarenhet i sina väg toohello Microsoft Cloud.</span><span class="sxs-lookup"><span data-stu-id="f4a00-237">Both solutions – accomplished with adoption – are hello foundation which ensures that hello users of these solutions have hello most pleasant, successful and effective experience in their route toohello Microsoft Cloud.</span></span>
<span data-ttu-id="f4a00-238">Nederländska översättning ánd en stödjande film över här: http://caase.com/over-ons/</span><span class="sxs-lookup"><span data-stu-id="f4a00-238">Dutch translation ánd a supporting movie over here: http://caase.com/over-ons/</span></span>

> <span data-ttu-id="f4a00-239">Åtgärden region: nederländska baserat globalt omfattande</span><span class="sxs-lookup"><span data-stu-id="f4a00-239">Operation region: Dutch based, global reach</span></span>
> 
> <span data-ttu-id="f4a00-240">Samarbeta status: [guld](https://partnercenter.microsoft.com/pcv/solution-providers/caasecom_4295593260/51159_3)</span><span class="sxs-lookup"><span data-stu-id="f4a00-240">Partner status: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/caasecom_4295593260/51159_3)</span></span>
> 
> <span data-ttu-id="f4a00-241">Microsoft Cloud-leverantör: Ja</span><span class="sxs-lookup"><span data-stu-id="f4a00-241">Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="f4a00-242">Sessionsbaserade RemoteApp- och fjärrskrivbordsanslutning lösningar: Ja, både</span><span class="sxs-lookup"><span data-stu-id="f4a00-242">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="f4a00-243">Lösningar för migrering till Azure RemoteApp: Ja, [mer](http://caase.com/diensten/microsoft-azure/).</span><span class="sxs-lookup"><span data-stu-id="f4a00-243">Azure RemoteApp migration solutions: Yes, [learn more](http://caase.com/diensten/microsoft-azure/).</span></span>
> 
> 
> <span data-ttu-id="f4a00-244">Nederländerna:</span><span class="sxs-lookup"><span data-stu-id="f4a00-244">Netherlands:</span></span>
> 
> <span data-ttu-id="f4a00-245">Rigtersbleek Zandvoort 10 (Tyskland Spinnerij)</span><span class="sxs-lookup"><span data-stu-id="f4a00-245">Rigtersbleek-Zandvoort 10 (De Spinnerij)</span></span>
> 
> <span data-ttu-id="f4a00-246">7521 BE, Enschede</span><span class="sxs-lookup"><span data-stu-id="f4a00-246">7521 BE, Enschede</span></span>
> 
> <span data-ttu-id="f4a00-247">Telefon: +31 (0) 88 4320 000</span><span class="sxs-lookup"><span data-stu-id="f4a00-247">Phone: +31 (0) 88 4320 000</span></span>


#### <a name="nerdio"></a><span data-ttu-id="f4a00-248">Nerdio</span><span class="sxs-lookup"><span data-stu-id="f4a00-248">Nerdio</span></span>
<span data-ttu-id="f4a00-249">[Nerdio för Azure](http://getnerdio.com/nfa/) är en plattform för automatisering av IT som levererar löjligt enkelt etablering, hantering och optimering av fullständig IT-miljöer i hello Microsoft cloud.</span><span class="sxs-lookup"><span data-stu-id="f4a00-249">[Nerdio for Azure](http://getnerdio.com/nfa/) is an IT automation platform that delivers ridiculously simple provisioning, management and optimization of complete IT environments in hello Microsoft cloud.</span></span> <span data-ttu-id="f4a00-250">Klara av virtuella skrivbord, fjärrprogram och servrar i ett par timmar.</span><span class="sxs-lookup"><span data-stu-id="f4a00-250">Stand up virtual desktops, remote apps and servers in a couple of hours.</span></span> <span data-ttu-id="f4a00-251">Administrera hello-miljö i tre gånger eller mindre med Nerdio Admin Portal.</span><span class="sxs-lookup"><span data-stu-id="f4a00-251">Administer hello environment in three clicks or less with Nerdio Admin Portal.</span></span> <span data-ttu-id="f4a00-252">Använd intelligent automatisk skalning och spara 40 too60% i Azure IaaS-resurser.</span><span class="sxs-lookup"><span data-stu-id="f4a00-252">Use intelligent auto-scaling and save 40 too60% in Azure IaaS resources.</span></span>

> <span data-ttu-id="f4a00-253">Primär plats: Chicago, IL åtgärden region: Worldwide Partner status: [guld](https://partnercenter.microsoft.com/en-us/pcv/solution-providers/adar-inc_341c9afa-f12c-46f5-8f7b-3f9ef59a66a5/3a7ae479-3ac2-42f6-84e2-d456dc7424e1) Microsoft Molntjänstleverantör: Ja</span><span class="sxs-lookup"><span data-stu-id="f4a00-253">Primary location: Chicago, IL Operation region: Worldwide Partner status: [Gold](https://partnercenter.microsoft.com/en-us/pcv/solution-providers/adar-inc_341c9afa-f12c-46f5-8f7b-3f9ef59a66a5/3a7ae479-3ac2-42f6-84e2-d456dc7424e1) Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="f4a00-254">Sessionsbaserade RemoteApp- och fjärrskrivbordsanslutning lösningar: Ja, både</span><span class="sxs-lookup"><span data-stu-id="f4a00-254">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="f4a00-255">Lösningar för migrering till Azure RemoteApp: Ja</span><span class="sxs-lookup"><span data-stu-id="f4a00-255">Azure RemoteApp migration solutions: Yes</span></span>
> 
> 
> <span data-ttu-id="f4a00-256">8001 Lincoln Ave</span><span class="sxs-lookup"><span data-stu-id="f4a00-256">8001 Lincoln Ave</span></span>
> 
> <span data-ttu-id="f4a00-257">Suite 212</span><span class="sxs-lookup"><span data-stu-id="f4a00-257">Suite 212</span></span>
> 
> <span data-ttu-id="f4a00-258">Skokie, IL 60077</span><span class="sxs-lookup"><span data-stu-id="f4a00-258">Skokie, IL 60077</span></span>
> 
> <span data-ttu-id="f4a00-259">USA</span><span class="sxs-lookup"><span data-stu-id="f4a00-259">USA</span></span>
> 
> <span data-ttu-id="f4a00-260">(844) 4NERDIO externt 6</span><span class="sxs-lookup"><span data-stu-id="f4a00-260">(844) 4NERDIO ext. 6</span></span>
> 
> [sayhello@getnerdio.com](mailto:sayhello@getnerdio.com)

#### <a name="saasplaza"></a><span data-ttu-id="f4a00-261">**SaaSplaza**</span><span class="sxs-lookup"><span data-stu-id="f4a00-261">**SaaSplaza**</span></span>
<span data-ttu-id="f4a00-262">[SaaSplaza](http://www.saasplaza.com/) erbjuder fullständigt Microsoft Dynamics portfölj (NAV, AX, GP, SL, CRM) privata och offentliga moln (Azure).</span><span class="sxs-lookup"><span data-stu-id="f4a00-262">[SaaSplaza](http://www.saasplaza.com/) offers complete Microsoft Dynamics portfolio (NAV, AX, GP, SL, CRM) private and public cloud (Azure).</span></span>

> <span data-ttu-id="f4a00-263">Primär plats: Nederländerna</span><span class="sxs-lookup"><span data-stu-id="f4a00-263">Primary location: Netherlands</span></span>
> 
> <span data-ttu-id="f4a00-264">Åtgärden Region: över hela världen</span><span class="sxs-lookup"><span data-stu-id="f4a00-264">Operation Region: Worldwide</span></span>
> 
> <span data-ttu-id="f4a00-265">Samarbeta status: [guld](https://partnercenter.microsoft.com/pcv/solution-providers/saasplaza_4295495801/791011_2?k=saasplaza)</span><span class="sxs-lookup"><span data-stu-id="f4a00-265">Partner status: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/saasplaza_4295495801/791011_2?k=saasplaza)</span></span>
> 
> <span data-ttu-id="f4a00-266">Microsoft Cloud-leverantör: Ja</span><span class="sxs-lookup"><span data-stu-id="f4a00-266">Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="f4a00-267">Sessionsbaserade RemoteApp- och fjärrskrivbordsanslutning lösningar: Ja, både</span><span class="sxs-lookup"><span data-stu-id="f4a00-267">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="f4a00-268">**EMEA**:</span><span class="sxs-lookup"><span data-stu-id="f4a00-268">**EMEA**:</span></span>
> 
> <span data-ttu-id="f4a00-269">Prins Mauritslaan 29 35</span><span class="sxs-lookup"><span data-stu-id="f4a00-269">Prins Mauritslaan 29-35</span></span>
> 
> <span data-ttu-id="f4a00-270">71 LP Badhoevedorp</span><span class="sxs-lookup"><span data-stu-id="f4a00-270">71 LP Badhoevedorp</span></span>
> 
> <span data-ttu-id="f4a00-271">hello Nederländerna</span><span class="sxs-lookup"><span data-stu-id="f4a00-271">hello Netherlands</span></span>
> 
> <span data-ttu-id="f4a00-272">Telefon: +31 20 547 8060</span><span class="sxs-lookup"><span data-stu-id="f4a00-272">Phone: +31 20 547 8060</span></span> 
> 
>  <span data-ttu-id="f4a00-273">**Americas**:</span><span class="sxs-lookup"><span data-stu-id="f4a00-273">**Americas**:</span></span>
> 
> <span data-ttu-id="f4a00-274">171 NIEDERSACHSEN väg, Suite 105</span><span class="sxs-lookup"><span data-stu-id="f4a00-274">171 Saxony Road, Suite 105</span></span>
> 
> <span data-ttu-id="f4a00-275">Encinitas, CA 92024</span><span class="sxs-lookup"><span data-stu-id="f4a00-275">Encinitas, CA 92024</span></span>
> 
> <span data-ttu-id="f4a00-276">San Diego</span><span class="sxs-lookup"><span data-stu-id="f4a00-276">San Diego</span></span>
> 
> <span data-ttu-id="f4a00-277">USA</span><span class="sxs-lookup"><span data-stu-id="f4a00-277">United States</span></span>
> 
> <span data-ttu-id="f4a00-278">Telefon: +1 858 385 8900</span><span class="sxs-lookup"><span data-stu-id="f4a00-278">Phone: +1 858 385 8900</span></span> 
> 
> <span data-ttu-id="f4a00-279">**APAC**:</span><span class="sxs-lookup"><span data-stu-id="f4a00-279">**APAC**:</span></span>
> 
> <span data-ttu-id="f4a00-280">105 Cecil gata</span><span class="sxs-lookup"><span data-stu-id="f4a00-280">105 Cecil Street</span></span>
>    
> <span data-ttu-id="f4a00-281">\#11-08 hello Åttahörning</span><span class="sxs-lookup"><span data-stu-id="f4a00-281">\#11-08, hello Octagon</span></span>
> 
> <span data-ttu-id="f4a00-282">Singapore 069534</span><span class="sxs-lookup"><span data-stu-id="f4a00-282">Singapore 069534</span></span>
> 
> <span data-ttu-id="f4a00-283">Singapore</span><span class="sxs-lookup"><span data-stu-id="f4a00-283">Singapore</span></span>
>   
> <span data-ttu-id="f4a00-284">Telefon - Singapore: + 65 6222 6591</span><span class="sxs-lookup"><span data-stu-id="f4a00-284">Phone - Singapore: +65 6222 6591</span></span>
> 
> <span data-ttu-id="f4a00-285">Telefon - Australien: +61 2 8310 5568</span><span class="sxs-lookup"><span data-stu-id="f4a00-285">Phone - Australia: +61 2 8310 5568</span></span> 
>    
> <span data-ttu-id="f4a00-286">Telefon - Nya Zeeland: +64 4 488 0321</span><span class="sxs-lookup"><span data-stu-id="f4a00-286">Phone - New Zealand: +64 4 488 0321</span></span>
> 
## <a name="need-more-help"></a><span data-ttu-id="f4a00-287">Behöver du mer hjälp?</span><span class="sxs-lookup"><span data-stu-id="f4a00-287">Need more help?</span></span>
<span data-ttu-id="f4a00-288">Fortfarande behöver hjälp att välja eller har fler frågor?</span><span class="sxs-lookup"><span data-stu-id="f4a00-288">Still need help choosing or have further questions?</span></span> <span data-ttu-id="f4a00-289">Använd någon av följande metoder tooget hjälp hello.</span><span class="sxs-lookup"><span data-stu-id="f4a00-289">Use one of hello following methods tooget help.</span></span> 

1. <span data-ttu-id="f4a00-290">Skicka e-post på [ arainfo@microsoft.com ](mailto:arainfo@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="f4a00-290">Email us at [arainfo@microsoft.com](mailto:arainfo@microsoft.com).</span></span>
2. <span data-ttu-id="f4a00-291">Kontakta [Azure-supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="f4a00-291">Contact [Azure support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span> <span data-ttu-id="f4a00-292">Starta genom att öppna en [Azure supportärende](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="f4a00-292">Start by opening an [Azure support case](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span>
3. <span data-ttu-id="f4a00-293">Ring oss.</span><span class="sxs-lookup"><span data-stu-id="f4a00-293">Call us.</span></span> <span data-ttu-id="f4a00-294">[Hitta ett lokalt försäljning nummer](https://azure.microsoft.com/overview/sales-number/).</span><span class="sxs-lookup"><span data-stu-id="f4a00-294">[Find a local sales number](https://azure.microsoft.com/overview/sales-number/).</span></span>

