---
title: "aaaDashboard, övervaka, skala, konfigurera och Hybrid-anslutningar i BizTalk-tjänst | Microsoft Docs"
description: "Läs mer om hello kontroller och övervaka prestanda på hello klassiska portal flikar för BizTalk-tjänst: instrumentpanelen, övervaka, skala, konfigurera och Hybridanslutningar. MABS WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 7a1815db-0de2-4274-8be0-198c1b077324
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: c9fafdad20489571ee3849bbacd2c2b10933154f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="review-hello-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a>Granska hello instrumentpanelen, övervaka, skala, konfigurera och Hybridanslutning flikar

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

När du skapar BizTalk Service och distribuera ditt program kan du ändra vissa inställningar för hello BizTalk-tjänst och övervaka hello programprestanda. 

När du öppnar hello klassiska Azure-portalen kan du placeras automatiskt på hello **alla objekt** fliken tooview din BizTalk Service väljer BizTalk Service i hello **alla objekt** TABB eller välj hello **BIZTALK-tjänst** fliken; och välj sedan namnet på din BizTalk Service.

Med följande flikar hello öppnas ett nytt fönster. Det här avsnittet beskrivs de här flikarna.

## <a name="quickstart-quickstartquickstart"></a>Snabbstart (![Snabbstart][Quickstart])
Beroende på hello BizTalk Services Edition kanske inte alla alternativ som finns tillgängliga. 

<table border="1">
    <tr>
        <td><strong>Hämta hello-verktyg</strong></td>
        <td>Hämta hello BizTalk Services SDK tooinstall hello Visual Studio-projektmallar på utvecklingsdatorn lokalt. De här mallarna skapar hello <strong>BizTalk-tjänst</strong> (brygga) och hello <strong>BizTalk-tjänstens artefakter</strong> (Transform) Visual Studio-projekt som är distribuerade tooyour BizTalk Service.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">Hur jag börja använda hello Azure BizTalk Services SDK </a> och <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">installerar hello Azure BizTalk Services SDK</a> visar hello steg tooget igång.
        </td>
    </tr>
    <tr>
        <td><strong>Skapa partner avtal</strong></td>
        <td>Öppnas hello Azure BizTalk-Services-portalen i Azure där du lägger till partner och skapa X12 AS2-, och EDI EDIFACT-avtal.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurera komponenter för EDI-meddelanden på BizTalk-Services-portalen</a> visar hello steg tooget igång.
        </td>
    </tr>

<tr>
        <td><strong>Mer information om BizTalk-tjänst</strong></td>
        <td>Gå toohello <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">learning center</a> toolearn mer om Azure BizTalk-tjänst.</td>
</tr>
</table>


I Aktivitetsfältet längst ned hello hello kan du:

<table border="1">

<tr>
<td><strong>Hantera</strong> programdistributionen</td>
<td>Öppnar hello Azure BizTalk-Services-portalen. hello BizTalk-Services-portalen är hello ingång tooEDI konfigurationen, inklusive lägga till partner och skapa X12 AS2-, och EDIFACT-avtal.
<br/><br/>
Detta är hello samma som <strong>skapa partner avtal</strong> på hello <strong>Snabbstart</strong> fliken.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurera komponenter för EDI-meddelanden på BizTalk-Services-portalen</a> finns mer information om hello BizTalk-Services-portalen.</td>
</tr>

<tr>
<td><strong>Anslutningsinformationen</strong> av hello Access Control Namespace</td>
<td>När du väljer anslutningsinformationen hello Access Control Namespace, standard utfärdaren och som standard nyckeln visas. Du kan kopiera dessa värden.
<br/><br/>
Du kan också öppna hello Access Control-portalen. <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Skapa en åtkomstkontroll Namespace</a> finns mer information om hello Access Control-portalen.</td>
</tr>

<tr>
<td><strong>Synkronisera nycklar</strong> i hello Storage-konto</td>
<td>När du skapar ett lagringskonto skapas automatiskt en primär nyckel och en sekundär nyckel. Dessa krypteringsnycklar styra åtkomst tooyour Storage-konto. BizTalk Service används automatiskt hello primärnyckel. <strong>Synkronisera nycklar</strong> aktivera användare tooswitch mellan hello primärnyckel och hello sekundärnyckeln utan att störa hello BizTalk Service.
<br/><br/>
Till exempel vill du hello BizTalk Service toouse en ny primärnyckel för hello Storage-konto. toodo detta:
<br/><br/>
<ol>
<li>Välj BizTalk Service och välj <strong>synkronisera nycklar</strong>. Välj hello sekundärnyckeln. När du gör detta startar hello BizTalk Service med hello sekundärnyckeln.</li>
<li>Välj ditt lagringskonto i hello klassiska Azure-portalen och återskapa hello primärnyckel. Kom ihåg att BizTalk Service använder hello sekundärnyckeln.</li>
<li>Välj BizTalk Service och välj <strong>synkronisera nycklar</strong>. Nu Välj hello primärnyckel. Detta är hello nya primärnyckeln som du har återskapats.</li>
<li>Välj ditt lagringskonto i hello klassiska Azure-portalen och återskapa hello sekundärnyckeln.</li>
</ol>
<br/>
Den här processen kallas ”förnyelse nycklar”. hello syftet är tooenable användare tooswitch mellan hello primärnyckel och hello sekundärnyckeln utan att störa hello BizTalk Service.</td>
</tr>

<tr>
<td><strong>Ta bort</strong> ditt program</td>
<td>När du väljer Ta bort BizTalk Service och alla objekt som har distribuerats tooit tas bort.</td>
</tr>
</table>


## <a name="dashboard"></a>Instrumentpanel
Beroende på hello BizTalk Services Edition kanske inte alla alternativ som finns tillgängliga. 

När du markerar du namnet på BizTalk Service visas hello instrumentpanelen. I instrumentpanelen kan du:

##### <a name="usage-overview-shows-hello-number-of-used-hybrid-connections"></a>Översikt för användning: Visar hello antalet använda Hybridanslutningar
Visar även hello dataanvändning i GB. 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a>Mått diagrammet: Visar en fast lista med prestandamått
De här måtten ange realtid värden om hello hälsotillstånd hello BizTalk Service. Du kan också välja hello **relativa** eller **absolut** värden och hello tidsintervallet **intervall** av hello mått som visas i diagrammet hello. 

En beskrivning av dessa prestandamått gå för[tillgängliga mått](#Metrics) i det här avsnittet.

##### <a name="quick-glance-lists-your-biztalk-service-properties"></a>Snabböversikten: Visar en lista över BizTalk Service-egenskaper
<table border="1">

<tr>
<td><strong>Uppdatera autentiseringsuppgifterna för spårning av databasen</strong></td>
<td>Ändringar hello användarnamnet och lösenordet som används för toolog i hello spårning av databasen.</td>
</tr>
<tr>
<td><strong>Uppdatera SSL-certifikat</strong></td>
<td>Uppdatera hello BizTalk Service toouse ett annat SSL-certifikat. Ett självsignerat certifikat för SSL skapas automatiskt när du <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">skapa hello BizTalk Service</a>.</td>
</tr>
<tr>
<td><strong>Hämta certifikatet</strong></td>
<td>Du kan hämta hello SSL-certifikatet som används av din lokala dator BizTalk Service tooa.</td>
</tr>
<tr>
<td><strong>Status</strong></td>
<td>Visar hello aktuell status för BizTalk Service. Se <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk-tjänst: tjänsten tillstånd diagram</a>. </td>
</tr>
<tr>
<td><strong>Tjänst-URL</strong></td>
<td>hello-URL för BizTalk Service. Detta är hello samma som hello <strong>domän-URL</strong> anges när BizTalk Service skapas.</td>
</tr>
<tr>
<td><strong>Offentliga virtuella IP (VIP)-adressen</strong></td>
<td>hello IP-adress som tilldelats tooyour BizTalk Service. Den används för alla inkommande slutpunkter och är hello källadress för utgående trafik. IP-adressen hör tooyour BizTalk Service så länge den har skapats. Om du tar bort hello BizTalk Service tilldelas hello IP-adress tooanother BizTalk Service.</td>
</tr>
<tr>
<td><strong>ACS-Namespace</strong></td>
<td>Verifierar med hello BizTalk Service.</td>
</tr>
<tr>
<td><strong>Utgåva</strong></td>
<td>Visar hello Edition anges när hello BizTalk Service skapas.</td>
</tr>
<tr>
<td><strong>Plats</strong></td>
<td>Visar hello geografiska region som är värd för BizTalk Service.</td>
</tr>
<tr>
<td><strong>Skapa</strong></td>
<td>Visar hello datum och tid hello BizTalk Service har skapats.</td>
</tr>
<tr>
<td><strong>Spårning av databasen</strong></td>
<td>hello Azure SQL Database namn som lagrar hello spårning tabeller som används av BizTalk Service. 
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Krav för Explained</a> innehåller information om hello spårning av databasen.</td>
</tr>
<tr>
<td><strong>Övervaka/arkivering lagring</strong></td>
<td>hello Azure Storage kontonamn som lagrar hello övervakning utdata från BizTalk Service.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Krav för Explained</a> innehåller information om hello Storage-konto.</td>
</tr>
<tr>
<td><strong>Prenumerationsnamn</strong></td>
<td>Visar hello-prenumeration som är värd för BizTalk Service. hello prenumeration styr åtkomst toohello klassiska Azure-portalen.</td>
</tr>
<tr>
<td><strong>Prenumerations-ID</strong></td>
<td>Ett prenumerations-ID genereras automatiskt när en prenumeration har skapats. När du använder REST API: er kan behöva du tooenter hello prenumerations-ID.</td>
</tr>
</table>

[BizTalk-tjänst: Etablering med hjälp av Azure klassiska portal](http://go.microsoft.com/fwlink/p/?LinkID=302280) visar hello steg toocreate en BizTalk Service.

##### <a name="manage-connection-information-sync-keys-and-delete-in-hello-task-bar"></a>Hantera anslutningsinformationen, synkronisera nycklar, och ta bort i Aktivitetsfältet hello:
<table border="1">

<tr>
<td><strong>Hantera</strong> programdistributionen</td>
<td>Öppnar hello Azure BizTalk-Services-portalen. hello BizTalk-Services-portalen är hello ingång tooEDI konfigurationen, inklusive lägga till partner och skapa X12 AS2-, och EDIFACT-avtal.
<br/><br/>
Detta är hello samma som <strong>skapa partner avtal</strong> på hello <strong>Snabbstart</strong> fliken.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurera komponenter för EDI-meddelanden på BizTalk-Services-portalen</a> finns mer information om hello BizTalk-Services-portalen.</td>
</tr>
<tr>
<td><strong>Anslutningsinformationen</strong> av hello Access Control Namespace</td>
<td>Visar hello Access Control Namespace, standard utfärdare och standard nyckeln värden. som kan kopieras.
<br/><br/>
Du kan också öppna hello Access Control-portalen. Access Control portalen är hello samma som med hello Active Directory på hello vänstra navigeringsfönstret.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Hantera din ACS-Namespace</a> finns mer information om hello Access Control-portalen.</td>
</tr>
<tr>
<td><strong>Synkronisera nycklar</strong> i hello Storage-konto</td>
<td>När du skapar ett lagringskonto skapas automatiskt en primär nyckel och en sekundär nyckel. Dessa krypteringsnycklar styra åtkomst tooyour Storage-konto. BizTalk Service används automatiskt hello primärnyckel. <strong>Synkronisera nycklar</strong> aktivera användare tooswitch mellan hello primärnyckel och hello sekundärnyckeln utan att störa hello BizTalk Service.
<br/><br/>
Till exempel vill du hello BizTalk Service toouse en ny primärnyckel för hello Storage-konto. toodo detta:
<br/><br/>
<ol>
<li>Välj BizTalk Service och välj <strong>synkronisera nycklar</strong>. Välj hello sekundärnyckeln. När du gör detta startar hello BizTalk Service med hello sekundärnyckeln.</li>
<li>Välj ditt lagringskonto i hello klassiska Azure-portalen och återskapa hello primärnyckel. Kom ihåg att BizTalk Service använder hello sekundärnyckeln.</li>
<li>Välj BizTalk Service och välj <strong>synkronisera nycklar</strong>. Nu Välj hello primärnyckel. Detta är hello nya primärnyckeln som du har återskapats.</li>
<li>Välj ditt lagringskonto i hello klassiska Azure-portalen och återskapa hello sekundärnyckeln.</li>
</ol>
<br/>
Den här processen kallas ”förnyelse nycklar”. hello syftet är tooenable användare tooswitch mellan hello primärnyckel och hello sekundärnyckeln utan att störa hello BizTalk Service.</td>
</tr>

<tr>
<td><strong>Ta bort</strong> ditt program</td>
<td>BizTalk Service och alla objekt som har distribuerats tooit tas bort.</td>
</tr>
</table>


## <a name="monitor"></a>Övervaka
Gäller inte toohello Free Edition.

När du markerar du namnet på BizTalk Service hello övervakningsfliken är tillgängligt och visar hello följande:

##### <a name="metric-graph-displays-hello-selected-performance-metrics"></a>Mått diagram: Visar hello valt prestandamått
De här måtten ange realtid värden om hello hälsotillstånd hello BizTalk Service. Du kan välja vilka prestandamått visas. Maximalt sex prestandamått kan visas samtidigt. 

Du kan också välja hello **relativa** eller **absolut** värden och hello tidsintervallet **intervall** av hello mått som visas. 

##### <a name="tooremove-or-display-metrics-in-hello-graph"></a>tooremove eller visa mått i hello diagram:
1. Välj hello **övervakaren** fliken.
2. Välj **lägga till mätvärden** i Aktivitetsfältet hello:  
   ![Välj Lägg till mått][AddMetrics]
3. Kontrollera hello prestandamått som du vill toodisplay.
4. Välj hello markering tooreturn toohello **övervakaren** fliken.
5. Välj hello cirkel nästa toohello mått toodisplay som mått värde i hello diagram.  
   
    Till exempel hello **CPU-användning** mått är nedtonad; utdata visas inte i hello diagram:  
   ![CPU-användning mått är nedtonad][GrayedMetric]  
   
    Välj hello nedtonad cirkel tooenable hello **CPU-användning** mått toodisplay utdata i hello diagram:  
   ![CPU-användning mått är aktiverad][EnabledMetric]
6. tooremove ett mått från hello visa diagrammet och hello listan, Välj **ta bort måttet** i hello Aktivitetsfältet. tooadd hello mått tillbaka toohello listan markerar **lägga till mätvärden** i Aktivitetsfältet hello Kontrollera hello mått och välj hello markering tooreturn toohello **övervakaren** fliken. Välj hello nedtonade cirkel tooenable hello mått.

## <a name="Metrics"></a>Tillgängliga mått
hello följande räknare/prestandamått är tillgängliga:

<table border="1">

<tr>
<td><strong>RountdTrip svarstid</strong></td>
<td>Visar hello Genomsnittlig tid i millisekunder (ms) tooprocess ett meddelande från hello tid hello-meddelande tas emot förrän hello-meddelande bearbetas fullständigt av hello BizTalk Service över alla bryggor. Endast meddelanden som har bearbetat räknas.<br/><br/>
En tidsstämpel skapas när hello följande händelser inträffar:
<ul>
<li>Meddelandet anger hello gateway</li>
<li>Meddelandet är routade toohello mål</li>
<li>Mål-svar har tagits emot</li>
<li>Mål bekräftelse svaret toohello gateway</li>
</ul>
<br/>
Det här måttet visar hello resultatet av hello följande beräkning:
<br/><br/>
[Mål bekräftelse svaret toohello gateway] - [meddelandet anger hello gateway]</td>
</tr>
<tr>
<td><strong>Fel vid källan</strong></td>
<td>Visar hello sammanlagt antal meddelanden som inte godkänts av hello BizTalk Service när hämta meddelanden från hello källa slutpunkter.</td>
</tr>
<tr>
<td><strong>CPU-användning</strong></td>
<td>Visar hello genomsnittlig processortid i procent för alla rollinstanser.</td>
</tr>
<tr>
<td><strong>Svarstid för bearbetning</strong></td>
<td>Visar hello Genomsnittlig tid i millisekunder (ms) tooprocess ett meddelande av hello BizTalk Service över alla bryggor, exklusive hello tid som ägnats åt mål. Endast meddelanden som har bearbetat räknas.<br/><br/>
En tidsstämpel skapas när var och en av hello följande händelser inträffar:

<ul>
<li>Meddelandet anger hello gateway</li>
<li>Meddelandet är routade toohello mål</li>
<li>Mål-svar har tagits emot</li>
<li>Mål bekräftelse svaret toohello gateway</li>
</ul>
<br/>Det här måttet visar hello resultatet av hello följande beräkning:<br/><br/>
[Mål bekräftelse svaret toohello gateway] - [meddelandet anger hello gateway] - [mål svar] + [meddelandet är routade toohello mål]</td>
</tr>
<tr>
<td><strong>Fel i processen</strong></td>
<td>Visar hello sammanlagt antal meddelanden som misslyckades under bearbetning av hello BizTalk Service över alla hello bryggor inom ett tidsintervall.</td>
</tr>
<tr>
<td><strong>Meddelanden som skickas</strong></td>
<td>Visar hello Totalt antal meddelanden som skickas av hello BizTalk Service över alla bryggor inom ett tidsintervall. Det här måttet ökar stegvis när ett meddelande som skickas från en pipeline når hello väg mål. Det här måttet visar inte att ett meddelande har bearbetat.<br/><br/>
I ett scenario med Request-Reply ökas hello mått när hello väg mål skickar en inleverans bekräftelse tillbaka toohello pipeline.</td>
</tr>
<tr>
<td><strong>Mottagna meddelanden</strong></td>
<td>Visar hello Totalt antal meddelanden mottagna genom hello BizTalk Service över alla bryggor inom ett tidsintervall. Det här måttet ökar stegvis när ett nytt meddelande tas emot av hello pipeline.</td>
</tr>
<tr>
<td><strong>Meddelanden i processen</strong></td>
<td>Visar hello sammanlagt antal meddelanden som för närvarande bearbetas av hello BizTalk Service inom ett tidsintervall.</td>
</tr>
<tr>
<td><strong>Meddelanden som bearbetas</strong></td>
<td>Visar hello Totalt antal meddelanden som har behandlats av hello BizTalk Service över alla bryggor inom ett tidsintervall. Det här måttet ökar stegvis när ett meddelande har tas emot av hello pipeline och har routade toohello mål.</td>
</tr>
</table>


## <a name="scale"></a>Skala
I hello skala, du lägger till eller subtrahera hello antalet enheter som används av BizTalk Service. Som standard finns en enhet är konfigurerad. Ytterligare enheter kan läggas till tooscale BizTalk Service. Om du ökar hello skala ökar genomströmningen. hello mängden resurser ökar också inklusive distribuerade platslänkbryggor, avtal, LOB-anslutningar och processorkraft. Exempelvis kan du öka hello skala från 1 enhet too2 enheter. I så fall kan distribuera du dubbla hello antalet platslänkbryggor, dubbla hello avtal, dubbla hello LOB-anslutningar och dubbla hello processorkraft.

Vissa versioner av BizTalk ger inte ett alternativ för skala. I så fall kan är en enhet tillåtet. toodetermine hur många enheter som den här versionen kan skalas för referera[BizTalk-tjänst: utgåvor diagram](biztalk-editions-feature-chart.md).

Öka hello antalet enheter som kan påverka priser. Om du ökar hello enheter, väljer du **spara** visar ett meddelande som talar om fakturering påverkas. Sedan kan du välja toocontinue. Om du ökar hello antalet enheter ändras hello BizTalk Service status från aktivt tooUpdating. I hello uppdaterar tillstånd fortsätter BizTalk Service toorun.

[BizTalk-tjänst: Utgåvor diagram](biztalk-editions-feature-chart.md) definierar ”enhet”.

## <a name="configure"></a>Konfigurera
Gäller inte tooHybrid anslutningar.

Anger hello Status för säkerhetskopiering tooNone eller automatisk. Om värdet är tooNone, inga säkerhetskopior skapas automatiskt. Ange tooAutomatic, när du konfigurerar hello säkerhetskopieringsplatsen, hello ofta hello säkerhetskopiering och hur länge tookeep hello säkerhetskopior. 

[BizTalk-tjänst: Säkerhetskopiera och återställa](biztalk-backup-restore.md) innehåller hello information. 

## <a name="HybridConnections"></a>Hybridanslutningar
Hybridanslutningar ansluter ett Azure-program, som Webbappar eller Mobilappar i Azure App Service, tooan lokala resursen som använder en statisk TCP-port, till exempel SQL Server, MySQL, http-webb-API: er och mest anpassade webbtjänster. Hybridanslutningar hanteras i BizTalk-tjänst i hello klassiska Azure-portalen.

toocreate Hybridanslutningar i Azure App Service finns [åtkomst till lokala resurser genom att använda hybridanslutningar i Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md).

toocreate eller hantera Hybridanslutningar i Azure BizTalk Services, se [Hybridanslutningar](integration-hybrid-connection-overview.md).

## <a name="next"></a>Nästa
Nu när du är bekant med hello olika flikarna, du kan lära dig mer om hello Azure BizTalk-Services-funktioner:

* [BizTalk Services: Begränsning](biztalk-throttling-thresholds.md)  
* [BizTalk Services: Utfärdarens namn och nyckel](biztalk-issuer-name-issuer-key.md)  
* [BizTalk Services: Säkerhetskopiering och återställning](biztalk-backup-restore.md)

## <a name="see-also"></a>Se även
* [Hybridanslutningar](integration-hybrid-connection-overview.md)  
* [BizTalk-tjänst: Utvecklare, Basic, Standard och Premium-utgåvor diagram](biztalk-editions-feature-chart.md)  
* [BizTalk-tjänst: Etablering med hjälp av Azure klassiska portal](biztalk-provision-services.md)  
* [BizTalk-tjänst: BizTalk-tjänstens tillstånd-diagram](biztalk-service-state-chart.md)  
* [Hur jag börja använda hello Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[Quickstart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png

