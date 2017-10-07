---
title: "aaaManage nätverkssäkerhetsgrupper - Azure-portalen | Microsoft Docs"
description: "Lär dig hur toomanage nätverkssäkerhetsgrupper med hello Azure-portalen."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: faee5ac8-f4c4-4f97-ade5-197a37aad496
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 53fb29e60cbc2a535f6cf03e430d9e703e97b216
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-network-security-groups-using-hello-azure-portal"></a><span data-ttu-id="3b84c-103">Hantera nätverkssäkerhetsgrupper med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3b84c-103">Manage network security groups using hello Azure portal</span></span>

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="3b84c-104">Den här artikeln beskriver hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="3b84c-104">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="3b84c-105">Du kan också [skapa NSG: er i hello klassiska distributionsmodellen](virtual-networks-create-nsg-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="3b84c-105">You can also [create NSGs in hello classic deployment model](virtual-networks-create-nsg-classic-ps.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="3b84c-106">hello exempel PowerShell-kommandona nedan förväntar sig en enkel miljö som redan har skapats utifrån hello scenariot ovan.</span><span class="sxs-lookup"><span data-stu-id="3b84c-106">hello sample PowerShell commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="3b84c-107">Om du vill toorun hello kommandon som de visas i det här dokumentet, först skapa hello testmiljö genom att distribuera [mallen](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), klickar du på **distribuera tooAzure**, Ersätt hello Standardparametervärden Om nödvändigt och hello anvisningar i hello portal.</span><span class="sxs-lookup"><span data-stu-id="3b84c-107">If you want toorun hello commands as they are displayed in this document, first build hello test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span> <span data-ttu-id="3b84c-108">hello steg nedan används **RG NSG** som hello hello resurs grupp hello mallens namn har distribuerats till.</span><span class="sxs-lookup"><span data-stu-id="3b84c-108">hello steps below use **RG-NSG** as hello name of hello resource group hello template was deployed to.</span></span>

## <a name="create-hello-nsg-frontend-nsg"></a><span data-ttu-id="3b84c-109">Skapa hello NSG-klientdel NSG</span><span class="sxs-lookup"><span data-stu-id="3b84c-109">Create hello NSG-FrontEnd NSG</span></span>
<span data-ttu-id="3b84c-110">toocreate hello **NSG-klientdel** NSG enligt hello scenariot ovan, följ hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="3b84c-110">toocreate hello **NSG-FrontEnd** NSG as shown in hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="3b84c-111">Från en webbläsare, navigerar du toohttp://portal.azure.com och, om det behövs, logga in med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="3b84c-111">From a browser, navigate toohttp://portal.azure.com and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="3b84c-112">Klicka på **Bläddra >** > **Nätverkssäkerhetsgrupper**.</span><span class="sxs-lookup"><span data-stu-id="3b84c-112">Click **Browse >** > **Network Security Groups**.</span></span>
   
    ![Azure portal – NSG: er](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)
3. <span data-ttu-id="3b84c-114">I hello **Nätverkssäkerhetsgrupper** bladet, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="3b84c-114">In hello **Network security groups** blade, click **Add**.</span></span>
   
    ![Azure portal – NSG: er](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)
4. <span data-ttu-id="3b84c-116">I hello **skapa nätverkssäkerhetsgruppen** bladet, skapa en NSG som heter *NSG-klientdel* i hello *RG NSG* resursgrupp och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="3b84c-116">In hello **Create network security group** blade, create an NSG named *NSG-FrontEnd* in hello *RG-NSG* resource group, and then click **Create**.</span></span>
   
    ![Azure portal – NSG: er](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a><span data-ttu-id="3b84c-118">Skapa regler i en befintlig nätverkssäkerhetsgrupp (NSG)</span><span class="sxs-lookup"><span data-stu-id="3b84c-118">Create rules in an existing NSG</span></span>
<span data-ttu-id="3b84c-119">toocreate regler i en befintlig NSG från hello Azure-portalen gör hello nedan.</span><span class="sxs-lookup"><span data-stu-id="3b84c-119">toocreate rules in an existing NSG from hello Azure portal, follow hello steps below.</span></span>

1. <span data-ttu-id="3b84c-120">Klicka på **Bläddra >** > **Nätverkssäkerhetsgrupper**.</span><span class="sxs-lookup"><span data-stu-id="3b84c-120">Click **Browse >** > **Network security groups**.</span></span>
2. <span data-ttu-id="3b84c-121">Hello listan med NSG: er, på **NSG-klientdel** > **inkommande säkerhetsregler**</span><span class="sxs-lookup"><span data-stu-id="3b84c-121">In hello list of NSGs, click **NSG-FrontEnd** > **Inbound security rules**</span></span>
   
    ![Azure portal – NSG-klientdel](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)
3. <span data-ttu-id="3b84c-123">I hello lista över **inkommande säkerhetsregler**, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="3b84c-123">In hello list of **Inbound security rules**, click **Add**.</span></span>
   
    ![Azure portal – Lägg till regel](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)
4. <span data-ttu-id="3b84c-125">I hello **Lägg till ingående säkerhetsregel** bladet, skapa en regel med namnet *regel för web* med prioriteten för *200* åtkomst via *TCP* tooport *80* tooany VM från någon datakälla och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3b84c-125">In hello **Add inbound security rule** blade, create a rule named *web-rule* with priority of *200* allowing access via *TCP* tooport *80* tooany VM from any source, and then click **OK**.</span></span> <span data-ttu-id="3b84c-126">Observera att de flesta av de här inställningarna är standardvärden redan.</span><span class="sxs-lookup"><span data-stu-id="3b84c-126">Notice that most of these settings are default values already.</span></span>
   
    ![Azure portal – regelinställningar](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)
5. <span data-ttu-id="3b84c-128">Efter några sekunder visas hello ny regel i hello NSG.</span><span class="sxs-lookup"><span data-stu-id="3b84c-128">After a few seconds you will see hello new rule in hello NSG.</span></span>
   
    ![Azure portal – ny regel](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)
6. <span data-ttu-id="3b84c-130">Upprepa steg too6 toocreate en inkommande regel med namnet *rdp-regel* med prioritet *250* åtkomst via *TCP* tooport *3389* tooany virtuell dator från en källa.</span><span class="sxs-lookup"><span data-stu-id="3b84c-130">Repeat steps  too6 toocreate an inbound rule named *rdp-rule* with a priority of *250* allowing access via *TCP* tooport *3389* tooany VM from any source.</span></span>

## <a name="associate-hello-nsg-toohello-frontend-subnet"></a><span data-ttu-id="3b84c-131">Associera hello NSG toohello klientdel undernät</span><span class="sxs-lookup"><span data-stu-id="3b84c-131">Associate hello NSG toohello FrontEnd subnet</span></span>
1. <span data-ttu-id="3b84c-132">Klicka på **Bläddra >** > **resursgrupper** > **RG NSG**.</span><span class="sxs-lookup"><span data-stu-id="3b84c-132">Click **Browse >** > **Resource groups** > **RG-NSG**.</span></span>
2. <span data-ttu-id="3b84c-133">I hello **RG NSG** bladet, klickar du på **...**   >  **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="3b84c-133">In hello **RG-NSG** blade, click **...** > **TestVNet**.</span></span>
   
    ![Azure portal – TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)
3. <span data-ttu-id="3b84c-135">I hello **inställningar** bladet, klickar du på **undernät** > **klientdel** > **nätverkssäkerhetsgruppen**  >  **NSG-klientdel**.</span><span class="sxs-lookup"><span data-stu-id="3b84c-135">In hello **Settings** blade, click **Subnets** > **FrontEnd** > **Network security group** > **NSG-FrontEnd**.</span></span>
   
    ![Azure portal – undernätsinställningar](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)
4. <span data-ttu-id="3b84c-137">I hello **klientdel** bladet, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="3b84c-137">In hello **FrontEnd** blade, click **Save**.</span></span>
   
    ![Azure portal – undernätsinställningar](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-hello-nsg-backend-nsg"></a><span data-ttu-id="3b84c-139">Skapa hello NSG BackEnd NSG</span><span class="sxs-lookup"><span data-stu-id="3b84c-139">Create hello NSG-BackEnd NSG</span></span>
<span data-ttu-id="3b84c-140">toocreate hello **NSG BackEnd** NSG och koppla den toohello **BackEnd** undernät, följ hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="3b84c-140">toocreate hello **NSG-BackEnd** NSG and associate it toohello **BackEnd** subnet, follow hello steps below.</span></span>

1. <span data-ttu-id="3b84c-141">Upprepa hello stegen i [skapa hello NSG-klientdel NSG](#Create-the-NSG-FrontEnd-NSG) toocreate en NSG som heter *NSG-serverdelen*</span><span class="sxs-lookup"><span data-stu-id="3b84c-141">Repeat hello steps in [Create hello NSG-FrontEnd NSG](#Create-the-NSG-FrontEnd-NSG) toocreate an NSG named *NSG-BackEnd*</span></span>
2. <span data-ttu-id="3b84c-142">Upprepa hello stegen i [skapa regler i en befintlig NSG](#Create-rules-in-an-existing-NSG) toocreate hello **inkommande** regler i hello nedan.</span><span class="sxs-lookup"><span data-stu-id="3b84c-142">Repeat hello steps in [Create rules in an existing NSG](#Create-rules-in-an-existing-NSG) toocreate hello **inbound** rules in hello table below.</span></span>
   
   | <span data-ttu-id="3b84c-143">Regel för inkommande trafik</span><span class="sxs-lookup"><span data-stu-id="3b84c-143">Inbound rule</span></span> | <span data-ttu-id="3b84c-144">Regel för utgående trafik</span><span class="sxs-lookup"><span data-stu-id="3b84c-144">Outbound rule</span></span> |
   | --- | --- |
   | ![Azure portal - regel för inkommande trafik](./media/virtual-networks-create-nsg-arm-pportal/figure17.png) |![Azure portal - regel för utgående trafik](./media/virtual-networks-create-nsg-arm-pportal/figure18.png) |
3. <span data-ttu-id="3b84c-147">Upprepa hello stegen i [associera hello NSG toohello klientdel undernät](#Associate-the-NSG-to-the-FrontEnd-subnet) tooassociate hello **NSG Backend** NSG toohello **BackEnd** undernät.</span><span class="sxs-lookup"><span data-stu-id="3b84c-147">Repeat hello steps in [Associate hello NSG toohello FrontEnd subnet](#Associate-the-NSG-to-the-FrontEnd-subnet) tooassociate hello **NSG-Backend** NSG toohello **BackEnd** subnet.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3b84c-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3b84c-148">Next Steps</span></span>
* <span data-ttu-id="3b84c-149">Lär dig hur för[hantera befintliga NSG: er](virtual-network-manage-nsg-arm-portal.md)</span><span class="sxs-lookup"><span data-stu-id="3b84c-149">Learn how too[manage existing NSGs](virtual-network-manage-nsg-arm-portal.md)</span></span>
* <span data-ttu-id="3b84c-150">[Aktivera loggning](virtual-network-nsg-manage-log.md) för NSG: er.</span><span class="sxs-lookup"><span data-stu-id="3b84c-150">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>

