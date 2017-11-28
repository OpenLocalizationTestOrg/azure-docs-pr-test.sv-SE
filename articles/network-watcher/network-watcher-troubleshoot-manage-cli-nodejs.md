---
title: "Felsöka Azure virtuell nätverksgateway och anslutningar - Azure CLI 1.0 | Microsoft Docs"
description: "Den här sidan förklarar hur du använder Azure Nätverksbevakaren felsöka Azure CLI 1.0"
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
ms.openlocfilehash: 9de4b2a0bdda7ffbd269883877a708d67312092f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-azure-cli-10"></a><span data-ttu-id="73a26-103">Felsöka virtuella nätverksgateway och anslutningar med hjälp av Azure Network Watcher Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="73a26-103">Troubleshoot Virtual Network Gateway and Connections using Azure Network Watcher Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="73a26-104">Portal</span><span class="sxs-lookup"><span data-stu-id="73a26-104">Portal</span></span>](network-watcher-troubleshoot-manage-portal.md)
> - [<span data-ttu-id="73a26-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="73a26-105">PowerShell</span></span>](network-watcher-troubleshoot-manage-powershell.md)
> - [<span data-ttu-id="73a26-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="73a26-106">CLI 1.0</span></span>](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [<span data-ttu-id="73a26-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="73a26-107">CLI 2.0</span></span>](network-watcher-troubleshoot-manage-cli.md)
> - [<span data-ttu-id="73a26-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="73a26-108">REST API</span></span>](network-watcher-troubleshoot-manage-rest.md)

<span data-ttu-id="73a26-109">Nätverksbevakaren innehåller många funktioner relateras till att förstå nätverksresurserna i Azure.</span><span class="sxs-lookup"><span data-stu-id="73a26-109">Network Watcher provides many capabilities as it relates to understanding your network resources in Azure.</span></span> <span data-ttu-id="73a26-110">En av dessa funktioner är resurs felsökning.</span><span class="sxs-lookup"><span data-stu-id="73a26-110">One of these capabilities is resource troubleshooting.</span></span> <span data-ttu-id="73a26-111">Felsökning av resursen kan anropas via portalen, PowerShell, CLI eller REST API.</span><span class="sxs-lookup"><span data-stu-id="73a26-111">Resource troubleshooting can be called through the portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="73a26-112">När den anropas, Nätverksbevakaren kontrollerar hälsotillståndet för en virtuell nätverksgateway eller en anslutning och returnerar resultatet.</span><span class="sxs-lookup"><span data-stu-id="73a26-112">When called, Network Watcher inspects the health of a Virtual Network Gateway or a Connection and returns its findings.</span></span>

<span data-ttu-id="73a26-113">Den här artikeln använder plattformsoberoende Azure CLI version 1.0, som är tillgänglig för Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="73a26-113">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="73a26-114">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="73a26-114">Before you begin</span></span>

<span data-ttu-id="73a26-115">Det här scenariot förutsätter att du redan har följt stegen i [skapa en Nätverksbevakaren](network-watcher-create.md) att skapa en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="73a26-115">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

<span data-ttu-id="73a26-116">En lista över stöds gateway typer besök [stöd för Gateway-typer](/network-watcher-troubleshoot-overview.md#supported-gateway-types).</span><span class="sxs-lookup"><span data-stu-id="73a26-116">For a list of supported gateway types visit, [Supported Gateway types](/network-watcher-troubleshoot-overview.md#supported-gateway-types).</span></span>

## <a name="overview"></a><span data-ttu-id="73a26-117">Översikt</span><span class="sxs-lookup"><span data-stu-id="73a26-117">Overview</span></span>

<span data-ttu-id="73a26-118">Felsökning av resursen ger möjlighet felsöka problem som uppstår med virtuella Nätverksgatewayer och anslutningar.</span><span class="sxs-lookup"><span data-stu-id="73a26-118">Resource troubleshooting provides the ability troubleshoot issues that arise with Virtual Network Gateways and Connections.</span></span> <span data-ttu-id="73a26-119">När en begäran skickas till resursen felsökning, loggar som efterfrågas och kontrolleras.</span><span class="sxs-lookup"><span data-stu-id="73a26-119">When a request is made to resource troubleshooting, logs are being queried and inspected.</span></span> <span data-ttu-id="73a26-120">Resultaten returneras när kontroll har slutförts.</span><span class="sxs-lookup"><span data-stu-id="73a26-120">When inspection is complete, the results are returned.</span></span> <span data-ttu-id="73a26-121">Resursen felsökning förfrågningar är tidskrävande begäranden, vilket kan ta flera minuter att returnera ett resultat.</span><span class="sxs-lookup"><span data-stu-id="73a26-121">Resource troubleshooting requests are long running requests, which could take multiple minutes to return a result.</span></span> <span data-ttu-id="73a26-122">Loggar från felsökning lagras i en behållare för ett lagringskonto som har angetts.</span><span class="sxs-lookup"><span data-stu-id="73a26-122">The logs from troubleshooting are stored in a container on a storage account that is specified.</span></span>

## <a name="retrieve-a-virtual-network-gateway-connection"></a><span data-ttu-id="73a26-123">Hämta en Gateway för virtuell nätverksanslutning</span><span class="sxs-lookup"><span data-stu-id="73a26-123">Retrieve a Virtual Network Gateway Connection</span></span>

<span data-ttu-id="73a26-124">I det här exemplet är resurs felsökning som kördes på en anslutning.</span><span class="sxs-lookup"><span data-stu-id="73a26-124">In this example, resource troubleshooting is being ran on a Connection.</span></span> <span data-ttu-id="73a26-125">Du kan också flytta den virtuella Nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="73a26-125">You can also pass it a Virtual Network Gateway.</span></span> <span data-ttu-id="73a26-126">Följande cmdlet visar vpn-anslutningar i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="73a26-126">The following cmdlet lists the vpn-connections in a resource group.</span></span>

```azurecli
azure network vpn-connection list -g resourceGroupName
```

<span data-ttu-id="73a26-127">Du kan också köra kommandot för att se anslutningar i en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="73a26-127">You can also run the command to see the connections in a subscription.</span></span>

```azurecli
azure network vpn-connection list -s subscription
```

<span data-ttu-id="73a26-128">När du har namnet på anslutningen kan köra du det här kommandot för att hämta dess resurs-Id:</span><span class="sxs-lookup"><span data-stu-id="73a26-128">Once you have the name of the connection, you can run this command to get its resource Id:</span></span>

```azurecli
azure network vpn-connection show -g resourceGroupName -n connectionName
```

## <a name="create-a-storage-account"></a><span data-ttu-id="73a26-129">skapar ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="73a26-129">Create a storage account</span></span>

<span data-ttu-id="73a26-130">Felsökning av resursen returnerar data om hälsotillståndet för resursen, sparas även loggar till ett lagringskonto som ska granskas.</span><span class="sxs-lookup"><span data-stu-id="73a26-130">Resource troubleshooting returns data about the health of the resource, it also saves logs to a storage account to be reviewed.</span></span> <span data-ttu-id="73a26-131">I det här steget ska vi skapa ett lagringskonto, om det finns ett befintligt lagringskonto kan du använda den.</span><span class="sxs-lookup"><span data-stu-id="73a26-131">In this step, we create a storage account, if an existing storage account exists you can use it.</span></span>

1. <span data-ttu-id="73a26-132">Skapa lagringskontot</span><span class="sxs-lookup"><span data-stu-id="73a26-132">Create the storage account</span></span>

    ```azurecli
    azure storage account create -n storageAccountName -l location -g resourceGroupName
    ```

1. <span data-ttu-id="73a26-133">Hämta nycklar för lagringskonto</span><span class="sxs-lookup"><span data-stu-id="73a26-133">Get the storage account keys</span></span>

    ```azurecli
    azure storage account keys list storageAccountName -g resourcegroupName
    ```

1. <span data-ttu-id="73a26-134">Skapa behållaren</span><span class="sxs-lookup"><span data-stu-id="73a26-134">Create the container</span></span>

    ```azurecli
    azure storage container create --account-name storageAccountName -g resourcegroupName --account-key {storageAccountKey} --container logs
    ```

## <a name="run-network-watcher-resource-troubleshooting"></a><span data-ttu-id="73a26-135">Kör Nätverksbevakaren resurs felsökning</span><span class="sxs-lookup"><span data-stu-id="73a26-135">Run Network Watcher resource troubleshooting</span></span>

<span data-ttu-id="73a26-136">Felsökning av resurser med den `network watcher troubleshoot` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="73a26-136">You troubleshoot resources with the `network watcher troubleshoot` cmdlet.</span></span> <span data-ttu-id="73a26-137">Vi skickar cmdlet resursgrupp, namnet på Nätverksbevakaren Id för anslutningen, Id för storage-konto och sökvägen till blob att lagra felsökning resulterar i.</span><span class="sxs-lookup"><span data-stu-id="73a26-137">We pass the cmdlet the resource group, the name of the Network Watcher, the Id of the connection, the Id of the storage account, and the path to the blob to store the troubleshoot results in.</span></span>

```azurecli
azure network watcher troubleshoot -g resourceGroupName -n networkWatcherName -t connectionId -i storageId -p storagePath
```

<span data-ttu-id="73a26-138">När du kör cmdleten, granskar Nätverksbevakaren resurs för att kontrollera hälsotillståndet.</span><span class="sxs-lookup"><span data-stu-id="73a26-138">Once you run the cmdlet, Network Watcher reviews the resource to verify the health.</span></span> <span data-ttu-id="73a26-139">Den returnerar resultaten till shell och lagrar loggar av resultaten i storage-konto som har angetts.</span><span class="sxs-lookup"><span data-stu-id="73a26-139">It returns the results to the shell and stores logs of the results in the storage account specified.</span></span>

## <a name="understanding-the-results"></a><span data-ttu-id="73a26-140">Förstå resultaten</span><span class="sxs-lookup"><span data-stu-id="73a26-140">Understanding the results</span></span>

<span data-ttu-id="73a26-141">Åtgärden texten innehåller allmänna råd om hur du löser problemet.</span><span class="sxs-lookup"><span data-stu-id="73a26-141">The action text provides general guidance on how to resolve the issue.</span></span> <span data-ttu-id="73a26-142">Om en åtgärd kan vidtas för utfärdande, finns en länk med mer information.</span><span class="sxs-lookup"><span data-stu-id="73a26-142">If an action can be taken for the issue, a link is provided with additional guidance.</span></span> <span data-ttu-id="73a26-143">I fall där det finns inga ytterligare riktlinjer, svaret innehåller URL: en för att öppna ett supportärende.</span><span class="sxs-lookup"><span data-stu-id="73a26-143">In the case where there is no additional guidance, the response provides the url to open a support case.</span></span>  <span data-ttu-id="73a26-144">Mer information om egenskaperna för svaret och vad som ingår finns [nätverk Watcher Felsökning: översikt](network-watcher-troubleshoot-overview.md)</span><span class="sxs-lookup"><span data-stu-id="73a26-144">For more information about the properties of the response and what is included, visit [Network Watcher Troubleshoot overview](network-watcher-troubleshoot-overview.md)</span></span>

<span data-ttu-id="73a26-145">Anvisningar för att hämta filer från azure storage-konton, referera till [komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="73a26-145">For instructions on downloading files from azure storage accounts, refer to [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="73a26-146">Ett annat verktyg som kan användas är Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="73a26-146">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="73a26-147">Mer information om Lagringsutforskaren hittar du här på följande länk: [Lagringsutforskaren](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="73a26-147">More information about Storage Explorer can be found here at the following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

## <a name="next-steps"></a><span data-ttu-id="73a26-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="73a26-148">Next steps</span></span>

<span data-ttu-id="73a26-149">Om inställningarna har ändrats som stoppa VPN-anslutning, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) att spåra de grupp och säkerhet Nätverkssäkerhetsregler som kan vara i fråga.</span><span class="sxs-lookup"><span data-stu-id="73a26-149">If settings have been changed that stop VPN connectivity, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) to track down the network security group and security rules that may be in question.</span></span>
