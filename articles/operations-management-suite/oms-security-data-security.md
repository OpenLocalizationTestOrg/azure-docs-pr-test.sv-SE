---
title: "aaaOperations Management Suite säkerhets- och granska lösningen datasäkerhet | Microsoft Docs"
description: "Det här dokumentet beskriver hur data hanteras och skyddas i säkerhets- och granskningslösningen i Operations Management Suite."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 9cdf7deb-2a30-4672-b89f-71179ee8326a
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: 9c4181b3b491e4f7f0c57d7252eca78a819722d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="operations-management-suite-security-and-audit-solution-data-security"></a>Datasäkerhet i säkerhets- och granskningslösningen i Operations Management Suite
toohelp kunder förebygga, upptäcka och åtgärda toothreats, [Operations Management Suite (OMS) säkerhets- och granska lösningen](operations-management-suite-overview.md) samlar in och bearbetar data om dina resurser, som omfattar:

* Säkerhetshändelselogg
* ETW-händelser (Event Tracing for Windows)
* AppLocker-granskningshändelser
* Windows-brandväggslogg
* Advanced Threat Analytics-händelser
* Resultat från utvärdering av säkerhetsbaslinje
* Resultat från utvärdering av program mot skadlig kod
* Resultat från utvärdering av uppdateringar/korrigeringar
* Systemloggar dataströmmar som explicit har aktiverats på hello-agent

Vi gör starka åtaganden tooprotect hello sekretess och säkerhet för dessa data. Microsoft följer riktlinjer för toostrict efterlevnad och säkerhet – från kodning toooperating en tjänst.
Den här artikeln beskriver hur data hanteras och skyddas i säkerhets- och granskningslösningen i OMS.

## <a name="data-sources"></a>Datakällor
OMS säkerhets- och granska lösningen analysera data från virtuella datorer och fysiska datorer där hello OMS-agenten är installerad. Säkerhets- och granskningslösningen i OMS kan samla in konfigurationsinformation om säkerhetshändelser, t.ex. Windows-händelser, granskningsloggar, IIS-loggar och syslog-meddelanden. Exempel på den här typen av data är: operativsystemets typ och version, aktiva processer, datornamn, IP-adresser, inloggad användare och klient-ID.  

## <a name="data-protection"></a>Dataskydd
**Dataavgränsning**: Data lagras logiskt separat på varje komponent i hela hello-tjänsten. Alla data taggas efter organisation. Den här taggning kvarstår under hello data livscykel och den tillämpas på varje nivå av hello-tjänsten. 

**Dataåtkomst**: tooprovide säkerhetsrekommendationer och undersöka potentiella säkerhetshot, Microsoft-Personal kan komma åt information som samlas in och analyseras av tjänster. Vi följer toohello [licensvillkor för Microsoft Online Services](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) och [sekretesspolicy](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), vilket tillstånd att Microsoft inte använder kundinformation eller härledd information från den för reklam eller liknande kommersiella ändamål. tooprovide säkerhetsrekommendationer och undersöka potentiella säkerhetshot, Microsoft-Personal kan komma åt information som samlas in och analyseras av tjänster. Vi använder bara kundinformation som behövs tooprovide du med Azure-tjänsterna, inklusive syfte som är kompatibla med dessa tjänster. Du kan behålla alla rättigheter tooyour egna data.

**Använd data**: Microsoft använder mönster och hotinformation ses över flera innehavare tooenhance våra förhindra och upptäcka funktioner; vi gör i enlighet med hello sekretess åtaganden som beskrivs i vår [sekretess Instruktionen](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

> [!NOTE]
> Dataplats har konfigurerats på hello OMS arbetsytan nivå under hello Skapa arbetsyta, vilket är en del av hello inledande OMS säkerhet och granska konfigurationen.
> 
> 

## <a name="see-also"></a>Se även
I det här dokumentet har du lärt dig hur data hanteras och skyddas i OMS. toolearn mer om OMS säkerhets- och granska lösningen, se:

* [Översikt över Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Övervakning och svarar tooSecurity varningar i Operations Management Suite säkerhet och granska lösning](oms-security-responding-alerts.md)
* [Övervaka resurser i säkerhets- och granskningslösningen i Operations Management Suite](oms-security-monitoring-resources.md)

