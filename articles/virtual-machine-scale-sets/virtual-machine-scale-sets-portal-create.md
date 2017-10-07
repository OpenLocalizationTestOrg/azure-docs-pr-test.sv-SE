---
title: "aaaCreate en Skaluppsättning för virtuell dator med hjälp av hello Azure-portalen | Microsoft Docs"
description: "Distribuera skaluppsättningar med hjälp av Azure portal."
keywords: "Skaluppsättningar för den virtuella datorn"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: negat
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 23c88f4b1ba99994a38f8886f60735da74e5c17e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-virtual-machine-scale-set-with-hello-azure-portal"></a><span data-ttu-id="9d627-104">Hur toocreate en Skaluppsättning för virtuell dator med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9d627-104">How toocreate a Virtual Machine Scale Set with hello Azure portal</span></span>
<span data-ttu-id="9d627-105">Den här kursen visar hur lätt det är toocreate Skalningsuppsättning en virtuell dator på bara några minuter med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9d627-105">This tutorial shows you how easy it is toocreate a Virtual Machine Scale Set in just a few minutes, by using hello Azure portal.</span></span> <span data-ttu-id="9d627-106">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="9d627-106">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/) before you begin.</span></span>

## <a name="choose-hello-vm-image-from-hello-marketplace"></a><span data-ttu-id="9d627-107">Välj hello VM-avbildning från marketplace hello</span><span class="sxs-lookup"><span data-stu-id="9d627-107">Choose hello VM image from hello marketplace</span></span>
<span data-ttu-id="9d627-108">Du kan enkelt distribuera en skala som anges med CentOS, virtuell CoreOS, Debian, öppna Suse, Red Hat Enterprise Linux, SUSE Linux Enterprise Server, Ubuntu Server eller Windows Server-avbildningar från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="9d627-108">From hello portal, you can easily deploy a scale set with CentOS, CoreOS, Debian, Open Suse, Red Hat Enterprise Linux, SUSE Linux Enterprise Server, Ubuntu Server, or Windows Server images.</span></span>

<span data-ttu-id="9d627-109">Först gå toohello [Azure-portalen](https://portal.azure.com) i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="9d627-109">First, navigate toohello [Azure portal](https://portal.azure.com) in a web browser.</span></span> <span data-ttu-id="9d627-110">Klicka på `New`, söka efter `scale set`, och välj sedan hello `Virtual machine scale set` post:</span><span class="sxs-lookup"><span data-stu-id="9d627-110">Click `New`, search for `scale set`, and then select hello `Virtual machine scale set` entry:</span></span>

![ScaleSetPortalOverview](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalOverview.PNG)

## <a name="create-hello-scale-set"></a><span data-ttu-id="9d627-112">Skapa hello skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="9d627-112">Create hello scale set</span></span>
<span data-ttu-id="9d627-113">Nu kan du använda hello standardinställningarna och snabbt skapa hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="9d627-113">Now you can use hello default settings and quickly create hello scale set.</span></span>

* <span data-ttu-id="9d627-114">På hello `Basics` bladet, ange ett namn för hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="9d627-114">On hello `Basics` blade, enter a name for hello scale set.</span></span> <span data-ttu-id="9d627-115">Det här namnet blir grundläggande hello av hello FQDN för hello belastningsutjämnaren framför hello skaluppsättning, så kontrollera hello namn är unikt över alla Azure.</span><span class="sxs-lookup"><span data-stu-id="9d627-115">This name becomes hello base of hello FQDN of hello load balancer in front of hello scale set, so make sure hello name is unique across all Azure.</span></span>
* <span data-ttu-id="9d627-116">Välj din önskade OS skriver, ange ditt önskade användarnamn och välj vilken autentiseringsmetod skriver du föredrar.</span><span class="sxs-lookup"><span data-stu-id="9d627-116">Select your desired OS type, enter your desired username, and select which authentication type you prefer.</span></span> <span data-ttu-id="9d627-117">Om du väljer ett lösenord, det måste vara minst 12 tecken långa och uppfylla tre utanför hello fyra följande krav på komplexitet: en gemen, en versal, en siffra och ett specialtecken.</span><span class="sxs-lookup"><span data-stu-id="9d627-117">If you choose a password, it must be at least 12 characters long and meet three out of hello four following complexity requirements: one lower case character, one upper case character, one number, and one special character.</span></span> <span data-ttu-id="9d627-118">Läs mer om [krav för användarnamn och lösenord](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm).</span><span class="sxs-lookup"><span data-stu-id="9d627-118">See more about [username and password requirements](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm).</span></span> <span data-ttu-id="9d627-119">Om du väljer `SSH public key`, vara säker på att tooonly klistra in i den offentliga nyckeln inte din privata nyckel:</span><span class="sxs-lookup"><span data-stu-id="9d627-119">If you choose `SSH public key`, be sure tooonly paste in your public key, NOT your private key:</span></span>

![ScaleSetPortalBasics](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalBasics.PNG)

* <span data-ttu-id="9d627-121">Välj om du skulle som toolimit hello skaluppsättning tooa enda placering grupp eller om den ska omfatta flera grupper för placering.</span><span class="sxs-lookup"><span data-stu-id="9d627-121">Choose whether you would like toolimit hello scale set tooa single placement group or whether it should span multiple placement groups.</span></span> <span data-ttu-id="9d627-122">Att tillåta hello skaluppsättning toospan placering grupper ger för skala anger över 100 virtuella datorer med vissa begränsningar i kapaciteten (upp too1 000).</span><span class="sxs-lookup"><span data-stu-id="9d627-122">Allowing hello scale set toospan placement groups allows for scale sets over 100 VMs in capacity (up too1,000) with certain limitations.</span></span> <span data-ttu-id="9d627-123">Mer information finns i [denna dokumentation](./virtual-machine-scale-sets-placement-groups.md).</span><span class="sxs-lookup"><span data-stu-id="9d627-123">For more information, see [this documentation](./virtual-machine-scale-sets-placement-groups.md).</span></span>
* <span data-ttu-id="9d627-124">Ange önskade resursgruppens namn och plats och klicka sedan på `OK`.</span><span class="sxs-lookup"><span data-stu-id="9d627-124">Enter your desired resource group name and location, and then click `OK`.</span></span>
* <span data-ttu-id="9d627-125">På hello `Virtual machine scale set service settings` bladet: Ange den önskade domännamnet (hello FQDN för hello belastningsutjämnaren framför hello skaluppsättning hello base).</span><span class="sxs-lookup"><span data-stu-id="9d627-125">On hello `Virtual machine scale set service settings` blade: enter your desired domain name label (hello base of hello FQDN for hello load balancer in front of hello scale set).</span></span> <span data-ttu-id="9d627-126">Den här etiketten måste vara unikt inom alla Azure.</span><span class="sxs-lookup"><span data-stu-id="9d627-126">This label must be unique across all Azure.</span></span>
* <span data-ttu-id="9d627-127">Välj önskat operativsystem diskavbildning, instansantal och storleken på datorn.</span><span class="sxs-lookup"><span data-stu-id="9d627-127">Choose your desired operating system disk image, instance count, and machine size.</span></span>
* <span data-ttu-id="9d627-128">Välj önskad disk-typ: hanterad eller ohanterad.</span><span class="sxs-lookup"><span data-stu-id="9d627-128">Choose your desired disk type: managed or unmanaged.</span></span> <span data-ttu-id="9d627-129">Mer information finns i [denna dokumentation](./virtual-machine-scale-sets-managed-disks.md).</span><span class="sxs-lookup"><span data-stu-id="9d627-129">For more information, see [this documentation](./virtual-machine-scale-sets-managed-disks.md).</span></span> <span data-ttu-id="9d627-130">Om du har valt toohave hello skaluppsättning sträcker sig över flera placering grupper, det här alternativet är inte tillgänglig eftersom hanterade diskar krävs för att skala anger toospan placering grupper.</span><span class="sxs-lookup"><span data-stu-id="9d627-130">If you chose toohave hello scale set span multiple placement groups, this option will not be available because managed disk is required for scale sets toospan placement groups.</span></span>
* <span data-ttu-id="9d627-131">Aktivera eller inaktivera Autoskala och konfigurera om aktiverat:</span><span class="sxs-lookup"><span data-stu-id="9d627-131">Enable or disable autoscale and configure if enabled:</span></span>

![ScaleSetPortalService](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalService.PNG)

* <span data-ttu-id="9d627-133">På hello `Summary` bladet när verifieringen är klar klickar du på `OK` toostart hello skaluppsättning distribution.</span><span class="sxs-lookup"><span data-stu-id="9d627-133">On hello `Summary` blade, when validation is done, click `OK` toostart hello scale set deployment.</span></span>


## <a name="connect-tooa-vm-in-hello-scale-set"></a><span data-ttu-id="9d627-134">Ansluta tooa VM i hello skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="9d627-134">Connect tooa VM in hello scale set</span></span>
<span data-ttu-id="9d627-135">Om du har valt toolimit nivå inställt tooa enda placering grupp och hello skaluppsättning distribueras med NAT regler har konfigurerat toolet du ansluta toohello skaluppsättningen enkelt (om inte, tooconnect toohello virtuella datorer i hello skala har angetts måste du förmodligen toocreate en jumpbox i hello samma virtuella nätverk som hello skaluppsättning).</span><span class="sxs-lookup"><span data-stu-id="9d627-135">If you chose toolimit your scale set tooa single placement group, then hello scale set is deployed with NAT rules configured toolet you connect toohello scale set easily (if not, tooconnect toohello virtual machines in hello scale set, you likely need toocreate a jumpbox in hello same virtual network as hello scale set).</span></span> <span data-ttu-id="9d627-136">toosee dem, navigera toohello `Inbound NAT Rules` fliken av hello belastningsutjämnare för hello skaluppsättning för:</span><span class="sxs-lookup"><span data-stu-id="9d627-136">toosee them, navigate toohello `Inbound NAT Rules` tab of hello load balancer for hello scale set:</span></span>

![ScaleSetPortalNatRules](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalNatRules.PNG)

<span data-ttu-id="9d627-138">Du kan ansluta tooeach VM i hello skala anges med dessa NAT-regler.</span><span class="sxs-lookup"><span data-stu-id="9d627-138">You can connect tooeach VM in hello scale set using these NAT rules.</span></span> <span data-ttu-id="9d627-139">Till exempel för en skaluppsättning för Windows om det finns en NAT-regel för inkommande port 50000, du kan ansluta toothat datorn via RDP på `<load-balancer-ip-address>:50000`.</span><span class="sxs-lookup"><span data-stu-id="9d627-139">For instance, for a Windows scale set, if there is a NAT rule on incoming port 50000, you could connect toothat machine via RDP on `<load-balancer-ip-address>:50000`.</span></span> <span data-ttu-id="9d627-140">För en Linux-skaluppsättning kan du ansluta hello kommandot `ssh -p 50000 <username>@<load-balancer-ip-address>`.</span><span class="sxs-lookup"><span data-stu-id="9d627-140">For a Linux scale set, you would connect using hello command `ssh -p 50000 <username>@<load-balancer-ip-address>`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9d627-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9d627-141">Next steps</span></span>
<span data-ttu-id="9d627-142">Dokumentation om hur toodeploy skalningsuppsättningarna från hello CLI finns [denna dokumentation](virtual-machine-scale-sets-cli-quick-create.md).</span><span class="sxs-lookup"><span data-stu-id="9d627-142">For documentation on how toodeploy scale sets from hello CLI, see [this documentation](virtual-machine-scale-sets-cli-quick-create.md).</span></span>

<span data-ttu-id="9d627-143">Dokumentation om hur toodeploy skalningsuppsättningarna från PowerShell finns [denna dokumentation](virtual-machine-scale-sets-windows-create.md).</span><span class="sxs-lookup"><span data-stu-id="9d627-143">For documentation on how toodeploy scale sets from PowerShell, see [this documentation](virtual-machine-scale-sets-windows-create.md).</span></span>

<span data-ttu-id="9d627-144">Dokumentation om hur toodeploy skalningsuppsättningarna från Visual Studio finns [denna dokumentation](virtual-machine-scale-sets-vs-create.md).</span><span class="sxs-lookup"><span data-stu-id="9d627-144">For documentation on how toodeploy scale sets from Visual Studio, see [this documentation](virtual-machine-scale-sets-vs-create.md).</span></span>

<span data-ttu-id="9d627-145">För allmän dokumentation kolla hello [dokumentationen översiktssidan för skalningsuppsättningar](virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9d627-145">For general documentation, check out hello [documentation overview page for scale sets](virtual-machine-scale-sets-overview.md).</span></span>

<span data-ttu-id="9d627-146">Allmän information kolla hello [huvudsakliga Landningssida för skalningsuppsättningar](https://azure.microsoft.com/services/virtual-machine-scale-sets/).</span><span class="sxs-lookup"><span data-stu-id="9d627-146">For general information, check out hello [main landing page for scale sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/).</span></span>

