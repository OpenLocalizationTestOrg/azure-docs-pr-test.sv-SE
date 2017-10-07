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
# <a name="troubleshoot-network-security-groups-using-azure-powershell"></a>Felsöka Nätverkssäkerhetsgrupper med hjälp av Azure PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-nsg-troubleshoot-portal.md)
> * [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
> 
> 

Om du konfigurerade Nätverkssäkerhetsgrupper (NSG: er) på den virtuella datorn (VM) och har problem med anslutningen VM, för den här artikeln innehåller en översikt över diagnostikfunktionerna för NSG: er toohelp fortsätta felsökningen.

NSG: er kan du toocontrol hello typer av trafik som flöde till och från virtuella datorer (VM). NSG: er kan vara tillämpade toosubnets i ett Azure Virtual Network (VNet), nätverksgränssnitt (NIC) eller båda. hello effektiva reglerna tooa NIC är en sammanställning av hello regler som finns i hello NSG: er används tooa NIC och hello undernät, det är anslutet till. Regler för dessa NSG: er kan ibland står i konflikt med varandra och påverka nätverksanslutning för en virtuell dator.  

Du kan visa alla hello effektiva säkerhetsregler från dina NSG: er som tillämpas på den Virtuella datorns nätverkskort. Den här artikeln visar hur tootroubleshoot VM anslutningsproblem med dessa regler i hello Azure Resource Manager-distributionsmodellen. Om du inte är bekant med principerna för VNet och NSG läsa hello [för virtuella nätverk](virtual-networks-overview.md) och [Nätverkssäkerhetsgrupper](virtual-networks-nsg.md) översikt artiklar.

## <a name="using-effective-security-rules-tootroubleshoot-vm-traffic-flow"></a>Med hjälp av effektiva säkerhetsregler tootroubleshoot VM trafikflöde
hello-scenariot som följer är ett exempel på ett vanligt anslutningsproblem:

En virtuell dator med namnet *VM1* är en del av ett undernät med namnet *Undernät1* inom ett VNet med namnet *WestUS VNet1*. Ett försök tooconnect toohello VM som använder RDP över TCP-port 3389 misslyckas. NSG: er som tillämpas på båda hello NIC *VM1 NIC1* och hello undernät *Undernät1*. Trafik tooTCP port 3389 tillåts i hello NSG som är kopplad till nätverksgränssnittet hello *VM1 NIC1*, men TCP pinga tooVM1's port 3389 misslyckas.

När det här exemplet används TCP-port 3389, hello följande steg kan vara används toodetermine inkommande och utgående anslutningsfel via alla portar.

## <a name="detailed-troubleshooting-steps"></a>Detaljerad felsökning
Fullständig hello följande steg tootroubleshoot NSG: er för en virtuell dator:

1. Starta en Azure PowerShell-sessionen och logga in tooAzure. Om du inte är bekant med Azure PowerShell, läsa hello [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) artikel.
2. Ange följande kommando tooreturn alla NSG-regler tillämpas tooa nätverkskort med namnet hello *VM1 NIC1* i hello resursgruppen *RG1*:
   
        Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
   
   > [!TIP]
   > Om du inte vet hello namnet på ett nätverkskort, ange hello efter kommandot tooretrieve hello namnen på alla nätverkskort i en resursgrupp: 
   > 
   > `Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name`
   > 
   > 
   
    hello följande är ett exempel på hello effektiva regler utdatan för hello *VM1 NIC1* NIC:
   
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
   
    Observera följande information i hello utdata hello:
   
   * Det finns två **NetworkSecurityGroup** avsnitt: en är associerad med ett undernät (*Undernät1*) och ett är kopplat till ett nätverkskort (*VM1 NIC1*). I det här exemplet har en NSG tillämpad tooeach.
   * **Associationen** visar hello resurs (undernät eller NIC) angivna NSG är associerad med. Om hello NSG-resurs är flyttas/koppla bort omedelbart innan du kör det här kommandot kan eventuellt du toowait några sekunder för hello ändra tooreflect i hello kommandoutdata. 
   * Hej namn som inleds med *defaultSecurityRules*: när en NSG skapas, skapas flera standardregler för säkerhet i den. Standardreglerna kan inte tas bort, men de kan åsidosättas med högre Prioritetsregler.
     Läs hello [NSG översikt](virtual-networks-nsg.md#default-rules) artikel toolearn mer information om NSG standard säkerhetsregler.
   * **ExpandedAddressPrefix** expanderar hello adressprefix för NSG standardtaggar. Taggar representerar flera adressprefix. Utökningen av hello taggar kan vara användbart när du felsöker VM anslutning till eller från specifika adressprefix. Till exempel utökar VIRTUAL_NETWORK taggen med VNET-peering tooshow peerkoppla VNet-prefix i hello tidigare utdata.
     
     > [!NOTE]
     > Hej endast visar effektiva kommandoregler om en NSG är associerad med ett undernät och/eller ett nätverkskort. En virtuell dator kan ha flera nätverkskort med olika NSG: er som används. När du felsöker kan köra hello-kommando för varje nätverkskort.
     > 
     > 
3. tooease filtrering över större antal NSG-regler, ange följande kommandon tootroubleshoot ytterligare hello: 
   
        $NSGs = Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
        $NSGs.EffectiveSecurityRules | Sort-Object Direction, Access, Priority | Out-GridView
   
    Ett filter för RDP-trafik (TCP-port 3389), är tillämpas toohello rutnätsvy som visas i följande bild hello:
   
    ![Regellistan](./media/virtual-network-nsg-troubleshoot-powershell/rules.png)
4. Som du ser i hello rutnätsvy finns både tillåta och neka regler för RDP. hello utdata från steg 2 visar den hello *DenyRDP* regeln är i hello NSG tillämpas toohello undernät. Regler för inkommande trafik NSG: er används toohello undernät bearbetas först. Om en matchning hittas bearbetas hello NSG tillämpas toohello nätverksgränssnittet inte. I det här fallet hello *DenyRDP* regeln från hello undernät blockerar RDP toohello VM (**VM1**).
   
   > [!NOTE]
   > En virtuell dator kan ha flera nätverkskort anslutna tooit. Varje kanske anslutna tooa olika undernät. Eftersom hello kommandon i föregående steg i hello körs mot ett nätverkskort, är det viktigt tooensure som du anger hello NIC som du har med hello anslutningsfel till. Om du inte är säker på att köra du alltid hello kommandon mot varje nätverkskort anslutet toohello VM.
   > 
   > 
5. tooRDP i VM1, ändra hello *neka RDP (port 3389)* regel för*Tillåt RDP(3389)* i hello **Undernät1 NSG** NSG. Kontrollera att TCP-port 3389 är öppen genom att öppna en RDP-anslutning toohello VM eller hello PsPing verktyget. Du kan lära dig mer om PsPing genom att läsa hello [PsPing hämtningssidan](https://technet.microsoft.com/sysinternals/psping.aspx)
   
    Du kan eller ta bort regler från en NSG med hello information i hello utdata från hello följande kommando:
   
        Get-Help *-AzureRmNetworkSecurityRuleConfig

## <a name="considerations"></a>Överväganden
Överväg följande punkter när du felsöker problem med nätverksanslutningen hello:

* Standard NSG-regler som blockerar inkommande åtkomst från hello internet och bara bevilja VNet inkommande trafik. Regler som uttryckligen läggas tooallow inkommande åtkomst från Internet, vid behov.
* Om det finns inga NSG säkerhetsregler som orsakar en virtuell dators network connectivity toofail, kan hello problemet bero på följande:
  * Brandväggsprogram körs i hello VM-operativsystem
  * Vägar som konfigurerats för virtuella installationer eller lokal trafik. Internet-trafiken kan vara omdirigerade tooon lokala via Tvingad tunneltrafik. En RDP/SSH-anslutning från hello Internet tooyour VM kanske inte fungerar med den här inställningen, beroende på hur hello lokalt nätverksmaskinvara hanterar den här trafiken. Läs hello [felsökning vägar](virtual-network-routes-troubleshoot-powershell.md) artikel toolearn hur toodiagnose väg problem som kan hindra hello trafikflödet i och ut ur hello VM. 
* Om du har peerkoppla Vnet, som standard, expandera hello VIRTUAL_NETWORK taggen automatiskt tooinclude prefix för peerkoppla Vnet. Du kan visa dessa prefix i hello **ExpandedAddressPrefix** lista, tootroubleshoot alla problem relaterade tooVNet peering anslutning. 
* Effektiva säkerhetsregler visas bara om det finns en NSG som är associerade med hello Virtuella nätverkskort och undernät. 
* Om det finns inga NSG: er som är associerade med hello NIC eller undernät och att du har en offentlig IP-adress som tilldelats tooyour VM, vara alla portar öppna för inkommande och utgående åtkomst. Om hello VM har en offentlig IP-adress, rekommenderas tillämpa NSG: er toohello NIC eller undernät.  

