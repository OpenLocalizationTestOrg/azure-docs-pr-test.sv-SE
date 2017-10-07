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
# <a name="create-and-upload-a-windows-server-vhd-tooazure"></a>Skapa och ladda upp en Windows Server VHD-tooAzure
Den här artikeln visar hur tooupload egna generaliserad VM avbildning som en virtuell hårddisk (VHD) som du kan använda toocreate virtuella datorer. Mer information om diskar och virtuella hårddiskar i Microsoft Azure finns [om diskar och virtuella hårddiskar för virtuella datorer](../about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Du kan också [överför](../upload-generalized-managed.md) en virtuell dator med hello Resource Manager-modellen.

## <a name="prerequisites"></a>Krav
Den här artikeln förutsätter att du har:

* **En Azure-prenumeration** -om du inte har någon, kan du [öppna ett Azure-konto gratis](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
* **[Microsoft Azure PowerShell](/powershell/azure/overview)**  -du har hello Microsoft Azure PowerShell module installerad och konfigurerad toouse din prenumeration.
* **EN. VHD-filen** - operativsystemet som lagras i en VHD-filen och anslutna tooa virtuell dator stöds Windows. Kontrollera toosee om hello serverroller som körs på hello VHD som stöds av Sysprep. Mer information finns i [Sysprep-stöd för serverroller](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

    > [!IMPORTANT]
    > Hej VHDX-format stöds inte i Microsoft Azure. Du kan konvertera hello tooVHD diskformat med hjälp av Hyper-V Manager eller hello [Convert-VHD cmdlet](http://technet.microsoft.com/library/hh848454.aspx). Mer information finns i det här [blogginlägg](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).

## <a name="step-1-prep-hello-vhd"></a>Steg 1: Förbered hello VHD
Innan du laddar upp hello VHD tooAzure måste generaliseras med Sysprep-verktyget för hello toobe. Detta förbereder hello VHD toobe används som en bild. Mer information om Sysprep finns [hur tooUse Sysprep: en introduktion](http://technet.microsoft.com/library/bb457073.aspx). Säkerhetskopiera hello VM innan du kör Sysprep.

Från hello virtuell dator som hello operativsystemet har installerats för att slutföra hello nedan:

1. Logga in toohello operativsystem.
2. Öppna ett kommandotolksfönster som administratör. Ändra hello katalogen för**%windir%\system32\sysprep**, och kör sedan `sysprep.exe`.

    ![Öppna ett kommandotolksfönster](./media/createupload-vhd/sysprep_commandprompt.png)
3. Hej **systemförberedelseverktyget** dialogrutan visas.

   ![Starta Sysprep](./media/createupload-vhd/sysprepgeneral.png)
4. I hello **systemförberedelseverktyget**väljer **ange System Box Experience OOBE (Out of)** och se till att **Generalize** är markerad.
5. I **avstängningsalternativ**väljer **avstängning**.
6. Klicka på **OK**.

## <a name="step-2-create-a-storage-account-and-a-container"></a>Steg 2: Skapa ett lagringskonto och en behållare
Du behöver ett lagringskonto i Azure så att du har en plats tooupload hello VHD-fil. Det här steget visar hur toocreate ett konto eller get hello information du behöver från ett befintligt konto. Ersätt hello variabler i &lsaquo; hakparenteser &rsaquo; med din egen information.

1. Inloggning

    ```powershell
    Add-AzureAccount
    ```

2. Ange din Azure-prenumeration.

    ```powershell
    Select-AzureSubscription -SubscriptionName <SubscriptionName>
    ```

3. Skapa ett nytt lagringskonto. hello namnet på lagringskontot för hello bör vara unikt, 3 till 24 tecken. hello namnet kan vara valfri kombination av bokstäver och siffror. Du måste också toospecify en plats som ”Öst oss”

    ```powershell
    New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>
    ```

4. Ange hello nytt lagringskonto som hello standard.

    ```powershell
    Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>
    ```

5. Skapa en ny behållare.

    ```powershell
    New-AzureStorageContainer -Name <ContainerName> -Permission Off
    ```

## <a name="step-3-upload-hello-vhd-file"></a>Steg 3: Överför hello VHD-filen
Använd hello [Add-AzureVhd](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevhd) tooupload hello VHD.

Från hello Azure PowerShell-fönster du använde i hello föregående steg, typen hello följande kommando och Ersätt hello variabler i &lsaquo; hakparenteser &rsaquo; med din egen information.

```powershell
Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>
```

## <a name="step-4-add-hello-image-tooyour-list-of-custom-images"></a>Steg 4: Lägg till hello avbildningen tooyour lista över anpassade avbildningar
Använd hello [Lägg till AzureVMImage](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevmimage) cmdlet tooadd hello toohello bildlista av egna, anpassade avbildningar.

```powershell
Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"
```

## <a name="next-steps"></a>Nästa steg
Du kan nu [skapa en egen virtuell dator](createportal.md) med hello bild du har överförts.
