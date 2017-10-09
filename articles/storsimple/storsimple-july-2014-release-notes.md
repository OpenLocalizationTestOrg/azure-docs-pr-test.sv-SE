---
title: aaaStorSimple 8000 versionen version viktig information | Microsoft Docs
description: "Beskriver nya funktioner för hello, öppna problem och tillgängliga lösningar för hello juli 2014 Microsoft Azure StorSimple version."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 12f1796e-37c3-42b4-b997-a84fc1950c20
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/18/2016
ms.author: v-sharos
ms.openlocfilehash: 74863a3e2811dc7be5e6f482a5be4bbc37e3cd71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-release-version-release-notes---july-2014"></a>StorSimple 8000-serien släpper Version viktig information - juli 2014
## <a name="overview"></a>Översikt
hello följer viktig information identifiera hello kritiska öppna problem för hello StorSimple 8000-serien juli 2014 allmän tillgänglighet (GA) versionen av Microsoft Azure StorSimple. Den här versionen motsvarar toosoftware version 6.3.9600.17215.  

Om inget annat anges här viktiga informationen gäller tooall modeller av hello StorSimple-enhet. hello viktig information uppdateras kontinuerligt; allteftersom allvarliga problem som kräver en lösning upptäcks, läggs de till. Överväg att hello följande information innan du distribuerar Microsoft Azure StorSimple-lösningen.  

## <a name="known-issues-in-this-release"></a>Kända problem i den här versionen
hello följande tabell innehåller en översikt över kända problem i den här versionen.  

| Nej. | Funktion | Problem | Kommentarer/lösning | Gäller toophysical enhet | Gäller toovirtual enhet |
| --- | --- | --- | --- | --- | --- |
| 1 |Fabriksåterställning |I vissa fall, när du utför en fabriksåterställning hello StorSimple-enhet kan ha fastnat och visa det här meddelandet: **Återställ toofactory pågår (fas 8)**. Detta händer om du trycker på CTRL + C när hello cmdlet pågår. |Tryck inte på CTRL + C när du initierar en fabriksåterställning. Om du redan är i det här tillståndet kan kontakta Microsoft Support för nästa steg. |Ja |Nej |
| 2 |Disk kvorum |I sällsynta fall om hello merparten av diskar i hello EBOD hölje av 8600-enheten är frånkopplad ledde till Ingen disk i kvorum kommer sedan hello lagringspoolen vara offline. Den förblir offline även om hello diskar återansluts. |Du behöver tooreboot hello enhet. Hello problemet kvarstår kontaktar du Microsoft Support för nästa steg. |Ja |Nej |
| 3 |Fel i moln ögonblicksbild |I sällsynta fall kan en ögonblicksbild i molnet kan misslyckas med felet hello **säkerhetskopiering maxgräns uppnåtts**. Detta inträffar om du överskrider 255 online kloner på hello samma enhet, från hello samma ursprungliga volym som har tagits bort. | |Ja |Ja |
| 4 |Felaktig styrenhets-ID |När en domänkontrollant ersättning utförs kan styrenhet 0 visas som kontrollant 1. Under controller ersättning, när hello bild läses in från hello peer-noden kan hello styrenhets-ID visas först som domänkontrollant hello-peer-ID. I sällsynta fall kan problemet visas efter en omstart. |Ingen användaråtgärd krävs. Den här situationen ska lösas av sig självt när hello controller ersättning är klar. |Ja |Nej |
| 5 |Enheten övervakning diagram |Hello enheten övervakning diagram fungerar inte när grundläggande eller NTLM-autentisering är aktiverat i hello proxyserverkonfiguration för hello enhet i hello StorSimple Manager-tjänsten. |Ändra hello webbproxykonfiguration för hello enheten registrerad med StorSimple Manager-tjänsten så att autentisering har angetts tooNONE. toodo detta, kör hello hello Windows PowerShell för StorSimple Set-HcsWebProxy cmdlet. |Ja |Ja |
| 6 |Lagringskonton |Använda hello Storage service toodelete hello storage-konto är ett scenario som inte stöds. Detta leder tooa situation där användardata inte kan hämtas. | |Ja |Ja |
| 7 |Återställning efter fel |En återställning efter fel inom 24 timmar för katastrofåterställning (DR) stöds inte. | |Ja |Nej |
| 8 |Enheten växling vid fel |Flera redundans av en volymbehållare från hello samma källa enheten toodifferent målenheterna inte stöds. Växling vid fel från en enskild döda enhet toomultiple enheter gör hello volymbehållare på hello först redundansväxlats enhet förlorar dataägarskap. Efter en växling, ska behållarna volymen visas eller fungera annorlunda när de visas i hello klassiska Azure-portalen. | |Ja |Nej |
| 9 |Installation |Under StorSimple-kort för SharePoint-installation behöver du tooprovide en enhet IP-adress för hello installera toofinish har. | |Ja |Nej |
| 10 |Nätverksgränssnitt |Nätverksgränssnitten DATA 2 och DATA 3 har växlats hello programvaran. |Kontakta Microsoft Support om du behöver tooconfigure dessa gränssnitt. |Ja |Nej |

