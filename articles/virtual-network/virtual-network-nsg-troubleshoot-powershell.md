---
title: "Felsöka Nätverkssäkerhetsgrupper - PowerShell | Microsoft Docs"
description: "Lär dig hur du felsöker Nätverkssäkerhetsgrupper i Azure Resource Manager-distributionsmodellen med hjälp av Azure PowerShell."
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 4c732bb7-5cb1-40af-9e6d-a2a307c2a9c4
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: 5edaf7197576ac1c0bd1fc6bed21fd65ed135106
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-network-security-groups-using-azure-powershell"></a><span data-ttu-id="140e3-103">Felsöka Nätverkssäkerhetsgrupper med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="140e3-103">Troubleshoot Network Security Groups using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="140e3-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="140e3-104">Azure Portal</span></span>](virtual-network-nsg-troubleshoot-portal.md)
> * [<span data-ttu-id="140e3-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="140e3-105">PowerShell</span></span>](virtual-network-nsg-troubleshoot-powershell.md)
> 
> 

<span data-ttu-id="140e3-106">Om du konfigurerade Nätverkssäkerhetsgrupper (NSG: er) på den virtuella datorn (VM) och har problem med anslutningen VM, innehåller den här artikeln en översikt över diagnostikfunktionerna för NSG: er för att fortsätta felsökningen.</span><span class="sxs-lookup"><span data-stu-id="140e3-106">If you configured Network Security Groups (NSGs) on your virtual machine (VM) and are experiencing VM connectivity issues, this article provides an overview of diagnostics capabilities for NSGs to help troubleshoot further.</span></span>

<span data-ttu-id="140e3-107">NSG: er kan du styra vilka typer av trafik som flöde till och från virtuella datorer (VM).</span><span class="sxs-lookup"><span data-stu-id="140e3-107">NSGs enable you to control the types of traffic that flow in and out of your virtual machines (VMs).</span></span> <span data-ttu-id="140e3-108">NSG: er kan tillämpas på undernät i ett Azure Virtual Network (VNet), nätverksgränssnitt (NIC) eller båda.</span><span class="sxs-lookup"><span data-stu-id="140e3-108">NSGs can be applied to subnets in an Azure Virtual Network (VNet), network interfaces (NIC), or both.</span></span> <span data-ttu-id="140e3-109">Effektiva reglerna som gäller för ett nätverkskort är en sammanställning av regler som finns i NSG: er som tillämpas på ett nätverkskort och undernät som den är ansluten till.</span><span class="sxs-lookup"><span data-stu-id="140e3-109">The effective rules applied to a NIC are an aggregation of the rules that exist in the NSGs applied to a NIC and the subnet it is connected to.</span></span> <span data-ttu-id="140e3-110">Regler för dessa NSG: er kan ibland står i konflikt med varandra och påverka nätverksanslutning för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="140e3-110">Rules across these NSGs can sometimes conflict with each other and impact a VM's network connectivity.</span></span>  

<span data-ttu-id="140e3-111">Du kan visa alla effektiva säkerhetsregler från dina NSG: er som tillämpas på den Virtuella datorns nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="140e3-111">You can view all the effective security rules from your NSGs, as applied on your VM's NICs.</span></span> <span data-ttu-id="140e3-112">Den här artikeln visar hur du felsöker problem med nätverksanslutningen VM med hjälp av reglerna i Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="140e3-112">This article shows how to troubleshoot VM connectivity issues using these rules in the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="140e3-113">Om du inte är bekant med principerna för VNet och NSG läsa den [för virtuella nätverk](virtual-networks-overview.md) och [Nätverkssäkerhetsgrupper](virtual-networks-nsg.md) översikt artiklar.</span><span class="sxs-lookup"><span data-stu-id="140e3-113">If you're not familiar with VNet and NSG concepts, read the [Virtual network](virtual-networks-overview.md) and [Network security groups](virtual-networks-nsg.md) overview articles.</span></span>

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a><span data-ttu-id="140e3-114">Använda effektiva säkerhetsregler för att felsöka VM trafikflöde</span><span class="sxs-lookup"><span data-stu-id="140e3-114">Using Effective Security Rules to troubleshoot VM traffic flow</span></span>
<span data-ttu-id="140e3-115">Det scenario som följer är ett exempel på ett vanligt anslutningsproblem:</span><span class="sxs-lookup"><span data-stu-id="140e3-115">The scenario that follows is an example of a common connection problem:</span></span>

<span data-ttu-id="140e3-116">En virtuell dator med namnet *VM1* är en del av ett undernät med namnet *Undernät1* inom ett VNet med namnet *WestUS VNet1*.</span><span class="sxs-lookup"><span data-stu-id="140e3-116">A VM named *VM1* is part of a subnet named *Subnet1* within a VNet named *WestUS-VNet1*.</span></span> <span data-ttu-id="140e3-117">Ett försök att ansluta till den virtuella datorn med RDP över TCP-port 3389 misslyckas.</span><span class="sxs-lookup"><span data-stu-id="140e3-117">An attempt to connect to the VM using RDP over TCP port 3389 fails.</span></span> <span data-ttu-id="140e3-118">NSG: er som tillämpas på båda NIC *VM1 NIC1* och undernätet *Undernät1*.</span><span class="sxs-lookup"><span data-stu-id="140e3-118">NSGs are applied at both the NIC *VM1-NIC1* and the subnet *Subnet1*.</span></span> <span data-ttu-id="140e3-119">Trafik till TCP-port 3389 tillåts i NSG: N som är kopplad till nätverksgränssnittet *VM1 NIC1*, men TCP ping till VM1 är port 3389 misslyckas.</span><span class="sxs-lookup"><span data-stu-id="140e3-119">Traffic to TCP port 3389 is allowed in the NSG associated with the network interface *VM1-NIC1*, however TCP ping to VM1's port 3389 fails.</span></span>

<span data-ttu-id="140e3-120">Följande steg kan användas för att fastställa inkommande och utgående anslutningsfel över alla portar när det här exemplet används TCP-port 3389.</span><span class="sxs-lookup"><span data-stu-id="140e3-120">While this example uses TCP port 3389, the following steps can be used to determine inbound and outbound connection failures over any port.</span></span>

## <a name="detailed-troubleshooting-steps"></a><span data-ttu-id="140e3-121">Detaljerad felsökning</span><span class="sxs-lookup"><span data-stu-id="140e3-121">Detailed Troubleshooting Steps</span></span>
<span data-ttu-id="140e3-122">Utför följande steg för att felsöka NSG: er för en virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="140e3-122">Complete the following steps to troubleshoot NSGs for a VM:</span></span>

1. <span data-ttu-id="140e3-123">Starta en Azure PowerShell-sessionen och logga in på Azure.</span><span class="sxs-lookup"><span data-stu-id="140e3-123">Start an Azure PowerShell session and login to Azure.</span></span> <span data-ttu-id="140e3-124">Om du inte är bekant med Azure PowerShell kan du läsa den [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview) artikel.</span><span class="sxs-lookup"><span data-stu-id="140e3-124">If you're not familiar with using Azure PowerShell, read the [How to install and configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="140e3-125">Ange följande kommando för att returnera alla NSG-regler tillämpas på ett nätverkskort med namnet *VM1 NIC1* i resursgruppen *RG1*:</span><span class="sxs-lookup"><span data-stu-id="140e3-125">Enter the following command to return all NSG rules applied to a NIC named *VM1-NIC1* in the resource group *RG1*:</span></span>
   
        Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [!TIP]
   > <span data-ttu-id="140e3-126">Om du inte vet namnet på ett nätverkskort, kan du ange följande kommando för att hämta namnen på alla nätverkskort i en resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="140e3-126">If you don't know the name of a NIC, enter the following command to retrieve the names of all NICs in a resource group:</span></span> 
   > 
   > `Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name`
   > 
   > 
   
    <span data-ttu-id="140e3-127">Följande är ett exempel på effektiva regler utdata returneras för den *VM1 NIC1* NIC:</span><span class="sxs-lookup"><span data-stu-id="140e3-127">The following text is a sample of the effective rules output returned for the *VM1-NIC1* NIC:</span></span>
   
        NetworkSecurityGroup   : {
                                   "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/VM1-NIC1-NSG"
                                 }
        Association            : {
                                   "NetworkInterface": {
                                     "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkInterfaces/VM1-NIC1"
                                   }
                                 }
        EffectiveSecurityRules : [
                                 {
                                 "Name": "securityRules/allowRDP",
                                 "Protocol": "Tcp",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "3389-3389",
                                 "SourceAddressPrefix": "Internet",
                                 "DestinationAddressPrefix": "0.0.0.0/0",
                                 "ExpandedSourceAddressPrefix": [… ],
                                 "ExpandedDestinationAddressPrefix": [],
                                 "Access": "Allow",
                                 "Priority": 1000,
                                 "Direction": "Inbound"
                                 },
                                 {
                                 "Name": "defaultSecurityRules/AllowVnetInBound",
                                 "Protocol": "All",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "0-65535",
                                 "SourceAddressPrefix": "VirtualNetwork",
                                 "DestinationAddressPrefix": "VirtualNetwork",
                                 "ExpandedSourceAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                 "ExpandedDestinationAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                  "Access": "Allow",
                                  "Priority": 65000,
                                  "Direction": "Inbound"
                                  },…
                         ]
   
        NetworkSecurityGroup   : {
                                   "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/Subnet1-NSG"
                                 }
        Association            : {
                                   "Subnet": {
                                     "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/virtualNetworks/WestUS-VNet1/subnets/Subnet1"
                                 }
                                 }
        EffectiveSecurityRules : [
                                 {
                                "Name": "securityRules/denyRDP",
                                "Protocol": "Tcp",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "3389-3389",
                                "SourceAddressPrefix": "Internet",
                                "DestinationAddressPrefix": "0.0.0.0/0",
                                "ExpandedSourceAddressPrefix": [
                                   ... ],
                                "ExpandedDestinationAddressPrefix": [],
                                "Access": "Deny",
                                "Priority": 1000,
                                "Direction": "Inbound"
                                },
                                {
                                "Name": "defaultSecurityRules/AllowVnetInBound",
                                "Protocol": "All",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "0-65535",
                                "SourceAddressPrefix": "VirtualNetwork",
                                "DestinationAddressPrefix": "VirtualNetwork",
                                "ExpandedSourceAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "ExpandedDestinationAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "Access": "Allow",
                                "Priority": 65000,
                                "Direction": "Inbound"
                                },...
                                ]
   
    <span data-ttu-id="140e3-128">Observera följande information i utdata:</span><span class="sxs-lookup"><span data-stu-id="140e3-128">Note the following information in the output:</span></span>
   
   * <span data-ttu-id="140e3-129">Det finns två **NetworkSecurityGroup** avsnitt: en är associerad med ett undernät (*Undernät1*) och ett är kopplat till ett nätverkskort (*VM1 NIC1*).</span><span class="sxs-lookup"><span data-stu-id="140e3-129">There are two **NetworkSecurityGroup** sections: One is associated with a subnet (*Subnet1*) and one is associated with a NIC (*VM1-NIC1*).</span></span> <span data-ttu-id="140e3-130">I det här exemplet har en NSG tillämpats för varje.</span><span class="sxs-lookup"><span data-stu-id="140e3-130">In this example, an NSG has been applied to each.</span></span>
   * <span data-ttu-id="140e3-131">**Associationen** visar resurs (undernät eller NIC) angivna NSG är associerad med.</span><span class="sxs-lookup"><span data-stu-id="140e3-131">**Association** shows the resource (subnet or NIC) a given NSG is associated with.</span></span> <span data-ttu-id="140e3-132">Om NSG-resurs är flyttas/koppla bort omedelbart innan du kör det här kommandot kan behöva du vänta några sekunder innan ändringen så att den återger kommandots utdata.</span><span class="sxs-lookup"><span data-stu-id="140e3-132">If the NSG resource is moved/disassociated immediately before running this command, you may need to wait a few seconds for the change to reflect in the command output.</span></span> 
   * <span data-ttu-id="140e3-133">De namn som inleds med *defaultSecurityRules*: när en NSG skapas, skapas flera standardregler för säkerhet i den.</span><span class="sxs-lookup"><span data-stu-id="140e3-133">The rule names that are prefaced with *defaultSecurityRules*: When an NSG is created, several default security rules are created within it.</span></span> <span data-ttu-id="140e3-134">Standardreglerna kan inte tas bort, men de kan åsidosättas med högre Prioritetsregler.</span><span class="sxs-lookup"><span data-stu-id="140e3-134">Default rules can't be removed, but they can be overridden with higher priority rules.</span></span>
     <span data-ttu-id="140e3-135">Läs den [NSG översikt](virtual-networks-nsg.md#default-rules) artikeln om du vill lära dig mer om NSG standard säkerhetsregler.</span><span class="sxs-lookup"><span data-stu-id="140e3-135">Read the [NSG overview](virtual-networks-nsg.md#default-rules) article to learn more about NSG default security rules.</span></span>
   * <span data-ttu-id="140e3-136">**ExpandedAddressPrefix** expanderar adressprefix för NSG standardtaggar.</span><span class="sxs-lookup"><span data-stu-id="140e3-136">**ExpandedAddressPrefix** expands the address prefixes for NSG default tags.</span></span> <span data-ttu-id="140e3-137">Taggar representerar flera adressprefix.</span><span class="sxs-lookup"><span data-stu-id="140e3-137">Tags represent multiple address prefixes.</span></span> <span data-ttu-id="140e3-138">Utökningen av taggar kan vara användbart när du felsöker VM anslutning till eller från specifika adressprefix.</span><span class="sxs-lookup"><span data-stu-id="140e3-138">Expansion of the tags can be useful when troubleshooting VM connectivity to/from specific address prefixes.</span></span> <span data-ttu-id="140e3-139">Till exempel med VNET-peering utökas VIRTUAL_NETWORK taggen visas peerkoppla VNet-prefix i föregående utdata.</span><span class="sxs-lookup"><span data-stu-id="140e3-139">For example, with VNET peering, VIRTUAL_NETWORK tag expands to show peered VNet prefixes in the previous output.</span></span>
     
     > [!NOTE]
     > <span data-ttu-id="140e3-140">Kommandot endast visar effektiva reglerna om en NSG är associerad med ett undernät och/eller ett nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="140e3-140">The command only shows effective rules if an NSG is associated with either a subnet, a NIC, or both.</span></span> <span data-ttu-id="140e3-141">En virtuell dator kan ha flera nätverkskort med olika NSG: er som används.</span><span class="sxs-lookup"><span data-stu-id="140e3-141">A VM may have multiple NICs with different NSGs applied.</span></span> <span data-ttu-id="140e3-142">När du felsöker, kör du kommandot för varje nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="140e3-142">When troubleshooting, run the command for each NIC.</span></span>
     > 
     > 
3. <span data-ttu-id="140e3-143">För att underlätta filtrering över större antal NSG-regler kan du ange följande kommandon för att fortsätta felsökningen:</span><span class="sxs-lookup"><span data-stu-id="140e3-143">To ease filtering over larger number of NSG rules, enter the following commands to troubleshoot further:</span></span> 
   
        $NSGs = Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
        $NSGs.EffectiveSecurityRules | Sort-Object Direction, Access, Priority | Out-GridView
   
    <span data-ttu-id="140e3-144">Ett filter för RDP-trafik (TCP-port 3389), används till diagramvyn, enligt följande bild:</span><span class="sxs-lookup"><span data-stu-id="140e3-144">A filter for RDP traffic (TCP port 3389), is applied to the grid view, as shown in the following picture:</span></span>
   
    ![Regellistan](./media/virtual-network-nsg-troubleshoot-powershell/rules.png)
4. <span data-ttu-id="140e3-146">Som du ser i diagramvyn finns både tillåta och neka regler för RDP.</span><span class="sxs-lookup"><span data-stu-id="140e3-146">As you can see in the grid view, there are both allow and deny rules for RDP.</span></span> <span data-ttu-id="140e3-147">Utdata från steg 2 visar att den *DenyRDP* regeln är i NSG tillämpad på undernätet.</span><span class="sxs-lookup"><span data-stu-id="140e3-147">The output from step 2 shows that the *DenyRDP* rule is in the NSG applied to the subnet.</span></span> <span data-ttu-id="140e3-148">Regler för inkommande trafik bearbetas NSG: er tillämpad på undernätet först.</span><span class="sxs-lookup"><span data-stu-id="140e3-148">For inbound rules, NSGs applied to the subnet are processed first.</span></span> <span data-ttu-id="140e3-149">Om en matchning hittas, bearbetas inte NSG tillämpas för nätverksgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="140e3-149">If a match is found, the NSG applied to the network interface is not processed.</span></span> <span data-ttu-id="140e3-150">I det här fallet den *DenyRDP* regeln från undernätet blockerar RDP till den virtuella datorn (**VM1**).</span><span class="sxs-lookup"><span data-stu-id="140e3-150">In this case, the *DenyRDP* rule from the subnet blocks RDP to the VM (**VM1**).</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="140e3-151">En virtuell dator kan ha flera nätverkskort som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="140e3-151">A VM may have multiple NICs attached to it.</span></span> <span data-ttu-id="140e3-152">Varje kanske är anslutet till ett annat undernät.</span><span class="sxs-lookup"><span data-stu-id="140e3-152">Each may be connected to a different subnet.</span></span> <span data-ttu-id="140e3-153">Eftersom kommandona i föregående steg körs mot ett nätverkskort, är det viktigt att se till att du anger du får anslutningsfel till nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="140e3-153">Since the commands in the previous steps are run against a NIC, it's important to ensure that you specify the NIC you're having the connectivity failure to.</span></span> <span data-ttu-id="140e3-154">Om du inte är säker på att köra du alltid kommandon mot varje nätverkskort som är anslutet till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="140e3-154">If you're not sure, you can always run the commands against each NIC attached to the VM.</span></span>
   > 
   > 
5. <span data-ttu-id="140e3-155">Till RDP till VM1 ändra den *neka RDP (port 3389)* regel att *Tillåt RDP(3389)* i den **Undernät1 NSG** NSG.</span><span class="sxs-lookup"><span data-stu-id="140e3-155">To RDP into VM1, change the *Deny RDP (3389)* rule to *Allow RDP(3389)* in the **Subnet1-NSG** NSG.</span></span> <span data-ttu-id="140e3-156">Kontrollera att TCP-port 3389 är öppen genom att öppna en RDP-anslutning till den virtuella datorn eller med hjälp av verktyget PsPing.</span><span class="sxs-lookup"><span data-stu-id="140e3-156">Confirm that TCP port 3389 is open by opening an RDP connection to the VM or using the PsPing tool.</span></span> <span data-ttu-id="140e3-157">Du kan lära dig mer om PsPing genom att läsa den [PsPing hämtningssidan](https://technet.microsoft.com/sysinternals/psping.aspx)</span><span class="sxs-lookup"><span data-stu-id="140e3-157">You can learn more about PsPing by reading the [PsPing download page](https://technet.microsoft.com/sysinternals/psping.aspx)</span></span>
   
    <span data-ttu-id="140e3-158">Du kan eller ta bort regler från en NSG med hjälp av informationen i utdata från kommandot:</span><span class="sxs-lookup"><span data-stu-id="140e3-158">You can or remove rules from an NSG by using the information in the output from the following command:</span></span>
   
        Get-Help *-AzureRmNetworkSecurityRuleConfig

## <a name="considerations"></a><span data-ttu-id="140e3-159">Överväganden</span><span class="sxs-lookup"><span data-stu-id="140e3-159">Considerations</span></span>
<span data-ttu-id="140e3-160">Tänk på följande när du felsöker problem med nätverksanslutningen:</span><span class="sxs-lookup"><span data-stu-id="140e3-160">Consider the following points when troubleshooting connectivity problems:</span></span>

* <span data-ttu-id="140e3-161">Standard NSG-regler ska blockera inkommande åtkomst från internet och tillåter bara VNet inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="140e3-161">Default NSG rules will block inbound access from the internet and only permit VNet inbound traffic.</span></span> <span data-ttu-id="140e3-162">Regler bör läggas uttryckligen vill tillåta inkommande åtkomst från Internet, som krävs.</span><span class="sxs-lookup"><span data-stu-id="140e3-162">Rules should be explicitly added to allow inbound access from Internet, as required.</span></span>
* <span data-ttu-id="140e3-163">Om det finns inga NSG säkerhetsregler som orsakar en virtuell dator är ansluten till nätverket misslyckas, kan det bero på följande:</span><span class="sxs-lookup"><span data-stu-id="140e3-163">If there are no NSG security rules causing a VM’s network connectivity to fail, the problem may be due to:</span></span>
  * <span data-ttu-id="140e3-164">Brandväggsprogram som körs i den Virtuella datorns operativsystem</span><span class="sxs-lookup"><span data-stu-id="140e3-164">Firewall software running within the VM's operating system</span></span>
  * <span data-ttu-id="140e3-165">Vägar som konfigurerats för virtuella installationer eller lokal trafik.</span><span class="sxs-lookup"><span data-stu-id="140e3-165">Routes configured for virtual appliances or on-premises traffic.</span></span> <span data-ttu-id="140e3-166">Internet-trafik kan omdirigeras till lokalt via Tvingad tunneltrafik.</span><span class="sxs-lookup"><span data-stu-id="140e3-166">Internet traffic can be redirected to on-premises via forced-tunneling.</span></span> <span data-ttu-id="140e3-167">En RDP/SSH-anslutning från Internet till den virtuella datorn kanske inte fungerar med den här inställningen, beroende på hur trafiken hanteras i nätverksmaskinvara lokalt.</span><span class="sxs-lookup"><span data-stu-id="140e3-167">An RDP/SSH connection from the Internet to your VM may not work with this setting, depending on how the on-premises network hardware handles this traffic.</span></span> <span data-ttu-id="140e3-168">Läs den [felsökning vägar](virtual-network-routes-troubleshoot-powershell.md) artikel för att lära dig att felsöka väg problem som kan hindra flödet av trafik till och från den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="140e3-168">Read the [Troubleshooting Routes](virtual-network-routes-troubleshoot-powershell.md) article to learn how to diagnose route problems that may be impeding the flow of traffic in and out of the VM.</span></span> 
* <span data-ttu-id="140e3-169">Om du har peerkoppla Vnet, som standard, expanderar taggen VIRTUAL_NETWORK automatiskt för att inkludera prefix för peerkoppla Vnet.</span><span class="sxs-lookup"><span data-stu-id="140e3-169">If you have peered VNets, by default, the VIRTUAL_NETWORK tag will automatically expand to include prefixes for peered VNets.</span></span> <span data-ttu-id="140e3-170">Du kan visa dessa prefix i den **ExpandedAddressPrefix** lista, för att felsöka eventuella problem som är relaterade till VNet-peering anslutning.</span><span class="sxs-lookup"><span data-stu-id="140e3-170">You can view these prefixes in the **ExpandedAddressPrefix** list, to troubleshoot any issues related to VNet peering connectivity.</span></span> 
* <span data-ttu-id="140e3-171">Effektiva säkerhetsregler visas bara om det finns en NSG som är kopplade till Virtuellt datorns nätverkskort och undernät.</span><span class="sxs-lookup"><span data-stu-id="140e3-171">Effective security rules are only shown if there is an NSG associated with the VM’s NIC and or subnet.</span></span> 
* <span data-ttu-id="140e3-172">Om det finns inga NSG: er som är kopplade till nätverkskortet eller undernät och att du har en offentlig IP-adress som tilldelats till den virtuella datorn, kommer alla portar vara öppna för inkommande och utgående åtkomst.</span><span class="sxs-lookup"><span data-stu-id="140e3-172">If there are no NSGs associated with the NIC or subnet and you have a public IP address assigned to your VM, all ports will be open for inbound and outbound access.</span></span> <span data-ttu-id="140e3-173">Om den virtuella datorn har en offentlig IP-adress, rekommenderas tillämpa NSG: er till nätverkskort eller undernät.</span><span class="sxs-lookup"><span data-stu-id="140e3-173">If the VM has a public IP address, applying NSGs to the NIC or subnet is strongly recommended.</span></span>  

