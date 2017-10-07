---
title: "aaaConfigure datakällor i OMS Log Analytics | Microsoft Docs"
description: "Datakällor definierar hello data att logganalys samlar in från agenter och andra anslutna källor.  Den här artikeln beskriver hello begreppet hur Log Analytics använder datakällor, förklarar hello information om hur tooconfigure dem, och ger en översikt över hello olika datakällor som finns tillgängliga."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 67710115-c861-40f8-a377-57c7fa6909b4
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/23/2017
ms.author: bwren
ms.openlocfilehash: ebe8d29a2442a654b98004f624181ff406868e2c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-sources-in-log-analytics"></a>Datakällor i logganalys
Logganalys samlar in data från hello anslutna källor i OMS-arbetsytan och lagrar den i OMS-databasen.  hello-data som samlas in från varje definieras av hello datakällor som du konfigurerar.  Data i hello OMS databasen lagras som en uppsättning poster.  Varje datakälla skapar poster för en viss typ med varje typ med en egen uppsättning egenskaper.

![Logga Analytics datainsamling](./media/log-analytics-data-sources/overview.png)

Datakällor är annat än OMS-lösningar som också samla in data från anslutna källor och skapa poster i hello OMS-databasen.  Lösningar kan läggas till tooyour arbetsyta från hello lösningar galleriet och vanligtvis ger ytterligare analysverktyg i hello OMS-portalen.  

## <a name="summary-of-data-sources"></a>Översikt över datakällor
hello-datakällor som är tillgängliga i logganalys visas i hello i den följande tabellen.  Varje har en länk tooa separat artikel tillhandahåller information för datakällan.

| Datakälla | Händelsetyp | Beskrivning |
|:--- |:--- |:--- |
| [Anpassade loggar](log-analytics-data-sources-custom-logs.md) |\<Loggnamn\>_CL |Textfiler på Windows- eller Linux-agenter som innehåller informationen i felloggen. |
| [Windows-händelseloggar](log-analytics-data-sources-windows-events.md) |Händelse |Händelser som samlas in från hello händelseloggen på Windows-datorer. |
| [Windows-prestandaräknare](log-analytics-data-sources-performance-counters.md) |Perf |Prestandaräknare som samlats in från Windows-datorer. |
| [Linux-prestandaräknare](log-analytics-data-sources-performance-counters.md) |Perf |Prestandaräknare som samlats in från Linux-datorer. |
| [IIS-loggar](log-analytics-data-sources-iis-logs.md) |W3CIISLog |Internet Information Services loggar i W3C-format. |
| [Syslog](log-analytics-data-sources-syslog.md) |Syslog |Syslog-händelser på Windows- eller Linux-datorer. |

## <a name="configuring-data-sources"></a>Konfigurera datakällor
Du konfigurerar datakällor från hello **Data** -menyn i logganalys **inställningar**.  Ingen konfiguration levereras tooall anslutna källor i OMS-arbetsyta.  Du kan för närvarande utesluta alla eventuella agenter från den här konfigurationen.

![Konfigurera Windows-händelser](./media/log-analytics-data-sources/configure-events.png)

1. I hello OMS-konsolen klickar du på hello **inställningar** panelen eller hello **inställningar** knappen hello överst i hello-skärmen.
2. Välj **Data**.
3. Klicka på hello datakälla tooconfigure.
4. Följ hello länken toohello dokumentationen för varje datakälla i hello ovanför tabell information på deras konfiguration.

> [!NOTE]
> Du kan konfigurera logganalys datakällor för närvarande i hello Azure-portalen.

## <a name="data-collection"></a>Datainsamling
Konfigurationer för datakällan levereras tooagents som är direkt anslutna tooLog Analytics inom några minuter.  hello angivna data som samlas in från hello agent och levereras direkt tooLog Analytics på intervall specifika tooeach datakälla.  Hello dokumentationen för varje datakälla som innehåller dessa uppgifter.

System Center Operations Manager (SCOM) agenter i en ansluten hanteringsgrupp, konfigurationer för datakällan översättas till hanteringspaket och levereras toohello hanteringsgruppen var femte minut som standard.  hello agent hämtar hello hanteringspaket som andra och samlar in hello angivna data. Beroende på hello datakälla hello data kommer att antingen skickas tooa hanteringsservern som vidarebefordrar hello data toohello logganalys eller skickar agenten hello hello data tooLog Analytics utan att gå via hello management-servern. Se för[data samling information om OMS-funktioner och lösningar](log-analytics-add-solutions.md#data-collection-details) mer information.  Du kan läsa om information om ansluta SCOM och OMS och ändra hello frekvens konfigurationen levereras på [konfigurerar integrering med System Center Operations Manager](log-analytics-om-agents.md).

Om hello-agenten är tooconnect tooLog Analytics eller Operations Manager, kommer att fortsätta toocollect data som den levererar när den upprättar en anslutning.  Data kan gå förlorade om hello mängden data når hello maximal cachestorlek för hello-klienten, eller om hello-agenten inte är kan tooestablish en anslutning inom 24 timmar.

## <a name="log-analytics-records"></a>Log Analytics-poster
Alla data som samlas in av logganalys lagras i hello OMS-databas som poster.  Innehåller information som samlas in av olika datakällor har sin egen uppsättning egenskaper och identifieras genom sina **typen** egenskapen.  Se hello dokumentationen för varje datakälla och lösning för information på varje posttyp.

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [lösningar](log-analytics-add-solutions.md) att lägga till funktioner tooLog Analytics och också samla in data till hello OMS-databasen.
* Lär dig mer om [logga sökningar](log-analytics-log-searches.md) tooanalyze hello data som samlas in från datakällor och lösningar.  
* Konfigurera [aviseringar](log-analytics-alerts.md) tooproactively meddelar dig om viktiga data som samlas in från datakällor och lösningar.
