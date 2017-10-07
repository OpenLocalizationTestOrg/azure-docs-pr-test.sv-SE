---
title: aaaDiagnostics verktyget tootroubleshoot StorSimple 8000 enhet | Microsoft Docs
description: "Beskriver hello StorSimple enheten lägen och förklarar hur toouse Windows PowerShell för StorSimple toochange hello enhet-läge."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: alkohli
ms.openlocfilehash: e8b7fdbc44d2533973b63da841335ba73ba0014b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-diagnostics-tool-tootroubleshoot-8000-series-device-issues"></a><span data-ttu-id="f92f2-103">Använd hello StorSimple diagnostikverktyget tootroubleshoot 8000-serien problem med enheter</span><span class="sxs-lookup"><span data-stu-id="f92f2-103">Use hello StorSimple Diagnostics Tool tootroubleshoot 8000 series device issues</span></span>

## <a name="overview"></a><span data-ttu-id="f92f2-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="f92f2-104">Overview</span></span>

<span data-ttu-id="f92f2-105">Hej StorSimple diagnostikverktyget diagnostiserar problem relaterade toosystem, prestanda, nätverk och maskinvara komponentens hälsostatus för en StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="f92f2-105">hello StorSimple Diagnostics tool diagnoses issues related toosystem, performance, network, and hardware component health for a StorSimple device.</span></span> <span data-ttu-id="f92f2-106">hello diagnostikverktyget kan användas i olika scenarier.</span><span class="sxs-lookup"><span data-stu-id="f92f2-106">hello diagnostics tool can be used in various scenarios.</span></span> <span data-ttu-id="f92f2-107">Dessa scenarier som inkluderar arbetsbelastning planerar, distribuerar en StorSimple-enhet, bedöma hello nätverksmiljö och bestämmer hello resultatet av en operativa enhet.</span><span class="sxs-lookup"><span data-stu-id="f92f2-107">These scenarios include workload planning, deploying a StorSimple device, assessing hello network environment, and determining hello performance of an operational device.</span></span> <span data-ttu-id="f92f2-108">Den här artikeln innehåller en översikt över hello-diagnostik och beskriver hur hello verktyget kan användas med en StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="f92f2-108">This article provides an overview of hello diagnostics tool and describes how hello tool can be used with a StorSimple device.</span></span>

<span data-ttu-id="f92f2-109">hello-diagnostik är främst avsett för StorSimple 8000-serien lokala enheter (8100 och 8600).</span><span class="sxs-lookup"><span data-stu-id="f92f2-109">hello diagnostics tool is primarily intended for StorSimple 8000 series on-premises devices (8100 and 8600).</span></span>

## <a name="run-diagnostics-tool"></a><span data-ttu-id="f92f2-110">Kör diagnostik</span><span class="sxs-lookup"><span data-stu-id="f92f2-110">Run diagnostics tool</span></span>

<span data-ttu-id="f92f2-111">Det här verktyget kan köras via hello Windows PowerShell-gränssnittet för din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="f92f2-111">This tool can be run via hello Windows PowerShell interface of your StorSimple device.</span></span> <span data-ttu-id="f92f2-112">Det finns två sätt tooaccess hello lokala gränssnittet på din enhet:</span><span class="sxs-lookup"><span data-stu-id="f92f2-112">There are two ways tooaccess hello local interface of your device:</span></span>

* <span data-ttu-id="f92f2-113">[Använd PuTTY tooconnect toohello enhetens seriekonsol](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="f92f2-113">[Use PuTTY tooconnect toohello device serial console](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
* <span data-ttu-id="f92f2-114">[Fjärransluta till hello verktyget via hello Windows PowerShell för StorSimple](storsimple-remote-connect.md).</span><span class="sxs-lookup"><span data-stu-id="f92f2-114">[Remotely access hello tool via hello Windows PowerShell for StorSimple](storsimple-remote-connect.md).</span></span>

<span data-ttu-id="f92f2-115">I den här artikeln förutsätter vi att du har anslutit toohello enhetens seriekonsol via PuTTY.</span><span class="sxs-lookup"><span data-stu-id="f92f2-115">In this article, we assume that you have connected toohello device serial console via PuTTY.</span></span>

#### <a name="toorun-hello-diagnostics-tool"></a><span data-ttu-id="f92f2-116">toorun hello-diagnostik</span><span class="sxs-lookup"><span data-stu-id="f92f2-116">toorun hello diagnostics tool</span></span>

<span data-ttu-id="f92f2-117">När du har anslutit toohello Windows PowerShell-gränssnittet för hello enhet, utföra hello följande steg toorun hello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="f92f2-117">Once you have connected toohello Windows PowerShell interface of hello device, perform hello following steps toorun hello cmdlet.</span></span>
1. <span data-ttu-id="f92f2-118">Logga in toohello enhetens seriekonsol genom att följa stegen hello i [använda PuTTY tooconnect toohello enhetens seriekonsol](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="f92f2-118">Log on toohello device serial console by following hello steps in [Use PuTTY tooconnect toohello device serial console](storsimple-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console).</span></span>

2. <span data-ttu-id="f92f2-119">Skriv följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="f92f2-119">Type hello following command:</span></span>

    `Invoke-HcsDiagnostics`

    <span data-ttu-id="f92f2-120">Om hello omfattningsparametern inte anges körs alla hello diagnostiktest hello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="f92f2-120">If hello scope parameter is not specified, hello cmdlet executes all hello diagnostic tests.</span></span> <span data-ttu-id="f92f2-121">Dessa tester inkluderar system, komponenthälsa för maskinvara, nätverk och prestanda.</span><span class="sxs-lookup"><span data-stu-id="f92f2-121">These tests include system, hardware component health, network, and performance.</span></span> 
    
    <span data-ttu-id="f92f2-122">toorun endast specifika test, ange hello omfattningsparameter.</span><span class="sxs-lookup"><span data-stu-id="f92f2-122">toorun only a specific test, specify hello scope parameter.</span></span> <span data-ttu-id="f92f2-123">Till exempel toorun endast hello nätverkstest, typ</span><span class="sxs-lookup"><span data-stu-id="f92f2-123">For instance, toorun only hello network test, type</span></span>

    `Invoke-HcsDiagnostics -Scope Network`

3. <span data-ttu-id="f92f2-124">Markera och kopiera hello utdata från hello PuTTY fönstret till en textfil för ytterligare analys.</span><span class="sxs-lookup"><span data-stu-id="f92f2-124">Select and copy hello output from hello PuTTY window into a text file for further analysis.</span></span>

## <a name="scenarios-toouse-hello-diagnostics-tool"></a><span data-ttu-id="f92f2-125">Diagnostik för scenarier toouse hello</span><span class="sxs-lookup"><span data-stu-id="f92f2-125">Scenarios toouse hello diagnostics tool</span></span>

<span data-ttu-id="f92f2-126">Använd hello diagnostik verktyget tootroubleshoot hello nätverk, prestanda, system och maskinvara hälsotillstånd hello system.</span><span class="sxs-lookup"><span data-stu-id="f92f2-126">Use hello diagnostics tool tootroubleshoot hello network, performance, system, and hardware health of hello system.</span></span> <span data-ttu-id="f92f2-127">Här följer några möjliga scenarier:</span><span class="sxs-lookup"><span data-stu-id="f92f2-127">Here are some possible scenarios:</span></span>

* <span data-ttu-id="f92f2-128">**Enheten offline** -din StorSimple 8000-serien enheten är offline.</span><span class="sxs-lookup"><span data-stu-id="f92f2-128">**Device offline** - Your StorSimple 8000 series device is offline.</span></span> <span data-ttu-id="f92f2-129">Dock från hello Windows PowerShell-gränssnittet verkar att båda hello domänkontrollanter är igång.</span><span class="sxs-lookup"><span data-stu-id="f92f2-129">However, from hello Windows PowerShell interface, it seems that both hello controllers are up and running.</span></span>
    * <span data-ttu-id="f92f2-130">Du kan använda det här verktyget toothen bestämma hello nätverket tillstånd.</span><span class="sxs-lookup"><span data-stu-id="f92f2-130">You can use this tool toothen determine hello network state.</span></span>
         
         > [!NOTE]
         > <span data-ttu-id="f92f2-131">Använd inte det här verktyget tooassess prestanda- och nätverksinställningar på en enhet innan hello-registrering (eller konfigurera via installationsguiden).</span><span class="sxs-lookup"><span data-stu-id="f92f2-131">Do not use this tool tooassess performance and network settings on a device before hello registration (or configuring via setup wizard).</span></span> <span data-ttu-id="f92f2-132">En giltig IP-adress tilldelas toohello enhet under installationsguiden och registrering.</span><span class="sxs-lookup"><span data-stu-id="f92f2-132">A valid IP is assigned toohello device during setup wizard and registration.</span></span> <span data-ttu-id="f92f2-133">Du kan köra denna cmdlet på en enhet som inte är registrerad, för maskinvara hälso- och system.</span><span class="sxs-lookup"><span data-stu-id="f92f2-133">You can run this cmdlet, on a device that is not registered, for hardware health and system.</span></span> <span data-ttu-id="f92f2-134">Använd hello omfattningsparametern, till exempel:</span><span class="sxs-lookup"><span data-stu-id="f92f2-134">Use hello scope parameter, for example:</span></span>
         >
         > `Invoke-HcsDiagnostics -Scope Hardware`
         >
         > `Invoke-HcsDiagnostics -Scope System`

* <span data-ttu-id="f92f2-135">**Beständiga enhetsproblem** -du har enhetsproblem med som verkar vara toopersist.</span><span class="sxs-lookup"><span data-stu-id="f92f2-135">**Persistent device issues** - You are experiencing device issues that seem toopersist.</span></span> <span data-ttu-id="f92f2-136">Till exempel misslyckas registreringen.</span><span class="sxs-lookup"><span data-stu-id="f92f2-136">For instance, registration is failing.</span></span> <span data-ttu-id="f92f2-137">Du kan också ha problem med enhetsproblem när hello enheten är korrekt registrerade och fungerar på ett tag.</span><span class="sxs-lookup"><span data-stu-id="f92f2-137">You could also be experiencing device issues after hello device is successfully registered and operational for a while.</span></span>
    * <span data-ttu-id="f92f2-138">I detta fall kan använda verktyget för preliminär felsökning innan du loggar en tjänstbegäran med Microsoft-supporten.</span><span class="sxs-lookup"><span data-stu-id="f92f2-138">In this case, use this tool for preliminary troubleshooting before you log a service request with Microsoft Support.</span></span> <span data-ttu-id="f92f2-139">Vi rekommenderar att du kör det här verktyget och avbilda hello utdata från det här verktyget.</span><span class="sxs-lookup"><span data-stu-id="f92f2-139">We recommend that you run this tool and capture hello output of this tool.</span></span> <span data-ttu-id="f92f2-140">Sedan kan du ge den här utdata tooSupport tooexpedite felsökning.</span><span class="sxs-lookup"><span data-stu-id="f92f2-140">You can then provide this output tooSupport tooexpedite troubleshooting.</span></span>
    * <span data-ttu-id="f92f2-141">Om det finns några maskinvarufel för komponent eller ett kluster, bör du logga in en supportbegäran.</span><span class="sxs-lookup"><span data-stu-id="f92f2-141">If there are any hardware component or cluster failures, you should log in a Support request.</span></span>

* <span data-ttu-id="f92f2-142">**Lite Enhetsprestanda** -din StorSimple-enhet är långsam.</span><span class="sxs-lookup"><span data-stu-id="f92f2-142">**Low device performance** - Your StorSimple device is slow.</span></span>
    * <span data-ttu-id="f92f2-143">I det här fallet köra denna cmdlet med scope parametern set tooperformance.</span><span class="sxs-lookup"><span data-stu-id="f92f2-143">In this case, run this cmdlet with scope parameter set tooperformance.</span></span> <span data-ttu-id="f92f2-144">Analysera hello utdata.</span><span class="sxs-lookup"><span data-stu-id="f92f2-144">Analyze hello output.</span></span> <span data-ttu-id="f92f2-145">Du får hello molnet skrivskyddad svarstider.</span><span class="sxs-lookup"><span data-stu-id="f92f2-145">You get hello cloud read-write latencies.</span></span> <span data-ttu-id="f92f2-146">Använd hello rapporterade svarstiderna som maximalt hastigheterna mål, ta hänsyn till vissa kostnader för hello interna databearbetning och sedan distribuera hello arbetsbelastningar på hello system.</span><span class="sxs-lookup"><span data-stu-id="f92f2-146">Use hello reported latencies as maximum achievable target, factor in some overhead for hello internal data processing, and then deploy hello workloads on hello system.</span></span> <span data-ttu-id="f92f2-147">Mer information finns för[använda hello test tootroubleshoot enheten nätverksprestanda](#network-test).</span><span class="sxs-lookup"><span data-stu-id="f92f2-147">For more information, go too[Use hello network test tootroubleshoot device performance](#network-test).</span></span>


## <a name="diagnostics-test-and-sample-outputs"></a><span data-ttu-id="f92f2-148">Diagnostik för testning och exempel utdata</span><span class="sxs-lookup"><span data-stu-id="f92f2-148">Diagnostics test and sample outputs</span></span>

### <a name="hardware-test"></a><span data-ttu-id="f92f2-149">Maskinvarutest</span><span class="sxs-lookup"><span data-stu-id="f92f2-149">Hardware test</span></span>

<span data-ttu-id="f92f2-150">Det här testet hello status för maskinvarukomponenter hello, hello över inbyggd programvara och hello disk inbyggd programvara körs på datorn.</span><span class="sxs-lookup"><span data-stu-id="f92f2-150">This test determines hello status of hello hardware components, hello USM firmware, and hello disk firmware running on your system.</span></span>

* <span data-ttu-id="f92f2-151">hello maskinvarukomponenter rapporterade är de komponenterna som misslyckats hello-test eller finns inte i hello system.</span><span class="sxs-lookup"><span data-stu-id="f92f2-151">hello hardware components reported are those components that failed hello test or are not present in hello system.</span></span>
* <span data-ttu-id="f92f2-152">hello över inbyggd programvara och disk versioner av inbyggd rapporteras för hello styrenhet 0, 1 domänkontrollant, och delade komponenter i systemet.</span><span class="sxs-lookup"><span data-stu-id="f92f2-152">hello USM firmware and disk firmware versions are reported for hello Controller 0, Controller 1, and shared components in your system.</span></span> <span data-ttu-id="f92f2-153">En fullständig lista över maskinvarukomponenter finns:</span><span class="sxs-lookup"><span data-stu-id="f92f2-153">For a complete list of hardware components, go to:</span></span>

    * [<span data-ttu-id="f92f2-154">Komponenter i primära hölje</span><span class="sxs-lookup"><span data-stu-id="f92f2-154">Components in primary enclosure</span></span>](storsimple-monitor-hardware-status.md#component-list-for-primary-enclosure-of-storsimple-device)
    * [<span data-ttu-id="f92f2-155">Komponenter i EBOD hölje</span><span class="sxs-lookup"><span data-stu-id="f92f2-155">Components in EBOD enclosure</span></span>](storsimple-monitor-hardware-status.md#component-list-for-ebod-enclosure-of-storsimple-device)

> [!NOTE]
> <span data-ttu-id="f92f2-156">Om hello maskinvarutest rapporterar misslyckade komponenter, [logga in en tjänstbegäran med Microsoft-supporten](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="f92f2-156">If hello hardware test reports failed components, [log in a service request with Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>

#### <a name="sample-output-of-hardware-test-run-on-an-8100-device"></a><span data-ttu-id="f92f2-157">Exempel på utdata av maskinvarutest som körs på en 8100-enhet</span><span class="sxs-lookup"><span data-stu-id="f92f2-157">Sample output of hardware test run on an 8100 device</span></span>

<span data-ttu-id="f92f2-158">Här är ett exempel på utdata från en StorSimple 8100-enhet.</span><span class="sxs-lookup"><span data-stu-id="f92f2-158">Here is a sample output from a StorSimple 8100 device.</span></span> <span data-ttu-id="f92f2-159">Hello 8100 modellen enhet finns hello EBOD hölje inte.</span><span class="sxs-lookup"><span data-stu-id="f92f2-159">In hello 8100 model device, hello EBOD enclosure is not present.</span></span> <span data-ttu-id="f92f2-160">Därför rapporteras hello EBOD domänkontrollant komponenter inte.</span><span class="sxs-lookup"><span data-stu-id="f92f2-160">Hence, hello EBOD controller components are not reported.</span></span>

```
Controller0>Invoke-HcsDiagnostics -Scope Hardware
Running hardware diagnostics ...
--------------------------------------------------
Hardware components failed or not present
----------------------

           Type           State      Controller           Index     EnclosureId
           ----           -----      ----------           -----     -----------
...rVipResource      NotPresent            None               1            None
...rVipResource      NotPresent            None               2            None
...rVipResource      NotPresent            None               3            None
...rVipResource      NotPresent            None               4            None
...rVipResource      NotPresent            None               5            None
...rVipResource      NotPresent            None               6            None
...rVipResource      NotPresent            None               7            None
...rVipResource      NotPresent            None               8            None
...rVipResource      NotPresent            None               9            None
...rVipResource      NotPresent            None              10            None
...rVipResource      NotPresent            None              11            None

Firmware information
----------------------
TalladegaController : ActiveBIOS:0.45.0010
                      BackupBIOS:0.45.0006
                      MainCPLD:17.0.000b
                      ActiveBMCRoot:2.0.001F
                      BackupBMCRoot:2.0.001F
                      BMCBoot:2.0.0002
                      LsiFirmware:20.00.04.00
                      LsiBios:07.37.00.00
                      Battery1Firmware:06.2C
                      Battery2Firmware:06.2C
                      DomFirmware:X231600
                      CanisterFirmware:3.5.0.56
                      CanisterBootloader:5.03
                      CanisterConfigCRC:0x9134777A
                      CanisterVPDStructure:0x06
                      CanisterGEMCPLD:0x19
                      CanisterVPDCRC:0x142F7DC2
                      MidplaneVPDStructure:0x0C
                      MidplaneVPDCRC:0xA6BD4F64
                      MidplaneCPLD:0x10
                      PCM1Firmware:1.00|1.05
                      PCM1VPDStructure:0x05
                      PCM1VPDCRC:0x41BEF99C
                      PCM2Firmware:1.00|1.05
                      PCM2VPDStructure:0x05
                      PCM2VPDCRC:0x41BEF99C

EbodController      :
DisksFirmware       : SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08

TalladegaController : ActiveBIOS:0.45.0010
                      BackupBIOS:0.45.0006
                      MainCPLD:17.0.000b
                      ActiveBMCRoot:2.0.001F
                      BackupBMCRoot:2.0.001F
                      BMCBoot:2.0.0002
                      LsiFirmware:20.00.04.00
                      LsiBios:07.37.00.00
                      Battery1Firmware:06.2C
                      Battery2Firmware:06.2C
                      DomFirmware:X231600
                      CanisterFirmware:3.5.0.56
                      CanisterBootloader:5.03
                      CanisterConfigCRC:0x9134777A
                      CanisterVPDStructure:0x06
                      CanisterGEMCPLD:0x19
                      CanisterVPDCRC:0x142F7DC2
                      MidplaneVPDStructure:0x0C
                      MidplaneVPDCRC:0xA6BD4F64
                      MidplaneCPLD:0x10
                      PCM1Firmware:1.00|1.05
                      PCM1VPDStructure:0x05
                      PCM1VPDCRC:0x41BEF99C
                      PCM2Firmware:1.00|1.05
                      PCM2VPDStructure:0x05
                      PCM2VPDCRC:0x41BEF99C

EbodController      :
DisksFirmware       : SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      SmrtStor:TXA2D20400GA6XYR:KZ50
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08
                      WD:WD4001FYYG-01SL3:VR08

--------------------------------------------------
```

### <a name="system-test"></a><span data-ttu-id="f92f2-161">System-test</span><span class="sxs-lookup"><span data-stu-id="f92f2-161">System test</span></span>

<span data-ttu-id="f92f2-162">Det här testet rapporterar hello Systeminformation, hello tillgängliga uppdateringar, hello klusterinformation och hello tjänstinformation för din enhet.</span><span class="sxs-lookup"><span data-stu-id="f92f2-162">This test reports hello system information, hello updates available, hello cluster information, and hello service information for your device.</span></span>

* <span data-ttu-id="f92f2-163">hello Systeminformation innehåller hello modellen, enhetens serienummer, tidszon, status för domänkontrollanten och detaljerad hello programvaruversion som körs på hello system.</span><span class="sxs-lookup"><span data-stu-id="f92f2-163">hello system information includes hello model, device serial number, time zone, controller status, and hello detailed software version running on hello system.</span></span> <span data-ttu-id="f92f2-164">toounderstand hello olika systemparametrar rapporteras som hello utdata gå för[tolka Systeminformation](#appendix-interpreting-system-information).</span><span class="sxs-lookup"><span data-stu-id="f92f2-164">toounderstand hello various system parameters reported as hello output, go too[Interpreting system information](#appendix-interpreting-system-information).</span></span>

* <span data-ttu-id="f92f2-165">hello update tillgänglighet rapporter om hello regular och underhåll lägen är tillgängliga och deras associerade paketnamn.</span><span class="sxs-lookup"><span data-stu-id="f92f2-165">hello update availability reports whether hello regular and maintenance modes are available and their associated package names.</span></span> <span data-ttu-id="f92f2-166">Om `RegularUpdates` och `MaintenanceModeUpdates` är `false`, detta indikerar att hello uppdateringar inte är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="f92f2-166">If `RegularUpdates` and `MaintenanceModeUpdates` are `false`, this indicates that hello updates are not available.</span></span> <span data-ttu-id="f92f2-167">Enheten är uppdaterad.</span><span class="sxs-lookup"><span data-stu-id="f92f2-167">Your device is up-to-date.</span></span>
* <span data-ttu-id="f92f2-168">hello klusterinformation innehåller hello information om olika logiska komponenter till alla hello HCS klustergrupper och deras respektive status.</span><span class="sxs-lookup"><span data-stu-id="f92f2-168">hello cluster information contains hello information on various logical components of all hello HCS cluster groups and their respective statuses.</span></span> <span data-ttu-id="f92f2-169">Om du ser ett offline klustergrupp i det här avsnittet av hello rapport [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md).</span><span class="sxs-lookup"><span data-stu-id="f92f2-169">If you see an offline cluster group in this section of hello report, [contact Microsoft Support](storsimple-contact-microsoft-support.md).</span></span>
* <span data-ttu-id="f92f2-170">hello tjänstinformation innehåller hello namn och status för alla hello HCS och TIS-tjänster som körs på enheten.</span><span class="sxs-lookup"><span data-stu-id="f92f2-170">hello service information includes hello names and statuses of all hello HCS and CiS services running on your device.</span></span> <span data-ttu-id="f92f2-171">Den här informationen är användbart för hello Microsoft-supporten felsöka hello enheten problemet.</span><span class="sxs-lookup"><span data-stu-id="f92f2-171">This information is helpful for hello Microsoft Support in troubleshooting hello device issue.</span></span>

#### <a name="sample-output-of-system-test-run-on-an-8100-device"></a><span data-ttu-id="f92f2-172">Exempel på utdata från system-test som körs på en 8100-enhet</span><span class="sxs-lookup"><span data-stu-id="f92f2-172">Sample output of system test run on an 8100 device</span></span>

<span data-ttu-id="f92f2-173">Här är ett exempel på utdata från hello system test som körs på en 8100-enhet.</span><span class="sxs-lookup"><span data-stu-id="f92f2-173">Here is a sample output of hello system test run on an 8100 device.</span></span>

```
Controller0>Invoke-HcsDiagnostics -Scope System
Running system diagnostics ...
--------------------------------------------------

System information
----------------------
Controller0:

InstanceId              : 7382407f-a56b-4622-8f3f-756fe04cfd38
Name                    : 8100-SHX0991003G467K
Model                   : 8100
SerialNumber            : SHX0991003G467K
TimeZone                : (UTC-08:00) Pacific Time (US & Canada)
CurrentController       : Controller0
ActiveController        : Controller0
Controller0Status       : Normal
Controller1Status       : Normal
SystemMode              : Normal
FriendlySoftwareVersion : StorSimple 8000 Series Update 4.0
HcsSoftwareVersion      : 6.3.9600.17820
ApiVersion              : 9.0.0.0
VhdVersion              : 6.3.9600.17759
OSVersion               : 6.3.9600.0
CisAgentVersion         : 1.0.9441.0
MdsAgentVersion         : 35.2.2.0
Lsisas2Version          : 2.0.78.0
Capacity                : 219902325555200
RemoteManagementMode    : Disabled
FipsMode                : Enabled

Controller1:
InstanceId              : 7382407f-a56b-4622-8f3f-756fe04cfd38
Name                    : 8100-SHX0991003G467K
Model                   : 8100
SerialNumber            : SHX0991003G467K
TimeZone                :
CurrentController       : Controller0
ActiveController        : Controller0
Controller0Status       : Normal
Controller1Status       : Normal
SystemMode              : Normal
FriendlySoftwareVersion : StorSimple 8000 Series Update 4.0
HcsSoftwareVersion      : 6.3.9600.17820
ApiVersion              : 9.0.0.0
VhdVersion              : 6.3.9600.17759
OSVersion               : 6.3.9600.0
CisAgentVersion         : 1.0.9441.0
MdsAgentVersion         : 35.2.2.0
Lsisas2Version          : 2.0.78.0
Capacity                : 219902325555200
RemoteManagementMode    : HttpsAndHttpEnabled
FipsMode                : Enabled

Update availability
----------------------
RegularUpdates              : False
MaintenanceModeUpdates      : False
RegularUpdatesTitle         : {}
MaintenanceModeUpdatesTitle : {}

Cluster information
----------------------
Name                          State OwnerGroup
----                          ----- ----------
ApplicationHostRLUA           Online HCS Cluster Group
Data0v4                       Online HCS Cluster Group
HCS Vnic Resource             Online HCS Cluster Group
hcs_cloud_connectivity_...    Online HCS Cluster Group
hcs_controller_replacement    Online HCS Cluster Group
hcs_datapath_service          Online HCS Cluster Group
hcs_management_servic         Online HCS Cluster Group
hcs_nvram_service             Online HCS Cluster Group
hcs_passive_datapath          Online HCS Passive Cluster Group
hcs_platform_service          Online HCS Cluster Group
hcs_saas_agent_service        Online HCS Cluster Group
HddDataClusterDisk            Online HCS Cluster Group
HddMgmtClusterDisk            Online HCS Cluster Group
HddReplClusterDisk            Online HCS Cluster Group
iSCSI Target Server           Online HCS Cluster Group
NvramClusterDisk              Online HCS Cluster Group
SSAdminRLUA                   Online HCS Cluster Group
SsdDataClusterDisk            Online HCS Cluster Group
SsdNvramClusterDisk           Online HCS Cluster Group

Service information
----------------------
Name                                          Status DisplayName
----                                          ------ -----------
CiSAgentSvc                                   Stopped CiS Service Agent
hcs_cloud_connectivity_...                    Running hcs_cloud_connectivity...
hcs_controller_replacement                    Running HCS Controller Replace...
hcs_datapath_service                          Running HCS Datapath Service
hcs_management_service                        Running HCS Management Service
hcs_minishell                                 Running hcs_minishell
HCS_NVRAM_Service                             Running HCS NVRAM Service
hcs_passive_datapath                          Stopped HCS Passive Datapath S...
hcs_platform_service                          Running HCS Platform Monitor S...
hcs_saas_agent_service                        Running hcs_saas_agent_service
hcs_startup                                   Stopped hcs_startup
--------------------------------------------------
```

### <a name="network-test"></a><span data-ttu-id="f92f2-174">Nätverkstest</span><span class="sxs-lookup"><span data-stu-id="f92f2-174">Network test</span></span>

<span data-ttu-id="f92f2-175">Det här testet kontrollerar hello status för hello nätverksgränssnitt, portar, DNS och NTP serveranslutning, SSL certifikat, lagringskontouppgifter, anslutning toohello Update-servrar och proxy webbanslutningen på StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="f92f2-175">This test validates hello status of hello network interfaces, ports, DNS and NTP server connectivity, SSL certificate, storage account credentials, connectivity toohello Update servers, and web proxy connectivity on your StorSimple device.</span></span>

#### <a name="sample-output-of-network-test-when-only-data0-is-enabled"></a><span data-ttu-id="f92f2-176">Exempel på utdata för nätverket testa när endast DATA0 är aktiverad</span><span class="sxs-lookup"><span data-stu-id="f92f2-176">Sample output of network test when only DATA0 is enabled</span></span>

<span data-ttu-id="f92f2-177">Här är ett exempel på utdata för hello 8100-enhet.</span><span class="sxs-lookup"><span data-stu-id="f92f2-177">Here is a sample output of hello 8100 device.</span></span> <span data-ttu-id="f92f2-178">Du kan se i hello utdata som:</span><span class="sxs-lookup"><span data-stu-id="f92f2-178">You can see in hello output that:</span></span>
* <span data-ttu-id="f92f2-179">Endast DATA 0 och DATA 1 nätverksgränssnitt är aktiverad och konfigurerad.</span><span class="sxs-lookup"><span data-stu-id="f92f2-179">Only DATA 0 and DATA 1 network interfaces are enabled and configured.</span></span>
* <span data-ttu-id="f92f2-180">DATA 2-5 är inte aktiverade i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="f92f2-180">DATA 2 - 5 are not enabled in hello portal.</span></span>
* <span data-ttu-id="f92f2-181">hello DNS-serverkonfigurationen är giltig och hello enhet kan ansluta via hello DNS-server.</span><span class="sxs-lookup"><span data-stu-id="f92f2-181">hello DNS server configuration is valid and hello device can connect via hello DNS server.</span></span>
* <span data-ttu-id="f92f2-182">hello NTP-serveranslutning är också bra.</span><span class="sxs-lookup"><span data-stu-id="f92f2-182">hello NTP server connectivity is also fine.</span></span>
* <span data-ttu-id="f92f2-183">Portarna 80 och 443 är öppna.</span><span class="sxs-lookup"><span data-stu-id="f92f2-183">Ports 80 and 443 are open.</span></span> <span data-ttu-id="f92f2-184">Dock är port 9354 blockerad.</span><span class="sxs-lookup"><span data-stu-id="f92f2-184">However, port 9354 is blocked.</span></span> <span data-ttu-id="f92f2-185">Baserat på hello [system nätverkskrav](storsimple-system-requirements.md), behöver du tooopen den här porten för hello service bus-kommunikation.</span><span class="sxs-lookup"><span data-stu-id="f92f2-185">Based on hello [system network requirements](storsimple-system-requirements.md), you need tooopen this port for hello service bus communication.</span></span>
* <span data-ttu-id="f92f2-186">hello SSL-certifikat är giltigt.</span><span class="sxs-lookup"><span data-stu-id="f92f2-186">hello SSL certification is valid.</span></span>
* <span data-ttu-id="f92f2-187">hello-enhet kan ansluta toohello lagringskonto: _myss8000storageacct_.</span><span class="sxs-lookup"><span data-stu-id="f92f2-187">hello device can connect toohello storage account: _myss8000storageacct_.</span></span>
* <span data-ttu-id="f92f2-188">hello anslutningen tooUpdate servrar är giltig.</span><span class="sxs-lookup"><span data-stu-id="f92f2-188">hello connectivity tooUpdate servers is valid.</span></span>
* <span data-ttu-id="f92f2-189">hello webbproxy har inte konfigurerats på den här enheten.</span><span class="sxs-lookup"><span data-stu-id="f92f2-189">hello web proxy is not configured on this device.</span></span>

#### <a name="sample-output-of-network-test-when-data0-and-data1-are-enabled"></a><span data-ttu-id="f92f2-190">Utdata från nätverkstest när DATA0 och fil1 har aktiverats</span><span class="sxs-lookup"><span data-stu-id="f92f2-190">Sample output of network test when DATA0 and DATA1 are enabled</span></span>

```
Controller0>Invoke-HcsDiagnostics -Scope Network
Running network diagnostics ....
--------------------------------------------------
Validating networks .....
Name                Entity              Result              Details
----                ------              ------              -------
Network interface   Data0               Valid
Network interface   Data1               Valid
Network interface   Data2               Not enabled
Network interface   Data3               Not enabled
Network interface   Data4               Not enabled
Network interface   Data5               Not enabled
DNS                 10.222.118.154      Valid
NTP                 time.windows.com    Valid
Port                80                  Open
Port                443                 Open
Port                9354                Blocked
SSL certificate     https://myss8000... Valid
Storage account ... myss8000storageacct Valid
URL                 http://download.... Valid
URL                 http://download.... Valid
Web proxy                               Not enabled         Web proxy is not...
--------------------------------------------------
```

### <a name="performance-test"></a><span data-ttu-id="f92f2-191">Prestandatest</span><span class="sxs-lookup"><span data-stu-id="f92f2-191">Performance test</span></span>

<span data-ttu-id="f92f2-192">Det här testet rapporterar hello molnet prestanda via hello molnet skrivskyddad svarstiderna för din enhet.</span><span class="sxs-lookup"><span data-stu-id="f92f2-192">This test reports hello cloud performance via hello cloud read-write latencies for your device.</span></span> <span data-ttu-id="f92f2-193">Det här verktyget kan vara används tooestablish en baslinje för hello molnet prestanda som du kan uppnå med StorSimple.</span><span class="sxs-lookup"><span data-stu-id="f92f2-193">This tool can be used tooestablish a baseline of hello cloud performance that you can achieve with StorSimple.</span></span> <span data-ttu-id="f92f2-194">hello verktyget rapporter hello maximala prestanda (bästa möjliga scenario för skrivskyddad svarstiderna) som du kan hämta för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="f92f2-194">hello tool reports hello maximum performance (best case scenario for read-write latencies) that you can get for your connection.</span></span>

<span data-ttu-id="f92f2-195">Som hello rapporteras hello högsta prestanda, kan vi använda hello rapporterade svarstiderna skrivskyddad som mål när du distribuerar hello arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="f92f2-195">As hello tool reports hello maximum achievable performance, we can use hello reported read-write latencies as targets when deploying hello workloads.</span></span>

<span data-ttu-id="f92f2-196">hello test simulerar hello blob-storlekar som är associerade med hello annan volymtyper på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="f92f2-196">hello test simulates hello blob sizes associated with hello different volume types on hello device.</span></span> <span data-ttu-id="f92f2-197">Vanliga nivåer och säkerhetskopior av lokalt fästa volymer använder en 64 KB blob-storlek.</span><span class="sxs-lookup"><span data-stu-id="f92f2-197">Regular tiered and backups of locally pinned volumes use a 64 KB blob size.</span></span> <span data-ttu-id="f92f2-198">Nivåindelade volymer med Arkiv alternativet markerat använda 512 KB storleken för blob-data.</span><span class="sxs-lookup"><span data-stu-id="f92f2-198">Tiered volumes with archive option checked use 512 KB blob data size.</span></span> <span data-ttu-id="f92f2-199">Om enheten har konfigurerade nivåindelade och lokalt fästa volymer endast hello test motsvarande too64 KB blob datastorleken körs.</span><span class="sxs-lookup"><span data-stu-id="f92f2-199">If your device has tiered and locally pinned volumes configured, only hello test corresponding too64 KB blob data size is run.</span></span>

<span data-ttu-id="f92f2-200">toouse det här verktyget utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f92f2-200">toouse this tool, perform hello following steps:</span></span>

1.  <span data-ttu-id="f92f2-201">Skapa först en blandning av nivåindelade volymer och nivåindelade volymer med arkiverade alternativet är markerat.</span><span class="sxs-lookup"><span data-stu-id="f92f2-201">First, create a mix of tiered volumes and tiered volumes with archived option checked.</span></span> <span data-ttu-id="f92f2-202">Den här åtgärden säkerställer att hello-verktyget körs hello tester för både 64 KB och 512 KB blob-storlekar.</span><span class="sxs-lookup"><span data-stu-id="f92f2-202">This action ensures that hello tool runs hello tests for both 64 KB and 512 KB blob sizes.</span></span>

2. <span data-ttu-id="f92f2-203">Kör hello cmdlet när du har skapat och konfigurerat hello volymer.</span><span class="sxs-lookup"><span data-stu-id="f92f2-203">Run hello cmdlet after you have created and configured hello volumes.</span></span> <span data-ttu-id="f92f2-204">Ange:</span><span class="sxs-lookup"><span data-stu-id="f92f2-204">Type:</span></span>

    `Invoke-HcsDiagnostics -Scope Performance`

3. <span data-ttu-id="f92f2-205">Anteckna hello skrivskyddad fördröjningar som rapporterats av hello-verktyget.</span><span class="sxs-lookup"><span data-stu-id="f92f2-205">Make a note of hello read-write latencies reported by hello tool.</span></span> <span data-ttu-id="f92f2-206">Det här testet kan ta flera minuter toorun innan den rapporterar hello resultat.</span><span class="sxs-lookup"><span data-stu-id="f92f2-206">This test can take several minutes toorun before it reports hello results.</span></span>

4. <span data-ttu-id="f92f2-207">Om hello anslutning svarstiderna är alla under hello förväntade intervall och hello fördröjningar som rapporterats av hello verktyget kan användas som maximalt hastigheterna mål när du distribuerar hello arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="f92f2-207">If hello connection latencies are all under hello expected range, then hello latencies reported by hello tool can be used as maximum achievable target when deploying hello workloads.</span></span> <span data-ttu-id="f92f2-208">Ta hänsyn till vissa kostnader för interna databearbetning.</span><span class="sxs-lookup"><span data-stu-id="f92f2-208">Factor in some overhead for internal data processing.</span></span>

    <span data-ttu-id="f92f2-209">Hello-diagnostik är hög om hello skrivskyddad fördröjningar som rapporterats av:</span><span class="sxs-lookup"><span data-stu-id="f92f2-209">If hello read-write latencies reported by hello diagnostics tool are high:</span></span>

    1. <span data-ttu-id="f92f2-210">Konfigurera Storage Analytics för blob-tjänster och analysera hello utdata toounderstand hello svarstiderna för hello Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="f92f2-210">Configure Storage Analytics for blob services and analyze hello output toounderstand hello latencies for hello Azure storage account.</span></span> <span data-ttu-id="f92f2-211">Detaljerade anvisningar finns för[aktivera och konfigurera Storage Analytics](../storage/common/storage-enable-and-view-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="f92f2-211">For detailed instructions, go too[enable and configure Storage Analytics](../storage/common/storage-enable-and-view-metrics.md).</span></span> <span data-ttu-id="f92f2-212">Om dessa svarstider är hög och jämförbara toohello siffror som du har fått från hello StorSimple-diagnostik, måste toolog en tjänstbegäran med Azure storage.</span><span class="sxs-lookup"><span data-stu-id="f92f2-212">If those latencies are also high and comparable toohello numbers you received from hello StorSimple Diagnostics tool, then you need toolog a service request with Azure storage.</span></span>

    2. <span data-ttu-id="f92f2-213">Om hello storage-konto svarstiderna låg kontaktar du din nätverket administratören tooinvestigate svarstid för eventuella problem i nätverket.</span><span class="sxs-lookup"><span data-stu-id="f92f2-213">If hello storage account latencies are low, contact your network administrator tooinvestigate any latency issues in your network.</span></span>

#### <a name="sample-output-of-performance-test-run-on-an-8100-device"></a><span data-ttu-id="f92f2-214">Exempel på utdata av prestandatest som körs på en 8100-enhet</span><span class="sxs-lookup"><span data-stu-id="f92f2-214">Sample output of performance test run on an 8100 device</span></span>

```
Controller0>Invoke-HcsDiagnostics -Scope Performance
Running performance diagnostics...
--------------------------------------------------
Cloud performance: writing blobs
Cloud write latency: 194 ms using credential 'myss8000storageacct', blob size '64KB'
Cloud performance: reading blobs..
Cloud read latency: 544 ms using credential 'myss8000storageacct', blob size '64KB'
Cloud performance: writing blobs.
Cloud write latency: 369 ms using credential 'myss8000storageacct', blob size '512KB'
Cloud performance: reading blobs...
Cloud read latency: 4924 ms using credential 'myss8000storageacct', blob size '512KB'
--------------------------------------------------
Controller0>
```

## <a name="appendix-interpreting-system-information"></a><span data-ttu-id="f92f2-215">Bilaga: tolka Systeminformation</span><span class="sxs-lookup"><span data-stu-id="f92f2-215">Appendix: interpreting system information</span></span>

<span data-ttu-id="f92f2-216">Här är en tabell som beskriver vilka hello olika Windows PowerShell-parametrar i hello Systeminformation mappa till.</span><span class="sxs-lookup"><span data-stu-id="f92f2-216">Here is a table describing what hello various Windows PowerShell parameters in hello system information map to.</span></span> 

| <span data-ttu-id="f92f2-217">PowerShell-Parameter</span><span class="sxs-lookup"><span data-stu-id="f92f2-217">PowerShell Parameter</span></span>    | <span data-ttu-id="f92f2-218">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f92f2-218">Description</span></span>  |
|-------------------------|------------------|
| <span data-ttu-id="f92f2-219">Instans-ID</span><span class="sxs-lookup"><span data-stu-id="f92f2-219">Instance ID</span></span>             | <span data-ttu-id="f92f2-220">Varje domänkontrollant har en unik identifierare eller ett GUID som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="f92f2-220">Every controller has a unique identifier or a GUID associated with it.</span></span>|
| <span data-ttu-id="f92f2-221">Namn</span><span class="sxs-lookup"><span data-stu-id="f92f2-221">Name</span></span>                    | <span data-ttu-id="f92f2-222">hello eget namn på hello enheten som konfigurerats via hello Azure-portalen under distributionen av enheten.</span><span class="sxs-lookup"><span data-stu-id="f92f2-222">hello friendly name of hello device as configured through hello Azure portal during device deployment.</span></span> <span data-ttu-id="f92f2-223">eget namn för hello standard är hello enhetens serienummer.</span><span class="sxs-lookup"><span data-stu-id="f92f2-223">hello default friendly name is hello device serial number.</span></span> |
| <span data-ttu-id="f92f2-224">Modellen</span><span class="sxs-lookup"><span data-stu-id="f92f2-224">Model</span></span>                   | <span data-ttu-id="f92f2-225">hello modell för enheten StorSimple 8000-serien.</span><span class="sxs-lookup"><span data-stu-id="f92f2-225">hello model of your StorSimple 8000 series device.</span></span> <span data-ttu-id="f92f2-226">hello modell kan vara 8100 eller 8600.</span><span class="sxs-lookup"><span data-stu-id="f92f2-226">hello model can be 8100 or 8600.</span></span>|
| <span data-ttu-id="f92f2-227">Serienummer</span><span class="sxs-lookup"><span data-stu-id="f92f2-227">SerialNumber</span></span>            | <span data-ttu-id="f92f2-228">hello enhetens serienummer som tilldelas i hello fabriken och 15 tecken.</span><span class="sxs-lookup"><span data-stu-id="f92f2-228">hello device serial number is assigned at hello factory and is 15 characters long.</span></span> <span data-ttu-id="f92f2-229">Exempelvis anger 8600-SHX0991003G44HT:</span><span class="sxs-lookup"><span data-stu-id="f92f2-229">For instance, 8600-SHX0991003G44HT indicates:</span></span><br> <span data-ttu-id="f92f2-230">8600 – är hello enhetsmodell.</span><span class="sxs-lookup"><span data-stu-id="f92f2-230">8600 – Is hello device model.</span></span><br><span data-ttu-id="f92f2-231">SHX – är hello tillverkar plats.</span><span class="sxs-lookup"><span data-stu-id="f92f2-231">SHX – Is hello manufacturing site.</span></span><br> <span data-ttu-id="f92f2-232">0991003 - är en specifik produkt.</span><span class="sxs-lookup"><span data-stu-id="f92f2-232">0991003 - Is a specific product.</span></span> <br> <span data-ttu-id="f92f2-233">G44HT hello 5 sista siffrorna är stegvis toocreate unika serienummer.</span><span class="sxs-lookup"><span data-stu-id="f92f2-233">G44HT- hello last 5 digits are incremented toocreate unique serial numbers.</span></span> <span data-ttu-id="f92f2-234">Det får inte vara en sekventiell uppsättning.</span><span class="sxs-lookup"><span data-stu-id="f92f2-234">This may not be a sequential set.</span></span>|
| <span data-ttu-id="f92f2-235">Tidszon</span><span class="sxs-lookup"><span data-stu-id="f92f2-235">TimeZone</span></span>                | <span data-ttu-id="f92f2-236">hello enheten tidszon som konfigurerats i hello Azure-portalen under distributionen av enheten.</span><span class="sxs-lookup"><span data-stu-id="f92f2-236">hello device time zone as configured in hello Azure portal during device deployment.</span></span>|
| <span data-ttu-id="f92f2-237">CurrentController</span><span class="sxs-lookup"><span data-stu-id="f92f2-237">CurrentController</span></span>       | <span data-ttu-id="f92f2-238">hello-domänkontrollant som du är ansluten toothrough hello Windows PowerShell-gränssnittet för din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="f92f2-238">hello controller that you are connected toothrough hello Windows PowerShell interface of your StorSimple device.</span></span>|
| <span data-ttu-id="f92f2-239">ActiveController</span><span class="sxs-lookup"><span data-stu-id="f92f2-239">ActiveController</span></span>        | <span data-ttu-id="f92f2-240">hello-domänkontrollant som är aktiv på enheten och kontrollerar alla hello nätverks- och åtgärder.</span><span class="sxs-lookup"><span data-stu-id="f92f2-240">hello controller that is active on your device and is controlling all hello network and disk operations.</span></span> <span data-ttu-id="f92f2-241">Detta kan vara styrenhet 0 eller 1 för domänkontrollanten.</span><span class="sxs-lookup"><span data-stu-id="f92f2-241">This can be Controller 0 or Controller 1.</span></span>  |
| <span data-ttu-id="f92f2-242">Controller0Status</span><span class="sxs-lookup"><span data-stu-id="f92f2-242">Controller0Status</span></span>       | <span data-ttu-id="f92f2-243">hello status för styrenhet 0 på enheten.</span><span class="sxs-lookup"><span data-stu-id="f92f2-243">hello status of Controller 0 on your device.</span></span> <span data-ttu-id="f92f2-244">status för hello domänkontrollanten kan vara normal, i återställningsläge eller kan inte nås.</span><span class="sxs-lookup"><span data-stu-id="f92f2-244">hello controller status can be normal, in recovery mode, or unreachable.</span></span>|
| <span data-ttu-id="f92f2-245">Controller1Status</span><span class="sxs-lookup"><span data-stu-id="f92f2-245">Controller1Status</span></span>       | <span data-ttu-id="f92f2-246">hello status för styrenheten 1 på enheten.</span><span class="sxs-lookup"><span data-stu-id="f92f2-246">hello status of Controller 1 on your device.</span></span>  <span data-ttu-id="f92f2-247">status för hello domänkontrollanten kan vara normal, i återställningsläge eller kan inte nås.</span><span class="sxs-lookup"><span data-stu-id="f92f2-247">hello controller status can be normal, in recovery mode, or unreachable.</span></span>|
| <span data-ttu-id="f92f2-248">SystemMode</span><span class="sxs-lookup"><span data-stu-id="f92f2-248">SystemMode</span></span>              | <span data-ttu-id="f92f2-249">Hej övergripande status för din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="f92f2-249">hello overall status of your StorSimple device.</span></span> <span data-ttu-id="f92f2-250">hello Enhetsstatus kan vara normal, underhåll, eller inaktiverade (motsvarar toodeactivated i hello Azure-portalen).</span><span class="sxs-lookup"><span data-stu-id="f92f2-250">hello device status can be normal, maintenance, or decommissioned (corresponds toodeactivated in hello Azure portal).</span></span>|
| <span data-ttu-id="f92f2-251">FriendlySoftwareVersion</span><span class="sxs-lookup"><span data-stu-id="f92f2-251">FriendlySoftwareVersion</span></span> | <span data-ttu-id="f92f2-252">hello eget sträng som motsvarar toohello programvaruversionen för enheten.</span><span class="sxs-lookup"><span data-stu-id="f92f2-252">hello friendly string that corresponds toohello device software version.</span></span> <span data-ttu-id="f92f2-253">För ett system som kör uppdatering 4 blir hello eget programvaruversionen StorSimple 8000 Series uppdatering 4.0.</span><span class="sxs-lookup"><span data-stu-id="f92f2-253">For a system running Update 4, hello friendly software version would be StorSimple 8000 Series Update 4.0.</span></span>|
| <span data-ttu-id="f92f2-254">HcsSoftwareVersion</span><span class="sxs-lookup"><span data-stu-id="f92f2-254">HcsSoftwareVersion</span></span>      | <span data-ttu-id="f92f2-255">Hej HCS programvaruversionen körs på enheten.</span><span class="sxs-lookup"><span data-stu-id="f92f2-255">hello HCS software version running on your device.</span></span> <span data-ttu-id="f92f2-256">Till exempel hello HCS programvara version motsvarande tooStorSimple 8000-serien uppdatering 4.0 är 6.3.9600.17820.</span><span class="sxs-lookup"><span data-stu-id="f92f2-256">For instance, hello HCS software version corresponding tooStorSimple 8000 Series Update 4.0 is 6.3.9600.17820.</span></span> |
| <span data-ttu-id="f92f2-257">ApiVersion</span><span class="sxs-lookup"><span data-stu-id="f92f2-257">ApiVersion</span></span>              | <span data-ttu-id="f92f2-258">hello programvaruversion hello Windows PowerShell-API för hello HCS enhet.</span><span class="sxs-lookup"><span data-stu-id="f92f2-258">hello software version of hello Windows PowerShell API of hello HCS device.</span></span>|
| <span data-ttu-id="f92f2-259">VhdVersion</span><span class="sxs-lookup"><span data-stu-id="f92f2-259">VhdVersion</span></span>              | <span data-ttu-id="f92f2-260">hello programvaruversion hello fabriksavbildning som hello enheten levererades med.</span><span class="sxs-lookup"><span data-stu-id="f92f2-260">hello software version of hello factory image that hello device was shipped with.</span></span> <span data-ttu-id="f92f2-261">Om du återställer din enhet toofactory standardvärden kör den här programvaruversionen.</span><span class="sxs-lookup"><span data-stu-id="f92f2-261">If you reset your device toofactory defaults, then it runs this software version.</span></span>|
| <span data-ttu-id="f92f2-262">OSVersion</span><span class="sxs-lookup"><span data-stu-id="f92f2-262">OSVersion</span></span>               | <span data-ttu-id="f92f2-263">hello programvaruversion hello Windows Server-operativsystem körs på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="f92f2-263">hello software version of hello Windows Server operating system running on hello device.</span></span> <span data-ttu-id="f92f2-264">Hej StorSimple-enhet baseras hello Windows Server 2012 R2 som motsvarar too6.3.9600.</span><span class="sxs-lookup"><span data-stu-id="f92f2-264">hello StorSimple device is based off hello Windows Server 2012 R2 that corresponds too6.3.9600.</span></span>|
| <span data-ttu-id="f92f2-265">CisAgentVersion</span><span class="sxs-lookup"><span data-stu-id="f92f2-265">CisAgentVersion</span></span>         | <span data-ttu-id="f92f2-266">hello-version för din TIS-agenten som körs på din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="f92f2-266">hello version for your Cis agent running on your StorSimple device.</span></span> <span data-ttu-id="f92f2-267">Den här agenten kan kommunicera med hello StorSimple Manager-tjänsten körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="f92f2-267">This agent helps communicate with hello StorSimple Manager service running in Azure.</span></span>|
| <span data-ttu-id="f92f2-268">MdsAgentVersion</span><span class="sxs-lookup"><span data-stu-id="f92f2-268">MdsAgentVersion</span></span>         | <span data-ttu-id="f92f2-269">hello version motsvarande toohello Mds-agenten som körs på din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="f92f2-269">hello version corresponding toohello Mds agent running on your StorSimple device.</span></span> <span data-ttu-id="f92f2-270">Den här agenten flyttar data toohello övervakning och diagnostik Service (MDS).</span><span class="sxs-lookup"><span data-stu-id="f92f2-270">This agent moves data toohello Monitoring and Diagnostics Service (MDS).</span></span>|
| <span data-ttu-id="f92f2-271">Lsisas2Version</span><span class="sxs-lookup"><span data-stu-id="f92f2-271">Lsisas2Version</span></span>          | <span data-ttu-id="f92f2-272">Hej version motsvarande toohello LSI drivrutiner på din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="f92f2-272">hello version corresponding toohello LSI drivers on your StorSimple device.</span></span>|
| <span data-ttu-id="f92f2-273">Kapacitet</span><span class="sxs-lookup"><span data-stu-id="f92f2-273">Capacity</span></span>                | <span data-ttu-id="f92f2-274">hello total kapacitet för hello-enhet i byte.</span><span class="sxs-lookup"><span data-stu-id="f92f2-274">hello total capacity of hello device in bytes.</span></span>|
| <span data-ttu-id="f92f2-275">RemoteManagementMode</span><span class="sxs-lookup"><span data-stu-id="f92f2-275">RemoteManagementMode</span></span>    | <span data-ttu-id="f92f2-276">Anger om hello enheten kan fjärrhanteras via dess Windows PowerShell-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="f92f2-276">Indicates whether hello device can be remotely managed via its Windows PowerShell interface.</span></span> |
| <span data-ttu-id="f92f2-277">FipsMode</span><span class="sxs-lookup"><span data-stu-id="f92f2-277">FipsMode</span></span>                | <span data-ttu-id="f92f2-278">Anger om hello USA FIPS Federal Information Processing Standard ()-läge är aktiverat på enheten.</span><span class="sxs-lookup"><span data-stu-id="f92f2-278">Indicates whether hello United States Federal Information Processing Standard (FIPS) mode is enabled on your device.</span></span> <span data-ttu-id="f92f2-279">hello FIPS 140-standarden definierar kryptografiska algoritmer som godkänts för användning av oss federala myndigheter datorsystem för hello skyddet av känsliga data.</span><span class="sxs-lookup"><span data-stu-id="f92f2-279">hello FIPS 140 standard defines cryptographic algorithms approved for use by US Federal government computer systems for hello protection of sensitive data.</span></span> <span data-ttu-id="f92f2-280">FIPS-läge är aktiverat som standard för enheter som kör uppdatering 4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="f92f2-280">For devices running Update 4 or later, FIPS mode is enabled by default.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f92f2-281">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f92f2-281">Next steps</span></span>

* <span data-ttu-id="f92f2-282">Läs hello [syntaxen för hello Invoke-HcsDiagnostics cmdlet](https://technet.microsoft.com/library/mt795371.aspx).</span><span class="sxs-lookup"><span data-stu-id="f92f2-282">Learn hello [syntax of hello Invoke-HcsDiagnostics cmdlet](https://technet.microsoft.com/library/mt795371.aspx).</span></span>

* <span data-ttu-id="f92f2-283">Läs mer om hur för[felsöka distributionsproblem](storsimple-troubleshoot-deployment.md) på StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="f92f2-283">Learn more about how too[troubleshoot deployment issues](storsimple-troubleshoot-deployment.md) on your StorSimple device.</span></span>
