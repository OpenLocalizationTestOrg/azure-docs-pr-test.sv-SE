---
title: "aaaAzure virtuella datorer hög tillgänglighet för SAP NetWeaver på SUSE Linux Enterprise Server för SAP-program | Microsoft Docs"
description: "Hög tillgänglighet guide för SAP NetWeaver på SUSE Linux Enterprise Server för SAP-program"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: mssedusch
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: sedusch
ms.openlocfilehash: e944103df92d5ffec9196189f138e25972bea79f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms-on-suse-linux-enterprise-server-for-sap-applications"></a><span data-ttu-id="413bb-103">Hög tillgänglighet för SAP NetWeaver på Azure Virtual Machines på SUSE Linux Enterprise Server för SAP-program</span><span class="sxs-lookup"><span data-stu-id="413bb-103">High availability for SAP NetWeaver on Azure VMs on SUSE Linux Enterprise Server for SAP applications</span></span>

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
[1410736]:https://launchpad.support.sap.com/#/notes/1410736

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[suse-drbd-guide]:https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha_techguides/book_sleha_techguides.html

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged-md%2Fazuredeploy.json
[template-file-server]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-file-server-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability.md

<span data-ttu-id="413bb-113">Den här artikeln beskrivs hur toodeploy hello virtuella datorer, konfigurera hello virtuella datorer, installera hello klustret framework och installera en högtillgänglig SAP NetWeaver 7,50.</span><span class="sxs-lookup"><span data-stu-id="413bb-113">This article describes how toodeploy hello virtual machines, configure hello virtual machines, install hello cluster framework and install a highly available SAP NetWeaver 7.50 system.</span></span>
<span data-ttu-id="413bb-114">I hello exempelkonfigurationer installationskommandon osv. ASCS-instansnummer 00, ÄNDARE instansnummer 02 och SAP System-ID NWS används.</span><span class="sxs-lookup"><span data-stu-id="413bb-114">In hello example configurations, installation commands etc. ASCS instance number 00, ERS instance number 02 and SAP System ID NWS is used.</span></span> <span data-ttu-id="413bb-115">hello namnen på de hello resurser (till exempel virtuella datorer, virtuella nätverk) i hello exemplet förutsätter att du har använt hello [konvergerat mallen] [ template-converged] med SAP ID NWS toocreate hello systemresurser.</span><span class="sxs-lookup"><span data-stu-id="413bb-115">hello names of hello resources (for example virtual machines, virtual networks) in hello example assume that you have used hello [converged template][template-converged] with SAP system ID NWS toocreate hello resources.</span></span>

<span data-ttu-id="413bb-116">Läs hello först efter SAP anteckningar och dokument</span><span class="sxs-lookup"><span data-stu-id="413bb-116">Read hello following SAP Notes and papers first</span></span>

* <span data-ttu-id="413bb-117">SAP-kommentar [1928533], som innehåller:</span><span class="sxs-lookup"><span data-stu-id="413bb-117">SAP Note [1928533], which has:</span></span>
  * <span data-ttu-id="413bb-118">Lista över Azure VM-storlekar som stöds för hello distributionen av program</span><span class="sxs-lookup"><span data-stu-id="413bb-118">List of Azure VM sizes that are supported for hello deployment of SAP software</span></span>
  * <span data-ttu-id="413bb-119">Viktiga kapacitetsinformation för Azure VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="413bb-119">Important capacity information for Azure VM sizes</span></span>
  * <span data-ttu-id="413bb-120">Stöds SAP-programvara och operativsystem (OS) och kombinationer av databasen</span><span class="sxs-lookup"><span data-stu-id="413bb-120">Supported SAP software, and operating system (OS) and database combinations</span></span>
  * <span data-ttu-id="413bb-121">SAP kernel version som krävs för Windows och Linux i Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="413bb-121">Required SAP kernel version for Windows and Linux on Microsoft Azure</span></span>

* <span data-ttu-id="413bb-122">SAP-kommentar [2015553] listar kraven för stöd för SAP SAP programvarudistributioner i Azure.</span><span class="sxs-lookup"><span data-stu-id="413bb-122">SAP Note [2015553] lists prerequisites for SAP-supported SAP software deployments in Azure.</span></span>
* <span data-ttu-id="413bb-123">SAP-kommentar [2205917] har rekommenderat OS-inställningar för SUSE Linux Enterprise Server för SAP-program</span><span class="sxs-lookup"><span data-stu-id="413bb-123">SAP Note [2205917] has recommended OS settings for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="413bb-124">SAP-kommentar [1944799] har SAP HANA riktlinjer för SUSE Linux Enterprise Server för SAP-program</span><span class="sxs-lookup"><span data-stu-id="413bb-124">SAP Note [1944799] has SAP HANA Guidelines for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="413bb-125">SAP-kommentar [2178632] innehåller detaljerad information om all övervakning mått som rapporterats för SAP i Azure.</span><span class="sxs-lookup"><span data-stu-id="413bb-125">SAP Note [2178632] has detailed information about all monitoring metrics reported for SAP in Azure.</span></span>
* <span data-ttu-id="413bb-126">SAP-kommentar [2191498] hello krävs SAP värden Agent-version för Linux i Azure.</span><span class="sxs-lookup"><span data-stu-id="413bb-126">SAP Note [2191498] has hello required SAP Host Agent version for Linux in Azure.</span></span>
* <span data-ttu-id="413bb-127">SAP-kommentar [2243692] har licensieringsinformation SAP på Linux i Azure.</span><span class="sxs-lookup"><span data-stu-id="413bb-127">SAP Note [2243692] has information about SAP licensing on Linux in Azure.</span></span>
* <span data-ttu-id="413bb-128">SAP-kommentar [1984787] har allmän information om SUSE Linux Enterprise Server 12.</span><span class="sxs-lookup"><span data-stu-id="413bb-128">SAP Note [1984787] has general information about SUSE Linux Enterprise Server 12.</span></span>
* <span data-ttu-id="413bb-129">SAP-kommentar [1999351] innehåller ytterligare felsökningsinformation för hello Azure förbättrad övervakning av tillägget för SAP.</span><span class="sxs-lookup"><span data-stu-id="413bb-129">SAP Note [1999351] has additional troubleshooting information for hello Azure Enhanced Monitoring Extension for SAP.</span></span>
* <span data-ttu-id="413bb-130">[SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) har alla nödvändiga SAP anteckningar för Linux.</span><span class="sxs-lookup"><span data-stu-id="413bb-130">[SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) has all required SAP Notes for Linux.</span></span>
* <span data-ttu-id="413bb-131">[Azure virtuella datorer planering och implementering för SAP på Linux][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="413bb-131">[Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide]</span></span>
* <span data-ttu-id="413bb-132">[Distribution av Azure virtuella datorer för SAP på Linux (den här artikeln)][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="413bb-132">[Azure Virtual Machines deployment for SAP on Linux (this article)][deployment-guide]</span></span>
* <span data-ttu-id="413bb-133">[Azure virtuella datorer DBMS-distribution för SAP på Linux][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="413bb-133">[Azure Virtual Machines DBMS deployment for SAP on Linux][dbms-guide]</span></span>
* <span data-ttu-id="413bb-134">[Prestanda för SAP HANA SR optimerade Scenario][suse-hana-ha-guide]</span><span class="sxs-lookup"><span data-stu-id="413bb-134">[SAP HANA SR Performance Optimized Scenario][suse-hana-ha-guide]</span></span>  
  <span data-ttu-id="413bb-135">hello-guiden innehåller all nödvändig information tooset SAP HANA System replikering på lokalt.</span><span class="sxs-lookup"><span data-stu-id="413bb-135">hello guide contains all required information tooset up SAP HANA System Replication on-premises.</span></span> <span data-ttu-id="413bb-136">Använd den här guiden som utgångspunkt.</span><span class="sxs-lookup"><span data-stu-id="413bb-136">Use this guide as a baseline.</span></span>
* <span data-ttu-id="413bb-137">[Hög NFS lagringsutrymme med DRBD och Pacemaker] [ suse-drbd-guide] hello guiden innehåller all nödvändig information tooset upp en NFS-server med hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="413bb-137">[Highly Available NFS Storage with DRBD and Pacemaker][suse-drbd-guide] hello guide contains all required information tooset up a highly available NFS server.</span></span> <span data-ttu-id="413bb-138">Använd den här guiden som utgångspunkt.</span><span class="sxs-lookup"><span data-stu-id="413bb-138">Use this guide as a baseline.</span></span>


## <a name="overview"></a><span data-ttu-id="413bb-139">Översikt</span><span class="sxs-lookup"><span data-stu-id="413bb-139">Overview</span></span>

<span data-ttu-id="413bb-140">tooachieve hög tillgänglighet, SAP NetWeaver kräver en NFS-server.</span><span class="sxs-lookup"><span data-stu-id="413bb-140">tooachieve high availability, SAP NetWeaver requires an NFS server.</span></span> <span data-ttu-id="413bb-141">hello NFS-servern är konfigurerad i separata kluster och kan användas av flera SAP-system.</span><span class="sxs-lookup"><span data-stu-id="413bb-141">hello NFS server is configured in a separate cluster and can be used by multiple SAP systems.</span></span>

![Översikt över SAP NetWeaver hög tillgänglighet](./media/high-availability-guide-suse/img_001.png)

<span data-ttu-id="413bb-143">hello NFS-servern, SAP NetWeaver ASCS, SAP NetWeaver SCS, SAP NetWeaver ÄNDARE och hello SAP HANA-databas använda virtuella värdnamn och virtuella IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="413bb-143">hello NFS server, SAP NetWeaver ASCS, SAP NetWeaver SCS, SAP NetWeaver ERS and hello SAP HANA database use virtual hostname and virtual IP addresses.</span></span> <span data-ttu-id="413bb-144">På Azure är en belastningsutjämning obligatoriska toouse en virtuell IP-adress.</span><span class="sxs-lookup"><span data-stu-id="413bb-144">On Azure, a load balancer is required toouse a virtual IP address.</span></span> <span data-ttu-id="413bb-145">hello visar följande lista hello konfiguration av hello belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="413bb-145">hello following list shows hello configuration of hello load balancer.</span></span>

### <a name="nfs-server"></a><span data-ttu-id="413bb-146">NFS-Server</span><span class="sxs-lookup"><span data-stu-id="413bb-146">NFS Server</span></span>
* <span data-ttu-id="413bb-147">Konfiguration för klientdel</span><span class="sxs-lookup"><span data-stu-id="413bb-147">Frontend configuration</span></span>
  * <span data-ttu-id="413bb-148">IP-adressen 10.0.0.4</span><span class="sxs-lookup"><span data-stu-id="413bb-148">IP address 10.0.0.4</span></span>
* <span data-ttu-id="413bb-149">Backend-konfiguration</span><span class="sxs-lookup"><span data-stu-id="413bb-149">Backend configuration</span></span>
  * <span data-ttu-id="413bb-150">Ansluten tooprimary nätverksgränssnitt för alla virtuella datorer som ska ingå i hello NFS-kluster</span><span class="sxs-lookup"><span data-stu-id="413bb-150">Connected tooprimary network interfaces of all virtual machines that should be part of hello NFS cluster</span></span>
* <span data-ttu-id="413bb-151">Avsökningsport</span><span class="sxs-lookup"><span data-stu-id="413bb-151">Probe Port</span></span>
  * <span data-ttu-id="413bb-152">Port 61000</span><span class="sxs-lookup"><span data-stu-id="413bb-152">Port 61000</span></span>
* <span data-ttu-id="413bb-153">Regler för belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="413bb-153">Loadbalancing rules</span></span>
  * <span data-ttu-id="413bb-154">2049 TCP</span><span class="sxs-lookup"><span data-stu-id="413bb-154">2049 TCP</span></span> 
  * <span data-ttu-id="413bb-155">2049 UDP</span><span class="sxs-lookup"><span data-stu-id="413bb-155">2049 UDP</span></span>

### <a name="ascs"></a><span data-ttu-id="413bb-156">(A) SCS</span><span class="sxs-lookup"><span data-stu-id="413bb-156">(A)SCS</span></span>
* <span data-ttu-id="413bb-157">Konfiguration för klientdel</span><span class="sxs-lookup"><span data-stu-id="413bb-157">Frontend configuration</span></span>
  * <span data-ttu-id="413bb-158">IP-adress 10.0.0.10</span><span class="sxs-lookup"><span data-stu-id="413bb-158">IP address 10.0.0.10</span></span>
* <span data-ttu-id="413bb-159">Backend-konfiguration</span><span class="sxs-lookup"><span data-stu-id="413bb-159">Backend configuration</span></span>
  * <span data-ttu-id="413bb-160">Anslutna tooprimary nätverksgränssnitt för alla virtuella datorer som ska ingå i klustret för hello (A) SCS/ÄNDARE</span><span class="sxs-lookup"><span data-stu-id="413bb-160">Connected tooprimary network interfaces of all virtual machines that should be part of hello (A)SCS/ERS cluster</span></span>
* <span data-ttu-id="413bb-161">Avsökningsport</span><span class="sxs-lookup"><span data-stu-id="413bb-161">Probe Port</span></span>
  * <span data-ttu-id="413bb-162">Port 620**&lt;nr&gt;**</span><span class="sxs-lookup"><span data-stu-id="413bb-162">Port 620**&lt;nr&gt;**</span></span>
* <span data-ttu-id="413bb-163">Regler för belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="413bb-163">Loadbalancing rules</span></span>
  * <span data-ttu-id="413bb-164">32**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="413bb-164">32**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="413bb-165">36**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="413bb-165">36**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="413bb-166">39**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="413bb-166">39**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="413bb-167">81**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="413bb-167">81**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="413bb-168">5**&lt;nr&gt;**13 TCP</span><span class="sxs-lookup"><span data-stu-id="413bb-168">5**&lt;nr&gt;**13 TCP</span></span>
  * <span data-ttu-id="413bb-169">5**&lt;nr&gt;**14 TCP</span><span class="sxs-lookup"><span data-stu-id="413bb-169">5**&lt;nr&gt;**14 TCP</span></span>
  * <span data-ttu-id="413bb-170">5**&lt;nr&gt;**16 TCP</span><span class="sxs-lookup"><span data-stu-id="413bb-170">5**&lt;nr&gt;**16 TCP</span></span>

### <a name="ers"></a><span data-ttu-id="413bb-171">ÄNDARE</span><span class="sxs-lookup"><span data-stu-id="413bb-171">ERS</span></span>
* <span data-ttu-id="413bb-172">Konfiguration för klientdel</span><span class="sxs-lookup"><span data-stu-id="413bb-172">Frontend configuration</span></span>
  * <span data-ttu-id="413bb-173">IP-adress 10.0.0.11</span><span class="sxs-lookup"><span data-stu-id="413bb-173">IP address 10.0.0.11</span></span>
* <span data-ttu-id="413bb-174">Backend-konfiguration</span><span class="sxs-lookup"><span data-stu-id="413bb-174">Backend configuration</span></span>
  * <span data-ttu-id="413bb-175">Anslutna tooprimary nätverksgränssnitt för alla virtuella datorer som ska ingå i klustret för hello (A) SCS/ÄNDARE</span><span class="sxs-lookup"><span data-stu-id="413bb-175">Connected tooprimary network interfaces of all virtual machines that should be part of hello (A)SCS/ERS cluster</span></span>
* <span data-ttu-id="413bb-176">Avsökningsport</span><span class="sxs-lookup"><span data-stu-id="413bb-176">Probe Port</span></span>
  * <span data-ttu-id="413bb-177">Port 621**&lt;nr&gt;**</span><span class="sxs-lookup"><span data-stu-id="413bb-177">Port 621**&lt;nr&gt;**</span></span>
* <span data-ttu-id="413bb-178">Regler för belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="413bb-178">Loadbalancing rules</span></span>
  * <span data-ttu-id="413bb-179">33**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="413bb-179">33**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="413bb-180">5**&lt;nr&gt;**13 TCP</span><span class="sxs-lookup"><span data-stu-id="413bb-180">5**&lt;nr&gt;**13 TCP</span></span>
  * <span data-ttu-id="413bb-181">5**&lt;nr&gt;**14 TCP</span><span class="sxs-lookup"><span data-stu-id="413bb-181">5**&lt;nr&gt;**14 TCP</span></span>
  * <span data-ttu-id="413bb-182">5**&lt;nr&gt;**16 TCP</span><span class="sxs-lookup"><span data-stu-id="413bb-182">5**&lt;nr&gt;**16 TCP</span></span>

### <a name="sap-hana"></a><span data-ttu-id="413bb-183">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="413bb-183">SAP HANA</span></span>
* <span data-ttu-id="413bb-184">Konfiguration för klientdel</span><span class="sxs-lookup"><span data-stu-id="413bb-184">Frontend configuration</span></span>
  * <span data-ttu-id="413bb-185">IP-adressen 10.0.0.12</span><span class="sxs-lookup"><span data-stu-id="413bb-185">IP address 10.0.0.12</span></span>
* <span data-ttu-id="413bb-186">Backend-konfiguration</span><span class="sxs-lookup"><span data-stu-id="413bb-186">Backend configuration</span></span>
  * <span data-ttu-id="413bb-187">Ansluten tooprimary nätverksgränssnitt för alla virtuella datorer som ska ingå i klustret för hello HANA</span><span class="sxs-lookup"><span data-stu-id="413bb-187">Connected tooprimary network interfaces of all virtual machines that should be part of hello HANA cluster</span></span>
* <span data-ttu-id="413bb-188">Avsökningsport</span><span class="sxs-lookup"><span data-stu-id="413bb-188">Probe Port</span></span>
  * <span data-ttu-id="413bb-189">Port 625**&lt;nr&gt;**</span><span class="sxs-lookup"><span data-stu-id="413bb-189">Port 625**&lt;nr&gt;**</span></span>
* <span data-ttu-id="413bb-190">Regler för belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="413bb-190">Loadbalancing rules</span></span>
  * <span data-ttu-id="413bb-191">3**&lt;nr&gt;**15 TCP</span><span class="sxs-lookup"><span data-stu-id="413bb-191">3**&lt;nr&gt;**15 TCP</span></span>
  * <span data-ttu-id="413bb-192">3**&lt;nr&gt;**17 TCP</span><span class="sxs-lookup"><span data-stu-id="413bb-192">3**&lt;nr&gt;**17 TCP</span></span>

## <a name="setting-up-a-highly-available-nfs-server"></a><span data-ttu-id="413bb-193">Hur du konfigurerar en NFS-server med hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="413bb-193">Setting up a highly available NFS server</span></span>

### <a name="deploying-linux"></a><span data-ttu-id="413bb-194">Distribuera Linux</span><span class="sxs-lookup"><span data-stu-id="413bb-194">Deploying Linux</span></span>

<span data-ttu-id="413bb-195">hello Azure Marketplace innehåller en bild för SUSE Linux Enterprise Server för SAP program 12 som du kan använda toodeploy nya virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="413bb-195">hello Azure Marketplace contains an image for SUSE Linux Enterprise Server for SAP Applications 12 that you can use toodeploy new virtual machines.</span></span>
<span data-ttu-id="413bb-196">Du kan använda någon av hello Snabbstart mallar på github toodeploy alla nödvändiga resurser.</span><span class="sxs-lookup"><span data-stu-id="413bb-196">You can use one of hello quick start templates on github toodeploy all required resources.</span></span> <span data-ttu-id="413bb-197">hello mallen distribuerar hello virtuella datorer, hello belastningsutjämnare, tillgänglighetsuppsättning osv. Följ dessa steg toodeploy hello mallen:</span><span class="sxs-lookup"><span data-stu-id="413bb-197">hello template deploys hello virtual machines, hello load balancer, availability set etc. Follow these steps toodeploy hello template:</span></span>

1. <span data-ttu-id="413bb-198">Öppna hello [mall för SAP server] [ template-file-server] i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="413bb-198">Open hello [SAP file server template][template-file-server] in hello Azure portal</span></span>   
1. <span data-ttu-id="413bb-199">Ange följande parametrar hello</span><span class="sxs-lookup"><span data-stu-id="413bb-199">Enter hello following parameters</span></span>
   1. <span data-ttu-id="413bb-200">Resurs-Prefix</span><span class="sxs-lookup"><span data-stu-id="413bb-200">Resource Prefix</span></span>  
      <span data-ttu-id="413bb-201">Ange hello-prefix som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="413bb-201">Enter hello prefix you want toouse.</span></span> <span data-ttu-id="413bb-202">hello värdet används som ett prefix för hello resurser som distribueras.</span><span class="sxs-lookup"><span data-stu-id="413bb-202">hello value is used as a prefix for hello resources that are deployed.</span></span>
   2. <span data-ttu-id="413bb-203">OS-typen</span><span class="sxs-lookup"><span data-stu-id="413bb-203">Os Type</span></span>  
      <span data-ttu-id="413bb-204">Välj en av hello Linux-distributioner.</span><span class="sxs-lookup"><span data-stu-id="413bb-204">Select one of hello Linux distributions.</span></span> <span data-ttu-id="413bb-205">Det här exemplet väljer du SLES 12</span><span class="sxs-lookup"><span data-stu-id="413bb-205">For this example, select SLES 12</span></span>
   3. <span data-ttu-id="413bb-206">Användarnamn och lösenord för Admin administratör</span><span class="sxs-lookup"><span data-stu-id="413bb-206">Admin Username and Admin Password</span></span>  
      <span data-ttu-id="413bb-207">En ny användare skapas som kan vara används toolog på toohello datorn.</span><span class="sxs-lookup"><span data-stu-id="413bb-207">A new user is created that can be used toolog on toohello machine.</span></span>
   4. <span data-ttu-id="413bb-208">Undernät-Id</span><span class="sxs-lookup"><span data-stu-id="413bb-208">Subnet Id</span></span>  
      <span data-ttu-id="413bb-209">hello-ID för virtuella datorer i hello undernät toowhich hello ska anslutas till.</span><span class="sxs-lookup"><span data-stu-id="413bb-209">hello ID of hello subnet toowhich hello virtual machines should be connected to.</span></span> <span data-ttu-id="413bb-210">Lämna tomt om du vill toocreate ett nytt virtuellt nätverk eller välj hello undernätet för din VPN eller Expressroute virtuellt nätverk tooconnect hello tooyour lokalt nätverk för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="413bb-210">Leave empty if you want toocreate a new virtual network or select hello subnet of your VPN or Express Route virtual network tooconnect hello virtual machine tooyour on-premises network.</span></span> <span data-ttu-id="413bb-211">hello ID ser oftast ut så /subscriptions/**&lt;prenumerations-id&gt;**/resourceGroups/**&lt;resursgruppnamn&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;virtuella nätverksnamnet&gt;**/subnets/**&lt;undernätsnamn&gt;**</span><span class="sxs-lookup"><span data-stu-id="413bb-211">hello ID usually looks like /subscriptions/**&lt;subscription id&gt;**/resourceGroups/**&lt;resource group name&gt;**/providers/Microsoft.Network/virtualNetworks/**&lt;virtual network name&gt;**/subnets/**&lt;subnet name&gt;**</span></span>

### <a name="installation"></a><span data-ttu-id="413bb-212">Installation</span><span class="sxs-lookup"><span data-stu-id="413bb-212">Installation</span></span>

<span data-ttu-id="413bb-213">hello följande föregås antingen **[A]** -tillämpliga tooall noder **[1]** -endast tillämpliga toonode 1 eller **[2]** -endast tillämpliga toonode 2.</span><span class="sxs-lookup"><span data-stu-id="413bb-213">hello following items are prefixed with either **[A]** - applicable tooall nodes, **[1]** - only applicable toonode 1 or **[2]** - only applicable toonode 2.</span></span>

1. <span data-ttu-id="413bb-214">**[A]**  Uppdatera SLES</span><span class="sxs-lookup"><span data-stu-id="413bb-214">**[A]** Update SLES</span></span>

   <pre><code>
   sudo zypper update
   </code></pre>

1. <span data-ttu-id="413bb-215">**[1]**  Aktivera ssh-åtkomst</span><span class="sxs-lookup"><span data-stu-id="413bb-215">**[1]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa
   
   # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy hello public key
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

2. <span data-ttu-id="413bb-216">**[2]**  Aktivera ssh-åtkomst</span><span class="sxs-lookup"><span data-stu-id="413bb-216">**[2]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa

   # insert hello public key you copied in hello last step into hello authorized keys file on hello second server
   sudo vi /root/.ssh/authorized_keys
   
   # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy hello public key   
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

1. <span data-ttu-id="413bb-217">**[1]**  Aktivera ssh-åtkomst</span><span class="sxs-lookup"><span data-stu-id="413bb-217">**[1]** Enable ssh access</span></span>

   <pre><code>
   # insert hello public key you copied in hello last step into hello authorized keys file on hello first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. <span data-ttu-id="413bb-218">**[A]**  Installera HA filnamnstillägget</span><span class="sxs-lookup"><span data-stu-id="413bb-218">**[A]** Install HA extension</span></span>
   
   <pre><code>
   sudo zypper install sle-ha-release fence-agents
   </code></pre>

1. <span data-ttu-id="413bb-219">**[A]**  Installationsprogrammet värdnamnsmatchning</span><span class="sxs-lookup"><span data-stu-id="413bb-219">**[A]** Setup host name resolution</span></span>   

   <span data-ttu-id="413bb-220">Du kan använda en DNS-server, eller så kan du ändra hello/etc/hosts på alla noder.</span><span class="sxs-lookup"><span data-stu-id="413bb-220">You can either use a DNS server or modify hello /etc/hosts on all nodes.</span></span> <span data-ttu-id="413bb-221">Det här exemplet visar hur toouse hello/etc/hosts-filen.</span><span class="sxs-lookup"><span data-stu-id="413bb-221">This example shows how toouse hello /etc/hosts file.</span></span>
   <span data-ttu-id="413bb-222">Ersätt hello IP-adress och hello värdnamn i hello följande kommandon</span><span class="sxs-lookup"><span data-stu-id="413bb-222">Replace hello IP address and hello hostname in hello following commands</span></span>

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   <span data-ttu-id="413bb-223">Infoga hello följande rader för/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="413bb-223">Insert hello following lines too/etc/hosts.</span></span> <span data-ttu-id="413bb-224">Ändra hello IP-adress och värddatornamn toomatch din miljö</span><span class="sxs-lookup"><span data-stu-id="413bb-224">Change hello IP address and hostname toomatch your environment</span></span>   
   
   <pre><code>
   # IP address of hello load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   </code></pre>

1. <span data-ttu-id="413bb-225">**[1]**  Installera kluster</span><span class="sxs-lookup"><span data-stu-id="413bb-225">**[1]** Install Cluster</span></span>
   
   <pre><code>
   sudo ha-cluster-init
   
   # Do you want toocontinue anyway? [y/N] -> y
   # Network address toobind too(for example: 192.168.1.0) [10.79.227.0] -> ENTER
   # Multicast address (for example: 239.x.x.x) [239.174.218.125] -> ENTER
   # Multicast port [5405] -> ENTER
   # Do you wish toouse SBD? [y/N] -> N
   # Do you wish tooconfigure an administration IP? [y/N] -> N
   </code></pre>

1. <span data-ttu-id="413bb-226">**[2]**  Lägg till nod toocluster</span><span class="sxs-lookup"><span data-stu-id="413bb-226">**[2]** Add node toocluster</span></span>
   
   <pre><code> 
   sudo ha-cluster-join

   # WARNING: NTP is not configured toostart at system boot.
   # WARNING: No watchdog device found. If SBD is used, hello cluster will be unable toostart without a watchdog.
   # Do you want toocontinue anyway? [y/N] -> y
   # IP address or hostname of existing node (for example: 192.168.1.1) [] -> IP address of node 1 for example 10.0.0.10
   # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
   </code></pre>

1. <span data-ttu-id="413bb-227">**[A]**  Ändra hacluster lösenord toohello samma lösenord</span><span class="sxs-lookup"><span data-stu-id="413bb-227">**[A]** Change hacluster password toohello same password</span></span>

   <pre><code> 
   sudo passwd hacluster
   </code></pre>

1. <span data-ttu-id="413bb-228">**[A]**  Och konfigurera corosync toouse andra transport och lägga till nodelist.</span><span class="sxs-lookup"><span data-stu-id="413bb-228">**[A]** Configure corosync toouse other transport and add nodelist.</span></span> <span data-ttu-id="413bb-229">Klustret fungerar inte på annat sätt.</span><span class="sxs-lookup"><span data-stu-id="413bb-229">Cluster will not work otherwise.</span></span>
   
   <pre><code> 
   sudo vi /etc/corosync/corosync.conf   
   </code></pre>

   <span data-ttu-id="413bb-230">Lägg till följande fetstil innehåll toohello hello.</span><span class="sxs-lookup"><span data-stu-id="413bb-230">Add hello following bold content toohello file.</span></span>
   
   <pre><code> 
   [...]
     interface { 
        [...] 
     }
     <b>transport:      udpu</b>
   } 
   <b>nodelist {
     node {
      # IP address of <b>prod-nfs-0</b>
      ring0_addr:10.0.0.5
     }
     node {
      # IP address of <b>prod-nfs-1</b>
      ring0_addr:10.0.0.6
     } 
   }</b>
   logging {
     [...]
   </code></pre>

   <span data-ttu-id="413bb-231">Starta om tjänsten för hello corosync</span><span class="sxs-lookup"><span data-stu-id="413bb-231">Then restart hello corosync service</span></span>

   <pre><code>
   sudo service corosync restart
   </code></pre>

1. <span data-ttu-id="413bb-232">**[A]**  Installera drbd komponenter</span><span class="sxs-lookup"><span data-stu-id="413bb-232">**[A]** Install drbd components</span></span>

   <pre><code>
   sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. <span data-ttu-id="413bb-233">**[A]**  Skapa en partition för hello drbd enhet</span><span class="sxs-lookup"><span data-stu-id="413bb-233">**[A]** Create a partition for hello drbd device</span></span>

   <pre><code>
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdc'
   </code></pre>

1. <span data-ttu-id="413bb-234">**[A]**  Skapa LVM konfigurationer</span><span class="sxs-lookup"><span data-stu-id="413bb-234">**[A]** Create LVM configurations</span></span>

   <pre><code>
   sudo pvcreate /dev/sdc1   
   sudo vgcreate vg_NFS /dev/sdc1
   sudo lvcreate -l 100%FREE -n <b>NWS</b> vg_NFS
   </code></pre>

1. <span data-ttu-id="413bb-235">**[A]**  Skapa hello NFS drbd enhet</span><span class="sxs-lookup"><span data-stu-id="413bb-235">**[A]** Create hello NFS drbd device</span></span>

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_nfs.res
   </code></pre>

   <span data-ttu-id="413bb-236">Infoga hello konfiguration för hello ny drbd enhet och avsluta</span><span class="sxs-lookup"><span data-stu-id="413bb-236">Insert hello configuration for hello new drbd device and exit</span></span>

   <pre><code>
   resource <b>NWS</b>_nfs {
      protocol     C;
      disk {
         on-io-error       pass_on;
      }
      on <b>prod-nfs-0</b> {
         address   <b>10.0.0.5</b>:7790;
         device    /dev/drbd0;
         disk      /dev/vg_NFS/NWS;
         meta-disk internal;
      }
      on <b>prod-nfs-1</b> {
         address   <b>10.0.0.6</b>:7790;
         device    /dev/drbd0;
         disk      /dev/vg_NFS/NWS;
         meta-disk internal;
      }
   }
   </code></pre>

   <span data-ttu-id="413bb-237">Skapa hello drbd enheten och starta den</span><span class="sxs-lookup"><span data-stu-id="413bb-237">Create hello drbd device and start it</span></span>

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_nfs
   sudo drbdadm up <b>NWS</b>_nfs
   </code></pre>

1. <span data-ttu-id="413bb-238">**[1]**  Hoppa över inledande synkronisering</span><span class="sxs-lookup"><span data-stu-id="413bb-238">**[1]** Skip initial synchronization</span></span>

   <pre><code>
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_nfs
   </code></pre>

1. <span data-ttu-id="413bb-239">**[1]**  Hello primära noden</span><span class="sxs-lookup"><span data-stu-id="413bb-239">**[1]** Set hello primary node</span></span>

   <pre><code>
   sudo drbdadm primary --force <b>NWS</b>_nfs
   </code></pre>

1. <span data-ttu-id="413bb-240">**[1]**  Vänta tills hello nya drbd enheter som ska synkroniseras</span><span class="sxs-lookup"><span data-stu-id="413bb-240">**[1]** Wait until hello new drbd devices are synchronized</span></span>

   <pre><code>
   sudo cat /proc/drbd

   # version: 8.4.6 (api:1/proto:86-101)
   # GIT-hash: 833d830e0152d1e457fa7856e71e11248ccf3f70 build by abuild@sheep14, 2016-05-09 23:14:56
   # 0: cs:Connected ro:Primary/Secondary ds:UpToDate/UpToDate C r-----
   #    ns:0 nr:0 dw:0 dr:912 al:8 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   </code></pre>

1. <span data-ttu-id="413bb-241">**[1]**  Skapa filsystem på hello drbd enheter</span><span class="sxs-lookup"><span data-stu-id="413bb-241">**[1]** Create file systems on hello drbd devices</span></span>

   <pre><code>
   sudo mkfs.xfs /dev/drbd0
   </code></pre>


### <a name="configure-cluster-framework"></a><span data-ttu-id="413bb-242">Konfigurera klustret Framework</span><span class="sxs-lookup"><span data-stu-id="413bb-242">Configure Cluster Framework</span></span>

1. <span data-ttu-id="413bb-243">**[1]**  Ändra standardinställningarna för hello</span><span class="sxs-lookup"><span data-stu-id="413bb-243">**[1]** Change hello default settings</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# rsc_defaults resource-stickiness="1"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. <span data-ttu-id="413bb-244">**[1]**  Lägg till hello NFS drbd enhet toohello klusterkonfigurationen</span><span class="sxs-lookup"><span data-stu-id="413bb-244">**[1]** Add hello NFS drbd device toohello cluster configuration</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive drbd_<b>NWS</b>_nfs \
     ocf:linbit:drbd \
     params drbd_resource="<b>NWS</b>_nfs" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"

   crm(live)configure# ms ms-drbd_<b>NWS</b>_nfs drbd_<b>NWS</b>_nfs \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true" interleave="true"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. <span data-ttu-id="413bb-245">**[1]**  Skapa hello NFS-servern</span><span class="sxs-lookup"><span data-stu-id="413bb-245">**[1]** Create hello NFS server</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive nfsserver \
     systemd:nfs-server \
     op monitor interval="30s"

   crm(live)configure# clone cl-nfsserver nfsserver interleave="true"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. <span data-ttu-id="413bb-246">**[1]**  Skapa hello filsystem för NFS-resurser</span><span class="sxs-lookup"><span data-stu-id="413bb-246">**[1]** Create hello NFS File System resources</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive fs_<b>NWS</b>_sapmnt \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd0 \
     directory=/srv/nfs/<b>NWS</b>  \
     fstype=xfs \
     op monitor interval="10s"

   crm(live)configure# group g-<b>NWS</b>_nfs fs_<b>NWS</b>_sapmnt

   crm(live)configure# order o-<b>NWS</b>_drbd_before_nfs inf: \
     ms-drbd_<b>NWS</b>_nfs:promote g-<b>NWS</b>_nfs:start
   
   crm(live)configure# colocation col-<b>NWS</b>_nfs_on_drbd inf: \
     g-<b>NWS</b>_nfs ms-drbd_<b>NWS</b>_nfs:Master

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. <span data-ttu-id="413bb-247">**[1]**  Skapa hello NFS export</span><span class="sxs-lookup"><span data-stu-id="413bb-247">**[1]** Create hello NFS exports</span></span>

   <pre><code>
   sudo mkdir /srv/nfs/<b>NWS</b>/sidsys
   sudo mkdir /srv/nfs/<b>NWS</b>/sapmntsid
   sudo mkdir /srv/nfs/<b>NWS</b>/trans

   sudo crm configure

   crm(live)configure# primitive exportfs_<b>NWS</b> \
     ocf:heartbeat:exportfs \
     params directory="/srv/nfs/<b>NWS</b>" \
     options="rw,no_root_squash" \
     clientspec="*" fsid=0 \
     wait_for_leasetime_on_stop=true \
     op monitor interval="30s"

   crm(live)configure# modgroup g-<b>NWS</b>_nfs add exportfs_<b>NWS</b>

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. <span data-ttu-id="413bb-248">**[1]**  Skapa en virtuell IP-resurs och hälsoavsökningen för hello intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="413bb-248">**[1]** Create a virtual IP resource and health-probe for hello internal load balancer</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive vip_<b>NWS</b>_nfs IPaddr2 \
     params ip=<b>10.0.0.4</b> cidr_netmask=24 \
     op monitor interval=10 timeout=20

   crm(live)configure# primitive nc_<b>NWS</b>_nfs anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k 610<b>00</b>" \
     op monitor timeout=20s interval=10 depth=0

   crm(live)configure# modgroup g-<b>NWS</b>_nfs add nc_<b>NWS</b>_nfs
   crm(live)configure# modgroup g-<b>NWS</b>_nfs add vip_<b>NWS</b>_nfs

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

### <a name="create-stonith-device"></a><span data-ttu-id="413bb-249">Skapa STONITH enhet</span><span class="sxs-lookup"><span data-stu-id="413bb-249">Create STONITH device</span></span>

<span data-ttu-id="413bb-250">Hej STONITH enhet använder ett huvudnamn för tjänsten tooauthorize mot Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="413bb-250">hello STONITH device uses a Service Principal tooauthorize against Microsoft Azure.</span></span> <span data-ttu-id="413bb-251">Följ dessa steg toocreate ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="413bb-251">Follow these steps toocreate a Service Principal.</span></span>

1. <span data-ttu-id="413bb-252">Gå för<https://portal.azure.com></span><span class="sxs-lookup"><span data-stu-id="413bb-252">Go too<https://portal.azure.com></span></span>
1. <span data-ttu-id="413bb-253">Öppna hello Azure Active Directory-bladet</span><span class="sxs-lookup"><span data-stu-id="413bb-253">Open hello Azure Active Directory blade</span></span>  
   <span data-ttu-id="413bb-254">Gå tooProperties och anteckna hello Directory-Id. Detta är hello **klient-id**.</span><span class="sxs-lookup"><span data-stu-id="413bb-254">Go tooProperties and write down hello Directory Id. This is hello **tenant id**.</span></span>
1. <span data-ttu-id="413bb-255">Klicka på appen registreringar</span><span class="sxs-lookup"><span data-stu-id="413bb-255">Click App registrations</span></span>
1. <span data-ttu-id="413bb-256">Klicka på Lägg till</span><span class="sxs-lookup"><span data-stu-id="413bb-256">Click Add</span></span>
1. <span data-ttu-id="413bb-257">Ange ett namn, Välj typ av program ”Web app/API”, ange en inloggnings-URL (till exempel http://localhost) och klicka på Skapa</span><span class="sxs-lookup"><span data-stu-id="413bb-257">Enter a Name, select Application Type "Web app/API", enter a sign-on URL (for example http://localhost) and click Create</span></span>
1. <span data-ttu-id="413bb-258">hello inloggnings-URL används inte och kan vara en giltig URL</span><span class="sxs-lookup"><span data-stu-id="413bb-258">hello sign-on URL is not used and can be any valid URL</span></span>
1. <span data-ttu-id="413bb-259">Välj hello nya App och klicka på nycklarna i hello på fliken Inställningar</span><span class="sxs-lookup"><span data-stu-id="413bb-259">Select hello new App and click Keys in hello Settings tab</span></span>
1. <span data-ttu-id="413bb-260">Ange en beskrivning för en ny nyckel, Välj ”upphör aldrig att gälla” och klicka på Spara</span><span class="sxs-lookup"><span data-stu-id="413bb-260">Enter a description for a new key, select "Never expires" and click Save</span></span>
1. <span data-ttu-id="413bb-261">Skriv ned hello värde.</span><span class="sxs-lookup"><span data-stu-id="413bb-261">Write down hello Value.</span></span> <span data-ttu-id="413bb-262">Den används som hello **lösenord** för hello tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="413bb-262">It is used as hello **password** for hello Service Principal</span></span>
1. <span data-ttu-id="413bb-263">Skriv ned hello program-Id. Den används som hello användarnamn (**inloggnings-id** i hello stegen nedan) för hello tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="413bb-263">Write down hello Application Id. It is used as hello username (**login id** in hello steps below) of hello Service Principal</span></span>

<span data-ttu-id="413bb-264">hello tjänstens huvudnamn har inte behörigheter tooaccess Azure-resurser som standard.</span><span class="sxs-lookup"><span data-stu-id="413bb-264">hello Service Principal does not have permissions tooaccess your Azure resources by default.</span></span> <span data-ttu-id="413bb-265">Du behöver toogive hello tjänstens huvudnamn behörigheter toostart och stoppa (frigöra) alla virtuella datorer i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="413bb-265">You need toogive hello Service Principal permissions toostart and stop (deallocate) all virtual machines of hello cluster.</span></span>

1. <span data-ttu-id="413bb-266">Gå toohttps://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="413bb-266">Go toohttps://portal.azure.com</span></span>
1. <span data-ttu-id="413bb-267">Öppna hello bladet för alla resurser</span><span class="sxs-lookup"><span data-stu-id="413bb-267">Open hello All resources blade</span></span>
1. <span data-ttu-id="413bb-268">Välj hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="413bb-268">Select hello virtual machine</span></span>
1. <span data-ttu-id="413bb-269">Klicka på åtkomstkontroll (IAM)</span><span class="sxs-lookup"><span data-stu-id="413bb-269">Click Access control (IAM)</span></span>
1. <span data-ttu-id="413bb-270">Klicka på Lägg till</span><span class="sxs-lookup"><span data-stu-id="413bb-270">Click Add</span></span>
1. <span data-ttu-id="413bb-271">Välj hello roll ägare</span><span class="sxs-lookup"><span data-stu-id="413bb-271">Select hello role Owner</span></span>
1. <span data-ttu-id="413bb-272">Ange hello namnet på hello-program som du skapade ovan</span><span class="sxs-lookup"><span data-stu-id="413bb-272">Enter hello name of hello application you created above</span></span>
1. <span data-ttu-id="413bb-273">Klicka på OK</span><span class="sxs-lookup"><span data-stu-id="413bb-273">Click OK</span></span>

#### <a name="1-create-hello-stonith-devices"></a><span data-ttu-id="413bb-274">**[1]**  Skapa hello STONITH enheter</span><span class="sxs-lookup"><span data-stu-id="413bb-274">**[1]** Create hello STONITH devices</span></span>

<span data-ttu-id="413bb-275">När du har redigerat hello behörigheter för hello virtuella datorer kan du konfigurera hello STONITH enheter i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="413bb-275">After you edited hello permissions for hello virtual machines, you can configure hello STONITH devices in hello cluster.</span></span>

<pre><code>
sudo crm configure

# replace hello bold string with your subscription id, resource group, tenant id, service principal id and password

crm(live)configure# primitive rsc_st_azure_1 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# primitive rsc_st_azure_2 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started

crm(live)configure# commit
crm(live)configure# exit
</code></pre>

#### <a name="1-enable-hello-use-of-a-stonith-device"></a><span data-ttu-id="413bb-276">**[1]**  Aktivera hello användning av en STONITH-enhet</span><span class="sxs-lookup"><span data-stu-id="413bb-276">**[1]** Enable hello use of a STONITH device</span></span>

<pre><code>
sudo crm configure property stonith-enabled=true 
</code></pre>

## <a name="setting-up-ascs"></a><span data-ttu-id="413bb-277">Skapa (A) SCS</span><span class="sxs-lookup"><span data-stu-id="413bb-277">Setting up (A)SCS</span></span>

### <a name="deploying-linux"></a><span data-ttu-id="413bb-278">Distribuera Linux</span><span class="sxs-lookup"><span data-stu-id="413bb-278">Deploying Linux</span></span>

<span data-ttu-id="413bb-279">hello Azure Marketplace innehåller en bild för SUSE Linux Enterprise Server för SAP program 12 som du kan använda toodeploy nya virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="413bb-279">hello Azure Marketplace contains an image for SUSE Linux Enterprise Server for SAP Applications 12 that you can use toodeploy new virtual machines.</span></span> <span data-ttu-id="413bb-280">hello marketplace-avbildning innehåller hello resurs agent för SAP NetWeaver.</span><span class="sxs-lookup"><span data-stu-id="413bb-280">hello marketplace image contains hello resource agent for SAP NetWeaver.</span></span>

<span data-ttu-id="413bb-281">Du kan använda någon av hello Snabbstart mallar på github toodeploy alla nödvändiga resurser.</span><span class="sxs-lookup"><span data-stu-id="413bb-281">You can use one of hello quick start templates on github toodeploy all required resources.</span></span> <span data-ttu-id="413bb-282">hello mallen distribuerar hello virtuella datorer, hello belastningsutjämnare, tillgänglighetsuppsättning osv. Följ dessa steg toodeploy hello mallen:</span><span class="sxs-lookup"><span data-stu-id="413bb-282">hello template deploys hello virtual machines, hello load balancer, availability set etc. Follow these steps toodeploy hello template:</span></span>

1. <span data-ttu-id="413bb-283">Öppna hello [ASCS/SCS Multi SID-mall] [ template-multisid-xscs] eller hello [konvergerat mallen] [ template-converged] på hello Azure portal hello ASCS/SCS mall skapar hello belastningsutjämning regler för hello SAP NetWeaver ASCS/SCS och ÄNDARE (endast Linux) instanser medan hello konvergerade mallen skapar också hello belastningsutjämning regler för en databas (till exempel Microsoft SQL Server eller SAP HANA).</span><span class="sxs-lookup"><span data-stu-id="413bb-283">Open hello [ASCS/SCS Multi SID template][template-multisid-xscs] or hello [converged template][template-converged] on hello Azure portal hello ASCS/SCS template only creates hello load-balancing rules for hello SAP NetWeaver ASCS/SCS and ERS (Linux only) instances whereas hello converged template also creates hello load-balancing rules for a database (for example Microsoft SQL Server or SAP HANA).</span></span> <span data-ttu-id="413bb-284">Om du planerar tooinstall ett SAP NetWeaver baserat system och du bör också tooinstall hello databasen på Hej samma datorer, Använd hello [konvergerat mallen][template-converged].</span><span class="sxs-lookup"><span data-stu-id="413bb-284">If you plan tooinstall an SAP NetWeaver based system and you also want tooinstall hello database on hello same machines, use hello [converged template][template-converged].</span></span>
1. <span data-ttu-id="413bb-285">Ange följande parametrar hello</span><span class="sxs-lookup"><span data-stu-id="413bb-285">Enter hello following parameters</span></span>
   1. <span data-ttu-id="413bb-286">Resursen Prefix (endast mall ASCS/SCS Multi SID)</span><span class="sxs-lookup"><span data-stu-id="413bb-286">Resource Prefix (ASCS/SCS Multi SID template only)</span></span>  
      <span data-ttu-id="413bb-287">Ange hello-prefix som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="413bb-287">Enter hello prefix you want toouse.</span></span> <span data-ttu-id="413bb-288">hello värdet används som ett prefix för hello resurser som distribueras.</span><span class="sxs-lookup"><span data-stu-id="413bb-288">hello value is used as a prefix for hello resources that are deployed.</span></span>
   3. <span data-ttu-id="413bb-289">SAP System-Id (endast konvergerade mall)</span><span class="sxs-lookup"><span data-stu-id="413bb-289">Sap System Id (converged template only)</span></span>  
      <span data-ttu-id="413bb-290">Ange hello SAP system Id hello SAP-system som du vill tooinstall.</span><span class="sxs-lookup"><span data-stu-id="413bb-290">Enter hello SAP system Id of hello SAP system you want tooinstall.</span></span> <span data-ttu-id="413bb-291">hello Id används som ett prefix för hello resurser som distribueras.</span><span class="sxs-lookup"><span data-stu-id="413bb-291">hello Id is used as a prefix for hello resources that are deployed.</span></span>
   4. <span data-ttu-id="413bb-292">Stack-typ</span><span class="sxs-lookup"><span data-stu-id="413bb-292">Stack Type</span></span>  
      <span data-ttu-id="413bb-293">Ange hello SAP NetWeaver stack</span><span class="sxs-lookup"><span data-stu-id="413bb-293">Select hello SAP NetWeaver stack type</span></span>
   5. <span data-ttu-id="413bb-294">OS-typen</span><span class="sxs-lookup"><span data-stu-id="413bb-294">Os Type</span></span>  
      <span data-ttu-id="413bb-295">Välj en av hello Linux-distributioner.</span><span class="sxs-lookup"><span data-stu-id="413bb-295">Select one of hello Linux distributions.</span></span> <span data-ttu-id="413bb-296">Det här exemplet väljer du SLES 12 BYOS</span><span class="sxs-lookup"><span data-stu-id="413bb-296">For this example, select SLES 12 BYOS</span></span>
   6. <span data-ttu-id="413bb-297">DB-typ</span><span class="sxs-lookup"><span data-stu-id="413bb-297">Db Type</span></span>  
      <span data-ttu-id="413bb-298">Välj HANA</span><span class="sxs-lookup"><span data-stu-id="413bb-298">Select HANA</span></span>
   7. <span data-ttu-id="413bb-299">Storlek för SAP-System</span><span class="sxs-lookup"><span data-stu-id="413bb-299">Sap System Size</span></span>  
      <span data-ttu-id="413bb-300">hello mängden SAP hello nya system ger.</span><span class="sxs-lookup"><span data-stu-id="413bb-300">hello amount of SAPS hello new system provides.</span></span> <span data-ttu-id="413bb-301">Om du inte är säker på hur många SAP hello systemet behöver be din SAP-teknikpartner eller systemintegreraren</span><span class="sxs-lookup"><span data-stu-id="413bb-301">If you are not sure how many SAPS hello system requires, please ask your SAP Technology Partner or System Integrator</span></span>
   8. <span data-ttu-id="413bb-302">Systemets tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="413bb-302">System Availability</span></span>  
      <span data-ttu-id="413bb-303">Välj hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="413bb-303">Select HA</span></span>
   9. <span data-ttu-id="413bb-304">Användarnamn och lösenord för Admin administratör</span><span class="sxs-lookup"><span data-stu-id="413bb-304">Admin Username and Admin Password</span></span>  
      <span data-ttu-id="413bb-305">En ny användare skapas som kan vara används toolog på toohello datorn.</span><span class="sxs-lookup"><span data-stu-id="413bb-305">A new user is created that can be used toolog on toohello machine.</span></span>
   10. <span data-ttu-id="413bb-306">Undernät-Id</span><span class="sxs-lookup"><span data-stu-id="413bb-306">Subnet Id</span></span>  
   <span data-ttu-id="413bb-307">hello-ID för virtuella datorer i hello undernät toowhich hello ska anslutas till.</span><span class="sxs-lookup"><span data-stu-id="413bb-307">hello ID of hello subnet toowhich hello virtual machines should be connected to.</span></span>  <span data-ttu-id="413bb-308">Lämna tomt om du vill toocreate ett nytt virtuellt nätverk eller välj hello samma undernät som du användas eller skapas som en del av hello distribution för NFS-server.</span><span class="sxs-lookup"><span data-stu-id="413bb-308">Leave empty if you want toocreate a new virtual network or select hello same subnet that you used or created as part of hello NFS server deployment.</span></span> <span data-ttu-id="413bb-309">hello ID ser oftast ut så /subscriptions/**&lt;prenumerations-id&gt;**/resourceGroups/**&lt;resursgruppnamn&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;virtuella nätverksnamnet&gt;**/subnets/**&lt;undernätsnamn&gt;**</span><span class="sxs-lookup"><span data-stu-id="413bb-309">hello ID usually looks like /subscriptions/**&lt;subscription id&gt;**/resourceGroups/**&lt;resource group name&gt;**/providers/Microsoft.Network/virtualNetworks/**&lt;virtual network name&gt;**/subnets/**&lt;subnet name&gt;**</span></span>

### <a name="installation"></a><span data-ttu-id="413bb-310">Installation</span><span class="sxs-lookup"><span data-stu-id="413bb-310">Installation</span></span>

<span data-ttu-id="413bb-311">hello följande föregås antingen **[A]** -tillämpliga tooall noder **[1]** -endast tillämpliga toonode 1 eller **[2]** -endast tillämpliga toonode 2.</span><span class="sxs-lookup"><span data-stu-id="413bb-311">hello following items are prefixed with either **[A]** - applicable tooall nodes, **[1]** - only applicable toonode 1 or **[2]** - only applicable toonode 2.</span></span>

1. <span data-ttu-id="413bb-312">**[A]**  Uppdatera SLES</span><span class="sxs-lookup"><span data-stu-id="413bb-312">**[A]** Update SLES</span></span>

   <pre><code>
   sudo zypper update
   </code></pre>

1. <span data-ttu-id="413bb-313">**[1]**  Aktivera ssh-åtkomst</span><span class="sxs-lookup"><span data-stu-id="413bb-313">**[1]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa
   
   # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy hello public key
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

2. <span data-ttu-id="413bb-314">**[2]**  Aktivera ssh-åtkomst</span><span class="sxs-lookup"><span data-stu-id="413bb-314">**[2]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa

   # insert hello public key you copied in hello last step into hello authorized keys file on hello second server
   sudo vi /root/.ssh/authorized_keys
   
   # Enter file in which toosave hello key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy hello public key   
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

1. <span data-ttu-id="413bb-315">**[1]**  Aktivera ssh-åtkomst</span><span class="sxs-lookup"><span data-stu-id="413bb-315">**[1]** Enable ssh access</span></span>

   <pre><code>
   # insert hello public key you copied in hello last step into hello authorized keys file on hello first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. <span data-ttu-id="413bb-316">**[A]**  Installera HA filnamnstillägget</span><span class="sxs-lookup"><span data-stu-id="413bb-316">**[A]** Install HA extension</span></span>
   
   <pre><code>
   sudo zypper install sle-ha-release fence-agents
   </code></pre>

1. <span data-ttu-id="413bb-317">**[A]**  Uppdatering SAP resurs agenter</span><span class="sxs-lookup"><span data-stu-id="413bb-317">**[A]** Update SAP resource agents</span></span>  
   
   <span data-ttu-id="413bb-318">En korrigering för hello resurs agenter paketet är obligatoriska toouse hello ny konfiguration, som beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="413bb-318">A patch for hello resource-agents package is required toouse hello new configuration, that is described in this article.</span></span> <span data-ttu-id="413bb-319">Du kan kontrollera, om hello korrigering har redan installerats med hello följande kommando</span><span class="sxs-lookup"><span data-stu-id="413bb-319">You can check, if hello patch is already installed with hello following command</span></span>

   <pre><code>
   sudo grep 'parameter name="IS_ERS"' /usr/lib/ocf/resource.d/heartbeat/SAPInstance
   </code></pre>

   <span data-ttu-id="413bb-320">hello utdata ska vara liknar</span><span class="sxs-lookup"><span data-stu-id="413bb-320">hello output should be similar to</span></span>

   <pre><code>
   &lt;parameter name="IS_ERS" unique="0" required="0"&gt;
   </code></pre>

   <span data-ttu-id="413bb-321">Om hello grep kommando inte hittar hello IS_ERS parameter måste tooinstall hello korrigering som visas på [hello SUSE hämtningssidan](https://download.suse.com/patch/finder/#bu=suse&familyId=&productId=&dateRange=&startDate=&endDate=&priority=&architecture=&keywords=resource-agents)</span><span class="sxs-lookup"><span data-stu-id="413bb-321">If hello grep command does not find hello IS_ERS parameter, you need tooinstall hello patch listed on [hello SUSE download page](https://download.suse.com/patch/finder/#bu=suse&familyId=&productId=&dateRange=&startDate=&endDate=&priority=&architecture=&keywords=resource-agents)</span></span>

   <pre><code>
   # example for patch for SLES 12 SP1
   sudo zypper in -t patch SUSE-SLE-HA-12-SP1-2017-885=1
   # example for patch for SLES 12 SP2
   sudo zypper in -t patch SUSE-SLE-HA-12-SP2-2017-886=1
   </code></pre>

1. <span data-ttu-id="413bb-322">**[A]**  Installationsprogrammet värdnamnsmatchning</span><span class="sxs-lookup"><span data-stu-id="413bb-322">**[A]** Setup host name resolution</span></span>   

   <span data-ttu-id="413bb-323">Du kan använda en DNS-server, eller så kan du ändra hello/etc/hosts på alla noder.</span><span class="sxs-lookup"><span data-stu-id="413bb-323">You can either use a DNS server or modify hello /etc/hosts on all nodes.</span></span> <span data-ttu-id="413bb-324">Det här exemplet visar hur toouse hello/etc/hosts-filen.</span><span class="sxs-lookup"><span data-stu-id="413bb-324">This example shows how toouse hello /etc/hosts file.</span></span>
   <span data-ttu-id="413bb-325">Ersätt hello IP-adress och hello värdnamn i hello följande kommandon</span><span class="sxs-lookup"><span data-stu-id="413bb-325">Replace hello IP address and hello hostname in hello following commands</span></span>

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   <span data-ttu-id="413bb-326">Infoga hello följande rader för/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="413bb-326">Insert hello following lines too/etc/hosts.</span></span> <span data-ttu-id="413bb-327">Ändra hello IP-adress och värddatornamn toomatch din miljö</span><span class="sxs-lookup"><span data-stu-id="413bb-327">Change hello IP address and hostname toomatch your environment</span></span>   
   
   <pre><code>
   # IP address of hello load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   # IP address of hello load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.0.0.10 nws-ascs</b>
   # IP address of hello load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.0.0.11 nws-ers</b>
   # IP address of hello load balancer frontend configuration for database
   <b>10.0.0.12 nws-db</b>
   </code></pre>

1. <span data-ttu-id="413bb-328">**[1]**  Installera kluster</span><span class="sxs-lookup"><span data-stu-id="413bb-328">**[1]** Install Cluster</span></span>
   
   <pre><code>
   sudo ha-cluster-init
   
   # Do you want toocontinue anyway? [y/N] -> y
   # Network address toobind too(for example: 192.168.1.0) [10.79.227.0] -> ENTER
   # Multicast address (for example: 239.x.x.x) [239.174.218.125] -> ENTER
   # Multicast port [5405] -> ENTER
   # Do you wish toouse SBD? [y/N] -> N
   # Do you wish tooconfigure an administration IP? [y/N] -> N
   </code></pre>

1. <span data-ttu-id="413bb-329">**[2]**  Lägg till nod toocluster</span><span class="sxs-lookup"><span data-stu-id="413bb-329">**[2]** Add node toocluster</span></span>
   
   <pre><code> 
   sudo ha-cluster-join

   # WARNING: NTP is not configured toostart at system boot.
   # WARNING: No watchdog device found. If SBD is used, hello cluster will be unable toostart without a watchdog.
   # Do you want toocontinue anyway? [y/N] -> y
   # IP address or hostname of existing node (for example: 192.168.1.1) [] -> IP address of node 1 for example 10.0.0.10
   # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
   </code></pre>

1. <span data-ttu-id="413bb-330">**[A]**  Ändra hacluster lösenord toohello samma lösenord</span><span class="sxs-lookup"><span data-stu-id="413bb-330">**[A]** Change hacluster password toohello same password</span></span>

   <pre><code> 
   sudo passwd hacluster
   </code></pre>

1. <span data-ttu-id="413bb-331">**[A]**  Och konfigurera corosync toouse andra transport och lägga till nodelist.</span><span class="sxs-lookup"><span data-stu-id="413bb-331">**[A]** Configure corosync toouse other transport and add nodelist.</span></span> <span data-ttu-id="413bb-332">Klustret fungerar inte på annat sätt.</span><span class="sxs-lookup"><span data-stu-id="413bb-332">Cluster will not work otherwise.</span></span>
   
   <pre><code> 
   sudo vi /etc/corosync/corosync.conf   
   </code></pre>

   <span data-ttu-id="413bb-333">Lägg till följande fetstil innehåll toohello hello.</span><span class="sxs-lookup"><span data-stu-id="413bb-333">Add hello following bold content toohello file.</span></span>
   
   <pre><code> 
   [...]
     interface { 
        [...] 
     }
     <b>transport:      udpu</b>
   } 
   <b>nodelist {
     node {
      # IP address of <b>nws-cl-0</b>
      ring0_addr:     10.0.0.14
     }
     node {
      # IP address of <b>nws-cl-1</b>
      ring0_addr:     10.0.0.13
     } 
   }</b>
   logging {
     [...]
   </code></pre>

   <span data-ttu-id="413bb-334">Starta om tjänsten för hello corosync</span><span class="sxs-lookup"><span data-stu-id="413bb-334">Then restart hello corosync service</span></span>

   <pre><code>
   sudo service corosync restart
   </code></pre>

1. <span data-ttu-id="413bb-335">**[A]**  Installera drbd komponenter</span><span class="sxs-lookup"><span data-stu-id="413bb-335">**[A]** Install drbd components</span></span>

   <pre><code>
   sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. <span data-ttu-id="413bb-336">**[A]**  Skapa en partition för hello drbd enhet</span><span class="sxs-lookup"><span data-stu-id="413bb-336">**[A]** Create a partition for hello drbd device</span></span>

   <pre><code>
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdc'
   </code></pre>

1. <span data-ttu-id="413bb-337">**[A]**  Skapa LVM konfigurationer</span><span class="sxs-lookup"><span data-stu-id="413bb-337">**[A]** Create LVM configurations</span></span>

   <pre><code>
   sudo pvcreate /dev/sdc1   
   sudo vgcreate vg_<b>NWS</b> /dev/sdc1
   sudo lvcreate -l 50%FREE -n <b>NWS</b>_ASCS vg_<b>NWS</b>
   sudo lvcreate -l 50%FREE -n <b>NWS</b>_ERS vg_<b>NWS</b>
   </code></pre>

1. <span data-ttu-id="413bb-338">**[A]**  Skapa hello SCS drbd enhet</span><span class="sxs-lookup"><span data-stu-id="413bb-338">**[A]** Create hello SCS drbd device</span></span>

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_ascs.res
   </code></pre>

   <span data-ttu-id="413bb-339">Infoga hello konfiguration för hello ny drbd enhet och avsluta</span><span class="sxs-lookup"><span data-stu-id="413bb-339">Insert hello configuration for hello new drbd device and exit</span></span>

   <pre><code>
   resource <b>NWS</b>_ascs {
      protocol     C;
      disk {
         on-io-error       pass_on;
      }
      on <b>nws-cl-0</b> {
         address   <b>10.0.0.14</b>:7791;
         device    /dev/drbd0;
         disk      /dev/vg_NWS/NWS_ASCS;
         meta-disk internal;
      }
      on <b>nws-cl-1</b> {
         address   <b>10.0.0.13</b>:7791;
         device    /dev/drbd0;
         disk      /dev/vg_NWS/NWS_ASCS;
         meta-disk internal;
      }
   }
   </code></pre>

   <span data-ttu-id="413bb-340">Skapa hello drbd enheten och starta den</span><span class="sxs-lookup"><span data-stu-id="413bb-340">Create hello drbd device and start it</span></span>

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_ascs
   sudo drbdadm up <b>NWS</b>_ascs
   </code></pre>

1. <span data-ttu-id="413bb-341">**[A]**  Skapa hello ÄNDARE drbd enhet</span><span class="sxs-lookup"><span data-stu-id="413bb-341">**[A]** Create hello ERS drbd device</span></span>

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_ers.res
   </code></pre>

   <span data-ttu-id="413bb-342">Infoga hello konfiguration för hello ny drbd enhet och avsluta</span><span class="sxs-lookup"><span data-stu-id="413bb-342">Insert hello configuration for hello new drbd device and exit</span></span>

   <pre><code>
   resource <b>NWS</b>_ers {
      protocol     C;
      disk {
         on-io-error       pass_on;
      }
      on <b>nws-cl-0</b> {
         address   <b>10.0.0.14</b>:7792;
         device    /dev/drbd1;
         disk      /dev/vg_NWS/NWS_ERS;
         meta-disk internal;
      }
      on <b>nws-cl-1</b> {
         address   <b>10.0.0.13</b>:7792;
         device    /dev/drbd1;
         disk      /dev/vg_NWS/NWS_ERS;
         meta-disk internal;
      }
   }
   </code></pre>

   <span data-ttu-id="413bb-343">Skapa hello drbd enheten och starta den</span><span class="sxs-lookup"><span data-stu-id="413bb-343">Create hello drbd device and start it</span></span>

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_ers
   sudo drbdadm up <b>NWS</b>_ers
   </code></pre>

1. <span data-ttu-id="413bb-344">**[1]**  Hoppa över inledande synkronisering</span><span class="sxs-lookup"><span data-stu-id="413bb-344">**[1]** Skip initial synchronization</span></span>

   <pre><code>
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_ascs
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_ers
   </code></pre>

1. <span data-ttu-id="413bb-345">**[1]**  Hello primära noden</span><span class="sxs-lookup"><span data-stu-id="413bb-345">**[1]** Set hello primary node</span></span>

   <pre><code>
   sudo drbdadm primary --force <b>NWS</b>_ascs
   sudo drbdadm primary --force <b>NWS</b>_ers
   </code></pre>

1. <span data-ttu-id="413bb-346">**[1]**  Vänta tills hello nya drbd enheter som ska synkroniseras</span><span class="sxs-lookup"><span data-stu-id="413bb-346">**[1]** Wait until hello new drbd devices are synchronized</span></span>

   <pre><code>
   sudo cat /proc/drbd

   # version: 8.4.6 (api:1/proto:86-101)
   # GIT-hash: 833d830e0152d1e457fa7856e71e11248ccf3f70 build by abuild@sheep14, 2016-05-09 23:14:56
   # 0: cs:<b>Connected</b> ro:Primary/Secondary ds:<b>UpToDate/UpToDate</b> C r-----
   #     ns:93991268 nr:0 dw:93991268 dr:93944920 al:383 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   # 1: cs:<b>Connected</b> ro:Primary/Secondary ds:<b>UpToDate/UpToDate</b> C r-----
   #     ns:6047920 nr:0 dw:6047920 dr:6039112 al:34 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   # 2: cs:<b>Connected</b> ro:Primary/Secondary ds:<b>UpToDate/UpToDate</b> C r-----
   #     ns:5142732 nr:0 dw:5142732 dr:5133924 al:30 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   </code></pre>

1. <span data-ttu-id="413bb-347">**[1]**  Skapa filsystem på hello drbd enheter</span><span class="sxs-lookup"><span data-stu-id="413bb-347">**[1]** Create file systems on hello drbd devices</span></span>

   <pre><code>
   sudo mkfs.xfs /dev/drbd0
   sudo mkfs.xfs /dev/drbd1
   </code></pre>


### <a name="configure-cluster-framework"></a><span data-ttu-id="413bb-348">Konfigurera klustret Framework</span><span class="sxs-lookup"><span data-stu-id="413bb-348">Configure Cluster Framework</span></span>

<span data-ttu-id="413bb-349">**[1]**  Ändra standardinställningarna för hello</span><span class="sxs-lookup"><span data-stu-id="413bb-349">**[1]** Change hello default settings</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# rsc_defaults resource-stickiness="1"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

## <a name="prepare-for-sap-netweaver-installation"></a><span data-ttu-id="413bb-350">Förbereda för SAP NetWeaver installation</span><span class="sxs-lookup"><span data-stu-id="413bb-350">Prepare for SAP NetWeaver installation</span></span>

1. <span data-ttu-id="413bb-351">**[A]**  Skapa hello delade kataloger</span><span class="sxs-lookup"><span data-stu-id="413bb-351">**[A]** Create hello shared directories</span></span>

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NWS</b>
   sudo mkdir -p /usr/sap/trans
   sudo mkdir -p /usr/sap/<b>NWS</b>/SYS

   sudo chattr +i /sapmnt/<b>NWS</b>
   sudo chattr +i /usr/sap/trans
   sudo chattr +i /usr/sap/<b>NWS</b>/SYS
   </code></pre>

1. <span data-ttu-id="413bb-352">**[A]**  Konfigurera autofs</span><span class="sxs-lookup"><span data-stu-id="413bb-352">**[A]** Configure autofs</span></span>
 
   <pre><code>
   sudo vi /etc/auto.master

   # Add hello following line toohello file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   <span data-ttu-id="413bb-353">Skapa en fil med</span><span class="sxs-lookup"><span data-stu-id="413bb-353">Create a file with</span></span>

   <pre><code>
   sudo vi /etc/auto.direct

   # Add hello following lines toohello file, save and exit
   /sapmnt/<b>NWS</b> -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/trans
   /usr/sap/<b>NWS</b>/SYS -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sidsys
   </code></pre>

   <span data-ttu-id="413bb-354">Starta om autofs toomount hello nya resurser</span><span class="sxs-lookup"><span data-stu-id="413bb-354">Restart autofs toomount hello new shares</span></span>

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. <span data-ttu-id="413bb-355">**[A]**  Konfigurera växla fil</span><span class="sxs-lookup"><span data-stu-id="413bb-355">**[A]** Configure SWAP file</span></span>
 
   <pre><code>
   sudo vi /etc/waagent.conf

   # Set hello property ResourceDisk.EnableSwap tooy
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>

   # Set hello size of hello SWAP file with property ResourceDisk.SwapSizeMB
   # hello free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check hello SWAP space with command swapon
   # Size of hello swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   <span data-ttu-id="413bb-356">Starta om hello Agent tooactivate hello ändring</span><span class="sxs-lookup"><span data-stu-id="413bb-356">Restart hello Agent tooactivate hello change</span></span>

   <pre><code>
   sudo service waagent restart
   </code></pre>

### <a name="installing-sap-netweaver-ascsers"></a><span data-ttu-id="413bb-357">Installera SAP NetWeaver ASCS/ÄNDARE</span><span class="sxs-lookup"><span data-stu-id="413bb-357">Installing SAP NetWeaver ASCS/ERS</span></span>

1. <span data-ttu-id="413bb-358">**[1]**  Skapa en virtuell IP-resurs och hälsoavsökningen för hello intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="413bb-358">**[1]** Create a virtual IP resource and health-probe for hello internal load balancer</span></span>

   <pre><code>
   sudo crm node standby <b>nws-cl-1</b>
   sudo crm configure

   crm(live)configure# primitive drbd_<b>NWS</b>_ASCS \
     ocf:linbit:drbd \
     params drbd_resource="<b>NWS</b>_ascs" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"

   crm(live)configure# ms ms-drbd_<b>NWS</b>_ASCS drbd_<b>NWS</b>_ASCS \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true"

   crm(live)configure# primitive fs_<b>NWS</b>_ASCS \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd0 \
     directory=/usr/sap/<b>NWS</b>/ASCS<b>00</b>  \
     fstype=xfs \
     op monitor interval="10s"

   crm(live)configure# primitive vip_<b>NWS</b>_ASCS IPaddr2 \
     params ip=<b>10.0.0.10</b> cidr_netmask=24 \
     op monitor interval=10 timeout=20

   crm(live)configure# primitive nc_<b>NWS</b>_ASCS anything \
     params binfile="/usr/bin/nc" cmdline_options="-l -k 620<b>00</b>" \
     op monitor timeout=20s interval=10 depth=0
   
   crm(live)configure# group g-<b>NWS</b>_ASCS nc_<b>NWS</b>_ASCS vip_<b>NWS</b>_ASCS fs_<b>NWS</b>_ASCS \
      meta resource-stickiness=3000

   crm(live)configure# order o-<b>NWS</b>_drbd_before_ASCS inf: \
     ms-drbd_<b>NWS</b>_ASCS:promote g-<b>NWS</b>_ASCS:start
   
   crm(live)configure# colocation col-<b>NWS</b>_ASCS_on_drbd inf: \
     ms-drbd_<b>NWS</b>_ASCS:Master g-<b>NWS</b>_ASCS
   
   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

   <span data-ttu-id="413bb-359">Se till att hello klusterstatus är det ok och att alla resurser har startats.</span><span class="sxs-lookup"><span data-stu-id="413bb-359">Make sure that hello cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="413bb-360">Det är inte viktigt på vilka nod hello resurser körs.</span><span class="sxs-lookup"><span data-stu-id="413bb-360">It is not important on which node hello resources are running.</span></span>

   <pre><code>
   sudo crm_mon -r

   # Node nws-cl-1: standby
   # <b>Online: [ nws-cl-0 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      Stopped: [ nws-cl-1 ]
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-0</b>
   </code></pre>

1. <span data-ttu-id="413bb-361">**[1]**  Installera SAP NetWeaver ASCS</span><span class="sxs-lookup"><span data-stu-id="413bb-361">**[1]** Install SAP NetWeaver ASCS</span></span>  

   <span data-ttu-id="413bb-362">Installera SAP NetWeaver ASCS som rot på hello första nod med ett virtuellt värdnamn som till exempel mappar toohello IP-adressen för hello klientdel konfiguration av belastningsutjämning för hello ASCS <b>nws ascs</b>, <b>10.0.0.10</b>och hello-instansnummer som du använde för hello avsökning av hello belastningsutjämnare till exempel <b>00</b>.</span><span class="sxs-lookup"><span data-stu-id="413bb-362">Install SAP NetWeaver ASCS as root on hello first node using a virtual hostname that maps toohello IP address of hello load balancer frontend configuration for hello ASCS for example <b>nws-ascs</b>, <b>10.0.0.10</b> and hello instance number that you used for hello probe of hello load balancer for example <b>00</b>.</span></span>

   <span data-ttu-id="413bb-363">Du kan använda hello sapinst parametern SAPINST_REMOTE_ACCESS_USER tooallow en icke-rotanvändaren tooconnect toosapinst.</span><span class="sxs-lookup"><span data-stu-id="413bb-363">You can use hello sapinst parameter SAPINST_REMOTE_ACCESS_USER tooallow a non-root user tooconnect toosapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. <span data-ttu-id="413bb-364">**[1]**  Skapa en virtuell IP-resurs och hälsoavsökningen för hello intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="413bb-364">**[1]** Create a virtual IP resource and health-probe for hello internal load balancer</span></span>

   <pre><code>
   sudo crm node standby <b>nws-cl-0</b>
   sudo crm node online <b>nws-cl-1</b>
   sudo crm configure

   crm(live)configure# primitive drbd_<b>NWS</b>_ERS \
     ocf:linbit:drbd \
     params drbd_resource="<b>NWS</b>_ers" \
     op monitor interval="15" role="Master" \
     op monitor interval="30" role="Slave"

   crm(live)configure# ms ms-drbd_<b>NWS</b>_ERS drbd_<b>NWS</b>_ERS \
     meta master-max="1" master-node-max="1" clone-max="2" \
     clone-node-max="1" notify="true"

   crm(live)configure# primitive fs_<b>NWS</b>_ERS \
     ocf:heartbeat:Filesystem \
     params device=/dev/drbd1 \
     directory=/usr/sap/<b>NWS</b>/ERS<b>02</b>  \
     fstype=xfs \
     op monitor interval="10s"

   crm(live)configure# primitive vip_<b>NWS</b>_ERS IPaddr2 \
     params ip=<b>10.0.0.11</b> cidr_netmask=24 \
     op monitor interval=10 timeout=20

   crm(live)configure# primitive nc_<b>NWS</b>_ERS anything \
    params binfile="/usr/bin/nc" cmdline_options="-l -k 621<b>02</b>" \
    op monitor timeout=20s interval=10 depth=0

   crm(live)configure# group g-<b>NWS</b>_ERS nc_<b>NWS</b>_ERS vip_<b>NWS</b>_ERS fs_<b>NWS</b>_ERS

   crm(live)configure# order o-<b>NWS</b>_drbd_before_ERS inf: \
     ms-drbd_<b>NWS</b>_ERS:promote g-<b>NWS</b>_ERS:start
   
   crm(live)configure# colocation col-<b>NWS</b>_ERS_on_drbd inf: \
     ms-drbd_<b>NWS</b>_ERS:Master g-<b>NWS</b>_ERS
   
   crm(live)configure# commit
   # WARNING: Resources nc_NWS_ASCS,nc_NWS_ERS,nc_NWS_nfs violate uniqueness for parameter "binfile": "/usr/bin/nc"
   # Do you still want toocommit (y/n)? y

   crm(live)configure# exit
   
   </code></pre>
 
   <span data-ttu-id="413bb-365">Se till att hello klusterstatus är det ok och att alla resurser har startats.</span><span class="sxs-lookup"><span data-stu-id="413bb-365">Make sure that hello cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="413bb-366">Det är inte viktigt på vilka nod hello resurser körs.</span><span class="sxs-lookup"><span data-stu-id="413bb-366">It is not important on which node hello resources are running.</span></span>

   <pre><code>
   sudo crm_mon -r

   # Node <b>nws-cl-0: standby</b>
   # <b>Online: [ nws-cl-1 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      Stopped: [ nws-cl-0 ]
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   #  Master/Slave Set: ms-drbd_NWS_ERS [drbd_NWS_ERS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      Stopped: [ nws-cl-0 ]
   #  Resource Group: g-NWS_ERS
   #      nc_NWS_ERS (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ERS (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   </code></pre>

1. <span data-ttu-id="413bb-367">**[2]**  Installera SAP NetWeaver ÄNDARE</span><span class="sxs-lookup"><span data-stu-id="413bb-367">**[2]** Install SAP NetWeaver ERS</span></span>  

   <span data-ttu-id="413bb-368">Installera SAP NetWeaver ÄNDARE som rot på hello andra nod med ett virtuellt värdnamn som till exempel mappar toohello IP-adressen för hello klientdel konfiguration av belastningsutjämning för hello ÄNDARE <b>nws ändare</b>, <b>10.0.0.11</b> och hello-instansnummer som du använde för hello avsökning av hello belastningsutjämnare till exempel <b>02</b>.</span><span class="sxs-lookup"><span data-stu-id="413bb-368">Install SAP NetWeaver ERS as root on hello second node using a virtual hostname that maps toohello IP address of hello load balancer frontend configuration for hello ERS for example <b>nws-ers</b>, <b>10.0.0.11</b> and hello instance number that you used for hello probe of hello load balancer for example <b>02</b>.</span></span>

   <span data-ttu-id="413bb-369">Du kan använda hello sapinst parametern SAPINST_REMOTE_ACCESS_USER tooallow en icke-rotanvändaren tooconnect toosapinst.</span><span class="sxs-lookup"><span data-stu-id="413bb-369">You can use hello sapinst parameter SAPINST_REMOTE_ACCESS_USER tooallow a non-root user tooconnect toosapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   > [!NOTE]
   > <span data-ttu-id="413bb-370">Använd SWPM SP 20 PL 05 eller högre.</span><span class="sxs-lookup"><span data-stu-id="413bb-370">Please use SWPM SP 20 PL 05 or higher.</span></span> <span data-ttu-id="413bb-371">Lägre versioner korrekt inte hello behörigheter och hello installationen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="413bb-371">Lower versions do not set hello permissions correctly and hello installation will fail.</span></span>
   > 

1. <span data-ttu-id="413bb-372">**[1]**  Anpassa hello ASCS/SCS och ÄNDARE instans-profiler</span><span class="sxs-lookup"><span data-stu-id="413bb-372">**[1]** Adapt hello ASCS/SCS and ERS instance profiles</span></span>
 
   * <span data-ttu-id="413bb-373">ASCS/SCS-profil</span><span class="sxs-lookup"><span data-stu-id="413bb-373">ASCS/SCS profile</span></span>

   <pre><code> 
   sudo vi /sapmnt/<b>NWS</b>/profile/<b>NWS</b>_<b>ASCS00</b>_<b>nws-ascs</b>

   # Change hello restart command tooa start command
   #Restart_Program_01 = local $(_EN) pf=$(_PF)
   Start_Program_01 = local $(_EN) pf=$(_PF)

   # Add hello following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector

   # Add hello keep alive parameter
   enque/encni/set_so_keepalive = true
   </code></pre>

   * <span data-ttu-id="413bb-374">ÄNDARE profil</span><span class="sxs-lookup"><span data-stu-id="413bb-374">ERS profile</span></span>

   <pre><code> 
   sudo vi /sapmnt/<b>NWS</b>/profile/<b>NWS</b>_ERS<b>02</b>_<b>nws-ers</b>

   # Add hello following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector
   </code></pre>


1. <span data-ttu-id="413bb-375">**[A]**  Konfigurera Keep-Alive</span><span class="sxs-lookup"><span data-stu-id="413bb-375">**[A]** Configure Keep Alive</span></span>

   <span data-ttu-id="413bb-376">hello kommunikation mellan hello SAP NetWeaver programservern och hello ASCS/SCS dirigeras via en belastningsutjämnare för programvara.</span><span class="sxs-lookup"><span data-stu-id="413bb-376">hello communication between hello SAP NetWeaver application server and hello ASCS/SCS is routed through a software load balancer.</span></span> <span data-ttu-id="413bb-377">hello belastningsutjämnaren kopplar från inaktiva anslutningar efter en konfigurerbar tidsgräns.</span><span class="sxs-lookup"><span data-stu-id="413bb-377">hello load balancer disconnects inactive connections after a configurable timeout.</span></span> <span data-ttu-id="413bb-378">tooprevent det du behöver tooset en parameter i hello SAP NetWeaver ASCS/SCS profil och ändra inställningar för hello Linux-system.</span><span class="sxs-lookup"><span data-stu-id="413bb-378">tooprevent this you need tooset a parameter in hello SAP NetWeaver ASCS/SCS profile and change hello Linux system settings.</span></span> <span data-ttu-id="413bb-379">Läs [SAP Obs 1410736] [ 1410736] för mer information.</span><span class="sxs-lookup"><span data-stu-id="413bb-379">Please read [SAP Note 1410736][1410736] for more information.</span></span>
   
   <span data-ttu-id="413bb-380">Hej ASCS/SCS profil parametern placera/encni/set_so_keepalive har redan lagts till i hello sista steget.</span><span class="sxs-lookup"><span data-stu-id="413bb-380">hello ASCS/SCS profile parameter enque/encni/set_so_keepalive was already added in hello last step.</span></span>

   <pre><code> 
   # Change hello Linux system configuration
   sudo sysctl net.ipv4.tcp_keepalive_time=120
   </code></pre>

1. <span data-ttu-id="413bb-381">**[A]**  Konfigurera hello SAP användare efter hello installationen</span><span class="sxs-lookup"><span data-stu-id="413bb-381">**[A]** Configure hello SAP users after hello installation</span></span>
 
   <pre><code>
   # Add sidadm toohello haclient group
   sudo usermod -aG haclient <b>nws</b>adm   
   </code></pre>

1. <span data-ttu-id="413bb-382">**[1]**  Lägg till hello ASCS och ÄNDARE SAP services toohello sapservice-fil</span><span class="sxs-lookup"><span data-stu-id="413bb-382">**[1]** Add hello ASCS and ERS SAP services toohello sapservice file</span></span>

   <span data-ttu-id="413bb-383">Lägg till hello ASCS-posten toohello andra tjänstnoden och kopiera hello ÄNDARE service post toohello första noden.</span><span class="sxs-lookup"><span data-stu-id="413bb-383">Add hello ASCS service entry toohello second node and copy hello ERS service entry toohello first node.</span></span>

   <pre><code>
   cat /usr/sap/sapservices | grep ASCS<b>00</b> | sudo ssh <b>nws-cl-1</b> "cat >>/usr/sap/sapservices"
   sudo ssh <b>nws-cl-1</b> "cat /usr/sap/sapservices" | grep ERS<b>02</b> | sudo tee -a /usr/sap/sapservices
   </code></pre>

1. <span data-ttu-id="413bb-384">**[1]**  Skapa hello SAP klusterresurser</span><span class="sxs-lookup"><span data-stu-id="413bb-384">**[1]** Create hello SAP cluster resources</span></span>

   <pre><code>
   sudo crm configure property maintenance-mode="true"

   sudo crm configure

   crm(live)configure# primitive rsc_sap_<b>NWS</b>_ASCS<b>00</b> SAPInstance \
    operations $id=rsc_sap_<b>NWS</b>_ASCS<b>00</b>-operations \
    op monitor interval=11 timeout=60 on_fail=restart \
    params InstanceName=<b>NWS</b>_ASCS<b>00</b>_<b>nws-ascs</b> START_PROFILE="/sapmnt/<b>NWS</b>/profile/<b>NWS</b>_ASCS<b>00</b>_<b>nws-ascs</b>" \
    AUTOMATIC_RECOVER=false \
    meta resource-stickiness=5000 failure-timeout=60 migration-threshold=1 priority=10

   crm(live)configure# primitive rsc_sap_<b>NWS</b>_ERS<b>02</b> SAPInstance \
    operations $id=rsc_sap_<b>NWS</b>_ERS<b>02</b>-operations \
    op monitor interval=11 timeout=60 on_fail=restart \
    params InstanceName=<b>NWS</b>_ERS<b>02</b>_<b>nws-ers</b> START_PROFILE="/sapmnt/<b>NWS</b>/profile/<b>NWS</b>_ERS<b>02</b>_<b>nws-ers</b>" AUTOMATIC_RECOVER=false IS_ERS=true \
    meta priority=1000

   crm(live)configure# modgroup g-<b>NWS</b>_ASCS add rsc_sap_<b>NWS</b>_ASCS<b>00</b>
   crm(live)configure# modgroup g-<b>NWS</b>_ERS add rsc_sap_<b>NWS</b>_ERS<b>02</b>

   crm(live)configure# colocation col_sap_<b>NWS</b>_no_both -5000: g-<b>NWS</b>_ERS g-<b>NWS</b>_ASCS
   crm(live)configure# location loc_sap_<b>NWS</b>_failover_to_ers rsc_sap_<b>NWS</b>_ASCS<b>00</b> rule 2000: runs_ers_<b>NWS</b> eq 1
   crm(live)configure# order ord_sap_<b>NWS</b>_first_start_ascs Optional: rsc_sap_<b>NWS</b>_ASCS<b>00</b>:start rsc_sap_<b>NWS</b>_ERS<b>02</b>:stop symmetrical=false

   crm(live)configure# commit
   crm(live)configure# exit

   sudo crm configure property maintenance-mode="false"
   sudo crm node online <b>nws-cl-0</b>
   </code></pre>

   <span data-ttu-id="413bb-385">Se till att hello klusterstatus är det ok och att alla resurser har startats.</span><span class="sxs-lookup"><span data-stu-id="413bb-385">Make sure that hello cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="413bb-386">Det är inte viktigt på vilka nod hello resurser körs.</span><span class="sxs-lookup"><span data-stu-id="413bb-386">It is not important on which node hello resources are running.</span></span>

   <pre><code>
   sudo crm_mon -r

   # Online: <b>[ nws-cl-0 nws-cl-1 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      <b>Slaves: [ nws-cl-1 ]</b>
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-0</b>
   #      rsc_sap_NWS_ASCS00 (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-0</b>
   #  Master/Slave Set: ms-drbd_NWS_ERS [drbd_NWS_ERS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      <b>Slaves: [ nws-cl-0 ]</b>
   #  Resource Group: g-NWS_ERS
   #      nc_NWS_ERS (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ERS (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   #      rsc_sap_NWS_ERS02  (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-1</b>
   </code></pre>

### <a name="create-stonith-device"></a><span data-ttu-id="413bb-387">Skapa STONITH enhet</span><span class="sxs-lookup"><span data-stu-id="413bb-387">Create STONITH device</span></span>

<span data-ttu-id="413bb-388">Hej STONITH enhet använder ett huvudnamn för tjänsten tooauthorize mot Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="413bb-388">hello STONITH device uses a Service Principal tooauthorize against Microsoft Azure.</span></span> <span data-ttu-id="413bb-389">Följ dessa steg toocreate ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="413bb-389">Follow these steps toocreate a Service Principal.</span></span>

1. <span data-ttu-id="413bb-390">Gå för<https://portal.azure.com></span><span class="sxs-lookup"><span data-stu-id="413bb-390">Go too<https://portal.azure.com></span></span>
1. <span data-ttu-id="413bb-391">Öppna hello Azure Active Directory-bladet</span><span class="sxs-lookup"><span data-stu-id="413bb-391">Open hello Azure Active Directory blade</span></span>  
   <span data-ttu-id="413bb-392">Gå tooProperties och anteckna hello Directory-Id. Detta är hello **klient-id**.</span><span class="sxs-lookup"><span data-stu-id="413bb-392">Go tooProperties and write down hello Directory Id. This is hello **tenant id**.</span></span>
1. <span data-ttu-id="413bb-393">Klicka på appen registreringar</span><span class="sxs-lookup"><span data-stu-id="413bb-393">Click App registrations</span></span>
1. <span data-ttu-id="413bb-394">Klicka på Lägg till</span><span class="sxs-lookup"><span data-stu-id="413bb-394">Click Add</span></span>
1. <span data-ttu-id="413bb-395">Ange ett namn, Välj typ av program ”Web app/API”, ange en inloggnings-URL (till exempel http://localhost) och klicka på Skapa</span><span class="sxs-lookup"><span data-stu-id="413bb-395">Enter a Name, select Application Type "Web app/API", enter a sign-on URL (for example http://localhost) and click Create</span></span>
1. <span data-ttu-id="413bb-396">hello inloggnings-URL används inte och kan vara en giltig URL</span><span class="sxs-lookup"><span data-stu-id="413bb-396">hello sign-on URL is not used and can be any valid URL</span></span>
1. <span data-ttu-id="413bb-397">Välj hello nya App och klicka på nycklarna i hello på fliken Inställningar</span><span class="sxs-lookup"><span data-stu-id="413bb-397">Select hello new App and click Keys in hello Settings tab</span></span>
1. <span data-ttu-id="413bb-398">Ange en beskrivning för en ny nyckel, Välj ”upphör aldrig att gälla” och klicka på Spara</span><span class="sxs-lookup"><span data-stu-id="413bb-398">Enter a description for a new key, select "Never expires" and click Save</span></span>
1. <span data-ttu-id="413bb-399">Skriv ned hello värde.</span><span class="sxs-lookup"><span data-stu-id="413bb-399">Write down hello Value.</span></span> <span data-ttu-id="413bb-400">Den används som hello **lösenord** för hello tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="413bb-400">It is used as hello **password** for hello Service Principal</span></span>
1. <span data-ttu-id="413bb-401">Skriv ned hello program-Id. Den används som hello användarnamn (**inloggnings-id** i hello stegen nedan) för hello tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="413bb-401">Write down hello Application Id. It is used as hello username (**login id** in hello steps below) of hello Service Principal</span></span>

<span data-ttu-id="413bb-402">hello tjänstens huvudnamn har inte behörigheter tooaccess Azure-resurser som standard.</span><span class="sxs-lookup"><span data-stu-id="413bb-402">hello Service Principal does not have permissions tooaccess your Azure resources by default.</span></span> <span data-ttu-id="413bb-403">Du behöver toogive hello tjänstens huvudnamn behörigheter toostart och stoppa (frigöra) alla virtuella datorer i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="413bb-403">You need toogive hello Service Principal permissions toostart and stop (deallocate) all virtual machines of hello cluster.</span></span>

1. <span data-ttu-id="413bb-404">Gå toohttps://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="413bb-404">Go toohttps://portal.azure.com</span></span>
1. <span data-ttu-id="413bb-405">Öppna hello bladet för alla resurser</span><span class="sxs-lookup"><span data-stu-id="413bb-405">Open hello All resources blade</span></span>
1. <span data-ttu-id="413bb-406">Välj hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="413bb-406">Select hello virtual machine</span></span>
1. <span data-ttu-id="413bb-407">Klicka på åtkomstkontroll (IAM)</span><span class="sxs-lookup"><span data-stu-id="413bb-407">Click Access control (IAM)</span></span>
1. <span data-ttu-id="413bb-408">Klicka på Lägg till</span><span class="sxs-lookup"><span data-stu-id="413bb-408">Click Add</span></span>
1. <span data-ttu-id="413bb-409">Välj hello roll ägare</span><span class="sxs-lookup"><span data-stu-id="413bb-409">Select hello role Owner</span></span>
1. <span data-ttu-id="413bb-410">Ange hello namnet på hello-program som du skapade ovan</span><span class="sxs-lookup"><span data-stu-id="413bb-410">Enter hello name of hello application you created above</span></span>
1. <span data-ttu-id="413bb-411">Klicka på OK</span><span class="sxs-lookup"><span data-stu-id="413bb-411">Click OK</span></span>

#### <a name="1-create-hello-stonith-devices"></a><span data-ttu-id="413bb-412">**[1]**  Skapa hello STONITH enheter</span><span class="sxs-lookup"><span data-stu-id="413bb-412">**[1]** Create hello STONITH devices</span></span>

<span data-ttu-id="413bb-413">När du har redigerat hello behörigheter för hello virtuella datorer kan du konfigurera hello STONITH enheter i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="413bb-413">After you edited hello permissions for hello virtual machines, you can configure hello STONITH devices in hello cluster.</span></span>

<pre><code>
sudo crm configure

# replace hello bold string with your subscription id, resource group, tenant id, service principal id and password

crm(live)configure# primitive rsc_st_azure_1 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# primitive rsc_st_azure_2 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started

crm(live)configure# commit
crm(live)configure# exit
</code></pre>

#### <a name="1-enable-hello-use-of-a-stonith-device"></a><span data-ttu-id="413bb-414">**[1]**  Aktivera hello användning av en STONITH-enhet</span><span class="sxs-lookup"><span data-stu-id="413bb-414">**[1]** Enable hello use of a STONITH device</span></span>

<span data-ttu-id="413bb-415">Aktivera hello användning av en STONITH-enhet</span><span class="sxs-lookup"><span data-stu-id="413bb-415">Enable hello use of a STONITH device</span></span>

<pre><code>
sudo crm configure property stonith-enabled=true 
</code></pre>

## <a name="install-database"></a><span data-ttu-id="413bb-416">Installera databasen</span><span class="sxs-lookup"><span data-stu-id="413bb-416">Install database</span></span>

<span data-ttu-id="413bb-417">I det här exemplet är en SAP HANA System replikering installeras och konfigureras.</span><span class="sxs-lookup"><span data-stu-id="413bb-417">In this example an SAP HANA System Replication is installed and configured.</span></span> <span data-ttu-id="413bb-418">SAP HANA körs i samma kluster som hello SAP NetWeaver ASCS/SCS och ÄNDARE hello.</span><span class="sxs-lookup"><span data-stu-id="413bb-418">SAP HANA will run in hello same cluster as hello SAP NetWeaver ASCS/SCS and ERS.</span></span> <span data-ttu-id="413bb-419">Du kan också installera SAP HANA på ett kluster.</span><span class="sxs-lookup"><span data-stu-id="413bb-419">You can also install SAP HANA on a dedicated cluster.</span></span> <span data-ttu-id="413bb-420">Se [hög tillgänglighet för SAP HANA på Azure Virtual Machines (virtuella datorer)] [ sap-hana-ha] för mer information.</span><span class="sxs-lookup"><span data-stu-id="413bb-420">See [High Availability of SAP HANA on Azure Virtual Machines (VMs)][sap-hana-ha] for more information.</span></span>

### <a name="prepare-for-sap-hana-installation"></a><span data-ttu-id="413bb-421">Förbereda för SAP HANA-installation</span><span class="sxs-lookup"><span data-stu-id="413bb-421">Prepare for SAP HANA installation</span></span>

<span data-ttu-id="413bb-422">Normalt bör du använda LVM för volymer som lagrar data och loggfiler.</span><span class="sxs-lookup"><span data-stu-id="413bb-422">We generally recommend using LVM for volumes that store data and log files.</span></span> <span data-ttu-id="413bb-423">I testsyfte kan välja du också toostore hello data och loggfilen fil direkt på en vanlig disk.</span><span class="sxs-lookup"><span data-stu-id="413bb-423">For testing purposes, you can also choose toostore hello data and log file directly on a plain disk.</span></span>

1. <span data-ttu-id="413bb-424">**[A]**  LVM</span><span class="sxs-lookup"><span data-stu-id="413bb-424">**[A]** LVM</span></span>  
   <span data-ttu-id="413bb-425">hello exemplet nedan förutsätter att hello virtuella datorer har fyra datadiskar som är anslutna som ska använda toocreate två volymer.</span><span class="sxs-lookup"><span data-stu-id="413bb-425">hello example below assumes that hello virtual machines have four data disks attached that should be used toocreate two volumes.</span></span>
   
   <span data-ttu-id="413bb-426">Skapa fysiska volymer för alla diskar som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="413bb-426">Create physical volumes for all disks that you want toouse.</span></span>
   
   <pre><code>
   sudo pvcreate /dev/sdd
   sudo pvcreate /dev/sde
   sudo pvcreate /dev/sdf
   sudo pvcreate /dev/sdg
   </code></pre>
   
   <span data-ttu-id="413bb-427">Skapa en volym grupp för hello-datafiler, en volym grupp för hello loggfiler och en för hello delade katalogen för SAP HANA</span><span class="sxs-lookup"><span data-stu-id="413bb-427">Create a volume group for hello data files, one volume group for hello log files and one for hello shared directory of SAP HANA</span></span>
   
   <pre><code>
   sudo vgcreate vg_hana_data /dev/sdd /dev/sde
   sudo vgcreate vg_hana_log /dev/sdf
   sudo vgcreate vg_hana_shared /dev/sdg
   </code></pre>
   
   <span data-ttu-id="413bb-428">Skapa hello logiska volymer</span><span class="sxs-lookup"><span data-stu-id="413bb-428">Create hello logical volumes</span></span>
   
   <pre><code>
   sudo lvcreate -l 100%FREE -n hana_data vg_hana_data
   sudo lvcreate -l 100%FREE -n hana_log vg_hana_log
   sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared
   sudo mkfs.xfs /dev/vg_hana_data/hana_data
   sudo mkfs.xfs /dev/vg_hana_log/hana_log
   sudo mkfs.xfs /dev/vg_hana_shared/hana_shared
   </code></pre>
   
   <span data-ttu-id="413bb-429">Skapa hello montera kataloger och kopiera hello UUID för alla logiska volymer</span><span class="sxs-lookup"><span data-stu-id="413bb-429">Create hello mount directories and copy hello UUID of all logical volumes</span></span>
   
   <pre><code>
   sudo mkdir -p /hana/data
   sudo mkdir -p /hana/log
   sudo mkdir -p /hana/shared
   sudo chattr +i /hana/data
   sudo chattr +i /hana/log
   sudo chattr +i /hana/shared
   # write down hello id of /dev/vg_hana_data/hana_data, /dev/vg_hana_log/hana_log and /dev/vg_hana_shared/hana_shared
   sudo blkid
   </code></pre>
   
   <span data-ttu-id="413bb-430">Skapa autofs poster för hello tre logiska volymer</span><span class="sxs-lookup"><span data-stu-id="413bb-430">Create autofs entries for hello three logical volumes</span></span>
   
   <pre><code>
   sudo vi /etc/auto.direct
   </code></pre>
   
   <span data-ttu-id="413bb-431">Infoga den här raden toosudo vi /etc/auto.direct</span><span class="sxs-lookup"><span data-stu-id="413bb-431">Insert this line toosudo vi /etc/auto.direct</span></span>
   
   <pre><code>
   /hana/data -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_data/hana_data&gt;</b>
   /hana/log -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_log/hana_log&gt;</b>
   /hana/shared -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_shared/hana_shared&gt;</b>
   </code></pre>
   
   <span data-ttu-id="413bb-432">Montera hello nya volymer</span><span class="sxs-lookup"><span data-stu-id="413bb-432">Mount hello new volumes</span></span>
   
   <pre><code>
   sudo service autofs restart 
   </code></pre>

1. <span data-ttu-id="413bb-433">**[A]**  Vanlig diskar</span><span class="sxs-lookup"><span data-stu-id="413bb-433">**[A]** Plain Disks</span></span>  

   <span data-ttu-id="413bb-434">För små eller demonstrera system, du kan placera HANA data och loggfilen filer på en disk.</span><span class="sxs-lookup"><span data-stu-id="413bb-434">For small or demo systems, you can place your HANA data and log files on one disk.</span></span> <span data-ttu-id="413bb-435">hello följande kommandon skapar en partition på /dev/sdc och formatera den med xfs.</span><span class="sxs-lookup"><span data-stu-id="413bb-435">hello following commands create a partition on /dev/sdc and format it with xfs.</span></span>
   ```bash
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdd'
   sudo mkfs.xfs /dev/sdd1
   
   # write down hello id of /dev/sdd1
   sudo /sbin/blkid
   sudo vi /etc/auto.direct
   ```
   
   <span data-ttu-id="413bb-436">Infoga den här raden too/etc/auto.direct</span><span class="sxs-lookup"><span data-stu-id="413bb-436">Insert this line too/etc/auto.direct</span></span>
   <pre><code>
   /hana -fstype=xfs :UUID=<b>&lt;UUID&gt;</b>
   </code></pre>
   
   <span data-ttu-id="413bb-437">Skapa hello målkatalogen och montera hello disk.</span><span class="sxs-lookup"><span data-stu-id="413bb-437">Create hello target directory and mount hello disk.</span></span>
   
   <pre><code>
   sudo mkdir /hana
   sudo chattr +i /hana
   sudo service autofs restart
   </code></pre>

### <a name="installing-sap-hana"></a><span data-ttu-id="413bb-438">Installera SAP HANA</span><span class="sxs-lookup"><span data-stu-id="413bb-438">Installing SAP HANA</span></span>

<span data-ttu-id="413bb-439">hello följande baseras på kapitel 4 i hello [SAP HANA SR prestanda optimerade scenariot guiden] [ suse-hana-ha-guide] tooinstall SAP HANA System replikering.</span><span class="sxs-lookup"><span data-stu-id="413bb-439">hello following steps are based on chapter 4 of hello [SAP HANA SR Performance Optimized Scenario guide][suse-hana-ha-guide] tooinstall SAP HANA System Replication.</span></span> <span data-ttu-id="413bb-440">Läs den innan du fortsätter hello-installationen.</span><span class="sxs-lookup"><span data-stu-id="413bb-440">Please read it before you continue hello installation.</span></span>

1. <span data-ttu-id="413bb-441">**[A]**  Kör hdblcm från hello HANA DVD</span><span class="sxs-lookup"><span data-stu-id="413bb-441">**[A]** Run hdblcm from hello HANA DVD</span></span>
   
   <pre><code>
   sudo hdblcm --sid=<b>HDB</b> --number=<b>03</b> --action=install --batch --password=<b>&lt;password&gt;</b> --system_user_password=<b>&lt;password for system user&gt;</b>

   sudo /hana/shared/<b>HDB</b>/hdblcm/hdblcm --action=configure_internal_network --listen_interface=internal --internal_network=<b>10.0.0/24</b> --password=<b>&lt;password for system user&gt;</b> --batch
   </code></pre>

1. <span data-ttu-id="413bb-442">**[A]**  Uppgradera SAP Värdagenten</span><span class="sxs-lookup"><span data-stu-id="413bb-442">**[A]** Upgrade SAP Host Agent</span></span>

   <span data-ttu-id="413bb-443">Hämta hello senaste SAP Värdagenten Arkiv från hello [SAP Softwarecenter] [ sap-swcenter] och kör hello följande kommando tooupgrade hello agent.</span><span class="sxs-lookup"><span data-stu-id="413bb-443">Download hello latest SAP Host Agent archive from hello [SAP Softwarecenter][sap-swcenter] and run hello following command tooupgrade hello agent.</span></span> <span data-ttu-id="413bb-444">Ersätt hello sökvägen toohello toopoint toohello arkivfilen du laddade ned.</span><span class="sxs-lookup"><span data-stu-id="413bb-444">Replace hello path toohello archive toopoint toohello file you downloaded.</span></span>
   <pre><code>
   sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <b>&lt;path tooSAP Host Agent SAR&gt;</b> 
   </code></pre>

1. <span data-ttu-id="413bb-445">**[1]**  Skapa HANA replikering (som rot)</span><span class="sxs-lookup"><span data-stu-id="413bb-445">**[1]** Create HANA replication (as root)</span></span>  

   <span data-ttu-id="413bb-446">Kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="413bb-446">Run hello following command.</span></span> <span data-ttu-id="413bb-447">Kontrollera att tooreplace fetstil strängar (HANA System-ID HDB och instans antalet 03) med hello värdena för SAP HANA-installationen.</span><span class="sxs-lookup"><span data-stu-id="413bb-447">Make sure tooreplace bold strings (HANA System ID HDB and instance number 03) with hello values of your SAP HANA installation.</span></span>
   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
   hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN too<b>hdb</b>hasync' 
   hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
   </code></pre>

1. <span data-ttu-id="413bb-448">**[A]**  Skapa keystore posten (rot)</span><span class="sxs-lookup"><span data-stu-id="413bb-448">**[A]** Create keystore entry (as root)</span></span>

   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>&lt;passwd&gt;</b>
   </code></pre>

1. <span data-ttu-id="413bb-449">**[1]**  Backup database (som rot)</span><span class="sxs-lookup"><span data-stu-id="413bb-449">**[1]** Backup database (as root)</span></span>

   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbsql -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')" 
   </code></pre>

1. <span data-ttu-id="413bb-450">**[1]**  Växla toohello HANA sapsid användare och skapa hello primär plats.</span><span class="sxs-lookup"><span data-stu-id="413bb-450">**[1]** Switch toohello HANA sapsid user and create hello primary site.</span></span>

   <pre><code>
   su - <b>hdb</b>adm
   hdbnsutil -sr_enable –-name=<b>SITE1</b>
   </code></pre>

1. <span data-ttu-id="413bb-451">**[2]**  Växla toohello HANA sapsid användare och skapa hello sekundär plats.</span><span class="sxs-lookup"><span data-stu-id="413bb-451">**[2]** Switch toohello HANA sapsid user and create hello secondary site.</span></span>

   <pre><code>
   su - <b>hdb</b>adm
   sapcontrol -nr <b>03</b> -function StopWait 600 10
   hdbnsutil -sr_register --remoteHost=<b>nws-cl-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
   </code></pre>

1. <span data-ttu-id="413bb-452">**[1]**  Skapa SAP HANA klusterresurser</span><span class="sxs-lookup"><span data-stu-id="413bb-452">**[1]** Create SAP HANA cluster resources</span></span>

   <span data-ttu-id="413bb-453">Först skapa hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="413bb-453">First, create hello topology.</span></span>
   
   <pre><code>
   sudo crm configure

   # replace hello bold string with your instance number and HANA system id
   
   crm(live)configure# primitive rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b>   ocf:suse:SAPHanaTopology \
     operations $id="rsc_sap2_<b>HDB</b>_HDB<b>03</b>-operations" \
     op monitor interval="10" timeout="600" \
     op start interval="0" timeout="600" \
     op stop interval="0" timeout="300" \
     params SID="<b>HDB</b>" InstanceNumber="<b>03</b>"
    
   crm(live)configure# clone cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \
     meta is-managed="true" clone-node-max="1" target-role="Started" interleave="true"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>
   
   <span data-ttu-id="413bb-454">Skapa sedan hello HANA resurser</span><span class="sxs-lookup"><span data-stu-id="413bb-454">Next, create hello HANA resources</span></span>
   
   <pre><code>
   sudo crm configure

   # replace hello bold string with your instance number, HANA system id and hello frontend IP address of hello Azure load balancer. 
    
   crm(live)configure# primitive rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHana \
     operations $id="rsc_sap_<b>HDB</b>_HDB<b>03</b>-operations" \
     op start interval="0" timeout="3600" \
     op stop interval="0" timeout="3600" \
     op promote interval="0" timeout="3600" \
     op monitor interval="60" role="Master" timeout="700" \
     op monitor interval="61" role="Slave" timeout="700" \
     params SID="<b>HDB</b>" InstanceNumber="<b>03</b>" PREFER_SITE_TAKEOVER="true" \
     DUPLICATE_PRIMARY_TIMEOUT="7200" AUTOMATED_REGISTER="false"
    
   crm(live)configure# ms msl_SAPHana_<b>HDB</b>_HDB<b>03</b> rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> \
     meta is-managed="true" notify="true" clone-max="2" clone-node-max="1" \
     target-role="Started" interleave="true"
    
   crm(live)configure# primitive rsc_ip_<b>HDB</b>_HDB<b>03</b> ocf:heartbeat:IPaddr2 \ 
     meta target-role="Started" is-managed="true" \ 
     operations $id="rsc_ip_<b>HDB</b>_HDB<b>03</b>-operations" \ 
     op monitor interval="10s" timeout="20s" \ 
     params ip="<b>10.0.0.12</b>" 

   crm(live)configure# primitive rsc_nc_<b>HDB</b>_HDB<b>03</b> anything \ 
     params binfile="/usr/bin/nc" cmdline_options="-l -k 625<b>03</b>" \ 
     op monitor timeout=20s interval=10 depth=0 

   crm(live)configure# group g_ip_<b>HDB</b>_HDB<b>03</b> rsc_ip_<b>HDB</b>_HDB<b>03</b> rsc_nc_<b>HDB</b>_HDB<b>03</b>
    
   crm(live)configure# colocation col_saphana_ip_<b>HDB</b>_HDB<b>03</b> 2000: g_ip_<b>HDB</b>_HDB<b>03</b>:Started \ 
     msl_SAPHana_<b>HDB</b>_HDB<b>03</b>:Master  

   crm(live)configure# order ord_SAPHana_<b>HDB</b>_HDB<b>03</b> 2000: cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \ 
     msl_SAPHana_<b>HDB</b>_HDB<b>03</b>
    
   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

   <span data-ttu-id="413bb-455">Se till att hello klusterstatus är det ok och att alla resurser har startats.</span><span class="sxs-lookup"><span data-stu-id="413bb-455">Make sure that hello cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="413bb-456">Det är inte viktigt på vilka nod hello resurser körs.</span><span class="sxs-lookup"><span data-stu-id="413bb-456">It is not important on which node hello resources are running.</span></span>

   <pre><code>
   sudo crm_mon -r

   # <b>Online: [ nws-cl-0 nws-cl-1 ]</b>
   # 
   # Full list of resources:
   # 
   #  Master/Slave Set: ms-drbd_NWS_ASCS [drbd_NWS_ASCS]
   #      <b>Masters: [ nws-cl-1 ]</b>
   #      <b>Slaves: [ nws-cl-0 ]</b>
   #  Resource Group: g-NWS_ASCS
   #      nc_NWS_ASCS        (ocf::heartbeat:anything):      <b>Started nws-cl-1</b>
   #      vip_NWS_ASCS       (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-1</b>
   #      fs_NWS_ASCS        (ocf::heartbeat:Filesystem):    <b>Started nws-cl-1</b>
   #      rsc_sap_NWS_ASCS00 (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-1</b>
   #  Master/Slave Set: ms-drbd_NWS_ERS [drbd_NWS_ERS]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      <b>Slaves: [ nws-cl-1 ]</b>
   #  Resource Group: g-NWS_ERS
   #      nc_NWS_ERS (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   #      vip_NWS_ERS        (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      fs_NWS_ERS (ocf::heartbeat:Filesystem):    <b>Started nws-cl-0</b>
   #      rsc_sap_NWS_ERS02  (ocf::heartbeat:SAPInstance):   <b>Started nws-cl-0</b>
   #  Clone Set: cln_SAPHanaTopology_HDB_HDB03 [rsc_SAPHanaTopology_HDB_HDB03]
   #      <b>Started: [ nws-cl-0 nws-cl-1 ]</b>
   #  Master/Slave Set: msl_SAPHana_HDB_HDB03 [rsc_SAPHana_HDB_HDB03]
   #      <b>Masters: [ nws-cl-0 ]</b>
   #      <b>Slaves: [ nws-cl-1 ]</b>
   #  Resource Group: g_ip_HDB_HDB03
   #      rsc_ip_HDB_HDB03   (ocf::heartbeat:IPaddr2):       <b>Started nws-cl-0</b>
   #      rsc_nc_HDB_HDB03   (ocf::heartbeat:anything):      <b>Started nws-cl-0</b>
   # rsc_st_azure_1  (stonith:fence_azure_arm):      <b>Started nws-cl-0</b>
   # rsc_st_azure_2  (stonith:fence_azure_arm):      <b>Started nws-cl-1</b>
   </code></pre>

1. <span data-ttu-id="413bb-457">**[1]**  Installera hello SAP NetWeaver-databasinstans</span><span class="sxs-lookup"><span data-stu-id="413bb-457">**[1]** Install hello SAP NetWeaver database instance</span></span>

   <span data-ttu-id="413bb-458">Installera hello SAP NetWeaver-databasinstans som rot med hjälp av ett virtuellt värdnamn som mappar toohello IP-adressen för hello klientdel konfiguration av belastningsutjämning för hello databasen till exempel <b>nws-db</b> och <b>10.0.0.12</b>.</span><span class="sxs-lookup"><span data-stu-id="413bb-458">Install hello SAP NetWeaver database instance as root using a virtual hostname that maps toohello IP address of hello load balancer frontend configuration for hello database for example <b>nws-db</b> and <b>10.0.0.12</b>.</span></span>

   <span data-ttu-id="413bb-459">Du kan använda hello sapinst parametern SAPINST_REMOTE_ACCESS_USER tooallow en icke-rotanvändaren tooconnect toosapinst.</span><span class="sxs-lookup"><span data-stu-id="413bb-459">You can use hello sapinst parameter SAPINST_REMOTE_ACCESS_USER tooallow a non-root user tooconnect toosapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

## <a name="sap-netweaver-application-server-installation"></a><span data-ttu-id="413bb-460">SAP NetWeaver application server-installation</span><span class="sxs-lookup"><span data-stu-id="413bb-460">SAP NetWeaver application server installation</span></span>

<span data-ttu-id="413bb-461">Följ dessa steg tooinstall en SAP-programserver.</span><span class="sxs-lookup"><span data-stu-id="413bb-461">Follow these steps tooinstall an SAP application server.</span></span> <span data-ttu-id="413bb-462">hello stegen nedan förutsätter att du installerar hello Programserver på en annan server än hello ASCS/SCS och HANA servrar.</span><span class="sxs-lookup"><span data-stu-id="413bb-462">hello steps bellow assume that you install hello application server on a server different from hello ASCS/SCS and HANA servers.</span></span> <span data-ttu-id="413bb-463">Annars behövs några av hello stegen nedan (t.ex. Konfigurera värdnamnsmatchning) inte.</span><span class="sxs-lookup"><span data-stu-id="413bb-463">Otherwise some of hello steps below (like configuring host name resolution) are not needed.</span></span>

1. <span data-ttu-id="413bb-464">Konfigurera värdnamnsmatchning</span><span class="sxs-lookup"><span data-stu-id="413bb-464">Setup host name resolution</span></span>    
   <span data-ttu-id="413bb-465">Du kan använda en DNS-server, eller så kan du ändra hello/etc/hosts på alla noder.</span><span class="sxs-lookup"><span data-stu-id="413bb-465">You can either use a DNS server or modify hello /etc/hosts on all nodes.</span></span> <span data-ttu-id="413bb-466">Det här exemplet visar hur toouse hello/etc/hosts-filen.</span><span class="sxs-lookup"><span data-stu-id="413bb-466">This example shows how toouse hello /etc/hosts file.</span></span>
   <span data-ttu-id="413bb-467">Ersätt hello IP-adress och hello värdnamn i hello följande kommandon</span><span class="sxs-lookup"><span data-stu-id="413bb-467">Replace hello IP address and hello hostname in hello following commands</span></span>
   ```bash
   sudo vi /etc/hosts
   ```
   <span data-ttu-id="413bb-468">Infoga hello följande rader för/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="413bb-468">Insert hello following lines too/etc/hosts.</span></span> <span data-ttu-id="413bb-469">Ändra hello IP-adress och värddatornamn toomatch din miljö</span><span class="sxs-lookup"><span data-stu-id="413bb-469">Change hello IP address and hostname toomatch your environment</span></span>    
    
   <pre><code>
   # IP address of hello load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   # IP address of hello load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.0.0.10 nws-ascs</b>
   # IP address of hello load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.0.0.11 nws-ers</b>
   # IP address of hello load balancer frontend configuration for database
   <b>10.0.0.12 nws-db</b>
   # IP address of hello application server
   <b>10.0.0.8 nws-di-0</b>
   </code></pre>

1. <span data-ttu-id="413bb-470">Skapa hello sapmnt katalog</span><span class="sxs-lookup"><span data-stu-id="413bb-470">Create hello sapmnt directory</span></span>

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NWS</b>
   sudo mkdir -p /usr/sap/trans

   sudo chattr +i /sapmnt/<b>NWS</b>
   sudo chattr +i /usr/sap/trans
   </code></pre>

1. <span data-ttu-id="413bb-471">Konfigurera autofs</span><span class="sxs-lookup"><span data-stu-id="413bb-471">Configure autofs</span></span>
 
   <pre><code>
   sudo vi /etc/auto.master

   # Add hello following line toohello file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   <span data-ttu-id="413bb-472">Skapa en ny fil med</span><span class="sxs-lookup"><span data-stu-id="413bb-472">Create a new file with</span></span>

   <pre><code>
   sudo vi /etc/auto.direct

   # Add hello following lines toohello file, save and exit
   /sapmnt/<b>NWS</b> -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/trans
   </code></pre>

   <span data-ttu-id="413bb-473">Starta om autofs toomount hello nya resurser</span><span class="sxs-lookup"><span data-stu-id="413bb-473">Restart autofs toomount hello new shares</span></span>

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. <span data-ttu-id="413bb-474">Konfigurera växlingsfilen</span><span class="sxs-lookup"><span data-stu-id="413bb-474">Configure SWAP file</span></span>
 
   <pre><code>
   sudo vi /etc/waagent.conf

   # Set hello property ResourceDisk.EnableSwap tooy
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>

   # Set hello size of hello SWAP file with property ResourceDisk.SwapSizeMB
   # hello free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check hello SWAP space with command swapon
   # Size of hello swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   <span data-ttu-id="413bb-475">Starta om hello Agent tooactivate hello ändring</span><span class="sxs-lookup"><span data-stu-id="413bb-475">Restart hello Agent tooactivate hello change</span></span>

   <pre><code>
   sudo service waagent restart
   </code></pre>

1. <span data-ttu-id="413bb-476">Installera SAP NetWeaver programserver</span><span class="sxs-lookup"><span data-stu-id="413bb-476">Install SAP NetWeaver application server</span></span>

   <span data-ttu-id="413bb-477">Installera en primär eller ytterligare SAP NetWeaver programserver.</span><span class="sxs-lookup"><span data-stu-id="413bb-477">Install a primary or additional SAP NetWeaver applications server.</span></span>

   <span data-ttu-id="413bb-478">Du kan använda hello sapinst parametern SAPINST_REMOTE_ACCESS_USER tooallow en icke-rotanvändaren tooconnect toosapinst.</span><span class="sxs-lookup"><span data-stu-id="413bb-478">You can use hello sapinst parameter SAPINST_REMOTE_ACCESS_USER tooallow a non-root user tooconnect toosapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. <span data-ttu-id="413bb-479">Uppdatera SAP HANA säker lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="413bb-479">Update SAP HANA secure store</span></span>

   <span data-ttu-id="413bb-480">Uppdatera hello SAP HANA säker lagra toopoint toohello virtuella namnet på hello SAP HANA System Replication installationen.</span><span class="sxs-lookup"><span data-stu-id="413bb-480">Update hello SAP HANA secure store toopoint toohello virtual name of hello SAP HANA System Replication setup.</span></span>
   <pre><code>
   su - <b>nws</b>adm
   hdbuserstore SET DEFAULT <b>nws-db</b>:3<b>03</b>15 <b>SAPABAP1</b> <b>&lt;password of ABAP schema&gt;</b>
   </code></pre>

## <a name="next-steps"></a><span data-ttu-id="413bb-481">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="413bb-481">Next steps</span></span>
* <span data-ttu-id="413bb-482">[Azure virtuella datorer planering och implementering för SAP][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="413bb-482">[Azure Virtual Machines planning and implementation for SAP][planning-guide]</span></span>
* <span data-ttu-id="413bb-483">[Distribution av Azure virtuella datorer för SAP][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="413bb-483">[Azure Virtual Machines deployment for SAP][deployment-guide]</span></span>
* <span data-ttu-id="413bb-484">[Azure virtuella datorer DBMS-distribution för SAP][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="413bb-484">[Azure Virtual Machines DBMS deployment for SAP][dbms-guide]</span></span>
* <span data-ttu-id="413bb-485">hur tooestablish hög tillgänglighet och planera för katastrofåterställning för SAP HANA i Azure (stora instanser), se toolearn [SAP HANA (stora instanser) hög tillgänglighet och katastrofåterställning recovery på Azure](hana-overview-high-availability-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="413bb-485">toolearn how tooestablish high availability and plan for disaster recovery of SAP HANA on Azure (large instances), see [SAP HANA (large instances) high availability and disaster recovery on Azure](hana-overview-high-availability-disaster-recovery.md).</span></span>
* <span data-ttu-id="413bb-486">hur tooestablish hög tillgänglighet och planera för katastrofåterställning för SAP HANA på Azure Virtual Machines, se toolearn [hög tillgänglighet för SAP HANA på Azure Virtual Machines (virtuella datorer)][sap-hana-ha]</span><span class="sxs-lookup"><span data-stu-id="413bb-486">toolearn how tooestablish high availability and plan for disaster recovery of SAP HANA on Azure VMs, see [High Availability of SAP HANA on Azure Virtual Machines (VMs)][sap-hana-ha]</span></span>
