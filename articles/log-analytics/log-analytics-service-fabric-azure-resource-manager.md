---
title: "aaaAssess Service Fabric-program med Log Analytics med hjälp av hello Azure-portalen | Microsoft Docs"
description: "Du kan använda hello Service Fabric-lösning i Log Analytics med hjälp av hello Azure portal tooassess hello risk och hälsotillståndet hos Service Fabric-program micro-tjänster, noder och kluster."
services: log-analytics
documentationcenter: 
author: niniikhena
manager: jochan
editor: 
ms.assetid: 9c91aacb-c48e-466c-b792-261f25940c0c
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: nini
ms.openlocfilehash: 891c7f6e5ed511ac18599bdc280ab3dc09700fbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assess-service-fabric-applications-and-micro-services-with-hello-azure-portal"></a>Utvärdera Service Fabric-program och micro-tjänster med hello Azure-portalen

> [!div class="op_single_selector"]
> * [Resource Manager](log-analytics-service-fabric-azure-resource-manager.md)
> * [PowerShell](log-analytics-service-fabric.md)
>
>

![Service Fabric-symbolen](./media/log-analytics-service-fabric/service-fabric-assessment-symbol.png)

Den här artikeln beskrivs hur toouse hello Service Fabric-lösning i logganalys toohelp identifiera och felsöka problem i Service Fabric-klustret.

hello Service Fabric-lösningen använder Azure-diagnostik data från din Service Fabric virtuella datorer genom att samla in data från Azure BOMULLSTUSS tabellerna. Logganalys läser sedan Service Fabric framework händelser, inklusive **händelser för tillförlitlig tjänst**, **aktören händelser**, **drifthändelser**, och **anpassad ETW-händelser**. Med hello lösning instrumentpanel är du kan tooview anmärkningsvärda problem och relevanta händelser i Service Fabric-miljö.

tooget igång med hello lösning, behöver du tooconnect Service Fabric-kluster tooa logganalys-arbetsytan. Här följer tre scenarier tooconsider:

1. Om du inte har distribuerat Service Fabric-kluster använder hello stegen i ***distribuera ett Service Fabric-kluster anslutna tooa logganalys-arbetsytan*** toodeploy ett nytt kluster och har konfigurerat tooreport tooLog Analytics.
2. Om du behöver toocollect prestandaräknare från värdar-toouse andra OMS-lösningar, till exempel säkerhet på Service Fabric-kluster, gör hello i ***distribuera ett Service Fabric-kluster anslutna tooa logganalys-arbetsytan med VM-tillägg installerad.***
3. Om du redan har distribuerat Service Fabric-kluster och vill tooconnect den tooLog Analytics, gör hello i ***att lägga till en befintlig lagring konto tooLog Analytics.***

## <a name="deploy-a-service-fabric-cluster-connected-tooa-log-analytics-workspace"></a>Distribuera ett Service Fabric-kluster anslutna tooa logganalys-arbetsytan.
Den här mallen hello följande:

1. Distribuerar en Azure Service Fabric klustret redan ansluten tooa logganalys-arbetsytan. Du har hello alternativet toocreate arbetsyta vid distribution av hello mall eller inkommande hello namnet på en redan befintlig logganalys-arbetsyta.
2. Lägger till hello diagnostiska storage-konto toohello logganalys-arbetsytan.
3. Aktiverar hello Service Fabric-lösning i logganalys-arbetsytan.

[![Distribuera tooAzure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)

När du har valt hello distribuera ovan, hello Azure portal öppnas med parametrar du tooedit. Vara säker på att toocreate en ny resursgrupp om du ange ett nytt namn för logganalys-arbetsyta:

![Service Fabric](./media/log-analytics-service-fabric/2.png)

![Service Fabric](./media/log-analytics-service-fabric/3.png)

Godkänn hello juridiska villkoren och klicka på **skapa** toostart hello distribution. När hello distributionen är klar du bör se hello ny arbetsyta och kluster skapas och hello WADServiceFabric * händelse, WADWindowsEventLogs och WADETWEvent tabeller som har lagts till:

![Service Fabric](./media/log-analytics-service-fabric/4.png)

## <a name="deploy-a-service-fabric-cluster-connected-tooa-log-analytics-workspace-with-vm-extension-installed"></a>Distribuera ett Service Fabric-kluster anslutna tooa logganalys-arbetsytan med VM-tillägg som är installerad.

Den här mallen hello följande:

1. Distribuerar en Azure Service Fabric klustret redan ansluten tooa logganalys-arbetsytan. Du kan skapa en ny arbetsyta eller använda en befintlig.
2. Lägger till hello diagnostiska storage-konton toohello logganalys-arbetsytan.
3. Aktiverar hello Service Fabric-lösning i hello logganalys-arbetsytan.
4. Installerar tillägget för hello MMA agent i varje virtuell dator skala i Service Fabric-klustret. Med hello MMA-agenten är installerad, är du kan tooview prestandamått om noderna.

[![Distribuera tooAzure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)

Följande hello samma stegen ovan inkommande hello nödvändiga parametrar och startar en distribution. Igen bör du se hello ny arbetsyta, kluster och BOMULLSTUSS tabeller alla skapade:

![Service Fabric](./media/log-analytics-service-fabric/5.png)

### <a name="viewing-performance-data"></a>Visar prestandadata

tooview prestandadata från din noder:


[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

- Starta hello logganalys-arbetsytan från hello Azure-portalen.
  ![Service Fabric](./media/log-analytics-service-fabric/6.png)
- Gå tooSettings hello vänster och välj Data >> Windows prestandaräknare >> ”hello Lägg till valda prestandaräknare”: ![Service Fabric](./media/log-analytics-service-fabric/7.png)
- Använd hello följande frågor toodelve till nyckelvärden om noderna i loggen sökning:

    a. Jämför hello Genomsnittlig CPU-användning för alla noder i hello senast en timme toosee vilka noder har problem och med vilka tidsintervall en nod har en topp:

    ```
    Type=Perf ObjectName=Processor CounterName="% Processor Time"|measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/10.png)

    b. Visa liknande linjediagram för tillgängligt minne på varje nod med den här frågan:

    ```
    Type=Perf ObjectName=Memory CounterName="Available MBytes Memory" | measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    tooview en lista över alla noder, visar hello exakt medelvärdet för tillgängliga megabyte för varje nod, Använd den här frågan:

    ```
    Type=Perf (ObjectName=Memory) (CounterName="Available MBytes") | measure avg(CounterValue) by Computer
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/11.png)

    c. I hello fall som du vill toodrill ned till en viss nod genom att undersöka hello timvis medelvärde, lägsta, högsta och 75 percentil CPU-användning, du är kan toodo detta genom att med hjälp av den här frågan (Ersätt datorn fältet):

    ```
    Type=Perf CounterName="% Processor Time" InstanceName=_Total Computer="BaconDC01.BaconLand.com"| measure min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) by Computer Interval 1HOUR
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/12.png)

Mer information om prestandamått i logganalys på hello [Operations Management Suite-blogg](https://blogs.technet.microsoft.com/msoms/tag/metrics/).


## <a name="adding-an-existing-storage-account-toolog-analytics"></a>Lägga till en befintlig lagring konto tooLog Analytics

Den här mallen lägger bara till befintlig lagring konton tooa ny eller befintlig logganalys-arbetsytan.

[![Distribuera tooAzure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)

> [!NOTE]
> Att välja en resursgrupp, Välj om du arbetar med en redan befintlig logganalys-arbetsyta ”Använd befintligt” och Sök efter hello resursgruppen som innehåller hello logganalys-arbetsytan. Skapa en ny en om annars.
> ![Service Fabric](./media/log-analytics-service-fabric/8.png)
>
>

När den här mallen har distribuerats, kommer du att kunna toosee hello lagring konto ansluten tooyour logganalys-arbetsytan. I den här instansen jag har lagt till en mer lagringsutrymme konto toohello Exchange arbetsytan jag skapade ovan.
![Service Fabric](./media/log-analytics-service-fabric/9.png)

## <a name="view-service-fabric-events"></a>Visa händelser för Service Fabric

När hello distributioner har slutförts och hello Service Fabric-lösningen har aktiverats på arbetsytan, Välj hello **Service Fabric** panelen i hello logganalys portal toolaunch hello Service Fabric-instrumentpanelen. hello instrumentpanelen innehåller hello kolumner i hello i den följande tabellen. Varje kolumn visar hello översta 10 händelser efter antal matchande att kolumnens sökvillkor för hello angivna tidsintervallet. Du kan köra en sökning i logg som innehåller hello hela listan genom att klicka på **se alla** längst ned hello rätt i varje kolumn, eller klicka på kolumnrubriken hello.

| **Service Fabric-händelse** | **Beskrivning** |
| --- | --- |
| Anmärkningsvärda problem |En översikt över problem som till exempel RunAsyncFailures RunAsynCancellations och noden används. |
| Operativa händelser |Viktiga operativa händelser, till exempel distributioner och uppgradering av programmet. |
| Händelser för tillförlitlig tjänst |Viktiga tillförlitlig tjänsthändelser sådana Runasyncinvocations. |
| Aktören händelser |Viktiga aktören händelser som genererats av din micro-tjänster, till exempel undantag som utlöses av en aktörsmetod, aktören aktiveringar och avaktiveringar och så vidare. |
| Programhändelser |Alla anpassade ETW händelser som genererats av dina program. |

![Service Fabric-instrumentpanelen](./media/log-analytics-service-fabric/sf3.png)

![Service Fabric-instrumentpanelen](./media/log-analytics-service-fabric/sf4.png)

hello följande tabell visar metoder för insamling av data och annan information om hur data samlas in för Service Fabric.

| Plattform | Styr Agent | Operations Manager-agent | Azure Storage | Operations Manager som krävs? | Operations Manager agent-data som skickas via management-grupp | Frekvens för samlingen |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |  |  | &#8226; |  |  |10 minuter |

> [!NOTE]
> Du kan ändra hello omfånget för dessa händelser i hello Service Fabric-lösningen genom att klicka på **Data baserat på senaste 7 dagarna** hello överst i hello instrumentpanelen. Du kan också visa händelser som genereras inom hello senaste sju dagarna, en dag eller sex timmar. Du kan också välja **anpassade** toospecify datumintervall.
>
>

## <a name="next-steps"></a>Nästa steg

* Använd [loggen söker i logganalys](log-analytics-log-searches.md) tooview detaljerad Service Fabric händelsedata.
