---
title: aaaStorSimple virtuella matris viktig information | Microsoft Docs
description: "Beskriver viktiga öppna problem och lösningar för hello virtuella StorSimple-matris."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 84908160-2b8b-4f4f-a674-f39aaa0bd4de
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/13/2016
ms.author: alkohli
ms.openlocfilehash: ca7b543f95cf5787b7fef39f53887161ebfa7fcb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-virtual-array-release-notes"></a>Viktig information för virtuell StorSimple-matris
## <a name="overview"></a>Översikt
hello identifiera följande versionsinformation hello kritiska öppna problem för hello mars 2016 allmän tillgänglighet (GA) versionen av hello Microsoft Azure StorSimple virtuell matris (även kallat hello StorSimple lokala virtuella enheten eller hello virtuella StorSimple enhet). Den här versionen motsvarar toosoftware version 10.0.10271.0.

hello viktig information uppdateras kontinuerligt, och allteftersom allvarliga problem som kräver en lösning upptäcks, läggs de. Granska noggrant hello informationen i hello viktig information innan du distribuerar din virtuella StorSimple-enhet. 

hello följande tabell innehåller en översikt över kända problem i den här versionen.

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

