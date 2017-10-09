---
title: aaaNested Traffic Manager-profiler | Microsoft Docs
description: "Den här artikeln förklarar hello kapslade profiler funktionen av Azure Traffic Manager"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: f1b112c4-a3b1-496e-90eb-41e235a49609
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: kumud
ms.openlocfilehash: e476d58a82ed94d12731156280c9665f980de0e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="nested-traffic-manager-profiles"></a>Kapslade Traffic Manager-profiler

Traffic Manager innehåller ett antal metoder för routning av nätverkstrafik som tillåter toocontrol hur Traffic Manager väljer vilka slutpunkten ska ta emot trafik från varje slutanvändare. Mer information finns i [Traffic Manager routning av nätverkstrafik metoder](traffic-manager-routing-methods.md).

Varje Traffic Manager-profil anger en enda metod för routning av nätverkstrafik. Det finns emellertid scenarier som kräver mer avancerad routning av nätverkstrafik än hello routning tillhandahålls som en enskild Traffic Manager-profil. Du kan kapsla Traffic Manager profiler toocombine hello fördelarna med fler än en metod för routning av nätverkstrafik. Kapslade profiler kan du toooverride hello standard Traffic Manager beteende toosupport större och mer komplexa programdistributioner.

hello som följande exempel visar hur toouse kapslade Traffic Manager-profiler i olika scenarier.

## <a name="example-1-combining-performance-and-weighted-traffic-routing"></a>Exempel 1: Kombinera 'Prestanda' och 'Viktat' routning av nätverkstrafik

Anta att du har distribuerat ett program i hello följande Azure-regioner: västra USA, västra Europa och Östasien. Du använder Traffic Manager-prestanda' routning av nätverkstrafik metoden toodistribute trafik toohello region närmaste toohello user.

![Enskild Traffic Manager-profilen][4]

Anta att du vill tootest en tooyour uppdateringstjänsten innan den distribueras mer brett. Vill du toouse hello 'viktat' routning av nätverkstrafik metoden toodirect en liten procentandel av trafiken tooyour Testa distributionen. Du ställa in hello testdistributionen tillsammans med hello befintliga produktionsdistributionen i västra Europa.

Du kan inte kombinera båda viktat och ' prestanda-routning av nätverkstrafik i en enda profil. toosupport i det här scenariot du skapar en Traffic Manager-profil som använder hello två Västeuropa slutpunkter och hello 'Viktat'-trafikroutning metod. Sedan lägger du till den här profilen 'child' som en slutpunkt toohello 'överordnad'-profil. hello överordnade profil fortfarande använder hello prestanda routning av nätverkstrafik metoden och innehåller hello andra globala distributioner som slutpunkter.

hello följande diagram illustrerar det här exemplet:

![Kapslade Traffic Manager-profiler][2]

I den här konfigurationen distribuerar trafik dirigeras via hello överordnade profil trafik över regioner normalt. Inom västra Europa distribuerar hello kapslad profil trafik toohello produktions- och slutpunkter enligt toohello viktningar som tilldelats.

När hello överordnade profil använder hello 'Prestanda' routning av nätverkstrafik metoden, måste varje slutpunkt tilldelas en plats. hello plats tilldelas när du konfigurerar hello slutpunkt. Välj hello Azure-region närmaste tooyour distribution. hello är Azure-regioner hello plats värden som stöds av hello Internet svarstid för tabellen. Mer information finns i [Traffic Manager-prestanda' routning av nätverkstrafik metoden](traffic-manager-routing-methods.md#performance).

## <a name="example-2-endpoint-monitoring-in-nested-profiles"></a>Exempel 2: Slutpunktsövervakning i kapslade profiler

Traffic Manager övervakar aktivt hello hälsotillståndet för varje tjänstslutpunkten. Om en slutpunkt är i feltillstånd, dirigerar Traffic Manager användare tooalternative slutpunkter toopreserve hello tillgängligheten för din tjänst. Den här slutpunkten övervakning och växling vid fel gäller tooall routning av nätverkstrafik metoder. Mer information finns i [Traffic Manager-slutpunkten övervakning](traffic-manager-monitoring.md). Slutpunktsövervakning fungerar på olika sätt för kapslade profiler. Med kapslade profiler utföra hello överordnade profil inte hälsokontroller på hello underordnade direkt. Hello hälsotillstånd hello underordnade profilen slutpunkter är i stället använda toocalculate hello övergripande hälsa för hello underordnade profilen. Den här hälsoinformation sprids in hello kapslad profil hierarki. hello överordnade profilen använder den här aggregerade hälsa toodetermine om toodirect trafikprofil toohello underordnade. Se hello [vanliga frågor och svar](traffic-manager-FAQs.md#traffic-manager-nested-profiles) mer information om hälsoövervakning av kapslade profiler.

Returnerar toohello föregående exempel anta hello Produktionsdistribution i västra Europa misslyckas. Som standard leder hello 'child' profil alla trafik toohello Testa distributionen. Om hello Testa distributionen också misslyckas hello överordnade profil som avgör att hello underordnade profilen inte bör ta emot trafik eftersom alla underordnade slutpunkter är felfria. Sedan distribuerar hello överordnade profil trafik toohello andra regioner.

![Kapslad profil med växling vid fel (standardinställning)][3]

Du kanske nöjd med den här ordningen. Eller kanske berörda att all trafik för Västeuropa nu kommer toohello testdistributionen i stället för en begränsad delmängd trafik. Testa distributionen oavsett hello hälsotillstånd hello, vill toofail över toohello andra regioner när hello Produktionsdistribution i västra Europa misslyckas. tooenable redundans, du kan ange hello 'MinChildEndpoints' parameter när du konfigurerar hello underordnade profilen som en slutpunkt i hello överordnade profilen. hello parametern avgör hello minsta antalet tillgängliga slutpunkterna i hello underordnade profilen. hello standardvärdet är '1'. I det här scenariot kan du ange hello MinChildEndpoints värdet too2. Under tröskelvärdet hello överordnade profil anser hello hela underordnade profilen toobe otillgänglig och dirigerar trafik toohello slutpunkter.

hello visar följande bild den här konfigurationen:

![Kapslad profil redundans med 'MinChildEndpoints' = 2][4]

> [!NOTE]
> hello 'Priority' routning av nätverkstrafik metod distribuerar alla trafik tooa enda slutpunkt. Därför är det lite syfte i inställningen MinChildEndpoints än '1' för en underordnad profil.

## <a name="example-3-prioritized-failover-regions-in-performance-traffic-routing"></a>Exempel 3: Prioriteras redundans regioner i 'prestanda-trafikroutning

hello standardbeteendet för hello 'Prestanda' routning av nätverkstrafik metoden är utformad tooavoid över inläsning bredvid hello närmast slutpunkt och orsakar sammanhängande flera fel. När en slutpunkt inte fördelade all trafik som skulle ha dirigerats toothat slutpunkten är jämnt toohello slutpunkter på alla regioner.

!['Prestanda-trafik routning med standard-redundans][5]

Men du kanske föredrar hello Västeuropa trafik redundans tooWest oss, och endast dirigera trafik tooother regioner när båda slutpunkterna är inte tillgänglig. Du kan skapa den här lösningen med hjälp av en underordnad profil med hello 'Priority' routning av nätverkstrafik metod.

!['Prestanda-trafik routning med förmånliga redundans][6]

Eftersom hello Västeuropa slutpunkt har högre prioritet än hello västra USA slutpunkt, all trafik skickas toohello Västeuropa slutpunkt när båda slutpunkterna är online. Om det inte går att Västeuropa, är dess trafik dirigerad tooWest USA. Med hello kapslad profil är trafik dirigerad tooEast Asien endast när misslyckas både västra Europa och USA, västra.

Du kan upprepa det här mönstret för alla regioner. Ersätt alla tre slutpunkter i profilen för hello överordnade med tre underordnade profiler, var och en prioriterad failover-sekvens.

## <a name="example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-hello-same-region"></a>Exempel 4: Kontrollera 'prestanda-trafik routning mellan flera slutpunkter i hello samma region

Anta att hello 'Prestanda' routning av nätverkstrafik metoden används i en profil som har mer än en slutpunkt i en viss region. Som standard dirigeras trafik toothat region fördelas jämnt över alla tillgängliga slutpunkterna i den regionen.

!['Prestanda-trafik routning i region trafikfördelning (standardinställning)][7]

I stället för att lägga till flera slutpunkter i Västeuropa innesluts dessa slutpunkter i en separat underordnade profil. hello underordnade profilen läggs toohello överordnade som hello endast slutpunkt i västra Europa. hello-inställningar på hello underordnade profilen kan styra hello trafikfördelning med Västeuropa genom att aktivera prioritetsbaserad eller viktat trafikroutning i detta område.

!['Prestanda-trafik routning med anpassade trafikfördelning i region][8]

## <a name="example-5-per-endpoint-monitoring-settings"></a>Exempel 5: Per slutpunkt övervakningsinställningarna

Anta att du använder migrera Traffic Manager toosmoothly trafik från en äldre lokalt webbplats tooa nya molnbaserade versionen finns i Azure. Hälsa för toouse hello startsidan URI toomonitor platsen vill hello äldre plats du. Men för hello nya molnbaserade versionen, du implementerar en anpassad övervakning sida (sökvägen ' / monitor.aspx') som innehåller ytterligare kontroller.

![Traffic Manager-slutpunkt övervakning (standardinställning)][9]

hello övervakningsinställningarna i Traffic Manager-profilen gäller tooall slutpunkter inom en enskild profil. Med kapslade profiler, kan du använda en annan underordnad profil per plats toodefine olika inställningar för övervakning.

![Övervakning med per slutpunktsinställningar Traffic Manager-slutpunkt][10]

## <a name="next-steps"></a>Nästa steg

Lär dig mer om [Traffic Manager-profiler](traffic-manager-overview.md)

Lär dig hur för[skapa en Traffic Manager-profil](traffic-manager-create-profile.md)

<!--Image references-->
[1]: ./media/traffic-manager-nested-profiles/figure-1.png
[2]: ./media/traffic-manager-nested-profiles/figure-2.png
[3]: ./media/traffic-manager-nested-profiles/figure-3.png
[4]: ./media/traffic-manager-nested-profiles/figure-4.png
[5]: ./media/traffic-manager-nested-profiles/figure-5.png
[6]: ./media/traffic-manager-nested-profiles/figure-6.png
[7]: ./media/traffic-manager-nested-profiles/figure-7.png
[8]: ./media/traffic-manager-nested-profiles/figure-8.png
[9]: ./media/traffic-manager-nested-profiles/figure-9.png
[10]: ./media/traffic-manager-nested-profiles/figure-10.png
