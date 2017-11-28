---
title: Skapa en virtuell dator med en statisk offentlig IP-adress - Azure-portalen | Microsoft Docs
description: "Lär dig hur du skapar en virtuell dator med en statisk offentlig IP-adress med hjälp av Azure portal."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e9546bcc-f300-428f-b94a-056c5bd29035
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 233e4eea8439320c1c7446e2c2b2e9d379351a3e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-the-azure-portal"></a><span data-ttu-id="9b78d-103">Skapa en virtuell dator med en statisk offentlig IP-adress med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="9b78d-103">Create a VM with a static public IP address using the Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="9b78d-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9b78d-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="9b78d-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9b78d-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="9b78d-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9b78d-106">Azure CLI</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="9b78d-107">Mall</span><span class="sxs-lookup"><span data-stu-id="9b78d-107">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="9b78d-108">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="9b78d-108">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="9b78d-109">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="9b78d-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="9b78d-110">Den här artikeln täcker distributionsmodell hanteraren för filserverresurser, som Microsoft rekommenderar för de flesta nya distributioner i stället för den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="9b78d-110">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the classic deployment model.</span></span>

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a><span data-ttu-id="9b78d-111">Skapa en virtuell dator med en statisk offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="9b78d-111">Create a VM with a static public IP</span></span>

<span data-ttu-id="9b78d-112">Om du vill skapa en virtuell dator med en statisk offentlig IP-adress i Azure-portalen, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="9b78d-112">To create a VM with a static public IP address in the Azure portal, complete the following steps:</span></span>

1. <span data-ttu-id="9b78d-113">Navigera till [Azure-portalen](https://portal.azure.com) från en webbläsare och logga in med ditt Azure-konto vid behov.</span><span class="sxs-lookup"><span data-stu-id="9b78d-113">From a browser, navigate to the [Azure portal](https://portal.azure.com) and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="9b78d-114">Klicka på det övre vänstra hörnet av portalen **ny**>>**Compute**>**Windows Server 2012 R2 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="9b78d-114">On the top left hand corner of the portal, click **New**>>**Compute**>**Windows Server 2012 R2 Datacenter**.</span></span>
3. <span data-ttu-id="9b78d-115">I den **Välj en distributionsmodell** väljer **Resource Manager** och på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="9b78d-115">In the **Select a deployment model** list, select **Resource Manager** and click **Create**.</span></span>
4. <span data-ttu-id="9b78d-116">I den **grunderna** bladet ange VM-information som visas nedan och klickar sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9b78d-116">In the **Basics** blade, enter the VM information as shown below, and then click **OK**.</span></span>
   
    ![Azure portal – grunderna](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)
5. <span data-ttu-id="9b78d-118">I den **välja en storlek** bladet, klickar du på **A1 Standard** enligt nedan, och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="9b78d-118">In the **Choose a size** blade, click **A1 Standard** as shown below, and then click **Select**.</span></span>
   
    ![Azure portal – Välj en storlek](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)
6. <span data-ttu-id="9b78d-120">I den **inställningar** bladet, klickar du på **offentliga IP-adressen**, i den **skapa offentlig IP-adress** bladet under **tilldelning**, klickar du på  **Statisk** enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="9b78d-120">In the **Settings** blade, click **Public IP address**, then in the **Create public IP address** blade, under **Assignment**, click **Static** as shown below.</span></span> <span data-ttu-id="9b78d-121">Och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9b78d-121">And then click **OK**.</span></span>
   
    ![Azure portal – skapa offentlig IP-adress](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)
7. <span data-ttu-id="9b78d-123">I den **inställningar** bladet, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9b78d-123">In the **Settings** blade, click **OK**.</span></span>
8. <span data-ttu-id="9b78d-124">Granska de **sammanfattning** bladet som visas nedan och klickar sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9b78d-124">Review the **Summary** blade, as shown below, and then click **OK**.</span></span>
   
    ![Azure portal – skapa offentlig IP-adress](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)
9. <span data-ttu-id="9b78d-126">Lägg märke till den nya ikonen på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="9b78d-126">Notice the new tile in your dashboard.</span></span>
   
    ![Azure portal – skapa offentlig IP-adress](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)
10. <span data-ttu-id="9b78d-128">När den virtuella datorn har skapats kan den **inställningar** bladet visas enligt nedan</span><span class="sxs-lookup"><span data-stu-id="9b78d-128">Once the VM is created, the **Settings** blade will be displayed as shown below</span></span>
    
    ![Azure portal – skapa offentlig IP-adress](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)

