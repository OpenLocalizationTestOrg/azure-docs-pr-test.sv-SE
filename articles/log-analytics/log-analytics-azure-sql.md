---
title: "aaaAzure SQL Analytics lösning i Log Analytics | Microsoft Docs"
description: "hello Azure SQL Analytics lösningen hjälper dig att hantera dina Azure SQL-databaser."
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
ms.openlocfilehash: fe228bb3cb3f9d578a84707c3917f02fbeb8a627
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview-in-log-analytics"></a>Övervaka Azure SQL Database med Azure SQL Analytics (förhandsgranskning) i logganalys

![Azure SQL Analytics symbol](./media/log-analytics-azure-sql/azure-sql-symbol.png)

hello Azure SQL Analytics lösning i Azure Log Analytics samlar in och visualizes viktiga mått för prestanda för SQL Azure. Använder hello mått som du samlar in hello-lösning kan skapa du anpassade regler för övervakning och aviseringar. Du kan övervaka Azure SQL Database och elastisk pool mått över flera Azure-prenumerationer och elastiska pooler och visualisera dem. hello lösningen hjälper dig också tooidentify problem på varje nivå i program-stacken.  Den använder [Azure diagnostisk mått](log-analytics-azure-storage.md) tillsammans med logganalys-vyer toopresent data om alla Azure SQL-databaser och elastiska pooler i en enda logganalys-arbetsytan.

Den här preview-lösningen stöder för närvarande upp too150 000 Azure SQL-databaser och 5 000 SQL elastiska pooler per arbetsytan.

hello Azure SQL Analytics lösning, precis som andra tillgängliga för Log Analytics hjälper dig att övervaka och ta emot meddelanden om hello hälsotillstånd resurserna i Azure – i det här fallet, Azure SQL Database. Microsoft Azure SQL Database är en skalbar relationsdatabas-tjänst som tillhandahåller välbekanta SQL-Server-liknande funktioner tooapplications körs i hello Azure-molnet. Logganalys hjälper dig att toocollect, korrelera och visualisera strukturerade och Ostrukturerade data.

## <a name="connected-sources"></a>Anslutna källor

hello Azure SQL Analytics lösningen använder inte agenter tooconnect toohello logganalys-tjänsten.

hello i den följande tabellen beskrivs hello anslutna källor som stöds av den här lösningen.

| Ansluten källa | Support | Beskrivning |
| --- | --- | --- |
| [Windows-agenter](log-analytics-windows-agents.md) | Nej | Direkt Windows-agenter som inte används av hello lösning. |
| [Linux-agenter](log-analytics-linux-agents.md) | Nej | Direkt Linux-agenter som inte används av hello lösning. |
| [SCOM-hanteringsgrupp](log-analytics-om-agents.md) | Nej | En direkt anslutning från hello SCOM-agent tooLog Analytics används inte av hello-lösning. |
| [Azure Storage-konto](log-analytics-azure-storage.md) | Nej | Logganalys läser inte hello data från ett lagringskonto. |
| [Azure-diagnostik](log-analytics-azure-storage.md) | Ja | Azure mått data skickas tooLog Analytics direkt av Azure. |

## <a name="prerequisites"></a>Krav

- En Azure-prenumeration. Om du inte har någon, kan du skapa en för [ledigt](https://azure.microsoft.com/free/).
- Logganalys-arbetsytan. Du kan använda en befintlig eller kan du [skapa en ny](log-analytics-get-started.md) innan du börjar använda den här lösningen.
- Aktivera Azure-diagnostik för din Azure SQL-databaser och elastiska pooler och [konfigurera dem toosend sina data tooLog Analytics](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).

## <a name="configuration"></a>Konfiguration

Utför följande steg tooadd hello Azure SQL Analytics lösning tooyour arbetsytan hello.

1. Lägg till hello Azure SQL Analytics lösning tooyour arbetsyta från [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview) eller genom att använda hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).
2. I hello Azure-portalen klickar du på **ny** (hello symbolen +), i hello lista över resurser, väljer du **övervakning + Management**.  
    ![Övervakning och hantering](./media/log-analytics-azure-sql/monitoring-management.png)
3. I hello **övervakning + Management** lista Klicka **se alla**.
4. I hello **rekommenderas** klickar du på **mer** , och sedan i hello ny lista, **Azure SQL Analytics (förhandsgranskning)** och markera den.  
    ![Azure SQL Analytics-lösning](./media/log-analytics-azure-sql/azure-sql-solution-portal.png)
5. I hello **Azure SQL Analytics (förhandsgranskning)** bladet, klickar du på **skapa**.  
    ![Skapa](./media/log-analytics-azure-sql/portal-create.png)
6. I hello **Skapa ny lösning** bladet, Välj hello arbetsytan som du vill tooadd hello lösning tooand och klicka sedan på **skapa**.  
    ![Lägg till tooworkspace](./media/log-analytics-azure-sql/add-to-workspace.png)


### <a name="tooconfigure-multiple-azure-subscriptions"></a>tooconfigure flera Azure-prenumerationer

toosupport flera prenumerationer använder hello PowerShell-skriptet från [aktivera Azure resource mått loggning med hjälp av PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/). Ange hello arbetsytan resurs-ID som en parameter när du kör skriptet hello toosend diagnostikdata från resurser i en Azure-prenumeration tooa arbetsyta i en annan Azure-prenumeration.

**Exempel**

```
PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/oms/providers/microsoft.operationalinsights/workspaces/omsws"
```

```
PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
```

## <a name="using-hello-solution"></a>Med hello-lösning

När du lägger till hello lösning tooyour arbetsytan hello Azure SQL Analytics panelen läggs tooyour arbetsytan och det visas i Översikt. hello panelen visar hello antalet Azure SQL-databaser och elastiska pooler i Azure SQL som hello lösningen är ansluten till.

![Azure SQL Analytics sida vid sida](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a>Visa analysdata för Azure SQL

Klicka på hello **Azure SQL Analytics** panelen tooopen hello Azure SQL instrumentpanelen. hello instrumentpanelen innehåller hello blad som anges nedan. Varje bladet visar too15 resurser (prenumeration, server, elastisk pool och databas). Klicka på någon av hello resurser tooopen hello instrumentpanelen för den särskilda resursen. Elastisk Pool eller databasen innehåller hello diagram med mått för en markerad resurs. Klicka på ett diagram tooopen hello loggen Sök dialogruta.

| Bladet | Beskrivning |
|---|---|
| Prenumerationer | Listan över prenumerationer med antal anslutna servrar, pooler och databaser. |
| Servrar | Listan över servrar med antalet anslutna pooler och databaser. |
| Elastiska pooler | Lista över anslutna elastiska pooler med högsta GB och eDTU i hello observerade tidsperioden. |
|Databaser | Lista över anslutna databaser med högsta GB och DTU i hello observerade tidsperioden.|


### <a name="analyze-data-and-create-alerts"></a>Analysera data och skapa varningar

Du kan enkelt skapa aviseringar med hello data från Azure SQL Database-resurser. Här följer några användbara [loggen Sök](log-analytics-log-searches.md) frågor som du kan använda för aviseringar:

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]


*Hög DTU på Azure SQL-databas*

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/DATABASES/"* MetricName=dtu_consumption_percent | measure Avg(Average) by Resource interval 5minutes
```

*Hög DTU på Azure SQL Database-elastisk Pool*

```
Type=AzureMetrics ResourceProvider="MICROSOFT.SQL" ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource interval 5minutes
```

Du kan använda dessa avisering-baserade frågor tooalert på specifika tröskelvärden för både Azure SQL Database och elastiska pooler. tooconfigure en avisering för din OMS-arbetsyta:

#### <a name="tooconfigure-an-alert-for-your-workspace"></a>tooconfigure en avisering för din arbetsyta

1. Gå toohello [OMS-portalen](http://mms.microsoft.com/) och logga in.
2. Öppna hello-arbetsyta som du har konfigurerat för hello lösning.
3. Klicka på översiktssidan för hello hello **Azure SQL Analytics (förhandsgranskning)** panelen.
4. Kör något av hello exempelfrågor.
5. I loggen sökning, klickar du på **avisering**.  
![Skapa en avisering i sökningen](./media/log-analytics-azure-sql/create-alert01.png)
6. På hello **lägga till Varningsregeln** konfigurerar hello lämpliga egenskaper och hello specifika tröskelvärden som du vill använda och klicka sedan på **spara**.  
![lägga till varningsregel](./media/log-analytics-azure-sql/create-alert02.png)

### <a name="act-on-azure-sql-analytics-data"></a>Arbeta med Azure SQL analysdata

Exempelvis är en hello mest användbara frågor som du kan utföra toocompare hello DTU-användningen för alla Azure SQL elastiska pooler över alla dina Azure-prenumerationer. Databasen genomströmning Units (DTU) ger ett sätt toodescribe hello relativa kapacitet för nivåerna Basic, Standard och Premium-databaser och pooler. Dtu: er baseras på ett blandat mått av CPU, minne, läser och skriver. Dtu: er ökar hello power erbjuds av hello prestanda ökar. Till exempel har prestandanivå med 5 dtu: er fem gånger mer än en prestandanivå med 1 DTU. En högsta DTU-kvot gäller tooeach server och elastisk pool.

Genom att köra hello efter logg sökfråga kan se du lätt om du modellfunktionerna eller över använder din SQL Azure elastiska pooler.

```
Type=AzureMetrics ResourceId=*"/ELASTICPOOLS/"* MetricName=dtu_consumption_percent | measure avg(Average) by Resource | display LineChart
```

>[!NOTE]
> Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), hello ovan frågan skulle ändra toohello följande.
>
>`search in (AzureMetrics) isnotempty(ResourceId) and "/ELASTICPOOLS/" and MetricName == "dtu_consumption_percent" | summarize AggregatedValue = avg(Average) by bin(TimeGenerated, 1h), Resource | render timechart`

I följande exempel hello, ser du att en elastisk pool har ett hårt nära 100% DTU medan andra har mycket lite användning. Du kan undersöka ytterligare tootroubleshoot eventuella ändringar i din miljö som använder Azure aktivitetsloggar.

![Sökresultat för logg - hög belastning](./media/log-analytics-azure-sql/log-search-high-util.png)

## <a name="see-also"></a>Se även

- Använd [loggen sökningar](log-analytics-log-searches.md) i logganalys tooview detaljerade Azure SQL-data.
- [Skapa dina egna instrumentpaneler](log-analytics-dashboards.md) Azure SQL data visas.
- [Skapa aviseringar](log-analytics-alerts.md) när specifika Azure SQL-händelser inträffar.
