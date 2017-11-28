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
ms.openlocfilehash: bd568d34091209390c57d22475559bdb99ad2c59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-azure-network-watcher-powershell"></a><span data-ttu-id="4369b-103">Felsöka virtuella nätverksgateway och anslutningar med hjälp av Azure Network Watcher PowerShell</span><span class="sxs-lookup"><span data-stu-id="4369b-103">Troubleshoot Virtual Network Gateway and Connections using Azure Network Watcher PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="4369b-104">Portal</span><span class="sxs-lookup"><span data-stu-id="4369b-104">Portal</span></span>](network-watcher-troubleshoot-manage-portal.md)
> - [<span data-ttu-id="4369b-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4369b-105">PowerShell</span></span>](network-watcher-troubleshoot-manage-powershell.md)
> - [<span data-ttu-id="4369b-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="4369b-106">CLI 1.0</span></span>](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [<span data-ttu-id="4369b-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="4369b-107">CLI 2.0</span></span>](network-watcher-troubleshoot-manage-cli.md)
> - [<span data-ttu-id="4369b-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="4369b-108">REST API</span></span>](network-watcher-troubleshoot-manage-rest.md)

<span data-ttu-id="4369b-109">Nätverksbevakaren innehåller många funktioner när det gäller toounderstanding nätverksresurserna i Azure.</span><span class="sxs-lookup"><span data-stu-id="4369b-109">Network Watcher provides many capabilities as it relates toounderstanding your network resources in Azure.</span></span> <span data-ttu-id="4369b-110">En av dessa funktioner är resurs felsökning.</span><span class="sxs-lookup"><span data-stu-id="4369b-110">One of these capabilities is resource troubleshooting.</span></span> <span data-ttu-id="4369b-111">Felsökning av resursen kan anropas via hello portal, PowerShell, CLI eller REST API.</span><span class="sxs-lookup"><span data-stu-id="4369b-111">Resource troubleshooting can be called through hello portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="4369b-112">När den anropas, Nätverksbevakaren inspektera hello hälsotillståndet för en virtuell nätverksgateway eller en anslutning och returnerar resultatet.</span><span class="sxs-lookup"><span data-stu-id="4369b-112">When called, Network Watcher inspects hello health of a Virtual Network Gateway or a Connection and returns its findings.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="4369b-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="4369b-113">Before you begin</span></span>

<span data-ttu-id="4369b-114">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en Nätverksbevakaren](network-watcher-create.md) toocreate en Nätverksbevakaren.</span><span class="sxs-lookup"><span data-stu-id="4369b-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

<span data-ttu-id="4369b-115">En lista över stöds gateway typer besök [stöd för Gateway-typer](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span><span class="sxs-lookup"><span data-stu-id="4369b-115">For a list of supported gateway types visit, [Supported Gateway types](network-watcher-troubleshoot-overview.md#supported-gateway-types).</span></span>

## <a name="overview"></a><span data-ttu-id="4369b-116">Översikt</span><span class="sxs-lookup"><span data-stu-id="4369b-116">Overview</span></span>

<span data-ttu-id="4369b-117">Felsökning av resursen ger hello möjlighet felsöka problem som uppstår med virtuella Nätverksgatewayer och anslutningar.</span><span class="sxs-lookup"><span data-stu-id="4369b-117">Resource troubleshooting provides hello ability troubleshoot issues that arise with Virtual Network Gateways and Connections.</span></span> <span data-ttu-id="4369b-118">När en förfrågan görs tooresource felsökning, loggar som efterfrågas och kontrolleras.</span><span class="sxs-lookup"><span data-stu-id="4369b-118">When a request is made tooresource troubleshooting, logs are being queried and inspected.</span></span> <span data-ttu-id="4369b-119">Hello resultat returneras när kontroll har slutförts.</span><span class="sxs-lookup"><span data-stu-id="4369b-119">When inspection is complete, hello results are returned.</span></span> <span data-ttu-id="4369b-120">Resursen felsökning förfrågningar är tidskrävande begäranden, vilket kan ta flera minuter tooreturn ett resultat.</span><span class="sxs-lookup"><span data-stu-id="4369b-120">Resource troubleshooting requests are long running requests, which could take multiple minutes tooreturn a result.</span></span> <span data-ttu-id="4369b-121">hello loggar från felsökning lagras i en behållare för ett lagringskonto som har angetts.</span><span class="sxs-lookup"><span data-stu-id="4369b-121">hello logs from troubleshooting are stored in a container on a storage account that is specified.</span></span>

## <a name="troubleshoot-a-gateway-or-connection"></a><span data-ttu-id="4369b-122">Felsöka en gateway eller -anslutning</span><span class="sxs-lookup"><span data-stu-id="4369b-122">Troubleshoot a gateway or connection</span></span>

1. <span data-ttu-id="4369b-123">Navigera toohello [Azure-portalen](https://portal.azure.com) och på **nätverk** > **Nätverksbevakaren** > **VPN-diagnostik**</span><span class="sxs-lookup"><span data-stu-id="4369b-123">Navigate toohello [Azure portal](https://portal.azure.com) and click **Networking** > **Network Watcher** > **VPN Diagnostics**</span></span>
2. <span data-ttu-id="4369b-124">Välj en **prenumeration**, **resursgruppen**, och **plats**.</span><span class="sxs-lookup"><span data-stu-id="4369b-124">Select a **Subscription**, **Resource Group**, and **Location**.</span></span>
3. <span data-ttu-id="4369b-125">Felsökning av resursen returnerar data om hello hälsotillstånd hello resurs.</span><span class="sxs-lookup"><span data-stu-id="4369b-125">Resource troubleshooting returns data about hello health of hello resource.</span></span> <span data-ttu-id="4369b-126">Här sparas även relevanta loggar tooa lagringskonto toobe ses över.</span><span class="sxs-lookup"><span data-stu-id="4369b-126">It also saves relevant logs tooa storage account toobe reviewed.</span></span> <span data-ttu-id="4369b-127">Klicka på **Lagringskonto** tooselect ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="4369b-127">Click **Storage Account** tooselect a storage account.</span></span>
4. <span data-ttu-id="4369b-128">På hello **lagringskonton** bladet Välj ett befintligt lagringskonto eller klicka på **+ lagringskonto** toocreate en ny.</span><span class="sxs-lookup"><span data-stu-id="4369b-128">On hello **Storage accounts** blade, select an existing storage account or click **+ Storage account** toocreate a new one.</span></span>
5. <span data-ttu-id="4369b-129">På hello **behållare** bladet Välj en befintlig behållare eller klicka på **+ behållare** toocreate en ny behållare.</span><span class="sxs-lookup"><span data-stu-id="4369b-129">On hello **Containers** blade, choose an existing container or click **+ Container** toocreate a new container.</span></span> <span data-ttu-id="4369b-130">När du är klar klickar du på **Välj**</span><span class="sxs-lookup"><span data-stu-id="4369b-130">When complete click **Select**</span></span>
6. <span data-ttu-id="4369b-131">Välj tootroubleshoot för hello Gateway och anslutning till resurser och klicka på **Starta felsökning**</span><span class="sxs-lookup"><span data-stu-id="4369b-131">Select hello Gateway and Connection resources tootroubleshoot and click **Start Troubleshooting**</span></span>

<span data-ttu-id="4369b-132">Om du har valt flera resurser körs felsökning samtidigt på gateway-resurser.</span><span class="sxs-lookup"><span data-stu-id="4369b-132">If multiple resources are selected, troubleshooting is run concurrently on gateway resources.</span></span> <span data-ttu-id="4369b-133">Det kan inte köras på en anslutning och dess tillhörande gatewayen på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="4369b-133">It can not run on a connection and it's associated gateway at hello same time.</span></span> <span data-ttu-id="4369b-134">Det rekommenderas tootroubleshoot gateways först anslutningen felsökning är en längre process.</span><span class="sxs-lookup"><span data-stu-id="4369b-134">It is recommended tootroubleshoot gateways first as connection troubleshooting is a longer process.</span></span> <span data-ttu-id="4369b-135">När VPN-diagnostik körs på en resurs hello **felsökning STATUS** kolumnen visar statusen **körs**</span><span class="sxs-lookup"><span data-stu-id="4369b-135">While VPN Diagnostics is running on a resource hello **TROUBLESHOOTING STATUS** column will show a status of **Running**</span></span>

<span data-ttu-id="4369b-136">När du är färdig hello kolumnen statusändringar för**felfri** eller **ohälsosam**.</span><span class="sxs-lookup"><span data-stu-id="4369b-136">When complete, hello status column changes too**Healthy** or **Unhealthy**.</span></span>

![Felsöka klar][2]

## <a name="understanding-hello-results"></a><span data-ttu-id="4369b-138">Förstå hello resultat</span><span class="sxs-lookup"><span data-stu-id="4369b-138">Understanding hello results</span></span>

<span data-ttu-id="4369b-139">I hello **information** avsnitt av hello fönstret hello **Status** fliken visar hello status hello senaste felsökning kör på hello valda resursen.</span><span class="sxs-lookup"><span data-stu-id="4369b-139">In hello **Details** section of hello window, hello **Status** tab shows hello status of hello last troubleshooting run on hello selected resource.</span></span> <span data-ttu-id="4369b-140">Resultat av hello senaste diagnostiken är visas xx minuter efter hello senast kördes.</span><span class="sxs-lookup"><span data-stu-id="4369b-140">Results of hello latest diagnostic are shown xx minutes after hello last run.</span></span>

|<span data-ttu-id="4369b-141">Egenskap</span><span class="sxs-lookup"><span data-stu-id="4369b-141">Property</span></span>  |<span data-ttu-id="4369b-142">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4369b-142">Description</span></span>  |
|---------|---------|
|<span data-ttu-id="4369b-143">Resurs</span><span class="sxs-lookup"><span data-stu-id="4369b-143">Resource</span></span>     | <span data-ttu-id="4369b-144">En länk toohello resurs.</span><span class="sxs-lookup"><span data-stu-id="4369b-144">A link toohello resource.</span></span>        |
|<span data-ttu-id="4369b-145">Sökvägen till lagring</span><span class="sxs-lookup"><span data-stu-id="4369b-145">Storage path</span></span>     |  <span data-ttu-id="4369b-146">Sökvägen toohello storage-konto och en behållare som innehåller hello-loggar (om någon har producerats under hello kör).</span><span class="sxs-lookup"><span data-stu-id="4369b-146">Path toohello storage account and container that contain hello logs (if any were produced during hello run).</span></span> <span data-ttu-id="4369b-147">Den här inställningen sparas inte när du lämnar hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="4369b-147">This setting does not persist after you leave hello portal.</span></span>        |
|<span data-ttu-id="4369b-148">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="4369b-148">Summary</span></span>     | <span data-ttu-id="4369b-149">Sammanfattning av hello resurshälsa.</span><span class="sxs-lookup"><span data-stu-id="4369b-149">Summary of hello resource health.</span></span>        |
|<span data-ttu-id="4369b-150">Information om</span><span class="sxs-lookup"><span data-stu-id="4369b-150">Detail</span></span>     | <span data-ttu-id="4369b-151">Detaljerad information om hello resurshälsa.</span><span class="sxs-lookup"><span data-stu-id="4369b-151">Detailed information on hello resource health.</span></span>        |
|<span data-ttu-id="4369b-152">Senaste körning</span><span class="sxs-lookup"><span data-stu-id="4369b-152">Last run</span></span>     | <span data-ttu-id="4369b-153">hello tid hello senaste gången felsökning har körts.</span><span class="sxs-lookup"><span data-stu-id="4369b-153">hello time hello last time troubleshooting was ran.</span></span>        |


<span data-ttu-id="4369b-154">Hej **åtgärd** fliken innehåller allmänna råd om hur tooresolve hello problemet.</span><span class="sxs-lookup"><span data-stu-id="4369b-154">hello **Action** tab provides general guidance on how tooresolve hello issue.</span></span> <span data-ttu-id="4369b-155">Om en åtgärd kan vidtas för hello problemet, tillhandahålls en länk med mer information.</span><span class="sxs-lookup"><span data-stu-id="4369b-155">If an action can be taken for hello issue, a link is provided with additional guidance.</span></span> <span data-ttu-id="4369b-156">I hello fall där det finns inga ytterligare riktlinjer, hello svaret innehåller hello url tooopen ett supportärende.</span><span class="sxs-lookup"><span data-stu-id="4369b-156">In hello case where there is no additional guidance, hello response provides hello url tooopen a support case.</span></span>  <span data-ttu-id="4369b-157">Mer information om hello egenskaper för hello svar och vad som ingår finns [nätverk Watcher Felsökning: översikt](network-watcher-troubleshoot-overview.md)</span><span class="sxs-lookup"><span data-stu-id="4369b-157">For more information about hello properties of hello response and what is included, visit [Network Watcher Troubleshoot overview](network-watcher-troubleshoot-overview.md)</span></span>


## <a name="next-steps"></a><span data-ttu-id="4369b-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4369b-158">Next steps</span></span>

<span data-ttu-id="4369b-159">Om inställningarna har ändrats som stoppa VPN-anslutning, se [hantera Nätverkssäkerhetsgrupper](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack ned hello grupp och säkerhet Nätverkssäkerhetsregler som kan vara i fråga.</span><span class="sxs-lookup"><span data-stu-id="4369b-159">If settings have been changed that stop VPN connectivity, see [Manage Network Security Groups](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack down hello network security group and security rules that may be in question.</span></span>


[2]: ./media/network-watcher-troubleshoot-manage-portal/2.png
