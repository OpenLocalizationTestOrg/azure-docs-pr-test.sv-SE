---
title: "Hög tillgänglighet för SAP HANA på virtuella Azure-datorer (VM) | Microsoft Docs"
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
ms.openlocfilehash: 951150e621d21037b0adde7287b9f985290d8d11
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="high-availability-of-sap-hana-on-azure-virtual-machines-vms"></a><span data-ttu-id="5b69f-103">Hög tillgänglighet för SAP HANA på virtuella Azure-datorer (VM)</span><span class="sxs-lookup"><span data-stu-id="5b69f-103">High Availability of SAP HANA on Azure Virtual Machines (VMs)</span></span>

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

<span data-ttu-id="5b69f-104">[2205917]:https://launchpad.support.sap.com/#/notes/2205917</span><span class="sxs-lookup"><span data-stu-id="5b69f-104">[2205917]:https://launchpad.support.sap.com/#/notes/2205917</span></span>
<span data-ttu-id="5b69f-105">[1944799]:https://launchpad.support.sap.com/#/notes/1944799</span><span class="sxs-lookup"><span data-stu-id="5b69f-105">[1944799]:https://launchpad.support.sap.com/#/notes/1944799</span></span>
<span data-ttu-id="5b69f-106">[1928533]:https://launchpad.support.sap.com/#/notes/1928533</span><span class="sxs-lookup"><span data-stu-id="5b69f-106">[1928533]:https://launchpad.support.sap.com/#/notes/1928533</span></span>
<span data-ttu-id="5b69f-107">[2015553]:https://launchpad.support.sap.com/#/notes/2015553</span><span class="sxs-lookup"><span data-stu-id="5b69f-107">[2015553]:https://launchpad.support.sap.com/#/notes/2015553</span></span>
<span data-ttu-id="5b69f-108">[2178632]:https://launchpad.support.sap.com/#/notes/2178632</span><span class="sxs-lookup"><span data-stu-id="5b69f-108">[2178632]:https://launchpad.support.sap.com/#/notes/2178632</span></span>
<span data-ttu-id="5b69f-109">[2191498]:https://launchpad.support.sap.com/#/notes/2191498</span><span class="sxs-lookup"><span data-stu-id="5b69f-109">[2191498]:https://launchpad.support.sap.com/#/notes/2191498</span></span>
<span data-ttu-id="5b69f-110">[2243692]:https://launchpad.support.sap.com/#/notes/2243692</span><span class="sxs-lookup"><span data-stu-id="5b69f-110">[2243692]:https://launchpad.support.sap.com/#/notes/2243692</span></span>
<span data-ttu-id="5b69f-111">[1984787]:https://launchpad.support.sap.com/#/notes/1984787</span><span class="sxs-lookup"><span data-stu-id="5b69f-111">[1984787]:https://launchpad.support.sap.com/#/notes/1984787</span></span>
<span data-ttu-id="5b69f-112">[1999351]:https://launchpad.support.sap.com/#/notes/1999351</span><span class="sxs-lookup"><span data-stu-id="5b69f-112">[1999351]:https://launchpad.support.sap.com/#/notes/1999351</span></span>

[hana-ha-guide-replication]:sap-hana-high-availability.md#14c19f65-b5aa-4856-9594-b81c7e4df73d
[hana-ha-guide-shared-storage]:sap-hana-high-availability.md#498de331-fa04-490b-997c-b078de457c9d

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[sap-swcenter]:https://launchpad.support.sap.com/#/softwarecenter
[template-multisid-db]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged%2Fazuredeploy.json

<span data-ttu-id="5b69f-113">Lokalt, du kan använda antingen HANA System replikering eller använder delad lagring för att upprätta hög tillgänglighet för SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="5b69f-113">On-premises, you can use either HANA System Replication or use shared storage to establish high availability for SAP HANA.</span></span>
<span data-ttu-id="5b69f-114">Vi stöds för närvarande bara ställa in HANA System replikering på Azure.</span><span class="sxs-lookup"><span data-stu-id="5b69f-114">We currently only support setting up HANA System Replication on Azure.</span></span> <span data-ttu-id="5b69f-115">SAP HANA replikering består av en nod och minst en slav nod.</span><span class="sxs-lookup"><span data-stu-id="5b69f-115">SAP HANA Replication consists of one master node and at least one slave node.</span></span> <span data-ttu-id="5b69f-116">Ändringar av data på huvudnoden replikeras till de underordnade noderna synkront eller asynkront.</span><span class="sxs-lookup"><span data-stu-id="5b69f-116">Changes to the data on the master node are replicated to the slave nodes synchronously or asynchronously.</span></span>

<span data-ttu-id="5b69f-117">Den här artikeln beskriver hur du distribuerar virtuella datorer, konfigurera virtuella datorer, installera kluster framework, installera och konfigurera SAP HANA System replikering.</span><span class="sxs-lookup"><span data-stu-id="5b69f-117">This article describes how to deploy the virtual machines, configure the virtual machines, install the cluster framework, install and configure SAP HANA System Replication.</span></span>
<span data-ttu-id="5b69f-118">Installationen kommandon etc. instansnummer 03 och HANA System-ID HDB används i exempelkonfigurationer.</span><span class="sxs-lookup"><span data-stu-id="5b69f-118">In the example configurations, installation commands etc. instance number 03 and HANA System ID HDB is used.</span></span>

<span data-ttu-id="5b69f-119">Läs följande SAP anteckningar och papper först</span><span class="sxs-lookup"><span data-stu-id="5b69f-119">Read the following SAP Notes and papers first</span></span>

* <span data-ttu-id="5b69f-120">SAP-kommentar [1928533], som innehåller:</span><span class="sxs-lookup"><span data-stu-id="5b69f-120">SAP Note [1928533], which has:</span></span>
  * <span data-ttu-id="5b69f-121">Lista över Azure VM-storlekar som stöds för distribution av program</span><span class="sxs-lookup"><span data-stu-id="5b69f-121">List of Azure VM sizes that are supported for the deployment of SAP software</span></span>
  * <span data-ttu-id="5b69f-122">Viktiga kapacitetsinformation för Azure VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="5b69f-122">Important capacity information for Azure VM sizes</span></span>
  * <span data-ttu-id="5b69f-123">Stöds SAP-programvara och operativsystem (OS) och kombinationer av databasen</span><span class="sxs-lookup"><span data-stu-id="5b69f-123">Supported SAP software, and operating system (OS) and database combinations</span></span>
  * <span data-ttu-id="5b69f-124">SAP kernel version som krävs för Windows och Linux i Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="5b69f-124">Required SAP kernel version for Windows and Linux on Microsoft Azure</span></span>
* <span data-ttu-id="5b69f-125">SAP-kommentar [2015553] listar kraven för stöd för SAP SAP programvarudistributioner i Azure.</span><span class="sxs-lookup"><span data-stu-id="5b69f-125">SAP Note [2015553] lists prerequisites for SAP-supported SAP software deployments in Azure.</span></span>
* <span data-ttu-id="5b69f-126">SAP-kommentar [2205917] har rekommenderat OS-inställningar för SUSE Linux Enterprise Server för SAP-program</span><span class="sxs-lookup"><span data-stu-id="5b69f-126">SAP Note [2205917] has recommended OS settings for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="5b69f-127">SAP-kommentar [1944799] har SAP HANA riktlinjer för SUSE Linux Enterprise Server för SAP-program</span><span class="sxs-lookup"><span data-stu-id="5b69f-127">SAP Note [1944799] has SAP HANA Guidelines for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="5b69f-128">SAP-kommentar [2178632] innehåller detaljerad information om all övervakning mått som rapporterats för SAP i Azure.</span><span class="sxs-lookup"><span data-stu-id="5b69f-128">SAP Note [2178632] has detailed information about all monitoring metrics reported for SAP in Azure.</span></span>
* <span data-ttu-id="5b69f-129">SAP-kommentar [2191498] har Värdagenten för SAP-version som krävs för Linux i Azure.</span><span class="sxs-lookup"><span data-stu-id="5b69f-129">SAP Note [2191498] has the required SAP Host Agent version for Linux in Azure.</span></span>
* <span data-ttu-id="5b69f-130">SAP-kommentar [2243692] har licensieringsinformation SAP på Linux i Azure.</span><span class="sxs-lookup"><span data-stu-id="5b69f-130">SAP Note [2243692] has information about SAP licensing on Linux in Azure.</span></span>
* <span data-ttu-id="5b69f-131">SAP-kommentar [1984787] har allmän information om SUSE Linux Enterprise Server 12.</span><span class="sxs-lookup"><span data-stu-id="5b69f-131">SAP Note [1984787] has general information about SUSE Linux Enterprise Server 12.</span></span>
* <span data-ttu-id="5b69f-132">SAP-kommentar [1999351] innehåller ytterligare felsökningsinformation för Azure förbättrad övervakning av tillägget för SAP.</span><span class="sxs-lookup"><span data-stu-id="5b69f-132">SAP Note [1999351] has additional troubleshooting information for the Azure Enhanced Monitoring Extension for SAP.</span></span>
* <span data-ttu-id="5b69f-133">[SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) har alla nödvändiga SAP anteckningar för Linux.</span><span class="sxs-lookup"><span data-stu-id="5b69f-133">[SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) has all required SAP Notes for Linux.</span></span>
* <span data-ttu-id="5b69f-134">[Azure virtuella datorer planering och implementering för SAP på Linux][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="5b69f-134">[Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide]</span></span>
* <span data-ttu-id="5b69f-135">[Distribution av Azure virtuella datorer för SAP på Linux (den här artikeln)][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="5b69f-135">[Azure Virtual Machines deployment for SAP on Linux (this article)][deployment-guide]</span></span>
* <span data-ttu-id="5b69f-136">[Azure virtuella datorer DBMS-distribution för SAP på Linux][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="5b69f-136">[Azure Virtual Machines DBMS deployment for SAP on Linux][dbms-guide]</span></span>
* <span data-ttu-id="5b69f-137">[Scenario för SAP HANA SR prestanda optimerade] [ suse-hana-ha-guide] guiden innehåller all nödvändig information för att ställa in SAP HANA System replikering på lokalt.</span><span class="sxs-lookup"><span data-stu-id="5b69f-137">[SAP HANA SR Performance Optimized Scenario][suse-hana-ha-guide] The guide contains all required information to set up SAP HANA System Replication on-premises.</span></span> <span data-ttu-id="5b69f-138">Använd den här guiden som utgångspunkt.</span><span class="sxs-lookup"><span data-stu-id="5b69f-138">Use this guide as a baseline.</span></span>

## <a name="deploying-linux"></a><span data-ttu-id="5b69f-139">Distribuera Linux</span><span class="sxs-lookup"><span data-stu-id="5b69f-139">Deploying Linux</span></span>

<span data-ttu-id="5b69f-140">Resurs-agent för SAP HANA ingår i SUSE Linux Enterprise Server för SAP-program.</span><span class="sxs-lookup"><span data-stu-id="5b69f-140">The resource agent for SAP HANA is included in SUSE Linux Enterprise Server for SAP Applications.</span></span>
<span data-ttu-id="5b69f-141">Azure Marketplace innehåller en bild för SUSE Linux Enterprise Server för SAP program 12 med BYOS (ta med din egen prenumeration) som du kan använda för att distribuera nya virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5b69f-141">The Azure Marketplace contains an image for SUSE Linux Enterprise Server for SAP Applications 12 with BYOS (Bring Your Own Subscription) that you can use to deploy new virtual machines.</span></span>

### <a name="manual-deployment"></a><span data-ttu-id="5b69f-142">Manuell distribution</span><span class="sxs-lookup"><span data-stu-id="5b69f-142">Manual Deployment</span></span>

1. <span data-ttu-id="5b69f-143">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="5b69f-143">Create a Resource Group</span></span>
1. <span data-ttu-id="5b69f-144">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="5b69f-144">Create a Virtual Network</span></span>
1. <span data-ttu-id="5b69f-145">Skapa två Storage-konton</span><span class="sxs-lookup"><span data-stu-id="5b69f-145">Create two Storage Accounts</span></span>
1. <span data-ttu-id="5b69f-146">Skapa en Tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="5b69f-146">Create an Availability Set</span></span>  
   <span data-ttu-id="5b69f-147">Ange högsta uppdateringsdomän</span><span class="sxs-lookup"><span data-stu-id="5b69f-147">Set max update domain</span></span>
1. <span data-ttu-id="5b69f-148">Skapa en belastningsutjämnare (internt)</span><span class="sxs-lookup"><span data-stu-id="5b69f-148">Create a Load Balancer (internal)</span></span>  
   <span data-ttu-id="5b69f-149">Välj VNET i steget ovan</span><span class="sxs-lookup"><span data-stu-id="5b69f-149">Select VNET of step above</span></span>
1. <span data-ttu-id="5b69f-150">Skapa virtuell dator 1</span><span class="sxs-lookup"><span data-stu-id="5b69f-150">Create Virtual Machine 1</span></span>  
   <span data-ttu-id="5b69f-151">https://Portal.Azure.com/#Create/SUSE-byos.SLES-for-SAP-byos12-SP1</span><span class="sxs-lookup"><span data-stu-id="5b69f-151">https://portal.azure.com/#create/suse-byos.sles-for-sap-byos12-sp1</span></span>  
   <span data-ttu-id="5b69f-152">SLES för SAP program 12 SP1 (BYOS)</span><span class="sxs-lookup"><span data-stu-id="5b69f-152">SLES For SAP Applications 12 SP1 (BYOS)</span></span>  
   <span data-ttu-id="5b69f-153">Välj Lagringskonto 1</span><span class="sxs-lookup"><span data-stu-id="5b69f-153">Select Storage Account 1</span></span>  
   <span data-ttu-id="5b69f-154">Välj Tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="5b69f-154">Select Availability Set</span></span>  
1. <span data-ttu-id="5b69f-155">Skapa virtuell dator 2</span><span class="sxs-lookup"><span data-stu-id="5b69f-155">Create Virtual Machine 2</span></span>  
   <span data-ttu-id="5b69f-156">https://Portal.Azure.com/#Create/SUSE-byos.SLES-for-SAP-byos12-SP1</span><span class="sxs-lookup"><span data-stu-id="5b69f-156">https://portal.azure.com/#create/suse-byos.sles-for-sap-byos12-sp1</span></span>  
   <span data-ttu-id="5b69f-157">SLES för SAP program 12 SP1 (BYOS)</span><span class="sxs-lookup"><span data-stu-id="5b69f-157">SLES For SAP Applications 12 SP1 (BYOS)</span></span>  
   <span data-ttu-id="5b69f-158">Välj Lagringskonto 2</span><span class="sxs-lookup"><span data-stu-id="5b69f-158">Select Storage Account 2</span></span>   
   <span data-ttu-id="5b69f-159">Välj Tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="5b69f-159">Select Availability Set</span></span>  
1. <span data-ttu-id="5b69f-160">Lägg till Datadiskar</span><span class="sxs-lookup"><span data-stu-id="5b69f-160">Add Data Disks</span></span>
1. <span data-ttu-id="5b69f-161">Konfigurera belastningsutjämnaren</span><span class="sxs-lookup"><span data-stu-id="5b69f-161">Configure the load balancer</span></span>
    1. <span data-ttu-id="5b69f-162">Skapa en klientdel IP-adresspool</span><span class="sxs-lookup"><span data-stu-id="5b69f-162">Create a frontend IP pool</span></span>
        1. <span data-ttu-id="5b69f-163">Öppna belastningsutjämnaren, Välj klientdelens IP-pool och klicka på Lägg till</span><span class="sxs-lookup"><span data-stu-id="5b69f-163">Open the load balancer, select frontend IP pool and click Add</span></span>
        1. <span data-ttu-id="5b69f-164">Ange namnet på den nya klientdelens IP-adresspoolen (till exempel hana-klientdel)</span><span class="sxs-lookup"><span data-stu-id="5b69f-164">Enter the name of the new frontend IP pool (for example hana-frontend)</span></span>
       1. <span data-ttu-id="5b69f-165">Klicka på OK</span><span class="sxs-lookup"><span data-stu-id="5b69f-165">Click OK</span></span>
        1. <span data-ttu-id="5b69f-166">När du har skapat den nya klientdelens IP-poolen Skriv ned dess IP-adress</span><span class="sxs-lookup"><span data-stu-id="5b69f-166">After the new frontend IP pool is created, write down its IP address</span></span>
    1. <span data-ttu-id="5b69f-167">Skapa en serverdelspool</span><span class="sxs-lookup"><span data-stu-id="5b69f-167">Create a backend pool</span></span>
        1. <span data-ttu-id="5b69f-168">Öppna belastningsutjämnaren, Välj serverdelspooler och klicka på Lägg till</span><span class="sxs-lookup"><span data-stu-id="5b69f-168">Open the load balancer, select backend pools and click Add</span></span>
        1. <span data-ttu-id="5b69f-169">Ange namnet på den nya serverdelspoolen (till exempel hana-serverdel)</span><span class="sxs-lookup"><span data-stu-id="5b69f-169">Enter the name of the new backend pool (for example hana-backend)</span></span>
        1. <span data-ttu-id="5b69f-170">Klicka på Lägg till en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="5b69f-170">Click Add a virtual machine</span></span>
        1. <span data-ttu-id="5b69f-171">Välj Tillgänglighetsuppsättningen som du skapade tidigare</span><span class="sxs-lookup"><span data-stu-id="5b69f-171">Select the Availability Set you created earlier</span></span>
        1. <span data-ttu-id="5b69f-172">Välj de virtuella datorerna i SAP HANA-kluster</span><span class="sxs-lookup"><span data-stu-id="5b69f-172">Select the virtual machines of the SAP HANA cluster</span></span>
        1. <span data-ttu-id="5b69f-173">Klicka på OK</span><span class="sxs-lookup"><span data-stu-id="5b69f-173">Click OK</span></span>
    1. <span data-ttu-id="5b69f-174">Skapa en hälsoavsökningen</span><span class="sxs-lookup"><span data-stu-id="5b69f-174">Create a health probe</span></span>
       1. <span data-ttu-id="5b69f-175">Öppna belastningsutjämnaren, Välj hälsoavsökningar och klicka på Lägg till</span><span class="sxs-lookup"><span data-stu-id="5b69f-175">Open the load balancer, select health probes and click Add</span></span>
        1. <span data-ttu-id="5b69f-176">Ange namnet på den nya hälsoavsökningen (till exempel hana-hp)</span><span class="sxs-lookup"><span data-stu-id="5b69f-176">Enter the name of the new health probe (for example hana-hp)</span></span>
        1. <span data-ttu-id="5b69f-177">Välj TCP som protokoll, port 625**03**, hålla intervallet 5 och tröskelvärde för ohälsosamt värde 2</span><span class="sxs-lookup"><span data-stu-id="5b69f-177">Select TCP as protocol, port 625**03**, keep Interval 5 and Unhealthy threshold 2</span></span>
        1. <span data-ttu-id="5b69f-178">Klicka på OK</span><span class="sxs-lookup"><span data-stu-id="5b69f-178">Click OK</span></span>
    1. <span data-ttu-id="5b69f-179">Skapa regler för belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="5b69f-179">Create load balancing rules</span></span>
        1. <span data-ttu-id="5b69f-180">Öppna belastningsutjämnaren, Välj regler för belastningsutjämning och klicka på Lägg till</span><span class="sxs-lookup"><span data-stu-id="5b69f-180">Open the load balancer, select load balancing rules and click Add</span></span>
        1. <span data-ttu-id="5b69f-181">Ange namnet på den nya regeln för belastningsutjämnare (till exempel hana-lb-3**03**15)</span><span class="sxs-lookup"><span data-stu-id="5b69f-181">Enter the name of the new load balancer rule (for example hana-lb-3**03**15)</span></span>
        1. <span data-ttu-id="5b69f-182">Välj IP-adress för klientdel, serverdelspool och hälsa avsökning du skapade tidigare (till exempel hana-klientdel)</span><span class="sxs-lookup"><span data-stu-id="5b69f-182">Select the frontend IP address, backend pool and health probe you created earlier (for example hana-frontend)</span></span>
        1. <span data-ttu-id="5b69f-183">Håll protokollet TCP, ange port 3**03**15</span><span class="sxs-lookup"><span data-stu-id="5b69f-183">Keep protocol TCP, enter port 3**03**15</span></span>
        1. <span data-ttu-id="5b69f-184">Öka tidsgränsen för inaktivitet till 30 minuter</span><span class="sxs-lookup"><span data-stu-id="5b69f-184">Increase idle timeout to 30 minutes</span></span>
       1. <span data-ttu-id="5b69f-185">**Se till att aktivera flytande IP**</span><span class="sxs-lookup"><span data-stu-id="5b69f-185">**Make sure to enable Floating IP**</span></span>
        1. <span data-ttu-id="5b69f-186">Klicka på OK</span><span class="sxs-lookup"><span data-stu-id="5b69f-186">Click OK</span></span>
        1. <span data-ttu-id="5b69f-187">Upprepa stegen ovan för port 3**03**17</span><span class="sxs-lookup"><span data-stu-id="5b69f-187">Repeat the steps above for port 3**03**17</span></span>

### <a name="deploy-with-template"></a><span data-ttu-id="5b69f-188">Distribuera med mall</span><span class="sxs-lookup"><span data-stu-id="5b69f-188">Deploy with template</span></span>
<span data-ttu-id="5b69f-189">Du kan använda en av Snabbstart mallar på github för att distribuera alla nödvändiga resurser.</span><span class="sxs-lookup"><span data-stu-id="5b69f-189">You can use one of the quick start templates on github to deploy all required resources.</span></span> <span data-ttu-id="5b69f-190">Mallen distribuerar virtuella datorer, belastningsutjämnare, tillgänglighetsuppsättning osv. Följ dessa steg om du vill distribuera mallen:</span><span class="sxs-lookup"><span data-stu-id="5b69f-190">The template deploys the virtual machines, the load balancer, availability set etc. Follow these steps to deploy the template:</span></span>

1. <span data-ttu-id="5b69f-191">Öppna den [databasen mallen] [ template-multisid-db] eller [konvergerat mallen] [ template-converged] på Azure Portal mallen databasen skapas endast regler för belastningsutjämning för en databas medan mallen konvergerade skapar också regler för belastningsutjämning för en ASCS/SCS ÄNDARE (endast Linux)-instans.</span><span class="sxs-lookup"><span data-stu-id="5b69f-191">Open the [database template][template-multisid-db] or the [converged template][template-converged] on the Azure Portal The database template only creates the load-balancing rules for a database whereas the converged template also creates the load-balancing rules for an ASCS/SCS and ERS (Linux only) instance.</span></span> <span data-ttu-id="5b69f-192">Om du planerar att installera ett SAP NetWeaver baserat system och du även vill installera ASCS/SCS-instans på samma datorer använder den [konvergerat mallen][template-converged].</span><span class="sxs-lookup"><span data-stu-id="5b69f-192">If you plan to install an SAP NetWeaver based system and you also want to install the ASCS/SCS instance on the same machines, use the [converged template][template-converged].</span></span>
1. <span data-ttu-id="5b69f-193">Ange följande parametrar</span><span class="sxs-lookup"><span data-stu-id="5b69f-193">Enter the following parameters</span></span>
    1. <span data-ttu-id="5b69f-194">SAP System-Id</span><span class="sxs-lookup"><span data-stu-id="5b69f-194">Sap System Id</span></span>  
       <span data-ttu-id="5b69f-195">Ange SAP-systemet Id för SAP-system som du vill installera.</span><span class="sxs-lookup"><span data-stu-id="5b69f-195">Enter the SAP system Id of the SAP system you want to install.</span></span> <span data-ttu-id="5b69f-196">Id som ska användas som ett prefix för de resurser som har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="5b69f-196">The Id will be used as a prefix for the resources that are deployed.</span></span>
    1. <span data-ttu-id="5b69f-197">Stacken typ (gäller endast om du använder mallen konvergerade)</span><span class="sxs-lookup"><span data-stu-id="5b69f-197">Stack Type (only applicable if you use the converged template)</span></span>  
       <span data-ttu-id="5b69f-198">Välj typ för SAP NetWeaver stack</span><span class="sxs-lookup"><span data-stu-id="5b69f-198">Select the SAP NetWeaver stack type</span></span>
    1. <span data-ttu-id="5b69f-199">OS-typen</span><span class="sxs-lookup"><span data-stu-id="5b69f-199">Os Type</span></span>  
       <span data-ttu-id="5b69f-200">Välj en av Linux-distributioner.</span><span class="sxs-lookup"><span data-stu-id="5b69f-200">Select one of the Linux distributions.</span></span> <span data-ttu-id="5b69f-201">Det här exemplet väljer du SLES 12 BYOS</span><span class="sxs-lookup"><span data-stu-id="5b69f-201">For this example, select SLES 12 BYOS</span></span>
    1. <span data-ttu-id="5b69f-202">DB-typ</span><span class="sxs-lookup"><span data-stu-id="5b69f-202">Db Type</span></span>  
       <span data-ttu-id="5b69f-203">Välj HANA</span><span class="sxs-lookup"><span data-stu-id="5b69f-203">Select HANA</span></span>
    1. <span data-ttu-id="5b69f-204">Storlek för SAP-System</span><span class="sxs-lookup"><span data-stu-id="5b69f-204">Sap System Size</span></span>  
       <span data-ttu-id="5b69f-205">Mängden SAP ger det nya systemet.</span><span class="sxs-lookup"><span data-stu-id="5b69f-205">The amount of SAPS the new system will provide.</span></span> <span data-ttu-id="5b69f-206">Om du inte vet hur många SAP systemet kräver, be din SAP-teknikpartner eller systemintegreraren</span><span class="sxs-lookup"><span data-stu-id="5b69f-206">If you are not sure how many SAPS the system will require, please ask your SAP Technology Partner or System Integrator</span></span>
    1. <span data-ttu-id="5b69f-207">Systemets tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="5b69f-207">System Availability</span></span>  
       <span data-ttu-id="5b69f-208">Välj hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="5b69f-208">Select HA</span></span>
    1. <span data-ttu-id="5b69f-209">Användarnamn och lösenord för Admin administratör</span><span class="sxs-lookup"><span data-stu-id="5b69f-209">Admin Username and Admin Password</span></span>  
       <span data-ttu-id="5b69f-210">En ny användare skapas som kan användas för att logga in på datorn.</span><span class="sxs-lookup"><span data-stu-id="5b69f-210">A new user is created that can be used to log on to the machine.</span></span>
    1. <span data-ttu-id="5b69f-211">Ny eller befintlig undernät</span><span class="sxs-lookup"><span data-stu-id="5b69f-211">New Or Existing Subnet</span></span>  
       <span data-ttu-id="5b69f-212">Avgör om ett nytt virtuellt nätverk och undernät som ska skapas eller ett befintligt undernät som ska användas.</span><span class="sxs-lookup"><span data-stu-id="5b69f-212">Determines whether a new virtual network and subnet should be created or an existing subnet should be used.</span></span> <span data-ttu-id="5b69f-213">Om du redan har ett virtuellt nätverk som är anslutna till ditt lokala nätverk väljer du befintliga.</span><span class="sxs-lookup"><span data-stu-id="5b69f-213">If you already have a virtual network that is connected to your on-premises network, select existing.</span></span>
    1. <span data-ttu-id="5b69f-214">Undernät-Id</span><span class="sxs-lookup"><span data-stu-id="5b69f-214">Subnet Id</span></span>  
    <span data-ttu-id="5b69f-215">ID för det undernät som de virtuella datorerna ska anslutas till.</span><span class="sxs-lookup"><span data-stu-id="5b69f-215">The ID of the subnet to which the virtual machines should be connected to.</span></span> <span data-ttu-id="5b69f-216">Välj undernätet i ditt VPN eller Expressroute virtuella nätverk att ansluta den virtuella datorn till ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="5b69f-216">Select the subnet of your VPN or Express Route virtual network to connect the virtual machine to your on-premises network.</span></span> <span data-ttu-id="5b69f-217">ID som ser oftast ut så /subscriptions/`<subscription id`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`></span><span class="sxs-lookup"><span data-stu-id="5b69f-217">The ID usually looks like /subscriptions/`<subscription id`>/resourceGroups/`<resource group name`>/providers/Microsoft.Network/virtualNetworks/`<virtual network name`>/subnets/`<subnet name`></span></span>

## <a name="setting-up-linux-ha"></a><span data-ttu-id="5b69f-218">Konfigurera Linux hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="5b69f-218">Setting up Linux HA</span></span>

<span data-ttu-id="5b69f-219">Följande objekt med antingen [A] - prefixet gäller för alla noder är [1] – gäller endast för nod 1 eller [2] - gäller endast för nod 2.</span><span class="sxs-lookup"><span data-stu-id="5b69f-219">The following items are prefixed with either [A] - applicable to all nodes, [1] - only applicable to node 1 or [2] - only applicable to node 2.</span></span>

1. <span data-ttu-id="5b69f-220">[A] SLES SAP BYOS endast - registrera SLES för att kunna använda databaser</span><span class="sxs-lookup"><span data-stu-id="5b69f-220">[A] SLES for SAP BYOS only - Register SLES to be able to use the repositories</span></span>
1. <span data-ttu-id="5b69f-221">[A] SLES för SAP BYOS endast - Lägg till offentliga moln modul</span><span class="sxs-lookup"><span data-stu-id="5b69f-221">[A] SLES for SAP BYOS only - Add public-cloud module</span></span>
1. <span data-ttu-id="5b69f-222">[A] uppdatera SLES</span><span class="sxs-lookup"><span data-stu-id="5b69f-222">[A] Update SLES</span></span>
    ```bash
    sudo zypper update

    ```

1. <span data-ttu-id="5b69f-223">[1] aktivera ssh-åtkomst</span><span class="sxs-lookup"><span data-stu-id="5b69f-223">[1] Enable ssh access</span></span>
    ```bash
    sudo ssh-keygen -tdsa
    
    # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy the public key
    sudo cat /root/.ssh/id_dsa.pub
    ```

2. <span data-ttu-id="5b69f-224">[2] aktivera ssh-åtkomst</span><span class="sxs-lookup"><span data-stu-id="5b69f-224">[2] Enable ssh access</span></span>
    ```bash
    sudo ssh-keygen -tdsa

    # insert the public key you copied in the last step into the authorized keys file on the second server
    sudo vi /root/.ssh/authorized_keys
    
    # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy the public key    
    sudo cat /root/.ssh/id_dsa.pub
    ```

1. <span data-ttu-id="5b69f-225">[1] aktivera ssh-åtkomst</span><span class="sxs-lookup"><span data-stu-id="5b69f-225">[1] Enable ssh access</span></span>
    ```bash
    # insert the public key you copied in the last step into the authorized keys file on the first server
    sudo vi /root/.ssh/authorized_keys
    
    ```

1. <span data-ttu-id="5b69f-226">[A] installera tillägg för hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="5b69f-226">[A] Install HA extension</span></span>
    ```bash
    sudo zypper install sle-ha-release fence-agents
    
    ```

1. <span data-ttu-id="5b69f-227">[A] disklayouten för installationsprogrammet för</span><span class="sxs-lookup"><span data-stu-id="5b69f-227">[A] Setup disk layout</span></span>
    1. <span data-ttu-id="5b69f-228">LVM</span><span class="sxs-lookup"><span data-stu-id="5b69f-228">LVM</span></span>  
    <span data-ttu-id="5b69f-229">Generellt rekommenderar vi för att använda LVM för volymer som lagrar data och loggfiler.</span><span class="sxs-lookup"><span data-stu-id="5b69f-229">We generally recommend to use LVM for volumes that store data and log files.</span></span> <span data-ttu-id="5b69f-230">Exemplet nedan förutsätter att de virtuella datorerna har fyra datadiskar som är anslutna som ska användas för att skapa två volymer.</span><span class="sxs-lookup"><span data-stu-id="5b69f-230">The example below assumes that the virtual machines have four data disks attached that should be used to create two volumes.</span></span>
        * <span data-ttu-id="5b69f-231">Skapa fysiska volymer för alla diskar som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="5b69f-231">Create physical volumes for all disks that you want to use.</span></span>
    <pre><code>
    sudo pvcreate /dev/sdc
    sudo pvcreate /dev/sdd
    sudo pvcreate /dev/sde
    sudo pvcreate /dev/sdf
    </code></pre>
        * <span data-ttu-id="5b69f-232">Skapa en volym grupp för datafiler, en volym grupp för loggfilerna och en för den delade katalogen för SAP HANA</span><span class="sxs-lookup"><span data-stu-id="5b69f-232">Create a volume group for the data files, one volume group for the log files and one for the shared directory of SAP HANA</span></span>
    <pre><code>
    sudo vgcreate vg_hana_data /dev/sdc /dev/sdd
    sudo vgcreate vg_hana_log /dev/sde
    sudo vgcreate vg_hana_shared /dev/sdf
    </code></pre>
        * <span data-ttu-id="5b69f-233">Skapa logiska volymer</span><span class="sxs-lookup"><span data-stu-id="5b69f-233">Create the logical volumes</span></span>
    <pre><code>
    sudo lvcreate -l 100%FREE -n hana_data vg_hana_data
    sudo lvcreate -l 100%FREE -n hana_log vg_hana_log
    sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared
    sudo mkfs.xfs /dev/vg_hana_data/hana_data
    sudo mkfs.xfs /dev/vg_hana_log/hana_log
    sudo mkfs.xfs /dev/vg_hana_shared/hana_shared
    </code></pre>
        * <span data-ttu-id="5b69f-234">Skapa mount-kataloger och kopiera alla logiska volymer UUID</span><span class="sxs-lookup"><span data-stu-id="5b69f-234">Create the mount directories and copy the UUID of all logical volumes</span></span>
    <pre><code>
    sudo mkdir -p /hana/data
    sudo mkdir -p /hana/log
    sudo mkdir -p /hana/shared
    # write down the id of /dev/vg_hana_data/hana_data, /dev/vg_hana_log/hana_log and /dev/vg_hana_shared/hana_shared
    sudo blkid
    </code></pre>
        * <span data-ttu-id="5b69f-235">Skapa fstab poster för de tre logiska volymerna</span><span class="sxs-lookup"><span data-stu-id="5b69f-235">Create fstab entries for the three logical volumes</span></span>
    <pre><code>
    sudo vi /etc/fstab
    </code></pre>
    <span data-ttu-id="5b69f-236">Infoga raden till /etc/fstab</span><span class="sxs-lookup"><span data-stu-id="5b69f-236">Insert this line to /etc/fstab</span></span>
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_data/hana_data&gt;</b> /hana/data xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_log/hana_log&gt;</b> /hana/log xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_shared/hana_shared&gt;</b> /hana/shared xfs  defaults,nofail  0  2
    </code></pre>
        * <span data-ttu-id="5b69f-237">Montera nya volymer</span><span class="sxs-lookup"><span data-stu-id="5b69f-237">Mount the new volumes</span></span>
    <pre><code>
    sudo mount -a
    </code></pre>
    1. <span data-ttu-id="5b69f-238">Vanlig diskar</span><span class="sxs-lookup"><span data-stu-id="5b69f-238">Plain Disks</span></span>  
       <span data-ttu-id="5b69f-239">För små eller demonstrera system, du kan placera HANA data och loggfilen filer på en disk.</span><span class="sxs-lookup"><span data-stu-id="5b69f-239">For small or demo systems, you can place your HANA data and log files on one disk.</span></span> <span data-ttu-id="5b69f-240">Följande kommandon skapar en partition på /dev/sdc och formatera den med xfs.</span><span class="sxs-lookup"><span data-stu-id="5b69f-240">The following commands create a partition on /dev/sdc and format it with xfs.</span></span>
    ```bash
    sudo fdisk /dev/sdc
    sudo mkfs.xfs /dev/sdc1
    
    # <a name="write-down-the-id-of-devsdc1"></a><span data-ttu-id="5b69f-241">Anteckna id för /dev/sdc1</span><span class="sxs-lookup"><span data-stu-id="5b69f-241">write down the id of /dev/sdc1</span></span>
    <span data-ttu-id="5b69f-242">sudo/sbin/blkid sudo vi/etc/fstab</span><span class="sxs-lookup"><span data-stu-id="5b69f-242">sudo /sbin/blkid  sudo vi /etc/fstab</span></span>
    ```

    Insert this line to /etc/fstab
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID&gt;</b> /hana xfs  defaults,nofail  0  2
    </code></pre>

    Create the target directory and mount the disk.

    ```bash
    sudo mkdir /hana
    sudo mount -a
    ```

1. <span data-ttu-id="5b69f-243">[A] konfigurera namnmatchning för alla värdar</span><span class="sxs-lookup"><span data-stu-id="5b69f-243">[A] Setup host name resolution for all hosts</span></span>  
    <span data-ttu-id="5b69f-244">Du kan använda en DNS-server, eller så kan du ändra de/etc/hosts på alla noder.</span><span class="sxs-lookup"><span data-stu-id="5b69f-244">You can either use a DNS server or modify the /etc/hosts on all nodes.</span></span> <span data-ttu-id="5b69f-245">Det här exemplet visar hur du använder/etc/hosts-filen.</span><span class="sxs-lookup"><span data-stu-id="5b69f-245">This example shows how to use the /etc/hosts file.</span></span>
   <span data-ttu-id="5b69f-246">Ersätt IP-adressen och värdnamnet i följande kommandon</span><span class="sxs-lookup"><span data-stu-id="5b69f-246">Replace the IP address and the hostname in the following commands</span></span>
    ```bash
    sudo vi /etc/hosts
    ```
    <span data-ttu-id="5b69f-247">Infoga följande rader till/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="5b69f-247">Insert the following lines to /etc/hosts.</span></span> <span data-ttu-id="5b69f-248">Ändra IP-adressen och värdnamnet som matchar din miljö</span><span class="sxs-lookup"><span data-stu-id="5b69f-248">Change the IP address and hostname to match your environment</span></span>    
    
    <pre><code>
    <b>&lt;IP address of host 1&gt; &lt;hostname of host 1&gt;</b>
    <b>&lt;IP address of host 2&gt; &lt;hostname of host 2&gt;</b>
    </code></pre>

1. <span data-ttu-id="5b69f-249">[1] installera kluster</span><span class="sxs-lookup"><span data-stu-id="5b69f-249">[1] Install Cluster</span></span>
    ```bash
    sudo ha-cluster-init
    
    # Do you want to continue anyway? [y/N] -> y
    # Network address to bind to (e.g.: 192.168.1.0) [10.79.227.0] -> ENTER
    # Multicast address (e.g.: 239.x.x.x) [239.174.218.125] -> ENTER
    # Multicast port [5405] -> ENTER
    # Do you wish to use SBD? [y/N] -> N
    # Do you wish to configure an administration IP? [y/N] -> N
    ```
        
1. <span data-ttu-id="5b69f-250">[2] Lägg till nod i klustret</span><span class="sxs-lookup"><span data-stu-id="5b69f-250">[2] Add node to cluster</span></span>
    ```bash
    sudo ha-cluster-join
        
    # WARNING: NTP is not configured to start at system boot.
    # WARNING: No watchdog device found. If SBD is used, the cluster will be unable to start without a watchdog.
    # Do you want to continue anyway? [y/N] -> y
    # IP address or hostname of existing node (e.g.: 192.168.1.1) [] -> IP address of node 1 e.g. 10.0.0.5
    # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
    ```

1. <span data-ttu-id="5b69f-251">[A] ändra hacluster lösenord till samma lösenord</span><span class="sxs-lookup"><span data-stu-id="5b69f-251">[A] Change hacluster password to the same password</span></span>
    ```bash
    sudo passwd hacluster
    
    ```

1. <span data-ttu-id="5b69f-252">[A] konfigurera corosync om du vill använda andra transport och lägga till nodelist.</span><span class="sxs-lookup"><span data-stu-id="5b69f-252">[A] Configure corosync to use other transport and add nodelist.</span></span> <span data-ttu-id="5b69f-253">Klustret fungerar inte på annat sätt.</span><span class="sxs-lookup"><span data-stu-id="5b69f-253">Cluster will not work otherwise.</span></span>
    ```bash
    sudo vi /etc/corosync/corosync.conf    
    
    ```

    <span data-ttu-id="5b69f-254">Lägg till följande fetstil innehåll i filen.</span><span class="sxs-lookup"><span data-stu-id="5b69f-254">Add the following bold content to the file.</span></span>
    
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

    <span data-ttu-id="5b69f-255">Starta om tjänsten corosync</span><span class="sxs-lookup"><span data-stu-id="5b69f-255">Then restart the corosync service</span></span>

    ```bash
    sudo service corosync restart
    
    ```

1. <span data-ttu-id="5b69f-256">[A] Installationspaketen HANA hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="5b69f-256">[A] Install HANA HA packages</span></span>  
    ```bash
    sudo zypper install SAPHanaSR
    
    ```

## <a name="installing-sap-hana"></a><span data-ttu-id="5b69f-257">Installera SAP HANA</span><span class="sxs-lookup"><span data-stu-id="5b69f-257">Installing SAP HANA</span></span>

<span data-ttu-id="5b69f-258">Följ kapitel 4 i den [SAP HANA SR prestanda optimerade scenariot guiden] [ suse-hana-ha-guide] att installera SAP HANA System replikering.</span><span class="sxs-lookup"><span data-stu-id="5b69f-258">Follow chapter 4 of the [SAP HANA SR Performance Optimized Scenario guide][suse-hana-ha-guide] to install SAP HANA System Replication.</span></span>

1. <span data-ttu-id="5b69f-259">[A] kör hdblcm från DVD: n HANA</span><span class="sxs-lookup"><span data-stu-id="5b69f-259">[A] Run hdblcm from the HANA DVD</span></span>
    * <span data-ttu-id="5b69f-260">Välj installation -> 1</span><span class="sxs-lookup"><span data-stu-id="5b69f-260">Choose installation -> 1</span></span>
    * <span data-ttu-id="5b69f-261">Välj ytterligare komponenter för installation -> 1</span><span class="sxs-lookup"><span data-stu-id="5b69f-261">Select additional components for installation -> 1</span></span>
    * <span data-ttu-id="5b69f-262">Ange installationssökväg [/ hana/delade]: RETUR -></span><span class="sxs-lookup"><span data-stu-id="5b69f-262">Enter Installation Path [/hana/shared]: -> ENTER</span></span>
    * <span data-ttu-id="5b69f-263">Ange lokala värdnamn [.]: -> RETUR</span><span class="sxs-lookup"><span data-stu-id="5b69f-263">Enter Local Host Name [..]: -> ENTER</span></span>
    * <span data-ttu-id="5b69f-264">Vill du lägga till ytterligare värdar systemet?</span><span class="sxs-lookup"><span data-stu-id="5b69f-264">Do you want to add additional hosts to the system?</span></span> <span data-ttu-id="5b69f-265">(j/n) [n]: RETUR -></span><span class="sxs-lookup"><span data-stu-id="5b69f-265">(y/n) [n]: -> ENTER</span></span>
    * <span data-ttu-id="5b69f-266">Ange SAP HANA System-ID:<SID of HANA e.g. HDB></span><span class="sxs-lookup"><span data-stu-id="5b69f-266">Enter SAP HANA System ID: <SID of HANA e.g. HDB></span></span>
    * <span data-ttu-id="5b69f-267">Ange instansnummer [00]:</span><span class="sxs-lookup"><span data-stu-id="5b69f-267">Enter Instance Number [00]:</span></span>   
  <span data-ttu-id="5b69f-268">HANA instansnummer.</span><span class="sxs-lookup"><span data-stu-id="5b69f-268">HANA Instance number.</span></span> <span data-ttu-id="5b69f-269">Använd 03 om du har använt Azure-mall eller följt exemplet ovan</span><span class="sxs-lookup"><span data-stu-id="5b69f-269">Use 03 if you used the Azure Template or followed the example above</span></span>
    * <span data-ttu-id="5b69f-270">Välj läge för databasen / ange Index [1]: -> RETUR</span><span class="sxs-lookup"><span data-stu-id="5b69f-270">Select Database Mode / Enter Index [1]: -> ENTER</span></span>
    * <span data-ttu-id="5b69f-271">Välj systemanvändning / ange Index [4]:</span><span class="sxs-lookup"><span data-stu-id="5b69f-271">Select System Usage / Enter Index [4]:</span></span>  
  <span data-ttu-id="5b69f-272">Välj system användning</span><span class="sxs-lookup"><span data-stu-id="5b69f-272">Select the system Usage</span></span>
    * <span data-ttu-id="5b69f-273">Ange platsen för datavolymer [/ hana/data/HDB]: RETUR -></span><span class="sxs-lookup"><span data-stu-id="5b69f-273">Enter Location of Data Volumes [/hana/data/HDB]: -> ENTER</span></span>
    * <span data-ttu-id="5b69f-274">Ange platsen för Loggvolymer [/ hana/log/HDB]: RETUR -></span><span class="sxs-lookup"><span data-stu-id="5b69f-274">Enter Location of Log Volumes [/hana/log/HDB]: -> ENTER</span></span>
    * <span data-ttu-id="5b69f-275">Begränsa maximal minnesallokering?</span><span class="sxs-lookup"><span data-stu-id="5b69f-275">Restrict maximum memory allocation?</span></span> <span data-ttu-id="5b69f-276">[n]: RETUR -></span><span class="sxs-lookup"><span data-stu-id="5b69f-276">[n]: -> ENTER</span></span>
    * <span data-ttu-id="5b69f-277">Ange värdnamnet för certifikat för värden ”...”</span><span class="sxs-lookup"><span data-stu-id="5b69f-277">Enter Certificate Host Name For Host '...'</span></span> <span data-ttu-id="5b69f-278">[...]: RETUR -></span><span class="sxs-lookup"><span data-stu-id="5b69f-278">[...]: -> ENTER</span></span>
    * <span data-ttu-id="5b69f-279">Ange SAP värden Agent användare (sapadm) lösenord:</span><span class="sxs-lookup"><span data-stu-id="5b69f-279">Enter SAP Host Agent User (sapadm) Password:</span></span>
    * <span data-ttu-id="5b69f-280">Bekräfta SAP värden Agent användare (sapadm) lösenord:</span><span class="sxs-lookup"><span data-stu-id="5b69f-280">Confirm SAP Host Agent User (sapadm) Password:</span></span>
    * <span data-ttu-id="5b69f-281">Ange systemadministratören (hdbadm) lösenord:</span><span class="sxs-lookup"><span data-stu-id="5b69f-281">Enter System Administrator (hdbadm) Password:</span></span>
    * <span data-ttu-id="5b69f-282">Bekräfta systemadministratören (hdbadm) lösenord:</span><span class="sxs-lookup"><span data-stu-id="5b69f-282">Confirm System Administrator (hdbadm) Password:</span></span>
    * <span data-ttu-id="5b69f-283">Ange System administratör arbetskatalog [/ usr/sap/HDB/hem]: RETUR -></span><span class="sxs-lookup"><span data-stu-id="5b69f-283">Enter System Administrator Home Directory [/usr/sap/HDB/home]: -> ENTER</span></span>
    * <span data-ttu-id="5b69f-284">Ange System inloggningsgränssnitt för administratör [/ bin/del]: RETUR -></span><span class="sxs-lookup"><span data-stu-id="5b69f-284">Enter System Administrator Login Shell [/bin/sh]: -> ENTER</span></span>
    * <span data-ttu-id="5b69f-285">Ange systemadministratören användar-ID [1001]: -> RETUR</span><span class="sxs-lookup"><span data-stu-id="5b69f-285">Enter System Administrator User ID [1001]: -> ENTER</span></span>
    * <span data-ttu-id="5b69f-286">Ange ID för användargrupp (sapsys) [79]: RETUR -></span><span class="sxs-lookup"><span data-stu-id="5b69f-286">Enter ID of User Group (sapsys) [79]: -> ENTER</span></span>
    * <span data-ttu-id="5b69f-287">Ange lösenord för användare (SYSTEM) av databasen:</span><span class="sxs-lookup"><span data-stu-id="5b69f-287">Enter Database User (SYSTEM) Password:</span></span>
    * <span data-ttu-id="5b69f-288">Bekräfta databas användarlösenord (SYSTEM):</span><span class="sxs-lookup"><span data-stu-id="5b69f-288">Confirm Database User (SYSTEM) Password:</span></span>
    * <span data-ttu-id="5b69f-289">Starta om systemet efter omstart av datorn?</span><span class="sxs-lookup"><span data-stu-id="5b69f-289">Restart system after machine reboot?</span></span> <span data-ttu-id="5b69f-290">[n]: RETUR -></span><span class="sxs-lookup"><span data-stu-id="5b69f-290">[n]: -> ENTER</span></span>
    * <span data-ttu-id="5b69f-291">Vill du fortsätta?</span><span class="sxs-lookup"><span data-stu-id="5b69f-291">Do you want to continue?</span></span> <span data-ttu-id="5b69f-292">(j/n):</span><span class="sxs-lookup"><span data-stu-id="5b69f-292">(y/n):</span></span>  
  <span data-ttu-id="5b69f-293">Kontrollera sammanfattningen och ange y om du vill fortsätta</span><span class="sxs-lookup"><span data-stu-id="5b69f-293">Validate the summary and enter y to continue</span></span>
1. <span data-ttu-id="5b69f-294">[A] uppgradera SAP Värdagenten</span><span class="sxs-lookup"><span data-stu-id="5b69f-294">[A] Upgrade SAP Host Agent</span></span>  
  <span data-ttu-id="5b69f-295">Hämta senaste SAP Värdagenten arkivet från den [SAP Softwarecenter] [ sap-swcenter] och kör följande kommando för att uppgradera agenten.</span><span class="sxs-lookup"><span data-stu-id="5b69f-295">Download the latest SAP Host Agent archive from the [SAP Softwarecenter][sap-swcenter] and run the following command to upgrade the agent.</span></span> <span data-ttu-id="5b69f-296">Ersätt sökvägen till arkivet så att den pekar till den fil som du hämtat.</span><span class="sxs-lookup"><span data-stu-id="5b69f-296">Replace the path to the archive to point to the file you downloaded.</span></span>
    ```bash
    sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <path to SAP Host Agent SAR>
    ```

1. <span data-ttu-id="5b69f-297">[1] Skapa HANA replikering (som rot)</span><span class="sxs-lookup"><span data-stu-id="5b69f-297">[1] Create HANA replication (as root)</span></span>  
    <span data-ttu-id="5b69f-298">Kör följande kommando.</span><span class="sxs-lookup"><span data-stu-id="5b69f-298">Run the following command.</span></span> <span data-ttu-id="5b69f-299">Se till att ersätta fetstil strängar (HANA System-ID HDB och instans antalet 03) med värden för SAP HANA-installationen.</span><span class="sxs-lookup"><span data-stu-id="5b69f-299">Make sure to replace bold strings (HANA System ID HDB and instance number 03) with the values of your SAP HANA installation.</span></span>
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
    hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN TO <b>hdb</b>hasync' 
    hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
    </code></pre>

1. <span data-ttu-id="5b69f-300">[A] Skapa keystore-post (som rot)</span><span class="sxs-lookup"><span data-stu-id="5b69f-300">[A] Create keystore entry (as root)</span></span>
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>passwd</b>
    </code></pre>
1. <span data-ttu-id="5b69f-301">[1] backup database (som rot)</span><span class="sxs-lookup"><span data-stu-id="5b69f-301">[1] Backup database (as root)</span></span>
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')" 
    </code></pre>
1. <span data-ttu-id="5b69f-302">[1] växla till sapsid användare (till exempel hdbadm) och skapa den primära platsen.</span><span class="sxs-lookup"><span data-stu-id="5b69f-302">[1] Switch to the sapsid user (for example hdbadm) and create the primary site.</span></span>
    <pre><code>
    su - <b>hdb</b>adm
    hdbnsutil -sr_enable –-name=<b>SITE1</b>
    </code></pre>
1. <span data-ttu-id="5b69f-303">[2] växla till sapsid användare (till exempel hdbadm) och skapa en sekundär plats.</span><span class="sxs-lookup"><span data-stu-id="5b69f-303">[2] Switch to the sapsid user (for example hdbadm) and create the secondary site.</span></span>
    <pre><code>
    su - <b>hdb</b>adm
    sapcontrol -nr <b>03</b> -function StopWait 600 10
    hdbnsutil -sr_register --remoteHost=<b>saphanavm1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
    </code></pre>

## <a name="configure-cluster-framework"></a><span data-ttu-id="5b69f-304">Konfigurera klustret Framework</span><span class="sxs-lookup"><span data-stu-id="5b69f-304">Configure Cluster Framework</span></span>

<span data-ttu-id="5b69f-305">Ändra standardinställningarna</span><span class="sxs-lookup"><span data-stu-id="5b69f-305">Change the default settings</span></span>

<pre>
sudo vi crm-defaults.txt
# enter the following to crm-defaults.txt
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

# <a name="now-we-load-the-file-to-the-cluster"></a><span data-ttu-id="5b69f-306">Vi kan nu läsa in filen i klustret</span><span class="sxs-lookup"><span data-stu-id="5b69f-306">now we load the file to the cluster</span></span>
<span data-ttu-id="5b69f-307">sudo crm konfigurera belastningen update crm-defaults.txt</span><span class="sxs-lookup"><span data-stu-id="5b69f-307">sudo crm configure load update crm-defaults.txt</span></span>
</pre>

### <a name="create-stonith-device"></a><span data-ttu-id="5b69f-308">Skapa STONITH enhet</span><span class="sxs-lookup"><span data-stu-id="5b69f-308">Create STONITH device</span></span>

<span data-ttu-id="5b69f-309">STONITH enheten använder ett huvudnamn för tjänsten för att godkänna mot Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="5b69f-309">The STONITH device uses a Service Principal to authorize against Microsoft Azure.</span></span> <span data-ttu-id="5b69f-310">Följ dessa steg för att skapa ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5b69f-310">Please follow these steps to create a Service Principal.</span></span>

1. <span data-ttu-id="5b69f-311">Gå till <https://portal.azure.com></span><span class="sxs-lookup"><span data-stu-id="5b69f-311">Go to <https://portal.azure.com></span></span>
1. <span data-ttu-id="5b69f-312">Öppna bladet Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5b69f-312">Open the Azure Active Directory blade</span></span>  
   <span data-ttu-id="5b69f-313">Gå till egenskaperna och Skriv ned Directory-Id.</span><span class="sxs-lookup"><span data-stu-id="5b69f-313">Go to Properties and write down the Directory Id.</span></span> <span data-ttu-id="5b69f-314">Det här är den **klient-id**.</span><span class="sxs-lookup"><span data-stu-id="5b69f-314">This is the **tenant id**.</span></span>
1. <span data-ttu-id="5b69f-315">Klicka på appen registreringar</span><span class="sxs-lookup"><span data-stu-id="5b69f-315">Click App registrations</span></span>
1. <span data-ttu-id="5b69f-316">Klicka på Lägg till</span><span class="sxs-lookup"><span data-stu-id="5b69f-316">Click Add</span></span>
1. <span data-ttu-id="5b69f-317">Ange ett namn, Välj typ av program ”Web app/API”, ange en inloggnings-URL (till exempel http://localhost) och klicka på Skapa</span><span class="sxs-lookup"><span data-stu-id="5b69f-317">Enter a Name, select Application Type "Web app/API", enter a sign-on URL (for example http://localhost) and click Create</span></span>
1. <span data-ttu-id="5b69f-318">URL för inloggning används inte och kan vara en giltig URL</span><span class="sxs-lookup"><span data-stu-id="5b69f-318">The sign-on URL is not used and can be any valid URL</span></span>
1. <span data-ttu-id="5b69f-319">Välj den nya appen och klicka på nycklar på fliken Inställningar</span><span class="sxs-lookup"><span data-stu-id="5b69f-319">Select the new App and click Keys in the Settings tab</span></span>
1. <span data-ttu-id="5b69f-320">Ange en beskrivning för en ny nyckel, Välj ”upphör aldrig att gälla” och klicka på Spara</span><span class="sxs-lookup"><span data-stu-id="5b69f-320">Enter a description for a new key, select "Never expires" and click Save</span></span>
1. <span data-ttu-id="5b69f-321">Anteckna värdet.</span><span class="sxs-lookup"><span data-stu-id="5b69f-321">Write down the Value.</span></span> <span data-ttu-id="5b69f-322">Den används som den **lösenord** för tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="5b69f-322">It is used as the **password** for the Service Principal</span></span>
1. <span data-ttu-id="5b69f-323">Skriv ned det program-Id.</span><span class="sxs-lookup"><span data-stu-id="5b69f-323">Write down the Application Id.</span></span> <span data-ttu-id="5b69f-324">Den används som användarnamnet (**inloggnings-id** i stegen nedan) för tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="5b69f-324">It is used as the username (**login id** in the steps below) of the Service Principal</span></span>

<span data-ttu-id="5b69f-325">Tjänstens huvudnamn har inte behörighet att komma åt Azure-resurser som standard.</span><span class="sxs-lookup"><span data-stu-id="5b69f-325">The Service Principal does not have permissions to access your Azure resources by default.</span></span> <span data-ttu-id="5b69f-326">Du behöver ge behörighet att starta och stoppa tjänstens huvudnamn (frigöra) alla virtuella datorer i klustret.</span><span class="sxs-lookup"><span data-stu-id="5b69f-326">You need to give the Service Principal permissions to start and stop (deallocate) all virtual machines of the cluster.</span></span>

1. <span data-ttu-id="5b69f-327">Gå till https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="5b69f-327">Go to https://portal.azure.com</span></span>
1. <span data-ttu-id="5b69f-328">Öppna bladet alla resurser</span><span class="sxs-lookup"><span data-stu-id="5b69f-328">Open the All resources blade</span></span>
1. <span data-ttu-id="5b69f-329">Välj den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="5b69f-329">Select the virtual machine</span></span>
1. <span data-ttu-id="5b69f-330">Klicka på åtkomstkontroll (IAM)</span><span class="sxs-lookup"><span data-stu-id="5b69f-330">Click Access control (IAM)</span></span>
1. <span data-ttu-id="5b69f-331">Klicka på Lägg till</span><span class="sxs-lookup"><span data-stu-id="5b69f-331">Click Add</span></span>
1. <span data-ttu-id="5b69f-332">Välj rollen ägare</span><span class="sxs-lookup"><span data-stu-id="5b69f-332">Select the role Owner</span></span>
1. <span data-ttu-id="5b69f-333">Ange namnet på programmet som du skapade ovan</span><span class="sxs-lookup"><span data-stu-id="5b69f-333">Enter the name of the application you created above</span></span>
1. <span data-ttu-id="5b69f-334">Klicka på OK</span><span class="sxs-lookup"><span data-stu-id="5b69f-334">Click OK</span></span>

<span data-ttu-id="5b69f-335">När du har redigerat behörigheter för de virtuella datorerna kan du konfigurera STONITH-enheter i klustret.</span><span class="sxs-lookup"><span data-stu-id="5b69f-335">After you edited the permissions for the virtual machines, you can configure the STONITH devices in the cluster.</span></span>

<pre>
sudo vi crm-fencing.txt
# enter the following to crm-fencing.txt
# replace the bold string with your subscription id, resource group, tenant id, service principal id and password
<code>
primitive rsc_st_azure_1 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

primitive rsc_st_azure_2 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started
</code>

# <a name="now-we-load-the-file-to-the-cluster"></a><span data-ttu-id="5b69f-336">Vi kan nu läsa in filen i klustret</span><span class="sxs-lookup"><span data-stu-id="5b69f-336">now we load the file to the cluster</span></span>
<span data-ttu-id="5b69f-337">sudo crm konfigurera belastningen update crm-fencing.txt</span><span class="sxs-lookup"><span data-stu-id="5b69f-337">sudo crm configure load update crm-fencing.txt</span></span>
</pre>

### <a name="create-sap-hana-resources"></a><span data-ttu-id="5b69f-338">Skapa SAP HANA-resurser</span><span class="sxs-lookup"><span data-stu-id="5b69f-338">Create SAP HANA resources</span></span>

<pre>
sudo vi crm-saphanatop.txt
# enter the following to crm-saphana.txt
# replace the bold string with your instance number and HANA system id
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

# <a name="now-we-load-the-file-to-the-cluster"></a><span data-ttu-id="5b69f-339">Vi kan nu läsa in filen i klustret</span><span class="sxs-lookup"><span data-stu-id="5b69f-339">now we load the file to the cluster</span></span>
<span data-ttu-id="5b69f-340">sudo crm konfigurera belastningen update crm-saphanatop.txt</span><span class="sxs-lookup"><span data-stu-id="5b69f-340">sudo crm configure load update crm-saphanatop.txt</span></span>
</pre>

<pre>
sudo vi crm-saphana.txt
# enter the following to crm-saphana.txt
# replace the bold string with your instance number, HANA system id and the frontend IP address of the Azure load balancer. 
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

# <a name="now-we-load-the-file-to-the-cluster"></a><span data-ttu-id="5b69f-341">Vi kan nu läsa in filen i klustret</span><span class="sxs-lookup"><span data-stu-id="5b69f-341">now we load the file to the cluster</span></span>
<span data-ttu-id="5b69f-342">sudo crm konfigurera belastningen update crm-saphana.txt</span><span class="sxs-lookup"><span data-stu-id="5b69f-342">sudo crm configure load update crm-saphana.txt</span></span>
</pre>

### <a name="test-cluster-setup"></a><span data-ttu-id="5b69f-343">Inställningar för kluster</span><span class="sxs-lookup"><span data-stu-id="5b69f-343">Test cluster setup</span></span>
<span data-ttu-id="5b69f-344">Följande avsnittet beskriver hur du kan testa din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="5b69f-344">The following chapter describe how you can test your setup.</span></span> <span data-ttu-id="5b69f-345">Varje test förutsätter att du är rot och SAP HANA master körs på den virtuella datorn saphanavm1.</span><span class="sxs-lookup"><span data-stu-id="5b69f-345">Every test assumes that you are root and the SAP HANA master is running on the virtual machine saphanavm1.</span></span>

#### <a name="fencing-test"></a><span data-ttu-id="5b69f-346">Avgränsningar Test</span><span class="sxs-lookup"><span data-stu-id="5b69f-346">Fencing Test</span></span>

<span data-ttu-id="5b69f-347">Du kan testa installationen av agenten avgränsningar genom att inaktivera nätverksgränssnittet på noden saphanavm1.</span><span class="sxs-lookup"><span data-stu-id="5b69f-347">You can test the setup of the fencing agent by disabling the network interface on node saphanavm1.</span></span>

<pre><code>
sudo ifdown eth0
</code></pre>

<span data-ttu-id="5b69f-348">Den virtuella datorn bör nu hämta startas om eller stoppas beroende på klusterkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="5b69f-348">The virtual machine should now get restarted or stopped depending on your cluster configuration.</span></span>
<span data-ttu-id="5b69f-349">Om du ställer in stonith-åtgärd till av den virtuella datorn kommer att stoppas och resurserna som migreras till den virtuella datorn som körs.</span><span class="sxs-lookup"><span data-stu-id="5b69f-349">If you set the stonith-action to off, the virtual machine will be stopped and the resources are migrated to the running virtual machine.</span></span>

<span data-ttu-id="5b69f-350">När du startar den virtuella datorn igen SAP HANA-resurs inte längre att fungera som sekundär om du ställer in AUTOMATED_REGISTER = ”false”.</span><span class="sxs-lookup"><span data-stu-id="5b69f-350">Once you start the virtual machine again, the SAP HANA resource will fail to start as secondary if you set AUTOMATED_REGISTER="false".</span></span> <span data-ttu-id="5b69f-351">I det här fallet måste du konfigurera HANA-instans som sekundär genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5b69f-351">In this case, you need to configure the HANA instance as secondary by executing the following command:</span></span>

<pre><code>
su - <b>hdb</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>

# switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-manual-failover"></a><span data-ttu-id="5b69f-352">Testa en manuell växling</span><span class="sxs-lookup"><span data-stu-id="5b69f-352">Testing a manual failover</span></span>

<span data-ttu-id="5b69f-353">Du kan testa en manuell växling genom att stoppa tjänsten pacemaker på noden saphanavm1.</span><span class="sxs-lookup"><span data-stu-id="5b69f-353">You can test a manual failover by stopping the pacemaker service on node saphanavm1.</span></span>
<pre><code>
service pacemaker stop
</code></pre>

<span data-ttu-id="5b69f-354">Efter växling vid fel, kan du starta tjänsten igen.</span><span class="sxs-lookup"><span data-stu-id="5b69f-354">After the failover, you can start the service again.</span></span> <span data-ttu-id="5b69f-355">SAP HANA-resursen på saphanavm1 inte längre att fungera som sekundär om du ställer in AUTOMATED_REGISTER = ”false”.</span><span class="sxs-lookup"><span data-stu-id="5b69f-355">The SAP HANA resource on saphanavm1 will fail to start as secondary if you set AUTOMATED_REGISTER="false".</span></span> <span data-ttu-id="5b69f-356">I det här fallet måste du konfigurera HANA-instans som sekundär genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5b69f-356">In this case, you need to configure the HANA instance as secondary by executing the following command:</span></span>

<pre><code>
service pacemaker start
su - <b>hdb</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 


# switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-migration"></a><span data-ttu-id="5b69f-357">Testa en migrering</span><span class="sxs-lookup"><span data-stu-id="5b69f-357">Testing a migration</span></span>

<span data-ttu-id="5b69f-358">Du kan migrera SAP HANA huvudnoden genom att köra följande kommando</span><span class="sxs-lookup"><span data-stu-id="5b69f-358">You can migrate the SAP HANA master node by executing the following command</span></span>
<pre><code>
crm resource migrate msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
crm resource migrate g_ip_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
</code></pre>

<span data-ttu-id="5b69f-359">Detta bör migrera SAP HANA-huvudnod och den grupp som innehåller saphanavm2 virtuella IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="5b69f-359">This should migrate the SAP HANA master node and the group that contains the virtual IP address to saphanavm2.</span></span>
<span data-ttu-id="5b69f-360">SAP HANA-resursen på saphanavm1 inte längre att fungera som sekundär om du ställer in AUTOMATED_REGISTER = ”false”.</span><span class="sxs-lookup"><span data-stu-id="5b69f-360">The SAP HANA resource on saphanavm1 will fail to start as secondary if you set AUTOMATED_REGISTER="false".</span></span> <span data-ttu-id="5b69f-361">I det här fallet måste du konfigurera HANA-instans som sekundär genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5b69f-361">In this case, you need to configure the HANA instance as secondary by executing the following command:</span></span>

<pre><code>
su - <b>hdb</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 
</code></pre>

<span data-ttu-id="5b69f-362">Migreringen skapar plats begränsningar som måste tas bort igen.</span><span class="sxs-lookup"><span data-stu-id="5b69f-362">The migration creates location contraints that need to be deleted again.</span></span>

<pre><code>
crm configure edited

# delete location contraints that are named like the following contraint. You should have two contraints, one for the SAP HANA resource and one for the IP address group.
location cli-prefer-g_ip_<b>HDB</b>_HDB<b>03</b> g_ip_<b>HDB</b>_HDB<b>03</b> role=Started inf: <b>saphanavm2</b>
</code></pre>

<span data-ttu-id="5b69f-363">Du måste också rensa det sekundära noden tillståndet</span><span class="sxs-lookup"><span data-stu-id="5b69f-363">You also need to cleanup the state of the secondary node resource</span></span>

<pre><code>
# switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

## <a name="next-steps"></a><span data-ttu-id="5b69f-364">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5b69f-364">Next steps</span></span>
* <span data-ttu-id="5b69f-365">[Azure virtuella datorer planering och implementering för SAP][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="5b69f-365">[Azure Virtual Machines planning and implementation for SAP][planning-guide]</span></span>
* <span data-ttu-id="5b69f-366">[Distribution av Azure virtuella datorer för SAP][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="5b69f-366">[Azure Virtual Machines deployment for SAP][deployment-guide]</span></span>
* <span data-ttu-id="5b69f-367">[Azure virtuella datorer DBMS-distribution för SAP][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="5b69f-367">[Azure Virtual Machines DBMS deployment for SAP][dbms-guide]</span></span>
* <span data-ttu-id="5b69f-368">Information om hur du upprättar och planera för katastrofåterställning för SAP HANA i Azure (stora instanser) med hög tillgänglighet finns [SAP HANA (stora instanser) hög tillgänglighet och katastrofåterställning recovery på Azure](hana-overview-high-availability-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="5b69f-368">To learn how to establish high availability and plan for disaster recovery of SAP HANA on Azure (large instances), see [SAP HANA (large instances) high availability and disaster recovery on Azure](hana-overview-high-availability-disaster-recovery.md).</span></span> 
