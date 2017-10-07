---
title: "aaaPlan kapacitet och skalning för Virtuella Hyper-V-replikering (utan VMM) tooAzure med Azure Site Recovery | Microsoft Docs"
description: "Använd den här artikeln tooplan kapacitet och skala vid replikering av Hyper-V VMs tooAzure med Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8687af60-5b50-481c-98ee-0750cbbc2c57
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: f5b029f273e51c01c7d918d176832f6d1735b5f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-3-plan-capacity-and-scaling-for-hyper-v-tooazure-replication"></a>Steg 3: Planera kapacitet och skalning för Hyper-V tooAzure replikering

Använd den här artikeln toofigure ut planera för kapacitet och skala vid replikering av lokala Hyper-V virtuella datorer (utan VMM för System Center) tooAzure med [Azure Site Recovery](site-recovery-overview.md).

När du har läst den här artikeln efter eventuella kommentarer längst ned hello eller tekniska frågor om hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="how-do-i-start-capacity-planning"></a>Hur börjar jag kapacitetsplanering?


Du samla in information om replikeringsmiljön och planera kapacitet med den här informationen, tillsammans med hello överväganden markerat i den här artikeln.


## <a name="gather-information"></a>Samla in information

1. Samla in information om replikeringsmiljön, inklusive virtuella datorer, diskar per virtuell dator och lagringsutrymme per disk.
2. Identifiera dina dagliga (omsättningen) förändringstakten för replikerade data. Hämta hello [Hyper-V kapacitetsplanering verktyget](https://www.microsoft.com/download/details.aspx?id=39057) tooget hello förändringstakten. Vi rekommenderar att du kör det här verktyget över en vecka toocapture medelvärden.
 

## <a name="figure-out-capacity"></a>Ta reda på kapaciteten

Baserat på hello information som du har samla du kör hello [Site Recovery Kapacitetsplaneringsverktyget för](http://aka.ms/asr-capacity-planner-excel) tooanalyze din källmiljö arbetsbelastningar, beräkna bandbreddsbehov och serverresurser för hello källplats och hello resurser (virtuella datorer och lagring osv), som du behöver i hello målplats. Du kan köra verktyget hello på ett par olika lägen:

- Snabb planering: kör hello-verktyget i det här läget tooget nätverks- och projektioner baserat på ett genomsnittligt antal virtuella datorer, diskar, lagring och förändringstakten.
- Detaljerad planering: kör hello-verktyget i det här läget och ange information för varje arbetsbelastning på VM-nivå. Analysera VM-kompatibilitet och får nätverks- och projektioner.

Kör nu hello verktyget:

1. Hämta hello [verktyget](http://aka.ms/asr-capacity-planner-excel)
2. toorun hello snabb planner, Följ [instruktionerna](site-recovery-capacity-planner.md#run-the-quick-planner), och välj hello scenario **Hyper-V tooAzure**.
3. toorun Hej detaljerad planner, Följ [instruktionerna](site-recovery-capacity-planner.md#run-the-detailed-planner), och välj hello scenario **Hyper-V tooAzure**.

## <a name="control-network-bandwidth"></a>Kontrollera nätverksbandbredd

När du har beräknade hello bandbredd som du behöver ha ett antal alternativ för att styra hello mängden bandbredd som används för replikering:

* **Begränsa bandbredden**: Hyper-V-trafik som replikeras tooAzure går igenom en specifik Hyper-V-värd. Du kan begränsa bandbredden på värdservern för hello.
* **Påverka bandbredd**: du kan påverka hello bandbredd som används för replikering med hjälp av några registernycklar.

### <a name="throttle-bandwidth"></a>Begränsa bandbredden
1. Öppna hello Microsoft Azure Backup MMC snapin-modulen på hello Hyper-V-värdservern. Som standard är en genväg till Microsoft Azure Backup finns på hello skrivbordet eller i C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. I snapin-modulen hello klickar du på **ändra egenskaper för**.
3. På hello **begränsning** väljer **aktivera Användningsbegränsning för internetbandbredd för säkerhetskopieringsåtgärder**, och ange hello gränserna för arbete och fritid timmar. Giltigt intervall är från 512 kbit/s too102 Mbit/s per sekund.

    ![Begränsa bandbredden](./media/hyper-v-site-walkthrough-capacity/throttle2.png)

Du kan också använda hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) cmdlet tooset begränsning. Här är ett exempel:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** anger att ingen begränsning krävs.

### <a name="influence-network-bandwidth"></a>Påverka nätverkets bandbredd
1. I registret hello navigera för**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * tooinfluence hello bandbredd trafik på en replikering disk, ändra hello värdet hello **UploadThreadsPerVM**, eller skapa hello nyckeln om den inte finns.
   * tooinfluence hello bandbredd för redundanstrafik från Azure, ändra värdet för hello **DownloadThreadsPerVM**.
2. hello standardvärdet är 4. I ett ”överetablerat” nätverk bör registernycklarna ändras från hello standardvärden. hello maximala är 32. Övervaka trafik toooptimize hello värde.

## <a name="next-steps"></a>Nästa steg

Gå för[steg 4: Planera nätverk](hyper-v-site-walkthrough-network.md).
