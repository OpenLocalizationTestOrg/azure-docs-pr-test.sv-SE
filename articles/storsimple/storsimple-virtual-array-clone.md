---
title: "aaaClone en säkerhetskopiering av virtuella StorSimple-matris | Microsoft Docs"
description: "Lär dig hur tooclone en säkerhetskopiering och återställning av en fil från din virtuella StorSimple-matris."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: af6e979c-55e3-477c-b53e-a76a697f80c9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2016
ms.author: alkohli
ms.openlocfilehash: 21bfcae48ee07762179cf00ce842b6094abe18ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="clone-from-a-backup-of-your-storsimple-virtual-array"></a>Klona från en säkerhetskopia av din virtuella StorSimple-matris

## <a name="overview"></a>Översikt

Den här artikeln beskriver steg för steg hur tooclone en säkerhetskopia av dina resurser eller volymer på Microsoft Azure StorSimple virtuell matrisen. hello klonade säkerhetskopiering är toorecover används en fil som tagits bort eller förlorade. hello artikeln innehåller detaljerade anvisningar tooperform en återställning på objektnivå på din virtuella StorSimple-matrisen konfigurerad som en filserver.

## <a name="clone-shares-from-a-backup-set"></a>Klona resurser från en säkerhetskopia

**Se till att du har tillräckligt med utrymme på hello enheten toocomplete åtgärden innan du försöker tooclone resurser.** tooclone från en säkerhetskopia i hello [Azure-portalen](https://portal.azure.com/), utför följande steg hello.

#### <a name="tooclone-a-share"></a>tooclone en resurs

1. Bläddra för**enheter** bladet. Välj och klicka på enheten och klicka sedan på **resurser**. Välj hello resursen som du vill tooclone hello resursen tooinvoke hello snabbmenyn. Välj **klona**.
   
   ![Klona en säkerhetskopia](./media/storsimple-virtual-array-clone/cloneshare1.png)
2. I hello **klona** bladet, klickar du på **säkerhetskopiering > Välj** och sedan hello följande: 
   
   a.    Filtrera en säkerhetskopiering på den här enheten baserat på hello tidsintervall. Du kan välja mellan **senaste 7 dagarna**, **de senaste 30 dagarna**, och **senaste året**.
   
   b.    I hello filtrerade säkerhetskopieringar som visas väljer du en säkerhetskopiering tooclone från.
   
   c.    Klicka på **OK**.
   
   ![Klona en säkerhetskopia](./media/storsimple-virtual-array-clone/cloneshare3.png)
3. I hello **klona** bladet, klickar du på **mål inställningar** och sedan hello följande:
   
   a.    Ange ett resursnamn. hello resursnamnet får innehålla 3-127 tecken.
   
   b.    Du kan också ange en beskrivning för hello klonade resurs.
   
   c.    Du kan inte ändra hello typ av hello resursen som du återställer till. En skiktad resurs klonas som en skiktad och en lokalt Fäst resurs lokalt Fäst.
   
   d.    hello kapacitet har angetts som lika toohello storlek hello-resursen du klona från.
   
   e.    Tilldela hello administratörer för den här resursen. Du kommer att kunna toomodify hello resursegenskaper via Utforskaren när hello klona är klar.
   
   f.    Klicka på **OK**.
   
   ![Klona en säkerhetskopia](./media/storsimple-virtual-array-clone/cloneshare6.png)

4. Klicka på **klona** toostart ett jobb för klon. När hello jobbet är klart hello kopieringen startar och du får ett meddelande. toomonitor hello förloppet för kloning, gå toohello **jobb** bladet och klicka på hello tooview jobbet jobbinformation.
5. När hello klona har skapats, gå tillbaka toohello **resurser** bladet på enheten.
6. Du kan nu visa hello ny klonade resurs i hello listan över resurser på enheten. En skiktad resurs är klonad eftersom nivåer och en lokalt Fäst resurs som en lokalt Fäst resurs.
   
   ![Klona en säkerhetskopia](./media/storsimple-virtual-array-clone/cloneshare10.png)

## <a name="clone-volumes-from-a-backup-set"></a>Klona volymer från en säkerhetskopia

tooclone från en säkerhetskopia i hello Azure-portalen du har tooperform steg liknande toohello som när du klonar en resurs. åtgärd för kloning av hello klonar hello säkerhetskopiering tooa ny volym på hello samma virtuella enheter; Du kan inte klona tooa annan enhet.

#### <a name="tooclone-a-volume"></a>tooclone en volym

1. Bläddra för**enheter** bladet. Välj och klicka på enheten och klicka sedan på **volymer**. Markera hello volym som du vill tooclone hello volym tooinvoke hello snabbmenyn. Välj **klona**.
   
   ![Klona en volym](./media/storsimple-virtual-array-clone/clonevolume1.png)
2. I hello **klona** bladet, klickar du på **säkerhetskopiering** och sedan hello följande: 
   
   a.    Filtrera en säkerhetskopiering på den här enheten baserat på hello tidsintervall. Du kan välja mellan **senaste 7 dagarna**, **de senaste 30 dagarna**, och **senaste året**. 
   
   b.    I hello filtrerade säkerhetskopieringar som visas väljer du en säkerhetskopiering tooclone från.
   
   c.    Klicka på **OK**.
   
   ![Klona en säkerhetskopia](./media/storsimple-virtual-array-clone/clonevolume3.png)
3. I hello **klona** bladet, klickar du på **mål volyminställningar** och sedan hello följande::
   
   a. hello enhetsnamn fylls i automatiskt.
   
   b. Ange ett volymnamn på för hello **klonas volym**. hello volymnamn måste innehålla 3 too127 tecken.
   
   c. hello volymtyp anges automatiskt toohello ursprungliga volymen. En nivåindelad volym är klonad eftersom nivåer och en lokalt Fäst volym lokalt Fäst.
   
   d. För hello **anslutna värdar**, klickar du på **Välj**.
   
   ![Klona en säkerhetskopia](./media/storsimple-virtual-array-clone/clonevolume4.png)
4. I hello **anslutna värdar** bladet Välj en befintlig ACR eller lägga till en ny ACR. tooadd nya ACR måste tooprovide ACR-namn och värden hello IQN. Klicka på **Välj**.
   
   ![Klona en säkerhetskopia](./media/storsimple-virtual-array-clone/clonevolume5.png)
5. Klicka på **klona** toolaunch ett jobb för klon.
   
   ![Klona en säkerhetskopia](./media/storsimple-virtual-array-clone/clonevolume6.png)  
6. Kloningen startar när hello klona jobbet har skapats. När du har skapat hello klona visas den på hello volymer bladet på enheten. Observera att en nivåindelad volym är klonad eftersom nivåer och en lokalt Fäst volym klonas när en lokalt Fäst volym.
   
   ![Klona en säkerhetskopia](./media/storsimple-virtual-array-clone/clonevolume8.png)
7. När hello-volymen visas online på hello lista över volymer är hello volymen tillgänglig för användning. Uppdatera hello lista med mål i egenskapsfönstret för iSCSI-initieraren på hello iSCSI-initieraren värden. Ett nytt mål som innehåller hello klonade volymnamn ska visas som ”inaktiva” under hello status-kolumnen.
8. Välj hello mål och klicka på **Anslut**. När hello initierare är anslutna toohello mål, hello bör statusen ändras för**ansluten**.
9. I hello **Diskhantering** fönstret, hello monterade volymer visas som visas i följande illustration hello. Högerklicka på hello identifierade volymen (klicka på hello diskens namn) och klicka sedan på **Online**.

> [!IMPORTANT]
> När försök tooclone en volym eller en resurs från en säkerhetskopia om hello klona jobb misslyckas, en målvolym eller resurs kan fortfarande skapas i hello-portalen. Det är viktigt att du tar bort den här målvolymen eller dela i hello portal toominimize eventuella framtida problem som härrör från det här elementet.
> 
> 

## <a name="item-level-recovery-ilr"></a>Återställning på objektnivå (ILR)

Den här versionen introducerar hello av återställning på objektnivå (ILR) på en virtuell StorSimple-matrisen som konfigurerats som en filserver. hello-funktionen kan du toodo detaljerade återställning av filer och mappar från en säkerhetskopiering i molnet över alla resurser som hello på hello StorSimple-enhet. Du kan hämta borttagna filer från de senaste säkerhetskopior med hjälp av en självbetjäningsmodell.

Varje resurs har en *.backups* mapp som innehåller hello senaste säkerhetskopieringar. Du kan navigera toohello önskad säkerhetskopia, kopiera relevanta filer och mappar från hello säkerhetskopia och återställa dem. Den här funktionen eliminerar tooadministrators anrop för att återställa filer från säkerhetskopior.

1. När du utför hello återställning på Objektnivå, kan du visa hello säkerhetskopieringar via Utforskaren. Klicka på specifika hello-resurs som du vill toolook vid hello säkerhetskopieringen för. Du ser en *.backups* mappen skapas under hello-resurs som lagrar alla hello säkerhetskopior. Expandera hello *.backups* mappen tooview hello säkerhetskopieringar. hello mappen visar hello utvidgad hello hela hierarkin för säkerhetskopiering. Den här vyn skapas på begäran och vanligtvis tar endast några sekunder toocreate.
   
   hello sista fem säkerhetskopieringar visas i det här sättet och kan vara används tooperform en artikelnivå återställning. hello fem nya säkerhetskopior ta med både hello standard schemalagda och hello manuella säkerhetskopieringar.
   
   * **Schemalagda säkerhetskopieringar** med samma namn som &lt;enhetsnamn&gt;DailySchedule-ÅÅÅÅMMDD-HHMMSS-UTC.
   * **Manuell säkerhetskopiering** med samma namn som Ad hoc-ÅÅÅÅMMDD-HHMMSS-UTC.
     
     ![](./media/storsimple-virtual-array-clone/image14.png)

2. Identifiera hello säkerhetskopiering med hello den senaste versionen av hello bort filen. Även om hello mappnamnet innehåller en UTC-tidsstämpel i varje hello föregående fall, är hello tid det på vilka hello skapa mappen hello faktisk enhet när hello starta säkerhetskopieringen. Använd hello mappen tidsstämpel toolocate och identifiera hello säkerhetskopieringar.

3. Leta upp mappen hello eller hello-fil som du vill toorestore i hello-säkerhetskopia som du identifierade i föregående steg i hello. Observera att du kan bara visa hello filer eller mappar som du har behörighet för. Kontakta en resurs-administratören om du inte kan komma åt vissa filer eller mappar. Hej administratör kan använda Utforskaren tooedit hello resursbehörigheter och ge dig åtkomst toohello specifik fil eller mapp. Det är rekommenderad praxis som hello resursen administratör är en grupp i stället för en enskild användare.

4. Kopiera hello fil- eller hello mappen toohello lämplig på din StorSimple-filserver.

## <a name="next-steps"></a>Nästa steg

Läs mer om hur för[administrera din virtuella StorSimple-matris med hello lokala webbgränssnittet](storsimple-ova-web-ui-admin.md).

