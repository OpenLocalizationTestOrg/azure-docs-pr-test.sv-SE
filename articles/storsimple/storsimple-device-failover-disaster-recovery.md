---
title: aaaStorSimple redundans och disaster recovery | Microsoft Docs
description: "Lär dig hur toofail över tooitself din StorSimple-enhet, en annan fysisk enhet eller en virtuell enhet."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 5751598e-49c8-42b3-8121-fea5857a7d83
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/16/2016
ms.author: alkohli
ms.openlocfilehash: 00ce365f8a9095d1f0292e665d7f9eaa844b44ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="failover-and-disaster-recovery-for-your-storsimple-device"></a>Redundans och disaster recovery för din StorSimple-enhet
## <a name="overview"></a>Översikt
Den här självstudiekursen beskriver hello steg krävs toofail via en StorSimple-enhet i en katastrof hello-händelse. En redundansväxling kan du toomigrate dina data från en källenhet i hello datacenter tooanother fysiska eller även en virtuell enhet finns i hello samma eller en annan geografisk plats. 

Katastrofåterställning (DR) styrd via funktionen för hello enheten växling vid fel och initieras från hello **enheter** sidan. Den här sidan tabulates alla hello StorSimple-enheter anslutna tooyour StorSimple Manager-tjänsten. För varje enhet, hello eget namn, status, etablerad och maximal kapacitet visas typ och modell.

![Enheter-sidan](./media/storsimple-device-failover-disaster-recovery/IC740972.png)

hello anvisningarna i den här kursen gäller tooStorSimple fysiska och virtuella enheter över alla programvaruversioner.

## <a name="disaster-recovery-dr-and-device-failover"></a>Katastrofåterställning (DR) och växling vid fel för enheten
I en (DR) katastrofåterställning, hello primära enhet slutar fungera. I så fall kan du flytta hello molndata som associeras med hello misslyckad enhet tooanother enhet genom att använda hello primära enhet som hello *källa* och ange en annan enhet som hello *mål*. Du kan välja en eller flera volym behållare toomigrate toohello målenhet. Den här processen är refererad tooas hello *redundans*. 

Under hello redundans hello volymbehållarna från hello källenheten ändra ägarskap och är överförda toohello målenhet. När hello volymbehållare ändra ägarskap, tas dessa bort från hello källenheten. När hello borttagningen är klar misslyckades kan sedan hello målenhet tillbaka.

Vanligtvis är en Katastrofåterställning, hello senaste säkerhetskopian används toorestore hello data toohello målenhet. Men om det finns flera principer för säkerhetskopiering för hello samma volym, sedan hello säkerhetskopieringsprincip med hello största antal volymer hämtar plockats och hello senaste säkerhetskopian från principen är används toorestore hello data på hello målenhet.

Som exempel, om det finns två säkerhetskopieringsprinciper (en standard och en anpassad) *defaultPol*, *customPol* med hello följande information:

* *defaultPol* : en volym *vol1*, kör daglig början på 10:30 PM.
* *customPol* : fyra volymer *vol1*, *vol2*, *vol3*, *vol4*, kör daglig början på 10:00 PM.

I det här fallet *customPol* kommer att användas som innehåller flera volymer och vi prioriterar för kraschkonsekvens. hello senaste säkerhetskopian från den här principen är används toorestore data.

## <a name="considerations-for-device-failover"></a>Överväganden för växling vid fel för enheten
I en katastrof hello-händelse, kan du välja toofail över din StorSimple-enhet:

* tooa fysisk enhet 
* tooitself
* tooa virtuell enhet

Kom ihåg hello följande för varje enhet redundans:

* hello kraven för Katastrofåterställning är att alla hello volymer inom hello volymbehållare är offline och hello volymbehållare har en associerad ögonblicksbild i molnet. 
* hello tillgängliga målenheterna för Katastrofåterställning är enheter som har tillräckligt diskutrymme tooaccommodate hello valda volymbehållare. 
* hello enheter som är anslutna tooyour tjänsten men inte uppfyller villkoren för hello tillräckligt utrymme är inte tillgänglig som målenheter.
* Efter en DR för en begränsad tid hello data access prestanda kan påverkas avsevärt, som hello enhet kommer måste tooaccess hello data från molnet hello och lagras lokalt.

#### <a name="device-failover-across-software-versions"></a>Enheten redundans över programvaruversioner
En StorSimple Manager-tjänsten i en distribution kan ha flera enheter, både fysiska och virtuella, alla kör olika versioner. Beroende på hello programvaruversionen kan hello volymtyper på hello enheter också vara olika. Till exempel en enheten som kör uppdatering 2 eller högre skulle ha lokalt Fäst och nivåindelade volymer (med arkivering som en del av skikt). En enhet före uppdatering 2 på hello andra sidan kan ha nivåer och arkivering volymer. 

Använd hello efter tabellen toodetermine om kan du växla över tooanother enhet som kör en annan programvara version och hello beteendet för volymtyper under Katastrofåterställning.

| Växla över från | Tillåten för den fysiska enheten | Tillåtna för virtuella enheten |
| --- | --- | --- |
| Uppdatering 2 toopre-uppdatering 1 (version 0.1, 0,2, 0.3) |Nej |Nej |
| Uppdatering 2 tooUpdate 1 (1, 1.1, 1.2) |Ja <br></br>Om använder lokalt Fäst eller nivåindelade volymer eller en blandning av två, hello alltid misslyckades över volymer som nivåer. |Ja<br></br>Om du använder lokalt Fäst volymer, kunde dessa över nivåer. |
| Uppdatering 2 tooUpdate 2 (senare version) |Ja<br></br>Om du använder lokalt fästa eller nivåindelade volymer eller en blandning av två har hello volymer alltid redundansväxlats som hello startar volymtyp; nivåer som nivåindelade och lokalt Fäst lokalt Fäst. |Ja<br></br>Om du använder lokalt Fäst volymer, kunde dessa över nivåer. |

#### <a name="partial-failover-across-software-versions"></a>Partiell redundans över programvaruversioner
Följ dessa riktlinjer om du avser tooperform en partiell redundans med en StorSimple-källenheten körs före uppdateringen 1 tooa mål som kör uppdatering 1 eller senare. 

| Partiell växling från | Tillåten för den fysiska enheten | Tillåtna för virtuella enheten |
| --- | --- | --- |
| Före uppdatering 1 (version 0.1, 0,2, 0.3) tooUpdate 1 eller senare |Ja, se nedan för hello bästa praxis tips. |Ja, se nedan för hello bästa praxis tips. |

> [!TIP]
> Det uppstod molnet metadata och data format ändras uppdatering 1 och senare versioner. Därför kan rekommenderar vi inte en partiell växling från före uppdateringen 1 tooUpdate 1 eller senare versioner. Om du behöver tooperform en partiell växling vid fel, rekommenderar vi att du först installera uppdatering 1 eller senare på båda hello enheter (källa och mål) och fortsätt sedan med hello växling vid fel. 
> 
> 

## <a name="fail-over-tooanother-physical-device"></a>Växla över tooanother fysisk enhet
Utför följande steg toorestore hello din enhet tooa fysiska målenhet.

1. Kontrollera att hello volymbehållare som du vill ha toofail över har associerade molnögonblicksbilder.
2. På hello **enheter** klickar du på hello **Volymbehållare** fliken.
3. Välj en volymbehållare som du vill att toofail över tooanother enhet. Klicka på hello volym behållaren toodisplay hello lista över volymer i den här behållaren. Välj en volym och klicka på **ta Offline** tootake hello volymen offline. Upprepa proceduren för alla hello volymer i hello volymbehållare.
4. Hello Upprepa föregående steg för alla hello volymbehållare som toofail över tooanother enhet.
5. På hello **enheter** klickar du på **redundans**.
6. I guiden för hello som öppnas, under **välja volym behållaren toofail över**:
   
   1. Välj hello volymbehållare som toofail över i hello lista över volymbehållare.
      **Endast hello volymbehållare med associerade molnögonblicksbilder och offline volymer visas.**
   2. Under **välja en målenhet** för hello volymer i hello valt behållare, väljer du en målenhet hello nedrullningsbara listan över tillgängliga enheter. Endast hello-enheter som har tillgänglig kapacitet för hello visas i listrutan hello.
   3. Slutligen granska alla hello växling vid fel inställningar under **bekräfta redundans**. Klicka på kryssikonen hello ![kryssikonen](./media/storsimple-device-failover-disaster-recovery/IC740895.png).
7. Skapas ett jobb för växling vid fel som kan övervakas via hello **jobb** sidan. Om hello volymbehållare som du redundansväxlade har lokala volymer, kommer du se enskilda återställningsjobb för varje lokal volym (inte för nivåindelade volymer) i hello behållaren. Dessa återställningspunkter jobb kan ta ganska tid toocomplete. Det är troligt att hello beställningsjobbet kan slutföra tidigare. Observera att dessa volymer måste lokala garantier endast när hello återställningsjobb har slutförts. När hello växling vid fel har slutförts går toohello **enheter** sidan.                                            
   
   1. Välj hello-enhet som har använts som hello målenhet för hello failover-processen.
   2. Gå toohello **Volymbehållare** sidan. Alla hello volymbehållare, tillsammans med hello volymer från hello gamla enhet, ska visas.

## <a name="failover-using-a-single-device"></a>Redundans med en enskild enhet
Utföra hello följande steg om du bara har en enda enhet och behov tooperform en växling vid fel.

1. Skapa molnögonblicksbilder av alla hello volymer i enheten.
2. Återställa din enhet toofactory standardvärden. Följ hello detaljerade instruktioner i [hur tooreset toofactory en StorSimple-enheten standardinställningar](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).
3. Konfigurera din enhet och registrera den igen med din StorSimple Manager-tjänst.
4. På hello **enheter** sidan hello gamla enheten ska visa **Offline**. hello nyregistrerade enheten ska visa **Online**.
5. Slutför hello minimikraven för konfiguration av hello enhet först för nya hello-enhet. 
   
   > [!IMPORTANT]
   > **Din DR misslyckas på grund av ett fel i hello aktuella implementeringen om hello lägsta konfigurationen inte har slutförts först. Problemet korrigeras i en senare version.**
   > 
   > 
6. Välj hello gamla enhet (offline status) och klicka på **redundans**. Växla över den här enheten och ange hello målenhet som hello nyregistrerade enheten hello i guiden som visas. Detaljerade anvisningar finns för[växla över tooanother fysisk enhet](#fail-over-to-another-physical-device).
7. En enhet återställningsjobbet skapas att du kan övervaka från hello **jobb** sidan.
8. När hello jobbet har slutförts, komma åt hello ny enhet och gå toohello **Volymbehållare** sidan. Alla hello volymbehållarna från hello gamla enheten nu bör migrerade toohello ny enhet.

## <a name="fail-over-tooa-storsimple-virtual-device"></a>Växla över tooa virtuell StorSimple-enhet
Du måste ha en StorSimple virtuell enhet som har skapats och konfigurerats tidigare toorunning den här proceduren. Om Kör uppdatering 2, Överväg att använda en virtuell enhet för 8020 för hello DR som har 64 TB och använder Premium-lagring. 

Utföra hello följande steg toorestore hello enheten tooa mål virtuella StorSimple-enheten.

1. Kontrollera att hello volymbehållare som du vill ha toofail över har associerade molnögonblicksbilder.
2. På hello **enheter** klickar du på hello **Volymbehållare** fliken.
3. Välj en volymbehållare som du vill att toofail över tooanother enhet. Klicka på hello volym behållaren toodisplay hello lista över volymer i den här behållaren. Välj en volym och klicka på **ta Offline** tootake hello volymen offline. Upprepa proceduren för alla hello volymer i hello volymbehållare.
4. Hello Upprepa föregående steg för alla hello volymbehållare som toofail över tooanother enhet.
5. På hello **enheter** klickar du på **redundans**.
6. I guiden för hello som öppnas, under **välja volym behållaren toofailover**, Slutför hello följande:
   
    a. Välj hello volymbehållare som toofail över i hello lista över volymbehållare.
   
    **Endast hello volymbehållare med associerade molnögonblicksbilder och offline volymer visas.**
   
    b. Under **välja en målenhet för hello volymer i hello valt behållare**, Välj hello virtuella StorSimple-enheten från hello nedrullningsbara listan över tillgängliga enheter. **Endast hello-enheter som har tillräckligt med kapacitet visas i listrutan hello.**  
7. Slutligen granska alla hello växling vid fel inställningar under **bekräfta redundans**. Klicka på kryssikonen hello ![kryssikonen](./media/storsimple-device-failover-disaster-recovery/IC740895.png).
8. När hello växling vid fel har slutförts går toohello **enheter** sidan.
   
    a. Välj hello virtuella StorSimple-enheten som användes som hello målenhet för hello failover-processen.
   
    b. Gå toohello **Volymbehållare** sidan. Alla hello volymbehållare, tillsammans med hello volymer från hello gamla enheten bör nu visas.

![Video tillgänglig](./media/storsimple-device-failover-disaster-recovery/Video_icon.png) **Video tillgänglig**

toowatch en video som visar hur du kan återställa en misslyckad över fysiska enheten tooa-enhet i hello moln, klickar du på [här](https://azure.microsoft.com/documentation/videos/storsimple-and-disaster-recovery/).

## <a name="failback"></a>Återställning efter fel
För uppdatering 3 och senare versioner stöd StorSimple också för återställning efter fel. När hello redundansväxlingen är klar, utförs hello följande åtgärder:

* hello volymbehållare som har redundansväxlats rensas från hello källenheten.
* Ett bakgrundsjobb per volymbehållare (redundansväxlats) initieras på hello källenheten. Om du försöker toofailback medan hello jobb pågår, får du en avisering toothat effekt. Du behöver toowait tills hello jobbet har slutförts toostart hello återställning efter fel. 
  
    hello tid toocomplete hello borttagning av volymbehållare är beroende av olika faktorer som mängden data, ålder hello data, antal säkerhetskopior och hello nätverksbandbredden som finns tillgänglig för hello åtgärden. Om du planerar att testa redundans/återställning efter fel, rekommenderar vi att du testar volymbehållare med mindre data (GB). I de flesta fall kan du starta hello återställning 24 timmar efter hello redundansväxlingen är klar. 

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar
FRÅGOR. **Vad händer om hello DR misslyckas eller har lyckats delvis?**

A. Om hello DR misslyckas, rekommenderar vi att du försöker igen. hello gång runt DR vet vad alla gjordes och när stoppats hello processen hello första gången. hello DR-processen startar från den tidpunkten och framåt. 

FRÅGOR. **Kan jag ta bort en enhet när hello enheten redundans pågår?**

A. Du kan inte ta bort en enhet medan en DR pågår. Du kan bara ta bort din enhet när hello DR är klar.

FRÅGOR.    **När startar hello skräpinsamling på hello källenheten så att hello lokala data på källenheten har tagits bort?**

A. Skräpinsamling aktiveras på hello källenheten förrän hello enheten rensas helt. rensning av hello innehåller Rensa objekt som har växlats över från hello källenheten, till exempel volymer, säkerhetskopieobjekt (inte data), volymbehållare och principer.

FRÅGOR. **Vad händer om hello ta bort jobb som är associerade med hello volymbehållare i hello källenheten misslyckas?**

A.  Om hello tar bort jobbet misslyckas, behöver du toomanually utlösaren hello borttagning av hello volymbehållare. I hello **enheter** väljer enheten källa och klicka på **volymbehållare**. Välj hello volymbehållare som du inte över och hello längst ned på sidan för hello, klickar du på **ta bort**. När du har tagit bort alla hello redundansväxlats volymbehållare på hello källenheten kan du starta hello återställning efter fel.

## <a name="business-continuity-disaster-recovery-bcdr"></a>Katastrofåterställning för verksamhetskontinuitet (BCDR)
En business continuity (BCDR) katastrofåterställning inträffar när hello hela Azure-datacenter slutar att fungera. Detta kan påverka din StorSimple Manager-tjänsten och hello associerad StorSimple-enheter.

Om StorSimple-enheter som registrerats precis innan en katastrof inträffade måste dessa StorSimple-enheter tooundergo en fabriksåterställa. Efter hello katastrofåterställning visas hello StorSimple-enhet som offline. Hej StorSimple-enhet måste tas bort från hello-portalen och en fabriksåterställning ska göras, följt av en ny registrering.

## <a name="next-steps"></a>Nästa steg
* När du har utfört en växling vid fel, kanske du måste för[inaktivera eller ta bort din StorSimple-enhet](storsimple-deactivate-and-delete-device.md).
* Information om hur toouse hello StorSimple Manager-tjänsten finns för[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

