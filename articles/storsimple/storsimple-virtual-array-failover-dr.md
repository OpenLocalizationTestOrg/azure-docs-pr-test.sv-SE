---
title: StorSimple virtuell matris disaster recovery och enheten redundans | Microsoft Docs
description: Mer information om hur du redundans din virtuella StorSimple-matris.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 3c1f9c62-af57-4634-a0d8-435522d969aa
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 12079f8dbc409afe5acc274fa08bda878c90b76e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="disaster-recovery-and-device-failover-for-your-storsimple-virtual-array-via-azure-portal"></a><span data-ttu-id="83602-103">Disaster recovery och enheten redundans för din virtuella StorSimple-matrisen via Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="83602-103">Disaster recovery and device failover for your StorSimple Virtual Array via Azure portal</span></span>

## <a name="overview"></a><span data-ttu-id="83602-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="83602-104">Overview</span></span>
<span data-ttu-id="83602-105">Den här artikeln beskriver katastrofåterställning för din Microsoft Azure StorSimple virtuell matrisen inklusive detaljerade instruktioner att växla över till en annan virtuell matris.</span><span class="sxs-lookup"><span data-stu-id="83602-105">This article describes the disaster recovery for your Microsoft Azure StorSimple Virtual Array including the detailed steps to fail over to another virtual array.</span></span> <span data-ttu-id="83602-106">En redundansväxling kan du flytta data från en *källa* enheten i datacentret till en *mål* enhet.</span><span class="sxs-lookup"><span data-stu-id="83602-106">A failover allows you to move your data from a *source* device in the datacenter to a *target* device.</span></span> <span data-ttu-id="83602-107">Målenheten kan finnas i samma eller en annan geografisk plats.</span><span class="sxs-lookup"><span data-stu-id="83602-107">The target device may be located in the same or a different geographical location.</span></span> <span data-ttu-id="83602-108">Enhet för växling vid fel är för hela enheten.</span><span class="sxs-lookup"><span data-stu-id="83602-108">The device failover is for the entire device.</span></span> <span data-ttu-id="83602-109">Under växling vid fel ändras molndata för källan ägarskap till som målenheten.</span><span class="sxs-lookup"><span data-stu-id="83602-109">During failover, the cloud data for the source device changes ownership to that of the target device.</span></span>

<span data-ttu-id="83602-110">Den här artikeln gäller bara för virtuella StorSimple-matriser.</span><span class="sxs-lookup"><span data-stu-id="83602-110">This article is applicable to StorSimple Virtual Arrays only.</span></span> <span data-ttu-id="83602-111">Om du vill växla över en 8000-serieenhet, gå till [enheten redundans och disaster recovery av StorSimple-enheten](storsimple-device-failover-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="83602-111">To fail over an 8000 series device, go to [Device failover and disaster recovery of your StorSimple device](storsimple-device-failover-disaster-recovery.md).</span></span>

## <a name="what-is-disaster-recovery-and-device-failover"></a><span data-ttu-id="83602-112">Vad är disaster recovery och enheten redundans?</span><span class="sxs-lookup"><span data-stu-id="83602-112">What is disaster recovery and device failover?</span></span>

<span data-ttu-id="83602-113">I en (DR) katastrofåterställning, den primära enheten slutar fungera.</span><span class="sxs-lookup"><span data-stu-id="83602-113">In a disaster recovery (DR) scenario, the primary device stops functioning.</span></span> <span data-ttu-id="83602-114">I det här scenariot kan du flytta molndata som är associerade med den misslyckade enheten till en annan enhet.</span><span class="sxs-lookup"><span data-stu-id="83602-114">In this scenario, you can move the cloud data associated with the failed device to another device.</span></span> <span data-ttu-id="83602-115">Du kan använda den primära enheten som den *källa* och ange en annan enhet som den *mål*.</span><span class="sxs-lookup"><span data-stu-id="83602-115">You can use the primary device as the *source* and specify another device as the *target*.</span></span> <span data-ttu-id="83602-116">Den här processen kallas den *redundans*.</span><span class="sxs-lookup"><span data-stu-id="83602-116">This process is referred to as the *failover*.</span></span> <span data-ttu-id="83602-117">Ändra ägarskap och överförs till målenheten under växling vid fel, alla volymer eller resurser från källan.</span><span class="sxs-lookup"><span data-stu-id="83602-117">During failover, all the volumes or the shares from the source device change ownership and are transferred to the target device.</span></span> <span data-ttu-id="83602-118">Ingen filtrering av data är tillåten.</span><span class="sxs-lookup"><span data-stu-id="83602-118">No filtering of the data is allowed.</span></span>

<span data-ttu-id="83602-119">DR modelleras som en fullständig enhet återställning med hjälp av den termiska karta-baserade skiktning och spårning.</span><span class="sxs-lookup"><span data-stu-id="83602-119">DR is modeled as a full device restore using the heat map–based tiering and tracking.</span></span> <span data-ttu-id="83602-120">En termisk karta definieras genom att tilldela värdet termiska data baserat på läsa och skriva mönster.</span><span class="sxs-lookup"><span data-stu-id="83602-120">A heat map is defined by assigning a heat value to the data based on read and write patterns.</span></span> <span data-ttu-id="83602-121">Den här termiska mappa sedan nivåer lägsta värme-datasegment till molnet först samtidigt som datasegment hög värme (används mest) på den lokala nivån.</span><span class="sxs-lookup"><span data-stu-id="83602-121">This heat map then tiers the lowest heat data chunks to the cloud first while keeping the high heat (most used) data chunks in the local tier.</span></span> <span data-ttu-id="83602-122">Under en Katastrofåterställning använder StorSimple termisk karta för att återställa och rehydrate data från molnet.</span><span class="sxs-lookup"><span data-stu-id="83602-122">During a DR, StorSimple uses the heat map to restore and rehydrate the data from the cloud.</span></span> <span data-ttu-id="83602-123">Enheten hämtar alla volymer/resurser i den senaste senaste säkerhetskopieringen (vilket anges internt) och utför en återställning från säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="83602-123">The device fetches all the volumes/shares in the last recent backup (as determined internally) and performs a restore from that backup.</span></span> <span data-ttu-id="83602-124">Virtuella matrisen samordnar hela DR-processen.</span><span class="sxs-lookup"><span data-stu-id="83602-124">The virtual array orchestrates the entire DR process.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="83602-125">På källenheten tas bort i slutet av enheten redundans och därför en återställning stöds inte.</span><span class="sxs-lookup"><span data-stu-id="83602-125">The source device is deleted at the end of device failover and hence a failback is not supported.</span></span>
> 
> 

<span data-ttu-id="83602-126">Katastrofåterställning är samordnade via funktionen med redundans i enheten och initieras från den **enheter** bladet.</span><span class="sxs-lookup"><span data-stu-id="83602-126">Disaster recovery is orchestrated via the device failover feature and is initiated from the **Devices** blade.</span></span> <span data-ttu-id="83602-127">Det här bladet tabulates alla StorSimple-enheter som är anslutna till din StorSimple Device Manager-tjänst.</span><span class="sxs-lookup"><span data-stu-id="83602-127">This blade tabulates all the StorSimple devices connected to your StorSimple Device Manager service.</span></span> <span data-ttu-id="83602-128">Du kan se eget namn, status, etablerad och maximal kapacitet, typ och modell för varje enhet.</span><span class="sxs-lookup"><span data-stu-id="83602-128">For each device, you can see the friendly name, status, provisioned and maximum capacity, type, and model.</span></span>

## <a name="prerequisites-for-device-failover"></a><span data-ttu-id="83602-129">Krav för växling vid fel för enheten</span><span class="sxs-lookup"><span data-stu-id="83602-129">Prerequisites for device failover</span></span>

### <a name="prerequisites"></a><span data-ttu-id="83602-130">Krav</span><span class="sxs-lookup"><span data-stu-id="83602-130">Prerequisites</span></span>

<span data-ttu-id="83602-131">För en enhet växling, kontrollerar du att följande krav är uppfyllda:</span><span class="sxs-lookup"><span data-stu-id="83602-131">For a device failover, ensure that the following prerequisites are satisfied:</span></span>

* <span data-ttu-id="83602-132">Källan måste finnas i en **inaktiverad** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="83602-132">The source device needs to be in a **Deactivated** state.</span></span>
* <span data-ttu-id="83602-133">Målenheten måste visas som **redo att konfigurera** i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="83602-133">The target device needs to show up as **Ready to set up** in the Azure portal.</span></span> <span data-ttu-id="83602-134">Etablera en virtuell Målmatrisen av samma eller högre kapacitet.</span><span class="sxs-lookup"><span data-stu-id="83602-134">Provision a target virtual array of the same or higher capacity.</span></span> <span data-ttu-id="83602-135">Använda lokala webbgränssnittet för att konfigurera och registrera har virtuella Målmatrisen.</span><span class="sxs-lookup"><span data-stu-id="83602-135">Use the local web UI to configure and successfully register the target virtual array.</span></span>
  
  > [!IMPORTANT]
  > <span data-ttu-id="83602-136">Försök inte konfigurera den registrerade virtuella enheten via tjänsten.</span><span class="sxs-lookup"><span data-stu-id="83602-136">Do not attempt to configure the registered virtual device through the service.</span></span> <span data-ttu-id="83602-137">Ingen enhetskonfiguration ska utföras via tjänsten.</span><span class="sxs-lookup"><span data-stu-id="83602-137">No device configuration should be performed through the service.</span></span>
  > 
  > 
* <span data-ttu-id="83602-138">Målenheten kan inte ha samma namn som källan.</span><span class="sxs-lookup"><span data-stu-id="83602-138">The target device cannot have the same name as the source device.</span></span>
* <span data-ttu-id="83602-139">Käll- och enheten måste vara av samma typ.</span><span class="sxs-lookup"><span data-stu-id="83602-139">The source and target device have to be the same type.</span></span> <span data-ttu-id="83602-140">Du kan bara redundansväxla virtuella matriskonfiguration som en filserver till en annan filserver.</span><span class="sxs-lookup"><span data-stu-id="83602-140">You can only fail over a virtual array configured as a file server to another file server.</span></span> <span data-ttu-id="83602-141">Detsamma gäller för en iSCSI-server.</span><span class="sxs-lookup"><span data-stu-id="83602-141">The same is true for an iSCSI server.</span></span>
* <span data-ttu-id="83602-142">För en filserver DR rekommenderar vi att du ansluter målenheten till samma domän som källa.</span><span class="sxs-lookup"><span data-stu-id="83602-142">For a file server DR, we recommend that you join the target device to the same domain as the source.</span></span> <span data-ttu-id="83602-143">Den här konfigurationen garanterar att behörigheter till resursen löses automatiskt.</span><span class="sxs-lookup"><span data-stu-id="83602-143">This configuration ensures that the share permissions are automatically resolved.</span></span> <span data-ttu-id="83602-144">Endast redundans till en målenhet i samma domän.</span><span class="sxs-lookup"><span data-stu-id="83602-144">Only the failover to a target device in the same domain.</span></span>
* <span data-ttu-id="83602-145">De tillgängliga målenheterna för Katastrofåterställning är enheter som har samma eller större kapacitet jämfört med källan.</span><span class="sxs-lookup"><span data-stu-id="83602-145">The available target devices for DR are devices that have the same or larger capacity compared to the source device.</span></span> <span data-ttu-id="83602-146">De enheter som är anslutna till din tjänst men inte uppfyller villkoren för tillräckligt med utrymme är inte tillgängliga som målenheter.</span><span class="sxs-lookup"><span data-stu-id="83602-146">The devices that are connected to your service but do not meet the criteria of sufficient space are not available as target devices.</span></span>

### <a name="other-considerations"></a><span data-ttu-id="83602-147">Andra överväganden</span><span class="sxs-lookup"><span data-stu-id="83602-147">Other considerations</span></span>

* <span data-ttu-id="83602-148">För en planerad redundans</span><span class="sxs-lookup"><span data-stu-id="83602-148">For a planned failover</span></span> 
  
  * <span data-ttu-id="83602-149">Vi rekommenderar att du vidtar alla volymer eller resurser på källenheten offline.</span><span class="sxs-lookup"><span data-stu-id="83602-149">We recommend that you take all the volumes or shares on the source device offline.</span></span>
  * <span data-ttu-id="83602-150">Vi rekommenderar att du gör en säkerhetskopia av enheten och fortsätt sedan med växling vid fel för att minimera dataförlust.</span><span class="sxs-lookup"><span data-stu-id="83602-150">We recommend that you take a backup of the device and then proceed with the failover to minimize data loss.</span></span> 
* <span data-ttu-id="83602-151">En oplanerad redundans med enheten använder den senaste säkerhetskopian för att återställa data.</span><span class="sxs-lookup"><span data-stu-id="83602-151">For an unplanned failover, the device uses the most recent backup to restore the data.</span></span>

### <a name="device-failover-prechecks"></a><span data-ttu-id="83602-152">Enheten redundans prechecks</span><span class="sxs-lookup"><span data-stu-id="83602-152">Device failover prechecks</span></span>

<span data-ttu-id="83602-153">Innan DR börjar utför prechecks på enheten.</span><span class="sxs-lookup"><span data-stu-id="83602-153">Before the DR begins, the device performs prechecks.</span></span> <span data-ttu-id="83602-154">Dessa kontroller att säkerställa att inga fel inträffar när DR påbörjas.</span><span class="sxs-lookup"><span data-stu-id="83602-154">These checks help ensure that no errors occur when DR commences.</span></span> <span data-ttu-id="83602-155">Prechecks omfattar:</span><span class="sxs-lookup"><span data-stu-id="83602-155">The prechecks include:</span></span>

* <span data-ttu-id="83602-156">Verifiera lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="83602-156">Validating the storage account.</span></span>
* <span data-ttu-id="83602-157">Kontrollerar anslutningen molnet till Azure.</span><span class="sxs-lookup"><span data-stu-id="83602-157">Checking the cloud connectivity to Azure.</span></span>
* <span data-ttu-id="83602-158">Kontrollerar diskutrymme på målenheten.</span><span class="sxs-lookup"><span data-stu-id="83602-158">Checking available space on the target device.</span></span>
* <span data-ttu-id="83602-159">Kontrollerar om en iSCSI-server enheten källvolymen har</span><span class="sxs-lookup"><span data-stu-id="83602-159">Checking if an iSCSI server source device volume has</span></span>
  
  * <span data-ttu-id="83602-160">giltiga ACR-namn.</span><span class="sxs-lookup"><span data-stu-id="83602-160">valid ACR names.</span></span>
  * <span data-ttu-id="83602-161">giltig IQN (högst 220 tecken).</span><span class="sxs-lookup"><span data-stu-id="83602-161">valid IQN (not exceeding 220 characters).</span></span>
  * <span data-ttu-id="83602-162">giltig CHAP-lösenord (12-16 tecken lång).</span><span class="sxs-lookup"><span data-stu-id="83602-162">valid CHAP passwords (12-16 characters long).</span></span>

<span data-ttu-id="83602-163">Du kan inte fortsätta med DR om något av föregående prechecks misslyckas.</span><span class="sxs-lookup"><span data-stu-id="83602-163">If any of the preceding prechecks fail, you cannot proceed with the DR.</span></span> <span data-ttu-id="83602-164">Lösa dessa problem och försök sedan DR.</span><span class="sxs-lookup"><span data-stu-id="83602-164">Resolve those issues and then retry DR.</span></span>

<span data-ttu-id="83602-165">När ar har slutförts överförs ägarskapet till molndata på källan till målenheten.</span><span class="sxs-lookup"><span data-stu-id="83602-165">After the DR is successfully completed, the ownership of the cloud data on the source device is transferred to the target device.</span></span> <span data-ttu-id="83602-166">Källenhet är sedan inte längre i portalen.</span><span class="sxs-lookup"><span data-stu-id="83602-166">The source device is then no longer available in the portal.</span></span> <span data-ttu-id="83602-167">Blockeras åtkomst till alla volymer/resurser på källan och målenheten blir aktiv.</span><span class="sxs-lookup"><span data-stu-id="83602-167">Access to all the volumes/shares on the source device is blocked and the target device becomes active.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="83602-168">Om enheten är inte längre tillgänglig, förbrukar den virtuella datorn som du har etablerats på värdsystemet fortfarande resurser.</span><span class="sxs-lookup"><span data-stu-id="83602-168">Though the device is no longer available, the virtual machine that you provisioned on the host system is still consuming resources.</span></span> <span data-ttu-id="83602-169">När ar har slutförts kan du ta bort den här virtuella datorn från värdsystemet.</span><span class="sxs-lookup"><span data-stu-id="83602-169">Once the DR is successfully complete, you can delete this virtual machine from your host system.</span></span>
> 
> 

## <a name="fail-over-to-a-virtual-array"></a><span data-ttu-id="83602-170">Växla över till en virtuell matris</span><span class="sxs-lookup"><span data-stu-id="83602-170">Fail over to a virtual array</span></span>

<span data-ttu-id="83602-171">Vi rekommenderar att etablera, konfigurera och registrera en annan virtuell StorSimple-matris med din StorSimple Device Manager-tjänsten innan du kör den här proceduren.</span><span class="sxs-lookup"><span data-stu-id="83602-171">We recommend that you provision, configure, and register another StorSimple Virtual Array with your StorSimple Device Manager service before you run this procedure.</span></span>

> [!IMPORTANT]
> 
> * <span data-ttu-id="83602-172">Du kan inte växla över från en StorSimple 8000-serieenhet till en virtuell enhet 1200.</span><span class="sxs-lookup"><span data-stu-id="83602-172">You cannot fail over from a StorSimple 8000 series device to a 1200 virtual device.</span></span>
> * <span data-ttu-id="83602-173">Du kan växla över från en virtuell enhet FIPS Federal Information Processing Standard () aktiverade till en annan FIPS-aktiverad enhet eller till en icke-FIPS-enhet som har distribuerats i Government-portalen.</span><span class="sxs-lookup"><span data-stu-id="83602-173">You can fail over from a Federal Information Processing Standard (FIPS) enabled virtual device to another FIPS enabled device or to a non-FIPS device deployed in the Government portal.</span></span>


<span data-ttu-id="83602-174">Utför följande steg för att återställa enheten till en mål virtuella StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="83602-174">Perform the following steps to restore the device to a target StorSimple virtual device.</span></span>

1. <span data-ttu-id="83602-175">Etablera och konfigurera en målenhet som uppfyller den [krav för växling vid fel för enheten](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="83602-175">Provision and configure a target device that meets the [prerequisites for device failover](#prerequisites).</span></span> <span data-ttu-id="83602-176">Slutför enhetskonfiguration via lokala webbgränssnittet och registrera till din StorSimple Device Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="83602-176">Complete the device configuration via the local web UI and register it to your StorSimple Device Manager service.</span></span> <span data-ttu-id="83602-177">Om du skapar en filserver, går du till steg 1 av [som filserver](storsimple-virtual-array-deploy3-fs-setup.md#step-1-complete-the-local-web-ui-setup-and-register-your-device).</span><span class="sxs-lookup"><span data-stu-id="83602-177">If creating a file server, go to step 1 of [set up as file server](storsimple-virtual-array-deploy3-fs-setup.md#step-1-complete-the-local-web-ui-setup-and-register-your-device).</span></span> <span data-ttu-id="83602-178">Om du skapar en iSCSI-server går du till steg 1 av [konfigurerats som server för iSCSI-](storsimple-virtual-array-deploy3-iscsi-setup.md#step-1-complete-the-local-web-ui-setup-and-register-your-device).</span><span class="sxs-lookup"><span data-stu-id="83602-178">If creating an iSCSI server, go to step 1 of [set up as iSCSI server](storsimple-virtual-array-deploy3-iscsi-setup.md#step-1-complete-the-local-web-ui-setup-and-register-your-device).</span></span>

2. <span data-ttu-id="83602-179">Ta volymer/resurser offline på värden.</span><span class="sxs-lookup"><span data-stu-id="83602-179">Take volumes/shares offline on the host.</span></span> <span data-ttu-id="83602-180">Om du vill koppla volymer/resurser finns i operativsystemet – specifika instruktionerna för värden.</span><span class="sxs-lookup"><span data-stu-id="83602-180">To take the volumes/shares offline, refer to the operating system–specific instructions for the host.</span></span> <span data-ttu-id="83602-181">Om det inte redan offline, måste du ta alla volymer/resurser offline på enheten genom att göra följande.</span><span class="sxs-lookup"><span data-stu-id="83602-181">If not already offline, you need to take all the volumes/shares offline on the device by doing the following.</span></span>
   
    1. <span data-ttu-id="83602-182">Gå till **enheter** bladet och välj din enhet.</span><span class="sxs-lookup"><span data-stu-id="83602-182">Go to **Devices** blade and select your device.</span></span>
   
    2. <span data-ttu-id="83602-183">Gå till **Inställningar > Hantera > resurser** (eller **Inställningar > Hantera > volymer**).</span><span class="sxs-lookup"><span data-stu-id="83602-183">Go to **Settings > Manage > Shares** (or **Settings > Manage > Volumes**).</span></span> 
   
    3. <span data-ttu-id="83602-184">Välj en resurs/volym, högerklicka och välj **ta offline**.</span><span class="sxs-lookup"><span data-stu-id="83602-184">Select a share/volume, right click and select **Take offline**.</span></span> 
   
    4. <span data-ttu-id="83602-185">När du uppmanas att bekräfta att kontrollera **jag förstår konsekvenserna av att ta resursen offline.**</span><span class="sxs-lookup"><span data-stu-id="83602-185">When prompted for confirmation, check **I understand the impact of taking this share offline.**</span></span> 
   
    5. <span data-ttu-id="83602-186">Klicka på **ta offline**.</span><span class="sxs-lookup"><span data-stu-id="83602-186">Click **Take offline**.</span></span>

3. <span data-ttu-id="83602-187">I Enhetshanteraren för StorSimple-tjänsten går du till **Management > enheter**.</span><span class="sxs-lookup"><span data-stu-id="83602-187">In your StorSimple Device Manager service, go to **Management > Devices**.</span></span> <span data-ttu-id="83602-188">I den **enheter** bladet Välj och klicka på din källenhet.</span><span class="sxs-lookup"><span data-stu-id="83602-188">In the **Devices** blade, select and click your source device.</span></span>

4. <span data-ttu-id="83602-189">I din **enheten instrumentpanelen** bladet, klickar du på **inaktivera**.</span><span class="sxs-lookup"><span data-stu-id="83602-189">In your **Device dashboard** blade, click **Deactivate**.</span></span>

5. <span data-ttu-id="83602-190">I den **inaktivera** bladet du uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="83602-190">In the **Deactivate** blade, you are prompted for confirmation.</span></span> <span data-ttu-id="83602-191">Inaktivering av enheten är en *permanent* process som inte kan ångras.</span><span class="sxs-lookup"><span data-stu-id="83602-191">Device deactivation is a *permanent* process that cannot be undone.</span></span> <span data-ttu-id="83602-192">Du har också en påminnelse om att ta dina resurser/volymer offline på värden.</span><span class="sxs-lookup"><span data-stu-id="83602-192">You are also reminded to take your shares/volumes offline on the host.</span></span> <span data-ttu-id="83602-193">Skriv enhetsnamnet för att bekräfta och klicka på **inaktivera**.</span><span class="sxs-lookup"><span data-stu-id="83602-193">Type the device name to confirm and click **Deactivate**.</span></span>
   
    ![](./media/storsimple-virtual-array-failover-dr/failover1.png)
6. <span data-ttu-id="83602-194">Inaktiveringen startar.</span><span class="sxs-lookup"><span data-stu-id="83602-194">The deactivation starts.</span></span> <span data-ttu-id="83602-195">Du får ett meddelande när inaktiveringen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="83602-195">You will receive a notification after the deactivation is successfully completed.</span></span>
   
    ![](./media/storsimple-virtual-array-failover-dr/failover2.png)
7. <span data-ttu-id="83602-196">Enhetens tillstånd ändras på sidan enheter nu till **inaktiverad**.</span><span class="sxs-lookup"><span data-stu-id="83602-196">On the Devices page, the device state will now change to **Deactivated**.</span></span>
    ![](./media/storsimple-virtual-array-failover-dr/failover3.png)
8. <span data-ttu-id="83602-197">I den **enheter** bladet Välj och klicka på inaktiverad källenheten för redundans.</span><span class="sxs-lookup"><span data-stu-id="83602-197">In the **Devices** blade, select and click the deactivated source device for failover.</span></span> 
9. <span data-ttu-id="83602-198">I den **enheten instrumentpanelen** bladet, klickar du på **växla över**.</span><span class="sxs-lookup"><span data-stu-id="83602-198">In the **Device dashboard** blade, click **Fail over**.</span></span> 
10. <span data-ttu-id="83602-199">I den **växla över enhet** bladet gör du följande:</span><span class="sxs-lookup"><span data-stu-id="83602-199">In the **Fail over device** blade, do the following:</span></span>
    
    1. <span data-ttu-id="83602-200">Källan enhetens fält fylls i automatiskt.</span><span class="sxs-lookup"><span data-stu-id="83602-200">The source device field is automatically populated.</span></span> <span data-ttu-id="83602-201">Observera den totala datastorleken för källan.</span><span class="sxs-lookup"><span data-stu-id="83602-201">Note the total data size for the source device.</span></span> <span data-ttu-id="83602-202">Storleken på data ska vara mindre än den tillgängliga kapaciteten på målenheten.</span><span class="sxs-lookup"><span data-stu-id="83602-202">The data size should be lesser than the available capacity on the target device.</span></span> <span data-ttu-id="83602-203">Granska informationen som är associerade med källenheten som enhetsnamn, total kapacitet och namnen på de resurser som har redundansväxlats.</span><span class="sxs-lookup"><span data-stu-id="83602-203">Review the details associated with the source device such as device name, total capacity, and the names of the shares that are failed over.</span></span>

    2. <span data-ttu-id="83602-204">Välj den nedrullningsbara listan över tillgängliga enheter en **målenhet**.</span><span class="sxs-lookup"><span data-stu-id="83602-204">From the dropdown list of available devices, choose a **Target device**.</span></span> <span data-ttu-id="83602-205">Endast de enheter som har tillräcklig kapacitet för visas i listrutan.</span><span class="sxs-lookup"><span data-stu-id="83602-205">Only the devices that have sufficient capacity are displayed in the dropdown list.</span></span>

    3. <span data-ttu-id="83602-206">Kontrollera att **jag förstår att den här åtgärden växlar över data till målenheten**.</span><span class="sxs-lookup"><span data-stu-id="83602-206">Check that **I understand that this operation will fail over data to the target device**.</span></span> 

    4. <span data-ttu-id="83602-207">Klicka på **växla över**.</span><span class="sxs-lookup"><span data-stu-id="83602-207">Click **Fail over**.</span></span>
    
        ![](./media/storsimple-virtual-array-failover-dr/failover4.png)
11. <span data-ttu-id="83602-208">Initierar ett jobb för växling vid fel och du får ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="83602-208">A failover job initiates and you receive a notification.</span></span> <span data-ttu-id="83602-209">Gå till **enheter > jobb** att övervaka växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="83602-209">Go to **Devices > Jobs** to monitor the failover.</span></span>
    
     ![](./media/storsimple-virtual-array-failover-dr/failover5.png)
12. <span data-ttu-id="83602-210">I den **jobb** bladet du ser ett failover-jobb skapas för källan.</span><span class="sxs-lookup"><span data-stu-id="83602-210">In the **Jobs** blade, you see a failover job created for the source device.</span></span> <span data-ttu-id="83602-211">Det här jobbet utför DR prechecks.</span><span class="sxs-lookup"><span data-stu-id="83602-211">This job performs the DR prechecks.</span></span>
    
    ![](./media/storsimple-virtual-array-failover-dr/failover6.png)
    
     <span data-ttu-id="83602-212">När DR prechecks är lyckas, kommer beställningsjobbet starta återställningsjobb för varje resurs/volym som finns på källenheten.</span><span class="sxs-lookup"><span data-stu-id="83602-212">After the DR prechecks are successful, the failover job will spawn restore jobs for each share/volume that exists on your source device.</span></span>
    
    ![](./media/storsimple-virtual-array-failover-dr/failover7.png)
13. <span data-ttu-id="83602-213">När redundansväxlingen är klar, går du till den **enheter** bladet.</span><span class="sxs-lookup"><span data-stu-id="83602-213">After the failover is complete, go to the **Devices** blade.</span></span>
    
    1. <span data-ttu-id="83602-214">Välj och klicka på den virtuella StorSimple-enheten som användes som målenhet för failover-processen.</span><span class="sxs-lookup"><span data-stu-id="83602-214">Select and click the StorSimple device that was used as the target device for the failover process.</span></span>
    2. <span data-ttu-id="83602-215">Gå till **Inställningar > Management > resurser** (eller **volymer** om iSCSI-server).</span><span class="sxs-lookup"><span data-stu-id="83602-215">Go to **Settings > Management > Shares** (or **Volumes** if iSCSI server).</span></span> <span data-ttu-id="83602-216">I den **resurser** bladet kan du visa alla resurser (volymer) från den gamla enheten.</span><span class="sxs-lookup"><span data-stu-id="83602-216">In the **Shares** blade, you can view all the shares (volumes) from the old device.</span></span>
        ![](./media/storsimple-virtual-array-failover-dr/failover9.png)
14. <span data-ttu-id="83602-217">Du behöver [skapa en DNS-alias](https://support.microsoft.com/kb/168322) så att alla program som försöker ansluta till kan omdirigeras till den nya enheten.</span><span class="sxs-lookup"><span data-stu-id="83602-217">You will need to [create a DNS alias](https://support.microsoft.com/kb/168322) so that all the applications that are trying to connect can get redirected to the new device.</span></span>

## <a name="errors-during-dr"></a><span data-ttu-id="83602-218">Fel uppstod vid Katastrofåterställning</span><span class="sxs-lookup"><span data-stu-id="83602-218">Errors during DR</span></span>

<span data-ttu-id="83602-219">**Molnet anslutningen avbrott under Katastrofåterställning**</span><span class="sxs-lookup"><span data-stu-id="83602-219">**Cloud connectivity outage during DR**</span></span>

<span data-ttu-id="83602-220">Om molnet anslutningen avbryts när DR har startats och innan enheten återställningen är slutförd, misslyckas DR.</span><span class="sxs-lookup"><span data-stu-id="83602-220">If the cloud connectivity is disrupted after DR has started and before the device restore is complete, the DR will fail.</span></span> <span data-ttu-id="83602-221">Du får ett meddelande om failore.</span><span class="sxs-lookup"><span data-stu-id="83602-221">You receive a failore notification.</span></span> <span data-ttu-id="83602-222">Målenhet för Katastrofåterställning är markerad som *inte kan användas.*</span><span class="sxs-lookup"><span data-stu-id="83602-222">The target device for DR is marked as *unusable.*</span></span> <span data-ttu-id="83602-223">Du kan inte använda samma målenhet för framtida DRs.</span><span class="sxs-lookup"><span data-stu-id="83602-223">You cannot use the same target device for future DRs.</span></span>

<span data-ttu-id="83602-224">**Ingen kompatibel målenheter**</span><span class="sxs-lookup"><span data-stu-id="83602-224">**No compatible target devices**</span></span>

<span data-ttu-id="83602-225">Om tillgängliga målservrar enheter inte har tillräckligt med utrymme, ser du ett fel i syfte att det finns ingen kompatibel målenheter.</span><span class="sxs-lookup"><span data-stu-id="83602-225">If the available target devices do not have sufficient space, you see an error to the effect that there are no compatible target devices.</span></span>

<span data-ttu-id="83602-226">**Precheck fel**</span><span class="sxs-lookup"><span data-stu-id="83602-226">**Precheck failures**</span></span>

<span data-ttu-id="83602-227">Om en av prechecks inte är uppfyllt, se precheck fel.</span><span class="sxs-lookup"><span data-stu-id="83602-227">If one of the prechecks is not satisfied, then you see precheck failures.</span></span>

## <a name="business-continuity-disaster-recovery-bcdr"></a><span data-ttu-id="83602-228">Katastrofåterställning för verksamhetskontinuitet (BCDR)</span><span class="sxs-lookup"><span data-stu-id="83602-228">Business continuity disaster recovery (BCDR)</span></span>

<span data-ttu-id="83602-229">En business continuity (BCDR) katastrofåterställning inträffar när hela Azure-datacentret slutar att fungera.</span><span class="sxs-lookup"><span data-stu-id="83602-229">A business continuity disaster recovery (BCDR) scenario occurs when the entire Azure datacenter stops functioning.</span></span> <span data-ttu-id="83602-230">Detta kan påverka din StorSimple Device Manager-tjänst och de associera StorSimple-enheterna.</span><span class="sxs-lookup"><span data-stu-id="83602-230">This can affect your StorSimple Device Manager service and the associated StorSimple devices.</span></span>

<span data-ttu-id="83602-231">Om StorSimple-enheter som registrerats precis innan en katastrof inträffade behöva dessa StorSimple-enheter som ska tas bort.</span><span class="sxs-lookup"><span data-stu-id="83602-231">If there are StorSimple devices that were registered just before a disaster occurred, then these StorSimple devices may need to be deleted.</span></span> <span data-ttu-id="83602-232">Efter en katastrof kan du återskapa och konfigurerar enheterna.</span><span class="sxs-lookup"><span data-stu-id="83602-232">After the disaster, you can recreate and configure those devices.</span></span>

## <a name="next-steps"></a><span data-ttu-id="83602-233">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="83602-233">Next steps</span></span>

<span data-ttu-id="83602-234">Mer information om hur du [administrera din virtuella StorSimple-matris med lokala webbgränssnittet](storsimple-ova-web-ui-admin.md).</span><span class="sxs-lookup"><span data-stu-id="83602-234">Learn more about how to [administer your StorSimple Virtual Array using the local web UI](storsimple-ova-web-ui-admin.md).</span></span>

