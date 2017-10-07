---
title: "aaaLearn om funktionerna i BizTalk-tjänst versioner | Microsoft Docs"
description: "Jämför hello funktioner hello BizTalk-tjänst version: Frigör utvecklare, Basic, Standard och Premium. MABS, WABS."
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: c589629f-06b1-44bb-b8ca-1db71826ea59
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 81626fa743a7190e7c78a0fd90b3054a08982b02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-editions-chart"></a>BizTalk Services: Diagram över utgåvor

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Azure BizTalk Services erbjuder flera olika utgåvor. Använd den här artikeln toodetermine vilken utgåva passar dina behov scenario och företag.

## <a name="compare-hello-editions"></a>Jämför hello utgåvor
**Kostnadsfri (förhandsversion)**

Kan skapa och hantera hybridanslutningar. En Hybridanslutning är ett enkelt sätt tooconnect en Azure-webbplatsen tooan lokalt system, t.ex. SQL Server.

**Developer**

Innehåller hybridanslutningar, EAI- och EDI-meddelandebehandling med en lättanvänd hanteringsportal för handelspartners, samt stöd för vanliga EDI-scheman och omfattande EDI-bearbetning över X12 och AS2. Kan skapa vanliga EAI scenarier anslutningstjänsterna i hello moln med alla HTTP/S-, REST-, FTP-, WCF- och SFTP-protokoll tooread och skriva meddelanden.  Använda anslutningen tooon lokala LOB system med färdiga att använda SAP, Oracle eBusiness, Oracle DB, Siebel och SQL Server-kort. Använd en centrerad miljö för utvecklare med Visual Studio-verktyg för enkel utveckling och distribution. Begränsad toodevelopment och testa ändamål endast med ingen serviceavtalet (SLA).

**Basic**

Innehåller de flesta av hello Developer funktioner med ökningar i Hybridanslutningar EAI platslänkbryggor, EDI-avtal och BizTalk-kort Pack anslutningar. Erbjuder också hög tillgänglighet och hello alternativet tooscale med en serviceavtalet (SLA).

**Standard**

Innehåller alla grundläggande hello-funktionerna med ökningar i Hybridanslutningar EAI platslänkbryggor, EDI-avtal och BizTalk-kort Pack anslutningar. Erbjuder också hög tillgänglighet och hello alternativet tooscale med en serviceavtalet (SLA).

**Premium**

Innehåller alla Standard hello-funktioner med ökningar i Hybridanslutningar EAI platslänkbryggor, EDI-avtal och BizTalk-kort Pack anslutningar. Även arkivering, hög tillgänglighet och hello alternativet tooscale med en serviceavtalet (SLA).

## <a name="editions-chart"></a>Diagram över utgåvor
hello visar följande tabell hello skillnader.

<table border="1">
<tr bgcolor="FAF9F9">
        <th></th>
        <th>Kostnadsfri (förhandsversion)</th>
        <th>Developer</th>
        <th>Basic</th>
        <th>Standard</th>
        <th>Premium</th>
</tr>

<tr>
<td><strong>Startpris</strong></td>
<td colspan="5"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011"> Priser för Azure BizTalk Services</a> <br/><br/> <a HREF="http://azure.microsoft.com/pricing/calculator/?scenario=full"> Priskalkylator för Azure</a></td>
</tr>
<tr>
<td><strong>Minsta standardkonfiguration</strong></td>
<td>1 Free-enhet</td>
<td>1 Developer-enhet</td>
<td>1 Basic-enhet</td>
<td>1 Standard-enhet</td>
<td>1 Premium-enhet</td>
</tr>
<tr>
<td><strong>Skalning</strong></td>
<td>Ingen skalning</td>
<td>Ingen skalning</td>
<td>Ja, i steg om 1 Basic-enhet</td>
<td>Ja, i steg om 1 Standard-enhet</td>
<td>Ja, i steg om 1 Premium-enhet</td>
</tr>
<tr>
<td><strong>Högsta tillåtna skalning ut</strong></td>
<td>Ingen skalning</td>
<td>Ingen skalning</td>
<td>Too8 enheter</td>
<td>Too8 enheter</td>
<td>Too8 enheter</td>
</tr>
<tr>
<td><strong>EAI-bryggor per enhet</strong></td>
<td>Ingår inte</td>
<td>25</td>
<td>25</td>
<td>125</td>
<td>500</td>
</tr>
<tr>
<td><strong>EDI, AS2</strong>
<br/><br/>
Innefattar TPM-avtal</td>
<td>Ingår inte</td>
<td>Ingår. 10 avtal per enhet.</td>
<td>Ingår. 50 avtal per enhet.</td>
<td>Ingår. 250 avtal per enhet.</td>
<td>Ingår. 1 000 avtal per enhet.</td>
</tr>
<tr>
<td><strong>Hybridanslutningar per enhet</strong></td>
<td>5</td>
<td>5</td>
<td>10</td>
<td>50</td>
<td>100</td>
</tr>
<tr>
<td><strong>Hybridanslutningens dataöverföring (GB) per enhet</strong></td>
<td>5</td>
<td>5</td>
<td>50</td>
<td>250</td>
<td>500</td>
</tr>
<tr>
<td><strong>BizTalk-tjänst för kortet anslutningar tooon lokala LOB-system</strong></td>
<td>Ingår inte</td>
<td>1 anslutning</td>
<td>2 anslutningar</td>
<td>5 anslutningar</td>
<td>25 anslutningar</td>
</tr>
<tr>
<td align="left"><strong>Protokoll/system som stöds:</strong>
<ul>
<li>HTTP</li>
<li>HTTPS</li>
<li>FTP</li>
<li>SFTP</li>
<li>WCF</li>
<li>Service Bus (SB)</li>
<li>Azure-blobb</li>
<li>REST API:er</li>
</ul>
</td>
<td>Ingår inte</td>
<td>Ingår</td>
<td>Ingår</td>
<td>Ingår</td>
<td>Ingår</td>
</tr>
<tr>
<td><strong>Hög tillgänglighet</strong>
<br/><br/>
För serviceavtal (SLA), läs mer i <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011"> Prissättning för Azure BizTalk Services</a>.
</td>
<td>Ingår inte</td>
<td>Ingår inte</td>
<td>Ingår</td>
<td>Ingår</td>
<td>Ingår</td>
</tr>
<tr>
<td><strong>Säkerhetskopiering och återställning</strong></td>
<td>Ingår inte</td>
<td>Ingår</td>
<td>Ingår</td>
<td>Ingår</td>
<td>Ingår</td>
</tr>
<tr>
<td><strong>Spårning</strong></td>
<td>Ingår inte</td>
<td>Ingår</td>
<td>Ingår</td>
<td>Ingår</td>
<td>Ingår</td>
</tr>
<tr>
<td><strong>Arkivering</strong><br/><br/>
Innehåller NRR (Non-Repudiation of Receipt) och hämtning av spårade meddelanden</td>
<td>Ingår inte</td>
<td>Ingår</td>
<td>Ingår inte</td>
<td>Ingår inte</td>
<td>Ingår</td>
</tr>
<tr>
<td><strong>Användning av anpassad kod</strong></td>
<td>Ingår inte</td>
<td>Ingår</td>
<td>Ingår</td>
<td>Ingår</td>
<td>Ingår</td>
</tr>
<tr>
<td><strong>Användning av transformeringar, inklusive anpassad XSLT</strong></td>
<td>Ingår inte</td>
<td>Ingår</td>
<td>Ingår</td>
<td>Ingår</td>
<td>Ingår</td>
</tr>
</table>

> [!NOTE]
> För återhämtning vid maskinvarufel innebär hög tillgänglighet att ha flera virtuella datorer i en enda BizTalk-enhet.
> 
> 

## <a name="faqs"></a>Vanliga frågor och svar
#### <a name="what-is-a-biztalk-unit"></a>Vad är en BizTalk-enhet?
”Enhet” är hello atomiska en BizTalk-tjänst för Azure-distribution. Varje utgåva levereras med en enhet som har annan beräkningskapacitet och minne. Till exempel har en Basic-enhet mer databearbetning än Developer, och Standard har mer databearbetning än Basic och så vidare. En BizTalk-tjänst skalas i enheter.

#### <a name="what-is-hello-difference-between-biztalk-services-and-azure-biztalk-vm"></a>Vad är hello skillnaden mellan BizTalk-tjänst och Azure BizTalk VM?
BizTalk-tjänst ger en true plattform som en-tjänst (PaaS)-arkitektur för att skapa lösningar för katalogintegrering i hello molnet. Med hello PaaS modell du helt fokusera på hello programlogiken och lämna alla hello infrastruktur för hantering av tooMicrosoft, inklusive:

* Inga måste toomanage eller korrigering virtuella datorer.
* Microsoft garanterar tillgängligheten.
* Du kan styra skala på begäran genom att helt enkelt begära mer eller mindre kapacitet via hello Azure-portalen.

BizTalk Server i Azure Virtual Machines ger en ”infrastruktur som en tjänst”-arkitektur (IaaS). Du skapar virtuella datorer och konfigurera dem exakt som den lokala miljön, vilket gör det enklare toorun befintliga program i hello moln med inga kodändringar. Med IaaS är du fortfarande ansvarig för att konfigurera hello virtuella datorer, hantera hello virtuella datorer (till exempel installera programvara och operativsystemskorrigeringar) och bygga hello program för hög tillgänglighet.

Om du funderar på att skapa nya integrationslösningar som minimerar din infrastrukturhantering, bör du använda BizTalk Services. Om du behöver tooquickly migrera dina befintliga BizTalk-lösningar eller söker efter en på-begäran miljö toodevelop och testa BizTalk Server program, Använd BizTalk Server på en virtuell dator i Azure.

#### <a name="what-is-hello-difference-between-biztalk-adapter-service-and-hybrid-connections"></a>Vad är hello skillnaden mellan nätverkskort BizTalk-tjänst och Hybridanslutningar?
hello BizTalk-tjänst för nätverkskortet används av en Azure BizTalk-tjänst. hello BizTalk-tjänst för kortet använder hello BizTalk Adapter Pack tooconnect tooan lokalt rad Affärsappar (LOB) system. En Hybridanslutning ger ett enkelt och praktiskt sätt tooconnect Azure-program som hello Web Apps i Azure App Service och Azure Mobile Services tooan lokal resurs.

#### <a name="what-does-hybrid-connection-data-transfer-gb-per-unit-mean-is-this-per-minutehourdayweekmonth-what-happens-when-hello-limit-is-reached"></a>Vad innebär ”Hybridanslutningens dataöverföring (GB) per enhet”? Är detta per minut/timme/dag/vecka/månad? Vad händer när hello gränsen har nåtts?
Hej Hybridanslutning kostnaden per enhet beror på hello BizTalk Services edition. Kostnaderna beror helt enkelt på hur mycket data du överför. Att exempelvis överföra 10 GB data per dag kostar mindre än att överföra 100 GB per dag. Använd hello [Priskalkylatorn](https://azure.microsoft.com/pricing/calculator/?scenario=full) för BizTalk-tjänst toodetermine kostnader. Normalt tillämpas hello gränser dagligen. Om du överskrider hello gränsen debiteras alla överförbrukning med hello 1 USD per GB.

#### <a name="when-i-create-an-agreement-in-biztalk-services-why-does-hello-number-of-bridges-go-up-by-two-instead-of-just-one"></a>När jag skapar ett avtal i BizTalk-tjänst, varför hello antalet bryggor går av två i stället för en?
Varje avtal består av två olika bryggor: en kommunikationsbrygga på sändningssidan och en kommunikationsbrygga på mottagningssidan.

#### <a name="what-happens-when-i-hit-hello-quota-limit-on-hello-number-of-bridges-or-agreements"></a>Vad händer när jag träffar hello kvotgräns för hello antalet bryggor eller avtalen?
Du kan inte toodeploy eventuella nya bryggor eller skapa några nya avtal. toodeploy mer, behöver du tooscale toomore enheter hello BizTalk-tjänst eller uppgradera tooa högre edition.

#### <a name="how-do-i-migrate-from-one-tier-of-biztalk-services-tooanother"></a>Hur migrera från en nivå i BizTalk-tjänst tooanother?
hello Free edition kan inte migreras eller 'skalats ut' tooanother nivå och går inte att säkerhetskopiera och återställa tooanother nivå. Om du behöver ett annat skikt kan du skapa en ny BizTalk Service med hjälp av hello ny nivå. Alla artefakter som skapats med hjälp av hello Free edition, inklusive hybridanslutningar, måste toobe återskapas i hello ny BizTalk Service. 

För hello kvarvarande versioner, Använd hello säkerhetskopiering och återställning för att migrera din artefakter från en nivå tooanother. Till exempel säkerhetskopiera din artefakter i hello standardnivån och återställa dem toohello Premium-nivån. [BizTalk-tjänst: Säkerhetskopiera och återställa](biztalk-backup-restore.md) beskriver migreringssökvägar som hello stöds och visar vilka artefakter säkerhetskopieras. Observera att hybridanslutningar inte säkerhetskopieras. När du säkerhetskopierar och återställer tooa ny nivå, återskapa du sedan hello hybridanslutningar.  

#### <a name="is-hello-biztalk-adapter-service-included-in-hello-service-how-do-i-receive-hello-software"></a>Ingår hello BizTalk-tjänst för nätverkskort i hello tjänsten? Hur får jag hello programvara?
Ja, hello BizTalk-tjänst för nätverkskortet med hello BizTalk Adapter Pack ingår i hello Azure BizTalk Services SDK [hämta](http://www.microsoft.com/download/details.aspx?id=39087).

## <a name="next-steps"></a>Nästa steg
toocreate Azure BizTalk-tjänst i hello Azure går för[BizTalk-tjänst: etablering med hjälp av hello Azure-portalen](biztalk-provision-services.md). för att skapa program, gå toostart[Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="additional-resources"></a>Ytterligare resurser
* [BizTalk-tjänst: Etablering med hjälp av hello Azure-portalen](biztalk-provision-services.md)<br/>
* [BizTalk Services: Etablering av statusdiagram](biztalk-service-state-chart.md)<br/>
* [BizTalk Services: Flikarna Instrumentpanel, Övervakare och Skalning](biztalk-dashboard-monitor-scale-tabs.md)<br/>
* [BizTalk Services: Säkerhetskopiering och återställning](biztalk-backup-restore.md)<br/>
* [BizTalk Services: Begränsning](biztalk-throttling-thresholds.md)<br/>
* [BizTalk Services: Utfärdarens namn och nyckel](biztalk-issuer-name-issuer-key.md)<br/>
* [Hur jag börja använda hello Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>

