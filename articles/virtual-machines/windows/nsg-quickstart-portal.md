---
title: "aaaOpen portar tooa VM med hjälp av hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur tooopen en port / skapa en slutpunkt tooyour Windows VM med hello resource manager-distributionsmodellen i hello Azure-portalen"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: f7cf0319-5ee7-435e-8f94-c484bf5ee6f1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: aba789c65254651899aa688f256fe616c3d0126d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooopen-ports-tooa-virtual-machine-with-hello-azure-portal"></a><span data-ttu-id="7be66-103">Hur tooopen portarna tooa virtuell dator med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7be66-103">How tooopen ports tooa virtual machine with hello Azure portal</span></span>
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a><span data-ttu-id="7be66-104">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="7be66-104">Quick commands</span></span>
<span data-ttu-id="7be66-105">Du kan också [utför dessa steg med hjälp av Azure PowerShell](nsg-quickstart-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="7be66-105">You can also [perform these steps using Azure PowerShell](nsg-quickstart-powershell.md).</span></span>

<span data-ttu-id="7be66-106">Först skapa en säkerhetsgrupp för ditt nätverk.</span><span class="sxs-lookup"><span data-stu-id="7be66-106">First, create your Network Security Group.</span></span> <span data-ttu-id="7be66-107">Välj en resursgrupp i hello portal, Välj **Lägg till**, och Sök efter och välj **nätverkssäkerhetsgruppen**:</span><span class="sxs-lookup"><span data-stu-id="7be66-107">Select a resource group in hello portal, choose **Add**, then search for and select **Network security group**:</span></span>

![Lägg till en Nätverkssäkerhetsgrupp](./media/nsg-quickstart-portal/add-nsg.png)

<span data-ttu-id="7be66-109">Ange ett namn för din Nätverkssäkerhetsgruppen, Välj eller skapa en resursgrupp och välja en plats.</span><span class="sxs-lookup"><span data-stu-id="7be66-109">Enter a name for your Network Security Group, select or create a resource group, and select a location.</span></span> <span data-ttu-id="7be66-110">Välj **skapa** när du är klar:</span><span class="sxs-lookup"><span data-stu-id="7be66-110">Select **Create** when finished:</span></span>

![Skapa en säkerhetsgrupp för nätverk](./media/nsg-quickstart-portal/create-nsg.png)

<span data-ttu-id="7be66-112">Välj en ny Nätverkssäkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="7be66-112">Select your new Network Security Group.</span></span> <span data-ttu-id="7be66-113">Välj 'Inkommande säkerhetsregler ”och sedan hello **Lägg till** knappen toocreate en regel:</span><span class="sxs-lookup"><span data-stu-id="7be66-113">Select 'Inbound security rules', then select hello **Add** button toocreate a rule:</span></span>

![Lägg till en inkommande regel](./media/nsg-quickstart-portal/add-inbound-rule.png)

<span data-ttu-id="7be66-115">Välj en gemensam **Service** hello nedrullningsbara menyn som *HTTP*.</span><span class="sxs-lookup"><span data-stu-id="7be66-115">Choose a common **Service** from hello drop-down menu, such as *HTTP*.</span></span> <span data-ttu-id="7be66-116">Du kan också välja *anpassad* tooprovide toouse för en viss port.</span><span class="sxs-lookup"><span data-stu-id="7be66-116">You can also select *Custom* tooprovide a specific port toouse.</span></span> <span data-ttu-id="7be66-117">Om du vill ändra hello prioritet eller namn.</span><span class="sxs-lookup"><span data-stu-id="7be66-117">If desired, change hello priority or name.</span></span> <span data-ttu-id="7be66-118">hello prioritet påverkar hello ordning i vilken regler - hello lägre hello numeriskt värde, hello tidigare hello-regel används.</span><span class="sxs-lookup"><span data-stu-id="7be66-118">hello priority affects hello order in which rules are applied - hello lower hello numerical value, hello earlier hello rule is applied.</span></span> <span data-ttu-id="7be66-119">Du kan också välja **Avancerat** hello överst i den här skärmen tooenter en specifik datakälla IP-block eller port intervall, till exempel.</span><span class="sxs-lookup"><span data-stu-id="7be66-119">You can also select **Advanced** at hello top of this screen tooenter a specific source IP block or port range, for example.</span></span> <span data-ttu-id="7be66-120">När du är klar väljer du **OK** toocreate hello regel:</span><span class="sxs-lookup"><span data-stu-id="7be66-120">When you are ready, select **OK** toocreate hello rule:</span></span>

![Skapa en regel för inkommande trafik](./media/nsg-quickstart-portal/create-inbound-rule.png)

<span data-ttu-id="7be66-122">Det sista steget är tooassociate nätverkssäkerheten gruppen med ett undernät eller ett visst nätverksgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="7be66-122">Your final step is tooassociate your Network Security Group with a subnet or a specific network interface.</span></span> <span data-ttu-id="7be66-123">Vi associera hello säkerhetsgrupp för nätverk med ett undernät.</span><span class="sxs-lookup"><span data-stu-id="7be66-123">Let's associate hello Network Security Group with a subnet.</span></span> <span data-ttu-id="7be66-124">Välj **undernät**, Välj **associera**:</span><span class="sxs-lookup"><span data-stu-id="7be66-124">Select **Subnets**, then choose **Associate**:</span></span>

![Associera en Nätverkssäkerhetsgrupp med ett undernät](./media/nsg-quickstart-portal/associate-subnet.png)

<span data-ttu-id="7be66-126">Välj det virtuella nätverket och välj sedan hello rätt undernät:</span><span class="sxs-lookup"><span data-stu-id="7be66-126">Select your virtual network, and then select hello appropriate subnet:</span></span>

![Associera en Nätverkssäkerhetsgrupp med virtuella nätverk](./media/nsg-quickstart-portal/select-vnet-subnet.png)

<span data-ttu-id="7be66-128">Nu har du skapat en Nätverkssäkerhetsgrupp skapas en regel för inkommande trafik som tillåter trafik på port 80 och som är associerade med ett undernät.</span><span class="sxs-lookup"><span data-stu-id="7be66-128">You have now created a Network Security Group, created an inbound rule that allows traffic on port 80, and associated it with a subnet.</span></span> <span data-ttu-id="7be66-129">Alla virtuella datorer som du ansluter toothat undernät kan nås på port 80.</span><span class="sxs-lookup"><span data-stu-id="7be66-129">Any VMs you connect toothat subnet are reachable on port 80.</span></span>

## <a name="more-information-on-network-security-groups"></a><span data-ttu-id="7be66-130">Mer information om Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="7be66-130">More information on Network Security Groups</span></span>
<span data-ttu-id="7be66-131">hello här snabb kommandon kan du tooget upp och körs med trafiken flödar tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="7be66-131">hello quick commands here allow you tooget up and running with traffic flowing tooyour VM.</span></span> <span data-ttu-id="7be66-132">Nätverkssäkerhetsgrupper ger många bra funktioner och granularitet för styra åtkomst tooyour resurser.</span><span class="sxs-lookup"><span data-stu-id="7be66-132">Network Security Groups provide many great features and granularity for controlling access tooyour resources.</span></span> <span data-ttu-id="7be66-133">Du kan läsa mer om [skapar en säkerhetsgrupp för nätverk och ACL-regler här](../../virtual-network/virtual-networks-create-nsg-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="7be66-133">You can read more about [creating a Network Security Group and ACL rules here](../../virtual-network/virtual-networks-create-nsg-arm-ps.md).</span></span>

<span data-ttu-id="7be66-134">Du bör placera dina virtuella datorer bakom en belastningsutjämnare i Azure för hög tillgänglighet webbprogram.</span><span class="sxs-lookup"><span data-stu-id="7be66-134">For highly available web applications, you should place your VMs behind an Azure Load Balancer.</span></span> <span data-ttu-id="7be66-135">hello belastningsutjämnare distribuerar trafik tooVMs med en Nätverkssäkerhetsgrupp som tillhandahåller trafikfiltrering.</span><span class="sxs-lookup"><span data-stu-id="7be66-135">hello load balancer distributes traffic tooVMs, with a Network Security Group that provides traffic filtering.</span></span> <span data-ttu-id="7be66-136">Mer information finns i [hur tooload saldo Linux virtuella datorer i Azure toocreate högtillgänglig programmet](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="7be66-136">For more information, see [How tooload balance Linux virtual machines in Azure toocreate a highly available application](tutorial-load-balancer.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7be66-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7be66-137">Next steps</span></span>
<span data-ttu-id="7be66-138">I det här exemplet skapas en enkel regel tooallow HTTP-trafik.</span><span class="sxs-lookup"><span data-stu-id="7be66-138">In this example, you created a simple rule tooallow HTTP traffic.</span></span> <span data-ttu-id="7be66-139">Du kan hitta information om hur du skapar mer detaljerad miljöer i hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="7be66-139">You can find information on creating more detailed environments in hello following articles:</span></span>

* [<span data-ttu-id="7be66-140">Översikt över Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="7be66-140">Azure Resource Manager overview</span></span>](../../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="7be66-141">Vad är en nätverkssäkerhetsgrupp (NSG)?</span><span class="sxs-lookup"><span data-stu-id="7be66-141">What is a Network Security Group (NSG)?</span></span>](../../virtual-network/virtual-networks-nsg.md)