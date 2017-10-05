---
title: "Azure virtuella datorer hög tillgänglighet för SAP NetWeaver på SUSE Linux Enterprise Server för SAP-program | Microsoft Docs"
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
ms.openlocfilehash: 16e09797926f29bc18cb05671c986c74f9c2d4f8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms-on-suse-linux-enterprise-server-for-sap-applications"></a><span data-ttu-id="f8d96-103">Hög tillgänglighet för SAP NetWeaver på Azure Virtual Machines på SUSE Linux Enterprise Server för SAP-program</span><span class="sxs-lookup"><span data-stu-id="f8d96-103">High availability for SAP NetWeaver on Azure VMs on SUSE Linux Enterprise Server for SAP applications</span></span>

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

<span data-ttu-id="f8d96-104">[2205917]:https://launchpad.support.sap.com/#/notes/2205917</span><span class="sxs-lookup"><span data-stu-id="f8d96-104">[2205917]:https://launchpad.support.sap.com/#/notes/2205917</span></span>
<span data-ttu-id="f8d96-105">[1944799]:https://launchpad.support.sap.com/#/notes/1944799</span><span class="sxs-lookup"><span data-stu-id="f8d96-105">[1944799]:https://launchpad.support.sap.com/#/notes/1944799</span></span>
<span data-ttu-id="f8d96-106">[1928533]:https://launchpad.support.sap.com/#/notes/1928533</span><span class="sxs-lookup"><span data-stu-id="f8d96-106">[1928533]:https://launchpad.support.sap.com/#/notes/1928533</span></span>
<span data-ttu-id="f8d96-107">[2015553]:https://launchpad.support.sap.com/#/notes/2015553</span><span class="sxs-lookup"><span data-stu-id="f8d96-107">[2015553]:https://launchpad.support.sap.com/#/notes/2015553</span></span>
<span data-ttu-id="f8d96-108">[2178632]:https://launchpad.support.sap.com/#/notes/2178632</span><span class="sxs-lookup"><span data-stu-id="f8d96-108">[2178632]:https://launchpad.support.sap.com/#/notes/2178632</span></span>
<span data-ttu-id="f8d96-109">[2191498]:https://launchpad.support.sap.com/#/notes/2191498</span><span class="sxs-lookup"><span data-stu-id="f8d96-109">[2191498]:https://launchpad.support.sap.com/#/notes/2191498</span></span>
<span data-ttu-id="f8d96-110">[2243692]:https://launchpad.support.sap.com/#/notes/2243692</span><span class="sxs-lookup"><span data-stu-id="f8d96-110">[2243692]:https://launchpad.support.sap.com/#/notes/2243692</span></span>
<span data-ttu-id="f8d96-111">[1984787]:https://launchpad.support.sap.com/#/notes/1984787</span><span class="sxs-lookup"><span data-stu-id="f8d96-111">[1984787]:https://launchpad.support.sap.com/#/notes/1984787</span></span>
<span data-ttu-id="f8d96-112">[1999351]:https://launchpad.support.sap.com/#/notes/1999351</span><span class="sxs-lookup"><span data-stu-id="f8d96-112">[1999351]:https://launchpad.support.sap.com/#/notes/1999351</span></span>
[1410736]:https://launchpad.support.sap.com/#/notes/1410736

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[suse-drbd-guide]:https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha_techguides/book_sleha_techguides.html

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged-md%2Fazuredeploy.json
[template-file-server]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-file-server-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability.md

<span data-ttu-id="f8d96-113">Den här artikeln beskriver hur du distribuerar virtuella datorer, konfigurera virtuella datorer, installera kluster framework och installera en högtillgänglig SAP NetWeaver 7,50.</span><span class="sxs-lookup"><span data-stu-id="f8d96-113">This article describes how to deploy the virtual machines, configure the virtual machines, install the cluster framework and install a highly available SAP NetWeaver 7.50 system.</span></span>
<span data-ttu-id="f8d96-114">I exempelkonfigurationer installationskommandon osv. ASCS-instansnummer 00, ÄNDARE instansnummer 02 och SAP System-ID NWS används.</span><span class="sxs-lookup"><span data-stu-id="f8d96-114">In the example configurations, installation commands etc. ASCS instance number 00, ERS instance number 02 and SAP System ID NWS is used.</span></span> <span data-ttu-id="f8d96-115">Namnen på de resurser (till exempel virtuella datorer, virtuella nätverk) i exemplet förutsätter att du har använt den [konvergerat mallen] [ template-converged] med SAP system-ID NWS att skapa resurser.</span><span class="sxs-lookup"><span data-stu-id="f8d96-115">The names of the resources (for example virtual machines, virtual networks) in the example assume that you have used the [converged template][template-converged] with SAP system ID NWS to create the resources.</span></span>

<span data-ttu-id="f8d96-116">Läs följande SAP anteckningar och papper först</span><span class="sxs-lookup"><span data-stu-id="f8d96-116">Read the following SAP Notes and papers first</span></span>

* <span data-ttu-id="f8d96-117">SAP-kommentar [1928533], som innehåller:</span><span class="sxs-lookup"><span data-stu-id="f8d96-117">SAP Note [1928533], which has:</span></span>
  * <span data-ttu-id="f8d96-118">Lista över Azure VM-storlekar som stöds för distribution av program</span><span class="sxs-lookup"><span data-stu-id="f8d96-118">List of Azure VM sizes that are supported for the deployment of SAP software</span></span>
  * <span data-ttu-id="f8d96-119">Viktiga kapacitetsinformation för Azure VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="f8d96-119">Important capacity information for Azure VM sizes</span></span>
  * <span data-ttu-id="f8d96-120">Stöds SAP-programvara och operativsystem (OS) och kombinationer av databasen</span><span class="sxs-lookup"><span data-stu-id="f8d96-120">Supported SAP software, and operating system (OS) and database combinations</span></span>
  * <span data-ttu-id="f8d96-121">SAP kernel version som krävs för Windows och Linux i Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="f8d96-121">Required SAP kernel version for Windows and Linux on Microsoft Azure</span></span>

* <span data-ttu-id="f8d96-122">SAP-kommentar [2015553] listar kraven för stöd för SAP SAP programvarudistributioner i Azure.</span><span class="sxs-lookup"><span data-stu-id="f8d96-122">SAP Note [2015553] lists prerequisites for SAP-supported SAP software deployments in Azure.</span></span>
* <span data-ttu-id="f8d96-123">SAP-kommentar [2205917] har rekommenderat OS-inställningar för SUSE Linux Enterprise Server för SAP-program</span><span class="sxs-lookup"><span data-stu-id="f8d96-123">SAP Note [2205917] has recommended OS settings for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="f8d96-124">SAP-kommentar [1944799] har SAP HANA riktlinjer för SUSE Linux Enterprise Server för SAP-program</span><span class="sxs-lookup"><span data-stu-id="f8d96-124">SAP Note [1944799] has SAP HANA Guidelines for SUSE Linux Enterprise Server for SAP Applications</span></span>
* <span data-ttu-id="f8d96-125">SAP-kommentar [2178632] innehåller detaljerad information om all övervakning mått som rapporterats för SAP i Azure.</span><span class="sxs-lookup"><span data-stu-id="f8d96-125">SAP Note [2178632] has detailed information about all monitoring metrics reported for SAP in Azure.</span></span>
* <span data-ttu-id="f8d96-126">SAP-kommentar [2191498] har Värdagenten för SAP-version som krävs för Linux i Azure.</span><span class="sxs-lookup"><span data-stu-id="f8d96-126">SAP Note [2191498] has the required SAP Host Agent version for Linux in Azure.</span></span>
* <span data-ttu-id="f8d96-127">SAP-kommentar [2243692] har licensieringsinformation SAP på Linux i Azure.</span><span class="sxs-lookup"><span data-stu-id="f8d96-127">SAP Note [2243692] has information about SAP licensing on Linux in Azure.</span></span>
* <span data-ttu-id="f8d96-128">SAP-kommentar [1984787] har allmän information om SUSE Linux Enterprise Server 12.</span><span class="sxs-lookup"><span data-stu-id="f8d96-128">SAP Note [1984787] has general information about SUSE Linux Enterprise Server 12.</span></span>
* <span data-ttu-id="f8d96-129">SAP-kommentar [1999351] innehåller ytterligare felsökningsinformation för Azure förbättrad övervakning av tillägget för SAP.</span><span class="sxs-lookup"><span data-stu-id="f8d96-129">SAP Note [1999351] has additional troubleshooting information for the Azure Enhanced Monitoring Extension for SAP.</span></span>
* <span data-ttu-id="f8d96-130">[SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) har alla nödvändiga SAP anteckningar för Linux.</span><span class="sxs-lookup"><span data-stu-id="f8d96-130">[SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) has all required SAP Notes for Linux.</span></span>
* <span data-ttu-id="f8d96-131">[Azure virtuella datorer planering och implementering för SAP på Linux][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="f8d96-131">[Azure Virtual Machines planning and implementation for SAP on Linux][planning-guide]</span></span>
* <span data-ttu-id="f8d96-132">[Distribution av Azure virtuella datorer för SAP på Linux (den här artikeln)][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="f8d96-132">[Azure Virtual Machines deployment for SAP on Linux (this article)][deployment-guide]</span></span>
* <span data-ttu-id="f8d96-133">[Azure virtuella datorer DBMS-distribution för SAP på Linux][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="f8d96-133">[Azure Virtual Machines DBMS deployment for SAP on Linux][dbms-guide]</span></span>
* <span data-ttu-id="f8d96-134">[Prestanda för SAP HANA SR optimerade Scenario][suse-hana-ha-guide]</span><span class="sxs-lookup"><span data-stu-id="f8d96-134">[SAP HANA SR Performance Optimized Scenario][suse-hana-ha-guide]</span></span>  
  <span data-ttu-id="f8d96-135">Guiden innehåller all information som krävs för att ställa in SAP HANA System Replication lokala.</span><span class="sxs-lookup"><span data-stu-id="f8d96-135">The guide contains all required information to set up SAP HANA System Replication on-premises.</span></span> <span data-ttu-id="f8d96-136">Använd den här guiden som utgångspunkt.</span><span class="sxs-lookup"><span data-stu-id="f8d96-136">Use this guide as a baseline.</span></span>
* <span data-ttu-id="f8d96-137">[Hög NFS lagringsutrymme med DRBD och Pacemaker] [ suse-drbd-guide] guiden innehåller all nödvändig information för att ställa in en NFS-server med hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="f8d96-137">[Highly Available NFS Storage with DRBD and Pacemaker][suse-drbd-guide] The guide contains all required information to set up a highly available NFS server.</span></span> <span data-ttu-id="f8d96-138">Använd den här guiden som utgångspunkt.</span><span class="sxs-lookup"><span data-stu-id="f8d96-138">Use this guide as a baseline.</span></span>


## <a name="overview"></a><span data-ttu-id="f8d96-139">Översikt</span><span class="sxs-lookup"><span data-stu-id="f8d96-139">Overview</span></span>

<span data-ttu-id="f8d96-140">För att uppnå hög tillgänglighet kräver SAP NetWeaver NFS-servern.</span><span class="sxs-lookup"><span data-stu-id="f8d96-140">To achieve high availability, SAP NetWeaver requires an NFS server.</span></span> <span data-ttu-id="f8d96-141">NFS-servern är konfigurerad i separata kluster och kan användas av flera SAP-system.</span><span class="sxs-lookup"><span data-stu-id="f8d96-141">The NFS server is configured in a separate cluster and can be used by multiple SAP systems.</span></span>

![Översikt över SAP NetWeaver hög tillgänglighet](./media/high-availability-guide-suse/img_001.png)

<span data-ttu-id="f8d96-143">NFS-servern, SAP NetWeaver ASCS, SAP NetWeaver SCS, SAP NetWeaver ÄNDARE och SAP HANA-databas ska du använda virtuella värdnamn och virtuella IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="f8d96-143">The NFS server, SAP NetWeaver ASCS, SAP NetWeaver SCS, SAP NetWeaver ERS and the SAP HANA database use virtual hostname and virtual IP addresses.</span></span> <span data-ttu-id="f8d96-144">På Azure krävs en belastningsutjämnare för att använda en virtuell IP-adress.</span><span class="sxs-lookup"><span data-stu-id="f8d96-144">On Azure, a load balancer is required to use a virtual IP address.</span></span> <span data-ttu-id="f8d96-145">I följande lista visar konfigurationen av belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="f8d96-145">The following list shows the configuration of the load balancer.</span></span>

### <a name="nfs-server"></a><span data-ttu-id="f8d96-146">NFS-Server</span><span class="sxs-lookup"><span data-stu-id="f8d96-146">NFS Server</span></span>
* <span data-ttu-id="f8d96-147">Konfiguration för klientdel</span><span class="sxs-lookup"><span data-stu-id="f8d96-147">Frontend configuration</span></span>
  * <span data-ttu-id="f8d96-148">IP-adressen 10.0.0.4</span><span class="sxs-lookup"><span data-stu-id="f8d96-148">IP address 10.0.0.4</span></span>
* <span data-ttu-id="f8d96-149">Backend-konfiguration</span><span class="sxs-lookup"><span data-stu-id="f8d96-149">Backend configuration</span></span>
  * <span data-ttu-id="f8d96-150">Ansluten till primära nätverksgränssnitt för alla virtuella datorer som ska ingå i klustret för NFS</span><span class="sxs-lookup"><span data-stu-id="f8d96-150">Connected to primary network interfaces of all virtual machines that should be part of the NFS cluster</span></span>
* <span data-ttu-id="f8d96-151">Avsökningsport</span><span class="sxs-lookup"><span data-stu-id="f8d96-151">Probe Port</span></span>
  * <span data-ttu-id="f8d96-152">Port 61000</span><span class="sxs-lookup"><span data-stu-id="f8d96-152">Port 61000</span></span>
* <span data-ttu-id="f8d96-153">Regler för belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="f8d96-153">Loadbalancing rules</span></span>
  * <span data-ttu-id="f8d96-154">2049 TCP</span><span class="sxs-lookup"><span data-stu-id="f8d96-154">2049 TCP</span></span> 
  * <span data-ttu-id="f8d96-155">2049 UDP</span><span class="sxs-lookup"><span data-stu-id="f8d96-155">2049 UDP</span></span>

### <a name="ascs"></a><span data-ttu-id="f8d96-156">(A) SCS</span><span class="sxs-lookup"><span data-stu-id="f8d96-156">(A)SCS</span></span>
* <span data-ttu-id="f8d96-157">Konfiguration för klientdel</span><span class="sxs-lookup"><span data-stu-id="f8d96-157">Frontend configuration</span></span>
  * <span data-ttu-id="f8d96-158">IP-adress 10.0.0.10</span><span class="sxs-lookup"><span data-stu-id="f8d96-158">IP address 10.0.0.10</span></span>
* <span data-ttu-id="f8d96-159">Backend-konfiguration</span><span class="sxs-lookup"><span data-stu-id="f8d96-159">Backend configuration</span></span>
  * <span data-ttu-id="f8d96-160">Ansluten till primära nätverksgränssnitt för alla virtuella datorer som ska ingå i (A) SCS/ÄNDARE kluster</span><span class="sxs-lookup"><span data-stu-id="f8d96-160">Connected to primary network interfaces of all virtual machines that should be part of the (A)SCS/ERS cluster</span></span>
* <span data-ttu-id="f8d96-161">Avsökningsport</span><span class="sxs-lookup"><span data-stu-id="f8d96-161">Probe Port</span></span>
  * <span data-ttu-id="f8d96-162">Port 620**&lt;nr&gt;**</span><span class="sxs-lookup"><span data-stu-id="f8d96-162">Port 620**&lt;nr&gt;**</span></span>
* <span data-ttu-id="f8d96-163">Regler för belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="f8d96-163">Loadbalancing rules</span></span>
  * <span data-ttu-id="f8d96-164">32**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="f8d96-164">32**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="f8d96-165">36**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="f8d96-165">36**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="f8d96-166">39**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="f8d96-166">39**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="f8d96-167">81**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="f8d96-167">81**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="f8d96-168">5**&lt;nr&gt;**13 TCP</span><span class="sxs-lookup"><span data-stu-id="f8d96-168">5**&lt;nr&gt;**13 TCP</span></span>
  * <span data-ttu-id="f8d96-169">5**&lt;nr&gt;**14 TCP</span><span class="sxs-lookup"><span data-stu-id="f8d96-169">5**&lt;nr&gt;**14 TCP</span></span>
  * <span data-ttu-id="f8d96-170">5**&lt;nr&gt;**16 TCP</span><span class="sxs-lookup"><span data-stu-id="f8d96-170">5**&lt;nr&gt;**16 TCP</span></span>

### <a name="ers"></a><span data-ttu-id="f8d96-171">ÄNDARE</span><span class="sxs-lookup"><span data-stu-id="f8d96-171">ERS</span></span>
* <span data-ttu-id="f8d96-172">Konfiguration för klientdel</span><span class="sxs-lookup"><span data-stu-id="f8d96-172">Frontend configuration</span></span>
  * <span data-ttu-id="f8d96-173">IP-adress 10.0.0.11</span><span class="sxs-lookup"><span data-stu-id="f8d96-173">IP address 10.0.0.11</span></span>
* <span data-ttu-id="f8d96-174">Backend-konfiguration</span><span class="sxs-lookup"><span data-stu-id="f8d96-174">Backend configuration</span></span>
  * <span data-ttu-id="f8d96-175">Ansluten till primära nätverksgränssnitt för alla virtuella datorer som ska ingå i (A) SCS/ÄNDARE kluster</span><span class="sxs-lookup"><span data-stu-id="f8d96-175">Connected to primary network interfaces of all virtual machines that should be part of the (A)SCS/ERS cluster</span></span>
* <span data-ttu-id="f8d96-176">Avsökningsport</span><span class="sxs-lookup"><span data-stu-id="f8d96-176">Probe Port</span></span>
  * <span data-ttu-id="f8d96-177">Port 621**&lt;nr&gt;**</span><span class="sxs-lookup"><span data-stu-id="f8d96-177">Port 621**&lt;nr&gt;**</span></span>
* <span data-ttu-id="f8d96-178">Regler för belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="f8d96-178">Loadbalancing rules</span></span>
  * <span data-ttu-id="f8d96-179">33**&lt;nr&gt;**  TCP</span><span class="sxs-lookup"><span data-stu-id="f8d96-179">33**&lt;nr&gt;** TCP</span></span>
  * <span data-ttu-id="f8d96-180">5**&lt;nr&gt;**13 TCP</span><span class="sxs-lookup"><span data-stu-id="f8d96-180">5**&lt;nr&gt;**13 TCP</span></span>
  * <span data-ttu-id="f8d96-181">5**&lt;nr&gt;**14 TCP</span><span class="sxs-lookup"><span data-stu-id="f8d96-181">5**&lt;nr&gt;**14 TCP</span></span>
  * <span data-ttu-id="f8d96-182">5**&lt;nr&gt;**16 TCP</span><span class="sxs-lookup"><span data-stu-id="f8d96-182">5**&lt;nr&gt;**16 TCP</span></span>

### <a name="sap-hana"></a><span data-ttu-id="f8d96-183">SAP HANA</span><span class="sxs-lookup"><span data-stu-id="f8d96-183">SAP HANA</span></span>
* <span data-ttu-id="f8d96-184">Konfiguration för klientdel</span><span class="sxs-lookup"><span data-stu-id="f8d96-184">Frontend configuration</span></span>
  * <span data-ttu-id="f8d96-185">IP-adressen 10.0.0.12</span><span class="sxs-lookup"><span data-stu-id="f8d96-185">IP address 10.0.0.12</span></span>
* <span data-ttu-id="f8d96-186">Backend-konfiguration</span><span class="sxs-lookup"><span data-stu-id="f8d96-186">Backend configuration</span></span>
  * <span data-ttu-id="f8d96-187">Ansluten till primära nätverksgränssnitt för alla virtuella datorer som ska ingå i klustret HANA</span><span class="sxs-lookup"><span data-stu-id="f8d96-187">Connected to primary network interfaces of all virtual machines that should be part of the HANA cluster</span></span>
* <span data-ttu-id="f8d96-188">Avsökningsport</span><span class="sxs-lookup"><span data-stu-id="f8d96-188">Probe Port</span></span>
  * <span data-ttu-id="f8d96-189">Port 625**&lt;nr&gt;**</span><span class="sxs-lookup"><span data-stu-id="f8d96-189">Port 625**&lt;nr&gt;**</span></span>
* <span data-ttu-id="f8d96-190">Regler för belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="f8d96-190">Loadbalancing rules</span></span>
  * <span data-ttu-id="f8d96-191">3**&lt;nr&gt;**15 TCP</span><span class="sxs-lookup"><span data-stu-id="f8d96-191">3**&lt;nr&gt;**15 TCP</span></span>
  * <span data-ttu-id="f8d96-192">3**&lt;nr&gt;**17 TCP</span><span class="sxs-lookup"><span data-stu-id="f8d96-192">3**&lt;nr&gt;**17 TCP</span></span>

## <a name="setting-up-a-highly-available-nfs-server"></a><span data-ttu-id="f8d96-193">Hur du konfigurerar en NFS-server med hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="f8d96-193">Setting up a highly available NFS server</span></span>

### <a name="deploying-linux"></a><span data-ttu-id="f8d96-194">Distribuera Linux</span><span class="sxs-lookup"><span data-stu-id="f8d96-194">Deploying Linux</span></span>

<span data-ttu-id="f8d96-195">Azure Marketplace innehåller en bild för SUSE Linux Enterprise Server för SAP program 12 som du kan använda för att distribuera nya virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="f8d96-195">The Azure Marketplace contains an image for SUSE Linux Enterprise Server for SAP Applications 12 that you can use to deploy new virtual machines.</span></span>
<span data-ttu-id="f8d96-196">Du kan använda en av Snabbstart mallar på github för att distribuera alla nödvändiga resurser.</span><span class="sxs-lookup"><span data-stu-id="f8d96-196">You can use one of the quick start templates on github to deploy all required resources.</span></span> <span data-ttu-id="f8d96-197">Mallen distribuerar virtuella datorer, belastningsutjämnare, tillgänglighetsuppsättning osv. Följ dessa steg om du vill distribuera mallen:</span><span class="sxs-lookup"><span data-stu-id="f8d96-197">The template deploys the virtual machines, the load balancer, availability set etc. Follow these steps to deploy the template:</span></span>

1. <span data-ttu-id="f8d96-198">Öppna den [mall för SAP server] [ template-file-server] i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f8d96-198">Open the [SAP file server template][template-file-server] in the Azure portal</span></span>   
1. <span data-ttu-id="f8d96-199">Ange följande parametrar</span><span class="sxs-lookup"><span data-stu-id="f8d96-199">Enter the following parameters</span></span>
   1. <span data-ttu-id="f8d96-200">Resurs-Prefix</span><span class="sxs-lookup"><span data-stu-id="f8d96-200">Resource Prefix</span></span>  
      <span data-ttu-id="f8d96-201">Ange det prefix som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="f8d96-201">Enter the prefix you want to use.</span></span> <span data-ttu-id="f8d96-202">Värdet används som ett prefix för de resurser som har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="f8d96-202">The value is used as a prefix for the resources that are deployed.</span></span>
   2. <span data-ttu-id="f8d96-203">OS-typen</span><span class="sxs-lookup"><span data-stu-id="f8d96-203">Os Type</span></span>  
      <span data-ttu-id="f8d96-204">Välj en av Linux-distributioner.</span><span class="sxs-lookup"><span data-stu-id="f8d96-204">Select one of the Linux distributions.</span></span> <span data-ttu-id="f8d96-205">Det här exemplet väljer du SLES 12</span><span class="sxs-lookup"><span data-stu-id="f8d96-205">For this example, select SLES 12</span></span>
   3. <span data-ttu-id="f8d96-206">Användarnamn och lösenord för Admin administratör</span><span class="sxs-lookup"><span data-stu-id="f8d96-206">Admin Username and Admin Password</span></span>  
      <span data-ttu-id="f8d96-207">En ny användare skapas som kan användas för att logga in på datorn.</span><span class="sxs-lookup"><span data-stu-id="f8d96-207">A new user is created that can be used to log on to the machine.</span></span>
   4. <span data-ttu-id="f8d96-208">Undernät-Id</span><span class="sxs-lookup"><span data-stu-id="f8d96-208">Subnet Id</span></span>  
      <span data-ttu-id="f8d96-209">ID för det undernät som de virtuella datorerna ska anslutas till.</span><span class="sxs-lookup"><span data-stu-id="f8d96-209">The ID of the subnet to which the virtual machines should be connected to.</span></span> <span data-ttu-id="f8d96-210">Lämna tomt om du vill skapa ett nytt virtuellt nätverk eller välj undernätet i ditt VPN eller Expressroute virtuella nätverk att ansluta den virtuella datorn till ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="f8d96-210">Leave empty if you want to create a new virtual network or select the subnet of your VPN or Express Route virtual network to connect the virtual machine to your on-premises network.</span></span> <span data-ttu-id="f8d96-211">ID som ser oftast ut så /subscriptions/**&lt;prenumerations-id&gt;**/resourceGroups/**&lt;resursgruppnamn&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;virtuella nätverksnamnet&gt;**/subnets/**&lt;undernätsnamn&gt;**</span><span class="sxs-lookup"><span data-stu-id="f8d96-211">The ID usually looks like /subscriptions/**&lt;subscription id&gt;**/resourceGroups/**&lt;resource group name&gt;**/providers/Microsoft.Network/virtualNetworks/**&lt;virtual network name&gt;**/subnets/**&lt;subnet name&gt;**</span></span>

### <a name="installation"></a><span data-ttu-id="f8d96-212">Installation</span><span class="sxs-lookup"><span data-stu-id="f8d96-212">Installation</span></span>

<span data-ttu-id="f8d96-213">Följande objekt är prefixet antingen **[A]** - gäller för alla noder, **[1]** – gäller endast för nod 1 eller **[2]** – gäller endast för nod 2.</span><span class="sxs-lookup"><span data-stu-id="f8d96-213">The following items are prefixed with either **[A]** - applicable to all nodes, **[1]** - only applicable to node 1 or **[2]** - only applicable to node 2.</span></span>

1. <span data-ttu-id="f8d96-214">**[A]**  Uppdatera SLES</span><span class="sxs-lookup"><span data-stu-id="f8d96-214">**[A]** Update SLES</span></span>

   <pre><code>
   sudo zypper update
   </code></pre>

1. <span data-ttu-id="f8d96-215">**[1]**  Aktivera ssh-åtkomst</span><span class="sxs-lookup"><span data-stu-id="f8d96-215">**[1]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa
   
   # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy the public key
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

2. <span data-ttu-id="f8d96-216">**[2]**  Aktivera ssh-åtkomst</span><span class="sxs-lookup"><span data-stu-id="f8d96-216">**[2]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa

   # insert the public key you copied in the last step into the authorized keys file on the second server
   sudo vi /root/.ssh/authorized_keys
   
   # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy the public key   
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

1. <span data-ttu-id="f8d96-217">**[1]**  Aktivera ssh-åtkomst</span><span class="sxs-lookup"><span data-stu-id="f8d96-217">**[1]** Enable ssh access</span></span>

   <pre><code>
   # insert the public key you copied in the last step into the authorized keys file on the first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. <span data-ttu-id="f8d96-218">**[A]**  Installera HA filnamnstillägget</span><span class="sxs-lookup"><span data-stu-id="f8d96-218">**[A]** Install HA extension</span></span>
   
   <pre><code>
   sudo zypper install sle-ha-release fence-agents
   </code></pre>

1. <span data-ttu-id="f8d96-219">**[A]**  Installationsprogrammet värdnamnsmatchning</span><span class="sxs-lookup"><span data-stu-id="f8d96-219">**[A]** Setup host name resolution</span></span>   

   <span data-ttu-id="f8d96-220">Du kan använda en DNS-server, eller så kan du ändra de/etc/hosts på alla noder.</span><span class="sxs-lookup"><span data-stu-id="f8d96-220">You can either use a DNS server or modify the /etc/hosts on all nodes.</span></span> <span data-ttu-id="f8d96-221">Det här exemplet visar hur du använder/etc/hosts-filen.</span><span class="sxs-lookup"><span data-stu-id="f8d96-221">This example shows how to use the /etc/hosts file.</span></span>
   <span data-ttu-id="f8d96-222">Ersätt IP-adressen och värdnamnet i följande kommandon</span><span class="sxs-lookup"><span data-stu-id="f8d96-222">Replace the IP address and the hostname in the following commands</span></span>

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   <span data-ttu-id="f8d96-223">Infoga följande rader till/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="f8d96-223">Insert the following lines to /etc/hosts.</span></span> <span data-ttu-id="f8d96-224">Ändra IP-adressen och värdnamnet som matchar din miljö</span><span class="sxs-lookup"><span data-stu-id="f8d96-224">Change the IP address and hostname to match your environment</span></span>   
   
   <pre><code>
   # IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   </code></pre>

1. <span data-ttu-id="f8d96-225">**[1]**  Installera kluster</span><span class="sxs-lookup"><span data-stu-id="f8d96-225">**[1]** Install Cluster</span></span>
   
   <pre><code>
   sudo ha-cluster-init
   
   # Do you want to continue anyway? [y/N] -> y
   # Network address to bind to (for example: 192.168.1.0) [10.79.227.0] -> ENTER
   # Multicast address (for example: 239.x.x.x) [239.174.218.125] -> ENTER
   # Multicast port [5405] -> ENTER
   # Do you wish to use SBD? [y/N] -> N
   # Do you wish to configure an administration IP? [y/N] -> N
   </code></pre>

1. <span data-ttu-id="f8d96-226">**[2]**  Lägg till nod i klustret</span><span class="sxs-lookup"><span data-stu-id="f8d96-226">**[2]** Add node to cluster</span></span>
   
   <pre><code> 
   sudo ha-cluster-join

   # WARNING: NTP is not configured to start at system boot.
   # WARNING: No watchdog device found. If SBD is used, the cluster will be unable to start without a watchdog.
   # Do you want to continue anyway? [y/N] -> y
   # IP address or hostname of existing node (for example: 192.168.1.1) [] -> IP address of node 1 for example 10.0.0.10
   # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
   </code></pre>

1. <span data-ttu-id="f8d96-227">**[A]**  Ändra hacluster lösenord till samma lösenord</span><span class="sxs-lookup"><span data-stu-id="f8d96-227">**[A]** Change hacluster password to the same password</span></span>

   <pre><code> 
   sudo passwd hacluster
   </code></pre>

1. <span data-ttu-id="f8d96-228">**[A]**  Konfigurera corosync om du vill använda andra transport och lägga till nodelist.</span><span class="sxs-lookup"><span data-stu-id="f8d96-228">**[A]** Configure corosync to use other transport and add nodelist.</span></span> <span data-ttu-id="f8d96-229">Klustret fungerar inte på annat sätt.</span><span class="sxs-lookup"><span data-stu-id="f8d96-229">Cluster will not work otherwise.</span></span>
   
   <pre><code> 
   sudo vi /etc/corosync/corosync.conf   
   </code></pre>

   <span data-ttu-id="f8d96-230">Lägg till följande fetstil innehåll i filen.</span><span class="sxs-lookup"><span data-stu-id="f8d96-230">Add the following bold content to the file.</span></span>
   
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

   <span data-ttu-id="f8d96-231">Starta om tjänsten corosync</span><span class="sxs-lookup"><span data-stu-id="f8d96-231">Then restart the corosync service</span></span>

   <pre><code>
   sudo service corosync restart
   </code></pre>

1. <span data-ttu-id="f8d96-232">**[A]**  Installera drbd komponenter</span><span class="sxs-lookup"><span data-stu-id="f8d96-232">**[A]** Install drbd components</span></span>

   <pre><code>
   sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. <span data-ttu-id="f8d96-233">**[A]**  Skapa en partition för drbd-enhet</span><span class="sxs-lookup"><span data-stu-id="f8d96-233">**[A]** Create a partition for the drbd device</span></span>

   <pre><code>
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdc'
   </code></pre>

1. <span data-ttu-id="f8d96-234">**[A]**  Skapa LVM konfigurationer</span><span class="sxs-lookup"><span data-stu-id="f8d96-234">**[A]** Create LVM configurations</span></span>

   <pre><code>
   sudo pvcreate /dev/sdc1   
   sudo vgcreate vg_NFS /dev/sdc1
   sudo lvcreate -l 100%FREE -n <b>NWS</b> vg_NFS
   </code></pre>

1. <span data-ttu-id="f8d96-235">**[A]**  Skapa NFS drbd enhet</span><span class="sxs-lookup"><span data-stu-id="f8d96-235">**[A]** Create the NFS drbd device</span></span>

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_nfs.res
   </code></pre>

   <span data-ttu-id="f8d96-236">Infoga konfigurationen för den nya drbd enheten och avsluta</span><span class="sxs-lookup"><span data-stu-id="f8d96-236">Insert the configuration for the new drbd device and exit</span></span>

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

   <span data-ttu-id="f8d96-237">Skapa drbd enheten och starta den</span><span class="sxs-lookup"><span data-stu-id="f8d96-237">Create the drbd device and start it</span></span>

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_nfs
   sudo drbdadm up <b>NWS</b>_nfs
   </code></pre>

1. <span data-ttu-id="f8d96-238">**[1]**  Hoppa över inledande synkronisering</span><span class="sxs-lookup"><span data-stu-id="f8d96-238">**[1]** Skip initial synchronization</span></span>

   <pre><code>
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_nfs
   </code></pre>

1. <span data-ttu-id="f8d96-239">**[1]**  Anger den primära noden</span><span class="sxs-lookup"><span data-stu-id="f8d96-239">**[1]** Set the primary node</span></span>

   <pre><code>
   sudo drbdadm primary --force <b>NWS</b>_nfs
   </code></pre>

1. <span data-ttu-id="f8d96-240">**[1]**  Vänta tills nya drbd enheter som ska synkroniseras</span><span class="sxs-lookup"><span data-stu-id="f8d96-240">**[1]** Wait until the new drbd devices are synchronized</span></span>

   <pre><code>
   sudo cat /proc/drbd

   # version: 8.4.6 (api:1/proto:86-101)
   # GIT-hash: 833d830e0152d1e457fa7856e71e11248ccf3f70 build by abuild@sheep14, 2016-05-09 23:14:56
   # 0: cs:Connected ro:Primary/Secondary ds:UpToDate/UpToDate C r-----
   #    ns:0 nr:0 dw:0 dr:912 al:8 bm:0 lo:0 pe:0 ua:0 ap:0 ep:1 wo:f oos:0
   </code></pre>

1. <span data-ttu-id="f8d96-241">**[1]**  Skapa filsystem på drbd-enheter</span><span class="sxs-lookup"><span data-stu-id="f8d96-241">**[1]** Create file systems on the drbd devices</span></span>

   <pre><code>
   sudo mkfs.xfs /dev/drbd0
   </code></pre>


### <a name="configure-cluster-framework"></a><span data-ttu-id="f8d96-242">Konfigurera klustret Framework</span><span class="sxs-lookup"><span data-stu-id="f8d96-242">Configure Cluster Framework</span></span>

1. <span data-ttu-id="f8d96-243">**[1]**  Ändra standardinställningarna</span><span class="sxs-lookup"><span data-stu-id="f8d96-243">**[1]** Change the default settings</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# rsc_defaults resource-stickiness="1"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. <span data-ttu-id="f8d96-244">**[1]**  Lägger till NFS drbd enhet klusterkonfigurationen</span><span class="sxs-lookup"><span data-stu-id="f8d96-244">**[1]** Add the NFS drbd device to the cluster configuration</span></span>

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

1. <span data-ttu-id="f8d96-245">**[1]**  Skapa NFS-servern</span><span class="sxs-lookup"><span data-stu-id="f8d96-245">**[1]** Create the NFS server</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# primitive nfsserver \
     systemd:nfs-server \
     op monitor interval="30s"

   crm(live)configure# clone cl-nfsserver nfsserver interleave="true"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

1. <span data-ttu-id="f8d96-246">**[1]**  Skapa filsystem för NFS-resurser</span><span class="sxs-lookup"><span data-stu-id="f8d96-246">**[1]** Create the NFS File System resources</span></span>

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

1. <span data-ttu-id="f8d96-247">**[1]**  Skapa NFS exporterar</span><span class="sxs-lookup"><span data-stu-id="f8d96-247">**[1]** Create the NFS exports</span></span>

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

1. <span data-ttu-id="f8d96-248">**[1]**  Skapa en virtuell IP-resurs och hälsoavsökningen för den interna belastningsutjämnaren</span><span class="sxs-lookup"><span data-stu-id="f8d96-248">**[1]** Create a virtual IP resource and health-probe for the internal load balancer</span></span>

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

### <a name="create-stonith-device"></a><span data-ttu-id="f8d96-249">Skapa STONITH enhet</span><span class="sxs-lookup"><span data-stu-id="f8d96-249">Create STONITH device</span></span>

<span data-ttu-id="f8d96-250">STONITH enheten använder ett huvudnamn för tjänsten för att godkänna mot Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f8d96-250">The STONITH device uses a Service Principal to authorize against Microsoft Azure.</span></span> <span data-ttu-id="f8d96-251">Följ dessa steg om du vill skapa ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f8d96-251">Follow these steps to create a Service Principal.</span></span>

1. <span data-ttu-id="f8d96-252">Gå till <https://portal.azure.com></span><span class="sxs-lookup"><span data-stu-id="f8d96-252">Go to <https://portal.azure.com></span></span>
1. <span data-ttu-id="f8d96-253">Öppna bladet Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f8d96-253">Open the Azure Active Directory blade</span></span>  
   <span data-ttu-id="f8d96-254">Gå till egenskaperna och Skriv ned Directory-Id.</span><span class="sxs-lookup"><span data-stu-id="f8d96-254">Go to Properties and write down the Directory Id.</span></span> <span data-ttu-id="f8d96-255">Det här är den **klient-id**.</span><span class="sxs-lookup"><span data-stu-id="f8d96-255">This is the **tenant id**.</span></span>
1. <span data-ttu-id="f8d96-256">Klicka på appen registreringar</span><span class="sxs-lookup"><span data-stu-id="f8d96-256">Click App registrations</span></span>
1. <span data-ttu-id="f8d96-257">Klicka på Lägg till</span><span class="sxs-lookup"><span data-stu-id="f8d96-257">Click Add</span></span>
1. <span data-ttu-id="f8d96-258">Ange ett namn, Välj typ av program ”Web app/API”, ange en inloggnings-URL (till exempel http://localhost) och klicka på Skapa</span><span class="sxs-lookup"><span data-stu-id="f8d96-258">Enter a Name, select Application Type "Web app/API", enter a sign-on URL (for example http://localhost) and click Create</span></span>
1. <span data-ttu-id="f8d96-259">URL för inloggning används inte och kan vara en giltig URL</span><span class="sxs-lookup"><span data-stu-id="f8d96-259">The sign-on URL is not used and can be any valid URL</span></span>
1. <span data-ttu-id="f8d96-260">Välj den nya appen och klicka på nycklar på fliken Inställningar</span><span class="sxs-lookup"><span data-stu-id="f8d96-260">Select the new App and click Keys in the Settings tab</span></span>
1. <span data-ttu-id="f8d96-261">Ange en beskrivning för en ny nyckel, Välj ”upphör aldrig att gälla” och klicka på Spara</span><span class="sxs-lookup"><span data-stu-id="f8d96-261">Enter a description for a new key, select "Never expires" and click Save</span></span>
1. <span data-ttu-id="f8d96-262">Anteckna värdet.</span><span class="sxs-lookup"><span data-stu-id="f8d96-262">Write down the Value.</span></span> <span data-ttu-id="f8d96-263">Den används som den **lösenord** för tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="f8d96-263">It is used as the **password** for the Service Principal</span></span>
1. <span data-ttu-id="f8d96-264">Skriv ned det program-Id.</span><span class="sxs-lookup"><span data-stu-id="f8d96-264">Write down the Application Id.</span></span> <span data-ttu-id="f8d96-265">Den används som användarnamnet (**inloggnings-id** i stegen nedan) för tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="f8d96-265">It is used as the username (**login id** in the steps below) of the Service Principal</span></span>

<span data-ttu-id="f8d96-266">Tjänstens huvudnamn har inte behörighet att komma åt Azure-resurser som standard.</span><span class="sxs-lookup"><span data-stu-id="f8d96-266">The Service Principal does not have permissions to access your Azure resources by default.</span></span> <span data-ttu-id="f8d96-267">Du behöver ge behörighet att starta och stoppa tjänstens huvudnamn (frigöra) alla virtuella datorer i klustret.</span><span class="sxs-lookup"><span data-stu-id="f8d96-267">You need to give the Service Principal permissions to start and stop (deallocate) all virtual machines of the cluster.</span></span>

1. <span data-ttu-id="f8d96-268">Gå till https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="f8d96-268">Go to https://portal.azure.com</span></span>
1. <span data-ttu-id="f8d96-269">Öppna bladet alla resurser</span><span class="sxs-lookup"><span data-stu-id="f8d96-269">Open the All resources blade</span></span>
1. <span data-ttu-id="f8d96-270">Välj den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="f8d96-270">Select the virtual machine</span></span>
1. <span data-ttu-id="f8d96-271">Klicka på åtkomstkontroll (IAM)</span><span class="sxs-lookup"><span data-stu-id="f8d96-271">Click Access control (IAM)</span></span>
1. <span data-ttu-id="f8d96-272">Klicka på Lägg till</span><span class="sxs-lookup"><span data-stu-id="f8d96-272">Click Add</span></span>
1. <span data-ttu-id="f8d96-273">Välj rollen ägare</span><span class="sxs-lookup"><span data-stu-id="f8d96-273">Select the role Owner</span></span>
1. <span data-ttu-id="f8d96-274">Ange namnet på programmet som du skapade ovan</span><span class="sxs-lookup"><span data-stu-id="f8d96-274">Enter the name of the application you created above</span></span>
1. <span data-ttu-id="f8d96-275">Klicka på OK</span><span class="sxs-lookup"><span data-stu-id="f8d96-275">Click OK</span></span>

#### <a name="1-create-the-stonith-devices"></a><span data-ttu-id="f8d96-276">**[1]**  Skapa STONITH-enheter</span><span class="sxs-lookup"><span data-stu-id="f8d96-276">**[1]** Create the STONITH devices</span></span>

<span data-ttu-id="f8d96-277">När du har redigerat behörigheter för de virtuella datorerna kan du konfigurera STONITH-enheter i klustret.</span><span class="sxs-lookup"><span data-stu-id="f8d96-277">After you edited the permissions for the virtual machines, you can configure the STONITH devices in the cluster.</span></span>

<pre><code>
sudo crm configure

# replace the bold string with your subscription id, resource group, tenant id, service principal id and password

crm(live)configure# primitive rsc_st_azure_1 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# primitive rsc_st_azure_2 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started

crm(live)configure# commit
crm(live)configure# exit
</code></pre>

#### <a name="1-enable-the-use-of-a-stonith-device"></a><span data-ttu-id="f8d96-278">**[1]**  Aktivera användning av en STONITH-enhet</span><span class="sxs-lookup"><span data-stu-id="f8d96-278">**[1]** Enable the use of a STONITH device</span></span>

<pre><code>
sudo crm configure property stonith-enabled=true 
</code></pre>

## <a name="setting-up-ascs"></a><span data-ttu-id="f8d96-279">Skapa (A) SCS</span><span class="sxs-lookup"><span data-stu-id="f8d96-279">Setting up (A)SCS</span></span>

### <a name="deploying-linux"></a><span data-ttu-id="f8d96-280">Distribuera Linux</span><span class="sxs-lookup"><span data-stu-id="f8d96-280">Deploying Linux</span></span>

<span data-ttu-id="f8d96-281">Azure Marketplace innehåller en bild för SUSE Linux Enterprise Server för SAP program 12 som du kan använda för att distribuera nya virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="f8d96-281">The Azure Marketplace contains an image for SUSE Linux Enterprise Server for SAP Applications 12 that you can use to deploy new virtual machines.</span></span> <span data-ttu-id="f8d96-282">Marketplace-avbildning innehåller resurs-agent för SAP NetWeaver.</span><span class="sxs-lookup"><span data-stu-id="f8d96-282">The marketplace image contains the resource agent for SAP NetWeaver.</span></span>

<span data-ttu-id="f8d96-283">Du kan använda en av Snabbstart mallar på github för att distribuera alla nödvändiga resurser.</span><span class="sxs-lookup"><span data-stu-id="f8d96-283">You can use one of the quick start templates on github to deploy all required resources.</span></span> <span data-ttu-id="f8d96-284">Mallen distribuerar virtuella datorer, belastningsutjämnare, tillgänglighetsuppsättning osv. Följ dessa steg om du vill distribuera mallen:</span><span class="sxs-lookup"><span data-stu-id="f8d96-284">The template deploys the virtual machines, the load balancer, availability set etc. Follow these steps to deploy the template:</span></span>

1. <span data-ttu-id="f8d96-285">Öppna den [ASCS/SCS Multi SID-mall] [ template-multisid-xscs] eller [konvergerat mallen] [ template-converged] på Azure-portalen i ASCS/SCS endast skapas på belastningsutjämning regler för SAP NetWeaver ASCS/SCS och ÄNDARE (endast Linux) instanser medan mallen konvergerade skapar även reglerna för belastningsutjämning för en databas (till exempel Microsoft SQL Server eller SAP HANA).</span><span class="sxs-lookup"><span data-stu-id="f8d96-285">Open the [ASCS/SCS Multi SID template][template-multisid-xscs] or the [converged template][template-converged] on the Azure portal The ASCS/SCS template only creates the load-balancing rules for the SAP NetWeaver ASCS/SCS and ERS (Linux only) instances whereas the converged template also creates the load-balancing rules for a database (for example Microsoft SQL Server or SAP HANA).</span></span> <span data-ttu-id="f8d96-286">Om du planerar att installera ett SAP NetWeaver baserat system och du även vill installera databasen på samma datorer använder den [konvergerat mallen][template-converged].</span><span class="sxs-lookup"><span data-stu-id="f8d96-286">If you plan to install an SAP NetWeaver based system and you also want to install the database on the same machines, use the [converged template][template-converged].</span></span>
1. <span data-ttu-id="f8d96-287">Ange följande parametrar</span><span class="sxs-lookup"><span data-stu-id="f8d96-287">Enter the following parameters</span></span>
   1. <span data-ttu-id="f8d96-288">Resursen Prefix (endast mall ASCS/SCS Multi SID)</span><span class="sxs-lookup"><span data-stu-id="f8d96-288">Resource Prefix (ASCS/SCS Multi SID template only)</span></span>  
      <span data-ttu-id="f8d96-289">Ange det prefix som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="f8d96-289">Enter the prefix you want to use.</span></span> <span data-ttu-id="f8d96-290">Värdet används som ett prefix för de resurser som har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="f8d96-290">The value is used as a prefix for the resources that are deployed.</span></span>
   3. <span data-ttu-id="f8d96-291">SAP System-Id (endast konvergerade mall)</span><span class="sxs-lookup"><span data-stu-id="f8d96-291">Sap System Id (converged template only)</span></span>  
      <span data-ttu-id="f8d96-292">Ange SAP-systemet Id för SAP-system som du vill installera.</span><span class="sxs-lookup"><span data-stu-id="f8d96-292">Enter the SAP system Id of the SAP system you want to install.</span></span> <span data-ttu-id="f8d96-293">Id som används som ett prefix för de resurser som har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="f8d96-293">The Id is used as a prefix for the resources that are deployed.</span></span>
   4. <span data-ttu-id="f8d96-294">Stack-typ</span><span class="sxs-lookup"><span data-stu-id="f8d96-294">Stack Type</span></span>  
      <span data-ttu-id="f8d96-295">Välj typ för SAP NetWeaver stack</span><span class="sxs-lookup"><span data-stu-id="f8d96-295">Select the SAP NetWeaver stack type</span></span>
   5. <span data-ttu-id="f8d96-296">OS-typen</span><span class="sxs-lookup"><span data-stu-id="f8d96-296">Os Type</span></span>  
      <span data-ttu-id="f8d96-297">Välj en av Linux-distributioner.</span><span class="sxs-lookup"><span data-stu-id="f8d96-297">Select one of the Linux distributions.</span></span> <span data-ttu-id="f8d96-298">Det här exemplet väljer du SLES 12 BYOS</span><span class="sxs-lookup"><span data-stu-id="f8d96-298">For this example, select SLES 12 BYOS</span></span>
   6. <span data-ttu-id="f8d96-299">DB-typ</span><span class="sxs-lookup"><span data-stu-id="f8d96-299">Db Type</span></span>  
      <span data-ttu-id="f8d96-300">Välj HANA</span><span class="sxs-lookup"><span data-stu-id="f8d96-300">Select HANA</span></span>
   7. <span data-ttu-id="f8d96-301">Storlek för SAP-System</span><span class="sxs-lookup"><span data-stu-id="f8d96-301">Sap System Size</span></span>  
      <span data-ttu-id="f8d96-302">Mängden SAP som innehåller det nya systemet.</span><span class="sxs-lookup"><span data-stu-id="f8d96-302">The amount of SAPS the new system provides.</span></span> <span data-ttu-id="f8d96-303">Om du inte vet hur många SAP systemet kräver, be din SAP-teknikpartner eller systemintegreraren</span><span class="sxs-lookup"><span data-stu-id="f8d96-303">If you are not sure how many SAPS the system requires, please ask your SAP Technology Partner or System Integrator</span></span>
   8. <span data-ttu-id="f8d96-304">Systemets tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="f8d96-304">System Availability</span></span>  
      <span data-ttu-id="f8d96-305">Välj hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="f8d96-305">Select HA</span></span>
   9. <span data-ttu-id="f8d96-306">Användarnamn och lösenord för Admin administratör</span><span class="sxs-lookup"><span data-stu-id="f8d96-306">Admin Username and Admin Password</span></span>  
      <span data-ttu-id="f8d96-307">En ny användare skapas som kan användas för att logga in på datorn.</span><span class="sxs-lookup"><span data-stu-id="f8d96-307">A new user is created that can be used to log on to the machine.</span></span>
   10. <span data-ttu-id="f8d96-308">Undernät-Id</span><span class="sxs-lookup"><span data-stu-id="f8d96-308">Subnet Id</span></span>  
   <span data-ttu-id="f8d96-309">ID för det undernät som de virtuella datorerna ska anslutas till.</span><span class="sxs-lookup"><span data-stu-id="f8d96-309">The ID of the subnet to which the virtual machines should be connected to.</span></span>  <span data-ttu-id="f8d96-310">Lämna tomt om du vill skapa ett nytt virtuellt nätverk eller Välj samma undernät som du användas eller skapas som en del av distributionen av NFS-servern.</span><span class="sxs-lookup"><span data-stu-id="f8d96-310">Leave empty if you want to create a new virtual network or select the same subnet that you used or created as part of the NFS server deployment.</span></span> <span data-ttu-id="f8d96-311">ID som ser oftast ut så /subscriptions/**&lt;prenumerations-id&gt;**/resourceGroups/**&lt;resursgruppnamn&gt;**/providers/ Microsoft.Network/virtualNetworks/**&lt;virtuella nätverksnamnet&gt;**/subnets/**&lt;undernätsnamn&gt;**</span><span class="sxs-lookup"><span data-stu-id="f8d96-311">The ID usually looks like /subscriptions/**&lt;subscription id&gt;**/resourceGroups/**&lt;resource group name&gt;**/providers/Microsoft.Network/virtualNetworks/**&lt;virtual network name&gt;**/subnets/**&lt;subnet name&gt;**</span></span>

### <a name="installation"></a><span data-ttu-id="f8d96-312">Installation</span><span class="sxs-lookup"><span data-stu-id="f8d96-312">Installation</span></span>

<span data-ttu-id="f8d96-313">Följande objekt är prefixet antingen **[A]** - gäller för alla noder, **[1]** – gäller endast för nod 1 eller **[2]** – gäller endast för nod 2.</span><span class="sxs-lookup"><span data-stu-id="f8d96-313">The following items are prefixed with either **[A]** - applicable to all nodes, **[1]** - only applicable to node 1 or **[2]** - only applicable to node 2.</span></span>

1. <span data-ttu-id="f8d96-314">**[A]**  Uppdatera SLES</span><span class="sxs-lookup"><span data-stu-id="f8d96-314">**[A]** Update SLES</span></span>

   <pre><code>
   sudo zypper update
   </code></pre>

1. <span data-ttu-id="f8d96-315">**[1]**  Aktivera ssh-åtkomst</span><span class="sxs-lookup"><span data-stu-id="f8d96-315">**[1]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa
   
   # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy the public key
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

2. <span data-ttu-id="f8d96-316">**[2]**  Aktivera ssh-åtkomst</span><span class="sxs-lookup"><span data-stu-id="f8d96-316">**[2]** Enable ssh access</span></span>

   <pre><code>
   sudo ssh-keygen -tdsa

   # insert the public key you copied in the last step into the authorized keys file on the second server
   sudo vi /root/.ssh/authorized_keys
   
   # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
   # Enter passphrase (empty for no passphrase): -> ENTER
   # Enter same passphrase again: -> ENTER
   
   # copy the public key   
   sudo cat /root/.ssh/id_dsa.pub
   </code></pre>

1. <span data-ttu-id="f8d96-317">**[1]**  Aktivera ssh-åtkomst</span><span class="sxs-lookup"><span data-stu-id="f8d96-317">**[1]** Enable ssh access</span></span>

   <pre><code>
   # insert the public key you copied in the last step into the authorized keys file on the first server
   sudo vi /root/.ssh/authorized_keys
   </code></pre>

1. <span data-ttu-id="f8d96-318">**[A]**  Installera HA filnamnstillägget</span><span class="sxs-lookup"><span data-stu-id="f8d96-318">**[A]** Install HA extension</span></span>
   
   <pre><code>
   sudo zypper install sle-ha-release fence-agents
   </code></pre>

1. <span data-ttu-id="f8d96-319">**[A]**  Uppdatering SAP resurs agenter</span><span class="sxs-lookup"><span data-stu-id="f8d96-319">**[A]** Update SAP resource agents</span></span>  
   
   <span data-ttu-id="f8d96-320">En korrigering för resurs-agenter paket krävs för att använda den nya konfigurationen som beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f8d96-320">A patch for the resource-agents package is required to use the new configuration, that is described in this article.</span></span> <span data-ttu-id="f8d96-321">Du kan kontrollera, om uppdateringen redan är installerad med följande kommando</span><span class="sxs-lookup"><span data-stu-id="f8d96-321">You can check, if the patch is already installed with the following command</span></span>

   <pre><code>
   sudo grep 'parameter name="IS_ERS"' /usr/lib/ocf/resource.d/heartbeat/SAPInstance
   </code></pre>

   <span data-ttu-id="f8d96-322">Resultatet bör likna</span><span class="sxs-lookup"><span data-stu-id="f8d96-322">The output should be similar to</span></span>

   <pre><code>
   &lt;parameter name="IS_ERS" unique="0" required="0"&gt;
   </code></pre>

   <span data-ttu-id="f8d96-323">Om kommandot grep inte hittar parametern IS_ERS, måste du installera uppdateringen på [SUSE hämtningssidan](https://download.suse.com/patch/finder/#bu=suse&familyId=&productId=&dateRange=&startDate=&endDate=&priority=&architecture=&keywords=resource-agents)</span><span class="sxs-lookup"><span data-stu-id="f8d96-323">If the grep command does not find the IS_ERS parameter, you need to install the patch listed on [the SUSE download page](https://download.suse.com/patch/finder/#bu=suse&familyId=&productId=&dateRange=&startDate=&endDate=&priority=&architecture=&keywords=resource-agents)</span></span>

   <pre><code>
   # example for patch for SLES 12 SP1
   sudo zypper in -t patch SUSE-SLE-HA-12-SP1-2017-885=1
   # example for patch for SLES 12 SP2
   sudo zypper in -t patch SUSE-SLE-HA-12-SP2-2017-886=1
   </code></pre>

1. <span data-ttu-id="f8d96-324">**[A]**  Installationsprogrammet värdnamnsmatchning</span><span class="sxs-lookup"><span data-stu-id="f8d96-324">**[A]** Setup host name resolution</span></span>   

   <span data-ttu-id="f8d96-325">Du kan använda en DNS-server, eller så kan du ändra de/etc/hosts på alla noder.</span><span class="sxs-lookup"><span data-stu-id="f8d96-325">You can either use a DNS server or modify the /etc/hosts on all nodes.</span></span> <span data-ttu-id="f8d96-326">Det här exemplet visar hur du använder/etc/hosts-filen.</span><span class="sxs-lookup"><span data-stu-id="f8d96-326">This example shows how to use the /etc/hosts file.</span></span>
   <span data-ttu-id="f8d96-327">Ersätt IP-adressen och värdnamnet i följande kommandon</span><span class="sxs-lookup"><span data-stu-id="f8d96-327">Replace the IP address and the hostname in the following commands</span></span>

   <pre><code>
   sudo vi /etc/hosts
   </code></pre>
   
   <span data-ttu-id="f8d96-328">Infoga följande rader till/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="f8d96-328">Insert the following lines to /etc/hosts.</span></span> <span data-ttu-id="f8d96-329">Ändra IP-adressen och värdnamnet som matchar din miljö</span><span class="sxs-lookup"><span data-stu-id="f8d96-329">Change the IP address and hostname to match your environment</span></span>   
   
   <pre><code>
   # IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.0.0.10 nws-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.0.0.11 nws-ers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.12 nws-db</b>
   </code></pre>

1. <span data-ttu-id="f8d96-330">**[1]**  Installera kluster</span><span class="sxs-lookup"><span data-stu-id="f8d96-330">**[1]** Install Cluster</span></span>
   
   <pre><code>
   sudo ha-cluster-init
   
   # Do you want to continue anyway? [y/N] -> y
   # Network address to bind to (for example: 192.168.1.0) [10.79.227.0] -> ENTER
   # Multicast address (for example: 239.x.x.x) [239.174.218.125] -> ENTER
   # Multicast port [5405] -> ENTER
   # Do you wish to use SBD? [y/N] -> N
   # Do you wish to configure an administration IP? [y/N] -> N
   </code></pre>

1. <span data-ttu-id="f8d96-331">**[2]**  Lägg till nod i klustret</span><span class="sxs-lookup"><span data-stu-id="f8d96-331">**[2]** Add node to cluster</span></span>
   
   <pre><code> 
   sudo ha-cluster-join

   # WARNING: NTP is not configured to start at system boot.
   # WARNING: No watchdog device found. If SBD is used, the cluster will be unable to start without a watchdog.
   # Do you want to continue anyway? [y/N] -> y
   # IP address or hostname of existing node (for example: 192.168.1.1) [] -> IP address of node 1 for example 10.0.0.10
   # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
   </code></pre>

1. <span data-ttu-id="f8d96-332">**[A]**  Ändra hacluster lösenord till samma lösenord</span><span class="sxs-lookup"><span data-stu-id="f8d96-332">**[A]** Change hacluster password to the same password</span></span>

   <pre><code> 
   sudo passwd hacluster
   </code></pre>

1. <span data-ttu-id="f8d96-333">**[A]**  Konfigurera corosync om du vill använda andra transport och lägga till nodelist.</span><span class="sxs-lookup"><span data-stu-id="f8d96-333">**[A]** Configure corosync to use other transport and add nodelist.</span></span> <span data-ttu-id="f8d96-334">Klustret fungerar inte på annat sätt.</span><span class="sxs-lookup"><span data-stu-id="f8d96-334">Cluster will not work otherwise.</span></span>
   
   <pre><code> 
   sudo vi /etc/corosync/corosync.conf   
   </code></pre>

   <span data-ttu-id="f8d96-335">Lägg till följande fetstil innehåll i filen.</span><span class="sxs-lookup"><span data-stu-id="f8d96-335">Add the following bold content to the file.</span></span>
   
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

   <span data-ttu-id="f8d96-336">Starta om tjänsten corosync</span><span class="sxs-lookup"><span data-stu-id="f8d96-336">Then restart the corosync service</span></span>

   <pre><code>
   sudo service corosync restart
   </code></pre>

1. <span data-ttu-id="f8d96-337">**[A]**  Installera drbd komponenter</span><span class="sxs-lookup"><span data-stu-id="f8d96-337">**[A]** Install drbd components</span></span>

   <pre><code>
   sudo zypper install drbd drbd-kmp-default drbd-utils
   </code></pre>

1. <span data-ttu-id="f8d96-338">**[A]**  Skapa en partition för drbd-enhet</span><span class="sxs-lookup"><span data-stu-id="f8d96-338">**[A]** Create a partition for the drbd device</span></span>

   <pre><code>
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdc'
   </code></pre>

1. <span data-ttu-id="f8d96-339">**[A]**  Skapa LVM konfigurationer</span><span class="sxs-lookup"><span data-stu-id="f8d96-339">**[A]** Create LVM configurations</span></span>

   <pre><code>
   sudo pvcreate /dev/sdc1   
   sudo vgcreate vg_<b>NWS</b> /dev/sdc1
   sudo lvcreate -l 50%FREE -n <b>NWS</b>_ASCS vg_<b>NWS</b>
   sudo lvcreate -l 50%FREE -n <b>NWS</b>_ERS vg_<b>NWS</b>
   </code></pre>

1. <span data-ttu-id="f8d96-340">**[A]**  Skapa SCS drbd enhet</span><span class="sxs-lookup"><span data-stu-id="f8d96-340">**[A]** Create the SCS drbd device</span></span>

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_ascs.res
   </code></pre>

   <span data-ttu-id="f8d96-341">Infoga konfigurationen för den nya drbd enheten och avsluta</span><span class="sxs-lookup"><span data-stu-id="f8d96-341">Insert the configuration for the new drbd device and exit</span></span>

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

   <span data-ttu-id="f8d96-342">Skapa drbd enheten och starta den</span><span class="sxs-lookup"><span data-stu-id="f8d96-342">Create the drbd device and start it</span></span>

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_ascs
   sudo drbdadm up <b>NWS</b>_ascs
   </code></pre>

1. <span data-ttu-id="f8d96-343">**[A]**  Skapa ÄNDARE drbd enhet</span><span class="sxs-lookup"><span data-stu-id="f8d96-343">**[A]** Create the ERS drbd device</span></span>

   <pre><code>
   sudo vi /etc/drbd.d/<b>NWS</b>_ers.res
   </code></pre>

   <span data-ttu-id="f8d96-344">Infoga konfigurationen för den nya drbd enheten och avsluta</span><span class="sxs-lookup"><span data-stu-id="f8d96-344">Insert the configuration for the new drbd device and exit</span></span>

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

   <span data-ttu-id="f8d96-345">Skapa drbd enheten och starta den</span><span class="sxs-lookup"><span data-stu-id="f8d96-345">Create the drbd device and start it</span></span>

   <pre><code>
   sudo drbdadm create-md <b>NWS</b>_ers
   sudo drbdadm up <b>NWS</b>_ers
   </code></pre>

1. <span data-ttu-id="f8d96-346">**[1]**  Hoppa över inledande synkronisering</span><span class="sxs-lookup"><span data-stu-id="f8d96-346">**[1]** Skip initial synchronization</span></span>

   <pre><code>
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_ascs
   sudo drbdadm new-current-uuid --clear-bitmap <b>NWS</b>_ers
   </code></pre>

1. <span data-ttu-id="f8d96-347">**[1]**  Anger den primära noden</span><span class="sxs-lookup"><span data-stu-id="f8d96-347">**[1]** Set the primary node</span></span>

   <pre><code>
   sudo drbdadm primary --force <b>NWS</b>_ascs
   sudo drbdadm primary --force <b>NWS</b>_ers
   </code></pre>

1. <span data-ttu-id="f8d96-348">**[1]**  Vänta tills nya drbd enheter som ska synkroniseras</span><span class="sxs-lookup"><span data-stu-id="f8d96-348">**[1]** Wait until the new drbd devices are synchronized</span></span>

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

1. <span data-ttu-id="f8d96-349">**[1]**  Skapa filsystem på drbd-enheter</span><span class="sxs-lookup"><span data-stu-id="f8d96-349">**[1]** Create file systems on the drbd devices</span></span>

   <pre><code>
   sudo mkfs.xfs /dev/drbd0
   sudo mkfs.xfs /dev/drbd1
   </code></pre>


### <a name="configure-cluster-framework"></a><span data-ttu-id="f8d96-350">Konfigurera klustret Framework</span><span class="sxs-lookup"><span data-stu-id="f8d96-350">Configure Cluster Framework</span></span>

<span data-ttu-id="f8d96-351">**[1]**  Ändra standardinställningarna</span><span class="sxs-lookup"><span data-stu-id="f8d96-351">**[1]** Change the default settings</span></span>

   <pre><code>
   sudo crm configure

   crm(live)configure# rsc_defaults resource-stickiness="1"

   crm(live)configure# commit
   crm(live)configure# exit
   </code></pre>

## <a name="prepare-for-sap-netweaver-installation"></a><span data-ttu-id="f8d96-352">Förbereda för SAP NetWeaver installation</span><span class="sxs-lookup"><span data-stu-id="f8d96-352">Prepare for SAP NetWeaver installation</span></span>

1. <span data-ttu-id="f8d96-353">**[A]**  Skapa delade kataloger</span><span class="sxs-lookup"><span data-stu-id="f8d96-353">**[A]** Create the shared directories</span></span>

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NWS</b>
   sudo mkdir -p /usr/sap/trans
   sudo mkdir -p /usr/sap/<b>NWS</b>/SYS

   sudo chattr +i /sapmnt/<b>NWS</b>
   sudo chattr +i /usr/sap/trans
   sudo chattr +i /usr/sap/<b>NWS</b>/SYS
   </code></pre>

1. <span data-ttu-id="f8d96-354">**[A]**  Konfigurera autofs</span><span class="sxs-lookup"><span data-stu-id="f8d96-354">**[A]** Configure autofs</span></span>
 
   <pre><code>
   sudo vi /etc/auto.master

   # Add the following line to the file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   <span data-ttu-id="f8d96-355">Skapa en fil med</span><span class="sxs-lookup"><span data-stu-id="f8d96-355">Create a file with</span></span>

   <pre><code>
   sudo vi /etc/auto.direct

   # Add the following lines to the file, save and exit
   /sapmnt/<b>NWS</b> -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/trans
   /usr/sap/<b>NWS</b>/SYS -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sidsys
   </code></pre>

   <span data-ttu-id="f8d96-356">Starta om autofs montera nya resurser</span><span class="sxs-lookup"><span data-stu-id="f8d96-356">Restart autofs to mount the new shares</span></span>

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. <span data-ttu-id="f8d96-357">**[A]**  Konfigurera växla fil</span><span class="sxs-lookup"><span data-stu-id="f8d96-357">**[A]** Configure SWAP file</span></span>
 
   <pre><code>
   sudo vi /etc/waagent.conf

   # Set the property ResourceDisk.EnableSwap to y
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>

   # Set the size of the SWAP file with property ResourceDisk.SwapSizeMB
   # The free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check the SWAP space with command swapon
   # Size of the swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   <span data-ttu-id="f8d96-358">Starta om agenten för att aktivera ändringen</span><span class="sxs-lookup"><span data-stu-id="f8d96-358">Restart the Agent to activate the change</span></span>

   <pre><code>
   sudo service waagent restart
   </code></pre>

### <a name="installing-sap-netweaver-ascsers"></a><span data-ttu-id="f8d96-359">Installera SAP NetWeaver ASCS/ÄNDARE</span><span class="sxs-lookup"><span data-stu-id="f8d96-359">Installing SAP NetWeaver ASCS/ERS</span></span>

1. <span data-ttu-id="f8d96-360">**[1]**  Skapa en virtuell IP-resurs och hälsoavsökningen för den interna belastningsutjämnaren</span><span class="sxs-lookup"><span data-stu-id="f8d96-360">**[1]** Create a virtual IP resource and health-probe for the internal load balancer</span></span>

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

   <span data-ttu-id="f8d96-361">Kontrollera att klustret status är ok och att alla resurser har startats.</span><span class="sxs-lookup"><span data-stu-id="f8d96-361">Make sure that the cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="f8d96-362">Det är inte viktigt i vilken nod som använder resurserna.</span><span class="sxs-lookup"><span data-stu-id="f8d96-362">It is not important on which node the resources are running.</span></span>

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

1. <span data-ttu-id="f8d96-363">**[1]**  Installera SAP NetWeaver ASCS</span><span class="sxs-lookup"><span data-stu-id="f8d96-363">**[1]** Install SAP NetWeaver ASCS</span></span>  

   <span data-ttu-id="f8d96-364">Installera SAP NetWeaver ASCS som rot på den första noden med hjälp av ett virtuellt värdnamn som till exempel mappar till IP-adressen för klientdel belastningsutjämningskonfigurationen för ASCS <b>nws ascs</b>, <b>10.0.0.10</b> och instansen tal som du använde för avsökning av belastningsutjämnaren till exempel <b>00</b>.</span><span class="sxs-lookup"><span data-stu-id="f8d96-364">Install SAP NetWeaver ASCS as root on the first node using a virtual hostname that maps to the IP address of the load balancer frontend configuration for the ASCS for example <b>nws-ascs</b>, <b>10.0.0.10</b> and the instance number that you used for the probe of the load balancer for example <b>00</b>.</span></span>

   <span data-ttu-id="f8d96-365">Du kan använda parametern sapinst SAPINST_REMOTE_ACCESS_USER för att tillåta en rot-användare att ansluta till sapinst.</span><span class="sxs-lookup"><span data-stu-id="f8d96-365">You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. <span data-ttu-id="f8d96-366">**[1]**  Skapa en virtuell IP-resurs och hälsoavsökningen för den interna belastningsutjämnaren</span><span class="sxs-lookup"><span data-stu-id="f8d96-366">**[1]** Create a virtual IP resource and health-probe for the internal load balancer</span></span>

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
   # Do you still want to commit (y/n)? y

   crm(live)configure# exit
   
   </code></pre>
 
   <span data-ttu-id="f8d96-367">Kontrollera att klustret status är ok och att alla resurser har startats.</span><span class="sxs-lookup"><span data-stu-id="f8d96-367">Make sure that the cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="f8d96-368">Det är inte viktigt i vilken nod som använder resurserna.</span><span class="sxs-lookup"><span data-stu-id="f8d96-368">It is not important on which node the resources are running.</span></span>

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

1. <span data-ttu-id="f8d96-369">**[2]**  Installera SAP NetWeaver ÄNDARE</span><span class="sxs-lookup"><span data-stu-id="f8d96-369">**[2]** Install SAP NetWeaver ERS</span></span>  

   <span data-ttu-id="f8d96-370">Installera SAP NetWeaver ÄNDARE som rot på den andra noden med hjälp av ett virtuellt värdnamn som mappar till IP-adressen för klientdel belastningsutjämningskonfigurationen för ÄNDARE exempelvis <b>nws ändare</b>, <b>10.0.0.11</b> och instansen tal som du använde för avsökning av belastningsutjämnaren till exempel <b>02</b>.</span><span class="sxs-lookup"><span data-stu-id="f8d96-370">Install SAP NetWeaver ERS as root on the second node using a virtual hostname that maps to the IP address of the load balancer frontend configuration for the ERS for example <b>nws-ers</b>, <b>10.0.0.11</b> and the instance number that you used for the probe of the load balancer for example <b>02</b>.</span></span>

   <span data-ttu-id="f8d96-371">Du kan använda parametern sapinst SAPINST_REMOTE_ACCESS_USER för att tillåta en rot-användare att ansluta till sapinst.</span><span class="sxs-lookup"><span data-stu-id="f8d96-371">You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

   > [!NOTE]
   > <span data-ttu-id="f8d96-372">Använd SWPM SP 20 PL 05 eller högre.</span><span class="sxs-lookup"><span data-stu-id="f8d96-372">Please use SWPM SP 20 PL 05 or higher.</span></span> <span data-ttu-id="f8d96-373">Lägre versioner anger inte behörigheter på rätt sätt och installationen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="f8d96-373">Lower versions do not set the permissions correctly and the installation will fail.</span></span>
   > 

1. <span data-ttu-id="f8d96-374">**[1]**  Adapt ASCS/SCS och ÄNDARE instansen profiler</span><span class="sxs-lookup"><span data-stu-id="f8d96-374">**[1]** Adapt the ASCS/SCS and ERS instance profiles</span></span>
 
   * <span data-ttu-id="f8d96-375">ASCS/SCS-profil</span><span class="sxs-lookup"><span data-stu-id="f8d96-375">ASCS/SCS profile</span></span>

   <pre><code> 
   sudo vi /sapmnt/<b>NWS</b>/profile/<b>NWS</b>_<b>ASCS00</b>_<b>nws-ascs</b>

   # Change the restart command to a start command
   #Restart_Program_01 = local $(_EN) pf=$(_PF)
   Start_Program_01 = local $(_EN) pf=$(_PF)

   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector

   # Add the keep alive parameter
   enque/encni/set_so_keepalive = true
   </code></pre>

   * <span data-ttu-id="f8d96-376">ÄNDARE profil</span><span class="sxs-lookup"><span data-stu-id="f8d96-376">ERS profile</span></span>

   <pre><code> 
   sudo vi /sapmnt/<b>NWS</b>/profile/<b>NWS</b>_ERS<b>02</b>_<b>nws-ers</b>

   # Add the following lines
   service/halib = $(DIR_CT_RUN)/saphascriptco.so
   service/halib_cluster_connector = /usr/bin/sap_suse_cluster_connector
   </code></pre>


1. <span data-ttu-id="f8d96-377">**[A]**  Konfigurera Keep-Alive</span><span class="sxs-lookup"><span data-stu-id="f8d96-377">**[A]** Configure Keep Alive</span></span>

   <span data-ttu-id="f8d96-378">Kommunikationen mellan SAP NetWeaver programservern och ASCS/SCS dirigeras via en belastningsutjämnare för programvara.</span><span class="sxs-lookup"><span data-stu-id="f8d96-378">The communication between the SAP NetWeaver application server and the ASCS/SCS is routed through a software load balancer.</span></span> <span data-ttu-id="f8d96-379">Belastningsutjämnaren kopplar från inaktiva anslutningar efter en konfigurerbar tidsgräns.</span><span class="sxs-lookup"><span data-stu-id="f8d96-379">The load balancer disconnects inactive connections after a configurable timeout.</span></span> <span data-ttu-id="f8d96-380">För att förhindra detta måste du ange en parameter i profilen för SAP NetWeaver ASCS/SCS och ändra inställningar för Linux-system.</span><span class="sxs-lookup"><span data-stu-id="f8d96-380">To prevent this you need to set a parameter in the SAP NetWeaver ASCS/SCS profile and change the Linux system settings.</span></span> <span data-ttu-id="f8d96-381">Läs [SAP Obs 1410736] [ 1410736] för mer information.</span><span class="sxs-lookup"><span data-stu-id="f8d96-381">Please read [SAP Note 1410736][1410736] for more information.</span></span>
   
   <span data-ttu-id="f8d96-382">Den ASCS/SCS profil parametern placera/encni/set_so_keepalive har redan lagts till i det sista steget.</span><span class="sxs-lookup"><span data-stu-id="f8d96-382">The ASCS/SCS profile parameter enque/encni/set_so_keepalive was already added in the last step.</span></span>

   <pre><code> 
   # Change the Linux system configuration
   sudo sysctl net.ipv4.tcp_keepalive_time=120
   </code></pre>

1. <span data-ttu-id="f8d96-383">**[A]**  Konfigurera SAP-användare efter installationen</span><span class="sxs-lookup"><span data-stu-id="f8d96-383">**[A]** Configure the SAP users after the installation</span></span>
 
   <pre><code>
   # Add sidadm to the haclient group
   sudo usermod -aG haclient <b>nws</b>adm   
   </code></pre>

1. <span data-ttu-id="f8d96-384">**[1]**  Lägg till ASCS och ÄNDARE SAP-tjänster i filen sapservice</span><span class="sxs-lookup"><span data-stu-id="f8d96-384">**[1]** Add the ASCS and ERS SAP services to the sapservice file</span></span>

   <span data-ttu-id="f8d96-385">Lägg till ASCS tjänsten post till den andra noden och kopiera posten ÄNDARE tjänsten till den första noden.</span><span class="sxs-lookup"><span data-stu-id="f8d96-385">Add the ASCS service entry to the second node and copy the ERS service entry to the first node.</span></span>

   <pre><code>
   cat /usr/sap/sapservices | grep ASCS<b>00</b> | sudo ssh <b>nws-cl-1</b> "cat >>/usr/sap/sapservices"
   sudo ssh <b>nws-cl-1</b> "cat /usr/sap/sapservices" | grep ERS<b>02</b> | sudo tee -a /usr/sap/sapservices
   </code></pre>

1. <span data-ttu-id="f8d96-386">**[1]**  Skapa klusterresurser SAP</span><span class="sxs-lookup"><span data-stu-id="f8d96-386">**[1]** Create the SAP cluster resources</span></span>

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

   <span data-ttu-id="f8d96-387">Kontrollera att klustret status är ok och att alla resurser har startats.</span><span class="sxs-lookup"><span data-stu-id="f8d96-387">Make sure that the cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="f8d96-388">Det är inte viktigt i vilken nod som använder resurserna.</span><span class="sxs-lookup"><span data-stu-id="f8d96-388">It is not important on which node the resources are running.</span></span>

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

### <a name="create-stonith-device"></a><span data-ttu-id="f8d96-389">Skapa STONITH enhet</span><span class="sxs-lookup"><span data-stu-id="f8d96-389">Create STONITH device</span></span>

<span data-ttu-id="f8d96-390">STONITH enheten använder ett huvudnamn för tjänsten för att godkänna mot Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f8d96-390">The STONITH device uses a Service Principal to authorize against Microsoft Azure.</span></span> <span data-ttu-id="f8d96-391">Följ dessa steg om du vill skapa ett huvudnamn för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f8d96-391">Follow these steps to create a Service Principal.</span></span>

1. <span data-ttu-id="f8d96-392">Gå till <https://portal.azure.com></span><span class="sxs-lookup"><span data-stu-id="f8d96-392">Go to <https://portal.azure.com></span></span>
1. <span data-ttu-id="f8d96-393">Öppna bladet Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f8d96-393">Open the Azure Active Directory blade</span></span>  
   <span data-ttu-id="f8d96-394">Gå till egenskaperna och Skriv ned Directory-Id.</span><span class="sxs-lookup"><span data-stu-id="f8d96-394">Go to Properties and write down the Directory Id.</span></span> <span data-ttu-id="f8d96-395">Det här är den **klient-id**.</span><span class="sxs-lookup"><span data-stu-id="f8d96-395">This is the **tenant id**.</span></span>
1. <span data-ttu-id="f8d96-396">Klicka på appen registreringar</span><span class="sxs-lookup"><span data-stu-id="f8d96-396">Click App registrations</span></span>
1. <span data-ttu-id="f8d96-397">Klicka på Lägg till</span><span class="sxs-lookup"><span data-stu-id="f8d96-397">Click Add</span></span>
1. <span data-ttu-id="f8d96-398">Ange ett namn, Välj typ av program ”Web app/API”, ange en inloggnings-URL (till exempel http://localhost) och klicka på Skapa</span><span class="sxs-lookup"><span data-stu-id="f8d96-398">Enter a Name, select Application Type "Web app/API", enter a sign-on URL (for example http://localhost) and click Create</span></span>
1. <span data-ttu-id="f8d96-399">URL för inloggning används inte och kan vara en giltig URL</span><span class="sxs-lookup"><span data-stu-id="f8d96-399">The sign-on URL is not used and can be any valid URL</span></span>
1. <span data-ttu-id="f8d96-400">Välj den nya appen och klicka på nycklar på fliken Inställningar</span><span class="sxs-lookup"><span data-stu-id="f8d96-400">Select the new App and click Keys in the Settings tab</span></span>
1. <span data-ttu-id="f8d96-401">Ange en beskrivning för en ny nyckel, Välj ”upphör aldrig att gälla” och klicka på Spara</span><span class="sxs-lookup"><span data-stu-id="f8d96-401">Enter a description for a new key, select "Never expires" and click Save</span></span>
1. <span data-ttu-id="f8d96-402">Anteckna värdet.</span><span class="sxs-lookup"><span data-stu-id="f8d96-402">Write down the Value.</span></span> <span data-ttu-id="f8d96-403">Den används som den **lösenord** för tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="f8d96-403">It is used as the **password** for the Service Principal</span></span>
1. <span data-ttu-id="f8d96-404">Skriv ned det program-Id.</span><span class="sxs-lookup"><span data-stu-id="f8d96-404">Write down the Application Id.</span></span> <span data-ttu-id="f8d96-405">Den används som användarnamnet (**inloggnings-id** i stegen nedan) för tjänstens huvudnamn</span><span class="sxs-lookup"><span data-stu-id="f8d96-405">It is used as the username (**login id** in the steps below) of the Service Principal</span></span>

<span data-ttu-id="f8d96-406">Tjänstens huvudnamn har inte behörighet att komma åt Azure-resurser som standard.</span><span class="sxs-lookup"><span data-stu-id="f8d96-406">The Service Principal does not have permissions to access your Azure resources by default.</span></span> <span data-ttu-id="f8d96-407">Du behöver ge behörighet att starta och stoppa tjänstens huvudnamn (frigöra) alla virtuella datorer i klustret.</span><span class="sxs-lookup"><span data-stu-id="f8d96-407">You need to give the Service Principal permissions to start and stop (deallocate) all virtual machines of the cluster.</span></span>

1. <span data-ttu-id="f8d96-408">Gå till https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="f8d96-408">Go to https://portal.azure.com</span></span>
1. <span data-ttu-id="f8d96-409">Öppna bladet alla resurser</span><span class="sxs-lookup"><span data-stu-id="f8d96-409">Open the All resources blade</span></span>
1. <span data-ttu-id="f8d96-410">Välj den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="f8d96-410">Select the virtual machine</span></span>
1. <span data-ttu-id="f8d96-411">Klicka på åtkomstkontroll (IAM)</span><span class="sxs-lookup"><span data-stu-id="f8d96-411">Click Access control (IAM)</span></span>
1. <span data-ttu-id="f8d96-412">Klicka på Lägg till</span><span class="sxs-lookup"><span data-stu-id="f8d96-412">Click Add</span></span>
1. <span data-ttu-id="f8d96-413">Välj rollen ägare</span><span class="sxs-lookup"><span data-stu-id="f8d96-413">Select the role Owner</span></span>
1. <span data-ttu-id="f8d96-414">Ange namnet på programmet som du skapade ovan</span><span class="sxs-lookup"><span data-stu-id="f8d96-414">Enter the name of the application you created above</span></span>
1. <span data-ttu-id="f8d96-415">Klicka på OK</span><span class="sxs-lookup"><span data-stu-id="f8d96-415">Click OK</span></span>

#### <a name="1-create-the-stonith-devices"></a><span data-ttu-id="f8d96-416">**[1]**  Skapa STONITH-enheter</span><span class="sxs-lookup"><span data-stu-id="f8d96-416">**[1]** Create the STONITH devices</span></span>

<span data-ttu-id="f8d96-417">När du har redigerat behörigheter för de virtuella datorerna kan du konfigurera STONITH-enheter i klustret.</span><span class="sxs-lookup"><span data-stu-id="f8d96-417">After you edited the permissions for the virtual machines, you can configure the STONITH devices in the cluster.</span></span>

<pre><code>
sudo crm configure

# replace the bold string with your subscription id, resource group, tenant id, service principal id and password

crm(live)configure# primitive rsc_st_azure_1 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# primitive rsc_st_azure_2 stonith:fence_azure_arm \
   params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

crm(live)configure# colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started

crm(live)configure# commit
crm(live)configure# exit
</code></pre>

#### <a name="1-enable-the-use-of-a-stonith-device"></a><span data-ttu-id="f8d96-418">**[1]**  Aktivera användning av en STONITH-enhet</span><span class="sxs-lookup"><span data-stu-id="f8d96-418">**[1]** Enable the use of a STONITH device</span></span>

<span data-ttu-id="f8d96-419">Aktivera användning av en STONITH-enhet</span><span class="sxs-lookup"><span data-stu-id="f8d96-419">Enable the use of a STONITH device</span></span>

<pre><code>
sudo crm configure property stonith-enabled=true 
</code></pre>

## <a name="install-database"></a><span data-ttu-id="f8d96-420">Installera databasen</span><span class="sxs-lookup"><span data-stu-id="f8d96-420">Install database</span></span>

<span data-ttu-id="f8d96-421">I det här exemplet är en SAP HANA System replikering installeras och konfigureras.</span><span class="sxs-lookup"><span data-stu-id="f8d96-421">In this example an SAP HANA System Replication is installed and configured.</span></span> <span data-ttu-id="f8d96-422">SAP HANA körs i samma kluster som SAP NetWeaver ASCS/SCS och ÄNDARE.</span><span class="sxs-lookup"><span data-stu-id="f8d96-422">SAP HANA will run in the same cluster as the SAP NetWeaver ASCS/SCS and ERS.</span></span> <span data-ttu-id="f8d96-423">Du kan också installera SAP HANA på ett kluster.</span><span class="sxs-lookup"><span data-stu-id="f8d96-423">You can also install SAP HANA on a dedicated cluster.</span></span> <span data-ttu-id="f8d96-424">Se [hög tillgänglighet för SAP HANA på Azure Virtual Machines (virtuella datorer)] [ sap-hana-ha] för mer information.</span><span class="sxs-lookup"><span data-stu-id="f8d96-424">See [High Availability of SAP HANA on Azure Virtual Machines (VMs)][sap-hana-ha] for more information.</span></span>

### <a name="prepare-for-sap-hana-installation"></a><span data-ttu-id="f8d96-425">Förbereda för SAP HANA-installation</span><span class="sxs-lookup"><span data-stu-id="f8d96-425">Prepare for SAP HANA installation</span></span>

<span data-ttu-id="f8d96-426">Normalt bör du använda LVM för volymer som lagrar data och loggfiler.</span><span class="sxs-lookup"><span data-stu-id="f8d96-426">We generally recommend using LVM for volumes that store data and log files.</span></span> <span data-ttu-id="f8d96-427">I testsyfte kan välja du också att lagra data och loggfilen direkt på en vanlig disk.</span><span class="sxs-lookup"><span data-stu-id="f8d96-427">For testing purposes, you can also choose to store the data and log file directly on a plain disk.</span></span>

1. <span data-ttu-id="f8d96-428">**[A]**  LVM</span><span class="sxs-lookup"><span data-stu-id="f8d96-428">**[A]** LVM</span></span>  
   <span data-ttu-id="f8d96-429">Exemplet nedan förutsätter att de virtuella datorerna har fyra datadiskar som är anslutna som ska användas för att skapa två volymer.</span><span class="sxs-lookup"><span data-stu-id="f8d96-429">The example below assumes that the virtual machines have four data disks attached that should be used to create two volumes.</span></span>
   
   <span data-ttu-id="f8d96-430">Skapa fysiska volymer för alla diskar som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="f8d96-430">Create physical volumes for all disks that you want to use.</span></span>
   
   <pre><code>
   sudo pvcreate /dev/sdd
   sudo pvcreate /dev/sde
   sudo pvcreate /dev/sdf
   sudo pvcreate /dev/sdg
   </code></pre>
   
   <span data-ttu-id="f8d96-431">Skapa en volym grupp för datafiler, en volym grupp för loggfilerna och en för den delade katalogen för SAP HANA</span><span class="sxs-lookup"><span data-stu-id="f8d96-431">Create a volume group for the data files, one volume group for the log files and one for the shared directory of SAP HANA</span></span>
   
   <pre><code>
   sudo vgcreate vg_hana_data /dev/sdd /dev/sde
   sudo vgcreate vg_hana_log /dev/sdf
   sudo vgcreate vg_hana_shared /dev/sdg
   </code></pre>
   
   <span data-ttu-id="f8d96-432">Skapa logiska volymer</span><span class="sxs-lookup"><span data-stu-id="f8d96-432">Create the logical volumes</span></span>
   
   <pre><code>
   sudo lvcreate -l 100%FREE -n hana_data vg_hana_data
   sudo lvcreate -l 100%FREE -n hana_log vg_hana_log
   sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared
   sudo mkfs.xfs /dev/vg_hana_data/hana_data
   sudo mkfs.xfs /dev/vg_hana_log/hana_log
   sudo mkfs.xfs /dev/vg_hana_shared/hana_shared
   </code></pre>
   
   <span data-ttu-id="f8d96-433">Skapa mount-kataloger och kopiera alla logiska volymer UUID</span><span class="sxs-lookup"><span data-stu-id="f8d96-433">Create the mount directories and copy the UUID of all logical volumes</span></span>
   
   <pre><code>
   sudo mkdir -p /hana/data
   sudo mkdir -p /hana/log
   sudo mkdir -p /hana/shared
   sudo chattr +i /hana/data
   sudo chattr +i /hana/log
   sudo chattr +i /hana/shared
   # write down the id of /dev/vg_hana_data/hana_data, /dev/vg_hana_log/hana_log and /dev/vg_hana_shared/hana_shared
   sudo blkid
   </code></pre>
   
   <span data-ttu-id="f8d96-434">Skapa autofs poster för de tre logiska volymerna</span><span class="sxs-lookup"><span data-stu-id="f8d96-434">Create autofs entries for the three logical volumes</span></span>
   
   <pre><code>
   sudo vi /etc/auto.direct
   </code></pre>
   
   <span data-ttu-id="f8d96-435">Infoga raden till sudo vi /etc/auto.direct</span><span class="sxs-lookup"><span data-stu-id="f8d96-435">Insert this line to sudo vi /etc/auto.direct</span></span>
   
   <pre><code>
   /hana/data -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_data/hana_data&gt;</b>
   /hana/log -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_log/hana_log&gt;</b>
   /hana/shared -fstype=xfs :UUID=<b>&lt;UUID of /dev/vg_hana_shared/hana_shared&gt;</b>
   </code></pre>
   
   <span data-ttu-id="f8d96-436">Montera nya volymer</span><span class="sxs-lookup"><span data-stu-id="f8d96-436">Mount the new volumes</span></span>
   
   <pre><code>
   sudo service autofs restart 
   </code></pre>

1. <span data-ttu-id="f8d96-437">**[A]**  Vanlig diskar</span><span class="sxs-lookup"><span data-stu-id="f8d96-437">**[A]** Plain Disks</span></span>  

   <span data-ttu-id="f8d96-438">För små eller demonstrera system, du kan placera HANA data och loggfilen filer på en disk.</span><span class="sxs-lookup"><span data-stu-id="f8d96-438">For small or demo systems, you can place your HANA data and log files on one disk.</span></span> <span data-ttu-id="f8d96-439">Följande kommandon skapar en partition på /dev/sdc och formatera den med xfs.</span><span class="sxs-lookup"><span data-stu-id="f8d96-439">The following commands create a partition on /dev/sdc and format it with xfs.</span></span>
   ```bash
   sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sdd'
   sudo mkfs.xfs /dev/sdd1
   
   # write down the id of /dev/sdd1
   sudo /sbin/blkid
   sudo vi /etc/auto.direct
   ```
   
   <span data-ttu-id="f8d96-440">Infoga raden till /etc/auto.direct</span><span class="sxs-lookup"><span data-stu-id="f8d96-440">Insert this line to /etc/auto.direct</span></span>
   <pre><code>
   /hana -fstype=xfs :UUID=<b>&lt;UUID&gt;</b>
   </code></pre>
   
   <span data-ttu-id="f8d96-441">Skapa målkatalogen och montera disken.</span><span class="sxs-lookup"><span data-stu-id="f8d96-441">Create the target directory and mount the disk.</span></span>
   
   <pre><code>
   sudo mkdir /hana
   sudo chattr +i /hana
   sudo service autofs restart
   </code></pre>

### <a name="installing-sap-hana"></a><span data-ttu-id="f8d96-442">Installera SAP HANA</span><span class="sxs-lookup"><span data-stu-id="f8d96-442">Installing SAP HANA</span></span>

<span data-ttu-id="f8d96-443">Följande steg baseras på kapitel 4 i den [SAP HANA SR prestanda optimerade scenariot guiden] [ suse-hana-ha-guide] att installera SAP HANA System replikering.</span><span class="sxs-lookup"><span data-stu-id="f8d96-443">The following steps are based on chapter 4 of the [SAP HANA SR Performance Optimized Scenario guide][suse-hana-ha-guide] to install SAP HANA System Replication.</span></span> <span data-ttu-id="f8d96-444">Läs den innan du fortsätter med installationen.</span><span class="sxs-lookup"><span data-stu-id="f8d96-444">Please read it before you continue the installation.</span></span>

1. <span data-ttu-id="f8d96-445">**[A]**  Kör hdblcm från DVD: n HANA</span><span class="sxs-lookup"><span data-stu-id="f8d96-445">**[A]** Run hdblcm from the HANA DVD</span></span>
   
   <pre><code>
   sudo hdblcm --sid=<b>HDB</b> --number=<b>03</b> --action=install --batch --password=<b>&lt;password&gt;</b> --system_user_password=<b>&lt;password for system user&gt;</b>

   sudo /hana/shared/<b>HDB</b>/hdblcm/hdblcm --action=configure_internal_network --listen_interface=internal --internal_network=<b>10.0.0/24</b> --password=<b>&lt;password for system user&gt;</b> --batch
   </code></pre>

1. <span data-ttu-id="f8d96-446">**[A]**  Uppgradera SAP Värdagenten</span><span class="sxs-lookup"><span data-stu-id="f8d96-446">**[A]** Upgrade SAP Host Agent</span></span>

   <span data-ttu-id="f8d96-447">Hämta senaste SAP Värdagenten arkivet från den [SAP Softwarecenter] [ sap-swcenter] och kör följande kommando för att uppgradera agenten.</span><span class="sxs-lookup"><span data-stu-id="f8d96-447">Download the latest SAP Host Agent archive from the [SAP Softwarecenter][sap-swcenter] and run the following command to upgrade the agent.</span></span> <span data-ttu-id="f8d96-448">Ersätt sökvägen till arkivet så att den pekar till den fil som du hämtat.</span><span class="sxs-lookup"><span data-stu-id="f8d96-448">Replace the path to the archive to point to the file you downloaded.</span></span>
   <pre><code>
   sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <b>&lt;path to SAP Host Agent SAR&gt;</b> 
   </code></pre>

1. <span data-ttu-id="f8d96-449">**[1]**  Skapa HANA replikering (som rot)</span><span class="sxs-lookup"><span data-stu-id="f8d96-449">**[1]** Create HANA replication (as root)</span></span>  

   <span data-ttu-id="f8d96-450">Kör följande kommando.</span><span class="sxs-lookup"><span data-stu-id="f8d96-450">Run the following command.</span></span> <span data-ttu-id="f8d96-451">Se till att ersätta fetstil strängar (HANA System-ID HDB och instans antalet 03) med värden för SAP HANA-installationen.</span><span class="sxs-lookup"><span data-stu-id="f8d96-451">Make sure to replace bold strings (HANA System ID HDB and instance number 03) with the values of your SAP HANA installation.</span></span>
   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
   hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN TO <b>hdb</b>hasync' 
   hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
   </code></pre>

1. <span data-ttu-id="f8d96-452">**[A]**  Skapa keystore posten (rot)</span><span class="sxs-lookup"><span data-stu-id="f8d96-452">**[A]** Create keystore entry (as root)</span></span>

   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>&lt;passwd&gt;</b>
   </code></pre>

1. <span data-ttu-id="f8d96-453">**[1]**  Backup database (som rot)</span><span class="sxs-lookup"><span data-stu-id="f8d96-453">**[1]** Backup database (as root)</span></span>

   <pre><code>
   PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
   hdbsql -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')" 
   </code></pre>

1. <span data-ttu-id="f8d96-454">**[1]**  Växla till HANA sapsid användare och skapa den primära platsen.</span><span class="sxs-lookup"><span data-stu-id="f8d96-454">**[1]** Switch to the HANA sapsid user and create the primary site.</span></span>

   <pre><code>
   su - <b>hdb</b>adm
   hdbnsutil -sr_enable –-name=<b>SITE1</b>
   </code></pre>

1. <span data-ttu-id="f8d96-455">**[2]**  Växla till HANA sapsid användare och skapa en sekundär plats.</span><span class="sxs-lookup"><span data-stu-id="f8d96-455">**[2]** Switch to the HANA sapsid user and create the secondary site.</span></span>

   <pre><code>
   su - <b>hdb</b>adm
   sapcontrol -nr <b>03</b> -function StopWait 600 10
   hdbnsutil -sr_register --remoteHost=<b>nws-cl-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
   </code></pre>

1. <span data-ttu-id="f8d96-456">**[1]**  Skapa SAP HANA klusterresurser</span><span class="sxs-lookup"><span data-stu-id="f8d96-456">**[1]** Create SAP HANA cluster resources</span></span>

   <span data-ttu-id="f8d96-457">Först skapa topologin.</span><span class="sxs-lookup"><span data-stu-id="f8d96-457">First, create the topology.</span></span>
   
   <pre><code>
   sudo crm configure

   # replace the bold string with your instance number and HANA system id
   
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
   
   <span data-ttu-id="f8d96-458">Skapa sedan HANA-resurser</span><span class="sxs-lookup"><span data-stu-id="f8d96-458">Next, create the HANA resources</span></span>
   
   <pre><code>
   sudo crm configure

   # replace the bold string with your instance number, HANA system id and the frontend IP address of the Azure load balancer. 
    
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

   <span data-ttu-id="f8d96-459">Kontrollera att klustret status är ok och att alla resurser har startats.</span><span class="sxs-lookup"><span data-stu-id="f8d96-459">Make sure that the cluster status is ok and that all resources are started.</span></span> <span data-ttu-id="f8d96-460">Det är inte viktigt i vilken nod som använder resurserna.</span><span class="sxs-lookup"><span data-stu-id="f8d96-460">It is not important on which node the resources are running.</span></span>

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

1. <span data-ttu-id="f8d96-461">**[1]**  Installera databasinstansen SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="f8d96-461">**[1]** Install the SAP NetWeaver database instance</span></span>

   <span data-ttu-id="f8d96-462">Installera SAP NetWeaver-databasinstans som rot med hjälp av ett virtuellt värdnamn som mappar till IP-adressen för klientdel belastningsutjämningskonfigurationen för databasen, till exempel <b>nws-db</b> och <b>10.0.0.12</b>.</span><span class="sxs-lookup"><span data-stu-id="f8d96-462">Install the SAP NetWeaver database instance as root using a virtual hostname that maps to the IP address of the load balancer frontend configuration for the database for example <b>nws-db</b> and <b>10.0.0.12</b>.</span></span>

   <span data-ttu-id="f8d96-463">Du kan använda parametern sapinst SAPINST_REMOTE_ACCESS_USER för att tillåta en rot-användare att ansluta till sapinst.</span><span class="sxs-lookup"><span data-stu-id="f8d96-463">You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

## <a name="sap-netweaver-application-server-installation"></a><span data-ttu-id="f8d96-464">SAP NetWeaver application server-installation</span><span class="sxs-lookup"><span data-stu-id="f8d96-464">SAP NetWeaver application server installation</span></span>

<span data-ttu-id="f8d96-465">Följ dessa steg om du vill installera en SAP-programserver.</span><span class="sxs-lookup"><span data-stu-id="f8d96-465">Follow these steps to install an SAP application server.</span></span> <span data-ttu-id="f8d96-466">Stegen nedan förutsätter att du installerar application server på en annan server än ASCS/SCS och HANA-servrar.</span><span class="sxs-lookup"><span data-stu-id="f8d96-466">The steps bellow assume that you install the application server on a server different from the ASCS/SCS and HANA servers.</span></span> <span data-ttu-id="f8d96-467">Annars behövs vissa av stegen nedan (t.ex. Konfigurera värdnamnsmatchning) inte.</span><span class="sxs-lookup"><span data-stu-id="f8d96-467">Otherwise some of the steps below (like configuring host name resolution) are not needed.</span></span>

1. <span data-ttu-id="f8d96-468">Konfigurera värdnamnsmatchning</span><span class="sxs-lookup"><span data-stu-id="f8d96-468">Setup host name resolution</span></span>    
   <span data-ttu-id="f8d96-469">Du kan använda en DNS-server, eller så kan du ändra de/etc/hosts på alla noder.</span><span class="sxs-lookup"><span data-stu-id="f8d96-469">You can either use a DNS server or modify the /etc/hosts on all nodes.</span></span> <span data-ttu-id="f8d96-470">Det här exemplet visar hur du använder/etc/hosts-filen.</span><span class="sxs-lookup"><span data-stu-id="f8d96-470">This example shows how to use the /etc/hosts file.</span></span>
   <span data-ttu-id="f8d96-471">Ersätt IP-adressen och värdnamnet i följande kommandon</span><span class="sxs-lookup"><span data-stu-id="f8d96-471">Replace the IP address and the hostname in the following commands</span></span>
   ```bash
   sudo vi /etc/hosts
   ```
   <span data-ttu-id="f8d96-472">Infoga följande rader till/etc/hosts.</span><span class="sxs-lookup"><span data-stu-id="f8d96-472">Insert the following lines to /etc/hosts.</span></span> <span data-ttu-id="f8d96-473">Ändra IP-adressen och värdnamnet som matchar din miljö</span><span class="sxs-lookup"><span data-stu-id="f8d96-473">Change the IP address and hostname to match your environment</span></span>    
    
   <pre><code>
   # IP address of the load balancer frontend configuration for NFS
   <b>10.0.0.4 nws-nfs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ASCS/SCS
   <b>10.0.0.10 nws-ascs</b>
   # IP address of the load balancer frontend configuration for SAP NetWeaver ERS
   <b>10.0.0.11 nws-ers</b>
   # IP address of the load balancer frontend configuration for database
   <b>10.0.0.12 nws-db</b>
   # IP address of the application server
   <b>10.0.0.8 nws-di-0</b>
   </code></pre>

1. <span data-ttu-id="f8d96-474">Skapa katalogen sapmnt</span><span class="sxs-lookup"><span data-stu-id="f8d96-474">Create the sapmnt directory</span></span>

   <pre><code>
   sudo mkdir -p /sapmnt/<b>NWS</b>
   sudo mkdir -p /usr/sap/trans

   sudo chattr +i /sapmnt/<b>NWS</b>
   sudo chattr +i /usr/sap/trans
   </code></pre>

1. <span data-ttu-id="f8d96-475">Konfigurera autofs</span><span class="sxs-lookup"><span data-stu-id="f8d96-475">Configure autofs</span></span>
 
   <pre><code>
   sudo vi /etc/auto.master

   # Add the following line to the file, save and exit
   +auto.master
   /- /etc/auto.direct
   </code></pre>

   <span data-ttu-id="f8d96-476">Skapa en ny fil med</span><span class="sxs-lookup"><span data-stu-id="f8d96-476">Create a new file with</span></span>

   <pre><code>
   sudo vi /etc/auto.direct

   # Add the following lines to the file, save and exit
   /sapmnt/<b>NWS</b> -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/sapmntsid
   /usr/sap/trans -nfsvers=4,nosymlink,sync <b>nws-nfs</b>:/trans
   </code></pre>

   <span data-ttu-id="f8d96-477">Starta om autofs montera nya resurser</span><span class="sxs-lookup"><span data-stu-id="f8d96-477">Restart autofs to mount the new shares</span></span>

   <pre><code>
   sudo systemctl enable autofs
   sudo service autofs restart
   </code></pre>

1. <span data-ttu-id="f8d96-478">Konfigurera växlingsfilen</span><span class="sxs-lookup"><span data-stu-id="f8d96-478">Configure SWAP file</span></span>
 
   <pre><code>
   sudo vi /etc/waagent.conf

   # Set the property ResourceDisk.EnableSwap to y
   # Create and use swapfile on resource disk.
   ResourceDisk.EnableSwap=<b>y</b>

   # Set the size of the SWAP file with property ResourceDisk.SwapSizeMB
   # The free space of resource disk varies by virtual machine size. Make sure that you do not set a value that is too big. You can check the SWAP space with command swapon
   # Size of the swapfile.
   ResourceDisk.SwapSizeMB=<b>2000</b>
   </code></pre>

   <span data-ttu-id="f8d96-479">Starta om agenten för att aktivera ändringen</span><span class="sxs-lookup"><span data-stu-id="f8d96-479">Restart the Agent to activate the change</span></span>

   <pre><code>
   sudo service waagent restart
   </code></pre>

1. <span data-ttu-id="f8d96-480">Installera SAP NetWeaver programserver</span><span class="sxs-lookup"><span data-stu-id="f8d96-480">Install SAP NetWeaver application server</span></span>

   <span data-ttu-id="f8d96-481">Installera en primär eller ytterligare SAP NetWeaver programserver.</span><span class="sxs-lookup"><span data-stu-id="f8d96-481">Install a primary or additional SAP NetWeaver applications server.</span></span>

   <span data-ttu-id="f8d96-482">Du kan använda parametern sapinst SAPINST_REMOTE_ACCESS_USER för att tillåta en rot-användare att ansluta till sapinst.</span><span class="sxs-lookup"><span data-stu-id="f8d96-482">You can use the sapinst parameter SAPINST_REMOTE_ACCESS_USER to allow a non-root user to connect to sapinst.</span></span>

   <pre><code>
   sudo &lt;swpm&gt;/sapinst SAPINST_REMOTE_ACCESS_USER=<b>sapadmin</b>
   </code></pre>

1. <span data-ttu-id="f8d96-483">Uppdatera SAP HANA säker lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="f8d96-483">Update SAP HANA secure store</span></span>

   <span data-ttu-id="f8d96-484">Uppdatera SAP HANA säker lagringstjänst att peka till det virtuella namnet på installationsprogrammet för SAP HANA System replikering.</span><span class="sxs-lookup"><span data-stu-id="f8d96-484">Update the SAP HANA secure store to point to the virtual name of the SAP HANA System Replication setup.</span></span>
   <pre><code>
   su - <b>nws</b>adm
   hdbuserstore SET DEFAULT <b>nws-db</b>:3<b>03</b>15 <b>SAPABAP1</b> <b>&lt;password of ABAP schema&gt;</b>
   </code></pre>

## <a name="next-steps"></a><span data-ttu-id="f8d96-485">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f8d96-485">Next steps</span></span>
* <span data-ttu-id="f8d96-486">[Azure virtuella datorer planering och implementering för SAP][planning-guide]</span><span class="sxs-lookup"><span data-stu-id="f8d96-486">[Azure Virtual Machines planning and implementation for SAP][planning-guide]</span></span>
* <span data-ttu-id="f8d96-487">[Distribution av Azure virtuella datorer för SAP][deployment-guide]</span><span class="sxs-lookup"><span data-stu-id="f8d96-487">[Azure Virtual Machines deployment for SAP][deployment-guide]</span></span>
* <span data-ttu-id="f8d96-488">[Azure virtuella datorer DBMS-distribution för SAP][dbms-guide]</span><span class="sxs-lookup"><span data-stu-id="f8d96-488">[Azure Virtual Machines DBMS deployment for SAP][dbms-guide]</span></span>
* <span data-ttu-id="f8d96-489">Information om hur du upprättar och planera för katastrofåterställning för SAP HANA i Azure (stora instanser) med hög tillgänglighet finns [SAP HANA (stora instanser) hög tillgänglighet och katastrofåterställning recovery på Azure](hana-overview-high-availability-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="f8d96-489">To learn how to establish high availability and plan for disaster recovery of SAP HANA on Azure (large instances), see [SAP HANA (large instances) high availability and disaster recovery on Azure](hana-overview-high-availability-disaster-recovery.md).</span></span>
* <span data-ttu-id="f8d96-490">Information om hur du upprättar och planera för katastrofåterställning för SAP HANA på virtuella Azure-datorer med hög tillgänglighet finns [hög tillgänglighet för SAP HANA på Azure Virtual Machines (virtuella datorer)][sap-hana-ha]</span><span class="sxs-lookup"><span data-stu-id="f8d96-490">To learn how to establish high availability and plan for disaster recovery of SAP HANA on Azure VMs, see [High Availability of SAP HANA on Azure Virtual Machines (VMs)][sap-hana-ha]</span></span>