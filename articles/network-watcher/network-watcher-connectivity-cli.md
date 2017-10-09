---
title: "aaaCheck anslutningen till Azure Nätverksbevakaren - Azure CLI 2.0 | Microsoft Docs"
description: "Den här sidan förklarar hur toouse anslutningen kontrolleras med hjälp av Azure CLI 2.0 Nätverksbevakaren"
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
ms.openlocfilehash: e94e0fad03fd36ebf4e1fdf9e3cfee934b289deb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="check-connectivity-with-azure-network-watcher-using-azure-cli-20"></a><span data-ttu-id="42850-103">Kontrollera anslutningen med Azure Nätverksbevakaren använder Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="42850-103">Check connectivity with Azure Network Watcher using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="42850-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="42850-104">PowerShell</span></span>](network-watcher-connectivity-powershell.md)
> - [<span data-ttu-id="42850-105">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="42850-105">CLI 2.0</span></span>](network-watcher-connectivity-cli.md)
> - [<span data-ttu-id="42850-106">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="42850-106">Azure REST API</span></span>](network-watcher-connectivity-rest.md)

<span data-ttu-id="42850-107">Lär dig hur toouse anslutning tooverify om en direkt TCP-anslutning från en virtuell dator tooa angivna slutpunkten kan upprättas.</span><span class="sxs-lookup"><span data-stu-id="42850-107">Learn how toouse connectivity tooverify if a direct TCP connection from a virtual machine tooa given endpoint can be established.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="42850-108">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="42850-108">Before you begin</span></span>

<span data-ttu-id="42850-109">Den här artikeln förutsätter att du har hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="42850-109">This article assumes you have hello following resources:</span></span>

* <span data-ttu-id="42850-110">En instans Nätverksbevakaren i hello region som du vill toocheck anslutning.</span><span class="sxs-lookup"><span data-stu-id="42850-110">An instance of Network Watcher in hello region you want toocheck connectivity.</span></span>

* <span data-ttu-id="42850-111">Virtuella datorer toocheck anslutningsmöjligheter med.</span><span class="sxs-lookup"><span data-stu-id="42850-111">Virtual machines toocheck connectivity with.</span></span>

[!INCLUDE [network-watcher-preview](../../includes/network-watcher-public-preview-notice.md)]

> [!IMPORTANT]
> <span data-ttu-id="42850-112">Kontrollera anslutningen kräver ett tillägg för virtuell dator `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="42850-112">Connectivity check requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="42850-113">Installera hello tillägg på en Windows VM finns [tillägg för virtuell dator i Azure Network Watcher Agent för Windows](../virtual-machines/windows/extensions-nwa.md) och för Linux VM besöka [tillägg för virtuell dator i Azure Network Watcher Agent för Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="42850-113">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="register-hello-preview-capability"></a><span data-ttu-id="42850-114">Registrera hello preview kapaciteten</span><span class="sxs-lookup"><span data-stu-id="42850-114">Register hello preview capability</span></span> 

<span data-ttu-id="42850-115">Kontrollera anslutningen är för närvarande i förhandsversion, toouse funktionen toobe registrerade måste.</span><span class="sxs-lookup"><span data-stu-id="42850-115">Connectivity check is currently in public preview, toouse this feature it needs toobe registered.</span></span> <span data-ttu-id="42850-116">toodo detta, kör hello följande CLI-exempel</span><span class="sxs-lookup"><span data-stu-id="42850-116">toodo this, run hello following CLI sample</span></span>

```azurecli 
az feature register --namespace Microsoft.Network --name AllowNetworkWatcherConnectivityCheck

az provider register --namespace Microsoft.Network 
``` 

<span data-ttu-id="42850-117">tooverify hello registreringen har slutförts, kör följande kommando CLI hello:</span><span class="sxs-lookup"><span data-stu-id="42850-117">tooverify hello registration was successful, run hello following CLI command:</span></span>

```azurecli
az feature show --namespace Microsoft.Network --name AllowNetworkWatcherConnectivityCheck 
```

<span data-ttu-id="42850-118">Om hello-funktionen har registrerats korrekt bör hello utdata motsvara hello följande:</span><span class="sxs-lookup"><span data-stu-id="42850-118">If hello feature was properly registered, hello output should match hello following:</span></span> 

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

## <a name="check-connectivity-tooa-virtual-machine"></a><span data-ttu-id="42850-119">Kontrollera anslutningen tooa virtuell dator</span><span class="sxs-lookup"><span data-stu-id="42850-119">Check connectivity tooa virtual machine</span></span>

<span data-ttu-id="42850-120">Det här exemplet kontrollerar anslutningen tooa virtuella måldatorn via port 80.</span><span class="sxs-lookup"><span data-stu-id="42850-120">This example checks connectivity tooa destination virtual machine over port 80.</span></span>

### <a name="example"></a><span data-ttu-id="42850-121">Exempel</span><span class="sxs-lookup"><span data-stu-id="42850-121">Example</span></span>

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-resource Database0 --dest-port 80
```

### <a name="response"></a><span data-ttu-id="42850-122">Svar</span><span class="sxs-lookup"><span data-stu-id="42850-122">Response</span></span>

<span data-ttu-id="42850-123">hello efter svar är från hello föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="42850-123">hello following response is from hello previous example.</span></span>  <span data-ttu-id="42850-124">I det här svaret hello `ConnectionStatus` är **inte åtkomlig**.</span><span class="sxs-lookup"><span data-stu-id="42850-124">In this response, hello `ConnectionStatus` is **Unreachable**.</span></span> <span data-ttu-id="42850-125">Du kan se alla hello avsökningar skickas misslyckades.</span><span class="sxs-lookup"><span data-stu-id="42850-125">You can see that all hello probes sent failed.</span></span> <span data-ttu-id="42850-126">hello-anslutningen misslyckades på hello virtuell installation på grund av tooa användardefinierat `NetworkSecurityRule` med namnet **UserRule_Port80**, konfigurerad tooblock inkommande trafik på port 80.</span><span class="sxs-lookup"><span data-stu-id="42850-126">hello connectivity failed at hello virtual appliance due tooa user-configured `NetworkSecurityRule` named **UserRule_Port80**, configured tooblock incoming traffic on port 80.</span></span> <span data-ttu-id="42850-127">Den här informationen kan vara problem med anslutningen används tooresearch.</span><span class="sxs-lookup"><span data-stu-id="42850-127">This information can be used tooresearch connection issues.</span></span>

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

## <a name="validate-routing-issues"></a><span data-ttu-id="42850-128">Validera routning problem</span><span class="sxs-lookup"><span data-stu-id="42850-128">Validate routing issues</span></span>

<span data-ttu-id="42850-129">hello exempel kontrollerar anslutningen mellan en virtuell dator och en fjärrslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="42850-129">hello example checks connectivity between a virtual machine and a remote endpoint.</span></span>

### <a name="example"></a><span data-ttu-id="42850-130">Exempel</span><span class="sxs-lookup"><span data-stu-id="42850-130">Example</span></span>

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-address 13.107.21.200 --dest-port 80
```

### <a name="response"></a><span data-ttu-id="42850-131">Svar</span><span class="sxs-lookup"><span data-stu-id="42850-131">Response</span></span>

<span data-ttu-id="42850-132">I följande exempel hello, hello `connectionStatus` visas som **inte åtkomlig**.</span><span class="sxs-lookup"><span data-stu-id="42850-132">In hello following example, hello `connectionStatus` is shown as **Unreachable**.</span></span> <span data-ttu-id="42850-133">I hello `hops` information hittar du `issues` att hello trafik har blockerats på grund av tooa `UserDefinedRoute`.</span><span class="sxs-lookup"><span data-stu-id="42850-133">In hello `hops` details, you can see under `issues` that hello traffic was blocked due tooa `UserDefinedRoute`.</span></span>

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

## <a name="check-website-latency"></a><span data-ttu-id="42850-134">Kontrollera svarstid för webbplats</span><span class="sxs-lookup"><span data-stu-id="42850-134">Check website latency</span></span>

<span data-ttu-id="42850-135">hello kontrollerar följande exempel hello anslutningen tooa webbplats.</span><span class="sxs-lookup"><span data-stu-id="42850-135">hello following example checks hello connectivity tooa website.</span></span>

### <a name="example"></a><span data-ttu-id="42850-136">Exempel</span><span class="sxs-lookup"><span data-stu-id="42850-136">Example</span></span>

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-address http://bing.com --dest-port 80
```

### <a name="response"></a><span data-ttu-id="42850-137">Svar</span><span class="sxs-lookup"><span data-stu-id="42850-137">Response</span></span>

<span data-ttu-id="42850-138">I hello efter svar, kan du se hello `connectionStatus` visas som **nåbar**.</span><span class="sxs-lookup"><span data-stu-id="42850-138">In hello following response, you can see hello `connectionStatus` shows as **Reachable**.</span></span> <span data-ttu-id="42850-139">När en anslutning lyckas tillhandahålls svarstider.</span><span class="sxs-lookup"><span data-stu-id="42850-139">When a connection is successful, latency values are provided.</span></span>

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

## <a name="check-connectivity-tooa-storage-endpoint"></a><span data-ttu-id="42850-140">Kontrollera anslutningen tooa lagring slutpunkt</span><span class="sxs-lookup"><span data-stu-id="42850-140">Check connectivity tooa storage endpoint</span></span>

<span data-ttu-id="42850-141">hello kontrollerar följande exempel hello anslutning från ett lagringskonto för virtuell dator tooa blogg.</span><span class="sxs-lookup"><span data-stu-id="42850-141">hello following example checks hello connectivity from a virtual machine tooa blog storage account.</span></span>

### <a name="example"></a><span data-ttu-id="42850-142">Exempel</span><span class="sxs-lookup"><span data-stu-id="42850-142">Example</span></span>

```azurecli
az network watcher test-connectivity --resource-group ContosoRG --source-resource MultiTierApp0 --dest-address https://contosoexamplesa.blob.core.windows.net/
```

### <a name="response"></a><span data-ttu-id="42850-143">Svar</span><span class="sxs-lookup"><span data-stu-id="42850-143">Response</span></span>

<span data-ttu-id="42850-144">hello är följande json hello exempelsvar körs hello föregående cmdlet.</span><span class="sxs-lookup"><span data-stu-id="42850-144">hello following json is hello example response from running hello previous cmdlet.</span></span> <span data-ttu-id="42850-145">Eftersom hello kontrollera lyckade hello `connectionStatus` egenskapen visas som **nåbar**.</span><span class="sxs-lookup"><span data-stu-id="42850-145">As hello check is successful, hello `connectionStatus` property shows as **Reachable**.</span></span>  <span data-ttu-id="42850-146">Hello information om hello antal hopp krävs tooreach hello storage-blob och svarstid finns.</span><span class="sxs-lookup"><span data-stu-id="42850-146">You are provided hello details regarding hello number of hops required tooreach hello storage blob and latency.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="42850-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="42850-147">Next steps</span></span>

<span data-ttu-id="42850-148">Lär dig hur fångar tooautomate paket med virtuella aviseringar genom att visa [skapar en avisering utlösta paketinsamling](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="42850-148">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="42850-149">Hitta om vissa trafik tillåts i eller utanför den virtuella datorn genom att besöka [Kontrollera Kontrollera IP-flöde](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="42850-149">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>
