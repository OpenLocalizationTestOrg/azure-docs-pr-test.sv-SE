---
title: "aaaCreate en virtuell Azure-dator med snabbare nätverk | Microsoft Docs"
description: "Lär dig hur toocreate en virtuell dator med snabbare nätverk."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 241362891bfe083ab32c2f558be54f63f87c0693
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-with-accelerated-networking"></a>Skapa en virtuell dator med snabbare nätverk

I kursen får du lära dig hur toocreate ett Azure Virtual Machine (virtuell dator) med snabbare nätverk. Snabbare nätverksfunktioner är GA för Windows och en offentlig förhandsgranskning för specifika Linux-distributioner. Snabbare nätverk gör det möjligt för enskild rot i/o-virtualisering (SR-IOV) tooa VM, vilket avsevärt minskar tiden dess nätverksprestanda. Den här sökvägen för högpresterande kringgår hello värden från hello datapath minskar latens och jitter CPU-belastningen för användning med hello mest krävande nätverksbelastning på VM-typer som stöds. Följande bild visar kommunikation mellan två virtuella datorer (VM) med och utan snabbare nätverksfunktioner hello:

![Jämförelse](./media/virtual-network-create-vm-accelerated-networking/image1.png)

Utan snabbare nätverk måste alla nätverkstrafiken till och från hello VM passerar hello värden och hello virtuella växeln. hello virtuella växeln tillhandahåller alla tvingande, så som nätverkssäkerhetsgrupper, komma åt Kontrollistor, isolering och andra virtualiserade services toonetwork nätverkstrafik. Mer om virtuella växlar, läsa hello toolearn [Hyper-V-nätverksvirtualisering och den virtuella växeln](https://technet.microsoft.com/library/jj945275.aspx) artikel.

Med snabbare nätverk nätverkstrafik når hello Virtuella datorns nätverksgränssnitt (NIC) och sedan vidarebefordras toohello VM. Alla nätverksprinciper som hello virtuell växel gäller utan snabbare nätverksfunktioner avläst och används i maskinvara. Tillämpa principen i maskinvara aktiverar hello NIC tooforward nätverkstrafik direkt toohello VM, kringgå hello värden och hello-växel, samtidigt som alla hello principen tillämpades på hello värden.

hello gäller fördelarna med snabbare nätverksfunktioner endast toohello virtuell dator som den är aktiverad på. För bästa resultat hello är det perfekta tooenable funktionen på minst två virtuella datorer anslutna toohello samma Azure Virtual Network (VNet). Vid kommunikation över Vnet eller anslutande lokalt har funktionen minimal inverkan toooverall svarstid.

> [!WARNING]
> Den här Linux Public Preview inte kan ha hello samma nivå av tillgänglighet och tillförlitlighet som funktioner som är i allmänhet tillgänglighetsversion. hello-funktionen stöds inte, kan ha begränsad kapacitet och kanske inte tillgänglig på alla platser i Azure. Kontrollera hello Azure Virtual Network uppdateringar sida för hello senaste meddelanden på tillgänglighet och status för den här funktionen.

## <a name="benefits"></a>Fördelar
* **Lägre latens / högre paket per sekund (pps):** tar bort hello virtuell växel från hello datapath tar bort hello tid paket i hello värden för behandling av princip för och ökar hello antalet paket som kan bearbetas i hello VM.
* **Minskar jitter:** virtuella växeln bearbetning beror på hello mängden principinformation som måste tillämpas toobe och hello arbetsbelastning av hello CPU som gör hello bearbetning. Avlastning hello princip tvingande toohello maskinvara tar bort den variationen genom att leverera paket direkt toohello VM, tar bort hello värden tooVM kommunikation och alla programvara avbrott och kontext växlar.
* **Minskas CPU-användning:** Bypassing hello virtuella växeln på värden för hello leder tooless CPU-belastningen för bearbetning av nätverkstrafik.

## <a name="Limitations"></a>Begränsningar
hello följande begränsningar gäller när du använder den här funktionen:

* **Network interface skapa:** Accelerated nätverk kan bara aktiveras för en ny nätverkskort. Det går inte att aktivera för en befintlig nätverkskort.
* **Skapa en virtuell dator:** A nätverkskortet med snabbare nätverksfunktioner som är aktiverad kan bara vara anslutna tooa VM när hello virtuell dator skapas. hello NIC får inte vara anslutna tooan befintliga VM.
* **Regioner:** virtuella Windows-datorer med snabbare nätverksfunktioner erbjuds i de flesta Azure-regioner. Linux virtuella datorer med snabbare nätverksfunktioner erbjuds i flera områden. hello-regioner som den här funktionen finns i expanderar. Se hello Azure virtuella nätverk uppdaterar blogg nedan hello senaste informationen.   
* **Operativsystem som stöds:** Windows: Microsoft Windows Server 2012 R2 Datacenter och Windows Server 2016. Linux: Ubuntu Server 16.04 LTS med kernel 4.4.0-77 eller högre, SLES 12 SP2, RHEL 7.3 och CentOS 7.3 (publicerad av ”falsk Wave programvara”).
* **VM-storlek:** generella och beräknings-optimerad instans storlekar med minst åtta kärnor. Mer information finns i hello [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) och [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM-storlekar artiklar. hello uppsättning stöds storlekar på VM-instansen kommer att expandera i hello framtida.
* **Distribution via Azure Resource Manager (ARM):** snabbare nätverk är inte tillgänglig för distribution via ASM/RDFE.

Ändringar toothese begränsningar meddelas via hello [virtuella Azure-nätverk uppdaterar](https://azure.microsoft.com/updates/accelerated-networking-in-preview) sidan.

## <a name="create-a-windows-vm"></a>Skapa en virtuell Windows-dator
Du kan använda hello Azure-portalen eller Azure [PowerShell](#windows-powershell) toocreate hello VM.

### <a name="windows-portal"></a>Portal

1. Öppna en webbläsare hello Azure [portal](https://portal.azure.com) och logga in med ditt Azure [konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Om du inte redan har ett konto kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/offers/ms-azr-0044p).
2. I hello-portalen klickar du på **+ ny** > **Compute** > **Windows Server 2016 Datacenter**. 
3. I hello **Windows Server 2016 Datacenter** bladet som visas, lämna *Resource Manager* valda under **Välj en distributionsmodell**, och klicka på **skapa** .
4. I hello **grunderna** bladet som visas, ange hello följande värden, lämna hello återstående standardalternativen eller Välj eller ange egna värden och på hello **OK** knappen:

    |Inställning|Värde|
    |---|---|
    |Namn|MyVm|
    |Resursgrupp|Lämna **Skapa nytt** markerad och ange *MyResourceGroup*|
    |Plats|Västra USA 2|

    Om du är ny tooAzure lär du dig mer om [resursgrupper](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [prenumerationer](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), och [platser](https://azure.microsoft.com/regions) (som även kallas tooas regioner).
5. I hello **välja en storlek** bladet som visas, ange *8* i hello **minsta kärnor** rutan och klicka sedan på **visa alla**.
6. Klicka på **DS4_V2 Standard**, eller någon stöds VM, klicka på hello **Välj** knappen.
7. I hello **inställningar** bladet som visas, lämnar du alla inställningar som-är undantag för **aktiverad** under **snabbare nätverk**, klicka på hello **OK** knappen. **Obs:** om föregående steg du valde i värdena för VM-storlek, operativsystem eller plats som inte listas i hello [begränsningar](#Limitations) i den här artikeln **Accelerated nätverk**inte visas.
8. I hello **sammanfattning** bladet som visas, klicka på hello **OK** knappen. Azure startar skapa hello VM. Skapa en virtuell dator tar några minuter.
9. tooinstall hello snabbare nätverksdrivrutin för Windows, fullständig hello stegen i hello [konfigurera Windows](#configure-windows) i den här artikeln.

### <a name="windows-powershell"></a>PowerShell
1. Installera hello senaste versionen av hello Azure PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modul. Om du är ny tooAzure PowerShell, läsa hello [översikt över Azure PowerShell](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel.
2. Starta en PowerShell-session genom att klicka på Start-knappen hello att skriva **powershell**, klicka på **PowerShell** från hello sökresultaten.
3. Ange hello i PowerShell-fönstret `login-azurermaccount` kommandot toosign in med ditt Azure [konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Om du inte redan har ett konto kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/offers/ms-azr-0044p).
4. Kopiera hello följande skript i webbläsaren:
    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name $RgName `
      -Location $Location
    
    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -Name MySubnet `
      -AddressPrefix 10.0.0.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName $RgName `
      -Location $Location `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet

    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
      -Name MyPublicIp `
      -ResourceGroupName $RgName `
      -Location $Location `
      -AllocationMethod Static

    # Create a virtual network interface and associate hello public IP address tooit
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Define a credential object for hello VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential

    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
      -VMName MyVM -VMSize Standard_DS4_v2 | `
      Set-AzureRmVMOperatingSystem `
      -Windows `
      -ComputerName myVM `
      -Credential $Cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName MicrosoftWindowsServer `
      -Offer WindowsServer `
      -Skus 2016-Datacenter `
      -Version latest | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id 

    # Create hello virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    #
    ```
5. Högerklicka på toopaste hello skriptet i PowerShell-fönster och starta kör den. Du tillfrågas om användarnamn och lösenord. Använd dessa autentiseringsuppgifter toolog i toohello VM när du ansluter tooit i hello nästa steg. Om hello skriptet misslyckas och du har ändrat värdena i hello skriptet innan du kör den, bekräfta hello-värden som du använde för VM-storlek, operativsystem, och platsen visas i hello [begränsningar](#Limitations) i den här artikeln.
6. tooinstall hello snabbare nätverksdrivrutin för Windows, fullständig hello stegen i hello [konfigurera Windows](#configure-windows) i den här artikeln.

### <a name="configure-windows"></a>Konfigurera Windows
När du har skapat hello VM i Azure måste du installera hello snabbare nätverksdrivrutin för Windows. Innan du slutför följande steg hello, måste du skapa en virtuell Windows-dator med snabbare nätverk med hjälp av antingen hello [portal](#windows-portal) eller [PowerShell](#windows-powershell) steg i den här artikeln. 

1. Öppna en webbläsare hello Azure [portal](https://portal.azure.com) och logga in med ditt Azure [konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Om du inte redan har ett konto kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/offers/ms-azr-0044p).
2. I hello som innehåller hello text *söka resurser* överst hello i hello Azure-portalen, Skriv *MyVm*. När **MyVm** visas i sökresultaten hello klickar du på den.
3. I hello **MyVm** bladet som visas, klicka på hello **Anslut** knappen i hello övre vänstra hörnet av hello-bladet. **Obs:** om **skapa** är synlig under hello **Anslut** knappen Azure inte ännu har skapar hello VM. Klicka på **Anslut** efter att du inte längre se **skapa** under hello **Anslut** knappen.
4. Tillåt din webbläsare toodownload hello **MyVm.rdp** fil.  När du har hämtat, klickar du på hello filen tooopen den. 
5. Klicka på hello **Anslut** knapp i hello **anslutning till fjärrskrivbord** som visas får ett meddelande om att hello utgivaren av hello anslutning inte kan identifieras.
6. I hello **Windows-säkerhet** som visas, klicka på **fler alternativ**, klicka på **Använd ett annat konto**. Ange hello användarnamn och lösenord som du angav i steg 4 och klicka sedan på hello **OK** knappen.
7. Klicka på hello **Ja** knappen i hello anslutning till fjärrskrivbord som visas, får ett meddelande om att hello identitet hello fjärrdatorn inte kan verifieras.
8. Högerklicka på hello Start-knappen och klicka på **Enhetshanteraren**. Expandera hello **nätverkskort** nod. Bekräfta att hello **Mellanox ConnectX 3 virtuella funktionen Ethernet-nätverkskort** visas som i följande bild hello:
   
    ![Enhetshanteraren](./media/virtual-network-create-vm-accelerated-networking/image2.png)

9. Snabbare nätverksfunktioner har nu aktiverats för den virtuella datorn.

## <a name="create-a-linux-vm"></a>Skapa en virtuell Linux-dator
Du kan använda hello Azure-portalen eller Azure [PowerShell](#linux-powershell) toocreate en Ubuntu eller SLES VM. Det finns ett annat arbetsflöde RHEL och CentOS virtuella datorer.  Se hello anvisningarna nedan.

### <a name="linux-portal"></a>Portal
1. Registrera dig för hello snabbare nätverk för Linux-förhandsgranskning genom att följa steg 1-5 för hello [och skapar en Linux VM - PowerShell](#linux-powershell) i den här artikeln.  Du kan inte registrera för hello förhandsgranskning i hello-portalen.
2. Slutför stegen 1 – 8 i hello [skapa en virtuell dator i Windows - portal](#windows-portal) i den här artikeln. I steg 2, klickar du på **Ubuntu Server 16.04 LTS** i stället för **Windows Server 2016 Datacenter**. För den här självstudiekursen Välj toouse lösenord istället för en SSH-nyckel om för Produktionsdistribution, du kan använda antingen. Om **snabbare nätverk** visas inte när du har slutfört steg 7 i hello [skapa en virtuell dator i Windows - portal](#windows-portal) avsnitt i den här artikeln är förmodligen för en av följande orsaker hello:
    - Du ännu inte har registrerats för hello förhandsgranskning. Kontrollera att din registreringstillstånd **registrerade**, enligt beskrivningen i steg 4 i hello [och skapar en Linux VM - Powershell](#linux-powershell) i den här artikeln. **Obs:** om du har deltagit i hello Accelerated nätverk för virtuella Windows-datorer preview (det är inte längre nödvändigt tooregister toouse snabbare nätverk för virtuella Windows-datorer), du inte är automatiskt registrerad för hello Accelerated nätverk för Förhandsgranskning av virtuella Linux-datorer. Du måste registrera dig för hello Accelerated nätverk för virtuella Linux-datorer Förhandsgranska tooparticipate i den.
    - Du har inte valt VM-storlek, operativsystem eller plats som anges i hello [begränsningar](#limitations) i den här artikeln.
3. tooinstall hello snabbare nätverksdrivrutin för Linux, fullständig hello stegen i hello [konfigurera Linux](#configure-linux) i den här artikeln.

### <a name="linux-powershell"></a>PowerShell

>[!WARNING]
>Om du skapar virtuella Linux-datorer med snabbare nätverk i en prenumeration och försök sedan toocreate en virtuell Windows-dator med snabbare nätverksfunktioner i hello samma prenumeration hello skapa en virtuell Windows-dator kan misslyckas. Den här förhandsversionen rekommenderas att du testar Linux och Windows-datorer med snabbare nätverk i separata prenumerationer.
>

1. Installera hello senaste versionen av hello Azure PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modul. Om du är ny tooAzure PowerShell, läsa hello [översikt över Azure PowerShell](/powershell/azure/get-started-azureps?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel.
2. Starta en PowerShell-session genom att klicka på Start-knappen hello att skriva **powershell**, klicka på **PowerShell** från hello sökresultaten.
3. Ange hello i PowerShell-fönstret `login-azurermaccount` kommandot toosign in med ditt Azure [konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Om du inte redan har ett konto kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/offers/ms-azr-0044p).
4. Registrera dig för hello snabbare nätverk för Azure preview genom att slutföra hello följande steg:
    - Skicka ett e-postmeddelande för[ axnpreview@microsoft.com ](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) med Azure prenumerations-ID och avsedd att användas. Vänta tills en e-postbekräftelse från Microsoft om prenumerationen har aktiverats.
    - Ange hello efter kommandot tooconfirm du är registrerad för hello preview:
    
        ```powershell
        Get-AzureRmProviderFeature -FeatureName AllowAcceleratedNetworkingForLinux -ProviderNamespace Microsoft.Network
        ```

        Fortsätt inte med steg 5 tills **registrerade** visas i hello utdata när du har angett hello föregående kommando. Din utdata måste se ut så hello följande utdata innan du fortsätter:
    
        ```powershell
        FeatureName                        ProviderName      RegistrationState
        -----------                        ------------      -----------------
        AllowAcceleratedNetworkingForLinux Microsoft.Network Registered
        ```
        
      >[!NOTE]
      >Om du har deltagit i hello Accelerated nätverk för virtuella Windows-datorer preview (det är inte längre nödvändigt tooregister toouse snabbare nätverk för virtuella Windows-datorer), du inte är automatiskt registrerad för hello Accelerated nätverk för virtuella Linux-datorer förhandsgranskning. Du måste registrera dig för hello Accelerated nätverk för virtuella Linux-datorer Förhandsgranska tooparticipate i den.
      >
5. Kopiera hello följande skript i webbläsaren ersätter Ubuntu eller SLES efter behov.  Igen, Redhat och CentOS har ett annat arbetsflöde som beskrivs nedan:

    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
      -Name $RgName `
      -Location $Location
    
    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -Name MySubnet `
      -AddressPrefix 10.0.0.0/24
    
    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName $RgName `
      -Location $Location `
      -Name MyVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet

    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
      -Name MyPublicIp `
      -ResourceGroupName $RgName `
      -Location $Location `
      -AllocationMethod Static

    # Create a virtual network interface and associate hello public IP address tooit
    $Nic = New-AzureRmNetworkInterface `
      -Name MyNic `
      -ResourceGroupName $RgName `
      -Location $Location `
      -SubnetId $Vnet.Subnets[0].Id `
      -PublicIpAddressId $Pip.Id `
      -EnableAcceleratedNetworking
     
    # Create a new Storage account and define hello new VM’s OSDisk name and its URI
    # Must end with ".vhd" extension
    $OSDiskName = "MyOsDiskName.vhd"
    # Storage account name must be between 3 and 24 characters in length and use numbers and lower-case letters only.
    $OSDiskSAName = "thestorageaccountname"  
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $RgName -Name $OSDiskSAName -Type "Standard_GRS" -Location $Location
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName
 
    # Define a credential object for hello VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential

    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
      -VMName MyVM -VMSize Standard_DS4_v2 | `
      Set-AzureRmVMOperatingSystem `
      -Linux `
      -ComputerName myVM `
      -Credential $Cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName Canonical `
      -Offer UbuntuServer `
      -Skus 16.04-LTS `
      -Version latest | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id | `
    Set-AzureRmVMOSDisk -Name $OSDiskName `
      -VhdUri $OSDiskUri `
      -CreateOption FromImage 

    # Create hello virtual machine.    
    New-AzureRmVM `
      -ResourceGroupName $RgName `
      -Location $Location `
      -VM $VmConfig
    ```
    
6. Högerklicka på toopaste hello skriptet i PowerShell-fönster och starta kör den. Du tillfrågas om användarnamn och lösenord. Använd dessa autentiseringsuppgifter toolog i toohello VM när du ansluter tooit i hello nästa steg. Om det inte går att hello skript, bekräftar du att:
    - Du är registrerad för hello förhandsgranskning, enligt beskrivningen i steg 4
    - Om du har ändrat VM-storlek, typ av operativsystem eller plats värden i hello skript innan den körs, att hello värden finns i hello [begränsningar](#Limitations) i den här artikeln.
7. tooinstall hello snabbare nätverksdrivrutin för Linux, fullständig hello stegen i hello [konfigurera Linux](#configure-linux) i den här artikeln.

### <a name="configure-linux"></a>Konfigurera Linux

När du har skapat hello VM i Azure måste du installera hello snabbare nätverksdrivrutin för Linux. Innan du slutför följande steg hello, måste du skapa en Linux VM snabbare nätverk med hjälp av antingen hello [portal](#linux-portal) eller [PowerShell](#linux-powershell) steg i den här artikeln. 

1. Öppna en webbläsare hello Azure [portal](https://portal.azure.com) och logga in med ditt Azure [konto](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Om du inte redan har ett konto kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/offers/ms-azr-0044p).
2. Hello överst i hello portalen toohello höger om hello *söka resurser* Klicka hello **> _** ikonen toostart ett Bash molnet gränssnitt (förhandsversion). hello Bash molnet shell fönstret visas längst ned hello hello portal och efter några sekunder visas en  **username@Azure:~ $** prompt. Om du kan SSH toohello VM från din dator i stället för på hello molnet shell förutsätts hello i den här självstudiekursen att du använder hello molnet shell.
3. Hello över hello portal hello i rutan som innehåller hello text *söka resurser*, typen *MyVm*. När **MyVm** visas i sökresultaten hello klickar du på den.
4. I hello **MyVm** bladet som visas, klicka på hello **Anslut** knappen i hello övre vänstra hörnet av hello-bladet. **Obs:** om **skapa** är synlig under hello **Anslut** knappen Azure inte ännu har skapar hello VM. Klicka på **Anslut** efter att du inte längre se **skapa** under hello **Anslut** knappen.
5. Azure öppnar en ruta som uppmanar tooenter hello `ssh adminuser@<ipaddress>`. Ange det här kommandot i hello molnet shell (eller kopiera den från hello ruta som fanns i steg 4 och klistra in den i toohello moln shell), och tryck sedan på RETUR.
6. Ange **Ja** toohello fråga tillfrågas du om du vill ansluta toocontinue, tryck på RETUR.
7. Ange hello lösenordet du angav när du skapar hello VM. En gång loggat in toohello VM, visas en adminuser@MyVm:~ $ prompten. Du är nu inloggad toohello VM via hello molnet shell session. **Obs:** moln shell sessioner timeout efter 10 minuter av inaktivitet.

Nu hello instruktioner kan variera beroende på hello-distribution som du använder. 

#### <a name="ubuntusles"></a>Ubuntu/SLES

1. I Kommandotolken hello ange `uname -r` och bekräfta hello-version för:

    * Ubuntu är ”4.4.0-77-generic” eller högre
    * SLES är ”4.4.59-92.20-default” eller högre

2. Skapa en avkastningen mellan hello standard nätverk vNIC och hello snabbare nätverk vNIC genom att köra hello-kommandon som följer. Nätverkstrafik använder hello högre prestanda snabbare nätverk vNIC medan hello avkastningen garanterar att nätverkstrafiken inte avbryts över vissa ändringar i konfigurationen.
          
     ```bash
     wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/configure_hv_sriov.sh
     chmod +x ./configure_hv_sriov.sh
     sudo ./configure_hv_sriov.sh
     ```
3. Efter att köra hello skriptet hello VM startar om efter 60 sekunder pausa.
4. Hello VM startas om och återansluter en gång tooit genom att följa steg 5 – 7 igen.
5. Kör hello `ifconfig` kommando och bekräfta att bond0 är nu tillgänglig och hello gränssnittet visas som upp. 
 
 >[!NOTE]
      >Program med snabbare nätverk måste kommunicera över hello *bond0* gränssnitt inte *eth0*.  hello Gränssnittsnamnet ändras innan snabbare nätverksfunktioner når allmän tillgänglighet.

#### <a name="rhelcentos"></a>RHEL/CentOS

Skapar en Red Hat Enterprise Linux eller CentOS 7.3 VM kräver vissa extra steg tooload hello senaste drivrutiner som behövs för SR-IOV och hello VF (Virtual Function) drivrutinen för hello nätverkskort. hello förbereds första fasen av hello instruktioner en avbildning som kan använda toomake en eller flera virtuella datorer som har hello drivrutiner som redan har lästs in.

##### <a name="phase-one-prepare-a-red-hat-enterprise-linux-or-centos-73-base-image"></a>Steg ett: Förbered en Red Hat Enterprise Linux eller CentOS 7.3 basavbildning. 

1.  Etablera en icke - SRIOV CentOS 7.3 VM på Azure

2.  Installera LIS 4.2.2:
    
    ```bash
    wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
    tar -xvf lis-rpms-4.2.2-2.tar.gz
    cd LISISO && sudo ./install.sh
    ```

3.  Hämta konfigurationsfiler
    
    ```bash
    cd /etc/udev/rules.d/  
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/60-hyperv-vf-name.rules 
    cd /usr/sbin/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/hv_vf_name 
    sudo chmod +x hv_vf_name
    cd /etc/sysconfig/network-scripts/
    sudo wget https://raw.githubusercontent.com/LIS/lis-next/master/tools/sriov/ifcfg-vf1   
    ```

4.  Ta bort etableringen av den här virtuella datorn

    ```bash
    sudo waagent -deprovision+user 
    ```

5.  Stoppa den virtuella datorn, från Azure-portalen och gå Toovm's ”diskar”, avbilda hello OSDisk VHD-URI. Den här URI: N innehåller hello basavbildning VHD namn och dess storage-konto. 
 
##### <a name="phase-two-provision-new-vms-on-azure"></a>Steg två: etablera nya virtuella datorer på Azure

1.  Etablera nya virtuella datorer baserade med ny AzureRMVMConfig med hello basavbildning VHD till i den första fasen, med AcceleratedNetworking aktiverad på hello virtuellt nätverkskort:

    ```powershell
    $RgName="MyResourceGroup"
    $Location="westus2"
    
    # Create a resource group
    New-AzureRmResourceGroup `
     -Name $RgName `
     -Location $Location

    # Create a subnet
    $Subnet = New-AzureRmVirtualNetworkSubnetConfig `
     -Name MySubnet `
     -AddressPrefix 10.0.0.0/24

    # Create a virtual network
    $Vnet=New-AzureRmVirtualNetwork `
     -ResourceGroupName $RgName `
     -Location $Location `
     -Name MyVnet `
     -AddressPrefix 10.0.0.0/16 `
     -Subnet $Subnet
    
    # Create a public IP address
    $Pip = New-AzureRmPublicIpAddress `
     -Name MyPublicIp `
     -ResourceGroupName $RgName `
     -Location $Location `
     -AllocationMethod Static
    
    # Create a virtual network interface and associate hello public IP address tooit
    $Nic = New-AzureRmNetworkInterface `
     -Name MyNic `
     -ResourceGroupName $RgName `
     -Location $Location `
     -SubnetId $Vnet.Subnets[0].Id `
     -PublicIpAddressId $Pip.Id `
     -EnableAcceleratedNetworking
    
    # Specify hello base image's VHD URI (from phase one step 5). 
    # Note: hello storage account of this base image vhd should have "Storage service encryption" disabled
    # See more from here: https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption
    # This is just an example URI, you will need tooreplace this when running this script
    $sourceUri="https://myexamplesa.blob.core.windows.net/vhds/CentOS73-Base-Test120170629111341.vhd" 

    # Specify a URI for hello location from which hello new image binary large object (BLOB) is copied toostart hello virtual machine. 
    # Must end with ".vhd" extension
    $OsDiskName = "MyOsDiskName.vhd" 
    $destOsDiskUri = "https://myexamplesa.blob.core.windows.net/vhds/" + $OsDiskName
    
    # Define a credential object for hello VM. PowerShell prompts you for a username and password.
    $Cred = Get-Credential
    
    # Create a custom virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
     -VMName MyVM -VMSize Standard_DS4_v2 | `
    Set-AzureRmVMOperatingSystem `
     -Linux `
     -ComputerName myVM `
     -Credential $Cred | `
    Add-AzureRmVMNetworkInterface -Id $Nic.Id | `
    Set-AzureRmVMOSDisk `
     -Name $OsDiskName `
     -SourceImageUri $sourceUri `
     -VhdUri $destOsDiskUri `
     -CreateOption FromImage `
     -Linux
    
    # Create hello virtual machine.    
    New-AzureRmVM `
     -ResourceGroupName $RgName `
     -Location $Location `
     -VM $VmConfig
    ```

2.  När virtuella datorer starta hello VF enhet genom att ”lspci” och kontrollera hello Mellanox posten. Vi bör till exempel se det här objektet i hello lspci utdata:
    
    ```
    0001:00:02.0 Ethernet controller: Mellanox Technologies MT27500/MT27520 Family [ConnectX-3/ConnectX-3 Pro Virtual Function]
    ```
    
3.  Kör hello partnerskap skript genom att:

    ```bash
    sudo bondvf.sh
    ```

4.  Starta om hello nya virtuella datorer:

    ```bash
    sudo reboot
    ```

hello VM ska starta med bond0 konfigurerats och hello snabbare nätverk sökväg aktiverad.  Kör `ifconfig` tooconfirm.
