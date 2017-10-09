---
title: "aaaHigh tillgänglighet för SAP HANA på Azure Virtual Machines (virtuella datorer) | Microsoft Docs"
description: "Skapa hög tillgänglighet för SAP HANA på virtuella Azure-datorer (VM)."
services: virtual-machines-linux
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/25/2017
ms.author: sedusch
ms.openlocfilehash: dcb9bb70594f9d97f8a888cec76300bcbe0bf1ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="high-availability-of-sap-hana-on-azure-virtual-machines-vms"></a><span data-ttu-id="eff04-103">Hög tillgänglighet för SAP HANA på virtuella Azure-datorer (VM)</span><span class="sxs-lookup"><span data-stu-id="eff04-103">High Availability of SAP HANA on Azure Virtual Machines (VMs)</span></span>

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[2205917]:https://launchpad.support.sap.com/#/notes/2205917
[1944799]:https://launchpad.support.sap.com/#/notes/1944799
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351

[hana-ha-guide-replication]:sap-hana-high-availability.md#14c19f65-b5aa-4856-9594-b81c7e4df73d
[hana-ha-guide-shared-storage]:sap-hana-high-availability.md#498de331-fa04-490b-997c-b078de457c9d

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[sap-swcenter]:https://launchpad.support.sap.com/#/softwarecenter
[template-multisid-db]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged%2Fazuredeploy.json

<span data-ttu-id="eff04-113">Lokalt, kan du använda antingen HANA System replikering eller använda delad lagring tooestablish hög tillgänglighet för SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="eff04-113">On-premises, you can use either HANA System Replication or use shared storage tooestablish high availability for SAP HANA.</span></span>
<span data-ttu-id="eff04-114">Vi stöds för närvarande bara ställa in HANA System replikering på Azure.</span><span class="sxs-lookup"><span data-stu-id="eff04-114">We currently only support setting up HANA System Replication on Azure.</span></span> <span data-ttu-id="eff04-115">SAP HANA replikering består av en nod och minst en slav nod.</span><span class="sxs-lookup"><span data-stu-id="eff04-115">SAP HANA Replication consists of one master node and at least one slave node.</span></span> <span data-ttu-id="eff04-116">Ändringar toohello data på hello huvudnoden replikerade toohello underordnade noder synkront eller asynkront.</span><span class="sxs-lookup"><span data-stu-id="eff04-116">Changes toohello data on hello master node are replicated toohello slave nodes synchronously or asynchronously.</span></span>

<span data-ttu-id="eff04-117">Den här artikeln beskriver hur toodeploy hello virtuella datorer, konfigurera hello virtuella datorer, installera hello klustret framework, installera och konfigurera replikering för SAP HANA-System.</span><span class="sxs-lookup"><span data-stu-id="eff04-117">This article describes how toodeploy hello virtual machines, configure hello virtual machines, install hello cluster framework, install and configure SAP HANA System Replication.</span></span>
<span data-ttu-id="eff04-118">Installationen kommandon etc. instansnummer 03 i hello Exempelkonfigurationer och HANA System-ID HDB används.</span><span class="sxs-lookup"><span data-stu-id="eff04-118">In hello example configurations, installation commands etc. instance number 03 and HANA System ID HDB is used.</span></span>

<span data-ttu-id="eff04-119">Läs hello först efter SAP anteckningar och dokument</span><span class="sxs-lookup"><span data-stu-id="eff04-119">Read hello following SAP Notes and papers first</span></span>

* <span data-ttu-id="eff04-120">SAP-kommentar [1928533], som innehåller:</span><span class="sxs-lookup"><span data-stu-id="eff04-120">SAP Note [1928533], which has:</span></span>
  * <span data-ttu-id="eff04-121">Lista över Azure VM-storlekar som stöds för hello distributionen av program</span><span class="sxs-lookup"><span data-stu-id="eff04-121">List of Azure VM sizes that are supported for hello deployment of SAP software</span></span>
  * <span data-ttu-id="eff04-122">Viktiga kapacitetsinformation för Azure VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="eff04-122">Important capacity information for Azure VM sizes</span></span>
  * <span data-ttu-id="eff04-123">Stöds SAP-programvara och operativsystem (OS) och kombinationer av databasen</span><span class="sxs-lookup"><span data-stu-id="eff04-123">Supported SAP software, and operating system (OS) and database combinations</span></span>
  * <span data-ttu-id="eff04-124">SAP kernel version som krävs för Windows och Linux i Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="eff04-124">Required SAP kernel version for Windows and Linux on Microsoft Azure</span></span>
* <span data-ttu-id="eff04-125">SAP-kommentar [2015553] listar kraven för stöd för SAP SAP programvarudistributioner i Azure.</span><span class="sxs-lookup"><span data-stu-id="eff04-125">SAP Note [2015553] lists prerequisites for SAP-supported SAP software deployments in Azure.</span></span>
* <span data-ttu-id="eff04-126">SAP-kommentar [2205917] har rekommenderat OS-inställningar för SUSE Linux Enterprise Server för SAP-program</span><span class="sxs-lookup"><span data-stu-id="eff04-126">SAP Note [2205917] has recommended OS settings for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="eff04-127">SAP-kommentar [1944799] har SAP HANA riktlinjer för SUSE Linux Enterprise Server för SAP-program</span><span class="sxs-lookup"><span data-stu-id="eff04-127">SAP Note [1944799] has SAP HANA Guidelines for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="eff04-128">SAP-kommentar [2178632] innehåller detaljerad information om all övervakning mått som rapporterats för SAP i Azure.</span><span class="sxs-lookup"><span data-stu-id="eff04-128">SAP Note [2178632] has detailed information about all monitoring metrics reported for SAP in Azure.</span></span>
* <span data-ttu-id="eff04-129">SAP-kommentar [2191498] hello krävs SAP värden Agent-version för Linux i Azure.</span><span class="sxs-lookup"><span data-stu-id="eff04-129">SAP Note [2191498] has hello required SAP Host Agent version for Linux in Azure.</span></span>
* <span data-ttu-id="eff04-130">SAP-kommentar [2243692] har licensieringsinformation SAP på Linux i Azure.</span><span class="sxs-lookup"><span data-stu-id="eff04-130">SAP Note [2243692] has information about SAP licensing on Linux in Azure.</span></span>
* <span data-ttu-id="eff04-131">SAP-kommentar [1984787] har allmän information om SUSE Linux Enterprise Server 12.</span><span class="sxs-lookup"><span data-stu-id="eff04-131">SAP Note [1984787] has general information about SUSE Linux Enterprise Server 12.</span></span>
* <span data-ttu-id="eff04-132">SAP-kommentar [1999351] innehåller ytterligare felsökningsinformation för hello Azure förbättrad övervakning av tillägget för SAP.</span><span class="sxs-lookup"><span data-stu-id="eff04-132">SAP Note [1999351] has additional troubleshooting information for hello Azure Enhanced Monitoring Extension for SAP.</span></span>
* <span data-ttu-id="eff04-133">[SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) har alla nödvändiga SAP anteckningar för Linux.</span><span class="sxs-lookup"><span data-stu-id="eff04-133">[SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) has all required SAP Notes for Linux.</span></span>
* <span data-ttu-id="eff04-134">[Azure virtuella datorer planering och implementering för SAP på Linux][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="eff04-134">[Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide]</span></span>
* <span data-ttu-id="eff04-135">[Distribution av Azure virtuella datorer för SAP på Linux (den här artikeln)][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="eff04-135">[Azure Virtual Machines deployment for SAP on Linux (this article)][deployment-guide]</span></span>
* <span data-ttu-id="eff04-136">[Azure virtuella datorer DBMS-distribution för SAP på Linux][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="eff04-136">[Azure Virtual Machines DBMS deployment for SAP on Linux][dbms-guide]</span></span>
* <span data-ttu-id="eff04-137">[Scenario för SAP HANA SR prestanda optimerade] [ suse-hana-ha-guide] hello guiden innehåller all nödvändig information tooset SAP HANA System replikering på lokalt.</span><span class="sxs-lookup"><span data-stu-id="eff04-137">[SAP HANA SR Performance Optimized Scenario][suse-hana-ha-guide] hello guide contains all required information tooset up SAP HANA System Replication on-premises.</span></span> <span data-ttu-id="eff04-138">Använd den här guiden som utgångspunkt.</span><span class="sxs-lookup"><span data-stu-id="eff04-138">Use this guide as a baseline.</span></span>

## <a name="deploying-linux"></a><span data-ttu-id="eff04-139">Distribuera Linux</span><span class="sxs-lookup"><span data-stu-id="eff04-139">Deploying Linux</span></span>

<span data-ttu-id="eff04-140">hello resurs agent för SAP HANA ingår i SUSE Linux Enterprise Server för SAP-program.</span><span class="sxs-lookup"><span data-stu-id="eff04-140">hello resource agent for SAP HANA is included in SUSE Linux Enterprise Server for SAP Applications.</span></span>
<span data-ttu-id="eff04-141">hello Azure Marketplace innehåller en bild för SUSE Linux Enterprise Server för SAP program 12 med BYOS (ta med din egen prenumeration) som du kan använda toodeploy nya virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="eff04-141">hello Azure Marketplace contains an image for SUSE Linux Enterprise Server for SAP Applications 12 with BYOS (Bring Your Own Subscription) that you can use toodeploy new virtual machines.</span></span>

### <a name="manual-deployment"></a><span data-ttu-id="eff04-142">Manuell distribution</span><span class="sxs-lookup"><span data-stu-id="eff04-142">Manual Deployment</span></span>

1. <span data-ttu-id="eff04-143">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="eff04-143">Create a Resource Group</span></span>
1. <span data-ttu-id="eff04-144">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="eff04-144">Create a Virtual Network</span></span>
1. <span data-ttu-id="eff04-145">Skapa två Storage-konton</span><span class="sxs-lookup"><span data-stu-id="eff04-145">Create two Storage Accounts</span></span>
1. <span data-ttu-id="eff04-146">Skapa en Tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="eff04-146">Create an Availability Set</span></span>  
   <span data-ttu-id="eff04-147">Ange högsta uppdateringsdomän</span><span class="sxs-lookup"><span data-stu-id="eff04-147">Set max update domain</span></span>
1. <span data-ttu-id="eff04-148">Skapa en belastningsutjämnare (internt)</span><span class="sxs-lookup"><span data-stu-id="eff04-148">Create a Load Balancer (internal)</span></span>  
   <span data-ttu-id="eff04-149">Välj VNET i steget ovan</span><span class="sxs-lookup"><span data-stu-id="eff04-149">Select VNET of step above</span></span>
1. <span data-ttu-id="eff04-150">Skapa virtuell dator 1</span><span class="sxs-lookup"><span data-stu-id="eff04-150">Create Virtual Machine 1</span></span>  
   <span data-ttu-id="eff04-151">https://Portal.Azure.com/#Create/SUSE-byos.SLES-for-SAP-byos12-SP1</span><span class="sxs-lookup"><span data-stu-id="eff04-151">https://portal.azure.com/#create/suse-byos.sles-for-sap-byos12-sp1</span></span>  
   <span data-ttu-id="eff04-152">SLES för SAP program 12 SP1 (BYOS)</span><span class="sxs-lookup"><span data-stu-id="eff04-152">SLES For SAP Applications 12 SP1 (BYOS)</span></span>  
   <span data-ttu-id="eff04-153">Välj Lagringskonto 1</span><span class="sxs-lookup"><span data-stu-id="eff04-153">Select Storage Account 1</span></span>  
   <span data-ttu-id="eff04-154">Välj Tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="eff04-154">Select Availability Set</span></span>  
1. <span data-ttu-id="eff04-155">Skapa virtuell dator 2</span><span class="sxs-lookup"><span data-stu-id="eff04-155">Create Virtual Machine 2</span></span>  
   <span data-ttu-id="eff04-156">https://Portal.Azure.com/#Create/SUSE-byos.SLES-for-SAP-byos12-SP1</span><span class="sxs-lookup"><span data-stu-id="eff04-156">https://portal.azure.com/#create/suse-byos.sles-for-sap-byos12-sp1</span></span>  
   <span data-ttu-id="eff04-157">SLES för SAP program 12 SP1 (BYOS)</span><span class="sxs-lookup"><span data-stu-id="eff04-157">SLES For SAP Applications 12 SP1 (BYOS)</span></span>  
   <span data-ttu-id="eff04-158">Välj Lagringskonto 2</span><span class="sxs-lookup"><span data-stu-id="eff04-158">Select Storage Account 2</span></span>   
   <span data-ttu-id="eff04-159">Välj Tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="eff04-159">Select Availability Set</span></span>  
1. <span data-ttu-id="eff04-160">Lägg till Datadiskar</span><span class="sxs-lookup"><span data-stu-id="eff04-160">Add Data Disks</span></span>
1. <span data-ttu-id="eff04-161">Konfigurera hello belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="eff04-161">Configure hello load balancer</span></span>
    1. <span data-ttu-id="eff04-162">Skapa en klientdel IP-adresspool</span><span class="sxs-lookup"><span data-stu-id="eff04-162">Create a frontend IP pool</span></span>
        1. <span data-ttu-id="eff04-163">Öppna hello belastningsutjämnare, Välj klientdelens IP-pool och klicka på Lägg till</span><span class="sxs-lookup"><span data-stu-id="eff04-163">Open hello load balancer, select frontend IP pool and click Add</span></span>
        1. <span data-ttu-id="eff04-164">Ange hello namnet på hello ny klientdelens IP-pool (till exempel hana-klientdel)</span><span class="sxs-lookup"><span data-stu-id="eff04-164">Enter hello name of hello new frontend IP pool (for example hana-frontend)</span></span>
       1. <span data-ttu-id="eff04-165">Klicka på OK</span><span class="sxs-lookup"><span data-stu-id="eff04-165">Click OK</span></span>
        1. <span data-ttu-id="eff04-166">När du har skapat hello ny klientdelens IP-pool Skriv ned dess IP-adress</span><span class="sxs-lookup"><span data-stu-id="eff04-166">After hello new frontend IP pool is created, write down its IP address</span></span>
    1. <span data-ttu-id="eff04-167">Skapa en serverdelspool</span><span class="sxs-lookup"><span data-stu-id="eff04-167">Create a backend pool</span></span>
        1. <span data-ttu-id="eff04-168">Öppna hello belastningsutjämnare, Välj serverdelspooler och klicka på Lägg till</span><span class="sxs-lookup"><span data-stu-id="eff04-168">Open hello load balancer, select backend pools and click Add</span></span>
        1. <span data-ttu-id="eff04-169">Ange hello namnet på hello nya serverdelspool (till exempel hana-serverdel)</span><span class="sxs-lookup"><span data-stu-id="eff04-169">Enter hello name of hello new backend pool (for example hana-backend)</span></span>
        1. <span data-ttu-id="eff04-170">Klicka på Lägg till en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="eff04-170">Click Add a virtual machine</span></span>
        1. <span data-ttu-id="eff04-171">Välj hello Tillgänglighetsuppsättning du skapade tidigare</span><span class="sxs-lookup"><span data-stu-id="eff04-171">Select hello Availability Set you created earlier</span></span>
        1. <span data-ttu-id="eff04-172">Välj hello virtuella datorer i hello SAP HANA-kluster</span><span class="sxs-lookup"><span data-stu-id="eff04-172">Select hello virtual machines of hello SAP HANA cluster</span></span>
        1. <span data-ttu-id="eff04-173">Klicka på OK</span><span class="sxs-lookup"><span data-stu-id="eff04-173">Click OK</span></span>
    1. <span data-ttu-id="eff04-174">Skapa en hälsoavsökningen</span><span class="sxs-lookup"><span data-stu-id="eff04-174">Create a health probe</span></span>
       1. <span data-ttu-id="eff04-175">Öppna hello belastningsutjämnare, Välj hälsoavsökningar och klicka på Lägg till</span><span class="sxs-lookup"><span data-stu-id="eff04-175">Open hello load balancer, select health probes and click Add</span></span>
        1. <span data-ttu-id="eff04-176">Ange hello namnet på hello nya hälsoavsökningen (till exempel hana-hp)</span><span class="sxs-lookup"><span data-stu-id="eff04-176">Enter hello name of hello new health probe (for example hana-hp)</span></span>
        1. <span data-ttu-id="eff04-177">Välj TCP som protokoll, port 625**03**, hålla intervallet 5 och tröskelvärde för ohälsosamt värde 2</span><span class="sxs-lookup"><span data-stu-id="eff04-177">Select TCP as protocol, port 625**03**, keep Interval 5 and Unhealthy threshold 2</span></span>
        1. <span data-ttu-id="eff04-178">Klicka på OK</span><span class="sxs-lookup"><span data-stu-id="eff04-178">Click OK</span></span>
    1. <span data-ttu-id="eff04-179">Skapa regler för belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="eff04-179">Create load balancing rules</span></span>
        1. <span data-ttu-id="eff04-180">Öppna hello belastningsutjämnare, Välj regler för belastningsutjämning och klicka på Lägg till</span><span class="sxs-lookup"><span data-stu-id="eff04-180">Open hello load balancer, select load balancing rules and click Add</span></span>
        1. <span data-ttu-id="eff04-181">Ange hello namnet på hello ny regel för belastningsutjämnare (till exempel hana-lb-3**03**15)</span><span class="sxs-lookup"><span data-stu-id="eff04-181">Enter hello name of hello new load balancer rule (for example hana-lb-3**03**15)</span></span>
        1. <span data-ttu-id="eff04-182">Välj hello klientdelens IP-adress, serverdelspool och hälsa avsökning du skapade tidigare (till exempel hana-klientdel)</span><span class="sxs-lookup"><span data-stu-id="eff04-182">Select hello frontend IP address, backend pool and health probe you created earlier (for example hana-frontend)</span></span>
        1. <span data-ttu-id="eff04-183">Håll protokollet TCP, ange port 3**03**15</span><span class="sxs-lookup"><span data-stu-id="eff04-183">Keep protocol TCP, enter port 3**03**15</span></span>
        1. <span data-ttu-id="eff04-184">Öka tidsgränsen för inaktivitet too30 minuter</span><span class="sxs-lookup"><span data-stu-id="eff04-184">Increase idle timeout too30 minutes</span></span>
       1. <span data-ttu-id="eff04-185">**Se till att tooenable flytande IP**</span><span class="sxs-lookup"><span data-stu-id="eff04-185">**Make sure tooenable Floating IP**</span></span>
        1. <span data-ttu-id="eff04-186">Klicka på OK</span><span class="sxs-lookup"><span data-stu-id="eff04-186">Click OK</span></span>
        1. <span data-ttu-id="eff04-187">Upprepa hello stegen ovan för port 3**03**17</span><span class="sxs-lookup"><span data-stu-id="eff04-187">Repeat hello steps above for port 3**03**17</span></span>

### <a name="deploy-with-template"></a><span data-ttu-id="eff04-188">Distribuera med mall</span><span class="sxs-lookup"><span data-stu-id="eff04-188">Deploy with template</span></span>
<span data-ttu-id="eff04-189">Du kan använda någon av hello Snabbstart mallar på github toodeploy alla nödvändiga resurser.</span><span class="sxs-lookup"><span data-stu-id="eff04-189">You can use one of hello quick start templates on github toodeploy all required resources.</span></span> <span data-ttu-id="eff04-190">hello mallen distribuerar hello virtuella datorer, hello belastningsutjämnare, tillgänglighetsuppsättning osv. Följ dessa steg toodeploy hello mallen:</span><span class="sxs-lookup"><span data-stu-id="eff04-190">hello template deploys hello virtual machines, hello load balancer, availability set etc. Follow these steps toodeploy hello template:</span></span>

1. <span data-ttu-id="eff04-191">Öppna hello [databasen mallen] [ template-multisid-db] eller hello [konvergerat mallen] [ template-converged] på hello Azure-portalen skapar hello databasen mallen endast hello regler för belastningsutjämning för en databas hello medan hello konvergerade mallen skapar också regler för belastningsutjämning för en ASCS/SCS och ÄNDARE (endast Linux)-instans.</span><span class="sxs-lookup"><span data-stu-id="eff04-191">Open hello [database template][template-multisid-db] or hello [converged template][template-converged] on hello Azure Portal hello database template only creates hello load-balancing rules for a database whereas hello converged template also creates hello load-balancing rules for an ASCS/SCS and ERS (Linux only) instance.</span></span> <span data-ttu-id="eff04-192">Om du planerar tooinstall ett SAP NetWeaver baserat system och du bör också tooinstall hello ASCS/SCS-instans på hello samma datorer, Använd hello [konvergerat mallen][template-converged].</span><span class="sxs-lookup"><span data-stu-id="eff04-192">If you plan tooinstall an SAP NetWeaver based system and you also want tooinstall hello ASCS/SCS instance on hello same machines, use hello [converged template][template-converged].</span></span>
1. <span data-ttu-id="eff04-193">Ange följande parametrar hello</span><span class="sxs-lookup"><span data-stu-id="eff04-193">Enter hello following parameters</span></span>
    1. <span data-ttu-id="eff04-194">SAP System-Id</span><span class="sxs-lookup"><span data-stu-id="eff04-194">Sap System Id</span></span>  
       <span data-ttu-id="eff04-195">Ange hello SAP system Id hello SAP-system som du vill tooinstall.</span><span class="sxs-lookup"><span data-stu-id="eff04-195">Enter hello SAP system Id of hello SAP system you want tooinstall.</span></span> <span data-ttu-id="eff04-196">hello Id ska användas som ett prefix för hello resurser som distribueras.</span><span class="sxs-lookup"><span data-stu-id="eff04-196">hello Id will be used as a prefix for hello resources that are deployed.</span></span>
    1. <span data-ttu-id="eff04-197">Stacken typ (gäller endast om du använder hello konvergerade mall)</span><span class="sxs-lookup"><span data-stu-id="eff04-197">Stack Type (only applicable if you use hello converged template)</span></span>  
       <span data-ttu-id="eff04-198">Ange hello SAP NetWeaver stack</span><span class="sxs-lookup"><span data-stu-id="eff04-198">Select hello SAP NetWeaver stack type</span></span>
    1. <span data-ttu-id="eff04-199">OS-typen</span><span class="sxs-lookup"><span data-stu-id="eff04-199">Os Type</span></span>  
       <span data-ttu-id="eff04-200">Välj en av hello Linux-distributioner.</span><span class="sxs-lookup"><span data-stu-id="eff04-200">Select one of hello Linux distributions.</span></span> <span data-ttu-id="eff04-201">Det här exemplet väljer du SLES 12 BYOS</span><span class="sxs-lookup"><span data-stu-id="eff04-201">For this example, select SLES 12 BYOS</span></span>
    1. <span data-ttu-id="eff04-202">DB-typ</span><span class="sxs-lookup"><span data-stu-id="eff04-202">Db Type</span></span>  
       <span data-ttu-id="eff04-203">Välj HANA</span><span class="sxs-lookup"><span data-stu-id="eff04-203">Select HANA</span></span>
    1. <span data-ttu-id="eff04-204">Storlek för SAP-System</span><span class="sxs-lookup"><span data-stu-id="eff04-204">Sap System Size</span></span>  
       <span data-ttu-id="eff04-205">hello mängden SAP hello nya system ger.</span><span class="sxs-lookup"><span data-stu-id="eff04-205">hello amount of SAPS hello new system will provide.</span></span> <span data-ttu-id="eff04-206">Om du inte är säker på hur många SAP hello system kräver be din SAP-teknikpartner eller systemintegreraren</span><span class="sxs-lookup"><span data-stu-id="eff04-206">If you are not sure how many SAPS hello system will require, please ask your SAP Technology Partner or System Integrator</span></span>
    1. <span data-ttu-id="eff04-207">Systemets tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="eff04-207">System Availability</span></span>  
       <span data-ttu-id="eff04-208">Välj hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="eff04-208">Select HA</span></span>
    1. <span data-ttu-id="eff04-209">Användarnamn och lösenord för Admin administratör</span><span class="sxs-lookup"><span data-stu-id="eff04-209">Admin Username and Admin Password</span></span>  
       <span data-ttu-id="eff04-210">En ny användare skapas som kan vara används toolog på toohello datorn.</span><span class="sxs-lookup"><span data-stu-id="eff04-210">A new user is created that can be used toolog on toohello machine.</span></span>
    1. <span data-ttu-id="eff04-211">Ny eller befintlig undernät</span><span class="sxs-lookup"><span data-stu-id="eff04-211">New Or Existing Subnet</span></span>  
       <span data-ttu-id="eff04-212">Avgör om ett nytt virtuellt nätverk och undernät som ska skapas eller ett befintligt undernät som ska användas.</span><span class="sxs-lookup"><span data-stu-id="eff04-212">Determines whether a new virtual network and subnet should be created or an existing subnet should be used.</span></span> <span data-ttu-id="eff04-213">Om du redan har ett virtuellt nätverk som är anslutna tooyour lokala nätverk väljer du befintliga.</span><span class="sxs-lookup"><span data-stu-id="eff04-213">If you already have a virtual network that is connected tooyour on-premises network, select existing.</span></span>
    1. <span data-ttu-id="eff04-214">Undernät-Id</span><span class="sxs-lookup"><span data-stu-id="eff04-214">Subnet Id</span></span>  
    <span data-ttu-id="eff04-215">hello-ID för virtuella datorer i hello undernät toowhich hello ska anslutas till.</span><span class="sxs-lookup"><span data-stu-id="eff04-215">hello ID of hello subnet toowhich hello virtual machines should be connected to.</span></span> <span data-ttu-id="eff04-216">Välj hello undernätet för din VPN eller Expressroute virtuellt nätverk tooconnect hello tooyour lokalt nätverk för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="eff04-216">Select hello subnet of your VPN or Express Route virtual network tooconnect hello virtual machine tooyour on-premises network.</span></span> <span data-ttu-id="eff04-217">hello ID ser oftast ut så /subscriptions/`<subscription id`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`></span><span class="sxs-lookup"><span data-stu-id="eff04-217">hello ID usually looks like /subscriptions/`<subscription id`>/resourceGroups/`<resource group name`>/providers/Microsoft.Network/virtualNetworks/`<virtual network name`>/subnets/`<subnet name`></span></span>

## <a name="setting-up-linux-ha"></a><span data-ttu-id="eff04-218">Konfigurera Linux hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="eff04-218">Setting up Linux HA</span></span>

<span data-ttu-id="eff04-219">med antingen [A] - tillämpliga tooall noder, [1] – gäller endast toonode 1 eller [2] - gäller endast toonode 2 prefixet hello följande objekt.</span><span class="sxs-lookup"><span data-stu-id="eff04-219">hello following items are prefixed with either [A] - applicable tooall nodes, [1] - only applicable toonode 1 or [2] - only applicable toonode 2.</span></span>

1. <span data-ttu-id="eff04-220">[A] SLES SAP BYOS endast - registrera SLES toobe kan toouse hello databaser</span><span class="sxs-lookup"><span data-stu-id="eff04-220">[A] SLES for SAP BYOS only - Register SLES toobe able toouse hello repositories</span></span>
1. <span data-ttu-id="eff04-221">[A] SLES för SAP BYOS endast - Lägg till offentliga moln modul</span><span class="sxs-lookup"><span data-stu-id="eff04-221">[A] SLES for SAP BYOS only - Add public-cloud module</span></span>
1. <span data-ttu-id="eff04-222">[A] uppdatera SLES</span><span class="sxs-lookup"><span data-stu-id="eff04-222">[A] Update SLES</span></span>
    ```bash
    sudo zypper update

    ```

1. <span data-ttu-id="eff04-223">[1] aktivera ssh-åtkomst</span><span class="sxs-lookup"><span data-stu-id="eff04-223">[1] Enable ssh access</span></span>
    ```bash
    sudo ssh-keygen -tdsa
    
    # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy hello public key
    sudo cat /root/.ssh/id_dsa.pub
    ```

2. <span data-ttu-id="eff04-224">[2] aktivera ssh-åtkomst</span><span class="sxs-lookup"><span data-stu-id="eff04-224">[2] Enable ssh access</span></span>
    ```bash
    sudo ssh-keygen -tdsa

    # insert hello public key you copied in hello last step into hello authorized keys file on hello second server
    sudo vi /root/.ssh/authorized_keys
    
    # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy hello public key    
    sudo cat /root/.ssh/id_dsa.pub
    ```

1. <span data-ttu-id="eff04-225">[1] aktivera ssh-åtkomst</span><span class="sxs-lookup"><span data-stu-id="eff04-225">[1] Enable ssh access</span></span>
    ```bash
    # insert hello public key you copied in hello last step into hello authorized keys file on hello first server
    sudo vi /root/.ssh/authorized_keys
    
    ```

1. <span data-ttu-id="eff04-226">[A] installera tillägg för hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="eff04-226">[A] Install HA extension</span></span>
    ```bash
    sudo zypper install sle-ha-release fence-agents
    
    ```

1. <span data-ttu-id="eff04-227">[A] disklayouten för installationsprogrammet för</span><span class="sxs-lookup"><span data-stu-id="eff04-227">[A] Setup disk layout</span></span>
    1. <span data-ttu-id="eff04-228">LVM</span><span class="sxs-lookup"><span data-stu-id="eff04-228">LVM</span></span>  
    <span data-ttu-id="eff04-229">Vi rekommenderar vanligtvis toouse LVM för volymer som lagrar data och loggfiler.</span><span class="sxs-lookup"><span data-stu-id="eff04-229">We generally recommend toouse LVM for volumes that store data and log files.</span></span> <span data-ttu-id="eff04-230">hello exemplet nedan förutsätter att hello virtuella datorer har fyra datadiskar som är anslutna som ska använda toocreate två volymer.</span><span class="sxs-lookup"><span data-stu-id="eff04-230">hello example below assumes that hello virtual machines have four data disks attached that should be used toocreate two volumes.</span></span>
        * <span data-ttu-id="eff04-231">Skapa fysiska volymer för alla diskar som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="eff04-231">Create physical volumes for all disks that you want toouse.</span></span>
    <pre><code>
    sudo pvcreate /dev/sdc
    sudo pvcreate /dev/sdd
    sudo pvcreate /dev/sde
    sudo pvcreate /dev/sdf
    </code></pre>
        * <span data-ttu-id="eff04-232">Skapa en volym grupp för hello-datafiler, en volym grupp för hello loggfiler och en för hello delade katalogen för SAP HANA</span><span class="sxs-lookup"><span data-stu-id="eff04-232">Create a volume group for hello data files, one volume group for hello log files and one for hello shared directory of SAP HANA</span></span>
    <pre><code>
    sudo vgcreate vg_hana_data /dev/sdc /dev/sdd
    sudo vgcreate vg_hana_log /dev/sde
    sudo vgcreate vg_hana_shared /dev/sdf
    </code></pre>
        * <span data-ttu-id="eff04-233">Skapa hello logiska volymer</span><span class="sxs-lookup"><span data-stu-id="eff04-233">Create hello logical volumes</span></span>
    <pre><code>
    sudo lvcreate -l 100%FREE -n hana_data vg_hana_data
    sudo lvcreate -l 100%FREE -n hana_log vg_hana_log
    sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared
    sudo mkfs.xfs /dev/vg_hana_data/hana_data
    sudo mkfs.xfs /dev/vg_hana_log/hana_log
    sudo mkfs.xfs /dev/vg_hana_shared/hana_shared
    </code></pre>
        * <span data-ttu-id="eff04-234">Skapa hello montera kataloger och kopiera hello UUID för alla logiska volymer</span><span class="sxs-lookup"><span data-stu-id="eff04-234">Create hello mount directories and copy hello UUID of all logical volumes</span></span>
    <pre><code>
    sudo mkdir -p /hana/data
    sudo mkdir -p /hana/log
    sudo mkdir -p /hana/shared
    # write down hello id of /dev/vg_hana_data/hana_data, /dev/vg_hana_log/hana_log and /dev/vg_hana_shared/hana_shared
    sudo blkid
    </code></pre>
        * <span data-ttu-id="eff04-235">Skapa fstab poster för hello tre logiska volymer</span><span class="sxs-lookup"><span data-stu-id="eff04-235">Create fstab entries for hello three logical volumes</span></span>
    <pre><code>
    sudo vi /etc/fstab
    </code></pre>
    <span data-ttu-id="eff04-236">Infoga den här raden för/etc/fstab</span><span class="sxs-lookup"><span data-stu-id="eff04-236">Insert this line too/etc/fstab</span></span>
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_data/hana_data&gt;</b> /hana/data xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_log/hana_log&gt;</b> /hana/log xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_shared/hana_shared&gt;</b> /hana/shared xfs  defaults,nofail  0  2
    </code></pre>
        * <span data-ttu-id="eff04-237">Montera hello nya volymer</span><span class="sxs-lookup"><span data-stu-id="eff04-237">Mount hello new volumes</span></span>
    <pre><code>
    sudo mount -a
    </code></pre>
    1. <span data-ttu-id="eff04-238">Vanlig diskar</span><span class="sxs-lookup"><span data-stu-id="eff04-238">Plain Disks</span></span>  
       <span data-ttu-id="eff04-239">För små eller demonstrera system, du kan placera HANA data och loggfilen filer på en disk.</span><span class="sxs-lookup"><span data-stu-id="eff04-239">For small or demo systems, you can place your HANA data and log files on one disk.</span></span> <span data-ttu-id="eff04-240">hello följande kommandon skapar en partition på /dev/sdc och formatera den med xfs.</span><span class="sxs-lookup"><span data-stu-id="eff04-240">hello following commands create a partition on /dev/sdc and format it with xfs.</span></span>
    ```bash
    sudo fdisk /dev/sdc
    sudo mkfs.xfs /dev/sdc1
    
    # <a name="write-down-hello-id-of-devsdc1"></a><span data-ttu-id="eff04-241">Skriv ned hello-id för /dev/sdc1</span><span class="sxs-lookup"><span data-stu-id="eff04-241">write down hello id of /dev/sdc1</span></span>
    <span data-ttu-id="eff04-242">sudo/sbin/blkid sudo vi/etc/fstab</span><span class="sxs-lookup"><span data-stu-id="eff04-242">sudo /sbin/blkid  sudo vi /etc/fstab</span></span>
    ```

    Insert this line too/etc/fstab
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID&gt;</b> /hana xfs  defaults,nofail  0  2
    </code></pre>

    Create hello target directory and mount hello disk.

    ```bash
    sudo mkdir /hana
    sudo mount -a
    ```

1. <span data-ttu-id="eff04-243">[A] konfigurera namnmatchning för alla värdar</span><span class="sxs-lookup"><span data-stu-id="eff04-243">[A] Setup host name resolution for all hosts</span></span>  
    <span data-ttu-id="eff04-244">Du kan använda en DNS-server, eller så kan du ändra hello/etc/hosts på alla noder.</span><span class="sxs-lookup"><span data-stu-id="eff04-244">You can either use a DNS server or modify hello /etc/hosts on all nodes.</span></span> <span data-ttu-id="eff04-245">Det här exemplet visar hur toouse hello/etc/hosts-filen.</span><span class="sxs-lookup"><span data-stu-id="eff04-245">This example shows how toouse hello /etc/hosts file.</span></span>
   <span data-ttu-id="eff04-246">Ersätt hello IP-adress och hello värdnamn i hello följande kommandon</span><span class="sxs-lookup"><span data-stu-id="eff04-246">Replace hello IP address and hello hostname in hello following commands</span></span>
    ```bash
    sudo vi /etc/hosts
    ```
    <span data-ttu-id="eff04-247">Infoga hello följande rader för/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="eff04-247">Insert hello following lines too/etc/hosts.</span></span> <span data-ttu-id="eff04-248">Ändra hello IP-adress och värddatornamn toomatch din miljö</span><span class="sxs-lookup"><span data-stu-id="eff04-248">Change hello IP address and hostname toomatch your environment</span></span>    
    
    <pre><code>
    <b>&lt;IP address of host 1&gt; &lt;hostname of host 1&gt;</b>
    <b>&lt;IP address of host 2&gt; &lt;hostname of host 2&gt;</b>
    </code></pre>

1. <span data-ttu-id="eff04-249">[1] installera kluster</span><span class="sxs-lookup"><span data-stu-id="eff04-249">[1] Install Cluster</span></span>
    ```bash
    sudo ha-cluster-init
    
    # Do you want toocontinue anyway? [y/N] -> y
    # Network address toobind too(e.g.: 192.168.1.0) [10.79.227.0] -> ENTER
    # Multicast address (e.g.: 239.x.x.x) [239.174.218.125] -> ENTER
    # Multicast port [5405] -> ENTER
    # Do you wish toouse SBD? [y/N] -> N
    # Do you wish tooconfigure an administration IP? [y/N] -> N
    ```
        
1. <span data-ttu-id="eff04-250">[2] Lägg till nod toocluster</span><span class="sxs-lookup"><span data-stu-id="eff04-250">[2] Add node toocluster</span></span>
    ```bash
    sudo ha-cluster-join
        
    # WARNING: NTP is not configured toostart at system boot.
    # WARNING: No watchdog device found. If SBD is used, hello cluster will be unable toostart without a watchdog.
    # Do you want toocontinue anyway? [y/N] -> y
    # IP address or hostname of existing node (e.g.: 192.168.1.1) [] -> IP address of node 1 e.g. 10.0.0.5
    # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
    ```

1. <span data-ttu-id="eff04-251">[A] ändra hacluster lösenord toohello samma lösenord</span><span class="sxs-lookup"><span data-stu-id="eff04-251">[A] Change hacluster password toohello same password</span></span>
    ```bash
    sudo passwd hacluster
    
    ```

1. <span data-ttu-id="eff04-252">[A] konfigurerar corosync toouse andra transport och lägger till nodelist.</span><span class="sxs-lookup"><span data-stu-id="eff04-252">[A] Configure corosync toouse other transport and add nodelist.</span></span> <span data-ttu-id="eff04-253">Klustret fungerar inte på annat sätt.</span><span class="sxs-lookup"><span data-stu-id="eff04-253">Cluster will not work otherwise.</span></span>
    ```bash
    sudo vi /etc/corosync/corosync.conf    
    
    ```

    <span data-ttu-id="eff04-254">Lägg till följande fetstil innehåll toohello hello.</span><span class="sxs-lookup"><span data-stu-id="eff04-254">Add hello following bold content toohello file.</span></span>
    
    <pre><code> 
    [...]
      interface { 
          [...] 
      }
      <b>transport:      udpu</b>
    } 
    <b>nodelist {
      node {
        ring0_addr:     < ip address of node 1 >
      }
      node {
        ring0_addr:     < ip address of node 2 > 
      } 
    }</b>
    logging {
      [...]
    </code></pre>

    <span data-ttu-id="eff04-255">Starta om tjänsten för hello corosync</span><span class="sxs-lookup"><span data-stu-id="eff04-255">Then restart hello corosync service</span></span>

    ```bash
    sudo service corosync restart
    
    ```

1. <span data-ttu-id="eff04-256">[A] Installationspaketen HANA hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="eff04-256">[A] Install HANA HA packages</span></span>  
    ```bash
    sudo zypper install SAPHanaSR
    
    ```

## <a name="installing-sap-hana"></a><span data-ttu-id="eff04-257">Installera SAP HANA</span><span class="sxs-lookup"><span data-stu-id="eff04-257">Installing SAP HANA</span></span>

<span data-ttu-id="eff04-258">Följ kapitel 4 i hello [SAP HANA SR prestanda optimerade scenariot guiden] [ suse-hana-ha-guide] tooinstall SAP HANA System replikering.</span><span class="sxs-lookup"><span data-stu-id="eff04-258">Follow chapter 4 of hello [SAP HANA SR Performance Optimized Scenario guide][suse-hana-ha-guide] tooinstall SAP HANA System Replication.</span></span>

1. <span data-ttu-id="eff04-259">[A] kör hdblcm från hello HANA DVD</span><span class="sxs-lookup"><span data-stu-id="eff04-259">[A] Run hdblcm from hello HANA DVD</span></span>
    * <span data-ttu-id="eff04-260">Välj installation -> 1</span><span class="sxs-lookup"><span data-stu-id="eff04-260">Choose installation -> 1</span></span>
    * <span data-ttu-id="eff04-261">Välj ytterligare komponenter för installation -> 1</span><span class="sxs-lookup"><span data-stu-id="eff04-261">Select additional components for installation -> 1</span></span>
    * <span data-ttu-id="eff04-262">Ange installationssökväg [/ hana/delade]: RETUR -></span><span class="sxs-lookup"><span data-stu-id="eff04-262">Enter Installation Path [/hana/shared]: -> ENTER</span></span>
    * <span data-ttu-id="eff04-263">Ange lokala värdnamn [.]: -> RETUR</span><span class="sxs-lookup"><span data-stu-id="eff04-263">Enter Local Host Name [..]: -> ENTER</span></span>
    * <span data-ttu-id="eff04-264">Vill du tooadd ytterligare värdar toohello system?</span><span class="sxs-lookup"><span data-stu-id="eff04-264">Do you want tooadd additional hosts toohello system?</span></span> <span data-ttu-id="eff04-265">(j/n) [n]: RETUR -></span><span class="sxs-lookup"><span data-stu-id="eff04-265">(y/n) [n]: -> ENTER</span></span>
    * <span data-ttu-id="eff04-266">Ange SAP HANA System-ID:<SID of HANA e.g. HDB></span><span class="sxs-lookup"><span data-stu-id="eff04-266">Enter SAP HANA System ID: <SID of HANA e.g. HDB></span></span>
    * <span data-ttu-id="eff04-267">Ange instansnummer [00]:</span><span class="sxs-lookup"><span data-stu-id="eff04-267">Enter Instance Number [00]:</span></span>   
  <span data-ttu-id="eff04-268">HANA instansnummer.</span><span class="sxs-lookup"><span data-stu-id="eff04-268">HANA Instance number.</span></span> <span data-ttu-id="eff04-269">Använd 03 om du har använt hello Azure-mall eller följt hello-exemplet ovan</span><span class="sxs-lookup"><span data-stu-id="eff04-269">Use 03 if you used hello Azure Template or followed hello example above</span></span>
    * <span data-ttu-id="eff04-270">Välj läge för databasen / ange Index [1]: -> RETUR</span><span class="sxs-lookup"><span data-stu-id="eff04-270">Select Database Mode / Enter Index [1]: -> ENTER</span></span>
    * <span data-ttu-id="eff04-271">Välj systemanvändning / ange Index [4]:</span><span class="sxs-lookup"><span data-stu-id="eff04-271">Select System Usage / Enter Index [4]:</span></span>  
  <span data-ttu-id="eff04-272">Välj hello system användning</span><span class="sxs-lookup"><span data-stu-id="eff04-272">Select hello system Usage</span></span>
    * <span data-ttu-id="eff04-273">Ange platsen för datavolymer [/ hana/data/HDB]: RETUR -></span><span class="sxs-lookup"><span data-stu-id="eff04-273">Enter Location of Data Volumes [/hana/data/HDB]: -> ENTER</span></span>
    * <span data-ttu-id="eff04-274">Ange platsen för Loggvolymer [/ hana/log/HDB]: RETUR -></span><span class="sxs-lookup"><span data-stu-id="eff04-274">Enter Location of Log Volumes [/hana/log/HDB]: -> ENTER</span></span>
    * <span data-ttu-id="eff04-275">Begränsa maximal minnesallokering?</span><span class="sxs-lookup"><span data-stu-id="eff04-275">Restrict maximum memory allocation?</span></span> <span data-ttu-id="eff04-276">[n]: RETUR -></span><span class="sxs-lookup"><span data-stu-id="eff04-276">[n]: -> ENTER</span></span>
    * <span data-ttu-id="eff04-277">Ange värdnamnet för certifikat för värden ”...” [...]: RETUR -></span><span class="sxs-lookup"><span data-stu-id="eff04-277">Enter Certificate Host Name For Host '...' [...]: -> ENTER</span></span>
    * <span data-ttu-id="eff04-278">Ange SAP värden Agent användare (sapadm) lösenord:</span><span class="sxs-lookup"><span data-stu-id="eff04-278">Enter SAP Host Agent User (sapadm) Password:</span></span>
    * <span data-ttu-id="eff04-279">Bekräfta SAP värden Agent användare (sapadm) lösenord:</span><span class="sxs-lookup"><span data-stu-id="eff04-279">Confirm SAP Host Agent User (sapadm) Password:</span></span>
    * <span data-ttu-id="eff04-280">Ange systemadministratören (hdbadm) lösenord:</span><span class="sxs-lookup"><span data-stu-id="eff04-280">Enter System Administrator (hdbadm) Password:</span></span>
    * <span data-ttu-id="eff04-281">Bekräfta systemadministratören (hdbadm) lösenord:</span><span class="sxs-lookup"><span data-stu-id="eff04-281">Confirm System Administrator (hdbadm) Password:</span></span>
    * <span data-ttu-id="eff04-282">Ange System administratör arbetskatalog [/ usr/sap/HDB/hem]: RETUR -></span><span class="sxs-lookup"><span data-stu-id="eff04-282">Enter System Administrator Home Directory [/usr/sap/HDB/home]: -> ENTER</span></span>
    * <span data-ttu-id="eff04-283">Ange System inloggningsgränssnitt för administratör [/ bin/del]: RETUR -></span><span class="sxs-lookup"><span data-stu-id="eff04-283">Enter System Administrator Login Shell [/bin/sh]: -> ENTER</span></span>
    * <span data-ttu-id="eff04-284">Ange systemadministratören användar-ID [1001]: -> RETUR</span><span class="sxs-lookup"><span data-stu-id="eff04-284">Enter System Administrator User ID [1001]: -> ENTER</span></span>
    * <span data-ttu-id="eff04-285">Ange ID för användargrupp (sapsys) [79]: RETUR -></span><span class="sxs-lookup"><span data-stu-id="eff04-285">Enter ID of User Group (sapsys) [79]: -> ENTER</span></span>
    * <span data-ttu-id="eff04-286">Ange lösenord för användare (SYSTEM) av databasen:</span><span class="sxs-lookup"><span data-stu-id="eff04-286">Enter Database User (SYSTEM) Password:</span></span>
    * <span data-ttu-id="eff04-287">Bekräfta databas användarlösenord (SYSTEM):</span><span class="sxs-lookup"><span data-stu-id="eff04-287">Confirm Database User (SYSTEM) Password:</span></span>
    * <span data-ttu-id="eff04-288">Starta om systemet efter omstart av datorn?</span><span class="sxs-lookup"><span data-stu-id="eff04-288">Restart system after machine reboot?</span></span> <span data-ttu-id="eff04-289">[n]: RETUR -></span><span class="sxs-lookup"><span data-stu-id="eff04-289">[n]: -> ENTER</span></span>
    * <span data-ttu-id="eff04-290">Vill du toocontinue?</span><span class="sxs-lookup"><span data-stu-id="eff04-290">Do you want toocontinue?</span></span> <span data-ttu-id="eff04-291">(j/n):</span><span class="sxs-lookup"><span data-stu-id="eff04-291">(y/n):</span></span>  
  <span data-ttu-id="eff04-292">Kontrollera hello sammanfattning och ange y toocontinue</span><span class="sxs-lookup"><span data-stu-id="eff04-292">Validate hello summary and enter y toocontinue</span></span>
1. <span data-ttu-id="eff04-293">[A] uppgradera SAP Värdagenten</span><span class="sxs-lookup"><span data-stu-id="eff04-293">[A] Upgrade SAP Host Agent</span></span>  
  <span data-ttu-id="eff04-294">Hämta hello senaste SAP Värdagenten Arkiv från hello [SAP Softwarecenter] [ sap-swcenter] och kör hello följande kommando tooupgrade hello agent.</span><span class="sxs-lookup"><span data-stu-id="eff04-294">Download hello latest SAP Host Agent archive from hello [SAP Softwarecenter][sap-swcenter] and run hello following command tooupgrade hello agent.</span></span> <span data-ttu-id="eff04-295">Ersätt hello sökvägen toohello toopoint toohello arkivfilen du laddade ned.</span><span class="sxs-lookup"><span data-stu-id="eff04-295">Replace hello path toohello archive toopoint toohello file you downloaded.</span></span>
    ```bash
    sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <path tooSAP Host Agent SAR>
    ```

1. <span data-ttu-id="eff04-296">[1] Skapa HANA replikering (som rot)</span><span class="sxs-lookup"><span data-stu-id="eff04-296">[1] Create HANA replication (as root)</span></span>  
    <span data-ttu-id="eff04-297">Kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="eff04-297">Run hello following command.</span></span> <span data-ttu-id="eff04-298">Kontrollera att tooreplace fetstil strängar (HANA System-ID HDB och instans antalet 03) med hello värdena för SAP HANA-installationen.</span><span class="sxs-lookup"><span data-stu-id="eff04-298">Make sure tooreplace bold strings (HANA System ID HDB and instance number 03) with hello values of your SAP HANA installation.</span></span>
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
    hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN too<b>hdb</b>hasync' 
    hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
    </code></pre>

1. <span data-ttu-id="eff04-299">[A] Skapa keystore-post (som rot)</span><span class="sxs-lookup"><span data-stu-id="eff04-299">[A] Create keystore entry (as root)</span></span>
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>passwd</b>
    </code></pre>
1. <span data-ttu-id="eff04-300">[1] backup database (som rot)</span><span class="sxs-lookup"><span data-stu-id="eff04-300">[1] Backup database (as root)</span></span>
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')" 
    </code></pre>
1. <span data-ttu-id="eff04-301">[1] växla toohello sapsid användare (till exempel hdbadm) och skapa hello primär plats.</span><span class="sxs-lookup"><span data-stu-id="eff04-301">[1] Switch toohello sapsid user (for example hdbadm) and create hello primary site.</span></span>
    <pre><code>
    su - <b>hdb</b>adm
    hdbnsutil -sr_enable –-name=<b>SITE1</b>
    </code></pre>
1. <span data-ttu-id="eff04-302">[2] växla toohello sapsid användare (till exempel hdbadm) och skapa hello sekundär plats.</span><span class="sxs-lookup"><span data-stu-id="eff04-302">[2] Switch toohello sapsid user (for example hdbadm) and create hello secondary site.</span></span>
    <pre><code>
    su - <b>hdb</b>adm
    sapcontrol -nr <b>03</b> -function StopWait 600 10
    hdbnsutil -sr_register --remoteHost=<b>saphanavm1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
    </code></pre>

## <a name="configure-cluster-framework"></a><span data-ttu-id="eff04-303">Konfigurera klustret Framework</span><span class="sxs-lookup"><span data-stu-id="eff04-303">Configure Cluster Framework</span></span>

<span data-ttu-id="eff04-304">Ändra standardinställningarna för hello</span><span class="sxs-lookup"><span data-stu-id="eff04-304">Change hello default settings</span></span>

<pre>
sudo vi crm-defaults.txt
# enter hello following toocrm-defaults.txt
<code>
property $id="cib-bootstrap-options" \
  no-quorum-policy="ignore" \
  stonith-enabled="true" \
  stonith-action="reboot" \
  stonith-timeout="150s"
rsc_defaults $id="rsc-options" \
  resource-stickiness="1000" \
  migration-threshold="5000"
op_defaults $id="op-options" \
  timeout="600"
</code>

# <a name="now-we-load-hello-file-toohello-cluster"></a><span data-ttu-id="eff04-305">Vi kan nu läsa in hello toohello kluster</span><span class="sxs-lookup"><span data-stu-id="eff04-305">now we load hello file toohello cluster</span></span>
<span data-ttu-id="eff04-306">sudo crm konfigurera belastningen update crm-defaults.txt</span><span class="sxs-lookup"><span data-stu-id="eff04-306">sudo crm configure load update crm-defaults.txt</span></span>
</pre>

### <a name="create-stonith-device"></a><span data-ttu-id="eff04-307">Skapa STONITH enhet</span><span class="sxs-lookup"><span data-stu-id="eff04-307">Create STONITH device</span></span>

<span data-ttu-id="eff04-308">Hej STONITH enhet använder ett huvudnamn för tjänsten tooauthorize mot Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="eff04-308">hello STONITH device uses a Service Principal tooauthorize against Microsoft Azure.</span></span> <span data-ttu-id="eff04-309">Följ dessa steg toocreate ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="eff04-309">Please follow these steps toocreate a Service Principal.</span></span>

1. <span data-ttu-id="eff04-310">Gå för<https://portal.azure.com></span><span class="sxs-lookup"><span data-stu-id="eff04-310">Go too<https://portal.azure.com></span></span>
1. <span data-ttu-id="eff04-311">Öppna hello Azure Active Directory-bladet</span><span class="sxs-lookup"><span data-stu-id="eff04-311">Open hello Azure Active Directory blade</span></span>  
   <span data-ttu-id="eff04-312">Gå tooProperties och anteckna hello Directory-Id. Detta är hello **klient-id**.</span><span class="sxs-lookup"><span data-stu-id="eff04-312">Go tooProperties and write down hello Directory Id. This is hello **tenant id**.</span></span>
1. <span data-ttu-id="eff04-313">Klicka på appen registreringar</span><span class="sxs-lookup"><span data-stu-id="eff04-313">Click App registrations</span></span>
1. <span data-ttu-id="eff04-314">Klicka på Lägg till</span><span class="sxs-lookup"><span data-stu-id="eff04-314">Click Add</span></span>
1. <span data-ttu-id="eff04-315">Ange ett namn, Välj typ av program ”Web app/API”, ange en inloggnings-URL (till exempel http://localhost) och klicka på Skapa</span><span class="sxs-lookup"><span data-stu-id="eff04-315">Enter a Name, select Application Type "Web app/API", enter a sign-on URL (for example http://localhost) and click Create</span></span>
1. <span data-ttu-id="eff04-316">hello inloggnings-URL används inte och kan vara en giltig URL</span><span class="sxs-lookup"><span data-stu-id="eff04-316">hello sign-on URL is not used and can be any valid URL</span></span>
1. <span data-ttu-id="eff04-317">Välj hello nya App och klicka på nycklarna i hello på fliken Inställningar</span><span class="sxs-lookup"><span data-stu-id="eff04-317">Select hello new App and click Keys in hello Settings tab</span></span>
1. <span data-ttu-id="eff04-318">Ange en beskrivning för en ny nyckel, Välj ”upphör aldrig att gälla” och klicka på Spara</span><span class="sxs-lookup"><span data-stu-id="eff04-318">Enter a description for a new key, select "Never expires" and click Save</span></span>
1. <span data-ttu-id="eff04-319">Skriv ned hello värde.</span><span class="sxs-lookup"><span data-stu-id="eff04-319">Write down hello Value.</span></span> <span data-ttu-id="eff04-320">Den används som hello **lösenord** för hello tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="eff04-320">It is used as hello **password** for hello Service Principal</span></span>
1. <span data-ttu-id="eff04-321">Skriv ned hello program-Id. Den används som hello användarnamn (**inloggnings-id** i hello stegen nedan) för hello tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="eff04-321">Write down hello Application Id. It is used as hello username (**login id** in hello steps below) of hello Service Principal</span></span>

<span data-ttu-id="eff04-322">hello tjänstens huvudnamn har inte behörigheter tooaccess Azure-resurser som standard.</span><span class="sxs-lookup"><span data-stu-id="eff04-322">hello Service Principal does not have permissions tooaccess your Azure resources by default.</span></span> <span data-ttu-id="eff04-323">Du behöver toogive hello tjänstens huvudnamn behörigheter toostart och stoppa (frigöra) alla virtuella datorer i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="eff04-323">You need toogive hello Service Principal permissions toostart and stop (deallocate) all virtual machines of hello cluster.</span></span>

1. <span data-ttu-id="eff04-324">Gå toohttps://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="eff04-324">Go toohttps://portal.azure.com</span></span>
1. <span data-ttu-id="eff04-325">Öppna hello bladet för alla resurser</span><span class="sxs-lookup"><span data-stu-id="eff04-325">Open hello All resources blade</span></span>
1. <span data-ttu-id="eff04-326">Välj hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="eff04-326">Select hello virtual machine</span></span>
1. <span data-ttu-id="eff04-327">Klicka på åtkomstkontroll (IAM)</span><span class="sxs-lookup"><span data-stu-id="eff04-327">Click Access control (IAM)</span></span>
1. <span data-ttu-id="eff04-328">Klicka på Lägg till</span><span class="sxs-lookup"><span data-stu-id="eff04-328">Click Add</span></span>
1. <span data-ttu-id="eff04-329">Välj hello roll ägare</span><span class="sxs-lookup"><span data-stu-id="eff04-329">Select hello role Owner</span></span>
1. <span data-ttu-id="eff04-330">Ange hello namnet på hello-program som du skapade ovan</span><span class="sxs-lookup"><span data-stu-id="eff04-330">Enter hello name of hello application you created above</span></span>
1. <span data-ttu-id="eff04-331">Klicka på OK</span><span class="sxs-lookup"><span data-stu-id="eff04-331">Click OK</span></span>

<span data-ttu-id="eff04-332">När du har redigerat hello behörigheter för hello virtuella datorer kan du konfigurera hello STONITH enheter i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="eff04-332">After you edited hello permissions for hello virtual machines, you can configure hello STONITH devices in hello cluster.</span></span>

<pre>
sudo vi crm-fencing.txt
# enter hello following toocrm-fencing.txt
# replace hello bold string with your subscription id, resource group, tenant id, service principal id and password
<code>
primitive rsc_st_azure_1 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

primitive rsc_st_azure_2 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started
</code>

# <a name="now-we-load-hello-file-toohello-cluster"></a><span data-ttu-id="eff04-333">Vi kan nu läsa in hello toohello kluster</span><span class="sxs-lookup"><span data-stu-id="eff04-333">now we load hello file toohello cluster</span></span>
<span data-ttu-id="eff04-334">sudo crm konfigurera belastningen update crm-fencing.txt</span><span class="sxs-lookup"><span data-stu-id="eff04-334">sudo crm configure load update crm-fencing.txt</span></span>
</pre>

### <a name="create-sap-hana-resources"></a><span data-ttu-id="eff04-335">Skapa SAP HANA-resurser</span><span class="sxs-lookup"><span data-stu-id="eff04-335">Create SAP HANA resources</span></span>

<pre>
sudo vi crm-saphanatop.txt
# enter hello following toocrm-saphana.txt
# replace hello bold string with your instance number and HANA system id
<code>
primitive rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHanaTopology \
    operations $id="rsc_sap2_<b>HDB</b>_HDB<b>03</b>-operations" \
    op monitor interval="10" timeout="600" \
    op start interval="0" timeout="600" \
    op stop interval="0" timeout="300" \
    params SID="<b>HDB</b>" InstanceNumber="<b>03</b>"

clone cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \
    meta is-managed="true" clone-node-max="1" target-role="Started" interleave="true"
</code>

# <a name="now-we-load-hello-file-toohello-cluster"></a><span data-ttu-id="eff04-336">Vi kan nu läsa in hello toohello kluster</span><span class="sxs-lookup"><span data-stu-id="eff04-336">now we load hello file toohello cluster</span></span>
<span data-ttu-id="eff04-337">sudo crm konfigurera belastningen update crm-saphanatop.txt</span><span class="sxs-lookup"><span data-stu-id="eff04-337">sudo crm configure load update crm-saphanatop.txt</span></span>
</pre>

<pre>
sudo vi crm-saphana.txt
# enter hello following toocrm-saphana.txt
# replace hello bold string with your instance number, HANA system id and hello frontend IP address of hello Azure load balancer. 
<code>
primitive rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHana \
    operations $id="rsc_sap_<b>HDB</b>_HDB<b>03</b>-operations" \
    op start interval="0" timeout="3600" \
    op stop interval="0" timeout="3600" \
    op promote interval="0" timeout="3600" \
    op monitor interval="60" role="Master" timeout="700" \
    op monitor interval="61" role="Slave" timeout="700" \
    params SID="<b>HDB</b>" InstanceNumber="<b>03</b>" PREFER_SITE_TAKEOVER="true" \
    DUPLICATE_PRIMARY_TIMEOUT="7200" AUTOMATED_REGISTER="false"

ms msl_SAPHana_<b>HDB</b>_HDB<b>03</b> rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> \
    meta is-managed="true" notify="true" clone-max="2" clone-node-max="1" \
    target-role="Started" interleave="true"

primitive rsc_ip_<b>HDB</b>_HDB<b>03</b> ocf:heartbeat:IPaddr2 \ 
    meta target-role="Started" is-managed="true" \ 
    operations $id="rsc_ip_<b>HDB</b>_HDB<b>03</b>-operations" \ 
    op monitor interval="10s" timeout="20s" \ 
    params ip="<b>10.0.0.21</b>" 
primitive rsc_nc_<b>HDB</b>_HDB<b>03</b> anything \ 
    params binfile="/usr/bin/nc" cmdline_options="-l -k 625<b>03</b>" \ 
    op monitor timeout=20s interval=10 depth=0 
group g_ip_<b>HDB</b>_HDB<b>03</b> rsc_ip_<b>HDB</b>_HDB<b>03</b> rsc_nc_<b>HDB</b>_HDB<b>03</b>
 
colocation col_saphana_ip_<b>HDB</b>_HDB<b>03</b> 2000: g_ip_<b>HDB</b>_HDB<b>03</b>:Started \ 
    msl_SAPHana_<b>HDB</b>_HDB<b>03</b>:Master  
order ord_SAPHana_<b>HDB</b>_HDB<b>03</b> 2000: cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \ 
    msl_SAPHana_<b>HDB</b>_HDB<b>03</b>
</code>

# <a name="now-we-load-hello-file-toohello-cluster"></a><span data-ttu-id="eff04-338">Vi kan nu läsa in hello toohello kluster</span><span class="sxs-lookup"><span data-stu-id="eff04-338">now we load hello file toohello cluster</span></span>
<span data-ttu-id="eff04-339">sudo crm konfigurera belastningen update crm-saphana.txt</span><span class="sxs-lookup"><span data-stu-id="eff04-339">sudo crm configure load update crm-saphana.txt</span></span>
</pre>

### <a name="test-cluster-setup"></a><span data-ttu-id="eff04-340">Inställningar för kluster</span><span class="sxs-lookup"><span data-stu-id="eff04-340">Test cluster setup</span></span>
<span data-ttu-id="eff04-341">hello följande kapitel beskrivs hur du kan testa din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="eff04-341">hello following chapter describe how you can test your setup.</span></span> <span data-ttu-id="eff04-342">Varje test förutsätter att du är rot och hello SAP HANA master körs på hello virtuella saphanavm1.</span><span class="sxs-lookup"><span data-stu-id="eff04-342">Every test assumes that you are root and hello SAP HANA master is running on hello virtual machine saphanavm1.</span></span>

#### <a name="fencing-test"></a><span data-ttu-id="eff04-343">Avgränsningar Test</span><span class="sxs-lookup"><span data-stu-id="eff04-343">Fencing Test</span></span>

<span data-ttu-id="eff04-344">Du kan testa hello installationen av hello avgränsningar agenten genom att inaktivera hello nätverksgränssnittet på noden saphanavm1.</span><span class="sxs-lookup"><span data-stu-id="eff04-344">You can test hello setup of hello fencing agent by disabling hello network interface on node saphanavm1.</span></span>

<pre><code>
sudo ifdown eth0
</code></pre>

<span data-ttu-id="eff04-345">hello virtuella datorn bör nu hämta startas om eller stoppas beroende på klusterkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="eff04-345">hello virtual machine should now get restarted or stopped depending on your cluster configuration.</span></span>
<span data-ttu-id="eff04-346">Om du ställer in hello stonith åtgärd toooff hello virtuella datorn kommer att stoppas och hello resurser är migrerade toohello som kör virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="eff04-346">If you set hello stonith-action toooff, hello virtual machine will be stopped and hello resources are migrated toohello running virtual machine.</span></span>

<span data-ttu-id="eff04-347">När du startar hello virtuella datorn igen hello SAP HANA resursen misslyckas toostart som sekundär om du ställer in AUTOMATED_REGISTER = ”false”.</span><span class="sxs-lookup"><span data-stu-id="eff04-347">Once you start hello virtual machine again, hello SAP HANA resource will fail toostart as secondary if you set AUTOMATED_REGISTER="false".</span></span> <span data-ttu-id="eff04-348">I det här fallet behöver du tooconfigure hello HANA instans som sekundär genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="eff04-348">In this case, you need tooconfigure hello HANA instance as secondary by executing hello following command:</span></span>

<pre><code>
su - <b>hdb</b>adm

# Stop hello HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>

# switch back tooroot and cleanup hello failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-manual-failover"></a><span data-ttu-id="eff04-349">Testa en manuell växling</span><span class="sxs-lookup"><span data-stu-id="eff04-349">Testing a manual failover</span></span>

<span data-ttu-id="eff04-350">Du kan testa en manuell växling av hello pacemaker tjänsten stoppas på noden saphanavm1.</span><span class="sxs-lookup"><span data-stu-id="eff04-350">You can test a manual failover by stopping hello pacemaker service on node saphanavm1.</span></span>
<pre><code>
service pacemaker stop
</code></pre>

<span data-ttu-id="eff04-351">Efter hello redundans kan du starta hello tjänsten igen.</span><span class="sxs-lookup"><span data-stu-id="eff04-351">After hello failover, you can start hello service again.</span></span> <span data-ttu-id="eff04-352">hello SAP HANA-resursen på saphanavm1 misslyckas toostart som sekundär om du ställer in AUTOMATED_REGISTER = ”false”.</span><span class="sxs-lookup"><span data-stu-id="eff04-352">hello SAP HANA resource on saphanavm1 will fail toostart as secondary if you set AUTOMATED_REGISTER="false".</span></span> <span data-ttu-id="eff04-353">I det här fallet behöver du tooconfigure hello HANA instans som sekundär genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="eff04-353">In this case, you need tooconfigure hello HANA instance as secondary by executing hello following command:</span></span>

<pre><code>
service pacemaker start
su - <b>hdb</b>adm

# Stop hello HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 


# switch back tooroot and cleanup hello failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-migration"></a><span data-ttu-id="eff04-354">Testa en migrering</span><span class="sxs-lookup"><span data-stu-id="eff04-354">Testing a migration</span></span>

<span data-ttu-id="eff04-355">Du kan migrera hello SAP HANA-huvudnoden genom att köra följande kommando hello</span><span class="sxs-lookup"><span data-stu-id="eff04-355">You can migrate hello SAP HANA master node by executing hello following command</span></span>
<pre><code>
crm resource migrate msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
crm resource migrate g_ip_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
</code></pre>

<span data-ttu-id="eff04-356">Detta bör migrera hello SAP HANA-huvudnod och hello-grupp som innehåller hello virtuella IP-adress toosaphanavm2.</span><span class="sxs-lookup"><span data-stu-id="eff04-356">This should migrate hello SAP HANA master node and hello group that contains hello virtual IP address toosaphanavm2.</span></span>
<span data-ttu-id="eff04-357">hello SAP HANA-resursen på saphanavm1 misslyckas toostart som sekundär om du ställer in AUTOMATED_REGISTER = ”false”.</span><span class="sxs-lookup"><span data-stu-id="eff04-357">hello SAP HANA resource on saphanavm1 will fail toostart as secondary if you set AUTOMATED_REGISTER="false".</span></span> <span data-ttu-id="eff04-358">I det här fallet behöver du tooconfigure hello HANA instans som sekundär genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="eff04-358">In this case, you need tooconfigure hello HANA instance as secondary by executing hello following command:</span></span>

<pre><code>
su - <b>hdb</b>adm

# Stop hello HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 
</code></pre>

<span data-ttu-id="eff04-359">hello migrering skapar plats begränsningar som behöver toobe tas bort.</span><span class="sxs-lookup"><span data-stu-id="eff04-359">hello migration creates location contraints that need toobe deleted again.</span></span>

<pre><code>
crm configure edited

# delete location contraints that are named like hello following contraint. You should have two contraints, one for hello SAP HANA resource and one for hello IP address group.
location cli-prefer-g_ip_<b>HDB</b>_HDB<b>03</b> g_ip_<b>HDB</b>_HDB<b>03</b> role=Started inf: <b>saphanavm2</b>
</code></pre>

<span data-ttu-id="eff04-360">Du måste också toocleanup hello tillstånd hello sekundära noden resurs</span><span class="sxs-lookup"><span data-stu-id="eff04-360">You also need toocleanup hello state of hello secondary node resource</span></span>

<pre><code>
# switch back tooroot and cleanup hello failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

## <a name="next-steps"></a><span data-ttu-id="eff04-361">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eff04-361">Next steps</span></span>
* <span data-ttu-id="eff04-362">[Azure virtuella datorer planering och implementering för SAP][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="eff04-362">[Azure Virtual Machines planning and implementation for SAP][planning-guide]</span></span>
* <span data-ttu-id="eff04-363">[Distribution av Azure virtuella datorer för SAP][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="eff04-363">[Azure Virtual Machines deployment for SAP][deployment-guide]</span></span>
* <span data-ttu-id="eff04-364">[Azure virtuella datorer DBMS-distribution för SAP][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="eff04-364">[Azure Virtual Machines DBMS deployment for SAP][dbms-guide]</span></span>
* <span data-ttu-id="eff04-365">hur tooestablish hög tillgänglighet och planera för katastrofåterställning för SAP HANA i Azure (stora instanser), se toolearn [SAP HANA (stora instanser) hög tillgänglighet och katastrofåterställning recovery på Azure](hana-overview-high-availability-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="eff04-365">toolearn how tooestablish high availability and plan for disaster recovery of SAP HANA on Azure (large instances), see [SAP HANA (large instances) high availability and disaster recovery on Azure](hana-overview-high-availability-disaster-recovery.md).</span></span> 
