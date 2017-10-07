---
title: aaaTraffic Manager Slutpunktstyper | Microsoft Docs
description: "Den här artikeln beskrivs olika typer av slutpunkter som kan användas med Azure Traffic Manager"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 4e506986-f78d-41d1-becf-56c8516e4d21
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/29/2017
ms.author: kumud
ms.openlocfilehash: 787412ac6207f76791bf3ff753d1df2767b1a964
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="traffic-manager-endpoints"></a>Traffic Manager-slutpunkter
Microsoft Azure Traffic Manager kan toocontrol hur nätverkstrafiken är distribuerade tooapplication distributioner som körs i olika datacenter. Konfigurera varje programdistribution som en slutpunkt i Traffic Manager. När Traffic Manager tar emot en begäran om DNS-, väljer en tillgänglig slutpunkt tooreturn i hello DNS-svar. Traffic manager baserar hello val på hello aktuell slutpunkt status och routning av nätverkstrafik hello-metoden. Mer information finns i [så Traffic Manager fungerar](traffic-manager-how-traffic-manager-works.md).

Det finns tre typer av slutpunkten som stöds av Traffic Manager:
* **Azure-slutpunkter** används för tjänster i Azure.
* **Externa slutpunkter** används för tjänster som finns utanför Azure, antingen lokalt eller med en annan värd provider.
* **Kapslade slutpunkter** är används toocombine Traffic Manager-profiler toocreate mer flexibel routning av nätverkstrafik scheman toosupport hello behov av större och mer komplexa distributioner.

Det finns ingen begränsning för hur slutpunkter av olika typer som kombineras i en enda Traffic Manager-profil. Varje profil kan innehåller en blandning av olika typer av slutpunkter.

hello följande avsnitt beskrivs varje typ av slutpunkt i större djup.

## <a name="azure-endpoints"></a>Azure-slutpunkter

Azure-slutpunkter används för Azure-baserade tjänster i Traffic Manager. följande Azure resurstyper hello stöds:

* 'Klassiskt' IaaS-VM och PaaS-molntjänster.
* Web Apps
* PublicIPAddress resurser (som kan vara anslutna tooVMs antingen direkt eller via en Azure belastningsutjämnare). Hej publicIpAddress måste ha ett DNS-namn som tilldelats toobe i en Traffic Manager-profil.

PublicIPAddress resurser är Azure Resource Manager-resurser. De finns inte i hello klassiska distributionsmodellen. Därför är de enda stöds i Traffic Manager-Azure Resource Manager upplevelser. hello stöds andra typer av slutpunkter via både Resource Manager och hello klassiska distributionsmodellen.

När du använder Azure-slutpunkter, identifierar Traffic Manager när en 'Klassiskt' IaaS VM, molnbaserad tjänst eller ett webbprogram stoppas och startas. Den här statusen visas i status för hello endpoint. Se [Traffic Manager slutpunktsövervakning](traffic-manager-monitoring.md#endpoint-and-profile-status) mer information. När hello underliggande tjänsten har stoppats Traffic Manager inte att utföra hälsokontroller för slutpunkten eller trafiken toohello slutpunkt. Inga Traffic Manager fakturering händelser inträffar för hello stoppade instansen. När hello-tjänsten har startats om återupptar fakturering och hello-slutpunkten är berättigad tooreceive trafik. Denna identifiering gäller inte tooPublicIpAddress slutpunkter.

## <a name="external-endpoints"></a>Externa slutpunkter

Externa slutpunkter används för utanför Azure-tjänster. Till exempel en tjänst finns lokalt eller med en annan provider. Externa slutpunkter kan användas enskilt eller tillsammans med Azure-slutpunkter i hello samma Traffic Manager-profilen. Kombinera Azure-slutpunkter med externa slutpunkter kan olika scenarier:

* Använd Azure tooprovide ökat redundans för ett befintligt lokalt i antingen en aktiv-aktiv eller aktivt-passivt växling modell.
* tooreduce program svarstid för användare hello världen, utöka en befintliga lokala program tooadditional geografiska platser i Azure. Mer information finns i [Traffic Manager-prestanda' routning av nätverkstrafik](traffic-manager-routing-methods.md#performance).
* Använd Azure tooprovide ytterligare kapacitet för ett befintligt lokalt program kontinuerligt eller som en ”burst-to-cloud-lösningen toomeet en topp i begäran.

I vissa fall är det användbart toouse externa slutpunkter tooreference Azure services (exempel finns hello [vanliga frågor och svar](traffic-manager-faqs.md#traffic-manager-endpoints)). I det här fallet debiteras hälsokontroller med hello Azure-slutpunkter hastighet, inte hello externa slutpunkter hastighet. Men till skillnad från Azure-slutpunkter, om du stoppar eller ta bort den underliggande tjänsten hello, hälsokontroll fakturering fortsätter tills du inaktivera eller ta bort hello slutpunkt i Traffic Manager.

## <a name="nested-endpoints"></a>Kapslade slutpunkter

Kapslade slutpunkter kombinera flera Traffic Manager profiler toocreate flexibla routning av nätverkstrafik scheman och support hello för större, komplexa distributioner. Med kapslade slutpunkter läggs en 'child'-profil som en slutpunkt tooa 'överordnad' profil. Båda hello underordnade och överordnade profiler kan innehålla slutpunkter av valfri typ, inklusive andra kapslade profiler. Mer information finns i [kapslade Traffic Manager-profiler](traffic-manager-nested-profiles.md).

## <a name="web-apps-as-endpoints"></a>Web Apps som slutpunkter

Vissa ytterligare förutsättningar gäller när du konfigurerar webbprogram som slutpunkter i Traffic Manager:

1. Endast Web Apps vid hello ”Standard” SKU eller över är berättigade för användning med Traffic Manager. Försöker tooadd ett webbprogram på en lägre SKU misslyckas. Nedgradera hello resulterar SKU av en befintlig Webbapp i Traffic Manager inte längre skicka trafik toothat Web App.
2. När en slutpunkt tar emot en HTTP-begäran används hello-värd-huvud i hello begäran toodetermine vilka Web App service hello begära. hello värdadressen innehåller hello DNS-namn som används för tooinitiate hello begäran, till exempel 'contosoapp.azurewebsites.net'. toouse ett annat DNS-namn med ditt webbprogram, hello DNS-namn måste vara registrerad som ett anpassat domännamn för hello App. När du lägger till en slutpunkt för webbprogram som en Azure slutpunkt registreras automatiskt hello Traffic Manager-profilen DNS-namn för hello App. Den här registreringen tas bort automatiskt när hello slutpunkt har tagits bort.
3. Varje Traffic Manager-profilen kan ha högst en Web App slutpunkt från varje Azure-region. toowork runt för den här begränsningen kan du konfigurera ett webbprogram som en extern slutpunkt. Mer information finns i hello [vanliga frågor och svar](traffic-manager-faqs.md#traffic-manager-endpoints).

## <a name="enabling-and-disabling-endpoints"></a>Aktivera och inaktivera slutpunkter

Om du inaktiverar en slutpunkt i Traffic Manager kan vara användbart tootemporarily ta bort trafik från en slutpunkt som är i underhållsläge eller som omdistribueras. Det kan vara återaktiveras när hello-slutpunkten körs igen.

Slutpunkter kan aktiveras och inaktiveras via hello Traffic Manager-portalen PowerShell, CLI eller REST API som stöds i både Resource Manager och hello klassiska distributionsmodellen.

> [!NOTE]
> Inaktiveringen av en Azure slutpunkt har inget toodo med dess Distributionsstatus i Azure. En Azure-tjänsten (t.ex. en virtuell dator eller Web App fortfarande körs och kan tooreceive trafik även om inaktiverad i Traffic Manager. Trafik kan åtgärdas direkt toohello tjänsten instans i stället för via hello Traffic Manager-profilen DNS-namn. Mer information finns i [hur Traffic Manager fungerar](traffic-manager-how-traffic-manager-works.md).

hello aktuella förutsättningarna för varje slutpunkt tooreceive trafik beror på hello följande faktorer:

* Hej profilstatusen (aktiverat/inaktiverat)
* status för endpoint hello (aktiverat/inaktiverat)
* hello resultaten av hello hälsokontroller för denna slutpunkt

Mer information finns i [Traffic Manager slutpunktsövervakning](traffic-manager-monitoring.md#endpoint-and-profile-status).

> [!NOTE]
> Eftersom Traffic Manager fungerar på hello DNS-nivå, är det tooinfluence befintliga anslutningar tooany slutpunkten. När en slutpunkt är inte tillgänglig, leder Traffic Manager nya anslutningar tooanother tillgänglig slutpunkt. Hello värden bakom hello inaktiverat eller inte felfri slutpunkt kan dock fortsätta tooreceive trafik via befintliga anslutningar förrän sessionerna avslutas. Program bör begränsa hello session varaktighet tooallow trafik toodrain från befintliga anslutningar.

Om alla slutpunkter i en profil är inaktiverade, eller om själva hello profilen är inaktiverad, skickar en 'NXDOMAIN' svar tooa nya DNS-fråga med Traffic Manager.


## <a name="next-steps"></a>Nästa steg

* Läs [hur Traffic Manager fungerar](traffic-manager-how-traffic-manager-works.md).
* Lär dig mer om Traffic Manager [endpoint övervaknings- och automatisk redundans](traffic-manager-monitoring.md).
* Lär dig mer om Traffic Manager [trafikroutningsmetoder](traffic-manager-routing-methods.md).
