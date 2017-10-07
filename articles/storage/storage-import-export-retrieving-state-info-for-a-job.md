---
title: "information om aaaRetrieving tillstånd för ett Azure Import/Export-jobb | Microsoft Docs"
description: "Lär dig hur tooobtain statusinformation för Microsoft Azure Import/Export service jobb."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 22d7e5f0-94da-49b4-a1ac-dd4c14a423c2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: muralikk
ms.openlocfilehash: cbc35660519573d92f641924ac0025c9e577d69b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="retrieving-state-information-for-an-importexport-job"></a>Hämtar information om tillstånd för en Import/Export-jobb
Du kan anropa hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) åtgärden tooretrieve information om både importera och exportera jobben. hello informationen som returneras innehåller:

-   hello aktuell status för hello jobb.

-   hello ungefärliga procentandel varje jobb har slutförts.

-   hello aktuell status för varje enhet.

-   URI: er för blob som innehåller fel och utförlig loggningsinformation (om aktiverat).

hello följande avsnitt beskrivs hello information som returnerades av hello `Get Job` igen.

## <a name="job-states"></a>Status för jobb
hello-tabellen och hello tillstånd diagrammet nedan beskriver hello tillstånd för ett jobb övergångar via under sin livslängd. hello aktuell status för jobbet hello kan fastställas genom att anropa hello `Get Job` igen.

![JobStates](./media/storage-import-export-retrieving-state-info-for-a-job/JobStates.png "JobStates")

hello beskrivs följande tabell varje tillstånd som ett jobb kan passera.

|Jobbets status|Beskrivning|
|---------------|-----------------|
|`Creating`|När du anropar hello placera Job-åtgärden, skapas ett jobb och dess status är inställd för`Creating`. Medan hello jobb i hello `Creating` tillstånd hello Import/Export service förutsätter hello enheter inte har levererade toohello datacenter. Ett jobb kan finnas kvar i hello `Creating` tillstånd för in tootwo veckor, efter vilken det automatiskt bort av hello-tjänsten.<br /><br /> Om du anropar hello Update jobbegenskaper åtgärden medan hello jobb i hello `Creating` tillstånd hello jobb finns kvar i hello `Creating` tillstånd och hello timeout är Återställ tootwo veckor.|
|`Shipping`|När du levererar paketet ska för att anropa hello uppdatera jobbegenskaper åtgärden hello tillstånd för jobb hello`Shipping`. hello leverans tillstånd kan bara anges om hello `DeliveryPackage` (postnummer operatör och spårningsnummer) och hello `ReturnAddress` egenskaper har angetts för hello jobb.<br /><br /> hello jobb finns kvar i hello leverans tillstånd för in tootwo veckor. Om två veckor godkänts och hello enheter har inte tagits emot, meddelas hello Import/Export service operatörer.|
|`Received`|När alla enheter har tagits emot på hello datacenter, ange hello jobbets status toohello mottagna tillstånd.|
|`Transferring`|När hello enheter har tagits emot på hello datacenter och minst en enhet har startat bearbetning, hello jobbets status anges vara toohello `Transferring` tillstånd. Se hello `Drive States` avsnittet nedan för mer information.|
|`Packaging`|När alla enheter har bearbetat hello-jobbet kommer att placeras i hello `Packaging` tillstånd tills hello enheter är levererade tillbaka toohello kund.|
|`Completed`|När alla enheter har levererade tillbaka toohello kunden om hello jobbet har slutförts utan fel, sedan hello jobb ställs in toohello `Completed` tillstånd. hello jobbet tas automatiskt bort efter 90 dagar i hello `Completed` tillstånd.|
|`Closed`|När alla enheter har levererade tillbaka toohello kund, om det har gjorts några fel under hello bearbetningen av hello jobb, sedan hello jobb ställs in toohello `Closed` tillstånd. hello jobbet tas automatiskt bort efter 90 dagar i hello `Closed` tillstånd.|

Du kan avbryta ett jobb endast på vissa tillstånd. Avbrutna jobb hoppar över hello kopiera steget, men annars följer hello samma tillstånd övergångar som ett jobb som inte har avbrutits.

hello beskrivs följande tabell fel som kan uppstå för varje jobbets status, samt hello effekt på hello jobb när ett fel uppstår.

|Jobbets status|Händelse|Lösning / nästa steg|
|---------------|-----------|------------------------------|
|`Creating or Undefined`|En eller flera enheter för ett jobb anlänt, men hello jobbet är inte i hello `Shipping` tillstånd eller det finns ingen post för hello jobbet i hello-tjänsten.|hello Import/Export service operations-teamet kommer försöka toocontact hello kunden toocreate eller uppdatera hello jobb med nödvändig information toomove hello jobbet framåt.<br /><br /> Om hello driftteamet är toocontact hello kunden inom två veckor, försöker hello operations-teamet tooreturn hello enheter.<br /><br /> I hello-händelse som hello enheter inte kan returneras och går inte att nå hello kunden hello enheter på ett säkert sätt att förstöras 90 dagar.<br /><br /> Observera att ett jobb inte kan behandlas förrän dess tillstånd uppdateras för`Shipping`.|
|`Shipping`|hello-paket för ett jobb har inte kommit över två veckor.|Hej driftteamet meddelar hello kunden hello saknas paket. Hej driftteamet kommer baserat på svar från hello kunden antingen utöka hello intervall toowait för hello paketet tooarrive eller hello avbryts.<br /><br /> I händelse av hello hello kunden inte kan kontaktas eller inte svarar inom 30 dagar hello driftteamet initierar åtgärd toomove hello jobbet från hello `Shipping` tillstånd direkt toohello `Closed` tillstånd.|
|`Completed/Closed`|hello enheter aldrig uppnåtts hello avsändaradress eller har skadats i leveransen (gäller endast tooan exportjobb).|Om hello enheter inte når hello avsändaradress, hello kunden ska första anropet hello Get Job-åtgärden eller kontrollera hello Jobbstatus i hello portal tooensure som hello enheter som har levererats. Om hello enheter har levererats, ska sedan hello kunden kontakta hello leverans providern tootry och hitta hello-enheter.<br /><br /> Om hello enheter skadas under transport vill hello-kund toorequest en annan exportjobb eller hämta hello saknas blobbar.|
|`Transferring/Packaging`|Jobbet har en felaktig eller saknas avsändaradress.|Hej driftteamet når ut toohello kontaktperson för hello jobbet tooobtain hello rätt adress.<br /><br /> I händelse av hello kan hello kunden inte nås, hello förstöras enheter på ett säkert sätt i 90 dagar.|
|`Creating / Shipping/ Transferring`|En enhet som inte visas i hello lista över enheter toobe importeras ingår i hello leverans paketet.|hello extra enheterna kommer inte att bearbetas och kommer att returneras toohello Kund när hello jobbet har slutförts.|

## <a name="drive-states"></a>Tillstånd för enheten
hello tabell och hello diagrammet nedan beskriver hello livscykeln för en enskild enhet som den övergår till ett jobb som importeras eller exporteras. Du kan hämta hello aktuell status för enheten genom att anropa hello `Get Job` igen och inspektera hello `State` element av hello `DriveList` egenskapen.

![DriveStates](./media/storage-import-export-retrieving-state-info-for-a-job/DriveStates.png "DriveStates")

hello beskrivs följande tabell varje tillstånd som en enhet kan passera.

|Enhetsstatus|Beskrivning|
|-----------------|-----------------|
|`Specified`|För ett importjobb när hello jobb skapas med hello placera Job-åtgärden hello ursprungligt tillstånd för en enhet är hello `Specified` tillstånd. För ett exportjobb eftersom ingen enhet anges när hello jobb skapas hello inledande Enhetsstatus är hello `Received` tillstånd.|
|`Received`|hello enhet övergångar toohello `Received` tillstånd när hello Import/Export service operatorn har bearbetat hello-enheter som har tagits emot från hello leverans företag för importen. För ett exportjobb hello inledande Enhetsstatus är hello `Received` tillstånd.|
|`NeverReceived`|hello enhet flyttas toohello `NeverReceived` tillstånd när hello-paket för ett jobb kommer, men hello paketet innehåller inte hello enhet. En enhet kan också flytta detta tillstånd om det har två veckor eftersom hello-tjänsten har tagit emot hello leveransinformation men hello paketet har inte kommit på hello datacenter.|
|`Transferring`|En enhet flyttas toohello `Transferring` tillstånd när hello service börjar tootransfer data från hello enhet tooWindows Azure Storage.|
|`Completed`|En enhet flyttas toohello `Completed` tillstånd när hello-tjänsten har överförts alla hello data utan fel.|
|`CompletedMoreInfo`|En enhet flyttas toohello `CompletedMoreInfo` tillstånd när hello tjänsten har påträffat några problem vid kopiering av data från eller toohello enhet. hello informationen kan innehålla fel, varningar och informationsmeddelanden om överskrivning blobbar.|
|`ShippedBack`|hello enhet flyttas toohello `ShippedBack` tillstånd när den har levererats från hello data center tillbaka toohello avsändaradress.|

hello följande tabell beskrivs hello enhet fel tillstånd och hello-åtgärder som vidtas för varje tillstånd.

|Enhetsstatus|Händelse|Lösning / nästa steg|
|-----------------|-----------|-----------------------------|
|`NeverReceived`|En enhet som har markerats som `NeverReceived` (eftersom den inte skickades som en del av hello jobbet leveransen) tas emot i en annan leveransen.|Hej driftteamet flyttas hello enhet toohello `Received` tillstånd.|
|`N/A`|En enhet som inte är en del av jobb som når hello datacenter som en del av ett annat jobb.|hello enheten kommer att markeras som en extra enhet och returneras toohello Kund när hello jobb som är associerade med hello ursprungliga paketet har slutförts.|

## <a name="faulted-states"></a>Felaktig tillstånd
När ett jobb eller en enhet inte tooprogress normalt via dess förväntade livscykel, hello jobb eller enheten kommer att flyttas till en `Faulted` tillstånd. Vid den punkten kontaktar hello driftteamet hello kunden via e-post eller telefon. När hello problemet är löst hello fel jobb eller enheten kommer att tas bort från hello `Faulted` Systemtillstånd och har flyttats till hello lämpliga tillstånd.

## <a name="next-steps"></a>Nästa steg

* [Med hjälp av hello Import/Export service REST API](storage-import-export-using-the-rest-api.md)
