---
title: "Felsöka Azure virtuell nätverksgateway och anslutningar - PowerShell | Microsoft Docs"
description: "Den här sidan förklarar hur du använder Azure Nätverksbevakaren felsöka PowerShell-cmdlet"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: f6f0a813-38b6-4a1f-8cfc-1dfdf979f595
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: 0bdffbac18d1d236b7674feed4dbc784e50e0ea8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-powershell"></a><span data-ttu-id="4a5fd-103">Felsöka virtuella nätverksgateway och anslutningar med hjälp av Azure Network Watcher PowerShell</span><span class="sxs-lookup"><span data-stu-id="4a5fd-103">Troubleshoot Virtual Network Gateway and Connections using Azure Network Watcher PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="4a5fd-104">Portal</span><span class="sxs-lookup"><span data-stu-id="4a5fd-104">Portal</span></span>](network-watcher-troubleshoot-manage-portal.md)
> - [<span data-ttu-id="4a5fd-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4a5fd-105">PowerShell</span></span>](network-watcher-troubleshoot-manage-powershell.md)
> - [<span data-ttu-id="4a5fd-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="4a5fd-106">CLI 1.0</span></span>](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [<span data-ttu-id="4a5fd-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="4a5fd-107">CLI 2.0</span></span>](network-watcher-troubleshoot-manage-cli.md)
> - [<span data-ttu-id="4a5fd-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="4a5fd-108">REST API</span></span>](network-watcher-troubleshoot-manage-rest.md)

<span data-ttu-id="4a5fd-109">Nätverksbevakaren innehåller många funktioner relateras till att förstå nätverksresurserna i Azure.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-109">Network Watcher provides many capabilities as it relates to understanding your network resources in Azure.</span></span> <span data-ttu-id="4a5fd-110">En av dessa funktioner är resurs felsökning.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-110">One of these capabilities is resource troubleshooting.</span></span> <span data-ttu-id="4a5fd-111">Felsökning av resursen kan anropas via portalen, PowerShell, CLI eller REST API.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-111">Resource troubleshooting can be called through the portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="4a5fd-112">När den anropas, Nätverksbevakaren kontrollerar hälsotillståndet för en virtuell nätverksgateway eller en anslutning och returnerar resultatet.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-112">When called, Network Watcher inspects the health of a Virtual Network Gateway or a Connection and returns its findings.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="4a5fd-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="4a5fd-113">Before you begin</span></span>

<span data-ttu-id="4a5fd-114">Det här scenariot förutsätter att du redan har följt stegen i [skapa en Nätverksbevakaren](network-watcher-create.md) att skapa en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-114">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

<span data-ttu-id="4a5fd-115">En lista över stöds gateway typer besök [stöd för Gateway-typer](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span><span class="sxs-lookup"><span data-stu-id="4a5fd-115">For a list of supported gateway types visit, [Supported Gateway types](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span></span>

## <a name="overview"></a><span data-ttu-id="4a5fd-116">Översikt</span><span class="sxs-lookup"><span data-stu-id="4a5fd-116">Overview</span></span>

<span data-ttu-id="4a5fd-117">Felsökning av resursen ger möjlighet felsöka problem som uppstår med virtuella Nätverksgatewayer och anslutningar.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-117">Resource troubleshooting provides the ability troubleshoot issues that arise with Virtual Network Gateways and Connections.</span></span> <span data-ttu-id="4a5fd-118">När en begäran skickas till resursen felsökning, loggar som efterfrågas och kontrolleras.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-118">When a request is made to resource troubleshooting, logs are being queried and inspected.</span></span> <span data-ttu-id="4a5fd-119">Resultaten returneras när kontroll har slutförts.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-119">When inspection is complete, the results are returned.</span></span> <span data-ttu-id="4a5fd-120">Resursen felsökning förfrågningar är tidskrävande begäranden, vilket kan ta flera minuter att returnera ett resultat.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-120">Resource troubleshooting requests are long running requests, which could take multiple minutes to return a result.</span></span> <span data-ttu-id="4a5fd-121">Loggar från felsökning lagras i en behållare för ett lagringskonto som har angetts.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-121">The logs from troubleshooting are stored in a container on a storage account that is specified.</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="4a5fd-122">Hämta Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="4a5fd-122">Retrieve Network Watcher</span></span>

<span data-ttu-id="4a5fd-123">Det första steget är att hämta Nätverksbevakaren-instans.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-123">The first step is to retrieve the Network Watcher instance.</span></span> <span data-ttu-id="4a5fd-124">Den `$networkWatcher` variabel har skickats till den `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet i steg 4.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-124">The `$networkWatcher` variable is passed to the `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet in step 4.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="retrieve-a-virtual-network-gateway-connection"></a><span data-ttu-id="4a5fd-125">Hämta en Gateway för virtuell nätverksanslutning</span><span class="sxs-lookup"><span data-stu-id="4a5fd-125">Retrieve a Virtual Network Gateway Connection</span></span>

<span data-ttu-id="4a5fd-126">I det här exemplet är resurs felsökning som kördes på en anslutning.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-126">In this example, resource troubleshooting is being ran on a Connection.</span></span> <span data-ttu-id="4a5fd-127">Du kan också flytta den virtuella Nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-127">You can also pass it a Virtual Network Gateway.</span></span>

```powershell
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name "2to3" -ResourceGroupName "testrg"
```

## <a name="create-a-storage-account"></a><span data-ttu-id="4a5fd-128">skapar ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="4a5fd-128">Create a storage account</span></span>

<span data-ttu-id="4a5fd-129">Felsökning av resursen returnerar data om hälsotillståndet för resursen, sparas även loggar till ett lagringskonto som ska granskas.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-129">Resource troubleshooting returns data about the health of the resource, it also saves logs to a storage account to be reviewed.</span></span> <span data-ttu-id="4a5fd-130">I det här steget ska vi skapa ett lagringskonto, om det finns ett befintligt lagringskonto kan du använda den.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-130">In this step, we create a storage account, if an existing storage account exists you can use it.</span></span>

```powershell
$sa = New-AzureRmStorageAccount -Name "contosoexamplesa" -SKU "Standard_LRS" -ResourceGroupName "testrg" -Location "WestCentralUS"
Set-AzureRmCurrentStorageAccount -ResourceGroupName $sa.ResourceGroupName -Name $sa.StorageAccountName
$sc = New-AzureStorageContainer -Name logs
```

## <a name="run-network-watcher-resource-troubleshooting"></a><span data-ttu-id="4a5fd-131">Kör Nätverksbevakaren resurs felsökning</span><span class="sxs-lookup"><span data-stu-id="4a5fd-131">Run Network Watcher resource troubleshooting</span></span>

<span data-ttu-id="4a5fd-132">Felsökning av resurser med den `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-132">You troubleshoot resources with the `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet.</span></span> <span data-ttu-id="4a5fd-133">Vi skickar cmdlet objektet Nätverksbevakaren, Id för anslutning eller virtuell nätverksgateway, storage-konto-id och sökvägen för att lagra resultaten.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-133">We pass the cmdlet the Network Watcher object, the Id of the Connection or Virtual Network Gateway, the storage account id, and the path to store the results.</span></span>

> [!NOTE]
> <span data-ttu-id="4a5fd-134">Den `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet är långvariga och kan ta några minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-134">The `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet is long running and may take a few minutes to complete.</span></span>

```powershell
Start-AzureRmNetworkWatcherResourceTroubleshooting -NetworkWatcher $networkWatcher -TargetResourceId $connection.Id -StorageId $sa.Id -StoragePath "$($sa.PrimaryEndpoints.Blob)$($sc.name)"
```

<span data-ttu-id="4a5fd-135">När du kör cmdleten, granskar Nätverksbevakaren resurs för att kontrollera hälsotillståndet.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-135">Once you run the cmdlet, Network Watcher reviews the resource to verify the health.</span></span> <span data-ttu-id="4a5fd-136">Den returnerar resultaten till shell och lagrar loggar av resultaten i storage-konto som har angetts.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-136">It returns the results to the shell and stores logs of the results in the storage account specified.</span></span>

## <a name="understanding-the-results"></a><span data-ttu-id="4a5fd-137">Förstå resultaten</span><span class="sxs-lookup"><span data-stu-id="4a5fd-137">Understanding the results</span></span>

<span data-ttu-id="4a5fd-138">Åtgärden texten innehåller allmänna råd om hur du löser problemet.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-138">The action text provides general guidance on how to resolve the issue.</span></span> <span data-ttu-id="4a5fd-139">Om en åtgärd kan vidtas för utfärdande, finns en länk med mer information.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-139">If an action can be taken for the issue, a link is provided with additional guidance.</span></span> <span data-ttu-id="4a5fd-140">I fall där det finns inga ytterligare riktlinjer, svaret innehåller URL: en för att öppna ett supportärende.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-140">In the case where there is no additional guidance, the response provides the url to open a support case.</span></span>  <span data-ttu-id="4a5fd-141">Mer information om egenskaperna för svaret och vad som ingår finns [nätverk Watcher Felsökning: översikt](network-watcher-troubleshoot-overview.md)</span><span class="sxs-lookup"><span data-stu-id="4a5fd-141">For more information about the properties of the response and what is included, visit [Network Watcher Troubleshoot overview](network-watcher-troubleshoot-overview.md)</span></span>

<span data-ttu-id="4a5fd-142">Anvisningar för att hämta filer från azure storage-konton, referera till [komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="4a5fd-142">For instructions on downloading files from azure storage accounts, refer to [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="4a5fd-143">Ett annat verktyg som kan användas är Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-143">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="4a5fd-144">Mer information om Lagringsutforskaren hittar du här på följande länk: [Lagringsutforskaren](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="4a5fd-144">More information about Storage Explorer can be found here at the following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

## <a name="next-steps"></a><span data-ttu-id="4a5fd-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4a5fd-145">Next steps</span></span>

<span data-ttu-id="4a5fd-146">Om inställningarna har ändrats som stoppa VPN-anslutning, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) att spåra de grupp och säkerhet Nätverkssäkerhetsregler som kan vara i fråga.</span><span class="sxs-lookup"><span data-stu-id="4a5fd-146">If settings have been changed that stop VPN connectivity, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) to track down the network security group and security rules that may be in question.</span></span>
