---
title: aaaStorSimple virtuella matris Update 0,6 viktig information | Microsoft Docs
description: "Beskriver viktiga öppna problem och lösningar för hello virtuella StorSimple-matris körs uppdatera 0,6."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/24/2017
ms.author: alkohli
ms.openlocfilehash: 7b573457dc07ce41e95d49d66ccc174045f31a42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-virtual-array-update-06-release-notes"></a>StorSimple virtuell matris uppdatering 0,6 viktig information

## <a name="overview"></a>Översikt

hello följande versionsinformation identifiera hello kritiska öppna problem och hello löst problem för Microsoft Azure StorSimple virtuell matris uppdateringar.

hello viktig information uppdateras kontinuerligt, och allteftersom allvarliga problem som kräver en lösning upptäcks, läggs de. Granska noggrant hello informationen i hello viktig information innan du distribuerar din virtuella StorSimple-matris.

Uppdatera 0,6 motsvarar toohello programvaruversion **10.0.10293.0**.

> [!IMPORTANT]
> - Uppdateringar kan störande och starta om enheten. Om i/o som pågår, ådrar hello enheten driftstopp. För detaljerade anvisningar om hur tooapply hello update gå för[uppdatering 0,6](storsimple-virtual-array-install-update-06.md).
>
> - Vi rekommenderar starkt att du installerar uppdateringen 0,6 omedelbart eftersom den innehåller viktiga säkerhetskorrigeringar.


## <a name="whats-new-in-hello-update-06"></a>Vad är nytt i hello Update 0,6
Uppdateringen 0,6 är en viktig uppdatering och bör distribueras direkt. Den här uppdateringen innehåller följande korrigeringar hello: 

- **Windows säkerhetskorrigeringar** -den här versionen har **Windows kritiska säkerhetskorrigeringar**. Granska hello följande säkerhetsuppdateringar för mer information om hello säkerhetsproblem och hello associerade korrigeringar:
    - [December 2016 säkerhet endast kvalitet uppdatering för Windows 8.1 och Windows Server 2012 R2](https://support.microsoft.com/help/3205400/december-2016-security-only-quality-update-for-windows-8.1-and-windows-server-2012-r2)
    - [Mars 2017 säkerhet endast kvalitet uppdatering för Windows 8.1 och Windows Server 2012 R2](https://support.microsoft.com/help/4012213/march-2017-security-only-quality-update-for-windows-8-1-and-windows-server-2012-r23)
    - [9 maj 2017 – KB4019213 (endast säkerhet uppdatering)](https://support.microsoft.com/help/4019213/windows-8-update-kb4019213)

- **Återställa korrigera** -det uppstod ett fel som kan förhindra att hello återställning från att slutföras i tidigare versioner. Det här felet har korrigerats i den här versionen.


## <a name="issues-fixed-in-hello-update-06"></a>Problem som åtgärdas i hello Update 0,6

hello innehåller följande tabell en översikt över problem som åtgärdas i den här versionen.

| Nej. | Funktion | Problem |
| --- | --- | --- |
| 1 |Säkerhet| Den här versionen innehåller viktiga uppdateringar för Windows-säkerhet. Vi rekommenderar att du installerar uppdateringen omedelbart.|
| 2 |Återställ| Det uppstod ett konkurrenstillstånd som förhindrar hello återställningsjobbet Slutför under en återställning. Hej buggfix löser den här konkurrenstillstånd.|


## <a name="known-issues-in-hello-update-06"></a>Kända problem i hello Update 0,6

hello följande tabell innehåller en översikt över kända problem för hello virtuella StorSimple-matris och omfattar hello problem versionen anges från hello tidigare versioner.

| Nej. | Funktion | Problem | Lösning/kommentarer |
| --- | --- | --- | --- |
| **1.** |Uppdateringar |hello virtuella enheter skapas i hello förhandsversionen får inte vara uppdaterade tooa stöd för allmän tillgänglighet version. |Dessa virtuella enheter måste flyttas över för hello allmän tillgänglighet versionen med hjälp av ett arbetsflöde för disaster recovery (DR). |
| **2.** |Etablerade datadisk |När du har etablerat en datadisk med en viss angiven storlek och skapat hello motsvarande virtuell StorSimple-enhet, du måste inte öka eller minska hello datadisk. Försöker toodo leder till förlust av alla hello data i hello lokala nivåerna för hello enhet. | |
| **3.** |Grupprincip |När en enhet är ansluten till domänen kan kan tillämpa en grupprincip påverkas negativt hello enheten igen. |Kontrollera att din virtuella matrisen är i sin egen organisationsenhet (OU) för Active Directory och inga grupprincipobjekt (GPO) är tillämpade tooit. |
| **4.** |Lokala webbgränssnittet |Om Förbättrad säkerhetsfunktioner är aktiverade i Internet Explorer (IE ESC), vissa lokala webbsidor Användargränssnittet, till exempel felsökning eller underhåll kanske inte fungerar korrekt. Knapparna på dessa sidor fungerar kanske inte heller. |Inaktivera Förbättrad säkerhetsfunktioner i Internet Explorer. |
| **5.** |Lokala webbgränssnittet |Hej nätverksgränssnitt i hello web Användargränssnittet visas som 10 Gbit/s gränssnitt i en Hyper-V virtuell dator. |Detta är en avbildning av Hyper-V. Hyper-V visas alltid 10 Gbit/s för virtuella nätverkskort. |
| **6.** |Nivåindelade volymer eller resurser |Byteintervall låsning för program som fungerar med hello StorSimple nivåindelade volymer stöds inte. Om låsning byte-intervallet är aktiverad, fungerar StorSimple skiktning inte. |Rekommenderade åtgärder är: <br></br>Inaktivera låsning i applogiken byte-intervallet.<br></br>Välj tooput data för det här programmet i lokalt fästa volymer i motsats tootiered volymer.<br></br>*Begränsning*: när med hjälp av lokalt fästa volymer och låsa byte-intervallet är aktiverat, hello lokalt Fäst volym kan det vara online innan hello återställningen är slutförd. I sådana fall om en återställning pågår måste sedan du vänta tills hello återställning toocomplete. |
| **7.** |Nivåindelad resurser |Arbeta med stora filer kan resultera i långsamma nivån. |När du arbetar med stora filer, rekommenderar vi att största hello-filen är mindre än 3% av hello resursen storlek. |
| **8.** |Används kapacitet för resurser |Du kan se dela förbrukning när det finns inga data på hello-resurs. Denna förbrukning beror hello används kapacitet för resurser som innehåller metadata. | |
| **9.** |Haveriberedskap |Du kan bara utföra hello återställning av en fil server toohello samma domän som hello källenheten. Disaster recovery tooa målenhet i en annan domän stöds inte i den här versionen. |Detta är implementerad i en senare version. Mer information finns för[redundans och disaster recovery för din virtuella StorSimple-matris](storsimple-virtual-array-failover-dr.md) |
| **10.** |Azure PowerShell |virtuella hello StorSimple-enheter kan inte hanteras via hello Azure PowerShell i den här versionen. |Alla hello hantering av hello virtuella enheter ska utföras via hello Azure-portalen och hello lokala webbgränssnittet. |
| **11.** |Ändra lösenordet |hello virtuella matris klientenhetskonsolen endast accepterar indata i en-us tangentbord format. | |
| **12.** |CHAP |CHAP autentiseringsuppgifter när skapat tas inte bort. Om du ändrar hello CHAP autentiseringsuppgifter du behöver tootake hello volymer och dessutom tar dem online för hello ändra tootake effekt. |Det här problemet åtgärdas i en senare version. |
| **13.** |iSCSI-server |hello 'används för lagring, visas för en iSCSI-volym kan skilja sig i hello StorSimple Device Manager-tjänsten och hello iSCSI-värden. |hello iSCSI-värden har hello filesystem vy.<br></br>hello enheten ser hello block allokeras när hello var i hello maximala storlek. |
| **14.** |Filserver |Om en fil i en mapp har en alternativ Data dataström (ADS) som är associerade med den kan säkerhetskopieras eller återställs via katastrofåterställning och klona objektet återställning hello ANNONSER inte. | |
| **15.** |Filserver |Symboliska länkar stöds inte. | |
| **16.** |Filserver |Filer som skyddas av Windows Krypterande filsystem (EFS) när kopieras över eller lagras på hello virtuella StorSimple-matris fil servern resulterar i en konfiguration som inte stöds.  | |
| **17.** |Uppdateringar |Om du ser fel kod: 2359302 (hex 0x240006) vid tooinstall en snabbkorrigering via hello lokala användargränssnitt och sedan detta innebär att hello snabbkorrigeringen är installerad på din enhet.   | |

## <a name="next-step"></a>Nästa steg
[Installera uppdatering 0,6](storsimple-virtual-array-install-update-06.md) på din virtuella StorSimple-matrisen.

## <a name="references"></a>Referenser
Letar du efter en äldre version anteckning? Gå till:

* [StorSimple virtuell matris Update 0,5 viktig information](storsimple-virtual-array-update-05-release-notes.md)
* [StorSimple virtuell matris Update 0,4 viktig information](storsimple-virtual-array-update-04-release-notes.md)
* [StorSimple virtuell matris Update 0,3 viktig information](storsimple-ova-update-03-release-notes.md)
* [StorSimple virtuell matris uppdatering 0.1 och 0,2 viktig information](storsimple-ova-update-01-release-notes.md)
* [StorSimple virtuell matris allmän tillgänglighet viktig information](storsimple-ova-pp-release-notes.md)

