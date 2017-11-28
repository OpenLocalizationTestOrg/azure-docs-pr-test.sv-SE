---
title: "aaaMultiple IP-adresser för virtuella datorer i Azure - PowerShell | Microsoft Docs"
description: "Lär dig hur tooassign flera IP-adresser tooa virtuell dator med hjälp av PowerShell | Resource Manager."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c44ea62f-7e54-4e3b-81ef-0b132111f1f8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/24/2017
ms.author: jdial;annahar
ms.openlocfilehash: df54c4386ce13521e660a3e7208c8c1ab1459bc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-powershell"></a><span data-ttu-id="dcafa-103">Tilldela flera IP-adresser toovirtual datorer med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="dcafa-103">Assign multiple IP addresses toovirtual machines using PowerShell</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

<span data-ttu-id="dcafa-104">Den här artikeln förklarar hur toocreate en virtuell dator (VM) via hello Azure Resource Manager distribution modellen med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dcafa-104">This article explains how toocreate a virtual machine (VM) through hello Azure Resource Manager deployment model using PowerShell.</span></span> <span data-ttu-id="dcafa-105">Flera IP-adresser kan inte tilldelas tooresources som skapats via hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="dcafa-105">Multiple IP addresses cannot be assigned tooresources created through hello classic deployment model.</span></span> <span data-ttu-id="dcafa-106">Mer om Azure distributionsmodeller läsa hello toolearn [förstår distributionsmodellerna](../resource-manager-deployment-model.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="dcafa-106">toolearn more about Azure deployment models, read hello [Understand deployment models](../resource-manager-deployment-model.md) article.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <span data-ttu-id="dcafa-107"><a name = "create"></a>Skapa en virtuell dator med flera IP-adresser</span><span class="sxs-lookup"><span data-stu-id="dcafa-107"><a name = "create"></a>Create a VM with multiple IP addresses</span></span>

<span data-ttu-id="dcafa-108">hello steg som följer beskrivs hur toocreate exempel VM med flera IP-adresser, enligt beskrivningen i hello scenario.</span><span class="sxs-lookup"><span data-stu-id="dcafa-108">hello steps that follow explain how toocreate an example VM with multiple IP addresses, as described in hello scenario.</span></span> <span data-ttu-id="dcafa-109">Ändra variabelvärden som krävs för din implementering.</span><span class="sxs-lookup"><span data-stu-id="dcafa-109">Change variable values as required for your implementation.</span></span>

1. <span data-ttu-id="dcafa-110">Öppna ett PowerShell-Kommandotolken och fullständig hello återstående stegen i det här avsnittet i en enda PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="dcafa-110">Open a PowerShell command prompt and complete hello remaining steps in this section within a single PowerShell session.</span></span> <span data-ttu-id="dcafa-111">Om du inte redan har PowerShell installeras och konfigureras, fullständig hello stegen i hello [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) artikel.</span><span class="sxs-lookup"><span data-stu-id="dcafa-111">If you don't already have PowerShell installed and configured, complete hello steps in hello [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="dcafa-112">Inloggningskonto tooyour med hello `login-azurermaccount` kommando.</span><span class="sxs-lookup"><span data-stu-id="dcafa-112">Login tooyour account with hello `login-azurermaccount` command.</span></span>
3. <span data-ttu-id="dcafa-113">Ersätt *myResourceGroup* och *westus* med ett namn och plats.</span><span class="sxs-lookup"><span data-stu-id="dcafa-113">Replace *myResourceGroup* and *westus* with a name and location of your choosing.</span></span> <span data-ttu-id="dcafa-114">Skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="dcafa-114">Create a resource group.</span></span> <span data-ttu-id="dcafa-115">En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="dcafa-115">A resource group is a logical container into which Azure resources are deployed and managed.</span></span>

    ```powershell
    $RgName   = "MyResourceGroup"
    $Location = "westus"

    New-AzureRmResourceGroup `
    -Name $RgName `
    -Location $Location
    ```

4. <span data-ttu-id="dcafa-116">Skapa ett virtuellt nätverk (VNet) och undernät i hello samma plats som hello resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="dcafa-116">Create a virtual network (VNet) and subnet in hello same location as hello resource group:</span></span>

    ```powershell
    
    # Create a subnet configuration
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name MySubnet `
    -AddressPrefix 10.0.0.0/24

    # Create a virtual network
    $VNet = New-AzureRmVirtualNetwork `
    -ResourceGroupName $RgName `
    -Location $Location `
    -Name MyVNet `
    -AddressPrefix 10.0.0.0/16 `
    -Subnet $subnetConfig

    # Get hello subnet object
    $Subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $SubnetConfig.Name -VirtualNetwork $VNet
    ```

5. <span data-ttu-id="dcafa-117">Skapa en nätverkssäkerhetsgrupp (NSG) och en regel.</span><span class="sxs-lookup"><span data-stu-id="dcafa-117">Create a network security group (NSG) and a rule.</span></span> <span data-ttu-id="dcafa-118">hello NSG skyddar hello VM använda regler för inkommande och utgående.</span><span class="sxs-lookup"><span data-stu-id="dcafa-118">hello NSG secures hello VM using inbound and outbound rules.</span></span> <span data-ttu-id="dcafa-119">I detta fall skapas en regel för inkommande trafik för port 3389, som tillåter inkommande anslutningar till fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="dcafa-119">In this case, an inbound rule is created for port 3389, which allows incoming remote desktop connections.</span></span>

    ```powershell
    
    # Create an inbound network security group rule for port 3389

    $NSGRule = New-AzureRmNetworkSecurityRuleConfig `
    -Name MyNsgRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 -Access Allow
    
    # Create a network security group
    $NSG = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName $RgName `
    -Location $Location `
    -Name MyNetworkSecurityGroup `
    -SecurityRules $NSGRule
    ```

6. <span data-ttu-id="dcafa-120">Definiera hello primära IP-konfiguration för hello NIC.</span><span class="sxs-lookup"><span data-stu-id="dcafa-120">Define hello primary IP configuration for hello NIC.</span></span> <span data-ttu-id="dcafa-121">Ändra 10.0.0.4 tooa giltig adress i hello undernät som du har skapat, om du inte använde hello-värdet som definierats tidigare.</span><span class="sxs-lookup"><span data-stu-id="dcafa-121">Change 10.0.0.4 tooa valid address in hello subnet you created, if you didn't use hello value defined previously.</span></span> <span data-ttu-id="dcafa-122">Innan du tilldelar en statisk IP-adress, rekommenderas det att du först bekräfta att den inte är redan används.</span><span class="sxs-lookup"><span data-stu-id="dcafa-122">Before assigning a static IP address, it's recommended that you first confirm it's not already in use.</span></span> <span data-ttu-id="dcafa-123">Ange hello kommando `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.4 -VirtualNetwork $VNet`.</span><span class="sxs-lookup"><span data-stu-id="dcafa-123">Enter hello command `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.4 -VirtualNetwork $VNet`.</span></span> <span data-ttu-id="dcafa-124">Om hello-adress är tillgänglig hello utdata returnerar *SANT*.</span><span class="sxs-lookup"><span data-stu-id="dcafa-124">If hello address is available, hello output returns *True*.</span></span> <span data-ttu-id="dcafa-125">Om den inte är tillgänglig hello utdata returnerar *FALSKT* och en lista med adresser som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="dcafa-125">If it's not available, hello output returns *False* and a list of addresses that are available.</span></span> 

    <span data-ttu-id="dcafa-126">I följande kommandon, hello **Ersätt < Ersätt-med-your-unikt-name > med hello unikt DNS-namnet toouse.**</span><span class="sxs-lookup"><span data-stu-id="dcafa-126">In hello following commands, **Replace <replace-with-your-unique-name> with hello unique DNS name toouse.**</span></span> <span data-ttu-id="dcafa-127">hello-namnet måste vara unikt inom alla offentliga IP-adresser inom en Azure-region.</span><span class="sxs-lookup"><span data-stu-id="dcafa-127">hello name must be unique across all public IP addresses within an Azure region.</span></span> <span data-ttu-id="dcafa-128">Det här är en valfri parameter.</span><span class="sxs-lookup"><span data-stu-id="dcafa-128">This is an optional parameter.</span></span> <span data-ttu-id="dcafa-129">Det kan tas bort om du bara vill tooconnect toohello VM med hello offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="dcafa-129">It can be removed if you only want tooconnect toohello VM using hello public IP address.</span></span>

    ```powershell
    
    # Create a public IP address
    $PublicIP1 = New-AzureRmPublicIpAddress `
    -Name "MyPublicIP1" `
    -ResourceGroupName $RgName `
    -Location $Location `
    -DomainNameLabel <replace-with-your-unique-name> `
    -AllocationMethod Static
        
    #Create an IP configuration with a static private IP address and assign hello public IP ddress tooit
    $IpConfigName1 = "IPConfig-1"
    $IpConfig1     = New-AzureRmNetworkInterfaceIpConfig `
    -Name $IpConfigName1 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.4 `
    -PublicIpAddress $PublicIP1 `
    -Primary
    ```

    <span data-ttu-id="dcafa-130">När du tilldelar flera IP-konfigurationer tooa NIC det måste tilldelas en konfiguration som hello *-primära*.</span><span class="sxs-lookup"><span data-stu-id="dcafa-130">When you assign multiple IP configurations tooa NIC, one configuration must be assigned as hello *-Primary*.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dcafa-131">Offentliga IP-adresser har en låg kostnad.</span><span class="sxs-lookup"><span data-stu-id="dcafa-131">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="dcafa-132">Mer om IP-priser, toolearn läsa hello [IP-adress priser](https://azure.microsoft.com/pricing/details/ip-addresses) sidan.</span><span class="sxs-lookup"><span data-stu-id="dcafa-132">toolearn more about IP address pricing, read hello [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="dcafa-133">Det finns en gräns toohello antalet offentliga IP-adresser som kan användas i en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="dcafa-133">There is a limit toohello number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="dcafa-134">Mer om hello gränser, läsa hello toolearn [Azure begränsar](../azure-subscription-service-limits.md#networking-limits) artikel.</span><span class="sxs-lookup"><span data-stu-id="dcafa-134">toolearn more about hello limits, read hello [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>

7. <span data-ttu-id="dcafa-135">Definiera hello sekundär IP-konfigurationer för hello NIC.</span><span class="sxs-lookup"><span data-stu-id="dcafa-135">Define hello secondary IP configurations for hello NIC.</span></span> <span data-ttu-id="dcafa-136">Du kan lägga till eller ta bort konfigurationer efter behov.</span><span class="sxs-lookup"><span data-stu-id="dcafa-136">You can add or remove configurations as necessary.</span></span> <span data-ttu-id="dcafa-137">Varje IP-adresskonfigurationen måste ha en privat IP-adress.</span><span class="sxs-lookup"><span data-stu-id="dcafa-137">Each IP configuration must have a private IP address assigned.</span></span> <span data-ttu-id="dcafa-138">Varje konfiguration kan du ha en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="dcafa-138">Each configuration can optionally have one public IP address assigned.</span></span>

    ```powershell
    
    # Create a public IP address
    $PublicIP2 = New-AzureRmPublicIpAddress `
    -Name "MyPublicIP2" `
    -ResourceGroupName $RgName `
    -Location $Location `
    -AllocationMethod Static
        
    #Create an IP configuration with a static private IP address and assign hello public IP ddress tooit
    $IpConfigName2 = "IPConfig-2"
    $IpConfig2     = New-AzureRmNetworkInterfaceIpConfig `
    -Name $IpConfigName2 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.5 `
    -PublicIpAddress $PublicIP2
        
    $IpConfigName3 = "IpConfig-3"
    $IpConfig3 = New-AzureRmNetworkInterfaceIpConfig `
    -Name $IPConfigName3 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.6
    ```

8. <span data-ttu-id="dcafa-139">Skapa hello NIC och associera hello tre IP-konfigurationer tooit:</span><span class="sxs-lookup"><span data-stu-id="dcafa-139">Create hello NIC and associate hello three IP configurations tooit:</span></span>

    ```powershell
    
    $NIC = New-AzureRmNetworkInterface `
    -Name MyNIC `
    -ResourceGroupName $RgName `
    -Location $Location `
    -NetworkSecurityGroupId $NSG.Id `
    -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3
    ```

    >[!NOTE]
    ><span data-ttu-id="dcafa-140">Även om alla konfigurationer tilldelas tooone nätverkskort i den här artikeln kan tilldela du flera IP-konfigurationer tooevery nätverkskortet som sitter toohello VM.</span><span class="sxs-lookup"><span data-stu-id="dcafa-140">Though all configurations are assigned tooone NIC in this article, you can assign multiple IP configurations tooevery NIC attached toohello VM.</span></span> <span data-ttu-id="dcafa-141">toolearn hur toocreate en virtuell dator med flera nätverkskort, läsa hello [skapa en virtuell dator med flera nätverkskort](virtual-network-deploy-multinic-arm-ps.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="dcafa-141">toolearn how toocreate a VM with multiple NICs, read hello [Create a VM with multiple NICs](virtual-network-deploy-multinic-arm-ps.md) article.</span></span>

9. <span data-ttu-id="dcafa-142">Skapa hello VM genom att ange hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="dcafa-142">Create hello VM by entering hello following commands:</span></span>

    ```powershell
    
    # Define a credential object. When you run these commands, you're prompted tooenter a sername and password for hello VM you're reating.
    $cred = Get-Credential
    
    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
    -VMName MyVM `
    -VMSize Standard_DS1_v2 | `
    Set-AzureRmVMOperatingSystem -Windows `
    -ComputerName MyVM `
    -Credential $cred | `
    Set-AzureRmVMSourceImage `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest | `
    Add-AzureRmVMNetworkInterface `
    -Id $NIC.Id
    
    # Create hello VM
    New-AzureRmVM `
    -ResourceGroupName $RgName `
    -Location $Location `
    -VM $VmConfig
    ```

10. <span data-ttu-id="dcafa-143">Lägg till hello privata IP-adresser toohello VM operativsystem genom att följa steg hello för operativsystemet i hello [lägga till IP-adresser tooa VM operativsystemet](#os-config) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="dcafa-143">Add hello private IP addresses toohello VM operating system by completing hello steps for your operating system in hello [Add IP addresses tooa VM operating system](#os-config) section of this article.</span></span> <span data-ttu-id="dcafa-144">Lägg inte till hello offentliga IP-adresser toohello operativsystem.</span><span class="sxs-lookup"><span data-stu-id="dcafa-144">Do not add hello public IP addresses toohello operating system.</span></span>

## <span data-ttu-id="dcafa-145"><a name="add"></a>Lägg till IP-adresser tooa VM</span><span class="sxs-lookup"><span data-stu-id="dcafa-145"><a name="add"></a>Add IP addresses tooa VM</span></span>

<span data-ttu-id="dcafa-146">Du kan lägga till privata och offentliga IP-adresser tooa NIC hello steg som följer.</span><span class="sxs-lookup"><span data-stu-id="dcafa-146">You can add private and public IP addresses tooa NIC by completing hello steps that follow.</span></span> <span data-ttu-id="dcafa-147">hello exemplen i följande avsnitt hello förutsätter att du redan har en virtuell dator med hello tre IP-konfigurationer som beskrivs i hello [scenariot](#Scenario) i den här artikeln, men det är inte nödvändigt att du gör.</span><span class="sxs-lookup"><span data-stu-id="dcafa-147">hello examples in hello following sections assume that you already have a VM with hello three IP configurations described in hello [scenario](#Scenario) in this article, but it's not required that you do.</span></span>

1. <span data-ttu-id="dcafa-148">Öppna ett PowerShell-Kommandotolken och fullständig hello återstående stegen i det här avsnittet i en enda PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="dcafa-148">Open a PowerShell command prompt and complete hello remaining steps in this section within a single PowerShell session.</span></span> <span data-ttu-id="dcafa-149">Om du inte redan har PowerShell installeras och konfigureras, fullständig hello stegen i hello [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) artikel.</span><span class="sxs-lookup"><span data-stu-id="dcafa-149">If you don't already have PowerShell installed and configured, complete hello steps in hello [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="dcafa-150">Ändra hello ”värden” Hej $Variables toohello namnet på hello NIC som du vill tooadd IP-adress tooand hello resurs grupp och plats hello NIC finns i följande:</span><span class="sxs-lookup"><span data-stu-id="dcafa-150">Change hello "values" of hello following $Variables toohello name of hello NIC you want tooadd IP address tooand hello resource group and location hello NIC exists in:</span></span>

    ```powershell
    $NicName  = "MyNIC"
    $RgName   = "MyResourceGroup"
    $Location = "westus"
    ```

    <span data-ttu-id="dcafa-151">Om du inte vet hello namnet på hello NIC som du vill toochange ange hello följande kommandon och sedan ändra hello värdena för variabler som hello tidigare:</span><span class="sxs-lookup"><span data-stu-id="dcafa-151">If you don't know hello name of hello NIC you want toochange, enter hello following commands, then change hello values of hello previous variables:</span></span>

    ```powershell
    Get-AzureRmNetworkInterface | Format-Table Name, ResourceGroupName, Location
    ```
3. <span data-ttu-id="dcafa-152">Skapa en variabel och ange den toohello befintliga nätverkskort genom att skriva följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="dcafa-152">Create a variable and set it toohello existing NIC by typing hello following command:</span></span>

    ```powershell
    $MyNIC = Get-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName
    ```
4. <span data-ttu-id="dcafa-153">Ändra i hello följande kommandon, *MyVNet* och *MySubnet* toohello namnen på hello VNet och undernät hello nätverkskortet är anslutet till.</span><span class="sxs-lookup"><span data-stu-id="dcafa-153">In hello following commands, change *MyVNet* and *MySubnet* toohello names of hello VNet and subnet hello NIC is connected to.</span></span> <span data-ttu-id="dcafa-154">Ange hello kommandon tooretrieve hello VNet och undernät objekt hello nätverkskortet är anslutet till:</span><span class="sxs-lookup"><span data-stu-id="dcafa-154">Enter hello commands tooretrieve hello VNet and subnet objects hello NIC is connected to:</span></span>

    ```powershell
    $MyVNet = Get-AzureRMVirtualnetwork -Name MyVNet -ResourceGroupName $RgName
    $Subnet = $MyVnet.Subnets | Where-Object { $_.Name -eq "MySubnet" }
    ```
    <span data-ttu-id="dcafa-155">Om du inte vet hello VNet eller undernät namn hello nätverkskortet är anslutet till, ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="dcafa-155">If you don't know hello VNet or subnet name hello NIC is connected to, enter hello following command:</span></span>
    ```powershell
    $MyNIC.IpConfigurations
    ```
    <span data-ttu-id="dcafa-156">Hello utdata och leta efter texten liknande toohello följande exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="dcafa-156">In hello output, look for text similar toohello following example output:</span></span>
    
    ```
    "Id": "/subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/MyVNet/subnets/MySubnet"
    ```
    <span data-ttu-id="dcafa-157">I det här resultatet *MyVnet* är hello VNet och *MySubnet* är hello undernät hello nätverkskortet är anslutet till.</span><span class="sxs-lookup"><span data-stu-id="dcafa-157">In this output, *MyVnet* is hello VNet and *MySubnet* is hello subnet hello NIC is connected to.</span></span>

5. <span data-ttu-id="dcafa-158">Slutför hello stegen i ett av följande avsnitt, baserat på dina krav hello:</span><span class="sxs-lookup"><span data-stu-id="dcafa-158">Complete hello steps in one of hello following sections, based on your requirements:</span></span>

    <span data-ttu-id="dcafa-159">**Lägg till en privat IP-adress**</span><span class="sxs-lookup"><span data-stu-id="dcafa-159">**Add a private IP address**</span></span>

    <span data-ttu-id="dcafa-160">tooadd en privat IP-adress tooa NIC, måste du skapa en IP-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="dcafa-160">tooadd a private IP address tooa NIC, you must create an IP configuration.</span></span> <span data-ttu-id="dcafa-161">hello följande kommando skapar en konfiguration med en statisk IP-adress 10.0.0.7.</span><span class="sxs-lookup"><span data-stu-id="dcafa-161">hello following command creates a configuration with a static IP address of 10.0.0.7.</span></span> <span data-ttu-id="dcafa-162">När du anger en statisk IP-adress, måste den vara en oanvända adress för hello undernät.</span><span class="sxs-lookup"><span data-stu-id="dcafa-162">When specifying a static IP address, it must be an unused address for hello subnet.</span></span> <span data-ttu-id="dcafa-163">Vi rekommenderar att du först testar hello adress tooensure är tillgänglig genom att ange hello `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.7 -VirtualNetwork $myVnet` kommando.</span><span class="sxs-lookup"><span data-stu-id="dcafa-163">It's recommended that you first test hello address tooensure it's available by entering hello `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.7 -VirtualNetwork $myVnet` command.</span></span> <span data-ttu-id="dcafa-164">Om hello IP-adress är tillgänglig hello utdata returnerar *SANT*.</span><span class="sxs-lookup"><span data-stu-id="dcafa-164">If hello IP address is available, hello output returns *True*.</span></span> <span data-ttu-id="dcafa-165">Om den inte är tillgänglig hello utdata returnerar *FALSKT*, och en lista med adresser som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="dcafa-165">If it's not available, hello output returns *False*, and a list of addresses that are available.</span></span>

    ```powershell
    Add-AzureRmNetworkInterfaceIpConfig -Name IPConfig-4 -NetworkInterface `
    $MyNIC -Subnet $Subnet -PrivateIpAddress 10.0.0.7
    ```
    <span data-ttu-id="dcafa-166">Skapa så många konfigurationer som du vill använda unika konfigurationsnamn och privata IP-adresser (för konfigurationer med statiska IP-adresser).</span><span class="sxs-lookup"><span data-stu-id="dcafa-166">Create as many configurations as you require, using unique configuration names and private IP addresses (for configurations with static IP addresses).</span></span>

    <span data-ttu-id="dcafa-167">Lägg till hello privata IP-adress toohello VM operativsystem genom att följa steg hello för operativsystemet i hello [lägga till IP-adresser tooa VM operativsystemet](#os-config) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="dcafa-167">Add hello private IP address toohello VM operating system by completing hello steps for your operating system in hello [Add IP addresses tooa VM operating system](#os-config) section of this article.</span></span>

    <span data-ttu-id="dcafa-168">**Lägg till en offentlig IP-adress**</span><span class="sxs-lookup"><span data-stu-id="dcafa-168">**Add a public IP address**</span></span>

    <span data-ttu-id="dcafa-169">En offentlig IP-adress har lagts till genom att associera en offentlig IP-adress resurs tooeither en ny IP-konfiguration eller en befintlig IP-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="dcafa-169">A public IP address is added by associating a public IP address resource tooeither a new IP configuration or an existing IP configuration.</span></span> <span data-ttu-id="dcafa-170">Slutför hello stegen i ett av hello avsnitten som följer, som du behöver.</span><span class="sxs-lookup"><span data-stu-id="dcafa-170">Complete hello steps in one of hello sections that follow, as you require.</span></span>

    > [!NOTE]
    > <span data-ttu-id="dcafa-171">Offentliga IP-adresser har en låg kostnad.</span><span class="sxs-lookup"><span data-stu-id="dcafa-171">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="dcafa-172">Mer om IP-priser, toolearn läsa hello [IP-adress priser](https://azure.microsoft.com/pricing/details/ip-addresses) sidan.</span><span class="sxs-lookup"><span data-stu-id="dcafa-172">toolearn more about IP address pricing, read hello [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="dcafa-173">Det finns en gräns toohello antalet offentliga IP-adresser som kan användas i en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="dcafa-173">There is a limit toohello number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="dcafa-174">Mer om hello gränser, läsa hello toolearn [Azure begränsar](../azure-subscription-service-limits.md#networking-limits) artikel.</span><span class="sxs-lookup"><span data-stu-id="dcafa-174">toolearn more about hello limits, read hello [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>
    >

    - <span data-ttu-id="dcafa-175">**Associera hello offentliga IP-adress resurs tooa nya IP-konfiguration**</span><span class="sxs-lookup"><span data-stu-id="dcafa-175">**Associate hello public IP address resource tooa new IP configuration**</span></span>
    
        <span data-ttu-id="dcafa-176">När du lägger till en offentlig IP-adress i en ny IP-konfiguration måste du också lägga en privat IP-adress, eftersom alla IP-konfigurationer måste ha en privat IP-adress.</span><span class="sxs-lookup"><span data-stu-id="dcafa-176">Whenever you add a public IP address in a new IP configuration, you must also add a private IP address, because all IP configurations must have a private IP address.</span></span> <span data-ttu-id="dcafa-177">Du kan lägga till en befintlig offentlig IP-adressresurs eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="dcafa-177">You can either add an existing public IP address resource, or create a new one.</span></span> <span data-ttu-id="dcafa-178">toocreate en ny ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="dcafa-178">toocreate a new one, enter hello following command:</span></span>
    
        ```powershell
        $myPublicIp3 = New-AzureRmPublicIpAddress `
        -Name "myPublicIp3" `
        -ResourceGroupName $RgName `
        -Location $Location `
        -AllocationMethod Static
        ```

        <span data-ttu-id="dcafa-179">toocreate en ny IP-konfiguration med en statisk privat IP-adress och hello associerade *myPublicIp3* offentlig IP adress resursen måste du ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="dcafa-179">toocreate a new IP configuration with a static private IP address and hello associated *myPublicIp3* public IP address resource, enter hello following command:</span></span>

        ```powershell
        Add-AzureRmNetworkInterfaceIpConfig `
        -Name IPConfig-4 `
        -NetworkInterface $myNIC `
        -Subnet $Subnet `
        -PrivateIpAddress 10.0.0.7 `
        -PublicIpAddress $myPublicIp3
        ```

    - <span data-ttu-id="dcafa-180">**Associera hello offentliga IP-adress resurs tooan befintliga IP-konfiguration**</span><span class="sxs-lookup"><span data-stu-id="dcafa-180">**Associate hello public IP address resource tooan existing IP configuration**</span></span>

        <span data-ttu-id="dcafa-181">En offentlig IP-adressresurs kan bara vara associerad tooan IP-konfiguration som inte redan har en associerad.</span><span class="sxs-lookup"><span data-stu-id="dcafa-181">A public IP address resource can only be associated tooan IP configuration that doesn't already have one associated.</span></span> <span data-ttu-id="dcafa-182">Du kan avgöra om en IP-konfiguration har en tillhörande offentliga IP-adress genom att ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="dcafa-182">You can determine whether an IP configuration has an associated public IP address by entering hello following command:</span></span>

        ```powershell
        $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
        ```

        <span data-ttu-id="dcafa-183">Du kan se utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="dcafa-183">You see output similar toohello following:</span></span>

        ```     
        Name       PrivateIpAddress PublicIpAddress                                           Primary
        
        IPConfig-1 10.0.0.4         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress    True
        IPConfig-2 10.0.0.5         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress   False
        IpConfig-3 10.0.0.6                                                                     False
        ```

        <span data-ttu-id="dcafa-184">Eftersom hello **PublicIpAddress** för *IpConfig-3* är tomt, inga offentliga IP-adressresurs är för närvarande associerad tooit.</span><span class="sxs-lookup"><span data-stu-id="dcafa-184">Since hello **PublicIpAddress** column for *IpConfig-3* is blank, no public IP address resource is currently associated tooit.</span></span> <span data-ttu-id="dcafa-185">Du kan lägga till en befintlig offentlig IP-adress resurs tooIpConfig-3 eller ange följande kommando toocreate en hello:</span><span class="sxs-lookup"><span data-stu-id="dcafa-185">You can add an existing public IP address resource tooIpConfig-3, or enter hello following command toocreate one:</span></span>

        ```powershell
        $MyPublicIp3 = New-AzureRmPublicIpAddress `
        -Name "MyPublicIp3" `
        -ResourceGroupName $RgName `
        -Location $Location -AllocationMethod Static
        ```

        <span data-ttu-id="dcafa-186">Ange följande kommando tooassociate hello offentliga IP-adressen resurs toohello befintliga IP-konfigurationen med namnet hello *IpConfig-3*:</span><span class="sxs-lookup"><span data-stu-id="dcafa-186">Enter hello following command tooassociate hello public IP address resource toohello existing IP configuration named *IpConfig-3*:</span></span>
    
        ```powershell
        Set-AzureRmNetworkInterfaceIpConfig `
        -Name IpConfig-3 `
        -NetworkInterface $mynic `
        -Subnet $Subnet `
        -PublicIpAddress $myPublicIp3
        ```

6. <span data-ttu-id="dcafa-187">Ange hello NIC med hello nya IP-konfigurationen genom att ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="dcafa-187">Set hello NIC with hello new IP configuration by entering hello following command:</span></span>

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $MyNIC
    ```

7. <span data-ttu-id="dcafa-188">Visa hello privata IP-adresser och hello offentliga IP-adress resurser tilldelade toohello NIC genom att ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="dcafa-188">View hello private IP addresses and hello public IP address resources assigned toohello NIC by entering hello following command:</span></span>

    ```powershell   
    $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
    ```
8. <span data-ttu-id="dcafa-189">Lägg till hello privata IP-adress toohello VM operativsystem genom att följa steg hello för operativsystemet i hello [lägga till IP-adresser tooa VM operativsystemet](#os-config) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="dcafa-189">Add hello private IP address toohello VM operating system by completing hello steps for your operating system in hello [Add IP addresses tooa VM operating system](#os-config) section of this article.</span></span> <span data-ttu-id="dcafa-190">Lägg inte till hello offentliga IP-adress toohello operativsystem.</span><span class="sxs-lookup"><span data-stu-id="dcafa-190">Do not add hello public IP address toohello operating system.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
