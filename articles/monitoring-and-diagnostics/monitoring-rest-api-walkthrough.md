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
# <a name="azure-monitoring-rest-api-walkthrough"></a><span data-ttu-id="92220-103">Azure övervakning REST API genomgång</span><span class="sxs-lookup"><span data-stu-id="92220-103">Azure Monitoring REST API Walkthrough</span></span>
<span data-ttu-id="92220-104">Den här artikeln beskrivs hur du tooperform autentisering så att din kod kan använda hello [Microsoft Azure övervakaren REST API-referens](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="92220-104">This article shows you how tooperform authentication so your code can use hello [Microsoft Azure Monitor REST API Reference](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span>         

<span data-ttu-id="92220-105">hello Azure API för övervakaren gör det möjligt tooprogrammatically hämta hello tillgängliga mått standarddefinitioner (hello typ av måttet som CPU-tid, begäranden, etc.) och granularitet måttvärden.</span><span class="sxs-lookup"><span data-stu-id="92220-105">hello Azure Monitor API makes it possible tooprogrammatically retrieve hello available default metric definitions (hello type of metric such as CPU Time, Requests, etc.), granularity, and metric values.</span></span> <span data-ttu-id="92220-106">När hämtas, sparas hello data i ett separat datalager som Azure SQL Database, Azure Cosmos DB eller Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="92220-106">Once retrieved, hello data can be saved in a separate data store such as Azure SQL Database, Azure Cosmos DB, or Azure Data Lake.</span></span> <span data-ttu-id="92220-107">Därifrån kan du utföra ytterligare analys efter behov.</span><span class="sxs-lookup"><span data-stu-id="92220-107">From there additional analysis can be performed as needed.</span></span>

<span data-ttu-id="92220-108">Förutom att arbeta med olika mått datapunkter som den här artikeln visar gör hello övervakaren API det möjligt toolist Varningsregler, visa aktivitetsloggar och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="92220-108">Besides working with various metric data points, as this article demonstrates, hello Monitor API makes it possible toolist alert rules, view activity logs, and much more.</span></span> <span data-ttu-id="92220-109">En fullständig lista över tillgängliga åtgärder finns i hello [Microsoft Azure övervakaren REST API-referens](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="92220-109">For a full list of available operations, see hello [Microsoft Azure Monitor REST API Reference](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span>

## <a name="authenticating-azure-monitor-requests"></a><span data-ttu-id="92220-110">Autentisering av Azure-Monitor förfrågningar</span><span class="sxs-lookup"><span data-stu-id="92220-110">Authenticating Azure Monitor Requests</span></span>
<span data-ttu-id="92220-111">hello första steget är tooauthenticate hello begäran.</span><span class="sxs-lookup"><span data-stu-id="92220-111">hello first step is tooauthenticate hello request.</span></span>

<span data-ttu-id="92220-112">Alla hello aktiviteter körs mot hello Azure övervakaren API Använd hello Azure Resource Manager autentisering modellen.</span><span class="sxs-lookup"><span data-stu-id="92220-112">All hello tasks executed against hello Azure Monitor API use hello Azure Resource Manager authentication model.</span></span> <span data-ttu-id="92220-113">Därför måste alla förfrågningar autentiseras med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="92220-113">Therefore, all requests must be authenticated with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="92220-114">En metod tooauthenticate hello klientprogrammet är toocreate ett huvudnamn för Azure AD-tjänsten och hämta hello-autentiseringstoken (JWT).</span><span class="sxs-lookup"><span data-stu-id="92220-114">One approach tooauthenticate hello client application is toocreate an Azure AD service principal and retrieve hello authentication (JWT) token.</span></span> <span data-ttu-id="92220-115">hello visar följande exempelskript och skapa en Azure AD tjänstens huvudnamn via PowerShell.</span><span class="sxs-lookup"><span data-stu-id="92220-115">hello following sample script demonstrates creating an Azure AD service principal via PowerShell.</span></span> <span data-ttu-id="92220-116">En mer detaljerad genomgång finns i dokumentationen för toohello på [med hjälp av Azure PowerShell toocreate en service principal tooaccess resurser](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-password).</span><span class="sxs-lookup"><span data-stu-id="92220-116">For a more detailed walk-through, refer toohello documentation on [using Azure PowerShell toocreate a service principal tooaccess resources](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-password).</span></span> <span data-ttu-id="92220-117">Det är också möjligt för[skapa ett huvudnamn för tjänsten via hello Azure-portalen](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="92220-117">It is also possible too[create a service principal via hello Azure portal](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span>

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

<span data-ttu-id="92220-118">tooquery hello Azure övervakaren API, hello klientprogram ska använda hello tidigare skapade service principal tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="92220-118">tooquery hello Azure Monitor API, hello client application should use hello previously created service principal tooauthenticate.</span></span> <span data-ttu-id="92220-119">hello följande exempel PowerShell-skriptet visar en metod som använder hello [Active Directory Authentication Library](../active-directory/active-directory-authentication-libraries.md) (ADAL) toohelp hämta hello JWT autentisering token.</span><span class="sxs-lookup"><span data-stu-id="92220-119">hello following example PowerShell script shows one approach, using hello [Active Directory Authentication Library](../active-directory/active-directory-authentication-libraries.md) (ADAL) toohelp get hello JWT authentication token.</span></span> <span data-ttu-id="92220-120">Hej JWT-token har skickats som en del av en HTTP-auktorisering parameter i begäranden toohello Azure övervakaren REST API.</span><span class="sxs-lookup"><span data-stu-id="92220-120">hello JWT token is passed as part of an HTTP Authorization parameter in requests toohello Azure Monitor REST API.</span></span>

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

<span data-ttu-id="92220-121">När hello autentisering steget av installationen är klar, kan sedan frågor köras mot hello Azure övervakaren REST API.</span><span class="sxs-lookup"><span data-stu-id="92220-121">Once hello authentication setup step is complete, queries can then be executed against hello Azure Monitor REST API.</span></span> <span data-ttu-id="92220-122">Det finns två användbara frågor:</span><span class="sxs-lookup"><span data-stu-id="92220-122">There are two helpful queries:</span></span>

1. <span data-ttu-id="92220-123">Lista hello måttdefinitionerna för en resurs</span><span class="sxs-lookup"><span data-stu-id="92220-123">List hello metric definitions for a resource</span></span>
2. <span data-ttu-id="92220-124">Hämta hello måttvärden</span><span class="sxs-lookup"><span data-stu-id="92220-124">Retrieve hello metric values</span></span>

## <a name="retrieve-metric-definitions"></a><span data-ttu-id="92220-125">Hämta Måttdefinitioner</span><span class="sxs-lookup"><span data-stu-id="92220-125">Retrieve Metric Definitions</span></span>
> [!NOTE]
> <span data-ttu-id="92220-126">tooretrieve måttdefinitioner med hello Azure övervakaren REST API använder ”2016-03-01” som hello API-versionen.</span><span class="sxs-lookup"><span data-stu-id="92220-126">tooretrieve metric definitions using hello Azure Monitor REST API, use "2016-03-01" as hello API version.</span></span>
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
<span data-ttu-id="92220-127">För en Azure-Logikapp visas hello måttdefinitioner liknande toohello följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="92220-127">For an Azure Logic App, hello metric definitions would appear similar toohello following screenshot:</span></span>

![ALT ”JSON vy av mått definition svar”.](./media/monitoring-rest-api-walkthrough/available_metric_definitions_logic_app_json_response_clean.png)

<span data-ttu-id="92220-129">Mer information finns i hello [listan hello måttdefinitionerna för en resurs i REST-API för Azure-Monitor](https://msdn.microsoft.com/library/azure/mt743621.aspx) dokumentation.</span><span class="sxs-lookup"><span data-stu-id="92220-129">For more information, see hello [List hello metric definitions for a resource in Azure Monitor REST API](https://msdn.microsoft.com/library/azure/mt743621.aspx) documentation.</span></span>

## <a name="retrieve-metric-values"></a><span data-ttu-id="92220-130">Hämta måttvärden</span><span class="sxs-lookup"><span data-stu-id="92220-130">Retrieve Metric Values</span></span>
<span data-ttu-id="92220-131">När hello definitioner av tillgängliga mått är kända, är det sedan möjligt tooretrieve hello relaterade måttvärden.</span><span class="sxs-lookup"><span data-stu-id="92220-131">Once hello available metric definitions are known, it is then possible tooretrieve hello related metric values.</span></span> <span data-ttu-id="92220-132">Använd hello mått name 'value' (inte hello 'localizedValue') för alla filtrera förfrågningar (till exempel hämta hello 'CpuTime' och 'Begär' mått datapunkter).</span><span class="sxs-lookup"><span data-stu-id="92220-132">Use hello metric’s name ‘value’ (not hello ‘localizedValue’) for any filtering requests (for example, retrieve hello ‘CpuTime’ and ‘Requests’ metric data points).</span></span> <span data-ttu-id="92220-133">Om inga filter har angetts, returneras hello standardmått.</span><span class="sxs-lookup"><span data-stu-id="92220-133">If no filters are specified, hello default metric is returned.</span></span>

> [!NOTE]
> <span data-ttu-id="92220-134">tooretrieve måttvärden med hello Azure övervakaren REST API använder ”2016-06-01” som hello API-versionen.</span><span class="sxs-lookup"><span data-stu-id="92220-134">tooretrieve metric values using hello Azure Monitor REST API, use "2016-06-01" as hello API version.</span></span>
>
>

<span data-ttu-id="92220-135">**Metoden**: hämta</span><span class="sxs-lookup"><span data-stu-id="92220-135">**Method**: GET</span></span>

<span data-ttu-id="92220-136">**Begärande-URI**: https://management.azure.com/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/*{ resurs-provider-namespace}*/*{resurstypen}*/*{resursnamn}*/providers/microsoft.insights/metrics?$filter= *{filter}*& api-version =*{apiVersion}*</span><span class="sxs-lookup"><span data-stu-id="92220-136">**Request URI**: https://management.azure.com/subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/*{resource-provider-namespace}*/*{resource-type}*/*{resource-name}*/providers/microsoft.insights/metrics?$filter=*{filter}*&api-version=*{apiVersion}*</span></span>

<span data-ttu-id="92220-137">Tooretrieve hello RunsSucceeded mått datapunkter för hello angivna tidsintervall och en tidskornet på 1 timme, hello begäran skulle till exempel vara följande:</span><span class="sxs-lookup"><span data-stu-id="92220-137">For example, tooretrieve hello RunsSucceeded metric data points for hello given time range and for a time grain of 1 hour, hello request would be as follows:</span></span>

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2016-09-23 and endTime eq 2016-09-24 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

<span data-ttu-id="92220-138">hello resultatet skulle visas liknande toohello exempel följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="92220-138">hello result would appear similar toohello example following screenshot:</span></span>

![ALT ”JSON-svar visar genomsnittlig svarstid värde”](./media/monitoring-rest-api-walkthrough/available_metrics_logic_app_json_response.png)

<span data-ttu-id="92220-140">tooretrieve flera data eller aggregering datapunkter, lägga till hello mått definition namn och aggregering typer toohello filter som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="92220-140">tooretrieve multiple data or aggregation points, add hello metric definition names and aggregation types toohello filter, as seen in hello following example:</span></span>

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2016-09-23T13:30:00Z and endTime eq 2016-09-23T14:30:00Z and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

### <a name="use-armclient"></a><span data-ttu-id="92220-141">Använd ARMClient</span><span class="sxs-lookup"><span data-stu-id="92220-141">Use ARMClient</span></span>
<span data-ttu-id="92220-142">Ett alternativt toousing PowerShell (som visas ovan), är toouse [ARMClient](https://github.com/projectkudu/ARMClient) på din Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="92220-142">An alternative toousing PowerShell (as shown above), is toouse [ARMClient](https://github.com/projectkudu/ARMClient) on your Windows machine.</span></span> <span data-ttu-id="92220-143">ARMClient hanterar automatiskt hello Azure AD-autentisering (och resulterande JWT-token).</span><span class="sxs-lookup"><span data-stu-id="92220-143">ARMClient handles hello Azure AD authentication (and resulting JWT token) automatically.</span></span> <span data-ttu-id="92220-144">hello beskriver följande steg användningen av ARMClient för att hämta mätvärden:</span><span class="sxs-lookup"><span data-stu-id="92220-144">hello following steps outline use of ARMClient for retrieving metric data:</span></span>

1. <span data-ttu-id="92220-145">Installera [Chocolatey](https://chocolatey.org/) och [ARMClient](https://github.com/projectkudu/ARMClient).</span><span class="sxs-lookup"><span data-stu-id="92220-145">Install [Chocolatey](https://chocolatey.org/) and [ARMClient](https://github.com/projectkudu/ARMClient).</span></span>
2. <span data-ttu-id="92220-146">Ange i ett terminalfönster *armclient.exe inloggning*.</span><span class="sxs-lookup"><span data-stu-id="92220-146">In a terminal window, type *armclient.exe login*.</span></span> <span data-ttu-id="92220-147">Detta uppmanar dig toolog i tooAzure.</span><span class="sxs-lookup"><span data-stu-id="92220-147">This prompts you toolog in tooAzure.</span></span>
3. <span data-ttu-id="92220-148">Typen *armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01*</span><span class="sxs-lookup"><span data-stu-id="92220-148">Type *armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01*</span></span>
4. <span data-ttu-id="92220-149">Typen *armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01*</span><span class="sxs-lookup"><span data-stu-id="92220-149">Type *armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01*</span></span>

![ALT ”Using ARMClient toowork med hello Azure övervakning REST-API”](./media/monitoring-rest-api-walkthrough/armclient_metricdefinitions.png)

## <a name="retrieve-hello-resource-id"></a><span data-ttu-id="92220-151">Hämta hello resurs-ID</span><span class="sxs-lookup"><span data-stu-id="92220-151">Retrieve hello Resource ID</span></span>
<span data-ttu-id="92220-152">Med hjälp av hello REST API hjälper verkligen toounderstand hello tillgängliga mått definitioner, granularitet och relaterade värden.</span><span class="sxs-lookup"><span data-stu-id="92220-152">Using hello REST API can really help toounderstand hello available metric definitions, granularity, and related values.</span></span> <span data-ttu-id="92220-153">Informationen är användbar när du använder hello [Azure hanteringsmodul](https://msdn.microsoft.com/library/azure/mt417623.aspx).</span><span class="sxs-lookup"><span data-stu-id="92220-153">That information is helpful when using hello [Azure Management Library](https://msdn.microsoft.com/library/azure/mt417623.aspx).</span></span>

<span data-ttu-id="92220-154">Hello resurs-ID toouse är hello föregående kod, hello fullständig sökväg toohello önskad Azure-resurs.</span><span class="sxs-lookup"><span data-stu-id="92220-154">For hello preceding code, hello resource ID toouse is hello full path toohello desired Azure resource.</span></span> <span data-ttu-id="92220-155">Till exempel tooquery mot en Azure-Webbapp hello resurs-ID är:</span><span class="sxs-lookup"><span data-stu-id="92220-155">For example, tooquery against an Azure Web App, hello resource ID would be:</span></span>

<span data-ttu-id="92220-156">*/subscriptions/{Subscription-ID}/resourceGroups/{Resource-group-name}/providers/Microsoft.Web/Sites/{Site-Name}/*</span><span class="sxs-lookup"><span data-stu-id="92220-156">*/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{site-name}/*</span></span>

<span data-ttu-id="92220-157">hello innehåller följande lista några exempel på resurs-ID-formaten för olika Azure-resurser:</span><span class="sxs-lookup"><span data-stu-id="92220-157">hello following list contains a few examples of resource ID formats for various Azure resources:</span></span>

* <span data-ttu-id="92220-158">**IoT-hubb** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.Devices/IotHubs/*{iot-hubb-name}*</span><span class="sxs-lookup"><span data-stu-id="92220-158">**IoT Hub** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Devices/IotHubs/*{iot-hub-name}*</span></span>
* <span data-ttu-id="92220-159">**Elastisk SQL-poolen** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.Sql/servers/*{pool-db}*/elasticpools/*{sql-pool-name}*</span><span class="sxs-lookup"><span data-stu-id="92220-159">**Elastic SQL Pool** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Sql/servers/*{pool-db}*/elasticpools/*{sql-pool-name}*</span></span>
* <span data-ttu-id="92220-160">**SQL-databas (v12)** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.Sql/servers/*{servernamn}* /databases/*{databasnamnet}*</span><span class="sxs-lookup"><span data-stu-id="92220-160">**SQL Database (v12)** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Sql/servers/*{server-name}*/databases/*{database-name}*</span></span>
* <span data-ttu-id="92220-161">**Service Bus** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.ServiceBus/*{namespace}* / *{servicebus-name}*</span><span class="sxs-lookup"><span data-stu-id="92220-161">**Service Bus** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.ServiceBus/*{namespace}*/*{servicebus-name}*</span></span>
* <span data-ttu-id="92220-162">**Skalningsuppsättningar** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.Compute/virtualMachineScaleSets/*{ VM-name}*</span><span class="sxs-lookup"><span data-stu-id="92220-162">**VM Scale Sets** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Compute/virtualMachineScaleSets/*{vm-name}*</span></span>
* <span data-ttu-id="92220-163">**Virtuella datorer** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.Compute/virtualMachines/*{vm-name}*</span><span class="sxs-lookup"><span data-stu-id="92220-163">**VMs** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Compute/virtualMachines/*{vm-name}*</span></span>
* <span data-ttu-id="92220-164">**Händelsehubbar** -/subscriptions/*{prenumerations-id}*/resourceGroups/*{resurs-gruppnamn}*/providers/Microsoft.EventHub/namespaces/*{ eventhub-namespace}*</span><span class="sxs-lookup"><span data-stu-id="92220-164">**Event Hubs** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.EventHub/namespaces/*{eventhub-namespace}*</span></span>

<span data-ttu-id="92220-165">Det finns alternativa metoder tooretrieving hello resurs-ID, inklusive användning av Azure Resursläsaren visar hello önskad resurs i hello Azure-portalen och via PowerShell eller hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="92220-165">There are alternative approaches tooretrieving hello resource ID, including using Azure Resource Explorer, viewing hello desired resource in hello Azure portal, and via PowerShell or hello Azure CLI.</span></span>

### <a name="azure-resource-explorer"></a><span data-ttu-id="92220-166">Azure Resource Explorer</span><span class="sxs-lookup"><span data-stu-id="92220-166">Azure Resource Explorer</span></span>
<span data-ttu-id="92220-167">toofind hello resurs-ID för en resurs, en bra metod är toouse hello [resursutforskaren Azure](https://resources.azure.com) verktyget.</span><span class="sxs-lookup"><span data-stu-id="92220-167">toofind hello resource ID for a desired resource, one helpful approach is toouse hello [Azure Resource Explorer](https://resources.azure.com) tool.</span></span> <span data-ttu-id="92220-168">Navigera toohello önskad resurs och titta på hello-ID som visas i följande skärmbild hello:</span><span class="sxs-lookup"><span data-stu-id="92220-168">Navigate toohello desired resource and then look at hello ID shown, as in hello following screenshot:</span></span>

![ALT ”Azure Resursläsaren”](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a><span data-ttu-id="92220-170">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="92220-170">Azure portal</span></span>
<span data-ttu-id="92220-171">hello resurs-ID kan också hämtas från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="92220-171">hello resource ID can also be obtained from hello Azure portal.</span></span> <span data-ttu-id="92220-172">toodo så navigera toohello önskad resurs och välj sedan Egenskaper.</span><span class="sxs-lookup"><span data-stu-id="92220-172">toodo so, navigate toohello desired resource and then select Properties.</span></span> <span data-ttu-id="92220-173">hello resurs-ID visas i hello egenskapsbladet som visas i följande skärmbild hello:</span><span class="sxs-lookup"><span data-stu-id="92220-173">hello Resource ID is displayed in hello Properties blade, as seen in hello following screenshot:</span></span>

![ALT ”resurs-ID visas i hello egenskapsbladet i hello Azure-portalen”](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a><span data-ttu-id="92220-175">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="92220-175">Azure PowerShell</span></span>
<span data-ttu-id="92220-176">hello resurs-ID kan hämtas med hjälp av Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="92220-176">hello resource ID can be retrieved using Azure PowerShell cmdlets as well.</span></span> <span data-ttu-id="92220-177">Till exempel köra tooobtain hello resurs-ID för en Azure-Webbapp hello Get-AzureRmWebApp cmdlet, som i följande skärmbild hello:</span><span class="sxs-lookup"><span data-stu-id="92220-177">For example, tooobtain hello resource ID for an Azure Web App, execute hello Get-AzureRmWebApp cmdlet, as in hello following screenshot:</span></span>

![ALT ”resurs-ID erhållits via PowerShell”](./media/monitoring-rest-api-walkthrough/resourceid_powershell.png)

### <a name="azure-cli"></a><span data-ttu-id="92220-179">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="92220-179">Azure CLI</span></span>
<span data-ttu-id="92220-180">tooretrieve Hej resurs-ID med hjälp av hello Azure CLI, köra hello 'azure webapp visa'-kommando och ange hello '--json-alternativ som visas i följande skärmbild hello:</span><span class="sxs-lookup"><span data-stu-id="92220-180">tooretrieve hello resource ID using hello Azure CLI, execute hello 'azure webapp show' command, specifying hello '--json' option, as shown in hello following screenshot:</span></span>

![ALT ”resurs-ID erhållits via PowerShell”](./media/monitoring-rest-api-walkthrough/resourceid_azurecli.png)

## <a name="retrieve-activity-log-data"></a><span data-ttu-id="92220-182">Hämta aktivitet loggdata</span><span class="sxs-lookup"><span data-stu-id="92220-182">Retrieve Activity Log Data</span></span>
<span data-ttu-id="92220-183">I tillägg tooworking med måttdefinitioner och andra relaterade värden är det också möjligt tooretrieve ytterligare intressanta insikter relaterade tooAzure resurser.</span><span class="sxs-lookup"><span data-stu-id="92220-183">In addition tooworking with metric definitions and related values, it is also possible tooretrieve additional interesting insights related tooAzure resources.</span></span> <span data-ttu-id="92220-184">Exempelvis är det möjligt tooquery [aktivitetsloggen](https://msdn.microsoft.com/library/azure/dn931934.aspx) data.</span><span class="sxs-lookup"><span data-stu-id="92220-184">As an example, it is possible tooquery [activity log](https://msdn.microsoft.com/library/azure/dn931934.aspx) data.</span></span> <span data-ttu-id="92220-185">hello följande exempel visas hur du använder loggdata för hello Azure övervakaren REST API tooquery aktiviteten inom ett visst datumintervall för en Azure-prenumeration:</span><span class="sxs-lookup"><span data-stu-id="92220-185">hello following sample demonstrates using hello Azure Monitor REST API tooquery activity log data within a specific date range for an Azure subscription:</span></span>

```PowerShell
$apiVersion = "2014-04-01"
$filter = "eventTimestamp ge '2016-09-23' and eventTimestamp le '2016-09-24'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

## <a name="next-steps"></a><span data-ttu-id="92220-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="92220-186">Next steps</span></span>
* <span data-ttu-id="92220-187">Granska hello [översikt av övervakning](monitoring-overview.md).</span><span class="sxs-lookup"><span data-stu-id="92220-187">Review hello [Overview of Monitoring](monitoring-overview.md).</span></span>
* <span data-ttu-id="92220-188">Visa hello [stöds mått med Azure-Monitor](monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="92220-188">View hello [Supported metrics with Azure Monitor](monitoring-supported-metrics.md).</span></span>
* <span data-ttu-id="92220-189">Granska hello [Microsoft Azure övervakaren REST API-referens](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span><span class="sxs-lookup"><span data-stu-id="92220-189">Review hello [Microsoft Azure Monitor REST API Reference](https://msdn.microsoft.com/library/azure/dn931943.aspx).</span></span>
* <span data-ttu-id="92220-190">Granska hello [Azure hanteringsmodul](https://msdn.microsoft.com/library/azure/mt417623.aspx).</span><span class="sxs-lookup"><span data-stu-id="92220-190">Review hello [Azure Management Library](https://msdn.microsoft.com/library/azure/mt417623.aspx).</span></span>
