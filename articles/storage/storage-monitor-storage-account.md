---
title: aaaHow toomonitor ett Azure Storage-konto | Microsoft Docs
description: "Lär dig hur toomonitor ett lagringskonto i Azure med hjälp av hello Azure-portalen."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: b83cba7b-4627-4ba7-b5d0-f1039fe30e78
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: marsma
ms.openlocfilehash: 782873648fdbccc0191a074cd4044f97af8b7c19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-storage-account-in-hello-azure-portal"></a>Övervaka ett lagringskonto i hello Azure-portalen

[Azure Storage Analytics](storage-analytics.md) ger mått för alla lagringstjänster och loggar för BLOB, köer och tabeller. Du kan använda hello [Azure-portalen](https://portal.azure.com) tooconfigure vilka mått och loggar för ditt konto har registrerats och konfigurera diagram som ger visuella representationer av mått-data.

> [!NOTE]
> Det finns kostnader i samband med att undersöka övervakningsdata i hello Azure-portalen. Mer information finns i [Storage Analytics och fakturering för](/rest/api/storageservices/Storage-Analytics-and-Billing).
>
> Azure File storage kan du för närvarande stöder Storage Analytics mätvärden, men ännu stöder inte loggning.
>
> Storage-konton med en typ av replikering av Zonredundant lagring (ZRS) för närvarande har inte hello mått eller loggningsfunktioner aktiverad.
> 
> Utförliga instruktioner om hur du använder Storage Analytics och andra verktyg tooidentify, diagnostisera i, och felsöka problem med Azure Storage, se [övervaka, diagnostisera och felsöka Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).
>

## <a name="configure-monitoring-for-a-storage-account"></a>Konfigurera övervakning för ett lagringskonto

1. I hello [Azure-portalen](https://portal.azure.com)väljer **lagringskonton**, och sedan hello konto namn tooopen hello konto lagringsinstrumentpanel.
1. Välj **diagnostik** i hello **övervakning** avsnitt i hello menyn bladet.

    ![MonitoringOptions](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)

1. Välj hello **typen** av mätvärdesdata för varje **service** du vill toomonitor och hello **bevarandeprincip** för hello data. Du kan också inaktivera övervakning genom att ange **Status** för**av**.

    ![MonitoringOptions](./media/storage-monitor-storage-account/stg-enable-metrics-01.png)

   Det finns två typer av mått som du kan aktivera för varje tjänst som är aktiverad som standard för nya storage-konton:

   * **Sammanställd**: samlar in mätvärden, till exempel ingång-/ utgång, tillgänglighet, svarstid och lyckade procenttal. De här måtten samman för hello blob, kön, tabell och Filtjänster.
   * **Per API**: I tillägg toohello sammanställd statistik och, samlar in hello samma uppsättning mätvärden för varje Lagringsåtgärden i hello Azure Storage service API.

   tooset hello databevarandeprincip, flytta hello **bevarande (dagar)** skjutreglaget eller ange hello antal dagar som data tooretain från 1 too365. hello standard för nya storage-konton är sju dagar. Om du inte vill att tooset en bevarandeprincip ange noll. Om det finns inga bevarandeprincip, är det upp tooyou toodelete hello övervakningsdata.

   > [!WARNING]
   > Du debiteras när du manuellt ta bort mått data. Inaktuella analysdata (data som är äldre än din bevarandeprincip) tas bort av hello system utan kostnad. Vi rekommenderar en bevarandeprincip baserat på hur länge du vill tooretain storage analytics-data för ditt konto. Se [vilka avgifter gör uppkommer när du aktiverar storage-mätvärden?](storage-enable-and-view-metrics.md#what-charges-do-you-incur-when-you-enable-storage-metrics) för mer information.
   >

1. När du är klar hello övervakningskonfigurationen Välj **spara**.

En standarduppsättning mått visas i diagram på hello lagring konto-bladet, samt hello enskild tjänst blad (blob, kön, tabell och filen). När du har aktiverat mått för en tjänst, kan det ta upp tooan timme för data tooappear i dess diagram. Du kan välja **redigera** alla mått diagram för[konfigurera vilka mått](#how-to-customize-metrics-charts) visas i hello diagram.

Du kan inaktivera insamling av mätvärden och loggning genom att ange **Status** för**av**.

> [!NOTE]
> Azure Storage använder [tabell lagring](storage-introduction.md#table-storage) toostore hello mätvärden för ditt lagringskonto och lagrar hello mått i tabeller i ditt konto. Mer information finns i. [Hur mått lagras](storage-analytics.md#how-metrics-are-stored).
>

## <a name="customize-metrics-charts"></a>Anpassa mått diagram

Använda följande procedur toochoose hello vilka lagring mått tooview i ett mått diagram. 

1. Starta genom att visa ett mått diagram för lagring i hello Azure-portalen. Du kan hitta diagram på hello **lagring kontoblad** och i hello **mått** bladet för en enskild tjänst (blob, kön, tabell, fil).

   I det här exemplet vi arbetar med hello följande diagram som visas på hello **lagring kontoblad**:

   ![Val av diagram i Azure-portalen](./media/storage-monitor-storage-account/stg-customize-chart-00.png)

1. Klicka någonstans i hello diagram tooopen hello **mått** bladet. Välj **redigera diagram** tooopen hello **redigera diagram** bladet.

   ![Redigera knappen på bladet för diagram](./media/storage-monitor-storage-account/stg-customize-chart-01.png)

1. På hello **redigera diagram** bladet, Välj hello **tidsintervall** av hello mått toodisplay i hello diagram och hello **service** (blob, kö, tabell, filen) vars mått som du vill toodisplay. Här har vi valt toodisplay hello tidigare veckans mätvärden för hello blob-tjänsten:

   ![Intervallet och tjänsten valet av tid hello redigera diagram-bladet](./media/storage-monitor-storage-account/stg-customize-chart-02.png)

1. Välj hello individuella **mått** du hade som visas i diagrammet hello och klicka sedan på **OK**. Till exempel här vi valt toodisplay hello *ContainerCount* och *ObjectCount* mått:

   ![Enskilda mått val i bladet redigera diagram](./media/storage-monitor-storage-account/stg-customize-chart-03.png)

I diagrammet inte påverkar hello samling eller aggregering lagring av övervakningsdata i hello storage-konto, endast hello visning av mätvärdesdata.

### <a name="metrics-availability-in-charts"></a>Mått tillgänglighet i diagram

hello lista över tillgängliga mått ändringar baserat på vilken tjänst som du har valt i hello listrutan och hello typ av diagram hello du redigerar. Du kan till exempel välja procentandel mått som *PercentNetworkError* och *PercentThrottlingError* bara om du redigerar ett diagram som visar enheter i procent:

![Begäran fel procentandel diagram i hello Azure-portalen](./media/storage-monitor-storage-account/stg-customize-chart-04.png)

### <a name="metrics-resolution"></a>Mått upplösning

hello mått som du har valt i diagnostik avgör hello lösning av hello mätvärden som är tillgängliga för ditt konto:

* **Sammanställd** övervakning innehåller mått, till exempel ingång-/ utgång, tillgänglighet, svarstid och lyckade procenttal. De här måtten samman från hello blob-, tabell-, fil- och queue-tjänster.
* **Per API** ger ökad upplösning mätvärden som är tillgängliga för enskilda lagringsåtgärder dessutom toohello servicenivåer mängder.

## <a name="configure-metrics-alerts"></a>Konfigurera aviseringar för mått

Du kan skapa aviseringar toonotify när tröskelvärdet har uppnåtts för storage resource mått.

1. tooopen hello **Varningsregler bladet**, bläddra nedåt toohello **övervakning** avsnitt i hello **menyn bladet** och välj **Varna regler**.
1. Välj **Lägg till avisering** tooopen hello **lägga till en varningsregel** bladet
1. Välj en **resurs** (blob, fil, kö, tabell) från hello listrutan och ange en **namn** och **beskrivning** för din nya varningsregel.
1. Välj hello **mått** som du vill att tooadd en varning och en avisering **villkoret**, och en **tröskelvärdet**. hello tröskelvärdet enhet ändras beroende på hello mått som du har valt. Till exempel är ”antal” hello enhetstyp för *ContainerCount*, medan hello enhet för hello *PercentNetworkError* mått är en del.
1. Välj hello **Period**. Mått som når eller överskrider hello tröskeln inom hello period utlösa en avisering.
1. (Valfritt) Konfigurera **e-post** och **Webhook** meddelanden. Mer information om webhooks finns [konfigurera en webhook på en Azure mått avisering](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Om du inte konfigurerar e-post eller webhook meddelanden visas aviseringar endast i hello Azure-portalen.

![”Lägg till en varningsregel-bladet i hello Azure-portalen](./media/storage-monitor-storage-account/stg-alert-rules-01.png)

## <a name="add-metrics-charts-toohello-portal-dashboard"></a>Lägg till mått diagram toohello portalens instrumentpanel

Du kan lägga till Azure Storage metrics diagram för någon av dina konton tooyour portal lagringsinstrumentpanel.

1. Välj Klicka **redigera instrumentpanel** när du visar instrumentpanelen i hello [Azure-portalen](https://portal.azure.com).
1. I hello **panelen galleriet**väljer **hitta panelerna genom** > **typen**.
1. Välj **typen** > **lagringskonton**.
1. I **resurser**, Välj hello lagringskonto vars mått som du vill tooadd toohello instrumentpanelen.
1. Välj **kategorier** > **övervakning**.
1. Dra och släpp hello diagram panelen på instrumentpanelen för hello mått som visas. Upprepa för alla mått som visas på hello instrumentpanelen. I följande bild hello, hello ”BLOB - Totalt antal begäranden för” diagrammet markeras som exempel, men alla hello diagram är tillgängliga för placering på instrumentpanelen.

   ![Panelen galleri i Azure-portalen](./media/storage-monitor-storage-account/stg-customize-dashboard-01.png)
1. Välj **klar anpassa** hello övre delen av hello instrumentpanelen när du är klar att lägga till diagram.

När du har lagt till diagram tooyour instrumentpanelen kan du anpassa dem enligt beskrivningen i [anpassa mått diagram](#how-to-customize-metrics-charts).

## <a name="configure-logging"></a>Konfigurera loggning

Du kan instruera Azure Storage toosave diagnostik loggar för läsa, skriva och ta bort begäranden för hello blob-, tabell- och kötjänster. Hej databevarandeprincip som du anger gäller även toothese loggar.

> [!NOTE]
> Azure File storage kan du för närvarande stöder Storage Analytics mätvärden, men ännu stöder inte loggning.
>

1. I hello [Azure-portalen](https://portal.azure.com)väljer **lagringskonton**, hello namnet på hello lagring konto tooopen hello storage-konto-bladet.
1. Välj **diagnostik** i hello **övervakning** avsnitt i hello menyn bladet.

    ![Diagnostik menyalternativet under övervakning i hello Azure-portalen.](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)
    
1. Se till att **Status** har angetts för**på**, och välj hello **services** som du vill att tooenable loggning.

    ![Konfigurera loggning i hello Azure-portalen.](./media/storage-monitor-storage-account/stg-enable-logging-01.png)
1. Klicka på **Spara**.

hello diagnostik loggar sparas i en blobbbehållare med namnet $logs i ditt lagringskonto. Du kan visa hello loggdata med en lagringsutforskare som hello [Microsoft Lagringsutforskaren](http://storageexplorer.com), eller via programmering med storage-klientbibliotek för hello eller PowerShell.

Information om att komma åt hello $logs behållaren finns [aktivera loggning för lagring och åtkomst till loggdata](/rest/api/storageservices/enabling-storage-logging-and-accessing-log-data).

## <a name="next-steps"></a>Nästa steg

* Hitta mer information om [statistik, loggning och fakturering](storage-analytics.md) för Storage Analytics.
* [Aktivera Azure Storage-mätvärden och visa mätvärdesdata](storage-enable-and-view-metrics.md) med hjälp av PowerShell och genom programmering med C#.
