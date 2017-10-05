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
ms.openlocfilehash: e135e719d8f267f6a189d0f8a903a49980a5a035
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-powershell"></a><span data-ttu-id="f1b00-103">Felsöka virtuella nätverksgateway och anslutningar med hjälp av Azure Network Watcher PowerShell</span><span class="sxs-lookup"><span data-stu-id="f1b00-103">Troubleshoot Virtual Network Gateway and Connections using Azure Network Watcher PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="f1b00-104">Portal</span><span class="sxs-lookup"><span data-stu-id="f1b00-104">Portal</span></span>](network-watcher-troubleshoot-manage-portal.md)
> - [<span data-ttu-id="f1b00-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f1b00-105">PowerShell</span></span>](network-watcher-troubleshoot-manage-powershell.md)
> - [<span data-ttu-id="f1b00-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f1b00-106">CLI 1.0</span></span>](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [<span data-ttu-id="f1b00-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f1b00-107">CLI 2.0</span></span>](network-watcher-troubleshoot-manage-cli.md)
> - [<span data-ttu-id="f1b00-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="f1b00-108">REST API</span></span>](network-watcher-troubleshoot-manage-rest.md)

<span data-ttu-id="f1b00-109">Nätverksbevakaren innehåller många funktioner relateras till att förstå nätverksresurserna i Azure.</span><span class="sxs-lookup"><span data-stu-id="f1b00-109">Network Watcher provides many capabilities as it relates to understanding your network resources in Azure.</span></span> <span data-ttu-id="f1b00-110">En av dessa funktioner är resurs felsökning.</span><span class="sxs-lookup"><span data-stu-id="f1b00-110">One of these capabilities is resource troubleshooting.</span></span> <span data-ttu-id="f1b00-111">Felsökning av resursen kan anropas via portalen, PowerShell, CLI eller REST API.</span><span class="sxs-lookup"><span data-stu-id="f1b00-111">Resource troubleshooting can be called through the portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="f1b00-112">När den anropas, Nätverksbevakaren kontrollerar hälsotillståndet för en virtuell nätverksgateway eller en anslutning och returnerar resultatet.</span><span class="sxs-lookup"><span data-stu-id="f1b00-112">When called, Network Watcher inspects the health of a Virtual Network Gateway or a Connection and returns its findings.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="f1b00-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="f1b00-113">Before you begin</span></span>

<span data-ttu-id="f1b00-114">Det här scenariot förutsätter att du redan har följt stegen i [skapa en Nätverksbevakaren](network-watcher-create.md) att skapa en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="f1b00-114">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

<span data-ttu-id="f1b00-115">En lista över stöds gateway typer besök [stöd för Gateway-typer](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span><span class="sxs-lookup"><span data-stu-id="f1b00-115">For a list of supported gateway types visit, [Supported Gateway types](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span></span>

## <a name="overview"></a><span data-ttu-id="f1b00-116">Översikt</span><span class="sxs-lookup"><span data-stu-id="f1b00-116">Overview</span></span>

<span data-ttu-id="f1b00-117">Felsökning av resursen ger möjlighet felsöka problem som uppstår med virtuella Nätverksgatewayer och anslutningar.</span><span class="sxs-lookup"><span data-stu-id="f1b00-117">Resource troubleshooting provides the ability troubleshoot issues that arise with Virtual Network Gateways and Connections.</span></span> <span data-ttu-id="f1b00-118">När en begäran skickas till resursen felsökning, loggar som efterfrågas och kontrolleras.</span><span class="sxs-lookup"><span data-stu-id="f1b00-118">When a request is made to resource troubleshooting, logs are being queried and inspected.</span></span> <span data-ttu-id="f1b00-119">Resultaten returneras när kontroll har slutförts.</span><span class="sxs-lookup"><span data-stu-id="f1b00-119">When inspection is complete, the results are returned.</span></span> <span data-ttu-id="f1b00-120">Resursen felsökning förfrågningar är tidskrävande begäranden, vilket kan ta flera minuter att returnera ett resultat.</span><span class="sxs-lookup"><span data-stu-id="f1b00-120">Resource troubleshooting requests are long running requests, which could take multiple minutes to return a result.</span></span> <span data-ttu-id="f1b00-121">Loggar från felsökning lagras i en behållare för ett lagringskonto som har angetts.</span><span class="sxs-lookup"><span data-stu-id="f1b00-121">The logs from troubleshooting are stored in a container on a storage account that is specified.</span></span>

## <a name="troubleshoot-a-gateway-or-connection"></a><span data-ttu-id="f1b00-122">Felsöka en gateway eller -anslutning</span><span class="sxs-lookup"><span data-stu-id="f1b00-122">Troubleshoot a gateway or connection</span></span>

1. <span data-ttu-id="f1b00-123">Navigera till den [Azure-portalen](https://portal.azure.com) och på **nätverk** > **Nätverksbevakaren** > **VPN-diagnostik**</span><span class="sxs-lookup"><span data-stu-id="f1b00-123">Navigate to the [Azure portal](https://portal.azure.com) and click **Networking** > **Network Watcher** > **VPN Diagnostics**</span></span>
2. <span data-ttu-id="f1b00-124">Välj en **prenumeration**, **resursgruppen**, och **plats**.</span><span class="sxs-lookup"><span data-stu-id="f1b00-124">Select a **Subscription**, **Resource Group**, and **Location**.</span></span>
3. <span data-ttu-id="f1b00-125">Felsökning av resursen returnerar data om hälsotillståndet för resursen.</span><span class="sxs-lookup"><span data-stu-id="f1b00-125">Resource troubleshooting returns data about the health of the resource.</span></span> <span data-ttu-id="f1b00-126">Här sparas även relevanta loggar till ett lagringskonto som ska granskas.</span><span class="sxs-lookup"><span data-stu-id="f1b00-126">It also saves relevant logs to a storage account to be reviewed.</span></span> <span data-ttu-id="f1b00-127">Klicka på **Lagringskonto** att välja ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f1b00-127">Click **Storage Account** to select a storage account.</span></span>
4. <span data-ttu-id="f1b00-128">På den **lagringskonton** bladet Välj ett befintligt lagringskonto eller klicka på **+ lagringskonto** att skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="f1b00-128">On the **Storage accounts** blade, select an existing storage account or click **+ Storage account** to create a new one.</span></span>
5. <span data-ttu-id="f1b00-129">På den **behållare** bladet Välj en befintlig behållare eller klicka på **+ behållare** att skapa en ny behållare.</span><span class="sxs-lookup"><span data-stu-id="f1b00-129">On the **Containers** blade, choose an existing container or click **+ Container** to create a new container.</span></span> <span data-ttu-id="f1b00-130">När du är klar klickar du på **Välj**</span><span class="sxs-lookup"><span data-stu-id="f1b00-130">When complete click **Select**</span></span>
6. <span data-ttu-id="f1b00-131">Välj Gateway och anslutning till resurser för att felsöka och klicka på **Starta felsökning**</span><span class="sxs-lookup"><span data-stu-id="f1b00-131">Select the Gateway and Connection resources to troubleshoot and click **Start Troubleshooting**</span></span>

<span data-ttu-id="f1b00-132">Om du har valt flera resurser körs felsökning samtidigt på gateway-resurser.</span><span class="sxs-lookup"><span data-stu-id="f1b00-132">If multiple resources are selected, troubleshooting is run concurrently on gateway resources.</span></span> <span data-ttu-id="f1b00-133">Det kan inte köras på en anslutning och dess tillhörande gateway på samma gång.</span><span class="sxs-lookup"><span data-stu-id="f1b00-133">It can not run on a connection and it's associated gateway at the same time.</span></span> <span data-ttu-id="f1b00-134">Det rekommenderas att felsöka gateways först anslutningen felsökning är en längre process.</span><span class="sxs-lookup"><span data-stu-id="f1b00-134">It is recommended to troubleshoot gateways first as connection troubleshooting is a longer process.</span></span> <span data-ttu-id="f1b00-135">När VPN-diagnostik körs på en resurs i **felsökning STATUS** kolumnen visar statusen **körs**</span><span class="sxs-lookup"><span data-stu-id="f1b00-135">While VPN Diagnostics is running on a resource the **TROUBLESHOOTING STATUS** column will show a status of **Running**</span></span>

<span data-ttu-id="f1b00-136">När du är klar i statuskolumnen ändras till **felfri** eller **ohälsosam**.</span><span class="sxs-lookup"><span data-stu-id="f1b00-136">When complete, the status column changes to **Healthy** or **Unhealthy**.</span></span>

![Felsöka klar][2]

## <a name="understanding-the-results"></a><span data-ttu-id="f1b00-138">Förstå resultaten</span><span class="sxs-lookup"><span data-stu-id="f1b00-138">Understanding the results</span></span>

<span data-ttu-id="f1b00-139">I den **information** i fönstret, den **Status** visar status för senaste felsökning kör på den valda resursen.</span><span class="sxs-lookup"><span data-stu-id="f1b00-139">In the **Details** section of the window, the **Status** tab shows the status of the last troubleshooting run on the selected resource.</span></span> <span data-ttu-id="f1b00-140">Senaste diagnostiken skall visas xx minuter efter den senaste körningen.</span><span class="sxs-lookup"><span data-stu-id="f1b00-140">Results of the latest diagnostic are shown xx minutes after the last run.</span></span>

|<span data-ttu-id="f1b00-141">Egenskap</span><span class="sxs-lookup"><span data-stu-id="f1b00-141">Property</span></span>  |<span data-ttu-id="f1b00-142">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f1b00-142">Description</span></span>  |
|---------|---------|
|<span data-ttu-id="f1b00-143">Resurs</span><span class="sxs-lookup"><span data-stu-id="f1b00-143">Resource</span></span>     | <span data-ttu-id="f1b00-144">En länk till resursen.</span><span class="sxs-lookup"><span data-stu-id="f1b00-144">A link to the resource.</span></span>        |
|<span data-ttu-id="f1b00-145">Sökvägen till lagring</span><span class="sxs-lookup"><span data-stu-id="f1b00-145">Storage path</span></span>     |  <span data-ttu-id="f1b00-146">Sökvägen till lagringskontot och behållare som innehåller loggarna (om någon har producerats under kör).</span><span class="sxs-lookup"><span data-stu-id="f1b00-146">Path to the storage account and container that contain the logs (if any were produced during the run).</span></span> <span data-ttu-id="f1b00-147">Den här inställningen sparas inte när du lämnar portalen.</span><span class="sxs-lookup"><span data-stu-id="f1b00-147">This setting does not persist after you leave the portal.</span></span>        |
|<span data-ttu-id="f1b00-148">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="f1b00-148">Summary</span></span>     | <span data-ttu-id="f1b00-149">Sammanfattning av hälsotillstånd för resursen.</span><span class="sxs-lookup"><span data-stu-id="f1b00-149">Summary of the resource health.</span></span>        |
|<span data-ttu-id="f1b00-150">Information om</span><span class="sxs-lookup"><span data-stu-id="f1b00-150">Detail</span></span>     | <span data-ttu-id="f1b00-151">Detaljerad information om hälsotillståndet för resursen.</span><span class="sxs-lookup"><span data-stu-id="f1b00-151">Detailed information on the resource health.</span></span>        |
|<span data-ttu-id="f1b00-152">Senaste körning</span><span class="sxs-lookup"><span data-stu-id="f1b00-152">Last run</span></span>     | <span data-ttu-id="f1b00-153">Den tid som den senaste gången felsökning har körts.</span><span class="sxs-lookup"><span data-stu-id="f1b00-153">The time the last time troubleshooting was ran.</span></span>        |


<span data-ttu-id="f1b00-154">Den **åtgärd** fliken innehåller allmänna råd om hur du löser problemet.</span><span class="sxs-lookup"><span data-stu-id="f1b00-154">The **Action** tab provides general guidance on how to resolve the issue.</span></span> <span data-ttu-id="f1b00-155">Om en åtgärd kan vidtas för utfärdande, finns en länk med mer information.</span><span class="sxs-lookup"><span data-stu-id="f1b00-155">If an action can be taken for the issue, a link is provided with additional guidance.</span></span> <span data-ttu-id="f1b00-156">I fall där det finns inga ytterligare riktlinjer, svaret innehåller URL: en för att öppna ett supportärende.</span><span class="sxs-lookup"><span data-stu-id="f1b00-156">In the case where there is no additional guidance, the response provides the url to open a support case.</span></span>  <span data-ttu-id="f1b00-157">Mer information om egenskaperna för svaret och vad som ingår finns [nätverk Watcher Felsökning: översikt](network-watcher-troubleshoot-overview.md)</span><span class="sxs-lookup"><span data-stu-id="f1b00-157">For more information about the properties of the response and what is included, visit [Network Watcher Troubleshoot overview](network-watcher-troubleshoot-overview.md)</span></span>


## <a name="next-steps"></a><span data-ttu-id="f1b00-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f1b00-158">Next steps</span></span>

<span data-ttu-id="f1b00-159">Om inställningarna har ändrats som stoppa VPN-anslutning, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) att spåra de grupp och säkerhet Nätverkssäkerhetsregler som kan vara i fråga.</span><span class="sxs-lookup"><span data-stu-id="f1b00-159">If settings have been changed that stop VPN connectivity, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) to track down the network security group and security rules that may be in question.</span></span>


[2]: ./media/network-watcher-troubleshoot-manage-portal/2.png
