---
title: "aaaBest praxis för Autoskala | Microsoft Docs"
description: "Autoskala mönster i Azure för Web Apps, skaluppsättningar för den virtuella datorn och molntjänster"
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 9fa2b94b-dfa5-4106-96ff-74fd1fba4657
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: ancav
ms.openlocfilehash: eb731c15e440af93a2675210583878814d0d8818
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-autoscale"></a>Bästa metoder för autoskalning
Den här artikeln lär du den bästa praxis tooautoscale i Azure. Azure övervakaren Autoskala gäller endast för[Skalningsuppsättningar i virtuella](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [molntjänster](https://azure.microsoft.com/services/cloud-services/), och [App Service - Webbappar](https://azure.microsoft.com/services/app-service/web/). Andra Azure-tjänster använda olika metoder för skalning.

## <a name="autoscale-concepts"></a>Autoskala begrepp
* En resurs kan bara ha *en* autoskalningsinställning
* En autoskalningsinställning kan ha en eller flera profiler och varje profil kan ha en eller flera Autoskala regler.
* En autoskalningsinställning skalas instanser vågrätt, vilket är *ut* genom att öka hello instanser och *i* genom att minska hello antal instanser.
  En autoskalningsinställning har en högsta och lägsta standardvärdet instanser.
* Ett jobb Autoskala alltid hello associerade mått tooscale av kontrollerar om den har passerat hello konfigurerade tröskeln för skalbar eller skala i. Du kan visa en lista av mätvärden som autoskalning kan skala genom på [Azure-Monitor autoskalning vanliga mått](insights-autoscale-common-metrics.md).
* Alla tröskelvärden beräknas på en instansnivå. Till exempel ”skala ut med 1 instans när Genomsnittlig CPU > 80% när instansantalet är 2”, innebär att skalbar när hello Genomsnittlig CPU i alla instanser som är större än 80%.
* Du får alltid meddelanden via e-post. Mer specifikt får hello ägare, deltagare och läsare av hello målresurs e-post. Du får alltid en *recovery* e-post när Autoskala återställer från ett fel och startar fungerar normalt.
* Du kan anmäla sig tooreceive en lyckad skala åtgärd avisering via e-post och webhooks.

## <a name="autoscale-best-practices"></a>Autoskala bästa praxis
Använd följande metodtips när du använder Autoskala hello.

### <a name="ensure-hello-maximum-and-minimum-values-are-different-and-have-an-adequate-margin-between-them"></a>Kontrollera hello högsta och lägsta värdena är olika och har en tillräcklig marginal mellan dem.
Om du har en inställning som har minst = 2, maximalt = 2 och hello aktuella standardinstansantalet är 2, ingen skalningsåtgärd kan uppstå. Behåll en tillräcklig marginal mellan hello högsta och lägsta antalet instanser, vilket är inklusiva. Autoskala skalas alltid mellan dessa gränser.

### <a name="manual-scaling-is-reset-by-autoscale-min-and-max"></a>Manuell skalning återställs av Autoskala min och max
Om du manuellt uppdatera hello instans tooa värdet för antal ovan eller nedan hello maximalt hello Autoskala motorn automatiskt skalas tillbaka toohello minsta (om nedan) eller hello maximala (om ovan). Exempelvis kan du ange hello intervallet mellan 3 och 6. Om du har en instans som körs kan skalas hello Autoskala motorn too3 instanser på dess nästa körning. På samma sätt är det skulle skala in 8 instanser tillbaka too6 nästa körs.  Manuell skalning är mycket tillfällig om du återställer hello Autoskala samt regler.

### <a name="always-use-a-scale-out-and-scale-in-rule-combination-that-performs-an-increase-and-decrease"></a>Använd alltid en skalbar och skala i regel kombination som utför en ökning och minskning
Om du använder endast en del av hello kombination har Autoskala skala in att enkel, eller i en, tills hello högsta eller lägsta, nåtts.

### <a name="do-not-switch-between-hello-azure-portal-and-hello-azure-classic-portal-when-managing-autoscale"></a>Växla inte mellan hello Azure-portalen och hello klassiska Azure-portalen när du hanterar Autoskala
Använd hello Azure-portalen (portal.azure.com) toocreate och hantera Autoskala inställningar för molntjänster och Apptjänster (webbprogram). Använd PowerShell, CLI eller REST API toocreate för Skalningsuppsättningar i virtuella datorer och hantera autoskalningsinställning. Växla inte mellan hello klassiska Azure-portalen (manage.windowsazure.com) och hello Azure-portalen (portal.azure.com) när du hanterar Autoskala konfigurationer. Hej klassiska Azure-portalen och dess underliggande backend har begränsningar. Flytta toohello Azure portal toomanage Autoskala med ett grafiskt användargränssnitt. hello alternativ är toouse hello Autoskala PowerShell, CLI eller REST API (via resursutforskaren i Azure).

### <a name="choose-hello-appropriate-statistic-for-your-diagnostics-metric"></a>Välj lämplig hello-statistiken för diagnostik-mått
För diagnostik statistik, kan du välja mellan *genomsnittlig*, *minsta*, *maximala* och *totala* som ett mått tooscale av. hello vanligaste statistiken är *genomsnittlig*.

### <a name="choose-hello-thresholds-carefully-for-all-metric-types"></a>Välj hello tröskelvärden noggrant för alla typer av mått
Vi rekommenderar att du noggrant välja olika tröskelvärden för skalbar och skala in baserat på praktiska situationer.

Vi *rekommenderar inte* Autoskala inställningar som hello exemplen nedan med hello samma eller mycket lik tröskelvärden för ut och villkor:

* Öka instanser av 1 räkna när antal trådar < = 600
* Minska instanser med 1 räkna när antal trådar > = 600

Nu ska vi titta på ett exempel på vad kan leda tooa beteende som kan verka förvirrande. Överväg att hello följande sekvens.

1. Anta att det finns 2 instanser toobegin med och sedan hello Genomsnittligt antal trådar per instans växer too625.
2. Autoskala skalas ut lägger till en 3-instans.
3. Därefter förutsätter att hello genomsnittlig trådar över instans hamnar too575.
4. Innan du skala försöker Autoskala tooestimate vilka hello sluttillstånd kommer att om den anpassas i. Till exempel 575 x 3 (aktuella standardinstansantalet) = 1,725 / 2 (sista antal instanser när skala) = 862.5 trådar. Detta innebär att Autoskala skulle ha tooimmediately skalbar igen trots att det skalas, om hello genomsnittlig tråd antal förblir hello samma eller även hamnar bara en liten del. Men om det som skalats ut igen, hello hela processen skulle upprepa inledande tooan oändlig loop.
5. tooavoid i den här situationen (som kallas ”flapping”) Autoskala skalas inte alls. I stället hoppar över och reevaluates hello villkoret igen hello nästa gång hello tjänstens jobbet körs. Detta kan förvirrande många eftersom Autoskala inte visas då toowork när hello genomsnittlig trådar 575.

Beräkning av under en skala i är avsedda tooavoid ”växlar” situationer där skala i och skalbar åtgärder kontinuerligt gå fram och tillbaka. Kom ihåg detta beteende när du väljer hello samma tröskelvärden för skalbar och i.

Vi rekommenderar att du väljer en tillräcklig marginal mellan hello skalbar och tröskelvärden. Exempelvis bör du hello efter bättre regeln kombination.

* Öka instanser av 1 räkna när CPU i % > = 80
* Minska instanser med 1 räkna när CPU i % < = 60

I det här fallet  

1. Anta att det finns 2 instanser toostart med.
2. Om hello Genomsnittlig CPU i % över instanser går too80 kan skalas Autoskala lägga till en tredje instans.
3. Anta nu att % hamnar too60 över tid hello CPU.
4. Autoskalas skala i regeln beräknar hello sluttillstånd om det vore tooscale i. Till exempel 60 x 3 (aktuella standardinstansantalet) = 180 / 2 (sista antal instanser när skala) = 90. Så Autoskala inte skala in eftersom den skulle ha tooscale ut igen omedelbart. I stället hoppas över och skala.
5. hello nästa gång Autoskala kontrollerar hello CPU fortsätter toofall too50. Beräknar igen - 50 x 3, instans = 150 / 2 instanser = 75, vilket är hello skalbar tröskelvärdet 80, så att det skalas in har too2 instanser.

### <a name="considerations-for-scaling-threshold-values-for-special-metrics"></a>Överväganden för skalning tröskelvärden för särskilda mått
 För särskilda mätvärden, till exempel lagrings- och Service Bus-kö längd mått är hello tröskelvärdet hello Genomsnittligt antal meddelanden som är tillgängliga per aktuellt antal instanser. Välj noga hello väljer hello tröskelvärde för det här måttet.

Vi illustrerar den med ett exempel tooensure du hur hello fungerar bättre.

* Öka instanser av 1 antal när lagring kön meddelandet antal > = 50
* Minska instanser av 1 antal när lagring kön meddelandet antal < = 10

Överväg följande sekvens hello:

1. Det finns 2 lagring kön instanser.
2. Meddelanden fortfarande och när du granskar hello storage-kö hello totala antalet läser 50. Du kan anta att Autoskala ska starta en skalbar-åtgärd. Observera dock att det fortfarande är 50/2 = 25 meddelanden per instans. Därför uppstår inte skalbar. För hello första skalbar toohappen vara hello totala antalet meddelanden i kö för lagring av hello 100.
3. Därefter förutsätter att hello totala meddelandemängd når 100.
4. En 3 lagring kön instans läggs till på grund av tooa skalbar åtgärd.  hello nästa skalbar åtgärd sker inte förrän hello totala antalet meddelanden i kö för hello når 150 eftersom 150/3 = 50.
5. Nu blir hello antal meddelanden i kö för hello mindre. Med 3 instanser hello första skala i åtgärden händer när hello Totalt antal meddelanden i alla köer summera too30 eftersom 30/3 = 10 meddelanden per instans som är hello skala i tröskelvärdet.

### <a name="considerations-for-scaling-when-multiple-profiles-are-configured-in-an-autoscale-setting"></a>Överväganden för skalning när flera profiler har konfigurerats i en autoskalningsinställning
Du kan välja en standardprofil som används alltid utan några beroende av schemat eller tid, i en autoskalningsinställning eller kan du välja en återkommande profil eller för en viss period med datum och tid.

När Autoskala tjänsten bearbetar dem, kontrollerar den alltid i hello följande ordning:

1. Fast datum-profil
2. Återkommande profil
3. Standardprofil (”Always”)

Om en profil är uppfyllt, kontrollerar inte Autoskala hello nästa profil villkor under den. Autoskala bearbetar endast en profil i taget. Detta betyder att om du vill tooalso innehåller ett bearbetningsvillkor från en lägre nivå profil, måste du också inkludera regler i hello aktuella profilen.

Nu ska vi se detta med hjälp av ett exempel:

hello bilden nedan visar ett autoskalningsinställning med en standardprofil minsta instanser = 2 och högsta förekomster = 10. I det här exemplet är regler konfigurerade tooscale ut när hello meddelandet i kön för hello är större än 10 och skala i när antalet för hello-meddelanden i kö för hello är mindre än 3. Hello resurs kan nu skala mellan 2 och 10 instanser.

Dessutom är en återkommande profil som angetts för måndag. Den värdet för minsta instanser = 2 och högsta förekomster = 12. Detta innebär måndag, hello första gången Autoskala söker efter det här villkoret om hello instansantalet 2 skalningen toohello nya minst 3. Så länge Autoskala kvarstår toofind felet profil matchade (måndag) bearbetas bara hello processorbaserad scale-out/i reglerna som konfigurerats för den här profilen. Just nu är kontrollerar den inte om hello kölängd. Men om du även vill hello kön längd villkoret toobe markerad bör du ta regler från hello standardprofil samt i profilen för måndag.

På liknande sätt när Autoskala växlar tillbaka toohello standardprofil, kontrollerar först den om hello lägsta och högsta villkor är uppfyllda. Om hello antalet instanser hello för närvarande är 12, växer den i too10, hello maximalt tillåtet för hello standardprofil.

![Autoskala inställningar](./media/insights-autoscale-best-practices/insights-autoscale-best-practices-2.png)

### <a name="considerations-for-scaling-when-multiple-rules-are-configured-in-a-profile"></a>Överväganden för skalning när flera regler är konfigurerade i en profil
Finns det fall där du kan ha tooset flera regler i en profil. hello följande uppsättning Autoskala regler som används av tjänster använder när flera regler har angetts.

På *skala ut*, Autoskala körs om någon regel uppfylls.
På *skala i*, Autoskala kräver alla regler toobe uppfylls.

tooillustrate, förutsätter att du har följande 4 Autoskala regler hello:

* Om CPU < 30% skala i med 1
* Om minnet < 50% skala i med 1
* Om CPU > 75%, skala ut med 1
* Om minnet > 75%, skala ut med 1

Hello följande inträffar:

* Om Processorn är 76% och minne är 50%, vi skala ut.
* Om Processorn är 50% minne är 76% vi skala ut.

Hej på andra sidan om är 25 procent för processor och minne är % av 51 Autoskala finns **inte** skala i. I ordning tooscale i måste CPU vara 29% och minne 49%.

### <a name="always-select-a-safe-default-instance-count"></a>Välj alltid en säker standardinstansantalet
Hej standardinstansantalet är viktigt Autoskala skalas service toothat beräkningen när mått inte är tillgängliga. Därför ska du välja en standard-instanser som är acceptabel för dina arbetsbelastningar.

### <a name="configure-autoscale-notifications"></a>Konfigurera meddelanden om autoskalning
Autoskala meddelar hello administratörer och deltagare hello resurs via e-post om något av följande villkor hello inträffar:

* Autoskala tjänst inte tootake en åtgärd.
* Mått är inte tillgängliga för Autoskala service toomake skala beslut.
* Mått är tillgängliga (återställning) igen toomake skala beslut.
  Dessutom toohello villkoren ovan, du kan konfigurera e-post eller webhook meddelanden tooget för lyckade skalningsåtgärder meddelas.
  
Du kan också använda en aktivitetsloggen avisering toomonitor hello hälsotillstånd hello Autoskala motorn. Här följer exempel för[och skapa en aktivitet loggen avisering toomonitor alla Autoskala motorn åtgärder för din prenumeration](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert) eller för[skapa en aktivitet loggen avisering toomonitor alla misslyckades Autoskala skalan i / skala åtgärder din prenumeration](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).

## <a name="next-steps"></a>Nästa steg
- [Skapa en aktivitet loggen avisering toomonitor alla Autoskala motorn åtgärder för din prenumeration.](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [Skapa en aktivitet loggen avisering toomonitor alla misslyckades Autoskala skalan i / skala ut åtgärder för din prenumeration](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)
