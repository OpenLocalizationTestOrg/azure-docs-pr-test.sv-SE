---
title: "aaaPlan kapacitet och skalning för VMware replikering tooAzure med Azure Site Recovery | Microsoft Docs"
description: "Använd den här artikeln tooplan kapacitet och skala vid replikering av virtuella VMware-datorer tooAzure med Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 0a1cd8eb-a8f7-4228-ab84-9449e0b2887b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/24/2017
ms.author: rayne
ms.openlocfilehash: 7ca9147d1b4611f6b4a67c3de3f27fb9878f4c4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="plan-capacity-and-scaling-for-vmware-replication-with-azure-site-recovery"></a>Planera kapacitet och skalning för VMware-replikering med Azure Site Recovery

Använd den här artikeln toofigure planera för kapacitet och skalning när replikera lokala virtuella VMware-datorer och fysiska servrar tooAzure med [Azure Site Recovery](site-recovery-overview.md).

## <a name="how-do-i-start-capacity-planning"></a>Hur börjar jag kapacitetsplanering?

Samla in information om replikeringsmiljön genom att köra hello [Azure Site Recovery-distribution Planner](https://aka.ms/asr-deployment-planner-doc) för VMware-replikering. [Lär dig mer](site-recovery-deployment-planner.md) om det här verktyget. Du kan samla in information om kompatibla och inkompatibla virtuella datorer, diskar per virtuell dator och data omsättningsuppdateringar per disk. hello verktyget omfattar även kraven på nätverksbandbredd och hello Azure infrastruktur som behövs för lyckad replikering och testa redundans.

## <a name="capacity-considerations"></a>Överväganden för kapacitetsplanering

**Komponent** | **Detaljer** |
--- | --- | ---
**Replikering** | **Maximal dagliga förändringstakten:** en skyddad dator kan bara använda en processerver och en enda process-server kan hantera en dagliga förändringstakten in too2 TB. Därför är 2 TB hello maximala dagliga data ändra hastigheten som stöds för en skyddad dator.<br/><br/> **Maximalt dataflöde:** en replikerad dator kan höra tooone storage-konto i Azure. Ett standardlagringskonto kan hantera upp till 20 000 begäranden per sekund och vi rekommenderar att du behåller hello antalet i/o-åtgärder per sekund (IOPS) över en källa datorn too20, 000. Till exempel om du har en källdator med 5 diskar och varje disk genererar 120 IOPS (om 8K storlek) på källdatorn hello, blir sedan inom hello Azure per disk IOPS högst 500. (hello antalet storage-konton som krävs är lika toohello totala källdatorn IOPS, dividerat med 20 000.)
**Konfigurationsserver** | hello konfigurationsservern ska vara kan toohandle hello daglig ändra frekvensen kapacitet för alla arbetsbelastningar som körs på skyddade datorer, och måste tillräcklig bandbredd toocontinuously replikera data tooAzure lagring.<br/><br/> Ett bra tips är att hitta hello konfigurationsservern på hello samma nätverk och LAN-segment som hello datorer som du vill tooprotect. Det kan finnas på ett annat nätverk, men datorer som du vill att tooprotect ska ha layer 3 nätverket synlighet tooit.<br/><br/> Storlek rekommendationer för hello konfigurationsservern sammanfattas i tabellen hello i hello efter avsnittet.
**Processervern** | hello första processervern installeras som standard på hello konfigurationsservern. Du kan distribuera ytterligare processer servrar tooscale din miljö. <br/><br/> Hej processervern tar emot replikeringsdata från skyddade datorer och optimerar dem med cachelagring, komprimering och kryptering. Skickar sedan hello data tooAzure. hello processen serverdatorn ska ha tillräckliga resurser tooperform dessa uppgifter.<br/><br/> Hej processervern använder ett diskbaserad cacheminne. Använd en separat cache 600 GB eller mer toohandle dataändringarna lagras i hello-händelse en flaskhalsar i nätverket eller avbrott.

## <a name="size-recommendations-for-hello-configuration-server"></a>Storlek rekommendationer för hello konfigurationsservern

**CPU** | **Minne** | **Cachestorleken för disk** | **Dataändringshastighet** | **Skyddade datorer**
--- | --- | --- | --- | ---
8 vCPUs (2 sockets * 4 kärnor @ 2,5 gigahertz (GHz)) | 16 GB | 300 GB | 500 GB eller mindre | Replikera färre än 100 datorer.
12 vCPUs (2 sockets * @ 2,5 GHz-6 kärnor) | 18 GB | 600 GB | 500 GB too1 TB | Replikera mellan 100 150 datorer.
16 vCPUs (2 sockets * 8 kärnor @ 2,5 GHz) | 32 GB | 1 TB | 1 TB too2 TB | Replikera mellan 150 200 datorer.
Distribuera en annan processerver | | | > 2 TB | Distribuera ytterligare servrar om du replikerar mer än 200 datorer eller ändrar hello dagliga data frekvensen överskrider 2 TB.

Där:

* Varje källdatorn har konfigurerats med 3 diskar på 100 GB vardera.
* Vi använde benchmarking lagring av 8 SAS-enheter på 10 K RPM, med RAID 10 för cache disk mått.

## <a name="size-recommendations-for-hello-process-server"></a>Storlek rekommendationer för hello processervern

Om du behöver tooprotect fler än 200 datorer eller hello dagliga förändringstakten är större än 2 TB, kan du lägga till processen servrar toohandle hello replikering belastningen. tooscale ut kan du:

* Öka hello antalet configuration-servrar. Du kan till exempel skydda too400 datorer med två konfigurationsservrar.
* Lägga till flera servrar och använda dessa toohandle trafik i stället för (eller förutom) hello konfigurationsservern.

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


## <a name="control-network-bandwidth"></a>Kontrollera nätverksbandbredd

När du har använt hello [hello distribution Kapacitetsplaneringsverktyget för](site-recovery-deployment-planner.md) toocalculate hello bandbredd som du behöver för replikering (hello inledande replikering och sedan delta), du kan styra hello mängden bandbredd som används för replikering med ett par alternativ:

* **Begränsa bandbredden**: VMware-trafik som replikeras tooAzure går igenom en specifik process-server. Du kan begränsa bandbredden på hello-datorer som körs som servrar.
* **Påverka bandbredd**: du kan påverka hello bandbredd som används för replikering med hjälp av några registernycklar:
  * Hej **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\UploadThreadsPerVM** registervärdet anger hello antalet trådar som används för att överföra data (inledande replikering eller delta) för en disk. Ett högre värde ökar hello nätverksbandbredd som används för replikering.
  * Hej **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DownloadThreadsPerVM** anger hello antalet trådar som används för att överföra data under återställning efter fel.

### <a name="throttle-bandwidth"></a>Begränsa bandbredden

1. Öppna hello Azure Backup MMC snapin-modulen på hello datorn agerar som hello processervern. Som standard en genväg för säkerhetskopiering är tillgänglig på hello skrivbordet eller i hello följande mapp: C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. I snapin-modulen hello, klickar du på **ändra egenskaper för**.

    ![Skärmbild av Azure Backup MMC snapin-modulen toochange egenskaper](./media/site-recovery-vmware-to-azure/throttle1.png)
3. På hello **begränsning** väljer **aktivera Användningsbegränsning för internetbandbredd för säkerhetskopieringsåtgärder**. Ange hello gränserna för arbete och fritid timmar. Giltigt intervall är från 512 kbit/s too102 Mbit/s per sekund.

    ![Skärmbild av Azure Backup egenskapsdialogrutan](./media/site-recovery-vmware-to-azure/throttle2.png)

Du kan också använda hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet tooset begränsning. Här är ett exempel:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** anger att ingen begränsning krävs.

### <a name="influence-network-bandwidth-for-a-vm"></a>Påverka nätverkets bandbredd för en virtuell dator

1. Hello VM registret, navigera för**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * tooinfluence hello bandbredd trafik på en replikering disk, ändra hello-värdet för **UploadThreadsPerVM**, eller skapa hello nyckeln om den inte finns.
   * tooinfluence hello bandbredd för redundanstrafik från Azure, ändra hello-värdet för **DownloadThreadsPerVM**.
2. hello standardvärdet är 4. I ett ”överetablerat” nätverk bör registernycklarna ändras från hello standardvärden. hello maximala är 32. Övervaka trafik toooptimize hello värde.


## <a name="deploy-additional-process-servers"></a>Distribuera ytterligare servrar

Om du har tooscale upp distributionen utöver 200 källdatorer eller du har totalt dagligen omsättningsuppdateringar av mer än 2 TB, måste ytterligare processer servrar toohandle hello trafikvolym. Följ dessa instruktioner tooset in hello processervern. När du skapat hello-server kan du migrera källa datorer toouse den.

1. I **Site Recovery servrar**, klicka på hello konfigurationsservern och klicka sedan på **Processervern**.

    ![Skärmbild av Site Recovery servrar alternativet tooadd en processerver](./media/site-recovery-vmware-to-azure/migrate-ps1.png)
2. I **servertyp**, klickar du på **processervern (lokalt)**.

    ![Skärmbild av Processervern dialogrutan](./media/site-recovery-vmware-to-azure/migrate-ps2.png)
3. Hämta hello Unified installationsprogram för Site Recovery-filen och kör det tooinstall hello processervern. Detta även registrerar den i hello-valvet.
4. I **innan du börjar**väljer **lägga till ytterligare processer servrar tooscale ut distribution**.
5. Fullständig hello guiden i hello samma sätt som du gjorde när du [konfigurera](#step-2-set-up-the-source-environment) hello konfigurationsservern.

    ![Skärmbild av Azure Site Recovery Unified installationsguiden](./media/site-recovery-vmware-to-azure/add-ps1.png)
6. I **Server konfigurationsinformation**anger hello hello configuration serverns IP-adress och lösenfrasen hello. tooobtain hello lösenfras, kör **[SiteRecoveryInstallationFolder]\home\sysystems\bin\genpassphrase.exe – n** på hello konfigurationsservern.

    ![Skärmbild av Server konfigurationsinformation sida](./media/site-recovery-vmware-to-azure/add-ps2.png)

### <a name="migrate-machines-toouse-hello-new-process-server"></a>Migrera datorer toouse hello nya processervern
1. I **inställningar** > **Site Recovery servrar**klickar hello konfigurationsservern och expandera sedan **bearbeta servrar**.

    ![Skärmbild av Processervern dialogrutan](./media/site-recovery-vmware-to-azure/migrate-ps2.png)
2. Högerklicka hello processen server används för närvarande och på **växel**.

    ![Dialogrutan Skärmbild av konfiguration av servern](./media/site-recovery-vmware-to-azure/migrate-ps3.png)
3. I **Välj målprocesserver**väljer hello nya processervern du vill toouse och välj sedan hello virtuella datorer som hello ska hantera. Klicka på hello information ikonen tooget information om hello-server. toohelp du läsa in beslut, hello genomsnittlig utrymme som behövs för tooreplicate varje valda virtuella datorn toohello nya processervern visas. Klicka på hello markerat toostart replikerar toohello nya processervern.


## <a name="next-steps"></a>Nästa steg

Hämta och köra hello [Azure Site Recovery-distribution Planner](https://aka.ms/asr-deployment-planner)
