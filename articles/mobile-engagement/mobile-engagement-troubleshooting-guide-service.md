---
title: "aaaAzure Mobile Engagement Troubleshooting Guide - tjänsten"
description: "Felsökning för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8b4275da-c0b4-4690-824a-48e9d7a1fc6e
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: cf48db323a873ccef95946f7bb26e8d7473c002f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-service-issues"></a>Felsökningsguide för problem med tjänsten
hello följande är möjliga problem som kan uppstå med hur Azure Mobile Engagement körs.

## <a name="service-outages"></a>Serviceavbrott
### <a name="issue"></a>Problem
* Problem som visas toobe som orsakas av Azure Mobile Engagement driftstopp.

### <a name="causes"></a>Orsaker
* Problem som visas toobe på grund av driftstopp för Azure Mobile Engagement kan orsakas av flera olika problem:
  * Isolerade problem som ursprungligen visas systemfel tooall med Azure Mobile Engagement
  * Kända problem som orsakas av server-avbrott (inte alltid visas i status-servern):
  * Schemaläggning av fördröjningar, målinriktning fel, Aktivitetsikon uppdateringsproblem, statistik sluta samla in Push slutar fungera kan API: er slutar att fungera, nya appar eller användare inte skapas, DNS-fel och Timeout-fel i hello Användargränssnittet eller API Apps på en enhet.
  * Molnet beroende avbrott [Azure Tjänststatus](http://status.azure.com/)
  * Push Notification Services (PNS) beroende avbrott
  * App Store avbrott

1) tootest toosee om hello problem är systemfel kan du testa hello samma funktionen från en annan

* Azure Mobile Engagement integrerat program
* Testenhet
* Testa enhetens OS-version
* Kampanj
* Användarkonto
* Webbläsaren (IE, Firefox, Chrome osv.)
* Dator

2) tootest om hello problem påverkar endast hello användargränssnitt eller hello API: er:

* Testa hello samma funktionen från båda hello Azure Mobile Engagement UI och hello Azure Mobile Engagement API: er.

3) tootest om hello problem med nätverket mobiltelefon:

* Testa när anslutna toohello Internet via Wi-Fi och medan du är ansluten via din mobiltelefon 3G-nätverk.
* Bekräfta att brandväggen inte blockerar alla hello Azure Mobile Engagement IP-adresser och portar.

4) tootest om hello problem med din enhet:

* Testa om enheten är kan tooconnect tooAzure Mobile Engagement med en annan integrerad Azure Mobile Engagement-app.
* Kontrollera att du kan generera händelser, jobb och krascher från din telefon kan ses i hello Azure Mobile Engagement-Användargränssnittet. 
* Testa om du kan skicka push-meddelanden från hello Azure Mobile Engagement UI tooyour enhet baserat på sin enhet-ID. 

5) tootest om hello problem med din App:

* Installera och testa programmet från en emulator i stället för från en fysisk enhet:

6) tootest om hello problem med OS-uppgraderingar tooend användaren enheter, vilket kräver en uppgradering tooresolve SDK:

* Testa programmet på olika enheter med olika versioner av hello OS.
* Bekräfta att du använder hello senaste versionen av hello SDK.

## <a name="connectivity-and-incorrect-information-issues"></a>Anslutning och felaktig Information problem
### <a name="issue"></a>Problem
* Problem som loggar in på hello Azure Mobile Engagement-Användargränssnittet.
* Anslutningsfel med hello Azure Mobile Engagement-API.
* Problem med att överföra appen Info taggar via hello Device API.
* Problem vid hämtning av loggar eller exporterade data från Azure Mobile Engagement.
* Felaktig information visas i hello Azure Mobile Engagement-Användargränssnittet.
* Felaktig information visas i Azure Mobile Engagement-loggar.

### <a name="causes"></a>Orsaker
* Bekräfta ditt användarkonto har tillräcklig behörighet tooperform hello aktivitet.
* Bekräfta att hello problemet inte är isolerad tooone dator eller det lokala nätverket.
* Bekräfta att den hello Azure Mobile Engagement-tjänsten har ingen rapporterade stillestånd.
* Bekräfta att din App Info märka filer följer alla de här reglerna:
  * Använd endast hello UTF8-teckenuppsättning (hello ANSI-teckenuppsättningen stöds inte).
  * Använda ett kommatecken ””, som hello avgränsningstecknet (du kan öppna en service begäran toorequest toochange hello .csv avgränsningstecknet från ett kommatecken ””, tooanother-tecken, till exempel ett semikolon ””;).
  * Använd alla gemen för booleska värden ”true” och ”false”.
  * Använda en fil som är mindre än hello maximal filstorlek 35 MB.

