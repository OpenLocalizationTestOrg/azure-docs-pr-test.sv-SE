---
title: "Azure övervakning REST API genomgången | Microsoft Docs"
description: "Så att autentisera förfrågningar till och använda Azure övervakning REST API."
author: mcollier
manager: 
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 565e6a88-3131-4a48-8b82-3effc9a3d5c6
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: mcollier
ms.openlocfilehash: 454a85c4752ec9c7522ef147d5ce594ef5992c32
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-monitoring-rest-api-walkthrough"></a>Azure övervakning REST API genomgång
Den här artikeln visar hur du autentisera så att din kod kan använda den [Microsoft Azure övervakaren REST API-referens](https://msdn.microsoft.com/library/azure/dn931943.aspx).         

Azure-Monitor API gör det möjligt att genom programmering hämta tillgängliga mått standarddefinitioner (typ av måttet som CPU-tid, begäranden, etc.), granularitet och måttvärden. När hämtas, kan du spara data i ett separat datalager som Azure SQL Database, Azure Cosmos DB eller Azure Data Lake. Därifrån kan du utföra ytterligare analys efter behov.

Förutom att arbeta med olika mått datapunkter som den här artikeln visar gör övervakaren API det möjligt att lista Varningsregler, visa aktivitetsloggar och mycket mer. En fullständig lista över tillgängliga åtgärder finns i [Microsoft Azure övervakaren REST API-referens](https://msdn.microsoft.com/library/azure/dn931943.aspx).

## <a name="authenticating-azure-monitor-requests"></a>Autentisering av Azure-Monitor förfrågningar
Det första steget är att autentisera begäran.

Alla aktiviteter som körs mot Azure-Monitor API använder Azure Resource Manager autentisering modell. Därför måste alla förfrågningar autentiseras med Azure Active Directory (AD Azure). En metod för att autentisera klientprogrammet är att skapa en Azure AD huvudnamn för tjänsten och hämta token för autentisering (JWT). Följande exempelskript visar att skapa en Azure AD-tjänsten huvudnamn via PowerShell. För en mer detaljerad genomgång finns i dokumentationen på [med Azure PowerShell för att skapa ett huvudnamn för tjänsten att komma åt resurser](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-password). Det är också möjligt att [skapa ett huvudnamn för tjänsten via Azure portal](../azure-resource-manager/resource-group-create-service-principal-portal.md).

```PowerShell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"
$location = "centralus"

# Authenticate to a specific Azure subscription.
Login-AzureRmAccount -SubscriptionId $subscriptionId

# Password for the service principal
$pwd = "{service-principal-password}"

# Create a new Azure AD application
$azureAdApplication = New-AzureRmADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $pwd

# Create a new service principal associated with the designated application
New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role to the newly created service principal
New-AzureRmRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

Om du vill fråga Azure-Monitor API bör klientprogrammet använder tidigare skapade tjänstens huvudnamn för att autentisera. I följande exempel PowerShell-skript visas en närmar sig, med hjälp av den [Active Directory Authentication Library](../active-directory/active-directory-authentication-libraries.md) (ADAL) för att hämta autentiseringen JWT-token. JWT-token har skickats som en del av en HTTP-auktorisering parameter i begäranden till Azure-Monitor REST-API.

```PowerShell
$azureAdApplication = Get-AzureRmADApplication -IdentifierUri "https://localhost/azure-monitor"

$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId

$clientId = $azureAdApplication.ApplicationId.Guid
$tenantId = $subscription.TenantId
$authUrl = "https://login.microsoftonline.com/${tenantId}"

$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($clientId, $pwd)

$result = $AuthContext.AcquireToken("https://management.core.windows.net/", $cred)

# Build an array of HTTP header values
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

När steget autentisering installationen är klar, kan sedan frågor köras mot Azure övervakaren REST API. Det finns två användbara frågor:

1. Visa en lista med mått definitioner för en resurs
2. Hämta mått värden

## <a name="retrieve-metric-definitions"></a>Hämta Måttdefinitioner
> [!NOTE]
> Om du vill hämta måttdefinitioner med hjälp av REST API för Azure-Monitor Använd ”2016-03-01” som API-versionen.
>
>

```PowerShell
$apiVersion = "2016-03-01"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metricDefinitions?api-version=${apiVersion}"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -Verbose
```
För en Azure-Logikapp visas mått definitionerna liknar följande skärmbild:

![ALT ”JSON vy av mått definition svar”.](./media/monitoring-rest-api-walkthrough/available_metric_definitions_logic_app_json_response_clean.png)

Mer information finns i [listan mått definitioner för en resurs i REST-API för Azure-Monitor](https://msdn.microsoft.com/library/azure/mt743621.aspx) dokumentation.

## <a name="retrieve-metric-values"></a>Hämta måttvärden
När tillgängliga mått definitionerna är kända, har det möjligt att hämta relaterade måttvärden. Använd det mått name 'value' (inte den ' localizedValue') för alla begäranden om filtrering (till exempel hämtar 'CpuTime' och 'Begär' mått datapunkter). Om inga filter har angetts, returneras standardmått.

> [!NOTE]
> Om du vill hämta mått värden med hjälp av REST API för Azure-Monitor Använd ”2016-06-01” som API-versionen.
>
>

**Metoden**: hämta

**Begärande-URI**: https://management.azure.com/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/*{ resurs-provider-namespace}*/*{resurstypen}*/*{resursnamn}*/providers/microsoft.insights/metrics?$filter= *{filter}*& api-version =*{apiVersion}*

Till exempel för att hämta RunsSucceeded mått datapunkter för det angivna tidsintervallet och för ett tidskorn 1 timme skulle begäran vara följande:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2016-09-23 and endTime eq 2016-09-24 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

Resultatet ska se ut ungefär så följande skärmbild:

![ALT ”JSON-svar visar genomsnittlig svarstid värde”](./media/monitoring-rest-api-walkthrough/available_metrics_logic_app_json_response.png)

Om du vill hämta flera punkter för data eller aggregering, lägga till mått definition namn och typer för aggregering i filtret, som visas i följande exempel:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2016-09-23T13:30:00Z and endTime eq 2016-09-23T14:30:00Z and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

### <a name="use-armclient"></a>Använd ARMClient
Ett alternativ till att använda PowerShell (som visas ovan), är att använda [ARMClient](https://github.com/projectkudu/ARMClient) på din Windows-dator. ARMClient hanterar automatiskt Azure AD-autentisering (och resulterande JWT-token). Följande steg beskriver användningen av ARMClient för att hämta mätvärden:

1. Installera [Chocolatey](https://chocolatey.org/) och [ARMClient](https://github.com/projectkudu/ARMClient).
2. Ange i ett terminalfönster *armclient.exe inloggning*. Detta uppmanar dig att logga in på Azure.
3. Typen *armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01*
4. Typen *armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01*

![ALT ”Using ARMClient att arbeta med Azure övervakning REST-API”](./media/monitoring-rest-api-walkthrough/armclient_metricdefinitions.png)

## <a name="retrieve-the-resource-id"></a>Hämta resurs-ID
REST-API kan verkligen hjälp för att förstå tillgängliga mått definitioner och granularitet och relaterade värden. Att informationen är användbar när du använder den [Azure hanteringsmodul](https://msdn.microsoft.com/library/azure/mt417623.aspx).

Föregående kod är resurs-ID för att använda den fullständiga sökvägen till den önskade Azure-resursen. Om du vill fråga mot en Azure-Webbapp, till exempel är resurs-ID:

*/subscriptions/{Subscription-ID}/resourceGroups/{Resource-group-name}/providers/Microsoft.Web/Sites/{Site-Name}/*

Följande lista innehåller några exempel på resurs-ID-formaten för olika Azure-resurser:

* **IoT-hubb** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.Devices/IotHubs/*{iot-hubb-name}*
* **Elastisk SQL-poolen** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.Sql/servers/*{pool-db}*/elasticpools/*{sql-pool-name}*
* **SQL-databas (v12)** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.Sql/servers/*{servernamn}* /databases/*{databasnamnet}*
* **Service Bus** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.ServiceBus/*{namespace}* / *{servicebus-name}*
* **Skalningsuppsättningar** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.Compute/virtualMachineScaleSets/*{ VM-name}*
* **Virtuella datorer** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.Compute/virtualMachines/*{vm-name}*
* **Händelsehubbar** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.EventHub/namespaces/*{ eventhub-namespace}*

Det finns alternativa metoder för hämtning av resurs-ID, inklusive användning av resursutforskaren i Azure, visa en resurs i Azure-portalen och via PowerShell eller Azure CLI.

### <a name="azure-resource-explorer"></a>Azure Resource Explorer
För att hitta resurs-ID för en resurs, en bra metod är att använda den [resursutforskaren Azure](https://resources.azure.com) verktyget. Navigera till en resurs och titta på det ID som visas i följande skärmbild:

![ALT ”Azure Resursläsaren”](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a>Azure Portal
Resurs-ID kan också hämtas från Azure-portalen. Gör du genom att gå till en resurs och välj sedan Egenskaper. Resurs-ID visas i bladet egenskaper som visas i följande skärmbild:

![ALT ”resurs-ID som visas i bladet egenskaper i Azure portal”](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a>Azure PowerShell
Resurs-ID kan hämtas med hjälp av Azure PowerShell-cmdlets. Till exempel för att få resurs-ID för en Webbapp i Azure kan köra cmdlet Get-AzureRmWebApp, som i följande skärmbild:

![ALT ”resurs-ID erhållits via PowerShell”](./media/monitoring-rest-api-walkthrough/resourceid_powershell.png)

### <a name="azure-cli"></a>Azure CLI
Om du vill hämta resurs-ID med hjälp av Azure CLI kör du kommandot ”Visa azure webapp-anger den '--json-alternativ, som visas i följande skärmbild:

![ALT ”resurs-ID erhållits via PowerShell”](./media/monitoring-rest-api-walkthrough/resourceid_azurecli.png)

## <a name="retrieve-activity-log-data"></a>Hämta aktivitet loggdata
Förutom att arbeta med måttdefinitioner och andra relaterade värden är det också möjligt att hämta ytterligare intressanta insikter relaterade till Azure-resurser. Exempelvis är det möjligt att fråga [aktivitetsloggen](https://msdn.microsoft.com/library/azure/dn931934.aspx) data. I följande exempel visas hur du använder Azure övervakaren REST API för att fråga aktivitet loggdata inom ett visst datumintervall för en Azure-prenumeration:

```PowerShell
$apiVersion = "2014-04-01"
$filter = "eventTimestamp ge '2016-09-23' and eventTimestamp le '2016-09-24'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

## <a name="next-steps"></a>Nästa steg
* Granska de [översikt över övervakning](monitoring-overview.md).
* Visa den [stöds mått med Azure-Monitor](monitoring-supported-metrics.md).
* Granska de [övervaka REST API-referens för Microsoft Azure](https://msdn.microsoft.com/library/azure/dn931943.aspx).
* Granska de [Azure hanteringsmodul](https://msdn.microsoft.com/library/azure/mt417623.aspx).
