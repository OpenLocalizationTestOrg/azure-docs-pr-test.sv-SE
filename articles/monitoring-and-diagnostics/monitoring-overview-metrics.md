---
title: "aaaOverview av mått i Microsoft Azure | Microsoft Docs"
description: "Översikt över mått och deras användning i Microsoft Azure"
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 405ec51c-0946-4ec9-b535-60f65c4a5bd1
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2017
ms.author: johnkem
ms.openlocfilehash: 2b97f51e0554dae95a929241ae1f0e25e5c215ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-metrics-in-microsoft-azure"></a>Översikt över mått i Microsoft Azure
Den här artikeln beskriver vad är i Microsoft Azure har sina fördelar, och hur toostart som använder dem.  

## <a name="what-are-metrics"></a>Vad är mått?
Azure-Monitor kan du tooconsume telemetri toogain insyn i hello prestanda och hälsotillståndet för dina arbetsbelastningar i Azure. hello är viktigaste Azure telemetridata hello mått (kallas även prestandaräknare) sänds av mest Azure-resurser. Övervakare för Azure tillhandahåller flera olika sätt tooconfigure och använda de här måtten för övervakning och felsökning.

## <a name="what-can-you-do-with-metrics"></a>Vad kan du göra med mått?
Mått är en del av telemetri och aktivera toodo hello följande uppgifter:

* **Spåra hello prestanda** för din resurs (till exempel en VM, webbplats eller logik app) genom att rita upp dess mått i en portal diagram och fästa diagrammet tooa instrumentpanelen.
* **Få ett meddelande om ett problem** att hello påverkan på prestandan för din resurs när en måttet överskrider ett visst tröskelvärde.
* **Konfigurera automatiska åtgärder**, till exempel autoskalning en resurs eller startar en runbook när en måttet överskrider ett visst tröskelvärde.
* **Utföra avancerade analyser** eller rapportering på prestanda eller användning trender för din resurs.
* **Arkivera** hello prestanda eller hälsa historiken för din resurs **för efterlevnad och granskning** syften.

## <a name="what-are-hello-characteristics-of-metrics"></a>Vad är hello egenskaper mått?
Mått har hello följande egenskaper:

* Alla mätvärden har **minut frekvens**. Du får ett värde varje minut från din resurs så att du nära realtid insyn i hello tillstånd och hälsotillståndet för din resurs.
* Mått är **tillgängliga omedelbart**. Du inte behöver tooopt i eller konfigurera ytterligare diagnostik.
* Du kan komma åt **30 dagar från historiken** för varje mått. Du kan snabbt se hello senare och månatliga trender i hello prestanda eller hälsotillståndet för din resurs.

Du kan också:

* Konfigurera ett mått **avisering regel som skickar ett meddelande eller tar automatisk åtgärd** när hello mått korsar hello tröskelvärde som du har angett. Autoskala är en automatisk specialåtgärd som aktiverar tooscale ut din resurs toomeet inkommande begäranden eller läses in på webbplatsen eller datorresurser. Du kan konfigurera en Autoskalningsinställning regel tooscale in eller ut baserat på ett mått som passerar ett tröskelvärde.

* **Väg** alla mått Application Insights eller logganalys (OMS) tooenable instant analytics Sök och anpassade aviseringar på mått data från dina resurser. Du kan också strömma mått tooan Event Hub, aktiverar du toothen dirigera dem tooAzure Stream Analytics eller toocustom appar för analys i nära realtid. Du ställa in Event Hub strömning med diagnostikinställningar.

* **Arkivera mått toostorage** längre bevarande eller använda dem för offline-rapportering. Du kan vidarebefordra dina mått tooAzure Blob storage när du konfigurerar inställningar för diagnostik för din resurs.

* Enkelt identifiera, komma åt, och **visa alla** via hello Azure-portalen när du väljer en resurs och rita hello mått i ett diagram.

* **Använda** hello mätvärden via hello nya Azure övervakaren REST API: er.

* **Frågan** mått med hjälp av hello PowerShell-cmdlets eller hello plattformsoberoende REST API.

  ![Routning av mätvärden i Azure-Monitor](./media/monitoring-overview-metrics/Metrics_Overview_v4.png)

## <a name="access-metrics-via-hello-portal"></a>Åtkomst till mätvärden via hello-portalen
Följande är en snabb genomgång av hur toocreate ett mått diagram med hjälp av hello Azure-portalen.

### <a name="tooview-metrics-after-creating-a-resource"></a>tooview mått när du har skapat en resurs
1. Öppna hello Azure-portalen.
2. Skapa en Azure App Service-webbplats.
3. När du skapar en webbplats går toohello **översikt** bladet för hello webbplats.
4. Du kan visa nya mått som en **övervakning** panelen. Du kan redigera hello sida vid sida och välja fler mått.

   ![Mått på en resurs i Azure-Monitor](./media/monitoring-overview-metrics/MetricsOverview1.png)

### <a name="tooaccess-all-metrics-in-a-single-place"></a>tooaccess alla mått i en enda plats
1. Öppna hello Azure-portalen.
2. Navigera toohello nya **övervakaren** fliken och sedan och välj hello **mått** alternativet under.
3. Välj din prenumeration, resursgrupp och hello namnet på hello resursen hello nedrullningsbara listan.
4. Visa hello tillgängliga mått lista. Välj sedan hello mått du är intresserad av och rita den.
5. Du kan fästa den toohello instrumentpanelen genom att klicka på hello PIN-kod på hello övre högra hörnet.

   ![Åtkomst till alla mått i en enda plats i Azure-Monitor](./media/monitoring-overview-metrics/MetricsOverview2.png)

> [!NOTE]
> Du kan komma åt värdnivå mått från virtuella datorer (Azure Resource Manager-baserat) och virtuella datorn anger utan några ytterligare inställningar för diagnostik. Dessa nya värdnivå mått är tillgängliga för Windows och Linux-instanser. De här måtten är inte toobe förväxlas med hello gäst-OS-nivå mått som du har åtkomst toowhen du aktiverar Azure-diagnostik på virtuella datorer eller virtuella datorer. toolearn mer om hur du konfigurerar Diagnostics, se [vad är Microsoft Azure-diagnostik](../azure-diagnostics.md).
>
>

## <a name="access-metrics-via-hello-rest-api"></a>Åtkomst till mätvärden via hello REST API
Azure mått kan nås via hello Azure-Monitor API: er. Det finns två API: er som hjälper dig identifiera och komma åt mått:

* Använd hello [mått för Azure-Monitor definitioner REST API](https://msdn.microsoft.com/library/mt743621.aspx) tooaccess hello listan över mått som är tillgängliga för en tjänst.
* Använd hello [Azure övervakaren mått REST API](https://msdn.microsoft.com/library/mt743622.aspx) tooaccess hello faktiska mätvärdesdata.

> [!NOTE]
> Den här artikeln beskriver hello mätvärden via hello [nya API: et för mått](https://msdn.microsoft.com/library/dn931930.aspx) för Azure-resurser. hello API-version för hello nya måttdefinitioner API är 2016-03-01 och hello version för mått API är 2016-09-01. hello äldre måttdefinitioner och mått som kan nås med hello API version 2014-04-01.
>
>

En mer detaljerad genomgång med hello Azure övervakaren REST API: er finns [REST-API för Azure-Monitor genomgången](monitoring-rest-api-walkthrough.md).

## <a name="export-metrics"></a>Exportera mått
Du kan gå toohello **diagnostikinställningarna** bladet under hello **övervakaren** fliken och visa hello exportalternativ för mått. Du kan välja mått (och diagnostikloggar) toobe dirigeras tooBlob lagring, tooAzure Händelsehubbar eller tooOMS för användningsfall som nämndes tidigare i den här artikeln.

 ![Exportalternativ för mätvärden i Azure-Monitor](./media/monitoring-overview-metrics/MetricsOverview3.png)

Du kan konfigurera detta via Resource Manager-mallar [PowerShell](insights-powershell-samples.md), [Azure CLI](insights-cli-samples.md), eller [REST API: er](https://msdn.microsoft.com/library/dn931943.aspx).

## <a name="take-action-on-metrics"></a>Utför en åtgärd på mått
Du kan konfigurera Varningsregler eller Autoskala inställningar tooreceive meddelanden eller vidta automatiserade åtgärder på måttdata.

### <a name="configure-alert-rules"></a>Konfigurera Varningsregler
Du kan konfigurera Varningsregler på mått. Dessa Varningsregler kan kontrollera om ett mått har passerat ett visst tröskelvärde. De kan sedan meddela dig via e-post eller starta en webhook som kan använda toorun alla anpassade skript. Du kan också använda hello webhook tooconfigure tredjepartsprodukt integreringar.

 ![Mått och Varningsregler i Azure-Monitor](./media/monitoring-overview-metrics/MetricsOverview4.png)

### <a name="autoscale-your-azure-resources"></a>Autoskala din Azure resurser
Vissa Azure-resurser stöd hello skalning in eller ut av flera instanser toohandle dina arbetsbelastningar. Autoskala gäller tooApp Service (Web Apps), skalningsuppsättningar i virtuella datorer och klassiska Azure-molntjänster. Du kan konfigurera automatiska regler tooscale in eller ut när ett mått som påverkar din arbetsbelastning överskrider ett tröskelvärde som du anger. Mer information finns i [översikt över autoskalning](monitoring-overview-autoscale.md).

 ![Mått och Autoskala i Azure-Monitor](./media/monitoring-overview-metrics/MetricsOverview5.png)

## <a name="learn-about-supported-services-and-metrics"></a>Lär dig mer om tjänster som stöds och mått
Azure övervakaren är en ny infrastruktur för mått. Den stöder hello följande Azure-tjänster i hello Azure-portalen och hello ny version av hello Azure övervakaren API:

* Virtuella datorer (Azure Resource Manager-baserade)
* Skalningsuppsättningar för virtuella datorer
* Batch
* Event Hubs namnområde
* Service Bus-namnrymd (endast premium SKU)
* SQL-databas (version 12)
* Elastisk SQL-poolen
* Webbplatser
* Webbservergrupper
* Logic Apps
* IoT-hubbar
* Redis Cache
* Nätverk: Programgatewayer
* Söka

Du kan visa en detaljerad lista över alla hello stöds tjänster och deras mått på [Azure-Monitor mått--stöds mått per resurstypen](monitoring-supported-metrics.md).

## <a name="next-steps"></a>Nästa steg
Läs toohello länkar i den här artikeln. Dessutom lär dig mer om:  

* [Vanliga mätvärden för autoskalning](insights-autoscale-common-metrics.md)
* [Hur toocreate Varningsregler](insights-alerts-portal.md)
* [Analysera loggar från Azure storage med logganalys](../log-analytics/log-analytics-azure-storage.md)
