---
title: "aaaManage paket som samlar in med Azure Nätverksbevakaren - REST API | Microsoft Docs"
description: "Den här sidan förklarar hur toomanage hello paket avbilda funktion i Nätverksbevakaren med hjälp av Azure REST API"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 53fe0324-835f-4005-afc8-145eeb314aeb
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 7a531fbe796e85e94961bd192d171defb299be05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-packet-captures-with-azure-network-watcher-using-azure-rest-api"></a><span data-ttu-id="03b04-103">Hantera paket insamlingar med Azure Nätverksbevakaren med hjälp av Azure REST API</span><span class="sxs-lookup"><span data-stu-id="03b04-103">Manage packet captures with Azure Network Watcher using Azure REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="03b04-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="03b04-104">Azure portal</span></span>](network-watcher-packet-capture-manage-portal.md)
> - [<span data-ttu-id="03b04-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="03b04-105">PowerShell</span></span>](network-watcher-packet-capture-manage-powershell.md)
> - [<span data-ttu-id="03b04-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="03b04-106">CLI 1.0</span></span>](network-watcher-packet-capture-manage-cli-nodejs.md)
> - [<span data-ttu-id="03b04-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="03b04-107">CLI 2.0</span></span>](network-watcher-packet-capture-manage-cli.md)
> - [<span data-ttu-id="03b04-108">Azure REST-API</span><span class="sxs-lookup"><span data-stu-id="03b04-108">Azure REST API</span></span>](network-watcher-packet-capture-manage-rest.md)

<span data-ttu-id="03b04-109">Nätverket Watcher paketinsamling kan toocreate avbilda sessioner tootrack trafik tooand från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="03b04-109">Network Watcher packet capture allows you toocreate capture sessions tootrack traffic tooand from a virtual machine.</span></span> <span data-ttu-id="03b04-110">Filter har angetts för hello avbilda session tooensure du fånga in endast hello trafik som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="03b04-110">Filters are provided for hello capture session tooensure you capture only hello traffic you want.</span></span> <span data-ttu-id="03b04-111">Paketinsamling hjälper toodiagnose nätverk avvikelser reaktivt och proaktivt.</span><span class="sxs-lookup"><span data-stu-id="03b04-111">Packet capture helps toodiagnose network anomalies both reactively and proactively.</span></span> <span data-ttu-id="03b04-112">Andra användningsområden omfattar att samla in nätverksstatistik får information om nätverket intrång, toodebug klient / server-kommunikation och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="03b04-112">Other uses include gathering network statistics, gaining information on network intrusions, toodebug client-server communications and much more.</span></span> <span data-ttu-id="03b04-113">Den här funktionen underlättar genom kan tooremotely utlösaren paket insamlingar, hello belastningen körs en paketinsamling manuellt och på hello önskade dator som sparar värdefull tid.</span><span class="sxs-lookup"><span data-stu-id="03b04-113">By being able tooremotely trigger packet captures, this capability eases hello burden of running a packet capture manually and on hello desired machine, which saves valuable time.</span></span>

<span data-ttu-id="03b04-114">Den här artikeln tar dig igenom hello olika administrativa uppgifter som är tillgängliga för paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="03b04-114">This article takes you through hello different management tasks that are currently available for packet capture.</span></span>

- [<span data-ttu-id="03b04-115">**Hämta en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="03b04-115">**Get a packet capture**</span></span>](#get-a-packet-capture)
- [<span data-ttu-id="03b04-116">**Visa en lista med alla paket-insamlingar**</span><span class="sxs-lookup"><span data-stu-id="03b04-116">**List all packet captures**</span></span>](#list-all-packet-captures)
- [<span data-ttu-id="03b04-117">**Fråga hello status för en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="03b04-117">**Query hello status of a packet capture**</span></span>](#query-packet-capture-status)
- [<span data-ttu-id="03b04-118">**Starta en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="03b04-118">**Start a packet capture**</span></span>](#start-packet-capture)
- [<span data-ttu-id="03b04-119">**Stoppa en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="03b04-119">**Stop a packet capture**</span></span>](#stop-packet-capture)
- [<span data-ttu-id="03b04-120">**Ta bort en paketinsamling**</span><span class="sxs-lookup"><span data-stu-id="03b04-120">**Delete a packet capture**</span></span>](#delete-packet-capture)

## <a name="before-you-begin"></a><span data-ttu-id="03b04-121">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="03b04-121">Before you begin</span></span>

<span data-ttu-id="03b04-122">I det här scenariot kan du anropa hello nätverket Watcher Rest API toorun IP flöda kontrollera.</span><span class="sxs-lookup"><span data-stu-id="03b04-122">In this scenario, you call hello Network Watcher Rest API toorun IP Flow Verify.</span></span> <span data-ttu-id="03b04-123">ARMclient är används toocall hello REST-API med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="03b04-123">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="03b04-124">ARMClient hittas på chocolatey på [ARMClient på Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="03b04-124">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="03b04-125">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="03b04-125">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

> <span data-ttu-id="03b04-126">Paketinsamling kräver ett tillägg för virtuell dator `AzureNetworkWatcherExtension`.</span><span class="sxs-lookup"><span data-stu-id="03b04-126">Packet capture requires a virtual machine extension `AzureNetworkWatcherExtension`.</span></span> <span data-ttu-id="03b04-127">Installera hello tillägg på en Windows VM finns [tillägg för virtuell dator i Azure Network Watcher Agent för Windows](../virtual-machines/windows/extensions-nwa.md) och för Linux VM besöka [tillägg för virtuell dator i Azure Network Watcher Agent för Linux](../virtual-machines/linux/extensions-nwa.md).</span><span class="sxs-lookup"><span data-stu-id="03b04-127">For installing hello extension on a Windows VM visit [Azure Network Watcher Agent virtual machine extension for Windows](../virtual-machines/windows/extensions-nwa.md) and for Linux VM visit [Azure Network Watcher Agent virtual machine extension for Linux](../virtual-machines/linux/extensions-nwa.md).</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="03b04-128">Logga in med ARMClient</span><span class="sxs-lookup"><span data-stu-id="03b04-128">Log in with ARMClient</span></span>

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a><span data-ttu-id="03b04-129">Hämta en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="03b04-129">Retrieve a virtual machine</span></span>

<span data-ttu-id="03b04-130">Kör följande skript tooreturn hello en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="03b04-130">Run hello following script tooreturn a virtual machine.</span></span> <span data-ttu-id="03b04-131">Den här informationen behövs för att starta en paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="03b04-131">This information is needed for starting a packet capture.</span></span>

<span data-ttu-id="03b04-132">hello måste följande kod variabler:</span><span class="sxs-lookup"><span data-stu-id="03b04-132">hello following code needs variables:</span></span>

- <span data-ttu-id="03b04-133">**subscriptionId** -hello prenumerations-id kan också hämtas med hello **Get-AzureRMSubscription** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="03b04-133">**subscriptionId** - hello subscription id can also be retrieved with hello **Get-AzureRMSubscription** cmdlet.</span></span>
- <span data-ttu-id="03b04-134">**resourceGroupName** - hello namn på en resursgrupp som innehåller virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="03b04-134">**resourceGroupName** - hello name of a resource group that contains virtual machines.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

<span data-ttu-id="03b04-135">Från hello följande utdata, används hello-id för hello virtuell dator i hello nästa exempel.</span><span class="sxs-lookup"><span data-stu-id="03b04-135">From hello following output, hello id of hello virtual machine is used in hello next example.</span></span>

```json
...
,
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```


## <a name="get-a-packet-capture"></a><span data-ttu-id="03b04-136">Hämta en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="03b04-136">Get a packet capture</span></span>

<span data-ttu-id="03b04-137">hello hämtar följande exempel hello status för en enskild paketinsamling</span><span class="sxs-lookup"><span data-stu-id="03b04-137">hello following example gets hello status of a single packet capture</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}/querystatus?api-version=2016-12-01"
```

<span data-ttu-id="03b04-138">hello är följande svar exempel på en typisk svaret som returnerades vid fråga hello status för en paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="03b04-138">hello following responses are examples of a typical response returned when querying hello status of a packet capture.</span></span>

```json
{
  "name": "TestPacketCapture5",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/TestPacketCapture6",
  "captureStartTime": "2016-12-06T17:20:01.5671279Z",
  "packetCaptureStatus": "Running",
  "packetCaptureError": []
}
```

```json
{
  "name": "TestPacketCapture5",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/TestPacketCapture6",
  "captureStartTime": "2016-12-06T17:20:01.5671279Z",
  "packetCaptureStatus": "Stopped",
  "stopReason": "TimeExceeded",
  "packetCaptureError": []
}
```

## <a name="list-all-packet-captures"></a><span data-ttu-id="03b04-139">Visa en lista med alla paket-insamlingar</span><span class="sxs-lookup"><span data-stu-id="03b04-139">List all packet captures</span></span>

<span data-ttu-id="03b04-140">hello följande exempel hämtar alla paket avbilda sessioner i en region.</span><span class="sxs-lookup"><span data-stu-id="03b04-140">hello following example gets all packet capture sessions in a region.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
armclient get "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures?api-version=2016-12-01"
```

<span data-ttu-id="03b04-141">hello följande svaret är ett exempel på ett typiskt svar returneras när du hämtar alla paket avbildas</span><span class="sxs-lookup"><span data-stu-id="03b04-141">hello following response is an example of a typical response returned when getting all packet captures</span></span>

```json
{
  "value": [
    {
      "name": "TestPacketCapture6",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/TestPacketCapture6",
      "etag": "W/\"091762e1-c23f-448b-89d5-37cf56e4c045\"",
      "properties": {
        "provisioningState": "Succeeded",
        "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute/virtualMachines/ContosoVM",
        "bytesToCapturePerPacket": 0,
        "totalBytesPerSession": 1073741824,
        "timeLimitInSeconds": 60,
        "storageLocation": {
          "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Storage/storageAccounts/contosoexamplergdiag374",
          "storagePath": "https://contosoexamplergdiag374.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/contosoexamplerg/providers/microsoft.compute/virtualmachines/contosovm/2016/12/06/packetcap
ture_17_19_53_056.cap",
          "filePath": "c:\\temp\\packetcapture.cap"
        },
        "filters": [
          {
            "protocol": "Any",
            "localIPAddress": "",
            "localPort": "",
            "remoteIPAddress": "",
            "remotePort": ""
          }
        ]
      }
    },
    {
      "name": "TestPacketCapture7",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher_westcentralus/packetCaptures/TestPacketCapture7",
      "etag": "W/\"091762e1-c23f-448b-89d5-37cf56e4c045\"",
      "properties": {
        "provisioningState": "Failed",
        "target": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute/virtualMachines/ContosoVM",
        "bytesToCapturePerPacket": 0,
        "totalBytesPerSession": 1073741824,
        "timeLimitInSeconds": 60,
        "storageLocation": {
          "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Storage/storageAccounts/contosoexamplergdiag374",
          "storagePath": "https://contosoexamplergdiag374.blob.core.windows.net/network-watcher-logs/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/contosoexamplerg/providers/microsoft.compute/virtualmachines/contosovm/2016/12/06/packetcap
ture_17_23_15_364.cap",
          "filePath": "c:\\temp\\packetcapture.cap"
        },
        "filters": [
          {
            "protocol": "Any",
            "localIPAddress": "",
            "localPort": "",
            "remoteIPAddress": "",
            "remotePort": ""
          }
        ]
      }
    }
  ]
}
```

## <a name="query-packet-capture-status"></a><span data-ttu-id="03b04-142">Fråga status för paket-avbildning</span><span class="sxs-lookup"><span data-stu-id="03b04-142">Query packet capture status</span></span>

<span data-ttu-id="03b04-143">hello följande exempel hämtar alla paket avbilda sessioner i en region.</span><span class="sxs-lookup"><span data-stu-id="03b04-143">hello following example gets all packet capture sessions in a region.</span></span>

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"
armclient get "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}/querystatus?api-version=2016-12-01"
```

<span data-ttu-id="03b04-144">hello är följande svaret ett exempel på ett typiskt svar som returnerades vid fråga hello status för en paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="03b04-144">hello following response is an example of a typical response returned when querying hello status of a packet capture.</span></span>

```json
{
    "name": "vm1PacketCapture",     "id": "/subscriptions/{guid}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkWatchers/{networkWatche rName}/packetCaptures/{packetCaptureName}",
   "captureStartTime" : "9/7/2016 12:35:24PM",
   "packetCaptureStatus" : "Stopped",
   "stopReason" : "TimeExceeded"
   "packetCaptureError" : [ ]
}
```

## <a name="start-packet-capture"></a><span data-ttu-id="03b04-145">Starta paketinsamling</span><span class="sxs-lookup"><span data-stu-id="03b04-145">Start packet capture</span></span>

<span data-ttu-id="03b04-146">hello följande exempel skapar en paketinsamling på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="03b04-146">hello following example creates a packet capture on a virtual machine.</span></span>  <span data-ttu-id="03b04-147">hello exempel är parametriserade tooallow flexibilitet att skapa ett exempel.</span><span class="sxs-lookup"><span data-stu-id="03b04-147">hello example is parameterized tooallow for flexibility in creating an example.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"
$storageaccountname = "contosoexamplergdiag374"
$vmName = "ContosoVM"
$bytestoCaptureperPacket = "0"
$bytesPerSession = "1073741824"
$captureTimeinSeconds = "60"
$localIP = ""
$localPort = "" # Examples are: 80, or 80-120
$remoteIP = ""
$remotePort = "" # Examples are: 80, or 80-120
$protocol = "" # Valid values are TCP, UDP and Any.
$targetUri = "" # Example: /subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.compute/virtualMachine/$vmName
$storageId = "" # Example: "https://mytestaccountname.blob.core.windows.net/capture/vm1Capture.cap"
$storagePath = ""
$localFilePath = "c:\\temp\\packetcapture.cap" # Example: "d:\capture\vm1Capture.cap"

$requestBody = @"
{
    'properties':  {
                       'target':  '/${targetUri}',
                       'bytesToCapturePerPacket':  '${bytestoCaptureperPacket}',
                       'totalBytesPerSession':  '${bytesPerSession}',
                       'timeLimitinSeconds':  '${captureTimeinSeconds}',
                       'storageLocation':  {
                                               'storageId':  '${storageId}',
                                               'storagePath':  '${storagePath}',
                                               'filePath':  '${localFilePath}'
                                           },
                       'filters':  [
                                       {
                                           'protocol':  '${protocol}',
                                           'localIPAddress':  '${localIP}',
                                           'localPort':  '${localPort}',
                                           'remoteIPAddress':  '${remoteIP}',
                                           'remotePort':  '${remotePort}'
                                       }
                                   ]
                   }
}
"@

armclient PUT "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}?api-version=2016-07-01" $requestbody
```

## <a name="stop-packet-capture"></a><span data-ttu-id="03b04-148">Stoppa paketinsamling</span><span class="sxs-lookup"><span data-stu-id="03b04-148">Stop packet capture</span></span>

<span data-ttu-id="03b04-149">hello följande exempel stoppar en paketinsamling på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="03b04-149">hello following example stops a packet capture on a virtual machine.</span></span>  <span data-ttu-id="03b04-150">hello exempel är parametriserade tooallow flexibilitet att skapa ett exempel.</span><span class="sxs-lookup"><span data-stu-id="03b04-150">hello example is parameterized tooallow for flexibility in creating an example.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}/stop?api-version=2016-12-01"
```

## <a name="delete-packet-capture"></a><span data-ttu-id="03b04-151">Ta bort paketinsamling</span><span class="sxs-lookup"><span data-stu-id="03b04-151">Delete packet capture</span></span>

<span data-ttu-id="03b04-152">hello följande exempel tar bort en paketinsamling på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="03b04-152">hello following example deletes a packet capture on a virtual machine.</span></span>  <span data-ttu-id="03b04-153">hello exempel är parametriserade tooallow flexibilitet att skapa ett exempel.</span><span class="sxs-lookup"><span data-stu-id="03b04-153">hello example is parameterized tooallow for flexibility in creating an example.</span></span>

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$packetCaptureName = "TestPacketCapture5"

armclient delete "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/packetCaptures/${packetCaptureName}?api-version=2016-12-01"
```

> [!NOTE]
> <span data-ttu-id="03b04-154">Om du tar bort en paketinsamling tas filen inte bort hello i hello storage-konto</span><span class="sxs-lookup"><span data-stu-id="03b04-154">Deleting a packet capture does not delete hello file in hello storage account</span></span>

## <a name="next-steps"></a><span data-ttu-id="03b04-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="03b04-155">Next steps</span></span>

<span data-ttu-id="03b04-156">Anvisningar för hämtning av filer från azure storage-konton finns för[komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="03b04-156">For instructions on downloading files from azure storage accounts, refer too[Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="03b04-157">Ett annat verktyg som kan användas är Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="03b04-157">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="03b04-158">Mer information om Lagringsutforskaren hittar du här på hello följande länk: [Lagringsutforskaren](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="03b04-158">More information about Storage Explorer can be found here at hello following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

<span data-ttu-id="03b04-159">Lär dig hur fångar tooautomate paket med virtuella aviseringar genom att visa [skapar en avisering utlösta paketinsamling](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="03b04-159">Learn how tooautomate packet captures with Virtual machine alerts by viewing [Create an alert triggered packet capture](network-watcher-alert-triggered-packet-capture.md)</span></span>













