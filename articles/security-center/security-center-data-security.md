---
title: "aaaAzure datasäkerhet i Security Center | Microsoft Docs"
description: "Det här dokumentet beskriver hur data hanteras och skyddas i Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 33f2c9f4-21aa-4f0c-9e5e-4cd1223e39d7
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2017
ms.author: yurid
ms.openlocfilehash: 30f8b11272dc5df6d485608abdaa62ba57e63f23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-data-security"></a>Datasäkerhet i Azure Security Center
toohelp kunder förebygga, upptäcka och åtgärda toothreats, Azure Security Center samlar in och bearbetar säkerhetsrelaterade data, inklusive konfigurationsinformation, metadata, händelseloggar, kraschdumpfiler och mycket mer. Microsoft följer riktlinjer för toostrict efterlevnad och säkerhet – från kodning toooperating en tjänst.

Den här artikeln beskriver hur data hanteras och skyddas i Azure Security Center.

>[!NOTE] 
>Från och med tidig juni 2017 Security Center använder hello Microsoft Monitoring Agent toocollect och lagra data. Se [Azure Security Center-plattformen migrering](security-center-platform-migration.md) toolearn mer. hello informationen i den här artikeln representerar Security Center-funktionalitet efter övergången toohello Microsoft Monitoring Agent.
>


## <a name="data-sources"></a>Datakällor
Azure Security Center analyserar data från följande källor tooprovide synlighet i din säkerhetstillstånd hello, identifiera säkerhetsrisker och rekommenderar åtgärder och identifiera active hot:

- Azure-tjänster: Använder information om hello konfiguration av Azure-tjänster som du har distribuerat genom att kommunicera med tjänstens resursprovidern.
- Nätverkstrafik: Använder samplade metadata för nätverkstrafiken från Microsofts infrastruktur, till exempel käll- och mål IP-adress/port, paketstorlek och nätverksprotokoll.
- Partnerlösningar: Använder säkerhetsvarningar från integrerade partnerlösningar, till exempel brandväggar och lösningar mot skadlig kod. 
- Virtual Machines and Servers: Använder information om konfigurationer och säkerhetshändelser, till exempel händelser och granskningsloggar i Windows, IIS-loggar, syslog-meddelanden och kraschdumpfiler från dina virtuella datorer. Dessutom när en avisering skapas kan Azure Security Center Generera en ögonblicksbild av hello Virtuella disk som påverkas och extrahera datorn artefakter relaterade toohello avisering från hello virtuell disk, till exempel en registerfil, för dataforensik ändamål.


## <a name="data-protection"></a>Dataskydd
**Dataavgränsning**: Data lagras logiskt separat på varje komponent i hela hello-tjänsten. Alla data taggas efter organisation. Den här taggning kvarstår under hello data livscykel och den tillämpas på varje nivå av hello-tjänsten.

**Dataåtkomst**: order tooprovide säkerhetsrekommendationer och undersöka potentiella säkerhetshot, Microsoft-Personal kan komma åt information som samlas in eller analyseras av Azure-tjänster, inklusive kraschdumpfiler, bearbeta skapande händelser VM diskbilder och artefakter som kan oavsiktligt innehålla kundinformation eller personliga data från dina virtuella datorer. Vi följer toohello [Microsoft Online Services-licensvillkoren och sekretesspolicyn](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31), vilket tillstånd att Microsoft inte använder kundinformation eller härledd information från den i reklam eller liknande kommersiellt syfte. Vi använder bara kundinformation som behövs tooprovide du med Azure-tjänsterna, inklusive syfte som är kompatibla med dessa tjänster. Du kan behålla alla rättigheter tooCustomer Data.

**Använd data**: Microsoft använder mönster och hotinformation ses över flera innehavare tooenhance våra förhindra och upptäcka funktioner; vi gör i enlighet med hello sekretess åtaganden som beskrivs i vår [sekretess Instruktionen](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

## <a name="data-location"></a>Dataplats

**Din arbetsytor**: en arbetsyta har angetts för hello följande regioner och data som samlas in från din Azure-datorer, inklusive krascher Dumpar och vissa typer av aviseringar data lagras i hello närmsta arbetsytan. 

| VM Geo                        | Workspace Geo |
|-------------------------------|---------------|
| USA, Brasilien, Kanada | USA |
| Europa, Storbritannien        | Europa        |
| Asien och stillahavsområdet, Japan, Indien    | Asien och stillahavsområdet  |
| Australien                     | Australien     |

 
Virtuella diskbilder lagras i hello samma lagringskonto som hello virtuell disk.
 
För virtuella datorer och servrar som körs i andra miljöer, t.ex. lokalt, kan du ange hello arbetsytan och region där insamlade data lagras. 

**Azure Security Center lagring**: Information om säkerhetsaviseringar, inklusive partner aviseringar lagras regionalt enligt toohello platsen för hello relaterade Azure-resurs, medan Information om hälsostatus för säkerhet och rekommendation lagras centralt i hello USA eller Europa enligt toocustomer's plats.
Azure Security Center samlar in tillfälliga kopior av dina kraschdumpfiler och analyserar dem efter tecken på kryphål och säkerhetsintrång. Azure Security Center utför den här analysen inom hello samma Geo som hello arbetsytan och tar bort hello tillfälliga kopior när analysen är klar.

Datorn artefakter lagras centralt i hello samma region som hello VM. 


## <a name="managing-data-collection-from-virtual-machines"></a>Hantera datainsamling från virtuella datorer

När du väljer att aktivera Security Center i Azure är datainsamling aktiverat för var och en av dina Azure-prenumerationer. Du kan också aktivera datainsamling för dina prenumerationer i hello säkerhetsprincip avsnitt i Azure Security Center. När datainsamlingen är aktiverat stöd för Azure Security Center tillhandahåller hello Microsoft Monitoring Agent på alla befintliga Azure virtuella datorer och nya filer som skapas. hello Microsoft Monitoring agent söker efter olika säkerhet relaterade konfigurationer och händelser till [Event Tracing for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) spår (ETW). Dessutom höjer hello operativsystemet händelselogghändelser under hello hello datorn som körs. Exempel på sådana data är: operativsystemets typ och version, operativsystemloggar (Windows-händelseloggar), processer som körs, datornamn, IP-adresser, inloggad användare och klient-ID. hello Microsoft Monitoring Agent läser händelseloggposter och ETW spårar och kopierar dem tooyour arbetsytor för analys. hello Microsoft Monitoring Agent kopieras även krascher dump filer tooyour arbetsytor.

Om du använder Azure Security Center ledigt, kan du inaktivera datainsamling från virtuella datorer i hello säkerhetsprincip. Insamling av data krävs för prenumerationer på hello standardnivån. Funktionerna för ögonblicksbilder av virtuella datordisker och artefaktinsamling är fortfarande aktiverade även om datainsamling har inaktiverats.


## <a name="see-also"></a>Se även
I det här dokumentet har du lärt dig hur data hanteras och skyddas i Azure Security Center. toolearn mer om Azure Security Center finns:

* [Planera för Azure Security Center och handboken](security-center-planning-and-operations-guide.md) – Lär dig hur tooplan och förstå hello design överväganden tooadopt Azure Security Center.
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar
* [Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) – finns vanliga frågor om hur du använder hello-tjänsten
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) – Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure
