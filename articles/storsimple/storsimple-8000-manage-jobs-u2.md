---
title: "aaaView och hantera jobb för StorSimple 8000-serien | Microsoft Docs"
description: "Beskriver hello StorSimple Enhetshanteraren service jobb-bladet och hur toouse den tootrack senaste aktuella och schemalagda säkerhetskopieringsjobb."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: a76acd67e2568478dd43d0fb16c1ae2dfb72a23d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-tooview-and-manage-jobs-update-3-and-later"></a>Använd hello StorSimple Enhetshanteraren service tooview och hantera jobb (uppdatering 3 och senare)

## <a name="overview"></a>Översikt
Hej **jobb** bladet innehåller en enda central portal för visning och hantera jobb som har startats på enheter anslutna tooyour StorSimple enheten Manager-tjänsten. Du kan visa schemalagda, körs, slutfört, avbrutet och misslyckade jobb för flera enheter. Resultat visas i tabellformat.

![Jobb-bladet](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

Du kan snabbt hitta hello jobb som du är intresserad av genom att filtrera på fält som:

* **Status för** – jobb kan vara pågår, lyckades, avbröts, misslyckades, stoppar eller har slutförts med fel.
* **Tidsintervallet** – jobb kan filtreras baserat på hello datum och ett tidsintervall. hello intervall har passerat 1 timme, senaste 24 timmarna, de senaste 7 dagarna, de senaste 30 dagarna, senaste året eller anpassade datum.
* **Typen** – hello jobbtypen kan vara schemalagd säkerhetskopiering, manuell säkerhetskopiering, återställning, säkerhetskopiering, klona volymen, växla över volymbehållare, skapa lokalt Fäst volym, ändra volymen, installera uppdateringar, samla in loggar för support och skapa moln-enhet.
* **Enheter** – jobb startas på en viss enhet ansluten tooyour-tjänst.
  
hello filtrerade jobben visas sedan som en tabell hello baserat på hello följande attribut:
  
* **Namnet** – schemalagd säkerhetskopiering, manuell säkerhetskopiering, återställning, säkerhetskopiering, klona volym växla över volymbehållare, skapar lokalt Fäst volym, ändra volymen, installera uppdateringar, samla in loggar för stöd eller skapa moln-enhet.
* **Status för** – körs, slutförda, avbrutna, misslyckades, stoppar eller slutfördes med fel.
* **Entiteten** – hello jobb kan vara kopplad till en volym, en princip för säkerhetskopiering eller en enhet. Till exempel associeras ett jobb för klon med en volym, en schemalagd säkerhetskopiering är associerad med en princip för säkerhetskopiering. En enhet jobb skapas på grund av en katastrofåterställning (DR) eller en återställning.
* **Enheten** – hello namn på hello enhet på vilken hello-jobbet startades.
* **Startats på** – hello tid då hello-jobbet startades.
* **Varaktighet** – hello krävs toocomplete hello jobbet.

hello lista över jobb uppdateras var 30: e sekund.

Du kan utföra hello följande jobbrelaterade åtgärder på den här sidan:

* Visa jobbinformation
* Avbryta ett jobb

## <a name="view-job-details"></a>Visa jobbinformation
Utföra hello följande steg tooview hello information för alla jobb.

#### <a name="tooview-job-details"></a>tooview jobbinformation
1. Gå tooyour StorSimple enheten Manager-tjänsten och klicka sedan på **jobb**.

2. I hello **jobb** bladet, visa hello jobb som du är intresserad av genom att köra en fråga med lämpliga filter. Du kan söka för slutförd, kör eller avbröts jobb.

    ![Jobb-bladet](./media/storsimple-8000-manage-jobs-u2/jobs1.png)

2. Välj och klicka på ett jobb.

    ![Jobb-bladet](./media/storsimple-8000-manage-jobs-u2/jobs3.png)

3. I hello jobbet informationsbladet ser du hello status, information, statistik och data statistik.
   
    ![Jobbinformation](./media/storsimple-8000-manage-jobs-u2/jobs4.png)

## <a name="cancel-a-job"></a>Avbryta ett jobb
Utföra hello följande steg toocancel ett jobb som körs.

> [!NOTE]
> Kan inte annulleras vissa jobb, t.ex ändra en toochange hello volym volymtyp eller expandera en volym.


### <a name="toocancel-a-job"></a>toocancel ett jobb
1. På hello **jobb** sidan, visa hello körs jobb som du vill toocancel genom att köra en fråga med lämpliga filter. Välj hello jobb.

2. Högerklicka på snabbmenyn för hello valda jobbet tooinvoke hello och på **Avbryt**.

    ![Jobbinformation](./media/storsimple-8000-manage-jobs-u2/jobs2.png)

3. Klicka på **Ja** när du uppmanas att bekräfta åtgärden. Det här jobbet avbryts nu.

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[hantera din StorSimple-säkerhetskopieringsprinciper](storsimple-8000-manage-backup-policies-u2.md).
* Lär dig hur för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).

