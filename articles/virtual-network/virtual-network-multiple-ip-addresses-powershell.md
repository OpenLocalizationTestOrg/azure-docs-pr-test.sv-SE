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
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-powershell"></a>Tilldela flera IP-adresser toovirtual datorer med hjälp av PowerShell

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Den här artikeln förklarar hur toocreate en virtuell dator (VM) via hello Azure Resource Manager distribution modellen med hjälp av PowerShell. Flera IP-adresser kan inte tilldelas tooresources som skapats via hello klassiska distributionsmodellen. Mer om Azure distributionsmodeller läsa hello toolearn [förstår distributionsmodellerna](../resource-manager-deployment-model.md) artikel.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Skapa en virtuell dator med flera IP-adresser

hello steg som följer beskrivs hur toocreate exempel VM med flera IP-adresser, enligt beskrivningen i hello scenario. Ändra variabelvärden som krävs för din implementering.

1. Öppna ett PowerShell-Kommandotolken och fullständig hello återstående stegen i det här avsnittet i en enda PowerShell-session. Om du inte redan har PowerShell installeras och konfigureras, fullständig hello stegen i hello [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) artikel.
2. Inloggningskonto tooyour med hello `login-azurermaccount` kommando.
3. Ersätt *myResourceGroup* och *westus* med ett namn och plats. Skapa en resursgrupp. En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.

    ```powershell
    $RgName   = "MyResourceGroup"
    $Location = "westus"

    New-AzureRmResourceGroup `
    -Name $RgName `
    -Location $Location
    ```

4. Skapa ett virtuellt nätverk (VNet) och undernät i hello samma plats som hello resursgrupp:

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

5. Skapa en nätverkssäkerhetsgrupp (NSG) och en regel. hello NSG skyddar hello VM använda regler för inkommande och utgående. I detta fall skapas en regel för inkommande trafik för port 3389, som tillåter inkommande anslutningar till fjärrskrivbord.

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

6. Definiera hello primära IP-konfiguration för hello NIC. Ändra 10.0.0.4 tooa giltig adress i hello undernät som du har skapat, om du inte använde hello-värdet som definierats tidigare. Innan du tilldelar en statisk IP-adress, rekommenderas det att du först bekräfta att den inte är redan används. Ange hello kommando `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.4 -VirtualNetwork $VNet`. Om hello-adress är tillgänglig hello utdata returnerar *SANT*. Om den inte är tillgänglig hello utdata returnerar *FALSKT* och en lista med adresser som är tillgängliga. 

    I följande kommandon, hello **Ersätt < Ersätt-med-your-unikt-name > med hello unikt DNS-namnet toouse.** hello-namnet måste vara unikt inom alla offentliga IP-adresser inom en Azure-region. Det här är en valfri parameter. Det kan tas bort om du bara vill tooconnect toohello VM med hello offentlig IP-adress.

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

    När du tilldelar flera IP-konfigurationer tooa NIC det måste tilldelas en konfiguration som hello *-primära*.

    > [!NOTE]
    > Offentliga IP-adresser har en låg kostnad. Mer om IP-priser, toolearn läsa hello [IP-adress priser](https://azure.microsoft.com/pricing/details/ip-addresses) sidan. Det finns en gräns toohello antalet offentliga IP-adresser som kan användas i en prenumeration. Mer om hello gränser, läsa hello toolearn [Azure begränsar](../azure-subscription-service-limits.md#networking-limits) artikel.

7. Definiera hello sekundär IP-konfigurationer för hello NIC. Du kan lägga till eller ta bort konfigurationer efter behov. Varje IP-adresskonfigurationen måste ha en privat IP-adress. Varje konfiguration kan du ha en offentlig IP-adress.

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

8. Skapa hello NIC och associera hello tre IP-konfigurationer tooit:

    ```powershell
    
    $NIC = New-AzureRmNetworkInterface `
    -Name MyNIC `
    -ResourceGroupName $RgName `
    -Location $Location `
    -NetworkSecurityGroupId $NSG.Id `
    -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3
    ```

    >[!NOTE]
    >Även om alla konfigurationer tilldelas tooone nätverkskort i den här artikeln kan tilldela du flera IP-konfigurationer tooevery nätverkskortet som sitter toohello VM. toolearn hur toocreate en virtuell dator med flera nätverkskort, läsa hello [skapa en virtuell dator med flera nätverkskort](virtual-network-deploy-multinic-arm-ps.md) artikel.

9. Skapa hello VM genom att ange hello följande kommandon:

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

10. Lägg till hello privata IP-adresser toohello VM operativsystem genom att följa steg hello för operativsystemet i hello [lägga till IP-adresser tooa VM operativsystemet](#os-config) i den här artikeln. Lägg inte till hello offentliga IP-adresser toohello operativsystem.

## <a name="add"></a>Lägg till IP-adresser tooa VM

Du kan lägga till privata och offentliga IP-adresser tooa NIC hello steg som följer. hello exemplen i följande avsnitt hello förutsätter att du redan har en virtuell dator med hello tre IP-konfigurationer som beskrivs i hello [scenariot](#Scenario) i den här artikeln, men det är inte nödvändigt att du gör.

1. Öppna ett PowerShell-Kommandotolken och fullständig hello återstående stegen i det här avsnittet i en enda PowerShell-session. Om du inte redan har PowerShell installeras och konfigureras, fullständig hello stegen i hello [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) artikel.
2. Ändra hello ”värden” Hej $Variables toohello namnet på hello NIC som du vill tooadd IP-adress tooand hello resurs grupp och plats hello NIC finns i följande:

    ```powershell
    $NicName  = "MyNIC"
    $RgName   = "MyResourceGroup"
    $Location = "westus"
    ```

    Om du inte vet hello namnet på hello NIC som du vill toochange ange hello följande kommandon och sedan ändra hello värdena för variabler som hello tidigare:

    ```powershell
    Get-AzureRmNetworkInterface | Format-Table Name, ResourceGroupName, Location
    ```
3. Skapa en variabel och ange den toohello befintliga nätverkskort genom att skriva följande kommando hello:

    ```powershell
    $MyNIC = Get-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName
    ```
4. Ändra i hello följande kommandon, *MyVNet* och *MySubnet* toohello namnen på hello VNet och undernät hello nätverkskortet är anslutet till. Ange hello kommandon tooretrieve hello VNet och undernät objekt hello nätverkskortet är anslutet till:

    ```powershell
    $MyVNet = Get-AzureRMVirtualnetwork -Name MyVNet -ResourceGroupName $RgName
    $Subnet = $MyVnet.Subnets | Where-Object { $_.Name -eq "MySubnet" }
    ```
    Om du inte vet hello VNet eller undernät namn hello nätverkskortet är anslutet till, ange hello följande kommando:
    ```powershell
    $MyNIC.IpConfigurations
    ```
    Hello utdata och leta efter texten liknande toohello följande exempel på utdata:
    
    ```
    "Id": "/subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/MyVNet/subnets/MySubnet"
    ```
    I det här resultatet *MyVnet* är hello VNet och *MySubnet* är hello undernät hello nätverkskortet är anslutet till.

5. Slutför hello stegen i ett av följande avsnitt, baserat på dina krav hello:

    **Lägg till en privat IP-adress**

    tooadd en privat IP-adress tooa NIC, måste du skapa en IP-konfiguration. hello följande kommando skapar en konfiguration med en statisk IP-adress 10.0.0.7. När du anger en statisk IP-adress, måste den vara en oanvända adress för hello undernät. Vi rekommenderar att du först testar hello adress tooensure är tillgänglig genom att ange hello `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.7 -VirtualNetwork $myVnet` kommando. Om hello IP-adress är tillgänglig hello utdata returnerar *SANT*. Om den inte är tillgänglig hello utdata returnerar *FALSKT*, och en lista med adresser som är tillgängliga.

    ```powershell
    Add-AzureRmNetworkInterfaceIpConfig -Name IPConfig-4 -NetworkInterface `
    $MyNIC -Subnet $Subnet -PrivateIpAddress 10.0.0.7
    ```
    Skapa så många konfigurationer som du vill använda unika konfigurationsnamn och privata IP-adresser (för konfigurationer med statiska IP-adresser).

    Lägg till hello privata IP-adress toohello VM operativsystem genom att följa steg hello för operativsystemet i hello [lägga till IP-adresser tooa VM operativsystemet](#os-config) i den här artikeln.

    **Lägg till en offentlig IP-adress**

    En offentlig IP-adress har lagts till genom att associera en offentlig IP-adress resurs tooeither en ny IP-konfiguration eller en befintlig IP-konfiguration. Slutför hello stegen i ett av hello avsnitten som följer, som du behöver.

    > [!NOTE]
    > Offentliga IP-adresser har en låg kostnad. Mer om IP-priser, toolearn läsa hello [IP-adress priser](https://azure.microsoft.com/pricing/details/ip-addresses) sidan. Det finns en gräns toohello antalet offentliga IP-adresser som kan användas i en prenumeration. Mer om hello gränser, läsa hello toolearn [Azure begränsar](../azure-subscription-service-limits.md#networking-limits) artikel.
    >

    - **Associera hello offentliga IP-adress resurs tooa nya IP-konfiguration**
    
        När du lägger till en offentlig IP-adress i en ny IP-konfiguration måste du också lägga en privat IP-adress, eftersom alla IP-konfigurationer måste ha en privat IP-adress. Du kan lägga till en befintlig offentlig IP-adressresurs eller skapa en ny. toocreate en ny ange hello följande kommando:
    
        ```powershell
        $myPublicIp3 = New-AzureRmPublicIpAddress `
        -Name "myPublicIp3" `
        -ResourceGroupName $RgName `
        -Location $Location `
        -AllocationMethod Static
        ```

        toocreate en ny IP-konfiguration med en statisk privat IP-adress och hello associerade *myPublicIp3* offentlig IP adress resursen måste du ange hello följande kommando:

        ```powershell
        Add-AzureRmNetworkInterfaceIpConfig `
        -Name IPConfig-4 `
        -NetworkInterface $myNIC `
        -Subnet $Subnet `
        -PrivateIpAddress 10.0.0.7 `
        -PublicIpAddress $myPublicIp3
        ```

    - **Associera hello offentliga IP-adress resurs tooan befintliga IP-konfiguration**

        En offentlig IP-adressresurs kan bara vara associerad tooan IP-konfiguration som inte redan har en associerad. Du kan avgöra om en IP-konfiguration har en tillhörande offentliga IP-adress genom att ange hello följande kommando:

        ```powershell
        $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
        ```

        Du kan se utdata liknande toohello följande:

        ```     
        Name       PrivateIpAddress PublicIpAddress                                           Primary
        
        IPConfig-1 10.0.0.4         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress    True
        IPConfig-2 10.0.0.5         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress   False
        IpConfig-3 10.0.0.6                                                                     False
        ```

        Eftersom hello **PublicIpAddress** för *IpConfig-3* är tomt, inga offentliga IP-adressresurs är för närvarande associerad tooit. Du kan lägga till en befintlig offentlig IP-adress resurs tooIpConfig-3 eller ange följande kommando toocreate en hello:

        ```powershell
        $MyPublicIp3 = New-AzureRmPublicIpAddress `
        -Name "MyPublicIp3" `
        -ResourceGroupName $RgName `
        -Location $Location -AllocationMethod Static
        ```

        Ange följande kommando tooassociate hello offentliga IP-adressen resurs toohello befintliga IP-konfigurationen med namnet hello *IpConfig-3*:
    
        ```powershell
        Set-AzureRmNetworkInterfaceIpConfig `
        -Name IpConfig-3 `
        -NetworkInterface $mynic `
        -Subnet $Subnet `
        -PublicIpAddress $myPublicIp3
        ```

6. Ange hello NIC med hello nya IP-konfigurationen genom att ange hello följande kommando:

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $MyNIC
    ```

7. Visa hello privata IP-adresser och hello offentliga IP-adress resurser tilldelade toohello NIC genom att ange hello följande kommando:

    ```powershell   
    $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
    ```
8. Lägg till hello privata IP-adress toohello VM operativsystem genom att följa steg hello för operativsystemet i hello [lägga till IP-adresser tooa VM operativsystemet](#os-config) i den här artikeln. Lägg inte till hello offentliga IP-adress toohello operativsystem.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
