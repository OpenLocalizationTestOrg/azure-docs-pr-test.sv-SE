---
title: "aaaLog Analytics vanliga frågor och svar | Microsoft Docs"
description: "Svaren toofrequently frågor och svar om hello Azure Log Analytics-tjänsten."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: ad536ff7-2c60-4850-a46d-230bc9e1ab45
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: magoedte
ms.openlocfilehash: 25931f521cbb6ec840184221c6c1a5794b3445f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-faq"></a>Vanliga frågor och svar om Log Analytics
Den här Microsoft-FAQ är en lista över vanliga frågor om logganalys i Microsoft Operations Management Suite (OMS). Om du har några ytterligare frågor om logganalys går toohello [diskussionsforum](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) och dina frågor. När en fråga är vanliga vi lägga till den toothis artikeln så att den finns snabbt och enkelt.

## <a name="general"></a>Allmänt

### <a name="q-does-log-analytics-use-hello-same-agent-as-azure-security-center"></a>FRÅGOR. Använder logganalys hello samma agent som Azure Security Center?

A. I tidig juni 2017 började Azure Security Center med hjälp av hello Microsoft Monitoring Agent toocollect och lagra data. Det finns fler toolearn [Azure Security Center-plattformen migrering vanliga frågor och svar](../security-center/security-center-platform-migration-faq.md).

### <a name="q-what-checks-are-performed-by-hello-ad-and-sql-assessment-solutions"></a>FRÅGOR. Vilka kontroller som utförs av hello AD och SQL-bedömning lösningar?

A. hello visar följande fråga en beskrivning av alla kontroller som utförs för närvarande:

```
(Type=SQLAssessmentRecommendation OR Type=ADAssessmentRecommendation) | dedup RecommendationId | select FocusArea, ActionArea, Recommendation, Description | sort Type, FocusArea,ActionArea, Recommendation
```

hello kan resultat sedan vara exporterade tooExcel för vidare undersökning.

### <a name="q-why-do-i-see-something-different-than-oms-in-system-center-operations-manager-console"></a>F: Varför visas något annat än *OMS* i System Center Operations Manager-konsolen?

S: beroende på vilka Update Rollup av Operations Manager på, kan du se en nod för *System Center Advisor*, *åtgärdsinformation*, eller *logganalys*.

Hej text sträng uppdateringen för*OMS* ingår i ett hanteringspaket som måste toobe importeras manuellt. toosee hello aktuell text och funktionalitet, följ hello instruktioner på hello senaste samlade KB för System Center Operations Manager artikeln och uppdatera hello-konsolen.

### <a name="q-is-there-an-on-premises-version-of-log-analytics"></a>F: finns det en *lokalt* version av Log Analytics?

S: Nej. Logganalys bearbetar och lagrar stora mängder data. Som en molntjänst logganalys är kan tooscale upp om det behövs och undvika eventuella prestanda påverkan tooyour miljö.

Ytterligare fördelar:
- Kör Microsoft hello logganalys infrastruktur, spara kostnader
- Vanlig distribution av funktionsuppdateringar och korrigeringar.

### <a name="q-how-do-i-troubleshoot-that-log-analytics-is-no-longer-collecting-data"></a>FRÅGOR. Hur felsöker jag att logganalys längre samlar in data?

S: Om du är på hello kostnadsfria prisnivån och har skickat mer än 500 MB data under en dag, stoppar datainsamling för hello resten av hello dag. Nådde hello dagliga gränsen är en vanlig orsak till att logganalys slutar att samla in data eller data visas toobe saknas.

Log Analytics skapar en händelse av typen *åtgärden* när datainsamlingen startar och stoppar. 

Kör följande fråga i Sök toocheck om du når hello dagliga gränsen och data saknas hello:`Type=Operation OperationCategory="Data Collection Status"`

När datainsamlingen slutar, hello *OperationStatus* är **varning**. När datainsamlingen startar hello *OperationStatus* är **lyckades**. 

hello följande tabell beskrivs skäl som datainsamling stoppar och en föreslagen åtgärd tooresume datainsamling:

| Insamling av orsak stoppar                       | tooresume datainsamling |
| -------------------------------------------------- | ----------------  |
| Dagliga gränsen för ledigt data uppnåtts<sup>1</sup>       | Vänta tills hello efter dag för samlingen tooautomatically omstart eller<br> Ändra tooa betald prisnivå |
| Azure-prenumeration är i ett pausat tillstånd på grund av: <br> Kostnadsfri utvärderingsversion avslutades <br> Azure-pass upphört att gälla <br> Varje månad utgiftsgräns uppnåtts (till exempel på en MSDN- eller Visual Studio-prenumeration)                          | Konvertera tooa betald prenumeration <br> Konvertera tooa betald prenumeration <br> Ta bort gränsen eller vänta tills gränsen återställs |

<sup>1</sup> om ditt arbetsområde på hello kostnadsfria prisnivån du är begränsad toosending 500 MB data per dag toohello tjänst. När du når hello dagliga gränsen stoppas datainsamling tills hello nästa dag. Data som skickas när datainsamling har stoppats är inte indexerat och är inte tillgängligt för sökning. När datainsamlingen återställs sker bearbetning endast för nya data som skickas. 

Log Analytics använder UTC-tid varje dag startar vid midnatt UTC. Om arbetsytan hello når hello dagliga gränsen, återupptar bearbetningen under hello första timmen hello nästa dag i UTC.

### <a name="q-how-can-i-be-notified-when-data-collection-stops"></a>FRÅGOR. Hur kan jag få meddelanden när datainsamling stoppar?

S: Använd hello stegen som beskrivs i [skapa en aviseringsregel](log-analytics-alerts-creating.md#create-an-alert-rule) toobe meddelas när datainsamling stoppar.

Ange när du skapar hello varning för när datainsamling slutar på:
- **Namnet** för*datainsamling har stoppats*
- **Allvarlighetsgrad** för*varning*
- **Sökfråga** för`Type=Operation OperationCategory="Data Collection Status" OperationStatus=Warning`
- **Tidsfönstret** för*2 timmar*.
- **Varna frekvens** toobe en timme sedan hello användningsdata uppdateras bara en gång i timmen.
- **Generera en avisering baserat på** toobe *antal resultat*
- **Antalet resultat** toobe *större än 0*

Använd hello stegen som beskrivs i [Lägg till åtgärder tooalert regler](log-analytics-alerts-actions.md) konfigurerar en åtgärd för hello varningsregeln att e-post, webhook eller runbook.


## <a name="configuration"></a>Konfiguration
### <a name="q-can-i-change-hello-name-of-hello-tableblob-container-used-tooread-from-azure-diagnostics-wad"></a>FRÅGOR. Kan jag ändra hello namnet på hello tabellen/blob-behållaren används tooread från Azure Diagnostics (BOMULLSTUSS)?

A. Nej, det är inte för närvarande kan tooread från valfri tabeller eller behållare i Azure-lagring.

### <a name="q-what-ip-addresses-does-hello-log-analytics-service-use-how-do-i-ensure-that-my-firewall-only-allows-traffic-toohello-log-analytics-service"></a>FRÅGOR. IP-adresser hello logganalys bruk? Hur kan jag vara säker på att min brandvägg bara tillåter trafik toohello logganalys-tjänsten?

A. hello logganalys-tjänsten är byggt på Azure. Log Analytics IP-adresser finns i hello [IP-intervall i Microsoft Azure Datacenter](http://www.microsoft.com/download/details.aspx?id=41653).

Eftersom tjänstdistributioner görs ändra hello faktiska IP-adresser för hello logganalys-tjänsten. hello DNS-namn tooallow genom brandväggen finns dokumenterade i [konfigurera proxy-och brandväggsinställningarna i logganalys](log-analytics-proxy-firewall.md).

### <a name="q-i-use-expressroute-for-connecting-tooazure-does-my-log-analytics-traffic-use-my-expressroute-connection"></a>FRÅGOR. Jag använder ExpressRoute för att ansluta tooAzure. Använder Mina logganalys trafik min ExpressRoute-anslutningen?

A. hello olika typer av trafik ExpressRoute beskrivs i hello [ExpressRoute dokumentation](../expressroute/expressroute-faqs.md#supported-services).

Trafik tooLog Analytics använder hello offentlig peering ExpressRoute-kretsen.

### <a name="q-is-there-a-simple-and-easy-way-toomove-an-existing-log-analytics-workspace-tooanother-log-analytics-workspaceazure-subscription"></a>FRÅGOR. Finns det en enkel och enkelt sätt toomove en tooanother för befintliga logganalys-arbetsytan logganalys-arbetsytan/Azure-prenumeration?

A. Hej `Move-AzureRmResource` cmdleten låter dig flytta logganalys-arbetsytan och ett Automation-konto från en Azure-prenumeration tooanother. Mer information finns i [flytta AzureRmResource](http://msdn.microsoft.com/library/mt652516.aspx).

Den här ändringen kan även göras i hello Azure-portalen.

Du kan inte flytta data från en tooanother för logganalys-arbetsytan eller ändra hello region som logganalys data lagras i.

### <a name="q-how-do-i-add-log-analytics-toosystem-center-operations-manager"></a>F: hur lägger jag till Log Analytics tooSystem Center Operations Manager?

S: uppdatering toohello senaste samlade uppdateringen och importera hanteringspaket gör tooconnect Operations Manager tooLog Analytics.

>[!NOTE]
>hello Operations Manager-anslutning tooLog Analytics är endast tillgängligt för System Center Operations Manager 2012 SP1 och senare.

### <a name="q-how-can-i-confirm-that-an-agent-is-able-toocommunicate-with-log-analytics"></a>F: hur kan jag bekräfta att en agent är kan toocommunicate med Log Analytics?

S: tooensure hello agenten kan kommunicera med OMS, gå till: Kontrollpanelen, säkerhet och inställningar, **Microsoft Monitoring Agent**.

Under hello **Azure logganalys (OMS)** fliken, leta efter en grön bock. En grön bockmarkering bekräftar att agenten hello är kan toocommunicate med hello OMS-tjänsten.

En gul varningsikon innebär hello agent har problem med kommunikation med OMS. En vanlig orsak är hello Microsoft Monitoring Agent-tjänsten har stoppats. Använda service control manager toorestart hello-tjänsten.

### <a name="q-how-do-i-stop-an-agent-from-communicating-with-log-analytics"></a>F: hur förhindrar en agent från att kommunicera med Log Analytics?

S: i System Center Operations Manager, ta bort hello datorn hello Advisor hanterad datorlistan. Operations Manager-uppdateringar hello konfigurationen av hello agent toono längre rapporten tooLog Analytics. För agenter direkt anslutna tooLog Analytics du stoppar dem från att kommunicera via: Kontrollpanelen, säkerhet och inställningar, **Microsoft Monitoring Agent**.
Under **Azure logganalys (OMS)**, ta bort alla arbetsytor i listan.

### <a name="q-why-am-i-getting-an-error-when-i-try-toomove-my-workspace-from-one-azure-subscription-tooanother"></a>F: Varför inträffar ett fel när jag försöker toomove Min arbetsyta från en Azure-prenumeration tooanother?

S: Om du använder hello Azure-portalen, se till att endast hello arbetsytan har valts för hello move. Välj inte hello lösningar – flyttas automatiskt när du flyttar hello arbetsytan. 

Se till att du har behörighet i både Azure-prenumerationer.

## <a name="agent-data"></a>Agenten data
### <a name="q-how-much-data-can-i-send-through-hello-agent-toolog-analytics-is-there-a-maximum-amount-of-data-per-customer"></a>FRÅGOR. Hur mycket data som kan skicka via hello agent tooLog Analytics? Finns det en maximal mängd data per kund?
A. hello fria abonnemang anger en daglig fjärrskrivbordsanslutning 500 MB per arbetsytan. hello standard och premiumplaner har ingen gräns på hello mängden data som överförs. Som en tjänst i molnet, Log Analytics utformats tooautomatically skalas upp toohandle hello volym kommer från en kund – även om det är TB per dag.

hello logganalys-agenten kunde utformad tooensure som den har en kompakta. En av våra kunder skrev en blogg om hello tester de utförs mot vår agent och hur stämpel som de var. hello datavolym varierar beroende på hello lösningar som du aktiverar. Du kan hitta detaljerad information om hello datavolym och se hello data genom att lösningen i hello [användning](log-analytics-usage.md) sidan.

Mer information kan du läsa en [kunden blogg](http://thoughtsonopsmgr.blogspot.com/2015/09/one-small-footprint-for-server-one.html) om hello låg storleken på hello OMS-agent.

### <a name="q-how-much-network-bandwidth-is-used-by-hello-microsoft-management-agent-mma-when-sending-data-toolog-analytics"></a>FRÅGOR. Hur mycket nätverksbandbredd används av hello Microsofts hanteringsagent (MMA) när du skickar data tooLog Analytics?

A. Bandbredd är en funktion på hello mängden data som skickas. Data komprimeras som skickas över hello nätverk.

### <a name="q-how-much-data-is-sent-per-agent"></a>FRÅGOR. Hur mycket data skickas per agent?

A. hello mängden data som skickats per agent beror på:

* hello-lösningar som du har aktiverat
* hello antalet loggar och prestandaräknare som samlas in
* hello datavolymen i hello loggar

hello kostnadsfria prisnivån är ett bra sätt tooonboard flera servrar och mäta hello vanliga datavolym. Den totala användningen visas på hello [användning](log-analytics-usage.md) sidan.

Använd hello följande fråga för datorer som kan toorun hello WireData agent toosee hur mycket data skickas:

```
Type=WireData (ProcessName="C:\\Program Files\\Microsoft Monitoring Agent\\Agent\\MonitoringHost.exe") (Direction=Outbound) | measure Sum(TotalBytes) by Computer
```



## <a name="next-steps"></a>Nästa steg
* [Kom igång med logganalys](log-analytics-get-started.md) toolearn mer om logganalys och komma igång i minuter.
