---
title: "aaaEstimate nätverksbandbredd för Azure RemoteApp | Microsoft Docs"
description: "Läs mer om hello kraven på nätverksbandbredd för din Azure RemoteApp-samlingar och appar."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 3127f4c7-f532-46c3-ba9b-649f647abec1
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 675ee82f26ddb46a3bb3e0ee95ed397e4064e45f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="estimate-azure-remoteapp-network-bandwidth-usage"></a>Beräkna Azure RemoteApp användningen av nätverksbandbredd
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Azure RemoteApp använder hello Remote Desktop Protocol (RDP) toocommunicate mellan program som körs i hello Azure-molnet och dina användare. Den här artikeln innehåller några grundläggande riktlinjer som du kan använda tooestimate som nätverkets användning och potentiellt utvärdera användningen av nätverksbandbredd per Azure RemoteApp-användare.

Uppskatta bandbreddsanvändning per användare är mycket komplex och kräver kör flera program samtidigt i samtidig körning scenarier där program kan påverka prestanda varandras baserat på deras behovet av nätverksbandbredd. Även hello typ av fjärrskrivbordsklienten (till exempel Mac-klienten jämfört med HTML5-klienten) kan leda toodifferent bandbredd resultat. toohelp som du arbetar med dessa problem kommer vi Bryt hello Användningsscenarier i flera hello vanliga kategorier tooreplicate verkliga scenarier. (Där hello verkligt scenario är naturligtvis en blandning av kategorier och skiljer sig av användaren.)

Innan vi gå längre - Observera att vi antar RDP tillhandahåller en bra tooexcellent för de flesta Användningsscenarier i nätverk med en fördröjning under 120 ms och bandbredd över 5 MB – detta baseras på RDPS möjlighet toodynamically justera genom att använda hello tillgängligt nätverk bandbredd och hello uppskattade bandbredd programbehov. Den här artikeln gäller utöver de ”de flesta Användningsscenarier” toolook hello kant var scenarier börjar toounwind och användarupplevelse börjar toodegrade.

Nu se följande artiklar för hello information, inklusive faktorer tooconsider baslinjen rekommendationer och vad vi inte inkluderade i våra bedömningar hello.

* [Hur får nätverkets bandbredd och kvaliteten på några fungerar tillsammans?](remoteapp-bandwidthexperience.md)
* [Testa din användandet av nätverkets bandbredd med några vanliga scenarier](remoteapp-bandwidthtests.md)
* [Snabb riktlinjer om du inte har hello tid eller möjlighet tootest](remoteapp-bandwidthguidelines.md)

## <a name="what-are-we-not-including"></a>Vad vi inte inkluderar?
När du har läst hello föreslagna testerna och våra rekommendationer för övergripande (och vilket är allmänna), Tänk på att det finns flera faktorer som vi inte ansett. Till exempel hello user experience komplikationer tillhandahålls av hello asymmetriska uppbyggnad överför kontra download bandbredd. hello asymmetriska arten av de flesta Wi-Fi-nätverk påverkar dessutom hello prestanda och hello user experience uppfattning. Hello underordnade trafik kan prioriteras lägre än hello uppströms, vilket kan öka hello antalet förlorade video eller ljud ramar och därför påverka hello användaren uppfattning av hello direktuppspelning för interaktiva scenarier. Du kan köra egna experiment toosee vad som är bra för din specifika användningsfall och nätverk.

Även om diskuterar vi omdirigering av enheter vi inte hänsyn till övervägande hello bandbredd effekten av hello nätverkstrafik på grund av anslutna enheter, t.ex. lagring, skrivare, skannrar, webbkameror och andra USB-enheter. hello effekten av dessa enheter ger hello bandbreddsbehov spikar i diagrammet tillfälligt vanligtvis och försvinner när hello åtgärden är klar. Men om gjort ofta att bandbredden begäran kan vara ganska märkbar.

Vi också inte handlar om hur en användare kan påverka andra användare inom hello samma nätverk. Till exempel en användare som använder 4K videon i ett 100 MB/s nätverk kan avsevärt påverka andra användare på samma nätverk försöker toodo hello samma aktivitet. Tyvärr hämtar progressivt svårare toodetermine hello effekten av samtidig användning toogive en gemensam eller omfattar alla rekommendation om hur hello utförs på aggregering. Allt vi kan säga är att hello underliggande protokollet teknik som gör hello kan utnyttja hello tillgänglig bandbredd, men den har sina begränsningar.

