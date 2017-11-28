---
title: "aaaConfigure MPIO på StorSimple Linux-värd | Microsoft Docs"
description: "Konfigurera MPIO på StorSimple anslutna tooa Linux-värd som kör CentOS 6.6"
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: tysonn
ms.assetid: ca289eed-12b7-4e2e-9117-adf7e2034f2f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2016
ms.author: alkohli
ms.openlocfilehash: d9f7e02903243494c909313fb2c33ac690764274
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-mpio-on-a-storsimple-host-running-centos"></a><span data-ttu-id="25ac6-103">Konfigurera MPIO på en StorSimple-värd som kör CentOS</span><span class="sxs-lookup"><span data-stu-id="25ac6-103">Configure MPIO on a StorSimple host running CentOS</span></span>
<span data-ttu-id="25ac6-104">Den här artikeln förklarar hello steg krävs tooconfigure flera sökvägar I/O (MPIO) på värdservern Centos 6.6.</span><span class="sxs-lookup"><span data-stu-id="25ac6-104">This article explains hello steps required tooconfigure Multipathing IO (MPIO) on your Centos 6.6 host server.</span></span> <span data-ttu-id="25ac6-105">hello-värdservern är anslutna tooyour Microsoft Azure StorSimple-enhet för hög tillgänglighet via iSCSI-initierare.</span><span class="sxs-lookup"><span data-stu-id="25ac6-105">hello host server is connected tooyour Microsoft Azure StorSimple device for high availability via iSCSI initiators.</span></span> <span data-ttu-id="25ac6-106">Det beskrivs i detalj hello automatisk identifiering av multipath enheter och hello specifika inställningarna endast för StorSimple-volymer.</span><span class="sxs-lookup"><span data-stu-id="25ac6-106">It describes in detail hello automatic discovery of multipath devices and hello specific setup only for StorSimple volumes.</span></span>

<span data-ttu-id="25ac6-107">Den här proceduren är tillämpliga tooall hello modeller av StorSimple 8000-serien enheter.</span><span class="sxs-lookup"><span data-stu-id="25ac6-107">This procedure is applicable tooall hello models of StorSimple 8000 series devices.</span></span>

> [!NOTE]
> <span data-ttu-id="25ac6-108">Den här proceduren kan inte användas för en virtuell StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="25ac6-108">This procedure cannot be used for a StorSimple virtual device.</span></span> <span data-ttu-id="25ac6-109">Mer information finns i hur tooconfigure värdservrar för din virtuella enhet.</span><span class="sxs-lookup"><span data-stu-id="25ac6-109">For more information, see how tooconfigure host servers for your virtual device.</span></span>
> 
> 

## <a name="about-multipathing"></a><span data-ttu-id="25ac6-110">Om flera sökvägar</span><span class="sxs-lookup"><span data-stu-id="25ac6-110">About multipathing</span></span>
<span data-ttu-id="25ac6-111">hello MPIO-funktionen kan du tooconfigure flera i/o-sökvägar mellan en värdserver och en lagringsenhet.</span><span class="sxs-lookup"><span data-stu-id="25ac6-111">hello multipathing feature allows you tooconfigure multiple I/O paths between a host server and a storage device.</span></span> <span data-ttu-id="25ac6-112">Dessa i/o-sökvägar är fysiska SAN-anslutningar som kan innehålla olika kablar, växlar, nätverksgränssnitt och domänkontrollanter.</span><span class="sxs-lookup"><span data-stu-id="25ac6-112">These I/O paths are physical SAN connections that can include separate cables, switches, network interfaces, and controllers.</span></span> <span data-ttu-id="25ac6-113">Multipathing aggregerar hello i/o-sökvägar, tooconfigure en ny enhet som är associerad med alla hello samman sökvägar.</span><span class="sxs-lookup"><span data-stu-id="25ac6-113">Multipathing aggregates hello I/O paths, tooconfigure a new device that is associated with all of hello aggregated paths.</span></span>

<span data-ttu-id="25ac6-114">hello syftet med flera sökvägar är dubbla:</span><span class="sxs-lookup"><span data-stu-id="25ac6-114">hello purpose of multipathing is two-fold:</span></span>

* <span data-ttu-id="25ac6-115">**Hög tillgänglighet**: den ger en alternativ sökväg om någon del av hello i/o-sökväg (t.ex en kabel växel, nätverksgränssnittet eller domänkontrollant) misslyckas.</span><span class="sxs-lookup"><span data-stu-id="25ac6-115">**High availability**: It provides an alternate path if any element of hello I/O path (such as a cable, switch, network interface, or controller) fails.</span></span>
* <span data-ttu-id="25ac6-116">**Belastningsutjämning**: beroende på hello konfiguration av lagringsenheten, förbättrar den hello prestanda genom att identifiera belastningar hello i/o-sökvägar och dynamiskt ombalansering dessa belastning.</span><span class="sxs-lookup"><span data-stu-id="25ac6-116">**Load balancing**: Depending on hello configuration of your storage device, it can improve hello performance by detecting loads on hello I/O paths and dynamically rebalancing those loads.</span></span>

### <a name="about-multipathing-components"></a><span data-ttu-id="25ac6-117">Om MPIO-komponenter</span><span class="sxs-lookup"><span data-stu-id="25ac6-117">About multipathing components</span></span>
<span data-ttu-id="25ac6-118">Flera sökvägar i Linux består av kernel-komponenter och användaren utrymme komponenter enligt tabellen nedan.</span><span class="sxs-lookup"><span data-stu-id="25ac6-118">Multipathing in Linux consists of kernel components and user-space components as tabulated below.</span></span>

* <span data-ttu-id="25ac6-119">**Kernel**: hello huvudkomponenten är hello *enheten mapper* som dras om i/o och stöd för växling vid fel för sökvägar och sökvägen grupper.</span><span class="sxs-lookup"><span data-stu-id="25ac6-119">**Kernel**: hello main component is hello *device-mapper* that reroutes I/O and supports failover for paths and path groups.</span></span>

* <span data-ttu-id="25ac6-120">**Användaren kan**: dessa är *multipath verktyg* som hanterar multipathed enheter genom att uppmana hello enhet – multipath mappningsmodulen vilka toodo.</span><span class="sxs-lookup"><span data-stu-id="25ac6-120">**User-space**: These are *multipath-tools* that manage multipathed devices by instructing hello device-mapper multipath module what toodo.</span></span> <span data-ttu-id="25ac6-121">hello-verktygen består av:</span><span class="sxs-lookup"><span data-stu-id="25ac6-121">hello tools consist of:</span></span>
   
   * <span data-ttu-id="25ac6-122">**Multipath**: Visar och konfigurerar multipathed enheterna.</span><span class="sxs-lookup"><span data-stu-id="25ac6-122">**Multipath**: lists and configures multipathed devices.</span></span>
   * <span data-ttu-id="25ac6-123">**Multipathd**: daemon som kör multipath och Övervakare hello sökvägar.</span><span class="sxs-lookup"><span data-stu-id="25ac6-123">**Multipathd**: daemon that executes multipath and monitors hello paths.</span></span>
   * <span data-ttu-id="25ac6-124">**Devmap namn**: ger en beskrivande enhetsnamn tooudev för devmaps.</span><span class="sxs-lookup"><span data-stu-id="25ac6-124">**Devmap-name**: provides a meaningful device-name tooudev for devmaps.</span></span>
   * <span data-ttu-id="25ac6-125">**Kpartx**: mappar linjär devmaps toodevice partitioner toomake multipath maps partitionable.</span><span class="sxs-lookup"><span data-stu-id="25ac6-125">**Kpartx**: maps linear devmaps toodevice partitions toomake multipath maps partitionable.</span></span>
   * <span data-ttu-id="25ac6-126">**Multipath.conf**: konfigurationsfilen för flera sökvägar daemon som används toooverwrite hello inbyggda configuration tabell.</span><span class="sxs-lookup"><span data-stu-id="25ac6-126">**Multipath.conf**: configuration file for multipath daemon that is used toooverwrite hello built-in configuration table.</span></span>

### <a name="about-hello-multipathconf-configuration-file"></a><span data-ttu-id="25ac6-127">Om hello multipath.conf konfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="25ac6-127">About hello multipath.conf configuration file</span></span>
<span data-ttu-id="25ac6-128">hello konfigurationsfilen `/etc/multipath.conf` gör många hello MPIO-funktioner kan konfigureras av användaren.</span><span class="sxs-lookup"><span data-stu-id="25ac6-128">hello configuration file `/etc/multipath.conf` makes many of hello multipathing features user-configurable.</span></span> <span data-ttu-id="25ac6-129">Hej `multipath` kommandot och hello kernel-daemon `multipathd` använda informationen i den här filen.</span><span class="sxs-lookup"><span data-stu-id="25ac6-129">hello `multipath` command and hello kernel daemon `multipathd` use information found in this file.</span></span> <span data-ttu-id="25ac6-130">hello filen samråd endast under hello konfigurationen av hello multipath enheter.</span><span class="sxs-lookup"><span data-stu-id="25ac6-130">hello file is consulted only during hello configuration of hello multipath devices.</span></span> <span data-ttu-id="25ac6-131">Kontrollera att alla ändringar innan du kör hello `multipath` kommando.</span><span class="sxs-lookup"><span data-stu-id="25ac6-131">Make sure that all changes are made before you run hello `multipath` command.</span></span> <span data-ttu-id="25ac6-132">Om du ändrar hello filen efteråt du behöver toostop och starta multipathd igen för hello ändringar tootake effekt.</span><span class="sxs-lookup"><span data-stu-id="25ac6-132">If you modify hello file afterwards, you will need toostop and start multipathd again for hello changes tootake effect.</span></span>

<span data-ttu-id="25ac6-133">Hej multipath.conf har fem avsnitt:</span><span class="sxs-lookup"><span data-stu-id="25ac6-133">hello multipath.conf has five sections:</span></span>

- <span data-ttu-id="25ac6-134">**Nivån systemstandard** *(standardinställningen)*: du kan åsidosätta standardvärdena för nivån.</span><span class="sxs-lookup"><span data-stu-id="25ac6-134">**System level defaults** *(defaults)*: You can override system level defaults.</span></span>
- <span data-ttu-id="25ac6-135">**Övriga enheter** *(svartlistat)*: du kan ange hello lista över enheter som inte kan styras av enheten mapper.</span><span class="sxs-lookup"><span data-stu-id="25ac6-135">**Blacklisted devices** *(blacklist)*: You can specify hello list of devices that should not be controlled by device-mapper.</span></span>
- <span data-ttu-id="25ac6-136">**Svartlista undantag** *(blacklist_exceptions)*: du kan identifiera specifika enheter toobe behandlas som multipath enheter även om anges i hello svartlistat.</span><span class="sxs-lookup"><span data-stu-id="25ac6-136">**Blacklist exceptions** *(blacklist_exceptions)*: You can identify specific devices toobe treated as multipath devices even if listed in hello blacklist.</span></span>
- <span data-ttu-id="25ac6-137">**Specifika inställningar för lagring controller** *(enheter)*: du kan ange inställningar som kommer att tillämpas toodevices som innehåller information om leverantör och produkt.</span><span class="sxs-lookup"><span data-stu-id="25ac6-137">**Storage controller specific settings** *(devices)*: You can specify configuration settings that will be applied toodevices that have Vendor and Product information.</span></span>
- <span data-ttu-id="25ac6-138">**Specifika Enhetsinställningar** *(multipaths)*: du kan använda det här avsnittet toofine finjustera hello konfigurationsinställningar för enskilda LUN.</span><span class="sxs-lookup"><span data-stu-id="25ac6-138">**Device specific settings** *(multipaths)*: You can use this section toofine-tune hello configuration settings for individual LUNs.</span></span>

## <a name="configure-multipathing-on-storsimple-connected-toolinux-host"></a><span data-ttu-id="25ac6-139">Konfigurera MPIO på StorSimple anslutna tooLinux värd</span><span class="sxs-lookup"><span data-stu-id="25ac6-139">Configure multipathing on StorSimple connected tooLinux host</span></span>
<span data-ttu-id="25ac6-140">En StorSimple enhet ansluten tooa Linux-värd kan konfigureras för hög tillgänglighet och belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="25ac6-140">A StorSimple device connected tooa Linux host can be configured for high availability and load balancing.</span></span> <span data-ttu-id="25ac6-141">Till exempel om hello Linux-värd har två gränssnitt som är anslutna toohello SAN och hello enhet har två gränssnitt anslutna toohello SAN-nätverk så att dessa gränssnitt finns på hello samma undernät och det ska finnas 4 sökvägar.</span><span class="sxs-lookup"><span data-stu-id="25ac6-141">For example, if hello Linux host has two interfaces connected toohello SAN and hello device has two interfaces connected toohello SAN such that these interfaces are on hello same subnet, then there will be 4 paths available.</span></span> <span data-ttu-id="25ac6-142">Men om varje gränssnitt för DATA i hello gränssnitt för enheten och värden är i ett annat IP-undernät (och inte dirigerbara) kan blir endast 2 sökvägar tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="25ac6-142">However, if each DATA interface on hello device and host interface are on a different IP subnet (and not routable), then only 2 paths will be available.</span></span> <span data-ttu-id="25ac6-143">Du kan konfigurera flera sökvägar tooautomatically identifiera alla hello tillgängliga sökvägar, välja en algoritm för belastningsutjämning för dessa sökvägar, tillämpa specifika konfigurationsinställningar för endast StorSimple-volymer, och sedan aktivera och kontrollera MPIO.</span><span class="sxs-lookup"><span data-stu-id="25ac6-143">You can configure multipathing tooautomatically discover all hello available paths, choose a load-balancing algorithm for those paths, apply specific configuration settings for StorSimple-only volumes, and then enable and verify multipathing.</span></span>

<span data-ttu-id="25ac6-144">hello beskrivs nedan hur tooconfigure MPIO när en StorSimple-enhet med två nätverksgränssnitt är anslutna tooa värd med två nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="25ac6-144">hello following procedure describes how tooconfigure multipathing when a StorSimple device with two network interfaces is connected tooa host with two network interfaces.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="25ac6-145">Krav</span><span class="sxs-lookup"><span data-stu-id="25ac6-145">Prerequisites</span></span>
<span data-ttu-id="25ac6-146">Det här avsnittet beskrivs hello konfigurationskrav för CentOS server och din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="25ac6-146">This section details hello configuration prerequisites for CentOS server and your StorSimple device.</span></span>

### <a name="on-centos-host"></a><span data-ttu-id="25ac6-147">På CentOS värden</span><span class="sxs-lookup"><span data-stu-id="25ac6-147">On CentOS host</span></span>
1. <span data-ttu-id="25ac6-148">Kontrollera att värddatorn CentOS har 2 nätverksgränssnitt som är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="25ac6-148">Make sure that your CentOS host has 2 network interfaces enabled.</span></span> <span data-ttu-id="25ac6-149">Ange:</span><span class="sxs-lookup"><span data-stu-id="25ac6-149">Type:</span></span>
   
    `ifconfig`
   
    <span data-ttu-id="25ac6-150">hello följande exempel visar hello utdata när två nätverksgränssnitt (`eth0` och `eth1`) finns på hello värden.</span><span class="sxs-lookup"><span data-stu-id="25ac6-150">hello following example shows hello output when two network interfaces (`eth0` and `eth1`) are present on hello host.</span></span>
   
        [root@centosSS ~]# ifconfig
        eth0  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:41  
          inet addr:10.126.162.65  Bcast:10.126.163.255  Mask:255.255.252.0
          inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3341/64 Scope:Global
          inet6 addr: fe80::215:5dff:fea2:3341/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
         RX packets:36536 errors:0 dropped:0 overruns:0 frame:0
          TX packets:6312 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:13994127 (13.3 MiB)  TX bytes:645654 (630.5 KiB)
   
        eth1  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:42  
          inet addr:10.126.162.66  Bcast:10.126.163.255  Mask:255.255.252.0
          inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3342/64 Scope:Global
          inet6 addr: fe80::215:5dff:fea2:3342/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:25962 errors:0 dropped:0 overruns:0 frame:0
          TX packets:11 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:2597350 (2.4 MiB)  TX bytes:754 (754.0 b)
   
        loLink encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:12 errors:0 dropped:0 overruns:0 frame:0
          TX packets:12 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:720 (720.0 b)  TX bytes:720 (720.0 b)
2. <span data-ttu-id="25ac6-151">Installera *verktyg för iSCSI-initieraren-webbplatsuppgradering* på CentOS servern.</span><span class="sxs-lookup"><span data-stu-id="25ac6-151">Install *iSCSI-initiator-utils* on your CentOS server.</span></span> <span data-ttu-id="25ac6-152">Utför följande steg tooinstall hello *verktyg för iSCSI-initieraren-webbplatsuppgradering*.</span><span class="sxs-lookup"><span data-stu-id="25ac6-152">Perform hello following steps tooinstall *iSCSI-initiator-utils*.</span></span>
   
   1. <span data-ttu-id="25ac6-153">Logga in som `root` till CentOS-värden.</span><span class="sxs-lookup"><span data-stu-id="25ac6-153">Log on as `root` into your CentOS host.</span></span>
   2. <span data-ttu-id="25ac6-154">Installera hello *verktyg för iSCSI-initieraren-webbplatsuppgradering*.</span><span class="sxs-lookup"><span data-stu-id="25ac6-154">Install hello *iSCSI-initiator-utils*.</span></span> <span data-ttu-id="25ac6-155">Ange:</span><span class="sxs-lookup"><span data-stu-id="25ac6-155">Type:</span></span>
      
       `yum install iscsi-initiator-utils`
   3. <span data-ttu-id="25ac6-156">Efter hello *verktyg för iSCSI-initieraren-webbplatsuppgradering* är korrekt installerat, starta hello iSCSI-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="25ac6-156">After hello *iSCSI-Initiator-utils* is successfully installed, start hello iSCSI service.</span></span> <span data-ttu-id="25ac6-157">Ange:</span><span class="sxs-lookup"><span data-stu-id="25ac6-157">Type:</span></span>
      
       `service iscsid start`
      
       <span data-ttu-id="25ac6-158">Vid tillfällen `iscsid` får inte börja och hello `--force` alternativet kan behövas</span><span class="sxs-lookup"><span data-stu-id="25ac6-158">On occasions, `iscsid` may not actually start and hello `--force` option may be needed</span></span>
   4. <span data-ttu-id="25ac6-159">tooensure att iSCSI-initieraren är aktiverad under starten, Använd hello `chkconfig` kommandot tooenable hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="25ac6-159">tooensure that your iSCSI initiator is enabled during boot time, use hello `chkconfig` command tooenable hello service.</span></span>
      
       `chkconfig iscsi on`
   5. <span data-ttu-id="25ac6-160">tooverify att det var korrekt installation, kör hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="25ac6-160">tooverify that that it was properly setup, run hello command:</span></span>
      
       `chkconfig --list | grep iscsi`
      
       <span data-ttu-id="25ac6-161">Ett exempel på utdata visas nedan.</span><span class="sxs-lookup"><span data-stu-id="25ac6-161">A sample output is shown below.</span></span>
      
           iscsi   0:off   1:off   2:on3:on4:on5:on6:off
           iscsid  0:off   1:off   2:on3:on4:on5:on6:off
      
       <span data-ttu-id="25ac6-162">Du kan se att iSCSI-miljö ska köras på starttiden på Kör nivå 2, 3, 4 och 5 från hello ovan exempel.</span><span class="sxs-lookup"><span data-stu-id="25ac6-162">From hello above example, you can see that your iSCSI environment will run on boot time on run levels 2, 3, 4, and 5.</span></span>
3. <span data-ttu-id="25ac6-163">Installera *enhet-mapper-multipath*.</span><span class="sxs-lookup"><span data-stu-id="25ac6-163">Install *device-mapper-multipath*.</span></span> <span data-ttu-id="25ac6-164">Ange:</span><span class="sxs-lookup"><span data-stu-id="25ac6-164">Type:</span></span>
   
    `yum install device-mapper-multipath`
   
    <span data-ttu-id="25ac6-165">hello installationen startar.</span><span class="sxs-lookup"><span data-stu-id="25ac6-165">hello installation will start.</span></span> <span data-ttu-id="25ac6-166">Typen **Y** toocontinue när du uppmanas att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="25ac6-166">Type **Y** toocontinue when prompted for confirmation.</span></span>

### <a name="on-storsimple-device"></a><span data-ttu-id="25ac6-167">På StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="25ac6-167">On StorSimple device</span></span>
<span data-ttu-id="25ac6-168">Din StorSimple-enhet bör ha:</span><span class="sxs-lookup"><span data-stu-id="25ac6-168">Your StorSimple device should have:</span></span>

* <span data-ttu-id="25ac6-169">Minst två gränssnitt som är aktiverade för iSCSI.</span><span class="sxs-lookup"><span data-stu-id="25ac6-169">A minimum of two interfaces enabled for iSCSI.</span></span> <span data-ttu-id="25ac6-170">tooverify att två gränssnitt som är iSCSI-aktiverade på din StorSimple-enhet utför hello följa stegen i hello Azure klassiska portal för din StorSimple-enhet:</span><span class="sxs-lookup"><span data-stu-id="25ac6-170">tooverify that two interfaces are iSCSI-enabled on your StorSimple device, perform hello following steps in hello Azure classic portal for your StorSimple device:</span></span>
  
  1. <span data-ttu-id="25ac6-171">Logga in hello klassiska portalen för din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="25ac6-171">Log into hello classic portal for your StorSimple device.</span></span>
  2. <span data-ttu-id="25ac6-172">Välj din StorSimple Manager-tjänsten, klicka på **enheter** och välj hello specifika StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="25ac6-172">Select your StorSimple Manager service, click **Devices** and choose hello specific StorSimple device.</span></span> <span data-ttu-id="25ac6-173">Klicka på **konfigurera** och kontrollera hello inställningar för nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="25ac6-173">Click **Configure** and verify hello network interface settings.</span></span> <span data-ttu-id="25ac6-174">En skärmbild med två iSCSI-aktiverade nätverkskort visas nedan.</span><span class="sxs-lookup"><span data-stu-id="25ac6-174">A screenshot with two iSCSI-enabled network interfaces is shown below.</span></span> <span data-ttu-id="25ac6-175">Här DATA 2 och DATA 3, både 10 GbE gränssnitt har aktiverats för iSCSI.</span><span class="sxs-lookup"><span data-stu-id="25ac6-175">Here DATA 2 and DATA 3, both 10 GbE interfaces are enabled for iSCSI.</span></span>
     
      ![MPIO StorsSimple DATA 2-konfiguration](./media/storsimple-configure-mpio-on-linux/IC761347.png)
     
      ![MPIO StorSimple DATA 3 Config](./media/storsimple-configure-mpio-on-linux/IC761348.png)
     
      <span data-ttu-id="25ac6-178">I hello **konfigurera** sidan</span><span class="sxs-lookup"><span data-stu-id="25ac6-178">In hello **Configure** page</span></span>
     
     1. <span data-ttu-id="25ac6-179">Se till att båda nätverksgränssnitt är iSCSI-aktiverade.</span><span class="sxs-lookup"><span data-stu-id="25ac6-179">Ensure that both network interfaces are iSCSI-enabled.</span></span> <span data-ttu-id="25ac6-180">Hej **iSCSI-aktiverade** fält ska anges för**Ja**.</span><span class="sxs-lookup"><span data-stu-id="25ac6-180">hello **iSCSI enabled** field should be set too**Yes**.</span></span>
     2. <span data-ttu-id="25ac6-181">Se till att hello nätverksgränssnitt har hello samma hastighet båda vara 1 GbE- eller 10 GbE.</span><span class="sxs-lookup"><span data-stu-id="25ac6-181">Ensure that hello network interfaces have hello same speed, both should be 1 GbE or 10 GbE.</span></span>
     3. <span data-ttu-id="25ac6-182">Notera hello IPv4-adresserna till hello iSCSI-aktiverade gränssnitt och spara för senare användning på hello värden.</span><span class="sxs-lookup"><span data-stu-id="25ac6-182">Note hello IPv4 addresses of hello iSCSI-enabled interfaces and save for later use on hello host.</span></span>
* <span data-ttu-id="25ac6-183">hello iSCSI-gränssnitt på StorSimple-enheten ska kunna nås från hello CentOS server.</span><span class="sxs-lookup"><span data-stu-id="25ac6-183">hello iSCSI interfaces on your StorSimple device should be reachable from hello CentOS server.</span></span>
      <span data-ttu-id="25ac6-184">tooverify, du behöver tooprovide hello IP-adresser för din StorSimple iSCSI-aktiverade nätverkskort på värdservern.</span><span class="sxs-lookup"><span data-stu-id="25ac6-184">tooverify this, you need tooprovide hello IP addresses of your StorSimple iSCSI-enabled network interfaces on your host server.</span></span> <span data-ttu-id="25ac6-185">Hej kommandon som används och hello motsvarande utdata med DATA2 (10.126.162.25) och DATA3 (10.126.162.26) visas nedan:</span><span class="sxs-lookup"><span data-stu-id="25ac6-185">hello commands used and hello corresponding output with DATA2 (10.126.162.25) and DATA3 (10.126.162.26) is shown below:</span></span>
  
        [root@centosSS ~]# iscsiadm -m discovery -t sendtargets -p 10.126.162.25:3260
        10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target
        10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target

### <a name="hardware-configuration"></a><span data-ttu-id="25ac6-186">Maskinvarukonfiguration</span><span class="sxs-lookup"><span data-stu-id="25ac6-186">Hardware configuration</span></span>
<span data-ttu-id="25ac6-187">Vi rekommenderar att du ansluter hello två iSCSI nätverksgränssnitt på separata sökvägar för redundans.</span><span class="sxs-lookup"><span data-stu-id="25ac6-187">We recommend that you connect hello two iSCSI network interfaces on separate paths for redundancy.</span></span> <span data-ttu-id="25ac6-188">hello bilden nedan visar hello rekommenderade maskinvarukonfigurationen för hög tillgänglighet och belastningsutjämning MPIO för din CentOS server och StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="25ac6-188">hello figure below shows hello recommended hardware configuration for high availability and load-balancing multipathing for your CentOS server and StorSimple device.</span></span>

![MPIO maskinvarukonfiguration för StorSimple tooLinux värden](./media/storsimple-configure-mpio-on-linux/MPIOHardwareConfigurationStorSimpleToLinuxHost2M.png)

<span data-ttu-id="25ac6-190">Som visas i föregående bild hello:</span><span class="sxs-lookup"><span data-stu-id="25ac6-190">As shown in hello preceding figure:</span></span>

* <span data-ttu-id="25ac6-191">StorSimple-enheten är i ett aktivt-passivt konfiguration med två domänkontrollanter.</span><span class="sxs-lookup"><span data-stu-id="25ac6-191">Your StorSimple device is in an active-passive configuration with two controllers.</span></span>
* <span data-ttu-id="25ac6-192">Två SAN-switchar är anslutna tooyour styrenheter.</span><span class="sxs-lookup"><span data-stu-id="25ac6-192">Two SAN switches are connected tooyour device controllers.</span></span>
* <span data-ttu-id="25ac6-193">Två iSCSI-initierarna har aktiverats på din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="25ac6-193">Two iSCSI initiators are enabled on your StorSimple device.</span></span>
* <span data-ttu-id="25ac6-194">Två nätverksgränssnitt är aktiverade på CentOS-värden.</span><span class="sxs-lookup"><span data-stu-id="25ac6-194">Two network interfaces are enabled on your CentOS host.</span></span>

<span data-ttu-id="25ac6-195">hello ovan konfiguration kommer att ge 4 separat sökvägar mellan enheten och hello värden om hello gränssnitt för data från värdar och är dirigerbart.</span><span class="sxs-lookup"><span data-stu-id="25ac6-195">hello above configuration will yield 4 separate paths between your device and hello host if hello host and data interfaces are routable.</span></span>

> [!IMPORTANT]
> * <span data-ttu-id="25ac6-196">Vi rekommenderar att du inte blandar 1 GbE och 10 GbE-nätverkskort för flera sökvägar.</span><span class="sxs-lookup"><span data-stu-id="25ac6-196">We recommend that you do not mix 1 GbE and 10 GbE network interfaces for multipathing.</span></span> <span data-ttu-id="25ac6-197">När du använder två nätverksgränssnitt, ska båda gränssnitten hello vara hello identiska.</span><span class="sxs-lookup"><span data-stu-id="25ac6-197">When using two network interfaces, both hello interfaces should be hello identical type.</span></span>
> * <span data-ttu-id="25ac6-198">På din StorSimple-enhet DATA0, fil1, DATA4 och DATA5 är 1 GbE-gränssnitt DATA2 och DATA3 är 10 GbE-nätverkskort. |</span><span class="sxs-lookup"><span data-stu-id="25ac6-198">On your StorSimple device, DATA0, DATA1, DATA4 and DATA5 are 1 GbE interfaces whereas DATA2 and DATA3 are 10 GbE network interfaces.|</span></span>
> 
> 

## <a name="configuration-steps"></a><span data-ttu-id="25ac6-199">Konfigurationssteg</span><span class="sxs-lookup"><span data-stu-id="25ac6-199">Configuration steps</span></span>
<span data-ttu-id="25ac6-200">hello konfigurationssteg för flera sökvägar innebär konfigurering hello tillgängliga sökvägar för automatisk identifiering, ange hello belastningsutjämning algoritmen toouse, aktivera MPIO och slutligen verifiera hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="25ac6-200">hello configuration steps for multipathing involve configuring hello available paths for automatic discovery, specifying hello load-balancing algorithm toouse, enabling multipathing and finally verifying hello configuration.</span></span> <span data-ttu-id="25ac6-201">Var och en av dessa steg beskrivs i detalj i följande avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="25ac6-201">Each of these steps is discussed in detail in hello following sections.</span></span>

### <a name="step-1-configure-multipathing-for-automatic-discovery"></a><span data-ttu-id="25ac6-202">Steg 1: Konfigurera MPIO för automatisk identifiering</span><span class="sxs-lookup"><span data-stu-id="25ac6-202">Step 1: Configure multipathing for automatic discovery</span></span>
<span data-ttu-id="25ac6-203">hello stöds multipath enheter kan identifieras och konfigureras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="25ac6-203">hello multipath-supported devices can be automatically discovered and configured.</span></span>

1. <span data-ttu-id="25ac6-204">Initiera `/etc/multipath.conf` fil.</span><span class="sxs-lookup"><span data-stu-id="25ac6-204">Initialize `/etc/multipath.conf` file.</span></span> <span data-ttu-id="25ac6-205">Ange:</span><span class="sxs-lookup"><span data-stu-id="25ac6-205">Type:</span></span>
   
     `mpathconf --enable`
   
    <span data-ttu-id="25ac6-206">hello ovanstående kommando skapar en `sample/etc/multipath.conf` fil.</span><span class="sxs-lookup"><span data-stu-id="25ac6-206">hello above command will create a `sample/etc/multipath.conf` file.</span></span>
2. <span data-ttu-id="25ac6-207">Starta flera sökvägar.</span><span class="sxs-lookup"><span data-stu-id="25ac6-207">Start multipath service.</span></span> <span data-ttu-id="25ac6-208">Ange:</span><span class="sxs-lookup"><span data-stu-id="25ac6-208">Type:</span></span>
   
    `service multipathd start`
   
    <span data-ttu-id="25ac6-209">Hello följande utdata visas:</span><span class="sxs-lookup"><span data-stu-id="25ac6-209">You will see hello following output:</span></span>
   
    `Starting multipathd daemon:`
3. <span data-ttu-id="25ac6-210">Aktivera automatisk identifiering av multipaths.</span><span class="sxs-lookup"><span data-stu-id="25ac6-210">Enable automatic discovery of multipaths.</span></span> <span data-ttu-id="25ac6-211">Ange:</span><span class="sxs-lookup"><span data-stu-id="25ac6-211">Type:</span></span>
   
    `mpathconf --find_multipaths y`
   
    <span data-ttu-id="25ac6-212">Detta kommer att ändra hello-standardinställningar för din `multipath.conf` enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="25ac6-212">This will modify hello defaults section of your `multipath.conf` as shown below:</span></span>
   
        defaults {
        find_multipaths yes
        user_friendly_names yes
        path_grouping_policy multibus
        }

### <a name="step-2-configure-multipathing-for-storsimple-volumes"></a><span data-ttu-id="25ac6-213">Steg 2: Konfigurera MPIO för StorSimple-volymer</span><span class="sxs-lookup"><span data-stu-id="25ac6-213">Step 2: Configure multipathing for StorSimple volumes</span></span>
<span data-ttu-id="25ac6-214">Som standard alla enheter är svart som anges i hello multipath.conf fil och ska kringgås.</span><span class="sxs-lookup"><span data-stu-id="25ac6-214">By default, all devices are black listed in hello multipath.conf file and will be bypassed.</span></span> <span data-ttu-id="25ac6-215">Du behöver toocreate svartlistat undantag tooallow MPIO för volymer från StorSimple-enheter.</span><span class="sxs-lookup"><span data-stu-id="25ac6-215">You will need toocreate blacklist exceptions tooallow multipathing for volumes from StorSimple devices.</span></span>

1. <span data-ttu-id="25ac6-216">Redigera hello `/etc/mulitpath.conf` fil.</span><span class="sxs-lookup"><span data-stu-id="25ac6-216">Edit hello `/etc/mulitpath.conf` file.</span></span> <span data-ttu-id="25ac6-217">Ange:</span><span class="sxs-lookup"><span data-stu-id="25ac6-217">Type:</span></span>
   
    `vi /etc/multipath.conf`
2. <span data-ttu-id="25ac6-218">Leta upp hello blacklist_exceptions avsnitt i hello multipath.conf fil.</span><span class="sxs-lookup"><span data-stu-id="25ac6-218">Locate hello blacklist_exceptions section in hello multipath.conf file.</span></span> <span data-ttu-id="25ac6-219">Din StorSimple-enhet måste toobe anges som ett svartlistat undantag i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="25ac6-219">Your StorSimple device needs toobe listed as a blacklist exception in this section.</span></span> <span data-ttu-id="25ac6-220">Du kan ta bort kommentarerna relevanta rader i den här filen toomodify det som visas nedan (Använd endast hello specifika modell för hello-enhet du använder):</span><span class="sxs-lookup"><span data-stu-id="25ac6-220">You can uncomment relevant lines in this file toomodify it as shown below (use only hello specific model of hello device you are using):</span></span>
   
        blacklist_exceptions {
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8100*"
            }
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8600*"
            }
           }

### <a name="step-3-configure-round-robin-multipathing"></a><span data-ttu-id="25ac6-221">Steg 3: Konfigurera MPIO för resursallokering</span><span class="sxs-lookup"><span data-stu-id="25ac6-221">Step 3: Configure round-robin multipathing</span></span>
<span data-ttu-id="25ac6-222">Belastningsutjämning algoritmen använder alla hello tillgängliga multipaths toohello aktiva styrenheten i ett balanserat, resursallokering sätt.</span><span class="sxs-lookup"><span data-stu-id="25ac6-222">This load-balancing algorithm uses all hello available multipaths toohello active controller in a balanced, round-robin fashion.</span></span>

1. <span data-ttu-id="25ac6-223">Redigera hello `/etc/multipath.conf` fil.</span><span class="sxs-lookup"><span data-stu-id="25ac6-223">Edit hello `/etc/multipath.conf` file.</span></span> <span data-ttu-id="25ac6-224">Ange:</span><span class="sxs-lookup"><span data-stu-id="25ac6-224">Type:</span></span>
   
    `vi /etc/multipath.conf`
2. <span data-ttu-id="25ac6-225">Under hello `defaults` avsnitt, ange hello `path_grouping_policy` för`multibus`.</span><span class="sxs-lookup"><span data-stu-id="25ac6-225">Under hello `defaults` section, set hello `path_grouping_policy` too`multibus`.</span></span> <span data-ttu-id="25ac6-226">Hej `path_grouping_policy` anger hello standard sökväg gruppering princip tooapply toounspecified multipaths.</span><span class="sxs-lookup"><span data-stu-id="25ac6-226">hello `path_grouping_policy` specifies hello default path grouping policy tooapply toounspecified multipaths.</span></span> <span data-ttu-id="25ac6-227">hello-standardinställningar ser ut som nedan.</span><span class="sxs-lookup"><span data-stu-id="25ac6-227">hello defaults section will look as shown below.</span></span>
   
        defaults {
                user_friendly_names yes
                path_grouping_policy multibus
        }

> [!NOTE]
> <span data-ttu-id="25ac6-228">Hej vanligaste värdena för `path_grouping_policy` omfattar:</span><span class="sxs-lookup"><span data-stu-id="25ac6-228">hello most common values of `path_grouping_policy` include:</span></span>
> 
> * <span data-ttu-id="25ac6-229">redundans = 1 sökväg per prioritet grupp</span><span class="sxs-lookup"><span data-stu-id="25ac6-229">failover = 1 path per priority group</span></span>
> * <span data-ttu-id="25ac6-230">multibus = alla giltiga sökvägar i grupp 1 prioritet</span><span class="sxs-lookup"><span data-stu-id="25ac6-230">multibus = all valid paths in 1 priority group</span></span>
> 
> 

### <a name="step-4-enable-multipathing"></a><span data-ttu-id="25ac6-231">Steg 4: Aktivera MPIO</span><span class="sxs-lookup"><span data-stu-id="25ac6-231">Step 4: Enable multipathing</span></span>
1. <span data-ttu-id="25ac6-232">Starta om hello `multipathd` daemon.</span><span class="sxs-lookup"><span data-stu-id="25ac6-232">Restart hello `multipathd` daemon.</span></span> <span data-ttu-id="25ac6-233">Ange:</span><span class="sxs-lookup"><span data-stu-id="25ac6-233">Type:</span></span>
   
    `service multipathd restart`
2. <span data-ttu-id="25ac6-234">hello utdata blir enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="25ac6-234">hello output will be as shown below:</span></span>
   
        [root@centosSS ~]# service multipathd start
        Starting multipathd daemon:  [OK]

### <a name="step-5-verify-multipathing"></a><span data-ttu-id="25ac6-235">Steg 5: Kontrollera MPIO</span><span class="sxs-lookup"><span data-stu-id="25ac6-235">Step 5: Verify multipathing</span></span>
1. <span data-ttu-id="25ac6-236">Kontrollera att iSCSI-anslutning upprättas med hello StorSimple-enheten på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="25ac6-236">First make sure that iSCSI connection is established with hello StorSimple device as follows:</span></span>
   
   <span data-ttu-id="25ac6-237">a.</span><span class="sxs-lookup"><span data-stu-id="25ac6-237">a.</span></span> <span data-ttu-id="25ac6-238">Identifiera din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="25ac6-238">Discover your StorSimple device.</span></span> <span data-ttu-id="25ac6-239">Ange:</span><span class="sxs-lookup"><span data-stu-id="25ac6-239">Type:</span></span>
      
    ```
    iscsiadm -m discovery -t sendtargets -p  <IP address of network interface on hello device>:<iSCSI port on StorSimple device>
    ```
    
    <span data-ttu-id="25ac6-240">hello utdata när IP-adress för DATA0 är 10.126.162.25 och port 3260 öppnas på hello StorSimple-enhet för utgående iSCSI-trafik är enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="25ac6-240">hello output when IP address for DATA0 is 10.126.162.25 and port 3260 is opened on hello StorSimple device for outbound iSCSI traffic is as shown below:</span></span>
    
    ```
    10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
    ```

    <span data-ttu-id="25ac6-241">Kopiera hello IQN för din StorSimple-enhet `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`, från hello föregående utdata.</span><span class="sxs-lookup"><span data-stu-id="25ac6-241">Copy hello IQN of your StorSimple device, `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`, from hello preceding output.</span></span>

   <span data-ttu-id="25ac6-242">b.</span><span class="sxs-lookup"><span data-stu-id="25ac6-242">b.</span></span> <span data-ttu-id="25ac6-243">Anslut med hjälp av mål IQN toohello-enhet.</span><span class="sxs-lookup"><span data-stu-id="25ac6-243">Connect toohello device using target IQN.</span></span> <span data-ttu-id="25ac6-244">Hej StorSimple-enhet är hello iSCSI-målet här.</span><span class="sxs-lookup"><span data-stu-id="25ac6-244">hello StorSimple device is hello iSCSI target here.</span></span> <span data-ttu-id="25ac6-245">Ange:</span><span class="sxs-lookup"><span data-stu-id="25ac6-245">Type:</span></span>

    ```
    iscsiadm -m node --login -T <IQN of iSCSI target>
    ```

    <span data-ttu-id="25ac6-246">hello följande exempel visas med ett mål IQN av `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`.</span><span class="sxs-lookup"><span data-stu-id="25ac6-246">hello following example shows output with a target IQN of `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`.</span></span> <span data-ttu-id="25ac6-247">hello utdata visar att du har anslutit toohello två iSCSI-aktiverade nätverksgränssnitt på enheten.</span><span class="sxs-lookup"><span data-stu-id="25ac6-247">hello output indicates that you have successfully connected toohello two iSCSI-enabled network interfaces on your device.</span></span>

    ```
    Logging in too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
    Logging in too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
    Logging in too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
    Logging in too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
    Login too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
    Login too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
    Login too[iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
    Login too[iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
    ```

    <span data-ttu-id="25ac6-248">Om du ser en enda värd-gränssnittet och här två sökvägar måste tooenable båda hello gränssnitten på värden för iSCSI.</span><span class="sxs-lookup"><span data-stu-id="25ac6-248">If you see only one host interface and two paths here, then you need tooenable both hello interfaces on host for iSCSI.</span></span> <span data-ttu-id="25ac6-249">Du kan följa hello [detaljerade instruktioner i dokumentationen för Linux](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/5/html/Online_Storage_Reconfiguration_Guide/iscsioffloadmain.html).</span><span class="sxs-lookup"><span data-stu-id="25ac6-249">You can follow hello [detailed instructions in Linux documentation](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/5/html/Online_Storage_Reconfiguration_Guide/iscsioffloadmain.html).</span></span>

2. <span data-ttu-id="25ac6-250">En volym är utsatta toohello CentOS servern från hello StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="25ac6-250">A volume is exposed toohello CentOS server from hello StorSimple device.</span></span> <span data-ttu-id="25ac6-251">Mer information finns i [steg 6: skapa en volym](storsimple-deployment-walkthrough.md#step-6-create-a-volume) via hello klassiska Azure-portalen på StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="25ac6-251">For more information, see [Step 6: Create a volume](storsimple-deployment-walkthrough.md#step-6-create-a-volume) via hello Azure classic portal on your StorSimple device.</span></span>

3. <span data-ttu-id="25ac6-252">Kontrollera hello tillgängliga sökvägar.</span><span class="sxs-lookup"><span data-stu-id="25ac6-252">Verify hello available paths.</span></span> <span data-ttu-id="25ac6-253">Ange:</span><span class="sxs-lookup"><span data-stu-id="25ac6-253">Type:</span></span>

      ```
      multipath –l
      ```

      <span data-ttu-id="25ac6-254">hello som följande exempel visar hello utdata för två nätverkskort på en StorSimple-enhet ansluten tooa värddator nätverksgränssnitt med två tillgängliga sökvägar.</span><span class="sxs-lookup"><span data-stu-id="25ac6-254">hello following example shows hello output for two network interfaces on a StorSimple device connected tooa single host network interface with two available paths.</span></span>

        ```
        mpathb (36486fd20cc081f8dcd3fccb992d45a68) dm-3 MSFT,STORSIMPLE 8100
        size=100G features='0' hwhandler='0' wp=rw
        `-+- policy='round-robin 0' prio=0 status=active
        |- 7:0:0:1 sdc 8:32 active undef running
        `- 6:0:0:1 sdd 8:48 active undef running
        ```

        hello following example shows hello output for two network interfaces on a StorSimple device connected tootwo host network interfaces with four available paths.

        ```
        mpathb (36486fd27a23feba1b096226f11420f6b) dm-2 MSFT,STORSIMPLE 8100
        size=100G features='0' hwhandler='0' wp=rw
        `-+- policy='round-robin 0' prio=0 status=active
        |- 17:0:0:0 sdb 8:16 active undef running
        |- 15:0:0:0 sdd 8:48 active undef running
        |- 14:0:0:0 sdc 8:32 active undef running
        `- 16:0:0:0 sde 8:64 active undef running
        ```

        After hello paths are configured, refer toohello specific instructions on your host operating system (Centos 6.6) toomount and format this volume.

## <a name="troubleshoot-multipathing"></a><span data-ttu-id="25ac6-255">Felsöka MPIO</span><span class="sxs-lookup"><span data-stu-id="25ac6-255">Troubleshoot multipathing</span></span>
<span data-ttu-id="25ac6-256">Det här avsnittet innehåller några nyttiga tips om du stöter på problem vid konfiguration av MPIO.</span><span class="sxs-lookup"><span data-stu-id="25ac6-256">This section provides some helpful tips if you run into any issues during multipathing configuration.</span></span>

<span data-ttu-id="25ac6-257">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="25ac6-257">Q.</span></span> <span data-ttu-id="25ac6-258">Visas inte hello ändringar i `multipath.conf` fil börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="25ac6-258">I do not see hello changes in `multipath.conf` file taking effect.</span></span>

<span data-ttu-id="25ac6-259">A.</span><span class="sxs-lookup"><span data-stu-id="25ac6-259">A.</span></span> <span data-ttu-id="25ac6-260">Om du har gjort några ändringar toohello `multipath.conf` filen, behöver du toorestart hello multipathing tjänst.</span><span class="sxs-lookup"><span data-stu-id="25ac6-260">If you have made any changes toohello `multipath.conf` file, you will need toorestart hello multipathing service.</span></span> <span data-ttu-id="25ac6-261">Skriv följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="25ac6-261">Type hello following command:</span></span>

    service multipathd restart

<span data-ttu-id="25ac6-262">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="25ac6-262">Q.</span></span> <span data-ttu-id="25ac6-263">Jag har aktiverat två nätverksgränssnitt på hello StorSimple-enhet och två nätverksgränssnitt på hello värden.</span><span class="sxs-lookup"><span data-stu-id="25ac6-263">I have enabled two network interfaces on hello StorSimple device and two network interfaces on hello host.</span></span> <span data-ttu-id="25ac6-264">När jag ange hello tillgängliga sökvägar, visas bara två sökvägar.</span><span class="sxs-lookup"><span data-stu-id="25ac6-264">When I list hello available paths, I see only two paths.</span></span> <span data-ttu-id="25ac6-265">Det förväntade toosee fyra tillgängliga sökvägar.</span><span class="sxs-lookup"><span data-stu-id="25ac6-265">I expected toosee four available paths.</span></span>

<span data-ttu-id="25ac6-266">A.</span><span class="sxs-lookup"><span data-stu-id="25ac6-266">A.</span></span> <span data-ttu-id="25ac6-267">Kontrollera att hello två sökvägar finns på hello samma undernät och inbördes.</span><span class="sxs-lookup"><span data-stu-id="25ac6-267">Make sure that hello two paths are on hello same subnet and routable.</span></span> <span data-ttu-id="25ac6-268">Om hello nätverksgränssnitt finns på olika VLAN och inte dirigeras visas bara två sökvägar.</span><span class="sxs-lookup"><span data-stu-id="25ac6-268">If hello network interfaces are on different vLANs and not routable, you will see only two paths.</span></span> <span data-ttu-id="25ac6-269">Enkelriktade tooverify är toomake till att du kan nå båda gränssnitten med hello värden från ett nätverksgränssnitt på hello StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="25ac6-269">One way tooverify this is toomake sure that you can reach both hello host interfaces from a network interface on hello StorSimple device.</span></span> <span data-ttu-id="25ac6-270">Du behöver för[kontaktar Microsoft Support](storsimple-contact-microsoft-support.md) som den här kontrollen kan bara göras via en supportsession.</span><span class="sxs-lookup"><span data-stu-id="25ac6-270">You will need too[contact Microsoft Support](storsimple-contact-microsoft-support.md) as this verification can only be done via a support session.</span></span>

<span data-ttu-id="25ac6-271">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="25ac6-271">Q.</span></span> <span data-ttu-id="25ac6-272">När jag Listar tillgängliga sökvägar syns inte några utdata.</span><span class="sxs-lookup"><span data-stu-id="25ac6-272">When I list available paths, I do not see any output.</span></span>

<span data-ttu-id="25ac6-273">A.</span><span class="sxs-lookup"><span data-stu-id="25ac6-273">A.</span></span> <span data-ttu-id="25ac6-274">Normalt inte ser några multipathed sökvägar tyder på problem med hello MPIO daemon och det är troligen att alla problem här ligger i hello `multipath.conf` fil.</span><span class="sxs-lookup"><span data-stu-id="25ac6-274">Typically, not seeing any multipathed paths suggests a problem with hello multipathing daemon, and it’s most likely that any problem here lies in hello `multipath.conf` file.</span></span>

<span data-ttu-id="25ac6-275">Det kan även vara värt att kontrollera att du verkligen kan se några diskar efter anslutning toohello mål, eftersom inget svar från hello multipath listor kan också innebära du inte har några diskar.</span><span class="sxs-lookup"><span data-stu-id="25ac6-275">It would also be worth checking that you can actually see some disks after connecting toohello target, as no response from hello multipath listings could also mean you don’t have any disks.</span></span>

* <span data-ttu-id="25ac6-276">Använd följande kommando toorescan hello SCSI-bussen hello:</span><span class="sxs-lookup"><span data-stu-id="25ac6-276">Use hello following command toorescan hello SCSI bus:</span></span>
  
    <span data-ttu-id="25ac6-277">`$ rescan-scsi-bus.sh `(en del av sg3_utils paketet)</span><span class="sxs-lookup"><span data-stu-id="25ac6-277">`$ rescan-scsi-bus.sh `(part of sg3_utils package)</span></span>
* <span data-ttu-id="25ac6-278">Skriv följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="25ac6-278">Type hello following commands:</span></span>
  
    `$ dmesg | grep sd*`
     
     <span data-ttu-id="25ac6-279">Eller</span><span class="sxs-lookup"><span data-stu-id="25ac6-279">Or</span></span>
  
    `$ fdisk –l`
  
    <span data-ttu-id="25ac6-280">Dessa kommer att returnera information om nyligen tillagda diskar.</span><span class="sxs-lookup"><span data-stu-id="25ac6-280">These will return details of recently added disks.</span></span>
* <span data-ttu-id="25ac6-281">toodetermine om det är en StorSimple-disk, Använd hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="25ac6-281">toodetermine whether it is a StorSimple disk, use hello following commands:</span></span>
  
    `cat /sys/block/<DISK>/device/model`
  
    <span data-ttu-id="25ac6-282">Detta returnerar en sträng som avgör om det är en StorSimple-disk.</span><span class="sxs-lookup"><span data-stu-id="25ac6-282">This will return a string, which will determine if it’s a StorSimple disk.</span></span>

<span data-ttu-id="25ac6-283">En mindre troligt men möjlig orsak kan också vara inaktuella iscsid pid.</span><span class="sxs-lookup"><span data-stu-id="25ac6-283">A less likely but possible cause could also be stale iscsid pid.</span></span> <span data-ttu-id="25ac6-284">Använd följande kommando toolog ut från iSCSI-sessioner för hello hello:</span><span class="sxs-lookup"><span data-stu-id="25ac6-284">Use hello following command toolog off from hello iSCSI sessions:</span></span>

    iscsiadm -m node --logout -p <Target_IP>

<span data-ttu-id="25ac6-285">Upprepa det här kommandot för alla hello anslutna nätverksgränssnitt på hello iSCSI-mål, vilket är din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="25ac6-285">Repeat this command for all hello connected network interfaces on hello iSCSI target, which is your StorSimple device.</span></span> <span data-ttu-id="25ac6-286">När du har loggat från alla hello iSCSI-sessioner, Använd hello iSCSI target IQN tooreestablish hello iSCSI-session.</span><span class="sxs-lookup"><span data-stu-id="25ac6-286">Once you have logged off from all hello iSCSI sessions, use hello iSCSI target IQN tooreestablish hello iSCSI session.</span></span> <span data-ttu-id="25ac6-287">Skriv följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="25ac6-287">Type hello following command:</span></span>

    iscsiadm -m node --login -T <TARGET_IQN>


<span data-ttu-id="25ac6-288">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="25ac6-288">Q.</span></span> <span data-ttu-id="25ac6-289">Jag är inte säker på om enheten är godkända.</span><span class="sxs-lookup"><span data-stu-id="25ac6-289">I am not sure if my device is whitelisted.</span></span>

<span data-ttu-id="25ac6-290">A.</span><span class="sxs-lookup"><span data-stu-id="25ac6-290">A.</span></span> <span data-ttu-id="25ac6-291">tooverify om enheten är godkända, Använd följande felsökning interaktiva kommandot hello:</span><span class="sxs-lookup"><span data-stu-id="25ac6-291">tooverify whether your device is whitelisted, use hello following troubleshooting interactive command:</span></span>

    multipathd –k
    multipathd> show devices
    available block devices:
    ram0 devnode blacklisted, unmonitored
    ram1 devnode blacklisted, unmonitored
    ram2 devnode blacklisted, unmonitored
    ram3 devnode blacklisted, unmonitored
    ram4 devnode blacklisted, unmonitored
    ram5 devnode blacklisted, unmonitored
    ram6 devnode blacklisted, unmonitored
    ram7 devnode blacklisted, unmonitored
    ram8 devnode blacklisted, unmonitored
    ram9 devnode blacklisted, unmonitored
    ram10 devnode blacklisted, unmonitored
    ram11 devnode blacklisted, unmonitored
    ram12 devnode blacklisted, unmonitored
    ram13 devnode blacklisted, unmonitored
    ram14 devnode blacklisted, unmonitored
    ram15 devnode blacklisted, unmonitored
    loop0 devnode blacklisted, unmonitored
    loop1 devnode blacklisted, unmonitored
    loop2 devnode blacklisted, unmonitored
    loop3 devnode blacklisted, unmonitored
    loop4 devnode blacklisted, unmonitored
    loop5 devnode blacklisted, unmonitored
    loop6 devnode blacklisted, unmonitored
    loop7 devnode blacklisted, unmonitored
    sr0 devnode blacklisted, unmonitored
    sda devnode whitelisted, monitored
    dm-0 devnode blacklisted, unmonitored
    dm-1 devnode blacklisted, unmonitored
    dm-2 devnode blacklisted, unmonitored
    sdb devnode whitelisted, monitored
    sdc devnode whitelisted, monitored
    dm-3 devnode blacklisted, unmonitored


<span data-ttu-id="25ac6-292">Mer information finns för[använda felsökning interaktiva kommandot för flera sökvägar](http://www.centos.org/docs/5/html/5.1/DM_Multipath/multipath_config_confirm.html).</span><span class="sxs-lookup"><span data-stu-id="25ac6-292">For more information, go too[use troubleshooting interactive command for multipathing](http://www.centos.org/docs/5/html/5.1/DM_Multipath/multipath_config_confirm.html).</span></span>

## <a name="list-of-useful-commands"></a><span data-ttu-id="25ac6-293">Lista över användbara kommandon</span><span class="sxs-lookup"><span data-stu-id="25ac6-293">List of useful commands</span></span>
| <span data-ttu-id="25ac6-294">Typ</span><span class="sxs-lookup"><span data-stu-id="25ac6-294">Type</span></span> | <span data-ttu-id="25ac6-295">Kommando</span><span class="sxs-lookup"><span data-stu-id="25ac6-295">Command</span></span> | <span data-ttu-id="25ac6-296">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="25ac6-296">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="25ac6-297">**iSCSI**</span><span class="sxs-lookup"><span data-stu-id="25ac6-297">**iSCSI**</span></span> |`service iscsid start` |<span data-ttu-id="25ac6-298">Starta iSCSI-tjänsten</span><span class="sxs-lookup"><span data-stu-id="25ac6-298">Start iSCSI service</span></span> |
| &nbsp; |`service iscsid stop` |<span data-ttu-id="25ac6-299">Stoppa tjänsten iSCSI</span><span class="sxs-lookup"><span data-stu-id="25ac6-299">Stop iSCSI service</span></span> |
| &nbsp; |`service iscsid restart` |<span data-ttu-id="25ac6-300">Starta om iSCSI-tjänsten</span><span class="sxs-lookup"><span data-stu-id="25ac6-300">Restart iSCSI service</span></span> |
| &nbsp; |`iscsiadm -m discovery -t sendtargets -p <TARGET_IP>` |<span data-ttu-id="25ac6-301">Identifiera tillgängliga mål på hello angetts adress</span><span class="sxs-lookup"><span data-stu-id="25ac6-301">Discover available targets on hello specified address</span></span> |
| &nbsp; |`iscsiadm -m node --login -T <TARGET_IQN>` |<span data-ttu-id="25ac6-302">Logga in toohello iSCSI-mål</span><span class="sxs-lookup"><span data-stu-id="25ac6-302">Log in toohello iSCSI target</span></span> |
| &nbsp; |`iscsiadm -m node --logout -p <Target_IP>` |<span data-ttu-id="25ac6-303">Logga ut från hello iSCSI-mål</span><span class="sxs-lookup"><span data-stu-id="25ac6-303">Log out from hello iSCSI target</span></span> |
| &nbsp; |`cat /etc/iscsi/initiatorname.iscsi` |<span data-ttu-id="25ac6-304">Namn på iSCSI-initierare</span><span class="sxs-lookup"><span data-stu-id="25ac6-304">Print iSCSI initiator name</span></span> |
| &nbsp; |`iscsiadm –m session –s <sessionid> -P 3` |<span data-ttu-id="25ac6-305">Kontrollera hello tillstånd på hello iSCSI-session och volym som identifieras på hello värden</span><span class="sxs-lookup"><span data-stu-id="25ac6-305">Check hello state of hello iSCSI session and volume discovered on hello host</span></span> |
| &nbsp; |`iscsi –m session` |<span data-ttu-id="25ac6-306">Visar alla hello iSCSI-sessioner mellan hello värden och hello StorSimple-enhet</span><span class="sxs-lookup"><span data-stu-id="25ac6-306">Shows all hello iSCSI sessions established between hello host and hello StorSimple device</span></span> |
|  | | |
| <span data-ttu-id="25ac6-307">**Flera sökvägar**</span><span class="sxs-lookup"><span data-stu-id="25ac6-307">**Multipathing**</span></span> |`service multipathd start` |<span data-ttu-id="25ac6-308">Starta daemon för flera sökvägar</span><span class="sxs-lookup"><span data-stu-id="25ac6-308">Start multipath daemon</span></span> |
| &nbsp; |`service multipathd stop` |<span data-ttu-id="25ac6-309">Stoppa multipath daemon</span><span class="sxs-lookup"><span data-stu-id="25ac6-309">Stop multipath daemon</span></span> |
| &nbsp; |`service multipathd restart` |<span data-ttu-id="25ac6-310">Starta om multipath daemon</span><span class="sxs-lookup"><span data-stu-id="25ac6-310">Restart multipath daemon</span></span> |
| &nbsp; |`chkconfig multipathd on` </br> <span data-ttu-id="25ac6-311">ELLER</span><span class="sxs-lookup"><span data-stu-id="25ac6-311">OR</span></span> </br> `mpathconf –with_chkconfig y` |<span data-ttu-id="25ac6-312">Aktivera multipath daemon toostart vid start</span><span class="sxs-lookup"><span data-stu-id="25ac6-312">Enable multipath daemon toostart at boot time</span></span> |
| &nbsp; |`multipathd –k` |<span data-ttu-id="25ac6-313">Starta hello interaktiva konsolen för felsökning</span><span class="sxs-lookup"><span data-stu-id="25ac6-313">Start hello interactive console for troubleshooting</span></span> |
| &nbsp; |`multipath –l` |<span data-ttu-id="25ac6-314">Lista över multipath anslutningar och enheter</span><span class="sxs-lookup"><span data-stu-id="25ac6-314">List multipath connections and devices</span></span> |
| &nbsp; |`mpathconf --enable` |<span data-ttu-id="25ac6-315">Skapa en exempelfil mulitpath.conf i`/etc/mulitpath.conf`</span><span class="sxs-lookup"><span data-stu-id="25ac6-315">Create a sample mulitpath.conf file in `/etc/mulitpath.conf`</span></span> |
|  | | |

## <a name="next-steps"></a><span data-ttu-id="25ac6-316">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="25ac6-316">Next steps</span></span>
<span data-ttu-id="25ac6-317">Du kanske också måste toorefer toohello följande CentoS 6.6 dokument som du konfigurerar MPIO på Linux-värd:</span><span class="sxs-lookup"><span data-stu-id="25ac6-317">As you are configuring MPIO on Linux host, you may also need toorefer toohello following CentoS 6.6 documents:</span></span>

* [<span data-ttu-id="25ac6-318">Hur du konfigurerar MPIO på CentOS</span><span class="sxs-lookup"><span data-stu-id="25ac6-318">Setting up MPIO on CentOS</span></span>](http://www.centos.org/docs/5/html/5.1/DM_Multipath/setup_procedure.html)
* [<span data-ttu-id="25ac6-319">Guide för Linux-utbildning</span><span class="sxs-lookup"><span data-stu-id="25ac6-319">Linux Training Guide</span></span>](http://linux-training.be/files/books/LinuxAdm.pdf)

