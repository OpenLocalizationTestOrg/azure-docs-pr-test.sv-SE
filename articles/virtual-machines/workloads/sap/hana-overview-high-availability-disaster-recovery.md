---
title: "Hög tillgänglighet och katastrofåterställning återställning av SAP HANA i Azure (stora instanser) | Microsoft Docs"
description: "Upprätta hög tillgänglighet och planera för katastrofåterställning för SAP HANA i Azure (stora instanser)."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/01/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f95e944fc3ec3a831d97386443eb644420ae54dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="sap-hana-large-instances-high-availability-and-disaster-recovery-on-azure"></a><span data-ttu-id="7a9e5-103">SAP HANA (stora instanser) hög tillgänglighet och katastrofåterställning recovery på Azure</span><span class="sxs-lookup"><span data-stu-id="7a9e5-103">SAP HANA (large instances) high availability and disaster recovery on Azure</span></span> 

<span data-ttu-id="7a9e5-104">Hög tillgänglighet och katastrofåterställning är viktiga aspekter av din verksamhetskritiska SAP HANA i Azure (stora instanser) servrar.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-104">High availability and disaster recovery are important aspects of running your mission-critical SAP HANA on Azure (large instances) servers.</span></span> <span data-ttu-id="7a9e5-105">Det är viktigt att arbeta med SAP, din systemintegreraren eller Microsoft skapa och implementera rätt hög-tillgänglighet/katastrofåterställning strategin korrekt.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-105">It's important to work with SAP, your system integrator, or Microsoft to properly architect and implement the right high-availability/disaster-recovery strategy.</span></span> <span data-ttu-id="7a9e5-106">Det är också viktigt att beakta återställningspunktmål och mål, som är specifika för din miljö.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-106">It is also important to consider the recovery point objective and recovery time objective, which are specific to your environment.</span></span>

## <a name="high-availability"></a><span data-ttu-id="7a9e5-107">Hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="7a9e5-107">High availability</span></span>

<span data-ttu-id="7a9e5-108">Microsoft stöder SAP HANA hög tillgänglighet metoder ”out-of-rutan” bland annat:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-108">Microsoft supports SAP HANA high-availability methods "out of the box," which include:</span></span>

- <span data-ttu-id="7a9e5-109">**Storage-replikering:** storage-system kan replikera alla data till en annan plats (inom, eller separat från samma datacenter).</span><span class="sxs-lookup"><span data-stu-id="7a9e5-109">**Storage replication:** The storage system's ability to replicate all data to another location (within, or separate from, the same datacenter).</span></span> <span data-ttu-id="7a9e5-110">SAP HANA fungerar oberoende av den här metoden.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-110">SAP HANA operates independently of this method.</span></span>
- <span data-ttu-id="7a9e5-111">**HANA system replikering:** replikering av alla data i SAP HANA till ett separat system för SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-111">**HANA system replication:** The replication of all data in SAP HANA to a separate SAP HANA system.</span></span> <span data-ttu-id="7a9e5-112">Mål minimeras via datareplikering med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-112">The recovery time objective is minimized through data replication at regular intervals.</span></span> <span data-ttu-id="7a9e5-113">SAP HANA stöder asynkrona, synkron i minnet och synkront läge (rekommenderas endast för SAP HANA-system som är i samma datacenter eller mindre än 100 KM från varandra).</span><span class="sxs-lookup"><span data-stu-id="7a9e5-113">SAP HANA supports asynchronous, synchronous in-memory, and synchronous modes (recommended only for SAP HANA systems that are within the same datacenter or less than 100 KM apart).</span></span> <span data-ttu-id="7a9e5-114">I den aktuella designen av HANA stora instans stämplar kan HANA system replikering användas för hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-114">In the current design of HANA large-instance stamps, HANA system replication can be used for high availability only.</span></span>
- <span data-ttu-id="7a9e5-115">**Värd för automatisk redundans:** en lokal fault-lösning ska användas som ett alternativ till att systemet replikering.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-115">**Host auto-failover:** A local fault-recovery solution to use as an alternative to system replication.</span></span> <span data-ttu-id="7a9e5-116">När huvudnoden blir otillgänglig, en eller flera vänteläge SAP HANA-noder som är konfigurerade i skalbar läge och SAP HANA växlar automatiskt över till en annan nod.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-116">When the master node becomes unavailable, one or more standby SAP HANA nodes are configured in scale-out mode and SAP HANA automatically fails over to another node.</span></span>

<span data-ttu-id="7a9e5-117">Mer information om hög tillgänglighet för SAP HANA finns i följande SAP-information:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-117">For more information on SAP HANA high availability, see the following SAP information:</span></span>

- [<span data-ttu-id="7a9e5-118">Vitboken för SAP HANA hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="7a9e5-118">SAP HANA High-Availability Whitepaper</span></span>](http://go.sap.com/documents/2016/05/f8e5eeba-737c-0010-82c7-eda71af511fa.html)
- [<span data-ttu-id="7a9e5-119">SAP HANA Administrationsguide</span><span class="sxs-lookup"><span data-stu-id="7a9e5-119">SAP HANA Administration Guide</span></span>](http://help.sap.com/hana/SAP_HANA_Administration_Guide_en.pdf)
- [<span data-ttu-id="7a9e5-120">SAP Academy videon på SAP HANA System-replikering</span><span class="sxs-lookup"><span data-stu-id="7a9e5-120">SAP Academy Video on SAP HANA System Replication</span></span>](http://scn.sap.com/community/hana-in-memory/blog/2015/05/19/sap-hana-system-replication)
- [<span data-ttu-id="7a9e5-121">SAP stöd Obs #1999880 – vanliga frågor och svar på SAP HANA System-replikering</span><span class="sxs-lookup"><span data-stu-id="7a9e5-121">SAP Support Note #1999880 – FAQ on SAP HANA System Replication</span></span>](https://bcs.wdf.sap.corp/sap/support/notes/1999880)
- <span data-ttu-id="7a9e5-122">[SAP stöd Obs #2165547 – SAP HANA säkerhetskopiering och återställning i SAP HANA-systemmiljön replikering](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)</span><span class="sxs-lookup"><span data-stu-id="7a9e5-122">[SAP Support Note #2165547 – SAP HANA Backup and Restore within SAP HANA System Replication Environment](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)</span></span>
- <span data-ttu-id="7a9e5-123">[SAP stöd Obs #1984882 – med SAP HANA System replikering för maskinvara Exchange med minsta/noll avbrottstid](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)</span><span class="sxs-lookup"><span data-stu-id="7a9e5-123">[SAP Support Note #1984882 – Using SAP HANA System Replication for Hardware Exchange with Minimum/Zero Downtime](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)</span></span>

## <a name="disaster-recovery"></a><span data-ttu-id="7a9e5-124">Haveriberedskap</span><span class="sxs-lookup"><span data-stu-id="7a9e5-124">Disaster recovery</span></span>

<span data-ttu-id="7a9e5-125">SAP HANA i Azure (stora instanser) erbjuds i två Azure-regioner i en geopolitiska region.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-125">SAP HANA on Azure (large instances) is offered in two Azure regions in a geopolitical region.</span></span> <span data-ttu-id="7a9e5-126">Är en direkt nätverksanslutning för att replikera data vid katastrofåterställning mellan de två stora instans stämplarna för två olika regioner.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-126">Between the two large-instance stamps of two different regions is a direct network connectivity for replicating data during disaster recovery.</span></span> <span data-ttu-id="7a9e5-127">Replikering av data är lagringsinfrastruktur baserat.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-127">The replication of the data is storage-infrastructure based.</span></span> <span data-ttu-id="7a9e5-128">Replikeringen görs inte som standard.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-128">The replication is not done by default.</span></span> <span data-ttu-id="7a9e5-129">Den är klar för kundkonfigurationer som sorterade disaster recovery.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-129">It is done for the customer configurations that ordered the disaster recovery.</span></span> <span data-ttu-id="7a9e5-130">I den aktuella designen kan inte HANA system replikering användas för katastrofåterställning.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-130">In the current design, HANA system replication can't be used for disaster recovery.</span></span>

<span data-ttu-id="7a9e5-131">Men om du vill dra nytta av katastrofåterställning, måste du starta att utforma nätverksanslutningen till de två olika Azure-regionerna.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-131">However, to take advantage of the disaster recovery, you need to start to design the network connectivity to the two different Azure regions.</span></span> <span data-ttu-id="7a9e5-132">Om du vill göra det, behöver du en Azure ExpressRoute-krets anslutning från lokala i dina viktigaste Azure-region och krets anslutning från lokal till din region för katastrofåterställning.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-132">To do so, you need an Azure ExpressRoute circuit connection from on-premises in your main Azure region and another circuit connection from on-premises to your disaster-recovery region.</span></span> <span data-ttu-id="7a9e5-133">Det här måttet bör ge en situation där en fullständig Azure-region, inklusive en Microsoft enterprise edge router (MSEE) plats, har ett problem.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-133">This measure would cover a situation in which a complete Azure region, including a Microsoft enterprise edge router (MSEE) location, has an issue.</span></span>

<span data-ttu-id="7a9e5-134">Du kan ansluta alla virtuella Azure-nätverk som ansluter till SAP HANA i Azure (stora instanser) i någon av regionerna till båda dessa ExpressRoute-kretsar som en andra åtgärd.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-134">As a second measure, you can connect all Azure virtual networks that connect to SAP HANA on Azure (large instances) in one of the regions to both of those ExpressRoute circuits.</span></span> <span data-ttu-id="7a9e5-135">Det här måttet adresser fall där endast en av platserna som MSEE som ansluter din lokala plats med Azure kan lämna avgift.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-135">This measure addresses a case where only one of the MSEE locations that connects your on-premises location with Azure goes off duty.</span></span>

<span data-ttu-id="7a9e5-136">Följande bild visar den bästa konfigurationen för katastrofåterställning:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-136">The following figure shows the optimal configuration for disaster recovery:</span></span>

![Bästa konfigurationen för katastrofåterställning](./media/hana-overview-high-availability-disaster-recovery/image1-optimal-configuration.png)

<span data-ttu-id="7a9e5-138">Optimal fallet för en katastrofåterställning konfiguration av nätverket är att ha två ExpressRoute-kretsar från lokal till de två olika Azure-regionerna.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-138">The optimal case for a disaster-recovery configuration of the network is to have two ExpressRoute circuits from on-premises to the two different Azure regions.</span></span> <span data-ttu-id="7a9e5-139">En krets går till region #1, kör en instans för produktion.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-139">One circuit goes to region #1, running a production instance.</span></span> <span data-ttu-id="7a9e5-140">Andra ExpressRoute-kretsen går till region #2, kör vissa icke-produktion HANA instanser.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-140">The second ExpressRoute circuit goes to region #2, running some non-production HANA instances.</span></span> <span data-ttu-id="7a9e5-141">(Detta är viktigt om en hel Azure-region, inklusive MSEE och stora instans stämpel stängs av rutnät.)</span><span class="sxs-lookup"><span data-stu-id="7a9e5-141">(This is important if an entire Azure region, including the MSEE and large-instance stamp, goes off the grid.)</span></span>

<span data-ttu-id="7a9e5-142">Som en andra åtgärd är olika virtuella nätverk anslutna till de olika ExpressRoute-kretsar som är anslutna till SAP HANA i Azure (stora instanser).</span><span class="sxs-lookup"><span data-stu-id="7a9e5-142">As a second measure, the various virtual networks are connected to the various ExpressRoute circuits that are connected to SAP HANA on Azure (large instances).</span></span> <span data-ttu-id="7a9e5-143">Du kan kringgå den plats där en MSEE misslyckas eller så kan du sänka återställningspunktmål för katastrofåterställning, diskussion senare.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-143">You can bypass the location where an MSEE is failing, or you can lower the recovery point objective for disaster recovery, as we discuss later.</span></span>

<span data-ttu-id="7a9e5-144">Nästa kraven för en katastrofåterställning installation är:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-144">The next requirements for a disaster-recovery setup are:</span></span>

- <span data-ttu-id="7a9e5-145">Du måste beställa SAP HANA på Azure (stora instanser) SKU: er av samma storlek som din produktion SKU: er och distribuerar dem i området för katastrofåterställning.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-145">You must order SAP HANA on Azure (large instances) SKUs of the same size as your production SKUs and deploy them in the disaster-recovery region.</span></span> <span data-ttu-id="7a9e5-146">Dessa instanser kan användas för att köra testet, sandbox eller QA HANA instanser.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-146">These instances can be used to run test, sandbox, or QA HANA instances.</span></span>
- <span data-ttu-id="7a9e5-147">En profil för katastrofåterställning måste du sortera för varje din SAP HANA på Azure (stora instanser) SKU: er som du vill återställa i återställningsplats, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-147">You must order a disaster-recovery profile for each of your SAP HANA on Azure (large instances) SKUs that you want to recover in the disaster-recovery site, if necessary.</span></span> <span data-ttu-id="7a9e5-148">Den här åtgärden leder till tilldelning av lagringsvolymer som är mål för storage-replikering från din produktionsregion i området för katastrofåterställning.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-148">This action leads to the allocation of storage volumes, which are the target of the storage replication from your production region into the disaster-recovery region.</span></span>

<span data-ttu-id="7a9e5-149">Efter att du uppfyller ovanstående krav är det ditt ansvar att starta storage-replikering.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-149">After you meet the preceding requirements, it is your responsibility to start the storage replication.</span></span> <span data-ttu-id="7a9e5-150">Basen för storage-replikering är i lagringsinfrastruktur som används för SAP HANA i Azure (stora instanser), lagring ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-150">In the storage infrastructure used for SAP HANA on Azure (large instances), the basis of storage replication is storage snapshots.</span></span> <span data-ttu-id="7a9e5-151">Du måste utföra för att starta replikering för katastrofåterställning:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-151">To start the disaster-recovery replication, you need to perform:</span></span>

- <span data-ttu-id="7a9e5-152">En ögonblicksbild av startenheter LUN, enligt beskrivningen ovan.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-152">A snapshot of your boot LUN, as described earlier.</span></span>
- <span data-ttu-id="7a9e5-153">En ögonblicksbild av dina HANA-relaterade volymer, som tidigare beskrivits.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-153">A snapshot of your HANA-related volumes, as described earlier.</span></span>

<span data-ttu-id="7a9e5-154">När du kör dessa ögonblicksbilder, dirigeras en första replik av volymer på de volymer som är associerade med din profil för katastrofåterställning i området för katastrofåterställning.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-154">After you execute these snapshots, an initial replica of the volumes is seeded on the volumes that are associated with your disaster-recovery profile in the disaster-recovery region.</span></span>

<span data-ttu-id="7a9e5-155">Därefter används ögonblicksbilden av senaste lagring för varje timme att replikera går utvecklar på lagringsvolymer.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-155">Subsequently, the most recent storage snapshot is used every hour to replicate the deltas that develop on the storage volumes.</span></span>

<span data-ttu-id="7a9e5-156">Det återställningspunktmål som uppnås med den här konfigurationen är från 60 till 90 minuter.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-156">The recovery point objective that's achieved with this configuration is from 60 to 90 minutes.</span></span> <span data-ttu-id="7a9e5-157">För att uppnå ett bättre återställningspunktmål i fallet med katastrofåterställning, kopierar du säkerhetskopieringarna av transaktionsloggen HANA från SAP HANA i Azure (stora instanser) till andra Azure-regionen.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-157">To achieve a better recovery point objective in the disaster-recovery case, copy the HANA transaction log backups from SAP HANA on Azure (large instances) to the other Azure region.</span></span> <span data-ttu-id="7a9e5-158">För att uppnå detta mål för återställningspunkt, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-158">To achieve this recovery point objective, do the following:</span></span>

1. <span data-ttu-id="7a9e5-159">Logga in så ofta som möjligt /hana/log/backup säkerhetskopiera HANA transaktionen.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-159">Back up the HANA transaction log as frequently as possible to /hana/log/backup.</span></span>
2. <span data-ttu-id="7a9e5-160">Kopiera säkerhetskopieringarna av transaktionsloggen när de är klara att en virtuell Azure virtuell dator (VM) som är i ett virtuellt nätverk som är ansluten till SAP HANA på Azure (stora instanser)-server.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-160">Copy the transaction log backups when they are finished to an Azure virtual machine (VM), which is in a virtual network that's connected to the SAP HANA on Azure (large instances) server.</span></span>
3. <span data-ttu-id="7a9e5-161">Kopiera säkerhetskopian från den virtuella datorn till en virtuell dator som är i ett virtuellt nätverk i området för katastrofåterställning.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-161">From that VM, copy the backup to a VM that's in a virtual network in the disaster-recovery region.</span></span>
4. <span data-ttu-id="7a9e5-162">Behåll transaktionen säkerhetskopior i den regionen i den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-162">Keep the transaction log backups in that region in the VM.</span></span>

<span data-ttu-id="7a9e5-163">Vid katastrofåterställning, när katastrofåterställning-profil har distribuerats på en faktiska server, kopiera säkerhetskopieringarna av transaktionsloggen från den virtuella datorn till SAP HANA i Azure (stora instanser) som nu är den primära servern i området för katastrofåterställning och återställa dessa säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-163">In case of disaster, after the disaster-recovery profile has been deployed on an actual server, copy the transaction log backups from the VM to the SAP HANA on Azure (large instances) that is now the primary server in the disaster-recovery region, and restore those backups.</span></span> <span data-ttu-id="7a9e5-164">Återställningen är möjligt eftersom tillståndet för HANA på katastrofåterställning diskarna som en HANA ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-164">This recovery is possible because the state of HANA on the disaster-recovery disks is that of a HANA snapshot.</span></span> <span data-ttu-id="7a9e5-165">Detta är den offset för ytterligare återställningar av säkerhetskopieringar av transaktionsloggen.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-165">This is the offset point for further restorations of transaction log backups.</span></span>

## <a name="backup-and-restore"></a><span data-ttu-id="7a9e5-166">Säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="7a9e5-166">Backup and restore</span></span>

<span data-ttu-id="7a9e5-167">En av de viktigaste aspekterna att operativsystemet databaser Kontrollera att databasen skyddas från olika katastrofer.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-167">One of the most important aspects to operating databases is making sure the database can be protected from various catastrophic events.</span></span> <span data-ttu-id="7a9e5-168">Dessa händelser kan orsakas av något från naturkatastrof enkel fel.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-168">These events can be caused by anything from natural disasters to simple user errors.</span></span>

<span data-ttu-id="7a9e5-169">Säkerhetskopiera en databas med möjlighet att återställa den till en valfri tidpunkt (exempelvis innan någon bort kritiska data), kan återställningen till ett tillstånd som är så nära som möjligt så som den var innan avbrott uppstod.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-169">Backing up a database, with the ability to restore it to any point in time (such as before somebody deleted critical data), allows restoration to a state that is as close as possible to the way it was before the disruption occurred.</span></span>

<span data-ttu-id="7a9e5-170">Två typer av säkerhetskopior måste utföras för bästa resultat:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-170">Two types of backups must be performed for best results:</span></span>

- <span data-ttu-id="7a9e5-171">Säkerhetskopior av databasen</span><span class="sxs-lookup"><span data-stu-id="7a9e5-171">Database backups</span></span>
- <span data-ttu-id="7a9e5-172">Säkerhetskopieringar av transaktionsloggen</span><span class="sxs-lookup"><span data-stu-id="7a9e5-172">Transaction log backups</span></span>

<span data-ttu-id="7a9e5-173">Fullständig säkerhetskopiering utförs på en på programnivå, får du även mer omfattande genom att utföra säkerhetskopieringar med lagring ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-173">In addition to full database backups performed at an application-level, you can be even more thorough by performing backups with storage snapshots.</span></span> <span data-ttu-id="7a9e5-174">Utför säkerhetskopior är också viktigt för att återställa databasen (och för att tömma loggar från redan bekräftats transaktioner).</span><span class="sxs-lookup"><span data-stu-id="7a9e5-174">Performing log backups is also important for restoring the database (and to empty the logs from already committed transactions).</span></span>

<span data-ttu-id="7a9e5-175">SAP HANA i Azure (stora instanser) erbjuder två alternativ för säkerhetskopiering och återställning:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-175">SAP HANA on Azure (large instances) offers two backup and restore options:</span></span>

- <span data-ttu-id="7a9e5-176">Göra det själv (DIY).</span><span class="sxs-lookup"><span data-stu-id="7a9e5-176">Do it yourself (DIY).</span></span> <span data-ttu-id="7a9e5-177">När du beräkna om du vill se till att tillräckligt mycket ledigt diskutrymme kan du utföra fullständiga säkerhetskopieringar för databasen och loggfilerna genom att använda metoder för disk-säkerhetskopiering (för att dessa diskar).</span><span class="sxs-lookup"><span data-stu-id="7a9e5-177">After you calculate to ensure enough disk space, perform full database and log backups by using disk backup methods (to those disks).</span></span> <span data-ttu-id="7a9e5-178">Över tiden, säkerhetskopiorna kopieras till ett Azure storage-konto (när du har skapat ett Azure-baserad filserver med praktiskt taget obegränsade storage) eller Använd ett Azure Backup-valv eller Azure kall storage.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-178">Over time, the backups are copied to an Azure storage account (after you set up an Azure-based file server with virtually unlimited storage), or use an Azure Backup vault or Azure cold storage.</span></span> <span data-ttu-id="7a9e5-179">Ett annat alternativ är att använda en tredje parts data protection verktyg, till exempel Commvault, där säkerhetskopiorna lagras när de har kopierats till ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-179">Another option is to use a third-party data protection tool, such as Commvault, to store the backups after they are copied to a storage account.</span></span> <span data-ttu-id="7a9e5-180">Alternativet själv säkerhetskopiering kan också krävas för data som behöver lagras under längre perioder för kompatibilitet och granskning.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-180">The DIY backup option might also be necessary for data that needs to be stored for longer periods for compliancy and auditing purposes.</span></span>
- <span data-ttu-id="7a9e5-181">Säkerhetskopiera och återställa funktioner som innehåller den underliggande infrastrukturen för SAP HANA i Azure (stora instanser).</span><span class="sxs-lookup"><span data-stu-id="7a9e5-181">Use the backup and restore functionality that the underlying infrastructure of SAP HANA on Azure (large instances) provides.</span></span> <span data-ttu-id="7a9e5-182">Det här alternativet uppfyller behovet av säkerhetskopieringar och det gör manuella säkerhetskopieringar nästan föråldrade (utom där säkerhetskopiering av data krävs för efterlevnad).</span><span class="sxs-lookup"><span data-stu-id="7a9e5-182">This option fulfills the need for backups, and it makes manual backups nearly obsolete (except where data backups are required for compliance purposes).</span></span> <span data-ttu-id="7a9e5-183">Resten av det här avsnittet adresser säkerhetskopiering och återställning av funktionerna som erbjuds med HANA (stora instanser).</span><span class="sxs-lookup"><span data-stu-id="7a9e5-183">The rest of this section addresses the backup and restore functionality that's offered with HANA (large instances).</span></span>

> [!NOTE]
> <span data-ttu-id="7a9e5-184">Snapshot-teknik som används av den underliggande infrastrukturen HANA (stora instanser) har ett beroende på SAP HANA ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-184">The snapshot technology that is used by the underlying infrastructure of HANA (large instances) has a dependency on SAP HANA snapshots.</span></span> <span data-ttu-id="7a9e5-185">SAP HANA ögonblicksbilder fungerar inte tillsammans med SAP HANA Multitenant databasen behållare.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-185">SAP HANA snapshots do not work in conjunction with SAP HANA Multitenant Database Containers.</span></span> <span data-ttu-id="7a9e5-186">Denna metod för säkerhetskopiering kan därför användas för att distribuera SAP HANA Multitenant databasen behållare.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-186">As a result, this method of backup cannot be used to deploy SAP HANA Multitenant Database Containers.</span></span>

### <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a><span data-ttu-id="7a9e5-187">Med hjälp av ögonblicksbilder för lagring av SAP HANA i Azure (stora instanser)</span><span class="sxs-lookup"><span data-stu-id="7a9e5-187">Using storage snapshots of SAP HANA on Azure (large instances)</span></span>

<span data-ttu-id="7a9e5-188">Lagringsinfrastruktur som underliggande SAP HANA i Azure (stora instanser) stöder begreppet en ögonblicksbild för lagring av volymer.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-188">The storage infrastructure underlying SAP HANA on Azure (large instances) supports the notion of a storage snapshot of volumes.</span></span> <span data-ttu-id="7a9e5-189">Både säkerhetskopiering och återställning av en viss volym stöds, med följande:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-189">Both backup and restoration of a particular volume are supported, with the following considerations:</span></span>

- <span data-ttu-id="7a9e5-190">I stället för säkerhetskopiering av databaser vidtas ögonblicksbilder av lagring regelbundet.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-190">Instead of database backups, storage volume snapshots are taken on a frequent basis.</span></span>
- <span data-ttu-id="7a9e5-191">Lagring ögonblicksbilden initierar en SAP HANA-ögonblicksbild innan ögonblicksbilden lagring körs.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-191">The storage snapshot initiates an SAP HANA snapshot before it executes the storage snapshot.</span></span> <span data-ttu-id="7a9e5-192">SAP HANA ögonblicksbilden är installationen avseende eventuell loggen återställningar efter återställningen av ögonblicksbilden lagring.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-192">This SAP HANA snapshot is the setup point for eventual log restorations after recovery of the storage snapshot.</span></span>
- <span data-ttu-id="7a9e5-193">Vid den punkt där lagring ögonblicksbilden har körts, tas SAP HANA-ögonblicksbild bort.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-193">At the point where the storage snapshot is executed successfully, the SAP HANA snapshot is deleted.</span></span>
- <span data-ttu-id="7a9e5-194">Säkerhetskopior av tas ofta och lagras i loggvolym för säkerhetskopiering eller i Azure.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-194">Log backups are taken frequently and stored in the log backup volume or in Azure.</span></span>
- <span data-ttu-id="7a9e5-195">Om databasen måste återställas till en viss punkt i tiden, skickas en begäran till Microsoft Azure-supporten (produktion avbrott) eller SAP HANA på Azure-tjänsthantering att återställa till en viss lagring ögonblicksbild (till exempel en planerad återställning av ett system med begränsat till det ursprungliga tillståndet).</span><span class="sxs-lookup"><span data-stu-id="7a9e5-195">If the database must be restored to a certain point in time, a request is made to Microsoft Azure Support (production outage) or SAP HANA on Azure Service Management to restore to a certain storage snapshot (for example, a planned restoration of a sandbox system to its original state).</span></span>
- <span data-ttu-id="7a9e5-196">SAP HANA-ögonblicksbilden som ingår i ögonblicksbilden lagring är en offset punkt för att tillämpa säkerhetskopior som har körts och lagrade när lagring ögonblicksbilden togs.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-196">The SAP HANA snapshot that's included in the storage snapshot is an offset point for applying log backups that have been executed and stored after the storage snapshot was taken.</span></span>
- <span data-ttu-id="7a9e5-197">Dessa säkerhetskopior kommer att återställa databasen till en viss punkt i tiden.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-197">These log backups are taken to restore the database back to a certain point in time.</span></span>

<span data-ttu-id="7a9e5-198">Anger säkerhetskopian\_namn skapas en ögonblicksbild av följande volymer:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-198">Specifying the backup\_name creates a snapshot of the following volumes:</span></span>

- <span data-ttu-id="7a9e5-199">Hana-data</span><span class="sxs-lookup"><span data-stu-id="7a9e5-199">hana/data</span></span>
- <span data-ttu-id="7a9e5-200">Hana/logg</span><span class="sxs-lookup"><span data-stu-id="7a9e5-200">hana/log</span></span>
- <span data-ttu-id="7a9e5-201">Hana eller in\_säkerhetskopiering (monterade som säkerhetskopia hana/loggfiler)</span><span class="sxs-lookup"><span data-stu-id="7a9e5-201">hana/log\_backup (mounted as backup into hana/log)</span></span>
- <span data-ttu-id="7a9e5-202">Hana/delat</span><span class="sxs-lookup"><span data-stu-id="7a9e5-202">hana/shared</span></span>

### <a name="storage-snapshot-considerations"></a><span data-ttu-id="7a9e5-203">Överväganden för ögonblicksbild av lagring</span><span class="sxs-lookup"><span data-stu-id="7a9e5-203">Storage snapshot considerations</span></span>

>[!NOTE]
><span data-ttu-id="7a9e5-204">Lagring ögonblicksbilderna _inte_ tillhandahålls utan kostnad, eftersom ytterligare lagringsutrymme måste allokeras.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-204">Storage snapshots are _not_ provided free of charge, because additional storage space must be allocated.</span></span>

<span data-ttu-id="7a9e5-205">Specifika säkerhetsnivån lagring ögonblicksbilder för SAP HANA i Azure (stora instanser) inkluderar:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-205">The specific mechanics of storage snapshots for SAP HANA on Azure (large instances) include:</span></span>

- <span data-ttu-id="7a9e5-206">En ögonblicksbild av en specifik lagring (vid tidpunkten när den tas) använder mycket lite lagringsutrymme.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-206">A specific storage snapshot (at the point in time when it is taken) consumes very little storage.</span></span>
- <span data-ttu-id="7a9e5-207">Ögonblicksbilden behöver lagra det ursprungliga innehållet i block som dataändringar innehåll och innehållet i filer som ändras på lagringsvolymen SAP HANA-data.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-207">As data content changes and the content in SAP HANA data files change on the storage volume, the snapshot needs to store the original block content.</span></span>
- <span data-ttu-id="7a9e5-208">Lagring ögonblicksbilden ökar i storlek.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-208">The storage snapshot increases in size.</span></span> <span data-ttu-id="7a9e5-209">Ju längre ögonblicksbilden finns, desto större blir lagring ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-209">The longer the snapshot exists, the larger the storage snapshot becomes.</span></span>
- <span data-ttu-id="7a9e5-210">Flera ändringar som gjorts till SAP HANA-databas volym under livslängden för lagring ögonblicksbild, desto större blir förbrukningen av diskutrymme för lagring ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-210">The more changes made to the SAP HANA database volume over the lifetime of a storage snapshot, the larger the space consumption of the storage snapshot becomes.</span></span>

<span data-ttu-id="7a9e5-211">SAP HANA i Azure (stora instanser) levereras med fast Volymstorlekar för SAP HANA data och loggfilen volymen.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-211">SAP HANA on Azure (large instances) comes with fixed volume sizes for the SAP HANA data and log volume.</span></span> <span data-ttu-id="7a9e5-212">Utför ögonblicksbilder av volymerna eats i utrymmet på volymen så att det är ditt ansvar att schemalägga ögonblicksbilder av lagring (inom SAP HANA på Azure [stora instanser] process).</span><span class="sxs-lookup"><span data-stu-id="7a9e5-212">Performing snapshots of those volumes eats into your volume space, so it is your responsibility to schedule storage snapshots (within the SAP HANA on Azure [large instances] process).</span></span>

<span data-ttu-id="7a9e5-213">Följande avsnitt innehåller information för att utföra dessa ögonblicksbilder, inklusive allmänna rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-213">The following sections provide information for performing these snapshots, including general recommendations:</span></span>

- <span data-ttu-id="7a9e5-214">Även om maskinvaran kan klara 255 ögonblicksbilder per volym, rekommenderar vi att du hålla betydligt lägre än detta antal.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-214">Though the hardware can sustain 255 snapshots per volume, we highly recommend that you stay well below this number.</span></span>
- <span data-ttu-id="7a9e5-215">Innan du utför lagring av ögonblicksbilder, övervaka och hålla reda på ledigt utrymme.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-215">Before you perform storage snapshots, monitor and keep track of free space.</span></span>
- <span data-ttu-id="7a9e5-216">Minska antalet lagring ögonblicksbilderna baserat på ledigt utrymme.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-216">Lower the number of storage snapshots based on free space.</span></span> <span data-ttu-id="7a9e5-217">Du kan behöva minska antalet ögonblicksbilder som du behåller eller du kan behöva utöka volymer.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-217">You might need to lower the number of snapshots that you keep, or you might need to extend the volumes.</span></span> <span data-ttu-id="7a9e5-218">(Du kan ordna ytterligare lagringsutrymme i enheter som 1 TB.)</span><span class="sxs-lookup"><span data-stu-id="7a9e5-218">(You can order additional storage in 1-TB units.)</span></span>
- <span data-ttu-id="7a9e5-219">Vi rekommenderar att du inte utför alla ögonblicksbilder för lagring under aktiviteter, till exempel data flyttas till SAP HANA med migrering Systemverktyg (R3load eller genom att återställa databaser för SAP HANA från säkerhetskopior).</span><span class="sxs-lookup"><span data-stu-id="7a9e5-219">During activities such as moving data into SAP HANA with system migration tools (with R3load, or by restoring SAP HANA databases from backups), we highly recommended that you not perform any storage snapshots.</span></span> <span data-ttu-id="7a9e5-220">(Om en system-migrering görs på ett nytt SAP HANA-system, lagring ögonblicksbilder inte behöver utföras.)</span><span class="sxs-lookup"><span data-stu-id="7a9e5-220">(If a system migration is being done on a new SAP HANA system, storage snapshots would not need to be performed.)</span></span>
- <span data-ttu-id="7a9e5-221">Under större omorganisering av SAP HANA tabeller bör lagring ögonblicksbilder undvikas om möjligt.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-221">During larger reorganizations of SAP HANA tables, storage snapshots should be avoided if possible.</span></span>
- <span data-ttu-id="7a9e5-222">Lagring ögonblicksbilder är en förutsättning för att engagera er katastrofåterställning funktionerna för SAP HANA i Azure (stora instanser).</span><span class="sxs-lookup"><span data-stu-id="7a9e5-222">Storage snapshots are a prerequisite to engaging the disaster-recovery capabilities of SAP HANA on Azure (large instances).</span></span>

### <a name="setting-up-storage-snapshots"></a><span data-ttu-id="7a9e5-223">Konfigurera lagring ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="7a9e5-223">Setting up storage snapshots</span></span>

1. <span data-ttu-id="7a9e5-224">Kontrollera att Perl är installerat i Linux-operativsystem på servern HANA (stora instanser).</span><span class="sxs-lookup"><span data-stu-id="7a9e5-224">Make sure that Perl is installed in the Linux operating system on the HANA (large instances) server.</span></span>
2. <span data-ttu-id="7a9e5-225">Ändra/etc/ssh/ssh\_config att lägga till raden _Macintosh hmac-sha1_.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-225">Modify /etc/ssh/ssh\_config to add the line _MACs hmac-sha1_.</span></span>
3. <span data-ttu-id="7a9e5-226">Skapa en SAP HANA säkerhetskopiering användarkonto på huvudnoden för varje SAP HANA-instans som du kör (om tillämpligt).</span><span class="sxs-lookup"><span data-stu-id="7a9e5-226">Create an SAP HANA backup user account on the master node for each SAP HANA instance you are running (if applicable).</span></span>
4. <span data-ttu-id="7a9e5-227">SAP HANA HDB klienten måste installeras på alla servrar för SAP HANA (stora instanser).</span><span class="sxs-lookup"><span data-stu-id="7a9e5-227">The SAP HANA HDB client must be installed on all SAP HANA (large instances) servers.</span></span>
5. <span data-ttu-id="7a9e5-228">På den första servern SAP HANA (stora instanser) för varje region, måste du skapa en offentlig nyckel för att komma åt det underliggande lagringsinfrastruktur som styr ögonblicksbilder skapas.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-228">On the first SAP HANA (large instances) server of each region, a public key must be created to access the underlying storage infrastructure that controls snapshot creation.</span></span>
6. <span data-ttu-id="7a9e5-229">Kopiera skriptet azure\_hana\_backup.pl från/skript till platsen för **hdbsql** för SAP HANA-installationen.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-229">Copy the script azure\_hana\_backup.pl from /scripts to the location of **hdbsql** of the SAP HANA installation.</span></span>
7. <span data-ttu-id="7a9e5-230">Kopiera filen HANABackupDetails.txt från/skript till samma plats som Perl-skript.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-230">Copy the HANABackupDetails.txt file from /scripts to the same location as the Perl script.</span></span>
8. <span data-ttu-id="7a9e5-231">Ändra filen HANABackupDetails.txt efter behov för önskad kund-specifikationer.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-231">Modify the HANABackupDetails.txt file as necessary for the appropriate customer specifications.</span></span>

### <a name="step-1-install-sap-hana-hdbclient"></a><span data-ttu-id="7a9e5-232">Steg 1: Installera SAP HANA HDBClient</span><span class="sxs-lookup"><span data-stu-id="7a9e5-232">Step 1: Install SAP HANA HDBClient</span></span>

<span data-ttu-id="7a9e5-233">Linux installerad på SAP HANA i Azure (stora instanser) innehåller mappar och skript som krävs för att köra ögonblicksbilder för SAP HANA-lagring för säkerhetskopiering och katastrofåterställning.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-233">The Linux installed on SAP HANA on Azure (large instances) includes the folders and scripts necessary to execute SAP HANA storage snapshots for backup and disaster-recovery purposes.</span></span> <span data-ttu-id="7a9e5-234">Dock är det ditt ansvar att installera SAP HANA HDBclient när du installerar SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-234">However, it is your responsibility to install SAP HANA HDBclient while you are installing SAP HANA.</span></span> <span data-ttu-id="7a9e5-235">(Microsoft installerar varken HDBclient eller SAP HANA.)</span><span class="sxs-lookup"><span data-stu-id="7a9e5-235">(Microsoft installs neither the HDBclient nor SAP HANA.)</span></span>

### <a name="step-2-change-etcsshsshconfig"></a><span data-ttu-id="7a9e5-236">Steg 2: Ändra/etc/ssh/ssh\_config</span><span class="sxs-lookup"><span data-stu-id="7a9e5-236">Step 2: Change /etc/ssh/ssh\_config</span></span>

<span data-ttu-id="7a9e5-237">Ändra/etc/ssh/ssh\_config genom att lägga till _Macintosh hmac-sha1_ rad som visas här:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-237">Change /etc/ssh/ssh\_config by adding _MACs hmac-sha1_ line as shown here:</span></span>
```
#   RhostsRSAAuthentication no
#   RSAAuthentication yes
#   PasswordAuthentication yes
#   HostbasedAuthentication no
#   GSSAPIAuthentication no
#   GSSAPIDelegateCredentials no
#   GSSAPIKeyExchange no
#   GSSAPITrustDNS no
#   BatchMode no
#   CheckHostIP yes
#   AddressFamily any
#   ConnectTimeout 0
#   StrictHostKeyChecking ask
#   IdentityFile ~/.ssh/identity
#   IdentityFile ~/.ssh/id_rsa
#   IdentityFile ~/.ssh/id_dsa
#   Port 22
Protocol 2
#   Cipher 3des
#   Ciphers aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-cbc,3des-cbc
#   MACs hmac-md5,hmac-sha1,umac-64@openssh.com,hmac-ripemd160
MACs hmac-sha1
#   EscapeChar ~
#   Tunnel no
#   TunnelDevice any:any
#   PermitLocalCommand no
#   VisualHostKey no
#   ProxyCommand ssh -q -W %h:%p gateway.example.com
```

### <a name="step-3-create-a-public-key"></a><span data-ttu-id="7a9e5-238">Steg 3: Skapa en offentlig nyckel</span><span class="sxs-lookup"><span data-stu-id="7a9e5-238">Step 3: Create a public key</span></span>

<span data-ttu-id="7a9e5-239">På den första SAP HANA på Azure (stora instanser)-server i varje Azure-region, skapar du en offentlig nyckel som används för att komma åt lagringsinfrastrukturen så att du kan skapa ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-239">On the first SAP HANA on Azure (large instances) server in each Azure region, create a public key to be used to access the storage infrastructure so that you can create snapshots.</span></span> <span data-ttu-id="7a9e5-240">Den offentliga nyckeln garanterar att ett lösenord inte krävs att logga in på lagring och inte bevaras lösenordsinformation.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-240">The public key ensures that a password is not required to sign in to the storage and that password credentials are not maintained.</span></span> <span data-ttu-id="7a9e5-241">Kör följande kommando för att generera den offentliga nyckeln i Linux på servern för SAP HANA (stora instanser):</span><span class="sxs-lookup"><span data-stu-id="7a9e5-241">In Linux on the SAP HANA (large instances) server, execute the following command to generate the public key:</span></span>
```
  ssh-keygen –t dsa –b 1024
```
<span data-ttu-id="7a9e5-242">Den nya platsen är _/root/.ssh/id\_dsa.pub.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-242">The new location is _/root/.ssh/id\_dsa.pub.</span></span> <span data-ttu-id="7a9e5-243">Ange inte en verklig lösenfras, eller så uppmanas du att ange lösenfrasen varje gång du loggar in.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-243">Do not enter an actual passphrase, or else you will be required to enter the passphrase each time you sign in.</span></span> <span data-ttu-id="7a9e5-244">Utan på **RETUR** två gånger för att ta bort Ange lösenfras kravet för att logga in.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-244">Instead, press **Enter** twice to remove the enter passphrase requirement for signing in.</span></span>

<span data-ttu-id="7a9e5-245">Kontrollera att den offentliga nyckeln korrigerades som förväntat genom att ändra mappar till /root/.ssh/ och sedan köra den **ls** kommando.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-245">Check to make sure that the public key was corrected as expected by changing folders to /root/.ssh/ and then executing the **ls** command.</span></span> <span data-ttu-id="7a9e5-246">Om nyckeln finns kan du kopiera den genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-246">If the key is present, you can copy it by running the following command:</span></span>

![Offentliga nyckeln kopieras genom att köra det här kommandot](./media/hana-overview-high-availability-disaster-recovery/image2-public-key.png)

<span data-ttu-id="7a9e5-248">Nu kontakta SAP HANA på Azure-tjänsthantering och ange nyckeln.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-248">At this point, contact SAP HANA on Azure Service Management and provide the key.</span></span> <span data-ttu-id="7a9e5-249">I representant använder den offentliga nyckeln för att registrera det i den underliggande lagringsinfrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-249">The service representative uses the public key to register it in the underlying storage infrastructure.</span></span>

### <a name="step-4-create-an-sap-hana-user-account"></a><span data-ttu-id="7a9e5-250">Steg 4: Skapa ett användarkonto för SAP HANA</span><span class="sxs-lookup"><span data-stu-id="7a9e5-250">Step 4: Create an SAP HANA user account</span></span>

<span data-ttu-id="7a9e5-251">Skapa ett användarkonto för SAP HANA SAP HANA Studio för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-251">Create an SAP HANA user account within SAP HANA Studio for backup purposes.</span></span> <span data-ttu-id="7a9e5-252">Det här kontot måste ha följande behörigheter: _säkerhetskopiering Admin_ och _katalog viktigt_.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-252">This account must have the following privileges: _Backup Admin_ and _Catalog Read_.</span></span> <span data-ttu-id="7a9e5-253">I det här exemplet användarnamnet SCADMIN skapas.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-253">In this example, the username SCADMIN is created.</span></span>

![Skapa en användare i HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image3-creating-user.png)

### <a name="step-5-authorize-the-sap-hana-user-account"></a><span data-ttu-id="7a9e5-255">Steg 5: Ge SAP HANA-användarkonto</span><span class="sxs-lookup"><span data-stu-id="7a9e5-255">Step 5: Authorize the SAP HANA user account</span></span>

<span data-ttu-id="7a9e5-256">Auktorisera SAP HANA-användarkonto (som ska användas av skripten utan tillstånd varje gång skriptet körs).</span><span class="sxs-lookup"><span data-stu-id="7a9e5-256">Authorize the SAP HANA user account (to be used by the scripts without requiring authorization every time the script is run).</span></span> <span data-ttu-id="7a9e5-257">Kommandot SAP HANA `hdbuserstore` kan skapas en SAP HANA användarnyckel, vilken lagras på en eller flera SAP HANA-noder.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-257">The SAP HANA command `hdbuserstore` allows the creation of an SAP HANA user key, which is stored on one or more SAP HANA nodes.</span></span> <span data-ttu-id="7a9e5-258">Nyckeln för användaren kan användare komma åt SAP HANA utan att behöva hantera lösenord från inom scripting processen som beskrivs senare.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-258">The user key also allows the user to access SAP HANA without having to manage passwords from within the scripting process that's discussed later.</span></span>

>[!IMPORTANT]
><span data-ttu-id="7a9e5-259">Kör följande kommando som `_root_`.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-259">Run the following command as `_root_`.</span></span> <span data-ttu-id="7a9e5-260">Annars fungerar skriptet inte korrekt.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-260">Otherwise, the script cannot work properly.</span></span>

<span data-ttu-id="7a9e5-261">Ange den `hdbuserstore` kommandot på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-261">Enter the `hdbuserstore` command as follows:</span></span>

![Ange kommandot hdbuserstore](./media/hana-overview-high-availability-disaster-recovery/image4-hdbuserstore-command.png)

<span data-ttu-id="7a9e5-263">I följande exempel, där användaren är SCADMIN01 och värdnamnet är lhanad01, är kommandot:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-263">In the following example, where the user is SCADMIN01 and the hostname is lhanad01, the command is:</span></span>
```
hdbuserstore set SCADMIN01 lhanad01:30115 <backup username> <password>
```
<span data-ttu-id="7a9e5-264">Hantera alla skript från en enskild server för skalbar HANA instanser.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-264">Manage all scripting from a single server for scale-out HANA instances.</span></span> <span data-ttu-id="7a9e5-265">I det här exemplet måste nyckeln för SAP HANA SCADMIN01 ändras för varje värd på ett sätt som visar värden som är relaterade till nyckeln.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-265">In this example, the SAP HANA key SCADMIN01 must be altered for each host in a way that reflects the host that is related to the key.</span></span> <span data-ttu-id="7a9e5-266">Det vill säga SAP HANA säkerhetskopiering kontot ändras med instans antalet HANA-databas **lhanad**.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-266">That is, the SAP HANA backup account is amended with the instance number of the HANA DB, **lhanad**.</span></span> <span data-ttu-id="7a9e5-267">Nyckeln måste ha administratörsbehörighet på den värd som har tilldelats och skalbar säkerhetskopiering användaren måste ha åtkomsträttigheter till alla instanser för SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-267">The key must have administrative privileges on the host it is assigned to, and the backup user for scale-out must have access rights to all SAP HANA instances.</span></span>
```
hdbuserstore set SCADMIN01 lhanad:30015 SCADMIN <password>
hdbuserstore set SCADMIN02 lhanad:30115 SCADMIN <password>
hdbuserstore set SCADMIN03 lhanad:30215 SCADMIN <password>
```

### <a name="step-6-copy-items-from-the-scripts-folder"></a><span data-ttu-id="7a9e5-268">Steg 6: Kopiera objekt från mappen/skript</span><span class="sxs-lookup"><span data-stu-id="7a9e5-268">Step 6: Copy items from the /scripts folder</span></span>

<span data-ttu-id="7a9e5-269">Kopiera följande objekt från mappen/skript (som finns på guld avbildningen av installationen) till arbetskatalog för **hdbsql**.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-269">Copy the following items from the /scripts folder (included on the gold image of the installation) to the working directory for **hdbsql**.</span></span> <span data-ttu-id="7a9e5-270">Katalogen för den aktuella HANA installationer är /hana/shared/D01/exe/linuxx86\_64/hdb.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-270">For current HANA installations, this directory is /hana/shared/D01/exe/linuxx86\_64/hdb.</span></span>
```
azure\_hana\_backup.pl
testHANAConnection.pl
testStorageSnapshotConnection.pl
removeTestStorageSnapshot.pl
HANABackupCustomerDetails.txt
```
<span data-ttu-id="7a9e5-271">Kopiera följande objekt om de använder skalbar eller OLAP:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-271">Copy the following items if they are running scale-out or OLAP:</span></span>
```
azure\_hana\_backup\_bw.pl
testHANAConnectionBW.pl
testStorageSnapshotConnectionBW.pl
removeTestStorageSnapshotBW.pl
HANABackupCustomerDetailsBW.txt
```
<span data-ttu-id="7a9e5-272">Filen HANABackupCustomerDetails.txt kan ändras på följande sätt för distribution av skala upp.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-272">The HANABackupCustomerDetails.txt file is modifiable as follows for a scale-up deployment.</span></span> <span data-ttu-id="7a9e5-273">Det är filen kontroll och konfiguration för det skript som körs storage snapshots.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-273">It is the control and configuration file for the script that runs the storage snapshots.</span></span> <span data-ttu-id="7a9e5-274">Du bör ha fått den _namn på säkerhetskopia_ och _lagring IP-adress_ från SAP HANA på Azure-tjänsthantering när dina instanser har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-274">You should have received the _Storage Backup Name_ and _Storage IP Address_ from SAP HANA on Azure Service Management when your instances were deployed.</span></span> <span data-ttu-id="7a9e5-275">Du kan inte ändra sekvensen beställning eller avstånd för någon av variablerna eller skriptet körs inte korrekt.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-275">You cannot modify the sequence, ordering, or spacing of any of the variables, or the script does not run properly.</span></span>

<span data-ttu-id="7a9e5-276">Konfigurationsfilen ser ut som en skala upp distribution:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-276">For a scale-up deployment, the configuration file would look like:</span></span>
```
#Provided by Microsoft Service Management
Storage Backup Name: lhanad01backup
Storage IP Address: 10.250.20.21
#Created by customer using hdbuserstore
HANA Backup Name: SCADMIND01
```
<span data-ttu-id="7a9e5-277">Filen HANABackupCustomerDetailsBW.txt ser ut som för en skalbar-konfiguration:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-277">For a scale-out configuration, the HANABackupCustomerDetailsBW.txt file would look like:</span></span>
```
#Provided by Microsoft Service Management
Storage Backup Name: lhanad01backup
Storage IP Address: 10.250.20.21
#Node IP addresses, instance numbers, and HANA backup name
#provided by customer.  HANA backup name created using
#hdbuserstore utility.
Node 1 IP Address: 10.254.15.21
Node 1 HANA instance number: 01
Node 1 HANA Backup Name: SCADMIN01
Node 2 IP Address: 10.254.15.22
Node 2 HANA instance number: 02
Node 2 HANA Backup Name: SCADMIN02
Node 3 IP Address: 10.254.15.23
Node 3 HANA instance number: 03
Node 3 HANA Backup Name: SCADMIN03
Node 4 IP Address: 10.254.15.24
Node 4 HANA instance number: 04
Node 4 HANA Backup Name: SCADMIN04
Node 5 IP Address: 10.254.15.25
Node 5 HANA instance number: 05
Node 5 HANA Backup Name: SCADMIN05
Node 6 IP Address: 10.254.15.26
Node 6 HANA instance number: 06
Node 6 HANA Backup Name: SCADMIN06
Node 7 IP Address: 10.254.15.27
Node 7 HANA instance number: 07
Node 7 HANA Backup Name: SCADMIN07
Node 8 IP Address: 10.254.15.28
Node 8 HANA instance number: 08
Node 8 HANA Backup Name: SCADMIN08
```
>[!NOTE]
><span data-ttu-id="7a9e5-278">Endast nod 1 information används för närvarande i skriptet faktiska HANA för lagring ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-278">Currently, only Node 1 details are used in the actual HANA storage snapshot script.</span></span> <span data-ttu-id="7a9e5-279">Vi rekommenderar att du testar åtkomst till eller från alla HANA noder så att om det ändras någonsin huvudnoden säkerhetskopiering kan du redan har kontrollerat att en annan nod kan ske dess genom att ändra informationen i nod 1.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-279">We recommend that you test access to or from all HANA nodes so that, if the master backup node ever changes, you have already ensured that any other node can take its place by modifying the details in Node 1.</span></span>

<span data-ttu-id="7a9e5-280">Om du vill söka efter rätt konfigurationerna i konfigurationsfilen eller korrekt anslutning till HANA instanser genom att köra något av följande skript:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-280">To check for the correct configurations in the configuration file or proper connectivity to the HANA instances, run either of the following scripts:</span></span>
- <span data-ttu-id="7a9e5-281">För en skala upp-konfiguration (oberoende på SAP arbetsbelastningen):</span><span class="sxs-lookup"><span data-stu-id="7a9e5-281">For a scale-up configuration (independent on SAP workload):</span></span>

 ```
testHANAConnection.pl
```
- <span data-ttu-id="7a9e5-282">För en skalbar konfiguration:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-282">For a scale-out configuration:</span></span>

 ```
testHANAConnectionBW.pl
```

<span data-ttu-id="7a9e5-283">Kontrollera att den master HANA-instansen har åtkomst till alla nödvändiga HANA-servrar.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-283">Ensure that the master HANA instance has access to all required HANA servers.</span></span> <span data-ttu-id="7a9e5-284">Det finns inga parametrar för skriptet, men du måste slutföra lämpliga HANABackupCustomerDetails / HANABackupCustomerDetailsBW filen för att skriptet ska köras.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-284">There are no parameters to the script, but you must complete the appropriate HANABackupCustomerDetails/ HANABackupCustomerDetailsBW file for the script to run properly.</span></span> <span data-ttu-id="7a9e5-285">Eftersom endast shell-kommando felkoder returneras, går det inte att kontrollera varje instans fel skript.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-285">Because only the shell command error codes are returned, it is not possible for the script to error-check every instance.</span></span> <span data-ttu-id="7a9e5-286">Skriptet ger även så vissa kommentarer att kontrollera.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-286">Even so, the script does provide some helpful comments for you to double-check.</span></span>

<span data-ttu-id="7a9e5-287">Att köra skriptet:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-287">To run the script:</span></span>
```
 ./testHANAConnection.pl
```
 <span data-ttu-id="7a9e5-288">Om skriptet erhåller har status för HANA instansen, visas ett meddelande som att HANA-anslutning har upprättats.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-288">If the script successfully obtains the status of the HANA instance, it displays a message that the HANA connection was successful.</span></span>

<span data-ttu-id="7a9e5-289">Dessutom är en andra typen av skript som du kan använda för att kontrollera master HANA instans serverns möjlighet att logga in på lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-289">Additionally, there is a second type of script you can use to check the master HANA instance server's ability to sign in to the storage.</span></span> <span data-ttu-id="7a9e5-290">Innan du kan köra azure\_hana\_säkerhetskopiering (\_bw) .pl skript, måste du köra skriptet nästa.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-290">Before you execute the azure\_hana\_backup(\_bw).pl script, you must execute the next script.</span></span> <span data-ttu-id="7a9e5-291">Om en volym innehåller inga ögonblicksbilder, det går inte att avgöra om volymen är helt enkelt tom eller om det finns en ssh gick inte att hämta information för ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-291">If a volume contains no snapshots, it is impossible to determine whether the volume is simply empty or there is an ssh failure to obtain the snapshot details.</span></span> <span data-ttu-id="7a9e5-292">Skriptet körs därför två steg:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-292">For this reason, the script executes two steps:</span></span>

- <span data-ttu-id="7a9e5-293">Verifierar att konsolen lagring är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-293">It verifies that the storage console is accessible.</span></span>
- <span data-ttu-id="7a9e5-294">En test- eller dummy, ögonblicksbild för varje volym skapas av HANA instans.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-294">It creates a test, or dummy, snapshot for each volume by HANA instance.</span></span>

<span data-ttu-id="7a9e5-295">Därför ingår HANA-instans som ett argument.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-295">For this reason, the HANA instance is included as an argument.</span></span> <span data-ttu-id="7a9e5-296">Igen, det går inte att felsökning för anslutning till lagringsplatsen, men skriptet innehåller användbara tips om körningen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-296">Again, it is not possible to provide error checking for the storage connection, but the script provides helpful hints if the execution fails.</span></span>

<span data-ttu-id="7a9e5-297">Skriptet körs som:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-297">The script is run as:</span></span>
```
 ./testStorageSnapshotConnection.pl <hana instance>
```
<span data-ttu-id="7a9e5-298">Eller den körs som:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-298">Or it is run as:</span></span>
```
./testStorageSnapshotConnectionBW.pl <hana instance>
```
<span data-ttu-id="7a9e5-299">Skriptet visar även ett meddelande om att du ska kunna logga in på rätt sätt på din distribuerade lagring-klient är organiserat efter logiska enhetsnummer (LUN) som används av server-instanser som du äger.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-299">The script also displays a message that you are able to sign in appropriately to your deployed storage tenant, which is organized around the logical unit numbers (LUNs) that are used by the server instances you own.</span></span>

<span data-ttu-id="7a9e5-300">Innan du kör de första lagring ögonblicksbild säkerhetskopieringarna, kör nästa skript och kontrollera att konfigurationen är korrekt.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-300">Before you execute the first storage snapshot-based backups, run the next scripts to make sure that the configuration is correct.</span></span>

<span data-ttu-id="7a9e5-301">När du har kört dessa skript kan du ta bort ögonblicksbilderna genom att köra:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-301">After running these scripts, you can delete the snapshots by executing:</span></span>
```
./removeTestStorageSnapshot.pl <hana instance>
```
<span data-ttu-id="7a9e5-302">Eller</span><span class="sxs-lookup"><span data-stu-id="7a9e5-302">Or</span></span>
```
./removeTestStorageSnapshot.pl <hana instance>
```

### <a name="step-7-perform-on-demand-snapshots"></a><span data-ttu-id="7a9e5-303">Steg 7: Utföra ögonblicksbilder på begäran</span><span class="sxs-lookup"><span data-stu-id="7a9e5-303">Step 7: Perform on-demand snapshots</span></span>

<span data-ttu-id="7a9e5-304">Utföra ögonblicksbilder på begäran (samt schemalägga regelbundna ögonblicksbilder med hjälp av cron) som beskrivs här.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-304">Perform on-demand snapshots (as well as schedule regular snapshots by using cron) as described here.</span></span>

<span data-ttu-id="7a9e5-305">Skala upp konfigurationer, kör du följande skript:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-305">For scale-up configurations, execute the following script:</span></span>
```
./azure_hana_backup.pl lhanad01 customer 20
```
<span data-ttu-id="7a9e5-306">Kör följande skript för skalbara konfigurationer:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-306">For scale-out configurations, execute the following script:</span></span>
```
./azure_hana_backup_bw.pl lhanad01 customer 20
```
<span data-ttu-id="7a9e5-307">Skalbar skriptet gör vissa ytterligare kontroller för att se till att alla HANA servrar kan nås och alla instanser av HANA returnera rätt status för instansen innan du fortsätter med att skapa ögonblicksbilder SAP HANA eller lagring.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-307">The scale-out script does some additional checking to make sure that all HANA servers can be accessed, and all HANA instances return appropriate status of the instance before proceeding with creating SAP HANA or storage snapshots.</span></span>

<span data-ttu-id="7a9e5-308">Följande argument är obligatoriska:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-308">The following arguments are required:</span></span>

- <span data-ttu-id="7a9e5-309">HANA instans kräver säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-309">The HANA instance requiring backup.</span></span>
- <span data-ttu-id="7a9e5-310">Snapshot-prefix för lagring ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-310">The snapshot prefix for the storage snapshot.</span></span>
- <span data-ttu-id="7a9e5-311">Antalet ögonblicksbilder som ska lagras för specifika prefix.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-311">The number of snapshots to be kept for the specific prefix.</span></span>

```
./azure_hana_backup.pl lhanad01 customer 20
```

<span data-ttu-id="7a9e5-312">Körningen av skriptet skapar ögonblicksbilden lagring i dessa tre skilda faser:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-312">The execution of the script creates the storage snapshot in these three distinct phases:</span></span>

- <span data-ttu-id="7a9e5-313">Köra en HANA ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-313">Execute a HANA snapshot.</span></span>
- <span data-ttu-id="7a9e5-314">Köra en ögonblicksbild för lagring.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-314">Execute a storage snapshot.</span></span>
- <span data-ttu-id="7a9e5-315">Ta bort ögonblicksbilden HANA.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-315">Remove the HANA snapshot.</span></span>

<span data-ttu-id="7a9e5-316">Kör skript genom att anropa från HDB körbara mappen som den kopierades till.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-316">Execute the script by calling it from the HDB executable folder that it was copied to.</span></span> <span data-ttu-id="7a9e5-317">Säkerhetskopierar minst följande volymer, men den också säkerhetskopierar alla volymer som har explicit SAP HANA-instansnamnet i namn på volymer.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-317">It backs up at least the following volumes, but it also backs up any volume that has the explicit SAP HANA instance name in the volume name.</span></span>
```
hana_data_<hana instance>_prod_t020_vol
hana_log_<hana instance>_prod_t020_vol
hana_log_backup_<hana instance>_prod_t020_vol
hana_shared_<hana instance>_prod_t020_vol
```
<span data-ttu-id="7a9e5-318">Kvarhållningsperioden administreras strikt, med antalet ögonblicksbilder som skickas som en parameter när du kör skriptet (till exempel 20 såg tidigare).</span><span class="sxs-lookup"><span data-stu-id="7a9e5-318">The retention period is strictly administered, with the number of snapshots submitted as a parameter when you execute the script (such as 20, shown previously).</span></span> <span data-ttu-id="7a9e5-319">Hur lång tid är därför en funktion av körningen och antalet ögonblicksbilder i anropet i skriptet.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-319">So the amount of time is a function of the period of execution and the number of snapshots in the call of the script.</span></span> <span data-ttu-id="7a9e5-320">Om antalet ögonblicksbilder som hålls överskrider det antal som namnges som en parameter i anropet i skriptet, äldsta lagring ögonblicksbilden av den här etiketten (i vårt fall tidigare _anpassade_) tas bort innan en ny ögonblicksbild körs.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-320">If the number of snapshots that are kept exceeds the number that are named as a parameter in the call of the script, the oldest storage snapshot of this label (in our previous case, _custom_) is deleted before a new snapshot is executed.</span></span> <span data-ttu-id="7a9e5-321">Detta innebär att antalet som den sista parametern för anropet är det tal du kan använda för att styra antalet ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-321">This means the number you give as the last parameter of the call is the number you can use to control the number of snapshots.</span></span>

>[!NOTE]
><span data-ttu-id="7a9e5-322">När du ändrar etiketten startas inventeringen igen.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-322">As soon as you change the label, the counting starts again.</span></span>

<span data-ttu-id="7a9e5-323">Du måste inkludera HANA instansnamnet som tillhandahålls av SAP HANA på Azure-tjänsthantering som ett argument om de skapar ögonblicksbilder i miljöer med flera noder.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-323">You need to include the HANA instance name that's provided by SAP HANA on Azure Service Management as an argument if they are creating snapshots in multi-node environments.</span></span> <span data-ttu-id="7a9e5-324">Namnet på SAP HANA på enheten i Azure (stora instanser) är tillräcklig i miljöer med en nod, men rekommenderas fortfarande instansnamnet HANA.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-324">In single-node environments, the name of the SAP HANA on Azure (large instances) unit is sufficient, but the HANA instance name is still recommended.</span></span>

<span data-ttu-id="7a9e5-325">Du kan dessutom säkerhetskopiera Start volumes\LUNs genom att använda samma skript.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-325">Additionally, you can back up boot volumes\LUNs by using the same script.</span></span> <span data-ttu-id="7a9e5-326">Du måste säkerhetskopiera startvolymen minst en gång när du först kör HANA, även om vi rekommenderar ett veckoschema eller nattetid schema för säkerhetskopiering för att starta i cron.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-326">You must back up your boot volume at least once when you first run HANA, although we recommend a weekly or nightly backup schedule for booting in cron.</span></span> <span data-ttu-id="7a9e5-327">Snarare än att lägga till en SAP HANA-instansnamn, infoga _Start_ som argument till skriptet på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-327">Rather than add an SAP HANA instance name, insert _boot_ as the argument into the script as follows:</span></span>
```
./azure_hana_backup boot customer 20
```
<span data-ttu-id="7a9e5-328">Samma bevarandeprincip är tillhandahållet startvolymen samt.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-328">The same retention policy is afforded to the boot volume as well.</span></span> <span data-ttu-id="7a9e5-329">Använda ögonblicksbilder på begäran, enligt beskrivningen tidigare för specialfall, som under en uppgradering av SAP förbättring av paketet (EHP) eller när du behöver skapa en ögonblicksbild av olika.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-329">Use on-demand snapshots, as described previously, for special cases only, such as during an SAP enhancement package (EHP) upgrade, or when you need to create a distinct storage snapshot.</span></span>

<span data-ttu-id="7a9e5-330">Vi rekommenderar att du kan utföra schemalagd lagring ögonblicksbilder med hjälp av cron och vi rekommenderar att du använder samma skript för alla säkerhetskopiering och katastrofåterställning behov (ändra skriptet indata som matchar de olika begärt säkerhetskopieringstiden).</span><span class="sxs-lookup"><span data-stu-id="7a9e5-330">We encourage you to perform scheduled storage snapshots using cron, and we recommend that you use the same script for all backups and disaster-recovery needs (modifying the script inputs to match the various requested backup times).</span></span> <span data-ttu-id="7a9e5-331">Dessa är alla schemalagda annorlunda i cron beroende på deras körningstid: varje timme, 12 timmar, varje dag eller varje vecka.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-331">These are all scheduled differently in cron depending on their execution time: hourly, 12-hour, daily, or weekly.</span></span> <span data-ttu-id="7a9e5-332">Cron-schemat är utformade för att skapa ögonblicksbilder av lagring som matchar tidigare vi nämnt kvarhållning etiketter för långsiktig extern säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-332">The cron schedule is designed to create storage snapshots that match the previously discussed retention labeling for long-term off-site backup.</span></span> <span data-ttu-id="7a9e5-333">Skriptet innehåller kommandon för att säkerhetskopiera alla produktionsvolymer, beroende på deras begärda frekvens (data och loggfiler säkerhetskopieras varje timme, medan startvolymen säkerhetskopieras varje dag).</span><span class="sxs-lookup"><span data-stu-id="7a9e5-333">The script includes commands to back up all production volumes, depending on their requested frequency (data and log files are backed up hourly, whereas the boot volume is backed up daily).</span></span>

<span data-ttu-id="7a9e5-334">Posterna i följande skript för cron körs varje timme vid tionde minut, var 12: e timme på tionde minut, och varje dag på tionde minut.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-334">The entries in the following cron script run every hour at the tenth minute, every 12 hours at the tenth minute, and daily at the tenth minute.</span></span> <span data-ttu-id="7a9e5-335">Cron jobb skapas så att endast en SAP HANA lagring ögonblicksbild äger rum under en viss timme så att varje timme och dagliga säkerhetskopieringar inte görs på samma gång (12:10:00).</span><span class="sxs-lookup"><span data-stu-id="7a9e5-335">The cron jobs are created in such a way that only one SAP HANA storage snapshot takes place during any particular hour, so that the hourly and daily backups do not occur at the same time (12:10 AM).</span></span> <span data-ttu-id="7a9e5-336">För att optimera din ögonblicksbilder skapas och replikering, ges SAP HANA på Azure-tjänsthantering rekommenderade tid du köra dina säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-336">To help optimize your snapshot creation and replication, SAP HANA on Azure Service Management provides the recommended time for you to run your backups.</span></span>

<span data-ttu-id="7a9e5-337">Standard-cron schemaläggning i /etc/crontab är följande:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-337">The default cron scheduling in /etc/crontab is as follows:</span></span>
```
10 1-11,13-23 * * * ./azure_hana_backup.pl lhanad01 hourly 66
10 12 * * *  ./azure_hana_backup.pl lhanad01 12hour 14
```
<span data-ttu-id="7a9e5-338">I de föregående anvisningarna i cron HANA volymer (utan startvolymen) för att hämta en varje timme ögonblicksbild med etiketten.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-338">In the previous cron instructions, the HANA volumes (without boot volume) get an hourly snapshot with the label.</span></span> <span data-ttu-id="7a9e5-339">Dessa ögonblicksbilder bevaras 66.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-339">Of these snapshots, 66 are retained.</span></span> <span data-ttu-id="7a9e5-340">Dessutom bevaras 14 ögonblicksbilder med etiketten 12-timmarsperiod.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-340">Additionally, 14 snapshots with the 12-hour label are retained.</span></span> <span data-ttu-id="7a9e5-341">Du hämta eventuellt timvis ögonblicksbilder för tre dagar, plus 12-timmarsperiod ögonblicksbilder för en annan fyra dagar, vilket ger dig en hela veckan ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-341">You potentially get hourly snapshots for three days, plus 12-hour snapshots for another four days, which gives you an entire week of snapshots.</span></span>

<span data-ttu-id="7a9e5-342">Schemalägga inom cron kan vara svårt, eftersom bara ett skript ska köras när som helst viss om skripten ut av flera minuter.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-342">Scheduling within cron can be tricky, because only one script should be executed at any particular time, unless the scripts are staggered by several minutes.</span></span> <span data-ttu-id="7a9e5-343">Om du vill daglig säkerhetskopiering för långsiktig kvarhållning kan en ögonblicksbild sparas tillsammans med en 12-timmarsperiod ögonblicksbild (med en kvarhållning antal sju varje) eller timvis ögonblicksbilden sprids ska utföras senare 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-343">If you want daily backups for long-term retention, either a daily snapshot is kept along with a 12-hour snapshot (with a retention count of seven each), or the hourly snapshot is staggered to take place 10 minutes later.</span></span> <span data-ttu-id="7a9e5-344">Endast en ögonblicksbild sparas i produktionsvolymen.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-344">Only one daily snapshot is kept in the production volume.</span></span>
```
10 1-11,13-23 * * * ./azure_hana_backup.pl lhanad01 hourly 66
10 12 * * *  ./azure_hana_backup.pl lhanad01 12hour 7
10 0 * * * ./azure_hana_backup.pl lhanad01 daily 7
```
<span data-ttu-id="7a9e5-345">Frekvenserna i den här listan är bara exempel.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-345">The frequencies listed here are only examples.</span></span> <span data-ttu-id="7a9e5-346">Använd följande kriterier för att visa din optimala antalet ögonblicksbilder:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-346">To derive your optimum number of snapshots, use the following criteria:</span></span>

- <span data-ttu-id="7a9e5-347">Krav i mål för återställning i tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-347">Requirements in recovery time objective for point-in-time recovery.</span></span>
- <span data-ttu-id="7a9e5-348">Användning av diskutrymme.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-348">Space usage.</span></span>
- <span data-ttu-id="7a9e5-349">Krav på att återställas peka mål och mål för eventuell återställning.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-349">Requirements in recovery point objective and recovery time objective for potential disaster recovery.</span></span>
- <span data-ttu-id="7a9e5-350">Eventuell körningen av HANA fullständiga databassäkerhetskopieringar mot diskar.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-350">Eventual execution of HANA full database backups against disks.</span></span> <span data-ttu-id="7a9e5-351">När en fullständig säkerhetskopiering av databasen mot diskar, eller _backint_ gränssnitt är utföra körningen av lagring ögonblicksbilder misslyckas.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-351">Whenever a full database backup against disks, or _backint_ interface, is performed, the execution of storage snapshots fails.</span></span> <span data-ttu-id="7a9e5-352">Om du planerar att köra fullständiga databassäkerhetskopieringar ovanpå lagring ögonblicksbilder, kontrollera att har körningen av lagring ögonblicksbilder inaktiverats under denna tid.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-352">If you plan to execute full database backups on top of storage snapshots, make sure that the execution of storage snapshots is disabled during this time.</span></span>

>[!IMPORTANT]
> <span data-ttu-id="7a9e5-353">Användningen av lagringsutrymme ögonblicksbilder för SAP HANA säkerhetskopior är giltigt endast när ögonblicksbilder utförs tillsammans med säkerhetskopior av SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-353">The use of storage snapshots for SAP HANA backups is valid only when the snapshots are performed in conjunction with SAP HANA log backups.</span></span> <span data-ttu-id="7a9e5-354">Dessa säkerhetskopior måste kunna omfatta tidsperioder mellan storage snapshots.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-354">These log backups need to be able to cover the time periods between the storage snapshots.</span></span> <span data-ttu-id="7a9e5-355">Om du har angett ett åtagande för användare av en punkt-in-time-återställning av 30 dagar, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-355">If you've set a commitment to users of a point-in-time recovery of 30 days, you need the following:</span></span>

- <span data-ttu-id="7a9e5-356">Möjlighet att komma åt en ögonblicksbild av lagring som är 30 dagar gamla.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-356">Ability to access a storage snapshot that is 30 days old.</span></span>
- <span data-ttu-id="7a9e5-357">Sammanhängande loggsäkerhetskopior under de senaste 30 dagarna.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-357">Contiguous log backups over the last 30 days.</span></span>

<span data-ttu-id="7a9e5-358">Skapa en ögonblicksbild av volymens loggavsnittet inom intervallet för säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-358">In the range of log backups, create a snapshot of the backup log volume as well.</span></span> <span data-ttu-id="7a9e5-359">Se dock till att utföra regelbundna säkerhetskopieringar så att du kan:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-359">However, be sure to perform regular log backups so that you can:</span></span>

- <span data-ttu-id="7a9e5-360">Ha sammanhängande loggsäkerhetskopior som behövs för att utföra i tidpunkt återställningar.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-360">Have the contiguous log backups needed to perform point-in-time recoveries.</span></span>
- <span data-ttu-id="7a9e5-361">Förhindra att loggvolymen SAP HANA utrymmet tar slut.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-361">Prevent the SAP HANA log volume from running out of space.</span></span>

<span data-ttu-id="7a9e5-362">Ett av de sista stegen är att schemalägga SAP HANA säkerhetskopieringsloggar i SAP HANA Studio.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-362">One of the last steps is to schedule SAP HANA backup logs in SAP HANA Studio.</span></span> <span data-ttu-id="7a9e5-363">Målområdet för SAP HANA loggavsnittet är särskilt skapade hana eller in\_säkerhetskopieringar volym med monteringspunkt för /hana/log/backups.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-363">The SAP HANA backup log target destination is the specially created hana/log\_backups volume with the mount point of /hana/log/backups.</span></span>

![Schemalägga SAP HANA säkerhetskopieringsloggar i SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

<span data-ttu-id="7a9e5-365">Du kan välja säkerhetskopior som är oftare än var 15: e minut.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-365">You can choose backups that are more frequent than every 15 minutes.</span></span> <span data-ttu-id="7a9e5-366">Vissa användare även utföra säkerhetskopior av varje minut, men vi inte rekommenderar kommer _över_ 15 minuter.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-366">Some users even perform log backups every minute, although we do not recommend going _over_ 15 minutes.</span></span>

<span data-ttu-id="7a9e5-367">Det sista steget är att säkerhetskopiera filbaserade (efter den första installationen av SAP HANA) om du vill skapa en enkel säkerhetskopiering posten som måste finnas i katalogen för säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-367">The final step is to perform a file-based backup (after the initial installation of SAP HANA) to create a single backup entry that must exist within the backup catalog.</span></span> <span data-ttu-id="7a9e5-368">Annars kan inte SAP HANA initiera de angivna säkerhetskopiorna.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-368">Otherwise SAP HANA cannot initiate your specified log backups.</span></span>

![Säkerhetskopiera filbaserade att skapa en enda post för säkerhetskopiering](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)

### <a name="monitoring-the-number-and-size-of-snapshots-on-the-disk-volume"></a><span data-ttu-id="7a9e5-370">Övervakning av antal och storlek ögonblicksbilder på volymen</span><span class="sxs-lookup"><span data-stu-id="7a9e5-370">Monitoring the number and size of snapshots on the disk volume</span></span>

<span data-ttu-id="7a9e5-371">Du kan övervaka antalet ögonblicksbilder och lagringförbrukningen av ögonblicksbilder på en viss lagringsvolym.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-371">On a particular storage volume, you can monitor the number of snapshots and the storage consumption of snapshots.</span></span> <span data-ttu-id="7a9e5-372">Den `ls` kommandot visas inte ögonblicksbild katalogen eller filer.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-372">The `ls` command doesn't show the snapshot directory or files.</span></span> <span data-ttu-id="7a9e5-373">Dock Linux OS-kommandot `du` matchar, med följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-373">However, the Linux OS command `du` does, with the following commands:</span></span>

- <span data-ttu-id="7a9e5-374">`du –sh .snapshot`innehåller alla ögonblicksbilder i katalogen ögonblicksbild totalt.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-374">`du –sh .snapshot` provides a total of all snapshots within the snapshot directory.</span></span>
- <span data-ttu-id="7a9e5-375">`du –sh --max-depth=1`Listar alla ögonblicksbilder som sparas i mappen .snapshot och storleken på varje ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-375">`du –sh --max-depth=1` lists all snapshots that are saved in the .snapshot folder and the size of each snapshot.</span></span>
- <span data-ttu-id="7a9e5-376">`du –hc`innehåller den totala storleken som används av alla ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-376">`du –hc` provides the total size used by all snapshots.</span></span>

<span data-ttu-id="7a9e5-377">Använd dessa kommandon för att se till att ögonblicksbilder som tas och lagras inte som förbrukar all lagring på volymer.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-377">Use these commands to make sure that the snapshots that are taken and stored are not consuming all the storage on the volumes.</span></span>

### <a name="reducing-the-number-of-snapshots-on-a-server"></a><span data-ttu-id="7a9e5-378">Minska antalet ögonblicksbilder på en server</span><span class="sxs-lookup"><span data-stu-id="7a9e5-378">Reducing the number of snapshots on a server</span></span>

<span data-ttu-id="7a9e5-379">Du kan minska antalet vissa etiketter ögonblicksbilder som du lagrar som beskrivits tidigare.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-379">As explained earlier, you can reduce the number of certain labels of snapshots that you store.</span></span> <span data-ttu-id="7a9e5-380">De två sista parametrarna för kommandot för att initiera en ögonblicksbild är en etikett och antalet ögonblicksbilder som du vill behålla.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-380">The last two parameters of the command to initiate a snapshot are a label and the number of snapshots you want to retain.</span></span>
```
./azure_hana_backup.pl lhanad01 customer 20
```
<span data-ttu-id="7a9e5-381">I det förra exemplet ögonblicksbild etiketten är _kunden_ och antalet ögonblicksbilder med den här etiketten ska behållas är _20_.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-381">In the previous example, the snapshot label is _customer_ and the number of snapshots with this label to be retained is _20_.</span></span> <span data-ttu-id="7a9e5-382">När du har besvarat förbrukningen av diskutrymme kan du vill minska antalet lagrade ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-382">As you respond to disk space consumption, you might want to reduce the number of stored snapshots.</span></span> <span data-ttu-id="7a9e5-383">Enkelt sätt att minska antalet ögonblicksbilder är att köra skriptet med den sista parametern inställd på 5:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-383">The easy way to reduce the number of snapshots is to run the script with the last parameter set to 5:</span></span>
```
./azure_hana_backup.pl lhanad01 customer 5
```
<span data-ttu-id="7a9e5-384">Skriptet som körs med den här inställningen antalet ögonblicksbilder, inklusive den nya lagring ögonblicksbilden därför _5_.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-384">As a result of running the script with this setting, the number of snapshots, including the new storage snapshot, is _5_.</span></span>

 >[!NOTE]
 > <span data-ttu-id="7a9e5-385">Det här skriptet minskar antalet ögonblicksbilder endast om den tidigare ögonblicksbilden av den senaste är mer än en timme gamla.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-385">This script reduces the number of snapshots only if the most recent previous snapshot is more than one hour old.</span></span> <span data-ttu-id="7a9e5-386">Skriptet tar inte bort ögonblicksbilderna som är mindre än en timme gamla.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-386">The script does not delete snapshots that are less than one hour old.</span></span>

<span data-ttu-id="7a9e5-387">Dessa begränsningar är relaterade till de valfria katastrofåterställning funktionerna som erbjuds.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-387">These restrictions are related to the optional disaster-recovery functionality offered.</span></span>

<span data-ttu-id="7a9e5-388">Om du inte längre vill hantera en uppsättning ögonblicksbilder med detta prefix kan du köra skriptet med _0_ kvarhållning antalet att ta bort alla ögonblicksbilder som matchar det prefixet som.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-388">If you no longer want to maintain a set of snapshots with that prefix, you can execute the script with _0_ as the retention number to remove all snapshots matching that prefix.</span></span> <span data-ttu-id="7a9e5-389">Ta bort alla ögonblicksbilder kan dock påverka funktionerna för katastrofåterställning.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-389">However, removing all snapshots can affect the capabilities of disaster recovery.</span></span>

### <a name="recovering-to-the-most-recent-hana-snapshot"></a><span data-ttu-id="7a9e5-390">Återställa till den senaste HANA ögonblicksbilden</span><span class="sxs-lookup"><span data-stu-id="7a9e5-390">Recovering to the most recent HANA snapshot</span></span>

<span data-ttu-id="7a9e5-391">I händelse av att det uppstår ett scenario för produktion och ned kan processen för att återställa från en ögonblicksbild av lagring initieras som en kundincident med SAP HANA på Azure-tjänsthantering.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-391">In the event that you experience a production-down scenario, the process of recovering from a storage snapshot can be initiated as a customer incident with SAP HANA on Azure Service Management.</span></span> <span data-ttu-id="7a9e5-392">Ett oväntat scenario kan vara en hög angelägenhetsgrad fråga om data har tagits bort i en produktionssystem och det enda sättet att hämta data är att återställa produktionsdatabasen.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-392">Such an unexpected scenario might be a high-urgency matter if data was deleted in a production system and the only way to retrieve the data is to restore the production database.</span></span>

<span data-ttu-id="7a9e5-393">Å andra sidan punkten i tidsåterställningen kan vara låg angelägenhetsgrad och planerade för dagar i förväg.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-393">On the other hand, a point-in-time recovery could be low urgency and planned for days in advance.</span></span> <span data-ttu-id="7a9e5-394">Du kan planera återställningen med SAP HANA på Azure-tjänsthantering i stället för att ett problem med hög prioritet.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-394">You can plan this recovery with SAP HANA on Azure Service Management instead of raising a high-priority issue.</span></span> <span data-ttu-id="7a9e5-395">Du kan till exempel planera försök en uppgradering av SAP-program genom att använda ett nytt paket för förbättring, och du sedan behöver återgå till en ögonblicksbild som representerar tillstånd innan uppgraderingen EHP.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-395">For example, you might be planning to try an upgrade of the SAP software by applying a new enhancement package, and you then need to revert back to a snapshot that represents the state before the EHP upgrade.</span></span>

<span data-ttu-id="7a9e5-396">Innan du skickar en begäran måste du göra vissa förberedelser.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-396">Before you issue the request, you need to do some preparation.</span></span> <span data-ttu-id="7a9e5-397">SAP HANA på Azure Service Management-teamet kan sedan hantera begäran och ge de återställda volymerna.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-397">SAP HANA on Azure Service Management team can then handle the request and provide the restored volumes.</span></span> <span data-ttu-id="7a9e5-398">Därefter kan är det du återställa HANA-databas utifrån ögonblicksbilderna.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-398">Afterward, it is up to you to restore the HANA database based on the snapshots.</span></span> <span data-ttu-id="7a9e5-399">Här är hur du förbereder för begäran:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-399">Here is how to prepare for the request:</span></span>

>[!NOTE]
><span data-ttu-id="7a9e5-400">Användargränssnittet kan skilja sig från följande skärmbilderna, beroende på SAP HANA-versionen som du använder.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-400">Your user interface might vary from the following screenshots, depending on the SAP HANA release you are using.</span></span>

1. <span data-ttu-id="7a9e5-401">Bestäm vilken ögonblicksbild för att återställa.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-401">Decide which snapshot to restore.</span></span> <span data-ttu-id="7a9e5-402">Endast hana/datavolym skulle återställas om du inte uppmanas annars.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-402">Only the hana/data volume would be restored unless you are instructed otherwise.</span></span>

2. <span data-ttu-id="7a9e5-403">Stäng HANA-instans.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-403">Shut down the HANA instance.</span></span>

 ![Stäng HANA-instans](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

3. <span data-ttu-id="7a9e5-405">Avmontera datavolymerna på varje nod för HANA-databas.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-405">Unmount the data volumes on each HANA database node.</span></span> <span data-ttu-id="7a9e5-406">Återställningen av ögonblicksbilden misslyckas om datavolymer inte är frånkopplade.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-406">The restoration of the snapshot fails if the data volumes are not unmounted.</span></span>

 ![Avmontera datavolymerna på varje nod för HANA-databas](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

4. <span data-ttu-id="7a9e5-408">Öppna en Azure-supportbegäran att instruera återställning av en specifik ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-408">Open an Azure support request to instruct the restoration of a specific snapshot.</span></span>

 - <span data-ttu-id="7a9e5-409">Under återställningen: SAP HANA på Azure-tjänsthantering kanske du ombeds att delta i ett konferenssamtal så att inga data är att gå vilse.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-409">During the restoration: SAP HANA on Azure Service Management might ask you to attend a conference call to ensure that no data is getting lost.</span></span>

 - <span data-ttu-id="7a9e5-410">Efter återställningen: SAP HANA på Azure-tjänsthantering meddelar dig när lagring ögonblicksbild har återställts.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-410">After the restoration: SAP HANA on Azure Service Management notifies you when the storage snapshot has been restored.</span></span>

5. <span data-ttu-id="7a9e5-411">Montera alla datavolymer när återställningsprocessen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-411">After the restoration process is complete, remount all data volumes.</span></span>

 ![Montera alla datavolymer](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)

6. <span data-ttu-id="7a9e5-413">Välj återställningsalternativ SAP HANA Studio, om de inte automatiskt kommer när du återansluter till HANA DB via SAP HANA Studio.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-413">Select recovery options within SAP HANA Studio, if they do not automatically come up when you reconnect to HANA DB through SAP HANA Studio.</span></span> <span data-ttu-id="7a9e5-414">I följande exempel visas en återställning till den senaste HANA ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-414">The following example shows a restoration to the last HANA snapshot.</span></span> <span data-ttu-id="7a9e5-415">En ögonblicksbild av lagring bäddar in en HANA ögonblicksbild och om du vill återställa till den senaste ögonblicksbilden av lagring, ska den senaste HANA ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-415">A storage snapshot embeds one HANA snapshot, and if you are restoring to the most recent storage snapshot, it should be the most recent HANA snapshot.</span></span> <span data-ttu-id="7a9e5-416">(Om du återställer till äldre lagring ögonblicksbilder, du måste leta upp HANA ögonblicksbilden baserat på tiden lagring ögonblicksbilden togs.)</span><span class="sxs-lookup"><span data-stu-id="7a9e5-416">(If you are restoring to older storage snapshots, you need to locate the HANA snapshot based on the time the storage snapshot was taken.)</span></span>

 ![Välj alternativ för Återställ SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image10-recover-options-a.png)

7. <span data-ttu-id="7a9e5-418">Välj **återställa databasen till en specifik säkerhetskopierings- eller ögonblicksbild**.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-418">Select **Recover the database to a specific data backup or storage snapshot**.</span></span>

 ![Fönstret ”Ange typ av återställning”](./media/hana-overview-high-availability-disaster-recovery/image11-recover-options-b.png)

8. <span data-ttu-id="7a9e5-420">Välj **ange säkerhetskopiering utan katalog**.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-420">Select **Specify backup without catalog**.</span></span>

 ![”Ange Säkerhetskopians plats”-fönstret](./media/hana-overview-high-availability-disaster-recovery/image12-recover-options-c.png)

9. <span data-ttu-id="7a9e5-422">I den **måltypen** väljer **ögonblicksbild**.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-422">In the **Destination Type** list, select **Snapshot**.</span></span>

 ![Fönstret ”Ange det säkerhetskopia att återställa”](./media/hana-overview-high-availability-disaster-recovery/image13-recover-options-d.png)

10. <span data-ttu-id="7a9e5-424">Klicka på **Slutför** att starta återställningsprocessen.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-424">Click **Finish** to start the recovery process.</span></span>

 ![Klicka på ”Slutför” om du vill starta återställningen](./media/hana-overview-high-availability-disaster-recovery/image14-recover-options-e.png)

11. <span data-ttu-id="7a9e5-426">HANA-databas är återställas och återställas till HANA ögonblicksbild som ingår i lagring ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-426">The HANA database is restored and recovered to the HANA snapshot that's included in the storage snapshot.</span></span>

 ![HANA-databas är återställas och återställas till HANA ögonblicksbild](./media/hana-overview-high-availability-disaster-recovery/image15-recover-options-f.png)

### <a name="recovering-to-the-most-recent-state"></a><span data-ttu-id="7a9e5-428">Återställa till det senaste tillståndet</span><span class="sxs-lookup"><span data-stu-id="7a9e5-428">Recovering to the most recent state</span></span>

<span data-ttu-id="7a9e5-429">Följande process återställer HANA ögonblicksbild som ingår i lagring ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-429">The following process restores the HANA snapshot that is included in the storage snapshot.</span></span> <span data-ttu-id="7a9e5-430">Den sedan återställer säkerhetskopieringarna av transaktionsloggen senaste tillståndet för databasen innan du återställer lagring ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-430">It then restores the transaction log backups to the most recent state of the database before restoring the storage snapshot.</span></span>

>[!IMPORTANT]
><span data-ttu-id="7a9e5-431">Innan du fortsätter, kontrollera att du har en fullständig och sammanhängande kedja av säkerhetskopieringar av transaktionsloggen.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-431">Before you proceed, make sure that you have a complete and contiguous chain of transaction log backups.</span></span> <span data-ttu-id="7a9e5-432">Utan dessa säkerhetskopior kan du återställa det aktuella tillståndet för databasen.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-432">Without these backups, you cannot restore the current state of the database.</span></span>

1. <span data-ttu-id="7a9e5-433">Slutför steg 1 – 6 i föregående procedur i ”återställning till senaste HANA ögonblicksbild”.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-433">Complete steps 1-6 of the preceding procedure in "Recovering to the most recent HANA snapshot."</span></span>

2. <span data-ttu-id="7a9e5-434">Välj **återställa databasen till det senaste tillståndet**.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-434">Select **Recover the database to its most recent state**.</span></span>

 ![Välj ”återställa databasen till det senaste tillståndet”](./media/hana-overview-high-availability-disaster-recovery/image16-recover-database-a.png)

3. <span data-ttu-id="7a9e5-436">Ange platsen för de senaste HANA säkerhetskopiorna.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-436">Specify the location of the most recent HANA log backups.</span></span> <span data-ttu-id="7a9e5-437">Platsen måste innehålla alla HANA säkerhetskopieringar av transaktionsloggen från HANA ögonblicksbild till det senaste tillståndet.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-437">The location needs to contain all HANA transaction log backups from the HANA snapshot to the most recent state.</span></span>

 ![Ange platsen för de senaste HANA säkerhetskopiorna](./media/hana-overview-high-availability-disaster-recovery/image17-recover-database-b.png)

4. <span data-ttu-id="7a9e5-439">Välj en säkerhetskopia som en bas som du vill återställa databasen.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-439">Select a backup as a base from which to recover the database.</span></span> <span data-ttu-id="7a9e5-440">I vårt exempel är HANA ögonblicksbild som ingick i lagring ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-440">In our example, this is the HANA snapshot that was included in the storage snapshot.</span></span> <span data-ttu-id="7a9e5-441">(Endast en ögonblicksbild finns i följande skärmbild).</span><span class="sxs-lookup"><span data-stu-id="7a9e5-441">(Only one snapshot is listed in the following screenshot.)</span></span>

 ![Välj en säkerhetskopia som en bas som du vill återställa databasen](./media/hana-overview-high-availability-disaster-recovery/image18-recover-database-c.png)

5. <span data-ttu-id="7a9e5-443">Avmarkera den **Använd Delta säkerhetskopieringar** kryssrutan om går inte finns mellan HANA ögonblicksbilder och det senaste tillståndet.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-443">Clear the **Use Delta Backups** check box if deltas do not exist between the time of the HANA snapshot and the most recent state.</span></span>

 ![Avmarkera kryssrutan ”Använd Delta säkerhetskopieringar” om det finns inga går](./media/hana-overview-high-availability-disaster-recovery/image19-recover-database-d.png)

6. <span data-ttu-id="7a9e5-445">Klicka på fönstret Sammanfattning **Slutför** att starta återställningen.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-445">On the summary screen, click **Finish** to start the restoration procedure.</span></span>

 ![Klicka på ”Slutför” på fönstret Sammanfattning](./media/hana-overview-high-availability-disaster-recovery/image20-recover-database-e.png)

### <a name="recovering-to-another-point-in-time"></a><span data-ttu-id="7a9e5-447">Återställa till en annan punkt i tiden</span><span class="sxs-lookup"><span data-stu-id="7a9e5-447">Recovering to another point in time</span></span>
<span data-ttu-id="7a9e5-448">Om du vill återställa till en punkt i tiden mellan HANA ögonblicksbilden (ingår i ögonblicksbilden av lagring) och ett som är senare än HANA ögonblicksbild point-in-time-återställning, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-448">To recover to a point in time between the HANA snapshot (included in the storage snapshot) and one that is later than the HANA snapshot point-in-time recovery, do the following:</span></span>

1. <span data-ttu-id="7a9e5-449">Kontrollera att du har alla säkerhetskopieringar av transaktionsloggen från HANA ögonblicksbild till den tid som du vill återställa.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-449">Make sure that you have all transaction log backups from the HANA snapshot to the time you want to recover.</span></span>
2. <span data-ttu-id="7a9e5-450">Börja under ”återställning till det senaste tillståndet”.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-450">Begin the procedure under "Recovering to the most recent state."</span></span>
3. <span data-ttu-id="7a9e5-451">I steg 2 i proceduren i det **ange återställningstyp** väljer **återställa databasen till följande punkt i tiden**anger punkten i tiden och utför steg 3-6.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-451">In step 2 of the procedure, in the **Specify Recovery Type** window, select **Recover the database to the following point in time**, specify the point in time, and then complete steps 3-6.</span></span>

## <a name="monitoring-the-execution-of-snapshots"></a><span data-ttu-id="7a9e5-452">Övervakning av körningen av ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="7a9e5-452">Monitoring the execution of snapshots</span></span>

<span data-ttu-id="7a9e5-453">Du behöver övervaka körning av lagring ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-453">You need to monitor the execution of storage snapshots.</span></span> <span data-ttu-id="7a9e5-454">Skriptet som kör en ögonblicksbild av lagring skriver utdata till en fil och sparar den på samma plats som Perl-skript.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-454">The script that executes a storage snapshot writes output to a file and then saves it to the same location as the Perl scripts.</span></span> <span data-ttu-id="7a9e5-455">En separat fil skrivs för varje ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-455">A separate file is written for each snapshot.</span></span> <span data-ttu-id="7a9e5-456">Utdata från varje fil visar de olika faserna att ögonblicksbild skriptet körs:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-456">The output of each file clearly shows the various phases that the snapshot script executes:</span></span>

- <span data-ttu-id="7a9e5-457">Hitta de volymer som behöver skapa en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="7a9e5-457">Finding the volumes that need to create a snapshot</span></span>
- <span data-ttu-id="7a9e5-458">Hitta ögonblicksbilder tas från dessa volymer</span><span class="sxs-lookup"><span data-stu-id="7a9e5-458">Finding the snapshots taken from these volumes</span></span>
- <span data-ttu-id="7a9e5-459">Ta bort eventuell befintlig ögonblicksbilder för att matcha antalet ögonblicksbilder som du har angett</span><span class="sxs-lookup"><span data-stu-id="7a9e5-459">Deleting eventual existing snapshots to match the number of snapshots you specified</span></span>
- <span data-ttu-id="7a9e5-460">Skapa en ögonblicksbild av HANA</span><span class="sxs-lookup"><span data-stu-id="7a9e5-460">Creating a HANA snapshot</span></span>
- <span data-ttu-id="7a9e5-461">Skapa ögonblicksbilden lagring över volymer</span><span class="sxs-lookup"><span data-stu-id="7a9e5-461">Creating the storage snapshot over the volumes</span></span>
- <span data-ttu-id="7a9e5-462">När du raderar ögonblicksbilden HANA</span><span class="sxs-lookup"><span data-stu-id="7a9e5-462">Deleting the HANA snapshot</span></span>
- <span data-ttu-id="7a9e5-463">Byta namn på den senaste ögonblicksbilden på **.0**</span><span class="sxs-lookup"><span data-stu-id="7a9e5-463">Renaming the most recent snapshot to **.0**</span></span>

<span data-ttu-id="7a9e5-464">Den viktigaste delen av skriptet är:</span><span class="sxs-lookup"><span data-stu-id="7a9e5-464">The most important part of the script is this:</span></span>
```
**********************Creating HANA snapshot**********************
Creating the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data create snapshot"" ...
HANA snapshot created successfully.
**********************Creating Storage snapshot**********************
Taking snapshot hourly.recent for hana_data_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_backup_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_shared_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for sapmnt_lhanad01_t020_vol ...
Snapshot created successfully.
**********************Deleting HANA snapshot**********************
Deleting the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data drop snapshot"" ...
HANA snapshot deletion successfully.
```
<span data-ttu-id="7a9e5-465">Från det här exemplet kan du se hur skriptet poster skapandet av HANA ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-465">You can see from this sample how the script records the creation of the HANA snapshot.</span></span> <span data-ttu-id="7a9e5-466">I fallet med skalbara måste initieras den här processen på huvudnoden.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-466">In the scale-out case, this process is initiated on the master node.</span></span> <span data-ttu-id="7a9e5-467">Huvudnoden initierar synkron genereringen av ögonblicksbilder på varje worker-nod.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-467">The master node initiates the synchronous creation of the snapshots on each of the worker nodes.</span></span> <span data-ttu-id="7a9e5-468">Sedan tas lagring ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-468">Then the storage snapshot is taken.</span></span> <span data-ttu-id="7a9e5-469">När du har körts lagring ögonblicksbilder HANA ögonblicksbilden har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="7a9e5-469">After the successful execution of the storage snapshots, the HANA snapshot is deleted.</span></span>
