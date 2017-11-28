---
title: "Skapa en intern belastningsutjämnare – Azure-portalen | Microsoft Docs"
description: "Lär dig hur du skapar en intern belastningsutjämnare i Resource Manager med hjälp av Azure Portal"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1ac14fb9-8d14-4892-bfe6-8bc74c48ae2c
ms.service: load-balancer
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 8fbe9d5d04d745de51e0e41516d6c12683c98637
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-internal-load-balancer-in-the-azure-portal"></a><span data-ttu-id="31018-103">Skapa en intern belastningsutjämnare i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="31018-103">Create an Internal load balancer in the Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="31018-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="31018-104">Azure Portal</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [<span data-ttu-id="31018-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="31018-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [<span data-ttu-id="31018-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="31018-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [<span data-ttu-id="31018-107">Mall</span><span class="sxs-lookup"><span data-stu-id="31018-107">Template</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="31018-108">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="31018-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="31018-109">Den här artikeln beskriver Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för [den klassiska distributionsmodellen](load-balancer-get-started-ilb-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="31018-109">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the [classic deployment model](load-balancer-get-started-ilb-classic-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="get-started-creating-an-internal-load-balancer-using-azure-portal"></a><span data-ttu-id="31018-110">Kom igång med att skapa en intern belastningsutjämnare med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="31018-110">Get started creating an Internal load balancer using Azure portal</span></span>

<span data-ttu-id="31018-111">Använd följande steg för att skapa en intern belastningsutjämnare från Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="31018-111">Use the following steps to create an internal load balancer from the Azure Portal.</span></span>

1. <span data-ttu-id="31018-112">Öppna en webbläsare, navigera till [Azure Portal](http://portal.azure.com) och logga in med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="31018-112">Open a browser, navigate to the [Azure portal](http://portal.azure.com), and sign in with your Azure account.</span></span>
2. <span data-ttu-id="31018-113">Längst upp till vänster på skärmen klickar du på **Nytt** > **Nätverk** > **Belastningsutjämnare**.</span><span class="sxs-lookup"><span data-stu-id="31018-113">In the upper left hand side of the screen, click **New** > **Networking** > **Load balancer**.</span></span>
3. <span data-ttu-id="31018-114">På bladet **Skapa belastningsutjämnare** skriver du ett **namn** för belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="31018-114">In the **Create load balancer** blade, enter a **Name** for your load balancer.</span></span>
4. <span data-ttu-id="31018-115">Under **Schema** klickar du på **Intern**.</span><span class="sxs-lookup"><span data-stu-id="31018-115">Under **Scheme**, click **Internal**.</span></span>
5. <span data-ttu-id="31018-116">Klicka på **Virtuellt nätverk** och välj sedan det virtuella nätverket där du vill skapa belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="31018-116">Click **Virtual network**, and then select the virtual network where you want to create the load balancer.</span></span>

   > [!NOTE]
   > <span data-ttu-id="31018-117">Om du inte hittar det virtuella nätverk som du vill använda, kontrollerar du vilken **plats** du använder för belastningsutjämnaren och gör ändringar därefter.</span><span class="sxs-lookup"><span data-stu-id="31018-117">If you do not see the virtual network you want to use, check the **Location** you are using for the load balancer, and change it accordingly.</span></span>

6. <span data-ttu-id="31018-118">Klicka på **Undernät** och välj sedan undernätet där du vill skapa belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="31018-118">Click **Subnet**, and then select the subnet where you want to create the load balancer.</span></span>
7. <span data-ttu-id="31018-119">Under **IP-adresstilldelning** klickar du antingen på **Dynamisk** eller **Statisk**, beroende på om du vill att IP-adressen för belastningsutjämnaren ska vara fast (statisk) eller inte.</span><span class="sxs-lookup"><span data-stu-id="31018-119">Under **IP address assignment**, click either **Dynamic** or **Static**, depending on whether you want the IP address for the load balancer to be fixed (static) or not.</span></span>

   > [!NOTE]
   > <span data-ttu-id="31018-120">Om du väljer att använda en statisk IP-adress måste du ange en adress för belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="31018-120">If you select to use a static IP address, you will have to provide an address for the load balancer.</span></span>

8. <span data-ttu-id="31018-121">Under **Resursgrupp** anger du antingen namnet på en ny resursgrupp för belastningsutjämnaren eller så klickar du på **Välj befintlig** och väljer en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="31018-121">Under **Resource group** either specify the name of a new resource group for the load balancer, or click **select existing** and select an existing resource group.</span></span>
9. <span data-ttu-id="31018-122">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="31018-122">Click **Create**.</span></span>

## <a name="configure-load-balancing-rules"></a><span data-ttu-id="31018-123">Konfigurera belastningsutjämningsregler</span><span class="sxs-lookup"><span data-stu-id="31018-123">Configure load balancing rules</span></span>

<span data-ttu-id="31018-124">När du har skapat en belastningsutjämnare navigerar du till belastningsutjämningsresursen för att konfigurera den.</span><span class="sxs-lookup"><span data-stu-id="31018-124">After the load balancer creation, navigate to the load balancer resource to configure it.</span></span>
<span data-ttu-id="31018-125">Du behöver konfigurera en serverdelsadresspool och en avsökning innan du kan konfigurera en belastningsutjämningsregel.</span><span class="sxs-lookup"><span data-stu-id="31018-125">You need to configure first a back-end address pool and a probe before configuring a load balancing rule.</span></span>

### <a name="step-1-configure-a-back-end-pool"></a><span data-ttu-id="31018-126">Steg 1: Konfigurera en serverdelspool</span><span class="sxs-lookup"><span data-stu-id="31018-126">Step 1: Configure a back-end pool</span></span>

1. <span data-ttu-id="31018-127">Klicka på **Bläddra** > **Belastningsutjämnare** i Azure Portal och klicka sedan på belastningsutjämnaren som du skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="31018-127">In the Azure portal, click **Browse** > **Load balancers**, and then click the load balancer you created above.</span></span>
2. <span data-ttu-id="31018-128">På bladet **Inställningar** klickar du på **Serverdelspooler**.</span><span class="sxs-lookup"><span data-stu-id="31018-128">In the **Settings** blade, click **Backend pools**.</span></span>
3. <span data-ttu-id="31018-129">På bladet **Serverdelspooler** klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="31018-129">In the **Backend address pools** blade, click **Add**.</span></span>
4. <span data-ttu-id="31018-130">På bladet **Lägg till serverdelspool** anger du ett **namn** för serverdelspoolen och sedan klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="31018-130">In the **Add backend pool** blade, enter a **Name** for the backend pool, and then click **OK**.</span></span>

### <a name="step-2-configure-a-probe"></a><span data-ttu-id="31018-131">Steg 2: Konfigurera en avsökning</span><span class="sxs-lookup"><span data-stu-id="31018-131">Step 2: Configure a probe</span></span>

1. <span data-ttu-id="31018-132">Klicka på **Bläddra** > **Belastningsutjämnare** i Azure Portal och klicka sedan på belastningsutjämnaren som du skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="31018-132">In the Azure portal, click **Browse** > **Load balancers**, and then click the load balancer you created above.</span></span>
2. <span data-ttu-id="31018-133">På bladet **Inställningar** klickar du på **Avsökningar**.</span><span class="sxs-lookup"><span data-stu-id="31018-133">In the **Settings** blade, click **Probes**.</span></span>
3. <span data-ttu-id="31018-134">På bladet **Avsökningar** klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="31018-134">In the **Probes**  blade, click **Add**.</span></span>
4. <span data-ttu-id="31018-135">På bladet **Lägg till avsökning** anger du ett **namn** för avsökningen.</span><span class="sxs-lookup"><span data-stu-id="31018-135">In the **Add probe** blade, enter a **Name** for the probe.</span></span>
5. <span data-ttu-id="31018-136">Under **Protokoll** väljer du **HTTP** (för webbplatser) eller **TCP** (för andra TCP-baserade program).</span><span class="sxs-lookup"><span data-stu-id="31018-136">Under **Protocol**, select **HTTP** (for web sites) or **TCP** (for other TCP based applications).</span></span>
6. <span data-ttu-id="31018-137">Under **Port** anger du porten som ska användas vid åtkomst till avsökningen.</span><span class="sxs-lookup"><span data-stu-id="31018-137">Under **Port**, specify the port to use when accessing the probe.</span></span>
7. <span data-ttu-id="31018-138">Under **Sökväg** (endast för HTTP-avsökningar) anger du sökvägen som används som avsökning.</span><span class="sxs-lookup"><span data-stu-id="31018-138">Under **Path** (for HTTP probes only), specify the path to use as a probe.</span></span>
8. <span data-ttu-id="31018-139">Under **Intervall** anger du hur ofta avsökning av programmet ska ske.</span><span class="sxs-lookup"><span data-stu-id="31018-139">Under **Interval** specify how frequently to probe the application.</span></span>
9. <span data-ttu-id="31018-140">Under **Unhealthy threshold** (Tröskelvärde för felstatus) anger du hur många försök som får misslyckas innan den virtuella datorn på serversidan markeras som felaktig.</span><span class="sxs-lookup"><span data-stu-id="31018-140">Under **Unhealthy threshold**, specify how many attempts should fail before the backend virtual machine is marked as unhealthy.</span></span>
10. <span data-ttu-id="31018-141">Klicka på **OK** för att skapa avsökningen.</span><span class="sxs-lookup"><span data-stu-id="31018-141">Click **OK** to create probe.</span></span>

### <a name="step-3-configure-load-balancing-rules"></a><span data-ttu-id="31018-142">Steg 3: Konfigurera belastningsutjämningsregler</span><span class="sxs-lookup"><span data-stu-id="31018-142">Step 3: Configure load balancing rules</span></span>

1. <span data-ttu-id="31018-143">Klicka på **Bläddra** > **Belastningsutjämnare** i Azure Portal och klicka sedan på belastningsutjämnaren som du skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="31018-143">In the Azure portal, click **Browse** > **Load balancers**, and then click the load balancer you created above.</span></span>
2. <span data-ttu-id="31018-144">På bladet **Inställningar** klickar du på **Belastningsutjämningsregler**.</span><span class="sxs-lookup"><span data-stu-id="31018-144">In the **Settings** blade, click **Load balancing rules**.</span></span>
3. <span data-ttu-id="31018-145">På bladet **Belastningsutjämningsregler** klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="31018-145">In the **Load balancing rules** blade, click **Add**.</span></span>
4. <span data-ttu-id="31018-146">På bladet **Add load balancing rule** (Lägg till belastningsutjämningsregel) anger du ett **namn** för regeln.</span><span class="sxs-lookup"><span data-stu-id="31018-146">In the **Add load balancing rule** blade, enter a **Name** for the rule.</span></span>
5. <span data-ttu-id="31018-147">Under **Protokoll** väljer du **HTTP** (för webbplatser) eller **TCP** (för andra TCP-baserade program).</span><span class="sxs-lookup"><span data-stu-id="31018-147">Under **Protocol**, select **HTTP** (for web sites) or **TCP** (for other TCP based applications).</span></span>
6. <span data-ttu-id="31018-148">Under **Port** anger du den port som klienter ansluter till i belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="31018-148">Under **Port**, specify the port clients connect to in the load balancer.</span></span>
7. <span data-ttu-id="31018-149">Under **Serverdelsport** anger du porten som ska användas i serverdelspoolen (vanligtvis är belastningsutjämnarporten och serverdelsporten densamma).</span><span class="sxs-lookup"><span data-stu-id="31018-149">Under **Backend port**, specify the port to be used in the backend pool (usually, the load balancer port and the backend port are the same).</span></span>
8. <span data-ttu-id="31018-150">Under **Serverdelspool** väljer du den serverdelspool du skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="31018-150">Under **Backend pool**, select the backend pool you created above.</span></span>
9. <span data-ttu-id="31018-151">Under **Sessionspermanens** väljer du hur du vill att sessioner ska sparas.</span><span class="sxs-lookup"><span data-stu-id="31018-151">Under **Session persistence**, select how you want sessions to persist.</span></span>
10. <span data-ttu-id="31018-152">Under **Timeout för inaktivitet (minuter)** anger du tidsgränsen för inaktivitet.</span><span class="sxs-lookup"><span data-stu-id="31018-152">Under **Idle timeout (minutes)**, specify the idle timeout.</span></span>
11. <span data-ttu-id="31018-153">Under **Flytande IP (direkt serverreturnering)** klickar du på **Inaktiverad** eller **Aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="31018-153">Under **Floating IP (direct server return)**, click **Disabled** or **Enabled**.</span></span>
12. <span data-ttu-id="31018-154">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="31018-154">Click **OK**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="31018-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="31018-155">Next steps</span></span>

[<span data-ttu-id="31018-156">Konfigurera ett distributionsläge för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="31018-156">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="31018-157">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="31018-157">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

