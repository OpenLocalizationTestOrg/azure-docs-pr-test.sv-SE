---
title: aaaStorSimple 8000 uppdatering 0,3 viktig information | Microsoft Docs
description: "Beskriver hello nya funktioner och korrigeringar, öppna problem och tillgängliga lösningar för hello februari 2015 Microsoft Azure StorSimple version (uppdatering 0.3)."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: b01bfd04-f9f8-45f4-ade8-95ac2b979e6a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/18/2016
ms.author: v-sharos
ms.openlocfilehash: 53638f9d286b2085c6b45f9e3fae75c34fc73e7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-update-03-release-notes---february-2015"></a>StorSimple 8000 uppdatering 0,3 viktig information – februari 2015
## <a name="overview"></a>Översikt
hello följande viktig information identifiera hello kritiska öppna problem för StorSimple 8000 uppdatering 0.3 ut i februari 2015. De innehåller också en lista över hello StorSimple programvara och programvara som ingår i den här versionen. Detta är hello tredje utgåvan efter hello StorSimple 8000-serien släpper version gjordes allmänt tillgänglig i juli 2014.

Den här uppdateringen ändras inte hello enheten programvaruversionen från hello januari update. Toobe version 6.3.9600.17312 fortsätter. Du kan bekräfta att hello-uppdatering har installerats genom att kontrollera hello **senaste uppdaterade** datum. Om hello datum är 2/10/2015 eller senare, sedan hello uppdateringen har slutförts.  

Vi rekommenderar att du söka efter och installera tillgängliga uppdateringar direkt när du har installerat din StorSimple-enhet. Du kan också aktivera Automatiska uppdateringar toodownload och installera viktiga uppdateringar från Microsoft så snart de blir tillgängliga. Mer information finns i [uppdatera din StorSimple-enhet](storsimple-update-device.md).  

Granska hello informationen i hello release notes innan du distribuerar hello uppdatera i din StorSimple-lösning.  

> [!IMPORTANT]
> * Använd hello StorSimple Manager-tjänsten och inte Windows PowerShell för StorSimple tooinstall hello februari uppdatering.   
> * Det tar ungefär en timme tooinstall uppdateringen. Om du installerar kumulativa uppdateringar kan hello processen ta cirka 3 timmar toocomplete.  
> * hello innehåller februari versionen av StorSimple inte några uppdateringar toohello virtuella StorSimple-enheten. Du kan använda alla tillgängliga Windows uppdateringar toohello virtuella enheter, inklusive de senaste säkerhetsuppdateringarna korrigeringar, men du kan inte se en ändring i version för hello virtuella enheten.  
> 
> 

Kontrollera att hello följande förutsättningar är uppfyllda tidigare tooupdating din StorSimple-enhet.  

* Se till att båda styrenheter körs innan du söker efter uppdateringar. Hello genomsökning misslyckas om antingen domänkontrollanten körs. tooverify hello styrenheter är i felfritt tillstånd, navigera för**maskinvarustatus** under hello **Underhåll** sidan. Om det finns komponenter som **behöver åtgärdas**, kontakta Microsoft Support innan du fortsätter.
* Kontrollera att fasta IP-adresser för både styrenhet 0 och kontrollant 1 är dirigerbara och kan ansluta toohello Internet som används för att underhålla hello uppdateringar toohello enhet. Du kan använda hello [Test-Connection cmdlet](https://technet.microsoft.com/library/hh849808.aspx) tooping en känd adress utanför hello nätverk, till exempel outlook.com, tooverify som hello domänkontrollant har anslutning toohello utanför nätverket.
* Kontrollera att portarna 80 och 443 är tillgängliga på din StorSimple-enhet för utgående kommunikation. Mer information finns i hello [nätverkskrav för din StorSimple-enhet](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
* Om hello enheten programvaruversion är äldre än 6.3.9600.17312 (oktober 2014-uppdatering), inaktivera hello Data 2 och Data 3 portar, om aktiverad, innan du startar hello uppdateringen. Lämnar hello Data 2 och Data 3 portar aktiverat när du har installerat uppdateringen hello kan leda till att din enhet controller toogo i återställningsläge. Observera att när du inaktiverar hello nätverksgränssnitt alla hello associerade volymer försätts i offlineläge och hello I/o kommer att avbrytas för hello varaktighet för hello-uppdateringen.  

## <a name="whats-new-in-hello-february-release"></a>Vad är nytt i hello februari versionen
Den här uppdateringen innehåller en korrigering för hello factory Återställ problem som uppstod på enheter som har uppgraderats från hello GA versionen toohello oktober 2014-versionen. Mer information finns i [problem som åtgärdas i den här versionen](#issues-fixed-in-the-february-release).   

Den här uppdateringen innehåller inte nya funktioner eller funktionalitet.  

## <a name="issues-fixed-in-hello-february-release"></a>Problem som åtgärdas i hello februari versionen
hello beskrivs följande tabell hello problem som har åtgärdats i den här uppdateringen.  

| Nej. | Funktion | Problem | Gäller toophysical enhet | Gäller toovirtual enhet |
| --- | --- | --- | --- | --- |
| 1 |Fabriksåterställning |Försök tooperform en fabriksåterställning på en enhet som ursprungligen hade hello GA-versionen (6.3.9600.17215) installerats men har uppdaterats toohello oktober versionen (version 6.3.9600.17312). Hej fabriksåterställa misslyckas och hello enheten blir instabil. |Ja |Nej |

## <a name="known-issues-in-hello-february-release"></a>Kända problem i hello februari versionen
hello följande tabell innehåller en översikt över kända problem i den här versionen.

| Nej. | Funktion | Problem | Kommentarer/lösning | Gäller toophysical enhet | Gäller toovirtual enhet |
| --- | --- | --- | --- | --- | --- |
| 1 |Fabriksåterställning |I vissa fall, när du utför en fabriksåterställning hello StorSimple-enhet kan ha fastnat och visa det här meddelandet: **Återställ toofactory pågår (fas 8)**. Detta händer om du trycker på CTRL + C när hello cmdlet pågår. |Tryck inte på CTRL + C när du initierar en fabriksåterställning. Om du redan är i det här tillståndet kan kontakta Microsoft Support för nästa steg. |Ja |Nej |
| 2 |Disk kvorum |I sällsynta fall om hello merparten av diskar i hello EBOD hölje av en 8600device är frånkopplad ledde till Ingen disk i kvorum kommer sedan hello lagringspoolen vara offline. Den förblir offline även om hello diskar återansluts. |Du behöver tooreboot hello enhet. Hello problemet kvarstår kontaktar du Microsoft Support för nästa steg. |Ja |Nej |
| 3 |Fel i moln ögonblicksbild |I sällsynta fall kan en ögonblicksbild i molnet kan misslyckas med felet hello **säkerhetskopiering maxgräns uppnåtts**. Detta inträffar om du överskrider 255 online kloner på hello samma enhet, från hello samma ursprungliga volym som har tagits bort. | |Ja |Ja |
| 4 |Felaktig styrenhets-ID |När en domänkontrollant ersättning utförs kan styrenhet 0 visas som kontrollant 1. Under controller ersättning, när hello bild läses in från hello peer-noden kan hello styrenhets-ID visas först som domänkontrollant hello-peer-ID. I sällsynta fall kan problemet visas efter en omstart. |Ingen användaråtgärd krävs. Den här situationen ska lösas av sig självt när hello controller ersättning är klar. |Ja |Nej |
| 5 |Enheten övervakning diagram |Hello enheten övervakning diagram fungerar inte när grundläggande eller NTLM-autentisering är aktiverat i hello proxyserverkonfiguration för hello enhet i hello StorSimple Manager-tjänsten. |Ändra hello webbproxykonfiguration för hello enheten registrerad med StorSimple Manager-tjänsten så att autentisering har angetts tooNONE. toodo detta, kör hello hello Windows PowerShell för StorSimple Set-HcsWebProxy cmdlet. |Ja |Ja |
| 6 |Lagringskonton |Använda hello Storage service toodelete hello storage-konto är ett scenario som inte stöds. Detta leder tooa situation där användardata inte kan hämtas. | |Ja |Ja |
| 7 |Enheten växling vid fel |Flera redundans av en volymbehållare från hello samma källa enheten toodifferent målenheterna inte stöds.    Växling vid fel från en enskild döda enhet toomultiple enheter gör hello volymbehållare på hello först redundansväxlats enhet förlorar dataägarskap. Efter en växling, ska behållarna volymen visas eller fungera annorlunda när de visas i hello klassiska Azure-portalen. | |Ja |Nej |
| 8 |Installation |Under StorSimple-kort för SharePoint-installation måste tooprovide en enhet IP-adress för hello installera toofinish har. | |Ja |Nej |
| 9 |Webbproxy |Om din webbproxykonfigurationen har HTTPS enligt hello-protokollet och sedan enhet-till-tjänst-kommunikation kommer att påverkas och hello enheten går offline. Stöd för paket skapas också i hello process förbrukar betydande resurser på enheten. |Se till att hello web proxy URL har HTTP som hello angivet protokoll. Mer information om hur för[konfigurera webbproxy för din enhet](storsimple-configure-web-proxy.md). |Ja |Nej |
| 10 |Webbproxy |Om du konfigurerar och aktiverar webbproxy på en registrerad enhet måste toorestart hello aktiva styrenheten på enheten. | |Ja |Nej |
| 11 |Hög molnet latens och hög i/o-arbetsbelastning |När din StorSimple-enhet påträffar en kombination av mycket hög molnet latens (ordning sekunder) och höga i/o-arbetsbelastning, hello enheten volymer som ingår i ett degraderat tillstånd och hello I/o kan misslyckas med felet ”enheten är inte klar”. |Du behöver toomanually omstart hello styrenheter eller utför en växling vid fel toorecover för enheten från den här situationen. |Ja |Nej |

## <a name="physical-device-updates-in-hello-february-release"></a>Uppdatering av fysiska enheter i hello februari versionen
Den här uppdateringen korrigeringar hello fabriksåterställa problem som uppstod på enheter som har uppgraderats från hello GA versionen toohello oktober 2014-versionen. Det innehåller inte några andra uppdateringar toohello StorSimple-enhet.  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-hello-february-release"></a>Serieansluten SCSI (SAS) domänkontrollant och firmware-uppdateringar i hello februari versionen
Den här versionen innehåller inte några uppdateringar toohello serieansluten SCSI (SAS) domänkontrollant eller hello inbyggd programvara. hello drivrutinsuppdateringen hade hello oktober 2014-versionen.  

## <a name="virtual-device-updates-in-hello-february-release"></a>Uppdatering av virtuella enheter i hello februari versionen
Den här versionen innehåller inte några uppdateringar för hello virtuella enheten. Uppdateringen ändrar inte hello programvaruversionen för en virtuell enhet.

