---
title: "aaaAzure nätverk Analytics lösning i Log Analytics | Microsoft Docs"
description: "Du kan använda hello Azure nätverk Analytics lösning i gruppen för Log Analytics tooreview Azure-nätverk säkerhetsloggar och Azure Programgateway loggar."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: ewinner
editor: 
ms.assetid: 66a3b8a1-6c55-4533-9538-cad60c18f28b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: richrund
ms.openlocfilehash: 3674189786bacccc82e6708e78f14c92178e6676
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-networking-monitoring-solutions-in-log-analytics"></a>Azure nätverk övervakning lösningar i logganalys

Logganalys erbjuder följande lösningar för att övervaka dina nätverk hello:
* Network Performance Monitor (NPM) till
 * Övervaka hello hälsotillstånd för nätverket
* Azure Application Gateway analytics tooreview
 * Azure Application Gateway-loggar
 * Azure Application Gateway-mått
* Azure Network Security Group analytics tooreview
 * Nätverkssäkerhetsgruppen för Azure-loggar

## <a name="network-performance-monitor-npm"></a>Network Performance Monitor (NPM)

Hej [Network Performance Monitor](log-analytics-network-performance-monitor.md) hanteringslösning är ett nätverk övervakningslösning som övervakar hello hälsa, tillgänglighet och tillgänglighet av nätverk.  Det är används toomonitor anslutningen mellan:

* offentliga molnet och lokalt
* datacenter och användarplatser (avdelningskontor)
* undernät som är värd för olika nivåer av ett program med flera nivåer.

Mer information finns i [Network Performance Monitor](log-analytics-network-performance-monitor.md).

## <a name="azure-application-gateway-and-network-security-group-analytics"></a>Azure Application Gateway och Nätverkssäkerhetsgruppen analytics
toouse hello lösningar:
1. Lägg till hello management lösning tooLog analyser och
2. Aktivera diagnostik toodirect hello diagnostik tooa logganalys-arbetsytan. Det är inte nödvändigt toowrite hello loggar tooAzure Blob storage.

Du kan aktivera diagnostik- och hello motsvarande lösning för en eller båda av Programgateway och nätverk säkerhetsgrupper.

Om du inte aktivera diagnostikloggning för en viss resurstyp och installera hello lösning hello instrumentpanelen blad för den här resursen är tomma och ett felmeddelande visas.

> [!NOTE]
> Hello stöds i januari 2017 sätt att skicka loggar från Programgatewayer och Nätverkssäkerhetsgrupper tooLog Analytics har ändrats. Om du ser hello **Azure-nätverk Analytics (föråldrad)** lösningen finns för[migrera från hello gamla nätverk Analytics lösning](#migrating-from-the-old-networking-analytics-solution) för steg behöver du toofollow.
>
>

## <a name="review-azure-networking-data-collection-details"></a>Granska Azure nätverk information för samlingen
samla in diagnostik loggarna direkt från Azure Programgatewayer och Nätverkssäkerhetsgrupper hello Azure Programgateway analyser och hello Network Security Group management Analyslösningar. Det är inte nödvändigt toowrite hello loggar tooAzure Blob storage och ingen agent krävs för datainsamling.

hello följande tabell visar metoder för insamling av data och annan information om hur data samlas in för Azure Programgateway analyser och hello Nätverkssäkerhetsgruppen analytics.

| Plattform | Styr agent | System Center Operations Manager-agenten | Azure | Operations Manager som krävs? | Operations Manager agent-data som skickas via management-grupp | Insamlingsfrekvens |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |  |  |&#8226; |  |  |När du har loggat |


## <a name="azure-application-gateway-analytics-solution-in-log-analytics"></a>Azure Application Gateway analytics lösning i logganalys

![Azure Application Gateway Analytics symbol](./media/log-analytics-azure-networking/azure-analytics-symbol.png)

följande loggar hello stöds för Programgatewayer:

* ApplicationGatewayAccessLog
* ApplicationGatewayPerformanceLog
* ApplicationGatewayFirewallLog

hello följande mått stöds för Programgatewayer:

* 5 minut genomflöde

### <a name="install-and-configure-hello-solution"></a>Installera och konfigurera hello lösning
Använd följande instruktioner tooinstall hello och konfigurera hello Azure Programgateway analytics lösningen:

1. Aktivera hello Azure Programgateway analytics lösningar från [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureAppGatewayAnalyticsOMS?tab=Overview) eller genom att använda hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).
2. Aktivera diagnostikloggning för hello [Programgatewayer](../application-gateway/application-gateway-diagnostics.md) du vill toomonitor.

#### <a name="enable-azure-application-gateway-diagnostics-in-hello-portal"></a>Aktivera Azure Programgateway diagnostik i hello-portalen

1. Navigera i hello Azure-portalen, toohello Programgateway resurs toomonitor
2. Välj *diagnostik loggar* tooopen hello följande sida

   ![Bild av Azure Programgateway resurs](./media/log-analytics-azure-networking/log-analytics-appgateway-enable-diagnostics01.png)
3. Klicka på *aktivera diagnostiken* tooopen hello följande sida

   ![Bild av Azure Programgateway resurs](./media/log-analytics-azure-networking/log-analytics-appgateway-enable-diagnostics02.png)
4. tooturn på diagnostik, klickar du på *på* under *Status*
5. Klicka på hello kryssrutan för *skicka tooLog Analytics*
6. Välj en befintlig logganalys-arbetsyta eller skapa en arbetsyta
7. Klicka på kryssrutan hello under **loggen** för varje hello loggen typer toocollect
8. Klicka på *spara* tooenable hello loggning av diagnostik tooLog Analytics

#### <a name="enable-azure-network-diagnostics-using-powershell"></a>Aktivera Azure Nätverksdiagnostik med PowerShell

hello följande PowerShell-skript innehåller ett exempel på hur tooenable loggning för programgatewayer.

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$gateway = Get-AzureRmApplicationGateway -Name 'ContosoGateway'

Set-AzureRmDiagnosticSetting -ResourceId $gateway.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-application-gateway-analytics"></a>Använd Azure Programgateway analytics
![Bild av Azure Programgateway analytics panelen](./media/log-analytics-azure-networking/log-analytics-appgateway-tile.png)

När du klickar på hello **Azure Programgateway analytics** panelen på hello översikt, kan du visa sammanfattningar av loggar och visa sedan detaljnivåerna i toodetails för hello följande kategorier:

* Programåtkomst för Gateway-loggar
  * Klient- och fel för åtkomstloggar för Programgateway
  * Förfrågningar per timme för varje Programgateway
  * Misslyckade förfrågningar per timme för varje Programgateway
  * Fel per användaragent för Programgatewayer
* Programprestanda för Gateway
  * Värden hälsa för Programgateway
  * Maximal och 95 percentil för Programgateway misslyckade begäranden

![Bild av instrumentpanelen för Azure Programgateway](./media/log-analytics-azure-networking/log-analytics-appgateway01.png)

![Bild av instrumentpanelen för Azure Programgateway](./media/log-analytics-azure-networking/log-analytics-appgateway02.png)

På hello **Azure Programgateway analytics** instrumentpanel, granska hello sammanfattningsinformation i ett hello blad och klicka sedan på en tooview detaljerad information om hello söksidan för loggen.

Du kan visa resultaten av tid, detaljerade resultat och Logghistoriken på någon av hello loggen söksidor. Du kan också filtrera efter facets toonarrow hello resultat.


## <a name="azure-network-security-group-analytics-solution-in-log-analytics"></a>Azure Network Security Group analytics lösning i logganalys

![Azure Network Security Group Analytics symbol](./media/log-analytics-azure-networking/azure-analytics-symbol.png)

följande loggar hello stöds för nätverkssäkerhetsgrupper:

* NetworkSecurityGroupEvent
* NetworkSecurityGroupRuleCounter

### <a name="install-and-configure-hello-solution"></a>Installera och konfigurera hello lösning
Använd följande instruktioner tooinstall hello och konfigurera hello Azure nätverk Analytics lösning:

1. Aktivera hello Azure Network Security Group analytics lösningar från [Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureNSGAnalyticsOMS?tab=Overview) eller genom att använda hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).
2. Aktivera diagnostikloggning för hello [Nätverkssäkerhetsgruppen](../virtual-network/virtual-network-nsg-manage-log.md) resurser du vill toomonitor.

### <a name="enable-azure-network-security-group-diagnostics-in-hello-portal"></a>Aktivera diagnostik av gruppen för Azure-nätverk i hello-portalen

1. Navigera i hello Azure-portalen, toohello Nätverkssäkerhetsgruppen resurs toomonitor
2. Välj *diagnostik loggar* tooopen hello följande sida

   ![Bild av Azure Network Security Group resurs](./media/log-analytics-azure-networking/log-analytics-nsg-enable-diagnostics01.png)
3. Klicka på *aktivera diagnostiken* tooopen hello följande sida

   ![Bild av Azure Network Security Group resurs](./media/log-analytics-azure-networking/log-analytics-nsg-enable-diagnostics02.png)
4. tooturn på diagnostik, klickar du på *på* under *Status*
5. Klicka på hello kryssrutan för *skicka tooLog Analytics*
6. Välj en befintlig logganalys-arbetsyta eller skapa en arbetsyta
7. Klicka på kryssrutan hello under **loggen** för varje hello loggen typer toocollect
8. Klicka på *spara* tooenable hello loggning av diagnostik tooLog Analytics

### <a name="enable-azure-network-diagnostics-using-powershell"></a>Aktivera Azure Nätverksdiagnostik med PowerShell

hello följande PowerShell-skript innehåller ett exempel på hur tooenable loggning för nätverkssäkerhetsgrupper
```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$nsg = Get-AzureRmNetworkSecurityGroup -Name 'ContosoNSG'

Set-AzureRmDiagnosticSetting -ResourceId $nsg.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-network-security-group-analytics"></a>Använd Azure Network Security Group analytics
När du klickar på hello **Azure Network Security Group analytics** panelen på hello översikt, kan du visa sammanfattningar av loggar och visa sedan detaljnivåerna i toodetails för hello följande kategorier:

* Nätverkssäkerhetsgruppen blockeras flöden
  * Regler för nätverkssäkerhetsgrupper med blockerade flöden
  * MAC-adresser med blockerade flöden
* Nätverkssäkerhetsgruppen tillåtna flöden
  * Regler för nätverkssäkerhetsgrupper med tillåtna flöden
  * MAC-adresser med tillåtna flöden

![Bild av instrumentpanelen för Azure Nätverkssäkerhetsgrupp](./media/log-analytics-azure-networking/log-analytics-nsg01.png)

![Bild av instrumentpanelen för Azure Nätverkssäkerhetsgrupp](./media/log-analytics-azure-networking/log-analytics-nsg02.png)

På hello **Azure Network Security Group analytics** instrumentpanel, granska hello sammanfattningsinformation i ett hello blad och klicka sedan på en tooview detaljerad information om hello söksidan för loggen.

Du kan visa resultaten av tid, detaljerade resultat och Logghistoriken på någon av hello loggen söksidor. Du kan också filtrera efter facets toonarrow hello resultat.

## <a name="migrating-from-hello-old-networking-analytics-solution"></a>Migrera från hello gamla nätverk Analytics lösning
Hello stöds i januari 2017 sätt att skicka loggar från Azure Programgatewayer och säkerhetsgrupper för Azure-nätverket tooLog Analytics har ändrats. Ändringarna ger hello följande fördelar:
+ Loggarna skrivs direkt tooLog Analytics utan hello måste toouse ett lagringskonto
+ Mindre fördröjning från hello tid när loggar är genereras toothem som är tillgängliga i logganalys
+ Färre konfigurationssteg
+ Ett vanligt format för alla typer av Azure-diagnostik

toouse hello uppdateras lösningar:

1. [Konfigurera diagnostik toobe tooLog Analytics skickas direkt från Azure Programgatewayer](#enable-azure-application-gateway-diagnostics-in-the-portal)
2. [Konfigurera diagnostik toobe tooLog Analytics skickas direkt från Azure Nätverkssäkerhetsgrupper](#enable-azure-network-security-group-diagnostics-in-the-portal)
2. Aktivera hello *Azure Application Gateway Analytics* och hello *Azure Network Security Group Analytics* lösning genom att använda hello process beskrivs i [lägga till logganalys lösningar från hello lösningar galleri](log-analytics-add-solutions.md)
3. Uppdatera alla sparade frågor, instrumentpaneler eller aviseringar toouse hello ny datatyp
  + Typen är tooAzureDiagnostics. Du kan använda hello ResourceType toofilter tooAzure nätverk loggar.

    | Istället för: | Användning: |
    | --- | --- |
    |`Type=NetworkApplicationgateways OperationName=ApplicationGatewayAccess`| `Type=AzureDiagnostics ResourceType=APPLICATIONGATEWAYS OperationName=ApplicationGatewayAccess` |
    |`Type=NetworkApplicationgateways OperationName=ApplicationGatewayPerformance` | `Type=AzureDiagnostics ResourceType=APPLICATIONGATEWAYS OperationName=ApplicationGatewayPerformance` |
    | `Type=NetworkSecuritygroups` | `Type=AzureDiagnostics ResourceType=NETWORKSECURITYGROUPS` |

   + För alla fält som har suffixet \_s, \_d, eller \_g i hello namn, ändra hello första toolower skiftlägeskänslighet
   + För alla fält som har suffixet \_o i namn hello data delas upp i enskilda fält baserat på hello kapslade fältnamn.
4. Ta bort hello *Azure nätverk Analytics (inaktuell)* lösning.
  + Om du använder PowerShell använder du`Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that hello workspace is in> -WorkspaceName <name of hello log analytics workspace> -IntelligencePackName "AzureNetwork" -Enabled $false`

Data som samlas in innan hello ändringen inte visas i hello ny lösning. Du kan fortsätta tooquery för den här data med hjälp av hello gamla typ och fältnamn.

## <a name="troubleshooting"></a>Felsökning
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a>Nästa steg
* Använd [logga sökningar i logganalys](log-analytics-log-searches.md) tooview detaljerad Azure-diagnostikdata.
