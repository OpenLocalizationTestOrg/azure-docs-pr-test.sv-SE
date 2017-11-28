---
title: "Skapa ett virtuellt Azure-nätverk (klassiskt) med flera undernät | Microsoft Docs"
description: "Lär dig hur du skapar ett virtuellt nätverk (klassiskt) med flera undernät i Azure."
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
ms.openlocfilehash: 95c2f4fe40590a8d809f634fb5b2c92d07421bb0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-virtual-network-classic-with-multiple-subnets"></a><span data-ttu-id="08e04-103">Skapa ett virtuellt nätverk (klassiskt) med flera undernät</span><span class="sxs-lookup"><span data-stu-id="08e04-103">Create a virtual network (classic) with multiple subnets</span></span>

> [!IMPORTANT]
> <span data-ttu-id="08e04-104">Azure har två [olika distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) för att skapa och arbeta med resurser: Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="08e04-104">Azure has two [different deployment models](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) for creating and working with resources: Resource Manager and classic.</span></span> <span data-ttu-id="08e04-105">Den här artikeln beskriver den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="08e04-105">This article covers using the classic deployment model.</span></span> <span data-ttu-id="08e04-106">Microsoft rekommenderar de flesta nya virtuella nätverk via den [Resource Manager](virtual-networks-create-vnet-arm-pportal.md) distributionsmodell.</span><span class="sxs-lookup"><span data-stu-id="08e04-106">Microsoft recommends creating most new virtual networks through the [Resource Manager](virtual-networks-create-vnet-arm-pportal.md) deployment model.</span></span>

<span data-ttu-id="08e04-107">I den här kursen lär du dig hur du skapar en grundläggande Azure virtuellt nätverk (klassiskt) som har olika offentliga och privata undernät.</span><span class="sxs-lookup"><span data-stu-id="08e04-107">In this tutorial, learn how to create a basic Azure virtual network (classic) that has separate public and private subnets.</span></span> <span data-ttu-id="08e04-108">Du kan skapa Azure-resurser, t.ex. virtuella datorer och molntjänster i ett undernät.</span><span class="sxs-lookup"><span data-stu-id="08e04-108">You can create Azure resources, like Virtual machines and Cloud services in a subnet.</span></span> <span data-ttu-id="08e04-109">Resurser som skapas på virtuella nätverk (klassiskt) kan kommunicera med varandra och resurser i andra nätverk som är anslutna till ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="08e04-109">Resources created in virtual networks (classic) can communicate with each other, and with resources in other networks connected to a virtual network.</span></span>

<span data-ttu-id="08e04-110">Mer information om alla [virtuellt nätverk](virtual-network-manage-network.md) och [undernät](virtual-network-manage-subnet.md) inställningar.</span><span class="sxs-lookup"><span data-stu-id="08e04-110">Learn more about all [virtual network](virtual-network-manage-network.md) and [subnet](virtual-network-manage-subnet.md) settings.</span></span>

> [!WARNING]
> <span data-ttu-id="08e04-111">Virtuella nätverk (klassiskt) omedelbart tas bort av Azure när en [prenumerationen har inaktiverats](../billing/billing-subscription-become-disable.md?toc=%2fazure%2fvirtual-network%2ftoc.json#you-reached-your-spending-limit).</span><span class="sxs-lookup"><span data-stu-id="08e04-111">Virtual networks (classic) are immediately deleted by Azure when a [subscription is disabled](../billing/billing-subscription-become-disable.md?toc=%2fazure%2fvirtual-network%2ftoc.json#you-reached-your-spending-limit).</span></span> <span data-ttu-id="08e04-112">Virtuella nätverk (klassiskt) tas bort oavsett om det finns resurser i det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="08e04-112">Virtual networks (classic) are deleted regardless of whether resources exist in the virtual network.</span></span> <span data-ttu-id="08e04-113">Om du återaktiverar prenumerationen senare måste du återskapa resurser som fanns i det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="08e04-113">If you later re-enable the subscription, resources that existed in the virtual network must be recreated.</span></span>

<span data-ttu-id="08e04-114">Du kan skapa ett virtuellt nätverk (klassiskt) med hjälp av den [Azure-portalen](#portal), [Azure-kommandoradsgränssnittet (CLI) 1.0](#azure-cli), eller [PowerShell](#powershell).</span><span class="sxs-lookup"><span data-stu-id="08e04-114">You can create a virtual network (classic) by using the [Azure portal](#portal), the [Azure command-line interface (CLI) 1.0](#azure-cli), or [PowerShell](#powershell).</span></span>

## <a name="portal"></a><span data-ttu-id="08e04-115">Portalen</span><span class="sxs-lookup"><span data-stu-id="08e04-115">Portal</span></span>

1. <span data-ttu-id="08e04-116">I en webbläsare går du till den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="08e04-116">In an Internet browser, go to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="08e04-117">Logga in med ditt [Azure-konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span><span class="sxs-lookup"><span data-stu-id="08e04-117">Log in using your [Azure account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account).</span></span> <span data-ttu-id="08e04-118">Om du inte har ett Azure-konto kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/offers/ms-azr-0044p).</span><span class="sxs-lookup"><span data-stu-id="08e04-118">If you don't have an Azure account, you can sign up for a [free trial](https://azure.microsoft.com/offers/ms-azr-0044p).</span></span>
2. <span data-ttu-id="08e04-119">Klicka på **+ ny** i portalen.</span><span class="sxs-lookup"><span data-stu-id="08e04-119">Click **+New** in the portal.</span></span>
3. <span data-ttu-id="08e04-120">Ange *för virtuella nätverk* i den **söka Marketplace** rutan längst upp i den **ny** bladet som visas.</span><span class="sxs-lookup"><span data-stu-id="08e04-120">Enter *Virtual network* in the **Search the Marketplace** box at the top of the **New** blade that appears.</span></span>  <span data-ttu-id="08e04-121">Klicka på **för virtuella nätverk** när den visas i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="08e04-121">Click **Virtual network** when it appears in the search results.</span></span>
4. <span data-ttu-id="08e04-122">Välj **klassiska** i den **Välj en distributionsmodell** rutan den **virtuellt nätverk** bladet som visas, klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="08e04-122">Select **Classic** in the **Select a deployment model** box in the **Virtual Network** blade that appears, then click **Create**.</span></span> 
5. <span data-ttu-id="08e04-123">Ange följande värden den **skapa virtuella nätverk (klassiska)** bladet och klicka sedan på **skapa**:</span><span class="sxs-lookup"><span data-stu-id="08e04-123">Enter the following values on the **Create virtual network (classic)** blade and then click **Create**:</span></span>

    |<span data-ttu-id="08e04-124">Inställning</span><span class="sxs-lookup"><span data-stu-id="08e04-124">Setting</span></span>|<span data-ttu-id="08e04-125">Värde</span><span class="sxs-lookup"><span data-stu-id="08e04-125">Value</span></span>|
    |---|---|
    |<span data-ttu-id="08e04-126">Namn</span><span class="sxs-lookup"><span data-stu-id="08e04-126">Name</span></span>|<span data-ttu-id="08e04-127">myVnet</span><span class="sxs-lookup"><span data-stu-id="08e04-127">myVnet</span></span>|
    |<span data-ttu-id="08e04-128">Adressutrymme</span><span class="sxs-lookup"><span data-stu-id="08e04-128">Address space</span></span>|<span data-ttu-id="08e04-129">10.0.0.0/16</span><span class="sxs-lookup"><span data-stu-id="08e04-129">10.0.0.0/16</span></span>|
    |<span data-ttu-id="08e04-130">Namn på undernät</span><span class="sxs-lookup"><span data-stu-id="08e04-130">Subnet name</span></span>|<span data-ttu-id="08e04-131">Offentligt</span><span class="sxs-lookup"><span data-stu-id="08e04-131">Public</span></span>|
    |<span data-ttu-id="08e04-132">Adressintervall för undernätet</span><span class="sxs-lookup"><span data-stu-id="08e04-132">Subnet address range</span></span>|<span data-ttu-id="08e04-133">10.0.0.0/24</span><span class="sxs-lookup"><span data-stu-id="08e04-133">10.0.0.0/24</span></span>|
    |<span data-ttu-id="08e04-134">Resursgrupp</span><span class="sxs-lookup"><span data-stu-id="08e04-134">Resource group</span></span>|<span data-ttu-id="08e04-135">Lämna **Skapa nytt** markerad och ange sedan **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="08e04-135">Leave **Create new** selected, and then enter **myResourceGroup**.</span></span>|
    |<span data-ttu-id="08e04-136">Prenumerationen och platsen</span><span class="sxs-lookup"><span data-stu-id="08e04-136">Subscription and location</span></span>|<span data-ttu-id="08e04-137">Välj din prenumeration och plats.</span><span class="sxs-lookup"><span data-stu-id="08e04-137">Select your subscription and location.</span></span>

    <span data-ttu-id="08e04-138">Om du har använt Azure lär du dig mer om [resursgrupper](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [prenumerationer](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), och [platser](https://azure.microsoft.com/regions) (kallas även *regioner*).</span><span class="sxs-lookup"><span data-stu-id="08e04-138">If you're new to Azure, learn more about [resource groups](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [subscriptions](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), and [locations](https://azure.microsoft.com/regions) (also referred to as *regions*).</span></span>
4. <span data-ttu-id="08e04-139">Du kan skapa en enda undernät när du skapar ett virtuellt nätverk i portalen.</span><span class="sxs-lookup"><span data-stu-id="08e04-139">In the portal, you can create only one subnet when you create a virtual network.</span></span> <span data-ttu-id="08e04-140">I den här självstudiekursen skapar du ett andra undernät när du har skapat det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="08e04-140">In this tutorial, you create a second subnet after you create the virtual network.</span></span> <span data-ttu-id="08e04-141">Senare kan du skapa Internet-tillgängliga resurser i den **offentliga** undernät.</span><span class="sxs-lookup"><span data-stu-id="08e04-141">You might later create Internet-accessible resources in the **Public** subnet.</span></span> <span data-ttu-id="08e04-142">Du kan också skapa resurser som inte är tillgänglig från Internet i den **privata** undernät.</span><span class="sxs-lookup"><span data-stu-id="08e04-142">You also might create resources that aren't accessible from the Internet in the **Private** subnet.</span></span> <span data-ttu-id="08e04-143">För att skapa andra undernät, ange **myVnet** i den **söka resurser** rutan längst upp på sidan.</span><span class="sxs-lookup"><span data-stu-id="08e04-143">To create the second subnet, enter **myVnet** in the **Search resources** box at the top of the page.</span></span> <span data-ttu-id="08e04-144">Klicka på **myVnet** när den visas i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="08e04-144">Click **myVnet** when it appears in the search results.</span></span>
5. <span data-ttu-id="08e04-145">Klicka på **undernät** (i den **inställningar** avsnitt) på den **skapa virtuella nätverk (klassiska)** bladet som visas.</span><span class="sxs-lookup"><span data-stu-id="08e04-145">Click **Subnets** (in the **SETTINGS** section) on the **Create virtual network (classic)** blade that appears.</span></span>
6. <span data-ttu-id="08e04-146">Klicka på **+ Lägg till** på den **myVnet - undernät** bladet som visas.</span><span class="sxs-lookup"><span data-stu-id="08e04-146">Click **+Add** on the **myVnet - Subnets** blade that appears.</span></span>
7. <span data-ttu-id="08e04-147">Ange **privata** för **namn** på den **Lägg till undernät** bladet.</span><span class="sxs-lookup"><span data-stu-id="08e04-147">Enter **Private** for **Name** on the **Add subnet** blade.</span></span> <span data-ttu-id="08e04-148">Ange **10.0.1.0/24** för **adressintervall**.</span><span class="sxs-lookup"><span data-stu-id="08e04-148">Enter **10.0.1.0/24** for **Address range**.</span></span>  <span data-ttu-id="08e04-149">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="08e04-149">Click **OK**.</span></span>
8. <span data-ttu-id="08e04-150">På den **myVnet - undernät** bladet hittar du den **offentliga** och **privata** undernät som du skapade.</span><span class="sxs-lookup"><span data-stu-id="08e04-150">On the **myVnet - Subnets** blade, you can see the **Public** and **Private** subnets that you created.</span></span>
9. <span data-ttu-id="08e04-151">**Valfria**: när du är klar med den här kursen kan du vill ta bort de resurser som du har skapat, så att du inte betalar användningsavgifter:</span><span class="sxs-lookup"><span data-stu-id="08e04-151">**Optional**: When you finish this tutorial, you might want to delete the resources that you created, so that you don't incur usage charges:</span></span>
    - <span data-ttu-id="08e04-152">Klicka på **översikt** på den **myVnet** bladet.</span><span class="sxs-lookup"><span data-stu-id="08e04-152">Click **Overview** on the **myVnet** blade.</span></span>
    - <span data-ttu-id="08e04-153">Klicka på den **ta bort** ikon på den **myVnet** bladet.</span><span class="sxs-lookup"><span data-stu-id="08e04-153">Click the **Delete** icon on the **myVnet** blade.</span></span>
    - <span data-ttu-id="08e04-154">Bekräfta borttagningen genom att klicka på **Ja** i den **ta bort virtuellt nätverk** rutan.</span><span class="sxs-lookup"><span data-stu-id="08e04-154">To confirm the deletion, click **Yes** in the **Delete virtual network** box.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="08e04-155">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="08e04-155">Azure CLI</span></span>

1. <span data-ttu-id="08e04-156">Du kan antingen [installera och konfigurera Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), eller använda CLI i gränssnittet för Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="08e04-156">You can either [install and configure the Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), or use the CLI within the Azure Cloud Shell.</span></span> <span data-ttu-id="08e04-157">Azure Cloud Shell är ett kostnadsfritt Bash-gränssnitt som du kan köra direkt i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="08e04-157">The Azure Cloud Shell is a free Bash shell that you can run directly within the Azure portal.</span></span> <span data-ttu-id="08e04-158">Den har Azure CLI förinstallerat och har konfigurerats för användning med ditt konto.</span><span class="sxs-lookup"><span data-stu-id="08e04-158">It has the Azure CLI preinstalled and configured to use with your account.</span></span> <span data-ttu-id="08e04-159">Om du vill få hjälp med CLI-kommandon, Skriv `azure <command> --help`.</span><span class="sxs-lookup"><span data-stu-id="08e04-159">To get help for CLI commands, type `azure <command> --help`.</span></span> 
2. <span data-ttu-id="08e04-160">I en CLI-session, logga in Azure med det kommando som följer.</span><span class="sxs-lookup"><span data-stu-id="08e04-160">In a CLI session, log in to Azure with the command that follows.</span></span> <span data-ttu-id="08e04-161">Om du klickar på **prova** i rutan nedan öppnas ett gränssnitt för molnet.</span><span class="sxs-lookup"><span data-stu-id="08e04-161">If you click **Try it** in the box below, a Cloud Shell opens.</span></span> <span data-ttu-id="08e04-162">Du kan logga in på Azure-prenumerationen, utan att ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="08e04-162">You can log in to your Azure subscription, without entering the following command:</span></span>

    ```azurecli-interactive
    azure login
    ```

3. <span data-ttu-id="08e04-163">Om du vill CLI i Service Management-läge kan du ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="08e04-163">To ensure the CLI is in Service Management mode, enter the following command:</span></span>

    ```azurecli-interactive
    azure config mode asm
    ```

4. <span data-ttu-id="08e04-164">Skapa ett virtuellt nätverk med ett privat undernät:</span><span class="sxs-lookup"><span data-stu-id="08e04-164">Create a virtual network with a private subnet:</span></span>

    ```azurecli-interactive
    azure network vnet create --vnet myVnet --address-space 10.0.0.0 --cidr 16  --subnet-name Private --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --location "East US"
    ```

5. <span data-ttu-id="08e04-165">Skapa en offentlig undernät i det virtuella nätverket:</span><span class="sxs-lookup"><span data-stu-id="08e04-165">Create a public subnet within the virtual network:</span></span>

    ```azurecli-interactive
    azure network vnet subnet create --name Public --vnet-name myVnet --address-prefix 10.0.1.0/24
    ```    
    
6. <span data-ttu-id="08e04-166">Granska de virtuella nätverk och undernät:</span><span class="sxs-lookup"><span data-stu-id="08e04-166">Review the virtual network and subnets:</span></span>

    ```azurecli-interactive
    azure network vnet show --vnet myVnet
    ```

7. <span data-ttu-id="08e04-167">**Valfria**: kanske du vill ta bort resurser som du skapade när du är klar med den här självstudiekursen så att du inte betalar användningsavgifter:</span><span class="sxs-lookup"><span data-stu-id="08e04-167">**Optional**: You might want to delete the resources that you created when you finish this tutorial, so that you don't incur usage charges:</span></span>

    ```azurecli-interactive
    azure network vnet delete --vnet myVnet --quiet
    ```

> [!NOTE]
> <span data-ttu-id="08e04-168">Även om du inte kan ange en resursgrupp för att skapa ett virtuellt nätverk (klassiskt) i med hjälp av CLI, Azure skapar det virtuella nätverket i en resursgrupp med namnet *standard-nätverk*.</span><span class="sxs-lookup"><span data-stu-id="08e04-168">Though you can't specify a resource group to create a virtual network (classic) in using the CLI, Azure creates the virtual network in a resource group named *Default-Networking*.</span></span>

## <a name="powershell"></a><span data-ttu-id="08e04-169">PowerShell</span><span class="sxs-lookup"><span data-stu-id="08e04-169">PowerShell</span></span>

1. <span data-ttu-id="08e04-170">Installera den senaste versionen av PowerShell [Azure](https://www.powershellgallery.com/packages/Azure) modul.</span><span class="sxs-lookup"><span data-stu-id="08e04-170">Install the latest version of the PowerShell [Azure](https://www.powershellgallery.com/packages/Azure) module.</span></span> <span data-ttu-id="08e04-171">Om du inte har använt Azure PowerShell kan du läsa [Översikt över Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="08e04-171">If you're new to Azure PowerShell, see [Azure PowerShell overview](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).</span></span>
2. <span data-ttu-id="08e04-172">Starta en PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="08e04-172">Start a PowerShell session.</span></span>
3. <span data-ttu-id="08e04-173">I PowerShell loggar du in i Azure genom att ange kommandot `Add-AzureAccount`.</span><span class="sxs-lookup"><span data-stu-id="08e04-173">In PowerShell, log in to Azure by entering the `Add-AzureAccount` command.</span></span>
4. <span data-ttu-id="08e04-174">Ändra följande sökväg och filnamn efter behov, och sedan exportera konfigurationsfilen befintliga nätverk:</span><span class="sxs-lookup"><span data-stu-id="08e04-174">Change the following path and filename, as appropriate, then export your existing network configuration file:</span></span>

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
    ```

5. <span data-ttu-id="08e04-175">Om du vill skapa ett virtuellt nätverk med offentliga och privata undernät, kan du använda valfri textredigerare att lägga till den **VirtualNetworkSite** elementet som följer till konfigurationsfilen på nätverket.</span><span class="sxs-lookup"><span data-stu-id="08e04-175">To create a virtual network with public and private subnets, use any text editor to add the **VirtualNetworkSite** element that follows to the network configuration file.</span></span>

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

    <span data-ttu-id="08e04-176">Granska hela [nätverk schemat för konfigurationsfilen](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="08e04-176">Review the full [network configuration file schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>

6. <span data-ttu-id="08e04-177">Importera konfigurationsfilen nätverk:</span><span class="sxs-lookup"><span data-stu-id="08e04-177">Import the network configuration file:</span></span>

    ```powershell
    Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
    ```

    > [!WARNING]
    > <span data-ttu-id="08e04-178">Importera en konfigurationsfil för ändrade nätverket kan orsaka ändringar av befintliga virtuella nätverk (klassiskt) i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="08e04-178">Importing a changed network configuration file can cause changes to existing virtual networks (classic) in your subscription.</span></span> <span data-ttu-id="08e04-179">Se till att du bara lägga till det tidigare virtuella nätverket och att du inte ändra eller ta bort alla befintliga virtuella nätverk från prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="08e04-179">Ensure you only add the previous virtual network and that you don't change or remove any existing virtual networks from your subscription.</span></span> 

7. <span data-ttu-id="08e04-180">Granska de virtuella nätverk och undernät:</span><span class="sxs-lookup"><span data-stu-id="08e04-180">Review the virtual network and subnets:</span></span>

    ```powershell
    Get-AzureVNetSite -VNetName "myVnet"
    ```

8. <span data-ttu-id="08e04-181">**Valfria**: kanske du vill ta bort resurser som du skapade när du är klar med den här självstudiekursen så att du inte betalar användningsavgifter.</span><span class="sxs-lookup"><span data-stu-id="08e04-181">**Optional**: You might want to delete the resources that you created when you finish this tutorial, so that you don't incur usage charges.</span></span> <span data-ttu-id="08e04-182">Om du vill ta bort det virtuella nätverket, Slutför stegen 4-6 igen, den här gången tar bort den **VirtualNetworkSite** element som du lade till i steg 5.</span><span class="sxs-lookup"><span data-stu-id="08e04-182">To delete the virtual network, complete steps 4-6 again, this time removing the **VirtualNetworkSite** element you added in step 5.</span></span>
 
> [!NOTE]
> <span data-ttu-id="08e04-183">Även om du inte kan ange en resursgrupp för att skapa ett virtuellt nätverk (klassiskt) i med hjälp av PowerShell, Azure skapar det virtuella nätverket i en resursgrupp med namnet *standard-nätverk*.</span><span class="sxs-lookup"><span data-stu-id="08e04-183">Though you can't specify a resource group to create a virtual network (classic) in using PowerShell, Azure creates the virtual network in a resource group named *Default-Networking*.</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="08e04-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="08e04-184">Next steps</span></span>

- <span data-ttu-id="08e04-185">Mer information om alla virtuella nätverk och undernätsinställningar, se [hantera virtuella nätverk](virtual-network-manage-network.md) och [hantera virtuella undernät](virtual-network-manage-subnet.md).</span><span class="sxs-lookup"><span data-stu-id="08e04-185">To learn about all virtual network and subnet settings, see [Manage virtual networks](virtual-network-manage-network.md) and [Manage virtual network subnets](virtual-network-manage-subnet.md).</span></span> <span data-ttu-id="08e04-186">Har du olika alternativ för att använda virtuella nätverk och undernät i en produktionsmiljö för att uppfylla olika krav.</span><span class="sxs-lookup"><span data-stu-id="08e04-186">You have various options for using virtual networks and subnets in a production environment to meet different requirements.</span></span>
- <span data-ttu-id="08e04-187">Filtrera inkommande och utgående trafik, skapa och använda [nätverkssäkerhetsgrupper](virtual-networks-nsg.md) till undernät.</span><span class="sxs-lookup"><span data-stu-id="08e04-187">To filter inbound and outbound subnet traffic, create and apply [network security groups](virtual-networks-nsg.md) to subnets.</span></span>
- <span data-ttu-id="08e04-188">Skapa en [Windows](../virtual-machines/windows/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) eller en [Linux](../virtual-machines/linux/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuella datorn och ansluter sedan till ett befintligt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="08e04-188">Create a [Windows](../virtual-machines/windows/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) or a [Linux](../virtual-machines/linux/classic/createportal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtual machine, and then connect it to an existing virtual network.</span></span>
- <span data-ttu-id="08e04-189">För att ansluta två virtuella nätverk i samma Azure-plats, skapa en [virtuellt nätverk peering](create-peering-different-deployment-models.md) mellan virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="08e04-189">To connect two virtual networks in the same Azure location, create a  [virtual network peering](create-peering-different-deployment-models.md) between the virtual networks.</span></span> <span data-ttu-id="08e04-190">Du kan peer (Resource Manager) för ett virtuellt nätverk till ett virtuellt nätverk (klassiskt), men du kan inte skapa en peering mellan två virtuella nätverk (klassiskt).</span><span class="sxs-lookup"><span data-stu-id="08e04-190">You can peer a virtual network (Resource Manager) to a virtual network (classic), but you cannot create a peering between two virtual networks (classic).</span></span>
- <span data-ttu-id="08e04-191">Anslut det virtuella nätverket till ett lokalt nätverk med hjälp av en [VPN-Gateway](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) eller [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) krets.</span><span class="sxs-lookup"><span data-stu-id="08e04-191">Connect the virtual network to an on-premises network by using a [VPN Gateway](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) or [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) circuit.</span></span>
