---
title: "aaaSafety för din StorSimple-enhet | Microsoft Docs"
description: "Beskriver konventioner för säkerhet, riktlinjer och överväganden och förklarar hur toosafely installera och använda din StorSimple-enhet."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: dae6d535-1ca2-4d2b-b221-6819043aa068
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: alkohli
ms.openlocfilehash: cb5e24582c0391d7b68cb5c74586815af4b58a8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="safely-install-and-operate-your-storsimple-device"></a>Installera och använda din StorSimple-enhet på ett säkert sätt
![Varningsikon](./media/storsimple-safety/IC740879.png)
![läsa säkerhet meddelande ikonen](./media/storsimple-safety/IC740885.png) **läsa hälsa och INFORMATION**

Läsa alla hello säkerhet och hälsoinformation i den här artikeln som gäller tooyour Microsoft Azure StorSimple-enhet. Behåll alla hello utskrivna guider som medföljer din StorSimple-enhet för framtida bruk. Fel toofollow instruktioner korrekt konfigurera, använda och hand för den här produkten kan öka hello risken för allvarlig skada eller död, eller skador toohello enheter eller enheter. En [nedladdningsbar version av den här guiden](http://www.microsoft.com/download/details.aspx?id=44233) är också tillgänglig.

## <a name="safety-icon-conventions"></a>Ikonen konventioner för säkerhet
Här följer hello ikoner att du kan hitta när du granskar hello säkerhet försiktighetsåtgärder toobe observerats när du konfigurerar och kör din Microsoft Azure StorSimple-enhet.

| Ikon | Beskrivning |
|:--- |:--- |
| ![Ikon för risk](./media/storsimple-safety/IC740879.png) **risk!** |Visar en farliga situation som, om inte undvikas resulterar i död eller allvarliga skador. Signal ordet är toobe begränsad toohello extrema fall. |
| ![Varningsikon](./media/storsimple-safety/IC740879.png) **varning!** |Visar en farliga situation som, om inte undvikas, kan leda till död eller allvarliga skador. |
| ![Varningsikon](./media/storsimple-safety/IC740879.png) **varning!** |Visar en farliga situation som, om inte undvikas, kan leda till lägre eller måttlig skada. |
| ![Lägg märke till ikonen](./media/storsimple-safety/IC740881.png) **meddelande:** |Anger information som anses vara viktiga, men inte risker relaterade. |
| ![Ikon för elektrisk undanröjs](./media/storsimple-safety/IC740882.png) **elektrisk undanröjs risk** |Hög spänning |
| ![Ikon för tunga vikt](./media/storsimple-safety/IC740883.png) **tunga vikt** | |
| ![Ingen användbar användare delar ikonen](./media/storsimple-safety/IC740879.png) **inga användare användbar delar** |Inte kommer åt såvida inte korrekt utbildade. |
| ![Läsa säkerhet meddelande ikonen](./media/storsimple-safety/IC740885.png)**först läsa alla instruktioner** | |
| ![Risken tipsikon](./media/storsimple-safety/IC740886.png) **tips risk** | |

## <a name="handling-precautions"></a>Åtgärder för hantering
![Varningsikon](./media/storsimple-safety/IC740879.png) ![tunga vikt ikonen](./media/storsimple-safety/IC740883.png) **varning!** 

tooreduce hello risken för skador:

* Ett helt konfigurerade hölje kan väga in too32 kg (70 pund); Försök inte toolift den själv.
* Innan du flyttar hello höljet Se alltid till att två personer som är tillgängliga toohandle hello vikt. Tänk på att en person som försöker toolift denna vikt kan klara skador.
* Lyft inte hello höljet av hello referenser i hello ström och kylning moduler (PCMs) finns på hello enhetens hello baksida. Dessa är inte utformad tootake hello vikt.

## <a name="connection-precautions"></a>Åtgärder för anslutningen
![Varningsikon](./media/storsimple-safety/IC740879.png) ![elektrisk undanröjs ikonen](./media/storsimple-safety/IC740882.png) **varning!**

tooreduce hello sannolikheten för skada, elektrisk undanröjs eller död:

* Koppla från alla ange power för fullständig isolering när drivs av flera AC-källor.
* Koppla permanent från hello enhet innan du flyttar den eller om du tror att den är skadad på något sätt.
* Ange en säker elektriska earth anslutning toohello strömförsörjning elkablar. Kontrollera att hello erfarenhet av att arbeta i hello höljet uppfyller hello nationella och lokala innan du tillämpar power.
* Se till att hello anslutning alltid är frånkopplad tidigare toohello borttagning av en PCM från hello höljet.
* Hänsyn till att hello plugin på hello ange strömsladd är hello huvudsakliga koppla från enheten, kontrollera att hello uttag befinner sig nära hello utrustning och är lätt tillgängliga.

![Varningsikon](./media/storsimple-safety/IC740879.png) ![elektrisk undanröjs ikonen](./media/storsimple-safety/IC740882.png) **varning!**

tooreduce hello sannolikheten överhettning eller brand från hello elektriska anslutningar:

* Ge en lämplig kraftkälla elektriska överlagring skydd toomeet hello krav som anges i hello tekniska specifikationer.
* Använd inte bifurcated strömkablar (leads ”Y”).
* toocomply med tillämpliga säkerhet, utsläpp och termiska krav, inga försättsblad ska tas bort och alla fack måste fyllas i med plugin-program eller enhet tomma värden.
* Se till att utrustning hello används på ett sätt som anges av hello tillverkare. Om den här utrustningen används på ett sätt som inte har angetts av hello tillverkare skrivas hello skyddet som hello utrustning.

![Lägg märke till ikonen](./media/storsimple-safety/IC740881.png) **meddelande:**

För hello fungera korrekt din utrustning och tooprevent produkten skada:

* hello RJ45 portar på hello baksidan hello enheten är för en Ethernet-anslutning. Dessa får inte vara anslutna tooa telekommunikation nätverk.
* Vara säker på att tooinstall hello-enhet i ett rack som kan hantera en fram till tillbaka kylning design.
* Alla plugin-program och tom nivåer tillhör hello Systemomslutning. Dessa måste bara tas bort när en ersättning kan läggas till direkt. hello system måste inte köras utan alla moduler eller tomma värden på plats.

## <a name="rack-system-precautions"></a>Rack system försiktighetsåtgärder
hello måste följande krav beaktas när du monterar hello-enhet i en rack kabinettfil.

![Varningsikon](./media/storsimple-safety/IC740879.png) ![risken tipsikon](./media/storsimple-safety/IC740886.png) **varning!**

tooreduce hello sannolikheten för skada från ett tips över:

* hello rack design ska ha stöd för hello totala vikten av hello installerat höljen och bör omfatta stabiliserande funktioner lämplig tooprevent hello rack från vältning eller som pushas under under installationen eller normal användning.
* När du läser in ett rack, Fyll hello rack nedifrån hello upp och tomt från hello uppifrån och ned.
* Skjut inte mer än en höljet out-of-rack hello vid tiden tooavoid hello risk för hello rack toppling.

![Varningsikon](./media/storsimple-safety/IC740879.png) ![elektrisk undanröjs ikonen](./media/storsimple-safety/IC740882.png) **varning!**

tooreduce hello sannolikheten för skada, elektrisk undanröjs eller död:

* hello rack ska ha en säker elektriska distributionssystem. Det måste ange över aktuella skydd för hello höljet och måste inte överlagras med hello totala antalet höljen installerad. hello elektrisk ström för förbrukning klassificering visas på hello märkskylten bör beaktas.
* hello elektriska distributionssystem måste ange en tillförlitlig grunden för varje enhet i hello rack.
* hello utformning av hello elektriska distributionssystem måste ta hänsyn hello totala grunden läckage aktuella från alla strömkällor i alla höljen. Observera att varje strömförsörjning i varje hölje grunden läckage ström 1.0 mA högst 60 Hz 264 v. hello rack kan kräva namngivning med ”hög LÄCKAGE aktuella. Grunden (earth) anslutning krävs innan du ansluter en tillgång ”.
* hello rack, när konfigurerats med hello höljen, måste uppfylla säkerhetskraven för hello i UL 60950-1 och IEC 60950-1/EN 60950-1.

![Lägg märke till ikonen](./media/storsimple-safety/IC740881.png) **meddelande:**

För hello rätt kylningen av rack systemet:

* Se till att hello rack design tar i beräkningen hello maximala höljet operativsystem temperatur av 35 grader (95 grader Fahrenheit).
* hello-systemet körs med low-pressure, bakre avgaser installation (ligger som skapas av rack dörrar och hinder inte tooexceed 5 Pascal [0,5 mätare]).

## <a name="power-cooling-module-pcm-precautions"></a>Power kylning modul (PCM) försiktighetsåtgärder
hello-enheten är utformad toooperate med två PCMs. Varje hello PCMs har en strömförsörjning och fläktar med dubbla axel. Under ett kritiskt tillstånd tillåter hello systemet till ett fel på en strömförsörjning medan normal drift. Två PCMs (och därför strömförsörjning) måste alltid vara installerad. Redundant strömförsörjning tillhandahåller inte en enda PCM. Därför kan hello bristande även en PCM medföra driftavbrott eller förlust av data.

![Varningsikon](./media/storsimple-safety/IC740879.png) ![elektrisk undanröjs ikonen](./media/storsimple-safety/IC740882.png) **varning!**

tooreduce hello sannolikheten för skada, elektrisk undanröjs eller död:

* Ta inte bort hello omfattar från hello PCM. Det finns en risk för elektrisk undanröjs i. tooreturn hello PCM och få en ny [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md).

![Lägg märke till ikonen](./media/storsimple-safety/IC740881.png) **meddelande:**

För hello fungera korrekt din utrustning och tooprevent produkten skada:

* Du måste ersätta hello misslyckades PCM inom 24 timmar. När en PCM har tagits bort för ersättning, måste hello ersättning slutföras inom 10 minuter efter borttagningen.
* Ta inte bort en PCM såvida inte en ersättning kan installeras direkt. hello höljet köras inte utan alla moduler på plats.

## <a name="electrostatic-discharge-esd-precautions"></a>Åtgärder för elektrostatisk avslutning ESD)
![Lägg märke till ikonen](./media/storsimple-safety/IC740881.png) **meddelande:**

Se hello följande ESD-relaterade åtgärder.

* Se till att du har installerat och kontrolleras en lämplig antistatiska handleden eller Ankelstövlar bandet.
* Se alla vanliga ESD försiktighetsåtgärder när du arbetar med moduler och komponenter.
* Undvik kontakt med bakplan komponenter och modulen kopplingar.
* ESD skada omfattas inte av garanti.

## <a name="battery-disposal-precautions"></a>Batteri avyttring försiktighetsåtgärder
hello-strömförsörjning använder en särskild batteri tooprotect hello innehållet i minne under tillfälligt, kortsiktig strömavbrott. hello batteri sitter i hello PCM. Behåll hello följande information i åtanke om hello batteri.

![Varningsikon](./media/storsimple-safety/IC740879.png) **varning!**

tooreduce hello risken för shorts, brand, explosion, skador eller död:

* Avyttra används batterierna i enlighet med nationell/regional förordningar.
* Ta isär koden, krossa, eller värma ovan 60 grader (140 grader Fahrenheit) eller inte förbränna. Ersätt hello PCM batteri med ett angivet batteri. Användning av en annan batteri kan utgöra en risk för brand eller explosion.
* Använd skyddande linjeslut batteridrift hello om dessa tas bort från hello strömförsörjning.

![Lägg märke till ikonen](./media/storsimple-safety/IC740881.png) **meddelande:**

När leverans eller annars transport hello batterierna av luft följer hello IATA Lithium batteri vägledning dokumentet finns på [http://www.iata.org/whatwedo/cargo/dgr/Pages/lithium-batteries.aspx](http://www.iata.org/whatwedo/cargo/dgr/Pages/lithium-batteries.aspx)

När du har granskat dessa meddelanden för säkerhet, hello nästa steg är toounpack, rack och kabelanslut din enhet.

## <a name="next-steps"></a>Nästa steg
* För 8100-enhet, gå för[installerar StorSimple 8100-enhet](storsimple-8100-hardware-installation.md).
* För 8600-enhet, gå för[installera din StorSimple-8600-enhet](storsimple-8600-hardware-installation.md).

