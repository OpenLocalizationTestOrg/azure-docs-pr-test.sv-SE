---
title: "aaaAutoscaling och Apptjänstmiljö v1"
description: "Autoskalning och Apptjänst-miljö"
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: c23af2d8-d370-4b1f-9b3e-8782321ddccb
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/11/2017
ms.author: ccompy
ms.openlocfilehash: 1a03cf494309e80596b64471d1a067b2f64a9fee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="autoscaling-and-app-service-environment-v1"></a>V1 autoskalning och Apptjänst-miljö

> [!NOTE]
> Den här artikeln handlar om hello Apptjänstmiljö v1.  Det finns en nyare version av hello Apptjänst-miljö som är enklare toouse och körs på mer kraftfulla infrastruktur. Mer om hello nya versionen börjar med hello toolearn [introduktion toohello Apptjänstmiljö](../app-service/app-service-environment/intro.md).
> 

Stöd för Azure Apptjänst-miljöer *autoskalning*. Du kan Autoskala enskilda arbetarpooler utifrån mått eller schema.

![Autoskala alternativ för en arbetspool.][intro]

Autoskalning optimerar användningen av resurser genom att automatiskt växande och minska storleken på en Apptjänst-miljö toofit din budget och/eller belastningen profil.

## <a name="configure-worker-pool-autoscale"></a>Konfigurera worker poolen Autoskala
Du kan komma åt hello Autoskala funktioner från hello **inställningar** fliken hello arbetspool.

![Fliken Inställningar i hello arbetspool.][settings-scale]

Därifrån är hello-gränssnittet ska vara ganska bekant eftersom den hello planera samma upplevelse som visas när du skalar en Apptjänst. 

![Inställningar för manuell skala.][scale-manual]

Du kan också konfigurera en profil för Autoskala.

![Autoskala inställningar.][scale-profile]

Autoskalningsprofiler är användbara tooset gränsen på din skala. På så sätt kan du ha en konsekvent prestanda som uppstår genom att ange ett skalvärde som nedre gräns (1) och en förutsägbar utgifter fjärrskrivbordsanslutning genom att ange en övre gräns (2).

![Skala inställningarna i profilen.][scale-profile2]

När du definierar en profil kan du lägga till Autoskala regler tooscale uppåt eller nedåt hello antalet instanser i hello arbetspool inom hello gränser som definierats av hello-profilen. Autoskala regler baserat på mått.

![Skalningsregeln.][scale-rule]

 Alla arbetspool eller frontend mått kan vara används toodefine Autoskala regler. De här måtten är hello samma mått som du kan övervaka i hello resurs bladet diagram eller ställa in aviseringar för.

## <a name="autoscale-example"></a>Autoskala exempel
Autoskala för en Apptjänst-miljö kan bäst illustreras genom att gå via ett scenario.

Den här artikeln beskrivs alla hello nödvändiga överväganden när du ställer in autoskalning. hello artikeln guidar dig igenom hello interaktioner som spelas upp när du räkna in autoskalning apptjänstmiljöer som finns i Apptjänst-miljö.

### <a name="scenario-introduction"></a>Scenariot introduktion
Frank är en sysadmin för ett företag som har migrerats till en del av hello arbetsbelastningar han hanterar tooan Apptjänst-miljö.

hello konfigurerad Apptjänst-miljön är toomanual skala på följande sätt:

* **Främre slutar:** 3
* **Arbetspool 1**: 10
* **Arbetspool 2**: 5
* **Arbetspool 3**: 5

Arbetspool 1 används för produktionsarbetsbelastningar medan arbetspool 2 och arbetspool 3 används för quality assurance (QA) och arbetsbelastningar för utveckling.

hello App Service-planer för QA och dev är konfigurerad toomanual skala. hello produktion App Service-plan anges tooautoscale toodeal med variationer i nätverksbelastningen och.

Frank är insatt i hello program. Han känner att hello användningsnivå för är mellan 9:00:00 och 18:00 eftersom detta är ett line-of-business (LOB)-program som medarbetarna använder när de arbetar med hello office. Användning utelämnar efter när användare är klar för den dagen. Utanför användningsnivå finns fortfarande vissa eftersom användare kan komma åt hello app från en fjärrdator med hjälp av sina mobila enheter eller hemdatorer. hello produktion programtjänstplanen är redan konfigurerat tooautoscale baserat på CPU-användning med hello följande regler:

![Inställningar för LOB-app.][asp-scale]

| **Autoskala profil – veckodagar – App Service-plan** | **Autoskala profil – helger – App Service-plan** |
| --- | --- |
| **Namn:** veckodag profil |**Namn:** helg profil |
| **Skala av:** schema-och prestanda |**Skala av:** schema-och prestanda |
| **Profil:** veckodagar |**Profil:** lördag |
| **Typ:** upprepning |**Typ:** upprepning |
| **Målområde:** 5 too20 instanser |**Målområde:** 3 too10 instanser |
| **Dagar:** måndag, tisdag, onsdag, torsdag, fredag |**Dagar:** lördag, söndag |
| **Starttid:** 9:00:00 |**Starttid:** 9:00:00 |
| **Tidszon:** UTC-08 |**Tidszon:** UTC-08 |
|  | |
| **Autoskala regeln (skala upp)** |**Autoskala regeln (skala upp)** |
| **Resurs:** produktion (Apptjänst-miljö) |**Resurs:** produktion (Apptjänst-miljö) |
| **Mått:** CPU i % |**Mått:** CPU i % |
| **Åtgärden:** större än 60% |**Åtgärden:** större än 80% |
| **Varaktighet:** 5 minuter |**Varaktighet:** 10 minuter |
| **Tid aggregering:** Genomsnittlig |**Tid aggregering:** Genomsnittlig |
| **Åtgärd:** öka antalet av 2 |**Åtgärd:** öka antalet med 1 |
| **Kall ned (minuter):** 15 |**Kall ned (minuter):** 20 |
|  | |
| **Autoskala regeln (skala ned)** |**Autoskala regeln (skala ned)** |
| **Resurs:** produktion (Apptjänst-miljö) |**Resurs:** produktion (Apptjänst-miljö) |
| **Mått:** CPU i % |**Mått:** CPU i % |
| **Åtgärden:** mindre än 30% |**Åtgärden:** mindre än 20% |
| **Varaktighet:** 10 minuter |**Varaktighet:** 15 minuter |
| **Tid aggregering:** Genomsnittlig |**Tid aggregering:** Genomsnittlig |
| **Åtgärd:** minska antalet med 1 |**Åtgärd:** minska antalet med 1 |
| **Kall ned (minuter):** 20 |**Kall ned (minuter):** 10 |

### <a name="app-service-plan-inflation-rate"></a>App Service-plan inflationen hastighet
Programtjänstplaner som är konfigurerade tooautoscale gör det på en högsta överföringshastighet per timme. Detta värde kan beräknas baserat på hello-värden som anges på hello Autoskala regeln.

Förstå och beräkna hello *App Service-plan inflationen hastighet* är viktigt för Apptjänst-miljö Autoskala eftersom skala ändringar tooa arbetspool inte omedelbar.

hello App Service-plan inflationen hastighet beräknas enligt följande:

![App Service-plan inflationen beräkning.][ASP-Inflation]

Baserat på hello Autoskala – skala upp regel för hello veckodag profil av hello App Service-plan:

![App Service-plan inflationen hastighet för veckodagar baserat på Autoskala – skala upp regeln.][Equation1]

Hello formeln skulle motsvara hello Autoskala – skala upp regel för hello helgens profil av hello App Service-plan hello gäller:

![App Service-plan inflationen hastighet för helger baserat på Autoskala – skala upp regeln.][Equation2]

Det här värdet kan också beräknas för skala ned åtgärder.

Baserat på hello Autoskala – skala ned regel för hello veckodag profil av hello App Service-plan detta skulle se ut så här:

![App Service-plan inflationen hastighet för veckodagar baserat på Autoskala – skala ned regeln.][Equation3]

Hello formeln skulle motsvara hello Autoskala – skala ned regel för hello helgens profil av hello App Service-plan hello gäller:  

![App Service-plan inflationen hastighet för helger baserat på Autoskala – skala ned regeln.][Equation4]

hello produktion App Service-plan kan växa på högst åtta instanser per timme under hello vecka och fyra instanser per timme när hello helg. Det kan släppa instanser på högst fyra instanser per timme under hello vecka och sex instanser per timme under helger.

Om flera App Service-planer finns i en arbetspool, har du toocalculate hello *totala inflationen hastigheten* som hello summan av hello inflationen hastighet för alla hello App Service-planer som värd i den poolen, worker.

![Beräkning av totala inflationen hastigheten för flera App Service-planer finns i en arbetspool.][ASP-Total-Inflation]

### <a name="use-hello-app-service-plan-inflation-rate-toodefine-worker-pool-autoscale-rules"></a>Använd hello Apptjänst planera inflationen hastighet toodefine worker poolen Autoskala regler
Worker pooler som är värdar för App Service-planer som är konfigurerade tooautoscale behöver att allokera en buffert av kapacitet. hello buffert tillåter hello Autoskala operations toogrow och minska programtjänstplanen efter behov. hello minsta buffertstorlek skulle beräknas hello App Service planera inflationen frekvens.

Eftersom skalningsåtgärder för Apptjänst-miljö ta viss tid tooapply, bör ändringar konto för ytterligare begäran ändringar som kan inträffa när en skalningsåtgärd pågår. tooaccommodate latens, rekommenderar vi att du använder hello beräknas App Service planera inflationen frekvens som hello minsta antal instanser som har lagts till för varje Autoskala-åtgärd.

Med den här informationen Frank definiera hello följande Autoskala profil och regler:

![Autoskala profil regler för LOB-exempel.][Worker-Pool-Scale]

| **Autoskala profil – veckodagar** | **Autoskala profil – söndag** |
| --- | --- |
| **Namn:** veckodag profil |**Namn:** helg profil |
| **Skala av:** schema-och prestanda |**Skala av:** schema-och prestanda |
| **Profil:** veckodagar |**Profil:** lördag |
| **Typ:** upprepning |**Typ:** upprepning |
| **Målområde:** 13 too25 instanser |**Målområde:** 6 too15 instanser |
| **Dagar:** måndag, tisdag, onsdag, torsdag, fredag |**Dagar:** lördag, söndag |
| **Starttid:** 7:00:00 |**Starttid:** 9:00:00 |
| **Tidszon:** UTC-08 |**Tidszon:** UTC-08 |
|  | |
| **Autoskala regeln (skala upp)** |**Autoskala regeln (skala upp)** |
| **Resurs:** arbetspool 1 |**Resurs:** arbetspool 1 |
| **Mått:** WorkersAvailable |**Mått:** WorkersAvailable |
| **Åtgärden:** mindre än 8 |**Åtgärden:** färre än 3 |
| **Varaktighet:** 20 minuter |**Varaktighet:** 30 minuter |
| **Tid aggregering:** Genomsnittlig |**Tid aggregering:** Genomsnittlig |
| **Åtgärd:** öka antalet med 8 |**Åtgärd:** öka antalet med 3 |
| **Kall ned (minuter):** 180 |**Kall ned (minuter):** 180 |
|  | |
| **Autoskala regeln (skala ned)** |**Autoskala regeln (skala ned)** |
| **Resurs:** arbetspool 1 |**Resurs:** arbetspool 1 |
| **Mått:** WorkersAvailable |**Mått:** WorkersAvailable |
| **Åtgärden:** större än 8 |**Åtgärden:** större än 3 |
| **Varaktighet:** 20 minuter |**Varaktighet:** 15 minuter |
| **Tid aggregering:** Genomsnittlig |**Tid aggregering:** Genomsnittlig |
| **Åtgärd:** minska antalet med 2 |**Åtgärd:** minska antalet med 3 |
| **Kall ned (minuter):** 120 |**Kall ned (minuter):** 120 |

hello mål-intervall som anges i profilen för hello beräknas genom hello minsta instanser som definierats i profilen för hello programtjänstplanen + buffert.

hello maximalt intervall skulle vara hello summan av alla hello maximalt intervall för alla App Service-planer finns i hello arbetspool.

hello öka antalet för hello skalas upp regler ska vara set tooat minst 1 X-App Service Plan inflationen hastighet för att skala upp.

Minska antalet kan vara justerade toosomething mellan 1/2 X eller 1 X hello App Service planera inflationen hastighet för att skala ned.

### <a name="autoscale-for-front-end-pool"></a>Autoskala för frontend-pool
Regler för frontend Autoskala är enklare än för arbetarpooler. Främst bör du  
Kontrollera att beakta varaktighet hello mätning och hello cooldown timers skalningsåtgärder på en apptjänstplan inte är omedelbar.

I det här scenariot vet Frank att hello Felfrekvens ökar när frontwebbservrarna når 80% CPU-användning och anger hello Autoskala regeln tooincrease instanser på följande sätt:

![Autoskala inställningar för frontend-pool.][Front-End-Scale]

| **Autoskala profil – framför slutar** |
| --- |
| **Namn:** Autoskala – framför slutar |
| **Skala av:** schema-och prestanda |
| **Profil:** varje dag |
| **Typ:** upprepning |
| **Målområde:** 3 too10 instanser |
| **Dagar:** varje dag |
| **Starttid:** 9:00:00 |
| **Tidszon:** UTC-08 |
|  |
| **Autoskala regeln (skala upp)** |
| **Resurs:** frontend-pool |
| **Mått:** CPU i % |
| **Åtgärden:** större än 60% |
| **Varaktighet:** 20 minuter |
| **Tid aggregering:** Genomsnittlig |
| **Åtgärd:** öka antalet med 3 |
| **Kall ned (minuter):** 120 |
|  |
| **Autoskala regeln (skala ned)** |
| **Resurs:** arbetspool 1 |
| **Mått:** CPU i % |
| **Åtgärden:** mindre än 30% |
| **Varaktighet:** 20 minuter |
| **Tid aggregering:** Genomsnittlig |
| **Åtgärd:** minska antalet med 3 |
| **Kall ned (minuter):** 120 |

<!-- IMAGES -->
[intro]: ./media/app-service-environment-auto-scale/introduction.png
[settings-scale]: ./media/app-service-environment-auto-scale/settings-scale.png
[scale-manual]: ./media/app-service-environment-auto-scale/scale-manual.png
[scale-profile]: ./media/app-service-environment-auto-scale/scale-profile.png
[scale-profile2]: ./media/app-service-environment-auto-scale/scale-profile-2.png
[scale-rule]: ./media/app-service-environment-auto-scale/scale-rule.png
[asp-scale]: ./media/app-service-environment-auto-scale/asp-scale.png
[ASP-Inflation]: ./media/app-service-environment-auto-scale/asp-inflation-rate.png
[Equation1]: ./media/app-service-environment-auto-scale/equation1.png
[Equation2]: ./media/app-service-environment-auto-scale/equation2.png
[Equation3]: ./media/app-service-environment-auto-scale/equation3.png
[Equation4]: ./media/app-service-environment-auto-scale/equation4.png
[ASP-Total-Inflation]: ./media/app-service-environment-auto-scale/asp-total-inflation-rate.png
[Worker-Pool-Scale]: ./media/app-service-environment-auto-scale/wp-scale.png
[Front-End-Scale]: ./media/app-service-environment-auto-scale/fe-scale.png
