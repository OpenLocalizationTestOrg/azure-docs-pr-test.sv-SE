---
title: "guide för aaaSupportability och tillbakadragning princip för Azure-Gästoperativsystem | Microsoft Docs"
description: "Innehåller information om Microsofts support för när det gäller toohello Azure-Gästoperativsystem som används av molntjänster."
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
editor: 
ms.assetid: 919dd781-4dc6-4e50-bda8-9632966c5458
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 5/26/2017
ms.author: raiye
ms.openlocfilehash: 6a86c42ac95d12bbf116d900b7afb26fc3fe34e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-guest-os-supportability-and-retirement-policy"></a>Azure-Gästoperativsystem support och tillbakadragning princip
hello information på den här sidan gäller toohello Azure gästoperativsystemet ([Gästoperativsystem](cloud-services-guestos-update-matrix.md)) för molntjänster worker och webbtjänst roller (PaaS). Den gäller inte tooVirtual datorer (IaaS).

Microsoft har en publicerade [Supportpolicy för hello Gästoperativsystem](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq). hello-sidan som du läser nu beskriver hur hello implementeras.

hello Policy

1. Microsoft stöder **minst hello senaste två familjer av hello Gästoperativsystem**. När en familj har dragits tillbaka har kunder 12 månader från hello officiella pensionering datum tooupdate tooa nyare stöds gäst OS-familj.
2. Microsoft stöder **minst hello senaste två versioner av hello stöds gäst-OS-familjer**.
3. Microsoft stöder **minst hello senaste två versioner av hello Azure SDK**. När en version av hello SDK har dragits tillbaka har kunder 12 månader från hello officiella pensionering datum tooupdate tooa senare version.

Ibland kan kan fler än två familjer eller versioner stödjas. Officiell Gästoperativsystem supportinformation visas på hello [versioner för gästoperativsystem till Azure och SDK-Kompatibilitetsmatris](cloud-services-guestos-update-matrix.md).

## <a name="when-a-guest-os-family-or-version-is-retired"></a>När en gäst-OS-familjen eller version är inaktiverad
En ny Gästoperativsystem **familj** introduceras stund efter hello-versionen av en ny officiella version av operativsystemet Windows Server för hello. När en ny gäst-OS-familj introduceras Microsoft dra tillbaka hello äldsta gäst-OS-familjen.

Nya Gästoperativsystem **versioner** introduceras om varje månad tooincorporate hello senaste MSRC-uppdateringar. På grund av hello regelbundna månatliga uppdateringar är en gäst-OS-version normalt inaktiverad 60 dagar efter versionen. Den här aktiviteten för att hålla minst två gäst-OS-versioner för varje familj som är tillgängliga för användning.

### <a name="process-during-a-guest-os-family-retirement"></a>Bearbeta under en gäst-OS-familjen pensionering
När hello pensionering har meddelats har kunder en 12 månad ”” övergångsperioden innan hello äldre familj officiellt tas bort från tjänsten. Den här övergångstid förlängas gottfinnande hello Microsoft. Uppdateringar publiceras på hello [versioner för gästoperativsystem till Azure och SDK-Kompatibilitetsmatris](cloud-services-guestos-update-matrix.md).

En stegvis pensionering process börjar sex (6) månader i hello övergångsperioden. Under denna tid:

1. Microsoft meddelar kunder i hello ur bruk.
2. hello nyare version av hello Azure SDK stöder inte hello dragits tillbaka gäst-OS-familjen.
3. Nya distributioner och redeployments molntjänster tillåts inte på hello dragits tillbaka familj

Microsoft kommer att fortsätta toointroduce ny gäst-OS-version införliva hello senaste MSRC uppdateringarna tills hello sista dagen i hello övergångsperioden kallas hello ”upphör att gälla”. På hello upphör att gälla, kommer stöds inte under hello Azure-serviceavtalet molntjänster fortfarande körs. Microsoft har hello gottfinnande tooforce uppgradera, ta bort eller stoppa tjänsterna efter detta datum.

### <a name="process-during-a-guest-os-version-retirement"></a>Bearbeta under en gäst-OS-Version ur bruk
Om kunder Gästoperativsystem tooautomatically uppdateringen, har de aldrig tooworry om hur du hanterar gäst-OS-versioner. De kommer alltid att använda hello senaste gäst-OS-versionen.

Gäst-OS-versioner släpps varje månad. På grund av hello frekvensen för vanliga versioner har varje version en fast livslängd.

60 dagar i hello livslängd en version som är ”*inaktiverad*”. ”Inaktiverat” innebär att hello version tas bort från hello-portalen. hello-versionen kan inte längre anges från hello CSCFG-konfigurationsfil. Befintliga distributioner lämnas körs. Men nya distributioner och distributioner av koden och konfigurationen tooexisting tillåts inte.

Tid när du har blivit ”inaktiverad” Hej gäst-OS-version ”*upphör att gälla*” och alla installationer som fortfarande kör den här versionen uppgraderas och ange tooautomatically uppdatering hello Gästoperativsystem i hello framtida. Förfallodatum görs i batchar så hello tidsperiod från avstängning tooexpiration kan variera.

Dessa punkter får göras längre på Microsofts gottfinnande tooease kunden övergångar. Ändringar förmedlas på hello [versioner för gästoperativsystem till Azure och SDK-Kompatibilitetsmatris](cloud-services-guestos-update-matrix.md).

### <a name="notifications-during-retirement"></a>Meddelanden under ur bruk
* **Family pensionering** <br>Microsoft använder blogginlägg och portalmeddelandet. Kunder som fortfarande använder en pensionerad gäst-OS-familj meddelas via direkt kommunikation (e-post, portal meddelanden, telefonsamtal) tooassigned tjänstadministratörer. Alla ändringar som kommer att publiceras toothis sida och hello RSS-feed visas hello början av den här sidan.
* **Version ur bruk** <br>Alla ändringar och hello datum som de sker kommer att publiceras toothis sida och hello RSS-feed visas hello början av den här sidan, inklusive versionen, inaktiverad och upphör att gälla. Tjänster-administratörer får e-post om de har distributioner som körs på en inaktiverad gäst-OS-version eller familjen. hello tidtagningen av dessa e-postmeddelanden kan variera. I allmänhet är de minst en månad före avstängning, även om den här tidpunkten inte är ett officiella SLA.

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar
**Hur kan jag för att minimera hello konsekvenserna av migrering?**

Vi rekommenderar att du använder senaste gäst-OS-familjen för att utforma din molntjänster.

1. Börja planera din migrering tooa nyare familj tidigt.
2. Ställ in tillfällig test distributioner tootest din molntjänst som körs på hello nya familj.
3. Ange din gäst-OS-version för**automatisk** (osVersion = * i hello [.cscfg](cloud-services-model-and-package.md#cscfg) fil) så hello migrering toonew gäst-OS-versioner sker automatiskt.

**Vad händer om min webbprogram kräver djupare integrering med hello OS?**

Om din web programarkitektur är beroende av underliggande funktioner i hello operativsystemet, använda plattformsfunktioner stöds som [Start uppgifter](cloud-services-startup-tasks.md) eller andra utökningsbarhet mekanismer. Du kan också använda [Azure Virtual Machines](https://azure.microsoft.com/documentation/scenarios/virtual-machines/) (IaaS-infrastruktur som en tjänst), där du ansvarar för att underhålla hello underliggande operativsystemet.

## <a name="next-steps"></a>Nästa steg
Granska hello senaste [Gästoperativsystem släpper](cloud-services-guestos-update-matrix.md).
