---
title: aaaCreate en virtuell dator med en statisk offentlig IP-adress - Azure-portalen | Microsoft Docs
description: "Lär dig hur toocreate en virtuell dator med en statisk offentlig IP-adress med hjälp av hello Azure-portalen."
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
ms.openlocfilehash: f74d2132785f06148757409ee0a44b98d1e4b98e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-hello-azure-portal"></a><span data-ttu-id="66f4d-103">Skapa en virtuell dator med en statisk offentlig IP-adress med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="66f4d-103">Create a VM with a static public IP address using hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="66f4d-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="66f4d-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="66f4d-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="66f4d-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="66f4d-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="66f4d-106">Azure CLI</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="66f4d-107">Mall</span><span class="sxs-lookup"><span data-stu-id="66f4d-107">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="66f4d-108">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="66f4d-108">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="66f4d-109">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="66f4d-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="66f4d-110">Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="66f4d-110">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a><span data-ttu-id="66f4d-111">Skapa en virtuell dator med en statisk offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="66f4d-111">Create a VM with a static public IP</span></span>

<span data-ttu-id="66f4d-112">toocreate en virtuell dator med en statisk offentlig IP-adressen i hello Azure-portalen fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="66f4d-112">toocreate a VM with a static public IP address in hello Azure portal, complete hello following steps:</span></span>

1. <span data-ttu-id="66f4d-113">Från en webbläsare, navigerar du toohello [Azure-portalen](https://portal.azure.com) och vid behov, logga in med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="66f4d-113">From a browser, navigate toohello [Azure portal](https://portal.azure.com) and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="66f4d-114">Klicka på hello övre vänstra hörnet av hello portal **ny**>>**Compute**>**Windows Server 2012 R2 Datacenter**.</span><span class="sxs-lookup"><span data-stu-id="66f4d-114">On hello top left hand corner of hello portal, click **New**>>**Compute**>**Windows Server 2012 R2 Datacenter**.</span></span>
3. <span data-ttu-id="66f4d-115">I hello **Välj en distributionsmodell** väljer **Resource Manager** och på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="66f4d-115">In hello **Select a deployment model** list, select **Resource Manager** and click **Create**.</span></span>
4. <span data-ttu-id="66f4d-116">I hello **grunderna** bladet ange hello VM information som visas nedan och klickar sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="66f4d-116">In hello **Basics** blade, enter hello VM information as shown below, and then click **OK**.</span></span>
   
    ![Azure portal – grunderna](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)
5. <span data-ttu-id="66f4d-118">I hello **välja en storlek** bladet, klickar du på **A1 Standard** enligt nedan, och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="66f4d-118">In hello **Choose a size** blade, click **A1 Standard** as shown below, and then click **Select**.</span></span>
   
    ![Azure portal – Välj en storlek](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)
6. <span data-ttu-id="66f4d-120">I hello **inställningar** bladet, klickar du på **offentliga IP-adressen**, sedan i hello **skapa offentlig IP-adress** bladet under **tilldelning**, klickar du på **Statiska** enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="66f4d-120">In hello **Settings** blade, click **Public IP address**, then in hello **Create public IP address** blade, under **Assignment**, click **Static** as shown below.</span></span> <span data-ttu-id="66f4d-121">Och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="66f4d-121">And then click **OK**.</span></span>
   
    ![Azure portal – skapa offentlig IP-adress](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)
7. <span data-ttu-id="66f4d-123">I hello **inställningar** bladet, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="66f4d-123">In hello **Settings** blade, click **OK**.</span></span>
8. <span data-ttu-id="66f4d-124">Granska hello **sammanfattning** bladet som visas nedan och klickar sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="66f4d-124">Review hello **Summary** blade, as shown below, and then click **OK**.</span></span>
   
    ![Azure portal – skapa offentlig IP-adress](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)
9. <span data-ttu-id="66f4d-126">Observera hello ny panel på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="66f4d-126">Notice hello new tile in your dashboard.</span></span>
   
    ![Azure portal – skapa offentlig IP-adress](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)
10. <span data-ttu-id="66f4d-128">När du har skapat hello VM hello **inställningar** bladet visas enligt nedan</span><span class="sxs-lookup"><span data-stu-id="66f4d-128">Once hello VM is created, hello **Settings** blade will be displayed as shown below</span></span>
    
    ![Azure portal – skapa offentlig IP-adress](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)

