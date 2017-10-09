---
title: aaaStorSimple 8000 uppdatering 0.1 viktig information | Microsoft Docs
description: "Beskriver hello nya funktioner och korrigeringar, öppna problem och tillgängliga lösningar för hello oktober 2014 Microsoft Azure StorSimple version (uppdatering 0.1)."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: fd35e3c3-4770-460c-999d-f72ab7053a20
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/21/2016
ms.author: alkohli
ms.openlocfilehash: 684e31cb0b356ec59a9d6c245e5d2bc062cc4f0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-update-01-release-notes--october-2014"></a>StorSimple 8000 uppdatering 0.1 viktig information – oktober 2014
## <a name="overview"></a>Översikt
hello följande viktig information identifiera hello kritiska öppna problem för StorSimple 8000 uppdatering 0.1 ut i oktober 2014. De innehåller också en lista över hello StorSimple programvara och programvara som ingår i den här versionen. Det här är första hello-versionen när hello StorSimple 8000-serien släpper version gjordes allmänt tillgänglig i juli 2014 och motsvarar toosoftware version 6.3.9600.17312.  

Vi rekommenderar att du söka efter och installera tillgängliga uppdateringar omedelbart efter att du installerar hello-enhet. Du kan också aktivera Automatiska uppdateringar toodownload och installera viktiga uppdateringar från Microsoft så snart de blir tillgängliga. Mer information finns i hur för[uppdatera din StorSimple-enhet](storsimple-update-device.md).  

Granska hello informationen i hello viktig information innan du distribuerar hello uppdateringar i din StorSimple-lösning.  

> [!IMPORTANT]
> * Använd hello StorSimple Manager-tjänsten och inte Windows PowerShell för StorSimple tooinstall hello oktober uppdateras.  
> * hello uppdateringar tar normalt ca 3 timmar toocomplete.  
> * hello innehåller oktober versionen av StorSimple inte några uppdateringar toohello virtuella StorSimple-enheten. Du kan använda alla tillgängliga Windows-uppdateringar, inklusive de senaste säkerhetskorrigeringar, men en ändring i version för hello virtuella enheten visas inte.  
> 
> 

Se hello till att följande krav är uppfyllda tidigare tooupdating din StorSimple-enhet.  

* Se till att båda styrenheter körs innan du söker efter uppdateringar. Hello genomsökning misslyckas om antingen domänkontrollanten körs. tooverify hello styrenheter är i felfritt tillstånd, navigera för**maskinvarustatus** under hello **Underhåll** sidan. Om det finns komponenter som **behöver åtgärdas**, kontakta Microsoft Support innan du fortsätter.  
* Kontrollera att fasta IP-adresser för både styrenhet 0 och 1 är dirigerbara och kan ansluta toohello Internet som används för att underhålla hello uppdateringar toohello enhet. Du kan använda hello [Test-Connection cmdlet](https://technet.microsoft.com/library/hh849808.aspx) tooping en känd adress utanför hello nätverk, till exempel outlook.com, tooverify som hello domänkontrollant har anslutning toohello utanför nätverket.  
* Se till att hello begärda utgående portar är tillgängliga på din StorSimple-enhet för utgående kommunikation. Mer information finns i hello [nätverkskrav för din StorSimple-enhet](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).  
* Om hello enheten programvaruversion är äldre än 6.3.9600.17312 (oktober 2014-uppdatering), inaktivera hello Data 2 och Data 3 portar, om aktiverad, innan du startar hello uppdateringen. Om du lämnar hello Data 2 och Data 3 portar aktiverad när du använder hello uppdatering, kanske din enhet controller toogo i återställningsläge. Observera att när du inaktiverar hello nätverksgränssnitt alla hello associerade volymer försätts i offlineläge och kommer att avbrytas hello i/o för hello varaktighet för hello-uppdateringen.  

## <a name="whats-new-in-hello-october-release"></a>Vad är nytt i hello oktober versionen
Den här uppdateringen innehåller hello följande förbättringar:

* Du kan nu använda hello StorSimple Manager-tjänsten UI toomanage din styrenheter. hello management åtgärderna är bland annat starta om avslutning, eller aktivera en domänkontrollant. Mer information finns för[hantera StorSimple-styrenheter](storsimple-manage-device-controller.md).  
* Du kan schemalägga WAN bandbreddsallokering enligt kombinationer av tooday veckan och tid på dagen. Det innebär att du toomake bättre användning av WAN-bandbredden vid låg belastning. Mallar för olika bandbredd tillåts för olika volymbehållare. Mer information finns för[hantera dina StorSimple bandbredd mallar](storsimple-manage-bandwidth-templates.md).  
* Du kan konfigurera e-postaviseringar tooproactively meddela hello administratörer och andra befintliga eller eventuellt kommande problem. Mer information finns för[konfigurera aviseringsinställningar](storsimple-manage-alerts.md#configure-alert-settings).  

## <a name="issues-fixed-in-hello-october-release"></a>Problem som åtgärdas i hello oktober versionen
hello innehåller följande tabell en översikt över problem som har lösts i den här uppdateringen.  

| Nej. | Funktion | Problem | Gäller toophysical enhet | Gäller toovirtual enhet |
| --- | --- | --- | --- | --- |
| 1 |Nätverksgränssnitt |I tidigare hello versionen, hello nätverksgränssnitten DATA 2 och DATA 3 har växlats hello programvaran. Problemet har åtgärdats i den här uppdateringen. Rensa hello inställningar och inaktivera dessa nätverksgränssnitt innan du installerar hello uppdateringen. När du har installerat uppdateringen hello behöver tooreconfigure dessa gränssnitt. |Ja |Nej |
| 2 |Support-paket |I hello tidigare version, om du körde hello Windows PowerShell **Export HcsSupportPackage** cmdlet tooretrieve hello Baseboard Management Controller (BMC) loggar, hello-åtgärden misslyckades med följande varning hello: ”hello Åtgärden lyckades på den här domänkontrollanten, men misslyckades på hello peer-domänkontrollant på grund av toohello följande fel. Kontrollera om hello peer är felfri och om hello aktuella noden kan ansluta toohello peer ”. Det här problemet löses nu. |Ja |Nej |
| 3 |Enheten växling vid fel |I hello tidigare version, fanns det en risk för data om en **identifiera säkerhetskopiering** jobb som misslyckades under en växling vid fel för enheten. Det här problemet löses nu. |Ja |Nej |
| 4 |Enheten växling vid fel |I hello föregående versionen, efter en växling på enheten, säkerhetskopieringar var synliga men hello associerade volymbehållare fanns inte på hello målenhet. Det här problemet löses nu. |Ja |Nej |
| 5 |Enheten växling vid fel |I hello tidigare version uppstod ett fel hello uppräkningen säkerhetskopieringar i molnet under hello registret-återställning som eventuellt kan orsaka toodata inkonsekvens om det fanns molnet problem med nätverksanslutningen. |Ja |Nej |
| 6 |Firmware-uppdatering |I hello tidigare version, hello enhetens inbyggda programvara Uppdateringsjobbet misslyckades och visas ett fel som anges att hello cmdlets var inte går att känna igen och hello uppdateringen misslyckades som ett resultat. har gått sedan hello domänkontrollanten i återställningsläge. Det här problemet löses nu. |Ja |Nej |
| 7 |Installation |Fel som orsakats av hello enheten inte som har avbildats korrekt under installationen har nu korrigerats. |Ja |Nej |
| 8 |Fabriksåterställning |Du kan nu om du vill hoppa över hello firmware Sök efter fabriksåterställning. Detta är en förändring från tidigare hello-versionen. |Ja |Nej |
| 9 |Fabriksåterställning |Hello tidigare version, när en cmdlet för återställning av fabriken kördes gjordes inbyggd programvara version kontrollerar endast för vissa komponenter. Ytterligare inbyggd programvara kontrollerar har gjorts efter hello första omstarten pågående hello, vilket kan leda till hello Återställ toofail. Den här snabbkorrigeringen säkerställer att alla hello inbyggd programvara kontrollerar görs när hello factory Återställ cmdleten körs och innan hello första systemet startas om. |Ja |Nej |
| 10 |Storage-konto viktiga rotation |Hej **Invoke-HcsmServiceDataEncryptionKeyChange** cmdlet används toorotate hello lagringskontonycklar nu prompter hello användaren tooenter hello krypteringsnyckel för tjänstdata. Detta är en förändring från hello föregående version i vilka hello krypteringsnyckel för tjänstdata skickades som en infogad-parameter. |Ja |Nej |
| 11 |Återställning efter fel inom 24 timmar |Vid katastrofåterställning utfördes inte hello rensning på hello källenheten toofail korrekt, orsakar återställning efter fel. Problemet har åtgärdats i den här versionen. |Ja |Nej |

## <a name="known-issues-in-hello-october-release"></a>Kända problem i hello oktober versionen
hello följande tabell innehåller en översikt över kända problem i den här versionen.

| Nej. | Funktion | Problem | Kommentarer/lösning | Gäller toophysical enhet | Gäller toovirtual enhet |
| --- | --- | --- | --- | --- | --- |
| 1 |Fabriksåterställning |I vissa fall, när du utför en fabriksåterställning hello StorSimple-enhet kan ha fastnat och visa det här meddelandet: **Återställ toofactory pågår (fas 8)**. Detta händer om du trycker på CTRL + C när hello cmdlet pågår. |Tryck inte på CTRL + C när du initierar en fabriksåterställning. Om du redan är i det här tillståndet kan kontakta Microsoft Support för nästa steg. |Ja |Nej |
| 2 |Fabriksåterställning |Gör inte fabriksåterställning en StorSimple-enhet som har uppdaterats från GA tooOctober 2014-versionen. |Den här åtgärden fungerar endast om en korrigering är installerad. Kontakta Microsoft Support tooget korrigeringsfilen krävs. |Ja |Nej |
| 3 |Disk kvorum |I sällsynta fall om hello merparten av diskar i hello EBOD hölje av 8600-enheten är frånkopplad ledde till Ingen disk i kvorum kommer sedan hello lagringspoolen vara offline. Den förblir offline även om hello diskar återansluts. |Du behöver tooreboot hello enhet. Hello problemet kvarstår kontaktar du Microsoft Support för nästa steg. |Ja |Nej |
| 4 |Fel i moln ögonblicksbild |I sällsynta fall kan en ögonblicksbild i molnet kan misslyckas med felet hello **säkerhetskopiering maxgräns uppnåtts**. Detta inträffar om du överskrider 255 online kloner på hello samma enhet, från hello samma ursprungliga volym som har tagits bort. | |Ja |Ja |
| 5 |Felaktig styrenhets-ID |När en domänkontrollant ersättning utförs kan styrenhet 0 visas som kontrollant 1. Under controller ersättning, när hello bild läses in från hello peer-noden kan hello styrenhets-ID visas först som domänkontrollant hello-peer-ID. I sällsynta fall kan problemet visas efter en omstart. |Ingen användaråtgärd krävs. Den här situationen ska lösas av sig självt när hello controller ersättning är klar. |Ja |Nej |
| 6 |Enheten övervakning diagram |Hello enheten övervakning diagram fungerar inte när grundläggande eller NTLM-autentisering är aktiverat i hello proxyserverkonfiguration för hello enhet i hello StorSimple Manager-tjänsten. |Ändra hello webbproxykonfiguration för hello enheten registrerad med StorSimple Manager-tjänsten så att autentisering har angetts tooNONE. toodo detta, kör hello hello Windows PowerShell för StorSimple Set-HcsWebProxy cmdlet. |Ja |Ja |
| 7 |Lagringskonton |Använda hello Storage service toodelete hello storage-konto är ett scenario som inte stöds. Detta leder tooa situation där användardata inte kan hämtas. | |Ja |Ja |
| 8 |Enheten växling vid fel |Flera redundans av en volymbehållare från hello samma källa enheten toodifferent målenheterna inte stöds. |Växling vid fel från en enskild döda enhet toomultiple enheter gör hello volymbehållare på hello först redundansväxlats enhet förlorar dataägarskap. Efter en växling, ska behållarna volymen visas eller fungera annorlunda när de visas i hello klassiska Azure-portalen. |Ja |Nej |
| 9 |Installation |Under StorSimple-kort för SharePoint-installation måste tooprovide en enhet IP-adress för hello installera toofinish har. | |Ja |Nej |
| 10 |Webbproxy |Om din webbproxykonfigurationen har HTTPS enligt hello-protokollet och sedan enhet-till-tjänst-kommunikation kommer att påverkas och hello enheten går offline. Stöd för paket skapas också i hello process förbrukar betydande resurser på enheten. |Se till att hello web proxy URL har HTTP som hello angivet protokoll. Mer information om hur för[konfigurera webbproxy för din enhet](storsimple-configure-web-proxy.md). |Ja |Nej |
| 11 |Webbproxy |Om du konfigurerar och aktiverar webbproxy på en registrerad enhet måste toorestart hello aktiva styrenheten på enheten. | |Ja |Nej |
| 12 |Hög molnet latens och hög i/o-arbetsbelastning |När din StorSimple-enhet påträffar en kombination av mycket hög molnet latens (ordning sekunder) och höga i/o-arbetsbelastning, hello enheten volymer som ingår i ett degraderat tillstånd och hello I/o kan misslyckas med felet ”enheten är inte klar”. |Du behöver toomanually omstart hello styrenheter eller utför en växling vid fel toorecover för enheten från den här situationen. |Ja |Nej |

## <a name="physical-device-updates-in-hello-october-release"></a>Uppdatering av fysiska enheter i hello oktober versionen
När uppdateringarna är tillämpade tooa fysisk enhet, ändrar hello programvaruversion too6.3.9600.17312. Om inget annat anges här viktiga informationen gäller tooall modeller av hello StorSimple-enhet. Mer information om de här uppdateringarna finns [oktober 2014 fysisk installation programuppdateringen för Microsoft Azure StorSimple-enhet](http://support.microsoft.com/kb/2986997).  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-hello-october-release"></a>Serieansluten SCSI (SAS) domänkontrollant och firmware-uppdateringar i hello oktober versionen
Den här versionen uppdaterar hello drivrutinen och hello inbyggd programvara på hello SAS av den fysiska enheten. Mer information om hello SAS-styrenhet uppdateringen finns [uppdatering under oktober 2014 för LSI SAS-enheter i Microsoft Azure StorSimple-enhet](http://support.microsoft.com/kb/2987020).   

Den här versionen gäller även en kumulativ firmware-uppdatering som hanterar tillförlitlighetsproblem med hello maskinvarukomponenter för enheten. Läs mer om hello firmware-uppdatering [oktober 2014 firmware-uppdatering för Microsoft Azure StorSimple-enhet](http://support.microsoft.com/kb/2987015).  

## <a name="virtual-device-updates-in-hello-october-release"></a>Uppdatering av virtuella enheter i hello oktober versionen
Den här versionen innehåller inte några uppdateringar för hello virtuella enheten. Uppdateringen ändrar inte hello programvaruversionen för en virtuell enhet.

