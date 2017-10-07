---
title: aaaHow toouse PowerShell toomanage Azure File storage | Microsoft Docs
description: "Lär dig toouse PowerShell toomanage Azure File storage."
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
ms.openlocfilehash: 7bd84c9cfa31782aedf4a209cb15d4b8d92e2737
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-powershell-toomanage-azure-file-storage"></a>Hur toouse PowerShell toomanage Azure File storage
Du kan använda Azure PowerShell toocreate och hantera filresurser.

## <a name="install-hello-powershell-cmdlets-for-azure-storage"></a>Installera hello PowerShell-cmdlets för Azure Storage
tooprepare toouse PowerShell, hämta och installera hello Azure PowerShell-cmdlets. Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs) för hello installera installationsplatser och installationsanvisningar.

> [!NOTE]
> Det rekommenderas att du hämtar och installerar eller uppgraderar toohello senaste Azure PowerShell-modulen.
> 
> 

Öppna ett Azure PowerShell-fönster genom att klicka på **Start** och skriva **Windows PowerShell**. hello PowerShell-fönstret läser in hello Azure Powershell-modulen för dig.

## <a name="create-a-context-for-your-storage-account-and-key"></a>Skapa en kontext för ditt lagringskonto och din nyckel
Skapa hello kontexten för lagringskontot. hello kontexten innehåller hello lagringskontots namn och åtkomstnyckel. Anvisningar om hur du kopierar din kontonyckel från hello [Azure-portalen](https://portal.azure.com), se [visa och kopiera åtkomstnycklar för lagring](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#view-and-copy-storage-access-keys).

Ersätt `storage-account-name` och `storage-account-key` med lagringskontonamn och nyckel i följande exempel hello.

```powershell
# create a context for account and key
$ctx=New-AzureStorageContext storage-account-name storage-account-key
```

## <a name="create-a-new-file-share"></a>Skapa en ny filresurs
Skapa hello ny resurs med namnet `logs`.

```powershell
# create a new share
$s = New-AzureStorageShare logs -Context $ctx
```

Nu har du en filresurs i File Storage. Härnäst ska vi lägga till en katalog och en fil.

> [!IMPORTANT]
> hello namnet på filresursen får bara innehålla gemener. Mer information om hur du namnger filresurser och filer finns i [Namnge och referera till resurser, kataloger, filer och Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).
> 
> 

## <a name="create-a-directory-in-hello-file-share"></a>Skapa en katalog i hello filresurs
Skapa en katalog på hello resurs. I följande exempel hello, hello directory heter `CustomLogs`.

```powershell
# create a directory in hello share
New-AzureStorageDirectory -Share $s -Path CustomLogs
```

## <a name="upload-a-local-file-toohello-directory"></a>Ladda upp en lokal katalog i toohello
Ladda upp en lokal fil toohello katalog. hello följande exempel överför en fil från `C:\temp\Log1.txt`. Redigera hello sökvägen så att den pekar tooa giltig fil på den lokala datorn.

```powershell
# upload a local file toohello new directory
Set-AzureStorageFileContent -Share $s -Source C:\temp\Log1.txt -Path CustomLogs
```

## <a name="list-hello-files-in-hello-directory"></a>Lista över hello filer i hello directory
toosee hello-fil i hello, kan du visa alla hello directory filer. Det här kommandot returnerar hello filer och underkataloger (om det finns några) i hello CustomLogs directory.

```powershell
# list files in hello new directory
Get-AzureStorageFile -Share $s -Path CustomLogs | Get-AzureStorageFile
```

Get-AzureStorageFile returnerar en lista över filer och kataloger för det katalogobjekt som du har angett. ”Get-AzureStorageFile-Share $s” returnerar en lista över filer och kataloger i rotkatalogen för hello. tooget en lista över filer i en underkatalog har toopass hello underkatalog tooGet-AzureStorageFile. Det är det här--hello första delen av kommandot hello toohello pipe returnerar en kataloginstans av hello underkatalogen CustomLogs. Därefter skickas detta till Get-AzureStorageFile, som returnerar hello filer och kataloger i CustomLogs.

## <a name="copy-files"></a>Kopiera filer
Från och med version 0.9.7 av Azure PowerShell kan kopiera du en fil för tooanother, en tooa blob-fil eller en tooa blob-fil. Nedan ser du hur tooperform dessa kopiera åtgärder med hjälp av PowerShell-cmdlets.

```powershell
# copy a file toohello new directory
Start-AzureStorageFileCopy -SrcShareName srcshare -SrcFilePath srcdir/hello.txt -DestShareName destshare -DestFilePath destdir/hellocopy.txt -Context $srcCtx -DestContext $destCtx

# copy a blob tooa file directory
Start-AzureStorageFileCopy -SrcContainerName srcctn -SrcBlobName hello2.txt -DestShareName hello -DestFilePath hellodir/hello2copy.txt -DestContext $ctx -Context $ctx
```
## <a name="next-steps"></a>Nästa steg
Mer information om Azure File Storage finns på följande länkar.

* [Vanliga frågor och svar](../storage-files-faq.md)
* [Felsökning i Windows](storage-troubleshoot-windows-file-connection-problems.md)      
* [Felsökning i Linux](storage-troubleshoot-linux-file-connection-problems.md)    