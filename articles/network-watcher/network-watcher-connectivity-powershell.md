---
title: "aaaCheck anslutningen till Azure Nätverksbevakaren - PowerShell | Microsoft Docs"
description: "Den här sidan förklarar hur tootest anslutning med Nätverksbevakaren med hjälp av PowerShell"
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
ms.openlocfilehash: 4bcb90a72f178445c38b7bd7fc5054c5d0c200bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="check-connectivity-with-azure-network-watcher-using-powershell"></a><span data-ttu-id="5e346-103">Kontrollera anslutningen med Azure Nätverksbevakaren med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="5e346-103">Check connectivity with Azure Network Watcher using PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="5e346-104">Portal</span><span class="sxs-lookup"><span data-stu-id="5e346-104">Portal</span></span>](network-watcher-connectivity-portal.md)
> - [<span data-ttu-id="5e346-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5e346-105">PowerShell</span></span>](network-watcher-connectivity-powershell.md)
> - [<span data-ttu-id="5e346-106">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="5e346-106">CLI 2.0</span></span>](network-watcher-connectivity-cli.md)
> - [<span data-ttu-id="5e346-107">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="5e346-107">Azure REST API</span></span>](network-watcher-connectivity-rest.md)

<span data-ttu-id="5e346-108">Lär dig hur toouse anslutning tooverify om en direkt TCP-anslutning från en virtuell dator tooa angivna slutpunkten kan upprättas.</span><span class="sxs-lookup"><span data-stu-id="5e346-108">Learn how toouse connectivity tooverify if a direct TCP connection from a virtual machine tooa given endpoint can be established.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="5e346-109">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="5e346-109">Before you begin</span></span>

<span data-ttu-id="5e346-110">Den här artikeln förutsätter att du har hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="5e346-110">This article assumes you have hello following resources:</span></span>

* <span data-ttu-id="5e346-111">En instans Nätverksbevakaren i hello region som du vill toocheck anslutning.</span><span class="sxs-lookup"><span data-stu-id="5e346-111">An instance of Network Watcher in hello region you want toocheck connectivity.</span></span>

* <span data-ttu-id="5e346-112">Virtuella datorer toocheck anslutningsmöjligheter med.</span><span class="sxs-lookup"><span data-stu-id="5e346-112">Virtual machines toocheck connectivity with.</span></span>

[!INCLUDE [network-watcher-preview](../../includes/network-watcher-public-preview-notice.md)]

> [!IMPORTANT]
> <span data-ttu-id="5e346-113">Kontrollera anslutningen kräver ett tillägg för virtuell dator `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="5e346-113">Connectivity check requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="5e346-114">Installera hello tillägg på en Windows VM finns [tillägg för virtuell dator i Azure Network Watcher Agent för Windows](../virtual-machines/windows/extensions-nwa.md) och för Linux VM besöka [tillägg för virtuell dator i Azure Network Watcher Agent för Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="5e346-114">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="register-hello-preview-capability"></a><span data-ttu-id="5e346-115">Registrera hello preview kapaciteten</span><span class="sxs-lookup"><span data-stu-id="5e346-115">Register hello preview capability</span></span>

<span data-ttu-id="5e346-116">Anslutningen är för närvarande i förhandsversion, toouse funktionen måste toobe registrerad.</span><span class="sxs-lookup"><span data-stu-id="5e346-116">Connectivity is currently in public preview, toouse this feature it needs toobe registered.</span></span> <span data-ttu-id="5e346-117">toodo detta, kör hello följande PowerShell-exempel:</span><span class="sxs-lookup"><span data-stu-id="5e346-117">toodo this, run hello following PowerShell sample:</span></span>

```powershell
Register-AzureRmProviderFeature -FeatureName AllowNetworkWatcherConnectivityCheck  -ProviderNamespace Microsoft.Network
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

<span data-ttu-id="5e346-118">tooverify hello registreringen har slutförts, kör följande Powershell-exempel hello:</span><span class="sxs-lookup"><span data-stu-id="5e346-118">tooverify hello registration was successful, run hello following Powershell sample:</span></span>

```powershell
Get-AzureRmProviderFeature -FeatureName AllowNetworkWatcherConnectivityCheck  -ProviderNamespace  Microsoft.Network
```

<span data-ttu-id="5e346-119">Om hello-funktionen har registrerats korrekt bör hello utdata motsvara hello följande:</span><span class="sxs-lookup"><span data-stu-id="5e346-119">If hello feature was properly registered, hello output should match hello following:</span></span>

```
FeatureName         ProviderName      RegistrationState
-----------         ------------      -----------------
AllowNetworkWatcherConnectivityCheck  Microsoft.Network Registered
```

## <a name="check-connectivity-tooa-virtual-machine"></a><span data-ttu-id="5e346-120">Kontrollera anslutningen tooa virtuell dator</span><span class="sxs-lookup"><span data-stu-id="5e346-120">Check connectivity tooa virtual machine</span></span>

<span data-ttu-id="5e346-121">Det här exemplet kontrollerar anslutningen tooa virtuella måldatorn via port 80.</span><span class="sxs-lookup"><span data-stu-id="5e346-121">This example checks connectivity tooa destination virtual machine over port 80.</span></span>

### <a name="example"></a><span data-ttu-id="5e346-122">Exempel</span><span class="sxs-lookup"><span data-stu-id="5e346-122">Example</span></span>

```powershell
$rgName = "ContosoRG"
$sourceVMName = "MultiTierApp0"
$destVMName = "Database0"

$RG = Get-AzureRMResourceGroup -Name $rgName

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $RG.Location } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

$VM1 = Get-AzureRMVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $sourceVMName
$VM2 = Get-AzureRMVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $destVMName

Test-AzureRmNetworkWatcherConnectivity -NetworkWatcher $networkWatcher -SourceId $VM1.Id -DestinationId $VM2.Id -DestinationPort 80
```

### <a name="response"></a><span data-ttu-id="5e346-123">Svar</span><span class="sxs-lookup"><span data-stu-id="5e346-123">Response</span></span>

<span data-ttu-id="5e346-124">hello efter svar är från hello föregående exempel.</span><span class="sxs-lookup"><span data-stu-id="5e346-124">hello following response is from hello previous example.</span></span>  <span data-ttu-id="5e346-125">I det här svaret hello `ConnectionStatus` är **inte åtkomlig**.</span><span class="sxs-lookup"><span data-stu-id="5e346-125">In this response, hello `ConnectionStatus` is **Unreachable**.</span></span> <span data-ttu-id="5e346-126">Du kan se alla hello avsökningar skickas misslyckades.</span><span class="sxs-lookup"><span data-stu-id="5e346-126">You can see that all hello probes sent failed.</span></span> <span data-ttu-id="5e346-127">hello-anslutningen misslyckades på hello virtuell installation på grund av tooa användardefinierat `NetworkSecurityRule` med namnet **UserRule_Port80**, konfigurerad tooblock inkommande trafik på port 80.</span><span class="sxs-lookup"><span data-stu-id="5e346-127">hello connectivity failed at hello virtual appliance due tooa user-configured `NetworkSecurityRule` named **UserRule_Port80**, configured tooblock incoming traffic on port 80.</span></span> <span data-ttu-id="5e346-128">Den här informationen kan vara problem med anslutningen används tooresearch.</span><span class="sxs-lookup"><span data-stu-id="5e346-128">This information can be used tooresearch connection issues.</span></span>

```
ConnectionStatus : Unreachable
AvgLatencyInMs   : 
MinLatencyInMs   : 
MaxLatencyInMs   : 
ProbesSent       : 100
ProbesFailed     : 100
Hops             : [
                     {
                       "Type": "Source",
                       "Id": "c5222ea0-3213-4f85-a642-cee63217c2f3",
                       "Address": "10.1.1.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGrou
                   ps/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurat
                   ions/ipconfig1",
                       "NextHopIds": [
                         "9283a9f0-cc5e-4239-8f5e-ae0f3c19fbaa"
                       ],
                       "Issues": []
                     },
                     {
                       "Type": "VirtualAppliance",
                       "Id": "9283a9f0-cc5e-4239-8f5e-ae0f3c19fbaa",
                       "Address": "10.1.2.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGrou
                   ps/ContosoRG/providers/Microsoft.Network/networkInterfaces/fwNic/ipConfiguratio
                   ns/ipconfig1",
                       "NextHopIds": [
                         "0f1500cd-c512-4d43-b431-7267e4e67017"
                       ],
                       "Issues": []
                     },
                     {
                       "Type": "VirtualAppliance",
                       "Id": "0f1500cd-c512-4d43-b431-7267e4e67017",
                       "Address": "10.1.3.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGrou
                   ps/ContosoRG/providers/Microsoft.Network/networkInterfaces/auNic/ipConfiguratio
                   ns/ipconfig1",
                       "NextHopIds": [
                         "a88940f8-5fbe-40da-8d99-1dee89240f64"
                       ],
                       "Issues": [
                         {
                           "Origin": "Outbound",
                           "Severity": "Error",
                           "Type": "NetworkSecurityRule",
                           "Context": [
                             {
                               "key": "RuleName",
                               "value": "UserRule_Port80"
                             }
                           ]
                         }
                       ]
                     },
                     {
                       "Type": "VnetLocal",
                       "Id": "a88940f8-5fbe-40da-8d99-1dee89240f64",
                       "Address": "10.1.4.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGrou
                   ps/ContosoRG/providers/Microsoft.Network/networkInterfaces/dbNic0/ipConfigurati
                   ons/ipconfig1",
                       "NextHopIds": [],
                       "Issues": []
                     }
                   ]
```

## <a name="validate-routing-issues"></a><span data-ttu-id="5e346-129">Validera routning problem</span><span class="sxs-lookup"><span data-stu-id="5e346-129">Validate routing issues</span></span>

<span data-ttu-id="5e346-130">hello exempel kontrollerar anslutningen mellan en virtuell dator och en fjärrslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="5e346-130">hello example checks connectivity between a virtual machine and a remote endpoint.</span></span>

### <a name="example"></a><span data-ttu-id="5e346-131">Exempel</span><span class="sxs-lookup"><span data-stu-id="5e346-131">Example</span></span>

```powershell
$rgName = "ContosoRG"
$sourceVMName = "MultiTierApp0"

$RG = Get-AzureRMResourceGroup -Name $rgName

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $RG.Location } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

$VM1 = Get-AzureRMVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $sourceVMName

Test-AzureRmNetworkWatcherConnectivity -NetworkWatcher $networkWatcher -SourceId $VM1.Id -DestinationAddress 13.107.21.200 -DestinationPort 80
```

### <a name="response"></a><span data-ttu-id="5e346-132">Svar</span><span class="sxs-lookup"><span data-stu-id="5e346-132">Response</span></span>

<span data-ttu-id="5e346-133">I följande exempel hello, hello `ConnectionStatus` visas som **inte åtkomlig**.</span><span class="sxs-lookup"><span data-stu-id="5e346-133">In hello following example, hello `ConnectionStatus` is shown as **Unreachable**.</span></span> <span data-ttu-id="5e346-134">I hello `Hops` information hittar du `Issues` att hello trafik har blockerats på grund av tooa `UserDefinedRoute`.</span><span class="sxs-lookup"><span data-stu-id="5e346-134">In hello `Hops` details, you can see under `Issues` that hello traffic was blocked due tooa `UserDefinedRoute`.</span></span> 

```
ConnectionStatus : Unreachable
AvgLatencyInMs   : 
MinLatencyInMs   : 
MaxLatencyInMs   : 
ProbesSent       : 100
ProbesFailed     : 100
Hops             : [
                     {
                       "Type": "Source",
                       "Id": "b4f7bceb-07a3-44ca-8bae-adec6628225f",
                       "Address": "10.1.1.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
                       "NextHopIds": [
                         "3fee8adf-692f-4523-b742-f6fdf6da6584"
                       ],
                       "Issues": [
                         {
                           "Origin": "Outbound",
                           "Severity": "Error",
                           "Type": "UserDefinedRoute",
                           "Context": [
                             {
                               "key": "RouteType",
                               "value": "User"
                             }
                           ]
                         }
                       ]
                     },
                     {
                       "Type": "Destination",
                       "Id": "3fee8adf-692f-4523-b742-f6fdf6da6584",
                       "Address": "13.107.21.200",
                       "ResourceId": "Unknown",
                       "NextHopIds": [],
                       "Issues": []
                     }
                   ]
```

## <a name="check-website-latency"></a><span data-ttu-id="5e346-135">Kontrollera svarstid för webbplats</span><span class="sxs-lookup"><span data-stu-id="5e346-135">Check website latency</span></span>

<span data-ttu-id="5e346-136">hello kontrollerar följande exempel hello anslutningen tooa webbplats.</span><span class="sxs-lookup"><span data-stu-id="5e346-136">hello following example checks hello connectivity tooa website.</span></span>

### <a name="example"></a><span data-ttu-id="5e346-137">Exempel</span><span class="sxs-lookup"><span data-stu-id="5e346-137">Example</span></span>

```powershell
$rgName = "ContosoRG"
$sourceVMName = "MultiTierApp0"

$RG = Get-AzureRMResourceGroup -Name $rgName

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $RG.Location } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

$VM1 = Get-AzureRMVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $sourceVMName

Test-AzureRmNetworkWatcherConnectivity -NetworkWatcher $networkWatcher -SourceId $VM1.Id -DestinationAddress http://bing.com/
```

### <a name="response"></a><span data-ttu-id="5e346-138">Svar</span><span class="sxs-lookup"><span data-stu-id="5e346-138">Response</span></span>

<span data-ttu-id="5e346-139">I hello efter svar, kan du se hello `ConnectionStatus` visas som **nåbar**.</span><span class="sxs-lookup"><span data-stu-id="5e346-139">In hello following response, you can see hello `ConnectionStatus` shows as **Reachable**.</span></span> <span data-ttu-id="5e346-140">När en anslutning lyckas tillhandahålls svarstider.</span><span class="sxs-lookup"><span data-stu-id="5e346-140">When a connection is successful, latency values are provided.</span></span>

```
ConnectionStatus : Reachable
AvgLatencyInMs   : 1
MinLatencyInMs   : 0
MaxLatencyInMs   : 7
ProbesSent       : 100
ProbesFailed     : 0
Hops             : [
                     {
                       "Type": "Source",
                       "Id": "1f0e3415-27b0-4bf7-a59d-3e19fb854e3e",
                       "Address": "10.1.1.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
                       "NextHopIds": [
                         "f99f2bd1-42e8-4bbf-85b6-5d21d00c84e0"
                       ],
                       "Issues": []
                     },
                     {
                       "Type": "Internet",
                       "Id": "f99f2bd1-42e8-4bbf-85b6-5d21d00c84e0",
                       "Address": "204.79.197.200",
                       "ResourceId": "Internet",
                       "NextHopIds": [],
                       "Issues": []
                     }
                   ]
```

## <a name="check-connectivity-tooa-storage-endpoint"></a><span data-ttu-id="5e346-141">Kontrollera anslutningen tooa lagring slutpunkt</span><span class="sxs-lookup"><span data-stu-id="5e346-141">Check connectivity tooa storage endpoint</span></span>

<span data-ttu-id="5e346-142">följande exempel hello testar hello anslutning från ett lagringskonto för virtuell dator tooa blogg.</span><span class="sxs-lookup"><span data-stu-id="5e346-142">hello following example tests hello connectivity from a virtual machine tooa blog storage account.</span></span>

### <a name="example"></a><span data-ttu-id="5e346-143">Exempel</span><span class="sxs-lookup"><span data-stu-id="5e346-143">Example</span></span>

```powershell
$rgName = "ContosoRG"
$sourceVMName = "MultiTierApp0"

$RG = Get-AzureRMResourceGroup -Name $rgName

$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $RG.Location }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

$VM1 = Get-AzureRMVM -ResourceGroupName $rgName | Where-Object -Property Name -EQ $sourceVMName

Test-AzureRmNetworkWatcherConnectivity -NetworkWatcher $networkWatcher -SourceId $VM1.Id -DestinationAddress https://contosostorageexample.blob.core.windows.net/ 
```

### <a name="response"></a><span data-ttu-id="5e346-144">Svar</span><span class="sxs-lookup"><span data-stu-id="5e346-144">Response</span></span>

<span data-ttu-id="5e346-145">hello är följande json hello exempelsvar körs hello föregående cmdlet.</span><span class="sxs-lookup"><span data-stu-id="5e346-145">hello following json is hello example response from running hello previous cmdlet.</span></span> <span data-ttu-id="5e346-146">Eftersom hello målet kan nås, hello `ConnectionStatus` egenskapen visas som **nåbar**.</span><span class="sxs-lookup"><span data-stu-id="5e346-146">As hello destination is reachable, hello `ConnectionStatus` property shows as **Reachable**.</span></span>  <span data-ttu-id="5e346-147">Hello information om hello antal hopp krävs tooreach hello storage-blob och svarstid finns.</span><span class="sxs-lookup"><span data-stu-id="5e346-147">You are provided hello details regarding hello number of hops required tooreach hello storage blob and latency.</span></span>

```
ConnectionStatus : Reachable
AvgLatencyInMs   : 1
MinLatencyInMs   : 0
MaxLatencyInMs   : 8
ProbesSent       : 100
ProbesFailed     : 0
Hops             : [
                     {
                       "Type": "Source",
                       "Id": "9e7f61d9-fb45-41db-83e2-c815a919b8ed",
                       "Address": "10.1.1.4",
                       "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/networkInterfaces/appNic0/ipConfigurations/ipconfig1",
                       "NextHopIds": [
                         "1e6d4b3c-7964-4afd-b959-aaa746ee0f15"
                       ],
                       "Issues": []
                     },
                     {
                       "Type": "Internet",
                       "Id": "1e6d4b3c-7964-4afd-b959-aaa746ee0f15",
                       "Address": "13.71.200.248",
                       "ResourceId": "Internet",
                       "NextHopIds": [],
                       "Issues": []
                     }
                   ]
```

## <a name="next-steps"></a><span data-ttu-id="5e346-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5e346-148">Next steps</span></span>

<span data-ttu-id="5e346-149">Hitta om vissa trafik tillåts i eller utanför den virtuella datorn genom att besöka [Kontrollera Kontrollera IP-flöde](network-watcher-check-ip-flow-verify-portal.md)</span><span class="sxs-lookup"><span data-stu-id="5e346-149">Find if certain traffic is allowed in or out of your VM by visiting [Check IP flow verify](network-watcher-check-ip-flow-verify-portal.md)</span></span>

<span data-ttu-id="5e346-150">Om trafik blockeras och får inte vara, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack hello network security grupp och säkerhet regler som har definierats.</span><span class="sxs-lookup"><span data-stu-id="5e346-150">If traffic is being blocked and it should not be, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that are defined.</span></span>

<!-- Image references -->














