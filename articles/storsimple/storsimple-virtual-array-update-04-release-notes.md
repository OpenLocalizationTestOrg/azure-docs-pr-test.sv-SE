---
title: aaaStorSimple virtuella matris Update 0,4 viktig information | Microsoft Docs
description: "Beskriver viktiga öppna uppdatera 0,4 problem och lösningar för hello virtuella StorSimple-matris körs."
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
ms.date: 04/05/2017
ms.author: alkohli
ms.openlocfilehash: 1fd174ff483f4f7b1b4a7853c9d9573d1e948cb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-virtual-array-update-04-release-notes"></a>StorSimple virtuell matris uppdatering 0,4 viktig information

## <a name="overview"></a>Översikt

hello följande versionsinformation identifiera hello kritiska öppna problem och hello löst problem för Microsoft Azure StorSimple virtuell matris uppdateringar.

hello viktig information uppdateras kontinuerligt, och allteftersom allvarliga problem som kräver en lösning upptäcks, läggs de. Granska noggrant hello informationen i hello viktig information innan du distribuerar din virtuella StorSimple-matris.

Uppdatera 0,4 motsvarar toohello programvaruversion **10.0.10289.0**.

> [!NOTE]
> Uppdateringar kan störande och starta om enheten. Om i/o som pågår, ådrar hello enheten driftstopp.


## <a name="whats-new-in-hello-update-04"></a>Vad är nytt i hello Update 0,4
Uppdatering 0,4 är i första hand en buggfix version som tillsammans med några förbättringar. I den här versionen har flera programfel, vilket resulterar i Säkerhetskopieringsfel i hello tidigare version åtgärdats. hello huvudsakliga förbättringar och korrigeringar av fel är följande:

- **Säkerhetskopiera prestandaförbättringar** -den här versionen har gjort flera viktiga förbättringar tooimprove hello säkerhetskopieringsprestanda. Därför finns hello säkerhetskopieringar som omfattar ett stort antal filer som i en betydande minskning av hello tid toocomplete, fullständiga och inkrementella säkerhetskopior.

- **Förbättrad återställningsprestanda** -den här versionen innehåller förbättringar som förbättrar prestandan för hello-återställning när du använder många filer. Med 2-4 miljoner filer, rekommenderar vi att du etablera en virtuell matris med 16 GB RAM-minne toosee hello förbättringar. När du använder mindre än 2 miljoner filer, hello minimikravet för hello virtuella datorn fortsätter toobe 8 GB RAM-minne.

- **Förbättringar av tooSupport paketet** -hello-förbättringar är loggning i hello statistik för disk, CPU, minne, nätverk och molntjänster i hello supportpaket därigenom förbättra hello processen att diagnostisera/felsökning av problem med enheter.

- **Gränsen lokalt Fäst iSCSI volymer too200 GB** -för lokalt fästa volymer, rekommenderar vi att du begränsar tooa 200 GB iSCSI-volym på din virtuella StorSimple-matrisen. hello lokala reservation för nivåindelade volymer fortsätter toobe 10% av hello etablerade volymens storlek men högst 200 GB. 

- **Backup-relaterade felkorrigeringar** -det fanns problem relaterade toobackups som skulle orsaka fel vid säkerhetskopiering i tidigare versioner av programvaran. Dessa programfel har åtgärdats i den här versionen.


## <a name="issues-fixed-in-hello-update-04"></a>Problem som åtgärdas i hello Update 0,4

hello innehåller följande tabell en översikt över problem som åtgärdas i den här versionen.

| Nej. | Funktion | Problem |
| --- | --- | --- |
| 1 |Prestanda vid säkerhetskopiering|Hello skulle tidigare versioner, hello säkerhetskopiering med många filer ta en lång tid toocomplete (i ordning hello dagar). I den här versionen finns både hello fullständiga och inkrementella säkerhetskopior i en betydande minskning av hello tid toocompletion. |
| 2 |Support-paket|Disk, CPU, minne, nätverk och molnet statistik är nu inloggad toohello Support loggar gör hello supportpaket mycket effektivt vid felsökning av problem med enheter.|
| 3 |Säkerhetskopiering |I tidigare versioner, kan långvariga säkerhetskopieringar resultera i ett utrymme matar på hello enhet ledde till ett fel vid säkerhetskopiering. Det här problemet åtgärdas i den här versionen genom att tillåta mer än 5 säkerhetskopieringar tooqueue i taget.|
| 4 |iSCSI | I tidigare versioner kunde hello lokala reservation för nivåindelade eller lokalt fästa volymer 10% av hello etablerade volymens storlek. I den här versionen hello lokala reservationen för iSCSI-volymer (lokalt Fäst eller nivåer) är begränsad too10% med högst upp till 200 GB (för nivåindelade volymer som är större än 2 TB) vilket frigöra mer utrymme på hello lokal disk. Vi rekommenderar att hello lokalt fästa volymer i den här versionen är begränsad too200 GB.|


## <a name="known-issues-in-hello-update-04"></a>Kända problem i hello Update 0,4

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
| **8.** |Används kapacitet för resurser |Du kan se dela förbrukning när det finns inga data på hello-resurs. Det beror på att hello används kapacitet för resurser som innehåller metadata. | |
| **9.** |Haveriberedskap |Du kan bara utföra hello återställning av en fil server toohello samma domän som hello källenheten. Disaster recovery tooa målenhet i en annan domän stöds inte i den här versionen. |Detta är implementerad i en senare version. |
| **10.** |Azure PowerShell |virtuella hello StorSimple-enheter kan inte hanteras via hello Azure PowerShell i den här versionen. |Alla hello hantering av hello virtuella enheter ska utföras via hello klassiska Azure-portalen och hello lokala webbgränssnittet. |
| **11.** |Ändra lösenordet |hello virtuella matris klientenhetskonsolen accepterar endast indata i en-US tangentbord format. | |
| **12.** |CHAP |CHAP autentiseringsuppgifter när skapat tas inte bort. Om du ändrar hello CHAP autentiseringsuppgifter du behöver tootake hello volymer och dessutom tar dem online för hello ändra tootake effekt. |Det här problemet åtgärdas i en senare version. |
| **13.** |iSCSI-server |hello 'används för lagring, visas för en iSCSI-volym kan skilja sig i hello StorSimple Manager-tjänsten och hello iSCSI-värden. |hello iSCSI-värden har hello filesystem vy.<br></br>hello enheten ser hello block allokeras när hello var i hello maximala storlek. |
| **14.** |Filserver |Om en fil i en mapp har en alternativ Data dataström (ADS) som är associerade med den kan säkerhetskopieras eller återställs via katastrofåterställning och klona objektet återställning hello ANNONSER inte. | |
| **15.** |Filserver |Symboliska länkar stöds inte. | |
| **16.** |Filserver |Filer som skyddas av Windows Krypterande filsystem (EFS) när kopieras över eller lagras på hello virtuella StorSimple-matris fil servern resulterar i en konfiguration som inte stöds.  | |

## <a name="next-step"></a>Nästa steg
[Installera uppdatering 0,4](storsimple-virtual-array-install-update-04.md) på din virtuella StorSimple-matrisen.

## <a name="references"></a>Referenser
Letar du efter en äldre version anteckning? Gå till: 

* [StorSimple virtuell matris Update 0,3 viktig information](storsimple-ova-update-03-release-notes.md)
* [StorSimple virtuell matris uppdatering 0.1 och 0,2 viktig information](storsimple-ova-update-01-release-notes.md)
* [StorSimple virtuell matris allmän tillgänglighet viktig information](storsimple-ova-pp-release-notes.md)

