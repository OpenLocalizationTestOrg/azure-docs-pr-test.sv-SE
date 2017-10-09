---
title: "aaaReplace en PCM på StorSimple-enheten | Microsoft Docs"
description: "Förklarar hur tooremove och Ersätt hello ström och kylning modul (PCM) på din StorSimple-enhet"
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 24a158cb-0b79-4908-bb5a-431e48760f6a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/18/2016
ms.author: alkohli
ms.openlocfilehash: cc19ccb29884557720f7538b90dfb05268330b72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a>Ersätta en ström och kylning modulen på din StorSimple-enhet
## <a name="overview"></a>Översikt
hello ström och kylning modul (PCM) i Microsoft Azure StorSimple-enheten består av en strömförsörjning och kylfläktar som styrs via hello primära och EBOD bilagor. Det finns endast en modell för PCM som är certifierad för varje enhet. hello primära höljet är certifierad för en 764 W PCM och hello EBOD hölje är certifierad för en 580 W PCM. Även om hello PCMs för hello primära höljet och hello EBOD hölje mellan är hello ersättning proceduren identiska.

Den här självstudiekursen beskrivs hur du:

* Ta bort en PCM
* Installera en ersättning PCM

> [!IMPORTANT]
> Innan du tar bort och ersätter en PCM granska hello säkerhetsinformation i [ersättning av StorSimple maskinvara komponenten](storsimple-hardware-component-replacement.md).
> 
> 

## <a name="before-you-replace-a-pcm"></a>Innan du ersätter en PCM
Tänk på följande viktiga problem innan du ersätter din PCM hello:

* Om hello strömförsörjning av hello PCM misslyckas, lämna hello felaktiga module installerad ta bort hello strömsladd hello-fläkt fortsätter tooreceive strömförsörjning från hello höljet och fortsätta tooprovide rätt kylning. Om hello-fläkt misslyckas måste hello PCM toobe ersättas omedelbart.
* Innan du tar bort hello PCM koppla hello ur strömmen hello PCM genom att inaktivera hello huvudsakliga växel (om sådan finns) eller genom att ta bort hello strömsladd fysiskt. Detta ger tooyour varningssystem att en power avstängning är nära förestående.
* Kontrollera att hello andra PCM fungerar för fortsatt systemåtgärd innan ersätter hello felaktiga PCM. En felaktig PCM måste ersättas med en fullt fungerande PCM så snart som möjligt.
* PCM modulen ersättning tar bara några minuter toocomplete, men den måste slutföras inom 10 minuter på att ta bort hello misslyckades PCM tooprevent överhettning.
* Observera att hello ersättning 764 W PCM moduler från hello factory inte innehålla hello säkerhetskopiering batterimodulen. Du behöver tooremove hello batteri från din felaktiga PCM och infoga det i hello ersättning modulen tidigare tooperforming hello ersättning. Mer information finns i hur för[ta bort och infoga en reserv batteri modul](storsimple-battery-replacement.md).

## <a name="remove-a-pcm"></a>Ta bort en PCM
Följ dessa instruktioner när du är klar tooremove en ström och kylning modul (PCM) från Microsoft Azure StorSimple-enhet.

> [!NOTE]
> Innan du tar bort din PCM kontrollerar du att rätt ersätter (764 W för hello primära höljet) eller 580 W för hello EBOD hölje.
> 
> 

#### <a name="tooremove-a-pcm"></a>tooremove en PCM
1. I hello klassiska Azure-portalen klickar du på **enheter** > **Underhåll** > **maskinvarustatus**. Kontrollera status för hello hello PCM komponenter under **delade komponenter** tooidentify som PCM misslyckades:
   
   * Om en strömförsörjning i PCM 0 har misslyckats hello status för **strömförsörjning i PCM 0** blir röd.
   * Om en strömförsörjning i PCM 1 har misslyckats hello status för **strömförsörjning i PCM 1** blir röd.
   * Om hello-fläkt i PCM 1 har misslyckats hello statusen **kylning 0 för PCM 0** eller **kylning 1 för PCM 0** blir röd.
2. Leta upp hello misslyckade PCM på hello tillbaka av hello primära enhet. Om du kör en 8600-modell, identifiera hello primära höljet genom att titta på hello System identifiering enhetsnummer visas hello frontpanel Indikator visas. Hej standard enhets-ID som visas på hello primära höljet är **00**, medan hello standard enhets-ID som visas på hello EBOD hölje är **01**. hello beskrivs följande diagram och tabellen hello frontpanel hello Indikator visas.
   
    ![System-ID på OPS frontpanel](./media/storsimple-power-cooling-module-replacement/IC740991.png)
   
     **Bild 1** framför panelen av hello-enhet  
   
   | Etikett | Beskrivning |
   |:--- |:--- |
   | 1 |Ljud av |
   | 2 |System power |
   | 3 |Modul-fel |
   | 4 |Logiska fel |
   | 5 |Enhet ID visas |
3. hello övervakning indikator led i hello baksidan hello primära höljet kan också användas tooidentify hello felaktiga PCM. Se hello följande diagram och tabell toounderstand hur toouse hello indikatorer toolocate hello felaktiga PCM. Till exempel om hello LEDDE motsvarande toohello **fläkt misslyckas** är upplysta, hello-fläkt misslyckades. Likaså om hello LEDDE för motsvarande**AC misslyckas** är upplysta, hello strömförsörjning misslyckades. 
   
    ![Bakplan för enheten PCM övervakning indikatorn indikatorer](./media/storsimple-power-cooling-module-replacement/IC740992.png)
   
     **Bild 2** tillbaka of PCM med indikator indikatorer
   
   | Etikett | Beskrivning |
   |:--- |:--- |
   | 1 |AC strömavbrott |
   | 2 |Fläkt fel |
   | 3 |Batteri fel |
   | 4 |PCM OK |
   | 5 |DC strömavbrott |
   | 6 |Batteri felfri |
4. Läs följande diagram över hello baksidan hello StorSimple enheten toolocate hello misslyckades PCM modulen toohello. PCM 0 är hello vänster och PCM 1 på hello rätt. hello i tabellen nedan beskrivs hello moduler.
   
     ![Bakplan av primära Höljesmoduler](./media/storsimple-power-cooling-module-replacement/IC740994.png)
   
     **Bild 3** baksidan av enheten med plugin-program 
   
   | Etikett | Beskrivning |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Styrenhet 0 |
   | 4 |Kontrollant 1 |
5. Aktivera ut hello felaktiga PCM och koppla från hello strömsladd leverans. Du kan nu ta bort hello PCM.
6. Tag hello låset och hello sida av hello PCM hantera mellan USB och pekfingret och klämma dem tillsammans tooopen hello referensen.
   
    ![Öppna PCM referensen](./media/storsimple-power-cooling-module-replacement/IC740995.png)
   
    **Bild 4** öppnas hello PCM hantera
7. Greppet hello hantera och ta bort hello PCM.
   
    ![Ta bort PCM-enhet](./media/storsimple-power-cooling-module-replacement/IC740996.png)
   
    **Bild 5** tar bort hello PCM

## <a name="install-a-replacement-pcm"></a>Installera en ersättning PCM
Följ dessa instruktioner tooinstall en PCM i din StorSimple-enhet. Se till att du har infogat hello säkerhetskopiering batteri modulen tidigare tooinstalling hello ersättning PCM (gäller too764 W PCMs). Mer information finns i hur för[ta bort och infoga en reserv batteri modul](storsimple-battery-replacement.md).

#### <a name="tooinstall-a-pcm"></a>tooinstall en PCM
1. Kontrollera att du har hello rätt ersättning PCM för denna enhet. hello primära höljet måste en 764 W PCM och hello EBOD hölje måste en 580 W PCM. Du bör inte försöka toouse hello 580 W PCM i hello primära hölje eller hello 764 W PCM i hello EBOD hölje. Följande bild visar där tooidentify informationen på hello etiketten som fästs toohello PCM hello.
   
    ![Enheten PCM etikett](./media/storsimple-power-cooling-module-replacement/IC740973.png)
   
    **Bild 6** PCM etikett
2. Sök efter skada toohello hölje, och var särskilt uppmärksam toohello kopplingar. 
   
   > [!NOTE]
   > **Installera inte hello modulen om någon koppling stift är böjda.**
   > 
   > 
3. Öppna position, bild hello-modulen till hello höljet med hello PCM hantera i hello.
   
    ![Installerar PCM-enhet](./media/storsimple-power-cooling-module-replacement/IC740975.png)
   
    **Bild 7** installerar hello PCM
4. Stäng hello PCM referensen manuellt. Du bör höra en klickning som snabbt tillkallar hello handtaget låstypen. 
   
   > [!NOTE]
   > tooensure som hello connector PIN-koder har blivit engagerade du försiktigt tug hello handtag utan att släppa hello spärren. Om hello PCM bilder ut, innebär det att hello spärren stängdes innan hello kopplingar ägnar åt.
   > 
   > 
5. Ansluta hello kablar toohello power kraftkälla och toohello PCM.
6. Skydda hello stam befrielse balar. 
7. Aktivera hello PCM.
8. Kontrollera att hello ersättning lyckades: hello klassiska Azure-portalen för din StorSimple Manager-tjänsten, navigera för**enheter** > **Underhåll**  >  **Maskinvarustatus**. Under **delade komponenter**, hello status för hello PCM vara grön. 
   
   > [!NOTE]
   > Det kan ta några minuter för hello ersättning PCM toocompletely initieras.
   > 
   > 

## <a name="next-steps"></a>Nästa steg
Lär dig mer om [ersättning av StorSimple maskinvara komponenten](storsimple-hardware-component-replacement.md).

