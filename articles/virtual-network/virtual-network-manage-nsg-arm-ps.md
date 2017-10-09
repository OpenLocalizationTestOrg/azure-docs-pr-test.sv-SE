---
title: "aaaManage nätverkssäkerhetsgrupper - Azure PowerShell | Microsoft Docs"
description: "Lär dig hur toomanage nätverkssäkerhetsgrupper med hjälp av PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3706ce6c-d9ae-46cb-a048-f0a4e84dc5cc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 930fe5e0827896ad67b24d84e41a5d3f898ba838
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-network-security-groups-using-powershell"></a><span data-ttu-id="9ef17-103">Hantera säkerhetsgrupper i nätverket med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="9ef17-103">Manage network security groups using PowerShell</span></span>

[!INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="9ef17-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="9ef17-104">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="9ef17-105">Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="9ef17-105">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>
>

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="retrieve-information"></a><span data-ttu-id="9ef17-106">Hämta Information</span><span class="sxs-lookup"><span data-stu-id="9ef17-106">Retrieve Information</span></span>
<span data-ttu-id="9ef17-107">Du kan visa dina befintliga NSG: er, hämta regler för en befintlig NSG och ta reda på vilka resurser en NSG är kopplad till.</span><span class="sxs-lookup"><span data-stu-id="9ef17-107">You can view your existing NSGs, retrieve rules for an existing NSG, and find out what resources an NSG is associated to.</span></span>

### <a name="view-existing-nsgs"></a><span data-ttu-id="9ef17-108">Visa befintliga NSG: er</span><span class="sxs-lookup"><span data-stu-id="9ef17-108">View existing NSGs</span></span>
<span data-ttu-id="9ef17-109">tooview alla befintliga NSG: er i en prenumeration kör hello `Get-AzureRmNetworkSecurityGroup` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="9ef17-109">tooview all existing NSGs in a subscription, run hello `Get-AzureRmNetworkSecurityGroup` cmdlet.</span></span>

<span data-ttu-id="9ef17-110">Förväntat resultat:</span><span class="sxs-lookup"><span data-stu-id="9ef17-110">Expected result:</span></span>

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 :                            
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

    Name                 : WEB1
    ResourceGroupName    : RG101
    Location             : eastus2
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/RG101/providers/M
                           icrosoft.Network/networkSecurityGroups/WEB1
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]


<span data-ttu-id="9ef17-111">tooview hello listan över NSG: er i en viss resursgrupp, kör hello `Get-AzureRmNetworkSecurityGroup` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="9ef17-111">tooview hello list of NSGs in a specific resource group, run hello `Get-AzureRmNetworkSecurityGroup` cmdlet.</span></span>

<span data-ttu-id="9ef17-112">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="9ef17-112">Expected output:</span></span>

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 :                            
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

### <a name="list-all-rules-for-an-nsg"></a><span data-ttu-id="9ef17-113">Visa en lista med alla regler för en NSG</span><span class="sxs-lookup"><span data-stu-id="9ef17-113">List all rules for an NSG</span></span>
<span data-ttu-id="9ef17-114">tooview hello regler för en NSG som heter **NSG-klientdel**, ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9ef17-114">tooview hello rules of an NSG named **NSG-FrontEnd**, enter hello following command:</span></span>

```powershell
Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd | Select SecurityRules -ExpandProperty SecurityRules
```

<span data-ttu-id="9ef17-115">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="9ef17-115">Expected output:</span></span>

    Name                     : rdp-rule
    Id                       : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule
    Etag                     : W/"[Id]"
    ProvisioningState        : Succeeded
    Description              : Allow RDP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 100
    Direction                : Inbound

    Name                     : web-rule
    Id                       : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
    Etag                     : W/"[Id]"
    ProvisioningState        : Succeeded
    Description              : Allow HTTP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 80
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 101
    Direction                : Inbound

> [!NOTE]
> <span data-ttu-id="9ef17-116">Du kan också använda `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` toolist hello standardregler från hello **NSG-klientdel** NSG.</span><span class="sxs-lookup"><span data-stu-id="9ef17-116">You can also use `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` toolist hello default rules from hello **NSG-FrontEnd** NSG.</span></span>
> 

### <a name="view-nsgs-associations"></a><span data-ttu-id="9ef17-117">Visa NSG: er associationer</span><span class="sxs-lookup"><span data-stu-id="9ef17-117">View NSGs associations</span></span>
<span data-ttu-id="9ef17-118">tooview vilka resurser hello **NSG-klientdel** NSG är associerad med, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9ef17-118">tooview what resources hello **NSG-FrontEnd** NSG is associate with, run hello following command:</span></span>

```powershell
Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
```

<span data-ttu-id="9ef17-119">Leta efter hello **NetworkInterfaces** och **undernät** egenskaper som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="9ef17-119">Look for hello **NetworkInterfaces** and **Subnets** properties as shown below:</span></span>

    NetworkInterfaces    : []
    Subnets              : [
                             {
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                               "IpConfigurations": []
                             }
                           ]

<span data-ttu-id="9ef17-120">I föregående exempel hello är hello NSG inte associerad tooany nätverksgränssnitt (NIC) Det är associerade tooa undernät med namnet **klientdel**.</span><span class="sxs-lookup"><span data-stu-id="9ef17-120">In hello previous example, hello NSG is not associated tooany network interfaces (NICs); it is associated tooa subnet named **FrontEnd**.</span></span>

## <a name="manage-rules"></a><span data-ttu-id="9ef17-121">Hantera regler</span><span class="sxs-lookup"><span data-stu-id="9ef17-121">Manage rules</span></span>
<span data-ttu-id="9ef17-122">Du kan lägga till regler tooan befintliga NSG, redigera befintliga regler och ta bort regler.</span><span class="sxs-lookup"><span data-stu-id="9ef17-122">You can add rules tooan existing NSG, edit existing rules, and remove rules.</span></span>

### <a name="add-a-rule"></a><span data-ttu-id="9ef17-123">Lägg till en regel</span><span class="sxs-lookup"><span data-stu-id="9ef17-123">Add a rule</span></span>
<span data-ttu-id="9ef17-124">tooadd en regel som tillåter **inkommande** trafik tooport **443** från alla datorn toohello **NSG-klientdel** NSG fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9ef17-124">tooadd a rule allowing **inbound** traffic tooport **443** from any machine toohello **NSG-FrontEnd** NSG, complete hello following steps:</span></span>

1. <span data-ttu-id="9ef17-125">Kör följande kommando tooretrieve hello befintliga NSG hello och lagra den i en variabel:</span><span class="sxs-lookup"><span data-stu-id="9ef17-125">Run hello following command tooretrieve hello existing NSG and store it in a variable:</span></span>

    ```powershell   
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. <span data-ttu-id="9ef17-126">Kör följande kommando tooadd hello en regel toohello NSG:</span><span class="sxs-lookup"><span data-stu-id="9ef17-126">Run hello following command tooadd a rule toohello NSG:</span></span>

    ```powershell
    Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
    -Name https-rule `
    -Description "Allow HTTPS" `
    -Access Allow `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 102 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 443
    ```

3. <span data-ttu-id="9ef17-127">toosave hello ändringar gjorts toohello NSG, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9ef17-127">toosave hello changes made toohello NSG, run hello following command:</span></span>

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```
    <span data-ttu-id="9ef17-128">Förväntad utdata visar endast hello säkerhetsregler:</span><span class="sxs-lookup"><span data-stu-id="9ef17-128">Expected output showing only hello security rules:</span></span>
   
        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"[Id]\"",
                                   "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "*",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="change-a-rule"></a><span data-ttu-id="9ef17-129">Ändra en regel</span><span class="sxs-lookup"><span data-stu-id="9ef17-129">Change a rule</span></span>
<span data-ttu-id="9ef17-130">toochange hello regeln som skapade ovan tooallow inkommande trafik från hello **Internet** endast gör hello nedan.</span><span class="sxs-lookup"><span data-stu-id="9ef17-130">toochange hello rule created above tooallow inbound traffic from hello **Internet** only, follow hello steps below.</span></span>

1. <span data-ttu-id="9ef17-131">Kör följande kommando tooretrieve hello befintliga NSG hello och lagra den i en variabel:</span><span class="sxs-lookup"><span data-stu-id="9ef17-131">Run hello following command tooretrieve hello existing NSG and store it in a variable:</span></span>

    ```powershell 
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. <span data-ttu-id="9ef17-132">Kör följande kommando med hello nya regelinställningar hello:</span><span class="sxs-lookup"><span data-stu-id="9ef17-132">Run hello following command with hello new rule settings:</span></span>

    ```powershell
    Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
    -Name https-rule `
    -Description "Allow HTTPS" `
    -Access Allow `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 102 `
    -SourceAddressPrefix Internet `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 443
    ```

3. <span data-ttu-id="9ef17-133">toosave hello ändringar gjorts toohello NSG, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9ef17-133">toosave hello changes made toohello NSG, run hello following command:</span></span>

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

    <span data-ttu-id="9ef17-134">Förväntad utdata visar endast hello säkerhetsregler:</span><span class="sxs-lookup"><span data-stu-id="9ef17-134">Expected output showing only hello security rules:</span></span>
   
        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"[Id]\"",
                                   "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="delete-a-rule"></a><span data-ttu-id="9ef17-135">Ta bort en regel</span><span class="sxs-lookup"><span data-stu-id="9ef17-135">Delete a rule</span></span>
1. <span data-ttu-id="9ef17-136">Kör följande kommando tooretrieve hello befintliga NSG hello och lagra den i en variabel:</span><span class="sxs-lookup"><span data-stu-id="9ef17-136">Run hello following command tooretrieve hello existing NSG and store it in a variable:</span></span>

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. <span data-ttu-id="9ef17-137">Kör följande kommando tooremove hello regeln från hello NSG hello:</span><span class="sxs-lookup"><span data-stu-id="9ef17-137">Run hello following command tooremove hello rule from hello NSG:</span></span>

    ```powershell
    Remove-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name https-rule
    ```

3. <span data-ttu-id="9ef17-138">Spara hello ändringar toohello NSG, genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="9ef17-138">Save hello changes made toohello NSG, by running hello following command:</span></span>

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

    <span data-ttu-id="9ef17-139">Förväntad utdata visar endast hello säkerhetsregler meddelande hello **https-regel** inte längre visas:</span><span class="sxs-lookup"><span data-stu-id="9ef17-139">Expected output showing only hello security rules, notice hello **https-rule** is no longer listed:</span></span>
   
        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 }
                               ]

## <a name="manage-associations"></a><span data-ttu-id="9ef17-140">Hantera kopplingar</span><span class="sxs-lookup"><span data-stu-id="9ef17-140">Manage associations</span></span>
<span data-ttu-id="9ef17-141">Du kan koppla en NSG toosubnets och nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="9ef17-141">You can associate an NSG toosubnets and NICs.</span></span> <span data-ttu-id="9ef17-142">Du kan också koppla bort en NSG från alla resurser som den är kopplad till.</span><span class="sxs-lookup"><span data-stu-id="9ef17-142">You can also dissociate an NSG from any resource it's associated to.</span></span>

### <a name="associate-an-nsg-tooa-nic"></a><span data-ttu-id="9ef17-143">Koppla en NSG tooa NIC</span><span class="sxs-lookup"><span data-stu-id="9ef17-143">Associate an NSG tooa NIC</span></span>
<span data-ttu-id="9ef17-144">tooassociate hello **NSG-klientdel** NSG toohello **TestNICWeb1** NIC, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9ef17-144">tooassociate hello **NSG-FrontEnd** NSG toohello **TestNICWeb1** NIC, complete hello following steps:</span></span>

1. <span data-ttu-id="9ef17-145">Kör följande kommando tooretrieve hello befintliga NSG hello och lagra den i en variabel:</span><span class="sxs-lookup"><span data-stu-id="9ef17-145">Run hello following command tooretrieve hello existing NSG and store it in a variable:</span></span>

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. <span data-ttu-id="9ef17-146">Hello kör följande kommando tooretrieve hello befintliga NIC och lagrar den i en variabel:</span><span class="sxs-lookup"><span data-stu-id="9ef17-146">Run hello following command tooretrieve hello existing NIC and store it in a variable:</span></span>

    ```powershell
    $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG -Name TestNICWeb1
    ```

3. <span data-ttu-id="9ef17-147">Ange hello **NetworkSecurityGroup** -egenskapen för hello **NIC** variabeln toohello värdet för hello **NSG** variabel, genom att ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9ef17-147">Set hello **NetworkSecurityGroup** property of hello **NIC** variable toohello value of hello **NSG** variable, by entering hello following command:</span></span>

    ```powershell
    $nic.NetworkSecurityGroup = $nsg
    ```

4. <span data-ttu-id="9ef17-148">toosave hello ändringar gjorts toohello NIC, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9ef17-148">toosave hello changes made toohello NIC, run hello following command:</span></span>

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```
   
    <span data-ttu-id="9ef17-149">Förväntad utdata visar endast hello **NetworkSecurityGroup** egenskapen:</span><span class="sxs-lookup"><span data-stu-id="9ef17-149">Expected output showing only hello **NetworkSecurityGroup** property:</span></span>
   
        NetworkSecurityGroup : {
                                 "SecurityRules": [],
                                 "DefaultSecurityRules": [],
                                 "NetworkInterfaces": [],
                                 "Subnets": [],
                                 "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                               }

### <a name="dissociate-an-nsg-from-a-nic"></a><span data-ttu-id="9ef17-150">Koppla bort en NSG från ett nätverkskort</span><span class="sxs-lookup"><span data-stu-id="9ef17-150">Dissociate an NSG from a NIC</span></span>
<span data-ttu-id="9ef17-151">toodissociate hello **NSG-klientdel** NSG från hello **TestNICWeb1** NIC, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9ef17-151">toodissociate hello **NSG-FrontEnd** NSG from hello **TestNICWeb1** NIC, complete hello following steps:</span></span>

1. <span data-ttu-id="9ef17-152">Hello kör följande kommando tooretrieve hello befintliga NIC och lagrar den i en variabel:</span><span class="sxs-lookup"><span data-stu-id="9ef17-152">Run hello following command tooretrieve hello existing NIC and store it in a variable:</span></span>

    ```powershell
    $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG -Name TestNICWeb1
    ```

2. <span data-ttu-id="9ef17-153">Ange hello **NetworkSecurityGroup** -egenskapen för hello **NIC** variabel för**$null** genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="9ef17-153">Set hello **NetworkSecurityGroup** property of hello **NIC** variable too**$null** by running hello following command:</span></span>

    ```powershell
    $nic.NetworkSecurityGroup = $null
    ```

3. <span data-ttu-id="9ef17-154">toosave hello ändringar gjorts toohello NIC, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9ef17-154">toosave hello changes made toohello NIC, run hello following command:</span></span>

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```
   
    <span data-ttu-id="9ef17-155">Förväntad utdata visar endast hello **NetworkSecurityGroup** egenskapen:</span><span class="sxs-lookup"><span data-stu-id="9ef17-155">Expected output showing only hello **NetworkSecurityGroup** property:</span></span>
   
        NetworkSecurityGroup : null

### <a name="dissociate-an-nsg-from-a-subnet"></a><span data-ttu-id="9ef17-156">Koppla bort en NSG från ett undernät</span><span class="sxs-lookup"><span data-stu-id="9ef17-156">Dissociate an NSG from a subnet</span></span>
<span data-ttu-id="9ef17-157">toodissociate hello **NSG-klientdel** NSG från hello **klientdel** undernät, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9ef17-157">toodissociate hello **NSG-FrontEnd** NSG from hello **FrontEnd** subnet, complete hello following steps:</span></span>

1. <span data-ttu-id="9ef17-158">Kör följande kommando tooretrieve hello befintliga VNet hello och lagra den i en variabel:</span><span class="sxs-lookup"><span data-stu-id="9ef17-158">Run hello following command tooretrieve hello existing VNet and store it in a variable:</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG -Name TestVNet
    ```

2. <span data-ttu-id="9ef17-159">Kör hello följande kommando tooretrieve hello **klientdel** undernät och lagra den i en variabel:</span><span class="sxs-lookup"><span data-stu-id="9ef17-159">Run hello following command tooretrieve hello **FrontEnd** subnet and store it in a variable:</span></span>

    ```powershell
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd
    ```
 
3. <span data-ttu-id="9ef17-160">Ange hello **NetworkSecurityGroup** -egenskapen för hello **undernät** variabel för**$null** genom att ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9ef17-160">Set hello **NetworkSecurityGroup** property of hello **subnet** variable too**$null** by entering hello following command:</span></span>

    ```powershell
    $subnet.NetworkSecurityGroup = $null
    ```

4. <span data-ttu-id="9ef17-161">toosave hello ändringar gjorts toohello undernät, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9ef17-161">toosave hello changes made toohello subnet, run hello following command:</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="9ef17-162">Förväntad utdata som visar hello egenskaper för hello **klientdel** undernät.</span><span class="sxs-lookup"><span data-stu-id="9ef17-162">Expected output showing only hello properties of hello **FrontEnd** subnet.</span></span> <span data-ttu-id="9ef17-163">Observera att det finns inte en egenskap för **NetworkSecurityGroup**:</span><span class="sxs-lookup"><span data-stu-id="9ef17-163">Notice there isn't a property for **NetworkSecurityGroup**:</span></span>
   
            ...
            Subnets           : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"[Id]\"",
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [
                                      {
                                        "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1"
                                      },
                                      {
                                        "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1"
                                      }
                                    ],
                                    "ProvisioningState": "Succeeded"
                                  },
                                    ...
                                ]

### <a name="associate-an-nsg-tooa-subnet"></a><span data-ttu-id="9ef17-164">Koppla en NSG tooa undernät</span><span class="sxs-lookup"><span data-stu-id="9ef17-164">Associate an NSG tooa subnet</span></span>
<span data-ttu-id="9ef17-165">tooassociate hello **NSG-klientdel** NSG toohello **FronEnd** undernät igen, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9ef17-165">tooassociate hello **NSG-FrontEnd** NSG toohello **FronEnd** subnet again, complete hello following steps:</span></span>

1. <span data-ttu-id="9ef17-166">Kör följande kommando tooretrieve hello befintliga VNet hello och lagra den i en variabel:</span><span class="sxs-lookup"><span data-stu-id="9ef17-166">Run hello following command tooretrieve hello existing VNet and store it in a variable:</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG -Name TestVNet
    ```

2. <span data-ttu-id="9ef17-167">Kör hello följande kommando tooretrieve hello **klientdel** undernät och lagra den i en variabel:</span><span class="sxs-lookup"><span data-stu-id="9ef17-167">Run hello following command tooretrieve hello **FrontEnd** subnet and store it in a variable:</span></span>

    ```powershell
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd
    ```
 
3. <span data-ttu-id="9ef17-168">Kör följande kommando tooretrieve hello befintliga NSG hello och lagra den i en variabel:</span><span class="sxs-lookup"><span data-stu-id="9ef17-168">Run hello following command tooretrieve hello existing NSG and store it in a variable:</span></span>

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

4. <span data-ttu-id="9ef17-169">Ange hello **NetworkSecurityGroup** -egenskapen för hello **undernät** variabel för**$null** genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="9ef17-169">Set hello **NetworkSecurityGroup** property of hello **subnet** variable too**$null** by running hello following command:</span></span>

    ```powershell
    $subnet.NetworkSecurityGroup = $nsg
    ```

5. <span data-ttu-id="9ef17-170">toosave hello ändringar gjorts toohello undernät, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9ef17-170">toosave hello changes made toohello subnet, run hello following command:</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="9ef17-171">Förväntad utdata visar endast hello **NetworkSecurityGroup** -egenskapen för hello **klientdel** undernät:</span><span class="sxs-lookup"><span data-stu-id="9ef17-171">Expected output showing only hello **NetworkSecurityGroup** property of hello **FrontEnd** subnet:</span></span>
   
        ...
        "NetworkSecurityGroup": {
                                  "SecurityRules": [],
                                  "DefaultSecurityRules": [],
                                  "NetworkInterfaces": [],
                                  "Subnets": [],
                                  "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                }
        ...

## <a name="delete-an-nsg"></a><span data-ttu-id="9ef17-172">Ta bort en NSG</span><span class="sxs-lookup"><span data-stu-id="9ef17-172">Delete an NSG</span></span>
<span data-ttu-id="9ef17-173">Du kan bara ta bort en NSG om den inte är kopplad till tooany resurs.</span><span class="sxs-lookup"><span data-stu-id="9ef17-173">You can only delete an NSG if it's not associated tooany resource.</span></span> <span data-ttu-id="9ef17-174">toodelete en NSG åtgärderna hello nedan.</span><span class="sxs-lookup"><span data-stu-id="9ef17-174">toodelete an NSG, follow hello steps below.</span></span>

1. <span data-ttu-id="9ef17-175">associerad toocheck hello resurser tooan NSG, kör hello `azure network nsg show` enligt [visa NSG: er kopplingarna](#View-NSGs-associations).</span><span class="sxs-lookup"><span data-stu-id="9ef17-175">toocheck hello resources associated tooan NSG, run hello `azure network nsg show` as shown in [View NSGs associations](#View-NSGs-associations).</span></span>
2. <span data-ttu-id="9ef17-176">Om hello NSG är associerad tooany nätverkskort, kör hello `azure network nic set` enligt [koppla bort en NSG från ett nätverkskort](#Dissociate-an-NSG-from-a-NIC) för varje nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="9ef17-176">If hello NSG is associated tooany NICs, run hello `azure network nic set` as shown in [Dissociate an NSG from a NIC](#Dissociate-an-NSG-from-a-NIC) for each NIC.</span></span> 
3. <span data-ttu-id="9ef17-177">Om hello NSG är associerad tooany undernät, kör hello `azure network vnet subnet set` enligt [koppla bort en NSG från ett undernät](#Dissociate-an-NSG-from-a-subnet) för varje undernät.</span><span class="sxs-lookup"><span data-stu-id="9ef17-177">If hello NSG is associated tooany subnet, run hello `azure network vnet subnet set` as shown in [Dissociate an NSG from a subnet](#Dissociate-an-NSG-from-a-subnet) for each subnet.</span></span>
4. <span data-ttu-id="9ef17-178">toodelete hello NSG, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="9ef17-178">toodelete hello NSG, run hello following command:</span></span>

    ```powershell
    Remove-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd -Force
    ```
   
   > [!NOTE]
   > <span data-ttu-id="9ef17-179">Hej `-Force` parametern ser du inte behöver tooconfirm hello borttagning.</span><span class="sxs-lookup"><span data-stu-id="9ef17-179">hello `-Force` parameter ensures you don't need tooconfirm hello deletion.</span></span>
   > 

## <a name="next-steps"></a><span data-ttu-id="9ef17-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9ef17-180">Next steps</span></span>
* <span data-ttu-id="9ef17-181">[Aktivera loggning](virtual-network-nsg-manage-log.md) för NSG: er.</span><span class="sxs-lookup"><span data-stu-id="9ef17-181">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>

