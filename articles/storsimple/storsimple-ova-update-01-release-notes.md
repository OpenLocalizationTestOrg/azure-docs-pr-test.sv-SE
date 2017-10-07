---
title: aaaStorSimple virtuella matris uppdateringar viktig information | Microsoft Docs
description: "Beskriver viktiga öppna problem och lösningar för hello virtuella StorSimple-matris Kör uppdatering 0,2 och 0,1."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 3993864d-2ddd-4302-a2f1-8d737fba6eab
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/16/2016
ms.author: alkohli
ms.openlocfilehash: dfd38890feeb667c95134f2adbb35ce2df165620
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-virtual-array-update-02-and-01-release-notes"></a>StorSimple virtuell matris uppdatera 0,2 och 0,1 viktig information
## <a name="overview"></a>Översikt
hello följande versionsinformation identifiera hello kritiska öppna problem och hello löst problem för Microsoft Azure StorSimple virtuell matris uppdateringar. (Microsoft Azure StorSimple virtuell matrisen är även kallat hello StorSimple lokala virtuella enheten eller hello virtuella StorSimple-enheten.) 

hello viktig information uppdateras kontinuerligt, och allteftersom allvarliga problem som kräver en lösning upptäcks, läggs de. Granska noggrant hello informationen i hello viktig information innan du distribuerar din virtuella StorSimple-enhet.

Uppdatera 0,2 motsvarar toohello programvaruversion **10.0.10280.0**; Uppdatering 0.1 är version **10.0.10279.0**. hello styckena nedan listas hello ändringar för varje uppdatering. 

> [!NOTE]
> Uppdateringar kan störande och startar om enheten. Om i/o som pågår, orsakar hello enheten driftstopp.
> 
> 

## <a name="issues-fixed-in-hello-update-02"></a>Problem som åtgärdas i hello Update 0,2
Uppdatera 0,2 innehåller alla ändringar från uppdatering 0.1 i tillägg toohello korrigeringen som beskrivs i följande tabell hello:

| Funktion | Problem |
| --- | --- |
| Uppdateringar |I hello senaste versionen identifieras uppdateringar inte automatiskt i hello klassiska Azure-portalen så att du haft toouse hello lokala Webbgränssnittet tooinstall uppdateringar. Det här problemet löses i den här versionen. När du har installerat uppdateringen 0,2 kan du installera framtida uppdateringar med hjälp av hello klassiska Azure-portalen. |

## <a name="whats-new-in-hello-update-01"></a>Vad är nytt i hello uppdatering 0.1
Uppdatering 0.1 innehåller hello följande felkorrigeringar och förbättringar. 

* **Förbättrad flexibilitet för molnet avbrott**: den här versionen har flera felkorrigeringar runt katastrofåterställning, säkerhetskopiering, återställning och skiktning i ett moln anslutningen avbrott hello-händelse. 
* **Bättre återställningsprestanda**: den här versionen har felkorrigeringar har avsevärt minska hello slutförandetid hello återställningsjobb.
* **Automatisk optimering för frigöring av utrymme**: när data tas bort på tunt allokerade volymer hello oanvända lagring Adressblock måste toobe frigöras. Den här versionen har förbättrad hello utrymme frigöring processen från hello moln, vilket resulterar i hello oanvända utrymme blir tillgängliga snabbare som jämfört med toohello tidigare versioner.
* **Den nya virtuella diskbilder**: nya VHD och VHDX VMDK är nu tillgängliga via hello klassiska Azure-portalen. Du kan hämta dessa avbildningar tooprovision nya uppdatering 0.1-enheter.
* **Förbättra hello riktighet Jobbstatus i hello portal**: I hello tidigare version av programvaran, jobbstatus rapportering i hello portal inte detaljerade. Det här problemet är löst i den här versionen.
* **Domän koppling upplevelse**: felkorrigeringar relaterade toodomain-anslutning och namnbyte av hello enhet.

## <a name="issues-fixed-in-hello-update-01"></a>Problem som åtgärdas i hello uppdatering 0.1
hello innehåller följande tabell en översikt över problem som åtgärdas i den här versionen.

| Nej. | Funktion | Problem |
| --- | --- | --- |
| 1 |VMDK |I vissa versioner av VMware påträffades hello OS-disk som sparse orsakar aviseringar och störa normal drift. Detta åtgärdades i den här versionen. |
| 2 |iSCSI-server |I hello senaste var hello användaren nödvändiga toospecify en gateway för varje aktiverat nätverksgränssnittet för din virtuella StorSimple-enhet. Denna funktion har ändrats i den här versionen så att hello användaren har tooconfigure minst en gateway för alla hello aktiverade nätverksgränssnitt. |
| 3 |Support-paket |I tidigare version av programmet hello, stöd för paketet collection misslyckades när hello paketet storlekar var större än 1 GB. Det här problemet löses i den här versionen. |
| 4 |Molnåtkomst |I hello senaste versionen om hello virtuella StorSimple-matrisen hade inte ansluten till nätverket och startades om skulle hello lokala Användargränssnittet ha problem med nätverksanslutningen. Det här problemet åtgärdas i den här versionen. |
| 5 |Övervakning av diagram |Hello molnet kapacitet användning diagram visas i hello tidigare version, efter en växling på enheten, felaktiga värden i hello klassiska Azure-portalen. Detta är fast hello aktuella versionen. |

## <a name="known-issues-in-hello-update-01"></a>Kända problem i hello uppdatering 0.1
hello följande tabell innehåller en översikt över kända problem för hello virtuella StorSimple-matris och omfattar hello problem versionen anges från hello tidigare versioner. **hello problem versionen som anges i den här versionen är markerade med en asterisk. Nästan alla hello problem i den här listan har överförts från hello GA-versionen av virtuella StorSimple-matris.**

| Nej. | Funktion | Problem | Lösning/kommentarer |
| --- | --- | --- | --- |
| **1.** |Uppdateringar |hello virtuella enheter skapas i hello förhandsversionen får inte vara uppdaterade tooa stöd för allmän tillgänglighet version. |Dessa virtuella enheter måste flyttas över för hello allmän tillgänglighet versionen med hjälp av ett arbetsflöde för disaster recovery (DR). |
| **2.** |Etablerade datadisk |När du har etablerat en datadisk med en viss angiven storlek och skapat hello motsvarande virtuell StorSimple-enhet, du måste inte öka eller minska hello datadisk. Försök toodo så leder till förlust av alla hello data i hello lokala nivåerna för hello enhet. | |
| **3.** |Grupprincip |När en enhet är ansluten till domänen kan kan tillämpa en grupprincip påverkas negativt hello enheten igen. |Kontrollera att din virtuella matrisen är i sin egen organisationsenhet (OU) för Active Directory och inga grupprincipobjekt (GPO) är tillämpade tooit. |
| **4.** |Lokala webbgränssnittet |Om Förbättrad säkerhetsfunktioner är aktiverade i Internet Explorer (IE ESC), vissa lokala webbsidor Användargränssnittet, till exempel felsökning eller underhåll kanske inte fungerar korrekt. Knapparna på dessa sidor fungerar kanske inte heller. |Inaktivera Förbättrad säkerhetsfunktioner i Internet Explorer. |
| **5.** |Lokala webbgränssnittet |Hej nätverksgränssnitt i hello web Användargränssnittet visas som 10 Gbit/s gränssnitt i en Hyper-V virtuell dator. |Detta är en avbildning av Hyper-V. Hyper-V visas alltid 10 Gbit/s för virtuella nätverkskort. |
| **6.** |Nivåindelade volymer eller resurser |Byteintervall låsning för program som fungerar med hello StorSimple nivåindelade volymer stöds inte. Om låsning byte-intervallet är aktiverad, fungerar StorSimple skiktning inte. |Rekommenderade åtgärder är: <br></br>Inaktivera låsning i applogiken byte-intervallet.<br></br>Välj tooput data för det här programmet i lokalt fästa volymer i motsats tootiered volymer.<br></br>*Begränsning*: om med hjälp av lokalt fästa volymer och låsa byte-intervallet är aktiverad, Tänk på att hello lokalt Fäst volym kan vara online innan hello återställningen är slutförd. I sådana fall om en återställning pågår måste sedan du vänta tills hello återställning toocomplete. |
| **7.** |Nivåindelad resurser |Arbeta med stora filer kan resultera i långsamma nivån. |När du arbetar med stora filer, rekommenderar vi att största hello-filen är mindre än 3% av hello resursen storlek. |
| **8.** |Används kapacitet för resurser |Du kan se dela förbrukning hello inte finns några data på hello-resurs. Det beror på att hello används kapacitet för resurser som innehåller metadata. | |
| **9.** |Haveriberedskap |Du kan bara utföra hello återställning av en fil server toohello samma domän som hello källenheten. Disaster recovery tooa målenhet i en annan domän stöds inte i den här versionen. |Detta kommer att genomföras i en senare version. |
| **10.** |Azure PowerShell |virtuella hello StorSimple-enheter kan inte hanteras via hello Azure PowerShell i den här versionen. |Alla hello hantering av hello virtuella enheter ska utföras via hello klassiska Azure-portalen och hello lokala webbgränssnittet. |
| **11.** |Ändra lösenordet |hello virtuella matris klientenhetskonsolen accepterar endast indata i en-US tangentbord format. | |
| **12.** |CHAP |CHAP autentiseringsuppgifter när skapat tas inte bort. Om du ändrar hello CHAP autentiseringsuppgifter ska du dessutom behöver tootake hello volymer och tar dem online för hello ändra tootake effekt. |Dessa kommer att åtgärdas i en senare version. |
| **13.** |iSCSI-server |hello 'används för lagring, visas för en iSCSI-volym kan skilja sig i hello StorSimple Manager-tjänsten och hello iSCSI-värden. |hello iSCSI-värden har hello filesystem vy.<br></br>hello enheten ser hello block allokeras när hello var i hello maximala storlek. |
| **14.** |Filen server * |Om en fil i en mapp har en alternativ Data dataström (ADS) som är associerade med den kan säkerhetskopieras eller återställs via katastrofåterställning och klona objektet återställning hello ANNONSER inte. | |

## <a name="next-step"></a>Nästa steg
[Installera uppdateringar](storsimple-ova-install-update-01.md) på din virtuella StorSimple-matrisen.

