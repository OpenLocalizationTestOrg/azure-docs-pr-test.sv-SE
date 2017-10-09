---
title: aaaStorSimple 8000-serien uppdatering 2.2 viktig information | Microsoft Docs
description: "Beskriver nya funktioner för hello, problem och lösningar för StorSimple 8000 Series uppdatering 2.2."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 5cf03ea8-2a0f-4552-b6dc-7ea517783d7b
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/18/2016
ms.author: alkohli
ms.openlocfilehash: a2902126f4011fa9ade64f63a8abdec095874fb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-update-22-release-notes"></a>StorSimple 8000 Series uppdatering 2.2 viktig information
## <a name="overview"></a>Översikt
hello följande viktig information beskrivs hello nya funktioner och identifiera hello kritiska öppna problem för StorSimple 8000 Series uppdatering 2.2. De innehåller också en lista över hello StorSimple programuppdateringar som ingår i den här versionen. 

Uppdatera 2.2 kan vara tillämpade tooany StorSimple-enhet som kör versionen (GA) eller uppdatering 0.1 via uppdatering 2.1. hello enheten version som är associerad med uppdatering 2.2 är 6.3.9600.17708.

Granska hello informationen i hello release notes innan du distribuerar hello uppdatera i din StorSimple-lösning.

> [!IMPORTANT]
> * 2.2 har endast programuppdateringar. Det tar cirka 1,5-2 timmar tooinstall uppdateringen. 
> * Om du kör uppdatering 2.1, rekommenderar vi att du installerar uppdateringen 2.2 så snart som möjligt.
> * För nya versioner, kan du inte se uppdateringar omedelbart eftersom vi gör en stegvis distribution av hello uppdateringar. Vänta några dagar och sedan söka efter uppdateringar igen som dessa är snart tillgängligt.
> 
> 

## <a name="whats-new-in-update-22"></a>Vad är nytt i uppdateringen 2.2
hello har följande viktiga förbättringar gjorts i uppdateringen 2.2.

* **Automatisk optimering för frigöring av utrymme** – när data tas bort på tunt allokerade volymer hello oanvända lagring Adressblock måste toobe frigöras. Den här versionen har förbättrad hello utrymme frigöring processen från hello moln, vilket resulterar i hello oanvända utrymme blir tillgängliga snabbare som jämfört med toohello tidigare versioner.
* **Ögonblicksbild prestandaförbättringar** – uppdatering 2.2 har förbättrad hello tid tooprocess en ögonblicksbild i molnet i vissa scenarier där stora volymer som används och det finns minimalt toono dataomsättningen. Ett scenario som vill dra nytta av den här förbättringen skulle vara hello Arkiv volymer.
* **Härdning av supportpaket samla in** – det har gjorts förbättringar i hello sätt hello supportpaket samlas in och överföra i den här versionen. 
* **Uppdatera förbättrad tillförlitlighet** – den här versionen har felkorrigeringar som resulterar i en förbättrad tillförlitlighet för uppdateringen.

## <a name="issues-fixed-in-update-22"></a>Problem som åtgärdas i uppdateringen 2.2
hello följande tabeller innehåller en översikt över problem som har korrigerats i uppdateringar 2.2 och 2.1.    

| Nej | Funktion | Problem | Gäller toophysical enhet | Gäller toovirtual enhet |
| --- | --- | --- | --- | --- |
| 1 |Värd-prestanda |I hello versionen tidigare, värden på klientsidan prestandaproblem observerades under hello skapandet av en lokalt Fäst volym och under hello konverteringen av en nivåindelad volym tooa lokalt Fäst volym. De här problemen har lösts i den här versionen vilket ledde till ett prestandaförbättring hello värden under hello volym konvertering och skapa procedurer. |Ja |Nej |
| 2 |Lokalt fästa volymer |I sällsynta fall skulle hello system krascha när du skapar en lokalt Fäst volym. Det här felet har korrigerats i den här versionen. |Ja |Nej |
| 3 |Skiktning |Det fanns sporadiska kraschar när hello metadata för hello StorSimple moln installationer (8010 och 8020) nivåer för hello molnet. Det här problemet löses i den här versionen. |Nej |Ja |
| 4 |Skapa en ögonblicksbild |Det fanns problem relaterade toohello inkrementell att ögonblicksbilder skapas i scenarier med stora volymer och minimal toono dataomsättningen. De här problemen har lösts i den här versionen. |Ja |Ja |
| 5 |Openstack autentisering |När du använder Openstack som hello molntjänstleverantör körs hello användaren i ett ovanligt fel relaterade toohello authentication där hello JSON-parsern resulterade i en krasch. Det här felet är åtgärdat i den här versionen. |Ja |Nej |
| 6 |Kopiera värden på klientsidan |I tidigare versioner av programvaran relaterade ett ovanligt fel toohello ODX tidsinställning påträffades vid kopiering av hello data från en volym tooanother volym. Detta leder till en domänkontrollant och växling vid fel och hello system kan potentiellt gå i återställningsläge. Det här felet är åtgärdat i den här versionen. |Ja |Nej |
| 7 |Windows Management Instrumentation (WMI) |I hello tidigare versioner av programvaran har flera instanser av web proxy misslyckades med undantaget hello ”<ManagementException> providern kunde inte läsas in”. Det här felet var attributet tooa WMI-minnesläcka och nu har åtgärdats. |Ja |Nej |
| 8 |Uppdatering |I vissa sällsynta fall kan i hello tidigare versioner av programvaran, hello användare tar emot en ”CisPowershellHcsscripterror” när du försöker tooscan eller installera uppdateringar. Det här problemet löses i den här versionen. |Ja |Ja |
| 9 |Support-paket |I den här versionen har förbättringar toohello sätt hello supportpaket samlas in och överföra. |Ja |Ja |

## <a name="known-issues-in-update-22"></a>Kända problem i uppdatering 2.2
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

## <a name="controller-and-firmware-updates-in-update-22"></a>Domänkontrollanten och firmware-uppdateringar i uppdateringen 2.2
Den här versionen har endast är programvarubaserad uppdateringar. Men om du uppdaterar från en tidigare version tooUpdate 2 måste tooinstall drivrutin, Storport, Spaceport, och (i vissa fall) disk uppdateringar av inbyggd programvara på din enhet.

Mer information om hur tooinstall hello drivrutin, Storport, Spaceport och uppdateringar av inbyggd disk, se [installera uppdateringen 2.2](storsimple-install-update-21.md) på StorSimple-enheten.

## <a name="virtual-device-updates-in-update-22"></a>Virtuell enhetsuppdateringar i uppdateringen 2.2
Den här uppdateringen kan inte vara tillämpade toohello virtuell enhet. Nya virtuella enheter behöver toobe skapas. 

## <a name="next-step"></a>Nästa steg
Lär dig hur för[installera uppdateringen 2.2](storsimple-install-update-21.md) på StorSimple-enheten.

