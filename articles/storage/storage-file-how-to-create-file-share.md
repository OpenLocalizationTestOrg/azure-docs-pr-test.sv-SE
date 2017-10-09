---
title: aaaHow toocreate en filresurs i Azure | Microsoft Docs
description: "Hur toocreate en Azure-filresurs i Azure File storage med hjälp av hello Azure-portalen, PowerShell och hello Azure CLI."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: c4c59966b82d743fb4ecf79f48c0c8e61295d03d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-file-share-in-azure-file-storage"></a>Skapa en filresurs i Azure File Storage
Du kan skapa Azure-filresurser med hjälp av [Azure-portalen](https://portal.azure.com/)hello Azure Storage PowerShell-cmdlets, hello Azure Storage-klientbibliotek eller hello Azure Storage REST API. I kursen får du lära dig:
* [Hur toocreate en Azure-fil dela med hello Azure-portalen](#Create file share through hello Portal)
* [Hur toocreate en Azure-filresurs med hjälp av Powershell](#Create file share using PowerShell)
* [Hur toocreate en Azure-filresurs med hjälp av CLI](#create-file-share-using-command-line-interface-cli)

## <a name="prerequisites"></a>Krav
toocreate en Azure-filresurs som du kan använda ett lagringskonto som redan finns eller [skapar ett nytt Azure storage-konto](storage-create-storage-account.md). toocreate en Azure-filresurs med PowerShell, behöver du hello kontonyckel och namnet på ditt lagringskonto... Du behöver lagringskontonyckel om du planerar toouse Powershell eller CLI.

## <a name="create-file-share-through-hello-portal"></a>Skapa filresurs via hello Portal
1. **Gå tooStorage kontobladet på Azure Portal**:    
    ![Bladet Lagringskonto](media/storage-file-how-to-create-file-share/create-file-share-portal1.png)

2. **Klicka på knappen Lägg till filresurs**:    
    ![Klicka på hello filen resursen knappen Lägg till](media/storage-file-how-to-create-file-share/create-file-share-portal2.png)

3. **Ange namn och kvot. Kvoten kan för närvarande maximalt vara 5TB**:    
    ![Ange ett namn och en önskad kvot för hello ny filresurs](media/storage-file-how-to-create-file-share/create-file-share-portal3.png)

4. **Se den nya filresursen**: ![Se den nya filresursen](media/storage-file-how-to-create-file-share/create-file-share-portal4.png)

5. **Ladda upp en fil**: ![Ladda upp en fil](media/storage-file-how-to-create-file-share/create-file-share-portal5.png)

6. **Bläddra till filresursen och hantera dina kataloger och filer**: ![Bläddra i filresurs](media/storage-file-how-to-create-file-share/create-file-share-portal6.png)


## <a name="create-file-share-through-powershell"></a>Skapa filresurs via PowerShell
tooprepare toouse PowerShell, hämta och installera hello Azure PowerShell-cmdlets. Se [hur tooinstall och konfigurera Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/) för hello installera installationsplatser och installationsanvisningar.

> [!Note]  
> Det rekommenderas att du hämtar och installerar eller uppgraderar toohello senaste Azure PowerShell-modulen.

1. **Skapa en kontext för ditt lagringskonto och nyckeln** hello kontexten innehåller hello lagringskontots namn och åtkomstnyckel. Anvisningar för hur du kopierar din kontonyckel från [Azure Portal](https://portal.azure.com/) finns i [Visa och kopiera åtkomstnycklar för lagring](storage-create-storage-account.md#view-and-copy-storage-access-keys).

    ```powershell
    $storageContext = New-AzureStorageContext <storage-account-name> <storage-account-key>
    ```
    
2. **Skapa en ny filresurs**:    
    
    ```powershell
    $share = New-AzureStorageShare logs -Context $storageContext
    ```

> [!Note]  
> hello namnet på filresursen får bara innehålla gemener. Mer information om hur du namnger filresurser och filer finns i [Namnge och referera till resurser, kataloger, filer och Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).

## <a name="create-file-share-through-command-line-interface-cli"></a>Skapa filresurs via kommandoradsgränssnittet (CLI)
1. **tooprepare toouse en kommandoradsgränssnittet (CLI), hämta och installera hello Azure CLI.**  
    Se [Install Azure CLI 2.0](/cli/azure/install-az-cli2.md) (Installera Azure CLI 2.0) och [Get started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md) (Kom igång med Azure CLI 2.0).

2. **Skapa en anslutning sträng toohello storage-konto där du vill att toocreate hello resursen.**  
    Ersätt ```<storage-account>``` och ```<resource_group>``` med ditt konto namn och resursen lagringsgruppen i hello följande exempel.

   ```azurecli
    current_env_conn_string = $(az storage account show-connection-string -n <storage-account> -g <resource-group> --query 'connectionString' -o tsv)

    if [[ $current_env_conn_string == "" ]]; then  
        echo "Couldn't retrieve hello connection string."
    fi
    ```

3. **Skapa en filresurs**
    ```azurecli
    az storage share create --name files --quota 2048 --connection-string $current_env_conn_string 1 > /dev/null
    ```

## <a name="next-steps"></a>Nästa steg
* [Anslut och montera filresurs – Windows](storage-file-how-to-use-files-windows.md)
* [Anslut och montera filresurs – Linux](storage-how-to-use-files-linux.md)
* [Anslut och montera filresurs – macOS](storage-file-how-to-use-files-mac.md)

Mer information om Azure File Storage finns på följande länkar.

* [Vanliga frågor och svar](storage-files-faq.md)
* [Felsökning](storage-troubleshoot-file-connection-problems.md)