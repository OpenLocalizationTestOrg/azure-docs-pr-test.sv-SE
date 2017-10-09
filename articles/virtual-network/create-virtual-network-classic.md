---
title: "aaaCreate virtuellt Azure-nätverk (klassiskt) med flera undernät | Microsoft Docs"
description: "Lär dig hur toocreate ett virtuellt nätverk (klassiskt) med flera undernät i Azure."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: cc7b9ad08d5c26dba09584762bedf614d2847032
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-classic-with-multiple-subnets"></a><span data-ttu-id="2ba72-103">Skapa ett virtuellt nätverk (klassiskt) med flera undernät</span><span class="sxs-lookup"><span data-stu-id="2ba72-103">Create a virtual network (classic) with multiple subnets</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2ba72-104">Azure har två [olika distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) för att skapa och arbeta med resurser: Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="2ba72-104">Azure has two [different deployment models](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) for creating and working with resources: Resource Manager and classic.</span></span> <span data-ttu-id="2ba72-105">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="2ba72-105">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="2ba72-106">Microsoft rekommenderar de flesta nya virtuella nätverk via hello [Resource Manager](virtual-networks-create-vnet-arm-pportal.md) distributionsmodell.</span><span class="sxs-lookup"><span data-stu-id="2ba72-106">Microsoft recommends creating most new virtual networks through hello [Resource Manager](virtual-networks-create-vnet-arm-pportal.md) deployment model.</span></span>

<span data-ttu-id="2ba72-107">I den här kursen lär du dig hur toocreate ett grundläggande Azure virtuella nätverk (klassiska) som har separata offentliga och privata undernät.</span><span class="sxs-lookup"><span data-stu-id="2ba72-107">In this tutorial, learn how toocreate a basic Azure virtual network (classic) that has separate public and private subnets.</span></span> <span data-ttu-id="2ba72-108">Du kan skapa Azure-resurser, t.ex. virtuella datorer och molntjänster i ett undernät.</span><span class="sxs-lookup"><span data-stu-id="2ba72-108">You can create Azure resources, like Virtual machines and Cloud services in a subnet.</span></span> <span data-ttu-id="2ba72-109">Resurser som skapas på virtuella nätverk (klassiskt) kan kommunicera med varandra och resurser i andra nätverk anslutet tooa virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="2ba72-109">Resources created in virtual networks (classic) can communicate with each other, and with resources in other networks connected tooa virtual network.</span></span>

<span data-ttu-id="2ba72-110">Mer information om alla [virtuellt nätverk](virtual-network-manage-network.md) och [undernät](virtual-network-manage-subnet.md) inställningar.</span><span class="sxs-lookup"><span data-stu-id="2ba72-110">Learn more about all [virtual network](virtual-network-manage-network.md) and [subnet](virtual-network-manage-subnet.md) settings.</span></span>

> [!WARNING]
> <span data-ttu-id="2ba72-111">Virtuella nätverk (klassiskt) omedelbart tas bort av Azure när en [prenumerationen har inaktiverats](../billing/billing-subscription-become-disable.md?toc=%2fazure%2fvirtual-network%2ftoc.json#you-reached-your-spending-limit).</span><span class="sxs-lookup"><span data-stu-id="2ba72-111">Virtual networks (classic) are immediately deleted by Azure when a [subscription is disabled](../billing/billing-subscription-become-disable.md?toc=%2fazure%2fvirtual-network%2ftoc.json#you-reached-your-spending-limit).</span></span> <span data-ttu-id="2ba72-112">Virtuella nätverk (klassiskt) tas bort oavsett om det finns resurser i hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="2ba72-112">Virtual networks (classic) are deleted regardless of whether resources exist in hello virtual network.</span></span> <span data-ttu-id="2ba72-113">Om du senare återaktivera hello prenumeration, måste du återskapa resurser som fanns i hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="2ba72-113">If you later re-enable hello subscription, resources that existed in hello virtual network must be recreated.</span></span>

<span data-ttu-id="2ba72-114">Du kan skapa ett virtuellt nätverk (klassiska) med hjälp av hello [Azure-portalen](#portal), hello [Azure-kommandoradsgränssnittet (CLI) 1.0](#azure-cli), eller [PowerShell](#powershell).</span><span class="sxs-lookup"><span data-stu-id="2ba72-114">You can create a virtual network (classic) by using hello [Azure portal](#portal), hello [Azure command-line interface (CLI) 1.0](#azure-cli), or [PowerShell](#powershell).</span></span>

## <a name="portal"></a><span data-ttu-id="2ba72-115">Portalen</span><span class="sxs-lookup"><span data-stu-id="2ba72-115">Portal</span></span>

1. <span data-ttu-id="2ba72-116">I en webbläsare går toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2ba72-116">In an Internet browser, go toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="2ba72-117">Logga in med ditt [Azure-konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="2ba72-117">Log in using your [Azure account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="2ba72-118">Om du inte har ett Azure-konto kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="2ba72-118">If you don't have an Azure account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
2. <span data-ttu-id="2ba72-119">Klicka på **+ ny** hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="2ba72-119">Click **+New** in hello portal.</span></span>
3. <span data-ttu-id="2ba72-120">Ange *för virtuella nätverk* i hello **Sök hello Marketplace** rutan hello överst i hello **ny** bladet som visas.</span><span class="sxs-lookup"><span data-stu-id="2ba72-120">Enter *Virtual network* in hello **Search hello Marketplace** box at hello top of hello **New** blade that appears.</span></span>  <span data-ttu-id="2ba72-121">Klicka på **för virtuella nätverk** när den visas i sökresultaten hello.</span><span class="sxs-lookup"><span data-stu-id="2ba72-121">Click **Virtual network** when it appears in hello search results.</span></span>
4. <span data-ttu-id="2ba72-122">Välj **klassiska** i hello **Välj en distributionsmodell** rutan i hello **virtuellt nätverk** bladet som visas, klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="2ba72-122">Select **Classic** in hello **Select a deployment model** box in hello **Virtual Network** blade that appears, then click **Create**.</span></span> 
5. <span data-ttu-id="2ba72-123">Ange följande värden på hello hello **skapa virtuella nätverk (klassiska)** bladet och klicka sedan på **skapa**:</span><span class="sxs-lookup"><span data-stu-id="2ba72-123">Enter hello following values on hello **Create virtual network (classic)** blade and then click **Create**:</span></span>

    |<span data-ttu-id="2ba72-124">Inställning</span><span class="sxs-lookup"><span data-stu-id="2ba72-124">Setting</span></span>|<span data-ttu-id="2ba72-125">Värde</span><span class="sxs-lookup"><span data-stu-id="2ba72-125">Value</span></span>|
    |---|---|
    |<span data-ttu-id="2ba72-126">Namn</span><span class="sxs-lookup"><span data-stu-id="2ba72-126">Name</span></span>|<span data-ttu-id="2ba72-127">myVnet</span><span class="sxs-lookup"><span data-stu-id="2ba72-127">myVnet</span></span>|
    |<span data-ttu-id="2ba72-128">Adressutrymme</span><span class="sxs-lookup"><span data-stu-id="2ba72-128">Address space</span></span>|<span data-ttu-id="2ba72-129">10.0.0.0/16</span><span class="sxs-lookup"><span data-stu-id="2ba72-129">10.0.0.0/16</span></span>|
    |<span data-ttu-id="2ba72-130">Namn på undernät</span><span class="sxs-lookup"><span data-stu-id="2ba72-130">Subnet name</span></span>|<span data-ttu-id="2ba72-131">Offentligt</span><span class="sxs-lookup"><span data-stu-id="2ba72-131">Public</span></span>|
    |<span data-ttu-id="2ba72-132">Adressintervall för undernätet</span><span class="sxs-lookup"><span data-stu-id="2ba72-132">Subnet address range</span></span>|<span data-ttu-id="2ba72-133">10.0.0.0/24</span><span class="sxs-lookup"><span data-stu-id="2ba72-133">10.0.0.0/24</span></span>|
    |<span data-ttu-id="2ba72-134">Resursgrupp</span><span class="sxs-lookup"><span data-stu-id="2ba72-134">Resource group</span></span>|<span data-ttu-id="2ba72-135">Lämna **Skapa nytt** markerad och ange sedan **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="2ba72-135">Leave **Create new** selected, and then enter **myResourceGroup**.</span></span>|
    |<span data-ttu-id="2ba72-136">Prenumerationen och platsen</span><span class="sxs-lookup"><span data-stu-id="2ba72-136">Subscription and location</span></span>|<span data-ttu-id="2ba72-137">Välj din prenumeration och plats.</span><span class="sxs-lookup"><span data-stu-id="2ba72-137">Select your subscription and location.</span></span>

    <span data-ttu-id="2ba72-138">Om du är ny tooAzure lär du dig mer om [resursgrupper](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [prenumerationer](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), och [platser](https://azure.microsoft.com/regions) (kallas även tooas *regioner*).</span><span class="sxs-lookup"><span data-stu-id="2ba72-138">If you're new tooAzure, learn more about [resource groups](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [subscriptions](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), and [locations](https://azure.microsoft.com/regions) (also referred tooas *regions*).</span></span>
4. <span data-ttu-id="2ba72-139">Du kan skapa bara ett undernät när du skapar ett virtuellt nätverk i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="2ba72-139">In hello portal, you can create only one subnet when you create a virtual network.</span></span> <span data-ttu-id="2ba72-140">I den här kursen skapar du ett andra undernät när du har skapat hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="2ba72-140">In this tutorial, you create a second subnet after you create hello virtual network.</span></span> <span data-ttu-id="2ba72-141">Du kan senare skapar Internet-tillgängliga resurser i hello **offentliga** undernät.</span><span class="sxs-lookup"><span data-stu-id="2ba72-141">You might later create Internet-accessible resources in hello **Public** subnet.</span></span> <span data-ttu-id="2ba72-142">Du kan också skapa resurser som inte är tillgänglig från hello Internet i hello **privata** undernät.</span><span class="sxs-lookup"><span data-stu-id="2ba72-142">You also might create resources that aren't accessible from hello Internet in hello **Private** subnet.</span></span> <span data-ttu-id="2ba72-143">toocreate Hej andra undernät, ange **myVnet** i hello **söka resurser** rutan hello överst på hello sidan.</span><span class="sxs-lookup"><span data-stu-id="2ba72-143">toocreate hello second subnet, enter **myVnet** in hello **Search resources** box at hello top of hello page.</span></span> <span data-ttu-id="2ba72-144">Klicka på **myVnet** när den visas i sökresultaten hello.</span><span class="sxs-lookup"><span data-stu-id="2ba72-144">Click **myVnet** when it appears in hello search results.</span></span>
5. <span data-ttu-id="2ba72-145">Klicka på **undernät** (i hello **inställningar** avsnitt) på hello **skapa virtuella nätverk (klassiska)** bladet som visas.</span><span class="sxs-lookup"><span data-stu-id="2ba72-145">Click **Subnets** (in hello **SETTINGS** section) on hello **Create virtual network (classic)** blade that appears.</span></span>
6. <span data-ttu-id="2ba72-146">Klicka på **+ Lägg till** på hello **myVnet - undernät** bladet som visas.</span><span class="sxs-lookup"><span data-stu-id="2ba72-146">Click **+Add** on hello **myVnet - Subnets** blade that appears.</span></span>
7. <span data-ttu-id="2ba72-147">Ange **privata** för **namn** på hello **Lägg till undernät** bladet.</span><span class="sxs-lookup"><span data-stu-id="2ba72-147">Enter **Private** for **Name** on hello **Add subnet** blade.</span></span> <span data-ttu-id="2ba72-148">Ange **10.0.1.0/24** för **adressintervall**.</span><span class="sxs-lookup"><span data-stu-id="2ba72-148">Enter **10.0.1.0/24** for **Address range**.</span></span>  <span data-ttu-id="2ba72-149">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2ba72-149">Click **OK**.</span></span>
8. <span data-ttu-id="2ba72-150">På hello **myVnet - undernät** bladet hittar du hello **offentliga** och **privata** undernät som du skapade.</span><span class="sxs-lookup"><span data-stu-id="2ba72-150">On hello **myVnet - Subnets** blade, you can see hello **Public** and **Private** subnets that you created.</span></span>
9. <span data-ttu-id="2ba72-151">**Valfria**: när du är klar med den här kursen du kanske vill toodelete hello resurser som du skapade, så att du inte betalar användningsavgifter:</span><span class="sxs-lookup"><span data-stu-id="2ba72-151">**Optional**: When you finish this tutorial, you might want toodelete hello resources that you created, so that you don't incur usage charges:</span></span>
    - <span data-ttu-id="2ba72-152">Klicka på **översikt** på hello **myVnet** bladet.</span><span class="sxs-lookup"><span data-stu-id="2ba72-152">Click **Overview** on hello **myVnet** blade.</span></span>
    - <span data-ttu-id="2ba72-153">Klicka på hello **ta bort** ikon på hello **myVnet** bladet.</span><span class="sxs-lookup"><span data-stu-id="2ba72-153">Click hello **Delete** icon on hello **myVnet** blade.</span></span>
    - <span data-ttu-id="2ba72-154">tooconfirm hello borttagning, klickar du på **Ja** i hello **ta bort virtuellt nätverk** rutan.</span><span class="sxs-lookup"><span data-stu-id="2ba72-154">tooconfirm hello deletion, click **Yes** in hello **Delete virtual network** box.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="2ba72-155">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2ba72-155">Azure CLI</span></span>

1. <span data-ttu-id="2ba72-156">Du kan antingen [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), eller Använd hello CLI inom hello Azure Cloud-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="2ba72-156">You can either [install and configure hello Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), or use hello CLI within hello Azure Cloud Shell.</span></span> <span data-ttu-id="2ba72-157">hello Azure Cloud-gränssnittet är ett kostnadsfritt Bash-gränssnitt som du kan köra direkt i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2ba72-157">hello Azure Cloud Shell is a free Bash shell that you can run directly within hello Azure portal.</span></span> <span data-ttu-id="2ba72-158">Den har hello Azure CLI förinstallerat och konfigurerats toouse med ditt konto.</span><span class="sxs-lookup"><span data-stu-id="2ba72-158">It has hello Azure CLI preinstalled and configured toouse with your account.</span></span> <span data-ttu-id="2ba72-159">tooget hjälp för CLI-kommandona skriver `azure <command> --help`.</span><span class="sxs-lookup"><span data-stu-id="2ba72-159">tooget help for CLI commands, type `azure <command> --help`.</span></span> 
2. <span data-ttu-id="2ba72-160">Logga in tooAzure med hello-kommando som följer i en CLI-session.</span><span class="sxs-lookup"><span data-stu-id="2ba72-160">In a CLI session, log in tooAzure with hello command that follows.</span></span> <span data-ttu-id="2ba72-161">Om du klickar på **prova** hello i rutan nedan öppnar ett gränssnitt för molnet.</span><span class="sxs-lookup"><span data-stu-id="2ba72-161">If you click **Try it** in hello box below, a Cloud Shell opens.</span></span> <span data-ttu-id="2ba72-162">Du kan logga in tooyour Azure-prenumeration, utan att ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="2ba72-162">You can log in tooyour Azure subscription, without entering hello following command:</span></span>

    ```azurecli-interactive
    azure login
    ```

3. <span data-ttu-id="2ba72-163">tooensure hello CLI är i Service Management-läge och ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="2ba72-163">tooensure hello CLI is in Service Management mode, enter hello following command:</span></span>

    ```azurecli-interactive
    azure config mode asm
    ```

4. <span data-ttu-id="2ba72-164">Skapa ett virtuellt nätverk med ett privat undernät:</span><span class="sxs-lookup"><span data-stu-id="2ba72-164">Create a virtual network with a private subnet:</span></span>

    ```azurecli-interactive
    azure network vnet create --vnet myVnet --address-space 10.0.0.0 --cidr 16  --subnet-name Private --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --location "East US"
    ```

5. <span data-ttu-id="2ba72-165">Skapa en offentlig undernät inom hello virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="2ba72-165">Create a public subnet within hello virtual network:</span></span>

    ```azurecli-interactive
    azure network vnet subnet create --name Public --vnet-name myVnet --address-prefix 10.0.1.0/24
    ```    
    
6. <span data-ttu-id="2ba72-166">Granska hello virtuella nätverk och undernät:</span><span class="sxs-lookup"><span data-stu-id="2ba72-166">Review hello virtual network and subnets:</span></span>

    ```azurecli-interactive
    azure network vnet show --vnet myVnet
    ```

7. <span data-ttu-id="2ba72-167">**Valfria**: du kanske vill toodelete hello resurser som du skapade när du är klar med den här självstudiekursen så att du inte betalar användningsavgifter:</span><span class="sxs-lookup"><span data-stu-id="2ba72-167">**Optional**: You might want toodelete hello resources that you created when you finish this tutorial, so that you don't incur usage charges:</span></span>

    ```azurecli-interactive
    azure network vnet delete --vnet myVnet --quiet
    ```

> [!NOTE]
> <span data-ttu-id="2ba72-168">Även om du inte kan ange en resurs grupp toocreate ett virtuellt nätverk (klassiskt) i med hjälp av hello CLI, Azure skapar hello virtuellt nätverk i en resursgrupp med namnet *standard-nätverk*.</span><span class="sxs-lookup"><span data-stu-id="2ba72-168">Though you can't specify a resource group toocreate a virtual network (classic) in using hello CLI, Azure creates hello virtual network in a resource group named *Default-Networking*.</span></span>

## <a name="powershell"></a><span data-ttu-id="2ba72-169">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2ba72-169">PowerShell</span></span>

1. <span data-ttu-id="2ba72-170">Installera hello senaste versionen av hello PowerShell [Azure](https://www.powershellgallery.com/packages/Azure) modul.</span><span class="sxs-lookup"><span data-stu-id="2ba72-170">Install hello latest version of hello PowerShell [Azure](https://www.powershellgallery.com/packages/Azure) module.</span></span> <span data-ttu-id="2ba72-171">Om du är ny tooAzure PowerShell Se [översikt över Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2ba72-171">If you're new tooAzure PowerShell, see [Azure PowerShell overview](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
2. <span data-ttu-id="2ba72-172">Starta en PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="2ba72-172">Start a PowerShell session.</span></span>
3. <span data-ttu-id="2ba72-173">Logga in tooAzure i PowerShell genom att ange hello `Add-AzureAccount` kommando.</span><span class="sxs-lookup"><span data-stu-id="2ba72-173">In PowerShell, log in tooAzure by entering hello `Add-AzureAccount` command.</span></span>
4. <span data-ttu-id="2ba72-174">Ändra hello följande sökväg och filnamn efter behov, sedan exportera konfigurationsfilen befintliga nätverk:</span><span class="sxs-lookup"><span data-stu-id="2ba72-174">Change hello following path and filename, as appropriate, then export your existing network configuration file:</span></span>

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
    ```

5. <span data-ttu-id="2ba72-175">toocreate ett virtuellt nätverk med offentliga och privata undernät, använder alla text editor tooadd hello **VirtualNetworkSite** elementet som följer toohello nätverk konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="2ba72-175">toocreate a virtual network with public and private subnets, use any text editor tooadd hello **VirtualNetworkSite** element that follows toohello network configuration file.</span></span>

    ```xml
    <VirtualNetworkSite name="myVnet" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Private">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Public">
            <AddressPrefix>10.0.1.0/24</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    ```

    <span data-ttu-id="2ba72-176">Granska hello fullständig [nätverk schemat för konfigurationsfilen](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="2ba72-176">Review hello full [network configuration file schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>

6. <span data-ttu-id="2ba72-177">Importera konfigurationsfilen för hello nätverk:</span><span class="sxs-lookup"><span data-stu-id="2ba72-177">Import hello network configuration file:</span></span>

    ```powershell
    Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
    ```

    > [!WARNING]
    > <span data-ttu-id="2ba72-178">Importera en konfigurationsfil för ändrade nätverket kan orsaka ändringar tooexisting virtuella nätverk (klassiskt) i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2ba72-178">Importing a changed network configuration file can cause changes tooexisting virtual networks (classic) in your subscription.</span></span> <span data-ttu-id="2ba72-179">Se till att du bara lägga till hello tidigare virtuella nätverket och att du inte ändra eller ta bort alla befintliga virtuella nätverk från prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="2ba72-179">Ensure you only add hello previous virtual network and that you don't change or remove any existing virtual networks from your subscription.</span></span> 

7. <span data-ttu-id="2ba72-180">Granska hello virtuella nätverk och undernät:</span><span class="sxs-lookup"><span data-stu-id="2ba72-180">Review hello virtual network and subnets:</span></span>

    ```powershell
    Get-AzureVNetSite -VNetName "myVnet"
    ```

8. <span data-ttu-id="2ba72-181">**Valfria**: du kanske vill toodelete hello resurser som du skapade när du är klar med den här självstudiekursen så att du inte betalar användningsavgifter.</span><span class="sxs-lookup"><span data-stu-id="2ba72-181">**Optional**: You might want toodelete hello resources that you created when you finish this tutorial, so that you don't incur usage charges.</span></span> <span data-ttu-id="2ba72-182">toodelete hello virtuellt nätverk, fullständig steg 4 – 6 igen, denna tid att ta bort hello **VirtualNetworkSite** element som du lade till i steg 5.</span><span class="sxs-lookup"><span data-stu-id="2ba72-182">toodelete hello virtual network, complete steps 4-6 again, this time removing hello **VirtualNetworkSite** element you added in step 5.</span></span>
 
> [!NOTE]
> <span data-ttu-id="2ba72-183">Även om du inte kan ange en resurs grupp toocreate ett virtuellt nätverk (klassiskt) i med hjälp av PowerShell, Azure skapar hello virtuellt nätverk i en resursgrupp med namnet *standard-nätverk*.</span><span class="sxs-lookup"><span data-stu-id="2ba72-183">Though you can't specify a resource group toocreate a virtual network (classic) in using PowerShell, Azure creates hello virtual network in a resource group named *Default-Networking*.</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="2ba72-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2ba72-184">Next steps</span></span>

- <span data-ttu-id="2ba72-185">toolearn om alla virtuella nätverk och undernätsinställningar Se [hantera virtuella nätverk](virtual-network-manage-network.md) och [hantera virtuella undernät](virtual-network-manage-subnet.md).</span><span class="sxs-lookup"><span data-stu-id="2ba72-185">toolearn about all virtual network and subnet settings, see [Manage virtual networks](virtual-network-manage-network.md) and [Manage virtual network subnets](virtual-network-manage-subnet.md).</span></span> <span data-ttu-id="2ba72-186">Har du olika alternativ för att använda virtuella nätverk och undernät i en produktionsmiljö miljö toomeet olika krav.</span><span class="sxs-lookup"><span data-stu-id="2ba72-186">You have various options for using virtual networks and subnets in a production environment toomeet different requirements.</span></span>
- <span data-ttu-id="2ba72-187">toofilter inkommande och utgående trafik på undernät, skapa och använda [nätverkssäkerhetsgrupper](virtual-networks-nsg.md) toosubnets.</span><span class="sxs-lookup"><span data-stu-id="2ba72-187">toofilter inbound and outbound subnet traffic, create and apply [network security groups](virtual-networks-nsg.md) toosubnets.</span></span>
- <span data-ttu-id="2ba72-188">Skapa en [Windows](../virtual-machines/windows/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) eller en [Linux](../virtual-machines/linux/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuella datorn och ansluter sedan tooan befintligt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="2ba72-188">Create a [Windows](../virtual-machines/windows/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) or a [Linux](../virtual-machines/linux/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtual machine, and then connect it tooan existing virtual network.</span></span>
- <span data-ttu-id="2ba72-189">tooconnect två virtuella nätverk i hello samma Azure-plats måste du skapa en [virtuellt nätverk peering](create-peering-different-deployment-models.md) mellan hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="2ba72-189">tooconnect two virtual networks in hello same Azure location, create a  [virtual network peering](create-peering-different-deployment-models.md) between hello virtual networks.</span></span> <span data-ttu-id="2ba72-190">Du kan peer-virtuella nätverk (Resource Manager) tooa virtuella nätverk (klassiska), men du kan inte skapa en peering mellan två virtuella nätverk (klassiska).</span><span class="sxs-lookup"><span data-stu-id="2ba72-190">You can peer a virtual network (Resource Manager) tooa virtual network (classic), but you cannot create a peering between two virtual networks (classic).</span></span>
- <span data-ttu-id="2ba72-191">Ansluta hello virtuellt nätverk tooan lokalt nätverk med hjälp av en [VPN-Gateway](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) eller [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) krets.</span><span class="sxs-lookup"><span data-stu-id="2ba72-191">Connect hello virtual network tooan on-premises network by using a [VPN Gateway](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) or [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) circuit.</span></span>
