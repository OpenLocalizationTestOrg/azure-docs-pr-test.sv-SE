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
# <a name="azure-monitoring-rest-api-walkthrough"></a><span data-ttu-id="72a6d-103">Azure övervakning REST API genomgång</span><span class="sxs-lookup"><span data-stu-id="72a6d-103">Azure Monitoring REST API Walkthrough</span></span>
<span data-ttu-id="72a6d-104">Den här artikeln visar hur du autentisera så att din kod kan använda den [Microsoft Azure övervakaren REST API-referens](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="72a6d-104">This article shows you how to perform authentication so your code can use the [Microsoft Azure Monitor REST API Reference](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span>         

<span data-ttu-id="72a6d-105">Azure-Monitor API gör det möjligt att genom programmering hämta tillgängliga mått standarddefinitioner (typ av måttet som CPU-tid, begäranden, etc.), granularitet och måttvärden.</span><span class="sxs-lookup"><span data-stu-id="72a6d-105">The Azure Monitor API makes it possible to programmatically retrieve the available default metric definitions (the type of metric such as CPU Time, Requests, etc.), granularity, and metric values.</span></span> <span data-ttu-id="72a6d-106">När hämtas, kan du spara data i ett separat datalager som Azure SQL Database, Azure Cosmos DB eller Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="72a6d-106">Once retrieved, the data can be saved in a separate data store such as Azure SQL Database, Azure Cosmos DB, or Azure Data Lake.</span></span> <span data-ttu-id="72a6d-107">Därifrån kan du utföra ytterligare analys efter behov.</span><span class="sxs-lookup"><span data-stu-id="72a6d-107">From there additional analysis can be performed as needed.</span></span>

<span data-ttu-id="72a6d-108">Förutom att arbeta med olika mått datapunkter som den här artikeln visar gör övervakaren API det möjligt att lista Varningsregler, visa aktivitetsloggar och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="72a6d-108">Besides working with various metric data points, as this article demonstrates, the Monitor API makes it possible to list alert rules, view activity logs, and much more.</span></span> <span data-ttu-id="72a6d-109">En fullständig lista över tillgängliga åtgärder finns i [Microsoft Azure övervakaren REST API-referens](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="72a6d-109">For a full list of available operations, see the [Microsoft Azure Monitor REST API Reference](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span>

## <a name="authenticating-azure-monitor-requests"></a><span data-ttu-id="72a6d-110">Autentisering av Azure-Monitor förfrågningar</span><span class="sxs-lookup"><span data-stu-id="72a6d-110">Authenticating Azure Monitor Requests</span></span>
<span data-ttu-id="72a6d-111">Det första steget är att autentisera begäran.</span><span class="sxs-lookup"><span data-stu-id="72a6d-111">The first step is to authenticate the request.</span></span>

<span data-ttu-id="72a6d-112">Alla aktiviteter som körs mot Azure-Monitor API använder Azure Resource Manager autentisering modell.</span><span class="sxs-lookup"><span data-stu-id="72a6d-112">All the tasks executed against the Azure Monitor API use the Azure Resource Manager authentication model.</span></span> <span data-ttu-id="72a6d-113">Därför måste alla förfrågningar autentiseras med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="72a6d-113">Therefore, all requests must be authenticated with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="72a6d-114">En metod för att autentisera klientprogrammet är att skapa en Azure AD huvudnamn för tjänsten och hämta token för autentisering (JWT).</span><span class="sxs-lookup"><span data-stu-id="72a6d-114">One approach to authenticate the client application is to create an Azure AD service principal and retrieve the authentication (JWT) token.</span></span> <span data-ttu-id="72a6d-115">Följande exempelskript visar att skapa en Azure AD-tjänsten huvudnamn via PowerShell.</span><span class="sxs-lookup"><span data-stu-id="72a6d-115">The following sample script demonstrates creating an Azure AD service principal via PowerShell.</span></span> <span data-ttu-id="72a6d-116">För en mer detaljerad genomgång finns i dokumentationen på [med Azure PowerShell för att skapa ett huvudnamn för tjänsten att komma åt resurser](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-password).</span><span class="sxs-lookup"><span data-stu-id="72a6d-116">For a more detailed walk-through, refer to the documentation on [using Azure PowerShell to create a service principal to access resources](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-password).</span></span> <span data-ttu-id="72a6d-117">Det är också möjligt att [skapa ett huvudnamn för tjänsten via Azure portal](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="72a6d-117">It is also possible to [create a service principal via the Azure portal](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span>

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

<span data-ttu-id="72a6d-118">Om du vill fråga Azure-Monitor API bör klientprogrammet använder tidigare skapade tjänstens huvudnamn för att autentisera.</span><span class="sxs-lookup"><span data-stu-id="72a6d-118">To query the Azure Monitor API, the client application should use the previously created service principal to authenticate.</span></span> <span data-ttu-id="72a6d-119">I följande exempel PowerShell-skript visas en närmar sig, med hjälp av den [Active Directory Authentication Library](../active-directory/active-directory-authentication-libraries.md) (ADAL) för att hämta autentiseringen JWT-token.</span><span class="sxs-lookup"><span data-stu-id="72a6d-119">The following example PowerShell script shows one approach, using the [Active Directory Authentication Library](../active-directory/active-directory-authentication-libraries.md) (ADAL) to help get the JWT authentication token.</span></span> <span data-ttu-id="72a6d-120">JWT-token har skickats som en del av en HTTP-auktorisering parameter i begäranden till Azure-Monitor REST-API.</span><span class="sxs-lookup"><span data-stu-id="72a6d-120">The JWT token is passed as part of an HTTP Authorization parameter in requests to the Azure Monitor REST API.</span></span>

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

<span data-ttu-id="72a6d-121">När steget autentisering installationen är klar, kan sedan frågor köras mot Azure övervakaren REST API.</span><span class="sxs-lookup"><span data-stu-id="72a6d-121">Once the authentication setup step is complete, queries can then be executed against the Azure Monitor REST API.</span></span> <span data-ttu-id="72a6d-122">Det finns två användbara frågor:</span><span class="sxs-lookup"><span data-stu-id="72a6d-122">There are two helpful queries:</span></span>

1. <span data-ttu-id="72a6d-123">Visa en lista med mått definitioner för en resurs</span><span class="sxs-lookup"><span data-stu-id="72a6d-123">List the metric definitions for a resource</span></span>
2. <span data-ttu-id="72a6d-124">Hämta mått värden</span><span class="sxs-lookup"><span data-stu-id="72a6d-124">Retrieve the metric values</span></span>

## <a name="retrieve-metric-definitions"></a><span data-ttu-id="72a6d-125">Hämta Måttdefinitioner</span><span class="sxs-lookup"><span data-stu-id="72a6d-125">Retrieve Metric Definitions</span></span>
> [!NOTE]
> <span data-ttu-id="72a6d-126">Om du vill hämta måttdefinitioner med hjälp av REST API för Azure-Monitor Använd ”2016-03-01” som API-versionen.</span><span class="sxs-lookup"><span data-stu-id="72a6d-126">To retrieve metric definitions using the Azure Monitor REST API, use "2016-03-01" as the API version.</span></span>
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
<span data-ttu-id="72a6d-127">För en Azure-Logikapp visas mått definitionerna liknar följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="72a6d-127">For an Azure Logic App, the metric definitions would appear similar to the following screenshot:</span></span>

![ALT ”JSON vy av mått definition svar”.](./media/monitoring-rest-api-walkthrough/available_metric_definitions_logic_app_json_response_clean.png)

<span data-ttu-id="72a6d-129">Mer information finns i [listan mått definitioner för en resurs i REST-API för Azure-Monitor](https://msdn.microsoft.com/library/azure/mt743621.aspx) dokumentation.</span><span class="sxs-lookup"><span data-stu-id="72a6d-129">For more information, see the [List the metric definitions for a resource in Azure Monitor REST API](https://msdn.microsoft.com/library/azure/mt743621.aspx) documentation.</span></span>

## <a name="retrieve-metric-values"></a><span data-ttu-id="72a6d-130">Hämta måttvärden</span><span class="sxs-lookup"><span data-stu-id="72a6d-130">Retrieve Metric Values</span></span>
<span data-ttu-id="72a6d-131">När tillgängliga mått definitionerna är kända, har det möjligt att hämta relaterade måttvärden.</span><span class="sxs-lookup"><span data-stu-id="72a6d-131">Once the available metric definitions are known, it is then possible to retrieve the related metric values.</span></span> <span data-ttu-id="72a6d-132">Använd det mått name 'value' (inte den ' localizedValue') för alla begäranden om filtrering (till exempel hämtar 'CpuTime' och 'Begär' mått datapunkter).</span><span class="sxs-lookup"><span data-stu-id="72a6d-132">Use the metric’s name ‘value’ (not the ‘localizedValue’) for any filtering requests (for example, retrieve the ‘CpuTime’ and ‘Requests’ metric data points).</span></span> <span data-ttu-id="72a6d-133">Om inga filter har angetts, returneras standardmått.</span><span class="sxs-lookup"><span data-stu-id="72a6d-133">If no filters are specified, the default metric is returned.</span></span>

> [!NOTE]
> <span data-ttu-id="72a6d-134">Om du vill hämta mått värden med hjälp av REST API för Azure-Monitor Använd ”2016-06-01” som API-versionen.</span><span class="sxs-lookup"><span data-stu-id="72a6d-134">To retrieve metric values using the Azure Monitor REST API, use "2016-06-01" as the API version.</span></span>
>
>

<span data-ttu-id="72a6d-135">**Metoden**: hämta</span><span class="sxs-lookup"><span data-stu-id="72a6d-135">**Method**: GET</span></span>

<span data-ttu-id="72a6d-136">**Begärande-URI**: https://management.azure.com/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/*{ resurs-provider-namespace}*/*{resurstypen}*/*{resursnamn}*/providers/microsoft.insights/metrics?$filter= *{filter}*& api-version =*{apiVersion}*</span><span class="sxs-lookup"><span data-stu-id="72a6d-136">**Request URI**: https://management.azure.com/subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/*{resource-provider-namespace}*/*{resource-type}*/*{resource-name}*/providers/microsoft.insights/metrics?$filter=*{filter}*&api-version=*{apiVersion}*</span></span>

<span data-ttu-id="72a6d-137">Till exempel för att hämta RunsSucceeded mått datapunkter för det angivna tidsintervallet och för ett tidskorn 1 timme skulle begäran vara följande:</span><span class="sxs-lookup"><span data-stu-id="72a6d-137">For example, to retrieve the RunsSucceeded metric data points for the given time range and for a time grain of 1 hour, the request would be as follows:</span></span>

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2016-09-23 and endTime eq 2016-09-24 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

<span data-ttu-id="72a6d-138">Resultatet ska se ut ungefär så följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="72a6d-138">The result would appear similar to the example following screenshot:</span></span>

![ALT ”JSON-svar visar genomsnittlig svarstid värde”](./media/monitoring-rest-api-walkthrough/available_metrics_logic_app_json_response.png)

<span data-ttu-id="72a6d-140">Om du vill hämta flera punkter för data eller aggregering, lägga till mått definition namn och typer för aggregering i filtret, som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="72a6d-140">To retrieve multiple data or aggregation points, add the metric definition names and aggregation types to the filter, as seen in the following example:</span></span>

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2016-09-23T13:30:00Z and endTime eq 2016-09-23T14:30:00Z and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

### <a name="use-armclient"></a><span data-ttu-id="72a6d-141">Använd ARMClient</span><span class="sxs-lookup"><span data-stu-id="72a6d-141">Use ARMClient</span></span>
<span data-ttu-id="72a6d-142">Ett alternativ till att använda PowerShell (som visas ovan), är att använda [ARMClient](https://github.com/projectkudu/ARMClient) på din Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="72a6d-142">An alternative to using PowerShell (as shown above), is to use [ARMClient](https://github.com/projectkudu/ARMClient) on your Windows machine.</span></span> <span data-ttu-id="72a6d-143">ARMClient hanterar automatiskt Azure AD-autentisering (och resulterande JWT-token).</span><span class="sxs-lookup"><span data-stu-id="72a6d-143">ARMClient handles the Azure AD authentication (and resulting JWT token) automatically.</span></span> <span data-ttu-id="72a6d-144">Följande steg beskriver användningen av ARMClient för att hämta mätvärden:</span><span class="sxs-lookup"><span data-stu-id="72a6d-144">The following steps outline use of ARMClient for retrieving metric data:</span></span>

1. <span data-ttu-id="72a6d-145">Installera [Chocolatey](https://chocolatey.org/) och [ARMClient](https://github.com/projectkudu/ARMClient).</span><span class="sxs-lookup"><span data-stu-id="72a6d-145">Install [Chocolatey](https://chocolatey.org/) and [ARMClient](https://github.com/projectkudu/ARMClient).</span></span>
2. <span data-ttu-id="72a6d-146">Ange i ett terminalfönster *armclient.exe inloggning*.</span><span class="sxs-lookup"><span data-stu-id="72a6d-146">In a terminal window, type *armclient.exe login*.</span></span> <span data-ttu-id="72a6d-147">Detta uppmanar dig att logga in på Azure.</span><span class="sxs-lookup"><span data-stu-id="72a6d-147">This prompts you to log in to Azure.</span></span>
3. <span data-ttu-id="72a6d-148">Typen *armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01*</span><span class="sxs-lookup"><span data-stu-id="72a6d-148">Type *armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01*</span></span>
4. <span data-ttu-id="72a6d-149">Typen *armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01*</span><span class="sxs-lookup"><span data-stu-id="72a6d-149">Type *armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01*</span></span>

![ALT ”Using ARMClient att arbeta med Azure övervakning REST-API”](./media/monitoring-rest-api-walkthrough/armclient_metricdefinitions.png)

## <a name="retrieve-the-resource-id"></a><span data-ttu-id="72a6d-151">Hämta resurs-ID</span><span class="sxs-lookup"><span data-stu-id="72a6d-151">Retrieve the Resource ID</span></span>
<span data-ttu-id="72a6d-152">REST-API kan verkligen hjälp för att förstå tillgängliga mått definitioner och granularitet och relaterade värden.</span><span class="sxs-lookup"><span data-stu-id="72a6d-152">Using the REST API can really help to understand the available metric definitions, granularity, and related values.</span></span> <span data-ttu-id="72a6d-153">Att informationen är användbar när du använder den [Azure hanteringsmodul](https://msdn.microsoft.com/library/azure/mt417623.aspx).</span><span class="sxs-lookup"><span data-stu-id="72a6d-153">That information is helpful when using the [Azure Management Library](https://msdn.microsoft.com/library/azure/mt417623.aspx).</span></span>

<span data-ttu-id="72a6d-154">Föregående kod är resurs-ID för att använda den fullständiga sökvägen till den önskade Azure-resursen.</span><span class="sxs-lookup"><span data-stu-id="72a6d-154">For the preceding code, the resource ID to use is the full path to the desired Azure resource.</span></span> <span data-ttu-id="72a6d-155">Om du vill fråga mot en Azure-Webbapp, till exempel är resurs-ID:</span><span class="sxs-lookup"><span data-stu-id="72a6d-155">For example, to query against an Azure Web App, the resource ID would be:</span></span>

<span data-ttu-id="72a6d-156">*/subscriptions/{Subscription-ID}/resourceGroups/{Resource-group-name}/providers/Microsoft.Web/Sites/{Site-Name}/*</span><span class="sxs-lookup"><span data-stu-id="72a6d-156">*/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{site-name}/*</span></span>

<span data-ttu-id="72a6d-157">Följande lista innehåller några exempel på resurs-ID-formaten för olika Azure-resurser:</span><span class="sxs-lookup"><span data-stu-id="72a6d-157">The following list contains a few examples of resource ID formats for various Azure resources:</span></span>

* <span data-ttu-id="72a6d-158">**IoT-hubb** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.Devices/IotHubs/*{iot-hubb-name}*</span><span class="sxs-lookup"><span data-stu-id="72a6d-158">**IoT Hub** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Devices/IotHubs/*{iot-hub-name}*</span></span>
* <span data-ttu-id="72a6d-159">**Elastisk SQL-poolen** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.Sql/servers/*{pool-db}*/elasticpools/*{sql-pool-name}*</span><span class="sxs-lookup"><span data-stu-id="72a6d-159">**Elastic SQL Pool** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Sql/servers/*{pool-db}*/elasticpools/*{sql-pool-name}*</span></span>
* <span data-ttu-id="72a6d-160">**SQL-databas (v12)** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.Sql/servers/*{servernamn}* /databases/*{databasnamnet}*</span><span class="sxs-lookup"><span data-stu-id="72a6d-160">**SQL Database (v12)** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Sql/servers/*{server-name}*/databases/*{database-name}*</span></span>
* <span data-ttu-id="72a6d-161">**Service Bus** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.ServiceBus/*{namespace}* / *{servicebus-name}*</span><span class="sxs-lookup"><span data-stu-id="72a6d-161">**Service Bus** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.ServiceBus/*{namespace}*/*{servicebus-name}*</span></span>
* <span data-ttu-id="72a6d-162">**Skalningsuppsättningar** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.Compute/virtualMachineScaleSets/*{ VM-name}*</span><span class="sxs-lookup"><span data-stu-id="72a6d-162">**VM Scale Sets** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Compute/virtualMachineScaleSets/*{vm-name}*</span></span>
* <span data-ttu-id="72a6d-163">**Virtuella datorer** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.Compute/virtualMachines/*{vm-name}*</span><span class="sxs-lookup"><span data-stu-id="72a6d-163">**VMs** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Compute/virtualMachines/*{vm-name}*</span></span>
* <span data-ttu-id="72a6d-164">**Händelsehubbar** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.EventHub/namespaces/*{ eventhub-namespace}*</span><span class="sxs-lookup"><span data-stu-id="72a6d-164">**Event Hubs** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.EventHub/namespaces/*{eventhub-namespace}*</span></span>

<span data-ttu-id="72a6d-165">Det finns alternativa metoder för hämtning av resurs-ID, inklusive användning av resursutforskaren i Azure, visa en resurs i Azure-portalen och via PowerShell eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="72a6d-165">There are alternative approaches to retrieving the resource ID, including using Azure Resource Explorer, viewing the desired resource in the Azure portal, and via PowerShell or the Azure CLI.</span></span>

### <a name="azure-resource-explorer"></a><span data-ttu-id="72a6d-166">Azure Resource Explorer</span><span class="sxs-lookup"><span data-stu-id="72a6d-166">Azure Resource Explorer</span></span>
<span data-ttu-id="72a6d-167">För att hitta resurs-ID för en resurs, en bra metod är att använda den [resursutforskaren Azure](https://resources.azure.com) verktyget.</span><span class="sxs-lookup"><span data-stu-id="72a6d-167">To find the resource ID for a desired resource, one helpful approach is to use the [Azure Resource Explorer](https://resources.azure.com) tool.</span></span> <span data-ttu-id="72a6d-168">Navigera till en resurs och titta på det ID som visas i följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="72a6d-168">Navigate to the desired resource and then look at the ID shown, as in the following screenshot:</span></span>

![ALT ”Azure Resursläsaren”](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a><span data-ttu-id="72a6d-170">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="72a6d-170">Azure portal</span></span>
<span data-ttu-id="72a6d-171">Resurs-ID kan också hämtas från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="72a6d-171">The resource ID can also be obtained from the Azure portal.</span></span> <span data-ttu-id="72a6d-172">Gör du genom att gå till en resurs och välj sedan Egenskaper.</span><span class="sxs-lookup"><span data-stu-id="72a6d-172">To do so, navigate to the desired resource and then select Properties.</span></span> <span data-ttu-id="72a6d-173">Resurs-ID visas i bladet egenskaper som visas i följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="72a6d-173">The Resource ID is displayed in the Properties blade, as seen in the following screenshot:</span></span>

![ALT ”resurs-ID som visas i bladet egenskaper i Azure portal”](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a><span data-ttu-id="72a6d-175">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="72a6d-175">Azure PowerShell</span></span>
<span data-ttu-id="72a6d-176">Resurs-ID kan hämtas med hjälp av Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="72a6d-176">The resource ID can be retrieved using Azure PowerShell cmdlets as well.</span></span> <span data-ttu-id="72a6d-177">Till exempel för att få resurs-ID för en Webbapp i Azure kan köra cmdlet Get-AzureRmWebApp, som i följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="72a6d-177">For example, to obtain the resource ID for an Azure Web App, execute the Get-AzureRmWebApp cmdlet, as in the following screenshot:</span></span>

![ALT ”resurs-ID erhållits via PowerShell”](./media/monitoring-rest-api-walkthrough/resourceid_powershell.png)

### <a name="azure-cli"></a><span data-ttu-id="72a6d-179">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="72a6d-179">Azure CLI</span></span>
<span data-ttu-id="72a6d-180">Om du vill hämta resurs-ID med hjälp av Azure CLI kör du kommandot ”Visa azure webapp-anger den '--json-alternativ, som visas i följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="72a6d-180">To retrieve the resource ID using the Azure CLI, execute the 'azure webapp show' command, specifying the '--json' option, as shown in the following screenshot:</span></span>

![ALT ”resurs-ID erhållits via PowerShell”](./media/monitoring-rest-api-walkthrough/resourceid_azurecli.png)

## <a name="retrieve-activity-log-data"></a><span data-ttu-id="72a6d-182">Hämta aktivitet loggdata</span><span class="sxs-lookup"><span data-stu-id="72a6d-182">Retrieve Activity Log Data</span></span>
<span data-ttu-id="72a6d-183">Förutom att arbeta med måttdefinitioner och andra relaterade värden är det också möjligt att hämta ytterligare intressanta insikter relaterade till Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="72a6d-183">In addition to working with metric definitions and related values, it is also possible to retrieve additional interesting insights related to Azure resources.</span></span> <span data-ttu-id="72a6d-184">Exempelvis är det möjligt att fråga [aktivitetsloggen](https://msdn.microsoft.com/library/azure/dn931934.aspx) data.</span><span class="sxs-lookup"><span data-stu-id="72a6d-184">As an example, it is possible to query [activity log](https://msdn.microsoft.com/library/azure/dn931934.aspx) data.</span></span> <span data-ttu-id="72a6d-185">I följande exempel visas hur du använder Azure övervakaren REST API för att fråga aktivitet loggdata inom ett visst datumintervall för en Azure-prenumeration:</span><span class="sxs-lookup"><span data-stu-id="72a6d-185">The following sample demonstrates using the Azure Monitor REST API to query activity log data within a specific date range for an Azure subscription:</span></span>

```PowerShell
$apiVersion = "2014-04-01"
$filter = "eventTimestamp ge '2016-09-23' and eventTimestamp le '2016-09-24'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

## <a name="next-steps"></a><span data-ttu-id="72a6d-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="72a6d-186">Next steps</span></span>
* <span data-ttu-id="72a6d-187">Granska de [översikt över övervakning](monitoring-overview.md).</span><span class="sxs-lookup"><span data-stu-id="72a6d-187">Review the [Overview of Monitoring](monitoring-overview.md).</span></span>
* <span data-ttu-id="72a6d-188">Visa den [stöds mått med Azure-Monitor](monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="72a6d-188">View the [Supported metrics with Azure Monitor](monitoring-supported-metrics.md).</span></span>
* <span data-ttu-id="72a6d-189">Granska de [övervaka REST API-referens för Microsoft Azure](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="72a6d-189">Review the [Microsoft Azure Monitor REST API Reference](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span>
* <span data-ttu-id="72a6d-190">Granska de [Azure hanteringsmodul](https://msdn.microsoft.com/library/azure/mt417623.aspx).</span><span class="sxs-lookup"><span data-stu-id="72a6d-190">Review the [Azure Management Library](https://msdn.microsoft.com/library/azure/mt417623.aspx).</span></span>
