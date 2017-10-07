---
title: "aaaStorSimple redundans, katastrofåterställning för enheter i 8000-serien | Microsoft Docs"
description: "Lär dig hur toofail över tooitself din StorSimple-enhet, en annan fysisk enhet eller en enhet i molnet."
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
ms.date: 05/03/2017
ms.author: alkohli
ms.openlocfilehash: 9d01ee30b15b77072b1d4dfe9a215abc83ffba28
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="failover-and-disaster-recovery-for-your-storsimple-8000-series-device"></a>Redundans och disaster recovery för enheten StorSimple 8000-serien

## <a name="overview"></a>Översikt

Den här artikeln beskriver hello enheten redundansfunktionen för hello StorSimple 8000-serien enheter och hur den här funktionen kan vara används toorecover StorSimple-enheter om en olycka inträffar. StorSimple använder enheten redundans toomigrate hello data från en källenhet i hello datacenter tooanother målenhet. hello riktlinjerna i den här artikeln gäller tooStorSimple 8000-serien fysiska enheter och molnet installationer med programvaruversioner uppdatering 3 och senare.

StorSimple använder hello **enheter** bladet toostart hello enheten funktionen med redundans i en katastrof hello-händelse. Det här bladet visar en lista över alla hello StorSimple-enheter anslutna tooyour StorSimple Device Manager-tjänsten.

![Bladet för enheter](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev1.png)


## <a name="disaster-recovery-dr-and-device-failover"></a>Katastrofåterställning (DR) och växling vid fel för enheten

I en (DR) katastrofåterställning, hello primära enhet slutar fungera. StorSimple använder hello primära enhet som _källa_ och flyttar hello associerade molnet data tooanother _mål_ enhet. Den här processen är refererad tooas hello *redundans*. hello följande bild illustreras hello växling vid fel.

![Vad som händer i enheten växling vid fel?](./media/storsimple-8000-device-failover-disaster-recovery/failover-dr-flow.png)

hello målenhet för ett redundanskluster kan vara en fysisk enhet eller även en moln-installation. hello målenhet kanske finns i hello samma eller en annan geografisk plats än hello källenheten.

Du kan välja volymbehållare för migrering under hello redundans. StorSimple ändras sedan hello ägarskap av behållarna volym från hello källa enheten toohello målenhet. När hello volymbehållare ändra ägarskap, StorSimple att tar bort dessa behållare från hello källenheten. När hello borttagningen är klar, kan du växla tillbaka hello målenhet. _Återställning efter fel_ överföringar hello ägarskap tillbaka toohello ursprungliga källenheten.

### <a name="cloud-snapshot-used-during-device-failover"></a>Ögonblicksbild i molnet används under växling vid fel för enheten

Hello senaste säkerhetskopian av molntjänster är en DR används toorestore hello data toohello målenhet. Mer information om molnögonblicksbilder finns [använda hello StorSimple Enhetshanteraren service tootake en manuell säkerhetskopiering](storsimple-8000-manage-backup-policies-u2.md#take-a-manual-backup).

Principer för säkerhetskopiering är kopplade till säkerhetskopiering på StorSimple 8000-serien. Om det finns flera principer för säkerhetskopiering för hello samma volym och StorSimple väljer hello säkerhetskopieringsprincip med hello största antal volymer. StorSimple använder sedan senaste säkerhetskopian av hello från hello valt säkerhetskopieringsprincip toorestore hello data på hello målenhet.

Anta att det finns två principer för säkerhetskopiering, *defaultPol* och *customPol*:

* *defaultPol*: en volym *vol1*, kör daglig början på 10:30 PM.
* *customPol*: fyra volymer *vol1*, *vol2*, *vol3*, *vol4*, kör daglig början på 10:00 PM.

I det här fallet StorSimple prioriterar för kraschkonsekvens och använder *customPol* eftersom den har fler volymer. hello senaste säkerhetskopian från den här principen är används toorestore data. Mer information om hur toocreate och hantera principer för säkerhetskopiering finns för[använda hello StorSimple Enhetshanteraren service toomanage säkerhetskopieringsprinciper](storsimple-8000-manage-backup-policies-u2.md).

## <a name="common-considerations-for-device-failover"></a>Vanliga överväganden för enheten redundans

Läs hello följande information innan du växla en enhet:

* Alla hello volymer inom hello volymbehållare måste vara offline innan enheten redundans startar. I en oplanerad redundans StotSimple volymer kommer automatiskt att kopplas från. Men om du utför en planerad redundans (tootest DR), måste du utföra alla hello volymer offline.
* Endast hello volymbehållare som har en associerad ögonblicksbild i molnet visas för Katastrofåterställning. Det måste finnas minst en volymbehållare med en toorecover ögonblicksbilddata associerade molnet.
* Om det finns molntjänster ögonblicksbilder som sträcker sig över över flera volymbehållare, växlar StorSimple behållarna volymen som en uppsättning. I sällsynta fall, om lokala ögonblicksbilder som sträcker sig över över flera volymbehållare men inte gör det associerade molnögonblicksbilder, StorSimple misslyckas över hello lokala ögonblicksbilder och hello lokala data går förlorade efter DR.
* hello tillgängliga målenheterna för Katastrofåterställning är enheter som har tillräckligt diskutrymme tooaccommodate hello valda volymbehållare. Alla enheter som inte har tillräckligt med utrymme, visas inte som målenheter.
* När en DR (för en begränsad tid), hello data access prestanda kan påverkas avsevärt, som hello enhet måste tooaccess hello data från molnet hello och lagras lokalt.

#### <a name="device-failover-across-software-versions"></a>Enheten redundans över programvaruversioner

En StorSimple Device Manager-tjänst i en distribution kan ha flera enheter, både fysiska och moln, alla kör olika versioner.

Använd hello efter tabellen toodetermine om du kan växla över eller växlar tillbaka tooanother enhet som kör en annan programvaruversion och hur hello volymtyper fungerar under Katastrofåterställning.

#### <a name="failover-and-failback-across-software-versions"></a>Redundans och återställning mellan olika programvaruversioner

| Växla över / växla tillbaka från | Fysisk enhet | Molninstallation |
| --- | --- | --- |
| Uppdatering 3 tooUpdate 4 |Nivåindelade volymer misslyckas över nivåer. <br></br>Lokalt fästa volymer växlar över lokalt Fäst. <br></br> Efter en växling vid fel när du skapar en ögonblicksbild på hello uppdatering 4 enhet [heatmap-baserade spårning](storsimple-update4-release-notes.md#whats-new-in-update-4) aktiveras. |Lokalt fästa volymer redundans som nivåindelade. |
| Uppdatering 4 tooUpdate 3 |Nivåindelade volymer misslyckas över nivåer. <br></br>Lokalt fästa volymer växlar över lokalt Fäst. <br></br> Säkerhetskopieringar används toorestore behåller heatmap metadata. <br></br>Heatmap-baserade spårning är inte tillgänglig i uppdatering 3 efter en återställning efter fel. |Lokalt fästa volymer redundans som nivåindelade. |


## <a name="device-failover-scenarios"></a>Scenarier för enheter med växling vid fel

Om det finns en katastrof, kan du välja toofail över din StorSimple-enhet:

* [tooa fysisk enhet](storsimple-8000-device-failover-physical-device.md).
* [tooitself](storsimple-8000-device-failover-same-device.md).
* [tooa moln installation](storsimple-8000-device-failover-cloud-appliance.md).

hello föregående artiklar innehåller detaljerade anvisningar för varje hello ovan redundans fall.


## <a name="failback"></a>Återställning efter fel

För uppdatering 3 och senare versioner stöd StorSimple också för återställning efter fel. Återställning är bara hello omvänd växling vid fel, hello mål blir hello källa och hello ursprungliga källenheten under hello redundans nu blir hello målenhet. 

Under återställning efter fel, StorSimple igen synkroniserar hello tillbaka toohello primär Dataplats, stoppas hello i/o- och programaktivitet och övergångar tillbaka toohello ursprungliga plats.

När en redundansväxlingen är klar, utför StorSimple hello följande åtgärder:

* StorSimple rensar hello volymbehållare som har redundansväxlats från hello källenheten.
* StorSimple initierar bakgrunden per volymbehållare (redundansväxlats) på hello källenheten. Om du försöker toofail tillbaka medan hello jobb pågår, kan du få en avisering toothat effekt. Vänta tills hello jobbet har slutförts toostart hello återställning efter fel.
* hello tidsåtgång toocomplete hello borttagning av volymbehållare beror på faktorer som mängden data, ålder hello data, antal säkerhetskopior och hello nätverksbandbredden som finns tillgänglig för hello åtgärden.

Om du planerar redundanstestningar eller testa återställning efter fel, rekommenderar vi att du testar volymbehållare med mindre data (GB). Vanligtvis kan du starta hello återställning 24 timmar efter hello redundansväxlingen är klar.

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

FRÅGOR. **Vad händer om hello DR misslyckas eller har lyckats delvis?**

A. Om hello DR misslyckas, rekommenderar vi att du försöker igen. hello andra enheten beställningsjobbet om du är medveten om hello fortskrider hello första jobb och startar från den tidpunkten och framåt.

FRÅGOR. **Kan jag ta bort en enhet när hello enheten redundans pågår?**

A. Du kan inte ta bort en enhet medan en DR pågår. Du kan bara ta bort din enhet när hello DR är klar. Du kan övervaka hello enheten redundansförloppet jobbet i hello **jobb** bladet.

FRÅGOR. **När startar hello skräpinsamling på hello källenheten så att hello lokala data på källenheten har tagits bort?**

A. Skräpinsamling ska aktiveras på hello källenheten hello enheten har rensats helt. rensning av hello innehåller Rensa objekt som har växlats över från hello källenheten, till exempel volymer, säkerhetskopieobjekt (inte data), volymbehållare och principer.

FRÅGOR. **Vad händer om hello ta bort jobb som är associerade med hello volymbehållare i hello källenheten misslyckas?**

A.  Om hello tar bort jobbet misslyckas kan du kan manuellt ta bort hello volymbehållare. I hello **enheter** bladet Välj källa enheten och klicka på **volymbehållare**. Välj hello volymbehållare som du inte över och hello längst ned på bladet hello klickar du på **ta bort**. När du har tagit bort alla hello redundansväxlats volymbehållare på hello källenheten kan du starta hello återställning efter fel. Mer information finns för[ta bort en volymbehållare](storsimple-8000-manage-volume-containers.md#delete-a-volume-container).

## <a name="business-continuity-disaster-recovery-bcdr"></a>Katastrofåterställning för verksamhetskontinuitet (BCDR)

En business continuity (BCDR) katastrofåterställning inträffar när hello hela Azure-datacenter slutar att fungera. Det här scenariot kan påverka din StorSimple Device Manager-tjänst och hello associerad StorSimple-enheter.

Om en StorSimple-enhet har registrerats precis innan en katastrof inträffade, kan den här enheten måste tooundergo en fabriksåterställa. Efter hello katastrofåterställning visas hello StorSimple-enhet i hello Azure-portalen som offline. Den här enheten måste tas bort från hello-portalen. Återställ hello enheten toofactory inställningar och registrera den igen med hello-tjänsten.

## <a name="next-steps"></a>Nästa steg

Välj något av hello följande scenarier för detaljerade anvisningar om du är klar tooperform redundans enhet:

- [Växla över tooanother fysisk enhet](storsimple-8000-device-failover-physical-device.md)
- [Växla över toohello samma enhet](storsimple-8000-device-failover-same-device.md)
- [Växla över tooStorSimple moln-enhet](storsimple-8000-device-failover-cloud-appliance.md)

Om du har redundansväxlats enheten, Välj något av följande alternativ för hello:

* [Inaktivera eller ta bort din StorSimple-enhet](storsimple-8000-deactivate-and-delete-device.md).
* [Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).

