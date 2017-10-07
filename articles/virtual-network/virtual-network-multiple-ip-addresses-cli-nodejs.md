---
title: "aaaVM med flera IP-adresser med hjälp av hello Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur tooassign flera IP-adresser tooa virtuella datorn använder hello Azure CLI 1.0 | Resource Manager."
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
ms.openlocfilehash: 83ad48e67309fb21d5aca967d4f2c01afdc0b5cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-azure-cli-10"></a><span data-ttu-id="339de-103">Tilldela flera IP-adresser toovirtual datorer med hjälp av Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="339de-103">Assign multiple IP addresses toovirtual machines using Azure CLI 1.0</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

<span data-ttu-id="339de-104">Den här artikeln förklarar hur toocreate en virtuell dator (VM) via hello Azure Resource Manager distribution modellen med hello Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="339de-104">This article explains how toocreate a virtual machine (VM) through hello Azure Resource Manager deployment model using hello Azure CLI 1.0.</span></span> <span data-ttu-id="339de-105">Flera IP-adresser kan inte tilldelas tooresources som skapats via hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="339de-105">Multiple IP addresses cannot be assigned tooresources created through hello classic deployment model.</span></span> <span data-ttu-id="339de-106">Mer om Azure distributionsmodeller läsa hello toolearn [förstår distributionsmodellerna](../resource-manager-deployment-model.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="339de-106">toolearn more about Azure deployment models, read hello [Understand deployment models](../resource-manager-deployment-model.md) article.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <span data-ttu-id="339de-107"><a name = "create"></a>Skapa en virtuell dator med flera IP-adresser</span><span class="sxs-lookup"><span data-stu-id="339de-107"><a name = "create"></a>Create a VM with multiple IP addresses</span></span>

<span data-ttu-id="339de-108">Du kan göra detta med hjälp av hello Azure CLI 1.0 (den här artikeln) eller hello [Azure CLI 2.0](virtual-network-multiple-ip-addresses-cli.md).</span><span class="sxs-lookup"><span data-stu-id="339de-108">You can complete this task using hello Azure CLI 1.0 (this article) or hello [Azure CLI 2.0](virtual-network-multiple-ip-addresses-cli.md).</span></span> <span data-ttu-id="339de-109">hello steg som följer beskrivs hur toocreate exempel VM med flera IP-adresser, enligt beskrivningen i hello scenario.</span><span class="sxs-lookup"><span data-stu-id="339de-109">hello steps that follow explain how toocreate an example VM with multiple IP addresses, as described in hello scenario.</span></span> <span data-ttu-id="339de-110">Ändra variabeln namn och IP-adresstyper som krävs för din implementering.</span><span class="sxs-lookup"><span data-stu-id="339de-110">Change variable names and IP address types as required for your implementation.</span></span>

1. <span data-ttu-id="339de-111">Installera och konfigurera hello Azure CLI 1.0 av följande hello steg i hello [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel och logga in på ditt Azure-konto med hello `azure-login` kommando.</span><span class="sxs-lookup"><span data-stu-id="339de-111">Install and configure hello Azure CLI 1.0 by following hello steps in hello [Install and Configure hello Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) article and log into your Azure account with hello `azure-login` command.</span></span>

2. <span data-ttu-id="339de-112">Skapa en resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="339de-112">Create a resource group:</span></span>
    
    ```azurecli
    RgName=myResourceGroup
    Location=westcentralus
    azure group create --name $RgName --location $Location
    ```
3. <span data-ttu-id="339de-113">Skapa ett virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="339de-113">Create a virtual network:</span></span>

    ```azurecli
    azure network vnet create --resource-group $RgName --location $Location --name myVNet \
    --address-prefixes 10.0.0.0/16
    ```
4. <span data-ttu-id="339de-114">Skapa ett undernät i hello virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="339de-114">Create a subnet in hello virtual network:</span></span>

    ```azurecli
    azure network vnet subnet create --name mySubnet --resource-group $RgName --vnet-name myVNet \
    --address-prefix 10.0.0.0/24
    ```
    
5. <span data-ttu-id="339de-115">Skapa ett lagringskonto för hello VM.</span><span class="sxs-lookup"><span data-stu-id="339de-115">Create  a storage account for hello VM.</span></span> <span data-ttu-id="339de-116">Innan du kör hello följande kommando, Ersätt *mittlagringskonto* med ett unikt namn.</span><span class="sxs-lookup"><span data-stu-id="339de-116">Before running hello following command, replace *mystorageaccount* with a unique name.</span></span> <span data-ttu-id="339de-117">hello namn måste vara unikt i Azure:</span><span class="sxs-lookup"><span data-stu-id="339de-117">hello name must be unique across Azure:</span></span>

    ```azurecli
    az storage account create --resource-group $RgName --location $Location --name mystorageaccount \
    --kind Storage --sku Standard_LRS
    ```

6. <span data-ttu-id="339de-118">Skapa hello IP-konfigurationer, ett nätverkskort och tilldela hello IP-konfigurationer toohello NIC.</span><span class="sxs-lookup"><span data-stu-id="339de-118">Create hello IP configurations, a NIC, and assign hello IP configurations toohello NIC.</span></span> <span data-ttu-id="339de-119">Du kan lägga till, ta bort eller ändra hello konfigurationer efter behov.</span><span class="sxs-lookup"><span data-stu-id="339de-119">You can add, remove, or change hello configurations as necessary.</span></span> <span data-ttu-id="339de-120">hello följande konfigurationer som beskrivs i hello scenariot:</span><span class="sxs-lookup"><span data-stu-id="339de-120">hello following configurations are described in hello scenario:</span></span>

    <span data-ttu-id="339de-121">**IPConfig-1**</span><span class="sxs-lookup"><span data-stu-id="339de-121">**IPConfig-1**</span></span>

    <span data-ttu-id="339de-122">Ange hello-kommandon som följer toocreate:</span><span class="sxs-lookup"><span data-stu-id="339de-122">Enter hello commands that follow toocreate:</span></span>

    - <span data-ttu-id="339de-123">En offentlig IP-adressresurs med en statisk offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="339de-123">A public IP address resource with a static public IP address</span></span>
    - <span data-ttu-id="339de-124">Ett nätverkskort, tilldela hello offentlig IP-adress och en statisk privat IP-adress tooit.</span><span class="sxs-lookup"><span data-stu-id="339de-124">A NIC, assigning hello public IP address and a static private IP address tooit.</span></span>
    
    <span data-ttu-id="339de-125">Ersätt *mypublicdns* med ett namn som är unikt inom hello Azure-plats.</span><span class="sxs-lookup"><span data-stu-id="339de-125">Replace *mypublicdns* with a name that is unique within hello Azure location.</span></span>

      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP1 \
      --domain-name-label mypublicdns --allocation-method Static
        
      azure network nic create --resource-group $RgName --location $Location --name myNic1 \
      --private-ip-address 10.0.0.4 --subnet-name mySubnet --subnet-vnet-name myVNet \
      --subnet-name mySubnet --public-ip-name myPublicIP1
      ```

      > [!NOTE]
      > <span data-ttu-id="339de-126">Offentliga IP-adresser har en låg kostnad.</span><span class="sxs-lookup"><span data-stu-id="339de-126">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="339de-127">Mer om IP-priser, toolearn läsa hello [IP-adress priser](https://azure.microsoft.com/pricing/details/ip-addresses) sidan.</span><span class="sxs-lookup"><span data-stu-id="339de-127">toolearn more about IP address pricing, read hello [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="339de-128">Det finns en gräns toohello antalet offentliga IP-adresser som kan användas i en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="339de-128">There is a limit toohello number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="339de-129">Mer om hello gränser, läsa hello toolearn [Azure begränsar](../azure-subscription-service-limits.md#networking-limits) artikel.</span><span class="sxs-lookup"><span data-stu-id="339de-129">toolearn more about hello limits, read hello [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>

    <span data-ttu-id="339de-130">**IPConfig-2**</span><span class="sxs-lookup"><span data-stu-id="339de-130">**IPConfig-2**</span></span>

     <span data-ttu-id="339de-131">Ange följande kommandon toocreate hello en ny offentlig IP-adressresurs och en ny IP-konfiguration med en statisk offentlig IP-adress och en statisk privat IP-adress:</span><span class="sxs-lookup"><span data-stu-id="339de-131">Enter hello following commands toocreate a new public IP address resource and a new IP configuration with a static public IP address and a static private IP address:</span></span>
    
      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP2
      --domain-name-label mypublicdns2 --allocation-method Static

      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --name IPConfig-2
      --private-ip-address 10.0.0.5 --public-ip-name myPublicIP2
      ```

    <span data-ttu-id="339de-132">**IPConfig-3**</span><span class="sxs-lookup"><span data-stu-id="339de-132">**IPConfig-3**</span></span>

    <span data-ttu-id="339de-133">Ange hello följande kommandon toocreate en IP-konfiguration med en statisk privat IP-adress och ingen offentlig IP-adress:</span><span class="sxs-lookup"><span data-stu-id="339de-133">Enter hello following commands toocreate an IP configuration with a static private IP address and no public IP address:</span></span>

      ```azurecli
      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --private-ip-address 10.0.0.6 \
      --name IPConfig-3
      ```

    >[!NOTE] 
    ><span data-ttu-id="339de-134">Även om den här artikeln tilldelar alla IP-konfigurationer tooa nätverkskort, kan du också tilldela flera IP-konfigurationer tooany NIC på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="339de-134">Though this article assigns all IP configurations tooa single NIC, you can also assign multiple IP configurations tooany NIC in a VM.</span></span> <span data-ttu-id="339de-135">toolearn hur toocreate en virtuell dator med flera nätverkskort, läsa hello skapa en virtuell dator med flera nätverkskort artikel.</span><span class="sxs-lookup"><span data-stu-id="339de-135">toolearn how toocreate a VM with multiple NICs, read hello Create a VM with multiple NICs article.</span></span>

7. <span data-ttu-id="339de-136">Skapa en virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="339de-136">Create a Linux VM</span></span> 

    ```azurecli
    az vm create --resource-group $RgName --name myVM1 --location $Location --nics myNic1 \
    --image UbuntuLTS --ssh-key-value ~/.ssh/id_rsa.pub --admin-username azureuser
    ```

    <span data-ttu-id="339de-137">toochange hello VM storlek tooStandard DS2 v2, till exempel lägger du till följande egenskapen hello `--vm-size Standard_DS3_v2` toohello `azure vm create` kommandot i steg 6.</span><span class="sxs-lookup"><span data-stu-id="339de-137">toochange hello VM size tooStandard DS2 v2, for example, simply add hello following property `--vm-size Standard_DS3_v2` toohello `azure vm create` command in step 6.</span></span>

8. <span data-ttu-id="339de-138">Ange följande kommando tooview hello NIC hello och hello associerade IP-konfigurationer:</span><span class="sxs-lookup"><span data-stu-id="339de-138">Enter hello following command tooview hello NIC and hello associated IP configurations:</span></span>

    ```azurecli
    azure network nic show --resource-group $RgName --name myNic1
    ```
9. <span data-ttu-id="339de-139">Lägg till hello privata IP-adresser toohello VM operativsystem genom att följa steg hello för operativsystemet i hello [lägga till IP-adresser tooa VM operativsystemet](#os-config) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="339de-139">Add hello private IP addresses toohello VM operating system by completing hello steps for your operating system in hello [Add IP addresses tooa VM operating system](#os-config) section of this article.</span></span>

## <span data-ttu-id="339de-140"><a name="add"></a>Lägg till IP-adresser tooa VM</span><span class="sxs-lookup"><span data-stu-id="339de-140"><a name="add"></a>Add IP addresses tooa VM</span></span>

<span data-ttu-id="339de-141">Du kan lägga till ytterligare privata och offentliga IP-adresser tooan befintliga nätverkskort genom att slutföra hello steg som följer.</span><span class="sxs-lookup"><span data-stu-id="339de-141">You can add additional private and public IP addresses tooan existing NIC by completing hello steps that follow.</span></span> <span data-ttu-id="339de-142">hello exempel bygger på hello [scenariot](#Scenario) beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="339de-142">hello examples build upon hello [scenario](#Scenario) described in this article.</span></span>

1. <span data-ttu-id="339de-143">Öppna Azure CLI och fullständig hello återstående stegen i det här avsnittet i en enda CLI-session.</span><span class="sxs-lookup"><span data-stu-id="339de-143">Open Azure CLI and complete hello remaining steps in this section within a single CLI session.</span></span> <span data-ttu-id="339de-144">Om du inte redan har installerat och konfigurerat Azure-CLI, fullständig hello stegen i hello [installera och konfigurera hello Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel och logga in på ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="339de-144">If you don't already have Azure CLI installed and configured, complete hello steps in hello [Install and Configure hello Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) article and log into your Azure account.</span></span>

2. <span data-ttu-id="339de-145">Slutför hello stegen i ett av följande avsnitt, baserat på dina krav hello:</span><span class="sxs-lookup"><span data-stu-id="339de-145">Complete hello steps in one of hello following sections, based on your requirements:</span></span>

    - <span data-ttu-id="339de-146">**Lägg till en privat IP-adress**</span><span class="sxs-lookup"><span data-stu-id="339de-146">**Add a private IP address**</span></span>
    
        <span data-ttu-id="339de-147">tooadd en privat IP-adress tooa NIC, måste du skapa en IP-konfiguration med hjälp av hello kommandot nedan.</span><span class="sxs-lookup"><span data-stu-id="339de-147">tooadd a private IP address tooa NIC, you must create an IP configuration using hello command below.</span></span> <span data-ttu-id="339de-148">hello statisk adress måste vara en oanvända adress för hello undernät.</span><span class="sxs-lookup"><span data-stu-id="339de-148">hello static address must be an unused address for hello subnet.</span></span>

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic1 \
        --private-ip-address 10.0.0.7 --name IPConfig-4
        ```
        <span data-ttu-id="339de-149">Skapa så många konfigurationer som du vill använda unika konfigurationsnamn och privata IP-adresser (för konfigurationer med statiska IP-adresser).</span><span class="sxs-lookup"><span data-stu-id="339de-149">Create as many configurations as you require, using unique configuration names and private IP addresses (for configurations with static IP addresses).</span></span>

    - <span data-ttu-id="339de-150">**Lägg till en offentlig IP-adress**</span><span class="sxs-lookup"><span data-stu-id="339de-150">**Add a public IP address**</span></span>
    
        <span data-ttu-id="339de-151">En offentlig IP-adress har lagts till genom att associera den tooeither en ny IP-konfiguration eller en befintlig IP-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="339de-151">A public IP address is added by associating it tooeither a new IP configuration or an existing IP configuration.</span></span> <span data-ttu-id="339de-152">Slutför hello stegen i ett av hello avsnitten som följer, som du behöver.</span><span class="sxs-lookup"><span data-stu-id="339de-152">Complete hello steps in one of hello sections that follow, as you require.</span></span>

        > [!NOTE]
        > <span data-ttu-id="339de-153">Offentliga IP-adresser har en låg kostnad.</span><span class="sxs-lookup"><span data-stu-id="339de-153">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="339de-154">Mer om IP-priser, toolearn läsa hello [IP-adress priser](https://azure.microsoft.com/pricing/details/ip-addresses) sidan.</span><span class="sxs-lookup"><span data-stu-id="339de-154">toolearn more about IP address pricing, read hello [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="339de-155">Det finns en gräns toohello antalet offentliga IP-adresser som kan användas i en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="339de-155">There is a limit toohello number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="339de-156">Mer om hello gränser, läsa hello toolearn [Azure begränsar](../azure-subscription-service-limits.md#networking-limits) artikel.</span><span class="sxs-lookup"><span data-stu-id="339de-156">toolearn more about hello limits, read hello [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>
        >

        <span data-ttu-id="339de-157">**Associera hello resurs tooa nya IP-konfiguration**</span><span class="sxs-lookup"><span data-stu-id="339de-157">**Associate hello resource tooa new IP configuration**</span></span>
    
        <span data-ttu-id="339de-158">När du lägger till en offentlig IP-adress i en ny IP-konfiguration måste du också lägga en privat IP-adress, eftersom alla IP-konfigurationer måste ha en privat IP-adress.</span><span class="sxs-lookup"><span data-stu-id="339de-158">Whenever you add a public IP address in a new IP configuration, you must also add a private IP address, because all IP configurations must have a private IP address.</span></span> <span data-ttu-id="339de-159">Du kan lägga till en befintlig offentlig IP-adressresurs eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="339de-159">You can either add an existing public IP address resource, or create a new one.</span></span> <span data-ttu-id="339de-160">toocreate en ny ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="339de-160">toocreate a new one, enter hello following command:</span></span>

        ```azurecli
        azure network public-ip create --resource-group myResourceGroup --location westcentralus --name myPublicIP3 \
        --domain-name-label mypublicdns3
        ```

        <span data-ttu-id="339de-161">toocreate en ny IP-konfiguration med en statisk privat IP-adress och hello associerade *myPublicIP3* offentlig IP adress resursen måste du ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="339de-161">toocreate a new IP configuration with a static private IP address and hello associated *myPublicIP3* public IP address resource, enter hello following command:</span></span>

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic --name IPConfig-4 \
        --private-ip-address 10.0.0.8 --public-ip-name myPublicIP3
        ```

        <span data-ttu-id="339de-162">**Associera hello resurs tooan befintliga IP-konfiguration**</span><span class="sxs-lookup"><span data-stu-id="339de-162">**Associate hello resource tooan existing IP configuration**</span></span>

        <span data-ttu-id="339de-163">En offentlig IP-adressresurs kan bara vara associerad tooan IP-konfiguration som inte redan har en associerad.</span><span class="sxs-lookup"><span data-stu-id="339de-163">A public IP address resource can only be associated tooan IP configuration that doesn't already have one associated.</span></span> <span data-ttu-id="339de-164">Du kan avgöra om en IP-konfiguration har en tillhörande offentliga IP-adress genom att ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="339de-164">You can determine whether an IP configuration has an associated public IP address by entering hello following command:</span></span>

        ```azurecli
        azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
        ```

        <span data-ttu-id="339de-165">Leta efter en rad liknande toohello som följer för IPConfig 3 i hello returnerade utdata:</span><span class="sxs-lookup"><span data-stu-id="339de-165">Look for a line similar toohello one that follows for IPConfig-3 in hello returned output:</span></span>

        ```         
        Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
        IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
        IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet
        ```
          
        <span data-ttu-id="339de-166">Eftersom hello **offentliga IP-Adressen** för *IpConfig-3* är tomt, inga offentliga IP-adressresurs är för närvarande associerad tooit.</span><span class="sxs-lookup"><span data-stu-id="339de-166">Since hello **Public IP** column for *IpConfig-3* is blank, no public IP address resource is currently associated tooit.</span></span> <span data-ttu-id="339de-167">Du kan lägga till en befintlig offentlig IP-adress resurs tooIpConfig-3 eller ange följande kommando toocreate en hello:</span><span class="sxs-lookup"><span data-stu-id="339de-167">You can add an existing public IP address resource tooIpConfig-3, or enter hello following command toocreate one:</span></span>

        ```azurecli
        azure network public-ip create --resource-group  myResourceGroup --location westcentralus \
        --name myPublicIP3 --domain-name-label mypublicdns3 --allocation-method Static
        ```

        <span data-ttu-id="339de-168">Ange följande kommando tooassociate hello offentliga IP-adressen resurs toohello befintliga IP-konfigurationen med namnet hello *IPConfig-3*:</span><span class="sxs-lookup"><span data-stu-id="339de-168">Enter hello following command tooassociate hello public IP address resource toohello existing IP configuration named *IPConfig-3*:</span></span>
        ```azurecli
        azure network nic ip-config set --resource-group myResourceGroup --nic-name myNic1 --name IPConfig-3 \
        --public-ip-name myPublicIP3
        ```

3. <span data-ttu-id="339de-169">Visa hello privata IP-adresser och hello offentliga IP-adress resurser tilldelade toohello NIC genom att ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="339de-169">View hello private IP addresses and hello public IP address resources assigned toohello NIC by entering hello following command:</span></span>

    ```azurecli
    azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
    ```

      <span data-ttu-id="339de-170">hello returnerade utdata är liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="339de-170">hello returned output is similar toohello following:</span></span>
      ```
      Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        
      default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
      IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
      IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet  myPublicIP3
      ```
4. <span data-ttu-id="339de-171">Lägg till hello privata IP-adresser som du har lagt till toohello NIC toohello VM operativsystem genom att följa instruktionerna hello i hello [lägga till IP-adresser tooa VM operativsystemet](#os-config) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="339de-171">Add hello private IP addresses you added toohello NIC toohello VM operating system by following hello instructions in hello [Add IP addresses tooa VM operating system](#os-config) section of this article.</span></span> <span data-ttu-id="339de-172">Lägg inte till hello offentliga IP-adresser toohello operativsystem.</span><span class="sxs-lookup"><span data-stu-id="339de-172">Do not add hello public IP addresses toohello operating system.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
