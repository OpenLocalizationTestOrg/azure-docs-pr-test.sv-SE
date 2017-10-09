---
title: aaaStorSimple 8000-serien uppdatering 4 viktig information | Microsoft Docs
description: "Beskriver nya funktioner för hello, problem och lösningar för StorSimple 8000 Series uppdatering 4."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/04/2017
ms.author: alkohli
ms.openlocfilehash: 4bca8ca222e6706fd6eaf56b702b0d34b6ffd1c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-update-4-release-notes"></a>StorSimple 8000 Series uppdatering 4 viktig information

## <a name="overview"></a>Översikt

hello följande viktig information beskrivs hello nya funktioner och identifiera hello kritiska öppna problem för StorSimple 8000 Series uppdatering 4. De innehåller också en lista över hello StorSimple programuppdateringar som ingår i den här versionen. 

Update 4 kan vara tillämpade tooany StorSimple-enhet som kör versionen (GA) eller uppdatering 0.1 via uppdatering 3.1. hello enheten version som är associerad med Update 4 är 6.3.9600.17820.

Granska hello informationen i hello release notes innan du distribuerar hello uppdatera i din StorSimple-lösning.

> [!IMPORTANT]
> * Uppdatering 4 har enhetsprogrammet, över inbyggd programvara, LSI drivrutiner och inbyggd programvara, disk inbyggd programvara, Storport och Spaceport, säkerhet och andra OS uppdateringar. Det tar cirka 4 timmar tooinstall uppdateringen. Disk firmware-uppdatering är en störande uppdatering och resulterar i ett driftstopp för din enhet. Vi rekommenderar att du installerar uppdatering 4 tookeep enheten är uppdaterad. 
> * För nya versioner, kan du inte se uppdateringar omedelbart eftersom vi gör en stegvis distribution av hello uppdateringar. Vänta några dagar och sedan söka efter uppdateringar igen som dessa är snart tillgängligt.

## <a name="whats-new-in-update-4"></a>Vad är nytt i uppdatering 4

hello har följande viktiga förbättringar och felkorrigeringar gjorts i uppdatering 4.

* **Smartare automated algoritmer för frigöring av utrymme** – i uppdatering 4 hello automatiserade utrymme frigöring algoritmer är förbättrad tooadjust hello utrymmesåtertagning cykler baserat på förväntat hello frigöras tillgängligt utrymme i hello molnet. 

* **Prestandaförbättringar för lokalt fästa volymer** – uppdatering 4 har förbättrats hello prestanda för lokalt fästa volymer i scenarier där hög datapåfyllning (Datastorlek för jämförbara toovolume).

* **Heatmap-baserad återställning** – i hello tidigare versioner, efter en katastrofåterställning (DR), hello data har återställts från hello molnet baserat på hello åtkomstmönster som resulterar i ett långsamt. 

    En ny funktion är implementerad i uppdatering 4 som spårar ofta data toocreate en heatmap när hello enhet är i användning tidigare tooDR (mest används datasegment har hög värme som används mindre segment har låg termiska). Efter DR, StorSimple använder hello heatmap tooautomatically återställning och rehydrate hello data från hello molnet. 

    Alla hello återställningar är nu heatmap baserat återställningar. Mer information om hur tooquery och Avbryt heatmap baserade återställning och rehydration jobb gå för[Windows PowerShell för StorSimple cmdlet-referens](https://technet.microsoft.com/library/dn688168.aspx).

* **StorSimple-diagnostik** – i uppdatering 4 en StorSimple-diagnostik verktyget används publicerat tooallow för att enkelt diagnostisera och felsökning av problem relaterade toosystem, nätverk, prestanda och maskinvara komponentens hälsostatus. Det här verktyget körs via hello Windows PowerShell för StorSimple. Mer information finns för[felsökning med StorSimple diagnostikverktyget](storsimple-8000-diagnostics.md).

* **UI-baserade StorSimple Migreringsverktyget** -tidigare toothis övergång, migrera data från 5000 7000-serien krävs hello användare tooexecute en del av arbetsflödet för migrering hello hello Azure PowerShell-gränssnittet. I den här versionen är ett enkelt att använda UI-baserad migrering StorSimple verktyget görs tillgänglig för stöd toofacilitate hello samma arbetsflödet för migrering. Det här verktyget kan också för hello konsolidering buckets för återställning. 

* **FIPS-relaterade ändringar** – den här versionen och senare, FIPS är aktiverad som standard på alla enheter i hello StorSimple 8000-serien för både hello Microsoft Azure Government och konton för offentliga Azure-molnet.

* **Uppdatera ändringarna i** – i den här versionen buggar relaterade tooupdate fel har åtgärdats.

* **Avisering för diskfel** -en ny avisering som varnar hello användare för nära förestående diskfel har lagts till i den här versionen. Om du får aviseringen, kontaktar du Microsoft Support tooship en ersättningsdisken. Mer information finns för[maskinvara meddelanden på din StorSimple-enhet](storsimple-manage-alerts.md#hardware-alerts).

* **Domänkontrollanten ersättning ändringar** -en cmdlet som tillåter hello tooquery hello användarstatus processens hello controller ersättning har lagts till i den här versionen. Mer information finns i toohello [status för ersättning av cmdlet tooquery domänkontrollanten](https://technet.microsoft.com/library/dn688168.aspx).


## <a name="issues-fixed-in-update-4"></a>Problem som åtgärdas i uppdatering 4

hello innehåller följande tabell en översikt över problem som har åtgärdats i uppdatering 4.    

| Nej | Funktion | Problem | Gäller toophysical enhet | Gäller toovirtual enhet |
| --- | --- | --- | --- | --- |
| 1 |Redundans |I hello tidigare version efter hello växling, det var ett problem relaterade toocleanup observerades vid hello kundens plats. Det här problemet löses i den här versionen. |Ja |Ja |
| 2 |Lokalt fästa volymer |I hello tidigare version uppstod ett problem toorelated volymer kan skapas för lokalt fästa volymer som skulle resultera i fel vid skapande av volymen. Det här problemet var roten orsakade och fast i den här versionen. |Ja |Nej |
| 3 |Support-paket |Det fanns problem relaterade tooSupport paket som skulle resultera i ett System.OutOfMemory undantag eller andra fel som leder stöd paketet skapas i föregående versionen. De här felen korrigeras i den här versionen. |Ja |Ja |
| 4 |Övervakning |I tidigare version relaterade det ett problem toomonitoring diagram för lokalt fästa volymer där förbrukning visades i EB. Det här problemet löses i den här versionen. |Ja |Ja |
| 5 |Migrering |Det fanns flera problem relaterade toohello tillförlitlighet för migrering från enheterna i serierna 5000 7000 serie too8000 i föregående versionen. Dessa problem har åtgärdats i den här versionen. |Ja |Ja |
| 6 |Uppdatering |I tidigare versioner, om det uppstod ett fel för uppdateringen, hello domänkontrollanter hamnar i återställningsläge och därför hello användaren gick inte att fortsätta med uppdateringen hello och måste toocontact Microsoft-supporten. <br> Det här beteendet har ändrats i den här versionen. Om hello användare har en update-fel när båda hello-styrenheter är hello köra samma version (Update 4) hello domänkontrollanter inte går i återställningsläge. Om användaren hello påträffar felet, rekommenderar vi att de vänta lite och försök sedan hello uppdateringen. hello försök lyckas. Om hello försök misslyckas, bör de kontakta Microsoft Support. |Ja |Ja |


## <a name="known-issues-in-update-4-from-previous-releases"></a>Kända problem i uppdatering 4 från tidigare versioner

Det finns inga nya kända problem i uppdatering 4. En lista över problem som överförs från tidigare versioner tooUpdate 4 Gå för[uppdatering 3 viktig information](storsimple-update3-release-notes.md#known-issues-in-update-3).

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-update-4"></a>Serieansluten SCSI (SAS) domänkontrollant och firmware-uppdateringar i uppdatering 4

Den här versionen har SAS-styrenhet och LSI drivrutinen och firmware-uppdateringar. Mer information om hur tooinstall dessa uppdateringar, se [installera uppdatering 4](storsimple-install-update-4.md) på StorSimple-enheten.

## <a name="virtual-device-updates-in-update-4"></a>Uppdatering av virtuella enheter i uppdatering 4

Den här uppdateringen kan inte vara tillämpade toohello StorSimple moln installation (även kallat hello virtuell enhet). Nya virtuella enheter behöver toobe skapas. 

## <a name="next-step"></a>Nästa steg

Lär dig hur för[installera uppdatering 4](storsimple-install-update-4.md) på StorSimple-enheten.

