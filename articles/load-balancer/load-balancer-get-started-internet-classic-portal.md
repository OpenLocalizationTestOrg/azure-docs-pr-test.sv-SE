---
title: "belastningsutjämnaren aaaCreate mot en Internet - Azure klassiska portal | Microsoft Docs"
description: "Lär dig hur toocreate ett Internet belastningsutjämnare i den klassiska modellen med hello klassiska Azure-portalen"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: fa3e93c0-968a-472d-a17c-65665c050db2
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 27b0d5af6e7b493fa94a9dfbfa260483ae95a2fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-hello-azure-classic-portal"></a><span data-ttu-id="767a4-103">Komma igång med en Internetuppkopplad belastningsutjämnare (klassisk) i hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="767a4-103">Get started creating an Internet facing load balancer (classic) in hello Azure classic portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="767a4-104">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="767a4-104">Azure classic portal</span></span>](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [<span data-ttu-id="767a4-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="767a4-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [<span data-ttu-id="767a4-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="767a4-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [<span data-ttu-id="767a4-107">Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="767a4-107">Azure Cloud Services</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="767a4-108">Innan du börjar arbeta med Azure-resurser, är det viktigt toounderstand att Azure har två distributionsmodeller: Azure Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="767a4-108">Before you work with Azure resources, it's important toounderstand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="767a4-109">Se till att du förstår [distributionsmodeller och verktyg](../azure-classic-rm.md) innan du börjar arbeta med Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="767a4-109">Make sure you understand [deployment models and tools](../azure-classic-rm.md) before you work with any Azure resource.</span></span> <span data-ttu-id="767a4-110">Du kan visa hello dokumentationen för olika verktyg genom att klicka på flikarna hello hello överst i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="767a4-110">You can view hello documentation for different tools by clicking hello tabs at hello top of this article.</span></span> <span data-ttu-id="767a4-111">Den här artikeln beskriver hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="767a4-111">This article covers hello classic deployment model.</span></span> <span data-ttu-id="767a4-112">Du kan också [Lär dig hur toocreate Internet-riktade belastningsutjämnaren med Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="767a4-112">You can also [Learn how toocreate an Internet facing load balancer using Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a><span data-ttu-id="767a4-113">Konfigurera en Internetuppkopplad belastningsutjämnare för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="767a4-113">Set up an Internet-facing load balancer for virtual machines</span></span>

<span data-ttu-id="767a4-114">Du måste skapa en belastningsutjämnad uppsättning i ordning tooload saldo nätverkstrafiken från hello Internet över hello virtuella datorer på en tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="767a4-114">In order tooload balance network traffic from hello Internet across hello virtual machines of a cloud service, you must create a load-balanced set.</span></span> <span data-ttu-id="767a4-115">Den här proceduren förutsätter att du redan har skapat hello virtuella datorer och att de är alla inom hello samma molntjänst.</span><span class="sxs-lookup"><span data-stu-id="767a4-115">This procedure assumes that you have already created hello virtual machines and that they are all within hello same cloud service.</span></span>

<span data-ttu-id="767a4-116">**tooconfigure en belastningsutjämnad uppsättning för virtuella datorer**</span><span class="sxs-lookup"><span data-stu-id="767a4-116">**tooconfigure a load-balanced set for virtual machines**</span></span>

1. <span data-ttu-id="767a4-117">I hello klassiska Azure-portalen klickar du på **virtuella datorer**, och klicka sedan på hello namnet på en virtuell dator i hello belastningsutjämnade uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="767a4-117">In hello Azure classic portal, click **Virtual Machines**, and then click hello name of a virtual machine in hello load-balanced set.</span></span>
2. <span data-ttu-id="767a4-118">Klicka på **Slutpunkter** och sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="767a4-118">Click **Endpoints**, and then click **Add**.</span></span>
3. <span data-ttu-id="767a4-119">På hello **lägga till en virtuell dator i slutpunkt tooa** klickar du på hello högerpilen.</span><span class="sxs-lookup"><span data-stu-id="767a4-119">On hello **Add an endpoint tooa virtual machine** page, click hello right arrow.</span></span>
4. <span data-ttu-id="767a4-120">På hello **ange hello information om hello endpoint** sidan:</span><span class="sxs-lookup"><span data-stu-id="767a4-120">On hello **Specify hello details of hello endpoint** page:</span></span>

   * <span data-ttu-id="767a4-121">I **namn**, Skriv ett namn för hello slutpunkten eller välj hello namn i hello lista över fördefinierade slutpunkter för vanliga protokoll.</span><span class="sxs-lookup"><span data-stu-id="767a4-121">In **Name**, type a name for hello endpoint or select hello name from hello list of predefined endpoints for common protocols.</span></span>
   * <span data-ttu-id="767a4-122">I **protokollet**, Välj hello protokoll som krävs av hello typ av slutpunkt, TCP eller UDP, vid behov.</span><span class="sxs-lookup"><span data-stu-id="767a4-122">In **Protocol**, select hello protocol required by hello type of endpoint, either TCP or UDP, as needed.</span></span>
   * <span data-ttu-id="767a4-123">I **offentlig Port och privat Port**, Skriv hello-portnummer som du vill hello virtuella toouse efter behov.</span><span class="sxs-lookup"><span data-stu-id="767a4-123">In **Public Port and Private Port**, type hello port numbers that you want hello virtual machine toouse, as needed.</span></span> <span data-ttu-id="767a4-124">Du kan använda hello privata porten och brandväggsregler på hello virtuella tooredirect trafik på sätt som passar ditt program.</span><span class="sxs-lookup"><span data-stu-id="767a4-124">You can use hello private port and firewall rules on hello virtual machine tooredirect traffic in a way that is appropriate for your application.</span></span> <span data-ttu-id="767a4-125">hello privat port kan hello samma som hello offentlig port.</span><span class="sxs-lookup"><span data-stu-id="767a4-125">hello private port can be hello same as hello public port.</span></span> <span data-ttu-id="767a4-126">Till exempel för en slutpunkt för webbtrafik (HTTP) och tilldela port 80 tooboth hello offentliga och privata portar.</span><span class="sxs-lookup"><span data-stu-id="767a4-126">For example, for an endpoint for web (HTTP) traffic, you could assign port 80 tooboth hello public and private port.</span></span>

5. <span data-ttu-id="767a4-127">Välj **skapa en belastningsutjämnad uppsättning**, och klicka sedan på högerpilen för hello.</span><span class="sxs-lookup"><span data-stu-id="767a4-127">Select **Create a load-balanced set**, and then click hello right arrow.</span></span>
6. <span data-ttu-id="767a4-128">På hello **konfigurera hello belastningsutjämnad uppsättning** sidan, skriver du ett namn för hello belastningsutjämnade uppsättningen och tilldela hello värden för avsökningen funktionssätt hello Azure belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="767a4-128">On hello **Configure hello load-balanced set** page, type a name for hello load-balanced set, and then assign hello values for probe behavior of hello Azure Load Balancer.</span></span> <span data-ttu-id="767a4-129">hello belastningsutjämnare använder avsökningar toodetermine om hello virtuella datorer i hello belastningsutjämnad uppsättning tillgängliga tooreceive inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="767a4-129">hello Load Balancer uses probes toodetermine if hello virtual machines in hello load-balanced set are available tooreceive incoming traffic.</span></span>
7. <span data-ttu-id="767a4-130">Klicka på hello markerat toocreate hello belastningsutjämnade slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="767a4-130">Click hello check mark toocreate hello load-balanced endpoint.</span></span> <span data-ttu-id="767a4-131">Du ser **Ja** i hello **belastningsutjämnade namnet** kolumn i hello **slutpunkter** för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="767a4-131">You will see **Yes** in hello **Load-balanced set name** column of hello **Endpoints** page for hello virtual machine.</span></span>
8. <span data-ttu-id="767a4-132">I hello-portalen klickar du på **virtuella datorer**, klicka på hello namnet på en ytterligare virtuell dator i hello belastningsutjämnade uppsättningen, **slutpunkter**, och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="767a4-132">In hello portal, click **Virtual Machines**, click hello name of an additional virtual machine in hello load-balanced set, click **Endpoints**, and then click **Add**.</span></span>
9. <span data-ttu-id="767a4-133">På hello **lägga till en virtuell dator i slutpunkt tooa** klickar du på **lägga till slutpunkten tooan befintlig belastningsutjämnad uppsättning**Välj hello namnet på hello belastningsutjämnad uppsättning och klicka sedan på högerpilen för hello.</span><span class="sxs-lookup"><span data-stu-id="767a4-133">On hello **Add an endpoint tooa virtual machine** page, click **Add endpoint tooan existing load-balanced set**, select hello name of hello load-balanced set, and then click hello right arrow.</span></span>
10. <span data-ttu-id="767a4-134">På hello **ange hello information om hello endpoint** sidan anger du ett namn för hello slutpunkten, och klicka sedan på hello är markerat.</span><span class="sxs-lookup"><span data-stu-id="767a4-134">On hello **Specify hello details of hello endpoint** page, type a name for hello endpoint, and then click hello check mark.</span></span>

<span data-ttu-id="767a4-135">Upprepa steg 8-10 för hello ytterligare virtuella datorer i hello belastningsutjämnade uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="767a4-135">For hello additional virtual machines in hello load-balanced set, repeat steps 8-10.</span></span>

## <a name="next-steps"></a><span data-ttu-id="767a4-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="767a4-136">Next steps</span></span>

[<span data-ttu-id="767a4-137">Komma igång med att konfigurera en intern belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="767a4-137">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="767a4-138">Konfigurera ett distributionsläge för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="767a4-138">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="767a4-139">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="767a4-139">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
