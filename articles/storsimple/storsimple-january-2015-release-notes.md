---
title: aaaStorSimple 8000 uppdatering 0,2 viktig information | Microsoft Docs
description: "Beskriver hello nya funktioner och korrigeringar, öppna problem och tillgängliga lösningar för hello januari 2015 Microsoft Azure StorSimple version (uppdatera 0,2)."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: d9684ae3-b38f-4678-9d70-e5dbc6b03350
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/16/2016
ms.author: v-sharos
ms.openlocfilehash: 1cee795df0b53d9b9276bc33074cf1a7aa188835
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-update-02-release-notes---january-2015"></a>StorSimple 8000 uppdatering 0,2 viktig information - januari 2015
## <a name="overview"></a>Översikt
hello identifiera följande versionsinformation hello kritiska öppna problem för hello januari 2015-versionen av Microsoft Azure StorSimple. De innehåller också en lista över hello StorSimple programvara och programvara som ingår i den här versionen. Detta är hello andra utgåvan efter hello StorSimple 8000-serien släpper version gjordes allmänt tillgänglig i juli 2014.

Den här uppdateringen ändras inte hello fysisk enhet programvaruversionen från hello oktober update. Toobe version 6.3.9600.17312 fortsätter. hello avbildningen används hello virtuella enheten bilden har ändrats i den här versionen. Därför kan visar alla hello nya virtuella enheter skapats efter 20/1/2015 hello-version som 6.3.9600.17361.  

Granska följande information i hello viktig information för hello januari 2015 update hello.

> [!IMPORTANT]
> * Den här uppdateringen är inte tillgänglig via Windows Update och kan inte installeras som andra uppdateringar. Enheten får inte den här uppdateringen, även om du har använt hello uppdateringar med hjälp av hello klassiska Azure-portalen. Den här uppdateringen gäller endast toovirtual enheter skapas efter 20 januari 2015. 
> * hello innehåller januari versionen av StorSimple inte några uppdateringar toohello fysiska StorSimple-enheten. Du kan använda alla tillgängliga Windows uppdateringar toohello virtuella enheter, inklusive de senaste säkerhetsuppdateringarna korrigeringar, men du kan inte se en ändring i version för fysiska hello StorSimple-enheten.
> 
> 

## <a name="whats-new-in-hello-january-release"></a>Vad är nytt i hello januari versionen
Den här uppdateringen innehåller en korrigering relaterade toohello volymer försätts i offlineläge på hello virtuell enhet. (Se [problem som åtgärdas i den här versionen](#issues-fixed-in-the-january-release).)  

hello uppdateringen innehåller inte nya funktioner eller funktionalitet.  

## <a name="issues-fixed-in-hello-january-release"></a>Problem som åtgärdas i hello januari versionen
hello beskrivs följande tabell hello problem som har åtgärdats i den här uppdateringen.

| Nej. | Funktion | Problem | Gäller toophysical enhet | Gäller toovirtual enhet |
| --- | --- | --- | --- | --- |
| 1 |Volymer försätts i offlineläge |När hög molnet svarstiderna kvarstår i flera minuter hello StorSimple virtuell enhet volymer i offlineläge på hello värdar. Den här snabbkorrigeringen ökar hello tröskelvärdet för fördröjningar i molnet, vilket minimerar hello situationer som skulle orsaka hello volymer toogo offline på värdar. |Nej |Ja |

## <a name="known-issues-in-hello-january-release"></a>Kända problem i hello januari versionen
hello följande tabell innehåller en översikt över kända problem i den här versionen.

| Nej. | Funktion | Problem | Kommentarer/lösning | Gäller toophysical enhet | Gäller toovirtual enhet |
| --- | --- | --- | --- | --- | --- |
| 1 |Fabriksåterställning |I vissa fall, när du utför en fabriksåterställning hello StorSimple-enhet kan ha fastnat och visa det här meddelandet: **Återställ toofactory pågår (fas 8).** Detta händer om du trycker på CTRL + C när hello cmdlet pågår. |Tryck inte på CTRL + C när du initierar en fabriksåterställning. Om du redan är i det här tillståndet kan kontakta Microsoft Support för nästa steg. |Ja |Nej |
| 2 |Disk kvorum |I sällsynta fall om hello merparten av diskar i hello EBOD hölje av 8600-enheten är frånkopplad ledde till Ingen disk i kvorum kommer sedan hello lagringspoolen vara offline. Den förblir offline även om hello diskar återansluts. |Du behöver tooreboot hello enhet. Hello problemet kvarstår kontaktar du Microsoft Support för nästa steg. |Ja |Nej |
| 3 |Fel i moln ögonblicksbild |I sällsynta fall kan en ögonblicksbild i molnet kan misslyckas med felet hello **säkerhetskopiering maxgräns uppnåtts**. Detta inträffar om du överskrider 255 online kloner på hello samma enhet, från hello samma ursprungliga volym som har tagits bort. | |Ja |Ja |
| 4 |Felaktig styrenhets-ID |När en domänkontrollant ersättning utförs kan styrenhet 0 visas som kontrollant 1. Under controller ersättning, när hello bild läses in från hello peer-noden kan hello styrenhets-ID visas först som domänkontrollant hello-peer-ID.  I sällsynta fall kan problemet visas efter en omstart. |Ingen användaråtgärd krävs. Den här situationen ska lösas av sig självt när hello controller ersättning är klar. |Ja |Nej |
| 5 |Enheten övervakning diagram |Hello enheten övervakning diagram fungerar inte när grundläggande eller NTLM-autentisering är aktiverat i hello proxyserverkonfiguration för hello enhet i hello StorSimple Manager-tjänsten. |Ändra hello webbproxykonfiguration för hello enheten registrerad med StorSimple Manager-tjänsten så att autentisering har angetts tooNONE. toodo detta, kör hello hello Windows PowerShell för StorSimple Set-HcsWebProxy cmdlet. |Ja |Ja |
| 6 |Lagringskonton |Använda hello Storage service toodelete hello storage-konto är ett scenario som inte stöds. Detta leder tooa situation där användardata inte kan hämtas. | |Ja |Ja |
| 7 |Enheten växling vid fel |Flera redundans av en volymbehållare från hello samma källa enheten toodifferent målenheterna inte stöds. |Växling vid fel från en enskild döda enhet toomultiple enheter gör hello volymbehållare på hello först redundansväxlats enhet förlorar dataägarskap. Efter en växling, ska behållarna volymen visas eller fungera annorlunda när de visas i hello klassiska Azure-portalen. |Ja |Nej |
| 8 |Installation |Under StorSimple-kort för SharePoint-installation måste tooprovide en enhet IP-adress för hello installera toofinish har. | |Ja |Nej |
| 9 |Webbproxy |Om din webbproxykonfigurationen har HTTPS enligt hello-protokollet och sedan enhet-till-tjänst-kommunikation kommer att påverkas och hello enheten går offline. Stöd för paket skapas också i hello process förbrukar betydande resurser på enheten. |Se till att hello web proxy URL har HTTP som hello angivet protokoll. Mer information om hur finns för[konfigurera webbproxy för din enhet](storsimple-configure-web-proxy.md). |Ja |Nej |
| 10 |Webbproxy |Om du konfigurerar och aktiverar webbproxy på en registrerad enhet måste toorestart hello aktiva styrenheten på enheten. | |Ja |Nej |
| 11 |Hög molnet latens och hög i/o-arbetsbelastning |När din StorSimple-enhet påträffar en kombination av mycket hög molnet latens (ordning sekunder) och höga i/o-arbetsbelastning, hello enheten volymer som ingår i ett degraderat tillstånd och hello I/o kan misslyckas med felet ”enheten är inte klar”. |Du behöver toomanually omstart hello styrenheter eller utför en växling vid fel toorecover för enheten från den här situationen. |Ja |Nej |

## <a name="physical-device-updates-in-hello-january-release"></a>Uppdatering av fysiska enheter i hello januari versionen
Den här uppdateringen innehåller inte några andra ändringar toohello StorSimple-enhet.

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-hello-january-release"></a>Serieansluten SCSI (SAS) domänkontrollant och firmware-uppdateringar i hello januari versionen
Den här versionen innehåller inte några uppdateringar toohello serieansluten SCSI (SAS) domänkontrollant eller hello inbyggd programvara. hello drivrutinsuppdateringen hade hello oktober 2014-versionen. 

## <a name="virtual-device-updates-in-hello-january-release"></a>Uppdatering av virtuella enheter i hello januari versionen
Den här versionen innehåller en uppdaterad bild för hello virtuella enheten. Alla virtuella hello enheter som du har skapat efter 20 januari 2015 visas hello programvaruversion som 6.3.9600.17361.

