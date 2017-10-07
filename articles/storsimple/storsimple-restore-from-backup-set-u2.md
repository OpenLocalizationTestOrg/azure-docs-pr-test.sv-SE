---
title: "aaaRestore en StorSimple-volym från en säkerhetskopia | Microsoft Docs"
description: "Förklarar hur hello toouse StorSimple Manager service säkerhetskopieringskatalogen sidan toorestore en StorSimple-volym från en säkerhetskopia."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 6f289c39-96c7-4d57-b68a-4bc2e99aef9d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/22/2017
ms.author: alkohli
ms.openlocfilehash: c2e38765e750749f5764b5cbf2167d3cd5edfe5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set-update-2"></a>Återställa en StorSimple-volym från en säkerhetskopia (uppdatering 2)
[!INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Översikt
Hej **säkerhetskopieringskatalogen** sidan visas alla hello säkerhetskopior som skapas när manuell eller automatisk säkerhetskopiering utförs. Använd den här sidan toolist och hantera säkerhetskopior, återställa från en säkerhetskopia eller klona en volym.

 ![Säkerhetskopiera katalog](./media/storsimple-restore-from-backup-set-u2/restore.png)

Den här självstudiekursen beskrivs hur toouse hello **säkerhetskopieringskatalogen** sidan toorestore enheten från en säkerhetskopia.

Du kan återställa en volym från en lokal eller säkerhetskopiering i molnet. I båda fallen ger hello återställningen hello volymen online direkt medan data hämtas i hello bakgrund. 

## <a name="before-you-restore"></a>Innan du återställer
Innan du startar en återställning, bör du vara medveten om hello följande varningar:

* **Kopplar från hello volym** – ta hello volymen offline på båda hello-värden och hello enheten innan du startar hello återställningen igen. Även om hello återställningen öppnar automatiskt hello-volym på hello enhet, måste du manuellt ta hello-enhet på hello värden. Du kan hämta hello-volym på hello värden när hello volymen är online på hello enhet. (Du behöver inte toowait tills hello återställningen är klar.) Procedurer finns för[kopplar från en volym](storsimple-manage-volumes-u2.md#take-a-volume-offline).
* **Volymtyp efter återställning** – borttagna volymer återställs utifrån hello typ i hello ögonblicksbild. Volymer som har lokalt Fäst återställs som lokalt fästa volymer och volymer som har nivåer återställs som nivåindelade volymer.
  
    För befintliga volymer åsidosätter hello aktuella användningstyp för hello volym hello-typ som är lagrad i hello ögonblicksbild. Till exempel om du återställer en volym från en ögonblicksbild som vidtogs när hello volymtyp har nivåer och att volymen är nu lokalt Fäst (förfaller tooa konvertering), och sedan hello volym återställs som en lokalt Fäst volym. På liknande sätt, om en befintlig lokalt Fäst volym expanderas och därefter återställts från en äldre ögonblicksbild vidtas när hello var mindre, hello återställda volym behåller hello aktuella utökade storleken.
  
    Du kan inte konvertera en volym från en nivåindelad volym tooa lokalt Fäst volym eller _vice versa_ medan hello volym återställs. Vänta tills hello återställningen är klar och sedan kan du konvertera hello tooanother volymtyp. Information om hur du konverterar en volym finns för[ändra hello volymtyp](storsimple-manage-volumes-u2.md#change-the-volume-type). 
* **hello volymstorleken avspeglas i hello återställts volym** – detta är viktigt om du återställer en lokalt Fäst volym som har tagits bort (eftersom lokalt fästa volymer är helt etablerad). Kontrollera att du har tillräckligt med utrymme innan du försöker toorestore en lokalt Fäst volym som har tagits bort. 
* **Du kan inte utöka en volym när den återställs** – vänta tills hello återställningen är klar innan du försöker tooexpand hello volym. Information om hur du utökar en volym finns för[ändra en volym](storsimple-manage-volumes-u2.md#modify-a-volume).
* **Du kan utföra en säkerhetskopiering när du återställer en lokal volym** – procedurer finns för[använda hello StorSimple Manager-tjänsten toomanage säkerhetskopieringsprinciper](storsimple-manage-backup-policies.md).
* **Du kan avbryta en återställningsåtgärd** – om du avbryter hello återställningsjobbet sedan hello volym återställs toohello tillstånd som innan du påbörjade hello återställning. Procedurer finns för[avbryta ett jobb](storsimple-manage-jobs-u2.md#cancel-a-job).

## <a name="how-does-restore-work"></a>Hur återställer arbete
För enheter som kör uppdatering 4 eller senare, implementeras en heatmap-baserad återställning. Hello värd begäranden tooaccess data når hello enhet, dessa begäranden spåras och en heatmap skapas. Hög begärandehastighet resulterar i datasegment med högre termiska medan lägre begärandehastighet översätter toochunks med lägre värme. Du måste ansluta till hello data minst två gånger toobe markerad som _varm_. En fil som ändras också har markerats som _varm_. När du startar hello återställning inträffar proaktiv hydrering av data baserat på hello heatmap. För versioner har före uppdatering 4 hello data hämtats vid återställning baserat på enbart åtkomst. 

Heatmap-baserade spårning aktiveras endast för nivåindelade volymer och lokalt fästa volymer inte stöds. Heatmap-baserad återställning stöds inte även när du klonar en volym tooanother enhet. Om det finns en återställning på plats och en lokal ögonblicksbild för hello volym toobe återställas finns på hello enhet vi inte rehydrate (eftersom data är redan tillgänglig lokalt). Som standard när du återställer initieras hello rehydration jobb som proaktivt rehydrate data baserat på hello heatmap. I uppdatering 4, kan Windows PowerShell-cmdlets använda tooquery rehydration jobb som körs, avbryts rehydration och hämta hello hello rehydration jobbets status.

* `Get-HcsRehydrationJob`-Denna cmdlet hämtar hello hello rehydration jobbets status. Ett enda rehydration jobb utlöses för en volym.
* `Set-HcsRehydrationJob`-Denna cmdlet kan du toopause, stoppa, återuppta hello rehydration jobbet när hello rehydration pågår. 

Mer information om rehydration cmdlets gå för[Windows PowerShell-cmdlet-referens för StorSimple](https://technet.microsoft.com/library/dn688168.aspx).

Med automatisk rehdyration vanligtvis förväntas högre tillfälligt läsprestanda. hello faktiska magniutde förbättringarna beror på faktorer som mönster för dataåtkomst, dataomsättningen och -datatypen. Du kan använda hello PowerShell-cmdleten toocancel ett rehydration-jobb. Kontakta Microsoft Support om du vill toopermanently inaktivera rehydration jobb för alla framtida hello-återställningar.

## <a name="how-toouse-hello-backup-catalog"></a>Hur toouse hello säkerhetskopieringskatalogen
Hej **säkerhetskopieringskatalogen** innehåller en fråga som hjälper dig att toonarrow valet av säkerhetskopian. Du kan filtrera hello säkerhetskopior som hämtas baserat på hello följande parametrar:

* **Enheten** – hello enhet på vilken hello säkerhetskopian skapades.
* **Säkerhetskopiera princip** eller **volym** – hello säkerhetskopieringsprincip eller volym som är associerade med den här säkerhetskopian.
* **Från** och **till** – hello intervallet för datum och tid när hello säkerhetskopian skapades.

hello filtrerade säkerhetskopior visas sedan som en tabell baserad på hello följande attribut:

* **Namnet** – hello namn på hello säkerhetskopieringsprincip eller volym som är associerade med hello säkerhetskopia.
* **Storlek** – hello verkliga storleken hos hello säkerhetskopia.
* **Skapas på** – hello datum och tid då hello säkerhetskopior skapades. 
* **Typen** – säkerhetskopior kan lokala ögonblicksbilder eller molnbaserade ögonblicksbilder. En lokal ögonblicksbild är en säkerhetskopia av din volymdata som lagras lokalt på hello enheten. En ögonblicksbild i molnet refererar toohello säkerhetskopiering av volymdata som finns i hello molnet. Lokala ögonblicksbilder ger snabbare åtkomst medan molnögonblicksbilder väljs för dataåterhämtning.
* **Initieras av** – hello säkerhetskopieringar kan initieras automatiskt enligt tooa schema eller manuellt av dig. (Du kan använda en princip för säkerhetskopiering tooschedule säkerhetskopior. Du kan också använda hello **ta säkerhetskopia** alternativet tootake en interaktiv säkerhetskopiering.)

## <a name="how-toorestore-your-storsimple-volume-from-a-backup"></a>Hur toorestore StorSimple-volym från en säkerhetskopia
Du kan använda hello **säkerhetskopieringskatalogen** sidan toorestore StorSimple-volym från en specifik säkerhetskopia. Men kom ihåg att återställa en volym återgår hello volym toohello tillstånd den hade när hello säkerhetskopian skapades. Alla data som lagts till efter hello säkerhetskopieringen går förlorade.

> [!WARNING]
> Återställa från en säkerhetskopia ersätter hello befintliga volymer från hello säkerhetskopia. Detta kan orsaka hello förlust av alla data som har skrivits när hello säkerhetskopian skapades.
> 
> 

### <a name="toorestore-your-volume"></a>toorestore volymen
1. På hello StorSimple Manager-tjänsten klickar du på hello **säkerhetskopieringskatalog** fliken.
   
    ![Säkerhetskopieringskatalogen](./media/storsimple-restore-from-backup-set-u2/restore.png)
2. Välj en säkerhetskopia på följande sätt:
   
   1. Välj lämplig hello-enhet.
   2. Välj hello volym eller säkerhetskopiering princip för säkerhetskopiering hello tooselect gärna i hello nedrullningsbara listan.
   3. Ange hello tidsintervall.
   4. Klicka på kryssikonen hello ![kryssikon](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png) tooexecute den här frågan.
      
      hello ska säkerhetskopior som är associerade med principen för hello markerad volym eller säkerhetskopiering visas i hello lista över säkerhetskopior.
3. Expandera hello säkerhetskopia tooview hello associerade volymer. Dessa volymer måste vara offline på hello värd och enheten innan du kan återställa dem. Komma åt hello volymer på hello **Volymbehållare** , och följ hello stegen i [kopplar från en volym](storsimple-manage-volumes-u2.md#take-a-volume-offline) tootake dem offline.
   
   > [!IMPORTANT]
   > Kontrollera att du har vidtagit hello volymer offline på värden för hello först innan du utför hello volymer på hello enhet. Om du inte vidtar hello volymer offline på värden för hello leda potentiellt toodata skadas.
   > 
   > 
4. Gå tillbaka toohello **säkerhetskopieringskatalogen** fliken och markera en säkerhetskopia.
5. Klicka på **återställa** på hello hello sidans nederkant.
6. Du uppmanas att bekräfta. Granska hello Återställ information och markera sedan kryssrutan för hello bekräftelse.
   
    ![Bekräftelsesida](./media/storsimple-restore-from-backup-set-u2/ConfirmRestore.png)
7. Klicka på kryssikonen hello ![kryssikon](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png). En återställningsjobbet börjar. Du kan visa hello jobbet genom att öppna hello **jobb** sidan. 
8. När hello återställningen är slutförd, kan du kontrollera att hello innehållet i volymerna som ersätts av volymer från hello säkerhetskopia.

![Video tillgänglig](./media/storsimple-restore-from-backup-set-u2/Video_icon.png) **Video tillgänglig**

toowatch en video som visar hur du kan använda hello klona och återställa funktioner i StorSimple toorecover borttagna filer, klickar du på [här](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="if-hello-restore-fails"></a>Om hello återställa misslyckas
Du får en avisering om hello återställer åtgärden misslyckas av någon anledning. Om detta inträffar kan är uppdatera hello säkerhetskopia av listan tooverify hello säkerhetskopiering fortfarande giltig. Om hello säkerhetskopierade är giltig och att du återställer från hello molnet, sedan bero anslutningsproblem hello problem. 

toocomplete hello återställningsåtgärden, ta hello volymen offline på hello värden och försök hello återställningen igen. Alla ändringar toohello volymdata som utfördes under återställningsprocessen hello går förlorade.

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[hantera StorSimple-volymer](storsimple-manage-volumes-u2.md).
* Lär dig hur för[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

