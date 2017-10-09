---
title: "aaaMonitor status för replikering av Active Directory med Azure Log Analytics | Microsoft Docs"
description: "hello pack för lösning av Active Directory-replikeringsstatus regelbundet övervakar Active Directory-miljön för eventuella replikeringsfel och rapporterar hello resultat på instrumentpanelen OMS."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 1b988972-8e01-4f83-a7f4-87f62778f91d
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 235e4f7a066ea50b79f681398182b22c91fb6d31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-active-directory-replication-status-with-log-analytics"></a>Visa replikeringsstatus för Active Directory med logganalys

![Statussymbolen för AD-replikering](./media/log-analytics-ad-replication-status/ad-replication-status-symbol.png)

Active Directory är en viktig del av ett företags IT-miljö. tooensure hög tillgänglighet och hög prestanda, varje domänkontrollant har sin egen kopia av hello Active Directory-databasen. Domänkontrollanterna med varandra i ordning toopropagate ändras över hello företaget. Fel i den här replikeringen kan orsaka en rad olika problem över hello företaget.

hello AD replikeringsstatus lösningspaket regelbundet övervakar Active Directory-miljön för eventuella replikeringsfel och rapporterar hello resultat på instrumentpanelen OMS.

## <a name="installing-and-configuring-hello-solution"></a>Installera och konfigurera hello lösning
Använd följande information tooinstall hello och konfigurera hello lösning.

* Du måste installera agenter på domänkontrollanter som är medlemmar i hello domän toobe utvärderas. Eller, du måste installera agenter på medlemsservrar och konfigurera hello agenter toosend AD replikering data tooOMS. hur tooconnect tooOMS för Windows-datorer, se toounderstand [ansluta Windows-datorer tooLog Analytics](log-analytics-windows-agents.md). Om domänkontrollanten är redan en del av en befintlig System Center Operations Manager-miljö som du vill tooconnect tooOMS, se [ansluta Operations Manager tooLog Analytics](log-analytics-om-agents.md).
* Lägg till hello replikeringsstatus för Active Directory-lösningen tooyour OMS-arbetsyta med hjälp av hello process beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).  Det krävs ingen ytterligare konfiguration.

## <a name="ad-replication-status-data-collection-details"></a>AD replikeringsstatus data samling information
hello följande tabell visar metoder för insamling av data och annan information om hur data samlas in för AD-replikeringsstatus.

| Plattform | Styr Agent | SCOM-agent | Azure Storage | SCOM krävs? | SCOM-agent data som skickas via management-grupp | Frekvens för samlingen |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |&#8226; |&#8226; |  |  |&#8226; |var femte dag |

## <a name="optionally-enable-a-non-domain-controller-toosend-ad-data-toooms"></a>Du kan också aktivera en icke-domänkontrollant toosend AD data tooOMS
Om du inte vill tooconnect alla domänkontrollanter direkt tooOMS, du kan använda andra OMS-anslutna datorer i din domän toocollect data för hello AD replikeringsstatus lösning pack och låta den skicka hello data.

### <a name="tooenable-a-non-domain-controller-toosend-ad-data-toooms"></a>tooenable en icke-domänkontrollant toosend AD data tooOMS
1. Kontrollera hello datorn är medlem i hello domän som du vill toomonitor med hello AD replikeringsstatus lösningen.
2. [Ansluta hello Windows datorn tooOMS](log-analytics-windows-agents.md) eller [ansluta den med hjälp av din befintliga Operations Manager-miljön tooOMS](log-analytics-om-agents.md), om den inte redan är ansluten.
3. Ange hello följande registernyckel på datorn:

   * Nyckel: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HealthService\Parameters\Management grupper\<ManagementGroupName > \Solutions\ADReplication**
   * Värde: **IsTarget**
   * Värdedata: **SANT**

   > [!NOTE]
   > Ändringarna inte gälla förrän din omstart hello Microsoft Monitoring Agent-tjänsten (HealthService.exe).
   >
   >

## <a name="understanding-replication-errors"></a>Förstå replikeringsfel
När du har AD status replikeringsdata som skickas tooOMS kan se du en panel liknande toohello följande bild på hello OMS-instrumentpanel som anger hur många replikeringsfel som du har för närvarande.  
![Replikeringsstatus för AD-panelen](./media/log-analytics-ad-replication-status/oms-ad-replication-tile.png)

**Kritiska replikeringsfel** gäller vid eller över 75% av hello [tombstone-livslängden](https://technet.microsoft.com/library/cc784932%28v=ws.10%29.aspx) för Active Directory-skogen.

När du klickar på hello sida vid sida kan du visa mer information om hello fel.
![AD replikeringsstatus instrumentpanelen](./media/log-analytics-ad-replication-status/oms-ad-replication-dash.png)

### <a name="destination-server-status-and-source-server-status"></a>Status för mål-servern och Status för käll-servern
Dessa kolumner visar hello status för målservrar och källservrar som replikeringsfel har uppstått. hello siffran efter varje domänkontrollantens namn anger hello antal replikeringsfel på domänkontrollanten.

hello-fel för både målet och källservrar visas eftersom vissa problem är enklare tootroubleshoot ur hello käll-servern och andra ur hello mål-servern.

I det här exemplet ser du att många målservrar har ungefär hello samma antal fel, men det finns en källserver (ADDC35) som har många fler fel än alla hello andra. Det är troligt att det finns problem på ADDC35 som orsakar det toofail toosend data tooits replikeringspartner. Korrigera hello problem på ADDC35 kan lösa många hello-fel som visas i hello målområdet server.

### <a name="replication-error-types"></a>Replikering feltyper
Det här området innehåller information om hello typer av fel upptäcktes i hela företaget. Varje fel har en unik numerisk kod och ett meddelande som kan hjälpa dig att avgöra hello grundorsaken till hello-fel.

hello ringen överst hello ger dig en uppfattning av vilka fel visas mer och mindre ofta i din miljö.

Den visar när flera domänkontrollanter får hello samma replikeringsfel. I så fall måste du kan vara kan toodiscover eller identifiera en lösning på en domänkontrollant och sedan upprepa den på andra domänkontrollanter påverkas av hello samma fel.

### <a name="tombstone-lifetime"></a>Tombstone-livslängden
hello tombstone-livslängden anger hur länge ett borttaget objekt, som anges tooas tombstone, sparas i hello Active Directory-databasen. När ett borttaget objekt överför hello tombstone-livslängden, bort en process som skräp samlingen automatiskt från hello Active Directory-databasen.

hello standard tombstone-livslängden är 180 dagar för de senaste versionerna av Windows, men det var 60 dagar i äldre versioner och kan ändras uttryckligen en Active Directory-administratör.

Det är viktigt tooknow om du har replikeringsfel som närmar dig eller har passerat hello tombstone-livslängden. Om två domänkontrollanter uppstår ett replikeringsfel som kvarstår efter hello tombstone-livslängden inaktivera replikering mellan de två domänkontrollanterna, även om hello underliggande replikeringsfel har åtgärdats.

hello Tombstone-livslängden området hjälper dig identifiera platser där inaktiverad replikeringen är i fara för inträffar. Varje fel i hello **över 100% TLS** kategori representerar en partition som inte har replikerats mellan sin käll- och server för vid minst hello tombstone-livslängden för hello skog.

I så fall kan bara korrigerat hello replikeringsfel inte finns tillräckligt med. Som minimum måste toomanually undersöka tooidentify och rensa kvarstående objekt innan du startar om replikeringen. Du kan även behöva toodecommission en domänkontrollant.

I tillägg tooidentifying replikeringsfel som beständiga tidigare hello tombstone-livslängden du också toopay uppmärksamhet tooany fel i hello **50 75% TLS** eller **75-100% TLS** kategorier.

Det här är fel som tydligt kvarvarande, tillfälligt, så att de behöver sannolikt åtgärd-tooresolve. hello bra är att de ännu inte har nått hello tombstone-livslängden. Om du åtgärda problemen omedelbart och *innan* de når hello tombstone-livslängden, replikering kan du starta om med minimala manuella åtgärder.

Som tidigare nämnts hello instrumentpanelen för hello AD replikeringsstatus lösningen visar hello antalet *kritiska* replikeringsfel i din miljö, som definieras som fel som är över 75% tombstone-livslängden (inklusive fel som är över 100% av TLS). Sträva efter tookeep siffran 0.

> [!NOTE]
> Alla hello tombstone livstid procentandel beräkningar baseras på hello faktiska tombstone-livslängden för Active Directory-skog så att du kan lita på att dessa procenttal är korrekta, även om du har en anpassad tombstone-livslängden inställd.
>
>

### <a name="ad-replication-status-details"></a>Statusinformation för AD-replikering
När du klickar på ett objekt i en hello listor kan du se ytterligare information om den med hjälp av loggen sökning. hello resultat är filtrerade tooshow endast hello fel relaterade toothat objekt. Till exempel om du klickar på hello första domänkontrollanten som visas under **serverstatus för mål (ADDC02)**, visas sökresultat filtreras tooshow fel med den domänkontrollant som hello målservern:

![AD status replikeringsfel i sökresultat](./media/log-analytics-ad-replication-status/oms-ad-replication-search-details.png)

Här kan du filtrera ytterligare, ändra hello sökfråga och så vidare. Mer information om hur du använder hello loggen Sök finns [logga sökningar](log-analytics-log-searches.md).

Hej **HelpLink** fältet visar hello-URL för en TechNet-sida med ytterligare information om det specifika felet. Du kan kopiera och klistra in den här länken i din webbläsare fönstret toosee information om felsökning och åtgärda hello fel.

Du kan också klicka på **exportera** tooexport hello resulterar tooExcel. Exportera hello data hjälper dig att visualisera data för replikering fel på något sätt som du vill.

![exporterade AD status replikeringsfel i Excel](./media/log-analytics-ad-replication-status/oms-ad-replication-export.png)

## <a name="ad-replication-status-faq"></a>AD-replikeringsstatus vanliga frågor och svar
**F: hur ofta är AD status replikeringsdata uppdateras?**
S: hello uppdateras var femte dag.

**F: finns det ett sätt tooconfigure hur ofta dessa data uppdateras?**
S: inte just nu.

**F: behöver jag tooadd alla min domän domänkontrollanter toomy OMS-arbetsyta i ordning toosee replikeringsstatus?**
S: inte måste bara en enda domänkontrollant läggas till. Om du har flera domänkontrollanter i OMS-arbetsytan skickas data från alla tooOMS.

**F: Jag vill inte tooadd någon domän domänkontrollanter toomy OMS-arbetsyta. Kan jag fortsätta att använda hello replikeringsstatus för AD-lösningen?**
S: Ja. Du kan ange hello-värdet för en nyckel registret tooenable den. Se [tooenable en icke-domänkontrollant toosend AD data tooOMS](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

**F: Vad är hello process som hello datainsamling hello namn?**
S: AdvisorAssessment.exe

**F: hur lång tid tar det för toobe för data som samlas in?**
S: data collection tid beror på hello storleken på hello Active Directory-miljö, men det tar vanligtvis mindre än 15 minuter.

**F: vilken typ av data som samlas in?**
S: replikering information som samlas in via LDAP.

**F: finns det en tooconfigure sätt när data samlas in?**
S: inte just nu.

**F: vad behörigheter göra måste toocollect data?**
S: normal användare behörighet tooActive Directory är tillräckliga.

## <a name="troubleshoot-data-collection-problems"></a>Felsökning av problem med samlingen
I ordning toocollect data kräver hello AD replikeringsstatus lösningspaket minst en domain controller-toobe ansluten tooyour OMS-arbetsyta. Tills du ansluter en domänkontrollant, visas ett meddelande som anger att **data samlas fortfarande**.

Om du behöver hjälp med att ansluta en av domänkontrollanterna kan du visa dokumentationen på [ansluta Windows-datorer tooLog Analytics](log-analytics-windows-agents.md). Alternativt, om domänkontrollanten är redan anslutet tooan befintliga System Center Operations Manager-miljön, kan du visa dokumentationen på [ansluta System Center Operations Manager tooLog Analytics](log-analytics-om-agents.md).

Om du inte direkt tooOMS eller tooSCOM, finns i någon av dina domänkontrollanter vill tooconnect [tooenable en icke-domänkontrollant toosend AD data tooOMS](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

## <a name="next-steps"></a>Nästa steg
* Använd [logga sökningar i logganalys](log-analytics-log-searches.md) tooview detaljerad statusdata för Active Directory-replikering.
