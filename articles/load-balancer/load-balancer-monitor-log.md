---
title: "aaaMonitor åtgärder, händelser och prestandaräknare för belastningsutjämnaren | Microsoft Docs"
description: "Lär dig hur tooenable avisering händelser och avsökning hälsa status loggning för Azure belastningsutjämnare"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: 56656d74-0241-4096-88c8-aa88515d676d
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: ac53c2254e06cad780ad6144c5c30f0085d12576
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-for-azure-load-balancer"></a>Logganalys för Azure Load Balancer

Du kan använda olika typer av loggar i Azure toomanage och felsöka belastningsutjämnare. Vissa av dessa loggar kan nås via hello-portalen. Alla loggar kan extraheras från Azure blob storage och visas i olika verktyg som Excel och PowerBI. Mer information om hello olika typer av loggar hello listan nedan.

* **Granskningsloggar:** du kan använda [Azure-granskningsloggarna](../monitoring-and-diagnostics/insights-debugging-with-events.md) (kallades tidigare operativa loggar) tooview alla åtgärder som skickats tooyour Azure-prenumeration(er) och deras status. Granskningsloggar är aktiverade som standard och kan visas i hello Azure-portalen.
* **Varna händelseloggar:** kan du använda den här loggen tooview aviseringar rasied hello belastningsutjämnaren. hello status för hello belastningsutjämnaren samlas var femte minut. Den här loggen skrivs endast om en belastningsutjämnare avisering händelsen utlöses.
* **Hälsotillstånd avsökningen loggar:** du kan använda den här loggen tooview problem som identifieras av din hälsoavsökningen, till exempel hello antalet instanser i en serverdelspool som inte tar emot förfrågningar från hello belastningsutjämnaren på grund av fel för avsökning av hälsotillstånd. Den här loggen skapas toowhen ändras i hello hälsostatus för avsökning.

> [!IMPORTANT]
> Logga analytics fungerar för närvarande endast för belastningsutjämnare mot Internet. Loggarna är bara tillgängliga för resurser som har distribuerats i hello Resource Manager-distributionsmodellen. Du kan inte använda loggar för resurser i hello klassiska distributionsmodellen. Läs mer om hello distributionsmodeller [förstå Resource Manager distribution och klassisk distribution](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="enable-logging"></a>Aktivera loggning

Granskningsloggning aktiveras automatiskt för varje resurs för hanteraren för filserverresurser. Du behöver tooenable händelse och hälsotillstånd avsökningen loggning toostart samlar in hello-data som är tillgängliga via dessa loggar. Använd följande steg tooenable loggning hello.

Logga in toohello [Azure-portalen](http://portal.azure.com). Om du inte redan har en belastningsutjämnare [skapa en belastningsutjämnare](load-balancer-get-started-internet-arm-ps.md) innan du fortsätter.

1. I hello-portalen klickar du på **Bläddra**.
2. Välj **belastningsutjämnare**.

    ![Portal - belastningsutjämnare](./media/load-balancer-monitor-log/load-balancer-browse.png)

3. Välj en befintlig belastningsutjämnare >> **alla inställningar**.
4. Hello höger sida av hello dialogrutan under hello namn av hello belastningsutjämnare rulla för**övervakning**, klickar du på **diagnostik**.

    ![Portal –--inställningarna för belastningsutjämnaren](./media/load-balancer-monitor-log/load-balancer-settings.png)

5. I hello **diagnostik** rutan under **Status**väljer **på**.
6. Klicka på **Lagringskonto**.
7. Under **loggar**, Välj ett befintligt lagringskonto eller skapa en ny. Använd hello skjutreglaget toodetermine hur många dagar med händelsedata sparas i hello händelseloggar. 
8. Klicka på **Spara**.

    ![Portal - diagnostik loggar](./media/load-balancer-monitor-log/load-balancer-diagnostics.png)

> [!NOTE]
> Granskningsloggar kräver inte ett separat lagringskonto. Hej användning av lagring för händelsen och hälsa avsökningen loggning kommer avgifter service.

## <a name="audit-log"></a>granskningslogg

hello granskningsloggen skapas som standard. hello loggar bevaras under 90 dagar i Azures händelseloggar store. Mer information om de här loggarna genom att läsa hello [visa händelser och granskningsloggar](../monitoring-and-diagnostics/insights-debugging-with-events.md) artikel.

## <a name="alert-event-log"></a>Aviseringen händelseloggen

Den här loggen genereras bara om du har aktiverat på en per belastningen belastningsutjämnaren basis. hello händelser loggas i JSON-format och lagras i hello storage-konto som du angav när du har aktiverat hello loggning. hello följande är ett exempel på en händelse.

```json
{
    "time": "2016-01-26T10:37:46.6024215Z",
    "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
    "category": "LoadBalancerAlertEvent",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
    "operationName": "LoadBalancerProbeHealthStatus",
    "properties": {
        "eventName": "Resource Limits Hit",
        "eventDescription": "Ports exhausted",
        "eventProperties": {
            "public ip address": "40.117.227.32"
        }
    }
}
```

hello JSON utdata visar hello *eventname* -egenskap som beskriver hello orsak för hello belastningsutjämnaren skapas en avisering. I det här fallet kunde hello avisering som genererats på grund av tooTCP port resursuttömning på grund av käll-IP NAT begränsar (SNAT).

## <a name="health-probe-log"></a>Hälsotillstånd avsökningen logg

Den här loggen genereras bara om du har aktiverat på en per belastningen belastningsutjämnaren bas enligt anvisningarna ovan. hello data lagras i hello storage-konto som du angav när du har aktiverat hello loggning. En behållare med namnet 'insikter-loggar loadbalancerprobehealthstatus' har skapats och hello följande data loggas:

```json
{
    "records":[
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 1,
            "healthPercentage": 50.000000
        }
    },
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 0,
            "healthPercentage": 100.000000
        }
    }]
}
```

hello JSON-utdata visar hello egenskaper fältet hello grundläggande information om hälsostatus för hello avsökning. Hej *dipDownCount* egenskapen visar hello Totalt antal instanser på hello serverdel som inte tar emot nätverkstrafik på grund av toofailed avsökningen svar.

## <a name="view-and-analyze-hello-audit-log"></a>Visa och analysera hello granskningsloggen

Du kan visa och analysera granskningsdata med hjälp av hello följande metoder:

* **Azure-verktyg:** hello granskningsloggar via Azure PowerShell, hello Azure kommandoradsgränssnittet (CLI), hello Azure REST API för att hämta information om eller hello Azure preview portal. Stegvisa instruktioner för varje metod beskrivs i hello [granskningsåtgärder med Resource Manager](../azure-resource-manager/resource-group-audit.md) artikel.
* **Powerbi:** om du inte redan har en [Power BI](https://powerbi.microsoft.com/pricing) konto, du kan testa det gratis. Med hjälp av hello [Azure-granskningsloggarna content pack för Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs), du kan analysera dina data med förkonfigurerade instrumentpaneler och du kan anpassa vyer toosuit dina krav.

## <a name="view-and-analyze-hello-health-probe-and-event-log"></a>Visa och analysera hello hälsoavsökningen och händelseloggen

Du behöver tooconnect tooyour storage-konto och hämta hello JSON-loggposter för händelsen och hälsa avsökningen loggar. När du hämtar hello JSON-filer, kan du omvandla dem tooCSV och visa i Excel, PowerBI eller andra data visualiseringen verktyg.

> [!TIP]
> Om du är bekant med Visual Studio och grundläggande begrepp för att ändra värdena för variabler i C# och konstanter, kan du använda hello [logga konverteraren verktyg](https://github.com/Azure-Samples/networking-dotnet-log-converter) tillgängliga från GitHub.

## <a name="additional-resources"></a>Ytterligare resurser

* [Visualisera dina Azure-granskningsloggarna med Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blogginlägg.
* [Visa och analysera Azure-granskningsloggar i Power BI och mycket mer](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogginlägg.

## <a name="next-steps"></a>Nästa steg

[Förstå avsökningar av belastningsutjämnare](load-balancer-custom-probe-overview.md)
