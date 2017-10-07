---
title: "aaaTroubleshoot Nätverkssäkerhetsgrupper - PowerShell | Microsoft Docs"
description: "Lär dig hur tootroubleshoot Nätverkssäkerhetsgrupper i hello Azure Resource Manager distribution modellen med hjälp av Azure PowerShell."
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
ms.openlocfilehash: 95fd60fa72cf6d17fa990e3c3eb7d980878f7c15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-network-security-groups-using-azure-powershell"></a><span data-ttu-id="ae321-103">Felsöka Nätverkssäkerhetsgrupper med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ae321-103">Troubleshoot Network Security Groups using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ae321-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ae321-104">Azure Portal</span></span>](virtual-network-nsg-troubleshoot-portal.md)
> * [<span data-ttu-id="ae321-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ae321-105">PowerShell</span></span>](virtual-network-nsg-troubleshoot-powershell.md)
> 
> 

<span data-ttu-id="ae321-106">Om du konfigurerade Nätverkssäkerhetsgrupper (NSG: er) på den virtuella datorn (VM) och har problem med anslutningen VM, för den här artikeln innehåller en översikt över diagnostikfunktionerna för NSG: er toohelp fortsätta felsökningen.</span><span class="sxs-lookup"><span data-stu-id="ae321-106">If you configured Network Security Groups (NSGs) on your virtual machine (VM) and are experiencing VM connectivity issues, this article provides an overview of diagnostics capabilities for NSGs toohelp troubleshoot further.</span></span>

<span data-ttu-id="ae321-107">NSG: er kan du toocontrol hello typer av trafik som flöde till och från virtuella datorer (VM).</span><span class="sxs-lookup"><span data-stu-id="ae321-107">NSGs enable you toocontrol hello types of traffic that flow in and out of your virtual machines (VMs).</span></span> <span data-ttu-id="ae321-108">NSG: er kan vara tillämpade toosubnets i ett Azure Virtual Network (VNet), nätverksgränssnitt (NIC) eller båda.</span><span class="sxs-lookup"><span data-stu-id="ae321-108">NSGs can be applied toosubnets in an Azure Virtual Network (VNet), network interfaces (NIC), or both.</span></span> <span data-ttu-id="ae321-109">hello effektiva reglerna tooa NIC är en sammanställning av hello regler som finns i hello NSG: er används tooa NIC och hello undernät, det är anslutet till.</span><span class="sxs-lookup"><span data-stu-id="ae321-109">hello effective rules applied tooa NIC are an aggregation of hello rules that exist in hello NSGs applied tooa NIC and hello subnet it is connected to.</span></span> <span data-ttu-id="ae321-110">Regler för dessa NSG: er kan ibland står i konflikt med varandra och påverka nätverksanslutning för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ae321-110">Rules across these NSGs can sometimes conflict with each other and impact a VM's network connectivity.</span></span>  

<span data-ttu-id="ae321-111">Du kan visa alla hello effektiva säkerhetsregler från dina NSG: er som tillämpas på den Virtuella datorns nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="ae321-111">You can view all hello effective security rules from your NSGs, as applied on your VM's NICs.</span></span> <span data-ttu-id="ae321-112">Den här artikeln visar hur tootroubleshoot VM anslutningsproblem med dessa regler i hello Azure Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="ae321-112">This article shows how tootroubleshoot VM connectivity issues using these rules in hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="ae321-113">Om du inte är bekant med principerna för VNet och NSG läsa hello [för virtuella nätverk](virtual-networks-overview.md) och [Nätverkssäkerhetsgrupper](virtual-networks-nsg.md) översikt artiklar.</span><span class="sxs-lookup"><span data-stu-id="ae321-113">If you're not familiar with VNet and NSG concepts, read hello [Virtual network](virtual-networks-overview.md) and [Network security groups](virtual-networks-nsg.md) overview articles.</span></span>

## <a name="using-effective-security-rules-tootroubleshoot-vm-traffic-flow"></a><span data-ttu-id="ae321-114">Med hjälp av effektiva säkerhetsregler tootroubleshoot VM trafikflöde</span><span class="sxs-lookup"><span data-stu-id="ae321-114">Using Effective Security Rules tootroubleshoot VM traffic flow</span></span>
<span data-ttu-id="ae321-115">hello-scenariot som följer är ett exempel på ett vanligt anslutningsproblem:</span><span class="sxs-lookup"><span data-stu-id="ae321-115">hello scenario that follows is an example of a common connection problem:</span></span>

<span data-ttu-id="ae321-116">En virtuell dator med namnet *VM1* är en del av ett undernät med namnet *Undernät1* inom ett VNet med namnet *WestUS VNet1*.</span><span class="sxs-lookup"><span data-stu-id="ae321-116">A VM named *VM1* is part of a subnet named *Subnet1* within a VNet named *WestUS-VNet1*.</span></span> <span data-ttu-id="ae321-117">Ett försök tooconnect toohello VM som använder RDP över TCP-port 3389 misslyckas.</span><span class="sxs-lookup"><span data-stu-id="ae321-117">An attempt tooconnect toohello VM using RDP over TCP port 3389 fails.</span></span> <span data-ttu-id="ae321-118">NSG: er som tillämpas på båda hello NIC *VM1 NIC1* och hello undernät *Undernät1*.</span><span class="sxs-lookup"><span data-stu-id="ae321-118">NSGs are applied at both hello NIC *VM1-NIC1* and hello subnet *Subnet1*.</span></span> <span data-ttu-id="ae321-119">Trafik tooTCP port 3389 tillåts i hello NSG som är kopplad till nätverksgränssnittet hello *VM1 NIC1*, men TCP pinga tooVM1's port 3389 misslyckas.</span><span class="sxs-lookup"><span data-stu-id="ae321-119">Traffic tooTCP port 3389 is allowed in hello NSG associated with hello network interface *VM1-NIC1*, however TCP ping tooVM1's port 3389 fails.</span></span>

<span data-ttu-id="ae321-120">När det här exemplet används TCP-port 3389, hello följande steg kan vara används toodetermine inkommande och utgående anslutningsfel via alla portar.</span><span class="sxs-lookup"><span data-stu-id="ae321-120">While this example uses TCP port 3389, hello following steps can be used toodetermine inbound and outbound connection failures over any port.</span></span>

## <a name="detailed-troubleshooting-steps"></a><span data-ttu-id="ae321-121">Detaljerad felsökning</span><span class="sxs-lookup"><span data-stu-id="ae321-121">Detailed Troubleshooting Steps</span></span>
<span data-ttu-id="ae321-122">Fullständig hello följande steg tootroubleshoot NSG: er för en virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="ae321-122">Complete hello following steps tootroubleshoot NSGs for a VM:</span></span>

1. <span data-ttu-id="ae321-123">Starta en Azure PowerShell-sessionen och logga in tooAzure.</span><span class="sxs-lookup"><span data-stu-id="ae321-123">Start an Azure PowerShell session and login tooAzure.</span></span> <span data-ttu-id="ae321-124">Om du inte är bekant med Azure PowerShell, läsa hello [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) artikel.</span><span class="sxs-lookup"><span data-stu-id="ae321-124">If you're not familiar with using Azure PowerShell, read hello [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="ae321-125">Ange följande kommando tooreturn alla NSG-regler tillämpas tooa nätverkskort med namnet hello *VM1 NIC1* i hello resursgruppen *RG1*:</span><span class="sxs-lookup"><span data-stu-id="ae321-125">Enter hello following command tooreturn all NSG rules applied tooa NIC named *VM1-NIC1* in hello resource group *RG1*:</span></span>
   
        Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [!TIP]
   > <span data-ttu-id="ae321-126">Om du inte vet hello namnet på ett nätverkskort, ange hello efter kommandot tooretrieve hello namnen på alla nätverkskort i en resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="ae321-126">If you don't know hello name of a NIC, enter hello following command tooretrieve hello names of all NICs in a resource group:</span></span> 
   > 
   > `Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name`
   > 
   > 
   
    <span data-ttu-id="ae321-127">hello följande är ett exempel på hello effektiva regler utdatan för hello *VM1 NIC1* NIC:</span><span class="sxs-lookup"><span data-stu-id="ae321-127">hello following text is a sample of hello effective rules output returned for hello *VM1-NIC1* NIC:</span></span>
   
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
   
    <span data-ttu-id="ae321-128">Observera följande information i hello utdata hello:</span><span class="sxs-lookup"><span data-stu-id="ae321-128">Note hello following information in hello output:</span></span>
   
   * <span data-ttu-id="ae321-129">Det finns två **NetworkSecurityGroup** avsnitt: en är associerad med ett undernät (*Undernät1*) och ett är kopplat till ett nätverkskort (*VM1 NIC1*).</span><span class="sxs-lookup"><span data-stu-id="ae321-129">There are two **NetworkSecurityGroup** sections: One is associated with a subnet (*Subnet1*) and one is associated with a NIC (*VM1-NIC1*).</span></span> <span data-ttu-id="ae321-130">I det här exemplet har en NSG tillämpad tooeach.</span><span class="sxs-lookup"><span data-stu-id="ae321-130">In this example, an NSG has been applied tooeach.</span></span>
   * <span data-ttu-id="ae321-131">**Associationen** visar hello resurs (undernät eller NIC) angivna NSG är associerad med.</span><span class="sxs-lookup"><span data-stu-id="ae321-131">**Association** shows hello resource (subnet or NIC) a given NSG is associated with.</span></span> <span data-ttu-id="ae321-132">Om hello NSG-resurs är flyttas/koppla bort omedelbart innan du kör det här kommandot kan eventuellt du toowait några sekunder för hello ändra tooreflect i hello kommandoutdata.</span><span class="sxs-lookup"><span data-stu-id="ae321-132">If hello NSG resource is moved/disassociated immediately before running this command, you may need toowait a few seconds for hello change tooreflect in hello command output.</span></span> 
   * <span data-ttu-id="ae321-133">Hej namn som inleds med *defaultSecurityRules*: när en NSG skapas, skapas flera standardregler för säkerhet i den.</span><span class="sxs-lookup"><span data-stu-id="ae321-133">hello rule names that are prefaced with *defaultSecurityRules*: When an NSG is created, several default security rules are created within it.</span></span> <span data-ttu-id="ae321-134">Standardreglerna kan inte tas bort, men de kan åsidosättas med högre Prioritetsregler.</span><span class="sxs-lookup"><span data-stu-id="ae321-134">Default rules can't be removed, but they can be overridden with higher priority rules.</span></span>
     <span data-ttu-id="ae321-135">Läs hello [NSG översikt](virtual-networks-nsg.md#default-rules) artikel toolearn mer information om NSG standard säkerhetsregler.</span><span class="sxs-lookup"><span data-stu-id="ae321-135">Read hello [NSG overview](virtual-networks-nsg.md#default-rules) article toolearn more about NSG default security rules.</span></span>
   * <span data-ttu-id="ae321-136">**ExpandedAddressPrefix** expanderar hello adressprefix för NSG standardtaggar.</span><span class="sxs-lookup"><span data-stu-id="ae321-136">**ExpandedAddressPrefix** expands hello address prefixes for NSG default tags.</span></span> <span data-ttu-id="ae321-137">Taggar representerar flera adressprefix.</span><span class="sxs-lookup"><span data-stu-id="ae321-137">Tags represent multiple address prefixes.</span></span> <span data-ttu-id="ae321-138">Utökningen av hello taggar kan vara användbart när du felsöker VM anslutning till eller från specifika adressprefix.</span><span class="sxs-lookup"><span data-stu-id="ae321-138">Expansion of hello tags can be useful when troubleshooting VM connectivity to/from specific address prefixes.</span></span> <span data-ttu-id="ae321-139">Till exempel utökar VIRTUAL_NETWORK taggen med VNET-peering tooshow peerkoppla VNet-prefix i hello tidigare utdata.</span><span class="sxs-lookup"><span data-stu-id="ae321-139">For example, with VNET peering, VIRTUAL_NETWORK tag expands tooshow peered VNet prefixes in hello previous output.</span></span>
     
     > [!NOTE]
     > <span data-ttu-id="ae321-140">Hej endast visar effektiva kommandoregler om en NSG är associerad med ett undernät och/eller ett nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="ae321-140">hello command only shows effective rules if an NSG is associated with either a subnet, a NIC, or both.</span></span> <span data-ttu-id="ae321-141">En virtuell dator kan ha flera nätverkskort med olika NSG: er som används.</span><span class="sxs-lookup"><span data-stu-id="ae321-141">A VM may have multiple NICs with different NSGs applied.</span></span> <span data-ttu-id="ae321-142">När du felsöker kan köra hello-kommando för varje nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="ae321-142">When troubleshooting, run hello command for each NIC.</span></span>
     > 
     > 
3. <span data-ttu-id="ae321-143">tooease filtrering över större antal NSG-regler, ange följande kommandon tootroubleshoot ytterligare hello:</span><span class="sxs-lookup"><span data-stu-id="ae321-143">tooease filtering over larger number of NSG rules, enter hello following commands tootroubleshoot further:</span></span> 
   
        $NSGs = Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
        $NSGs.EffectiveSecurityRules | Sort-Object Direction, Access, Priority | Out-GridView
   
    <span data-ttu-id="ae321-144">Ett filter för RDP-trafik (TCP-port 3389), är tillämpas toohello rutnätsvy som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="ae321-144">A filter for RDP traffic (TCP port 3389), is applied toohello grid view, as shown in hello following picture:</span></span>
   
    ![Regellistan](./media/virtual-network-nsg-troubleshoot-powershell/rules.png)
4. <span data-ttu-id="ae321-146">Som du ser i hello rutnätsvy finns både tillåta och neka regler för RDP.</span><span class="sxs-lookup"><span data-stu-id="ae321-146">As you can see in hello grid view, there are both allow and deny rules for RDP.</span></span> <span data-ttu-id="ae321-147">hello utdata från steg 2 visar den hello *DenyRDP* regeln är i hello NSG tillämpas toohello undernät.</span><span class="sxs-lookup"><span data-stu-id="ae321-147">hello output from step 2 shows that hello *DenyRDP* rule is in hello NSG applied toohello subnet.</span></span> <span data-ttu-id="ae321-148">Regler för inkommande trafik NSG: er används toohello undernät bearbetas först.</span><span class="sxs-lookup"><span data-stu-id="ae321-148">For inbound rules, NSGs applied toohello subnet are processed first.</span></span> <span data-ttu-id="ae321-149">Om en matchning hittas bearbetas hello NSG tillämpas toohello nätverksgränssnittet inte.</span><span class="sxs-lookup"><span data-stu-id="ae321-149">If a match is found, hello NSG applied toohello network interface is not processed.</span></span> <span data-ttu-id="ae321-150">I det här fallet hello *DenyRDP* regeln från hello undernät blockerar RDP toohello VM (**VM1**).</span><span class="sxs-lookup"><span data-stu-id="ae321-150">In this case, hello *DenyRDP* rule from hello subnet blocks RDP toohello VM (**VM1**).</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="ae321-151">En virtuell dator kan ha flera nätverkskort anslutna tooit.</span><span class="sxs-lookup"><span data-stu-id="ae321-151">A VM may have multiple NICs attached tooit.</span></span> <span data-ttu-id="ae321-152">Varje kanske anslutna tooa olika undernät.</span><span class="sxs-lookup"><span data-stu-id="ae321-152">Each may be connected tooa different subnet.</span></span> <span data-ttu-id="ae321-153">Eftersom hello kommandon i föregående steg i hello körs mot ett nätverkskort, är det viktigt tooensure som du anger hello NIC som du har med hello anslutningsfel till.</span><span class="sxs-lookup"><span data-stu-id="ae321-153">Since hello commands in hello previous steps are run against a NIC, it's important tooensure that you specify hello NIC you're having hello connectivity failure to.</span></span> <span data-ttu-id="ae321-154">Om du inte är säker på att köra du alltid hello kommandon mot varje nätverkskort anslutet toohello VM.</span><span class="sxs-lookup"><span data-stu-id="ae321-154">If you're not sure, you can always run hello commands against each NIC attached toohello VM.</span></span>
   > 
   > 
5. <span data-ttu-id="ae321-155">tooRDP i VM1, ändra hello *neka RDP (port 3389)* regel för*Tillåt RDP(3389)* i hello **Undernät1 NSG** NSG.</span><span class="sxs-lookup"><span data-stu-id="ae321-155">tooRDP into VM1, change hello *Deny RDP (3389)* rule too*Allow RDP(3389)* in hello **Subnet1-NSG** NSG.</span></span> <span data-ttu-id="ae321-156">Kontrollera att TCP-port 3389 är öppen genom att öppna en RDP-anslutning toohello VM eller hello PsPing verktyget.</span><span class="sxs-lookup"><span data-stu-id="ae321-156">Confirm that TCP port 3389 is open by opening an RDP connection toohello VM or using hello PsPing tool.</span></span> <span data-ttu-id="ae321-157">Du kan lära dig mer om PsPing genom att läsa hello [PsPing hämtningssidan](https://technet.microsoft.com/sysinternals/psping.aspx)</span><span class="sxs-lookup"><span data-stu-id="ae321-157">You can learn more about PsPing by reading hello [PsPing download page](https://technet.microsoft.com/sysinternals/psping.aspx)</span></span>
   
    <span data-ttu-id="ae321-158">Du kan eller ta bort regler från en NSG med hello information i hello utdata från hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ae321-158">You can or remove rules from an NSG by using hello information in hello output from hello following command:</span></span>
   
        Get-Help *-AzureRmNetworkSecurityRuleConfig

## <a name="considerations"></a><span data-ttu-id="ae321-159">Överväganden</span><span class="sxs-lookup"><span data-stu-id="ae321-159">Considerations</span></span>
<span data-ttu-id="ae321-160">Överväg följande punkter när du felsöker problem med nätverksanslutningen hello:</span><span class="sxs-lookup"><span data-stu-id="ae321-160">Consider hello following points when troubleshooting connectivity problems:</span></span>

* <span data-ttu-id="ae321-161">Standard NSG-regler som blockerar inkommande åtkomst från hello internet och bara bevilja VNet inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="ae321-161">Default NSG rules will block inbound access from hello internet and only permit VNet inbound traffic.</span></span> <span data-ttu-id="ae321-162">Regler som uttryckligen läggas tooallow inkommande åtkomst från Internet, vid behov.</span><span class="sxs-lookup"><span data-stu-id="ae321-162">Rules should be explicitly added tooallow inbound access from Internet, as required.</span></span>
* <span data-ttu-id="ae321-163">Om det finns inga NSG säkerhetsregler som orsakar en virtuell dators network connectivity toofail, kan hello problemet bero på följande:</span><span class="sxs-lookup"><span data-stu-id="ae321-163">If there are no NSG security rules causing a VM’s network connectivity toofail, hello problem may be due to:</span></span>
  * <span data-ttu-id="ae321-164">Brandväggsprogram körs i hello VM-operativsystem</span><span class="sxs-lookup"><span data-stu-id="ae321-164">Firewall software running within hello VM's operating system</span></span>
  * <span data-ttu-id="ae321-165">Vägar som konfigurerats för virtuella installationer eller lokal trafik.</span><span class="sxs-lookup"><span data-stu-id="ae321-165">Routes configured for virtual appliances or on-premises traffic.</span></span> <span data-ttu-id="ae321-166">Internet-trafiken kan vara omdirigerade tooon lokala via Tvingad tunneltrafik.</span><span class="sxs-lookup"><span data-stu-id="ae321-166">Internet traffic can be redirected tooon-premises via forced-tunneling.</span></span> <span data-ttu-id="ae321-167">En RDP/SSH-anslutning från hello Internet tooyour VM kanske inte fungerar med den här inställningen, beroende på hur hello lokalt nätverksmaskinvara hanterar den här trafiken.</span><span class="sxs-lookup"><span data-stu-id="ae321-167">An RDP/SSH connection from hello Internet tooyour VM may not work with this setting, depending on how hello on-premises network hardware handles this traffic.</span></span> <span data-ttu-id="ae321-168">Läs hello [felsökning vägar](virtual-network-routes-troubleshoot-powershell.md) artikel toolearn hur toodiagnose väg problem som kan hindra hello trafikflödet i och ut ur hello VM.</span><span class="sxs-lookup"><span data-stu-id="ae321-168">Read hello [Troubleshooting Routes](virtual-network-routes-troubleshoot-powershell.md) article toolearn how toodiagnose route problems that may be impeding hello flow of traffic in and out of hello VM.</span></span> 
* <span data-ttu-id="ae321-169">Om du har peerkoppla Vnet, som standard, expandera hello VIRTUAL_NETWORK taggen automatiskt tooinclude prefix för peerkoppla Vnet.</span><span class="sxs-lookup"><span data-stu-id="ae321-169">If you have peered VNets, by default, hello VIRTUAL_NETWORK tag will automatically expand tooinclude prefixes for peered VNets.</span></span> <span data-ttu-id="ae321-170">Du kan visa dessa prefix i hello **ExpandedAddressPrefix** lista, tootroubleshoot alla problem relaterade tooVNet peering anslutning.</span><span class="sxs-lookup"><span data-stu-id="ae321-170">You can view these prefixes in hello **ExpandedAddressPrefix** list, tootroubleshoot any issues related tooVNet peering connectivity.</span></span> 
* <span data-ttu-id="ae321-171">Effektiva säkerhetsregler visas bara om det finns en NSG som är associerade med hello Virtuella nätverkskort och undernät.</span><span class="sxs-lookup"><span data-stu-id="ae321-171">Effective security rules are only shown if there is an NSG associated with hello VM’s NIC and or subnet.</span></span> 
* <span data-ttu-id="ae321-172">Om det finns inga NSG: er som är associerade med hello NIC eller undernät och att du har en offentlig IP-adress som tilldelats tooyour VM, vara alla portar öppna för inkommande och utgående åtkomst.</span><span class="sxs-lookup"><span data-stu-id="ae321-172">If there are no NSGs associated with hello NIC or subnet and you have a public IP address assigned tooyour VM, all ports will be open for inbound and outbound access.</span></span> <span data-ttu-id="ae321-173">Om hello VM har en offentlig IP-adress, rekommenderas tillämpa NSG: er toohello NIC eller undernät.</span><span class="sxs-lookup"><span data-stu-id="ae321-173">If hello VM has a public IP address, applying NSGs toohello NIC or subnet is strongly recommended.</span></span>  

