---
title: "aaaTroubleshoot virtuella nätverksgateway och anslutningar med hjälp av Azure Nätverksbevakaren - REST | Microsoft Docs"
description: "Den här sidan förklarar hur tootroubleshoot virtuella Nätverksgatewayer och anslutningar med Azure Nätverksbevakaren med hjälp av REST"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e4d5f195-b839-4394-94ef-a04192766e55
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: cc89b46643fdbfefe53727b45d6b7d06914b58a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher"></a><span data-ttu-id="345dd-103">Felsöka virtuell nätverksgateway och anslutningar med hjälp av Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="345dd-103">Troubleshoot Virtual Network gateway and Connections using Azure Network Watcher</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="345dd-104">Portal</span><span class="sxs-lookup"><span data-stu-id="345dd-104">Portal</span></span>](network-watcher-troubleshoot-manage-portal.md)
> - [<span data-ttu-id="345dd-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="345dd-105">PowerShell</span></span>](network-watcher-troubleshoot-manage-powershell.md)
> - [<span data-ttu-id="345dd-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="345dd-106">CLI 1.0</span></span>](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [<span data-ttu-id="345dd-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="345dd-107">CLI 2.0</span></span>](network-watcher-troubleshoot-manage-cli.md)
> - [<span data-ttu-id="345dd-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="345dd-108">REST API</span></span>](network-watcher-troubleshoot-manage-rest.md)

<span data-ttu-id="345dd-109">Nätverksbevakaren innehåller många funktioner när det gäller toounderstanding nätverksresurserna i Azure.</span><span class="sxs-lookup"><span data-stu-id="345dd-109">Network Watcher provides many capabilities as it relates toounderstanding your network resources in Azure.</span></span> <span data-ttu-id="345dd-110">En av dessa funktioner är resurs felsökning.</span><span class="sxs-lookup"><span data-stu-id="345dd-110">One of these capabilities is resource troubleshooting.</span></span> <span data-ttu-id="345dd-111">Felsökning av resursen kan anropas via hello portal, PowerShell, CLI eller REST API.</span><span class="sxs-lookup"><span data-stu-id="345dd-111">Resource troubleshooting can be called through hello portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="345dd-112">När den anropas, Nätverksbevakaren inspektera hello hälsotillståndet för en virtuell nätverksgateway eller en anslutning och returnerar resultatet.</span><span class="sxs-lookup"><span data-stu-id="345dd-112">When called, Network Watcher inspects hello health of a Virtual Network Gateway or a Connection and returns its findings.</span></span>

<span data-ttu-id="345dd-113">Den här artikeln tar dig igenom hello olika administrativa uppgifter som är tillgängliga för felsökning av resursen.</span><span class="sxs-lookup"><span data-stu-id="345dd-113">This article takes you through hello different management tasks that are currently available for resource troubleshooting.</span></span>

- [<span data-ttu-id="345dd-114">**Felsöka en virtuell nätverksgateway**</span><span class="sxs-lookup"><span data-stu-id="345dd-114">**Troubleshoot a Virtual Network gateway**</span></span>](#troubleshoot-a-virtual-network-gateway)
- [<span data-ttu-id="345dd-115">**Felsöka en anslutning**</span><span class="sxs-lookup"><span data-stu-id="345dd-115">**Troubleshoot a Connection**</span></span>](#troubleshoot-connections)

## <a name="before-you-begin"></a><span data-ttu-id="345dd-116">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="345dd-116">Before you begin</span></span>

<span data-ttu-id="345dd-117">ARMclient är används toocall hello REST-API med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="345dd-117">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="345dd-118">ARMClient hittas på chocolatey på [ARMClient på Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="345dd-118">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="345dd-119">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="345dd-119">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

<span data-ttu-id="345dd-120">En lista över stöds gateway typer besök [stöd för Gateway-typer](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span><span class="sxs-lookup"><span data-stu-id="345dd-120">For a list of supported gateway types visit, [Supported Gateway types](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span></span>

## <a name="overview"></a><span data-ttu-id="345dd-121">Översikt</span><span class="sxs-lookup"><span data-stu-id="345dd-121">Overview</span></span>

<span data-ttu-id="345dd-122">Felsökning av nätverk Bevakare ger hello möjlighet felsöka problem som uppstår med virtuella nätverks-gateway och anslutningar.</span><span class="sxs-lookup"><span data-stu-id="345dd-122">Network Watcher troubleshooting provides hello ability troubleshoot issues that arise with Virtual Network gateways and Connections.</span></span> <span data-ttu-id="345dd-123">När en förfrågan görs toohello resurs felsökning, loggar frågar och kontrolleras.</span><span class="sxs-lookup"><span data-stu-id="345dd-123">When a request is made toohello resource troubleshooting, logs are querying and inspected.</span></span> <span data-ttu-id="345dd-124">Hello resultat returneras när kontroll har slutförts.</span><span class="sxs-lookup"><span data-stu-id="345dd-124">When inspection is complete, hello results are returned.</span></span> <span data-ttu-id="345dd-125">hello felsöka API-begäranden är långt begäranden, vilket kan ta flera minuter tooreturn ett resultat som körs.</span><span class="sxs-lookup"><span data-stu-id="345dd-125">hello troubleshoot API requests are long running requests, which could take multiple minutes tooreturn a result.</span></span> <span data-ttu-id="345dd-126">Loggfilerna lagras i en behållare för ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="345dd-126">Logs are stored in a container on a storage account.</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="345dd-127">Logga in med ARMClient</span><span class="sxs-lookup"><span data-stu-id="345dd-127">Log in with ARMClient</span></span>

```PowerShell
armclient login
```

## <a name="troubleshoot-a-virtual-network-gateway"></a><span data-ttu-id="345dd-128">Felsöka en virtuell nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="345dd-128">Troubleshoot a Virtual Network gateway</span></span>


### <a name="post-hello-troubleshoot-request"></a><span data-ttu-id="345dd-129">POST hello felsöka begäran</span><span class="sxs-lookup"><span data-stu-id="345dd-129">POST hello troubleshoot request</span></span>

<span data-ttu-id="345dd-130">hello följande exempel frågor hello status för en virtuell nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="345dd-130">hello following example queries hello status of a Virtual Network gateway.</span></span>

```powershell

$subscriptionId = "00000000-0000-0000-0000-000000000000"
$resourceGroupName = "ContosoRG"
$NWresourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$vnetGatewayName = "ContosoVNETGateway"
$storageAccountName = "contososa"
$containerName = "gwlogs"
$requestBody = @"
{
'TargetResourceId': '/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/virtualNetworkGateways/${vnetGatewayName}',
'Properties': {
'StorageId': '/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Storage/storageAccounts/${storageAccountName}',
'StoragePath': 'https://${storageAccountName}.blob.core.windows.net/${containerName}'
}
}
"@

}
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/troubleshoot?api-version=2016-03-30 "
```

<span data-ttu-id="345dd-131">Eftersom den här åtgärden är långt körs, hello URI för frågor hello igen och hello URI för hello resultatet returneras hello svarshuvud enligt hello efter svar:</span><span class="sxs-lookup"><span data-stu-id="345dd-131">Since this operation is long running, hello URI for querying hello operation and hello URI for hello result is returned in hello response header as shown in hello following response:</span></span>

<span data-ttu-id="345dd-132">**Viktiga värden**</span><span class="sxs-lookup"><span data-stu-id="345dd-132">**Important Values**</span></span>

* <span data-ttu-id="345dd-133">**Azure-asynkrona åtgärder** -egenskapen innehåller hello URI tooquery hello asynkrona felsöka igen</span><span class="sxs-lookup"><span data-stu-id="345dd-133">**Azure-AsyncOperation** - This property contains hello URI tooquery hello Async troubleshoot operation</span></span>
* <span data-ttu-id="345dd-134">**Plats** -egenskapen innehåller hello URI där hello resultat är när hello åtgärden har slutförts</span><span class="sxs-lookup"><span data-stu-id="345dd-134">**Location** - This property contains hello URI where hello results are when hello operation is complete</span></span>

```
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 10
x-ms-request-id: 8a1167b7-6768-4ac1-85dc-703c9c9b9247
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Strict-Transport-Security: max-age=31536000; includeSubDomains
Cache-Control: no-cache
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Server: Microsoft-HTTPAPI/2.0; Microsoft-HTTPAPI/2.0
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 4364d88a-bd08-422c-a716-dbb0cdc99f7b
x-ms-routing-request-id: NORTHCENTRALUS:20170112T183202Z:4364d88a-bd08-422c-a716-dbb0cdc99f7b
Date: Thu, 12 Jan 2017 18:32:01 GMT

null
```

### <a name="query-hello-async-operation-for-completion"></a><span data-ttu-id="345dd-135">Fråga hello async-åtgärden för slutförande</span><span class="sxs-lookup"><span data-stu-id="345dd-135">Query hello async operation for completion</span></span>

<span data-ttu-id="345dd-136">Använd hello operations URI tooquery för hello hello åtgärdens förlopp som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="345dd-136">Use hello operations URI tooquery for hello progress of hello operation as seen in hello following example:</span></span>

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30"
```

<span data-ttu-id="345dd-137">Medan hello pågår, hello svar visar **InProgress** som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="345dd-137">While hello operation is in progress, hello response shows **InProgress** as seen in hello following example:</span></span>

```json
{
  "status": "InProgress"
}
```

<span data-ttu-id="345dd-138">När hello åtgärden är klar hello status ändras för**lyckades**.</span><span class="sxs-lookup"><span data-stu-id="345dd-138">When hello operation is complete hello status changes too**Succeeded**.</span></span>

```json
{
  "status": "Succeeded"
}
```

### <a name="retrieve-hello-results"></a><span data-ttu-id="345dd-139">Hämta hello resultat</span><span class="sxs-lookup"><span data-stu-id="345dd-139">Retrieve hello results</span></span>

<span data-ttu-id="345dd-140">När hello status returneras är **lyckades**, anropa en GET-metod i hello operationResult URI tooretrieve hello resultat.</span><span class="sxs-lookup"><span data-stu-id="345dd-140">Once hello status returned is **Succeeded**, call a GET Method on hello operationResult URI tooretrieve hello results.</span></span>

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30"
```

<span data-ttu-id="345dd-141">hello är följande svar exempel på en typisk försämrad svaret som returnerades vid fråga hello resultat felsökning av en gateway.</span><span class="sxs-lookup"><span data-stu-id="345dd-141">hello following responses are examples of a typical degraded response returned when querying hello results of troubleshooting a gateway.</span></span> <span data-ttu-id="345dd-142">Se [förstå hello resultat](#understanding-the-results) tooget förstår vilka hello egenskaper i hello svar medelvärdet.</span><span class="sxs-lookup"><span data-stu-id="345dd-142">See [Understanding hello results](#understanding-the-results) tooget clarification on what hello properties in hello response mean.</span></span>

```json
{
  "startTime": "2017-01-12T10:31:41.562646-08:00",
  "endTime": "2017-01-12T18:31:48.677Z",
  "code": "Degraded",
  "results": [
    {
      "id": "PlatformInActive",
      "summary": "We are sorry, your VPN gateway is in standby mode",
      "detail": "During this time hello gateway will not initiate or accept VPN connections with on premises VPN devices or other Azure VPN Gateways. This is a transient state while hello Azure platform is being updated.",
      "recommendedActions": [
        {
          "actionText": "If hello condition persists, please try resetting your Azure VPN gateway",
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting hello VPN Gateway"
        },
        {
          "actionText": "If your VPN gateway isn't up and running by hello expected resolution time, contact support",
          "actionUri": "http://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    },
    {
      "id": "NoFault",
      "summary": "This VPN gateway is running normally",
      "detail": "There aren't any known Azure platform problems affecting this VPN Connection",
      "recommendedActions": [
        {
          "actionText": "If you are still experience problems with hello VPN gateway, please try resetting hello VPN gateway.",
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting VPN gateway"
        },
        {
          "actionText": "If you are experiencing problems you believe are caused by Azure, contact support",
          "actionUri": "http://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    }
  ]
}
```


## <a name="troubleshoot-connections"></a><span data-ttu-id="345dd-143">Felsöka anslutningar</span><span class="sxs-lookup"><span data-stu-id="345dd-143">Troubleshoot Connections</span></span>

<span data-ttu-id="345dd-144">hello följande exempel frågor hello status för en anslutning.</span><span class="sxs-lookup"><span data-stu-id="345dd-144">hello following example queries hello status of a Connection.</span></span>

```powershell

$subscriptionId = "00000000-0000-0000-0000-000000000000"
$resourceGroupName = "ContosoRG"
$NWresourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$connectionName = "VNET2toVNET1Connection"
$storageAccountName = "contososa"
$containerName = "gwlogs"
$requestBody = @{
"TargetResourceId": "/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/connections/${connectionName}",
"Properties": {
"StorageId": "/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Storage/storageAccounts/${storageAccountName}",
"StoragePath": "https://${storageAccountName}.blob.core.windows.net/${containerName}"
}

}
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/troubleshoot?api-version=2016-03-30 "
```

> [!NOTE]
> <span data-ttu-id="345dd-145">hello felsöka åtgärden inte kan köras parallellt på en anslutning och dess motsvarande gateways.</span><span class="sxs-lookup"><span data-stu-id="345dd-145">hello troubleshoot operation cannot be run in parallel on a Connection and its corresponding gateways.</span></span> <span data-ttu-id="345dd-146">hello-åtgärden måste slutföras tidigare toorunning på hello föregående resurs.</span><span class="sxs-lookup"><span data-stu-id="345dd-146">hello operation must complete prior toorunning it on hello previous resource.</span></span>

<span data-ttu-id="345dd-147">Eftersom det är en tidskrävande transaktion i hello svarshuvud returneras hello URI för frågor hello åtgärden och hello URI för hello resultatet enligt hello efter svar:</span><span class="sxs-lookup"><span data-stu-id="345dd-147">Since this is a long running transaction, in hello response header, hello URI for querying hello operation and hello URI for hello result is returned as shown in hello following response:</span></span>

<span data-ttu-id="345dd-148">**Viktiga värden**</span><span class="sxs-lookup"><span data-stu-id="345dd-148">**Important Values**</span></span>

* <span data-ttu-id="345dd-149">**Azure-asynkrona åtgärder** -egenskapen innehåller hello URI tooquery hello asynkrona felsöka igen</span><span class="sxs-lookup"><span data-stu-id="345dd-149">**Azure-AsyncOperation** - This property contains hello URI tooquery hello Async troubleshoot operation</span></span>
* <span data-ttu-id="345dd-150">**Plats** -egenskapen innehåller hello URI där hello resultat är när hello åtgärden har slutförts</span><span class="sxs-lookup"><span data-stu-id="345dd-150">**Location** - This property contains hello URI where hello results are when hello operation is complete</span></span>

```
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 10
x-ms-request-id: 8a1167b7-6768-4ac1-85dc-703c9c9b9247
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Strict-Transport-Security: max-age=31536000; includeSubDomains
Cache-Control: no-cache
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/8a1167b7-6768-4ac1-85dc-703c9c9b9247?api-version=2016-03-30
Server: Microsoft-HTTPAPI/2.0; Microsoft-HTTPAPI/2.0
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 4364d88a-bd08-422c-a716-dbb0cdc99f7b
x-ms-routing-request-id: NORTHCENTRALUS:20170112T183202Z:4364d88a-bd08-422c-a716-dbb0cdc99f7b
Date: Thu, 12 Jan 2017 18:32:01 GMT

null
```

### <a name="query-hello-async-operation-for-completion"></a><span data-ttu-id="345dd-151">Fråga hello async-åtgärden för slutförande</span><span class="sxs-lookup"><span data-stu-id="345dd-151">Query hello async operation for completion</span></span>

<span data-ttu-id="345dd-152">Använd hello operations URI tooquery för hello hello åtgärdens förlopp som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="345dd-152">Use hello operations URI tooquery for hello progress of hello operation as seen in hello following example:</span></span>

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operations/843b1c31-4717-4fdd-b7a6-4c786ca9c501?api-version=2016-03-30"
```

<span data-ttu-id="345dd-153">Medan hello pågår, hello svar visar **InProgress** som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="345dd-153">While hello operation is in progress, hello response shows **InProgress** as seen in hello following example:</span></span>

```json
{
  "status": "InProgress"
}
```

<span data-ttu-id="345dd-154">När hello-åtgärden har slutförts ändras hello status för**lyckades**.</span><span class="sxs-lookup"><span data-stu-id="345dd-154">When hello operation is complete, hello status changes too**Succeeded**.</span></span>

```json
{
  "status": "Succeeded"
}
```

<span data-ttu-id="345dd-155">hello är följande svar exempel på en typisk svaret som returnerades vid fråga hello resultat felsökning av en anslutning.</span><span class="sxs-lookup"><span data-stu-id="345dd-155">hello following responses are examples of a typical response returned when querying hello results of troubleshooting a Connection.</span></span>

### <a name="retrieve-hello-results"></a><span data-ttu-id="345dd-156">Hämta hello resultat</span><span class="sxs-lookup"><span data-stu-id="345dd-156">Retrieve hello results</span></span>

<span data-ttu-id="345dd-157">När hello status returneras är **lyckades**, anropa en GET-metod i hello operationResult URI tooretrieve hello resultat.</span><span class="sxs-lookup"><span data-stu-id="345dd-157">Once hello status returned is **Succeeded**, call a GET Method on hello operationResult URI tooretrieve hello results.</span></span>

```powershell
armclient get "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Network/locations/westcentralus/operationResults/843b1c31-4717-4fdd-b7a6-4c786ca9c501?api-version=2016-03-30"
```

<span data-ttu-id="345dd-158">hello är följande svar exempel på en typisk svaret som returnerades vid fråga hello resultat felsökning av en anslutning.</span><span class="sxs-lookup"><span data-stu-id="345dd-158">hello following responses are examples of a typical response returned when querying hello results of troubleshooting a Connection.</span></span>

```json
{
  "startTime": "2017-01-12T14:09:19.1215346-08:00",
  "endTime": "2017-01-12T22:09:23.747Z",
  "code": "UnHealthy",
  "results": [
    {
      "id": "PlatformInActive",
      "summary": "We are sorry, your VPN gateway is in standby mode",
      "detail": "During this time hello gateway will not initiate or accept VPN connections with on premises VPN devices or other Azure VPN Gateways. This 
is a transient state while hello Azure platform is being updated.",
      "recommendedActions": [
        {
          "actionText": "If hello condition persists, please try resetting your Azure VPN gateway",
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting hello VPN gateway"
        },
        {
          "actionText": "If your VPN Connection isn't up and running by hello expected resolution time, contact support",
          "actionUri": "http://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    },
    {
      "id": "NoFault",
      "summary": "This VPN Connection is running normally",
      "detail": "There aren't any known Azure platform problems affecting this VPN Connection",
      "recommendedActions": [
        {
          "actionText": "If you are still experience problems with hello VPN gateway, please try resetting hello VPN gateway.",
          "actionUri": "https://azure.microsoft.com/en-us/documentation/articles/vpn-gateway-resetgw-classic/",
          "actionUriText": "resetting VPN gateway"
        },
        {
          "actionText": "If you are experiencing problems you believe are caused by Azure, contact support",
          "actionUri": "http://azure.microsoft.com/support",
          "actionUriText": "contact support"
        }
      ]
    }
  ]
}
```

## <a name="understanding-hello-results"></a><span data-ttu-id="345dd-159">Förstå hello resultat</span><span class="sxs-lookup"><span data-stu-id="345dd-159">Understanding hello results</span></span>

<span data-ttu-id="345dd-160">hello åtgärd texten innehåller allmänna råd om hur tooresolve hello problemet.</span><span class="sxs-lookup"><span data-stu-id="345dd-160">hello action text provides general guidance on how tooresolve hello issue.</span></span> <span data-ttu-id="345dd-161">Om en åtgärd kan vidtas för hello problemet, tillhandahålls en länk med mer information.</span><span class="sxs-lookup"><span data-stu-id="345dd-161">If an action can be taken for hello issue, a link is provided with additional guidance.</span></span> <span data-ttu-id="345dd-162">I hello fall där det finns inga ytterligare riktlinjer, hello svaret innehåller hello url tooopen ett supportärende.</span><span class="sxs-lookup"><span data-stu-id="345dd-162">In hello case where there is no additional guidance, hello response provides hello url tooopen a support case.</span></span>  <span data-ttu-id="345dd-163">Mer information om hello egenskaper för hello svar och vad som ingår finns [nätverk Watcher Felsökning: översikt](network-watcher-troubleshoot-overview.md)</span><span class="sxs-lookup"><span data-stu-id="345dd-163">For more information about hello properties of hello response and what is included, visit [Network Watcher Troubleshoot overview](network-watcher-troubleshoot-overview.md)</span></span>

<span data-ttu-id="345dd-164">Anvisningar för hämtning av filer från azure storage-konton finns för[komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="345dd-164">For instructions on downloading files from azure storage accounts, refer too[Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="345dd-165">Ett annat verktyg som kan användas är Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="345dd-165">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="345dd-166">Mer information om Lagringsutforskaren hittar du här på hello följande länk: [Lagringsutforskaren](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="345dd-166">More information about Storage Explorer can be found here at hello following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

## <a name="next-steps"></a><span data-ttu-id="345dd-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="345dd-167">Next steps</span></span>

<span data-ttu-id="345dd-168">Om inställningarna har ändrats som stoppa VPN-anslutning, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack ned hello grupp och säkerhet Nätverkssäkerhetsregler som kan vara i fråga.</span><span class="sxs-lookup"><span data-stu-id="345dd-168">If settings have been changed that stop VPN connectivity, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that may be in question.</span></span>
