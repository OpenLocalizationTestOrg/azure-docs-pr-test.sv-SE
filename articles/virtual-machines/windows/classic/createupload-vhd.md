---
title: "aaaCreate och ladda upp en virtuell dator på avbildning med hjälp av Powershell | Microsoft Docs"
description: "Lär dig toocreate och ladda upp en generaliserad Windows Server-avbildning (VHD) med hjälp av hello klassiska distributionsmodellen och Azure Powershell."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 8c4a08fe-7714-4bf0-be87-c728a7806d3f
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: cynthn
ms.openlocfilehash: 093b57c9157cea5f348c8ba02b5700c917adbcdd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-upload-a-windows-server-vhd-tooazure"></a><span data-ttu-id="e56f8-103">Skapa och ladda upp en Windows Server VHD-tooAzure</span><span class="sxs-lookup"><span data-stu-id="e56f8-103">Create and upload a Windows Server VHD tooAzure</span></span>
<span data-ttu-id="e56f8-104">Den här artikeln visar hur tooupload egna generaliserad VM avbildning som en virtuell hårddisk (VHD) som du kan använda toocreate virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e56f8-104">This article shows you how tooupload your own generalized VM image as a virtual hard disk (VHD) so you can use it toocreate virtual machines.</span></span> <span data-ttu-id="e56f8-105">Mer information om diskar och virtuella hårddiskar i Microsoft Azure finns [om diskar och virtuella hårddiskar för virtuella datorer](../about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e56f8-105">For more details about disks and VHDs in Microsoft Azure, see [About Disks and VHDs for Virtual Machines](../about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e56f8-106">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="e56f8-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="e56f8-107">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="e56f8-107">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="e56f8-108">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="e56f8-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="e56f8-109">Du kan också [överför](../upload-generalized-managed.md) en virtuell dator med hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="e56f8-109">You can also [upload](../upload-generalized-managed.md) a virtual machine using hello Resource Manager model.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e56f8-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e56f8-110">Prerequisites</span></span>
<span data-ttu-id="e56f8-111">Den här artikeln förutsätter att du har:</span><span class="sxs-lookup"><span data-stu-id="e56f8-111">This article assumes you have:</span></span>

* <span data-ttu-id="e56f8-112">**En Azure-prenumeration** -om du inte har någon, kan du [öppna ett Azure-konto gratis](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="e56f8-112">**An Azure subscription** - If you don't have one, you can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
* <span data-ttu-id="e56f8-113">**[Microsoft Azure PowerShell](/powershell/azure/overview)**  -du har hello Microsoft Azure PowerShell module installerad och konfigurerad toouse din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e56f8-113">**[Microsoft Azure PowerShell](/powershell/azure/overview)** - You have hello Microsoft Azure PowerShell module installed and configured toouse your subscription.</span></span>
* <span data-ttu-id="e56f8-114">**EN. VHD-filen** - operativsystemet som lagras i en VHD-filen och anslutna tooa virtuell dator stöds Windows.</span><span class="sxs-lookup"><span data-stu-id="e56f8-114">**A .VHD file** - supported Windows operating system stored in a .vhd file and attached tooa virtual machine.</span></span> <span data-ttu-id="e56f8-115">Kontrollera toosee om hello serverroller som körs på hello VHD som stöds av Sysprep.</span><span class="sxs-lookup"><span data-stu-id="e56f8-115">Check toosee if hello server roles running on hello VHD are supported by Sysprep.</span></span> <span data-ttu-id="e56f8-116">Mer information finns i [Sysprep-stöd för serverroller](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span><span class="sxs-lookup"><span data-stu-id="e56f8-116">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="e56f8-117">Hej VHDX-format stöds inte i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="e56f8-117">hello VHDX format is not supported in Microsoft Azure.</span></span> <span data-ttu-id="e56f8-118">Du kan konvertera hello tooVHD diskformat med hjälp av Hyper-V Manager eller hello [Convert-VHD cmdlet](http://technet.microsoft.com/library/hh848454.aspx).</span><span class="sxs-lookup"><span data-stu-id="e56f8-118">You can convert hello disk tooVHD format using Hyper-V Manager or hello [Convert-VHD cmdlet](http://technet.microsoft.com/library/hh848454.aspx).</span></span> <span data-ttu-id="e56f8-119">Mer information finns i det här [blogginlägg](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).</span><span class="sxs-lookup"><span data-stu-id="e56f8-119">For details, see this [blogpost](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).</span></span>

## <a name="step-1-prep-hello-vhd"></a><span data-ttu-id="e56f8-120">Steg 1: Förbered hello VHD</span><span class="sxs-lookup"><span data-stu-id="e56f8-120">Step 1: Prep hello VHD</span></span>
<span data-ttu-id="e56f8-121">Innan du laddar upp hello VHD tooAzure måste generaliseras med Sysprep-verktyget för hello toobe.</span><span class="sxs-lookup"><span data-stu-id="e56f8-121">Before you upload hello VHD tooAzure, it needs toobe generalized by using hello Sysprep tool.</span></span> <span data-ttu-id="e56f8-122">Detta förbereder hello VHD toobe används som en bild.</span><span class="sxs-lookup"><span data-stu-id="e56f8-122">This prepares hello VHD toobe used as an image.</span></span> <span data-ttu-id="e56f8-123">Mer information om Sysprep finns [hur tooUse Sysprep: en introduktion](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="e56f8-123">For details about Sysprep, see [How tooUse Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span> <span data-ttu-id="e56f8-124">Säkerhetskopiera hello VM innan du kör Sysprep.</span><span class="sxs-lookup"><span data-stu-id="e56f8-124">Back up hello VM before running Sysprep.</span></span>

<span data-ttu-id="e56f8-125">Från hello virtuell dator som hello operativsystemet har installerats för att slutföra hello nedan:</span><span class="sxs-lookup"><span data-stu-id="e56f8-125">From hello virtual machine that hello operating system was installed to, complete hello following procedure:</span></span>

1. <span data-ttu-id="e56f8-126">Logga in toohello operativsystem.</span><span class="sxs-lookup"><span data-stu-id="e56f8-126">Sign in toohello operating system.</span></span>
2. <span data-ttu-id="e56f8-127">Öppna ett kommandotolksfönster som administratör.</span><span class="sxs-lookup"><span data-stu-id="e56f8-127">Open a command prompt window as an administrator.</span></span> <span data-ttu-id="e56f8-128">Ändra hello katalogen för**%windir%\system32\sysprep**, och kör sedan `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="e56f8-128">Change hello directory too**%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>

    ![Öppna ett kommandotolksfönster](./media/createupload-vhd/sysprep_commandprompt.png)
3. <span data-ttu-id="e56f8-130">Hej **systemförberedelseverktyget** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="e56f8-130">hello **System Preparation Tool** dialog box appears.</span></span>

   ![Starta Sysprep](./media/createupload-vhd/sysprepgeneral.png)
4. <span data-ttu-id="e56f8-132">I hello **systemförberedelseverktyget**väljer **ange System Box Experience OOBE (Out of)** och se till att **Generalize** är markerad.</span><span class="sxs-lookup"><span data-stu-id="e56f8-132">In hello **System Preparation Tool**, select **Enter System Out of Box Experience (OOBE)** and make sure that **Generalize** is checked.</span></span>
5. <span data-ttu-id="e56f8-133">I **avstängningsalternativ**väljer **avstängning**.</span><span class="sxs-lookup"><span data-stu-id="e56f8-133">In **Shutdown Options**, select **Shutdown**.</span></span>
6. <span data-ttu-id="e56f8-134">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e56f8-134">Click **OK**.</span></span>

## <a name="step-2-create-a-storage-account-and-a-container"></a><span data-ttu-id="e56f8-135">Steg 2: Skapa ett lagringskonto och en behållare</span><span class="sxs-lookup"><span data-stu-id="e56f8-135">Step 2: Create a storage account and a container</span></span>
<span data-ttu-id="e56f8-136">Du behöver ett lagringskonto i Azure så att du har en plats tooupload hello VHD-fil.</span><span class="sxs-lookup"><span data-stu-id="e56f8-136">You need a storage account in Azure so you have a place tooupload hello .vhd file.</span></span> <span data-ttu-id="e56f8-137">Det här steget visar hur toocreate ett konto eller get hello information du behöver från ett befintligt konto.</span><span class="sxs-lookup"><span data-stu-id="e56f8-137">This step shows you how toocreate an account, or get hello info you need from an existing account.</span></span> <span data-ttu-id="e56f8-138">Ersätt hello variabler i &lsaquo; hakparenteser &rsaquo; med din egen information.</span><span class="sxs-lookup"><span data-stu-id="e56f8-138">Replace hello variables in &lsaquo; brackets &rsaquo; with your own information.</span></span>

1. <span data-ttu-id="e56f8-139">Inloggning</span><span class="sxs-lookup"><span data-stu-id="e56f8-139">Login</span></span>

    ```powershell
    Add-AzureAccount
    ```

2. <span data-ttu-id="e56f8-140">Ange din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e56f8-140">Set your Azure subscription.</span></span>

    ```powershell
    Select-AzureSubscription -SubscriptionName <SubscriptionName>
    ```

3. <span data-ttu-id="e56f8-141">Skapa ett nytt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="e56f8-141">Create a new storage account.</span></span> <span data-ttu-id="e56f8-142">hello namnet på lagringskontot för hello bör vara unikt, 3 till 24 tecken.</span><span class="sxs-lookup"><span data-stu-id="e56f8-142">hello name of hello storage account should be unique, 3-24 characters.</span></span> <span data-ttu-id="e56f8-143">hello namnet kan vara valfri kombination av bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="e56f8-143">hello name can be any combination of letters and numbers.</span></span> <span data-ttu-id="e56f8-144">Du måste också toospecify en plats som ”Öst oss”</span><span class="sxs-lookup"><span data-stu-id="e56f8-144">You also need toospecify a location like "East US"</span></span>

    ```powershell
    New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>
    ```

4. <span data-ttu-id="e56f8-145">Ange hello nytt lagringskonto som hello standard.</span><span class="sxs-lookup"><span data-stu-id="e56f8-145">Set hello new storage account as hello default.</span></span>

    ```powershell
    Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>
    ```

5. <span data-ttu-id="e56f8-146">Skapa en ny behållare.</span><span class="sxs-lookup"><span data-stu-id="e56f8-146">Create a new container.</span></span>

    ```powershell
    New-AzureStorageContainer -Name <ContainerName> -Permission Off
    ```

## <a name="step-3-upload-hello-vhd-file"></a><span data-ttu-id="e56f8-147">Steg 3: Överför hello VHD-filen</span><span class="sxs-lookup"><span data-stu-id="e56f8-147">Step 3: Upload hello .vhd file</span></span>
<span data-ttu-id="e56f8-148">Använd hello [Add-AzureVhd](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevhd) tooupload hello VHD.</span><span class="sxs-lookup"><span data-stu-id="e56f8-148">Use hello [Add-AzureVhd](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevhd) tooupload hello VHD.</span></span>

<span data-ttu-id="e56f8-149">Från hello Azure PowerShell-fönster du använde i hello föregående steg, typen hello följande kommando och Ersätt hello variabler i &lsaquo; hakparenteser &rsaquo; med din egen information.</span><span class="sxs-lookup"><span data-stu-id="e56f8-149">From hello Azure PowerShell window you used in hello previous step, type hello following command and replace hello variables in &lsaquo; brackets &rsaquo; with your own information.</span></span>

```powershell
Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>
```

## <a name="step-4-add-hello-image-tooyour-list-of-custom-images"></a><span data-ttu-id="e56f8-150">Steg 4: Lägg till hello avbildningen tooyour lista över anpassade avbildningar</span><span class="sxs-lookup"><span data-stu-id="e56f8-150">Step 4: Add hello image tooyour list of custom images</span></span>
<span data-ttu-id="e56f8-151">Använd hello [Lägg till AzureVMImage](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevmimage) cmdlet tooadd hello toohello bildlista av egna, anpassade avbildningar.</span><span class="sxs-lookup"><span data-stu-id="e56f8-151">Use hello [Add-AzureVMImage](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevmimage) cmdlet tooadd hello image toohello list of your custom images.</span></span>

```powershell
Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"
```

## <a name="next-steps"></a><span data-ttu-id="e56f8-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e56f8-152">Next steps</span></span>
<span data-ttu-id="e56f8-153">Du kan nu [skapa en egen virtuell dator](createportal.md) med hello bild du har överförts.</span><span class="sxs-lookup"><span data-stu-id="e56f8-153">You can now [create a custom VM](createportal.md) using hello image you uploaded.</span></span>
