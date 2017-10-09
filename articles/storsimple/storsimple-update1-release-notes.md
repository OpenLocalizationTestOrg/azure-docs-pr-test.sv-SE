---
title: aaaStorSimple 8000-serien uppdatering 1.2 viktig information | Microsoft Docs
description: "Beskriver nya funktioner för hello, problem och lösningar för StorSimple 8000 Series uppdatering 1.2."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 6c9aae87-6f77-44b8-b7fa-ebbdc9d8517c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7f564b794573fc3302ab15732e8dd85632ab9243
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="update-12-release-notes-for-your-storsimple-8000-series-device"></a>Uppdatera 1.2 viktig information för enheten StorSimple 8000-serien

## <a name="overview"></a>Översikt
hello följande viktig information beskrivs hello nya funktioner och identifiera hello kritiska öppna problem för StorSimple 8000 Series uppdatering 1.2. De innehåller också en lista över hello StorSimple programvara, drivrutiner och uppdateringar av inbyggd disk i den här versionen. 

Uppdatera 1.2 kan vara tillämpade tooany StorSimple-enhet som kör versionen (GA), uppdatering 0.1, uppdatering 0,2 eller uppdatering 0.3 programvara. Uppdatering 1.2 är inte tillgängligt om enheten kör uppdatering 1 eller uppdatering 1.1. Om enheten kör versionen (GA), [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md) tooassist du med att installera uppdateringen.

följande tabell visar hello enheten programvaruversioner motsvarande tooUpdates 1, 1.1 och 1.2 hello.

| Om du kör uppdatering... | Det här är din enhet programvaruversionen. |
| --- | --- |
| Uppdatera 1.2 |6.3.9600.17584 |
| Uppdatera 1.1 |6.3.9600.17521 |
| Uppdatering 1.0 |6.3.9600.17491 |

Granska hello informationen i hello release notes innan du distribuerar hello uppdatera i din StorSimple-lösning. Mer information finns i hur för[installera uppdateringen 1.2 på din StorSimple-enhet](storsimple-install-update-1.md). 

> [!IMPORTANT]
> * Det tar cirka 5-10 timmar tooinstall uppdateringen (inklusive uppdateringar för Windows hello). 
> * 1.2 har programvara, LSI drivrutinen och uppdateringar av inbyggd disk. tooinstall hello anvisningar i [installera uppdateringen 1.2 på din StorSimple-enhet](storsimple-install-update-1.md).
> * För nya versioner, kan du inte se uppdateringar omedelbart eftersom vi gör en stegvis distribution av hello uppdateringar. Söker efter uppdateringar igen några dagar som dessa är snart tillgängligt.
> 
> 

## <a name="whats-new-in-update-12"></a>Vad är nytt i Update 1.2
Dessa funktioner publicerades först med uppdatering 1 som har gjorts tillgängliga tooa begränsad uppsättning användare. De flesta av hello StorSimple användare skulle se hello följande nya funktioner och förbättringar för hello Update 1.2-versionen:

* **Migrering från enheterna i serierna 5000 7000 serien too8000** – den här versionen introducerar en ny funktion för migrering där hello StorSimple-5000 7000-serien installation användare toomigrate sina data tooa StorSimple 8000-serien fysisk installation eller en virtuell installation. hello migrering funktionen har två viktiga värderingsförslag:                                                                  
  
  * **Kontinuitet för företag**, genom att aktivera migrering av befintliga data på 5000 7000-serien installationer too8000 serie installationer.
  * **Förbättrad funktionen erbjudanden av hello 8000-serien apparater**, till exempel effektiv centraliserad hantering av flera installationer via StorSimple Manager-tjänsten, bättre maskinvaruklass och uppdatera inbyggd programvara, virtuella installationer, data mobility och funktioner i hello framtida Översikt.
    
    Se toohello [Migreringsguide](http://www.microsoft.com/download/details.aspx?id=47322) mer information om hur toomigrate en StorSimple 5000 7000-serien tooan 8000-serien-enhet. 
* **Tillgänglighet i hello Azure Government Portal** – StorSimple är nu tillgänglig i hello Azure Government-portalen. Se hur för[distribuera en virtuell StorSimple-enhet i hello Azure Government Portal](storsimple-deployment-walkthrough-gov.md).
* **Stöd för andra molntjänstleverantörer** – hello andra molntjänstleverantörer som stöds är Amazon S3, Amazon S3 med RRS, HP och OpenStack (beta).
* **Uppdatera toolatest API: erna lagring** – med den här versionen, StorSimple har uppdaterats toohello senaste Azure Storage service API: er. StorSimple 8000-serien enheter som körs före uppdatering 1 programvaruversioner (versionen 0,1 och 0,2 0,3) använder versioner av hello Azure Storage Service API: er äldre än 17 juli 2009. Enligt informationen i hello uppdateras [meddelande om borttagning av Storage-tjänstversioner](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx), genom att 1 augusti 2016 dessa API: er att bli inaktuell. Det är viktigt att du tillämpar hello StorSimple 8000 Series uppdatering 1 tidigare tooAugust 1, 2016. Om du inte toodo så stoppas StorSimple-enheter fungerar korrekt.
* **Stöd för zonen Redundant lagring (ZRS)** – med hello uppgradera toohello senaste versionen av hello Storage-API: er, hello StorSimple 8000-serien stöder zonen Redundant lagring (ZRS) i tillägg tooLocally Redundant lagring (LRS) och Geo-redundant Lagring (GRS). Se toothis [artikel på Azure Storage redundansalternativ](../storage/common/storage-redundancy.md) ZRS information.
* **Förbättrad inledande distributionen och uppdatera experience** – i den här versionen hello installationen och uppdatera processer har förbättrats. hello installationen via hello installationsguiden är förbättrad tooprovide feedback toohello användaren om hello konfiguration och brandväggen nätverksinställningar är felaktiga. Ytterligare diagnostiska cmdlets har angetts toohelp dig med felsökning av nätverk för hello enhet. Se hello [distribution artikeln Felsöka](storsimple-troubleshoot-deployment.md) för mer information om hello nya diagnostiska-cmdlets som används för felsökning.

## <a name="issues-fixed-in-update-12"></a>Problem som åtgärdas i uppdateringen 1.2
hello följande tabell innehåller en översikt över problem som har korrigerats i uppdateringar 1.2, 1.1 och 1.    

| Nej. | Funktion | Problem | Korrigeras i uppdateringen | Gäller toophysical enhet | Gäller toovirtual enhet |
| --- | --- | --- | --- | --- | --- |
| 1 |Windows PowerShell för StorSimple |När en användare fjärransluta till hello StorSimple-enhet med hjälp av Windows PowerShell för StorSimple och sedan startas installationsguiden hello, uppstod en krasch så snart Data 0 IP har indata. Det här programfelet är nu fast i uppdatering 1. |Uppdatering 1 |Ja |Ja |
| 2 |Fabriksåterställning |I vissa fall, när du har utfört en fabriksåterställning hello StorSimple-enheten har fastnat och visas det här meddelandet: **Återställ toofactory pågår (fas 8)**. Detta har hänt om trycks ned CTRL + C när hello cmdlet pågick. Det här problemet löses nu. |Uppdatering 1 |Ja |Nej |
| 3 |Fabriksåterställning |Efter en misslyckad dubbla domänkontrollant fabrik återställts tilläts tooproceed med enhetsregistrering. Detta resulterade i en konfiguration som inte stöds. Ett felmeddelande visas i uppdatering 1 och registrering är blockerad på en enhet som har ett felaktigt fabriksåterställa. |Uppdatering 1 |Ja |Nej |
| 4 |Fabriksåterställning |I vissa fall höjdes falska positiva matchningsfel aviseringar. Felaktig matchning aviseringar genereras inte längre på enheter som kör uppdatering 1. |Uppdatering 1 |Ja |Nej |
| 5 |Fabriksåterställning |Om en fabriksåterställning avbröts tidigare toocompletion hello enheten anges återställningsläge och tillåter inte att du tooaccess Windows PowerShell för StorSimple. Det här problemet löses nu. |Uppdatering 1 |Ja |Nej |
| 6 |Haveriberedskap |Ett programfel för disaster recovery (DR) åtgärdades där DR misslyckas under hello identifiering av säkerhetskopior på hello målenhet. |Uppdatering 1 |Ja |Ja |
| 7 |Övervaka indikatorer |I vissa fall övervakning indikatorer på hello baksidan installation anger inte längre rätt status. hello blå Indikator har varit avstängd. DATA 0 och DATA 1 led blinkande även när dessa gränssnitt har inte konfigurerats. hello problemet har åtgärdats och övervakning indikatorer visar nu rätt status för hello. |Uppdatering 1 |Ja |Nej |
| 8 |Övervaka indikatorer |I vissa fall när uppdatering 1 hello blå ljus på hello aktiva styrenheten avstängd vilket gör det svårt tooidentify hello aktiva styrenhet. Det här problemet har korrigerats i den här versionen av korrigering. |Uppdatera 1.2 |Ja |Nej |
| 9 |Nätverksgränssnitt |I tidigare versioner av går en virtuell StorSimple-enhet som har konfigurerats med en icke-dirigerbara gateway offline. I den här versionen har hello routning mått för Data 0 gjorts hello lägsta; även om andra nätverksgränssnitt moln-aktiverat, därför dirigeras alla hello molnet trafik från hello enhet via Data 0. |Uppdatering 1 |Ja |Ja |
| 10 |Säkerhetskopior |Ett fel i uppdatering 1 som orsakade säkerhetskopieringar toofail när 24 dagar har åtgärdats i hello korrigering släpp Update 1.1. |Uppdatera 1.1 |Ja |Ja |
| 11 |Säkerhetskopior |Ett fel i tidigare versioner resulterade i sämre prestanda för molnögonblicksbilder med låg ändra priser. Det här felet har korrigerats i den här versionen av korrigering. |Uppdatera 1.2 |Ja |Ja |
| 12 |Uppdateringar |Ett fel i uppdatering 1 som rapporterats av en misslyckad uppgradering och gjorde hello domänkontrollanter toogo i återställningsläge, har korrigerats i den här versionen av korrigering. |Uppdatera 1.2 |Ja |Ja |

## <a name="known-issues-in-update-12"></a>Kända problem i Update 1.2
hello följande tabell innehåller en översikt över kända problem i den här versionen.

| Nej. | Funktion | Problem | Kommentarer/lösning | Gäller toophysical enhet | Gäller toovirtual enhet |
| --- | --- | --- | --- | --- | --- |
| 1 |Disk kvorum |I sällsynta fall om hello merparten av diskar i hello EBOD hölje av 8600-enheten är frånkopplad ledde till Ingen disk i kvorum kommer sedan hello lagringspoolen vara offline. Den förblir offline även om hello diskar återansluts. |Du behöver tooreboot hello enhet. Hello problemet kvarstår kontaktar du Microsoft Support för nästa steg. |Ja |Nej |
| 2 |Felaktig styrenhets-ID |När en domänkontrollant ersättning utförs kan styrenhet 0 visas som kontrollant 1. Under controller ersättning, när hello bild läses in från hello peer-noden kan hello styrenhets-ID visas först som domänkontrollant hello-peer-ID. I sällsynta fall kan problemet visas efter en omstart. |Ingen användaråtgärd krävs. Den här situationen ska lösas av sig självt när hello controller ersättning är klar. |Ja |Nej |
| 3 |Lagringskonton |Använda hello Storage service toodelete hello storage-konto är ett scenario som inte stöds. Detta leder tooa situation där användardata inte kan hämtas. |Ja |Ja | |
| 4 |Enheten växling vid fel |Flera redundans av en volymbehållare från hello samma källa enheten toodifferent målenheterna inte stöds. Enheten växling vid fel från en enskild döda enhet toomultiple enheter gör hello volymbehållare på hello först redundansväxlats enhet förlorar dataägarskap. Efter en växling, ska behållarna volymen visas eller fungera annorlunda när de visas i hello klassiska Azure-portalen. | |Ja |Nej |
| 5 |Installation |Under StorSimple-kort för SharePoint-installation måste tooprovide en enhet IP-adress för hello installera toofinish har. | |Ja |Nej |
| 6 |Webbproxy |Om din webbproxykonfigurationen har HTTPS enligt hello-protokollet och sedan enhet-till-tjänst-kommunikation kommer att påverkas och hello enheten går offline. Stöd för paket skapas också i hello process förbrukar betydande resurser på enheten. |Se till att hello web proxy URL har HTTP som hello angivet protokoll. Mer information finns för[konfigurera webbproxy för din enhet](storsimple-configure-web-proxy.md). |Ja |Nej |
| 7 |Webbproxy |Om du konfigurerar och aktiverar webbproxy på en registrerad enhet måste toorestart hello aktiva styrenheten på enheten. | |Ja |Nej |
| 8 |Hög molnet latens och hög i/o-arbetsbelastning |När din StorSimple-enhet påträffar en kombination av mycket hög molnet latens (ordning sekunder) och höga i/o-arbetsbelastning, hello enheten volymer som ingår i ett degraderat tillstånd och hello I/o kan misslyckas med felet ”enheten är inte klar”. |Du behöver toomanually omstart hello styrenheter eller utför en växling vid fel toorecover för enheten från den här situationen. |Ja |Nej |
| 9 |Azure PowerShell |När du använder hello StorSimple cmdlet **Get-AzureStorSimpleStorageAccountCredential &#124; Select-Object - först 1 - vänta** tooselect hello första objekt så att du kan skapa en ny **VolumeContainer** objekt hello cmdlet returnerar alla hello-objekt. |Omsluta hello cmdlet inom parentes på följande sätt: **(Get-Azure-StorSimpleStorageAccountCredential) &#124; Select-Object - först 1 - vänta** |Ja |Ja |
| 10 |Migrering |När flera volymbehållare skickas för migrering är hello ETA för senaste säkerhetskopian exakt endast för hello första volymbehållare. Dessutom startar parallella migreringen när hello första 4 säkerhetskopieringar i hello första volymbehållare har migrerats. |Vi rekommenderar att du migrerar en volymbehållare i taget. |Ja |Nej |
| 11 |Migrering |Volymer läggs inte toohello säkerhetskopiering principen eller hello virtuella diskgruppen efter hello återställning. |Du behöver tooadd dessa volymer tooa säkerhetskopieringsprincip ordning toocreate säkerhetskopior. |Ja |Ja |
| 12 |Migrering |När hello migreringen är klar, hello 5000/7000-serien enheten måste inte att komma åt hello migrerade databehållare. |Vi rekommenderar att du tar bort hello migrerade databehållare när hello migreringen är klar och bekräftats. |Ja |Nej |
| 13 |Klona och Katastrofåterställning |En StorSimple-enhet som kör uppdatering 1 kan inte klona eller utföra katastrofåterställning tooa enheter före uppdateringen 1 programvaran som körs. |Du behöver tooupdate hello mål enheten tooUpdate 1 tooallow dessa åtgärder |Ja |Ja |
| 14 |Migrering |Konfigurationssäkerhetskopia för migrering kan misslyckas på en serieenhet för 5000 7000-när volymen grupper med inga associerade volymer. |Ta bort alla hello tom volym grupper med inga associerade volymer och försök sedan hello konfigurationssäkerhetskopia. |Ja |Nej |

## <a name="physical-device-updates-in-update-12"></a>Fysisk enhetsuppdateringar i Update 1.2
Om korrigering uppdatera 1.2 tillämpade tooa fysisk enhet (som kör versioner tidigare tooUpdate 1), ändras hello programvaruversion too6.3.9600.17584.

## <a name="controller-and-firmware-updates-in-update-12"></a>Domänkontrollanten och firmware-uppdateringar i Update 1.2
Den här versionen uppdaterar hello drivrutinen och hello disk inbyggd programvara på din enhet.

* Mer information om hello SAS-styrenhet uppdateringen finns [uppdatering 1 för LSI SAS-enheter i Microsoft Azure StorSimple-enhet](https://support.microsoft.com/kb/3043005). 
* Läs mer om hello disk firmware-uppdatering [Disk firmware uppdatering 1 för Microsoft Azure StorSimple-enhet](https://support.microsoft.com/kb/3063416).

## <a name="virtual-device-updates-in-update-12"></a>Virtuell enhetsuppdateringar i Update 1.2
Den här uppdateringen kan inte vara tillämpade toohello virtuell enhet. Nya virtuella enheter behöver toobe skapas. 

## <a name="next-steps"></a>Nästa steg
* [Installera uppdatering 1.2 på din enhet](storsimple-install-update-1.md).

