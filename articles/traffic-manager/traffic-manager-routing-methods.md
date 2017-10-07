---
title: aaaAzure Traffic Manager - trafikroutningsmetoder | Microsoft Docs
description: "Det här artiklarna hjälper till att du förstår hello olika trafikroutningsmetoder används av Traffic Manager"
services: traffic-manager
documentationcenter: 
author: KumudD
manager: timlt
editor: 
ms.assetid: db1efbf6-6762-4c7a-ac99-675d4eeb54d0
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/13/2017
ms.author: kumud
ms.openlocfilehash: b3eeca63ab5f2b9cd4a3a6b6a8fd3e40059e32b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="traffic-manager-routing-methods"></a>Traffic Manager-dirigeringsmetoder

Azure Traffic Manager stöder fyra routning av nätverkstrafik metoder toodetermine hur nätverk tooroute trafik toohello olika slutpunkter. Traffic Manager gäller hello routning av nätverkstrafik metoden tooeach DNS-fråga tas emot. hello routning av nätverkstrafik metoden anger vilken slutpunkt returneras i hello DNS-svar.

Det finns fyra trafikroutningsmetoder i Traffic Manager:

* **[Prioritet](#priority):** Välj **prioritet** när du vill toouse primära tjänstslutpunkten för all trafik och ange säkerhetskopieringar om hello primära eller hello säkerhetskopiering slutpunkter inte är tillgängliga.
* **[Viktat](#weighted):** Välj **viktat** när du vill toodistribute trafik över en uppsättning slutpunkter, antingen jämnt eller bl.a tooweights som du definierar.
* **[Prestanda](#performance):** Välj **prestanda** när du har slutpunkter i olika geografiska platser och du vill att slutanvändarna toouse hello ”närmaste” endpoint vad gäller hello lägsta Nätverksfördröjningen.
* **[Geografisk](#geographic):** Välj **geografiska** så att användare dirigerad toospecific slutpunkter (Azure, externt eller kapslade) baserat på vilka geografiska plats deras DNS-fråga kommer från. Detta ger Traffic Manager kunder tooenable scenarier där känna till en användares geografisk region och routning dem baserat på som är viktigt. Exempel: uppfyller data suveränitet uppdrag, lokalisering av användaren & innehåll och mäta trafik från olika regioner.

Alla Traffic Manager-profiler innehåller övervakning av hälsotillstånd för slutpunkten och automatisk endpoint växling vid fel. Mer information finns i [Traffic Manager-slutpunkten övervakning](traffic-manager-monitoring.md). En enskild Traffic Manager-profil kan använda en enda trafikroutningsmetod. Du kan välja en annan trafikroutningsmetod för din profil när som helst. Ändringarna tillämpas inom en minut och utan avbrott uppstår. Routning av nätverkstrafik metoder kan kombineras med hjälp av kapslade Traffic Manager-profiler. Kapsling gör avancerade och flexibel routning av nätverkstrafik konfigurationer som uppfyller behoven för hello av större, komplexa program. Mer information finns i [kapslade Traffic Manager-profiler](traffic-manager-nested-profiles.md).

## <a name = "priority"></a>Prioritet routning av nätverkstrafik metod

Organisationen vill ofta tooprovide tillförlitlighet för dess tjänster genom att distribuera en eller flera säkerhetskopierade tjänster om deras primära tjänsten kraschar. hello 'Priority'-trafikroutning metoden kan Azure kunder tooeasily implementerar det här mönstret för växling vid fel.

![Azure Traffic Manager 'Priority' routning av nätverkstrafik, metod][1]

Hej trafikhanterarprofil innehåller en prioriterad lista med slutpunkter. Som standard skickar alla trafik toohello primära (högsta prioritet) slutpunkt i Traffic Manager. Om hello primära slutpunkt inte är tillgänglig, hello Traffic Manager vägar trafik toohello andra slutpunkten. Om båda hello primära och sekundära slutpunkter inte är tillgängliga går hello trafik toohello tredje, och så vidare. Tillgänglighet för hello slutpunkten är baserat på hello konfigurerats status (aktiverade eller inaktiverade) och hello pågående slutpunktsövervakning.

### <a name="configuring-endpoints"></a>Konfigurera slutpunkter

Med Azure Resource Manager kan du konfigurera hello slutpunktsprioritet uttryckligen med hjälp av hello 'priority'-egenskapen för varje slutpunkt. Den här egenskapen är ett värde mellan 1 och 1000. Lägre värden motsvarar högre prioritet. Slutpunkter kan inte dela prioritetsvärdet. Hello-egenskapen är valfri. När utelämnas används en standardprioritet utifrån hello endpoint ordning.

##<a name = "weighted"></a>Viktade routning av nätverkstrafik, metod
hello 'Viktat'-trafikroutning metod kan du toodistribute trafiken jämnt eller toouse en fördefinierad vikt.

![Azure Traffic Manager-viktad' routning av nätverkstrafik metod][2]

Hello viktat routning av nätverkstrafik metoden kan du tilldela en vikt tooeach slutpunkt i konfigurationen för hello Traffic Manager-profilen. hello vikt är ett heltal från 1 too1000. Den här parametern är valfri. Om det utelämnas används trafik chefer en standard vikt '1'.

För varje DNS-fråga tas emot, väljer slumpmässigt en tillgänglig slutpunkt i Traffic Manager. hello sannolikheten för att välja en slutpunkt som baseras på hello viktningar som tilldelats tooall tillgängliga slutpunkter. Med hjälp av hello vikt samma över alla slutpunkter resultat i en trafiken fördelas jämnt. Med högre eller lägre vikterna på specifika slutpunkter gör dessa slutpunkter toobe returnerade mer eller mindre ofta i hello DNS-svar.

hello viktas möjliggör vissa användbara scenarier:

* Stegvis Programuppgradering: tilldela en procentandel av trafiken tooroute tooa ny slutpunkt och gradvis öka hello-trafik över tid too100%.
* Programmet migrering tooAzure: skapa en profil med både Azure och externa slutpunkter. Justera hello vikt hello slutpunkter tooprefer hello nya slutpunkter.
* Moln-burst-överföring för ytterligare kapacitet: snabbt expandera en lokal distribution i hello moln genom att lägga till bakom en Traffic Manager-profilen. När du behöver extra kapacitet i hello moln du lägga till eller aktivera flera slutpunkter och anger vilken del av trafiken går tooeach slutpunkt.

hello Azure Resource Manager-portalen stöder hello konfiguration av Routning av viktat nätverkstrafik.  Du kan konfigurera vikterna med hello Resource Manager-versioner av Azure PowerShell, CLI och hello REST API: er.

Det är viktigt toounderstand att DNS-svar cachelagras av klienter och av hello rekursiv DNS-servrar som hello klienter använder tooresolve DNS-namn. Den här cachelagring kan påverka viktat trafik distributioner. När hello antalet klienter och rekursiva DNS-servrar är stort, fungerar trafikfördelning som förväntat. Men när hello antalet klienter eller rekursiv DNS-servrar är litet kan cachelagring avsevärt skeva hello trafikfördelning.

Vanliga användningsområden omfattar:

* Utvecklings- och testmiljöer
* Program kommunikation
* Program syftar till att en smala-användarbas som delar en gemensam rekursiv DNS-infrastruktur (till exempel anställda på företaget som ansluter via en proxyserver)

Dessa DNS-cachelagring effekter är vanliga tooall DNS-baserad trafik system, inte bara Azure Traffic Manager. I vissa fall kan ger uttryckligen rensar hello DNS-cache en lösning. I andra fall måste vara en alternativ metod för routning av nätverkstrafik lämpligare.

## <a name = "performance"></a>Prestanda routning av nätverkstrafik metod

Distribuera slutpunkter i två eller flera platser över hela världen hello kan förbättra hello svarstider för många program av Routning trafik toohello plats som är 'närmaste' tooyou. hello-prestanda' routning av nätverkstrafik metoden ger den här funktionen.

![Azure Traffic Manager-prestanda' routning av nätverkstrafik, metod][3]

hello 'närmaste' slutpunkt är inte nödvändigtvis närmaste mätt av geografiska avstånd. I stället anger hello 'Prestanda' routning av nätverkstrafik metoden hello närmaste slutpunkten genom att mäta Nätverksfördröjningen. Traffic Manager upprätthåller ett Internet latens tabell tootrack hello fram och åter tiden mellan IP-adressintervall och varje Azure-datacenter.

Traffic Manager letar upp hello källans IP-adress för hello inkommande DNS-begäran i hello Internet svarstid för tabellen. Traffic Manager väljer en tillgänglig slutpunkt i hello Azure-datacenter som har hello lägsta fördröjningen för IP-adressintervall, och returnerar sedan denna slutpunkt i hello DNS-svar.

Enligt beskrivningen i [så Traffic Manager fungerar](traffic-manager-overview.md#how-traffic-manager-works), Traffic Manager får inte DNS-frågor direkt från klienter. DNS-frågor kommer i stället från att hello klienter är konfigurerade toouse hello rekursiv DNS-tjänsten. Därför hello IP-adress som används för toodetermine hello 'närmaste' slutpunkt är inte hello klientens IP-adress, men det är hello IP-adressen för hello rekursiv DNS-tjänsten. I praktiken är IP-adressen en bra proxy för hello-klienten.


Traffic Manager regelbundet uppdateringar hello Internet latens tabell tooaccount efter ändringar i hello globala Internet och nya Azure-regioner. Programprestanda varierar, beroende på realtid variationer i belastningen över hello Internet. Routning av prestanda-trafik övervakar inte belastningen på en viss tjänstslutpunkt. Men om en slutpunkt blir otillgänglig, ingår Traffic Manager inte i DNS-svar på frågor.


Punkter toonote:

* Om din profil innehåller flera slutpunkter i hello samma Azure-region, och sedan Traffic Manager fördelar trafiken jämnt över hello tillgängliga slutpunkterna i den regionen. Om du föredrar en annan trafikfördelning inom en region, kan du använda [kapslade Traffic Manager-profiler](traffic-manager-nested-profiles.md).
* Om alla aktiverade slutpunkter i hello närmaste Azure-regionen är försämrad, Traffic Manager flyttar trafik toohello slutpunkter i hello nästa närmaste Azure-region. Om du vill toodefine en prioriterad failover-sekvens, Använd [kapslade Traffic Manager-profiler](traffic-manager-nested-profiles.md).
* När du använder hello prestanda trafikroutningsmetod med externa slutpunkter eller kapslade slutpunkter, måste toospecify hello platsen för dessa slutpunkter. Välj hello Azure-region närmaste tooyour distribution. Dessa platser är hello värden som stöds av hello Internet svarstid för tabellen.
* hello-algoritmen som väljer hello slutpunkten är deterministisk. Upprepad DNS-frågor från hello samma klient är dirigeras toohello samma slutpunkt. Klienter använder normalt olika rekursiv DNS-servrar när du reser. hello klienten kanske routade tooa annan endpoint. Routning kan också påverkas av uppdateringar toohello Internet svarstid för tabellen. Därför hello prestanda routning av nätverkstrafik metoden inte garanterar att en klient är alltid dirigeras toohello samma slutpunkt.
* När hello Internet latens tabellen ändras, kan du se att vissa klienter finns dirigerad tooa annan endpoint. Ändringen routning är exaktare baserat på aktuella svarstidsdata. De här uppdateringarna är nödvändiga toomaintain hello riktighet prestanda-routning av nätverkstrafik som hello Internet ständigt utvecklas.

## <a name = "geographic"></a>Geografisk routning av nätverkstrafik metod

Traffic Manager-profiler kan vara konfigurerade toouse hello geografiska routning metoden så att användare dirigeras toospecific slutpunkter (Azure, externt eller kapslade) baserat på vilka geografiska plats deras DNS-fråga som kommer från. Detta ger Traffic Manager kunder tooenable scenarier där känna till en användares geografisk region och routning dem baserat på som är viktigt. Exempel: uppfyller data suveränitet uppdrag, lokalisering av användaren & innehåll och mäta trafik från olika regioner.
När en profil har konfigurerats för geografisk routning varje slutpunkt som är kopplade till att profilen måste toohave en uppsättning geografiska områden som tilldelats tooit. Ett geografiskt område kan vara på följande detaljnivåer 
- World – en region
- Regional gruppering – till exempel Afrika, Mellanöstern, Australien/Pacific osv. 
- Land/Region – till exempel Irland Peru, Hongkong SAR osv. 
- Region – till exempel USA California, Australien-Queensland Kanada Alberta etc. (Obs: den här filnivå stöds bara för tillstånd / regioner i Australien, Kanada, Storbritannien och USA).

När en region eller en uppsättning regioner tilldelas tooan slutpunkt, hämtar förfrågningar från dessa regioner routade endast toothat slutpunkt. Traffic Manager använder hello källans IP-adress för hello DNS-fråga toodetermine hello region där en användare efterfrågar från – vanligtvis är hello IP-adressen för hello lokala DNS-matchning gör hello frågan hello användarens räkning.  

![Azure Traffic Manager-geografiska' routning av nätverkstrafik metod](./media/traffic-manager-routing-methods/geographic.png)

Traffic Manager läser hello källans IP-adress för hello DNS-fråga och bestämmer vilka geografiska region som det härstammar från. Den söker sedan toosee om det finns en slutpunkt som har den här geografiska region som mappats tooit. Den här sökningen börjar vid hello lägsta filnivå (region där den stöds, annars hello Land/Region nivån) och går alla hello sätt in toohello högsta nivån, som är **World**. hello matchning första med hjälp av den här traversal utses hello endpoint tooreturn i hello frågesvar. När matchning med en slutpunkt för kapslad typ, en slutpunkt inom den underordnade profilen returneras, baserat på dess routningsmetoden. följande punkter hello är tillämpliga toothis beteende:

- Ett geografiskt område kan vara mappade endast tooone slutpunkt i Traffic Manager-profil när hello routning är geografiska routning. Detta säkerställer att routning av användare är deterministisk och kunder kan aktivera scenarier som kräver entydigt geografisk gränser.
- Om en användares region kommer under två olika slutpunkter geografiska mappning, väljer hello slutpunkt med hello lägsta granularitet Traffic Manager och tar inte hänsyn routning begäranden från den region toohello andra slutpunkten. Anta till exempel att en geografisk routning typen profil med två slutpunkter - slutpunkt 1 och Endpoint2. Slutpunkt 1 konfigureras tooreceive trafik från Irland och Endpoint2 konfigureras tooreceive trafik från Europa. Om en begäran kommer från Irland, men det är alltid dirigeras tooEndpoint1.
- Eftersom en region kan vara mappade endast tooone slutpunkt, returneras den Traffic Manager oavsett hello slutpunkten är felfri eller inte.

    >[!IMPORTANT]
    >Du rekommenderas att kunder som använder hello geografiska routningsmetoden associeras med hello kapslade typen slutpunkter som har underordnade profiler som innehåller minst två slutpunkter inom varje.
- Om en matchning slutpunkt och som slutpunkten har hello **stoppad** tillstånd Traffic Manager returnerar svaret inga data. I det här fallet inga ytterligare sökningar görs högre upp i hierarkin för hello geografiska region. Detta gäller även för kapslade slutpunktstyper när hello underordnade profil i hello **stoppad** eller **inaktiverad** tillstånd.
- Om en slutpunkt visar en **inaktiverad** status, inte tas med i hello region matchning process. Detta gäller även för kapslade slutpunktstyper när hello-slutpunkten har hello **inaktiverad** tillstånd.
- Om en fråga kommer från en geografisk region som inte har någon mappning i den här profilen, returnerar Traffic Manager svaret inga data. Därför rekommenderar vi att kunder använder geografiska routning med en slutpunkt som helst av typen kapslade med minst två slutpunkter inom hello underordnade profilen med hello region **World** tilldelade tooit. Detta säkerställer att alla IP-adresser som inte mappas tooa region hanteras.

Enligt beskrivningen i [så Traffic Manager fungerar](traffic-manager-how-traffic-manager-works.md), Traffic Manager får inte DNS-frågor direkt från klienter. DNS-frågor kommer i stället från att hello klienter är konfigurerade toouse hello rekursiv DNS-tjänsten. Hello IP-adress som används för toodetermine hello regionen är därför inte hello klientens IP-adress, men det är hello IP-adressen för hello rekursiv DNS-tjänsten. I praktiken är IP-adressen en bra proxy för hello-klienten.


## <a name="next-steps"></a>Nästa steg

Lär dig hur toodevelop program med hög tillgänglighet med hjälp av [slutpunktsövervakning för Traffic Manager](traffic-manager-monitoring.md)

Lär dig hur för[skapa en Traffic Manager-profil](traffic-manager-create-profile.md)

<!--Image references-->
[1]: ./media/traffic-manager-routing-methods/priority.png
[2]: ./media/traffic-manager-routing-methods/weighted.png
[3]: ./media/traffic-manager-routing-methods/performance.png



