---
title: "aaaTroubleshoot Azure virtuell nätverksgateway och anslutningar - PowerShell | Microsoft Docs"
description: "Den här sidan förklarar hur toouse hello Azure Nätverksbevakaren felsöka PowerShell-cmdlet"
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
ms.openlocfilehash: b7dbe6e44e0fa1ea0481223a84098d7a57c068a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-powershell"></a><span data-ttu-id="dc2eb-103">Felsöka virtuella nätverksgateway och anslutningar med hjälp av Azure Network Watcher PowerShell</span><span class="sxs-lookup"><span data-stu-id="dc2eb-103">Troubleshoot Virtual Network Gateway and Connections using Azure Network Watcher PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="dc2eb-104">Portal</span><span class="sxs-lookup"><span data-stu-id="dc2eb-104">Portal</span></span>](network-watcher-troubleshoot-manage-portal.md)
> - [<span data-ttu-id="dc2eb-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="dc2eb-105">PowerShell</span></span>](network-watcher-troubleshoot-manage-powershell.md)
> - [<span data-ttu-id="dc2eb-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="dc2eb-106">CLI 1.0</span></span>](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [<span data-ttu-id="dc2eb-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="dc2eb-107">CLI 2.0</span></span>](network-watcher-troubleshoot-manage-cli.md)
> - [<span data-ttu-id="dc2eb-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="dc2eb-108">REST API</span></span>](network-watcher-troubleshoot-manage-rest.md)

<span data-ttu-id="dc2eb-109">Nätverksbevakaren innehåller många funktioner när det gäller toounderstanding nätverksresurserna i Azure.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-109">Network Watcher provides many capabilities as it relates toounderstanding your network resources in Azure.</span></span> <span data-ttu-id="dc2eb-110">En av dessa funktioner är resurs felsökning.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-110">One of these capabilities is resource troubleshooting.</span></span> <span data-ttu-id="dc2eb-111">Felsökning av resursen kan anropas via hello portal, PowerShell, CLI eller REST API.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-111">Resource troubleshooting can be called through hello portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="dc2eb-112">När den anropas, Nätverksbevakaren inspektera hello hälsotillståndet för en virtuell nätverksgateway eller en anslutning och returnerar resultatet.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-112">When called, Network Watcher inspects hello health of a Virtual Network Gateway or a Connection and returns its findings.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="dc2eb-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="dc2eb-113">Before you begin</span></span>

<span data-ttu-id="dc2eb-114">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

<span data-ttu-id="dc2eb-115">En lista över stöds gateway typer besök [stöd för Gateway-typer](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span><span class="sxs-lookup"><span data-stu-id="dc2eb-115">For a list of supported gateway types visit, [Supported Gateway types](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span></span>

## <a name="overview"></a><span data-ttu-id="dc2eb-116">Översikt</span><span class="sxs-lookup"><span data-stu-id="dc2eb-116">Overview</span></span>

<span data-ttu-id="dc2eb-117">Felsökning av resursen ger hello möjlighet felsöka problem som uppstår med virtuella Nätverksgatewayer och anslutningar.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-117">Resource troubleshooting provides hello ability troubleshoot issues that arise with Virtual Network Gateways and Connections.</span></span> <span data-ttu-id="dc2eb-118">När en förfrågan görs tooresource felsökning, loggar som efterfrågas och kontrolleras.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-118">When a request is made tooresource troubleshooting, logs are being queried and inspected.</span></span> <span data-ttu-id="dc2eb-119">Hello resultat returneras när kontroll har slutförts.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-119">When inspection is complete, hello results are returned.</span></span> <span data-ttu-id="dc2eb-120">Resursen felsökning förfrågningar är tidskrävande begäranden, vilket kan ta flera minuter tooreturn ett resultat.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-120">Resource troubleshooting requests are long running requests, which could take multiple minutes tooreturn a result.</span></span> <span data-ttu-id="dc2eb-121">hello loggar från felsökning lagras i en behållare för ett lagringskonto som har angetts.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-121">hello logs from troubleshooting are stored in a container on a storage account that is specified.</span></span>

## <a name="retrieve-network-watcher"></a><span data-ttu-id="dc2eb-122">Hämta Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="dc2eb-122">Retrieve Network Watcher</span></span>

<span data-ttu-id="dc2eb-123">hello första steget är tooretrieve hello Nätverksbevakaren instans.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-123">hello first step is tooretrieve hello Network Watcher instance.</span></span> <span data-ttu-id="dc2eb-124">Hej `$networkWatcher` variabel har skickats toohello `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet i steg 4.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-124">hello `$networkWatcher` variable is passed toohello `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet in step 4.</span></span>

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="retrieve-a-virtual-network-gateway-connection"></a><span data-ttu-id="dc2eb-125">Hämta en Gateway för virtuell nätverksanslutning</span><span class="sxs-lookup"><span data-stu-id="dc2eb-125">Retrieve a Virtual Network Gateway Connection</span></span>

<span data-ttu-id="dc2eb-126">I det här exemplet är resurs felsökning som kördes på en anslutning.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-126">In this example, resource troubleshooting is being ran on a Connection.</span></span> <span data-ttu-id="dc2eb-127">Du kan också flytta den virtuella Nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-127">You can also pass it a Virtual Network Gateway.</span></span>

```powershell
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name "2to3" -ResourceGroupName "testrg"
```

## <a name="create-a-storage-account"></a><span data-ttu-id="dc2eb-128">skapar ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="dc2eb-128">Create a storage account</span></span>

<span data-ttu-id="dc2eb-129">Felsökning av resursen returnerar data om hello hälsotillstånd hello resurs, sparas även loggar tooa storage-konto toobe ses över.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-129">Resource troubleshooting returns data about hello health of hello resource, it also saves logs tooa storage account toobe reviewed.</span></span> <span data-ttu-id="dc2eb-130">I det här steget ska vi skapa ett lagringskonto, om det finns ett befintligt lagringskonto kan du använda den.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-130">In this step, we create a storage account, if an existing storage account exists you can use it.</span></span>

```powershell
$sa = New-AzureRmStorageAccount -Name "contosoexamplesa" -SKU "Standard_LRS" -ResourceGroupName "testrg" -Location "WestCentralUS"
Set-AzureRmCurrentStorageAccount -ResourceGroupName $sa.ResourceGroupName -Name $sa.StorageAccountName
$sc = New-AzureStorageContainer -Name logs
```

## <a name="run-network-watcher-resource-troubleshooting"></a><span data-ttu-id="dc2eb-131">Kör Nätverksbevakaren resurs felsökning</span><span class="sxs-lookup"><span data-stu-id="dc2eb-131">Run Network Watcher resource troubleshooting</span></span>

<span data-ttu-id="dc2eb-132">Felsökning av resurser med hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-132">You troubleshoot resources with hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet.</span></span> <span data-ttu-id="dc2eb-133">Vi skicka hello adfsclaimruleset hello Nätverksbevakaren, hello-Id för hello anslutning eller virtuell nätverksgateway, hello lagring konto-id och hello sökvägen toostore hello resultat.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-133">We pass hello cmdlet hello Network Watcher object, hello Id of hello Connection or Virtual Network Gateway, hello storage account id, and hello path toostore hello results.</span></span>

> [!NOTE]
> <span data-ttu-id="dc2eb-134">Hej `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet är långvariga och kan ta några minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-134">hello `Start-AzureRmNetworkWatcherResourceTroubleshooting` cmdlet is long running and may take a few minutes toocomplete.</span></span>

```powershell
Start-AzureRmNetworkWatcherResourceTroubleshooting -NetworkWatcher $networkWatcher -TargetResourceId $connection.Id -StorageId $sa.Id -StoragePath "$($sa.PrimaryEndpoints.Blob)$($sc.name)"
```

<span data-ttu-id="dc2eb-135">När du kör cmdlet hello granskar Nätverksbevakaren hello resurshälsa tooverify hello.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-135">Once you run hello cmdlet, Network Watcher reviews hello resource tooverify hello health.</span></span> <span data-ttu-id="dc2eb-136">Den returnerar hello resultat toohello shell och lagrar loggar hello resultat i hello storage-konto anges.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-136">It returns hello results toohello shell and stores logs of hello results in hello storage account specified.</span></span>

## <a name="understanding-hello-results"></a><span data-ttu-id="dc2eb-137">Förstå hello resultat</span><span class="sxs-lookup"><span data-stu-id="dc2eb-137">Understanding hello results</span></span>

<span data-ttu-id="dc2eb-138">hello åtgärd texten innehåller allmänna råd om hur tooresolve hello problemet.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-138">hello action text provides general guidance on how tooresolve hello issue.</span></span> <span data-ttu-id="dc2eb-139">Om en åtgärd kan vidtas för hello problemet, tillhandahålls en länk med mer information.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-139">If an action can be taken for hello issue, a link is provided with additional guidance.</span></span> <span data-ttu-id="dc2eb-140">I hello fall där det finns inga ytterligare riktlinjer, hello svaret innehåller hello url tooopen ett supportärende.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-140">In hello case where there is no additional guidance, hello response provides hello url tooopen a support case.</span></span>  <span data-ttu-id="dc2eb-141">Mer information om hello egenskaper för hello svar och vad som ingår finns [nätverk Watcher Felsökning: översikt](network-watcher-troubleshoot-overview.md)</span><span class="sxs-lookup"><span data-stu-id="dc2eb-141">For more information about hello properties of hello response and what is included, visit [Network Watcher Troubleshoot overview](network-watcher-troubleshoot-overview.md)</span></span>

<span data-ttu-id="dc2eb-142">Anvisningar för hämtning av filer från azure storage-konton finns för[komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="dc2eb-142">For instructions on downloading files from azure storage accounts, refer too[Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="dc2eb-143">Ett annat verktyg som kan användas är Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-143">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="dc2eb-144">Mer information om Lagringsutforskaren hittar du här på hello följande länk: [Lagringsutforskaren](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="dc2eb-144">More information about Storage Explorer can be found here at hello following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

## <a name="next-steps"></a><span data-ttu-id="dc2eb-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dc2eb-145">Next steps</span></span>

<span data-ttu-id="dc2eb-146">Om inställningarna har ändrats som stoppa VPN-anslutning, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack ned hello grupp och säkerhet Nätverkssäkerhetsregler som kan vara i fråga.</span><span class="sxs-lookup"><span data-stu-id="dc2eb-146">If settings have been changed that stop VPN connectivity, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that may be in question.</span></span>
