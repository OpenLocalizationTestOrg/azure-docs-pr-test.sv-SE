---
title: "Förstå delade IP-adresser i Azure DevTest Labs | Microsoft Docs"
description: "Lär dig hur Azure DevTest Labs använder delade IP-adresser för att minimera de offentliga IP-adresser som krävs för att komma åt ditt labb virtuella datorer."
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
ms.openlocfilehash: 9f6e1980bf5ea5b41da98a135d89f1c5159921a7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="understand-shared-ip-addresses-in-azure-devtest-labs"></a><span data-ttu-id="5035f-103">Förstå delade IP-adresser i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="5035f-103">Understand shared IP addresses in Azure DevTest Labs</span></span>

<span data-ttu-id="5035f-104">Azure DevTest Labs kan lab virtuella datorer som delar samma offentliga IP-adress för att minimera antalet offentliga IP-adresser för att komma åt labbet enskilda virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5035f-104">Azure DevTest Labs lets lab VMs share the same public IP address to minimize the number of public IP addresses required to access your individual lab VMs.</span></span>  <span data-ttu-id="5035f-105">Den här artikeln beskriver hur delade IP-adresser arbete och deras relaterade konfigurationsalternativ.</span><span class="sxs-lookup"><span data-stu-id="5035f-105">This article describes how shared IPs work and their related configuration options.</span></span>

## <a name="shared-ip-setting"></a><span data-ttu-id="5035f-106">Delade IP-inställning</span><span class="sxs-lookup"><span data-stu-id="5035f-106">Shared IP setting</span></span>

<span data-ttu-id="5035f-107">När du skapar ett labb som den finns i ett undernät i ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="5035f-107">When you create a lab, it resides in a subnet of a virtual network.</span></span>  <span data-ttu-id="5035f-108">Det här undernätet skapas som standard med **aktivera delad offentlig IP** inställd på *Ja*.</span><span class="sxs-lookup"><span data-stu-id="5035f-108">By default, this subnet is created with **Enable shared public IP** set to *Yes*.</span></span>  <span data-ttu-id="5035f-109">Den här konfigurationen skapas en offentlig IP-adress för hela undernätet.</span><span class="sxs-lookup"><span data-stu-id="5035f-109">This configuration creates one public IP address for the entire subnet.</span></span>  <span data-ttu-id="5035f-110">Mer information om hur du konfigurerar virtuella nätverk och undernät finns [konfigurera ett virtuellt nätverk i Azure DevTest Labs](devtest-lab-configure-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="5035f-110">For more information about configuring virtual networks and subnets, see [Configure a virtual network in Azure DevTest Labs](devtest-lab-configure-vnet.md).</span></span>

![Nytt labb undernät](media/devtest-lab-shared-ip/lab-subnet.png)

<span data-ttu-id="5035f-112">För befintliga testlabb kan du aktivera det här alternativet genom att välja **konfiguration och principer > virtuella nätverk**.</span><span class="sxs-lookup"><span data-stu-id="5035f-112">For existing labs, you can enable this option by selecting **Configuration and policies > Virtual Networks**.</span></span> <span data-ttu-id="5035f-113">Välj ett virtuellt nätverk i listan och väljer sedan **aktivera DELAD offentlig IP-** för valda undernätet.</span><span class="sxs-lookup"><span data-stu-id="5035f-113">Then, select a virtual network from the list and choose **ENABLE SHARED PUBLIC IP** for a selected subnet.</span></span> <span data-ttu-id="5035f-114">Du kan också inaktivera det här alternativet i en labb om du inte vill dela en offentlig IP-adress i labbet virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5035f-114">You can also disable this option in any lab if you don't want to share a public IP address across lab VMs.</span></span>

<span data-ttu-id="5035f-115">Alla virtuella datorer skapas i den här övningen standard att använda delade IP.</span><span class="sxs-lookup"><span data-stu-id="5035f-115">Any VMs created in this lab default to using a shared IP.</span></span>  <span data-ttu-id="5035f-116">När du skapar den virtuella datorn måste den här inställningen kan observeras i den **avancerade inställningar** bladet under **IP-adresskonfiguration**.</span><span class="sxs-lookup"><span data-stu-id="5035f-116">When creating the VM, this setting can be observed in the **Advanced settings** blade under **IP address configuration**.</span></span>

![Ny virtuell dator](media/devtest-lab-shared-ip/new-vm.png)

- <span data-ttu-id="5035f-118">**Delade:** alla virtuella datorer skapas som **delade** placeras i en resursgrupp (RG).</span><span class="sxs-lookup"><span data-stu-id="5035f-118">**Shared:** All VMs created as **Shared** are placed into one resource group (RG).</span></span> <span data-ttu-id="5035f-119">En IP-adress har tilldelats för som RG och alla virtuella datorer i RG ska använda IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="5035f-119">A single IP address is assigned for that RG and all VMs in the RG will use that IP address.</span></span>
- <span data-ttu-id="5035f-120">**Publik:** varje VM som du skapar har sin egen IP-adress och skapas i sin egen resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="5035f-120">**Public:** Every VM you create has its own IP address and is created in its own resource group.</span></span>
- <span data-ttu-id="5035f-121">**Privat:** varje virtuell dator som du skapar använder en privat IP-adress.</span><span class="sxs-lookup"><span data-stu-id="5035f-121">**Private:** Every VM you create uses a private IP address.</span></span> <span data-ttu-id="5035f-122">Du kan inte ansluta till den här virtuella datorn direkt från internet med fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="5035f-122">You will not be able to connect to this VM directly from the internet with Remote Desktop.</span></span>

<span data-ttu-id="5035f-123">När en virtuell dator med delade IP är aktiverat läggs till undernätet, lägger till den virtuella datorn till en belastningsutjämnare automatiskt i DevTest Labs och tilldelar TCP-portnummer för den offentliga IP-adressen vidarebefordran till RDP-porten på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5035f-123">Whenever a VM with shared IP enabled is added to the subnet, DevTest Labs automatically adds the VM to a load balancer and assigns a TCP port number on the public IP address, forwarding to the RDP port on the VM.</span></span>  

## <a name="using-the-shared-ip"></a><span data-ttu-id="5035f-124">Använda delade IP</span><span class="sxs-lookup"><span data-stu-id="5035f-124">Using the shared IP</span></span>

- <span data-ttu-id="5035f-125">**Linux-användare:** SSH till den virtuella datorn med hjälp av IP-adress eller fullständigt domännamn, följt av ett kolon och porten.</span><span class="sxs-lookup"><span data-stu-id="5035f-125">**Linux users:** SSH to the VM by using the IP address or fully qualified domain name, followed by a colon, followed by the port.</span></span> <span data-ttu-id="5035f-126">I bilden nedan RDP-adress att ansluta till den virtuella datorn är exempelvis `doclab-lab13998814308000.centralus.cloudapp.azure.com:51686`.</span><span class="sxs-lookup"><span data-stu-id="5035f-126">For example, in the image below, the RDP address to connect to the VM is `doclab-lab13998814308000.centralus.cloudapp.azure.com:51686`.</span></span>

  ![VM-exempel](media/devtest-lab-shared-ip/vm-info.png)

- <span data-ttu-id="5035f-128">**Windows-användare:** markerar du den **Anslut** knappen på Azure-portalen att hämta en förkonfigurerad RDP-fil och komma åt den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5035f-128">**Windows users:** Select the **Connect** button on the Azure portal to download a pre-configured RDP file and access the VM.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5035f-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5035f-129">Next steps</span></span>

* [<span data-ttu-id="5035f-130">Definiera principer för labbet i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="5035f-130">Define lab policies in Azure DevTest Labs</span></span>](devtest-lab-set-lab-policy.md)
* [<span data-ttu-id="5035f-131">Konfigurera ett virtuellt nätverk i Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="5035f-131">Configure a virtual network in Azure DevTest Labs</span></span>](devtest-lab-configure-vnet.md)





