---
title: aaaCreate SQL Server-dator i Azure PowerShell (Resource Manager) | Microsoft Docs
description: "Innehåller steg och PowerShell-skript för att skapa en virtuell Azure-dator med SQL Server virtuella galleriavbildningar."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 98d50dd8-48ad-444f-9031-5378d8270d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/17/2017
ms.author: jroth
ms.openlocfilehash: 2b8cb8f69ff9894a95eab617816a60c8674eeefa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a>Etablera en virtuell dator med SQL Server med hjälp av Azure PowerShell (Resource Manager)
> [!div class="op_single_selector"]
> * [Portal](virtual-machines-windows-portal-sql-server-provision.md)
> * [PowerShell](virtual-machines-windows-ps-sql-create.md)
>
>

## <a name="overview"></a>Översikt
Den här kursen visar hur toocreate som en enda virtuell Azure-dator med hjälp av hello **Azure Resource Manager** distributionsmodellen med hjälp av Azure PowerShell-cmdlets. I den här självstudiekursen skapar vi en virtuell dator med en enskild disk från en avbildning i hello SQL-galleriet. Vi skapar nya providers för hello lagring, nätverk och beräkningsresurser som ska användas av hello virtuell dator. Om du har befintliga providers för någon av dessa resurser, kan du använda dessa providers i stället.

Om du behöver hello klassiska versionen av det här avsnittet, se [etablera en virtuell dator med SQL Server med hjälp av Azure PowerShell klassiska](../classic/ps-sql-create.md).

## <a name="prerequisites"></a>Krav
Den här kursen behöver du:

* En Azure-konto och prenumeration innan du börjar. Om du inte har någon registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).
* [Azure PowerShell)](/powershell/azure/overview), lägsta version av 1.4.0 eller senare (den här kursen som skrivits med version 1.5.0).
  * tooretrieve din version, typen **Azure Get-Module - ListAvailable**.

## <a name="configure-your-subscription"></a>Konfigurera din prenumeration
Öppna Windows PowerShell och upprätta åtkomst tooyour Azure-konto genom att köra följande cmdlet hello. Visas med en inloggning skärmen tooenter dina autentiseringsuppgifter. Använd hello samma e-post och lösenord du använder toosign i toohello Azure-portalen.

    Add-AzureRmAccount

När du har loggat in visas viss information på skärmen som inkluderar hello prenumerations-ID som du har loggat in. Detta är hello prenumeration där hello resurser för den här självstudiekursen kommer att skapas om du inte ändrar tooa annan prenumeration. Om du har flera SubscriptionIds, kör du följande cmdlet tooreturn hello en lista över alla dina SubscriptionIds:

    Get-AzureRmSubscription

toochange tooanother SubscriptionID, kör hello som följer med din önskade SubscriptionId.

    Select-AzureRmSubscription -SubscriptionId xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## <a name="define-image-variables"></a>Definiera variabler för avbildning
toosimplify användbarhet och förståelse av hello slutförts skriptet från den här självstudiekursen kommer vi starta genom att definiera ett antal variabler. Ändra hello parametervärden som du vill, men varning för namngivning begränsningar relaterade tooname längder och specialtecken när du ändrar hello värden.

### <a name="location-and-resource-group"></a>Plats och resursgruppen.
Använda två variabler toodefine hello data region och hello resursgrupp som du skapar hello andra resurser för hello virtuella datorn.

Ändra efter behov och kör följande cmdlets tooinitialize hello dessa variabler.

    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"

### <a name="storage-properties"></a>Lagringsegenskaper för
Använd hello följande variabler toodefine hello konto och hello lagringstyp för lagring toobe som används av hello virtuell dator.

Ändra efter behov och kör följande cmdlet tooinitialize hello dessa variabler. Observera att i det här exemplet använder [Premiumlagring](../../../storage/common/storage-premium-storage.md), som rekommenderas för produktionsarbetsbelastningar. Mer information om den här vägledningen och andra rekommendationer finns [prestandarelaterade Metodtips för SQL Server i Azure Virtual Machines](virtual-machines-windows-sql-performance.md).

    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

### <a name="network-properties"></a>Egenskaper för nätverk
Använd hello efter variabler toodefine hello nätverksgränssnitt, hello TCP/IP-allokeringsmetod, hello virtuella nätverksnamnet, hello virtuella undernätsnamn, hello intervall med IP-adresser för hello virtuella nätverk, hello intervall av IP-adresser för hello-undernät och hello den offentliga domänen namnet etikett toobe används av hello nätverk i hello virtuell dator.

Ändra efter behov och kör följande cmdlet tooinitialize hello dessa variabler.

    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $TCPIPAllocationMethod = "Dynamic"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $DomainName = "sqlvm1"

### <a name="virtual-machine-properties"></a>Egenskaper för virtuell dator
Använd hello följande variabler toodefine hello virtuellt datornamn, hello datornamn, hello storlek på virtuell dator och hello operativsystemet disknamnet för hello virtuella datorn.

Ändra efter behov och kör följande cmdlet tooinitialize hello dessa variabler.

    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

### <a name="image-properties"></a>Bildegenskaper
Använd hello följande variabler toodefine hello avbildningen toouse för hello virtuella datorn. I det här exemplet används hello SQL Server 2016 Enterprise bilden.

Ändra efter behov och kör följande cmdlet tooinitialize hello dessa variabler.

    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

Observera att du kan få en fullständig lista över SQL Server avbildningen erbjudanden med hello Get-AzureRmVMImageOffer kommando:

    Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'

Och du kan se hello SKU: er som är tillgängliga för ett erbjudande med hello Get-AzureRmVMImageSku kommando. hello följande kommando visar alla SKU: er för hello **SQL2014SP1 WS2012R2** erbjuder.

    Get-AzureRmVMImageSku -Location 'East US' -Publisher 'MicrosoftSQLServer' -Offer 'SQL2014SP1-WS2012R2' | Select Skus

## <a name="create-a-resource-group"></a>Skapa en resursgrupp
Med hello Resource Manager-distributionsmodellen är första hello-objekt som du skapar hello resursgruppen. Vi använder hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet toocreate en Azure-resursgrupp och dess resurser med hello resurs gruppera namn och plats som definierats av hello variabler som du tidigare har initierat.

Kör följande cmdlet toocreate hello din nya resursgrupp.

    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

## <a name="create-a-storage-account"></a>skapar ett lagringskonto
hello virtuell dator kräver lagringsresurser för hello operativsystemdisken och hello SQL Server-data och loggfiler. För enkelhetens skull skapar vi en enskild disk för båda. Du kan koppla ytterligare diskar senare med hello [Lägg till Azure-disken](/powershell/module/azure/add-azuredisk) cmdlet i ordning tooplace SQL Server-data och loggfiler på dedikerade diskar. Vi använder hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet toocreate en standardlagring konto i din nya resursgrupp och med hello lagringskontonamn, Sku namn och plats som definierats med hjälp av hello variabler som du tidigare har initierats.

Kör följande cmdlet toocreate hello det nya kontot.

    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

## <a name="create-network-resources"></a>Skapa nätverksresurser
hello virtuell dator kräver ett antal nätverksresurser för nätverksanslutningen.

* Varje virtuell dator kräver ett virtuellt nätverk.
* Ett virtuellt nätverk måste ha minst ett undernät som har definierats.
* Ett nätverksgränssnitt måste definieras med en offentlig eller privat IP-adress.

### <a name="create-a-virtual-network-subnet-configuration"></a>Skapa en konfiguration av undernät för virtuellt nätverk
Vi startar genom att skapa en konfiguration av undernät för våra virtuella nätverket. I våra självstudier ska vi skapar en standard-undernätet med hello [ny AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) cmdlet. Vi skapar våra undernätskonfiguration av virtuellt nätverk med hello namn och adress undernätsprefixet definieras med hjälp av hello variabler som du tidigare har initierat.

> [!NOTE]
> Du kan ange ytterligare egenskaper för hello virtuella undernät nätverkskonfigurationen med hjälp av denna cmdlet, men som ligger utanför hello i den här kursen.
>
>

Kör följande cmdlet toocreate hello konfigurationen av virtuellt undernät.

    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix

### <a name="create-a-virtual-network"></a>Skapa ett virtuellt nätverk
Därefter skapar vi vårt virtuellt nätverk med hello [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) cmdlet. Vi skapar vårt virtuella nätverk i din nya resursgrupp med hello namn, plats och adressprefixet definierats hello variabler som du tidigare har initierat, samt använder hello undernätskonfiguration som du definierade i hello föregående steg.

Kör följande cmdlet toocreate hello ditt virtuella nätverk.

    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig

### <a name="create-hello-public-ip-address"></a>Skapa hello offentlig IP-adress
Nu när vi har vårt virtuella nätverk som definierats måste tooconfigure en IP-adress för anslutning toohello virtuella datorn. Den här självstudiekursen skapar vi en offentlig IP-adress med hjälp av dynamisk IP-adressering toosupport Internet-anslutning. Vi använder hello [ny AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) cmdlet toocreate hello offentliga IP-adressen i hello resurs grupp skapat prevously och med hello namn, plats, fördelning och DNS-domännamnet anges med hjälp av hello variabler som du tidigare har initierat.

> [!NOTE]
> Du kan ange ytterligare egenskaper för hello offentliga IP-adressen som använder denna cmdlet, men som ligger utanför den här första självstudien hello. Du kan också skapa en privat adress eller en adress med en statisk adress, men som även är utanför hello omfånget för den här kursen.
>
>

Kör följande cmdlet toocreate hello din offentliga IP-adress.

    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName

### <a name="create-hello-network-interface"></a>Skapa hello nätverksgränssnittet
Vi är nu redo toocreate hello nätverksgränssnittet som våra virtuella datorn ska använda. Vi använder hello [ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) cmdlet toocreate våra nätverksgränssnitt i hello resursgrupp skapade tidigare och med hello namn, plats och undernät och offentliga IP-adressen har definierats.

Kör följande cmdlet toocreate hello nätverksgränssnittet.

    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

## <a name="configure-a-vm-object"></a>Konfigurera ett VM-objekt
Nu när vi har lagring och nätverksresurser som definierats är vi klara toodefine beräkningsresurser för hello virtuella datorn. I våra självstudier ska vi ange hello storlek på virtuell dator och egenskaper för olika operativsystem, ange hello nätverksgränssnitt som vi skapade tidigare, definiera blob-lagring och ange hello operativsystemdisken.

### <a name="create-hello-vm-object"></a>Skapa hello VM-objekt
Vi startar genom att ange hello storlek på virtuell dator. Den här självstudiekursen kommer specificerar vi en DS13. Vi använder hello [ny AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) cmdlet toocreate en konfigurerbar virtuella datorobjektet med hello namn och storlek som definieras med hjälp av hello variabler som du tidigare har initierat.

Köra hello följande cmdlet toocreate hello virtuella datorobjekt.

    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize

### <a name="create-a-credential-object-toohold-hello-name-and-password-for-hello-local-administrator-credentials"></a>Skapa en autentiseringsuppgift toohold hello namn och lösenord för hello lokal administratörsbehörighet
Innan vi kan ange hello operativsystemet egenskaper för hello virtuell dator, måste toosupply hello autentiseringsuppgifter för hello lokala administratörskontot som en säker sträng. tooaccomplish, kommer vi att använda hello [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.

Kör följande cmdlet hello och Skriv hello toouse namn och lösenord för hello lokala administratörskontot på hello Windows virtuell dator i hello Windows PowerShell autentiseringsuppgifter begäran-fönstret.

    $Credential = Get-Credential -Message "Type hello name and password of hello local administrator account."

### <a name="set-hello-operating-system-properties-for-hello-virtual-machine"></a>Ange hello operativsystemet egenskaper för hello virtuell dator
Vi är nu redo tooset hello virtuella datorns operativsystem egenskaper. Vi använder hello [Set AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) cmdlet tooset hello typ av operativsystem som Windows, kräver hello [virtuella datorns agent](../classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) toobe installerat, ange den hello kan automatiskt Uppdatera och ange hello virtuellt datornamn, hello datornamn och hello autentiseringsuppgifter med hjälp av hello variabler som du tidigare har initierat.

Köra hello följande cmdlet tooset hello operativsystemet egenskaper för den virtuella datorn.

    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate

### <a name="add-hello-network-interface-toohello-virtual-machine"></a>Lägg till hello network interface toohello virtuell dator
Därefter kommer vi lägga till hello nätverksgränssnitt som vi skapade tidigare toohello virtuella datorn. Vi använder hello [Lägg till AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) cmdlet tooadd hello nätverksgränssnitt med hello network interface variabel som du angav tidigare.

Köra hello följande cmdlet tooset hello nätverksgränssnitt för den virtuella datorn.

    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id

### <a name="set-hello-blob-storage-location-for-hello-disk-toobe-used-by-hello-virtual-machine"></a>Ange hello blob-lagringsplats för hello disk toobe används av hello virtuell dator
Vi kommer sedan att ange hello blob-lagringsplats för hello disk toobe används av hello virtuell dator med hello variabler som du angav tidigare.

Köra hello följande cmdlet tooset hello blob-lagringsplats.

    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"

### <a name="set-hello-operating-system-disk-properties-for-hello-virtual-machine"></a>Ange hello operativsystemet diskegenskaper för hello virtuell dator
Vi kommer därefter ange hello operativsystemet diskegenskaper för hello virtuell dator. Vi använder hello [Set AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) cmdlet toospecify som hello operativsystemet för hello virtuella datorn kommer från en avbildning, tooset cachelagring tooread endast (eftersom SQL Server installeras på hello samma disk) och definiera hello virtuellt datornamn och hello operativsystemdisk definieras med hjälp av hello variabler som vi definierade tidigare.

Köra hello följande cmdlet tooset hello operativsystemet diskegenskaper för den virtuella datorn.

    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

### <a name="specify-hello-platform-image-for-hello-virtual-machine"></a>Ange hello plattformsavbildning för hello virtuell dator
Vår senaste konfigurationssteget är toospecify hello plattformsavbildning för våra virtuella datorn. I våra självstudier ska använder vi hello senaste SQL Server 2016 CTP-bild. Vi använder hello [Set AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) cmdlet toouse avbildningen som definieras av hello variabler som du angav tidigare.

Köra hello följande cmdlet toospecify hello plattformsavbildning för den virtuella datorn.

    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## <a name="create-hello-sql-vm"></a>Skapa hello SQL VM
Nu när du är klar hello konfigurationssteg är klara toocreate hello virtuell dator. Vi använder hello [ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) cmdlet toocreate hello virtuell dator med hello variabler som vi har definierat.

Kör följande cmdlet toocreate hello den virtuella datorn.

    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

hello virtuell dator skapas. Observera att skapas ett standardlagringskonto för startdiagnostikinställningar eftersom hello har angetts för hello virtuella datorer är ett premiumlagringskonto.

Du kan nu visa den här datorn i hello Azure Portal toosee [offentliga IP-adressen och dess fullständigt kvalificerade domännamnet](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="example-script"></a>Exempelskriptet
hello innehåller följande skript hello fullständig PowerShell-skript för den här kursen. Det förutsätts att du redan har installationen hello Azure-prenumeration toouse med hello **Add-AzureRmAccount** och **Select-AzureRmSubscription** kommandon.

    # Variables
    ## Global
    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"
    ## Storage
    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

    ## Network
    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $TCPIPAllocationMethod = "Dynamic"
    $DomainName = "sqlvm1"

    ##Compute
    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

    ##Image
    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

    # Resource Group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

    # Storage
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

    # Network
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

    # Compute
    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
    $Credential = Get-Credential -Message "Type hello name and password of hello local administrator account."
    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

    # Image
    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

    ## Create hello VM in Azure
    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

## <a name="next-steps"></a>Nästa steg
När hello virtuell dator har skapats är du redo tooconnect toohello virtuell dator med installation och RDP-anslutning. Mer information finns i [ansluta tooa SQL Server-dator i Azure (Resource Manager)](virtual-machines-windows-sql-connect.md).

