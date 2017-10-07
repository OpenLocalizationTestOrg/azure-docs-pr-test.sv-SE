---
title: aaaStorSimple 8000-serien uppdatering 3 viktig information | Microsoft Docs
description: "Beskriver nya funktioner för hello, problem och lösningar för StorSimple 8000 Series uppdatering 3."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 2158aa7a-4ac3-42ba-8796-610d1adb984d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5bfcba61f7f210531437f650eafaad4dbd8c7c45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="update-3-release-notes-for-your-storsimple-8000-series-device"></a>Uppdatera 3 viktig information för enheten StorSimple 8000-serien

## <a name="overview"></a>Översikt
hello följande viktig information beskrivs hello nya funktioner och identifiera hello kritiska öppna problem för StorSimple 8000 Series uppdatering 3. De innehåller också en lista över hello StorSimple programuppdateringar som ingår i den här versionen. 

Uppdatering 3 kan vara tillämpade tooany StorSimple-enhet som kör versionen (GA) eller uppdatering 0.1 via uppdatering 2.2. hello enheten version som är associerad med uppdatering 3 är 6.3.9600.17759.

Granska hello informationen i hello release notes innan du distribuerar hello uppdatera i din StorSimple-lösning.

> [!IMPORTANT]
> * Update 3 har enhetsprogrammet, LSI drivrutiner och inbyggd programvara och Storport och Spaceport uppdateras. Det tar cirka 1,5-2 timmar tooinstall uppdateringen. 
> * För nya versioner, kan du inte se uppdateringar omedelbart eftersom vi gör en stegvis distribution av hello uppdateringar. Vänta några dagar och sedan söka efter uppdateringar igen som dessa är snart tillgängligt.
> 
> 

## <a name="whats-new-in-update-3"></a>Vad är nytt i uppdatering 3
hello har följande viktiga förbättringar och felkorrigeringar gjorts i uppdatering 3.

* **Automatisk frigöring adressutrymmesändringarna** – startar uppdatering 3 hello utrymme återvinning av algoritmer som kör på vänteläge hello av hello system, vilket resulterar i snabbare körning. Mer information om hello-portar som är nödvändiga toowork lagringsfrigöring finns toohello [StorSimple nätverkskrav](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
* **Prestandaförbättringar** – uppdatering 3 har förbättrats Läs-och skrivprestanda toohello moln.
* **Förbättringar av migreringsrelaterade** – i den här versionen flera programfel korrigeringar och förbättringar som gjordes för hello migrering funktion från 5000/7000-serien enheter too8000 serien. Mer information om hur toouse hello migrering funktionen Gå för[migrering från 5000/7000-serien enheten too8000 serieenhet](https://gallery.technet.microsoft.com/Azure-StorSimple-50007000-c1a0460b). 
* **Övervaka relaterade korrigeringar** – i den här versionen buggar relaterade toomonitoring diagram, service instrumentpanelen och enheten instrumentpanelen har lösts.

## <a name="issues-fixed-in-update-3"></a>Problem som åtgärdas i uppdatering 3
hello följande tabeller innehåller en översikt över problem som har åtgärdats i uppdatering 3.    

| Nej | Funktion | Problem | Gäller toophysical enhet | Gäller toovirtual enhet |
| --- | --- | --- | --- | --- |
| 1 |Värden på klientsidan datamigrering |I hello tidigare version, hello StorSimple-enhet för molnet försätts i offlineläge under en värd på klientsidan datamigreringen. Det här problemet löses i den här versionen. |Nej |Ja |
| 2 |Lokalt fästa volymer |I hello tidigare version fanns problem relaterade tooI i/O-fel och volymen Konverteringsfel datapath fel för lokalt fästa volymer. De här problemen var roten orsakade och fast i den här versionen. |Ja |Nej |
| 3 |Övervakning |Det fanns flera problem relaterade tooreporting enheter och övervaka samt enheten instrumentpanelen diagram där felaktig information visades för lokalt fästa volymer. De här problemen har lösts i den här versionen. |Ja |Nej |
| 4 |Tunga skrivningar i/o |När du använder StorSimple för arbetsbelastningar som involverar tunga skrivningar körs hello användaren i ett ovanligt fel där hello arbetsminne har som nivåer i hello moln. Det här felet är åtgärdat i den här versionen. |Ja |Ja |
| 5 |Säkerhetskopiering |I vissa sällsynta fall kan i hello tidigare versioner av programvaran, när användaren har gjort en säkerhetskopia av en fjärransluten klon de körs i Molnfel och hello åtgärden skulle fel ut. I den här versionen hello problemet har lösts och hello-åtgärden har slutförts. |Ja |Ja |
| 6 |Princip för säkerhetskopiering |I vissa sällsynta fall i hello tidigare versioner av programvara, det uppstod ett fel relaterade toohello borttagning av princip för säkerhetskopiering. Det här problemet löses i den här versionen. |Ja |Ja |

## <a name="known-issues-in-update-3"></a>Kända problem i uppdatering 3
hello följande tabell innehåller en översikt över kända problem i den här versionen.

| Nej. | Funktion | Problem | Kommentarer / lösning | Gäller toophysical enhet | Gäller toovirtual enhet |
| --- | --- | --- | --- | --- | --- |
| 1 |Disk kvorum |I sällsynta fall om hello merparten av diskar i hello EBOD hölje av 8600-enheten är frånkopplad ledde till Ingen disk i kvorum kommer hello lagringspoolen går offline. Den förblir offline även om hello diskar återansluts. |Du behöver tooreboot hello enhet. Hello problemet kvarstår kontaktar du Microsoft Support för nästa steg. |Ja |Nej |
| 2 |Felaktig styrenhets-ID |När en domänkontrollant ersättning utförs kan styrenhet 0 visas som kontrollant 1. Under controller ersättning, när hello bild läses in från hello peer-noden kan hello styrenhets-ID visas först som domänkontrollant hello-peer-ID. I sällsynta fall kan problemet visas efter en omstart. |Ingen användaråtgärd krävs. Den här situationen ska lösas av sig självt när hello controller ersättning är klar. |Ja |Nej |
| 3 |Lagringskonton |Använda hello Storage service toodelete hello storage-konto är ett scenario som inte stöds. Detta leder tooa situation där användardata inte kan hämtas. | |Ja |Ja |
| 4 |Enheten växling vid fel |Flera redundans av en volymbehållare från hello samma källa enheten toodifferent målenheterna inte stöds. Växling vid fel från en enskild döda enhet toomultiple enheter gör hello volymbehållare på hello först redundansväxlats enhet förlorar dataägarskap. Efter en växling, ska behållarna volymen visas eller fungera annorlunda när de visas i hello klassiska Azure-portalen. | |Ja |Nej |
| 5 |Installation |Under StorSimple-kort för SharePoint-installation måste tooprovide en enhet IP-adress för hello installera toofinish har. | |Ja |Nej |
| 6 |Webbproxy |Om din webbproxykonfigurationen har HTTPS enligt hello-protokollet och sedan enhet-till-tjänst-kommunikation kommer att påverkas och hello enheten går offline. Stöd för paket skapas också i hello process förbrukar betydande resurser på enheten. |Se till att hello web proxy URL har HTTP som hello angivet protokoll. Mer information finns för[konfigurera webbproxy för din enhet](storsimple-configure-web-proxy.md). |Ja |Nej |
| 7 |Webbproxy |Om du konfigurerar och aktiverar webbproxy på en registrerad enhet måste toorestart hello aktiva styrenheten på enheten. | |Ja |Nej |
| 8 |Hög molnet latens och hög i/o-arbetsbelastning |När din StorSimple-enhet påträffar en kombination av mycket hög molnet latens (ordning sekunder) och höga i/o-arbetsbelastning, hello enheten volymer som ingår i ett degraderat tillstånd och hello I/o kan misslyckas med felet ”enheten är inte klar”. |Du behöver toomanually omstart hello styrenheter eller utför en växling vid fel toorecover för enheten från den här situationen. |Ja |Nej |
| 9 |Azure PowerShell |När du använder hello StorSimple cmdlet **Get-AzureStorSimpleStorageAccountCredential &#124; Select-Object - först 1 - vänta** tooselect hello första objekt så att du kan skapa en ny **VolumeContainer** objekt hello cmdlet returnerar alla hello-objekt. |Omsluta hello cmdlet inom parentes på följande sätt: **(Get-Azure-StorSimpleStorageAccountCredential) &#124; Select-Object - först 1 - vänta** |Ja |Ja |
| 10 |Migrering |När flera volymbehållare skickas för migrering är hello ETA för senaste säkerhetskopian exakt endast för hello första volymbehållare. Dessutom startar parallella migreringen när hello första 4 säkerhetskopieringar i hello första volymbehållare har migrerats. |Vi rekommenderar att du migrerar en volymbehållare i taget. |Ja |Nej |
| 11 |Migrering |Volymer läggs inte toohello säkerhetskopiering principen eller hello virtuella diskgruppen efter hello återställning. |Du behöver tooadd dessa volymer tooa säkerhetskopieringsprincip ordning toocreate säkerhetskopior. |Ja |Ja |
| 12 |Migrering |När hello migreringen är klar, hello 5000/7000-serien enheten måste inte att komma åt hello migrerade databehållare. |Vi rekommenderar att du tar bort hello migrerade databehållare när hello migreringen är klar och bekräftats. |Ja |Nej |
| 13 |Klona och Katastrofåterställning |En StorSimple-enhet som kör uppdatering 1 kan inte klona eller utföra disaster recovery tooa enheter före uppdateringen 1 programvaran som körs. |Du behöver tooupdate hello mål enheten tooUpdate 1 tooallow dessa åtgärder |Ja |Ja |
| 14 |Migrering |Konfigurationssäkerhetskopia för migrering kan misslyckas på en serieenhet för 5000 7000-när volymen grupper med inga associerade volymer. |Ta bort alla hello tom volym grupper med inga associerade volymer och försök sedan hello konfigurationssäkerhetskopia. |Ja |Nej |
| 15 |Azure PowerShell-cmdlets och lokalt fästa volymer |Du kan inte skapa en lokalt Fäst volym via Azure PowerShell-cmdlets. (Alla volymer som du skapar via Azure PowerShell kommer att skikt.) |Använd alltid hello StorSimple Manager-tjänsten tooconfigure lokalt fästa volymer. |Ja |Nej |
| 16 |Diskutrymme för lokalt fästa volymer |Om du tar bort en lokalt Fäst volym uppdateras hello diskutrymme för nya volymer inte omedelbart. Hej StorSimple Manager-tjänstuppdateringar hello lokalt tillgängligt utrymme cirka varje timme. |Vänta tills en timme innan du försöker toocreate hello ny volym. |Ja |Nej |
| 17 |Lokalt fästa volymer |Din återställningsjobbet visar hello tillfälliga ögonblicksbilden säkerhetskopiering i hello säkerhetskopieringskatalogen, men endast för hello återställningsjobbet hello varaktighet. Dessutom kan det visar att en virtuell diskgrupp med prefix **tmpCollection** på hello **Säkerhetskopieringsprinciper** sidan, men endast för hello varaktighet hello återställningsjobbet. |Detta kan inträffa om din Återställningsjobbet har bara lokalt Fäst volymer eller en blandning av lokalt Fäst och nivåindelade volymer. Om hello återställningsjobbet innehåller endast nivåindelade volymer, sker inte det här beteendet. Inga användaråtgärder krävs. |Ja |Nej |
| 18 |Lokalt fästa volymer |Om du avbryter en återställningsjobbet och en domänkontrollant växling sker omedelbart efteråt hello återställningsjobbet visas **misslyckades** i stället för **avbruten**. Om en återställningsjobbet misslyckas och det uppstår redundans controller omedelbart efteråt hello återställningsjobbet visas **avbruten** i stället för **misslyckades**. |Detta kan inträffa om din Återställningsjobbet har bara lokalt Fäst volymer eller en blandning av lokalt Fäst och nivåindelade volymer. Om hello återställningsjobbet innehåller endast nivåindelade volymer, sker inte det här beteendet. Inga användaråtgärder krävs. |Ja |Nej |
| 19 |Lokalt fästa volymer |Om du avbryter en återställningsjobbet eller om en återställning misslyckas och sedan en domänkontrollant växling vid fel, en ytterligare återställningsjobbet visas på hello **jobb** sidan. |Detta kan inträffa om din Återställningsjobbet har bara lokalt Fäst volymer eller en blandning av lokalt Fäst och nivåindelade volymer. Om hello återställningsjobbet innehåller endast nivåindelade volymer, sker inte det här beteendet. Inga användaråtgärder krävs. |Ja |Nej |
| 20 |Lokalt fästa volymer |Om du försöker tooconvert en nivåindelad volym (skapats och klonade med Update 1.2 eller tidigare) tooa lokalt Fäst volym och enheten få slut på utrymme eller det finns ett avbrott i molnet och sedan hello clone(s) kan vara skadad. |Det här problemet uppstår bara med volymer som har skapats och klonade med före uppdateringen 2.1 programvara. Detta bör vara ett ovanligt scenario. | | |
| 21 |Volymkonvertering |Uppdatera inte hello ACRs bifogade tooa volym när en Volymkonvertering pågår (nivåindelade toolocally Fäst eller vice versa). Uppdatera hello ACRs kan resultera i skadade data. |Om det behövs, uppdatera hello ACRs tidigare toohello Volymkonvertering och gör inte ytterligare uppdateringar ACR medan hello konvertering pågår. | | |
| 22 |Uppdateringar |När du använder uppdatering 3, hello **Underhåll** sida i hello Azure klassiska portal kommer visa hello följande meddelande relaterade tooUpdate 2 - ”StorSimple 8000-serien uppdatering 2 innehåller hello möjligheten för Microsoft tooproactively samla in logga information från din enhet när vi identifiera potentiella problem ”. Detta vilseledande eftersom den anger enheten hello uppdaterade tooUpdate 2. När hello enhet har uppdaterats succeesfully tooUpdate 3, försvinner meddelandet. |Problemet korrigeras i en framtida version. |Ja |Nej |

## <a name="controller-and-firmware-updates-in-update-3"></a>Domänkontrollanten och firmware-uppdateringar i uppdatering 3
Den här versionen har LSI drivrutinen och firmware-uppdateringar. Mer information om hur tooinstall hello LSI drivrutinen och uppdateringar av inbyggd programvara, se [installera uppdatering 3](storsimple-install-update-3.md) på StorSimple-enheten.

## <a name="virtual-device-updates-in-update-3"></a>Uppdatering av virtuella enheter i uppdatering 3
Den här uppdateringen kan inte vara tillämpade toohello StorSimple moln installation (även kallat hello virtuell enhet). Nya virtuella enheter behöver toobe skapas. 

## <a name="next-step"></a>Nästa steg
Lär dig hur för[installera uppdatering 3](storsimple-install-update-3.md) på StorSimple-enheten.

