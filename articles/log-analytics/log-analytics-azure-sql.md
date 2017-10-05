---
title: "Azure SQL Analytics lösning i Log Analytics | Microsoft Docs"
description: "Azure SQL Analytics lösningen hjälper dig att hantera dina Azure SQL-databaser."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: b2712749-1ded-40c4-b211-abc51cc65171
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: banders
ms.openlocfilehash: cab45cc6dd621eb4a95ef5f1842ec38c25e980b6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview-in-log-analytics"></a>Övervaka Azure SQL Database med Azure SQL Analytics (förhandsgranskning) i logganalys

![Azure SQL Analytics symbol](./media/log-analytics-azure-sql/azure-sql-symbol.png)

Azure SQL Analytics lösningen i Azure Log Analytics samlar in och visualizes viktiga mått för prestanda för SQL Azure. Med det mått som du samlar in med lösningen kan skapa du anpassade regler för övervakning och aviseringar. Du kan övervaka Azure SQL Database och elastisk pool mått över flera Azure-prenumerationer och elastiska pooler och visualisera dem. Lösningen hjälper dig att identifiera problem på varje nivå i program-stacken.  Den använder [Azure diagnostisk mått](log-analytics-azure-storage.md) tillsammans med logganalys vyer presentera data om dina Azure SQL-databaser och elastiska pooler i en enda logganalys-arbetsyta.

Den här preview-lösningen stöder för närvarande, upp till 150 000 Azure SQL-databaser och 5 000 SQL elastiska pooler per arbetsytan.

Azure SQL Analytics-lösningen, precis som andra tillgängliga för Log Analytics hjälper dig att övervaka och ta emot meddelanden om hälsotillståndet för dina Azure-resurser – i det här fallet, Azure SQL Database. Microsoft Azure SQL Database är en skalbar relationsdatabas-tjänst som tillhandahåller välbekanta SQL-Server-liknande funktioner till program som körs i Azure-molnet. Logganalys hjälper dig att samla in, korrelera och visualisera strukturerade och Ostrukturerade data.

## <a name="connected-sources"></a>Anslutna källor

Azure SQL Analytics lösningen använder inte agenter för att ansluta till Log Analytics-tjänsten.

I följande tabell beskrivs de anslutna källor som stöds av den här lösningen.

| Ansluten källa | Support | Beskrivning |
| --- | --- | --- |
| [Windows-agenter](log-analytics-windows-agents.md) | Nej | Direkt Windows-agenter som inte används av lösningen. |
| [Linux-agenter](log-analytics-linux-agents.md) | Nej | Direkt Linux-agenter som inte används av lösningen. |
| [SCOM-hanteringsgrupp](log-analytics-om-agents.md) | Nej | En direkt anslutning från SCOM-agent till logganalys används inte av lösningen. |
| [Azure Storage-konto](log-analytics-azure-storage.md) | Nej | Logganalys inte att läsa data från ett lagringskonto. |
| [Azure-diagnostik](log-analytics-azure-storage.md) | Ja | Azure mått data skickas till logganalys direkt av Azure. |

## <a name="prerequisites"></a>Krav

- En Azure-prenumeration. Om du inte har någon, kan du skapa en för [ledigt](https://azure.microsoft.com/free/).
- Logganalys-arbetsytan. Du kan använda en befintlig eller kan du [skapa en ny](log-analytics-get-started.md) innan du börjar använda den här lösningen.
- Aktivera Azure-diagnostik för din Azure SQL-databaser och elastiska pooler och [konfigurera dem att skicka data till logganalys](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).

## <a name="configuration"></a>Konfiguration

Utför följande steg för att lägga till Azure SQL Analytics lösningen till din arbetsyta.

1. Lägg till Azure SQL Analytics-lösning till arbetsytan från [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) eller genom att använda processen som beskrivs i [lägga till logganalys lösningar från galleriet lösningar](log-analytics-add-solutions.md).
2. I Azure-portalen klickar du på **ny** (den symbolen +), Välj i listan över resurser, **övervakning + Management**.  
    ![Övervakning och hantering](./media/log-analytics-azure-sql/monitoring-management.png)
3. I den **övervakning + Management** lista Klicka **se alla**.
4. I den **rekommenderas** klickar du på **mer** , och sedan i den nya listan **Azure SQL Analytics (förhandsgranskning)** och markera den.  
    ![Azure SQL Analytics-lösning](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)
5. I den **Azure SQL Analytics (förhandsgranskning)** bladet, klickar du på **skapa**.  
    ![Skapa](./media/log-analytics-azure-sql/portal-create.png)
6. I den **Skapa ny lösning** bladet, väljer arbetsytan som du vill lägga till lösningen på och klicka sedan på **skapa**.  
    ![Lägg till arbetsytan](./media/log-analytics-azure-sql/add-to-workspace.png)


### <a name="to-configure-multiple-azure-subscriptions"></a>Så här konfigurerar du Azure-prenumerationer

För att stödja flera prenumerationer, använder du PowerShell-skriptet från [aktivera Azure resource mått loggning med hjälp av PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/). Ange arbetsytan resurs-ID som en parameter när du kör skriptet för att skicka diagnostikdata från resurser i en Azure-prenumeration till en arbetsyta i en annan Azure-prenumeration.

**Exempel**

```
PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/oms/providers/microsoft.operationalinsights/workspaces/omsws"
```

```
PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
```

## <a name="using-the-solution"></a>Använda lösningen

När du lägger till lösningen till din arbetsyta Azure SQL Analytics panel har lagts till din arbetsyta och det visas i Översikt. Panelen visar antalet Azure SQL-databaser och elastiska pooler i Azure SQL som lösningen är ansluten till.

![Azure SQL Analytics sida vid sida](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a>Visa analysdata för Azure SQL

Klicka på den **Azure SQL Analytics** öppna Azure SQL Analytics-instrumentpanelen. Instrumentpanelen innehåller blad som anges nedan. Varje bladet visar upp till 15 resurser (prenumeration, server, elastisk pool och databas). Klicka på någon av resurser för att öppna instrumentpanelen för den särskilda resursen. Elastisk Pool eller databasen innehåller diagram med mått för en markerad resurs. Klicka på ett diagram om du vill öppna dialogrutan Sök i loggfilen.

| Bladet | Beskrivning |
|---|---|
| Prenumerationer | Listan över prenumerationer med antal anslutna servrar, pooler och databaser. |
| Servrar | Listan över servrar med antalet anslutna pooler och databaser. |
| Elastiska pooler | Lista över anslutna elastiska pooler med högsta GB och eDTU i den observerade tidsperioden. |
|Databaser | Lista över anslutna databaser med högsta GB och DTU i den observerade tidsperioden.|


### <a name="analyze-data-and-create-alerts"></a>Analysera data och skapa varningar

Du kan enkelt skapa aviseringar med data från Azure SQL Database-resurser. Här följer några användbara [loggen Sök](log-analytics-log-searches.md) frågor som du kan använda för aviseringar:

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]


*Hög DTU på Azure SQL-databas*

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/DATABASES/"* MetricName=dtu_consumption_percent | measure Avg(Average) by Resource interval 5minutes
```

*Hög DTU på Azure SQL Database-elastisk Pool*

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource interval 5minutes
```

Du kan använda dessa avisering-baserade frågor för att Avisera om specifika tröskelvärden för både Azure SQL Database och elastiska pooler. Konfigurera en avisering för din OMS-arbetsyta:

#### <a name="to-configure-an-alert-for-your-workspace"></a>Så här konfigurerar du en avisering för din arbetsyta

1. Gå till den [OMS-portalen](http://mms.microsoft.com/) och logga in.
2. Öppna arbetsytan som du har konfigurerat för lösningen.
3. På sidan Översikt över den **Azure SQL Analytics (förhandsgranskning)** panelen.
4. Kör något av de exempel på frågorna.
5. I loggen sökning, klickar du på **avisering**.  
![Skapa en avisering i sökningen](./media/log-analytics-azure-sql/create-alert01.png)
6. På den **lägga till Varningsregeln** konfigurerar lämpliga egenskaper och specifika tröskelvärden som du vill använda och klicka sedan på **spara**.  
![lägga till varningsregel](./media/log-analytics-azure-sql/create-alert02.png)

### <a name="act-on-azure-sql-analytics-data"></a>Arbeta med Azure SQL analysdata

Exempelvis är en av de mest användbara frågor som du kan utföra att jämföra DTU-användningen för alla Azure SQL elastiska pooler över alla dina Azure-prenumerationer. Databasen genomströmning Units (DTU) är ett sätt att beskriva relativa kapacitet för nivåerna Basic, Standard och Premium-databaser och pooler. Dtu: er baseras på ett blandat mått av CPU, minne, läser och skriver. Dtu: er ökar, ökar den effekt som erbjuds av prestandanivå. Till exempel har prestandanivå med 5 dtu: er fem gånger mer än en prestandanivå med 1 DTU. En högsta DTU-kvot gäller för varje server och en elastisk pool.

Genom att köra följande loggen frågan kan se du lätt om du modellfunktionerna eller via använder din SQL Azure elastiska pooler.

```
Type=AzureMetrics ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource | display LineChart
```

>[!NOTE]
> Om ditt arbetsområde har uppgraderats till den [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), sedan frågan ovan skulle ändra till följande.
>
>`search in (AzureMetrics) isnotempty(ResourceId) and "/ELASTICPOOLS/" and MetricName == "dtu_consumption_percent" | summarize AggregatedValue = avg(Average) by bin(TimeGenerated, 1h), Resource | render timechart`

I följande exempel visas att en elastisk pool har ett hårt nära 100% DTU medan andra har mycket lite användning. Du kan undersöka vidare för att felsöka eventuella ändringar i din miljö som använder Azure aktivitetsloggar.

![Sökresultat för logg - hög belastning](./media/log-analytics-azure-sql/log-search-high-util.png)

## <a name="see-also"></a>Se även

- Använd [loggen sökningar](log-analytics-log-searches.md) i logganalys att visa detaljerad Azure SQL-data.
- [Skapa dina egna instrumentpaneler](log-analytics-dashboards.md) Azure SQL data visas.
- [Skapa aviseringar](log-analytics-alerts.md) när specifika Azure SQL-händelser inträffar.
