---
title: aaaUnderstand delade IP-adresser i Azure DevTest Labs | Microsoft Docs
description: "Lär dig hur Azure DevTest Labs som använder delade IP-adresser toominimize hello offentliga IP-adresser krävs tooaccess ditt labb virtuella datorer."
services: devtest-lab
documentationcenter: na
author: camsoper
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: casoper
ms.openlocfilehash: 8756410117a9d550d567d372174bf1ea96703c74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-shared-ip-addresses-in-azure-devtest-labs"></a><span data-ttu-id="c67f3-103">Förstå delade IP-adresser i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="c67f3-103">Understand shared IP addresses in Azure DevTest Labs</span></span>

<span data-ttu-id="c67f3-104">Azure DevTest Labs kan lab VMs resursen hello samma offentliga IP-adress toominimize hello antal offentliga IP-adresser krävs tooaccess labbet enskilda virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c67f3-104">Azure DevTest Labs lets lab VMs share hello same public IP address toominimize hello number of public IP addresses required tooaccess your individual lab VMs.</span></span>  <span data-ttu-id="c67f3-105">Den här artikeln beskriver hur delade IP-adresser arbete och deras relaterade konfigurationsalternativ.</span><span class="sxs-lookup"><span data-stu-id="c67f3-105">This article describes how shared IPs work and their related configuration options.</span></span>

## <a name="shared-ip-setting"></a><span data-ttu-id="c67f3-106">Delade IP-inställning</span><span class="sxs-lookup"><span data-stu-id="c67f3-106">Shared IP setting</span></span>

<span data-ttu-id="c67f3-107">När du skapar ett labb som den finns i ett undernät i ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="c67f3-107">When you create a lab, it resides in a subnet of a virtual network.</span></span>  <span data-ttu-id="c67f3-108">Det här undernätet skapas som standard med **aktivera delad offentlig IP** ställa in också*Ja*.</span><span class="sxs-lookup"><span data-stu-id="c67f3-108">By default, this subnet is created with **Enable shared public IP** set too*Yes*.</span></span>  <span data-ttu-id="c67f3-109">Den här konfigurationen skapas en offentlig IP-adress för hello hela undernätet.</span><span class="sxs-lookup"><span data-stu-id="c67f3-109">This configuration creates one public IP address for hello entire subnet.</span></span>  <span data-ttu-id="c67f3-110">Mer information om hur du konfigurerar virtuella nätverk och undernät finns [konfigurera ett virtuellt nätverk i Azure DevTest Labs](devtest-lab-configure-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="c67f3-110">For more information about configuring virtual networks and subnets, see [Configure a virtual network in Azure DevTest Labs](devtest-lab-configure-vnet.md).</span></span>

![Nytt labb undernät](media/devtest-lab-shared-ip/lab-subnet.png)

<span data-ttu-id="c67f3-112">För befintliga testlabb kan du aktivera det här alternativet genom att välja **konfiguration och principer > virtuella nätverk**.</span><span class="sxs-lookup"><span data-stu-id="c67f3-112">For existing labs, you can enable this option by selecting **Configuration and policies > Virtual Networks**.</span></span> <span data-ttu-id="c67f3-113">Välj ett virtuellt nätverk hello listan och väljer sedan **aktivera DELAD offentlig IP-** för valda undernätet.</span><span class="sxs-lookup"><span data-stu-id="c67f3-113">Then, select a virtual network from hello list and choose **ENABLE SHARED PUBLIC IP** for a selected subnet.</span></span> <span data-ttu-id="c67f3-114">Du kan också inaktivera det här alternativet i en labb om du inte vill tooshare en offentlig IP-adress i labbet virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c67f3-114">You can also disable this option in any lab if you don't want tooshare a public IP address across lab VMs.</span></span>

<span data-ttu-id="c67f3-115">Virtuella datorer skapas i den här övningen standard toousing en delad IP-adress.</span><span class="sxs-lookup"><span data-stu-id="c67f3-115">Any VMs created in this lab default toousing a shared IP.</span></span>  <span data-ttu-id="c67f3-116">När du skapar hello VM är den här inställningen kan observeras i hello **avancerade inställningar** bladet under **IP-adresskonfiguration**.</span><span class="sxs-lookup"><span data-stu-id="c67f3-116">When creating hello VM, this setting can be observed in hello **Advanced settings** blade under **IP address configuration**.</span></span>

![Ny virtuell dator](media/devtest-lab-shared-ip/new-vm.png)

- <span data-ttu-id="c67f3-118">**Delade:** alla virtuella datorer skapas som **delade** placeras i en resursgrupp (RG).</span><span class="sxs-lookup"><span data-stu-id="c67f3-118">**Shared:** All VMs created as **Shared** are placed into one resource group (RG).</span></span> <span data-ttu-id="c67f3-119">En IP-adress har tilldelats för som RG och alla virtuella datorer i hello RG ska använda IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="c67f3-119">A single IP address is assigned for that RG and all VMs in hello RG will use that IP address.</span></span>
- <span data-ttu-id="c67f3-120">**Publik:** varje VM som du skapar har sin egen IP-adress och skapas i sin egen resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="c67f3-120">**Public:** Every VM you create has its own IP address and is created in its own resource group.</span></span>
- <span data-ttu-id="c67f3-121">**Privat:** varje virtuell dator som du skapar använder en privat IP-adress.</span><span class="sxs-lookup"><span data-stu-id="c67f3-121">**Private:** Every VM you create uses a private IP address.</span></span> <span data-ttu-id="c67f3-122">Du kommer inte att kunna tooconnect toothis VM direkt från hello internet med fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="c67f3-122">You will not be able tooconnect toothis VM directly from hello internet with Remote Desktop.</span></span>

<span data-ttu-id="c67f3-123">När en virtuell dator med delade IP är aktiverat läggs toohello undernät, lägger till hello VM tooa belastningsutjämnare automatiskt i DevTest Labs och tilldelar TCP-portnummer på hello offentlig IP-adress och vidarebefordrar toohello RDP-porten på hello VM.</span><span class="sxs-lookup"><span data-stu-id="c67f3-123">Whenever a VM with shared IP enabled is added toohello subnet, DevTest Labs automatically adds hello VM tooa load balancer and assigns a TCP port number on hello public IP address, forwarding toohello RDP port on hello VM.</span></span>  

## <a name="using-hello-shared-ip"></a><span data-ttu-id="c67f3-124">Med hjälp av hello delade IP</span><span class="sxs-lookup"><span data-stu-id="c67f3-124">Using hello shared IP</span></span>

- <span data-ttu-id="c67f3-125">**Linux-användare:** SSH toohello VM med hjälp av hello IP-adress eller fullständigt domännamn, följt av ett kolon följt av hello port.</span><span class="sxs-lookup"><span data-stu-id="c67f3-125">**Linux users:** SSH toohello VM by using hello IP address or fully qualified domain name, followed by a colon, followed by hello port.</span></span> <span data-ttu-id="c67f3-126">Till exempel i hello bilden nedan, hello RDP-adress tooconnect toohello VM är `doclab-lab13998814308000.centralus.cloudapp.azure.com:51686`.</span><span class="sxs-lookup"><span data-stu-id="c67f3-126">For example, in hello image below, hello RDP address tooconnect toohello VM is `doclab-lab13998814308000.centralus.cloudapp.azure.com:51686`.</span></span>

  ![VM-exempel](media/devtest-lab-shared-ip/vm-info.png)

- <span data-ttu-id="c67f3-128">**Windows-användare:** väljer hello **Anslut** på hello Azure portal toodownload förkonfigurerade RDP-filen och komma åt hello VM.</span><span class="sxs-lookup"><span data-stu-id="c67f3-128">**Windows users:** Select hello **Connect** button on hello Azure portal toodownload a pre-configured RDP file and access hello VM.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c67f3-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c67f3-129">Next steps</span></span>

* [<span data-ttu-id="c67f3-130">Definiera principer för labbet i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="c67f3-130">Define lab policies in Azure DevTest Labs</span></span>](devtest-lab-set-lab-policy.md)
* [<span data-ttu-id="c67f3-131">Konfigurera ett virtuellt nätverk i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="c67f3-131">Configure a virtual network in Azure DevTest Labs</span></span>](devtest-lab-configure-vnet.md)





