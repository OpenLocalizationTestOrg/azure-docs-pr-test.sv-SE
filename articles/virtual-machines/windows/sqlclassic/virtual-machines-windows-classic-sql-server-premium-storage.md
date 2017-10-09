---
title: aaaUse Azure Premium-lagring med SQL Server | Microsoft Docs
description: "Den här artikeln använder resurser som har skapats med hello klassiska distributionsmodellen och ger vägledning om hur du använder Azure Premium-lagring med SQL Server som körs på Azure Virtual Machines."
services: virtual-machines-windows
documentationcenter: 
author: danielsollondon
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 7ccf99d7-7cce-4e3d-bbab-21b751ab0e88
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/01/2017
ms.author: jroth
ms.openlocfilehash: 393ea2020b39ea686302ae632e1049935c24af00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-premium-storage-with-sql-server-on-virtual-machines"></a>Använd Azure Premium Storage med SQL Server på virtuella datorer
## <a name="overview"></a>Översikt
[Azure Premium-lagring](../../../storage/common/storage-premium-storage.md) är hello nästa generation av lagring som innehåller låg latens och hög genomströmning IO. Det fungerar bäst för nyckel i/o-intensiv arbetsbelastning, till exempel SQL Server på IaaS [virtuella datorer](https://azure.microsoft.com/services/virtual-machines/).

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

Den här artikeln innehåller planering och vägledning för att migrera en virtuell dator som kör SQL Server toouse Premium-lagring. Detta inkluderar Azure-infrastrukturen (nätverk, lagring) och Gäst Windows VM steg. hello exemplet i hello [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) visar en fullständig omfattande slutet tooend migrering av hur toomove större virtuella datorer tootake nytta av bättre lokala SSD-lagring med PowerShell.

Det är viktigt toounderstand hello slutpunkt till slutpunkt-processen genom att använda Azure Premium-lagring med SQL Server på IAAS-VM. Detta omfattar:

* Identifiering av hello krav toouse Premium-lagring.
* Exempel på distribution av SQL Server på IaaS tooPremium lagring för nya distributioner.
* Exempel på migrera befintliga distributioner, både fristående servrar och distributioner med hjälp av SQL Always On-Tillgänglighetsgrupper.
* Möjliga migrering metoder.
* Fullständig slutpunkt till slutpunkt i exemplet visar Azure, Windows och SQL Server stegen för hello migrering av en befintlig Always On-implementering.

Mer information om SQL Server i Azure Virtual Machines finns [SQL Server i Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

**Skapad av:** Mikael Sol **Teknisk granskare:** Thomas Carlos Vargas sill, Sanjay Mishra, Pravin Mital, Juergen Thomas, Gonzalo Pettersson.

## <a name="prerequisites-for-premium-storage"></a>Krav för Premium-lagring
Det finns flera förutsättningar för att använda Premium-lagring.

### <a name="machine-size"></a>Storleken på datorn
För att använda Premium-lagring måste toouse DS-serien virtuella datorer (VM). Om du inte har använt DS-serien datorer i din molntjänst innan du måste ta bort befintliga VM hello hålla hello anslutna diskar och sedan skapa en ny molntjänst innan återskapa hello VM som DS * rollstorleken. Mer information om storlekar för virtuella datorer finns [virtuell dator och Molntjänststorlekar för Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="cloud-services"></a>Molntjänster
Du kan bara använda DS * virtuella datorer med Premium-lagring när de skapas i en ny molntjänst. Om du använder SQL Server alltid aktiverad i Azure syftar hello alltid lyssnare för toohello Azure interna eller externa belastningen belastningsutjämnaren IP-adress som är associerad med en tjänst i molnet. Den här artikeln fokuserar på hur toomigrate samtidigt är tillgänglig i det här scenariot.

> [!NOTE]
> En serie med DS * hello första virtuella dator som är distribuerade toohello ny molntjänst.
>
>

### <a name="regional-vnets"></a>Regional VNET
Du måste konfigurera hello virtuella nätverk (VNET) värd för dina virtuella datorer toobe regionala för DS * virtuella datorer. Den här ”utökar” Hej VNET är tooallow hello större virtuella datorer toobe etablerad på andra kluster och tillåta kommunikation mellan dem. I följande skärmbild hello, visar hello markerade platsen regional Vnet hello första resultatet visar ett ”smala” VNET.

![RegionalVNET][1]

Du kan höja ett Microsoft-supporten biljett toomigrate tooa regionalt VNET, Microsoft gör en ändring sedan toocomplete hello migrering tooregional Vnet, ändra hello egenskapen AffinityGroup i hello nätverkskonfigurationen. Exportera hello nätverkskonfigurationen i PowerShell och Skriv hello **AffinityGroup** egenskap i hello **VirtualNetworkSite** element med en **plats** Egenskapen. Ange `Location = XXXX` där `XXXX` är en Azure-region. Importera hello ny konfiguration.

Till exempel överväger hello följande konfiguration av virtuellt nätverk:

    <VirtualNetworkSite name="danAzureSQLnet" AffinityGroup="AzureSQLNetwork">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

toomove denna tooa regionalt VNET i västra Europa ändra hello configuration toohello följande:

    <VirtualNetworkSite name="danAzureSQLnet" Location="West Europe">
    <AddressSpace>
      <AddressPrefix>10.0.0.0/8</AddressPrefix>
      <AddressPrefix>172.16.0.0/12</AddressPrefix>
    </AddressSpace>
    <Subnets>
    ...
    </VirtualNetworkSite>

### <a name="storage-accounts"></a>Lagringskonton
Du behöver toocreate ett nytt lagringskonto som är konfigurerad för Premium-lagring. Observera att hello användning av Premium-lagring är inställt på hello storage-konto, inte på enskilda virtuella hårddiskar, men när du använder en DS * serien virtuell dator kan du bifoga VHD från Premium och standardlagring konton. Du kan överväga att detta om du inte vill att tooplace hello OS VHD på toohello Premium Storage-konto.

hello följande **ny AzureStorageAccountPowerShell** med hello ”Premium_LRS” **typen** skapar ett Premiumlagringskonto:

    $newstorageaccountname = "danpremstor"
    New-AzureStorageAccount -StorageAccountName $newstorageaccountname -Location "West Europe" -Type "Premium_LRS"   

### <a name="vhds-cache-settings"></a>Inställningar för cachelagring av virtuella hårddiskar
hello största skillnaden mellan att skapa diskar som är en del av ett premiumlagringskonto är hello diskcache-inställningen. För SQL Server-Data volym diskar det rekommenderas att du använder '**Läs cachelagring**'. För transaktionen loggvolymer hello diskcache-inställningen ska vara inställd för '**ingen**'. Detta skiljer sig från hello rekommendationer för Standard Storage-konton.

Det kopplade hello virtuella hårddiskar, kan hello cache-inställningen inte ändras. Du behöver toodetach och Återanslut hello VHD med en uppdaterad cache-inställningen.

### <a name="windows-storage-spaces"></a>Lagringsutrymmen för Windows
Du kan använda [Windows lagringsutrymmen](https://technet.microsoft.com/library/hh831739.aspx) som du gjorde med föregående standardlagring detta gör att du toomigrate en virtuell dator som redan använder lagringsutrymmen. hello exemplet i [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) (steg 9 och framåt) visar hello Powershell kod tooextract och importera en virtuell dator med flera anslutna virtuella hårddiskar.

Lagringspooler användes med Standard-Azure storage-konto tooenhance genomströmning och minska svarstiden. Du kan hitta värdet i testning lagringspooler med Premium-lagring för nya distributioner, men de lägger till ytterligare komplexitet med Lagringsinställningar.

#### <a name="how-toofind-which-azure-virtual-disks-map-toostorage-pools"></a>Hur toofind vilka virtuella Azure-diskar mappa toostorage pooler
Eftersom det finns olika cache inställningen rekommendationer för anslutna virtuella hårddiskar, kan du bestämma toocopy hello virtuella hårddiskar tooa Premium Storage-konto. När du återansluta dem toohello nya DS-serien VM, behöva tooalter hello cacheinställningarna. Det är enklare tooapply hello Premium-lagring rekommenderade inställningar när du har separata virtuella hårddiskar för hello SQL-Data filer och log-filer (istället för en enda virtuell Hårddisk som innehåller både).

> [!NOTE]
> Om du har SQL Server data och loggfilen filer på samma volym, hello cachelagring alternativ du väljer beror på hello-i/o-åtkomstmönster för din databasarbetsbelastningar hello. Testa bara kan visa vilket alternativ för cachelagring är bäst för det här scenariot.
>
>

Men om du använder Windows lagringsutrymmen som består av flera virtuella hårddiskar måste toolook på din ursprungliga skript tooidentify som ansluten virtuella hårddiskar finns i vilka specifika poolen, så du kan ange inställningar för cachelagring av hello därefter för varje disk.

Om du inte har ursprungliga skriptet tillgängliga tooshow vilka virtuella hårddiskar mappa toohello lagringspoolen, du kan använda hello följande steg toodetermine hello disklagring/poolen mappning.

Använd hello följande steg för varje disk:

1. Hämta listan över diskar kopplade tooVM med hello **Get-AzureVM** kommando:

    Get-AzureVM - ServiceName <servicename> -namnet <vmname> | Get-AzureDataDisk
2. Observera hello Diskname och LUN.

    ![DisknameAndLUN][2]
3. Fjärrskrivbord till hello VM. Gå sedan för**Datorhantering** | **Enhetshanteraren** | **diskenheter**. Titta på hello egenskaperna för varje hello Microsoft virtuella diskar

    ![VirtualDiskProperties][3]
4. hello LUN-nummer här är en referens toohello LUN-nummer som du anger när du ansluter hello VHD toohello VM.
5. Hello Microsoft Virtual Disk finns toohello **information** på fliken sedan hello **egenskapen** listan, gå för**drivrutinen nyckeln**. I hello **värdet**, Observera hello **Offset**, vilket är 0002 i hello följande skärmbild. hello 0002 anger hello PhysicalDisk2 som hello storage pool referenser.

    ![VirtualDiskPropertyDetails][4]
6. För varje lagringspool associerade dump ut hello diskar:

    Get-StoragePool - FriendlyName AMS1pooldata | Get-PhysicalDisk

    ![GetStoragePool][5]

Nu kan du använda anslutna den här informationen tooassociate virtuella hårddiskar tooPhysical diskar i lagringspooler.

När du har mappat virtuella hårddiskar tooPhysical diskar i lagringspooler som du kan koppla bort och kopiera dem över tooa Premium Storage-konto kan sedan koppla dem med hello rätt cache inställningen. Se hello exemplet i hello [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage), steg 8 till och med 12. Dessa steg visar hur tooextract en VM-ansluten virtuell Hårddisk disk configuration tooa CSV-fil, kopiera hello virtuella hårddiskar, alter hello disk cache konfigurationsinställningar och slutligen omdistribuera hello VM som en serie DS VM med alla hello anslutna diskar.

### <a name="vm-storage-bandwidth-and-vhd-storage-throughput"></a>VM-lagring bandbredd och kapaciteten för lagring av virtuell Hårddisk
hello beroende lagringsprestanda hello DS * VM-storlek som har angetts och hello VHD-storlek. hello virtuella datorer ha olika tillägg för hello antalet virtuella hårddiskar som kan bifogas och hello Maximal bandbredd som de stöder (MB/s). Hello specifika bandbredd siffror finns [virtuell dator och Molntjänststorlekar för Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Ökad IOPS uppnås med större storlekar för diskar. Du bör överväga när du funderar på sökvägen för migreringen. Mer information [finns hello tabellen för IOPS och disktyper](../../../storage/common/storage-premium-storage.md#scalability-and-performance-targets).

Slutligen bör du att virtuella datorer har olika maximal disk bandbredder de stöder för alla diskar som är anslutna. Du kan fylla hello maximal disk tillgänglig bandbredd för Virtuella datorns rollstorlek under hög belastning. Till exempel stöder en Standard_DS14 upp too512MB/s. därför med tre P30 diskar fylla hello diskbandbredden för hello VM. Men i det här exemplet hello genomströmning gränsen kan överskridas beroende på hello blandning av läsning och skrivning IOs.

## <a name="new-deployments"></a>Nya distributioner
hello följande två avsnitt visar hur du kan distribuera virtuella SQL Server-datorer tooPremium lagring. Som nämnts så bör behöver du inte nödvändigtvis tooplace hello OS-disken till Premium-lagring. Du kan välja toodo detta om du avsikten är tooplace alla i/o-arbetsbelastningar på hello OS-VHD.

hello första exemplet visar genom att använda befintliga avbildningar i Azure-galleriet. hello det andra exemplet visas hur toouse anpassade VM avbildning som du har i ett befintligt lagringskonto som Standard.

> [!NOTE]
> Dessa exempel förutsätter att du redan har skapat ett regionalt VNET.
>
>

### <a name="create-a-new-vm-with-premium-storage-with-gallery-image"></a>Skapa en ny virtuell dator med Premium-lagring med bild galleriet
hello exemplet nedan visar hur tooplace hello OS VHD till premium-lagring och bifoga Premium Storage virtuella hårddiskar. Du kan också placera hello OS-disken i ett standardlagringskonto och sedan koppla virtuella hårddiskar som finns i ett Premium Storage-konto. Båda scenarierna är visas.

    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Set up subscription
    Set-AzureSubscription -SubscriptionName $mysubscription
    Select-AzureSubscription -SubscriptionName $mysubscription -Current  

#### <a name="step-1-create-a-premium-storage-account"></a>Steg 1: Skapa ett Premiumlagringskonto
    #Create Premium Storage account, note Type
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  


#### <a name="step-2-create-a-new-cloud-service"></a>Steg 2: Skapa en ny molntjänst
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-reserve-a-cloud-service-vip-optional"></a>Steg 3: Reservera en Cloud Service VIP (valfritt)
    #check exisitng reserved VIP
    Get-AzureReservedIP

    $reservedVIPName = “sqlcloudVIP”
    New-AzureReservedIP –ReservedIPName $reservedVIPName –Label $reservedVIPName –Location $location

#### <a name="step-4-create-a-vm-container"></a>Steg 4: Skapa en VM-behållare
    #Generate storage keys for later
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    ##Generate storage acc contexts
    $xioContext = New-AzureStorageContext –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary   

    #Create container
    $containerName = 'vhds'
    New-AzureStorageContainer -Name $containerName -Context $xioContext

#### <a name="step-5-placing-os-vhd-on-standard-or-premium-storage"></a>Steg 5: Avyttring OS VHD Standard eller Premium-lagring
    #NOTE: Set up subscription and default storage account which will be used tooplace hello OS VHD in

    #If you want tooplace hello OS VHD Premium Storage Account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $newxiostorageaccountname  

    #If you wanted tooplace hello OS VHD Standard Storage Account but attach Premium Storage VHDs then you would run this instead:
    $standardstorageaccountname = "danstdams"

    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount  $standardstorageaccountname

#### <a name="step-6-create-vm"></a>Steg 6: Skapa en virtuell dator
    #Get list of available SQL Server Images from hello Azure Image Gallery.
    $galleryImage = Get-AzureVMImage | where-object {$_.ImageName -like "*SQL*2014*Enterprise*"}
    $image = $galleryImage.ImageName

    #Set up Machine Specific Information
    $vmName = "dsDan1"
    $vnet = "dansvnetwesteur"
    $subnet = "SQL"
    $ipaddr = "192.168.0.8"

    #Remember toochange tooDS series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "mycomplexpwd4*"

    #Create VM Config
    $vmConfigsl = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $image  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Add Data and Log Disks tooVM Config
    #Note hello size specified ‘-DiskSizeInGB 1023’, this will attach 2 x P30 Premium Storage Disk Type
    #Utilising hello Premium Storage enabled Storage account

    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-data1.vhd"
    $vmConfigsl | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "logDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-log1.vhd"

    #Create VM
    $vmConfigsl  | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)  

    #Add RDP Endpoint
    $EndpointNameRDPInt = "3389"
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Add-AzureEndpoint -Name "EndpointNameRDP" -Protocol "TCP" -PublicPort "53385" -LocalPort $EndpointNameRDPInt  | Update-AzureVM

    #Check VHD storage account, these should be in $newxiostorageaccountname
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName | Get-AzureDataDisk
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmName |Get-AzureOSDisk


### <a name="create-a-new-vm-toouse-premium-storage-with-a-custom-image"></a>Skapa en ny VM toouse Premium-lagring med en anpassad avbildning
Det här scenariot visar där du har befintliga anpassade avbildningar som finns i ett standardlagringskonto. Som tidigare nämnts om du vill tooplace hello OS VHD i Premium-lagring måste toocopy hello avbildning som finns i hello standardlagringskonto och överför dem tooa Premium-lagring innan den kan användas. Om du har en bild på lokalt, kan du också använda den här metoden toocopy som direkt toohello Premium Storage-konto.

#### <a name="step-1-create-storage-account"></a>Steg 1: Skapa Storage-konto
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Standard Storage account
    $origstorageaccountname = "danstdams"

#### <a name="step-2-create-cloud-service"></a>Steg 2 Skapa molntjänst
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location


#### <a name="step-3-use-existing-image"></a>Steg 3: Använd befintlig avbildning
Du kan använda en befintlig avbildning. Du kan [ta en bild av en befintlig dator](../classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Obs hello datorn du tar avbildningen har inte toobe DS * datorn. När du har hello avbildningen hello följande steg visar hur toocopy den toohello premiumlagringskonto med hello **Start AzureStorageBlobCopy** PowerShell-kommandot.

    #Get storage account keys:
    #Standard Storage account
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    #Premium Storage account
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Set up contexts for hello storage accounts:
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $destContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

#### <a name="step-4-copy-blob-between-storage-accounts"></a>Steg 4: Kopiera Blob mellan Storage-konton
    #Get Image VHD
    $myImageVHD = "dansoldonorsql2k14-os-2015-04-15.vhd"
    $containerName = 'vhds'

    #Copy Blob between accounts
    $blob = Start-AzureStorageBlobCopy -SrcBlob $myImageVHD -SrcContainer $containerName `
    -DestContainer vhds -Destblob "prem-$myImageVHD" `
    -Context $origContext -DestContext $destContext  

#### <a name="step-5-regularly-check-copy-status"></a>Steg 5: Kontrollera regelbundet-kopian status:
    $blob | Get-AzureStorageBlobCopyState

#### <a name="step-6-add-image-disk-tooazure-disk-repository-in-subscription"></a>Steg 6: Lägg till avbildning disk tooAzure disk databasen i prenumerationen
    $imageMediaLocation = $destContext.BlobEndPoint+"/"+$myImageVHD
    $newimageName = "prem"+"dansoldonorsql2k14"

    Add-AzureVMImage -ImageName $newimageName -MediaLocation $imageMediaLocation

> [!NOTE]
> Du kanske att även om hello statusrapporter om åtgärden lyckades, du kan fortfarande få ett lån diskfel. I så fall väntar du cirka 10 minuter.
>
>

#### <a name="step-7--build-hello-vm"></a>Steg 7: Skapa hello VM
Här bygger du hello VM från avbildningen och ansluta två Premium Storage virtuella hårddiskar:

    $newimageName = "prem"+"dansoldonorsql2k14"
    #Set up Machine Specific Information
    $vmName = "dansolchild"
    $vnet = "westeur"
    $subnet = "Clients"
    $ipaddr = "192.168.0.41"

    #This will need toobe a new cloud service
    $destcloudsvc = "danregsvcamsxio2"

    #Use tooDS Series VM
    $newInstanceSize = "Standard_DS1"

    #create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS3"

    #Machine User Credentials
    $userName = "myadmin"
    $pass = "theM)stC0mplexP@ssw0rd!”


    #Create VM Config
    $vmConfigsl2 = New-AzureVMConfig -Name $vmName -InstanceSize $newInstanceSize -ImageName $newimageName  -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` -AdminUserName $userName -Password $pass | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 0 -HostCaching "ReadOnly"  -DiskLabel "DataDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-Datadisk-1.vhd"
    $vmConfigsl2 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 1023 -LUN 1 -HostCaching "None"  -DiskLabel "LogDisk1" -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vmName-logdisk-1.vhd"



    $vmConfigsl2 | New-AzureVM –ServiceName $destcloudsvc -VNetName $vnet

## <a name="existing-deployments-that-do-not-use-always-on-availability-groups"></a>Befintliga distributioner som inte använder alltid på Tillgänglighetsgrupper
> [!NOTE]
> Befintliga distributioner först finns hello [krav](#prerequisites-for-premium-storage) i det här avsnittet.
>
>

Det finns olika överväganden för SQL Server-distributioner som inte använder alltid på Tillgänglighetsgrupper och det är möjligt. Om du inte använder alltid på och har en befintlig fristående SQL Server kan du uppgradera tooPremium lagring med hjälp av ett nytt cloud service och storage-konto. Tänk hello följande alternativ:

* **Skapa en ny SQL Server-VM**. Du kan skapa en ny SQL Server-VM som använder ett premiumlagringskonto, enligt beskrivningen i nya distributioner. Sedan säkerhetskopiera och återställa SQL Server-databaserna konfiguration och användare. hello program behöver uppdateras toobe tooreference hello nya SQL-servern om den används internt eller externt. Behöver du toocopy alla out-of-db-objekt som om du höll på en sida vid sida (SxS) SQL Server-migrering. Detta inkluderar objekt som inloggningar, certifikat och länkade servrar.
* **Migrera en befintlig SQL Server-VM**. Detta kräver att hello SQL Server-VM tas offline och sedan överföra den tooa nya Molntjänsten, som innehåller kopierar alla dess anslutna virtuella hårddiskar toohello Premium Storage-konto. När hello VM är online kommer programmet hello referera hello värdnamn för server som innan. Tänk på att hello hello befintliga diskens storlek påverkas hello prestandaegenskaper. En 400 GB disk hämtar avrundat tooa P20. Om du vet att du inte behöver som diskprestanda, kan du återskapa hello VM som virtuell dator DS-serien och bifoga Premium Storage virtuella hårddiskar på hello storlek och prestanda-specifikation som du behöver. Sedan kan du koppla från och Återanslut hello SQL DB-filer.

> [!NOTE]
> När kopierar hello VHD-diskar som du bör vara medveten om hello storlek, beroende på hello storlek innebär vilka Premium Storage disktyp de hör till, detta avgör disk prestanda-specifikationen. Kommer Azure att avrunda uppåt toohello närmsta disk storlek, så om du har en 400 GB disk, detta kommer att avrundas uppåt tooa P20. Beroende på din befintliga i/o-kraven i hello OS-VHD, kanske du inte behöver toomigrate denna tooa Premium Storage-konto.
>
>

Om din SQL Server används externt ändras hello cloud service VIP. Du har också tooupdate slutpunkter, ACL: er och DNS-inställningar.

## <a name="existing-deployments-that-use-always-on-availability-groups"></a>Befintliga distributioner som använder alltid på Tillgänglighetsgrupper
> [!NOTE]
> Befintliga distributioner först finns hello [krav](#prerequisites-for-premium-storage) i det här avsnittet.
>
>

Först ska vi titta på hur alltid på samverkar med Azure-nätverk i det här avsnittet. Vi kommer sedan att dela upp migreringar i tootwo scenarier: där vissa avbrott kan tillåtas migreringar och migreringar där du måste uppnå minimal avbrottstid.

Lokal SQL Server alltid på Tillgänglighetsgrupper använder en lyssnare lokalt som registrerar ett virtuella DNS-namn tillsammans med en IP-adress som delas mellan en eller flera SQL-servrar. När klienter ansluter dirigeras de via hello lyssnare IP toohello primära SQL-servern. Detta är hello-servern som äger hello alltid på IP-resurs som för närvarande.

![DeploymentsUseAlways på][6]

I Microsoft Azure kan du ha endast en IP-adress som tilldelats tooa NIC på hello VM, i ordning tooachieve Hej samma lager Abstraktionslager som lokalt, Azure använder hello IP-adress som är tilldelad toohello intern/extern belastningsutjämnare (ILB/ELB). hello IP-resurs som delas mellan hello servrar anges toohello samma IP-adress som hello ILB/ELB. Det har publicerats i hello DNS och klienttrafik överförs via hello ILB/ELB toohello primära SQL Server-repliken. Hej ILB/ELB vet vilken SQL Server är primär eftersom den använder avsökningar tooprobe hello alltid på IP-resurs. I föregående exempel hello den avsökningar varje nod som har en slutpunkt som refereras av hello ELB/ILB, beroende på vilket som svarar är hello primära SQL-servern.

> [!NOTE]
> hello ILB och ELB båda tilldelas tooa viss Azure cloud service, alla molnmigrering i Azure innebär därför troligen att hello Load Balancer IP ändras.
>
>

### <a name="migrating-always-on-deployments-that-can-allow-some-downtime"></a>Migrera alltid på distributioner som kan tillåta att vissa avbrott
Det finns två strategier toomigrate alltid distributioner så att vissa avbrott:

1. **Lägga till flera sekundära repliker tooan befintliga alltid på klustret**
2. **Migrera tooa nya alltid på klustret**

#### <a name="1-add-more-secondary-replicas-tooan-existing-always-on-cluster"></a>1. Lägga till flera sekundära repliker tooan befintliga alltid på klustret
En strategi är tooadd toohello mer sekundärservrar Always On-Tillgänglighetsgruppen. Du behöver tooadd dem i en ny molntjänst och uppdatera hello lyssnare med hello nya belastningen belastningsutjämnaren IP.

##### <a name="points-of-downtime"></a>Punkter stillestånd:
* Klusterverifieringen.
* Testa alltid på redundans för nya sekundärservrar.

Om du använder Windows lagringspooler inom hello VM för högre i/o-genomflöde kommer sedan dessa att kopplas från under en fullständig verifiering av klustret. hello verifieringstest krävs när du lägger till noder toohello klustret. hello tid det tar toorun hello test kan variera, så bör du testa detta i din miljö representativt test-tooget en ungefärlig tid för hur lång tid detta tar.

Du måste etablera tid där du kan utföra manuell växling vid fel och chaos tester på hello nyligen lagt till noder tooensure alltid på hög tillgänglighet funktioner som förväntat.

![DeploymentUseAlways On2][7]

> [!NOTE]
> Du bör avsluta alla instanser av SQL Server där hello lagringspooler används innan hello validering körs.
>
> ##### <a name="high-level-steps"></a>Anvisningar
>

1. Skapa två nya SQL-servrar i ny molntjänst med anslutna Premium-lagring.
2. Kopiera över fullständig säkerhetskopiering och återställning med **NORECOVERY**.
3. Kopiera över 'utanför user DB' beroende objekt, till exempel inloggningar osv.
4. Skapa en ny intern belastning belastningsutjämnare (ILB) eller använda en extern belastningen belastningsutjämnare (ELB) och sedan ställa in belastningen belastningsutjämnade slutpunkter på både nya noder.

   > [!NOTE]
   > Kontrollera att alla noder har hello rätt slutpunktskonfiguration innan du fortsätter
   >
   >
5. Stoppa användare/programåtkomst toohello SQL Server (om du använder lagringspooler).
6. Stoppa SQL Server Engine-tjänster på alla noder (om du använder lagringspooler).
7. Lägg till nya noder toocluster och kör fullständig verifiering.
8. När verifieringen är klar startar du alla SQL Server-tjänster.
9. Säkerhetskopiera transaktionsloggar och återställa databaserna.
10. Lägg till nya noder i hello alltid på Tillgänglighetsgruppen och placera replikering till **synkron**.
11. Lägg till hello IP-adressresurs av hello nya Cloud Service ILB/ELB via PowerShell för Always On baserat på hello flera platser exemplet i hello [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage). Ange hello i Windows-kluster **möjliga ägare** av hello **IP-adress** resurs toohello nya noder gamla. Avsnittet hello ”lägga till IP-adressresurs i samma undernät' i hello [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).
12. Redundans tooone hello nya noder.
13. Se hello nya noder automatiskt Redundanspartners och redundanstestningen.
14. Ta bort ursprungliga noder från Tillgänglighetsgruppen.

##### <a name="advantages"></a>Fördelar
* Ny SQL-servrar kan vara testas (SQL Server och program) innan de läggs tooAlways på.
* Du kan ändra hello VM-storlek och anpassa hello tooyour exakt lagringsbehov. Det kan dock vara bra tookeep alla hello SQL sökvägar hello samma.
* Du kan styra när hello överföringen av hello DB säkerhetskopieringar toohello sekundära repliker har startats. Detta skiljer sig från att använda Azure **Start AzureStorageBlobCopy** kommandot toocopy virtuella hårddiskar, eftersom det är en asynkron kopia.

##### <a name="disadvantages"></a>Nackdelar
* När du använder Windows-lagringspooler finns klusterdriftstopp under hello fullständig Klusterverifieringen för hello nya ytterligare noder.
* Beroende på hello SQL Server-Version och hello befintliga antalet sekundära repliker, kan inte vara kan tooadd flera sekundära repliker utan att ta bort befintliga sekundärservrar.
* Det kan finnas långa SQL dataöverföringstid när du konfigurerar hello sekundärservrar.
* Det finns ytterligare kostnad under migreringen när du har nya datorer som körs parallellt.

#### <a name="2-migrate-tooa-new-always-on-cluster"></a>2. Migrera tooa nya alltid på klustret
En annan strategi är toocreate en helt ny alltid på klustret med helt nya noder i ny molntjänst och omdirigering hello klienter toouse den.

##### <a name="points-of-downtime"></a>Punkter stillestånd
Det finns en avbrottstid när du överför program och användare toohello alltid på lyssnaren. hello driftstopp beror på:

* hello tidsåtgång toorestore slutliga transaction log säkerhetskopieringar toodatabases på nya servrar.
* hello tidsåtgång tooupdate klienten program toouse alltid på lyssnaren.

##### <a name="advantages"></a>Fördelar
* Du kan testa hello faktiska produktionsmiljön, SQL Server och Operativsystemets build-ändringar.
* Du har hello alternativet toocustomize hello lagring och toopotentially minska storleken för den virtuella datorn. Det kan leda till minskad kostnaden.
* Under den här processen kan du uppdatera din version eller SQL Server-version. Du kan också uppgradera hello operativsystem.
* hello tidigare alltid på kluster kan fungera som ett fast rollback-mål.

##### <a name="disadvantages"></a>Nackdelar
* Du måste toochange hello DNS-namnet på hello lyssnare om du vill att båda Always On-kluster körs samtidigt. Detta lägger till administration försämras under migreringen hello som klienten programmet strängar måste återspeglar hello ny lyssnare namn.
* Du måste implementera en synkroniseringsmekanism för mellan hello två miljöer tookeep dem som nära som möjligt toominimize hello slutlig synkroniseringskrav innan migreringen.
* Det läggs till kostnad under migreringen när du har hello nya miljö körs.

### <a name="migrating-always-on-deployments-for-minimal-downtime"></a>Migrera alltid på distributioner för minimal driftstörning
Det finns två metoder för att migrera Always On-distributioner för minimal driftstörning:

1. **Använda en befintlig sekundär: enskild plats**
2. **Använda befintlig sekundär tillgänglighetsreplik(er): flera platser**

#### <a name="1-utilize-an-existing-secondary-single-site"></a>1. Använda en befintlig sekundär: enskild plats
En strategi för minimal driftstörning är tootake ett befintligt moln sekundära och ta bort den från hello aktuella tjänst i molnet. Kopiera hello virtuella hårddiskar toohello nya Premium Storage-konto och skapa hello VM i hello ny molntjänst. Uppdatera hello lyssnare i kluster och växling vid fel.

##### <a name="points-of-downtime"></a>Punkter stillestånd
* Det finns en avbrottstid när du uppdaterar hello sista noden med hello belastningsutjämnade slutpunkt.
* Klient-återanslutning kan fördröjas beroende på din klient/DNS-konfiguration.
* Det finns ytterligare driftstopp om du väljer tootake hello alltid på klustret grupp offline tooswap ut hello IP-adresser. Du kan undvika detta genom att använda ett beroende eller och möjliga ägare för hello läggs IP-adressresurs. Avsnittet hello ”lägga till IP-adressresurs i samma undernät' i hello [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).

> [!NOTE]
> När du vill hello tillagda noden toopartake i som alltid på Failover-Partner, måste tooadd en Azure-slutpunkt med en referens toohello belastningen belastningsutjämnade anges. När du kör hello **Lägg till AzureEndpoint** kommandot toodo detta, öppna tooremain i aktuella anslutningar, men anslutningar toohello lyssnaren inte kan toobe upprätta förrän hello belastningsutjämnaren har uppdaterats. Testning av detta visas toolast 90-120seconds, detta bör testas.
>
>

##### <a name="advantages"></a>Fördelar
* Inga extra kostnaden under migreringen.
* En-till-en migrering.
* Minskad komplexitet.
* Ger ökad IOPS från Premium Storage SKU: er. När hello diskar har kopplats från hello VM och kopierade toohello ny molntjänst en 3 part att verktyget använda tooincrease hello VHD storlek, vilket ger högre genomflöden. Öka storleken på VHD finns i följande [forumdiskussion](https://social.msdn.microsoft.com/Forums/azure/4a9bcc9e-e5bf-4125-9994-7c154c9b0d52/resizing-azure-data-disk?forum=WAVirtualMachinesforWindows).

##### <a name="disadvantages"></a>Nackdelar
* Det finns en temporär förlust av hög tillgänglighet och Katastrofåterställning under migreringen.
* Eftersom det är en 1:1-migrering måste toouse en minsta VM-storlek som stöder antal virtuella hårddiskar, så du inte kanske kan toodownsize dina virtuella datorer.
* Det här scenariot använder hello Azure **Start AzureStorageBlobCopy** cmdlet, som är asynkron. Det finns inga SLA på Kopiera slutförande. hello tid hello kopior varierar, medan det beror på vänta i kön beror också på hello mängd data tootransfer. hello kopiera tid ökar om hello överföring ska tooanother Azure-datacenter som har stöd för Premium-lagring i en annan region. Om du har precis 2 noder kan du en möjlig lösning om hello kopiera tar längre tid än vid testning. Detta kan omfatta hello följande idéer.
  * Lägga till en tillfällig 3 SQL Server-nod för hög tillgänglighet innan hello migreringen överenskomna avbrott.
  * Kör hello migrering utanför Azure schemalagt underhåll.
  * Kontrollera att du har konfigurerat klustrets kvorum korrekt.  

##### <a name="high-level-steps"></a>Anvisningar
Det här dokumentet visar inte en fullständig slutet tooend exempelvis men hello [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage) innehåller information som kan vara balanserad tooperform detta.

![MinimalDowntime][8]

* Samla diskkonfigurationen och ta bort hello nod (ta inte bort anslutna virtuella hårddiskar).
* Skapa ett premiumlagringskonto-lagring och kopiera virtuella hårddiskar från hello standardlagringskonto
* Skapa ny molntjänst och omdistribuera hello SQL2 VM i Molntjänsten. Skapa hello VM med hjälp av hello kopieras ursprungliga OS VHD och bifoga hello kopieras virtuella hårddiskar.
* Konfigurera ILB / ELB och lägga till slutpunkter.
* Uppdatera lyssnare genom att antingen:
  * Tar hello alltid på gruppen offline och uppdaterar hello alltid på lyssnare med nya ILB / ELB IP-adress.
  * Eller lägga till hello IP-adressresurs av nya Cloud Service ILB/ELB via PowerShell i Windows-kluster. Sedan set hello möjliga ägare till hello IP-adress resurs toohello migreras nod, SQL2, och ange som hello nätverksnamn eller beroendet. Avsnittet hello ”lägga till IP-adressresurs i samma undernät' i hello [bilaga](#appendix-migrating-a-multisite-always-on-cluster-to-premium-storage).
* Kontrollera DNS-konfiguration/spridningsuppgift toohello klienter.
* Migrera SQL1 VM och gå igenom steg 2 – 4.
* Om du använder steg 5ii, Lägg sedan till SQL1 möjlig ägare till hello lägga till IP-adressresurs
* Redundanstestningen.

#### <a name="2-utilize-existing-secondary-replicas-multi-site"></a>2. Använda befintlig sekundär tillgänglighetsreplik(er): flera platser
Om du har noder i mer än en Azure-datacenter (DC) eller om du har en hybridmiljö kan du använda en Always On-konfiguration i den här miljön toominimize driftstopp.

hello-metoden är toochange hello alltid på synkronisering tooSynchronous för hello lokala eller sekundära Azure Domänkontrollant och sedan redundans över toothat SQL Server. Kopiera hello virtuella hårddiskar tooa Premium Storage-konto och distribuera om hello datorn till en ny molntjänst. Uppdatera hello-lyssnare och växlar sedan tillbaka.

##### <a name="points-of-downtime"></a>Punkter stillestånd
hello driftstopp består av hello tid toofailover toohello alternativ DC och tillbaka. Det beror också på klienten eller DNS-konfigurationen och klient-återanslutning kan vara fördröjd.
Tänk hello följande exempel på en hybrid Always On-konfiguration:

![MultiSite1][9]

##### <a name="advantages"></a>Fördelar
* Du kan använda befintliga infrastruktur.
* Du har hello alternativet toopre uppgraderingen hello Azure storage på hello DR Azure DC först.
* hello DR Azure DC-lagring kan konfigureras.
* Det finns minst två växling vid fel under migreringen, exklusive redundanstestning.
* Du inte behöver toomove SQL Server-data med säkerhetskopiering och återställning.

##### <a name="disadvantages"></a>Nackdelar
* Beroende på klienten åtkomst tooSQL Server, kan det vara ökad latens när SQL Server körs i ett alternativt DC toohello program.
* hello kopiera tiden för virtuella hårddiskar tooPremium lagring kan vara lång. Detta kan påverka ditt beslut om huruvida tookeep hello nod i hello Availability Group. Tänk på detta för när loggen arbetar belastningar körs under migreringen hello krävs, eftersom hello primära noden måste tookeep hello unreplicated transaktioner i dess transaktionsloggen. Därför kan detta växa avsevärt.
* Det här scenariot använder hello Azure **Start AzureStorageBlobCopy** cmdlet, som är asynkron. Det finns inga SLA på slutförande. hello tid hello kopior varierar, medan det beror på vänta i kön, beror också på hello mängd data tootransfer. Därför ha bara en nod i datacentret 2, du bör vidta säkerhetsåtgärder om hello kopiera tar längre tid än vid testning. Detta kan omfatta hello följande idéer.
  * Lägga till en tillfällig SQL-nod 2 för hög tillgänglighet innan hello migreringen överenskomna avbrott.
  * Kör hello migrering utanför Azure schemalagt underhåll.
  * Kontrollera att du har konfigurerat klustrets kvorum korrekt.

Det här scenariot förutsätter att du har dokumenterats din installation och vet hur hello lagring mappas i ordning toomake ändringar för den diskens cacheinställningarna.

##### <a name="high-level-steps"></a>Anvisningar
![Multisite2][10]

* Se hello lokalt / alternativa Azure DC hello SQL Server primära och gör den hello andra automatisk redundans Partner (AFP).
* Samla in information om diskkonfiguration från SQL2 och ta bort hello nod (ta inte bort anslutna virtuella hårddiskar).
* Skapa ett premiumlagringskonto-lagring och kopiera virtuella hårddiskar från hello standardlagringskonto.
* Skapa en ny molntjänst och skapa hello SQL2 VM med dess lagring Premier-diskar som är anslutna.
* Konfigurera ILB / ELB och lägga till slutpunkter.
* Uppdatera hello alltid på lyssnare med nya ILB / ELB IP-adress och testa redundans.
* Kontrollera hello DNS-konfiguration.
* Ändra hello AFP tooSQL2 och migrera SQL1 och gå igenom steg 2 – 5.
* Redundanstestningen.
* Växla hello AFP tillbaka tooSQL1 och SQL2

## <a name="appendix-migrating-a-multisite-always-on-cluster-toopremium-storage"></a>Bilaga: Migrera en Multisite alltid på klustret tooPremium lagring
hello resten av det här avsnittet innehåller ett detaljerat exempel konvertera en flera platser alltid på tooPremium klusterlagringen. Den konverterar också hello lyssnare från att använda en extern belastningsutjämnare (ELB) tooan interna belastningsutjämnare (ILB).

### <a name="environment"></a>Miljö
* Windows 2k 12 / SQL 2k 12
* 1 DB-filer på SP
* 2 x lagringspooler per nod

![Appendix1][11]

### <a name="vm"></a>VM:
I det här exemplet ska vi ska toodemonstrate flyttar från en ELB tooILB. ELB fanns innan ILB, så att det visar hur hello tooswitch toothis under migreringen.

![Appendix2][12]

### <a name="pre-steps-connect-toosubscription"></a>Pre steg: Ansluta tooSubscription
    Add-AzureAccount

    #Set up subscription
    Get-AzureSubscription

#### <a name="step-1-create-new-storage-account-and-cloud-service"></a>Steg 1: Skapa nytt Lagringskonto och Molntjänsten
    $mysubscription = "DansSubscription"
    $location = "West Europe"

    #Storage accounts
    #current storage account where hello vm toomigrate resides
    $origstorageaccountname = "danstdams"

    #Create Premium Storage account
    $newxiostorageaccountname = "danspremsams"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountname -Location $location -Type "Premium_LRS"  

    #Generate storage keys for later
    $originalstorage =  Get-AzureStorageKey -StorageAccountName $origstorageaccountname
    $xiostorage = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountname

    #Generate storage acc contexts
    $origContext = New-AzureStorageContext  –StorageAccountName $origstorageaccountname -StorageAccountKey $originalstorage.Primary
    $xioContext = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountname -StorageAccountKey $xiostorage.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $origstorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #CREATE NEW CLOUD SVC
    $vnet = "dansvnetwesteur"

    ##Existing cloud service
    $sourceSvc="dansolSrcAms"

    ##Create new cloud service
    $destcloudsvc = "danNewSvcAms"
    New-AzureService $destcloudsvc -Location $location

#### <a name="step-2-increase-hello-permitted-failures-on-resources-optional"></a>Steg 2: Öka hello tillåtna fel för resurser<Optional>
På vissa resurser som tillhör tooyour Always On-Tillgänglighetsgruppen finns begränsningar på hur många fel som kan uppstå under en period där hello görs försök toorestart hello resursgruppen. Det rekommenderas att du ökar det samtidigt som du går igenom den här proceduren eftersom om du inte manuellt redundans och utlösare redundans genom att stänga av datorer som du kan hämta Stäng toothis gränsen.

Det skulle vara försiktig toodouble hello fel ersättning toodo detta i hanteraren för redundanskluster, gå toohello hello alltid på resursgruppens egenskaper:

![Appendix3][13]

Ändra hello maximalt antal misslyckanden too6.

#### <a name="step-3-addition-ip-address-resource-for-cluster-group-optional"></a>Steg 3: Lägga IP-adressresurs för klustergruppen<Optional>
Om du har en enda IP-adress för hello klustergrupp och detta är justerade toohello moln undernät, varning, om du av misstag koppla från alla noder i klustret i hello molnet att nätverket och sedan hello klustrets IP-resurs och Klusternätverksnamnet inte kommer att kunna toocome online. I hello uppdaterar händelsen för detta hindrar det tooother klusterresurser.

#### <a name="step-4-dns-configuration"></a>Steg 4: DNS-konfiguration
tooimplement en smidig övergång beror på hur DNS används utnyttjade och uppdateras.
När alltid är installerat, skapas en klusterresurs för Windows-grupp om du öppnar Klusterhanteraren för växling visas att åtminstone den har tre resurser, hello två som hello dokumentet refererar tooare:

* Virtuellt nätverksnamn (VNN) – detta är hello DNS-namn som klienten ansluta toowhen förskjutning tooconnect tooSQL servrar via alltid på.
* IP-adressresurs – det här är IP-adressen som som är associerade med hello VNN hello, du kan ha flera och i multisitekonfigurationen har du en IP-adress per plats-undernät.

När anslutande tooSQL Server, hello-drivrutin för SQL Server-klienten hämtar hello DNS-poster som är associerade med hello-lyssnare och försök tooconnect tooeach alltid på associerade IP-adress, nedan diskuterar vi några faktorer som kan påverka detta.

hello antalet samtidiga DNS-poster som är associerade med hello grupplyssnarens namn beror inte bara på hello antalet IP-adresser som är kopplade men hello ' RegisterAllIpProviders'setting i redundanskluster för hello Always ON VNN resurs.

När du distribuerar alltid på i Azure finns olika steg toocreate hello lyssnare och IP-adresser, du toomanually konfigurera hello 'RegisterAllIpProviders' too1, detta är olika tooan lokalt alltid på distribution där det redan har angetts too1.

'RegisterAllIpProviders' är 0, och sedan visas bara en DNS-post i DNS som är associerade med hello lyssnare:

![Appendix4][14]

Om 'RegisterAllIpProviders' är 1:

![Appendix5][15]

hello koden nedan kommer dump ut hello VNN inställningar och ange det åt dig, du Anmärkning för hello ändra tootake kraft du behöver tootake hello VNN offline och slå på den online igen, den här ta hello lyssnare offline orsakar klienten anslutning avbrott.

    ##Always On Listener Name
    $ListenerName = "Mylistener"
    ##Get AlwaysOn Network Name Settings
    Get-ClusterResource $ListenerName| Get-ClusterParameter
    ##Set RegisterAllProvidersIP
    Get-ClusterResource $ListenerName| Set-ClusterParameter RegisterAllProvidersIP  1

I ett senare migreringssteg måste tooupdate hello alltid på lyssnare med en uppdaterad IP-adress som ska referera till en belastningsutjämnare, detta kommer att omfatta en IP-adress resurs borttagning och tillägg. När du hello IP-uppdatering måste tooensure hello nya IP-adress har uppdaterats i DNS-zonen och att hello klienter uppdaterar sina lokala DNS-cacheminnet.

Om klienterna finns i olika nätverkssegment och referera till en annan DNS-server, måste tooconsider vad händer om DNS-zonöverföring under migreringen hello hello programmet återansluta ska vara begränsad tid med minst hello zonen överför tid för alla nya IP-adresser för hello-lyssnaren. Om du är under tidsbegränsning här du ska diskutera och testa att tvinga en inkrementell zonöverföring med Windows-grupper, och också hello DNS värden poster tooa minskar Time tooLive (TTL), så att uppdatera hello-klienter. Mer information finns i [inkrementella zonöverföringar](https://technet.microsoft.com/library/cc958973.aspx) och [Start DnsServerZoneTransfer](https://technet.microsoft.com/library/jj649917.aspx).

Som standard hello TTL-värde för DNS-post som är associerad med hello lyssnare i alltid på i Azure är 1200 sekunder. Vill du kanske tooreduce detta om du är under tidsbegränsning under migreringen tooensure hello klienter uppdateringen deras DNS med hello uppdatera IP-adressen för hello-lyssnaren. Du kan se och ändra hello konfigurationen av dumpning ut hello konfigurationen av hello VNN:

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"
    #Look at HostRecordTTL
    Get-ClusterResource $ListenerName| Get-ClusterParameter

    #Set HostRecordTTL Examples
    Get-ClusterResource $ListenerName| Set-ClusterParameter -Name "HostRecordTTL" 120

Observera hello lägre hello HostRecordTTL, inträffar ett högre värde för DNS-trafik.

##### <a name="client-application-settings"></a>Inställningar för program
Om din SQL-klienten programmet stöder hello .net 4.5 SQLClient och du kan använda ' MULTISUBNETFAILOVER = TRUE ”nyckelord, detta rekommenderas toobe tillämpas så som den kan användas för snabbare anslutning tooSQL Always On-Tillgänglighetsgruppen under växling vid fel. Den räknar igenom alla IP-adresser som är associerade med hello alltid lyssnare för parallellt och utför en mer aggressiv TCP försök anslutningshastighet under en växling vid fel.

Mer information om hello inställningarna ovan finns [MultiSubnetFailover nyckelord och associerade funktioner](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover). Se även [SqlClient-stöd för hög tillgänglighet och katastrofåterställning](https://msdn.microsoft.com/library/hh205662\(v=vs.110\).aspx).

#### <a name="step-5-cluster-quorum-settings"></a>Steg 5: Inställningarna för klusterkvorum
Som du kommer toobe tar reda på minst en SQL Server ned samtidigt, bör du ändra hello kvoruminställningen för klustret, om filen filresurs vittne (FSW) med 2 noder, ska du ange hello kvorum tooallow Nodmajoritet och använda dynamiska röstning och det är tooallow för en enskild nod tooremain position.

    Set-ClusterQuorum -NodeMajority  

Mer information om att hantera och konfigurera hello klustrets kvorum finns [konfigurera och hantera hello kvorum i ett redundanskluster för Windows Server 2012](https://technet.microsoft.com/library/jj612870.aspx).

#### <a name="step-6-extract-existing-endpoints-and-acls"></a>Steg 6: Extrahera befintliga slutpunkter och ACL: er
    #GET Endpoint info
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureEndpoint
    #GET ACL Rules for Each EP, this example is for hello Always On Endpoint
    Get-AzureVM -ServiceName $destcloudsvc -Name $vmNameToMigrate | Get-AzureAclConfig -EndpointName "myAOEndPoint-LB"  

Spara dessa tooa textfil.

#### <a name="step-7-change-failover-partners-and-replication-modes"></a>Steg 7: Ändra Redundanspartners och replikering lägen
Om du har mer än 2 SQL-servrar du bör ändra hello växling vid fel på en annan sekundär i en annan Domänkontrollant eller lokala too'Synchronous' och göra det en automatisk redundans Partner (AFP), det är så du upprätthålla hög tillgänglighet, samtidigt som du gör ändringar. Du kan göra detta via TSQL av dock ändra SSMS:

![Appendix6][16]

#### <a name="step-8-remove-secondary-vm-from-cloud-service"></a>Steg 8: Ta bort sekundära virtuella datorn från Molntjänsten
Du bör planera toomigrate ett moln sekundära noden först om det är för närvarande primära du ska initiera en manuell växling vid fel.

    $vmNameToMigrate="dansqlams2"

    #Check machine status
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Shutdown secondary VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM


    #Extract disk configuration

    ##Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk config, make sure below returns hello disks associated with hello VM
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machibe is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild toonew cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-9-change-disk-caching-settings-in-csv-file-and-save"></a>Steg 9: Ändra disken cacheinställningar i CSV-filen och spara
Dessa bör anges tooREADONLY för datavolymer.

Dessa bör anges tooNONE för TLOG volymer.

![Appendix7][17]

#### <a name="step-10-copy-vhds"></a>Steg 10: Kopiera virtuella hårddiskar
    #Ensure you have created hello container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContext

    ####DISK COPYING####
    #Get disks from csv, get settings for each VHDs and copy tooPremium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname.blob.core.windows.net/vhds/$vhdname" `
    -SrcContext $origContext `
    -DestContainer $containerName `
    -DestBlob $vhdname `
    -DestContext $xioContext
       }



Du kan kontrollera statusen för hello kopia av hello virtuella hårddiskar toohello Premium Storage-konto:

    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContext
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix8][18]

Vänta tills alla dessa registreras åtgärden lyckades.

Information för enskilda BLOB:

    Get-AzureStorageBlobCopyState -Blob "blobname.vhd" -Container $containerName -Context $xioContext

#### <a name="step-11-register-os-disk"></a>Steg 11: Registrera OS-disk
    #Change storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountname
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

#### <a name="step-12-import-secondary-into-new-cloud-service"></a>Steg 12: Importera sekundär till ny molntjänst
hello koden nedan även använder hello läggas till här alternativ du kan importera hello dator och använda hello retainable VIP.

    #Build VM Config
    $ipaddr = "192.168.0.5"
    #Remember toochange tooXIO
    $newInstanceSize = "Standard_DS13"
    $subnet = "SQL"

    #Create new Avaiability Set
    $availabilitySet = "cloudmigAVAMS"

    #build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###Attaching disks tooa VM during a deploy tooa new cloud service and new storage account is different from just attaching VHDs toojust a redeploy in a new cloud service
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountname.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet ## Optional (-ReservedIPName $reservedVIPName)

#### <a name="step-13-create-ilb-on-new-cloud-svc-add-load-balanced-endpoints-and-acls"></a>Steg 13: Skapa ILB på nya molntjänster Svc lägga till belastningen belastningsutjämnade slutpunkter och ACL: er
    #Check for existing ILB
    GET-AzureInternalLoadBalancer -ServiceName $destcloudsvc

    $ilb="sqlIntIlbDest"
    $subnet = "SQL"
    $IP="192.168.0.25"
    Add-AzureInternalLoadBalancer -ServiceName $destcloudsvc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP

    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM

    #SET Azure ACLs or Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

    ####WAIT FOR FULL AlwaysOn RESYNCRONISATION!!!!!!!!!#####

#### <a name="step-14-update-always-on"></a>Steg 14: Uppdatera alltid på
    #Code toobe executed on a Cluster Node
    $ClusterNetworkNameAmsterdam = "Cluster Network 2" # hello azure cluster subnet network name
    $newCloudServiceIPAmsterdam = "192.168.0.25" # IP address of your cloud service

    $AGName = "myProductionAG"
    $ListenerName = "Mylistener"


    Add-ClusterResource "IP Address $newCloudServiceIPAmsterdam" -ResourceType "IP Address" -Group $AGName -ErrorAction Stop |  Set-ClusterParameter -Multiple @{"Address"="$newCloudServiceIPAmsterdam";"ProbePort"="59999";SubnetMask="255.255.255.255";"Network"=$ClusterNetworkNameAmsterdam;"OverrideAddressMatch"=1;"EnableDhcp"=0} -ErrorAction Stop

    #set dependancy and NETBIOS, then remove old IP address

    #set NETBIOS, then remove old IP address
    Get-ClusterGroup $AGName | Get-ClusterResource -Name "IP Address $newCloudServiceIPAmsterdam" | Set-ClusterParameter -Name EnableNetBIOS -Value 0

    #set dependency tooListener (OR Dependency) and delete previous IP Address resource that references:

    #Make sure no static records in DNS

![Appendix9][19]

Nu ska du ta bort Molntjänsten för hello gamla IP-adress.

![Appendix10][20]

#### <a name="step-15-dns-update-check"></a>Steg 15: DNS-uppdateringskontroll
Du bör nu se DNS-servrar på SQL Server-klient-nätverk och kontrollera att kluster har lagts till hello extra värdpost för hello lägga till IP-adress. Om dessa DNS-servrar inte har uppdaterat, Överväg att tvinga en DNS-zonöverföring och kontrollera att hello klienter i undernätet är kan tooresolve tooboth alltid på IP-adresser, det är så du inte behöver toowait automatisk DNS-replikeringen.

#### <a name="step-16-reconfigure-always-on"></a>Steg 16: Konfigurera om alltid på
Då vänta du hello sekundära noden som var migrerade toofully omsynkronisering med hello lokala nod och växla toosynchronous replikeringsnod och gör det hello AFP.  

#### <a name="step-17-migrate-second-node"></a>Steg 17: Migrera andra noden
    $vmNameToMigrate="dansqlams1"

    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

    #Get endpoint information
    $endpoint = Get-AzureVM -ServiceName $sourceSvc  -Name $vmNameToMigrate | Get-AzureEndpoint

    #Shutdown VM
    Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | stop-AzureVM

    #Get disk config

    #Building Existing Data Disk Configuration
    $file = "C:\Azure Storage Testing\mydiskconfig_$vmNameToMigrate.csv"
    $datadisks = @(Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureDataDisk )
    Add-Content $file “lun, vhdname, hostcaching, disklabel, diskName”
    foreach ($disk in $datadisks)
    {
      $vhdname = $disk.MediaLink.AbsolutePath -creplace  "/vhds/"
      $disk.Lun, , $disk.HostCaching, $vhdname, $disk.DiskLabel,$disks.DiskName
    # Write-Host "copying disk $disk"
    $adddisk = "{0},{1},{2},{3},{4}" -f $disk.Lun,$vhdname, $disk.HostCaching, $disk.DiskLabel, $disk.DiskName
    $adddisk | add-content -path $file
    }

    #Get OS Disk
    $osdisks = Get-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate | Get-AzureOSDisk ## | select -ExpandProperty MediaLink
    $osvhdname = $osdisks.MediaLink.AbsolutePath -creplace  "/vhds/"
    $osdisks.OS, $osdisks.HostCaching, $osvhdname, $osdisks.DiskLabel, $osdisks.DiskName
    $addosdisk = "{0},{1},{2},{3},{4}" -f $osdisks.OS,$osvhdname, $osdisks.HostCaching, $osdisks.Disklabel , $osdisks.DiskName
    $addosdisk | add-content -path $file

    #Import disk config
    $diskobjects  = Import-CSV $file

    #Check disk configuration
    $diskobjects

    #Identify OS Disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osdiskforbuild = $osdiskimport.diskName

    #Check machine is off
    Get-AzureVM -ServiceName $sourceSvc -Name  $vmNameToMigrate

    #Drop machine and rebuild toonew cls
    Remove-AzureVM -ServiceName $sourceSvc -Name $vmNameToMigrate

#### <a name="step-18-change-disk-caching-settings-in-csv-file-and-save"></a>Steg 18: Ändra disken cacheinställningar i CSV-filen och spara
Dessa bör anges tooREADONLY för datavolymer.

Dessa bör anges tooNONE för TLOG volymer.

![Appendix11][21]

#### <a name="step-19-create-new-independent-storage-account-for-secondary-node"></a>Steg 19: Skapa nytt oberoende Lagringskonto för sekundär nod
    $newxiostorageaccountnamenode2 = "danspremsams2"
    New-AzureStorageAccount -StorageAccountName $newxiostorageaccountnamenode2 -Location $location -Type "Premium_LRS"  

    #Reset hello storage account src if node 1 in a different storage account
    $origstorageaccountname2nd = "danstdams2"

    #Generate storage keys for later
    $xiostoragenode2 = Get-AzureStorageKey -StorageAccountName $newxiostorageaccountnamenode2

    #Generate storage acc contexts
    $xioContextnode2 = New-AzureStorageContext  –StorageAccountName $newxiostorageaccountnamenode2 -StorageAccountKey $xiostoragenode2.Primary  

    #Set up subscription and default storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

#### <a name="step-20-copy-vhds"></a>Steg 20: Kopiera virtuella hårddiskar
    #Ensure you have created hello container for these:
    $containerName = 'vhds'

    #Create container
    New-AzureStorageContainer -Name $containerName -Context $xioContextnode2  

    ####DISK COPYING####
    ##get disks from csv, get settings for each VHDs and copy tooPremium Storage accoun
    ForEach ($disk in $diskobjects)
       {
       $lun = $disk.Lun
       $vhdname = $disk.vhdname
       $cacheoption = $disk.HostCaching
       $disklabel = $disk.DiskLabel
       $diskName = $disk.DiskName
       Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname has cache setting : $cacheoption"

       #Start async copy
       Start-AzureStorageBlobCopy -srcUri "https://$origstorageaccountname2nd.blob.core.windows.net/vhds/$vhdname" `
        -SrcContext $origContext `
        -DestContainer $containerName `
        -DestBlob $vhdname `
        -DestContext $xioContextnode2
       }

    #Check for copy progress

    #check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContext


Du kan kontrollera hello VHD-kopian status för alla virtuella hårddiskar: ForEach ($disk i $diskobjects) {$lun = $disk. LUN $vhdname = $disk.vhdname $cacheoption = $disk. HostCaching $disklabel = $disk. Disketikett $diskName = $disk. DiskName

       $copystate = Get-AzureStorageBlobCopyState -Blob $vhdname -Container $containerName -Context $xioContextnode2
    Write-Host "Copying Disk Lun $lun, Label : $disklabel, VHD : $vhdname, STATUS = " $copystate.Status
       }

![Appendix12][22]

Vänta tills alla dessa registreras åtgärden lyckades.

Information för enskilda BLOB:

    #Check induvidual blob status
    Get-AzureStorageBlobCopyState -Blob "danRegSvcAms-dansqlams1-2014-07-03.vhd" -Container $containerName -Context $xioContextnode2

#### <a name="step-21-register-os-disk"></a>Steg 21: Registrera OS-disk
    #change storage account toohello new XIO storage account
    Set-AzureSubscription -SubscriptionName $mysubscription -CurrentStorageAccount $newxiostorageaccountnamenode2
    Select-AzureSubscription -SubscriptionName $mysubscription -Current

    #Register OS disk
    $osdiskimport = $diskobjects | where {$_.lun -eq "Windows"}
    $osvhd = $osdiskimport.vhdname
    $osdiskforbuild = $osdiskimport.diskName

    #Registering OS disk, but as XIO disk
    $xioDiskName = $osdiskforbuild + "xio"
    Add-AzureDisk -DiskName $xioDiskName -MediaLocation  "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$osvhd"  -Label "BootDisk" -OS "Windows"

    #Build VM Config
    $ipaddr = "192.168.0.4"
    $newInstanceSize = "Standard_DS13"

    #Join tooexisting Avaiability Set

    #Build machine config into object
    $vmConfig = New-AzureVMConfig -Name $vmNameToMigrate -InstanceSize $newInstanceSize -DiskName $xioDiskName -AvailabilitySetName $availabilitySet  ` | Add-AzureProvisioningConfig -Windows ` | Set-AzureSubnet -SubnetNames $subnet | Set-AzureStaticVNetIP -IPAddress $ipaddr

    #Reload disk config
    $diskobjects  = Import-CSV $file
    $datadiskimport = $diskobjects | where {$_.lun -ne "Windows"}

    ForEach ( $attachdatadisk in $datadiskimport)
       {
    $label = $attachdatadisk.disklabel
    $lunNo = $attachdatadisk.lun
    $hostcach = $attachdatadisk.hostcaching
    $datadiskforbuild = $attachdatadisk.diskName
    $vhdname = $attachdatadisk.vhdname

    ###This is different toojust a straight cloud service change
    #note if you do not have a disk label hello command below will fail, populate as required.
    $vmConfig | Add-AzureDataDisk -ImportFrom -MediaLocation "https://$newxiostorageaccountnamenode2.blob.core.windows.net/vhds/$vhdname" -LUN $lunNo -HostCaching $hostcach -DiskLabel $label

    }

    #Create VM
    $vmConfig  | New-AzureVM –ServiceName $destcloudsvc –Location $location -VNetName $vnet -Verbose

#### <a name="step-22-add-load-balanced-endpoints-and-acls"></a>Steg 22: Lägg till belastningen belastningsutjämnade slutpunkter och ACL: er
    #Endpoints
    $epname="sqlIntEP"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    Get-AzureVM –ServiceName $destcloudsvc –Name $vmNameToMigrate  | Add-AzureEndpoint -Name $epname -Protocol $prot -LocalPort $locport -PublicPort $pubport -ProbePort 59999 -ProbeIntervalInSeconds 5 -ProbeTimeoutInSeconds 11  -ProbeProtocol "TCP" -InternalLoadBalancerName $ilb -LBSetName $ilb -DirectServerReturn $true | Update-AzureVM


    #STOP!!! CHECK in hello Azure portal or Machine Endpoints through PowerShell that these Endpoints are created!

    #SET ACLs or Azure Network Security Groups & Windows FWs

    #http://msdn.microsoft.com/library/azure/dn495192.aspx

#### <a name="step-23-test-failover"></a>Steg 23: Testa redundans
Nu bör du låta hello migrerade nod synkronisera med hello lokalt alltid på noden, placera det i toosynchronous replikeringsläge och vänta tills den är synkroniserad. Sedan migreras växling från den första noden i lokal toohello, vilket är hello AFP. När du som har arbetat migreras ändra hello senast nod toohello AFP.

Du bör redundanstestningen mellan alla noder och kör om chaos testerna tooensure redundans fungerar som förväntat och en rimlig manor.

#### <a name="step-24-put-back-cluster-quorum-settings--dns-ttl--failover-pntrs--sync-settings"></a>Steg 24: Lägga tillbaka inställningarna för klusterkvorum / DNS TTL / Failover Pntrs / synkroniseringsinställningar
##### <a name="adding-ip-address-resource-on-same-subnet"></a>Lägger till IP-adressresurs på samma undernät
Om du har bara 2 SQL-servrar och vill toomigrate dem tooa nya Molntjänsten, men vill tookeep dem på hello samma undernät, kan du undvika tar du alltid offline toodelete hello ursprungliga hello lyssnare på IP-adress och lägga till hello nya IP-adressen. Om du migrerar hello VMs tooanother undernätet inte behöver du toodo detta som en ytterligare klusternätverk som kommer att referera till det undernätet.

När du har fört hello migreras sekundär och lagts till i hello nya IP-adressresurs för hello ny molntjänst innan redundans hello befintlig primär, bör du se dessa inom hello Klusterhanterare för växling vid fel:

tooadd i IP-adress finns hello [bilaga](#appendix-migrating-a-multisite-alwayson-cluster-to-premium-storage), steg 14.

1. Ändra hello möjlig ägare too'Existing primära SQL Server för hello aktuella IP-adressresurs ', i hello exemplet nedan 'dansqlams4':

    ![Appendix13][23]
2. Ändra hello möjlig ägare too'Migrated för hello nya IP-adressresurs, den sekundära SQL Server ”, i hello exemplet nedan 'dansqlams5':

    ![Appendix14][24]
3. När det här värdet kan du redundans och när hello sista noden migreras hello möjliga ägare måste redigeras så noden läggs till som möjlig ägare:

    ![Appendix15][25]

## <a name="additional-resources"></a>Ytterligare resurser
* [Azure Premium-lagring](../../../storage/common/storage-premium-storage.md)
* [Virtual Machines](https://azure.microsoft.com/services/virtual-machines/)
* [SQLServer i Azure-datorer](../sql/virtual-machines-windows-sql-server-iaas-overview.md)

<!-- IMAGES -->
[1]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/1_VNET_Portal.png
[2]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/2_Diskname_Lun.png
[3]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/3_Virtual_Disk_Properties.png
[4]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/4_Virtual_Disk_Properties_Details.png
[5]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/5_Get_Storage_Pool.png
[6]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/6_Deployments_Always_On.png
[7]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/7_Add_More_Secondaries.png
[8]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/8_Use_Existing_Secondary_Single_Site.png
[9]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site.png
[10]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/9_Use_Existing_Secondary_Multi_Site_b.png
[11]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_01.png
[12]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_02.png
[13]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_03.png
[14]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_04.png
[15]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_05.png
[16]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_06.png
[17]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_07.png
[18]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_08.png
[19]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_09.png
[20]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_10.png
[21]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_11.png
[22]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_12.png
[23]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_13.png
[24]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_14.png
[25]: ./media/virtual-machines-windows-classic-sql-server-premium-storage/10_Appendix_15.png
