---
title: aaaCreate SQL Server-dator i Azure PowerShell (klassisk) | Microsoft Docs
description: "Innehåller steg och PowerShell-skript för att skapa en virtuell Azure-dator med SQL Server virtuella galleriavbildningar. Det här avsnittet använder hello klassisk distribution läge."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-service-management
ms.assetid: b73be387-9323-4e08-be53-6e5928e3786e
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/07/2017
ms.author: jroth
ms.openlocfilehash: b14d5d9bc192fa0a21126395ee20ffd06b3bf47d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a>Etablera en virtuell dator med SQL Server med hjälp av Azure PowerShell (klassisk)

Den här artikeln innehåller steg för hur hello toocreate en SQL Server-dator i Azure med hjälp av PowerShell-cmdlets.

> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

Hello Resource Manager version av det här avsnittet finns [etablera en virtuell dator med SQL Server med Azure PowerShell Resource Manager](../sql/virtual-machines-windows-ps-sql-create.md).

### <a name="install-and-configure-powershell"></a>Installera och konfigurera PowerShell:
1. Om du inte har något Azure-konto besöker du sidan för [kostnadsfria utvärderingsversioner av Azure](https://azure.microsoft.com/pricing/free-trial/).
2. [Hämta och installera hello senaste Azure PowerShell-kommandon](/powershell/azure/overview).
3. Starta Windows PowerShell och ansluta tooyour Azure-prenumeration med hello **Add-AzureAccount** kommando.

   ```powershell
   Add-AzureAccount
   ```

## <a name="determine-your-target-azure-region"></a>Bestämma mål-Azure-region

SQL Server-dator ska finnas i en molnbaserad tjänst som finns på en viss Azure-region. hello följande steg när du toodetermine din region, storage-konto och molntjänst som ska användas för hello resten av hello kursen.

1. Fastställa hello datacenter som du vill toouse toohost SQL Server-VM. hello följande PowerShell-kommando visar en lista över tillgängliga regionnamn.

   ```powershell
   (Get-AzureLocation).Name
   ```

2. När du har hittat din önskade plats, ange en variabel med namnet **$dcLocation** toothat region. Hej exempelvis följande kommando anger hello region för ”östra USA”:

   ```powershell
   $dcLocation = "East US"
   ```

## <a name="set-your-subscription-and-storage-account"></a>Ange din prenumeration och storage-konto

1. Fastställa hello Azure-prenumeration ska användas för hello ny virtuell dator.

   ```powershell
   (Get-AzureSubscription).SubscriptionName
   ```

2. Tilldela dina mål Azure-prenumeration toohello **$subscr** variabeln. Ange detta till din aktuella Azure-prenumeration.

   ```powershell
   $subscr="<subscription name>"
   Select-AzureSubscription -SubscriptionName $subscr –Current
   ```

3. Sök sedan efter befintliga lagringskonton. hello visar följande skript alla lagringskonton som finns i din valda region:

   ```powershell
   (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName
   ```

   > [!NOTE]
   > Om du behöver ett nytt lagringskonto, måste du först skapa en alla gemen lagringskontonamnet med hello ny AzureStorageAccount kommandot som i följande exempel hello:`New-AzureStorageAccount -StorageAccountName "<storage account name>" -Location $dcLocation`

4. Tilldela hello mål storage-konto namnet toohello **$staccount**. Använd sedan **Set AzureSubscription** tooset hello prenumeration och aktuella storage-konto.

   ```powershell
   $staccount="<storage account name>"
   Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount
   ```

## <a name="select-a-sql-server-virtual-machine-image"></a>Välj en avbildning av virtuell dator för SQL Server

1. Ta reda på hello lista med tillgängliga virtuella datorer SQL Server-avbildningar från hello-galleriet. Dessa avbildningar som alla har en **ImageFamily** egenskap som börjar med ”SQL”. hello följande fråga visar hello avbildningen family tillgängliga tooyou som SQL Server vara förinstallerat.

   ```powershell
   Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily
   ```

2. När du har hittat hello virtuella avbildningen familj kan det finnas flera publicerade bilder i den här serien. Använd hello följande skript toofind hello senaste publicerade virtuella avbildningens namn för din valda avbildningen familj (exempelvis **SQL Server 2016 RTM Enterprise på Windows Server 2012 R2**):

   ```powershell
   $family="<ImageFamily value>"
   $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

   echo "Selected SQL Server image name:"
   echo "   $image"
   ```

## <a name="create-hello-virtual-machine"></a>Skapa hello virtuell dator

Skapa slutligen hello virtuella datorn med PowerShell:

1. Skapa ett moln service toohost hello ny virtuell dator. Observera att det är också möjligt toouse en befintlig molntjänst i stället. Skapa en ny variabel **$svcname** med hello korta namnet för hello-Molntjänsten.

   ```powershell
   $svcname = "<cloud service name>"
   New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation
   ```

2. Ange hello virtuella datornamn och en storlek. Mer information om storlekar för virtuella datorer finns [storlekar för virtuella datorer för Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

   ```powershell
   $vmname="<machine name>"
   $vmsize="<Specify one: Large, ExtraLarge, A5, A6, A7, A8, A9, or see hello link toohello other VM sizes>"
   $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image
   ```

3. Ange hello lokalt administratörskonto och lösenord.

   ```powershell
   $cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
   $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password
   ```

4. Kör hello följande skript toocreate hello virtuell dator.

   ```powershell
   New-AzureVM –ServiceName $svcname -VMs $vm1
   ```

> [!NOTE]
> Ytterligare förklaring och konfigurationsalternativ finns hello **skapa din kommandouppsättning** i avsnittet [Använd Azure PowerShell toocreate och förkonfigurera en Windows-baserade virtuella datorer](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="example-powershell-script"></a>Exempel PowerShell-skript

hello följande skript innehåller ett exempel på en komplett skript som skapar en **SQL Server 2016 RTM Enterprise på Windows Server 2012 R2** virtuella datorn. Om du använder det här skriptet kan anpassa du hello inledande variabler utifrån hello föregående steg i det här avsnittet.

```powershell
# Customize these variables based on your settings and requirements:
$dcLocation = "East US"
$subscr="mysubscription"
$staccount="mystorageaccount"
$family="SQL Server 2016 RTM Enterprise on Windows Server 2012 R2"
$svcname = "mycloudservice"
$vmname="myvirtualmachine"
$vmsize="A5"

# Set hello current subscription and storage account
# Comment out hello New-AzureStorageAccount line if hello account already exists
Select-AzureSubscription -SubscriptionName $subscr –Current
New-AzureStorageAccount -StorageAccountName $staccount -Location $dcLocation
Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

# Select hello most recent VM image in this image family:
$image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

# Create hello new cloud service; comment out this line if cloud service exists already:
New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

# Create hello VM config:
$vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

# Set administrator credentials:
$cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
$vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

# Create hello SQL Server VM:
New-AzureVM –ServiceName $svcname -VMs $vm1
```

## <a name="connect-with-remote-desktop"></a>Ansluta med fjärrskrivbord

1. Skapa hello RDP-filer i hello den aktuella användarens dokumentet mappen toolaunch dessa virtuella datorer toocomplete installationen:

   ```powershell
   $documentspath = [environment]::getfolderpath("mydocuments")
   Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"
   ```

2. Starta hello RDP-filen i hello dokumentkatalog. Anslut med Hej administratörsanvändarnamn och lösenord som tillhandahölls tidigare (till exempel om användarnamnet är VMAdmin, ange ”\VMAdmin” som hello användare och ange hello lösenord).

   ```powershell
   cd $documentspath
   .\vm1.rdp
   ```

## <a name="complete-hello-configuration-of-hello-sql-server-machine-for-remote-access"></a>Fullständig hello konfigurationen av hello dator med SQL Server för fjärråtkomst

Konfigurera SQL Server baserat på hello instruktionerna i efter inloggningen toohello datorn med fjärrskrivbord [steg för att konfigurera SQL Server-anslutningen i en Azure VM](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

## <a name="next-steps"></a>Nästa steg

Du kan hitta ytterligare instruktioner för att etablera virtuella datorer med PowerShell i hello [virtuella datorer dokumentationen](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

I många fall hello nästa steg är toomigrate databaser-toothis nya SQL Server-VM. Databasen migrering vägledning finns [migrera en databas tooSQL Server på en Azure VM](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).

Om du är intresserad av att använda hello Azure portal toocreate SQL virtuella datorer, se [etablering av en SQL Server-dator på Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md). Observera att hello kursen att får du via hello portalen skapar virtuella datorer med hjälp av hello rekommenderade Resource Manager-modellen i stället för hello klassiska modellen som används i det här avsnittet för PowerShell.

Dessutom toothese resurser, rekommenderar vi att du läser [andra avsnitt relaterade toorunning SQL Server i Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).
