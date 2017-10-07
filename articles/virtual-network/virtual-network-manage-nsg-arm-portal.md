---
title: "aaaManage NSG: er med hjälp av hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur toomanage befintliga NSG: er med hjälp av hello Azure-portalen."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 5d55679d-57da-457c-97dc-1e1973909ee5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2016
ms.author: jdial
ms.openlocfilehash: ad9a4060bd81bae4597ad5a4f59622e10cd214cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-nsgs-using-hello-portal"></a><span data-ttu-id="9e97a-103">Hantera NSG: er med hjälp av hello portal</span><span class="sxs-lookup"><span data-stu-id="9e97a-103">Manage NSGs using hello portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="9e97a-104">Portal</span><span class="sxs-lookup"><span data-stu-id="9e97a-104">Portal</span></span>](virtual-network-manage-nsg-arm-portal.md)
> * [<span data-ttu-id="9e97a-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9e97a-105">PowerShell</span></span>](virtual-network-manage-nsg-arm-ps.md)
> * [<span data-ttu-id="9e97a-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9e97a-106">Azure CLI</span></span>](virtual-network-manage-nsg-arm-cli.md)
>

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="9e97a-107">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="9e97a-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="9e97a-108">Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="9e97a-108">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>
>

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="retrieve-information"></a><span data-ttu-id="9e97a-109">Hämta Information</span><span class="sxs-lookup"><span data-stu-id="9e97a-109">Retrieve Information</span></span>
<span data-ttu-id="9e97a-110">Du kan visa dina befintliga NSG: er, hämta regler för en befintlig NSG och ta reda på vilka resurser en NSG är kopplad till.</span><span class="sxs-lookup"><span data-stu-id="9e97a-110">You can view your existing NSGs, retrieve rules for an existing NSG, and find out what resources an NSG is associated to.</span></span>

### <a name="view-existing-nsgs"></a><span data-ttu-id="9e97a-111">Visa befintliga NSG: er</span><span class="sxs-lookup"><span data-stu-id="9e97a-111">View existing NSGs</span></span>

<span data-ttu-id="9e97a-112">tooview alla befintliga NSG: er i en prenumeration fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9e97a-112">tooview all existing NSGs in a subscription, complete hello following steps:</span></span>

1. <span data-ttu-id="9e97a-113">Från en webbläsare, navigerar du toohttp://portal.azure.com och, om det behövs, logga in med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="9e97a-113">From a browser, navigate toohttp://portal.azure.com and, if necessary, sign in with your Azure account.</span></span>

2. <span data-ttu-id="9e97a-114">Klicka på **Bläddra >** > **Nätverkssäkerhetsgrupper**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-114">Click **Browse >** > **Network security groups**.</span></span>

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure1.png)

3. <span data-ttu-id="9e97a-116">Kontrollera hello listan över NSG: er i hello **Nätverkssäkerhetsgrupper** bladet.</span><span class="sxs-lookup"><span data-stu-id="9e97a-116">Check hello list of NSGs in hello **Network security groups** blade.</span></span>

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure2.png)

### <a name="view-nsgs-in-a-resource-group"></a><span data-ttu-id="9e97a-118">Visa NSG: er i en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="9e97a-118">View NSGs in a resource group</span></span>

<span data-ttu-id="9e97a-119">tooview hello listan över NSG: er i hello **RG NSG** resursgrupp, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9e97a-119">tooview hello list of NSGs in hello **RG-NSG** resource group, complete hello following steps:</span></span>

1. <span data-ttu-id="9e97a-120">Klicka på **resursgrupper >** > **RG NSG** > **...** .</span><span class="sxs-lookup"><span data-stu-id="9e97a-120">Click **Resource groups >** > **RG-NSG** > **...**.</span></span>

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure3.png)

2. <span data-ttu-id="9e97a-122">Hello lista över resurser, leta efter hello NSG visas objekt som visas i hello **resurser** bladet nedan.</span><span class="sxs-lookup"><span data-stu-id="9e97a-122">In hello list of resources, look for items displaying hello NSG icon, as shown in hello **Resources** blade below.</span></span>

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure4.png)

### <a name="list-all-rules-for-an-nsg"></a><span data-ttu-id="9e97a-124">Visa en lista med alla regler för en NSG</span><span class="sxs-lookup"><span data-stu-id="9e97a-124">List all rules for an NSG</span></span>

<span data-ttu-id="9e97a-125">tooview hello regler för en NSG som heter **NSG-klientdel**fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9e97a-125">tooview hello rules of an NSG named **NSG-FrontEnd**, complete hello following steps:</span></span>

1. <span data-ttu-id="9e97a-126">Från hello **Nätverkssäkerhetsgrupper** bladet eller hello **resurser** bladet som visas ovan, klickar du på **NSG-klientdel**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-126">From hello **Network security groups** blade, or hello **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>

2. <span data-ttu-id="9e97a-127">I hello **inställningar** klickar du på **inkommande säkerhetsregler**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-127">In hello **Settings** tab, click **Inbound security rules**.</span></span>

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure5.png)

3. <span data-ttu-id="9e97a-129">Hej **inkommande säkerhetsregler** bladet visas enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="9e97a-129">hello **Inbound security rules** blade is displayed as shown below.</span></span>

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure6.png)

4. <span data-ttu-id="9e97a-131">I hello **inställningar** klickar du på **utgående säkerhetsregler** toosee hello utgående regler.</span><span class="sxs-lookup"><span data-stu-id="9e97a-131">In hello **Settings** tab, click **Outbound security rules** toosee hello outbound rules.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9e97a-132">standardregler för tooview, klicka på hello **standard regler** ikonen hello överst i hello blad som visar hello regler.</span><span class="sxs-lookup"><span data-stu-id="9e97a-132">tooview default rules, click hello **Default rules** icon at hello top of hello blade that displays hello rules.</span></span>
    >

### <a name="view-nsgs-associations"></a><span data-ttu-id="9e97a-133">Visa NSG: er associationer</span><span class="sxs-lookup"><span data-stu-id="9e97a-133">View NSGs associations</span></span>

<span data-ttu-id="9e97a-134">tooview vilka resurser hello **NSG-klientdel** NSG är associerat med, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9e97a-134">tooview what resources hello **NSG-FrontEnd** NSG is associate with, complete hello following steps:</span></span>

1. <span data-ttu-id="9e97a-135">Från hello **Nätverkssäkerhetsgrupper** bladet eller hello **resurser** bladet som visas ovan, klickar du på **NSG-klientdel**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-135">From hello **Network security groups** blade, or hello **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>

2. <span data-ttu-id="9e97a-136">I hello **inställningar** klickar du på **undernät** tooview vilka undernät är associerade toohello NSG.</span><span class="sxs-lookup"><span data-stu-id="9e97a-136">In hello **Settings** tab, click **Subnets** tooview what subnets are associated toohello NSG.</span></span>

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure7.png)

3. <span data-ttu-id="9e97a-138">I hello **inställningar** klickar du på **nätverksgränssnitt** tooview vad nätverkskort associeras toohello NSG.</span><span class="sxs-lookup"><span data-stu-id="9e97a-138">In hello **Settings** tab, click **Network interfaces** tooview what NICs are associated toohello NSG.</span></span>

## <a name="manage-rules"></a><span data-ttu-id="9e97a-139">Hantera regler</span><span class="sxs-lookup"><span data-stu-id="9e97a-139">Manage rules</span></span>
<span data-ttu-id="9e97a-140">Du kan lägga till regler tooan befintliga NSG, redigera befintliga regler och ta bort regler.</span><span class="sxs-lookup"><span data-stu-id="9e97a-140">You can add rules tooan existing NSG, edit existing rules, and remove rules.</span></span>

### <a name="add-a-rule"></a><span data-ttu-id="9e97a-141">Lägg till en regel</span><span class="sxs-lookup"><span data-stu-id="9e97a-141">Add a rule</span></span>
<span data-ttu-id="9e97a-142">tooadd en regel som tillåter **inkommande** trafik tooport **443** från alla datorn toohello **NSG-klientdel** NSG fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9e97a-142">tooadd a rule allowing **inbound** traffic tooport **443** from any machine toohello **NSG-FrontEnd** NSG, complete hello following steps:</span></span>

1. <span data-ttu-id="9e97a-143">Från hello **Nätverkssäkerhetsgrupper** bladet eller hello **resurser** bladet som visas ovan, klickar du på **NSG-klientdel**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-143">From hello **Network security groups** blade, or hello **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="9e97a-144">I hello **inställningar** klickar du på **inkommande säkerhetsregler**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-144">In hello **Settings** tab, click **Inbound security rules**.</span></span>
3. <span data-ttu-id="9e97a-145">I hello **inkommande säkerhetsregler** bladet, klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-145">In hello **Inbound security rules** blade, click **Add**.</span></span> <span data-ttu-id="9e97a-146">Sedan hello **Lägg till ingående säkerhetsregel** bladet fyller hello värden som visas nedan och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-146">Then, in hello **Add inbound security rule** blade, fill hello values as shown below, and then click **OK**.</span></span>

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure8.png)

    <span data-ttu-id="9e97a-148">Lägg märke till efter några sekunder hello ny regel i hello **inkommande säkerhetsregler** bladet.</span><span class="sxs-lookup"><span data-stu-id="9e97a-148">After a few seconds, notice hello new rule in hello **Inbound security rules** blade.</span></span>

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure9.png)

### <a name="change-a-rule"></a><span data-ttu-id="9e97a-150">Ändra en regel</span><span class="sxs-lookup"><span data-stu-id="9e97a-150">Change a rule</span></span>
<span data-ttu-id="9e97a-151">toochange hello regeln som skapade ovan tooallow inkommande trafik från hello **Internet** endast, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9e97a-151">toochange hello rule created above tooallow inbound traffic from hello **Internet** only, complete hello following steps:</span></span>

1. <span data-ttu-id="9e97a-152">Från hello **Nätverkssäkerhetsgrupper** bladet eller hello **resurser** bladet som visas ovan, klickar du på **NSG-klientdel**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-152">From hello **Network security groups** blade, or hello **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="9e97a-153">I hello **inställningar** klickar du på hello regeln som skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="9e97a-153">In hello **Settings** tab, click hello rule created above.</span></span>
3. <span data-ttu-id="9e97a-154">I hello **Tillåt https** bladet, ändra hello **källa** egenskapen enligt nedan och klickar sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-154">In hello **allow-https** blade, change hello **Source** property as shown below, and then click **Save**.</span></span>

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure10.png)

### <a name="delete-a-rule"></a><span data-ttu-id="9e97a-156">Ta bort en regel</span><span class="sxs-lookup"><span data-stu-id="9e97a-156">Delete a rule</span></span>

<span data-ttu-id="9e97a-157">toodelete hello regeln skapade ovan, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9e97a-157">toodelete hello rule created above, complete hello following steps:</span></span>

1. <span data-ttu-id="9e97a-158">Från hello **Nätverkssäkerhetsgrupper** bladet eller hello **resurser** bladet som visas ovan, klickar du på **NSG-klientdel**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-158">From hello **Network security groups** blade, or hello **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="9e97a-159">I hello **inställningar** klickar du på hello regeln som skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="9e97a-159">In hello **Settings** tab, click hello rule created above.</span></span>
3. <span data-ttu-id="9e97a-160">I hello **Tillåt https** bladet, klickar du på **ta bort**, och klicka sedan på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-160">In hello **allow-https** blade, click **Delete**, and then click **Yes**.</span></span>

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure11.png)

## <a name="manage-associations"></a><span data-ttu-id="9e97a-162">Hantera kopplingar</span><span class="sxs-lookup"><span data-stu-id="9e97a-162">Manage associations</span></span>
<span data-ttu-id="9e97a-163">Du kan koppla en NSG toosubnets och nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="9e97a-163">You can associate an NSG toosubnets and NICs.</span></span> <span data-ttu-id="9e97a-164">Du kan också koppla bort en NSG från alla resurser som den är kopplad till.</span><span class="sxs-lookup"><span data-stu-id="9e97a-164">You can also dissociate an NSG from any resource it's associated to.</span></span>

### <a name="associate-an-nsg-tooa-nic"></a><span data-ttu-id="9e97a-165">Koppla en NSG tooa NIC</span><span class="sxs-lookup"><span data-stu-id="9e97a-165">Associate an NSG tooa NIC</span></span>
<span data-ttu-id="9e97a-166">tooassociate hello **NSG-klientdel** NSG toohello **TestNICWeb1** NIC, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9e97a-166">tooassociate hello **NSG-FrontEnd** NSG toohello **TestNICWeb1** NIC, complete hello following steps:</span></span>

1. <span data-ttu-id="9e97a-167">Från hello **Nätverkssäkerhetsgrupper** bladet eller hello **resurser** bladet som visas ovan, klickar du på **NSG-klientdel**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-167">From hello **Network security groups** blade, or hello **Resources** blade shown above, click **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="9e97a-168">I hello **inställningar** klickar du på **nätverksgränssnitt** > **associera** > **TestNICWeb1**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-168">In hello **Settings** tab, click **Network interfaces** > **Associate** > **TestNICWeb1**.</span></span>

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure12.png)

### <a name="dissociate-an-nsg-from-a-nic"></a><span data-ttu-id="9e97a-170">Koppla bort en NSG från ett nätverkskort</span><span class="sxs-lookup"><span data-stu-id="9e97a-170">Dissociate an NSG from a NIC</span></span>

<span data-ttu-id="9e97a-171">toodissociate hello **NSG-klientdel** NSG från hello **TestNICWeb1** NIC, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9e97a-171">toodissociate hello **NSG-FrontEnd** NSG from hello **TestNICWeb1** NIC, complete hello following steps:</span></span>

1. <span data-ttu-id="9e97a-172">Hello Azure-portalen, klicka på **resursgrupper >** > **RG NSG** > **...**   >  **TestNICWeb1**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-172">From hello Azure portal, click **Resource groups >** > **RG-NSG** > **...** > **TestNICWeb1**.</span></span>

2. <span data-ttu-id="9e97a-173">I hello **TestNICWeb1** bladet, klickar du på **ändra säkerhet...**   >  **Ingen**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-173">In hello **TestNICWeb1** blade, click **Change security...** > **None**.</span></span>

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure13.png)

> [!NOTE]
> <span data-ttu-id="9e97a-175">Du kan också använda det här bladet tooassociate hello NIC tooany befintliga NSG.</span><span class="sxs-lookup"><span data-stu-id="9e97a-175">You can also use this blade tooassociate hello NIC tooany existing NSG.</span></span>
>

### <a name="dissociate-an-nsg-from-a-subnet"></a><span data-ttu-id="9e97a-176">Koppla bort en NSG från ett undernät</span><span class="sxs-lookup"><span data-stu-id="9e97a-176">Dissociate an NSG from a subnet</span></span>

<span data-ttu-id="9e97a-177">toodissociate hello **NSG-klientdel** NSG från hello **klientdel** undernät, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9e97a-177">toodissociate hello **NSG-FrontEnd** NSG from hello **FrontEnd** subnet, complete hello following steps:</span></span>

1. <span data-ttu-id="9e97a-178">Hello Azure-portalen, klicka på **resursgrupper >** > **RG NSG** > **...**   >  **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-178">From hello Azure portal, click **Resource groups >** > **RG-NSG** > **...** > **TestVNet**.</span></span>

2. <span data-ttu-id="9e97a-179">I hello **inställningar** bladet, klickar du på **undernät** > **klientdel** > **nätverkssäkerhetsgruppen**  >  **Ingen**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-179">In hello **Settings** blade, click **Subnets** > **FrontEnd** > **Network security group** > **None**.</span></span>

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure14.png)

3. <span data-ttu-id="9e97a-181">I hello **klientdel** bladet, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-181">In hello **FrontEnd** blade, click **Save**.</span></span>

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure15.png)

### <a name="associate-an-nsg-tooa-subnet"></a><span data-ttu-id="9e97a-183">Koppla en NSG tooa undernät</span><span class="sxs-lookup"><span data-stu-id="9e97a-183">Associate an NSG tooa subnet</span></span>

<span data-ttu-id="9e97a-184">tooassociate hello **NSG-klientdel** NSG toohello **FronEnd** undernät igen, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9e97a-184">tooassociate hello **NSG-FrontEnd** NSG toohello **FronEnd** subnet again, complete hello following steps:</span></span>

1. <span data-ttu-id="9e97a-185">Hello Azure-portalen, klicka på **resursgrupper >** > **RG NSG** > **...**   >  **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-185">From hello Azure portal, click **Resource groups >** > **RG-NSG** > **...** > **TestVNet**.</span></span>
2. <span data-ttu-id="9e97a-186">I hello **inställningar** bladet, klickar du på **undernät** > **klientdel** > **nätverkssäkerhetsgruppen**  >  **NSG-klientdel**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-186">In hello **Settings** blade, click **Subnets** > **FrontEnd** > **Network security group** > **NSG-FrontEnd**.</span></span>
3. <span data-ttu-id="9e97a-187">I hello **klientdel** bladet, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-187">In hello **FrontEnd** blade, click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="9e97a-188">Du kan även associera en NSG tooa undernät från thh NSG **inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="9e97a-188">You can also associate an NSG tooa subnet from thh NSG's **Settings** blade.</span></span>
>

## <a name="delete-an-nsg"></a><span data-ttu-id="9e97a-189">Ta bort en NSG</span><span class="sxs-lookup"><span data-stu-id="9e97a-189">Delete an NSG</span></span>
<span data-ttu-id="9e97a-190">Du kan bara ta bort en NSG om den inte är kopplad till tooany resurs.</span><span class="sxs-lookup"><span data-stu-id="9e97a-190">You can only delete an NSG if it's not associated tooany resource.</span></span> <span data-ttu-id="9e97a-191">toodelete en NSG fullständig hello följande:.</span><span class="sxs-lookup"><span data-stu-id="9e97a-191">toodelete an NSG, complete hello following steps:.</span></span>

1. <span data-ttu-id="9e97a-192">Hello Azure-portalen, klicka på **resursgrupper >** > **RG NSG** > **...**   >  **NSG-klientdel**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-192">From hello Azure portal, click **Resource groups >** > **RG-NSG** > **...** > **NSG-FrontEnd**.</span></span>
2. <span data-ttu-id="9e97a-193">I hello **inställningar** bladet, klickar du på **nätverksgränssnitt**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-193">In hello **Settings** blade, click **Network interfaces**.</span></span>
3. <span data-ttu-id="9e97a-194">Om det finns några nätverkskort i listan, klicka på hello NIC och följ steg 2 i [koppla bort en NSG från ett nätverkskort](#Dissociate-an-NSG-from-a-NIC).</span><span class="sxs-lookup"><span data-stu-id="9e97a-194">If there are any NICs listed, click hello NIC, and follow step 2 in [Dissociate an NSG from a NIC](#Dissociate-an-NSG-from-a-NIC).</span></span>
4. <span data-ttu-id="9e97a-195">Upprepa steg 3 för varje nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="9e97a-195">Repeat step 3 for each NIC.</span></span>
5. <span data-ttu-id="9e97a-196">I hello **inställningar** bladet, klickar du på **undernät**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-196">In hello **Settings** blade, click **Subnets**.</span></span>
6. <span data-ttu-id="9e97a-197">Om det finns inga undernät i listan, klicka på hello undernät och följ steg 2 och 3 i [koppla bort en NSG från ett undernät](#Dissociate-an-NSG-from-a-subnet).</span><span class="sxs-lookup"><span data-stu-id="9e97a-197">If there are any subnets listed, click hello subnet and follow steps 2 and 3 in [Dissociate an NSG from a subnet](#Dissociate-an-NSG-from-a-subnet).</span></span>
7. <span data-ttu-id="9e97a-198">Rullar vänstra toohello **NSG-klientdel** bladet Klicka **ta bort** > **Ja**.</span><span class="sxs-lookup"><span data-stu-id="9e97a-198">Scrolls left toohello **NSG-FrontEnd** blade, then click **Delete** > **Yes**.</span></span>

    ![Azure portal – NSG: er](./media/virtual-network-manage-nsg-arm-portal/figure16.png)

## <a name="next-steps"></a><span data-ttu-id="9e97a-200">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9e97a-200">Next steps</span></span>
* <span data-ttu-id="9e97a-201">[Aktivera loggning](virtual-network-nsg-manage-log.md) för NSG: er.</span><span class="sxs-lookup"><span data-stu-id="9e97a-201">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>
