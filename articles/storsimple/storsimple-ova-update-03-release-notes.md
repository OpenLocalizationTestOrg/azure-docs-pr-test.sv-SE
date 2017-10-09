---
title: aaaStorSimple virtuella matris uppdateringar viktig information | Microsoft Docs
description: "Beskriver viktiga öppna problem och lösningar för hello virtuella StorSimple-matris körs uppdatering 0.3."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: b197651a-3c40-4185-b23d-4c8f22cfa8f4
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/15/2016
ms.author: alkohli
ms.openlocfilehash: 305e6419b248fde134abae65f3d2c241d72ffa18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-virtual-array-update-03-release-notes"></a>StorSimple virtuell matris uppdatering 0,3 viktig information
## <a name="overview"></a>Översikt
hello följande versionsinformation identifiera hello kritiska öppna problem och hello löst problem för Microsoft Azure StorSimple virtuell matris uppdateringar.

hello viktig information uppdateras kontinuerligt, och allteftersom allvarliga problem som kräver en lösning upptäcks, läggs de. Granska noggrant hello informationen i hello viktig information innan du distribuerar din virtuella StorSimple-matris.

Uppdatering 0.3 motsvarar toohello programvaruversion **10.0.10288.0**.

> [!NOTE]
> Uppdateringar kan störande och starta om enheten. Om i/o som pågår, ådrar hello enheten driftstopp.
> 
> 

## <a name="whats-new-in-hello-update-03"></a>Vad är nytt i hello uppdatering 0.3
Uppdatering 0.3 är i första hand en buggfix version. I den här versionen har flera programfel, vilket resulterar i Säkerhetskopieringsfel i hello tidigare version åtgärdats.

## <a name="issues-fixed-in-hello-update-03"></a>Problem som åtgärdas i hello uppdatering 0.3
hello innehåller följande tabell en översikt över problem som åtgärdas i den här versionen.

| Nej. | Funktion | Problem |
| --- | --- | --- |
| 1 |Säkerhetskopior |Ett problem påträffades i hello tidigare version där hello säkerhetskopieringar misslyckas toocomplete för en filresurs. Om det här problemet uppstod hello säkerhetskopieringsjobb misslyckas och en kritisk varning har aktiverats på hello StorSimple Manager-tjänsten toonotify hello användare. Det här problemet inte påverkar hello data på hello eller komma åt toohello data. hello grundorsaken har identifierats och åtgärdats i den här versionen. <br></br> hello korrigering gäller inte retroaktivt tooshares som redan ser det här problemet. Kunder som ser det här problemet bör du först gäller uppdatering 0.3 sedan kontakta Microsoft Support tooperform en fullständig säkerhetskopiering toofix hello systemproblem. I stället för att kontakta Microsoft Support kan kunder även återställa tooa ny resurs från en felfri säkerhetskopia för hello påverkas resurser. |
| 2 |iSCSI |Ett problem påträffades i hello tidigare version där hello volymer försvinner när du kopierar tooa datavolym på hello virtuella StorSimple-matris. Det här problemet åtgärdades i den här versionen. <br></br> hello korrigeringar gälla bara på nyskapad volymer. hello korrigeringar gäller inte retroaktivt toovolumes som redan ser det här problemet. Kunder bör toobring hello påverkas volymer online via hello klassiska Azure-portalen, utföra en säkerhetskopiering av dessa volymer och återställa dessa volymer toonew volymer. |

## <a name="known-issues-in-hello-update-03"></a>Kända problem i hello uppdatering 0.3
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

## <a name="next-step"></a>Nästa steg
[Installera uppdatering 0.3](storsimple-ova-install-update-01.md) på din virtuella StorSimple-matrisen.

## <a name="references"></a>Referenser
Letar du efter en äldre version anteckning? Gå till: 

* [StorSimple virtuell matris uppdatering 0.1 och 0,2 viktig information](storsimple-ova-update-01-release-notes.md)
* [StorSimple virtuell matris allmän tillgänglighet viktig information](storsimple-ova-pp-release-notes.md)

