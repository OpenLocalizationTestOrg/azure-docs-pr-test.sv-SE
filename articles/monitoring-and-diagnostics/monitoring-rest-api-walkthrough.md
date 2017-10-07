---
title: "aaaAzure genomgången övervakning REST API | Microsoft Docs"
description: "Hur begär tooauthenticate tooand Använd hello Azure övervakning REST API."
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
ms.openlocfilehash: b8ae3a03fd21af872f1dc5fed40a101a24ca1652
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-monitoring-rest-api-walkthrough"></a>Azure övervakning REST API genomgång
Den här artikeln beskrivs hur du tooperform autentisering så att din kod kan använda hello [Microsoft Azure övervakaren REST API-referens](https://msdn.microsoft.com/library/azure/dn931943.aspx).         

hello Azure API för övervakaren gör det möjligt tooprogrammatically hämta hello tillgängliga mått standarddefinitioner (hello typ av måttet som CPU-tid, begäranden, etc.) och granularitet måttvärden. När hämtas, sparas hello data i ett separat datalager som Azure SQL Database, Azure Cosmos DB eller Azure Data Lake. Därifrån kan du utföra ytterligare analys efter behov.

Förutom att arbeta med olika mått datapunkter som den här artikeln visar gör hello övervakaren API det möjligt toolist Varningsregler, visa aktivitetsloggar och mycket mer. En fullständig lista över tillgängliga åtgärder finns i hello [Microsoft Azure övervakaren REST API-referens](https://msdn.microsoft.com/library/azure/dn931943.aspx).

## <a name="authenticating-azure-monitor-requests"></a>Autentisering av Azure-Monitor förfrågningar
hello första steget är tooauthenticate hello begäran.

Alla hello aktiviteter körs mot hello Azure övervakaren API Använd hello Azure Resource Manager autentisering modellen. Därför måste alla förfrågningar autentiseras med Azure Active Directory (AD Azure). En metod tooauthenticate hello klientprogrammet är toocreate ett huvudnamn för Azure AD-tjänsten och hämta hello-autentiseringstoken (JWT). hello visar följande exempelskript och skapa en Azure AD tjänstens huvudnamn via PowerShell. En mer detaljerad genomgång finns i dokumentationen för toohello på [med hjälp av Azure PowerShell toocreate en service principal tooaccess resurser](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-password). Det är också möjligt för[skapa ett huvudnamn för tjänsten via hello Azure-portalen](../azure-resource-manager/resource-group-create-service-principal-portal.md).

```PowerShell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"
$location = "centralus"

# Authenticate tooa specific Azure subscription.
Login-AzureRmAccount -SubscriptionId $subscriptionId

# Password for hello service principal
$pwd = "{service-principal-password}"

# Create a new Azure AD application
$azureAdApplication = New-AzureRmADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $pwd

# Create a new service principal associated with hello designated application
New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role toohello newly created service principal
New-AzureRmRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

tooquery hello Azure övervakaren API, hello klientprogram ska använda hello tidigare skapade service principal tooauthenticate. hello följande exempel PowerShell-skriptet visar en metod som använder hello [Active Directory Authentication Library](../active-directory/active-directory-authentication-libraries.md) (ADAL) toohelp hämta hello JWT autentisering token. Hej JWT-token har skickats som en del av en HTTP-auktorisering parameter i begäranden toohello Azure övervakaren REST API.

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

När hello autentisering steget av installationen är klar, kan sedan frågor köras mot hello Azure övervakaren REST API. Det finns två användbara frågor:

1. Lista hello måttdefinitionerna för en resurs
2. Hämta hello måttvärden

## <a name="retrieve-metric-definitions"></a>Hämta Måttdefinitioner
> [!NOTE]
> tooretrieve måttdefinitioner med hello Azure övervakaren REST API använder ”2016-03-01” som hello API-versionen.
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
För en Azure-Logikapp visas hello måttdefinitioner liknande toohello följande skärmbild:

![ALT ”JSON vy av mått definition svar”.](./media/monitoring-rest-api-walkthrough/available_metric_definitions_logic_app_json_response_clean.png)

Mer information finns i hello [listan hello måttdefinitionerna för en resurs i REST-API för Azure-Monitor](https://msdn.microsoft.com/library/azure/mt743621.aspx) dokumentation.

## <a name="retrieve-metric-values"></a>Hämta måttvärden
När hello definitioner av tillgängliga mått är kända, är det sedan möjligt tooretrieve hello relaterade måttvärden. Använd hello mått name 'value' (inte hello 'localizedValue') för alla filtrera förfrågningar (till exempel hämta hello 'CpuTime' och 'Begär' mått datapunkter). Om inga filter har angetts, returneras hello standardmått.

> [!NOTE]
> tooretrieve måttvärden med hello Azure övervakaren REST API använder ”2016-06-01” som hello API-versionen.
>
>

**Metoden**: hämta

**Begärande-URI**: https://management.azure.com/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/*{ resurs-provider-namespace}*/*{resurstypen}*/*{resursnamn}*/providers/microsoft.insights/metrics?$filter= *{filter}*& api-version =*{apiVersion}*

Tooretrieve hello RunsSucceeded mått datapunkter för hello angivna tidsintervall och en tidskornet på 1 timme, hello begäran skulle till exempel vara följande:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2016-09-23 and endTime eq 2016-09-24 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

hello resultatet skulle visas liknande toohello exempel följande skärmbild:

![ALT ”JSON-svar visar genomsnittlig svarstid värde”](./media/monitoring-rest-api-walkthrough/available_metrics_logic_app_json_response.png)

tooretrieve flera data eller aggregering datapunkter, lägga till hello mått definition namn och aggregering typer toohello filter som visas i följande exempel hello:

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
Ett alternativt toousing PowerShell (som visas ovan), är toouse [ARMClient](https://github.com/projectkudu/ARMClient) på din Windows-dator. ARMClient hanterar automatiskt hello Azure AD-autentisering (och resulterande JWT-token). hello beskriver följande steg användningen av ARMClient för att hämta mätvärden:

1. Installera [Chocolatey](https://chocolatey.org/) och [ARMClient](https://github.com/projectkudu/ARMClient).
2. Ange i ett terminalfönster *armclient.exe inloggning*. Detta uppmanar dig toolog i tooAzure.
3. Typen *armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01*
4. Typen *armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01*

![ALT ”Using ARMClient toowork med hello Azure övervakning REST-API”](./media/monitoring-rest-api-walkthrough/armclient_metricdefinitions.png)

## <a name="retrieve-hello-resource-id"></a>Hämta hello resurs-ID
Med hjälp av hello REST API hjälper verkligen toounderstand hello tillgängliga mått definitioner, granularitet och relaterade värden. Informationen är användbar när du använder hello [Azure hanteringsmodul](https://msdn.microsoft.com/library/azure/mt417623.aspx).

Hello resurs-ID toouse är hello föregående kod, hello fullständig sökväg toohello önskad Azure-resurs. Till exempel tooquery mot en Azure-Webbapp hello resurs-ID är:

*/subscriptions/{Subscription-ID}/resourceGroups/{Resource-group-name}/providers/Microsoft.Web/Sites/{Site-Name}/*

hello innehåller följande lista några exempel på resurs-ID-formaten för olika Azure-resurser:

* **IoT-hubb** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.Devices/IotHubs/*{iot-hubb-name}*
* **Elastisk SQL-poolen** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.Sql/servers/*{pool-db}*/elasticpools/*{sql-pool-name}*
* **SQL-databas (v12)** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.Sql/servers/*{servernamn}* /databases/*{databasnamnet}*
* **Service Bus** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.ServiceBus/*{namespace}* / *{servicebus-name}*
* **Skalningsuppsättningar** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.Compute/virtualMachineScaleSets/*{ VM-name}*
* **Virtuella datorer** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.Compute/virtualMachines/*{vm-name}*
* **Händelsehubbar** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.EventHub/namespaces/*{ eventhub-namespace}*

Det finns alternativa metoder tooretrieving hello resurs-ID, inklusive användning av Azure Resursläsaren visar hello önskad resurs i hello Azure-portalen och via PowerShell eller hello Azure CLI.

### <a name="azure-resource-explorer"></a>Azure Resource Explorer
toofind hello resurs-ID för en resurs, en bra metod är toouse hello [resursutforskaren Azure](https://resources.azure.com) verktyget. Navigera toohello önskad resurs och titta på hello-ID som visas i följande skärmbild hello:

![ALT ”Azure Resursläsaren”](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a>Azure Portal
hello resurs-ID kan också hämtas från hello Azure-portalen. toodo så navigera toohello önskad resurs och välj sedan Egenskaper. hello resurs-ID visas i hello egenskapsbladet som visas i följande skärmbild hello:

![ALT ”resurs-ID visas i hello egenskapsbladet i hello Azure-portalen”](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a>Azure PowerShell
hello resurs-ID kan hämtas med hjälp av Azure PowerShell-cmdlets. Till exempel köra tooobtain hello resurs-ID för en Azure-Webbapp hello Get-AzureRmWebApp cmdlet, som i följande skärmbild hello:

![ALT ”resurs-ID erhållits via PowerShell”](./media/monitoring-rest-api-walkthrough/resourceid_powershell.png)

### <a name="azure-cli"></a>Azure CLI
tooretrieve Hej resurs-ID med hjälp av hello Azure CLI, köra hello 'azure webapp visa'-kommando och ange hello '--json-alternativ som visas i följande skärmbild hello:

![ALT ”resurs-ID erhållits via PowerShell”](./media/monitoring-rest-api-walkthrough/resourceid_azurecli.png)

## <a name="retrieve-activity-log-data"></a>Hämta aktivitet loggdata
I tillägg tooworking med måttdefinitioner och andra relaterade värden är det också möjligt tooretrieve ytterligare intressanta insikter relaterade tooAzure resurser. Exempelvis är det möjligt tooquery [aktivitetsloggen](https://msdn.microsoft.com/library/azure/dn931934.aspx) data. hello följande exempel visas hur du använder loggdata för hello Azure övervakaren REST API tooquery aktiviteten inom ett visst datumintervall för en Azure-prenumeration:

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
* Granska hello [översikt av övervakning](monitoring-overview.md).
* Visa hello [stöds mått med Azure-Monitor](monitoring-supported-metrics.md).
* Granska hello [Microsoft Azure övervakaren REST API-referens](https://msdn.microsoft.com/library/azure/dn931943.aspx).
* Granska hello [Azure hanteringsmodul](https://msdn.microsoft.com/library/azure/mt417623.aspx).
