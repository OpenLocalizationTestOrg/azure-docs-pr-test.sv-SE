---
title: "Virtuell dator med flera IP-adresser med hjälp av Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur du tilldelar flera IP-adresser till en virtuell dator med Azure CLI 1.0 | Resource Manager."
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: annahar
ms.openlocfilehash: 9f085dfa1fe4db36d58cb976bb550a46bf241ac7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-azure-cli-10"></a><span data-ttu-id="605fb-103">Tilldela flera IP-adresser till virtuella datorer med Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="605fb-103">Assign multiple IP addresses to virtual machines using Azure CLI 1.0</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

<span data-ttu-id="605fb-104">Den här artikeln förklaras hur du skapar en virtuell dator (VM) via Azure Resource Manager-distributionsmodellen med hjälp av Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="605fb-104">This article explains how to create a virtual machine (VM) through the Azure Resource Manager deployment model using the Azure CLI 1.0.</span></span> <span data-ttu-id="605fb-105">Flera IP-adresser kan inte tilldelas till resurser som skapats via den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="605fb-105">Multiple IP addresses cannot be assigned to resources created through the classic deployment model.</span></span> <span data-ttu-id="605fb-106">Om du vill veta mer om Azure distributionsmodeller kan läsa den [förstår distributionsmodellerna](../resource-manager-deployment-model.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="605fb-106">To learn more about Azure deployment models, read the [Understand deployment models](../resource-manager-deployment-model.md) article.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <span data-ttu-id="605fb-107"><a name = "create"></a>Skapa en virtuell dator med flera IP-adresser</span><span class="sxs-lookup"><span data-stu-id="605fb-107"><a name = "create"></a>Create a VM with multiple IP addresses</span></span>

<span data-ttu-id="605fb-108">Du kan göra detta med hjälp av Azure CLI 1.0 (den här artikeln) eller [Azure CLI 2.0](virtual-network-multiple-ip-addresses-cli.md).</span><span class="sxs-lookup"><span data-stu-id="605fb-108">You can complete this task using the Azure CLI 1.0 (this article) or the [Azure CLI 2.0](virtual-network-multiple-ip-addresses-cli.md).</span></span> <span data-ttu-id="605fb-109">De steg som följer beskrivs hur du skapar ett exempel VM med flera IP-adresser, enligt beskrivningen i scenariot.</span><span class="sxs-lookup"><span data-stu-id="605fb-109">The steps that follow explain how to create an example VM with multiple IP addresses, as described in the scenario.</span></span> <span data-ttu-id="605fb-110">Ändra variabeln namn och IP-adresstyper som krävs för din implementering.</span><span class="sxs-lookup"><span data-stu-id="605fb-110">Change variable names and IP address types as required for your implementation.</span></span>

1. <span data-ttu-id="605fb-111">Installera och konfigurera Azure CLI 1.0 genom att följa stegen i den [installera och konfigurera Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikeln och logga in på ditt Azure-konto med den `azure-login` kommando.</span><span class="sxs-lookup"><span data-stu-id="605fb-111">Install and configure the Azure CLI 1.0 by following the steps in the [Install and Configure the Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) article and log into your Azure account with the `azure-login` command.</span></span>

2. <span data-ttu-id="605fb-112">Skapa en resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="605fb-112">Create a resource group:</span></span>
    
    ```azurecli
    RgName=myResourceGroup
    Location=westcentralus
    azure group create --name $RgName --location $Location
    ```
3. <span data-ttu-id="605fb-113">Skapa ett virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="605fb-113">Create a virtual network:</span></span>

    ```azurecli
    azure network vnet create --resource-group $RgName --location $Location --name myVNet \
    --address-prefixes 10.0.0.0/16
    ```
4. <span data-ttu-id="605fb-114">Skapa ett undernät i det virtuella nätverket:</span><span class="sxs-lookup"><span data-stu-id="605fb-114">Create a subnet in the virtual network:</span></span>

    ```azurecli
    azure network vnet subnet create --name mySubnet --resource-group $RgName --vnet-name myVNet \
    --address-prefix 10.0.0.0/24
    ```
    
5. <span data-ttu-id="605fb-115">Skapa ett lagringskonto för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="605fb-115">Create  a storage account for the VM.</span></span> <span data-ttu-id="605fb-116">Innan du kör följande kommando ersätter *mittlagringskonto* med ett unikt namn.</span><span class="sxs-lookup"><span data-stu-id="605fb-116">Before running the following command, replace *mystorageaccount* with a unique name.</span></span> <span data-ttu-id="605fb-117">Namnet måste vara unikt i Azure:</span><span class="sxs-lookup"><span data-stu-id="605fb-117">The name must be unique across Azure:</span></span>

    ```azurecli
    az storage account create --resource-group $RgName --location $Location --name mystorageaccount \
    --kind Storage --sku Standard_LRS
    ```

6. <span data-ttu-id="605fb-118">Skapa IP-konfigurationer, ett nätverkskort och tilldela IP-konfigurationer till nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="605fb-118">Create the IP configurations, a NIC, and assign the IP configurations to the NIC.</span></span> <span data-ttu-id="605fb-119">Du kan lägga till, ta bort eller ändra konfigurationer efter behov.</span><span class="sxs-lookup"><span data-stu-id="605fb-119">You can add, remove, or change the configurations as necessary.</span></span> <span data-ttu-id="605fb-120">Följande konfigurationer beskrivs i scenariot:</span><span class="sxs-lookup"><span data-stu-id="605fb-120">The following configurations are described in the scenario:</span></span>

    <span data-ttu-id="605fb-121">**IPConfig-1**</span><span class="sxs-lookup"><span data-stu-id="605fb-121">**IPConfig-1**</span></span>

    <span data-ttu-id="605fb-122">Ange de kommandon som följer för att skapa:</span><span class="sxs-lookup"><span data-stu-id="605fb-122">Enter the commands that follow to create:</span></span>

    - <span data-ttu-id="605fb-123">En offentlig IP-adressresurs med en statisk offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="605fb-123">A public IP address resource with a static public IP address</span></span>
    - <span data-ttu-id="605fb-124">Ett nätverkskort tilldelas den offentliga IP-adress och en statisk privat IP-adress.</span><span class="sxs-lookup"><span data-stu-id="605fb-124">A NIC, assigning the public IP address and a static private IP address to it.</span></span>
    
    <span data-ttu-id="605fb-125">Ersätt *mypublicdns* med ett namn som är unikt i Azure-plats.</span><span class="sxs-lookup"><span data-stu-id="605fb-125">Replace *mypublicdns* with a name that is unique within the Azure location.</span></span>

      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP1 \
      --domain-name-label mypublicdns --allocation-method Static
        
      azure network nic create --resource-group $RgName --location $Location --name myNic1 \
      --private-ip-address 10.0.0.4 --subnet-name mySubnet --subnet-vnet-name myVNet \
      --subnet-name mySubnet --public-ip-name myPublicIP1
      ```

      > [!NOTE]
      > <span data-ttu-id="605fb-126">Offentliga IP-adresser har en låg kostnad.</span><span class="sxs-lookup"><span data-stu-id="605fb-126">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="605fb-127">Mer information om priser för IP-adress i [IP-adress priser](https://azure.microsoft.com/pricing/details/ip-addresses) sidan.</span><span class="sxs-lookup"><span data-stu-id="605fb-127">To learn more about IP address pricing, read the [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="605fb-128">Det finns en gräns för antalet offentliga IP-adresser som kan användas i en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="605fb-128">There is a limit to the number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="605fb-129">Mer information om gränserna finns i artikeln om [Azure-begränsningar](../azure-subscription-service-limits.md#networking-limits).</span><span class="sxs-lookup"><span data-stu-id="605fb-129">To learn more about the limits, read the [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>

    <span data-ttu-id="605fb-130">**IPConfig-2**</span><span class="sxs-lookup"><span data-stu-id="605fb-130">**IPConfig-2**</span></span>

     <span data-ttu-id="605fb-131">Ange följande kommandon för att skapa en ny offentlig IP-adressresurs och en ny IP-konfiguration med en statisk offentlig IP-adress och en statisk privat IP-adress:</span><span class="sxs-lookup"><span data-stu-id="605fb-131">Enter the following commands to create a new public IP address resource and a new IP configuration with a static public IP address and a static private IP address:</span></span>
    
      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP2
      --domain-name-label mypublicdns2 --allocation-method Static

      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --name IPConfig-2
      --private-ip-address 10.0.0.5 --public-ip-name myPublicIP2
      ```

    <span data-ttu-id="605fb-132">**IPConfig-3**</span><span class="sxs-lookup"><span data-stu-id="605fb-132">**IPConfig-3**</span></span>

    <span data-ttu-id="605fb-133">Ange följande kommandon för att skapa en IP-konfiguration med en statisk privat IP-adress och ingen offentlig IP-adress:</span><span class="sxs-lookup"><span data-stu-id="605fb-133">Enter the following commands to create an IP configuration with a static private IP address and no public IP address:</span></span>

      ```azurecli
      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --private-ip-address 10.0.0.6 \
      --name IPConfig-3
      ```

    >[!NOTE] 
    ><span data-ttu-id="605fb-134">Även om den här artikeln tilldelar alla IP-konfigurationer till ett enda nätverkskort, kan du också tilldela flera IP-konfigurationer till alla NIC på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="605fb-134">Though this article assigns all IP configurations to a single NIC, you can also assign multiple IP configurations to any NIC in a VM.</span></span> <span data-ttu-id="605fb-135">Läsa skapa en virtuell dator med flera nätverkskort artikeln om du vill veta hur du skapar en virtuell dator med flera nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="605fb-135">To learn how to create a VM with multiple NICs, read the Create a VM with multiple NICs article.</span></span>

7. <span data-ttu-id="605fb-136">Skapa en virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="605fb-136">Create a Linux VM</span></span> 

    ```azurecli
    az vm create --resource-group $RgName --name myVM1 --location $Location --nics myNic1 \
    --image UbuntuLTS --ssh-key-value ~/.ssh/id_rsa.pub --admin-username azureuser
    ```

    <span data-ttu-id="605fb-137">Om du vill ändra Standard DS2 v2 VM-storlek, till exempel bara lägga till följande egenskap `--vm-size Standard_DS3_v2` till den `azure vm create` kommandot i steg 6.</span><span class="sxs-lookup"><span data-stu-id="605fb-137">To change the VM size to Standard DS2 v2, for example, simply add the following property `--vm-size Standard_DS3_v2` to the `azure vm create` command in step 6.</span></span>

8. <span data-ttu-id="605fb-138">Ange följande kommando för att visa nätverkskortet och de associera IP-konfigurationerna:</span><span class="sxs-lookup"><span data-stu-id="605fb-138">Enter the following command to view the NIC and the associated IP configurations:</span></span>

    ```azurecli
    azure network nic show --resource-group $RgName --name myNic1
    ```
9. <span data-ttu-id="605fb-139">Lägga till privata IP-adresser i Virtuella operativsystem genom att slutföra stegen för operativsystemet i den [lägga till IP-adresser till ett VM-operativsystem](#os-config) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="605fb-139">Add the private IP addresses to the VM operating system by completing the steps for your operating system in the [Add IP addresses to a VM operating system](#os-config) section of this article.</span></span>

## <span data-ttu-id="605fb-140"><a name="add"></a>Lägg till IP-adresser till en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="605fb-140"><a name="add"></a>Add IP addresses to a VM</span></span>

<span data-ttu-id="605fb-141">Du kan lägga till ytterligare privata och offentliga IP-adresser till en befintlig NIC genom att slutföra de steg som följer.</span><span class="sxs-lookup"><span data-stu-id="605fb-141">You can add additional private and public IP addresses to an existing NIC by completing the steps that follow.</span></span> <span data-ttu-id="605fb-142">Exemplen bygger på den [scenariot](#Scenario) beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="605fb-142">The examples build upon the [scenario](#Scenario) described in this article.</span></span>

1. <span data-ttu-id="605fb-143">Öppna Azure CLI och utföra stegen i det här avsnittet i en enda CLI-session.</span><span class="sxs-lookup"><span data-stu-id="605fb-143">Open Azure CLI and complete the remaining steps in this section within a single CLI session.</span></span> <span data-ttu-id="605fb-144">Om du inte redan har installerat och konfigurerat Azure-CLI kan du slutföra stegen i den [installera och konfigurera Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel och logga in på ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="605fb-144">If you don't already have Azure CLI installed and configured, complete the steps in the [Install and Configure the Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) article and log into your Azure account.</span></span>

2. <span data-ttu-id="605fb-145">Utför stegen i följande avsnitt, baserat på dina krav:</span><span class="sxs-lookup"><span data-stu-id="605fb-145">Complete the steps in one of the following sections, based on your requirements:</span></span>

    - <span data-ttu-id="605fb-146">**Lägg till en privat IP-adress**</span><span class="sxs-lookup"><span data-stu-id="605fb-146">**Add a private IP address**</span></span>
    
        <span data-ttu-id="605fb-147">Om du vill lägga till en privat IP-adress till ett nätverkskort måste du skapa en IP-konfiguration med hjälp av kommandot nedan.</span><span class="sxs-lookup"><span data-stu-id="605fb-147">To add a private IP address to a NIC, you must create an IP configuration using the command below.</span></span> <span data-ttu-id="605fb-148">Den statiska adressen måste vara en adress som inte används för undernätet.</span><span class="sxs-lookup"><span data-stu-id="605fb-148">The static address must be an unused address for the subnet.</span></span>

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic1 \
        --private-ip-address 10.0.0.7 --name IPConfig-4
        ```
        <span data-ttu-id="605fb-149">Skapa så många konfigurationer som du vill använda unika konfigurationsnamn och privata IP-adresser (för konfigurationer med statiska IP-adresser).</span><span class="sxs-lookup"><span data-stu-id="605fb-149">Create as many configurations as you require, using unique configuration names and private IP addresses (for configurations with static IP addresses).</span></span>

    - <span data-ttu-id="605fb-150">**Lägg till en offentlig IP-adress**</span><span class="sxs-lookup"><span data-stu-id="605fb-150">**Add a public IP address**</span></span>
    
        <span data-ttu-id="605fb-151">En offentlig IP-adress har lagts till genom att associera den till en ny IP-konfiguration eller en befintlig IP-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="605fb-151">A public IP address is added by associating it to either a new IP configuration or an existing IP configuration.</span></span> <span data-ttu-id="605fb-152">Slutför stegen i något av avsnitten som följer, som du behöver.</span><span class="sxs-lookup"><span data-stu-id="605fb-152">Complete the steps in one of the sections that follow, as you require.</span></span>

        > [!NOTE]
        > <span data-ttu-id="605fb-153">Offentliga IP-adresser har en låg kostnad.</span><span class="sxs-lookup"><span data-stu-id="605fb-153">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="605fb-154">Mer information om priser för IP-adress i [IP-adress priser](https://azure.microsoft.com/pricing/details/ip-addresses) sidan.</span><span class="sxs-lookup"><span data-stu-id="605fb-154">To learn more about IP address pricing, read the [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="605fb-155">Det finns en gräns för antalet offentliga IP-adresser som kan användas i en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="605fb-155">There is a limit to the number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="605fb-156">Mer information om gränserna finns i artikeln om [Azure-begränsningar](../azure-subscription-service-limits.md#networking-limits).</span><span class="sxs-lookup"><span data-stu-id="605fb-156">To learn more about the limits, read the [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>
        >

        <span data-ttu-id="605fb-157">**Koppla resursen till en ny IP-konfiguration**</span><span class="sxs-lookup"><span data-stu-id="605fb-157">**Associate the resource to a new IP configuration**</span></span>
    
        <span data-ttu-id="605fb-158">När du lägger till en offentlig IP-adress i en ny IP-konfiguration måste du också lägga en privat IP-adress, eftersom alla IP-konfigurationer måste ha en privat IP-adress.</span><span class="sxs-lookup"><span data-stu-id="605fb-158">Whenever you add a public IP address in a new IP configuration, you must also add a private IP address, because all IP configurations must have a private IP address.</span></span> <span data-ttu-id="605fb-159">Du kan lägga till en befintlig offentlig IP-adressresurs eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="605fb-159">You can either add an existing public IP address resource, or create a new one.</span></span> <span data-ttu-id="605fb-160">Ange följande kommando för att skapa en ny:</span><span class="sxs-lookup"><span data-stu-id="605fb-160">To create a new one, enter the following command:</span></span>

        ```azurecli
        azure network public-ip create --resource-group myResourceGroup --location westcentralus --name myPublicIP3 \
        --domain-name-label mypublicdns3
        ```

        <span data-ttu-id="605fb-161">Att skapa en ny IP-konfiguration med en statisk privat IP-adress och den associerade *myPublicIP3* offentlig IP adress resursen måste du ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="605fb-161">To create a new IP configuration with a static private IP address and the associated *myPublicIP3* public IP address resource, enter the following command:</span></span>

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic --name IPConfig-4 \
        --private-ip-address 10.0.0.8 --public-ip-name myPublicIP3
        ```

        <span data-ttu-id="605fb-162">**Koppla resursen till en befintlig IP-konfiguration**</span><span class="sxs-lookup"><span data-stu-id="605fb-162">**Associate the resource to an existing IP configuration**</span></span>

        <span data-ttu-id="605fb-163">En offentlig IP-adressresurs kan bara vara kopplad till en IP-konfiguration som inte redan har en associerad.</span><span class="sxs-lookup"><span data-stu-id="605fb-163">A public IP address resource can only be associated to an IP configuration that doesn't already have one associated.</span></span> <span data-ttu-id="605fb-164">Du kan avgöra om en IP-konfiguration har en tillhörande offentliga IP-adress genom att ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="605fb-164">You can determine whether an IP configuration has an associated public IP address by entering the following command:</span></span>

        ```azurecli
        azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
        ```

        <span data-ttu-id="605fb-165">Leta efter en rad som liknar det som följer för IPConfig 3 i den returnerade utdatan:</span><span class="sxs-lookup"><span data-stu-id="605fb-165">Look for a line similar to the one that follows for IPConfig-3 in the returned output:</span></span>

        ```         
        Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
        IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
        IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet
        ```
          
        <span data-ttu-id="605fb-166">Eftersom den **offentliga IP-Adressen** för *IpConfig-3* är tomt, inga offentliga IP-adressresurs är för närvarande associerad till den.</span><span class="sxs-lookup"><span data-stu-id="605fb-166">Since the **Public IP** column for *IpConfig-3* is blank, no public IP address resource is currently associated to it.</span></span> <span data-ttu-id="605fb-167">Du kan lägga till en befintlig offentlig IP-adressresurs IpConfig-3 eller ange följande kommando för att skapa en:</span><span class="sxs-lookup"><span data-stu-id="605fb-167">You can add an existing public IP address resource to IpConfig-3, or enter the following command to create one:</span></span>

        ```azurecli
        azure network public-ip create --resource-group  myResourceGroup --location westcentralus \
        --name myPublicIP3 --domain-name-label mypublicdns3 --allocation-method Static
        ```

        <span data-ttu-id="605fb-168">Ange följande kommando för att associera den offentliga IP-adressresursen till befintliga IP-konfigurationen med namnet *IPConfig-3*:</span><span class="sxs-lookup"><span data-stu-id="605fb-168">Enter the following command to associate the public IP address resource to the existing IP configuration named *IPConfig-3*:</span></span>
        ```azurecli
        azure network nic ip-config set --resource-group myResourceGroup --nic-name myNic1 --name IPConfig-3 \
        --public-ip-name myPublicIP3
        ```

3. <span data-ttu-id="605fb-169">Visa de privata IP-adresserna och de offentliga IP-adressresurser som tilldelats till nätverkskortet genom att ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="605fb-169">View the private IP addresses and the public IP address resources assigned to the NIC by entering the following command:</span></span>

    ```azurecli
    azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
    ```

      <span data-ttu-id="605fb-170">Returnerade utdata liknar följande:</span><span class="sxs-lookup"><span data-stu-id="605fb-170">The returned output is similar to the following:</span></span>
      ```
      Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        
      default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
      IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
      IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet  myPublicIP3
      ```
4. <span data-ttu-id="605fb-171">Lägg till de privata IP-adresser som du har lagt till nätverkskortet på VM-operativsystemet genom att följa anvisningarna i den [lägga till IP-adresser till ett VM-operativsystem](#os-config) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="605fb-171">Add the private IP addresses you added to the NIC to the VM operating system by following the instructions in the [Add IP addresses to a VM operating system](#os-config) section of this article.</span></span> <span data-ttu-id="605fb-172">Lägg inte till de offentliga IP-adresserna till operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="605fb-172">Do not add the public IP addresses to the operating system.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
