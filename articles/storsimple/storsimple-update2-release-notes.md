---
title: "viktig information för aaaStorSimple 8000-serien uppdatering 2 | Microsoft Docs"
description: "Beskriver nya funktioner för hello, problem och lösningar för StorSimple 8000 Series uppdatering 2."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: e2c8bffd-7fc5-4b77-b632-a4f59edacc3a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/24/2016
ms.author: v-sharos
ms.openlocfilehash: 36c75aad900c7b1286a924732967b8ee519a3d4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-update-2-release-notes"></a>StorSimple 8000 Series uppdatering 2 viktig information
## <a name="overview"></a>Översikt
hello följande viktig information beskrivs hello nya funktioner och identifiera hello kritiska öppna problem för StorSimple 8000 Series uppdatering 2. De innehåller också en lista över hello StorSimple programvara, drivrutiner och uppdateringar av inbyggd disk i den här versionen. 

Uppdatering 2 kan vara tillämpade tooany StorSimple-enhet som kör versionen (GA) eller uppdatering 0.1 via Update 1.2. hello enheten version som är associerad med uppdatering 2 är 6.3.9600.17673.

Granska hello informationen i hello release notes innan du distribuerar hello uppdatera i din StorSimple-lösning.

> [!IMPORTANT]
> * Det tar cirka 4 – 7 timmar tooinstall uppdateringen (inklusive hello Windows-uppdateringar). 
> * Uppdatering 2 har programvara, LSI drivrutiner och uppdateringar av inbyggd SSD.
> * För nya versioner, kan du inte se uppdateringar omedelbart eftersom vi gör en stegvis distribution av hello uppdateringar. Vänta några dagar och sedan söka efter uppdateringar igen som dessa är snart tillgängligt.
> 
> 

## <a name="whats-new-in-update-2"></a>Vad är nytt i uppdatering 2
Uppdatering 2 introducerar hello följande nya funktioner.

* **Lokalt fästa volymer** – i tidigare versioner av hello StorSimple 8000-serien datablock har nivåindelade toohello molnet baserat på användning. Det fanns inga sätt tooguarantee som blockerar skulle stanna kvar på lokala. I uppdatering 2 när du skapar en volym kan du ange en volym som data lokalt Fäst och primära från volymen inte kommer att nivåindelade toohello moln. Ögonblicksbilder av lokalt fästa volymer kommer fortfarande att kopieras toohello molnet för säkerhetskopiering så att hello molnet kan användas för data mobility och disaster recovery. Du kan dessutom ändra hello volymtyp (det vill säga konvertera nivåindelade volymer toolocally Fäst volymer och konvertera lokalt fästa volymer tootiered). 
* **StorSimple virtuell enhet förbättringar** – tidigare hello StorSimple 8000-serien placerats hello virtuell enhet som en lösning för disaster recovery eller utveckling och testning. Det var bara en modell för virtuella enheten (model 1100). Uppdatering 2 innehåller två virtuella enhetsmodeller: 
  
  * 8010 (tidigare kallade hello 1100) –; ingen ändring har en kapacitet på 30 TB och använder Azure standardlagring.
  * – 8020 har 64 TB kapacitet och använder Azure Premium-lagring för bättre prestanda.
    
    Det finns en enda virtuell Hårddisk för både virtuella enhetsmodeller (8010/8020). När du startar hello virtuell enhet identifierar hello plattform parametrar och gäller hello rätt modellversion.
* **Förbättringar av nätverk** – uppdatering 2 innehåller hello följande förbättringar av nätverk:
  
  * Flera nätverkskort kan aktiveras för hello molnet så att växling vid fel kan inträffa om ett nätverkskort inte.
  * Routning förbättringar med fast molnet aktiverat block.
  * Online återförsök för misslyckade resurser innan en växling vid fel.
  * Nya aviseringar för tjänsten.
* **Uppdatera förbättringar** – i Uppdatera 1.2 och tidigare, hello StorSimple 8000-serien uppdaterades via två kanaler: Windows Update för klustring, iSCSI, och så vidare och Microsoft Update för binärfiler och inbyggd programvara.
    Uppdatering 2 använder Microsoft Update för alla uppdateringspaket. Detta ska leda tooless tid korrigeringar eller göra växling vid fel. 
* **Uppdateringar av inbyggd** – hello följande firmware uppdateringar ingår:
  
  * LSI: lsi_sas2.sys produktversionen 2.00.72.10
  * Endast SSD (ingen Hårddisk uppdateringar): XMGG, XGEG, KZ50, F6C2 och VR08
* **Proaktiv Support** – uppdatering 2 kan Microsoft toopull ytterligare diagnostikinformation från hello enhet. När våra driftteamet identifierar enheter som har problem, vi är bättre utrustade toocollect information från hello enhet och diagnostisera problem. **Genom att acceptera uppdatering 2 du låta oss tooprovide proaktiv stödet**.    

## <a name="issues-fixed-in-update-2"></a>Problem som åtgärdas i uppdatering 2
hello följande tabeller innehåller en översikt över problem som har lösts i uppdateringar 2.    

| Nej. | Funktion | Problem | Gäller toophysical enhet | Gäller toovirtual enhet |
| --- | --- | --- | --- | --- |
| 1 |Nätverksgränssnitt |Efter en uppgradering tooUpdate 1 rapporterade hello StorSimple Manager-tjänsten att hello Data2 och Data3 portar misslyckades på en domänkontrollant. Det här problemet har korrigerats. |Ja |Nej |
| 2 |Uppdateringar |Efter en uppgradering tooUpdate 1 detta akustiskt larm aviseringar som har uppstått i hello-klassiska Azure-portalen på flera enheter. Det här problemet har korrigerats. |Ja |Nej |
| 3 |Openstack autentisering |När du använder Openstack som din molntjänstleverantör, kan ett felmeddelande som ditt moln autentisering sträng var för lång. Problemet har åtgärdats. |Ja |Nej |

## <a name="known-issues-in-update-2"></a>Kända problem i uppdatering 2
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
| 20 |Lokalt fästa volymer |Om du försöker tooconvert en nivåindelad volym (skapats och klonade med Update 1.2 eller tidigare) tooa lokalt Fäst volym och enheten få slut på utrymme eller det finns ett avbrott i molnet och sedan hello clone(s) kan vara skadad. |Det här problemet uppstår bara med volymer som har skapats och klonade med före uppdatering 2 programvara. Detta bör vara ett ovanligt scenario. | | |
| 21 |Volymkonvertering |Uppdatera inte hello ACRs bifogade tooa volym när en Volymkonvertering pågår (nivåindelade toolocally Fäst eller vice versa). Uppdatera hello ACRs kan resultera i skadade data. |Om det behövs, uppdatera hello ACRs tidigare toohello Volymkonvertering och gör inte ytterligare uppdateringar ACR medan hello konvertering pågår. | | |

## <a name="controller-and-firmware-updates-in-update-2"></a>Domänkontrollanten och firmware-uppdateringar i uppdatering 2
Den här versionen uppdaterar hello drivrutinen och hello disk inbyggd programvara på din enhet.

* Mer information om hello LSI firmware uppdatera, se Microsoft Knowledge base-artikel 3121900. 
* Mer information om hello disk firmware uppdatera, se Microsoft Knowledge base-artikel 3121899.

## <a name="virtual-device-updates-in-update-2"></a>Uppdatering av virtuella enheter i uppdatering 2
Den här uppdateringen kan inte vara tillämpade toohello virtuell enhet. Nya virtuella enheter behöver toobe skapas. 

## <a name="next-step"></a>Nästa steg
Lär dig hur för[installera uppdatering 2](storsimple-install-update-2.md) på StorSimple-enheten.

