---
title: "aaaPlan kapacitet och skalning för fysisk server replication tooAzure med Azure Site Recovery | Microsoft Docs"
description: "Använd den här artikeln tooplan kapacitet och skala vid replikering av Windows-/ Linux fysiska servrar tooAzure med Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 554f59ee-0b49-4779-9737-90cb601ef6fe
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: rayne
ms.openlocfilehash: 209980963c07d13e15802a5da44769ac559217d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-3-plan-capacity-and-scaling-for-physical-server-tooazure-replication"></a>Steg 3: Planera kapacitet och skalning för fysisk server tooAzure replikering

Använd den här artikeln toofigure ut kapacitet och skalning, när du replikera lokala Windows-/ Linux fysiska servrar tooAzure med [Azure Site Recovery](site-recovery-overview.md).

Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="plan-deployment-capacity"></a>Planera kapacitet för distribution

1. Läs [artikel](site-recovery-plan-capacity-vmware.md) toolearn om hur du uppskattar replikeringskrav och riktlinjer för att ändra storlek på Site Recovery-komponenter.
2. Läsa hello överväganden nedan toolearn om skalning komponentservrarna och kontroll av bandbredden för replikerad dator.

## <a name="replication-considerations"></a>Överväganden för datareplikering

Använd dessa överväganden toofigure replikerade server-krav.

**Komponent** | **Detaljer** 
--- | --- 
**Replikering** | **Maximal dagliga förändringstakten:** en skyddad dator kan bara använda en processerver och en enda process-server kan hantera en dagliga förändringstakten in too2 TB. Därför är 2 TB hello maximala dagliga data ändra hastigheten som stöds för en skyddad dator.<br/><br/> **Maximalt dataflöde:** en replikerad dator kan höra tooone storage-konto i Azure. Ett standardlagringskonto kan hantera upp till 20 000 begäranden per sekund och vi rekommenderar att du behåller hello antalet i/o-åtgärder per sekund (IOPS) över en källa datorn too20, 000. Till exempel om du har en källdator med 5 diskar och varje disk genererar 120 IOPS (om 8K storlek) på källdatorn hello, blir sedan inom hello Azure per disk IOPS högst 500. (hello antalet storage-konton som krävs är lika toohello totala källdatorn IOPS, dividerat med 20 000.)

## <a name="configuration-server-capacity"></a>Konfiguration av serverkapacitet

hello konfigurationsservern ska vara kan toohandle hello daglig ändra frekvensen kapacitet för alla arbetsbelastningar som körs på skyddade datorer, och måste tillräcklig bandbredd toocontinuously replikera data tooAzure lagring.

Ett bra tips är att hitta hello konfigurationsservern på hello samma nätverk och LAN-segment som hello datorer som du vill tooprotect. Det kan finnas på ett annat nätverk, men datorer som du vill att tooprotect ska ha layer 3 nätverket synlighet tooit.

## <a name="sizing-recommendations"></a>Ändra storlek på rekommendationer

hello tabell sammanfattas sizing rekommendationer baserat på CPU.

**CPU** | **Minne** | **Cachestorleken för disk** | **Dataändringshastighet** | **Skyddade datorer**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 kärnor @ 2,5 gigahertz (GHz)) | 16 GB | 300 GB | 500 GB eller mindre | Replikera färre än 100 datorer.
12 vCPUs (2 sockets * @ 2,5 GHz-6 kärnor) | 18 GB | 600 GB | 500 GB too1 TB | Replikera mellan 100 150 datorer.
16 vCPUs (2 sockets * 8 kärnor @ 2,5 GHz) | 32 GB | 1 TB | 1 TB too2 TB | Replikera mellan 150 200 datorer.
Distribuera en annan processerver | | | > 2 TB | Distribuera ytterligare servrar om du replikerar mer än 200 datorer eller ändrar hello dagliga data frekvensen överskrider 2 TB.

Där:

* Varje källdatorn har konfigurerats med 3 diskar på 100 GB vardera.
* Vi använde benchmarking lagring av 8 SAS-enheter på 10 K RPM, med RAID 10 för cache disk mått.

## <a name="process-server-capacity"></a>Bearbeta serverkapacitet


Hej processervern tar emot replikeringsdata från skyddade datorer och optimerar dem med cachelagring, komprimering och kryptering. Skickar sedan hello data tooAzure.

- hello processen serverdatorn ska ha tillräckliga resurser tooperform dessa uppgifter.
- hello första processervern installeras som standard på hello konfigurationsservern. Du kan distribuera ytterligare processer servrar tooscale din miljö.
- Hej processervern använder ett diskbaserad cacheminne. Använd en separat cache 600 GB eller mer toohandle dataändringarna lagras i hello-händelse en flaskhalsar i nätverket eller avbrott.
- Om du behöver tooprotect fler än 200 datorer eller hello dagliga förändringstakten är större än 2 TB, kan du lägga till processen servrar toohandle hello replikering belastningen. tooscale ut kan du:
    - Öka hello antalet configuration-servrar. Du kan till exempel skydda too400 datorer med två konfigurationsservrar.
    - Lägga till flera servrar och använda dessa toohandle trafik i stället för (eller förutom) hello konfigurationsservern.


### <a name="example-process-server-scaling"></a>Exempel processen serverskalning

hello följande tabell beskriver ett scenario där:

* Du planerar inte toouse hello konfigurationsservern som en processerver.
* Du har konfigurerat en ytterligare processervern.
* Du har konfigurerat skyddade virtuella datorer toouse hello ytterligare processervern.
* Varje skyddade källdatorn har konfigurerats med tre diskar på 100 GB vardera.

**Konfigurationsserver** | **Ytterligare processervern** | **Cachestorleken för disk** | **Dataändringshastighet** | **Skyddade datorer**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 kärnor @ 2,5 GHz), 16 GB minne | 4 vCPUs (2 sockets * 2 kärnor @ 2,5 GHz), 8 GB minne | 300 GB | 250 GB eller mindre | Replikera 85 eller färre datorer.
8 vCPUs (2 sockets * 4 kärnor @ 2,5 GHz), 16 GB minne | 8 vCPUs (2 sockets * 4 kärnor @ 2,5 GHz), 12 GB minne | 600 GB | 250 GB too1 TB | Replikera mellan 85 150 datorer.
12 vCPUs (2 sockets * 6 kärnor @ 2,5 GHz), 18 GB minne | 12 vCPUs (2 sockets * 6 kärnor @ 2,5 GHz) 24 GB minne | 1 TB | 1 TB too2 TB | Replikera mellan 150 225 datorer.

Hej hur där du skala dina servrar beror på dina inställningar för en modell skala upp eller skala ut.  Du skala upp genom att distribuera några avancerad konfiguration och servrar eller skala ut genom att distribuera flera servrar med färre resurser. Till exempel om du behöver tooprotect 220 datorer kan du kan du göra något av följande hello:

* Ställ in hello konfigurationsservern med 12 vCPU, 18 GB minne och en ytterligare processervern med 12 vCPU, 24 GB minne. Konfigurera skyddade datorer toouse hello ytterligare processer endast server.
* Ställa in två configuration-servrar (2 x 8 vCPU, 16 GB RAM-minne) och två ytterligare servrar (1 x 8 vCPU och 4 vCPU x 1 toohandle 135 + 85 [220] datorer). Konfigurera skyddade datorer toouse hello ytterligare processer endast servrar.

## <a name="deploy-additional-process-servers"></a>Distribuera ytterligare servrar

1. Följ [instruktionerna](site-recovery-vmware-setup-azure-ps-resource-manager.md) tooset in en ytterligare processer-server.
2. Om du inte har hello lösenfras kör **[SiteRecoveryInstallationFolder]\home\sysystems\bin\genpassphrase.exe – n** på hello configuration server tooget den.
3. När du skapat hello processervern kan du migrera källa datorer toouse den.

    1. I **inställningar** > **Site Recovery servrar**, klicka på hello konfigurationsservern > **bearbeta servrar**.
    2. Högerklicka på hello processen server används för närvarande > **växel**.
    3. I **Välj målprocesserver**väljer hello processen du vill toouse och väljer hello VMs hello servern ska hantera.
    4. Klicka på ikonen för hello information. toohelp du läsa in beslut, hello genomsnittlig utrymme som behövs för tooreplicate varje valda VM toohello nya processervern visas.
    5. Klicka på hello markerat toostart replikering toohello nya processervern.

## <a name="control-network-bandwidth"></a>Kontrollera nätverksbandbredd

När du har kört [hello distribution Kapacitetsplaneringsverktyget för](site-recovery-deployment-planner.md) toocalculate hello bandbredd som du behöver för replikering (hello inledande replikering och sedan delta), du kan styra hello mängden bandbredd som används för replikering med flera olika alternativ:

* **Begränsa bandbredden**: VMware-trafik som replikeras tooAzure går igenom en specifik process-server. Du kan begränsa bandbredden på hello-datorer som körs som servrar.
* **Påverka bandbredd**: du kan påverka hello bandbredd som används för replikering med hjälp av några registernycklar:
  * Hej **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\UploadThreadsPerVM** registervärdet anger hello antalet trådar som används för att överföra data (inledande replikering eller delta) för en disk. Ett högre värde ökar hello nätverksbandbredd som används för replikering.
  * Hej **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DownloadThreadsPerVM** anger hello antalet trådar som används för att överföra data under återställning efter fel.

### <a name="throttle-bandwidth"></a>Begränsa bandbredden

1. Öppna hello Azure Backup MMC snapin-modulen på hello datorn agerar som hello processervern. Som standard en genväg för säkerhetskopiering är tillgänglig på hello skrivbordet eller i hello följande mapp: C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. I snapin-modulen hello, klickar du på **ändra egenskaper för**.
3. På hello **begränsning** väljer **aktivera Användningsbegränsning för internetbandbredd för säkerhetskopieringsåtgärder**.
4. Ange hello gränserna för arbete och fritid timmar. Giltigt intervall är från 512 kbit/s too102 Mbit/s per sekund.

    ![Begränsning](./media/physical-walkthrough-capacity/throttle2.png)

Du kan också använda hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet tooset begränsning. Här är ett exempel:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** anger att ingen begränsning krävs.

### <a name="influence-network-bandwidth-for-a-vm"></a>Påverka nätverkets bandbredd för en virtuell dator

1. I registret för hello VM går för**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * tooinfluence hello bandbredd trafik på en replikering disk, ändra hello-värdet för **UploadThreadsPerVM**, eller skapa hello nyckeln om den inte finns.
   * tooinfluence hello bandbredd för redundanstrafik från Azure, ändra hello-värdet för **DownloadThreadsPerVM**.
2. hello standardvärdet är 4. I en överetablerad nätverk ska registernycklarna ändras. hello maximala är 32. Övervaka trafik toooptimize hello värde.




## <a name="next-steps"></a>Nästa steg

Gå för[steg 4: Planera nätverk](physical-walkthrough-network.md).
