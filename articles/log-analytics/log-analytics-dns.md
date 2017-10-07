---
title: "aaaDNS Analytics lösning i Azure Log Analytics | Microsoft Docs"
description: "Konfigurera och använda hello DNS Analytics lösning i logganalys toogather insikter om DNS-infrastrukturen på säkerhet, prestanda och åtgärder."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f44a40c4-820a-406e-8c40-70bd8dc67ae7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: banders
ms.openlocfilehash: be7982c54b65ba0c4b1c15ae7516d02eced313f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="gather-insights-about-your-dns-infrastructure-with-hello-dns-analytics-preview-solution"></a>Samla in insikter om din DNS-infrastruktur med hello DNS Analytics Preview lösning

![DNS-Analytics symbol](./media/log-analytics-dns/dns-analytics-symbol.png)

Den här artikeln beskriver hur tooset in och använda hello Azure DNS Analytics lösning i Azure Log Analytics toogather insikter om DNS-infrastrukturen på säkerhet, prestanda och åtgärder.

DNS-analyser kan du:

- Identifiera klienter som försöker tooresolve skadliga domännamn.
- Identifiera inaktuella poster.
- Identifiera ofta efterfrågade domännamn och talkative DNS-klienter.
- Visa belastningen på DNS-servrar.
- Visa dynamiska DNS-registreringsfel.

hello lösningen samlar in, analyseras och korrelerar Windows DNS analytiska och granskningsloggar och andra relaterade data från DNS-servrar.

## <a name="connected-sources"></a>Anslutna källor

hello i den följande tabellen beskrivs hello anslutna källor som stöds av den här lösningen:

| **Ansluten datakälla** | **Support** | **Beskrivning** |
| --- | --- | --- |
| [Windows-agenter](log-analytics-windows-agents.md) | Ja | hello lösningen samlar in DNS-information från Windows-agenter. |
| [Linux-agenter](log-analytics-linux-agents.md) | Nej | hello lösningen inte samlar in DNS-information direkt Linux-agenter. |
| [System Center Operations Manager-hanteringsgruppen](log-analytics-om-agents.md) | Ja | hello lösningen samlar in DNS-information från agenter i en ansluten hanteringsgrupp för Operations Manager. En direkt anslutning från hello Operations Manager-agenten toohello Operations Management Suite krävs inte. Data vidarebefordras från hello management group toohello Operations Management Suite-databasen. |
| [Azure Storage-konto](log-analytics-azure-storage.md) | Nej | Azure-lagring används inte av hello-lösning. |

### <a name="data-collection-details"></a>Information om samlingen

hello lösningen samlar in DNS-inventerings- och DNS-händelse-relaterade data från hello DNS-servrar där en Log Analytics-agent har installerats. Dessa data och sedan överföra tooLog Analytics och visas i instrumentpanelen för hello-lösning. Lager-relaterad data, till exempel hello antal DNS-servrar, zoner och resursposter, samlas in genom att köra hello DNS-PowerShell-cmdlets. hello uppdateras en gång var två dagar. hello händelse-relaterad data samlas in nära realtid från hello [analytiska och granskningsloggar](https://technet.microsoft.com/library/dn800669.aspx#enhanc) tillhandahålls av utökade DNS-loggning och diagnostik i Windows Server 2012 R2.

## <a name="configuration"></a>Konfiguration

Använd följande information tooconfigure hello lösning hello:

- Du måste ha en [Windows](log-analytics-windows-agents.md) eller [Operations Manager](log-analytics-om-agents.md) agenten på varje DNS-server som du vill toomonitor.
- Du kan lägga till hello DNS Analytics lösning tooyour Operations Management Suite-arbetsyta från hello [Azure Marketplace](https://aka.ms/dnsanalyticsazuremarketplace). Du kan också använda hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).

hello lösning börjar samla in data utan hello behovet av ytterligare konfiguration. Du kan dock använda hello följande konfiguration toocustomize datainsamling.

### <a name="configure-hello-solution"></a>Konfigurera hello lösning

Hello lösning instrumentpanelen, klicka på **Configuration** tooopen hello konfiguration av DNS-sidan. Det finns två typer av konfigurationsändringar som du kan göra:

- **Listan över godkända domännamn**. hello-lösning bearbetar inte alla hello sökning-frågor. Det finns en godkänd lista över namnsuffix. hello sökning-frågor som matchar toohello domännamn som matchar namnsuffix i den här listan över godkända bearbetas inte av hello-lösning. Icke-bearbetning av listan över godkända domännamn hjälper toooptimize hello data som skickas tooLog Analytics. hello standard godkända innehåller populära offentliga domännamn, till exempel www.google.com och www.facebook.com. Du kan visa hello fullständig standardlistan genom att bläddra.

 Du kan ändra hello listan tooadd alla domänens namnsuffix som du vill tooview sökning insikter för. Du kan också ta bort alla domänens namnsuffix som du inte vill tooview sökning insikter för.

- **Tröskelvärdet för talkative klienter**. DNS-klienter som överskrider hello tröskelvärdet för hello antalet sökningsbegäranden är markerade i hello **DNS-klienter** bladet. hello standardtröskelvärdet är 1 000. Du kan redigera hello tröskelvärdet.

    ![Listan över godkända domännamn](./media/log-analytics-dns/dns-config.png)

## <a name="management-packs"></a>Hanteringspaket

Om du använder hello Microsoft Monitoring Agent tooconnect tooyour Operations Management Suite-arbetsyta är hello följande hanteringspaket installerad:

- Microsoft DNS-Data Collector informationspaketet (Microsft.IntelligencePacks.Dns)

Om din Operations Manager-hanteringsgrupp är anslutna tooyour Operations Management Suite-arbetsyta, är hello följande hanteringspaket installerade i Operations Manager när du lägger till den här lösningen. Det finns ingen konfiguration eller underhåll av dessa hanteringspaket:

- Microsoft DNS-Data Collector informationspaketet (Microsft.IntelligencePacks.Dns)
- Microsoft System Center Advisor DNS-konfiguration (Microsoft.IntelligencePack.Dns.Configuration)

Mer information om hur lösningen hanteringspaketen är uppdaterade finns [ansluta Operations Manager tooLog Analytics](log-analytics-om-agents.md).

## <a name="use-hello-dns-analytics-solution"></a>Använda hello DNS Analytics lösning

Det här avsnittet beskriver alla hello instrumentpanelen funktioner och hur toouse dem.

När du har lagt till hello lösning tooyour arbetsytan ger hello lösning panelen på hello Operations Management Suite översiktssidan en snabb översikt över DNS-infrastrukturen. Den innehåller hello antal DNS-servrar där hello data samlas in. Den innehåller också hello antalet förfrågningar som görs av klienter tooresolve skadliga domäner i hello senaste 24 timmarna. När du klickar på panelen hello öppnas hello lösning instrumentpanelen.

![DNS-Analytics sida vid sida](./media/log-analytics-dns/dns-tile.png)

### <a name="solution-dashboard"></a>Instrumentpanel för lösningen

hello lösning instrumentpanelen visar sammanfattningsinformation för hello olika funktioner i hello lösning. Den innehåller också länkar toohello detaljerad vy för kriminalteknisk analys och diagnos. Som standard visas hello data för hello senaste sju dagarna. Du kan ändra intervallet för hello datum och tid genom att använda hello **tid markering**som visas i följande bild hello:

![Tid markering](./media/log-analytics-dns/dns-time.png)

hello lösning instrumentpanelen visar hello följande blad:

**DNS-säkerhet**. Rapporter hello DNS-klienter som försöker toocommunicate med skadliga domäner. Med hjälp av Microsoft threat intelligence feeds identifiera DNS Analytics klientens IP-adresser som försöker tooaccess skadliga domäner. I många fall hello enheter som infekterats av skadlig kod ”uppringning” toohello ”kommando- och” mittpunkt hello skadliga domänen genom att lösa domännamn för skadlig kod.

![DNS-säkerhet bladet](./media/log-analytics-dns/dns-security-blade.png)

När du klickar på en klient IP-adress i listan hello loggen sökning öppnas och visar hello sökning detaljerad information om respektive hello-fråga. I följande exempel hello, DNS-Analytics upptäckte att hello kommunikation har utförts med en [IRCbot](https://www.microsoft.com/security/portal/threat/encyclopedia/entry.aspx?Name=Win32/IRCbot):

![Loggen sökresultat visar ircbot](./media/log-analytics-dns/ircbot.png)

hello information hjälper dig att tooidentify den:

- Klientens IP-adress som initierade hello-meddelande.
- Domännamnet som löser toohello skadliga IP.
- IP-adresser som hello domännamn matchar.
- Skadliga IP-adress.
- Allvarlighetsgraden hello problemet.
- Orsak till att godkänna hello skadliga IP.
- Identifieringstiden.

**Domäner som efterfrågas**. Ger hello vanligaste domännamn efterfrågas av hello DNS-klienter i din miljö. Du kan visa hello lista över alla hello domännamn som efterfrågas. Du kan också öka detaljnivån hello sökning begäran detaljer om ett visst domännamn i loggen sökningen.

![Domäner sökt bladet](./media/log-analytics-dns/domains-queried-blade.png)

**DNS-klienter**. Rapporter hello klienter *överträdelse hello tröskelvärdet* för antal frågor i hello valt tidsperiod. Du kan visa hello lista över alla hello DNS-klienter och hello information hello frågor som dem i loggen sökningen.

![Bladet för DNS-klienter](./media/log-analytics-dns/dns-clients-blade.png)

**Dynamiska DNS-registreringar**. Rapporter namnge registreringsfel. Alla registreringsfel för adress [resursposter](https://en.wikipedia.org/wiki/List_of_DNS_record_types) (typ A och AAAA) markeras tillsammans med hello klientens IP-adresser som gjort hello förfrågningar om klientregistrering. Du kan sedan använda denna information toofind hello grundorsaken till hello registreringsfel genom att följa dessa steg:

1. Hitta hello zoner som är auktoritära för hello namn som hello klienten försöker tooupdate.

2. Använd inventeringsinformation för hello lösning toocheck hello av zonen.

3. Kontrollera att hello dynamisk uppdatering för hello zonen är aktiverat.

4. Kontrollera om hello zonen har konfigurerats för säker dynamisk uppdatering eller inte.

    ![Bladet för dynamisk DNS-registrering](./media/log-analytics-dns/dynamic-dns-reg-blade.png)

**Namnge förfrågningar om klientregistrering**. hello övre panelen visas en trendlinje för lyckade och misslyckade DNS dynamisk uppdatering. hello nedre panelen visar hello topp 10 klienter som skickar toohello DNS-servrar, sorterade efter hello antalet fel misslyckade begäranden för DNS-uppdatering.

![Namnet registrering begäranden bladet ](./media/log-analytics-dns/name-reg-req-blade.png)

**Exempel på DDI Analytics frågor**. Innehåller en lista över hello vanligaste sökfrågor som hämta rådata analysdata direkt.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Exempelfrågor](./media/log-analytics-dns/queries.png)

Du kan använda de här frågorna som utgångspunkt för att skapa egna frågor för anpassad rapportering. hello frågor länka toohello DNS Analytics loggen söksidan där resultat visas:

- **Lista över DNS-servrar**. Visar en lista över alla DNS-servrar med deras associerade FQDN, domännamn, skogsnamnet och servern IP-adresser.
- **Lista över DNS-zoner**. Visar en lista över alla DNS-zoner med hello associerade zonnamnet, status för dynamisk uppdatering, namnservrar och DNSSEC-signering.
- **Oanvända resursposter**. Visar en lista över alla hello oanvända/föråldrade-resursposter. Den här listan innehåller hello resurs postens namn, typ av resurspost, hello associerade DNS-server, registrera Genereringstid och zonnamnet. Du kan använda den här listan tooidentify hello DNS-resursposter som inte längre används. Baserat på den här informationen kan du kan sedan ta bort dessa poster från hello DNS-servrar.
- **DNS-servrar fråga belastningen**. Visar information så att du kan få ett perspektiv för hello DNS läsa in på DNS-servrar. Denna information kan hjälpa dig att planera hello kapacitet för hello-servrar. Du kan gå toohello **mått** fliken toochange hello visa tooa grafiska visualiseringen. Den här vyn hjälper dig att förstå hur hello DNS ladda distribueras över DNS-servrar. DNS-fråga visar trender i frekvensen för varje server.

    ![Sökresultat för DNS-servrar frågan logg](./media/log-analytics-dns/dns-servers-query-load.png)

- **DNS-zoner fråga belastningen**. Visar hello DNS-zonen frågan per sekund statistik över alla zoner för hello på hello DNS-servrar som hanteras av hello-lösning. Klicka på hello **mått** fliken toochange hello vyn från detaljerade poster tooa grafiska visualisering av hello resultat.
- **Konfigurationshändelser**. Visar alla hello DNS konfigurationsändringshändelserna och associerade meddelanden. Därefter kan du filtrera händelserna baserat på tiden på hello händelse, händelse-ID, DNS-server eller aktivitetskategori. med hjälp av hello data kan du granska ändringar toospecific DNS-servrar vid specifika tidpunkter.
- **DNS-analytiska loggen**. Visar alla hello analytiska händelser på alla hello DNS-servrar som hanteras av hello-lösning. Därefter kan du filtrera händelserna baserat på tiden på hello händelse, händelse-ID, DNS-server klientens IP-Adressen som gjorts hello sökning frågan och fråga kategori för aktiviteten. DNS-server-analyshändelser aktivera Aktivitetsspårning på hello DNS-server. En analytiska händelse loggas varje gång hello servern skickar eller tar emot DNS-information.

### <a name="search-by-using-dns-analytics-log-search"></a>Sök med hjälp av DNS-Analytics loggen sökning

Du kan skapa en fråga på hello loggen söksidan. Du kan filtrera sökresultaten genom att använda aspekten kontroller. Du kan också skapa avancerade frågor tootransform, filter och rapporten på dina resultat. Starta med hello följande frågor:

1. I hello **frågan sökrutan**, typen `Type=DnsEvents` tooview alla hello DNS-händelser som genererats av hello DNS-servrar hanteras av hello-lösning. hello resultatlistan hello loggdata för alla händelser relaterade toolookup frågor, dynamiska registreringar och konfigurationsändringar.

    ![DnsEvents loggen Sök](./media/log-analytics-dns/log-search-dnsevents.png)  

    a. tooview hello loggdata för sökning-frågor, Välj **LookUpQuery** som hello **undertyp** filter från hello aspekten kontrollen hello vänster. En tabell som visar en lista över alla hello sökning frågan händelser för hello tidsperioden visas.

    b. Välj tooview hello loggdata för dynamiska registreringar **DynamicRegistration** som hello **undertyp** filter från hello aspekten kontrollen hello vänster. En tabell som innehåller alla händelser som hello dynamisk registrering för hello tidsperioden visas.

    c. tooview hello loggdata för konfigurationsändringar, Välj **ConfigurationChange** som hello **undertyp** filter från hello aspekten kontrollen hello vänster. En tabell med alla hello konfigurationsändringshändelserna för hello tidsperiod visas.

2. I hello **frågan sökrutan**, typen `Type=DnsInventory` tooview alla hello DNS-inventeringsrelaterade data för hello DNS-servrar hanteras av hello-lösning. hello resultatlistan hello loggdata för DNS-servrar, DNS-zoner och resursposter.

    ![DnsInventory loggen Sök](./media/log-analytics-dns/log-search-dnsinventory.png)

## <a name="feedback"></a>Feedback

Det finns två sätt kan du ge feedback:

- **UserVoice**. Bokför idéer för DNS-Analytics funktioner toowork på. Besök hello [Operations Management Suite UserVoice sidan](https://aka.ms/dnsanalyticsuservoice).
- **Ansluta till vår kommittén**. Vi vill alltid med nya kunder ansluta till vår kohorter tooget tidig toonew åtkomstfunktioner och hjälp oss att förbättra DNS Analytics. Om du vill ansluta till vår kohorter fylla [snabb undersökningen](https://aka.ms/dnsanalyticssurvey).

## <a name="next-steps"></a>Nästa steg

[Söka i loggar](log-analytics-log-searches.md) tooview detaljerad DNS-poster.
