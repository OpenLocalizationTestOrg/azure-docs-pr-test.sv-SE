---
title: "aaaReplicate en Dynamics AX-distribution för flera nivåer som med Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln beskriver hur tooreplicate och skydda Dynamics AX med hjälp av Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/24/2017
ms.author: asgang
ms.openlocfilehash: b974315ec50ab2ec43846b3d3f95c7de88b72fc3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-a-multi-tier-dynamics-ax-application-using-azure-site-recovery"></a><span data-ttu-id="c86b4-103">Replikera en Dynamics AX flernivåapp med hjälp av Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="c86b4-103">Replicate a multi-tier Dynamics AX application using Azure Site Recovery</span></span>

## <a name="overview"></a><span data-ttu-id="c86b4-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="c86b4-104">Overview</span></span>


<span data-ttu-id="c86b4-105">Microsoft Dynamics AX är en av hello populäraste ERP-lösning mellan företag toostandardized process mellan platser, hantera resurser och förenkla efterlevnad.</span><span class="sxs-lookup"><span data-stu-id="c86b4-105">Microsoft Dynamics AX is one of hello most popular ERP solution among enterprises toostandardized process across locations, manage resources and simplifying compliance.</span></span> <span data-ttu-id="c86b4-106">Considering hello programmet är kritiska tooan organisationen är det mycket viktigt att toobe som om en katastrof som programmet ska vara igång i minimitid.</span><span class="sxs-lookup"><span data-stu-id="c86b4-106">Considering hello application is business critical tooan organization it is very important toobe sure that if any disaster, application should be up and running in minimum time.</span></span>

<span data-ttu-id="c86b4-107">Microsoft Dynamics AX ger idag inte några funktioner för out box katastrofåterställning.</span><span class="sxs-lookup"><span data-stu-id="c86b4-107">Today, Microsoft Dynamics AX  does not provide any out-of-the-box disaster recovery capabilities.</span></span> <span data-ttu-id="c86b4-108">Microsoft Dynamics AX består av många server-komponenter som objektet programservern, Active Directory (AD), SQL Database-Server, SharePoint Server, etc. Reporting Server toomanage hello katastrofåterställning för var och en av dessa komponenter manuellt är inte bara billigare men även felbenägna.</span><span class="sxs-lookup"><span data-stu-id="c86b4-108">Microsoft Dynamics AX consists of many server components like Application Object Server, Active Directory (AD), SQL Database Server, SharePoint Server, Reporting Server etc. toomanage hello disaster recovery of each of these components manually is not only expensive but also error-prone.</span></span>

<span data-ttu-id="c86b4-109">Den här artikeln innehåller detaljerad information om hur du skapar en lösning för katastrofåterställning för din Dynamics AX-program med [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c86b4-109">This article explains in detail about how you can create a disaster recovery solution for your Dynamics AX application using [Azure Site Recovery](site-recovery-overview.md).</span></span> <span data-ttu-id="c86b4-110">Den omfattar också planerad/oplanerad/redundanstestning med ett klick återställningsplan, konfigurationer som stöds och förutsättningar.</span><span class="sxs-lookup"><span data-stu-id="c86b4-110">It also covers planned/unplanned/test failovers using one-click recovery plan, supported configurations, and prerequisites.</span></span>
<span data-ttu-id="c86b4-111">Azure Site Recovery-baserad lösning för katastrofåterställning är fullständigt testade certifierad och rekommenderas av Microsoft Dynamics AX.</span><span class="sxs-lookup"><span data-stu-id="c86b4-111">Azure Site Recovery based disaster recovery solution is fully tested, certified, and recommended by Microsoft Dynamics AX.</span></span>



## <a name="prerequisites"></a><span data-ttu-id="c86b4-112">Krav</span><span class="sxs-lookup"><span data-stu-id="c86b4-112">Prerequisites</span></span>

<span data-ttu-id="c86b4-113">Implementera haveriberedskap för Dynamics AX-program med hjälp av Azure Site Recovery kräver hello följande förutsättningar slutfördes.</span><span class="sxs-lookup"><span data-stu-id="c86b4-113">Implementing disaster recovery for Dynamics AX application using Azure Site Recovery requires hello following pre-requisites completed.</span></span>

<span data-ttu-id="c86b4-114">• En lokal Dynamics AX-distribution har konfigurerats</span><span class="sxs-lookup"><span data-stu-id="c86b4-114">•   An on-premises Dynamics AX deployment has been set up</span></span>

<span data-ttu-id="c86b4-115">• Azure Site Recovery Services-valvet har skapats i Microsoft Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c86b4-115">•   Azure Site Recovery Services vault has been created in Microsoft Azure subscription</span></span>

<span data-ttu-id="c86b4-116">• Om Azure är din återställningsplatsen kör hello Azure virtuella Readiness Assessment tool på virtuella datorer tooensure som de är kompatibla med virtuella Azure-datorer och Azure Site Recovery Services</span><span class="sxs-lookup"><span data-stu-id="c86b4-116">•   If Azure is your recovery site, run hello Azure Virtual Machine Readiness Assessment tool  on VMs tooensure that they are compatible with Azure VMs and Azure Site Recovery Services</span></span>


## <a name="site-recovery-support"></a><span data-ttu-id="c86b4-117">Site Recovery-stöd</span><span class="sxs-lookup"><span data-stu-id="c86b4-117">Site Recovery support</span></span>

<span data-ttu-id="c86b4-118">För hello syftet med att skapa den här artikeln, användes VMware-datorer med Dynamics AX 2012R3 på Windows Server 2012 R2 Enterprise.</span><span class="sxs-lookup"><span data-stu-id="c86b4-118">For hello purpose of creating this article, VMware virtual machines with Dynamics AX  2012R3 on Windows Server 2012 R2 Enterprise were used.</span></span> <span data-ttu-id="c86b4-119">Eftersom site recovery replikering oberoende av programmet hello rekommendationer som anges här förväntade toohold på samt följande scenarier.</span><span class="sxs-lookup"><span data-stu-id="c86b4-119">As site recovery replication is application agnostic, hello recommendations provided here are expected toohold on following scenarios as well.</span></span>

### <a name="source-and-target"></a><span data-ttu-id="c86b4-120">Källa och mål</span><span class="sxs-lookup"><span data-stu-id="c86b4-120">Source and target</span></span>

<span data-ttu-id="c86b4-121">**Scenario**</span><span class="sxs-lookup"><span data-stu-id="c86b4-121">**Scenario**</span></span> | <span data-ttu-id="c86b4-122">**tooa sekundär plats**</span><span class="sxs-lookup"><span data-stu-id="c86b4-122">**tooa secondary site**</span></span> | <span data-ttu-id="c86b4-123">**tooAzure**</span><span class="sxs-lookup"><span data-stu-id="c86b4-123">**tooAzure**</span></span>
--- | --- | ---
<span data-ttu-id="c86b4-124">**Hyper-V**</span><span class="sxs-lookup"><span data-stu-id="c86b4-124">**Hyper-V**</span></span> | <span data-ttu-id="c86b4-125">Ja</span><span class="sxs-lookup"><span data-stu-id="c86b4-125">Yes</span></span> | <span data-ttu-id="c86b4-126">Ja</span><span class="sxs-lookup"><span data-stu-id="c86b4-126">Yes</span></span>
<span data-ttu-id="c86b4-127">**VMware**</span><span class="sxs-lookup"><span data-stu-id="c86b4-127">**VMware**</span></span> | <span data-ttu-id="c86b4-128">Ja</span><span class="sxs-lookup"><span data-stu-id="c86b4-128">Yes</span></span> | <span data-ttu-id="c86b4-129">Ja</span><span class="sxs-lookup"><span data-stu-id="c86b4-129">Yes</span></span>
<span data-ttu-id="c86b4-130">**Fysisk server**</span><span class="sxs-lookup"><span data-stu-id="c86b4-130">**Physical server**</span></span> | <span data-ttu-id="c86b4-131">Ja</span><span class="sxs-lookup"><span data-stu-id="c86b4-131">Yes</span></span> | <span data-ttu-id="c86b4-132">Ja</span><span class="sxs-lookup"><span data-stu-id="c86b4-132">Yes</span></span>

## <a name="enable-dr-of-dynamics-ax-application-using-azure-site-recovery"></a><span data-ttu-id="c86b4-133">Aktivera DR Dynamics AX-program med hjälp av Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="c86b4-133">Enable DR of Dynamics AX application using Azure Site Recovery</span></span>
### <a name="protect-your-dynamics-ax-application"></a><span data-ttu-id="c86b4-134">Skydda din Dynamics AX-program</span><span class="sxs-lookup"><span data-stu-id="c86b4-134">Protect your Dynamics AX application</span></span>
<span data-ttu-id="c86b4-135">Varje komponent i hello Dynamics AX behov toobe skyddade tooenable hello hela appen replikering och återställning.</span><span class="sxs-lookup"><span data-stu-id="c86b4-135">Each component of hello Dynamics AX needs toobe protected tooenable hello complete application replication and recovery.</span></span> <span data-ttu-id="c86b4-136">Det här avsnittet beskriver:</span><span class="sxs-lookup"><span data-stu-id="c86b4-136">This section covers:</span></span>

<span data-ttu-id="c86b4-137">**1. Skydd av Active Directory**</span><span class="sxs-lookup"><span data-stu-id="c86b4-137">**1. Protection of Active Directory**</span></span>

<span data-ttu-id="c86b4-138">**2. Skydd av SQL-nivå**</span><span class="sxs-lookup"><span data-stu-id="c86b4-138">**2. Protection of SQL Tier**</span></span>

<span data-ttu-id="c86b4-139">**3. Skydd av appen och webb-nivåer**</span><span class="sxs-lookup"><span data-stu-id="c86b4-139">**3. Protection of App and Web Tiers**</span></span>

<span data-ttu-id="c86b4-140">**4. Nätverkskonfiguration**</span><span class="sxs-lookup"><span data-stu-id="c86b4-140">**4. Networking configuration**</span></span>

<span data-ttu-id="c86b4-141">**5. Återställningsplan**</span><span class="sxs-lookup"><span data-stu-id="c86b4-141">**5. Recovery Plan**</span></span>

### <a name="1-setup-ad-and-dns-replication"></a><span data-ttu-id="c86b4-142">1. Installationsprogrammet för AD- och DNS-replikering</span><span class="sxs-lookup"><span data-stu-id="c86b4-142">1. Setup AD and DNS replication</span></span>

<span data-ttu-id="c86b4-143">Active Directory krävs för hello DR-plats för toofunction för Dynamics AX-program.</span><span class="sxs-lookup"><span data-stu-id="c86b4-143">Active Directory is required on hello DR site for Dynamics AX application toofunction.</span></span> <span data-ttu-id="c86b4-144">Det finns två rekommenderade alternativ baserat på hello komplexitet hello kundens lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="c86b4-144">There are two recommended choices based on hello complexity of hello customer’s on-premises environment.</span></span>

<span data-ttu-id="c86b4-145">**Alternativ 1**</span><span class="sxs-lookup"><span data-stu-id="c86b4-145">**Option 1**</span></span>

<span data-ttu-id="c86b4-146">Om hello kunden har ett litet antal program och en enda domänkontrollant för sin hela lokal plats och kommer att redundansväxla hello hela webbplatsen tillsammans, och vi rekommenderar att du använder ASR replikering tooreplicate hello DC datorn toosecondary plats ( gäller för både plats tooSite och platsen tooAzure).</span><span class="sxs-lookup"><span data-stu-id="c86b4-146">If hello customer has a small number of applications and a single domain controller for his entire on-premises site and will be failing over hello entire site together, then we recommend using ASR-Replication tooreplicate hello DC machine toosecondary site (applicable for both Site tooSite and Site tooAzure).</span></span>

<span data-ttu-id="c86b4-147">**Alternativ 2**</span><span class="sxs-lookup"><span data-stu-id="c86b4-147">**Option 2**</span></span>

<span data-ttu-id="c86b4-148">Om hello kunden har ett stort antal program och kör en Active Directory-skog och kommer failover några program i taget, så rekommenderar vi att ställa in ytterligare en domänkontrollant på hello DR-plats (sekundär plats eller i Azure).</span><span class="sxs-lookup"><span data-stu-id="c86b4-148">If hello customer has a large number of applications and is running an Active Directory forest and will fail-over few applications at a time, then we recommend setting up an additional domain controller on hello DR site (secondary site or in Azure).</span></span>

<span data-ttu-id="c86b4-149">Se för[handbok för att göra en domänkontrollant tillgänglig på DR-plats](site-recovery-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="c86b4-149">Please refer too[companion guide on making a domain controller available on DR site](site-recovery-active-directory.md).</span></span> <span data-ttu-id="c86b4-150">I resten av det här dokumentet kommer vi antar att en Domänkontrollant är tillgänglig på DR-plats.</span><span class="sxs-lookup"><span data-stu-id="c86b4-150">For remainder of this document we will assume a DC is available on DR site.</span></span>

### <a name="2-setup-sql-server-replication"></a><span data-ttu-id="c86b4-151">2. Konfigurera SQL Server-replikering</span><span class="sxs-lookup"><span data-stu-id="c86b4-151">2. Setup SQL Server replication</span></span>
<span data-ttu-id="c86b4-152">Mer detaljerad teknisk information om hello rekommenderas för att skydda finns toocompanion guide [SQL nivå](site-recovery-sql.md).</span><span class="sxs-lookup"><span data-stu-id="c86b4-152">Please refer toocompanion guide  for detailed technical guidance on hello recommended option for protecting [SQL tier](site-recovery-sql.md).</span></span>

### <a name="3-enable-protection-for-dynamics-ax-client-and-aos-vms"></a><span data-ttu-id="c86b4-153">3. Aktivera skydd för Dynamics AX-klienten och AOS virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="c86b4-153">3. Enable protection for Dynamics AX client and AOS VMs</span></span>
<span data-ttu-id="c86b4-154">Konfigurera relevanta Azure Site Recovery baserat på om hello virtuella datorer distribueras på [Hyper-V](site-recovery-hyper-v-site-to-azure.md) eller på [VMware](site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="c86b4-154">Perform relevant Azure Site Recovery configuration based on whether hello VMs are deployed on [Hyper-V](site-recovery-hyper-v-site-to-azure.md) or on [VMware](site-recovery-vmware-to-azure.md).</span></span>

> [!TIP]
> <span data-ttu-id="c86b4-155">Rekommenderade krascher konsekvent frekvens tooconfigure är 15 minuter.</span><span class="sxs-lookup"><span data-stu-id="c86b4-155">Recommended Crash consistent frequency tooconfigure is 15 minutes.</span></span>
>

<span data-ttu-id="c86b4-156">hello nedan ögonblicksbild visar hello skydd för virtuella datorer Dynamics-komponenten i 'VMware plats tooAzure' skyddsscenariet.</span><span class="sxs-lookup"><span data-stu-id="c86b4-156">hello below snapshot shows hello protection status of Dynamics component VMs in ‘VMware site tooAzure’ protection scenario.</span></span>
<span data-ttu-id="c86b4-157">![Skyddade objekt](./media/site-recovery-dynamics-ax/protecteditems.png)</span><span class="sxs-lookup"><span data-stu-id="c86b4-157">![Protected items ](./media/site-recovery-dynamics-ax/protecteditems.png)</span></span>

### <a name="4-configure-networking"></a><span data-ttu-id="c86b4-158">4. Konfigurera nätverksfunktioner</span><span class="sxs-lookup"><span data-stu-id="c86b4-158">4. Configure Networking</span></span>
<span data-ttu-id="c86b4-159">Konfigurera VM beräknings- och nätverksinställningar</span><span class="sxs-lookup"><span data-stu-id="c86b4-159">Configure VM Compute and Network Settings</span></span>

<span data-ttu-id="c86b4-160">Konfigurera nätverksinställningar i Azure Site Recovery hello AX-klienten och AOS virtuella datorer så att hello Virtuella datornätverk hämta bifogade toohello rätt DR-nätverket efter redundans.</span><span class="sxs-lookup"><span data-stu-id="c86b4-160">For hello AX client and AOS VMs configure network settings in Azure Site Recovery so that hello VM networks get attached toohello right DR network after failover.</span></span> <span data-ttu-id="c86b4-161">Kontrollera hello DR-nätverk för de här nivåerna är dirigerbara toohello SQL-nivå.</span><span class="sxs-lookup"><span data-stu-id="c86b4-161">Ensure hello DR network for these tiers is routable toohello SQL tier.</span></span>

<span data-ttu-id="c86b4-162">Du kan välja hello VM i hello replikerade objekt tooconfigure hello nätverksinställningar enligt hello ögonblicksbild nedan.</span><span class="sxs-lookup"><span data-stu-id="c86b4-162">You can select hello VM in hello replicated items tooconfigure hello network settings as shown in hello snapshot below.</span></span>

* <span data-ttu-id="c86b4-163">För AOS-servrar väljer hello korrekt tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="c86b4-163">For AOS servers select hello correct availability set.</span></span>

* <span data-ttu-id="c86b4-164">Om du använder en statisk IP-adress och sedan ange hello IP-adress som du vill hello virtuella tootake i hello **mål-IP** fältet ![nätverksinställningar](./media/site-recovery-dynamics-ax/vmpropertiesaos1.png)</span><span class="sxs-lookup"><span data-stu-id="c86b4-164">If you are using a static IP then specify hello IP that you want hello virtual machine tootake in hello **Target IP** field ![Network Settings ](./media/site-recovery-dynamics-ax/vmpropertiesaos1.png)</span></span>



### <a name="5-creating-a-recovery-plan"></a><span data-ttu-id="c86b4-165">5. Skapa en återställningsplan</span><span class="sxs-lookup"><span data-stu-id="c86b4-165">5. Creating a recovery plan</span></span>

<span data-ttu-id="c86b4-166">Du kan skapa en återställningsplan i Azure Site Recovery tooautomate hello failover-processen.</span><span class="sxs-lookup"><span data-stu-id="c86b4-166">You can create a recovery plan in Azure Site Recovery tooautomate hello failover process.</span></span> <span data-ttu-id="c86b4-167">Lägg till app-nivå och webbnivå i hello planera för återställning.</span><span class="sxs-lookup"><span data-stu-id="c86b4-167">Add app tier and web tier in hello Recovery Plan.</span></span> <span data-ttu-id="c86b4-168">Ordna dem i olika grupper så som hello frontend avstängning innan app-nivå.</span><span class="sxs-lookup"><span data-stu-id="c86b4-168">Order them in different groups so that hello front-end shutdown before app tier.</span></span>

1)  <span data-ttu-id="c86b4-169">Välj hello Azure Site Recovery-valvet i din prenumeration och klicka på 'Återställningsplaner' sida vid sida.</span><span class="sxs-lookup"><span data-stu-id="c86b4-169">Select hello Azure Site Recovery vault in your subscription and click on ‘Recovery Plans’ tile.</span></span>

2)  <span data-ttu-id="c86b4-170">Klicka på ”+ Recovery planera och ange ett namn.</span><span class="sxs-lookup"><span data-stu-id="c86b4-170">Click on ‘+ Recovery plan and specify a name.</span></span>

3)  <span data-ttu-id="c86b4-171">Välj hello 'Source' och 'Target'.</span><span class="sxs-lookup"><span data-stu-id="c86b4-171">Select hello ‘Source’ and ‘Target’.</span></span> <span data-ttu-id="c86b4-172">hello mål kan vara Azure eller sekundär plats.</span><span class="sxs-lookup"><span data-stu-id="c86b4-172">hello target can be Azure or secondary site.</span></span> <span data-ttu-id="c86b4-173">Om du väljer Azure måste du ange hello distributionsmodell</span><span class="sxs-lookup"><span data-stu-id="c86b4-173">In case you choose Azure, you must specify hello deployment model</span></span>

![Skapa återställningsplan](./media/site-recovery-dynamics-ax/recoveryplancreation1.png)

4)  <span data-ttu-id="c86b4-175">Välj hello AOS och klienten VMs toohello återställningsplan och klicka på ✓.</span><span class="sxs-lookup"><span data-stu-id="c86b4-175">Select hello AOS and client VMs toohello recovery plan and click ✓.</span></span>
<span data-ttu-id="c86b4-176">![Skapa återställningsplan](./media/site-recovery-dynamics-ax/selectvms.png)</span><span class="sxs-lookup"><span data-stu-id="c86b4-176">![Create Recovery Plan](./media/site-recovery-dynamics-ax/selectvms.png)</span></span>


![Återställningsplan](./media/site-recovery-dynamics-ax/recoveryplan.png)

<span data-ttu-id="c86b4-178">Du kan anpassa hello återställningsplan för Dynamics AX-program genom att lägga till olika steg som förklaras nedan.</span><span class="sxs-lookup"><span data-stu-id="c86b4-178">You can customize hello recovery plan for Dynamics AX application by adding various steps as detailed below.</span></span> <span data-ttu-id="c86b4-179">hello ovan ögonblicksbild visar hello fullständig återställningsplan när du lägger till alla hello steg.</span><span class="sxs-lookup"><span data-stu-id="c86b4-179">hello above snapshot shows hello complete recovery plan after adding all hello steps.</span></span>

<span data-ttu-id="c86b4-180">*Så här:*</span><span class="sxs-lookup"><span data-stu-id="c86b4-180">*Steps:*</span></span>

<span data-ttu-id="c86b4-181">*1. SQL Server växlar över steg*</span><span class="sxs-lookup"><span data-stu-id="c86b4-181">*1. SQL Server fail over steps*</span></span>

<span data-ttu-id="c86b4-182">Referera för[lösning för SQL Server DR](site-recovery-sql.md) handbok för mer information om återställningsservern steg specifika tooSQL.</span><span class="sxs-lookup"><span data-stu-id="c86b4-182">Refer too[‘SQL Server DR Solution’](site-recovery-sql.md) companion guide  for details about recovery steps specific tooSQL server.</span></span>

<span data-ttu-id="c86b4-183">*2. Redundans grupp 1: Växla över hello AOS virtuella datorer*</span><span class="sxs-lookup"><span data-stu-id="c86b4-183">*2. Failover Group 1: Fail over hello AOS VMs*</span></span>

<span data-ttu-id="c86b4-184">Se till att hello vald återställningspunkt är så nära som möjligt toohello databasen PIT men inte vidare.</span><span class="sxs-lookup"><span data-stu-id="c86b4-184">Make sure that hello recovery point selected is as close as possible toohello database PIT but not ahead.</span></span>

<span data-ttu-id="c86b4-185">*3. Skript: Lägg till belastningsutjämnare (endast E-A)* lägga till ett skript (via Azure automation) efter AOS VM gruppen medföljer tooadd en load balancer tooit.</span><span class="sxs-lookup"><span data-stu-id="c86b4-185">*3. Script: Add load balancer (Only E-A)* Add a script (via Azure automation) after AOS VM group comes up tooadd a load balancer tooit.</span></span> <span data-ttu-id="c86b4-186">Du kan använda ett skript toodo den här uppgiften.</span><span class="sxs-lookup"><span data-stu-id="c86b4-186">You can use a script toodo this task.</span></span> <span data-ttu-id="c86b4-187">Läs artikeln [hur tooadd belastningsutjämnare för flera nivåer program Katastrofåterställning](https://azure.microsoft.com/blog/cloud-migration-and-disaster-recovery-of-load-balanced-multi-tier-applications-using-azure-site-recovery/)</span><span class="sxs-lookup"><span data-stu-id="c86b4-187">Refer article [how tooadd load balancer for multi-tier application DR](https://azure.microsoft.com/blog/cloud-migration-and-disaster-recovery-of-load-balanced-multi-tier-applications-using-azure-site-recovery/)</span></span>

<span data-ttu-id="c86b4-188">*4. Redundans, grupp 2: Växla över hello AX-klienten virtuella datorer.*</span><span class="sxs-lookup"><span data-stu-id="c86b4-188">*4. Failover Group 2: Fail over hello AX client VMs.*</span></span>
<span data-ttu-id="c86b4-189">Växla över hello webbnivå virtuella datorer som en del av hello återställningsplan.</span><span class="sxs-lookup"><span data-stu-id="c86b4-189">Fail over hello web tier VMs as part of hello recovery plan.</span></span>


### <a name="doing-a-test-failover"></a><span data-ttu-id="c86b4-190">Gör ett redundanstest</span><span class="sxs-lookup"><span data-stu-id="c86b4-190">Doing a test failover</span></span>

<span data-ttu-id="c86b4-191">Se too'AD DR Solution ' och tilläggsguiderna för Katastrofåterställning av SQL Server-lösning för överväganden specifika tooAD och SQLServer respektive under Redundanstestningen.</span><span class="sxs-lookup"><span data-stu-id="c86b4-191">Refer too‘AD DR Solution ’ and ‘SQL Server DR solution ’ companion guides for considerations specific tooAD and SQL server respectively during Test Failover.</span></span>

1.  <span data-ttu-id="c86b4-192">Gå tooAzure portal och välj Site Recovery-valvet.</span><span class="sxs-lookup"><span data-stu-id="c86b4-192">Go tooAzure  portal and select your Site Recovery vault.</span></span>
2.  <span data-ttu-id="c86b4-193">Klicka på hello återställningsplan som skapats för Dynamics AX.</span><span class="sxs-lookup"><span data-stu-id="c86b4-193">Click on hello recovery plan created for Dynamics AX.</span></span>
3.  <span data-ttu-id="c86b4-194">Klicka på Testa redundans.</span><span class="sxs-lookup"><span data-stu-id="c86b4-194">Click on ‘Test Failover’.</span></span>
4.  <span data-ttu-id="c86b4-195">Välj hello virtuellt nätverk toostart hello test failover process.</span><span class="sxs-lookup"><span data-stu-id="c86b4-195">Select hello virtual network toostart hello test fail-over process.</span></span>
5.  <span data-ttu-id="c86b4-196">Du kan utföra din verifieringar när hello sekundära miljön är upp.</span><span class="sxs-lookup"><span data-stu-id="c86b4-196">Once hello secondary environment is up, you can perform your validations.</span></span>
6.  <span data-ttu-id="c86b4-197">När hello verifieringar har slutförts, kan du välja verifieringar slutföra och hello redundanstestmiljön kommer att rensas.</span><span class="sxs-lookup"><span data-stu-id="c86b4-197">Once hello validations are complete, you can select ‘Validations complete’ and hello test failover environment will be cleaned.</span></span>

<span data-ttu-id="c86b4-198">Följ [vägledningen](site-recovery-test-failover-to-azure.md) toodo testa redundans.</span><span class="sxs-lookup"><span data-stu-id="c86b4-198">Follow [this guidance](site-recovery-test-failover-to-azure.md) toodo a test failover.</span></span>

### <a name="doing-a-failover"></a><span data-ttu-id="c86b4-199">Genomför en redundansväxling enligt</span><span class="sxs-lookup"><span data-stu-id="c86b4-199">Doing a failover</span></span>

1.  <span data-ttu-id="c86b4-200">Gå tooAzure portal och välj Site Recovery-valvet.</span><span class="sxs-lookup"><span data-stu-id="c86b4-200">Go tooAzure  portal and select your Site Recovery vault.</span></span>
2.  <span data-ttu-id="c86b4-201">Klicka på hello återställningsplan som skapats för Dynamics AX.</span><span class="sxs-lookup"><span data-stu-id="c86b4-201">Click on hello recovery plan created for Dynamics AX.</span></span>
3.  <span data-ttu-id="c86b4-202">Klicka på 'Redundans' och välj 'Redundans'.</span><span class="sxs-lookup"><span data-stu-id="c86b4-202">Click on ‘Failover’ and select ‘ Failover’.</span></span>
4.  <span data-ttu-id="c86b4-203">Välj hello målnätverket och klicka ✓ toostart hello failover-processen.</span><span class="sxs-lookup"><span data-stu-id="c86b4-203">Select hello target network and click ✓ toostart hello failover process.</span></span>

<span data-ttu-id="c86b4-204">Följ [vägledningen](site-recovery-failover.md) när du gör en redundansväxling.</span><span class="sxs-lookup"><span data-stu-id="c86b4-204">Follow [this guidance](site-recovery-failover.md) when you are doing a failover.</span></span>

### <a name="perform-a-failback"></a><span data-ttu-id="c86b4-205">Utför en återställning efter fel</span><span class="sxs-lookup"><span data-stu-id="c86b4-205">Perform a Failback</span></span>

<span data-ttu-id="c86b4-206">Se too'SQL Server DR lösning ' handbok för överväganden specifika tooSQL server vid återställning.</span><span class="sxs-lookup"><span data-stu-id="c86b4-206">Refer too‘SQL Server DR Solution ’ companion guide for considerations specific tooSQL server during Failback.</span></span>

1.  <span data-ttu-id="c86b4-207">Gå tooAzure portal och välj Site Recovery-valvet.</span><span class="sxs-lookup"><span data-stu-id="c86b4-207">Go tooAzure  portal and select your Site Recovery vault.</span></span>
2.  <span data-ttu-id="c86b4-208">Klicka på hello återställningsplan som skapats för Dynamics AX.</span><span class="sxs-lookup"><span data-stu-id="c86b4-208">Click on hello recovery plan created for Dynamics AX.</span></span>
3.  <span data-ttu-id="c86b4-209">Klicka på 'Redundans' och välj växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="c86b4-209">Click on ‘Failover’ and select failover.</span></span>
4.  <span data-ttu-id="c86b4-210">Klicka på Ändra-riktningen.</span><span class="sxs-lookup"><span data-stu-id="c86b4-210">Click on ‘Change Direction’.</span></span>
5.  <span data-ttu-id="c86b4-211">Välj hello lämpliga alternativ - synkronisering och alternativ för att skapa VM</span><span class="sxs-lookup"><span data-stu-id="c86b4-211">Select hello appropriate options - data synchronization and VM creation options</span></span>
6.  <span data-ttu-id="c86b4-212">Klicka på ✓ toostart hello 'Återställning efter fel' process.</span><span class="sxs-lookup"><span data-stu-id="c86b4-212">Click ✓ toostart hello ‘Failback’ process.</span></span>


<span data-ttu-id="c86b4-213">Följ [vägledningen](site-recovery-failback-azure-to-vmware.md) när du gör en återställning efter fel.</span><span class="sxs-lookup"><span data-stu-id="c86b4-213">Follow [this guidance](site-recovery-failback-azure-to-vmware.md) when you are doing a failback.</span></span>

##<a name="summary"></a><span data-ttu-id="c86b4-214">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="c86b4-214">Summary</span></span>
<span data-ttu-id="c86b4-215">Med Azure Site Recovery kan skapa du en fullständig automatiserad haveriberedskapsplan för Dynamics AX-program.</span><span class="sxs-lookup"><span data-stu-id="c86b4-215">Using Azure Site Recovery, you can create a complete automated disaster recovery plan for your Dynamics AX application.</span></span> <span data-ttu-id="c86b4-216">Du kan initiera hello redundans inom några sekunder från var som helst i hello händelse av ett avbrott och få programmet hello igång i minuter.</span><span class="sxs-lookup"><span data-stu-id="c86b4-216">You can initiate hello failover within seconds from anywhere in hello event of a disruption and get hello application up and running in minutes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c86b4-217">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c86b4-217">Next steps</span></span>
<span data-ttu-id="c86b4-218">Läs [vilka arbetsbelastningar kan jag skydda?](site-recovery-workload.md) toolearn mer om hur du skyddar arbetsbelastningar på företag med Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="c86b4-218">Read [What workloads can I protect?](site-recovery-workload.md) toolearn more about protecting enterprise workloads with Azure Site Recovery.</span></span>
