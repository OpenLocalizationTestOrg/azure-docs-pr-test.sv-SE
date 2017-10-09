---
title: "aaaReplace en domänkontrollant för StorSimple-enhet | Microsoft Docs"
description: "Förklarar hur tooremove och Ersätt en eller båda controller moduler på StorSimple-enheten."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: e25b52b7-60f5-47f3-bffc-6c157d57ab5d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/03/2017
ms.author: alkohli
ms.openlocfilehash: ebf5c5830120857f69909113e3a111f4dda30e57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replace-a-controller-module-on-your-storsimple-device"></a>Ersätta en domänkontrollant modul på din StorSimple-enhet
## <a name="overview"></a>Översikt
Den här självstudiekursen beskrivs hur tooremove och Ersätt en eller båda controller moduler i en StorSimple-enhet. Här beskrivs också hello underliggande logiken för hello enkla och dubbla domänkontrollant ersättning scenarier.

> [!NOTE]
> Tidigare tooperforming ersättning för en domänkontrollant rekommenderas alltid uppdatera din domänkontrollant firmware toohello senaste versionen.
> 
> tooprevent skada tooyour StorSimple-enhet, matar inte hello domänkontrollant förrän hello indikatorer visas som en av följande hello:
> 
> * Alla ljus är AVSTÄNGD.
> * Indikator 3 ![grön kryssikonen](./media/storsimple-controller-replacement/HCS_GreenCheckIcon.png), och ![rött kryss ikonen](./media/storsimple-controller-replacement/HCS_RedCrossIcon.png) är blinkande och Indikator 0 och Indikator 7 **på**.
> 
> 

hello följande tabell visar hello stöds controller ersättning scenarier.

| Ärende | Ersättning scenario | Proceduren |
|:--- |:--- |:--- |
| 1 |En domänkontrollant är i ett felaktigt tillstånd, hello andra domänkontrollanter är felfri och aktiva. |[Enkel domänkontrollant ersättning](#replace-a-single-controller), som beskriver hello [logiken bakom en enda domänkontrollant ersättning](#single-controller-replacement-logic), samt hello [ersättning steg](#single-controller-replacement-steps). |
| 2 |Båda hello domänkontrollanter har misslyckats och kräva ersättning. hello chassi, diskar och diskhölje är hälsosamma. |[Dubbla domänkontrollant ersättning](#replace-both-controllers), som beskriver hello [logiken bakom en dubbel domänkontrollant ersättning](#dual-controller-replacement-logic), samt hello [ersättning steg](#dual-controller-replacement-steps). |
| 3 |Domänkontrollanter från hello samma enhet eller från olika enheter växlas. hello chassi, diskar och diskenheter är hälsosamma. |Varningsmeddelande för matchning av datatyp som ska användas som plats visas. |
| 4 |En domänkontrollant saknas och hello andra domänkontrollant misslyckas. |[Dubbla domänkontrollant ersättning](#replace-both-controllers), som beskriver hello [logiken bakom en dubbel domänkontrollant ersättning](#dual-controller-replacement-logic), samt hello [ersättning steg](#dual-controller-replacement-steps). |
| 5 |En eller båda domänkontrollanter har misslyckats. Du kan inte komma åt hello enheten via seriekonsolen hello eller Windows PowerShell-fjärrkommunikation. |[Kontakta Microsoft Support](storsimple-contact-microsoft-support.md) manuell controller ersättning anvisningar. |
| 6 |hello domänkontrollanter har en annan version, vilket kan bero på följande:<ul><li>Domänkontrollanter har en annan programvaruversion.</li><li>Domänkontrollanter har en annan firmware-version.</li></ul> |Om hello controller programvaruversioner är olika, hello ersättning logik upptäcker att och uppdateringar hello programvaruversionen på hello ersättning domänkontrollant.<br><br>Om versioner av hello controller inbyggd är olika och hello gamla version på inbyggd programvara är **inte** automatiskt kan uppgraderas ett varningsmeddelande visas i hello klassiska Azure-portalen. Du ska söka efter uppdateringar och installera uppdateringar av inbyggd hello.</br></br>Om hello controller versioner av inbyggd är olika och hello gamla version på inbyggd programvara kan uppgraderas automatiskt hello controller ersättning logik identifiera detta och efter hello domänkontrollant startar hello inbyggd programvara uppdateras automatiskt. |

Du måste tooremove en domänkontrollant modul om den har misslyckats. En eller båda hello controller moduler kan misslyckas, vilket kan resultera i en enda domänkontrollant ersättare eller ett dual-styrenheten ersättning. Ersättning procedurer och hello logiken bakom dem, finns följande hello:

* [Ersätta en enskild domänkontrollant](#replace-a-single-controller)
* [Ersätt både domänkontrollanter](#replace-both-controllers)
* [Ta bort en domänkontrollant](#remove-a-controller)
* [Infoga en domänkontrollant](#insert-a-controller)
* [Identifiera hello aktiva styrenheten på enheten](#identify-the-active-controller-on-your-device)

> [!IMPORTANT]
> Innan du tar bort och ersätter en domänkontrollant kan granska hello säkerhetsinformation i [ersättning av StorSimple maskinvara komponenten](storsimple-hardware-component-replacement.md).
> 
> 

## <a name="replace-a-single-controller"></a>Ersätta en enda domänkontrollant
Du måste tooreplace en enda domänkontrollant när en hello två domänkontrollanter på hello Microsoft Azure StorSimple-enhet har misslyckats, är fel eller saknas. 

### <a name="single-controller-replacement-logic"></a>Styrenhet ersättning logik
Du bör ta bort hello misslyckade domänkontrollant i en enda domänkontrollant ersättning. (hello återstående domänkontrollant i hello enhet är hello aktiva styrenhet.) När du infogar hello ersättning controller utförs hello följande åtgärder:

1. hello ersättning av domänkontrollant startar omedelbart kommunicerar med hello StorSimple-enhet.
2. En ögonblicksbild av hello virtuell hårddisk (VHD) för hello aktiva styrenheten kopieras på hello ersättning domänkontrollant.
3. hello ögonblicksbild har ändrats så att när hello ersättning av domänkontrollant startar från den här virtuella Hårddisken, det tolkas som en vänteläge domänkontrollant.
4. När du är klar med ändringarna hello startas hello ersättning domänkontrollant som hello vänteläge domänkontrollant.
5. När båda hello domänkontrollanter kör är hello kluster online.

### <a name="single-controller-replacement-steps"></a>Styrenhet steg för ersättning
Slutför följande steg om en hello domänkontrollanter i din Microsoft Azure StorSimple-enhet misslyckas hello. (hello andra styrenheten måste vara aktiv och körs. Om båda domänkontrollanterna inte, eller fungera gå för[dual-styrenheten ersättning steg](#dual-controller-replacement-steps).)

> [!NOTE]
> Det kan ta 30 – 45 minuter för hello controller toorestart och återställa helt från hello styrenhet ersättning procedur. hello total tid för hela proceduren hello, inklusive kopplar hello kablar är cirka 2 timmar.
> 
> 

#### <a name="tooremove-a-single-failed-controller-module"></a>tooremove en misslyckad styrenhet-modul
1. Gå toohello StorSimple Manager-tjänsten i hello klassiska Azure-portalen klickar du på hello **enheter** fliken och klicka sedan på hello namnet på hello-enhet som du vill toomonitor.
2. Gå för**Underhåll > maskinvarustatus**. hello status för styrenhet 0 eller 1 för domänkontrollanten ska vara röd, vilket tyder på ett fel.
   
   > [!NOTE]
   > hello misslyckade domänkontrollant i en enda domänkontrollant ersättning är alltid en standby-styrenhet.
   > 
   > 
3. Använd bild 1 och hello efter tabellen toolocate hello felaktig styrenhet modulen.  
   
    ![Bakplan av primära Höljesmoduler](./media/storsimple-controller-replacement/IC740994.png)
   
    **Bild 1** tillbaka på StorSimple-enhet
   
   | Etikett | Beskrivning |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Styrenhet 0 |
   | 4 |Kontrollant 1 |
4. Ta bort alla hello anslutna nätverkskablarna från hello data hamnar på misslyckade hello-styrenhet. Om du använder en 8600 modell kan du också ta bort hello SAS-kablar som ansluter hello controller toohello EBOD domänkontrollant.
5. Gör så hello i [ta bort en domänkontrollant](#remove-a-controller) tooremove hello misslyckades domänkontrollant. 
6. Installera hello factory ersättning i hello samma fack från vilka hello misslyckade domänkontrollant har tagits bort. Detta utlöser hello styrenhet ersättning logik. Mer information finns i [enkel domänkontrollant ersättning logik](#single-controller-replacement-logic).
7. Medan hello styrenhet ersättning logik fortskrider i hello bakgrund, ansluta hello-kablar. Ta försiktighet tooconnect alla hello-kablar hello exakt samma sätt som de var anslutna innan hello ersättning.
8. Hello domänkontrollanten startar om och kontrollera hello **status domänkontrollanten** och hello **kluster status** i hello Azure klassiska portal tooverify som hello domänkontrollanten är den bakre tooa felfritt tillstånd och är i vänteläge .

> [!NOTE]
> Om du övervakar hello enheten via seriekonsolen hello, kan du se flera omstarter när hello domänkontrollant återställs från hello ersättning procedur. När hello menyn för seriekonsolen visas vet att hello ersättning har slutförts. Om hello menyn inte visas inom två timmar för att starta hello controller ersättning, [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md).
>
> Starta uppdatering 4, du kan också använda hello cmdlet `Get-HCSControllerReplacementStatus` i hello Windows PowerShell-gränssnittet för hello toomonitor hello Enhetsstatus processens hello controller ersättning.
> 

## <a name="replace-both-controllers"></a>Ersätt både domänkontrollanter
När både domänkontrollanter på hello Microsoft Azure StorSimple-enhet har misslyckats, är felaktiga eller saknas, måste tooreplace både domänkontrollanter. 

### <a name="dual-controller-replacement-logic"></a>Dubbla domänkontrollant ersättning logik
I en dubbel domänkontrollant ersättning du först ta bort både misslyckade domänkontrollanter och infoga ersättningar. När hello två ersättning domänkontrollanter infogas utförs hello följande åtgärder:

1. hello ersättning domänkontrollanten i uttag 0 kontrollerar hello följande:
   
   1. Använder den aktuella versioner av hello inbyggd och annan programvara?
   2. Det är en del av hello kluster?
   3. Är hello peer-domänkontrollant som körs och är det klustrade?
      
      Om ingen av dessa villkor är uppfyllda, hello domänkontrollant efter hello senaste daglig säkerhetskopiering (finns i hello **nonDOMstorage** på enheten S). hello controller kopierar hello senaste ögonblicksbild av hello VHD från hello säkerhetskopiering.
2. hello domänkontrollanten i uttag 0 använder hello ögonblicksbild tooimage sig själv.
3. Under tiden hello domänkontrollanten i uttag 1 väntar styrenhet 0 toocomplete hello avbildning och start.
4. När styrenhet 0 startas, identifierar kontrollant 1 hello-kluster skapas av styrenhet 0, vilket utlöser hello styrenhet ersättning logik. Mer information finns i [enkel domänkontrollant ersättning logik](#single-controller-replacement-logic).
5. Därefter kan både domänkontrollanter ska köras och hello kluster ska anslutas.

> [!IMPORTANT]
> Efter en dubbel domänkontrollant ersättning när hello StorSimple-enhet har konfigurerats, är det viktigt att du utföra en manuell säkerhetskopiering av hello enhet. Dagliga säkerhetskopior för konfiguration av enheter aktiveras inte förrän efter 24 timmar har löpt ut. Arbeta med [Microsoft-supporten](storsimple-contact-microsoft-support.md) toomake en manuell säkerhetskopiering av din enhet.
> 
> 

### <a name="dual-controller-replacement-steps"></a>Dubbla domänkontrollant ersättning steg
Det här arbetsflödet krävs när båda hello domänkontrollanter i din Microsoft Azure StorSimple-enhet har misslyckats. Detta kan inträffa i ett datacenter där hello kylning system slutar fungera och därför båda hello domänkontrollanter misslyckas inom en kort tidsperiod. Beroende på om hello StorSimple-enhet är avstängd eller, om du använder en 8600 eller en 8100-modellen, en annan uppsättning steg krävs.

> [!IMPORTANT]
> Det kan ta upp till 45 minuter too1 timme för hello controller toorestart och helt återställa från en dubbel domänkontrollant ersättning procedur. hello total tid för hela proceduren hello, inklusive kopplar hello kablar är ungefär 2,5 timmar.
> 
> 

#### <a name="tooreplace-both-controller-modules"></a>tooreplace både styrenhet-moduler
1. Om hello enheten stängs av, hoppa över detta steg och fortsätta toohello nästa steg. Inaktivera hello enheten om hello enheten är påslagen.
   
   1. Om du använder en modell för 8600 först tar bort hello primära höljet och stäng sedan av hello EBOD hölje.
   2. Vänta tills hello enheten har avslutats helt. Alla hello led i hello baksidan hello enhet ska vara inaktiverat.
2. Ta bort alla hello nätverkskablarna som är anslutna toohello dataportar. Om du använder en 8600 modell kan du också ta bort hello SAS-kablar som ansluter hello primära höljet toohello EBOD hölje.
3. Ta bort både domänkontrollanter från hello StorSimple-enhet. Mer information finns i [ta bort en domänkontrollant](#remove-a-controller).
4. Infoga hello factory ersättning för styrenhet 0 först och infoga Controller 1. Mer information finns i [infoga en domänkontrollant](#insert-a-controller). Detta utlöser hello dubbla domänkontrollant ersättning logik. Mer information finns i [dual-styrenheten ersättning logik](#dual-controller-replacement-logic).
5. Medan hello controller ersättning logik fortskrider i hello bakgrund, ansluta hello-kablar. Ta försiktighet tooconnect alla hello-kablar hello exakt samma sätt som de var anslutna innan hello ersättning. Se hello detaljerade instruktioner för din modell i hello kabel din enhet avsnitt i [installerar StorSimple 8100-enhet](storsimple-8100-hardware-installation.md) eller [installera din StorSimple-8600-enhet](storsimple-8600-hardware-installation.md).
6. Aktivera hello StorSimple-enhet. Om du använder en 8600-modellen:
   
   1. Kontrollera att hello EBOD hölje aktiveras först.
   2. Vänta tills hello EBOD hölje körs.
   3. Aktivera hello primära enhet.
   4. När hello första domänkontrollant startar och är i felfritt tillstånd, köra hello system.
      
      > [!NOTE]
      > Om du övervakar hello enheten via seriekonsolen hello, kan du se flera omstarter när hello domänkontrollant återställs från hello ersättning procedur. När hello-menyn för seriekonsolen visas sedan vet du att hello ersättning har slutförts. Om hello menyn inte visas inom 2,5 timmar för att starta hello controller ersättning, [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md).
      > 
      > 

## <a name="remove-a-controller"></a>Ta bort en domänkontrollant
Använd följande procedur tooremove en felaktig Styrenhetsmodul från din StorSimple-enhet hello.

> [!NOTE]
> hello följande illustrationer är för styrenhet 0. Kontrollant 1 skulle dessa vara omvänd.
> 
> 

#### <a name="tooremove-a-controller-module"></a>tooremove en domänkontrollant-modul
1. Tag hello modulen spärren mellan USB och pekfingret.
2. Försiktigt klämma din USB och pekfingret tillsammans toorelease hello controller spärren.
   
    ![Släppa controller spärren](./media/storsimple-controller-replacement/IC741047.png)
   
    **Bild 2** frisläppning controller spärren
3. Använd hello spärren som referens tooslide hello domänkontrollant utanför hello chassi.
   
    ![Glidande controller out-of-chassi](./media/storsimple-controller-replacement/IC741048.png)
   
    **Bild 3** glidande hello domänkontrollant utanför hello chassi

## <a name="insert-a-controller"></a>Infoga en domänkontrollant
Använd hello följa proceduren tooinstall en modul levererats från fabriken domänkontrollant när du tar bort en felaktig modul från din StorSimple-enhet.

#### <a name="tooinstall-a-controller-module"></a>tooinstall en domänkontrollant-modul
1. Kontrollera toosee om det inte finns någon skada toohello gränssnittet kopplingar. Installera inte hello modulen om någon av hello connector PIN-koder skadas eller böjas.
2. Skjut hello controller modul i hello chassi medan hello spärren släpps fullständigt. 
   
    ![Glidande domänkontrollant i chassi](./media/storsimple-controller-replacement/IC741053.png)
   
    **Bild 4** glidande domänkontrollant i hello chassi
3. Börja med hello controller modulen infogas, stänger hello spärren när fortsätter toopush hello controller modulen till hello chassi. hello spärren kommer engagera tooguide hello domänkontrollant på plats.
   
    ![Stänger controller spärren](./media/storsimple-controller-replacement/IC741054.png)
   
    **Bild 5** stänger hello controller spärren
4. Du är klar när hello spärren fästs på plats. Hej **OK** Indikator bör nu vara på.  
   
   > [!NOTE]
   > Det kan ta upp too5 minuter för hello styrenhet och hello Indikator tooactivate.
   > 
   > 
5. tooverify hello ersättning har slutförts i hello Azure klassiska portal, gå för**enheter** > **Underhåll** > **maskinvarustatus**, och kontrollera att både styrenhet 0 och kontrollant 1 är felfri (statusen är grön).

## <a name="identify-hello-active-controller-on-your-device"></a>Identifiera hello aktiva styrenheten på enheten
Det finns många situationer, till exempel första gången enheten registreringen eller domänkontrollant ersättning, som kräver att du toolocate hello aktiva styrenheten på StorSimple-enhet. hello aktiva styrenheten bearbetar alla hello inbyggd programvara och nätverk diskåtgärder. Du kan använda någon av följande metoder tooidentify hello aktiva styrenheten hello:

* [Använd hello Azure klassiska portal tooidentify hello aktiva styrenhet](#use-the-azure-classic-portal-to-identify-the-active-controller)
* [Använda Windows PowerShell för StorSimple tooidentify hello aktiva styrenhet](#use-windows-powershell-for-storsimple-to-identify-the-active-controller)
* [Kontrollera hello fysisk enhet tooidentify hello aktiva styrenhet](#check-the-physical-device-to-identify-the-active-controller)

Var och en av de här procedurerna beskrivs nedan.

### <a name="use-hello-azure-classic-portal-tooidentify-hello-active-controller"></a>Använd hello Azure klassiska portal tooidentify hello aktiva styrenhet
I Hej klassiska Azure-portalen, navigera för**enheter** > **Underhåll**, och rulla toohello **domänkontrollanter** avsnitt. Här kan du kontrollera vilka domänkontrollanten är aktiv.

![Identifiera aktiva styrenheten i klassiska Azure-portalen](./media/storsimple-controller-replacement/IC752072.png)

**Bild 6** Azure klassiska portal som visar hello aktiva styrenhet

### <a name="use-windows-powershell-for-storsimple-tooidentify-hello-active-controller"></a>Använda Windows PowerShell för StorSimple tooidentify hello aktiva styrenhet
När du öppnar din enhet via seriekonsolen hello visas Banderollmeddelandet. Hej Banderollmeddelandet innehåller grundläggande enhetsinformation, till exempel hello modellen, namn, version för installerad programvara och status för hello-domänkontrollant som du ansluter till. hello följande bild visar ett exempel på Banderollmeddelandet:

![Seriell Banderollmeddelandet](./media/storsimple-controller-replacement/IC741098.png)

**Bild 7** banderoll meddelande visar styrenhet 0 som aktiv

Du kan använda hello banderoll meddelandet toodetermine om hello domänkontrollant som du är ansluten toois aktiva eller passiva.

### <a name="check-hello-physical-device-tooidentify-hello-active-controller"></a>Kontrollera hello fysisk enhet tooidentify hello aktiva styrenhet
tooidentify hello aktiva styrenheten på enheten, leta upp hello blå Indikator ovan hello DATA 5 port på hello baksidan hello primära enhet.

Om denna Indikator blinkar hello domänkontrollanten är aktiv och hello andra domänkontrollanter är i vänteläge. Använd hello följande diagram och tabell som hjälp.

![Enheten primära höljet bakplan med dataportar](./media/storsimple-controller-replacement/IC741055.png)

**Figur 8** baksidan primär enhet med dataportar och övervakning indikatorer

| Etikett | Beskrivning |
|:--- |:--- |
| 1-6 |DATA 0 – 5 nätverksportar |
| 7 |Blå Indikator |

## <a name="next-steps"></a>Nästa steg
Lär dig mer om [ersättning av StorSimple maskinvara komponenten](storsimple-hardware-component-replacement.md).

