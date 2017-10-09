---
title: "aaaTroubleshoot Azure virtuell nätverksgateway och anslutningar - Azure CLI 2.0 | Microsoft Docs"
description: "Den här sidan förklarar hur toouse hello Azure Nätverksbevakaren felsöka Azure CLI 2.0"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 2838bc61-b182-4da8-8533-27db8fdbd177
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: 389e86f247722412a1d0e2e722dbf80bb7cff291
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-azure-cli-20"></a><span data-ttu-id="fe5ec-103">Felsöka virtuella nätverksgateway och anslutningar med hjälp av Azure Network Watcher Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="fe5ec-103">Troubleshoot Virtual Network Gateway and Connections using Azure Network Watcher Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="fe5ec-104">Portal</span><span class="sxs-lookup"><span data-stu-id="fe5ec-104">Portal</span></span>](network-watcher-troubleshoot-manage-portal.md)
> - [<span data-ttu-id="fe5ec-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="fe5ec-105">PowerShell</span></span>](network-watcher-troubleshoot-manage-powershell.md)
> - [<span data-ttu-id="fe5ec-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="fe5ec-106">CLI 1.0</span></span>](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [<span data-ttu-id="fe5ec-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="fe5ec-107">CLI 2.0</span></span>](network-watcher-troubleshoot-manage-cli.md)
> - [<span data-ttu-id="fe5ec-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="fe5ec-108">REST API</span></span>](network-watcher-troubleshoot-manage-rest.md)

<span data-ttu-id="fe5ec-109">Nätverksbevakaren innehåller många funktioner när det gäller toounderstanding nätverksresurserna i Azure.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-109">Network Watcher provides many capabilities as it relates toounderstanding your network resources in Azure.</span></span> <span data-ttu-id="fe5ec-110">En av dessa funktioner är resurs felsökning.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-110">One of these capabilities is resource troubleshooting.</span></span> <span data-ttu-id="fe5ec-111">Felsökning av resursen kan anropas via hello portal, PowerShell, CLI eller REST API.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-111">Resource troubleshooting can be called through hello portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="fe5ec-112">När den anropas, Nätverksbevakaren inspektera hello hälsotillståndet för en virtuell nätverksgateway eller en anslutning och returnerar resultatet.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-112">When called, Network Watcher inspects hello health of a Virtual Network Gateway or a Connection and returns its findings.</span></span>

<span data-ttu-id="fe5ec-113">Den här artikeln använder våra nästa generations CLI för hello management resursdistributionsmodell, Azure CLI 2.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-113">This article uses our next generation CLI for hello resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="fe5ec-114">tooperform hello stegen i den här artikeln, måste du för[installerar hello Azure-kommandoradsgränssnittet för Mac, Linux och Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="fe5ec-114">tooperform hello steps in this article, you need too[install hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="fe5ec-115">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="fe5ec-115">Before you begin</span></span>

<span data-ttu-id="fe5ec-116">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-116">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

<span data-ttu-id="fe5ec-117">En lista över stöds gateway typer besök [stöd för Gateway-typer](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span><span class="sxs-lookup"><span data-stu-id="fe5ec-117">For a list of supported gateway types visit, [Supported Gateway types](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span></span>

## <a name="overview"></a><span data-ttu-id="fe5ec-118">Översikt</span><span class="sxs-lookup"><span data-stu-id="fe5ec-118">Overview</span></span>

<span data-ttu-id="fe5ec-119">Felsökning av resursen ger hello möjlighet felsöka problem som uppstår med virtuella Nätverksgatewayer och anslutningar.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-119">Resource troubleshooting provides hello ability troubleshoot issues that arise with Virtual Network Gateways and Connections.</span></span> <span data-ttu-id="fe5ec-120">När en förfrågan görs tooresource felsökning, loggar som efterfrågas och kontrolleras.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-120">When a request is made tooresource troubleshooting, logs are being queried and inspected.</span></span> <span data-ttu-id="fe5ec-121">Hello resultat returneras när kontroll har slutförts.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-121">When inspection is complete, hello results are returned.</span></span> <span data-ttu-id="fe5ec-122">Resursen felsökning förfrågningar är tidskrävande begäranden, vilket kan ta flera minuter tooreturn ett resultat.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-122">Resource troubleshooting requests are long running requests, which could take multiple minutes tooreturn a result.</span></span> <span data-ttu-id="fe5ec-123">hello loggar från felsökning lagras i en behållare för ett lagringskonto som har angetts.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-123">hello logs from troubleshooting are stored in a container on a storage account that is specified.</span></span>

## <a name="retrieve-a-virtual-network-gateway-connection"></a><span data-ttu-id="fe5ec-124">Hämta en Gateway för virtuell nätverksanslutning</span><span class="sxs-lookup"><span data-stu-id="fe5ec-124">Retrieve a Virtual Network Gateway Connection</span></span>

<span data-ttu-id="fe5ec-125">I det här exemplet är resurs felsökning som kördes på en anslutning.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-125">In this example, resource troubleshooting is being ran on a Connection.</span></span> <span data-ttu-id="fe5ec-126">Du kan också flytta den virtuella Nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-126">You can also pass it a Virtual Network Gateway.</span></span> <span data-ttu-id="fe5ec-127">hello visar följande cmdlet hello vpn-anslutningar i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-127">hello following cmdlet lists hello vpn-connections in a resource group.</span></span>

```azurecli
az network vpn-connection list --resource-group resourceGroupName
```

<span data-ttu-id="fe5ec-128">När du har hello namnet på hello-anslutningen kan köra du det här kommandot tooget dess resursens Id:</span><span class="sxs-lookup"><span data-stu-id="fe5ec-128">Once you have hello name of hello connection, you can run this command tooget its resource Id:</span></span>

```azurecli
az network vpn-connection show --resource-group resourceGroupName --ids vpnConnectionIds
```

## <a name="create-a-storage-account"></a><span data-ttu-id="fe5ec-129">skapar ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="fe5ec-129">Create a storage account</span></span>

<span data-ttu-id="fe5ec-130">Felsökning av resursen returnerar data om hello hälsotillstånd hello resurs, sparas även loggar tooa storage-konto toobe ses över.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-130">Resource troubleshooting returns data about hello health of hello resource, it also saves logs tooa storage account toobe reviewed.</span></span> <span data-ttu-id="fe5ec-131">I det här steget ska vi skapa ett lagringskonto, om det finns ett befintligt lagringskonto kan du använda den.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-131">In this step, we create a storage account, if an existing storage account exists you can use it.</span></span>

1. <span data-ttu-id="fe5ec-132">Skapa hello storage-konto</span><span class="sxs-lookup"><span data-stu-id="fe5ec-132">Create hello storage account</span></span>

    ```azurecli
    az storage account create --name storageAccountName --location westcentralus --resource-group resourceGroupName --sku Standard_LRS
    ```

1. <span data-ttu-id="fe5ec-133">Hämta hello lagringskontonycklar</span><span class="sxs-lookup"><span data-stu-id="fe5ec-133">Get hello storage account keys</span></span>

    ```azurecli
    az storage account keys list --resource-group resourcegroupName --account-name storageAccountName
    ```

1. <span data-ttu-id="fe5ec-134">Skapa hello behållare</span><span class="sxs-lookup"><span data-stu-id="fe5ec-134">Create hello container</span></span>

    ```azurecli
    az storage container create --account-name storageAccountName --account-key {storageAccountKey} --name logs
    ```

## <a name="run-network-watcher-resource-troubleshooting"></a><span data-ttu-id="fe5ec-135">Kör Nätverksbevakaren resurs felsökning</span><span class="sxs-lookup"><span data-stu-id="fe5ec-135">Run Network Watcher resource troubleshooting</span></span>

<span data-ttu-id="fe5ec-136">Felsökning av resurser med hello `az network watcher troubleshooting` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-136">You troubleshoot resources with hello `az network watcher troubleshooting` cmdlet.</span></span> <span data-ttu-id="fe5ec-137">Vi skickar hello cmdlet hello resursgrupp, hello namnet på hello Nätverksbevakaren hello-Id för hello anslutning hello-Id för hello storage-konto och hello sökvägen toohello blob toostore hello felsöka resulterar i.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-137">We pass hello cmdlet hello resource group, hello name of hello Network Watcher, hello Id of hello connection, hello Id of hello storage account, and hello path toohello blob toostore hello troubleshoot results in.</span></span>

```azurecli
az network watcher troubleshooting start --resource-group resourceGroupName --resource resourceName --resource-type {vnetGateway/vpnConnection} --storage-account storageAccountName  --storage-path https://{storageAccountName}.blob.core.windows.net/{containerName}
```

<span data-ttu-id="fe5ec-138">När du kör cmdlet hello granskar Nätverksbevakaren hello resurshälsa tooverify hello.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-138">Once you run hello cmdlet, Network Watcher reviews hello resource tooverify hello health.</span></span> <span data-ttu-id="fe5ec-139">Den returnerar hello resultat toohello shell och lagrar loggar hello resultat i hello storage-konto anges.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-139">It returns hello results toohello shell and stores logs of hello results in hello storage account specified.</span></span>

## <a name="understanding-hello-results"></a><span data-ttu-id="fe5ec-140">Förstå hello resultat</span><span class="sxs-lookup"><span data-stu-id="fe5ec-140">Understanding hello results</span></span>

<span data-ttu-id="fe5ec-141">hello åtgärd texten innehåller allmänna råd om hur tooresolve hello problemet.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-141">hello action text provides general guidance on how tooresolve hello issue.</span></span> <span data-ttu-id="fe5ec-142">Om en åtgärd kan vidtas för hello problemet, tillhandahålls en länk med mer information.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-142">If an action can be taken for hello issue, a link is provided with additional guidance.</span></span> <span data-ttu-id="fe5ec-143">I hello fall där det finns inga ytterligare riktlinjer, hello svaret innehåller hello url tooopen ett supportärende.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-143">In hello case where there is no additional guidance, hello response provides hello url tooopen a support case.</span></span>  <span data-ttu-id="fe5ec-144">Mer information om hello egenskaper för hello svar och vad som ingår finns [nätverk Watcher Felsökning: översikt](network-watcher-troubleshoot-overview.md)</span><span class="sxs-lookup"><span data-stu-id="fe5ec-144">For more information about hello properties of hello response and what is included, visit [Network Watcher Troubleshoot overview](network-watcher-troubleshoot-overview.md)</span></span>

<span data-ttu-id="fe5ec-145">Anvisningar för hämtning av filer från azure storage-konton finns för[komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="fe5ec-145">For instructions on downloading files from azure storage accounts, refer too[Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="fe5ec-146">Ett annat verktyg som kan användas är Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-146">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="fe5ec-147">Mer information om Lagringsutforskaren hittar du här på hello följande länk: [Lagringsutforskaren](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="fe5ec-147">More information about Storage Explorer can be found here at hello following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe5ec-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fe5ec-148">Next steps</span></span>

<span data-ttu-id="fe5ec-149">Om inställningarna har ändrats som stoppa VPN-anslutning, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack ned hello grupp och säkerhet Nätverkssäkerhetsregler som kan vara i fråga.</span><span class="sxs-lookup"><span data-stu-id="fe5ec-149">If settings have been changed that stop VPN connectivity, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that may be in question.</span></span>
