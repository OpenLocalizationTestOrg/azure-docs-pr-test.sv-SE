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
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a><span data-ttu-id="370f5-104">Etablera en virtuell dator med SQL Server med hjälp av Azure PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="370f5-104">Provision a SQL Server virtual machine using Azure PowerShell (Classic)</span></span>

<span data-ttu-id="370f5-105">Den här artikeln innehåller steg för hur hello toocreate en SQL Server-dator i Azure med hjälp av PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="370f5-105">This article provides steps for how toocreate a SQL Server virtual machine in Azure by using hello PowerShell cmdlets.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="370f5-106">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="370f5-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="370f5-107">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="370f5-107">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="370f5-108">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="370f5-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="370f5-109">Hello Resource Manager version av det här avsnittet finns [etablera en virtuell dator med SQL Server med Azure PowerShell Resource Manager](../sql/virtual-machines-windows-ps-sql-create.md).</span><span class="sxs-lookup"><span data-stu-id="370f5-109">For hello Resource Manager version of this topic, see [Provision a SQL Server virtual machine using Azure PowerShell Resource Manager](../sql/virtual-machines-windows-ps-sql-create.md).</span></span>

### <a name="install-and-configure-powershell"></a><span data-ttu-id="370f5-110">Installera och konfigurera PowerShell:</span><span class="sxs-lookup"><span data-stu-id="370f5-110">Install and configure PowerShell:</span></span>
1. <span data-ttu-id="370f5-111">Om du inte har något Azure-konto besöker du sidan för [kostnadsfria utvärderingsversioner av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="370f5-111">If you do not have an Azure account, visit [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
2. <span data-ttu-id="370f5-112">[Hämta och installera hello senaste Azure PowerShell-kommandon](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="370f5-112">[Download and install hello latest Azure PowerShell commands](/powershell/azure/overview).</span></span>
3. <span data-ttu-id="370f5-113">Starta Windows PowerShell och ansluta tooyour Azure-prenumeration med hello **Add-AzureAccount** kommando.</span><span class="sxs-lookup"><span data-stu-id="370f5-113">Start Windows PowerShell, and connect it tooyour Azure subscription with hello **Add-AzureAccount** command.</span></span>

   ```powershell
   Add-AzureAccount
   ```

## <a name="determine-your-target-azure-region"></a><span data-ttu-id="370f5-114">Bestämma mål-Azure-region</span><span class="sxs-lookup"><span data-stu-id="370f5-114">Determine your target Azure region</span></span>

<span data-ttu-id="370f5-115">SQL Server-dator ska finnas i en molnbaserad tjänst som finns på en viss Azure-region.</span><span class="sxs-lookup"><span data-stu-id="370f5-115">Your SQL Server Virtual Machine will be hosted in a cloud service that resides a specific Azure region.</span></span> <span data-ttu-id="370f5-116">hello följande steg när du toodetermine din region, storage-konto och molntjänst som ska användas för hello resten av hello kursen.</span><span class="sxs-lookup"><span data-stu-id="370f5-116">hello following steps help you toodetermine your region, storage account, and cloud service that will be used for hello rest of hello tutorial.</span></span>

1. <span data-ttu-id="370f5-117">Fastställa hello datacenter som du vill toouse toohost SQL Server-VM.</span><span class="sxs-lookup"><span data-stu-id="370f5-117">Determine hello data center that you want toouse toohost your SQL Server VM.</span></span> <span data-ttu-id="370f5-118">hello följande PowerShell-kommando visar en lista över tillgängliga regionnamn.</span><span class="sxs-lookup"><span data-stu-id="370f5-118">hello following PowerShell command displays a list of available region names.</span></span>

   ```powershell
   (Get-AzureLocation).Name
   ```

2. <span data-ttu-id="370f5-119">När du har hittat din önskade plats, ange en variabel med namnet **$dcLocation** toothat region.</span><span class="sxs-lookup"><span data-stu-id="370f5-119">Once you've identified your preferred location, set a variable named **$dcLocation** toothat region.</span></span> <span data-ttu-id="370f5-120">Hej exempelvis följande kommando anger hello region för ”östra USA”:</span><span class="sxs-lookup"><span data-stu-id="370f5-120">For example, hello following command sets hello region too"East US":</span></span>

   ```powershell
   $dcLocation = "East US"
   ```

## <a name="set-your-subscription-and-storage-account"></a><span data-ttu-id="370f5-121">Ange din prenumeration och storage-konto</span><span class="sxs-lookup"><span data-stu-id="370f5-121">Set your subscription and storage account</span></span>

1. <span data-ttu-id="370f5-122">Fastställa hello Azure-prenumeration ska användas för hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="370f5-122">Determine hello Azure subscription you will use for hello new virtual machine.</span></span>

   ```powershell
   (Get-AzureSubscription).SubscriptionName
   ```

2. <span data-ttu-id="370f5-123">Tilldela dina mål Azure-prenumeration toohello **$subscr** variabeln.</span><span class="sxs-lookup"><span data-stu-id="370f5-123">Assign your target Azure subscription toohello **$subscr** variable.</span></span> <span data-ttu-id="370f5-124">Ange detta till din aktuella Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="370f5-124">Then set this as your current Azure subscription.</span></span>

   ```powershell
   $subscr="<subscription name>"
   Select-AzureSubscription -SubscriptionName $subscr –Current
   ```

3. <span data-ttu-id="370f5-125">Sök sedan efter befintliga lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="370f5-125">Then check for existing storage accounts.</span></span> <span data-ttu-id="370f5-126">hello visar följande skript alla lagringskonton som finns i din valda region:</span><span class="sxs-lookup"><span data-stu-id="370f5-126">hello following script displays all storage accounts that exist in your chosen region:</span></span>

   ```powershell
   (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName
   ```

   > [!NOTE]
   > <span data-ttu-id="370f5-127">Om du behöver ett nytt lagringskonto, måste du först skapa en alla gemen lagringskontonamnet med hello ny AzureStorageAccount kommandot som i följande exempel hello:`New-AzureStorageAccount -StorageAccountName "<storage account name>" -Location $dcLocation`</span><span class="sxs-lookup"><span data-stu-id="370f5-127">If you require a new storage account, first create an all-lower-case storage account name with hello New-AzureStorageAccount command as in hello following example: `New-AzureStorageAccount -StorageAccountName "<storage account name>" -Location $dcLocation`</span></span>

4. <span data-ttu-id="370f5-128">Tilldela hello mål storage-konto namnet toohello **$staccount**.</span><span class="sxs-lookup"><span data-stu-id="370f5-128">Assign hello target storage account name toohello **$staccount**.</span></span> <span data-ttu-id="370f5-129">Använd sedan **Set AzureSubscription** tooset hello prenumeration och aktuella storage-konto.</span><span class="sxs-lookup"><span data-stu-id="370f5-129">Then use **Set-AzureSubscription** tooset hello subscription and current storage account.</span></span>

   ```powershell
   $staccount="<storage account name>"
   Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount
   ```

## <a name="select-a-sql-server-virtual-machine-image"></a><span data-ttu-id="370f5-130">Välj en avbildning av virtuell dator för SQL Server</span><span class="sxs-lookup"><span data-stu-id="370f5-130">Select a SQL Server virtual machine image</span></span>

1. <span data-ttu-id="370f5-131">Ta reda på hello lista med tillgängliga virtuella datorer SQL Server-avbildningar från hello-galleriet.</span><span class="sxs-lookup"><span data-stu-id="370f5-131">Find out hello list of available SQL Server virtual machines images from hello gallery.</span></span> <span data-ttu-id="370f5-132">Dessa avbildningar som alla har en **ImageFamily** egenskap som börjar med ”SQL”.</span><span class="sxs-lookup"><span data-stu-id="370f5-132">These images all have an **ImageFamily** property that starts with "SQL".</span></span> <span data-ttu-id="370f5-133">hello följande fråga visar hello avbildningen family tillgängliga tooyou som SQL Server vara förinstallerat.</span><span class="sxs-lookup"><span data-stu-id="370f5-133">hello following query displays hello image family available tooyou that have SQL Server preinstalled.</span></span>

   ```powershell
   Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily
   ```

2. <span data-ttu-id="370f5-134">När du har hittat hello virtuella avbildningen familj kan det finnas flera publicerade bilder i den här serien.</span><span class="sxs-lookup"><span data-stu-id="370f5-134">When you find hello  virtual machine image family, there could be multiple published images in this family.</span></span> <span data-ttu-id="370f5-135">Använd hello följande skript toofind hello senaste publicerade virtuella avbildningens namn för din valda avbildningen familj (exempelvis **SQL Server 2016 RTM Enterprise på Windows Server 2012 R2**):</span><span class="sxs-lookup"><span data-stu-id="370f5-135">Use hello following script toofind hello latest published virtual machine image name for your selected image family (such as **SQL Server 2016 RTM Enterprise on Windows Server 2012 R2**):</span></span>

   ```powershell
   $family="<ImageFamily value>"
   $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

   echo "Selected SQL Server image name:"
   echo "   $image"
   ```

## <a name="create-hello-virtual-machine"></a><span data-ttu-id="370f5-136">Skapa hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="370f5-136">Create hello virtual machine</span></span>

<span data-ttu-id="370f5-137">Skapa slutligen hello virtuella datorn med PowerShell:</span><span class="sxs-lookup"><span data-stu-id="370f5-137">Finally, create hello virtual machine with PowerShell:</span></span>

1. <span data-ttu-id="370f5-138">Skapa ett moln service toohost hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="370f5-138">Create a cloud service toohost hello new VM.</span></span> <span data-ttu-id="370f5-139">Observera att det är också möjligt toouse en befintlig molntjänst i stället.</span><span class="sxs-lookup"><span data-stu-id="370f5-139">Note that it is also possible toouse an existing cloud service instead.</span></span> <span data-ttu-id="370f5-140">Skapa en ny variabel **$svcname** med hello korta namnet för hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="370f5-140">Create a new variable **$svcname** with hello short name of hello cloud service.</span></span>

   ```powershell
   $svcname = "<cloud service name>"
   New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation
   ```

2. <span data-ttu-id="370f5-141">Ange hello virtuella datornamn och en storlek.</span><span class="sxs-lookup"><span data-stu-id="370f5-141">Specify hello virtual machine name and a size.</span></span> <span data-ttu-id="370f5-142">Mer information om storlekar för virtuella datorer finns [storlekar för virtuella datorer för Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="370f5-142">For more information about virtual machine sizes, see [Virtual Machine Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

   ```powershell
   $vmname="<machine name>"
   $vmsize="<Specify one: Large, ExtraLarge, A5, A6, A7, A8, A9, or see hello link toohello other VM sizes>"
   $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image
   ```

3. <span data-ttu-id="370f5-143">Ange hello lokalt administratörskonto och lösenord.</span><span class="sxs-lookup"><span data-stu-id="370f5-143">Specify hello local administrator account and password.</span></span>

   ```powershell
   $cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
   $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password
   ```

4. <span data-ttu-id="370f5-144">Kör hello följande skript toocreate hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="370f5-144">Run hello following script toocreate hello virtual machine.</span></span>

   ```powershell
   New-AzureVM –ServiceName $svcname -VMs $vm1
   ```

> [!NOTE]
> <span data-ttu-id="370f5-145">Ytterligare förklaring och konfigurationsalternativ finns hello **skapa din kommandouppsättning** i avsnittet [Använd Azure PowerShell toocreate och förkonfigurera en Windows-baserade virtuella datorer](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="370f5-145">For additional explanation and configuration options, see hello **Build your command set** section in [Use Azure PowerShell toocreate and preconfigure Windows-based Virtual Machines](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

## <a name="example-powershell-script"></a><span data-ttu-id="370f5-146">Exempel PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="370f5-146">Example PowerShell script</span></span>

<span data-ttu-id="370f5-147">hello följande skript innehåller ett exempel på en komplett skript som skapar en **SQL Server 2016 RTM Enterprise på Windows Server 2012 R2** virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="370f5-147">hello following script provides an example of a complete script that creates a **SQL Server 2016 RTM Enterprise on Windows Server 2012 R2** virtual machine.</span></span> <span data-ttu-id="370f5-148">Om du använder det här skriptet kan anpassa du hello inledande variabler utifrån hello föregående steg i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="370f5-148">If you use this script, you must customize hello initial variables based on hello previous steps in this topic.</span></span>

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

## <a name="connect-with-remote-desktop"></a><span data-ttu-id="370f5-149">Ansluta med fjärrskrivbord</span><span class="sxs-lookup"><span data-stu-id="370f5-149">Connect with remote desktop</span></span>

1. <span data-ttu-id="370f5-150">Skapa hello RDP-filer i hello den aktuella användarens dokumentet mappen toolaunch dessa virtuella datorer toocomplete installationen:</span><span class="sxs-lookup"><span data-stu-id="370f5-150">Create hello RDP files in hello current user's document folder toolaunch these virtual machines toocomplete setup:</span></span>

   ```powershell
   $documentspath = [environment]::getfolderpath("mydocuments")
   Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"
   ```

2. <span data-ttu-id="370f5-151">Starta hello RDP-filen i hello dokumentkatalog.</span><span class="sxs-lookup"><span data-stu-id="370f5-151">In hello documents directory, launch hello RDP file.</span></span> <span data-ttu-id="370f5-152">Anslut med Hej administratörsanvändarnamn och lösenord som tillhandahölls tidigare (till exempel om användarnamnet är VMAdmin, ange ”\VMAdmin” som hello användare och ange hello lösenord).</span><span class="sxs-lookup"><span data-stu-id="370f5-152">Connect with hello administrator user name and password provided earlier (for example, if your user name was VMAdmin, specify "\VMAdmin" as hello user and provide hello password).</span></span>

   ```powershell
   cd $documentspath
   .\vm1.rdp
   ```

## <a name="complete-hello-configuration-of-hello-sql-server-machine-for-remote-access"></a><span data-ttu-id="370f5-153">Fullständig hello konfigurationen av hello dator med SQL Server för fjärråtkomst</span><span class="sxs-lookup"><span data-stu-id="370f5-153">Complete hello configuration of hello SQL Server Machine for remote access</span></span>

<span data-ttu-id="370f5-154">Konfigurera SQL Server baserat på hello instruktionerna i efter inloggningen toohello datorn med fjärrskrivbord [steg för att konfigurera SQL Server-anslutningen i en Azure VM](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).</span><span class="sxs-lookup"><span data-stu-id="370f5-154">After logging on toohello machine with remote desktop, configure SQL Server based on hello instructions in [Steps for configuring SQL Server connectivity in an Azure VM](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).</span></span>

## <a name="next-steps"></a><span data-ttu-id="370f5-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="370f5-155">Next steps</span></span>

<span data-ttu-id="370f5-156">Du kan hitta ytterligare instruktioner för att etablera virtuella datorer med PowerShell i hello [virtuella datorer dokumentationen](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="370f5-156">You can find additional instructions for provisioning virtual machines with PowerShell in hello [virtual machines documentation](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

<span data-ttu-id="370f5-157">I många fall hello nästa steg är toomigrate databaser-toothis nya SQL Server-VM.</span><span class="sxs-lookup"><span data-stu-id="370f5-157">In many cases, hello next step is toomigrate your databases toothis new SQL Server VM.</span></span> <span data-ttu-id="370f5-158">Databasen migrering vägledning finns [migrera en databas tooSQL Server på en Azure VM](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="370f5-158">For database migration guidance, see [Migrating a Database tooSQL Server on an Azure VM](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).</span></span>

<span data-ttu-id="370f5-159">Om du är intresserad av att använda hello Azure portal toocreate SQL virtuella datorer, se [etablering av en SQL Server-dator på Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="370f5-159">If you're also interested in using hello Azure portal toocreate SQL Virtual Machines, see [Provisioning a SQL Server Virtual Machine on Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md).</span></span> <span data-ttu-id="370f5-160">Observera att hello kursen att får du via hello portalen skapar virtuella datorer med hjälp av hello rekommenderade Resource Manager-modellen i stället för hello klassiska modellen som används i det här avsnittet för PowerShell.</span><span class="sxs-lookup"><span data-stu-id="370f5-160">Note that hello tutorial that walks you through hello portal creates VMs using hello recommended Resource Manager model, rather than hello classic model used in this PowerShell topic.</span></span>

<span data-ttu-id="370f5-161">Dessutom toothese resurser, rekommenderar vi att du läser [andra avsnitt relaterade toorunning SQL Server i Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="370f5-161">In addition toothese resources, we recommend that you review [other topics related toorunning SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>
