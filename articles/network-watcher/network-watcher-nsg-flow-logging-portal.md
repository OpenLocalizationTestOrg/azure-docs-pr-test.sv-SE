---
title: "Hantera nätverket grupp flödet säkerhetsloggar med Azure Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan förklarar hur du hanterar Nätverkssäkerhetsgruppen flöde loggar i Azure Nätverksbevakaren"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 01606cbf-d70b-40ad-bc1d-f03bb642e0af
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 41cb5ffab9bd3a3bed75ffdb6a7383ca1690f810
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-network-security-group-flow-logs-in-the-azure-portal"></a><span data-ttu-id="31313-103">Hantera nätverket grupp flödet säkerhetsloggar i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="31313-103">Manage network security group flow logs in the Azure portal</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="31313-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="31313-104">Azure portal</span></span>](network-watcher-nsg-flow-logging-portal.md)
> - [<span data-ttu-id="31313-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="31313-105">PowerShell</span></span>](network-watcher-nsg-flow-logging-powershell.md)
> - [<span data-ttu-id="31313-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="31313-106">CLI 1.0</span></span>](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [<span data-ttu-id="31313-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="31313-107">CLI 2.0</span></span>](network-watcher-nsg-flow-logging-cli.md)
> - [<span data-ttu-id="31313-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="31313-108">REST API</span></span>](network-watcher-nsg-flow-logging-rest.md)

<span data-ttu-id="31313-109">Nätverket säkerhetsloggar grupp flöde är en funktion i Nätverksbevakaren där du kan visa information om ingående och utgående IP-trafik via en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="31313-109">Network security group flow logs are a feature of Network Watcher that enables you to view information about ingress and egress IP traffic through a network security group.</span></span> <span data-ttu-id="31313-110">Loggarna flödet skrivs i JSON-format och innehåller viktig information, inklusive:</span><span class="sxs-lookup"><span data-stu-id="31313-110">These flow logs are written in JSON format and provide important information, including:</span></span> 

- <span data-ttu-id="31313-111">Utgående och inkommande flöden på grundval av per regel.</span><span class="sxs-lookup"><span data-stu-id="31313-111">Outbound and inbound flows on a per-rule basis.</span></span>
- <span data-ttu-id="31313-112">Nätverkskortet som flödet gäller för.</span><span class="sxs-lookup"><span data-stu-id="31313-112">The NIC that the flow applies to.</span></span>
- <span data-ttu-id="31313-113">5-tuppel information om flödet (källan/målet IP, källan/målet port och protokoll).</span><span class="sxs-lookup"><span data-stu-id="31313-113">5-tuple information about the flow (source/destination IP, source/destination port, protocol).</span></span>
- <span data-ttu-id="31313-114">Information om huruvida trafik tillåtas eller nekas.</span><span class="sxs-lookup"><span data-stu-id="31313-114">Information about whether traffic was allowed or denied.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="31313-115">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="31313-115">Before you begin</span></span>

<span data-ttu-id="31313-116">Det här scenariot förutsätter att du redan har följt stegen i [skapa en instans av Nätverksbevakaren](network-watcher-create.md).</span><span class="sxs-lookup"><span data-stu-id="31313-116">This scenario assumes you have already followed the steps in [Create a Network Watcher instance](network-watcher-create.md).</span></span> <span data-ttu-id="31313-117">Det här scenariot förutsätter att du har en resursgrupp med en giltig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="31313-117">The scenario also assumes that a you have a resource group with a valid virtual machine.</span></span>

## <a name="register-insights-provider"></a><span data-ttu-id="31313-118">Registrera providern insikter</span><span class="sxs-lookup"><span data-stu-id="31313-118">Register Insights provider</span></span>

<span data-ttu-id="31313-119">För flöde loggning för att fungera korrekt, den **Microsoft.Insights** provider måste vara registrerad.</span><span class="sxs-lookup"><span data-stu-id="31313-119">For flow logging to work successfully, the **Microsoft.Insights** provider must be registered.</span></span> <span data-ttu-id="31313-120">Gör följande om du vill registrera providern:</span><span class="sxs-lookup"><span data-stu-id="31313-120">To register the provider, take the following steps:</span></span> 

1. <span data-ttu-id="31313-121">Gå till **prenumerationer**, och sedan väljer du den prenumeration som du vill aktivera flödet loggar.</span><span class="sxs-lookup"><span data-stu-id="31313-121">Go to **Subscriptions**, and then select the subscription for which you want to enable flow logs.</span></span> 
2. <span data-ttu-id="31313-122">På den **prenumeration** bladet väljer **Resursproviders**.</span><span class="sxs-lookup"><span data-stu-id="31313-122">On the **Subscription** blade, select **Resource Providers**.</span></span> 
3. <span data-ttu-id="31313-123">Titta på listan med providers och kontrollera att den **microsoft.insights** providern är registrerad.</span><span class="sxs-lookup"><span data-stu-id="31313-123">Look at the list of providers, and verify that the **microsoft.insights** provider is registered.</span></span> <span data-ttu-id="31313-124">Om inte, välj sedan **registrera**.</span><span class="sxs-lookup"><span data-stu-id="31313-124">If not, then select **Register**.</span></span>

![Visa providers][providers]

## <a name="enable-flow-logs"></a><span data-ttu-id="31313-126">Aktivera flödet loggar</span><span class="sxs-lookup"><span data-stu-id="31313-126">Enable flow logs</span></span>

<span data-ttu-id="31313-127">Dessa steg leder dig genom processen för att aktivera flödet loggar på en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="31313-127">These steps take you through the process of enabling flow logs on a network security group.</span></span>

### <a name="step-1"></a><span data-ttu-id="31313-128">Steg 1</span><span class="sxs-lookup"><span data-stu-id="31313-128">Step 1</span></span>

<span data-ttu-id="31313-129">Gå till en Nätverksbevakaren-instans och välj sedan **NSG flöda loggar**.</span><span class="sxs-lookup"><span data-stu-id="31313-129">Go to a Network Watcher instance, and then select **NSG Flow logs**.</span></span>

![Översikt över flödet-loggar][1]

### <a name="step-2"></a><span data-ttu-id="31313-131">Steg 2</span><span class="sxs-lookup"><span data-stu-id="31313-131">Step 2</span></span>

<span data-ttu-id="31313-132">Välj en säkerhetsgrupp för nätverk i listan.</span><span class="sxs-lookup"><span data-stu-id="31313-132">Select a network security group from the list.</span></span>

![Översikt över flödet-loggar][2]

### <a name="step-3"></a><span data-ttu-id="31313-134">Steg 3</span><span class="sxs-lookup"><span data-stu-id="31313-134">Step 3</span></span> 

<span data-ttu-id="31313-135">På den **flöde loggar inställningar** bladet Ange status till **på**, och konfigurera ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="31313-135">On the **Flow logs settings** blade, set the status to **On**, and then configure a storage account.</span></span>  <span data-ttu-id="31313-136">När du är klar väljer du **OK**.</span><span class="sxs-lookup"><span data-stu-id="31313-136">When you're done, select **OK**.</span></span> <span data-ttu-id="31313-137">Välj sedan **spara**.</span><span class="sxs-lookup"><span data-stu-id="31313-137">Then select **Save**.</span></span>

![Översikt över flödet-loggar][3]

## <a name="download-flow-logs"></a><span data-ttu-id="31313-139">Hämta flödet loggar</span><span class="sxs-lookup"><span data-stu-id="31313-139">Download flow logs</span></span>

<span data-ttu-id="31313-140">Flödet loggar sparas i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="31313-140">Flow logs are saved in a storage account.</span></span> <span data-ttu-id="31313-141">Hämta loggarna om du vill visa dem flödet.</span><span class="sxs-lookup"><span data-stu-id="31313-141">Download your flow logs to view them.</span></span>

### <a name="step-1"></a><span data-ttu-id="31313-142">Steg 1</span><span class="sxs-lookup"><span data-stu-id="31313-142">Step 1</span></span>

<span data-ttu-id="31313-143">Om du vill hämta loggar flödet, Välj **kan du hämta flödet loggar från konfigurerade lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="31313-143">To download flow logs, select **You can download flow logs from configured storage accounts**.</span></span> <span data-ttu-id="31313-144">Det här steget kommer du till vyn storage-konto där du kan välja vilka loggarna om du vill hämta.</span><span class="sxs-lookup"><span data-stu-id="31313-144">This step takes you to a storage account view where you can choose which logs to download.</span></span>

![Flöda inställningarna för webbprogramloggar][4]

### <a name="step-2"></a><span data-ttu-id="31313-146">Steg 2</span><span class="sxs-lookup"><span data-stu-id="31313-146">Step 2</span></span>

<span data-ttu-id="31313-147">Gå till rätt storage-konto.</span><span class="sxs-lookup"><span data-stu-id="31313-147">Go to the correct storage account.</span></span> <span data-ttu-id="31313-148">Välj sedan **behållare** > **insights-log-networksecuritygroupflowevent**.</span><span class="sxs-lookup"><span data-stu-id="31313-148">Then select **Containers** > **insights-log-networksecuritygroupflowevent**.</span></span>

![Flöda inställningarna för webbprogramloggar][5]

### <a name="step-3"></a><span data-ttu-id="31313-150">Steg 3</span><span class="sxs-lookup"><span data-stu-id="31313-150">Step 3</span></span>

<span data-ttu-id="31313-151">Gå till platsen för loggen flödet, markerar du den och välj sedan **hämta**.</span><span class="sxs-lookup"><span data-stu-id="31313-151">Go to the location of the flow log, select it, and then select **Download**.</span></span>

![Flöda inställningarna för webbprogramloggar][6]

<span data-ttu-id="31313-153">Information om strukturen i loggen finns [nätverk grupp flödet loggen Säkerhetsöversikt](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="31313-153">For information about the structure of the log, visit [Network security group flow log overview](network-watcher-nsg-flow-logging-overview.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="31313-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="31313-154">Next steps</span></span>

<span data-ttu-id="31313-155">Lär dig hur du [visualisera dina NSG flödet loggar med PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="31313-155">Learn how to [visualize your NSG flow logs with PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md).</span></span>

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-portal/figure1.png
[2]: ./media/network-watcher-nsg-flow-logging-portal/figure2.png
[3]: ./media/network-watcher-nsg-flow-logging-portal/figure3.png
[4]: ./media/network-watcher-nsg-flow-logging-portal/figure4.png
[5]: ./media/network-watcher-nsg-flow-logging-portal/figure5.png
[6]: ./media/network-watcher-nsg-flow-logging-portal/figure6.png
[providers]: ./media/network-watcher-nsg-flow-logging-portal/providers.png
