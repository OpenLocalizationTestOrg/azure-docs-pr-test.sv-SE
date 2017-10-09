---
title: aaaStorSimple virtuella matris Update 0,5 viktig information | Microsoft Docs
description: "Beskriver viktiga öppna problem och lösningar för hello virtuella StorSimple-matris körs uppdatera 0,5."
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
ms.date: 05/08/2017
ms.author: alkohli
ms.openlocfilehash: a249e2e8f0105dcf6cc02d3136dfa3e37ea615d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-virtual-array-update-05-release-notes"></a>StorSimple virtuell matris uppdatering 0,5 viktig information

## <a name="overview"></a>Översikt

hello följande versionsinformation identifiera hello kritiska öppna problem och hello löst problem för Microsoft Azure StorSimple virtuell matris uppdateringar.

hello viktig information uppdateras kontinuerligt, och allteftersom allvarliga problem som kräver en lösning upptäcks, läggs de. Granska noggrant hello informationen i hello viktig information innan du distribuerar din virtuella StorSimple-matris.

Uppdatera 0,5 motsvarar toohello programvaruversion **10.0.10290.0**.

> [!NOTE]
> Uppdateringar kan störande och starta om enheten. Om i/o som pågår, ådrar hello enheten driftstopp. För detaljerade anvisningar om hur tooapply hello update gå för[uppdatering 0,5](storsimple-virtual-array-install-update-05.md).


## <a name="whats-new-in-hello-update-05"></a>Vad är nytt i hello Update 0,5
Uppdatering 0,5 är i första hand en buggfix version. hello huvudsakliga förbättringar och korrigeringar av fel är följande:

- **Säkerhetskopiera återhämtning förbättringar** -den här versionen har korrigeringar som förbättrar hello säkerhetskopiering återhämtning. I hello tidigare versioner, säkerhetskopior har gjorts endast för vissa undantag. Den här versionen återförsök alla hello säkerhetskopiering undantag och gör hello säkerhetskopieringar mer robust.

- **Uppdateringar för övervakning av lagringsutrymme användning** -startar 30 juni 2017 hello övervakning av lagringsutrymme användning för StorSimple virtuell enhet serien ska tas bort. Detta gäller toohello övervakning diagram för alla virtuella hello matriser som du kör uppdatering 0,4 eller lägre. Den här uppdateringen innehåller hello ändringar som krävs för du toocontinue hello använda lagringskvoten övervakning i hello Azure-portalen. Installera uppdateringen innan 30 juni 2017 toocontinue med hello övervakning funktionen.


## <a name="issues-fixed-in-hello-update-05"></a>Problem som åtgärdas i hello Update 0,5

hello innehåller följande tabell en översikt över problem som åtgärdas i den här versionen.

| Nej. | Funktion | Problem |
| --- | --- | --- |
| 1 |Säkerhetskopiering återhämtning| I hello tidigare versioner, säkerhetskopior har gjorts endast för vissa undantag. Den här versionen innehåller en korrigering toomake säkerhetskopieringar mer robust av du försöker alla hello säkerhetskopiering undantag.|
| 2 |Övervakning| hello övervakning av lagringsutrymme användning för StorSimple virtuell enhet serien att bli inaktuell från 30 juni 2017. Den här åtgärden påverkar hello övervakning diagram på hello StorSimple Device Manager-tjänsten körs på virtuella StorSimple-matriser (1200 model). Den här versionen har uppdateringar som tillåter hello användaren toocontinue hello användning av övervakning av lagringsutrymme användning för hello virtuella matriser utöver 30 juni 2017.|
| 3 |Filserver| I hello tidigare versioner av en användare kan av misstag kopiera krypterade filer toohello virtuella matris. Den här versionen innehåller en lösning som inte tillåter kopiering av krypterade filer toovirtual matris. Om enheten har befintliga krypterade filer-tidigare toohello uppdatera, fortsätter toofail säkerhetskopieringar tills alla hello krypterade filer tas bort från hello system. |


## <a name="known-issues-in-hello-update-05"></a>Kända problem i hello Update 0,5

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

## <a name="next-step"></a>Nästa steg
[Installera uppdatering 0,5](storsimple-virtual-array-install-update-05.md) på din virtuella StorSimple-matrisen.

## <a name="references"></a>Referenser
Letar du efter en äldre version anteckning? Gå till:

* [StorSimple virtuell matris Update 0,4 viktig information](storsimple-virtual-array-update-04-release-notes.md)
* [StorSimple virtuell matris Update 0,3 viktig information](storsimple-ova-update-03-release-notes.md)
* [StorSimple virtuell matris uppdatering 0.1 och 0,2 viktig information](storsimple-ova-update-01-release-notes.md)
* [StorSimple virtuell matris allmän tillgänglighet viktig information](storsimple-ova-pp-release-notes.md)

