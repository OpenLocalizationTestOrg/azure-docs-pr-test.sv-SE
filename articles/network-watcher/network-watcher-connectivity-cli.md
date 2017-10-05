---
title: "Kontrollera anslutningen till Azure Nätverksbevakaren - Azure CLI 2.0 | Microsoft Docs"
description: "Den här sidan förklarar hur du använder anslutning kontroll med hjälp av Azure CLI 2.0 Nätverksbevakaren"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/11/2017
ms.author: gwallace
ms.openlocfilehash: c1deaa40bfda0bf3858ad56d3d6a90df34351278
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="check-connectivity-with-azure-network-watcher-using-azure-cli-20"></a><span data-ttu-id="818b3-103">Kontrollera anslutningen med Azure Nätverksbevakaren använder Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="818b3-103">Check connectivity with Azure Network Watcher using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="818b3-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="818b3-104">PowerShell</span></span>](network-watcher-connectivity-powershell.md)
> - [<span data-ttu-id="818b3-105">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="818b3-105">CLI 2.0</span></span>](network-watcher-connectivity-cli.md)
> - [<span data-ttu-id="818b3-106">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="818b3-106">Azure REST API</span></span>](network-watcher-connectivity-rest.md)

<span data-ttu-id="818b3-107">Lär dig använda anslutningen för att kontrollera om en direkt TCP-anslutning från en virtuell dator till en viss slutpunkt kan upprättas.</span><span class="sxs-lookup"><span data-stu-id="818b3-107">Learn how to use connectivity to verify if a direct TCP connection from a virtual machine to a given endpoint can be established.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="818b3-108">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="818b3-108">Before you begin</span></span>

<span data-ttu-id="818b3-109">Den här artikeln förutsätter att du har följande resurser:</span><span class="sxs-lookup"><span data-stu-id="818b3-109">This article assumes you have the following resources:</span></span>

* <span data-ttu-id="818b3-110">En instans av Nätverksbevakaren i regionen som du vill kontrollera anslutningen.</span><span class="sxs-lookup"><span data-stu-id="818b3-110">An instance of Network Watcher in the region you want to check connectivity.</span></span>

* <span data-ttu-id="818b3-111">Virtuella datorer om du vill kontrollera anslutningen med.</span><span class="sxs-lookup"><span data-stu-id="818b3-111">Virtual machines to check connectivity with.</span></span>

[!INCLUDE [network-watcher-preview](../../includes/network-watcher-public-preview-notice.md)]

> [!IMPORTANT]
> <span data-ttu-id="818b3-112">Kontrollera anslutningen kräver ett tillägg för virtuell dator `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="818b3-112">Connectivity check requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="818b3-113">Installera tillägget på en Windows VM finns [tillägg för virtuell dator i Azure Network Watcher Agent för Windows](../virtual-machines/windows/extensions-nwa.md) och för Linux VM besöka [tillägg för virtuell dator i Azure Network Watcher Agent för Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="818b3-113">For installing the extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="register-the-preview-capability"></a><span data-ttu-id="818b3-114">Registrera funktionen för förhandsgranskning</span><span class="sxs-lookup"><span data-stu-id="818b3-114">Register the preview capability</span></span> 

<span data-ttu-id="818b3-115">Kontrollera anslutningen är för närvarande i förhandsversion att använda den här funktionen måste vara registrerad.</span><span class="sxs-lookup"><span data-stu-id="818b3-115">Connectivity check is currently in public preview, to use this feature it needs to be registered.</span></span> <span data-ttu-id="818b3-116">Gör detta genom att köra följande CLI exemplet</span><span class="sxs-lookup"><span data-stu-id="818b3-116">To do this, run the following CLI sample</span></span>

```azurecli 
az feature register --namespace Microsoft.Network --name AllowNetworkWatcherConnectivityCheck

az provider register --namespace Microsoft.Network 
``` 

<span data-ttu-id="818b3-117">Kontrollera registreringen lyckades, kör du kommandot CLI:</span><span class="sxs-lookup"><span data-stu-id="818b3-117">To verify the registration was successful, run the following CLI command:</span></span>

```azurecli
az feature show --namespace Microsoft.Network --name AllowNetworkWatcherConnectivityCheck 
```

<span data-ttu-id="818b3-118">Om funktionen har registrerats korrekt, bör dina utdata motsvara följande:</span><span class="sxs-lookup"><span data-stu-id="818b3-118">If the feature was properly registered, the output should match the following:</span></span> 

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Features/providers/Microsoft.Network/features/AllowNetworkWatcherConnectivityCheck",
  "name": "Microsoft.Network/AllowNetworkWatcherConnectivityCheck",
  "properties": {
    "state": "Registered"
  },
  "type": "Microsoft.Features/providers/features"
}
``` 

## <a name="check-connectivity-to-a-virtual-machine"></a><span data-ttu-id="818b3-119">Kontrollera anslutningen till en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="818b3-119">Check connectivity to a virtual machine</span></span>

<span data-ttu-id="818b3-120">Det här exemplet kontrollerar anslutningen till en virtuell dator för målet via port 80.</span><span class="sxs-lookup"><span data-stu-id="818b3-120">This example checks connectivity to a destination virtual machine over port 80.</span></span>

### <a name="example"></a><span data-ttu-id="818b3-121">Exempel</span><span class="sxs-lookup"><span data-stu-id="818b3-121">Example</span></span>

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-resource Database0 --dest-port 80
```

### <a name="response"></a><span data-ttu-id="818b3-122">Svar</span><span class="sxs-lookup"><span data-stu-id="818b3-122">Response</span></span>

<span data-ttu-id="818b3-123">Följande svar är från föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="818b3-123">The following response is from the previous example.</span></span>  <span data-ttu-id="818b3-124">I det här svaret på `ConnectionStatus` är **inte åtkomlig**.</span><span class="sxs-lookup"><span data-stu-id="818b3-124">In this response, the `ConnectionStatus` is **Unreachable**.</span></span> <span data-ttu-id="818b3-125">Du kan se att alla avsökningar skickas misslyckades.</span><span class="sxs-lookup"><span data-stu-id="818b3-125">You can see that all the probes sent failed.</span></span> <span data-ttu-id="818b3-126">Anslutningen misslyckades på den virtuella installationen på grund av ett användardefinierat `NetworkSecurityRule` med namnet **UserRule_Port80**, konfigurerad för att blockera inkommande trafik på port 80.</span><span class="sxs-lookup"><span data-stu-id="818b3-126">The connectivity failed at the virtual appliance due to a user-configured `NetworkSecurityRule` named **UserRule_Port80**, configured to block incoming traffic on port 80.</span></span> <span data-ttu-id="818b3-127">Den här informationen kan användas för att utreda problem med anslutningen.</span><span class="sxs-lookup"><span data-stu-id="818b3-127">This information can be used to research connection issues.</span></span>

```json
{
  "avgLatencyInMs": null,
  "connectionStatus": "Unreachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "bb01d336-d881-4808-9fbc-72f091974d68",
      "issues": [],
      "nextHopIds": [
        "f8b074e9-9980-496b-a35e-619f9bcbf648"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/ap
pNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "10.1.2.4",
      "id": "f8b074e9-9980-496b-a35e-619f9bcbf648",
      "issues": [],
      "nextHopIds": [
        "8a5857f3-6ab8-4b11-b9bf-a046d66b8696"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/fw
Nic/ipConfigurations/ipconfig1",
      "type": "VirtualAppliance"
    },
    {
      "address": "10.1.3.4",
      "id": "8a5857f3-6ab8-4b11-b9bf-a046d66b8696",
      "issues": [
        {
          "context": [
            {
              "key": "RuleName",
              "value": "UserRule_Port80"
            }
          ],
          "origin": "Outbound",
          "severity": "Error",
          "type": "NetworkSecurityRule"
        }
      ],
      "nextHopIds": [
        "6ce2f7a2-ceb4-4145-80e8-5d9f661655d6"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/au
Nic/ipConfigurations/ipconfig1",
      "type": "VirtualAppliance"
    },
    {
      "address": "10.1.4.4",
      "id": "6ce2f7a2-ceb4-4145-80e8-5d9f661655d6",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/db
Nic0/ipConfigurations/ipconfig1",
      "type": "VnetLocal"
    }
  ],
  "maxLatencyInMs": null,
  "minLatencyInMs": null,
  "probesFailed": 100,
  "probesSent": 100
}
```

## <a name="validate-routing-issues"></a><span data-ttu-id="818b3-128">Validera routning problem</span><span class="sxs-lookup"><span data-stu-id="818b3-128">Validate routing issues</span></span>

<span data-ttu-id="818b3-129">Exemplet kontrollerar anslutningen mellan en virtuell dator och en fjärrslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="818b3-129">The example checks connectivity between a virtual machine and a remote endpoint.</span></span>

### <a name="example"></a><span data-ttu-id="818b3-130">Exempel</span><span class="sxs-lookup"><span data-stu-id="818b3-130">Example</span></span>

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-address 13.107.21.200 --dest-port 80
```

### <a name="response"></a><span data-ttu-id="818b3-131">Svar</span><span class="sxs-lookup"><span data-stu-id="818b3-131">Response</span></span>

<span data-ttu-id="818b3-132">I följande exempel visas den `connectionStatus` visas som **inte åtkomlig**.</span><span class="sxs-lookup"><span data-stu-id="818b3-132">In the following example, the `connectionStatus` is shown as **Unreachable**.</span></span> <span data-ttu-id="818b3-133">I den `hops` information hittar du `issues` som trafiken har blockerats på grund av en `UserDefinedRoute`.</span><span class="sxs-lookup"><span data-stu-id="818b3-133">In the `hops` details, you can see under `issues` that the traffic was blocked due to a `UserDefinedRoute`.</span></span>

```json
{
  "avgLatencyInMs": null,
  "connectionStatus": "Unreachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "f2cb1868-2049-4839-b8ed-57a480d06f95",
      "issues": [
        {
          "context": [
            {
              "key": "RouteType",
              "value": "User"
            }
          ],
          "origin": "Outbound",
          "severity": "Error",
          "type": "UserDefinedRoute"
        }
      ],
      "nextHopIds": [
        "da4022db-0ab0-48c4-a507-dd4c03561ca5"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/ap
pNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "13.107.21.200",
      "id": "da4022db-0ab0-48c4-a507-dd4c03561ca5",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "Unknown",
      "type": "Destination"
    }
  ],
  "maxLatencyInMs": null,
  "minLatencyInMs": null,
  "probesFailed": 100,
  "probesSent": 100
}
```

## <a name="check-website-latency"></a><span data-ttu-id="818b3-134">Kontrollera svarstid för webbplats</span><span class="sxs-lookup"><span data-stu-id="818b3-134">Check website latency</span></span>

<span data-ttu-id="818b3-135">I följande exempel kontrollerar anslutningen till en webbplats.</span><span class="sxs-lookup"><span data-stu-id="818b3-135">The following example checks the connectivity to a website.</span></span>

### <a name="example"></a><span data-ttu-id="818b3-136">Exempel</span><span class="sxs-lookup"><span data-stu-id="818b3-136">Example</span></span>

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-address http://bing.com --dest-port 80
```

### <a name="response"></a><span data-ttu-id="818b3-137">Svar</span><span class="sxs-lookup"><span data-stu-id="818b3-137">Response</span></span>

<span data-ttu-id="818b3-138">I följande svar, kan du se den `connectionStatus` visas som **nåbar**.</span><span class="sxs-lookup"><span data-stu-id="818b3-138">In the following response, you can see the `connectionStatus` shows as **Reachable**.</span></span> <span data-ttu-id="818b3-139">När en anslutning lyckas tillhandahålls svarstider.</span><span class="sxs-lookup"><span data-stu-id="818b3-139">When a connection is successful, latency values are provided.</span></span>

```json
{
  "avgLatencyInMs": 2,
  "connectionStatus": "Reachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "639c2d19-e163-4dfd-8737-5018dd1168ae",
      "issues": [],
      "nextHopIds": [
        "fd43a6e7-c758-4f48-90aa-8db99105a4a3"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/ap
pNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "204.79.197.200",
      "id": "fd43a6e7-c758-4f48-90aa-8db99105a4a3",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "Internet",
      "type": "Internet"
    }
  ],
  "maxLatencyInMs": 7,
  "minLatencyInMs": 0,
  "probesFailed": 0,
  "probesSent": 100
}
```

## <a name="check-connectivity-to-a-storage-endpoint"></a><span data-ttu-id="818b3-140">Kontrollera anslutningen till en slutpunkt för lagring</span><span class="sxs-lookup"><span data-stu-id="818b3-140">Check connectivity to a storage endpoint</span></span>

<span data-ttu-id="818b3-141">I följande exempel kontrollerar anslutningen från en virtuell dator till en blogg storage-konto.</span><span class="sxs-lookup"><span data-stu-id="818b3-141">The following example checks the connectivity from a virtual machine to a blog storage account.</span></span>

### <a name="example"></a><span data-ttu-id="818b3-142">Exempel</span><span class="sxs-lookup"><span data-stu-id="818b3-142">Example</span></span>

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-address https://contosoexamplesa.blob.core.windows.net/
```

### <a name="response"></a><span data-ttu-id="818b3-143">Svar</span><span class="sxs-lookup"><span data-stu-id="818b3-143">Response</span></span>

<span data-ttu-id="818b3-144">Följande json är exempel svaret från att köra cmdlet tidigare.</span><span class="sxs-lookup"><span data-stu-id="818b3-144">The following json is the example response from running the previous cmdlet.</span></span> <span data-ttu-id="818b3-145">När kontrollen är lyckas den `connectionStatus` egenskapen visas som **nåbar**.</span><span class="sxs-lookup"><span data-stu-id="818b3-145">As the check is successful, the `connectionStatus` property shows as **Reachable**.</span></span>  <span data-ttu-id="818b3-146">Information om antalet hopp som krävs för att nå lagringsblob och fördröjning finns.</span><span class="sxs-lookup"><span data-stu-id="818b3-146">You are provided the details regarding the number of hops required to reach the storage blob and latency.</span></span>

```json
{
  "avgLatencyInMs": 1,
  "connectionStatus": "Reachable",
  "hops": [
    {
      "address": "10.1.1.4",
      "id": "5136acff-bf26-4c93-9966-4edb7dd40353",
      "issues": [],
      "nextHopIds": [
        "f8d958b7-3636-4d63-9441-602c1eb2fd56"
      ],
      "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
      "type": "Source"
    },
    {
      "address": "1.2.3.4",
      "id": "f8d958b7-3636-4d63-9441-602c1eb2fd56",
      "issues": [],
      "nextHopIds": [],
      "resourceId": "Internet",
      "type": "Internet"
    }
  ],
  "maxLatencyInMs": 7,
  "minLatencyInMs": 0,
  "probesFailed": 0,
  "probesSent": 100
}
```

## <a name="next-steps"></a><span data-ttu-id="818b3-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="818b3-147">Next steps</span></span>

<span data-ttu-id="818b3-148">Lär dig att automatisera insamlingar paket med virtuella aviseringar genom att visa [skapar en avisering utlösta paketinsamling](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="818b3-148">Learn how to automate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="818b3-149">Hitta om vissa trafik tillåts i eller utanför den virtuella datorn genom att besöka [Kontrollera Kontrollera IP-flöde](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="818b3-149">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>
