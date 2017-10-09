---
title: "aaaCreate en intern belastningsutjämnare - Azure-portalen | Microsoft Docs"
description: "Lär dig hur toocreate en intern belastningsutjämnare i Resource Manager med hello Azure-portalen"
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
ms.openlocfilehash: 80124217a84857b542eb41cb814ec97234176dd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internal-load-balancer-in-hello-azure-portal"></a><span data-ttu-id="f00a6-103">Skapa en intern belastningsutjämnare i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f00a6-103">Create an Internal load balancer in hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f00a6-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f00a6-104">Azure Portal</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [<span data-ttu-id="f00a6-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f00a6-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [<span data-ttu-id="f00a6-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f00a6-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [<span data-ttu-id="f00a6-107">Mall</span><span class="sxs-lookup"><span data-stu-id="f00a6-107">Template</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="f00a6-108">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="f00a6-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="f00a6-109">Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello [klassiska distributionsmodellen](load-balancer-get-started-ilb-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="f00a6-109">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](load-balancer-get-started-ilb-classic-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="get-started-creating-an-internal-load-balancer-using-azure-portal"></a><span data-ttu-id="f00a6-110">Kom igång med att skapa en intern belastningsutjämnare med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f00a6-110">Get started creating an Internal load balancer using Azure portal</span></span>

<span data-ttu-id="f00a6-111">Använd hello följande steg toocreate en intern belastningsutjämnare från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f00a6-111">Use hello following steps toocreate an internal load balancer from hello Azure Portal.</span></span>

1. <span data-ttu-id="f00a6-112">Öppna en webbläsare, navigera toohello [Azure-portalen](http://portal.azure.com), och logga in med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="f00a6-112">Open a browser, navigate toohello [Azure portal](http://portal.azure.com), and sign in with your Azure account.</span></span>
2. <span data-ttu-id="f00a6-113">Hello övre vänstra sida av hello-skärmen klickar du på **ny** > **nätverk** > **belastningsutjämnaren**.</span><span class="sxs-lookup"><span data-stu-id="f00a6-113">In hello upper left hand side of hello screen, click **New** > **Networking** > **Load balancer**.</span></span>
3. <span data-ttu-id="f00a6-114">I hello **skapa belastningsutjämnaren** bladet anger du en **namn** för din belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="f00a6-114">In hello **Create load balancer** blade, enter a **Name** for your load balancer.</span></span>
4. <span data-ttu-id="f00a6-115">Under **Schema** klickar du på **Intern**.</span><span class="sxs-lookup"><span data-stu-id="f00a6-115">Under **Scheme**, click **Internal**.</span></span>
5. <span data-ttu-id="f00a6-116">Klicka på **för virtuella nätverk**, och sedan väljer hello virtuellt nätverk där du vill att toocreate hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="f00a6-116">Click **Virtual network**, and then select hello virtual network where you want toocreate hello load balancer.</span></span>

   > [!NOTE]
   > <span data-ttu-id="f00a6-117">Om du inte ser hello virtuella nätverk som du vill toouse Kontrollera hello **plats** du använder för hello belastningsutjämnaren och ändras den.</span><span class="sxs-lookup"><span data-stu-id="f00a6-117">If you do not see hello virtual network you want toouse, check hello **Location** you are using for hello load balancer, and change it accordingly.</span></span>

6. <span data-ttu-id="f00a6-118">Klicka på **undernät**, och välj sedan hello undernät där du vill att toocreate hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="f00a6-118">Click **Subnet**, and then select hello subnet where you want toocreate hello load balancer.</span></span>
7. <span data-ttu-id="f00a6-119">Under **IP-adresstilldelning**, klicka på antingen **dynamiska** eller **statiska**, beroende på om du vill hello IP-adress för hello load balancer toobe fast (statisk) eller inte.</span><span class="sxs-lookup"><span data-stu-id="f00a6-119">Under **IP address assignment**, click either **Dynamic** or **Static**, depending on whether you want hello IP address for hello load balancer toobe fixed (static) or not.</span></span>

   > [!NOTE]
   > <span data-ttu-id="f00a6-120">Om du väljer toouse statisk IP-adress har du tooprovide en adress för hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="f00a6-120">If you select toouse a static IP address, you will have tooprovide an address for hello load balancer.</span></span>

8. <span data-ttu-id="f00a6-121">Under **resursgruppen** ange hello namnet på en ny resursgrupp för hello belastningsutjämnare, eller klicka på **Välj befintlig** och välj en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="f00a6-121">Under **Resource group** either specify hello name of a new resource group for hello load balancer, or click **select existing** and select an existing resource group.</span></span>
9. <span data-ttu-id="f00a6-122">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f00a6-122">Click **Create**.</span></span>

## <a name="configure-load-balancing-rules"></a><span data-ttu-id="f00a6-123">Konfigurera belastningsutjämningsregler</span><span class="sxs-lookup"><span data-stu-id="f00a6-123">Configure load balancing rules</span></span>

<span data-ttu-id="f00a6-124">När hello ladda belastningsutjämnaren skapande, navigera toohello load balancer resurs tooconfigure den.</span><span class="sxs-lookup"><span data-stu-id="f00a6-124">After hello load balancer creation, navigate toohello load balancer resource tooconfigure it.</span></span>
<span data-ttu-id="f00a6-125">Du behöver tooconfigure först en backend-adresspool och en avsökning innan du konfigurerar en belastningsutjämningsregel.</span><span class="sxs-lookup"><span data-stu-id="f00a6-125">You need tooconfigure first a back-end address pool and a probe before configuring a load balancing rule.</span></span>

### <a name="step-1-configure-a-back-end-pool"></a><span data-ttu-id="f00a6-126">Steg 1: Konfigurera en serverdelspool</span><span class="sxs-lookup"><span data-stu-id="f00a6-126">Step 1: Configure a back-end pool</span></span>

1. <span data-ttu-id="f00a6-127">I hello Azure-portalen klickar du på **Bläddra** > **belastningsutjämnare**, och klicka sedan på hello belastningsutjämnare som du skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="f00a6-127">In hello Azure portal, click **Browse** > **Load balancers**, and then click hello load balancer you created above.</span></span>
2. <span data-ttu-id="f00a6-128">I hello **inställningar** bladet, klickar du på **serverdelspooler**.</span><span class="sxs-lookup"><span data-stu-id="f00a6-128">In hello **Settings** blade, click **Backend pools**.</span></span>
3. <span data-ttu-id="f00a6-129">I hello **serverdelsadresspooler** bladet, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="f00a6-129">In hello **Backend address pools** blade, click **Add**.</span></span>
4. <span data-ttu-id="f00a6-130">I hello **lägga till serverdelspoolen** bladet anger du en **namn** hello serverdelspool och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f00a6-130">In hello **Add backend pool** blade, enter a **Name** for hello backend pool, and then click **OK**.</span></span>

### <a name="step-2-configure-a-probe"></a><span data-ttu-id="f00a6-131">Steg 2: Konfigurera en avsökning</span><span class="sxs-lookup"><span data-stu-id="f00a6-131">Step 2: Configure a probe</span></span>

1. <span data-ttu-id="f00a6-132">I hello Azure-portalen klickar du på **Bläddra** > **belastningsutjämnare**, och klicka sedan på hello belastningsutjämnare som du skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="f00a6-132">In hello Azure portal, click **Browse** > **Load balancers**, and then click hello load balancer you created above.</span></span>
2. <span data-ttu-id="f00a6-133">I hello **inställningar** bladet, klickar du på **avsökningar**.</span><span class="sxs-lookup"><span data-stu-id="f00a6-133">In hello **Settings** blade, click **Probes**.</span></span>
3. <span data-ttu-id="f00a6-134">I hello **avsökningar** bladet, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="f00a6-134">In hello **Probes**  blade, click **Add**.</span></span>
4. <span data-ttu-id="f00a6-135">I hello **Lägg till avsökning** bladet anger du en **namn** för hello avsökningen.</span><span class="sxs-lookup"><span data-stu-id="f00a6-135">In hello **Add probe** blade, enter a **Name** for hello probe.</span></span>
5. <span data-ttu-id="f00a6-136">Under **Protokoll** väljer du **HTTP** (för webbplatser) eller **TCP** (för andra TCP-baserade program).</span><span class="sxs-lookup"><span data-stu-id="f00a6-136">Under **Protocol**, select **HTTP** (for web sites) or **TCP** (for other TCP based applications).</span></span>
6. <span data-ttu-id="f00a6-137">Under **Port**, ange hello port toouse vid åtkomst till hello avsökning.</span><span class="sxs-lookup"><span data-stu-id="f00a6-137">Under **Port**, specify hello port toouse when accessing hello probe.</span></span>
7. <span data-ttu-id="f00a6-138">Under **sökväg** (för HTTP-avsökningar endast), ange hello sökvägen toouse som en avsökning.</span><span class="sxs-lookup"><span data-stu-id="f00a6-138">Under **Path** (for HTTP probes only), specify hello path toouse as a probe.</span></span>
8. <span data-ttu-id="f00a6-139">Under **intervall** ange hur ofta tooprobe hello program.</span><span class="sxs-lookup"><span data-stu-id="f00a6-139">Under **Interval** specify how frequently tooprobe hello application.</span></span>
9. <span data-ttu-id="f00a6-140">Under **tröskelvärde för ohälsosamt värde**, ange hur många försök får misslyckas innan hello backend virtuella datorn markeras som ohälsosam.</span><span class="sxs-lookup"><span data-stu-id="f00a6-140">Under **Unhealthy threshold**, specify how many attempts should fail before hello backend virtual machine is marked as unhealthy.</span></span>
10. <span data-ttu-id="f00a6-141">Klicka på **OK** toocreate avsökning.</span><span class="sxs-lookup"><span data-stu-id="f00a6-141">Click **OK** toocreate probe.</span></span>

### <a name="step-3-configure-load-balancing-rules"></a><span data-ttu-id="f00a6-142">Steg 3: Konfigurera belastningsutjämningsregler</span><span class="sxs-lookup"><span data-stu-id="f00a6-142">Step 3: Configure load balancing rules</span></span>

1. <span data-ttu-id="f00a6-143">I hello Azure-portalen klickar du på **Bläddra** > **belastningsutjämnare**, och klicka sedan på hello belastningsutjämnare som du skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="f00a6-143">In hello Azure portal, click **Browse** > **Load balancers**, and then click hello load balancer you created above.</span></span>
2. <span data-ttu-id="f00a6-144">I hello **inställningar** bladet, klickar du på **belastningsutjämningsregler**.</span><span class="sxs-lookup"><span data-stu-id="f00a6-144">In hello **Settings** blade, click **Load balancing rules**.</span></span>
3. <span data-ttu-id="f00a6-145">I hello **belastningsutjämningsregler** bladet, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="f00a6-145">In hello **Load balancing rules** blade, click **Add**.</span></span>
4. <span data-ttu-id="f00a6-146">I hello **Lägg till regel för belastningsutjämning** bladet anger du en **namn** för hello regeln.</span><span class="sxs-lookup"><span data-stu-id="f00a6-146">In hello **Add load balancing rule** blade, enter a **Name** for hello rule.</span></span>
5. <span data-ttu-id="f00a6-147">Under **Protokoll** väljer du **HTTP** (för webbplatser) eller **TCP** (för andra TCP-baserade program).</span><span class="sxs-lookup"><span data-stu-id="f00a6-147">Under **Protocol**, select **HTTP** (for web sites) or **TCP** (for other TCP based applications).</span></span>
6. <span data-ttu-id="f00a6-148">Under **Port**, ange hello port klienterna ansluter tooin hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="f00a6-148">Under **Port**, specify hello port clients connect tooin hello load balancer.</span></span>
7. <span data-ttu-id="f00a6-149">Under **serverdelsport**, ange hello port toobe används i hello serverdelspool (vanligtvis hello load balancer porten och serverdelsporten för hello är hello samma).</span><span class="sxs-lookup"><span data-stu-id="f00a6-149">Under **Backend port**, specify hello port toobe used in hello backend pool (usually, hello load balancer port and hello backend port are hello same).</span></span>
8. <span data-ttu-id="f00a6-150">Under **serverdelspool**väljer hello backend programpool du skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="f00a6-150">Under **Backend pool**, select hello backend pool you created above.</span></span>
9. <span data-ttu-id="f00a6-151">Under **sessionspersistence**, Välj hur du vill sessioner toopersist.</span><span class="sxs-lookup"><span data-stu-id="f00a6-151">Under **Session persistence**, select how you want sessions toopersist.</span></span>
10. <span data-ttu-id="f00a6-152">Under **timeout för inaktivitet (minuter)**, ange hello timeout för inaktivitet.</span><span class="sxs-lookup"><span data-stu-id="f00a6-152">Under **Idle timeout (minutes)**, specify hello idle timeout.</span></span>
11. <span data-ttu-id="f00a6-153">Under **Flytande IP (direkt serverreturnering)** klickar du på **Inaktiverad** eller **Aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="f00a6-153">Under **Floating IP (direct server return)**, click **Disabled** or **Enabled**.</span></span>
12. <span data-ttu-id="f00a6-154">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f00a6-154">Click **OK**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f00a6-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f00a6-155">Next steps</span></span>

[<span data-ttu-id="f00a6-156">Konfigurera ett distributionsläge för belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="f00a6-156">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="f00a6-157">Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="f00a6-157">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

