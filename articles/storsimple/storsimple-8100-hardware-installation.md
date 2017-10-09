---
title: aaaInstall Microsoft Azure StorSimple 8100-enhet | Microsoft Docs
description: Beskriver hur toounpack, rackmontera och kabelanslut din 8100 StorSimple-enhet innan du distribuerar och konfigurerar hello programvara.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 6098a01e-c031-488a-a8d7-0b607ce665e1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 76b94e57318b6c25dc3333ae73edc9e4752b7d1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="unpack-rack-mount-and-cable-your-storsimple-8100-device"></a>Packa upp, rackmontera, och kabelanslut din 8100 StorSimple-enhet
## <a name="overview"></a>Översikt
Din Microsoft Azure StorSimple 8100-enhet är en enda enhet, rackmonterad enhet. Den här självstudiekursen beskrivs hur toounpack, rackmonterad och kabel hello StorSimple 8100-enhet maskinvara innan du konfigurerar och distribuerar hello StorSimple-enhet.

## <a name="unpack-your-storsimple-8100-device"></a>Packa upp din StorSimple 8100-enhet
hello följande steg är tom, detaljerade anvisningar om hur toounpack lagringsenheten StorSimple 8100. Den här enheten levereras i en enda ruta.

### <a name="prepare-toounpack-your-device"></a>Förbereda toounpack enheten
Granska hello följande information innan du packar upp din enhet.

![Varningsikon](./media/storsimple-safety/IC740879.png)![tunga vikt ikonen](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **varning!**

1. Kontrollera att du har två personer tillgängliga toomanage hello vikten av hello höljet om hanterar manuellt. Ett helt konfigurerade hölje kan väga in too32 kg (70 pund).
2. Hello placeras på en plan, nivå-yta.

Därefter Slutför följande steg toounpack hello enheten.

#### <a name="toounpack-your-device"></a>toounpack din enhet
1. Granska hello rutan och hello paketering skum för crushes delar, vattenstämplar skador eller andra uppenbara skador. Öppna inte hello rutan om hello rutan eller paketering kraftigt är skadad. Kontrollera [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md) toohelp du bedöma om hello enheten är i gott skick.
2. Packa upp hello rutan. hello följande bild visar hello packat upp din StorSimple-enhet.
   
     ![Packa upp lagringsenheten](./media/storsimple-8100-hardware-installation/HCSUnpackyour2Udevice.png)
   
    **Packa upp visning av lagringsenheten**
   
   | Etikett | Beskrivning |
   | --- | --- |
   |   1 |Packkorg rutan |
   |   2 |Nedre skum |
   |   3 |Enhet |
   |   4 |Övre skum |
   |   5 |Tillbehör rutan |
3. Kontrollera att du har när hello rutan:
   
   * 1 enhet har hårddiskar.
   * 2 strömkablar
   * 1 övergång Ethernet-kabel
   * 2 seriekonsolen kablar
   * 1 seriell USB konverterare för seriell åtkomst
   * 1 förfalska T10 för
   * 4 QSFP-till-SFP + kort för användning med 10 GbE-nätverkskort
   * 1-rackmonterad kit (2 sida spår med montera maskinvara)
   * Komma igång-dokumentationen
     
     Om du inte fick någon hello objekten i listan ovan, [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md).

hello nästa steg är toorack montera din enhet.

## <a name="rack-mount-your-storsimple-8100-device"></a>Rackmonterade StorSimple 8100-enhet
Följ hello nästa steg tooinstall lagringsenheten StorSimple 8100 i en standard 19-tums rack med främre och bakre meddelandena. Hej StorSimple 8100-enhet har en enda primär enhet.

hello installation består av flera steg, som beskrivs i följande procedurer hello.

> [!IMPORTANT]
> StorSimple-enheter måste vara rackmonterade för att fungera korrekt.
> 
> 

### <a name="prepare-hello-site"></a>Förbereda hello plats
hello enheten måste vara installerad i ett standard 19-tums rack som har både främre och bakre meddelandena. Använd hello följa proceduren tooprepare för Rackinstallation.

#### <a name="tooprepare-hello-site-for-rack-installation"></a>tooprepare hello plats för Rackinstallation
1. Kontrollera att enheten hello vilar på ett säkert sätt på en flat stabilt och nivå arbete ytan (eller liknande).
2. Kontrollera att hello platsen där du ska tooset upp har standard AC-ström från en oberoende källa eller en rack power distributionsenhet (PDU) med en avbrottsfri ange (UPS).
3. Kontrollera att en 2U platsen är tillgänglig på hello rack som du avser att toomount hello enhet.

![Varningsikon](./media/storsimple-safety/IC740879.png)![tunga vikt ikonen](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **varning!**

Kontrollera att du har två personer tillgängliga toomanage hello vikt om du hanterar hello Enhetsinställningar manuellt. Ett helt konfigurerade hölje kan väga in too32 kg (70 pund).

### <a name="rack-prerequisites"></a>Rack krav
hello 8100 höljet är utformat för installation i en standard 19-tums rack CAB med:

* Minsta djup 27.84 tum från rack efter toopost.
* Högsta vikt för 32 kg för hello-enhet
* Högsta tillbaka tryck 5 Pascal (0,5 mätare).

### <a name="rack-mounting-rail-kit"></a>Rack montering tåg kit
En uppsättning montera spår har angetts för användning med hello 19-tums rack kabinettfilen. hello spår har testat toohandle hello maximala höljet vikt. Dessa spår kommer också att tillåta installation av flera höljen utan att förlora utrymme i hello rack.

#### <a name="tooinstall-hello-device-on-hello-rails"></a>tooinstall hello enheten hello spår
1. Utför det här steget endast om inre spår inte har installerats på enheten. Normalt installeras hello inre spår i hello fabriken. Om spår inte har installerats, installerar du hello tåg vänster och höger tåg bilder toohello sidor hello höljet chassin. De fäster med sex mått skruvar på varje sida. toohelp med orientering hello tåg bilder markeras **LH – fram** och **RH – fram**, och hello slutet som fästs mot hello bakom hello höljet har en avsmalnande end.<br/>
   
    ![Koppling tåg bilder tooenclosure chassi](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)

    **Koppling inre tåg bilder toohello sidor av hello hölje**
   
    Etikett | Beskrivning
    ----- | -----------
    1     | M 3 x 4 knappen head skruvar
    2     | Chassi bilder

2. Koppla hello yttre vänstra tåg och yttre höger sammansättningar toohello rack CAB lodräta medlemmar. hello hakparenteser markeras **LH**, **RH**, och **den här sidan uppåt** tooguide du via hello korrigera orientering.
3. Leta upp hello tåg PIN-koder på hello främre och bakre hello tåg sammansättningen. Utöka hello tåg toofit mellan hello rack inlägg och infoga hello PIN-koder i hello främre och bakre rack post lodräta medlem hål. Var noga med att hello tåg sammansättningen är nivå.
4. Använd två hello tillhandahålls mått skruvar toosecure hello tåg sammansättningen toohello rack lodräta medlemmar. Använd en skruv på hello främre och en på hello bakre.
5. Upprepa dessa steg för hello andra tåg-sammansättningen.<br/>
   
     ![Koppling tåg bilder toorack CAB](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)
   
    **Bifoga yttre tåg sammansättningar toohello rack**
   
   | Etikett | Beskrivning |
   | --- | --- |
   |   1 |Minskning skruv |
   |   2 |Ruta hål främre rack post skruv |
   |   3 |Vänster tåg främre plats PIN-koder |
   |   4 |Minskning skruv |
   |   5 |Vänster tåg bakre plats PIN-koder |

### <a name="mounting-hello-device-in-hello-rack"></a>Montera hello-enhet i hello rack
Använd hello rack spår som precis har installerats och utför följande steg toomount hello-enhet i hello rack hello.

#### <a name="toomount-hello-device"></a>toomount hello enhet
1. Med en assistent lyfter hello höljet och justera med hello rack spår.
2. Infoga noggrant hello enheten i hello spår och sedan push hello enheten helt i hello rack cab.<br/>
   
    ![Lägga till enheten i hello rack](./media/storsimple-8100-hardware-installation/HCSInsertingDeviceintheRack.png)
   
    **Montera hello-enhet i hello rack**
3. Ta bort hello vänster och höger främre flänsad caps genom att dra hello caps ledigt. hello flänsad caps Fäst bara till hello flänsar.
4. Skydda hello höljet i hello rack genom att installera en angivna stjärnskruvmejsel head skruv via varje flänsad vänster och höger.
5. Installera hello flänsad caps genom att trycka ned dem till rätt plats och fästa dem på plats.<br/>
   
     ![Installera flänsad caps](./media/storsimple-8100-hardware-installation/HCSInstallingFlangeCaps.png)
   
    **Installera hello flänsad caps**
   
   | Etikett | Beskrivning |
   | --- | --- |
   |   1 |Höljet fästanordning skruv |

hello nästa steg är toocable enheten till ström, nätverk och serieåtkomst.

## <a name="cable-your-storsimple-8100-device"></a>Kabelanslut din 8100 StorSimple-enhet
hello följande procedurer beskriver hur toocable din StorSimple 8100-enhet till ström, nätverk och seriella anslutningar.

### <a name="prerequisites"></a>Krav
Innan du börjar hello kablar på enheten, behöver du:

* Dina lagringsenhet, helt Uppackad och rackmonterade.
* 2 strömkablar som medföljde din enhet
* Åtkomst too2 Kraftfördelningsenheter (rekommenderas).
* Nätverkskablarna
* Angivna seriella kablar
* Seriell USB-konverteraren med hello lämplig drivrutin installeras på din dator (om det behövs)
* Angivna 4 QSFP-till-SFP + kort för användning med 10 GbE-nätverkskort
* [Maskinvara som stöds för hello 10 GbE-nätverkskort på din StorSimple-enhet](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="power-cabling"></a>Kablar
Enheten innehåller redundant ström och kylning moduler (PCMs). Både PCMs måste vara installerad och anslutna toodifferent power källor tooensure hög tillgänglighet.

Utför följande steg toocable hello enheten för power.

[!INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

### <a name="network-cabling"></a>Nätverkskablar
Enheten är en aktiv standby-konfiguration: samtidigt, en modul som domänkontrollanten är aktiv och bearbetning av alla disk- och åtgärder vid hello modulen andra domänkontrollanter är i vänteläge. Om en domänkontrollant inte hello vänteläge controller aktiveras omedelbart och fortsätter alla åtgärder för hello disk och nätverk.

toosupport den här redundant controller redundansen måste toocable enheten nätverk enligt beskrivningen i följande steg hello.

#### <a name="toocable-for-network-connection"></a>toocable för nätverksanslutning
1. Enheten har sex nätverksgränssnitt på varje domänkontrollant: fyra 1 Gbit/s och två 10 Gbit/s Ethernet-portar. Identifiera hello olika data portar på hello bakplan av enheten.
   
    ![Bakplan av 8100-enhet](./media/storsimple-8100-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)
   
    **Tillbaka på hello enhet visar dataportar**
   
   | Etikett | Beskrivning |
   | --- | --- |
   |   0,1,4,5 |1 GbE-nätverkskort |
   |   2,3 |10 GbE-nätverkskort |
   |   6 |Seriella portar |
2. Se följande diagram för nätverkskablar hello. (hello minsta nätverkskonfigurationen visas med blå heldragen linje. Ytterligare konfiguration krävs för hög tillgänglighet och prestanda visas med streckade linjer.)

    ![Kabelansluta den 2U för nätverket](./media/storsimple-8100-hardware-installation/HCSCableYour2UDeviceforNetwork.png)

    **Nätverket kablar för din enhet**

   |Etikett | Beskrivning |
   |----- | ----------- |
   | A    | LAN med Internetåtkomst |
   | B    | Styrenhet 0 |
   | C    | PCM 0 |
   | D    | Kontrollant 1 |
   | E    | PCM 1 |
   | F, G | Värdar |
   | 0-5  | Nätverksgränssnitt |



När kablar hello enheten kräver hello minimikraven för konfiguration:

* Minst två nätverkskort som är anslutna på varje domänkontrollant för att komma åt molnet och en för iSCSI. hello DATA 0 port aktiveras och konfigureras via hello seriekonsolen av hello enhet automatiskt. Förutom DATA 0 måste en annan dataport också toobe konfigureras via hello klassiska Azure-portalen. I det här fallet ansluta DATA 0 port toohello primära LAN (nätverk med åtkomst till Internet). hello ansluten andra data kan vara tooSAN/iSCSI-LAN (VLAN)-segmentet i hello nätverket, beroende på hello avsedd roll.
* Samma gränssnitt på varje domänkontrollant anslutna toohello samma nätverk tooensure tillgänglighet om det uppstår redundans domänkontrollant. Till exempel om du väljer tooconnect DATA 0 och DATA 3 för en hello domänkontrollanter måste tooconnect hello motsvarande DATA 0 och DATA 3 på hello andra styrenheten.

Kom ihåg för hög tillgänglighet och prestanda:

* Konfigurera ett par med nätverksgränssnittet för molnåtkomst (1 GbE) och en annan par för iSCSI (10 GbE rekommenderas) på varje domänkontrollant när det är möjligt.
* Ansluta nätverksgränssnitt från varje tootwo olika växlar tooensure tillgång till en domänkontrollant mot en switch-fel när det är möjligt. hello figur visar hello två 10 GbE nätverksgränssnitt, DATA 2 och DATA 3 från varje domänkontrollant anslutna tootwo olika växlar.

Mer information finns i toohello **nätverksgränssnitt** under hello [krav på hög tillgänglighet för din StorSimple-enhet](storsimple-system-requirements.md#high-availability-requirements-for-storsimple).

> [!NOTE]
> Om du använder SFP + mottagarna med din 10 GbE-nätverkskort, Använd hello tillhandahålls QSFP-SFP + nätverkskort. Mer information finns för[maskinvara som stöds för hello 10 GbE-nätverkskort på StorSimple-enheten](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).
> 
> 

### <a name="serial-port-cabling"></a>Serieport kablage
Utför följande steg toocable hello en serieport.

#### <a name="toocable-for-serial-connection"></a>toocable för seriell anslutning
1. Enheten har en seriell port på varje domänkontrollant som identifieras av en skiftnyckelikonen. Se toohello bild i hello [nätverkskablar](#network-cabling) avsnittet toolocate hello seriella portar på hello bakplan av enheten.
2. Identifiera hello aktiva styrenheten på din enhet bakplan. En blinkande blå Indikator anger hello styrenheten är aktiv.
3. Använda hello tillhandahålls seriella kablar (vid behov, hello USB-seriell konverterare för din bärbara dator) och ansluta din dator eller konsolen (med terminalemulering toohello enhet) toohello serieport av hello aktiva styrenhet.
4. Installera hello serial-USB-drivrutiner (levereras med hello enhet) på datorn.
5. Ställ in hello seriell anslutning enligt följande: Ange tooNone 115 200 baud, 8 databitar, 1 stoppa bitar, ingen paritet och flödeskontroll.
6. Kontrollera att anslutningen hello fungerar genom att trycka på RETUR på hello-konsolen. Menyn för seriekonsolen ska visas.

> [!NOTE]
> **Hantering av lights-Out**: se till att hello seriella anslutningar tooboth domänkontrollanter alltid är anslutna tooa seriekonsolen växel eller liknande utrustning när hello enheten har installerats i en fjärransluten datacenter eller i en lokal dator med begränsad åtkomst. Detta tillåter fjärrstyrning för out-of-band och stöd för åtgärder om det finns nätverksstörningar eller oväntade fel.
> 
> 

Enheten är nu kabelansluten till ström, nätverksåtkomst och seriell anslutning. hello nästa steg är tooconfigure hello programvara och distribuera din enhet.

## <a name="next-steps"></a>Nästa steg
Lär dig hur för[distribuera och konfigurera din lokala StorSimple-enhet](storsimple-deployment-walkthrough-u2.md).

