---
title: "aaaCreating en lokal avbildning av virtuell dator för hello Azure Marketplace | Microsoft Docs"
description: "Förstå och köra hello steg toocreate en lokala VM-avbildning och distribuera toohello Azure Marketplace för andra toopurchase."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 26dfbd5a-8685-4b19-987e-c20ca60540ec
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: c7a265330f1e494db8d0e981a38ee00d85746bb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-an-on-premises-virtual-machine-image-for-hello-azure-marketplace"></a>Utveckla en lokal avbildning av virtuell dator för hello Azure Marketplace
Vi rekommenderar starkt att du utveckla Azure virtuella hårddiskar (VHD) direkt i hello molnet genom att använda Remote Desktop Protocol. Om du måste det är dock möjligt toodownload en virtuell Hårddisk och utveckla genom att använda lokal infrastruktur.  

För lokal utveckling, måste du hämta hello operativsystemet VHD från hello skapade VM. De här stegen skulle ske steget 3.3, ovan.  

## <a name="download-a-vhd-image"></a>Hämta en VHD-avbildningen
### <a name="locate-a-blob-url"></a>Leta upp en blob-URL
I ordning toodownload hello VHD först lokalisera hello blob-URL för hello operativsystemdisken.

Leta upp hello blob-URL från hello nya [Microsoft Azure-portalen](https://portal.azure.com):

1. Gå för**Bläddra** > **VMs**, och välj sedan hello distribueras VM.
2. Under **konfigurera**väljer hello **diskar** panelen, vilket öppnar bladet för hello-diskar.
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img01.png)
3. Välj hello **OS-disken**, vilket öppnar ett nytt blad som visar egenskaper för disk, inklusive hello VHD-plats.
4. Kopiera URL: en blob.
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img02.png)
5. Nu kan ta bort hello distribuerade virtuella datorn utan att ta bort hello stödjande diskar. Du kan också avbryta hello VM i stället för att ta bort den. Hämta inte hello operativsystemet VHD när hello VM körs.
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img03.png)

### <a name="download-a-vhd"></a>Ladda ned en virtuell hårddisk
När du vet hello blob URL, du kan hämta hello VHD med hjälp av hello [Azure-portalen](http://manage.windowsazure.com/) eller PowerShell.  

> [!NOTE]
> För närvarande hello den här guiden skapas finns hello funktioner toodownload en virtuell Hårddisk inte ännu i hello nya Microsoft Azure-portalen.  
> 
> 

**Hämta hello operativsystemet VHD via hello aktuella [Azure-portalen](http://manage.windowsazure.com/)**

1. Logga in toohello Azure-portalen om du inte har gjort det redan.
2. Klicka på hello **lagring** fliken.
3. Välj hello storage-konto i vilka hello VHD lagras.
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img04.png)
4. Detta visar lagringskontoegenskaperna. Välj hello **behållare** fliken.
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img05.png)
5. Välj hello-behållare i vilken hello VHD lagras. När du skapade från hello portal lagras hello VHD i en VHD-behållare.
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img06.png)
6. Välj rätt operativsystem för hello VHD genom att jämföra hello URL toohello något du har sparat.
7. Klicka på **Hämta**.
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img07.png)

### <a name="download-a-vhd-by-using-powershell"></a>Hämta en virtuell Hårddisk med hjälp av PowerShell
Dessutom toousing hello Azure-portalen kan du använda hello [spara AzureVhd](http://msdn.microsoft.com/library/dn495297.aspx) cmdlet toodownload hello operativsystemet VHD.

        Save-AzureVhd –Source <storageURIOfVhd> `
        -LocalFilePath <diskLocationOnWorkstation> `
        -StorageKey <keyForStorageAccount>
Exempelvis spara AzureVhd-källa ”https://baseimagevm.blob.core.windows.net/vhds/BaseImageVM-6820cq00-BaseImageVM-os-1411003770191.vhd” - LocalFilePath ”C:\Users\Administrator\Desktop\baseimagevm.vhd” - StorageKey<String>

> [!NOTE]
> **Spara-AzureVhd** har också en **NumberOfThreads** alternativ som kan vara används tooincrease parallellitet toomake hello bästa utnyttjande av bandbredd för hello nedladdning.
> 
> 

## <a name="upload-vhds-tooan-azure-storage-account"></a>Överför virtuella hårddiskar tooan Azure storage-konto
Om du har förberett din virtuella hårddiskar lokala måste tooupload dem till en storage-konto i Azure. Det här steget sker efter att skapa den virtuella Hårddisken lokalt men innan du hämtar certifikat för VM-avbildning.

### <a name="create-a-storage-account-and-container"></a>Skapa ett lagringskonto och en behållare
Vi rekommenderar att virtuella hårddiskar ska överföras till ett lagringskonto i en region i hello USA. Alla virtuella hårddiskar för en enskild SKU ska placeras i en enskild behållare inom ett enda storage-konto.

toocreate ett lagringskonto som du kan använda hello [Microsoft Azure-portalen](https://portal.azure.com/), PowerShell eller kommandoradsverktyget för hello Linux.  

**Skapa ett lagringskonto från hello Microsoft Azure-portalen**

1. Klicka på **Ny**.
2. Välj **lagring**.
3. Fyll i hello lagringskontonamn och välj sedan en plats.
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img08.png)
4. Klicka på **Skapa**.
5. hello-bladet för hello skapade lagringskontot ska vara öppen. Om inte, Välj **Bläddra** > **Lagringskonton**. Konto bladet på hello lagring, Välj hello lagringskonto skapas.
6. Välj **behållare**.
   
   ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img09.png) 
7. Hello behållare bladet välj **Lägg till**, och ange behörigheter för behållaren av namn och hello en behållare. Välj **privata** behörigheter för behållaren.

> [!TIP]
> Vi rekommenderar att du skapar en behållare per att du planerar toopublish SKU.
> 
> 

  ![Rita](media/marketplace-publishing-vm-image-creation-on-premise/img10.png)

### <a name="create-a-storage-account-by-using-powershell"></a>Skapa ett lagringskonto med hjälp av PowerShell
Med hjälp av PowerShell, skapa ett lagringskonto med hjälp av hello [ny AzureStorageAccount](http://msdn.microsoft.com/library/dn495115.aspx) cmdlet.

        New-AzureStorageAccount -StorageAccountName “mystorageaccount” -Location “West US”

Därefter kan du skapa en behållare i detta lagringskonto med hjälp av hello [NewAzureStorageContainer](http://msdn.microsoft.com/library/dn495291.aspx) cmdlet.

        New-AzureStorageContainer -Name “containername” -Permission “Off”

> [!NOTE]
> Dessa kommandon förutsätter att hello aktuella kontexten för lagringskontot har redan angetts i PowerShell.   Se för[konfigurera Azure PowerShell](marketplace-publishing-powershell-setup.md) för mer information om PowerShell-installationen.  
> 
> ### <a name="create-a-storage-account-by-using-hello-command-line-tool-for-mac-and-linux"></a>Skapa ett lagringskonto med hjälp av kommandoradsverktyget hello för Mac- och Linux
> Från [Linux kommandoradsverktyget](../virtual-machines/linux/cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), skapa ett lagringskonto på följande sätt.
> 
> 

        azure storage account create mystorageaccount --location "West US"

Skapa en behållare på följande sätt.

        azure storage container create containername --account-name mystorageaccount --accountkey <accountKey>

## <a name="upload-a-vhd"></a>Ladda upp en virtuell hårddisk
När hello storage-konto och behållaren har skapats kan överföra du din förberedda virtuella hårddiskar. Du kan använda PowerShell, hello Linux-kommandoradsverktyget eller andra hanteringsverktyg för Azure Storage.

### <a name="upload-a-vhd-via-powershell"></a>Överföra en virtuell Hårddisk med PowerShell
Använd hello [Add-AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) cmdlet.

        Add-AzureVhd –Destination “http://mystorageaccount.blob.core.windows.net/containername/vmsku.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\vmsku.vhd”

### <a name="upload-a-vhd-by-using-hello-command-line-tool-for-mac-and-linux"></a>Överföra en virtuell Hårddisk med hjälp av kommandoradsverktyget hello för Mac- och Linux
Med hello [Linux kommandoradsverktyget](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2), använder hello följande: azure vm-avbildning skapa <image name> --plats <Location of hello data center> --Operativsystemet Linux<LocationOfLocalVHD>

## <a name="see-also"></a>Se även
* [Skapa en avbildning av virtuell dator för hello Marketplace](marketplace-publishing-vm-image-creation.md)
* [Konfigurera Azure PowerShell](marketplace-publishing-powershell-setup.md)

