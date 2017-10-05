---
title: "Hantera nätverkssäkerhetsgrupper - Azure PowerShell | Microsoft Docs"
description: "Lär dig hur du hanterar nätverkssäkerhetsgrupper med hjälp av PowerShell."
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
ms.openlocfilehash: ca7f4926ca4edf9d20612aca74f6ae5f0ed847b3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-network-security-groups-using-powershell"></a><span data-ttu-id="63198-103">Hantera säkerhetsgrupper i nätverket med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="63198-103">Manage network security groups using PowerShell</span></span>

[!INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="63198-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="63198-104">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="63198-105">Den här artikeln täcker distributionsmodell hanteraren för filserverresurser, som Microsoft rekommenderar för de flesta nya distributioner i stället för den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="63198-105">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the classic deployment model.</span></span>
>

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="retrieve-information"></a><span data-ttu-id="63198-106">Hämta Information</span><span class="sxs-lookup"><span data-stu-id="63198-106">Retrieve Information</span></span>
<span data-ttu-id="63198-107">Du kan visa dina befintliga NSG: er, hämta regler för en befintlig NSG och ta reda på vilka resurser en NSG är kopplad till.</span><span class="sxs-lookup"><span data-stu-id="63198-107">You can view your existing NSGs, retrieve rules for an existing NSG, and find out what resources an NSG is associated to.</span></span>

### <a name="view-existing-nsgs"></a><span data-ttu-id="63198-108">Visa befintliga NSG: er</span><span class="sxs-lookup"><span data-stu-id="63198-108">View existing NSGs</span></span>
<span data-ttu-id="63198-109">Om du vill visa alla befintliga NSG: er i en prenumeration kör den `Get-AzureRmNetworkSecurityGroup` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="63198-109">To view all existing NSGs in a subscription, run the `Get-AzureRmNetworkSecurityGroup` cmdlet.</span></span>

<span data-ttu-id="63198-110">Förväntat resultat:</span><span class="sxs-lookup"><span data-stu-id="63198-110">Expected result:</span></span>

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


<span data-ttu-id="63198-111">Om du vill visa listan över NSG: er i en viss resursgrupp, kör den `Get-AzureRmNetworkSecurityGroup` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="63198-111">To view the list of NSGs in a specific resource group, run the `Get-AzureRmNetworkSecurityGroup` cmdlet.</span></span>

<span data-ttu-id="63198-112">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="63198-112">Expected output:</span></span>

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

### <a name="list-all-rules-for-an-nsg"></a><span data-ttu-id="63198-113">Visa en lista med alla regler för en NSG</span><span class="sxs-lookup"><span data-stu-id="63198-113">List all rules for an NSG</span></span>
<span data-ttu-id="63198-114">Visa regler för en NSG som heter **NSG-klientdel**, anger du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="63198-114">To view the rules of an NSG named **NSG-FrontEnd**, enter the following command:</span></span>

```powershell
Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd | Select SecurityRules -ExpandProperty SecurityRules
```

<span data-ttu-id="63198-115">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="63198-115">Expected output:</span></span>

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
> <span data-ttu-id="63198-116">Du kan också använda `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` att lista standardregler från den **NSG-klientdel** NSG.</span><span class="sxs-lookup"><span data-stu-id="63198-116">You can also use `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` to list the default rules from the **NSG-FrontEnd** NSG.</span></span>
> 

### <a name="view-nsgs-associations"></a><span data-ttu-id="63198-117">Visa NSG: er associationer</span><span class="sxs-lookup"><span data-stu-id="63198-117">View NSGs associations</span></span>
<span data-ttu-id="63198-118">Visa vilka resurser de **NSG-klientdel** NSG är associerat med, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="63198-118">To view what resources the **NSG-FrontEnd** NSG is associate with, run the following command:</span></span>

```powershell
Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
```

<span data-ttu-id="63198-119">Leta efter den **NetworkInterfaces** och **undernät** egenskaper som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="63198-119">Look for the **NetworkInterfaces** and **Subnets** properties as shown below:</span></span>

    NetworkInterfaces    : []
    Subnets              : [
                             {
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                               "IpConfigurations": []
                             }
                           ]

<span data-ttu-id="63198-120">I det förra exemplet är NSG: N inte kopplad till någon nätverksgränssnitt (NIC) den är kopplad till ett undernät med namnet **klientdel**.</span><span class="sxs-lookup"><span data-stu-id="63198-120">In the previous example, the NSG is not associated to any network interfaces (NICs); it is associated to a subnet named **FrontEnd**.</span></span>

## <a name="manage-rules"></a><span data-ttu-id="63198-121">Hantera regler</span><span class="sxs-lookup"><span data-stu-id="63198-121">Manage rules</span></span>
<span data-ttu-id="63198-122">Du kan lägga till regler i en befintlig NSG, redigera befintliga regler och ta bort regler.</span><span class="sxs-lookup"><span data-stu-id="63198-122">You can add rules to an existing NSG, edit existing rules, and remove rules.</span></span>

### <a name="add-a-rule"></a><span data-ttu-id="63198-123">Lägg till en regel</span><span class="sxs-lookup"><span data-stu-id="63198-123">Add a rule</span></span>
<span data-ttu-id="63198-124">Att lägga till en regel som tillåter **inkommande** trafik till port **443** från en dator till den **NSG-klientdel** NSG, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="63198-124">To add a rule allowing **inbound** traffic to port **443** from any machine to the **NSG-FrontEnd** NSG, complete the following steps:</span></span>

1. <span data-ttu-id="63198-125">Kör följande kommando för att hämta befintliga NSG: N och lagra den i en variabel:</span><span class="sxs-lookup"><span data-stu-id="63198-125">Run the following command to retrieve the existing NSG and store it in a variable:</span></span>

    ```powershell   
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. <span data-ttu-id="63198-126">Kör följande kommando för att lägga till en regel till NSG: N:</span><span class="sxs-lookup"><span data-stu-id="63198-126">Run the following command to add a rule to the NSG:</span></span>

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

3. <span data-ttu-id="63198-127">Om du vill spara ändringarna i NSG: N, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="63198-127">To save the changes made to the NSG, run the following command:</span></span>

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```
    <span data-ttu-id="63198-128">Förväntad utdata visar endast säkerhetsregler:</span><span class="sxs-lookup"><span data-stu-id="63198-128">Expected output showing only the security rules:</span></span>
   
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

### <a name="change-a-rule"></a><span data-ttu-id="63198-129">Ändra en regel</span><span class="sxs-lookup"><span data-stu-id="63198-129">Change a rule</span></span>
<span data-ttu-id="63198-130">Så här ändrar du regeln som skapade ovan för att tillåta inkommande trafik från den **Internet** , Följ stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="63198-130">To change the rule created above to allow inbound traffic from the **Internet** only, follow the steps below.</span></span>

1. <span data-ttu-id="63198-131">Kör följande kommando för att hämta befintliga NSG: N och lagra den i en variabel:</span><span class="sxs-lookup"><span data-stu-id="63198-131">Run the following command to retrieve the existing NSG and store it in a variable:</span></span>

    ```powershell 
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. <span data-ttu-id="63198-132">Kör följande kommando med de nya inställningarna för regeln:</span><span class="sxs-lookup"><span data-stu-id="63198-132">Run the following command with the new rule settings:</span></span>

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

3. <span data-ttu-id="63198-133">Om du vill spara ändringarna i NSG: N, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="63198-133">To save the changes made to the NSG, run the following command:</span></span>

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

    <span data-ttu-id="63198-134">Förväntad utdata visar endast säkerhetsregler:</span><span class="sxs-lookup"><span data-stu-id="63198-134">Expected output showing only the security rules:</span></span>
   
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

### <a name="delete-a-rule"></a><span data-ttu-id="63198-135">Ta bort en regel</span><span class="sxs-lookup"><span data-stu-id="63198-135">Delete a rule</span></span>
1. <span data-ttu-id="63198-136">Kör följande kommando för att hämta befintliga NSG: N och lagra den i en variabel:</span><span class="sxs-lookup"><span data-stu-id="63198-136">Run the following command to retrieve the existing NSG and store it in a variable:</span></span>

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. <span data-ttu-id="63198-137">Kör följande kommando för att ta bort regeln från NSG: N:</span><span class="sxs-lookup"><span data-stu-id="63198-137">Run the following command to remove the rule from the NSG:</span></span>

    ```powershell
    Remove-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name https-rule
    ```

3. <span data-ttu-id="63198-138">Spara ändringarna i NSG: N, genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="63198-138">Save the changes made to the NSG, by running the following command:</span></span>

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

    <span data-ttu-id="63198-139">Förväntad utdata visar endast säkerhetsregler, meddelande i **https-regel** inte längre visas:</span><span class="sxs-lookup"><span data-stu-id="63198-139">Expected output showing only the security rules, notice the **https-rule** is no longer listed:</span></span>
   
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

## <a name="manage-associations"></a><span data-ttu-id="63198-140">Hantera kopplingar</span><span class="sxs-lookup"><span data-stu-id="63198-140">Manage associations</span></span>
<span data-ttu-id="63198-141">Du kan koppla en NSG till undernät och nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="63198-141">You can associate an NSG to subnets and NICs.</span></span> <span data-ttu-id="63198-142">Du kan också koppla bort en NSG från alla resurser som den är kopplad till.</span><span class="sxs-lookup"><span data-stu-id="63198-142">You can also dissociate an NSG from any resource it's associated to.</span></span>

### <a name="associate-an-nsg-to-a-nic"></a><span data-ttu-id="63198-143">Koppla en NSG till ett nätverkskort</span><span class="sxs-lookup"><span data-stu-id="63198-143">Associate an NSG to a NIC</span></span>
<span data-ttu-id="63198-144">Associera den **NSG-klientdel** NSG till den **TestNICWeb1** NIC, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="63198-144">To associate the **NSG-FrontEnd** NSG to the **TestNICWeb1** NIC, complete the following steps:</span></span>

1. <span data-ttu-id="63198-145">Kör följande kommando för att hämta befintliga NSG: N och lagra den i en variabel:</span><span class="sxs-lookup"><span data-stu-id="63198-145">Run the following command to retrieve the existing NSG and store it in a variable:</span></span>

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. <span data-ttu-id="63198-146">Kör följande kommando för att hämta befintlig NIC och lagra den i en variabel:</span><span class="sxs-lookup"><span data-stu-id="63198-146">Run the following command to retrieve the existing NIC and store it in a variable:</span></span>

    ```powershell
    $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG -Name TestNICWeb1
    ```

3. <span data-ttu-id="63198-147">Ange den **NetworkSecurityGroup** -egenskapen för den **NIC** variabeln till värdet för den **NSG** variabel, genom att ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="63198-147">Set the **NetworkSecurityGroup** property of the **NIC** variable to the value of the **NSG** variable, by entering the following command:</span></span>

    ```powershell
    $nic.NetworkSecurityGroup = $nsg
    ```

4. <span data-ttu-id="63198-148">Om du vill spara ändringarna i nätverkskortet, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="63198-148">To save the changes made to the NIC, run the following command:</span></span>

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```
   
    <span data-ttu-id="63198-149">Förväntad utdata visar endast den **NetworkSecurityGroup** egenskapen:</span><span class="sxs-lookup"><span data-stu-id="63198-149">Expected output showing only the **NetworkSecurityGroup** property:</span></span>
   
        NetworkSecurityGroup : {
                                 "SecurityRules": [],
                                 "DefaultSecurityRules": [],
                                 "NetworkInterfaces": [],
                                 "Subnets": [],
                                 "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                               }

### <a name="dissociate-an-nsg-from-a-nic"></a><span data-ttu-id="63198-150">Koppla bort en NSG från ett nätverkskort</span><span class="sxs-lookup"><span data-stu-id="63198-150">Dissociate an NSG from a NIC</span></span>
<span data-ttu-id="63198-151">Koppla bort den **NSG-klientdel** NSG från den **TestNICWeb1** NIC, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="63198-151">To dissociate the **NSG-FrontEnd** NSG from the **TestNICWeb1** NIC, complete the following steps:</span></span>

1. <span data-ttu-id="63198-152">Kör följande kommando för att hämta befintlig NIC och lagra den i en variabel:</span><span class="sxs-lookup"><span data-stu-id="63198-152">Run the following command to retrieve the existing NIC and store it in a variable:</span></span>

    ```powershell
    $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG -Name TestNICWeb1
    ```

2. <span data-ttu-id="63198-153">Ange den **NetworkSecurityGroup** -egenskapen för den **NIC** variabeln **$null** genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="63198-153">Set the **NetworkSecurityGroup** property of the **NIC** variable to **$null** by running the following command:</span></span>

    ```powershell
    $nic.NetworkSecurityGroup = $null
    ```

3. <span data-ttu-id="63198-154">Om du vill spara ändringarna i nätverkskortet, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="63198-154">To save the changes made to the NIC, run the following command:</span></span>

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```
   
    <span data-ttu-id="63198-155">Förväntad utdata visar endast den **NetworkSecurityGroup** egenskapen:</span><span class="sxs-lookup"><span data-stu-id="63198-155">Expected output showing only the **NetworkSecurityGroup** property:</span></span>
   
        NetworkSecurityGroup : null

### <a name="dissociate-an-nsg-from-a-subnet"></a><span data-ttu-id="63198-156">Koppla bort en NSG från ett undernät</span><span class="sxs-lookup"><span data-stu-id="63198-156">Dissociate an NSG from a subnet</span></span>
<span data-ttu-id="63198-157">Koppla bort den **NSG-klientdel** NSG från den **klientdel** undernät, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="63198-157">To dissociate the **NSG-FrontEnd** NSG from the **FrontEnd** subnet, complete the following steps:</span></span>

1. <span data-ttu-id="63198-158">Kör följande kommando för att hämta befintliga VNet och lagra den i en variabel:</span><span class="sxs-lookup"><span data-stu-id="63198-158">Run the following command to retrieve the existing VNet and store it in a variable:</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG -Name TestVNet
    ```

2. <span data-ttu-id="63198-159">Kör följande kommando för att hämta den **klientdel** undernät och lagra den i en variabel:</span><span class="sxs-lookup"><span data-stu-id="63198-159">Run the following command to retrieve the **FrontEnd** subnet and store it in a variable:</span></span>

    ```powershell
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd
    ```
 
3. <span data-ttu-id="63198-160">Ange den **NetworkSecurityGroup** -egenskapen för den **undernät** variabeln **$null** genom att ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="63198-160">Set the **NetworkSecurityGroup** property of the **subnet** variable to **$null** by entering the following command:</span></span>

    ```powershell
    $subnet.NetworkSecurityGroup = $null
    ```

4. <span data-ttu-id="63198-161">Om du vill spara ändringarna i undernätet, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="63198-161">To save the changes made to the subnet, run the following command:</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="63198-162">Förväntad utdata visar egenskaperna för den **klientdel** undernät.</span><span class="sxs-lookup"><span data-stu-id="63198-162">Expected output showing only the properties of the **FrontEnd** subnet.</span></span> <span data-ttu-id="63198-163">Observera att det finns inte en egenskap för **NetworkSecurityGroup**:</span><span class="sxs-lookup"><span data-stu-id="63198-163">Notice there isn't a property for **NetworkSecurityGroup**:</span></span>
   
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

### <a name="associate-an-nsg-to-a-subnet"></a><span data-ttu-id="63198-164">Koppla en NSG till ett undernät</span><span class="sxs-lookup"><span data-stu-id="63198-164">Associate an NSG to a subnet</span></span>
<span data-ttu-id="63198-165">Associera den **NSG-klientdel** NSG till den **FronEnd** undernät igen, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="63198-165">To associate the **NSG-FrontEnd** NSG to the **FronEnd** subnet again, complete the following steps:</span></span>

1. <span data-ttu-id="63198-166">Kör följande kommando för att hämta befintliga VNet och lagra den i en variabel:</span><span class="sxs-lookup"><span data-stu-id="63198-166">Run the following command to retrieve the existing VNet and store it in a variable:</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG -Name TestVNet
    ```

2. <span data-ttu-id="63198-167">Kör följande kommando för att hämta den **klientdel** undernät och lagra den i en variabel:</span><span class="sxs-lookup"><span data-stu-id="63198-167">Run the following command to retrieve the **FrontEnd** subnet and store it in a variable:</span></span>

    ```powershell
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd
    ```
 
3. <span data-ttu-id="63198-168">Kör följande kommando för att hämta befintliga NSG: N och lagra den i en variabel:</span><span class="sxs-lookup"><span data-stu-id="63198-168">Run the following command to retrieve the existing NSG and store it in a variable:</span></span>

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

4. <span data-ttu-id="63198-169">Ange den **NetworkSecurityGroup** -egenskapen för den **undernät** variabeln **$null** genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="63198-169">Set the **NetworkSecurityGroup** property of the **subnet** variable to **$null** by running the following command:</span></span>

    ```powershell
    $subnet.NetworkSecurityGroup = $nsg
    ```

5. <span data-ttu-id="63198-170">Om du vill spara ändringarna i undernätet, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="63198-170">To save the changes made to the subnet, run the following command:</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="63198-171">Förväntad utdata visar endast den **NetworkSecurityGroup** -egenskapen för den **klientdel** undernät:</span><span class="sxs-lookup"><span data-stu-id="63198-171">Expected output showing only the **NetworkSecurityGroup** property of the **FrontEnd** subnet:</span></span>
   
        ...
        "NetworkSecurityGroup": {
                                  "SecurityRules": [],
                                  "DefaultSecurityRules": [],
                                  "NetworkInterfaces": [],
                                  "Subnets": [],
                                  "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                }
        ...

## <a name="delete-an-nsg"></a><span data-ttu-id="63198-172">Ta bort en NSG</span><span class="sxs-lookup"><span data-stu-id="63198-172">Delete an NSG</span></span>
<span data-ttu-id="63198-173">Du kan bara ta bort en NSG om den inte är kopplad till en resurs.</span><span class="sxs-lookup"><span data-stu-id="63198-173">You can only delete an NSG if it's not associated to any resource.</span></span> <span data-ttu-id="63198-174">Följ stegen nedan om du vill ta bort en NSG.</span><span class="sxs-lookup"><span data-stu-id="63198-174">To delete an NSG, follow the steps below.</span></span>

1. <span data-ttu-id="63198-175">Om du vill kontrollera resurser som är associerade med en NSG kör den `azure network nsg show` enligt [visa NSG: er kopplingarna](#View-NSGs-associations).</span><span class="sxs-lookup"><span data-stu-id="63198-175">To check the resources associated to an NSG, run the `azure network nsg show` as shown in [View NSGs associations](#View-NSGs-associations).</span></span>
2. <span data-ttu-id="63198-176">Om NSG: N är kopplad till varje nätverkskort, kör du den `azure network nic set` enligt [koppla bort en NSG från ett nätverkskort](#Dissociate-an-NSG-from-a-NIC) för varje nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="63198-176">If the NSG is associated to any NICs, run the `azure network nic set` as shown in [Dissociate an NSG from a NIC](#Dissociate-an-NSG-from-a-NIC) for each NIC.</span></span> 
3. <span data-ttu-id="63198-177">Om NSG: N är kopplat till alla undernät måste köra den `azure network vnet subnet set` enligt [koppla bort en NSG från ett undernät](#Dissociate-an-NSG-from-a-subnet) för varje undernät.</span><span class="sxs-lookup"><span data-stu-id="63198-177">If the NSG is associated to any subnet, run the `azure network vnet subnet set` as shown in [Dissociate an NSG from a subnet](#Dissociate-an-NSG-from-a-subnet) for each subnet.</span></span>
4. <span data-ttu-id="63198-178">Ta bort NSG: N genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="63198-178">To delete the NSG, run the following command:</span></span>

    ```powershell
    Remove-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd -Force
    ```
   
   > [!NOTE]
   > <span data-ttu-id="63198-179">Den `-Force` parametern säkerställer du inte behöver bekräfta borttagningen.</span><span class="sxs-lookup"><span data-stu-id="63198-179">The `-Force` parameter ensures you don't need to confirm the deletion.</span></span>
   > 

## <a name="next-steps"></a><span data-ttu-id="63198-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="63198-180">Next steps</span></span>
* <span data-ttu-id="63198-181">[Aktivera loggning](virtual-network-nsg-manage-log.md) för NSG: er.</span><span class="sxs-lookup"><span data-stu-id="63198-181">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>

