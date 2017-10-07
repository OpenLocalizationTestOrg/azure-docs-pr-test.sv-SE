---
title: aaaView och hantera StorSimple jobb | Microsoft Docs
description: "Beskriver sidan för jobb hello StorSimple Manager-tjänsten och hur toouse den tootrack senaste aktuella och schemalagda säkerhetskopieringsjobb."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: cdd3e9e8-7ccd-40a3-8c07-0ab1338c0e59
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: d6ecdcbc3d8a4757c2328303f268e51c8ce26b65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-tooview-and-manage-storsimple-jobs-update-2"></a>Använd hello StorSimple Manager-tjänsten tooview och hantera StorSimple jobb (uppdatering 2)
[!INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Översikt
Hej **jobb** innehåller en enda central portal för visning och hantera jobb som har startats på enheter anslutna tooyour StorSimple Manager-tjänsten. Du kan visa schemalagda, körs, slutfört, avbrutet och misslyckade jobb för flera enheter. Resultat visas i tabellformat. 

![Sidan jobb](./media/storsimple-manage-jobs-u2/jobs.png)

Du kan snabbt hitta hello jobb som du är intresserad av genom att filtrera på fält som:

* **Status för** – jobb kan köras, slutförda, avbrutna, misslyckades, stoppar eller slutfördes med fel.
* **Från och till** – jobb kan filtreras baserat på hello datum och ett tidsintervall.
* **Typen** – hello jobbtypen kan vara säkerhetskopiering, manuell säkerhetskopiering, återställning, kloning, enheten redundans, skapar lokalt Fäst volym, ändra volymen, uppdatera, stöd för paket eller etablering av virtuella enheten.
* **Enheter** – jobb startas på en viss enhet ansluten tooyour-tjänst.
  hello filtrerade jobben visas sedan som en tabell hello baserat på hello följande attribut:
  
  * **Typen** – säkerhetskopiering, manuell säkerhetskopiering, återställning, kloning, enheten redundans, skapar lokalt Fäst volym, ändra volymen, uppdatera, stöd för paket eller etablering av virtuella enheten.
  * **Status för** – körs, slutförda, avbrutna, misslyckades, stoppar eller slutfördes med fel.
  * **Entiteten** – hello jobb kan vara kopplad till en volym, en princip för säkerhetskopiering eller en enhet. Till exempel associeras ett jobb för klon med en volym, en schemalagd säkerhetskopiering är associerad med en princip för säkerhetskopiering. En enhet jobb skapas på grund av en katastrofåterställning (DR) eller en återställning.
  * **Enheten** – hello namn på hello enhet på vilken hello-jobbet startades.
  * **Startats på** – hello tid då hello-jobbet startades.
  * **Förlopp** – hello procentandel slutförande av ett jobb som körs. Detta bör alltid vara 100% för slutförda jobb.

hello lista över jobb uppdateras var 30: e sekund.

Du kan utföra hello följande jobbrelaterade åtgärder på den här sidan:

* Visa jobbinformation
* Avbryta ett jobb

## <a name="view-job-details"></a>Visa jobbinformation
Utföra hello följande steg tooview hello information för alla jobb.

#### <a name="tooview-job-details"></a>tooview jobbinformation
1. På hello **jobb** sidan, visa hello jobb som du är intresserad av genom att köra en fråga med lämpliga filter. Du kan söka för slutförd, kör eller avbröts jobb.
2. Välj ett jobb.
3. Hello längst hello-sidan, klickar du på **information**.
4. I hello **säkerhetskopiering jobbinformation** dialogrutan kan du visa hello status, information, statistik och data statistik.
   
    ![Informationssidan för jobbet](./media/storsimple-manage-jobs-u2/JobDetails.png)

## <a name="cancel-a-job"></a>Avbryta ett jobb
Utföra hello följande steg toocancel ett jobb som körs.

> [!NOTE]
> Kan inte annulleras vissa jobb, t.ex ändra en toochange hello volym volymtyp eller expandera en volym.
> 
> 

### <a name="toocancel-a-job"></a>toocancel ett jobb
1. På hello **jobb** sidan, visa hello körs jobb som du vill toocancel genom att köra en fråga med lämpliga filter.
2. Välj hello jobb.
3. Hello längst hello-sidan, klickar du på **Avbryt**.
4. Klicka på **Ja** när du uppmanas att bekräfta åtgärden.

Det här jobbet avbryts nu.

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[hantera din StorSimple-säkerhetskopieringsprinciper](storsimple-manage-backup-policies.md).
* Lär dig hur för[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

