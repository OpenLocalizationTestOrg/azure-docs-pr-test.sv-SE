---
title: aaaInstall Microsoft Azure StorSimple 8600-enhet | Microsoft Docs
description: Beskriver hur toounpack, rackmontera och kabelanslut din 8600 StorSimple-enhet innan du distribuerar och konfigurerar hello programvara.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 3d82ba5f-3e34-40dc-9c33-50f952bc6be8
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 10/24/2016
ms.author: alkohli
ms.openlocfilehash: 0fc0ddf076725fededdde33a260b950b72edc8db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="unpack-rack-mount-and-cable-your-storsimple-8600-device"></a>Packa upp, rackmontera, och kabelanslut din 8600 StorSimple-enhet
## <a name="overview"></a>Översikt
Din Microsoft Azure StorSimple 8600-enhet är en enhet med dubbla höljet och består av en primär och en EBOD bilaga. Den här självstudiekursen beskrivs hur toounpack, rackmonterad och kabel hello StorSimple 8600-enhet maskinvara innan du konfigurerar hello StorSimple programvara.

## <a name="unpack-your-storsimple-8600-device"></a>Packa upp din StorSimple-8600-enhet
hello följande steg är tom, detaljerade anvisningar om hur toounpack lagringsenheten StorSimple 8600. Den här enheten levereras i två rutor, en för hello primära höljet och en annan för hello EBOD hölje. Dessa två rutor placeras sedan i en enda ruta.

### <a name="prepare-toounpack-your-device"></a>Förbereda toounpack enheten
Granska hello följande information innan du packar upp din enhet.

![Varningsikon](./media/storsimple-safety/IC740879.png)![tunga vikt ikonen](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **varning!**

1. Kontrollera att du har två personer tillgängliga toomanage hello vikten av hello enheten om hanterar manuellt. Ett helt konfigurerade hölje kan väga in too32 kg (70 pund).
2. Hello placeras på en plan, nivå-yta.

Därefter Slutför följande steg toounpack hello enheten.

#### <a name="toounpack-your-device"></a>toounpack din enhet
1. Granska hello rutan och hello paketering skum för crushes delar, vattenstämplar skador eller andra uppenbara skador. Öppna inte hello rutan om hello rutan eller paketering kraftigt är skadad. Kontrollera [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md) toohelp du bedöma om hello enheten är i gott skick.
2. Öppna hello yttre ruta och ta sedan ut hello två rutor motsvarande tooprimary och EBOD bilagor. Du kan nu packa upp hello primära och EBOD bilagor. hello följande bild visar hello packat upp en hello-höljen.
   
    ![Packa upp lagringsenheten](./media/storsimple-8600-hardware-installation/HCSUnpackyour4Udevice.png)
   
    **Packa upp visning av lagringsenheten**
   
   | Etikett | Beskrivning |
   | --- | --- |
   |   1 |Packkorg rutan |
   |   2 |SAS-kablar (i tillbehör och kablar fack) |
   |   3 |Nedre skum |
   |   4 |Enhet |
   |   5 |Övre skum |
   |   6 |Tillbehör rutan |
3. Kontrollera att du har när hello två rutor:
   
   * 1 primära hölje (hello primära höljet och EBOD hölje är i två separata rutorna)
   * 1 EBOD hölje
   * 4 strömkablar, 2 i respektive ruta
   * 2 SAS-kablar (tooconnect hello primära höljet tooEBOD hölje)
   * 1 övergång Ethernet-kabel
   * 2 seriekonsolen kablar
   * 1 seriell USB konverterare för seriell åtkomst
   * 4 QSFP-till-SFP + kort för användning med 10 GbE-nätverkskort
   * 2 rack montera kits (4 sida spår med montera maskinvara, 2 för hello primära höljet och EBOD hölje), 1 i respektive ruta
   * Komma igång-dokumentationen
     
     Om du inte fick någon hello objekten i listan ovan, [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md).  

hello nästa steg är toorack montera din enhet.

## <a name="rack-mount-your-storsimple-8600-device"></a>Rackmonterade din StorSimple-8600-enhet
Följ hello nästa steg tooinstall lagringsenheten StorSimple 8600 i en standard 19-tums rack med främre och bakre meddelandena. Den här enheten levereras med två höljen: en primär enhet och en EBOD bilaga. Båda dessa måste toobe rackmonterade.

hello installation består av flera steg, som beskrivs i följande procedurer hello.

> [!IMPORTANT]
> StorSimple-enheter måste vara rackmonterade för att fungera korrekt.
> 
> 

### <a name="site-preparation"></a>Förberedelse
hello-höljen måste installeras i ett standard 19-tums rack som har både främre och bakre meddelandena. Använd hello följa proceduren tooprepare för Rackinstallation.

#### <a name="tooprepare-hello-site-for-rack-installation"></a>tooprepare hello plats för Rackinstallation
1. Kontrollera att hello primära och EBOD höljen är vilande på ett säkert sätt på en flat stabilt och nivå arbetsyta (eller liknande).
2. Kontrollera hello platsen där du ska tooset upp har standard AC power från en oberoende källa eller ett rack Power Distribution enhet (PDU) med en avbrottsfri elkälla (UPS).
3. Kontrollera att en 4U (2 X 2U) platsen är tillgänglig på hello rack som du avser att toomount hello-höljen.

![Varningsikon](./media/storsimple-safety/IC740879.png)![tunga vikt ikonen](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **varning!**

 Kontrollera att du har två personer tillgängliga toomanage hello vikt om du hanterar hello Enhetsinställningar manuellt. Ett helt konfigurerade hölje kan väga in too32 kg (70 pund).

### <a name="rack-prerequisites"></a>Rack krav
hello-höljen är utformade för installation i en standard 19-tums rack CAB med:

* Minsta djup 27.84 tum från rack efter toopost
* Högsta vikt för 32 kg för hello-enhet
* Maximal som ligger på 5 Pascal (0,5 mätare)

### <a name="rack-mounting-rail-kit"></a>Rack montering tåg kit
En uppsättning montera spår anges för användning med hello 19-tums rack kabinettfilen. hello spår har testat toohandle hello maximala höljet vikt. Dessa spår kommer också att tillåta installation av flera höljen utan förlust av utrymme i hello rack. Installera hello EBOD hölje först.

#### <a name="tooinstall-hello-ebod-enclosure-on-hello-rails"></a>tooinstall hello EBOD hölje hello spår
1. Utför det här steget endast om inre spår inte har installerats på enheten. Normalt installeras hello inre spår i hello fabriken. Om spår inte har installerats, installerar du hello tåg vänster och höger tåg bilder toohello sidor hello höljet chassin. De fäster med sex mått skruvar på varje sida. toohelp med orientering hello tåg bilder markeras **LH – fram** och **RH – fram**, och hello slutet som fästs mot hello bakom hello höljet har en avsmalnande end.
   
    ![Koppling tåg bilder tooenclosure chassi](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)
   
    **Koppling tåg bilder toohello sidor av hello hölje**
   
   | Etikett | Beskrivning |
   | --- | --- |
   |  1 |M 3 x 4 knappen head skruvar |
   |  2 |Chassi bilder |
2. Koppla hello vänstra tåg och höger sammansättningar toohello rack CAB lodräta medlemmar. hello hakparenteser markeras **LH**, **RH**, och **den här sidan uppåt** tooguide du rätt orientering.
3. Leta upp hello tåg PIN-koder på hello främre och bakre hello tåg sammansättningen. Utöka hello tåg toofit mellan hello rack inlägg och infoga hello PIN-koder i hello främre och bakre rack post lodräta medlem hål. Var noga med att hello tåg sammansättningen är nivå.
4. Skydda hello tåg sammansättningen toohello rack lodräta medlemmar genom att två av hello mått skruvar tillhandahålls. Använd en skruv på hello främre och en på hello bakre.
5. Upprepa dessa steg för hello andra tåg-sammansättningen.
   
     ![Koppling tåg bilder toorack CAB](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)
   
    **Bifoga tåg sammansättningar toohello rack**
   
   | Etikett | Beskrivning |
   | --- | --- |
   |   1 |Minskning skruv |
   |   2 |Ruta hål främre rack post skruv |
   |   3 |Vänster främre tåg plats PIN-koder |
   |   4 |Minskning skruv |
   |   5 |Vänster bakre tåg plats PIN-koder |

### <a name="mounting-hello-ebod-enclosure-in-hello-rack"></a>Montera hello EBOD höljet i hello rack
Använd hello rack spår som precis har installerats och utför följande steg toomount hello EBOD höljet i hello rack hello.

#### <a name="toomount-hello-ebod-enclosure"></a>toomount hello EBOD hölje
1. Med en assistent lyfter hello höljet och justera med hello rack spår.
2. Infoga noggrant hello höljet i hello spår och sedan push-installera den helt i hello rack cab.
   
    ![Lägga till enheten i hello rack](./media/storsimple-8600-hardware-installation/HCSInsertingDeviceintheRack.png)
   
    **Montera hello höljet i hello rack**
3. Ta bort hello vänster och höger främre flänsad caps genom att dra hello caps ledigt. hello flänsad caps Fäst bara till hello flänsar.
4. Skydda hello höljet i hello rack genom att installera en angivna stjärnskruvmejsel head skruv via varje flänsad vänster och höger.
5. Installera hello flänsad caps genom att trycka ned dem till rätt plats och fästa dem till rätt plats.
   
     ![Installera flänsad caps](./media/storsimple-8600-hardware-installation/HCSInstallingFlangeCaps.png)
   
    **Installera hello flänsad caps**
   
   | Etikett | Beskrivning |
   | --- | --- |
   |   1 |Höljet fästanordning skruv |

### <a name="mounting-hello-primary-enclosure-in-hello-rack"></a>Montera hello primära höljet i hello rack
När du är klar med att montera hello EBOD hölje måste toomount hello primära höljet följande hello samma steg.

> [!NOTE]
> * Det är möjligt toohave några tomma platser i hello rack mellan hello primära höljet och hello EBOD hölje.
> * Använd hello tillhandahålls 2m SAS kabel tooconnect hello primära höljet toohello EBOD hölje.
> * Det finns inga begränsningar för hello relativa placeringen av hello head enhet toohello EBOD enhet. Därför hello primära höljet kan placeras i hello övre fack och hello EBOD hölje nedan, och vice versa.
> 
> 

hello nästa steg är toocable enheten till ström, nätverk och serieåtkomst.

## <a name="cable-your-storsimple-8600-device"></a>Kabelanslut din 8600 StorSimple-enhet
hello följande procedurer beskriver hur toocable din StorSimple-8600-enhet till ström, nätverk och seriella anslutningar.

### <a name="prerequisites"></a>Krav
Innan du börjar toocable enheten behöver du:

* Din primära höljet och hello EBOD hölje helt Uppackad
* 4 strömkablar (2 varje för hello primära och hello EBOD höljet) som medföljde din enhet
* 2 SAS-kablar medföljer hello enheten tooconnect hello EBOD hölje toohello primära enhet
* Åtkomst too2 Kraftfördelningsenheter (PDU) (rekommenderas)
* Nätverkskablarna
* Angivna seriella kablar
* Seriell USB-konverteraren med hello lämplig drivrutin installeras på din dator (om det behövs)
* Angivna 4 QSFP-till-SFP + kort för användning med 10 GbE-nätverkskort
* [Maskinvara som stöds för hello 10 GbE-nätverkskort på din StorSimple-enhet](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="sas-and-power-cabling"></a>SAS- och kablar
Enheten har både en primär enhet och en EBOD bilaga. Detta kräver hello enheter toobe kabelanslutna tillsammans för seriellt ansluten SCSI (SAS)-anslutning och ström.

När du konfigurerar enheten för hello första gången, gör hello SAS-kablar först och sedan utför hello anvisningar för kablar.

[!INCLUDE [storsimple-cable-8600-for-SAS](../../includes/storsimple-sas-cable-8600.md)]

[!INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

### <a name="network-cabling"></a>Nätverkskablar
Enheten är i ett aktivt vänteläge konfiguration: samtidigt, en modul som domänkontrollanten är aktiv och bearbetning av alla disk- och åtgärder vid hello modulen andra domänkontrollanter är i vänteläge. Om en domänkontrollant inträffar, aktiverar hello vänteläge controller omedelbart och fortsätter alla åtgärder för hello disk och nätverk.

toosupport den här redundant controller redundansen måste toocable enheten nätverk enligt hello följande steg.

#### <a name="toocable-for-network-connection"></a>toocable för nätverksanslutning
1. Enheten har sex nätverksgränssnitt på varje domänkontrollant: fyra 1 Gbit/s och två 10 Gbit/s Ethernet-portarna. Läs toohello följande illustration tooidentify hello dataportar på hello bakplan av enheten.
   
     ![Bakplan av 8600-enhet](./media/storsimple-8600-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)
   
    **Tillbaka på enheten visar hello dataportar**
   
   | Etikett | Beskrivning |
   | --- | --- |
   |   0,1,4,5 |1 GbE-nätverkskort |
   |   2,3 |10 GbE-nätverkskort |
   |   6 |Seriella portar |
2. Se följande diagram för nätverkskablar hello. (hello minsta nätverkskonfigurationen visas med blå heldragen linje. För hög tillgänglighet och prestanda visas ytterligare konfiguration krävs med streckade linjer.)

![Kabelansluta den 4U för nätverket](./media/storsimple-8600-hardware-installation/HCSCableYour4UDeviceforNetwork.png)

**Nätverket kablar för din enhet**

| Etikett | Beskrivning |
| --- | --- |
| A |LAN med Internetåtkomst |
| B |Styrenhet 0 |
| C |PCM 0 |
| D |Kontrollant 1 |
| E |PCM 1 |
| F |EBOD styrenhet 0 |
| G |EBOD kontrollant 1 |
| H, JAG |Värdar (till exempel filservrar) |
| 0-5 |Nätverksgränssnitt |
| 6 |Primär enhet |
| 7 |EBOD hölje |

När kablar hello enheten kräver hello minimikraven för konfiguration:

* Minst två nätverkskort som är anslutna på varje domänkontrollant för att komma åt molnet och en för iSCSI. hello DATA 0 port aktiveras och konfigureras via hello seriekonsolen av hello enhet automatiskt. Förutom DATA 0 måste en annan dataport också toobe konfigureras via hello klassiska Azure-portalen. I det här fallet ansluta DATA 0 port toohello primära LAN (nätverk med åtkomst till Internet). hello ansluten andra data kan vara tooSAN/iSCSI-LAN (VLAN)-segmentet i hello nätverket, beroende på hello avsedd roll.
* Samma gränssnitt på varje domänkontrollant anslutna toohello samma nätverk tooensure tillgänglighet om det uppstår redundans domänkontrollant. Till exempel om du väljer tooconnect DATA 0 och DATA 3 för en hello domänkontrollanter måste tooconnect hello motsvarande DATA 0 och DATA 3 på hello andra styrenheten.

Kom ihåg för hög tillgänglighet och prestanda:

* Konfigurera ett par med nätverksgränssnittet för molnåtkomst (1 GbE) och en annan par för iSCSI (10 GbE rekommenderas) på varje domänkontrollant när det är möjligt.
* Ansluta nätverksgränssnitt från varje tootwo olika växlar tooensure tillgång till en domänkontrollant mot en switch-fel när det är möjligt. hello figur visar hello två 10 GbE nätverksgränssnitt, DATA 2 och DATA 3 från varje domänkontrollant anslutna tootwo olika växlar. Mer information finns i toohello **nätverksgränssnitt** under hello [krav på hög tillgänglighet för din StorSimple-enhet](storsimple-system-requirements.md#high-availability-requirements-for-storsimple).

> [!NOTE]
> Om du använder SFP + mottagarna med din 10 GbE-nätverkskort, Använd hello tillhandahålls QSFP-SFP + nätverkskort. Mer information finns för[maskinvara som stöds för hello 10 GbE-nätverkskort på StorSimple-enheten](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).
> 
> 

### <a name="serial-port-cabling"></a>Serieport kablage
Utför följande steg toocable hello en serieport.

#### <a name="toocable-for-serial-connection"></a>toocable för seriell anslutning
1. Enheten har en seriell port på varje domänkontrollant som identifieras av en skiftnyckelikonen. toolocate hello seriella portar, se toohello bild som visar hello dataportar på hello baksidan av din enhet.
2. Identifiera hello aktiva styrenheten på din enhet bakplan. En blinkande blå Indikator anger hello styrenheten är aktiv.
3. Använda hello tillhandahålls seriekabel (vid behov, hello USB-seriell konverterare för din bärbara dator) och ansluta din dator eller konsolen (med terminalemulering toohello enhet) toohello serieport av hello aktiva styrenhet.
4. Installera hello serial-USB-drivrutiner (levereras med hello enhet) på datorn.
5. Ställ in hello seriell anslutning enligt följande:
   
   * 115 200 bit/s
   * 8 databitar
   * 1 stop-bitars
   * Ingen paritet
   * Ange flödeskontroll för**ingen**
6. Kontrollera att anslutningen hello fungerar genom att trycka på RETUR på hello-konsolen. Menyn för seriekonsolen ska visas.

> [!NOTE]
> **Lights-Out-hantering:** när hello enheter installeras i en fjärransluten datacenter eller i en lokal dator med begränsad åtkomst, kontrollera att hello seriella anslutningar tooboth domänkontrollanter alltid är anslutna tooa seriekonsolen växel eller liknande utrustning. Detta tillåter fjärrstyrning för out-of-band och stöd för åtgärder vid avbrott i nätverket eller oväntade fel.
> 
> 

Du har slutfört din enhet för ström, nätverksåtkomst, kablar och seriella connection.hello nästa steg är tooconfigure hello programvara på din enhet.

## <a name="next-steps"></a>Nästa steg
Du är nu redo för[distribuera och konfigurera din lokala StorSimple-enhet](storsimple-deployment-walkthrough-u2.md).

