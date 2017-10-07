---
title: aaaCreate en anpassade instrumentpanel i Azure Log Analytics | Microsoft Docs
description: "Den här guiden hjälper dig att förstå hur logganalys instrumentpaneler kan visualisera alla dina sparad logg-sökningar, vilket ger dig en enda lins tooview din miljö."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 73fcf131a91c743d473f37d5a40d52eaf78a7ba3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-dashboard-for-use-in-log-analytics"></a>Skapa en anpassad instrumentpanel för användning i logganalys

>[!NOTE]
> Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), och du inte kan skapa nya instrumentpaneler eller redigera befintliga instrumentpaneler. 

Den här guiden hjälper dig att förstå hur logganalys instrumentpaneler kan visualisera alla dina sparad logg-sökningar, vilket ger dig en enda lins tooview din miljö.

![Exempel instrumentpanelen](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

Alla anpassade hello-instrumentpaneler som du skapar i hello OMS-portalen är också tillgängliga i hello OMS Mobilapp. Se hello följande sidor för mer information om hello appar.

* [OMS mobilappen från hello Microsoft Store](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
* [OMS mobila appar från Apple iTunes](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![mobila instrumentpanelen](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a>Hur skapar jag min instrumentpanel
toobegin gå toohello OMS översiktssidan. Du ser hello **min instrumentpanel** panelen hello vänster. Klicka på den toodrill ned i instrumentpanelen.

![Översikt](./media/log-analytics-dashboards/oms-dashboards-overview.png)

## <a name="adding-a-tile"></a>Lägga till en panel
I instrumentpaneler drivs paneler av sparad logg-sökningar. OMS levereras med många färdiga sparad logg sökningar, så att du kan börja direkt. Använd hello följande steg dispositionen hur toobegin.

I hello Mina instrumentpanelsvy, klicka bara på **anpassa** tooenter anpassningsläge.

![Illustrationer](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 hello panel som öppnas hello höger hello sidan visas alla ditt arbetsområde sparad logg sökningar. sparad logg sökning som en panel toovisualize hovra över en sparad sökning och klicka sedan på hello **plus** symbolen.

![Lägg till paneler 1](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

När du klickar på hello **plus** symboler, en ny panel visas i hello Mina instrumentpanelsvy.

![Lägg till paneler 2](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)

## <a name="edit-a-tile"></a>Redigera en panel
I hello Mina instrumentpanelsvy, klicka bara på **anpassa** tooenter anpassningsläge. Klicka på hello panelen du vill tooedit. hello högra panelen ändrar tooedit och ger ett antal alternativ:

![Redigera sida vid sida](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Redigera sida vid sida](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a>Panelen visualiseringar
Det finns tre typer av panelen visualiseringar toochoose från:

| diagramtyp | syfte |
| --- | --- |
| ![Liggande stapeldiagram](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png) |Visar en tidslinje för din sparad logg sökresultat som ett stapeldiagram eller en lista med resultat av ett fält beroende på om sökningen loggen aggregerar resultaten efter ett fält eller inte. |
| ![Mått](./media/log-analytics-dashboards/oms-dashboards-metric.png) |Visar träffar din totala Sök resultatet som ett tal i en panel. Mått paneler kan du tooset ett tröskelvärde som ska markeras hello panelen när hello tröskelvärdet har uppnåtts. |
| ![raden](./media/log-analytics-dashboards/oms-dashboards-line.png) |Visar en tidslinje sparad logg Sök resultatet träffar med värden som ett linjediagram. |

### <a name="threshold"></a>Tröskelvärde
Du kan skapa ett tröskelvärde på en panel med hello mått visualiseringen. Välj på toocreate ett tröskelvärde på hello sida vid sida. Välj om toohighlight hello panelen när hello värde ligger över eller under hello valt tröskelvärde sedan ange hello tröskelvärdet nedan.

## <a name="organizing-hello-dashboard"></a>Ordna hello instrumentpanelen
tooorganize instrumentpanelen, navigera toohello Mina instrumentpanelsvy och klicka på **anpassa** tooenter anpassningsläge. Klicka och dra hello panelen du vill toomove och flytta den toowhere som du vill att sida vid sida-toobe.

![Ordna din instrumentpanel](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a>Ta bort en panel
tooremove en panel navigera toohello Mina instrumentpanelsvy och klicka på **anpassa** tooenter anpassningsläge. Välj hello panelen du vill tooremove på hello högra panelen och välj sedan **ta bort panelen**.

![Ta bort en panel](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a>Nästa steg
* Skapa [aviseringar](log-analytics-alerts.md) i logganalys toogenerate meddelanden och tooremediate problem.
