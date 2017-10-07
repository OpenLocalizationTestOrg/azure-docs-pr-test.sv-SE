---
title: "aaaManage nätverk grupp flödet säkerhetsloggar med Azure Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan förklarar hur toomanage Nätverkssäkerhetsgruppen flöde loggar i Azure Nätverksbevakaren"
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
ms.openlocfilehash: fb250337ab9d1a0c0d0d3569c00bc221dd102a3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-network-security-group-flow-logs-in-hello-azure-portal"></a><span data-ttu-id="d2535-103">Hantera nätverket grupp flödet säkerhetsloggar i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d2535-103">Manage network security group flow logs in hello Azure portal</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="d2535-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d2535-104">Azure portal</span></span>](network-watcher-nsg-flow-logging-portal.md)
> - [<span data-ttu-id="d2535-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d2535-105">PowerShell</span></span>](network-watcher-nsg-flow-logging-powershell.md)
> - [<span data-ttu-id="d2535-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="d2535-106">CLI 1.0</span></span>](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [<span data-ttu-id="d2535-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="d2535-107">CLI 2.0</span></span>](network-watcher-nsg-flow-logging-cli.md)
> - [<span data-ttu-id="d2535-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="d2535-108">REST API</span></span>](network-watcher-nsg-flow-logging-rest.md)

<span data-ttu-id="d2535-109">Nätverket säkerhetsloggar grupp flöde är en funktion i Nätverksbevakaren som gör att du tooview information om ingående och utgående IP-trafik via en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="d2535-109">Network security group flow logs are a feature of Network Watcher that enables you tooview information about ingress and egress IP traffic through a network security group.</span></span> <span data-ttu-id="d2535-110">Loggarna flödet skrivs i JSON-format och innehåller viktig information, inklusive:</span><span class="sxs-lookup"><span data-stu-id="d2535-110">These flow logs are written in JSON format and provide important information, including:</span></span> 

- <span data-ttu-id="d2535-111">Utgående och inkommande flöden på grundval av per regel.</span><span class="sxs-lookup"><span data-stu-id="d2535-111">Outbound and inbound flows on a per-rule basis.</span></span>
- <span data-ttu-id="d2535-112">hello NIC som hello flödet gäller.</span><span class="sxs-lookup"><span data-stu-id="d2535-112">hello NIC that hello flow applies to.</span></span>
- <span data-ttu-id="d2535-113">5-tuppel information om hello flödet (källan/målet IP, källan/målet port och protokoll).</span><span class="sxs-lookup"><span data-stu-id="d2535-113">5-tuple information about hello flow (source/destination IP, source/destination port, protocol).</span></span>
- <span data-ttu-id="d2535-114">Information om huruvida trafik tillåtas eller nekas.</span><span class="sxs-lookup"><span data-stu-id="d2535-114">Information about whether traffic was allowed or denied.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="d2535-115">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="d2535-115">Before you begin</span></span>

<span data-ttu-id="d2535-116">Det här scenariot förutsätter att du redan har följt stegen hello i [skapa en instans av Nätverksbevakaren](network-watcher-create.md).</span><span class="sxs-lookup"><span data-stu-id="d2535-116">This scenario assumes you have already followed hello steps in [Create a Network Watcher instance](network-watcher-create.md).</span></span> <span data-ttu-id="d2535-117">hello scenariot förutsätter att du har en resursgrupp med en giltig virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d2535-117">hello scenario also assumes that a you have a resource group with a valid virtual machine.</span></span>

## <a name="register-insights-provider"></a><span data-ttu-id="d2535-118">Registrera providern insikter</span><span class="sxs-lookup"><span data-stu-id="d2535-118">Register Insights provider</span></span>

<span data-ttu-id="d2535-119">För flöde loggning toowork, hello **Microsoft.Insights** provider måste vara registrerad.</span><span class="sxs-lookup"><span data-stu-id="d2535-119">For flow logging toowork successfully, hello **Microsoft.Insights** provider must be registered.</span></span> <span data-ttu-id="d2535-120">tooregister hello provider, vidta hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d2535-120">tooregister hello provider, take hello following steps:</span></span> 

1. <span data-ttu-id="d2535-121">Gå för**prenumerationer**, och välj sedan hello prenumeration som du vill tooenable flöde loggar.</span><span class="sxs-lookup"><span data-stu-id="d2535-121">Go too**Subscriptions**, and then select hello subscription for which you want tooenable flow logs.</span></span> 
2. <span data-ttu-id="d2535-122">På hello **prenumeration** bladet väljer **Resursproviders**.</span><span class="sxs-lookup"><span data-stu-id="d2535-122">On hello **Subscription** blade, select **Resource Providers**.</span></span> 
3. <span data-ttu-id="d2535-123">Titta på hello lista över leverantörer och kontrollera att hello **microsoft.insights** providern är registrerad.</span><span class="sxs-lookup"><span data-stu-id="d2535-123">Look at hello list of providers, and verify that hello **microsoft.insights** provider is registered.</span></span> <span data-ttu-id="d2535-124">Om inte, välj sedan **registrera**.</span><span class="sxs-lookup"><span data-stu-id="d2535-124">If not, then select **Register**.</span></span>

![Visa providers][providers]

## <a name="enable-flow-logs"></a><span data-ttu-id="d2535-126">Aktivera flödet loggar</span><span class="sxs-lookup"><span data-stu-id="d2535-126">Enable flow logs</span></span>

<span data-ttu-id="d2535-127">Dessa steg leder dig genom hello innebär att aktivera flödet loggar på en nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="d2535-127">These steps take you through hello process of enabling flow logs on a network security group.</span></span>

### <a name="step-1"></a><span data-ttu-id="d2535-128">Steg 1</span><span class="sxs-lookup"><span data-stu-id="d2535-128">Step 1</span></span>

<span data-ttu-id="d2535-129">Gå tooa Nätverksbevakaren instansen och välj sedan **NSG flöda loggar**.</span><span class="sxs-lookup"><span data-stu-id="d2535-129">Go tooa Network Watcher instance, and then select **NSG Flow logs**.</span></span>

![Översikt över flödet-loggar][1]

### <a name="step-2"></a><span data-ttu-id="d2535-131">Steg 2</span><span class="sxs-lookup"><span data-stu-id="d2535-131">Step 2</span></span>

<span data-ttu-id="d2535-132">Välj en nätverkssäkerhetsgrupp hello-listan.</span><span class="sxs-lookup"><span data-stu-id="d2535-132">Select a network security group from hello list.</span></span>

![Översikt över flödet-loggar][2]

### <a name="step-3"></a><span data-ttu-id="d2535-134">Steg 3</span><span class="sxs-lookup"><span data-stu-id="d2535-134">Step 3</span></span> 

<span data-ttu-id="d2535-135">På hello **flöde loggar inställningar** bladet ange hello status för**på**, och konfigurera ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d2535-135">On hello **Flow logs settings** blade, set hello status too**On**, and then configure a storage account.</span></span>  <span data-ttu-id="d2535-136">När du är klar väljer du **OK**.</span><span class="sxs-lookup"><span data-stu-id="d2535-136">When you're done, select **OK**.</span></span> <span data-ttu-id="d2535-137">Välj sedan **spara**.</span><span class="sxs-lookup"><span data-stu-id="d2535-137">Then select **Save**.</span></span>

![Översikt över flödet-loggar][3]

## <a name="download-flow-logs"></a><span data-ttu-id="d2535-139">Hämta flödet loggar</span><span class="sxs-lookup"><span data-stu-id="d2535-139">Download flow logs</span></span>

<span data-ttu-id="d2535-140">Flödet loggar sparas i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d2535-140">Flow logs are saved in a storage account.</span></span> <span data-ttu-id="d2535-141">Hämta din flödet loggar tooview dem.</span><span class="sxs-lookup"><span data-stu-id="d2535-141">Download your flow logs tooview them.</span></span>

### <a name="step-1"></a><span data-ttu-id="d2535-142">Steg 1</span><span class="sxs-lookup"><span data-stu-id="d2535-142">Step 1</span></span>

<span data-ttu-id="d2535-143">Välj toodownload flöde loggar **kan du hämta flödet loggar från konfigurerade lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="d2535-143">toodownload flow logs, select **You can download flow logs from configured storage accounts**.</span></span> <span data-ttu-id="d2535-144">Det här steget anser du tooa storage-konto där du kan välja vilka loggar toodownload.</span><span class="sxs-lookup"><span data-stu-id="d2535-144">This step takes you tooa storage account view where you can choose which logs toodownload.</span></span>

![Flöda inställningarna för webbprogramloggar][4]

### <a name="step-2"></a><span data-ttu-id="d2535-146">Steg 2</span><span class="sxs-lookup"><span data-stu-id="d2535-146">Step 2</span></span>

<span data-ttu-id="d2535-147">Gå toohello rätt storage-konto.</span><span class="sxs-lookup"><span data-stu-id="d2535-147">Go toohello correct storage account.</span></span> <span data-ttu-id="d2535-148">Välj sedan **behållare** > **insights-log-networksecuritygroupflowevent**.</span><span class="sxs-lookup"><span data-stu-id="d2535-148">Then select **Containers** > **insights-log-networksecuritygroupflowevent**.</span></span>

![Flöda inställningarna för webbprogramloggar][5]

### <a name="step-3"></a><span data-ttu-id="d2535-150">Steg 3</span><span class="sxs-lookup"><span data-stu-id="d2535-150">Step 3</span></span>

<span data-ttu-id="d2535-151">Gå toohello plats hello flödet loggen markerar du den och välj sedan **hämta**.</span><span class="sxs-lookup"><span data-stu-id="d2535-151">Go toohello location of hello flow log, select it, and then select **Download**.</span></span>

![Flöda inställningarna för webbprogramloggar][6]

<span data-ttu-id="d2535-153">Information om hello struktur hello loggen finns [nätverk grupp flödet loggen Säkerhetsöversikt](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d2535-153">For information about hello structure of hello log, visit [Network security group flow log overview](network-watcher-nsg-flow-logging-overview.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d2535-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d2535-154">Next steps</span></span>

<span data-ttu-id="d2535-155">Lär dig hur för[visualisera dina NSG flödet loggar med PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="d2535-155">Learn how too[visualize your NSG flow logs with PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md).</span></span>

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-portal/figure1.png
[2]: ./media/network-watcher-nsg-flow-logging-portal/figure2.png
[3]: ./media/network-watcher-nsg-flow-logging-portal/figure3.png
[4]: ./media/network-watcher-nsg-flow-logging-portal/figure4.png
[5]: ./media/network-watcher-nsg-flow-logging-portal/figure5.png
[6]: ./media/network-watcher-nsg-flow-logging-portal/figure6.png
[providers]: ./media/network-watcher-nsg-flow-logging-portal/providers.png
