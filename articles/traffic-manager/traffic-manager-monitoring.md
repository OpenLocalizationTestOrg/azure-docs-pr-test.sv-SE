---
title: "aaaAzure Traffic Manager-slutpunktsövervakning | Microsoft Docs"
description: "Den här artikeln kan hjälpa dig att förstå hur Traffic Manager använder slutpunktsövervakning och automatisk endpoint redundans toohelp Azure kunder distribuera program med hög tillgänglighet"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: fff25ac3-d13a-4af9-8916-7c72e3d64bc7
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/22/2017
ms.author: kumud
ms.openlocfilehash: b4862499c88bdb1951833d06199b034a07ac7576
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="traffic-manager-endpoint-monitoring"></a>Slutpunktsövervakning av Traffic Manager

Azure Traffic Manager innehåller inbyggda slutpunktsövervakning och automatisk endpoint redundans. Den här funktionen hjälper dig att leverera program med hög tillgänglighet som är flexibel tooendpoint fel, inklusive Azure-region fel.

## <a name="configure-endpoint-monitoring"></a>Konfigurera slutpunktsövervakning

tooconfigure slutpunktsövervakning, måste du ange hello följande inställningar på Traffic Manager-profilen:

* **Protokollet**. Välj HTTP, HTTPS eller TCP som hello protokoll med Traffic Manager när sökning endpoint-toocheck dess hälsa. HTTPS-övervakning kontrollerar inte om SSL-certifikatet är giltigt--den kontrollerar att hello certifikat.
* **Port**. Välj hello-port som används för hello-begäran.
* **Sökvägen**. Den här inställningen gäller bara för hello HTTP och HTTPS-protokoll, som att ange hello sökvägsinställningen är obligatorisk. Att erbjuda den här inställningen för hello TCP-protokollet övervakningsresultaten i ett fel. TCP-protokollet ge hello relativa sökvägen och hello webbsidan hello eller hello-fil som hello övervakning har åtkomst. Ett snedstreck (/) är ett giltigt för hello relativ sökväg. Det här värdet innebär att hello-filen är i hello rotkatalog (standard).
* **Avsökning av intervall**. Det här värdet anger hur ofta en slutpunkt är markerat för dess hälsa från en avsöknings Traffic Manager-agent. Du kan ange två värden här: 30 sekunder (vanlig sökning) och 10 sekunder (snabb avsökning). Om inga värden har angetts, anger hello profil tooa standardvärdet på 30 sekunder. Besök hello [Traffic Manager priser](https://azure.microsoft.com/pricing/details/traffic-manager) sidan toolearn mer om snabb avsöknings priser.
* **Antalet fel tolereras**. Det här värdet anger hur många fel som en avsöknings Traffic Manager-agenten kan tolerera innan du markerar den slutpunkten som ohälsosamt. Värdet kan variera mellan 0 och 9. Värdet 0 innebär ett enstaka övervakning fel kan orsaka att slutpunkten toobe markeras som ohälsosam. Om inget värde anges används hello standardvärdet 3.
* **Övervaka Timeout**. Den här egenskapen anger hello mängden tid hello Traffic Manager avsöknings agent ska vänta innan som kontrollerar ett fel när en avsökning för kontroll av hälsotillstånd skickas toohello slutpunkt. Om hello avsökning av intervallet anges too30 sekunder och du kan ange hello Timeout-värde mellan 5 och 10 sekunder. Om inget värde anges används standardvärdet 10 sekunder. Om hello avsökning av intervallet anges too10 sekunder och du kan ange hello Timeout-värde mellan 5 och 9 sekunder. Om inget Timeout-värde har angetts används standardvärdet 9 sekunder.

![Slutpunktsövervakning av Traffic Manager](./media/traffic-manager-monitoring/endpoint-monitoring-settings.png)

**Bild 1: Traffic Manager-slutpunktsövervakning**

## <a name="how-endpoint-monitoring-works"></a>Så här fungerar slutpunktsövervakning

Om hello övervakning är HTTP eller HTTPS, gör avsöknings hello Traffic Manager-agenten en GET-begäran toohello slutpunkten med hjälp av hello protokoll, portar och relativ sökväg som anges. Om du hämtar tillbaka ett svar 200 OK, och sedan denna slutpunkt anses felfri. Om hello svaret är ett annat värde eller, om inget svar tas emot inom tidsgränsen för hello anges, sedan hello Traffic Manager sökning agent försöker på nytt enligt toohello tolereras antalet fel inställningen (inget nytt försök görs om den här inställningen är 0) . Om hello antal upprepade fel är högre än hello tolereras antalet fel, sedan markeras denna slutpunkt som ohälsosam. 

Om hello övervakning protokollet är TCP, initierar avsöknings hello Traffic Manager-agenten en TCP-anslutningsbegäran med hjälp av angivna hello-porten. Om hello-ändpunkten svarar toohello begäran med en svar tooestablish hello-anslutning att hälsokontroll har markerats som lyckas och avsöknings hello Traffic Manager-agenten återställer hello TCP-anslutning. Om hello svaret är ett annat värde, eller om inget svar tas emot inom anges tidsgränsen för hello hello Traffic Manager sökning agent försöker på nytt enligt toohello tolereras antalet fel inställningen (inget nytt försök görs om den här inställningen är 0). Om hello antal upprepade fel är högre än hello tolereras antalet fel inställningen, markeras den slutpunkten feltillstånd.

I samtliga fall Traffic Manager-avsökningar från flera platser och hello på varandra följande fel bestämning sker inom varje region. Det innebär också att slutpunkter får hälsoavsökningar från Traffic Manager med en frekvens som är högre än hello-inställningen som används för avsökning av intervallet.

>[!NOTE]
>För HTTP eller HTTPS övervakning är vanligt på hello endpoint sida tooimplement en anpassad sida i ditt program – till exempel /health.aspx. Du kan utföra programspecifika kontroller, till exempel kontrollerar prestandaräknare eller Verifiera databasens tillgänglighet med hjälp av den här sökvägen för övervakning. Hello sidan returnerar baserat på dessa anpassade kontroller, en lämplig HTTP-statuskod.

Alla slutpunkter i en Traffic Manager-profil dela inställningarna för övervakning. Om du behöver toouse olika inställningar för olika slutpunkter för övervakning, kan du skapa [kapslade Traffic Manager-profiler](traffic-manager-nested-profiles.md#example-5-per-endpoint-monitoring-settings).

## <a name="endpoint-and-profile-status"></a>Status för slutpunkt och profil

Du kan aktivera och inaktivera Traffic Manager-profiler och slutpunkter. Men inträffa en ändring i slutpunkt status även som ett resultat av Traffic Manager automatiserade inställningar och processer.

### <a name="endpoint-status"></a>Status för endpoint

Du kan aktivera eller inaktivera en viss slutpunkt. hello underliggande tjänst, vilket kan fortfarande vara felfria påverkas inte. Ändra hello endpoint status kontroller hello tillgängligheten för hello slutpunkt i hello Traffic Manager-profilen. När en slutpunkt status är inaktiverad Traffic Manager kontrollerar inte dess hälsa och hello slutpunkten ingår inte i ett DNS-svar.

### <a name="profile-status"></a>Profilstatusen

Använder hello profilinställningen status, kan du aktivera eller inaktivera en viss profil. När status för endpoint påverkar en enda slutpunkt, påverkar profilstatusen hello hela profilen, inklusive alla slutpunkter. När du inaktiverar en profil hello slutpunkter kontrolleras inte för hälsotillstånd och inga slutpunkter ingår i ett DNS-svar. En [NXDOMAIN](https://tools.ietf.org/html/rfc2308) svarskoden returneras för hello DNS-fråga.

### <a name="endpoint-monitor-status"></a>Övervaka status för endpoint

Övervaka status för endpoint är ett Traffic Manager-värde som visar hello status hello slutpunkt. Du kan inte ändra den här inställningen manuellt. övervaka status för hello endpoint är en kombination av hello resultaten av slutpunktsövervakning och hello konfigurerad slutpunkt status. hello möjliga värden för status för endpoint Övervakare visas i hello följande tabell:

| Profilstatusen | Status för endpoint | Övervaka status för endpoint | Anteckningar |
| --- | --- | --- | --- |
| Disabled |Enabled |Inaktiva |hello profilen har inaktiverats. Även om hello endpoint status är aktiverad företräde hello profilstatusen (inaktiverat). Slutpunkter i inaktiverad profiler övervakas inte. Ett NXDOMAIN svarskoden returneras för hello DNS-fråga. |
| &lt;alla&gt; |Disabled |Disabled |hello-slutpunkten har inaktiverats. Inaktiverad slutpunkter övervakas inte. hello slutpunkten ingår inte i DNS-svar, därför kan den tar emot inte trafik. |
| Enabled |Enabled |Online |hello endpoint övervakas och är felfri. Den ingår i DNS-svar och kan ta emot trafik. |
| Enabled |Enabled |Försämrad |Slutpunkten övervakning hälsokontroller misslyckas. hello slutpunkten ingår inte i DNS-svar och tar inte emot trafik. <br>Ett undantag toothis är om alla slutpunkter är försämrade då alla anses toobe returneras i hello frågesvar).</br>|
| Enabled |Enabled |CheckingEndpoint |hello endpoint övervakas, men hello resultaten av hello första sensorn har ännu inte tagits emot. CheckingEndpoint är ett tillfälligt tillstånd som vanligtvis sker omedelbart efter att lägga till eller aktivera en slutpunkt i hello profil. En slutpunkt i det här tillståndet ingår i DNS-svar och kan ta emot trafik. |
| Enabled |Enabled |Stoppad |hello cloud service eller web app som hello endpoint punkter toois körs inte. Kontrollera hello cloud service- eller web app. Detta kan också inträffa om hello-slutpunkten är i Skriv kapslade slutpunkt och hello underordnade profilen har inaktiverats eller är inaktiv. <br>En slutpunkt med status stoppad övervakas inte. Den ingår inte i DNS-svar och ta emot inte trafik. Ett undantag toothis är om alla slutpunkter är försämrade då alla beaktas toobe returneras i hello frågesvar.</br>|

Mer information om hur övervaka status för endpoint beräknas för kapslade slutpunkter finns [kapslade Traffic Manager-profiler](traffic-manager-nested-profiles.md).

### <a name="profile-monitor-status"></a>Övervaka profilstatusen

Hej profilstatusen övervakaren är en kombination av hello konfigurerad profil status och slutpunkten hello övervakaren statusvärden för alla slutpunkter. hello möjliga värden beskrivs i följande tabell hello:

| Profilstatusen (som har konfigurerats) | Övervaka status för endpoint | Övervaka profilstatusen | Anteckningar |
| --- | --- | --- | --- |
| Disabled |&lt;alla&gt; eller en profil med några definierade slutpunkter. |Disabled |hello profilen har inaktiverats. |
| Enabled |hello status för minst en slutpunkt har degraderats. |Försämrad |Granska hello enskilda endpoint status värden toodetermine vilka slutpunkter kräva ytterligare åtgärder. |
| Enabled |Hej är för minst en slutpunkt Online. Inga slutpunkter har statusen degraderad. |Online |hello tjänsten tar emot trafik. Ingen ytterligare åtgärd krävs. |
| Enabled |Hej är för minst en slutpunkt CheckingEndpoint. Inga slutpunkter är Online eller degraderad status. |CheckingEndpoints |Den här övergångstillstånd inträffar när en profil om skapade eller aktiverade. hello endpoint hälsa kontrolleras för hello första gången. |
| Enabled |hello status för alla slutpunkter i profilen för hello är inaktiverad eller stoppad eller hello profilen har inga definierade slutpunkter. |Inaktiva |Inga slutpunkter är aktiva men hello profil är fortfarande aktiverat. |

## <a name="endpoint-failover-and-recovery"></a>Slutpunkten redundans och återställning

Traffic Manager kontrollerar regelbundet hello hälsotillståndet för varje slutpunkt, inklusive ohälsosamt slutpunkter. Traffic Manager identifierar när en slutpunkt blir felfritt och ger dig tillbaka till rotation.

En slutpunkt är ohälsosamt när någon av hello följande händelser inträffar:
- Om hello övervakning är HTTP eller HTTPS:
    - Icke-200 svar (inklusive en annan 2xx-kod eller en 301/302-omdirigering).
- Om hello övervakning protokollet är TCP: 
    - Ett svar skickas än ACK eller SYN ACK tas emot i svaret toohello synkroniseringsbegäran av Traffic Manager tooattempt upprätta en anslutning.
- Timeout. 
- Alla andra anslutningsproblem ledde hello-slutpunkt som inte kan nås.

Mer information om felsökning av misslyckade kontroller finns [felsökning försämrad status på Azure Traffic Manager](traffic-manager-troubleshooting-degraded.md). 

hello följande tidslinjen i bild 2 är en detaljerad beskrivning av hello övervaka processen för Traffic Manager-slutpunkt som har hello följande inställningar: övervaka protokollet är HTTP, avsöknings är 30 sekunder, antalet tillåten fel är 3, timeout-värde är 10 sekunder och DNS TTL är 30 sekunder.

![Traffic Manager-slutpunkt redundans och återställning efter fel-sekvens](./media/traffic-manager-monitoring/timeline.png)

**Bild 2: Traffic manager-slutpunkten redundans och återställning sekvens**

1. **HÄMTA**. För varje slutpunkt utför hello Traffic Manager övervakningssystemet hello sökväg som anges i hello övervakningsinställningarna en GET-begäran.
2. **200 OK**. hello övervakningssystem förväntar sig en HTTP-200 OK meddelandet toobe returnerade inom 10 sekunder. När den tar emot svaret identifierar att hello-tjänsten är tillgänglig.
3. **30 sekunder mellan kontroller**. hello endpoint hälsokontroll upprepas var 30: e sekund.
4. **Tjänsten är inte tillgänglig**. hello tjänsten blir otillgänglig. Traffic Manager märker inte förrän hello nästa hälsokontroll.
5. **Försök tooaccess hello övervakning sökvägen**. hello övervakningssystem utför en GET-begäran, men inte tar emot ett svar inom tidsgränsen för hello 10 sekunder (du kan också ett icke-200 svar kan tas emot). Försöker den med tre gånger, med 30 sekunders intervall. Om en av hello försöker lyckas, återställs hello antal försök.
6. **Statusen tooDegraded**. Efter en fjärde på varandra följande fel markerar hello övervakningssystem hello tillgänglig slutpunkt status som degraderad.
7. **Trafiken är distribueras tooother slutpunkter**. hello Traffic Manager DNS-namnservrar uppdateras och Traffic Manager inte längre returnerar hello slutpunkt i svaret tooDNS frågor. Nya anslutningar är riktat tooother, tillgängliga slutpunkter. Tidigare DNS-svar som innehåller den här slutpunkten kan dock fortfarande cachelagras av rekursiva DNS-servrar och DNS-klienter. Klienter fortsätta toouse hello endpoint förrän hello DNS-cachen upphör att gälla. Eftersom hello DNS-cachen upphör att gälla klienter gör den nya DNS-frågor och är riktat toodifferent slutpunkter. hello cachelagringens varaktighet styrs av hello TTL-inställningen i hello Traffic Manager-profil, till exempel 30 sekunder.
8. **Hälsotillstånd kontrollerar fortsätta**. Traffic Manager fortsätter toocheck hello hälsotillstånd hello slutpunkt när den har statusen degraderad. Traffic Manager identifierar när hello endpoint returnerar toohealth.
9. **Tjänsten är online igen**. hello tjänsten blir tillgänglig. hello endpoint behåller statusen degraderad i Traffic Manager tills hello övervakningssystem utför dess nästa hälsokontroll.
10. **Fortsätter med att trafiken tooservice**. Traffic Manager skickar en GET-begäran och tar emot ett statussvar 200 OK. hello tjänsten returnerade tooa felfritt tillstånd. hello Traffic Manager-namnservrar uppdateras och de börjar toohand ut hello tjänsten DNS-namnet i DNS-svar. Trafik returnerar toohello slutpunkt som cachelagrade DNS-svar som returnerar andra slutpunkter upphör att gälla och som befintliga anslutningar tooother slutpunkter avslutas.

    > [!NOTE]
    > Eftersom Traffic Manager fungerar på hello DNS-nivån, kan det påverka befintliga anslutningar tooany slutpunkten. När den dirigerar trafik mellan slutpunkter (antingen av ändrade inställningar eller under växling vid fel eller återställning efter fel) leder Traffic Manager nya anslutningar tooavailable slutpunkter. Slutpunkter kan dock fortsätta tooreceive trafik via befintliga anslutningar förrän sessionerna avslutas. tooenable trafik toodrain från befintliga anslutningar, program bör begränsa hello varaktighet för sessionen som används med varje slutpunkt.

## <a name="traffic-routing-methods"></a>Routning av nätverkstrafik metoder

När en slutpunkt har status degraderad kan tillbaka den inte längre i svaret tooDNS frågor. I stället är en annan slutpunkt valt och returneras. hello routning av nätverkstrafik metoden som konfigurerats i hello profil avgör hur hello alternativa slutpunkten är valt.

* **Prioritet**. Slutpunkter utgör en prioriterad lista. hello första tillgängliga slutpunkten hello listan returneras alltid. Om statusen för slutpunkten är försämrad returneras hello nästa slutpunkt.
* **Viktade**. Alla tillgängliga endpoint väljs slumpmässigt baserat på deras tilldelade vikt och hello vikten av hello tillgängliga slutpunkter.
* **Prestanda**. hello endpoint närmaste toohello slutanvändaren returneras. Om den är inte tillgänglig, en slutpunkt väljs slumpmässigt från alla hello tillgängliga slutpunkter. Om du väljer en slumpmässig slutpunkt undviker ett sammanhängande fel som kan uppstå när hello nästa närmaste endpoint blir överbelastad. Du kan konfigurera alternativa redundans planer för routning av prestanda-trafik med hjälp av [kapslade Traffic Manager-profiler](traffic-manager-nested-profiles.md#example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region).
* **Geografisk**. hello endpoint mappas tooserve hello geografisk plats baserat på hello fråga IP-adresserna returneras. Om den är inte tillgänglig, en annan slutpunkt inte valda toofailover, eftersom en geografisk plats kan vara mappade endast tooone slutpunkt i en profil (Mer information finns i hello [vanliga frågor och svar](traffic-manager-FAQs.md#traffic-manager-geographic-traffic-routing-method)). Som bästa praxis, när du använder geografiska routning, rekommenderar vi kunder toouse kapslade Traffic Manager-profiler med mer än en slutpunkt som hello slutpunkter för hello-profilen.

Mer information finns i [Traffic Manager routning av nätverkstrafik metoder](traffic-manager-routing-methods.md).

> [!NOTE]
> Ett inträffar undantag toonormal routning av nätverkstrafik när alla berättigade slutpunkter har status degraderad. Traffic Manager gör ”bästa prestanda” försök och *som om alla hello degraderad status slutpunkterna är faktiskt i ett onlinetillstånd*. Det här beteendet är bättre toohello alternativ som är toonot returnera valfri slutpunkt i hello DNS-svar. Inaktiverad eller stoppad slutpunkter övervakas inte, därför kan de anses inte vara berättigad till trafik.
>
> Detta tillstånd orsakas vanligen av felaktig konfiguration av hello-tjänsten, som:
>
> * En ACL access control list [] blockerar hello Traffic Manager-hälsokontroller.
> * En felaktig konfiguration av hello övervakning port eller protokollet i hello Traffic manager-profil.
>
> hello följd av detta beteende är att om Traffic Manager-hälsokontroller inte är korrekt konfigurerad, kan det verka från hello trafik som routning Traffic Manager *är* fungerar korrekt. Men i det här fallet kan endpoint växling vid fel inträffa som påverkar övergripande tillgänglighet. Det är viktigt toocheck att hello profil visar statusen Online inte statusen degraderad. En Online-status anger att hello Traffic Manager hälsa kontrollerar fungerar som förväntat.

Mer information om hur du felsöker misslyckades hälsokontroller, se [felsökning försämrad status på Azure Traffic Manager](traffic-manager-troubleshooting-degraded.md).



## <a name="next-steps"></a>Nästa steg

Läs [hur Traffic Manager fungerar](traffic-manager-how-traffic-manager-works.md)

Mer information om hello [routning av nätverkstrafik metoder](traffic-manager-routing-methods.md) stöds av Traffic Manager

Lär dig hur för[skapa en Traffic Manager-profil](traffic-manager-manage-profiles.md)

[Felsöka status degraderad](traffic-manager-troubleshooting-degraded.md) på en Traffic Manager-slutpunkt
