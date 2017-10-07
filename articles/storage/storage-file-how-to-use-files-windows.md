---
title: "aaaMount en Azure-filresurs och åtkomst hello delar i Windows | Microsoft Docs"
description: "Montera en filresurs i Azure och åtkomst hello resurs i Windows."
services: storage
documentationcenter: na
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
ms.openlocfilehash: 15ac468d9d7b8e0a195b024926ed4dd9790360d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="mount-an-azure-file-share-and-access-hello-share-in-windows"></a>Montera en filresurs i Azure och åtkomst hello resurs i Windows
[Azure File storage](storage-dotnet-how-to-use-files.md) är Microsofts enkelt toouse moln-filsystem. Azure-filresurser kan monteras i Windows och Windows Server. Den här artikeln visar tre olika sätt toomount en Azure-filresurs på Windows: med hello File Explorer-Gränssnittet via PowerShell och via hello kommandotolk. 

I ordning toomount en Azure-filresurs utanför hello Azure-region finns i, till exempel lokalt eller i en annan Azure-region, stöder hello OS SMB 3.0. 

Azure File-resursen kan monteras på Windows-datorn antingen lokalt eller i Azure VM beroende på operativsystemversion. Tabellen nedan visar hello 

| Windows-version        | SMB-version |Monteras på Azure VM|Monteras lokalt|
|------------------------|-------------|---------------------|---------------------|
| Windows 7              | SMB 2.1     | Ja                 | Nej                  |
| Windows Server 2008 R2 | SMB 2.1     | Ja                 | Nej                  |
| Windows 8              | SMB 3.0     | Ja                 | Ja                 |
| Windows Server 2012    | SMB 3.0     | Ja                 | Ja                 |
| Windows Server 2012 R2 | SMB 3.0     | Ja                 | Ja                 |
| Windows 10             | SMB 3.0     | Ja                 | Ja                 |

> [!Note]  
> Vi rekommenderar att alltid hello senaste KB för din version av Windows.

## <a name="aprerequisites-for-mounting-azure-file-share-with-windows"></a></a>Förutsättningar för att montera Azure-filresurser med Windows 
* **Lagringskontonamnet**: toomount ett Azure-filresurs, du behöver hello namnet på hello storage-konto.

* **Lagringskontonyckel**: toomount ett Azure-filresurs, du behöver hello primära (eller sekundära) lagringsnyckel. SAS-nycklar stöds inte för montering.

* **Kontrollera att port 445 är öppen**: Azure File Storage använder SMB-protokollet. SMB kommunicerar via TCP-port 445 - Kontrollera toosee om brandväggen inte blockerar TCP-portar 445 från klientdatorn.

## <a name="mount-hello-azure-file-share-with-file-explorer"></a>Montera hello Azure-filresurs med Utforskaren
> [!Note]  
> Observera att hello följa anvisningar visas på Windows 10 och kan skilja sig på äldre versioner. 

1. **Öppna Utforskaren**: Detta kan göras genom att öppna från hello Start-menyn eller genom att trycka på Win + E genväg.

2. **Navigera toohello ”den här datorn” objektet hello vänster av hello-fönstret. Detta ändrar hello menyerna hello menyfliksområdet. Under hello dator-menyn väljer du ”karta nätverksenhet”**.
    
    ![En skärmbild av hello ”mappa nätverksenheten” nedrullningsbara menyn](media/storage-file-how-to-use-files-windows/1_MountOnWindows10.png)

3. **Kopiera hello UNC-sökväg från hello ”Anslut” fönster i hello Azure-portalen**: en detaljerad beskrivning av hur toofind den här informationen kan hittas [här](storage-file-how-to-use-files-portal.md#connect-to-file-share).

    ![hello UNC-sökväg i hello Azure File storage Anslut rutan](media/storage-file-how-to-use-files-windows/portal_netuse_connect.png)

4. **Välj hello enhetsbeteckning och ange hello UNC-sökväg.** 
    
    ![En skärmbild av hello ”karta nätverksenhet” dialogrutan](media/storage-file-how-to-use-files-windows/2_MountOnWindows10.png)

5. **Använd hello Lagringskontonamnet föregås av `Azure\` som hello användarnamn och en Lagringskontonyckel som hello lösenord.**
    
    ![En skärmbild av hello Autentiseringsdialogrutan för nätverk](media/storage-file-how-to-use-files-windows/3_MountOnWindows10.png)

6. **Använd Azure-filresurser som du vill**.
    
    ![Azure-filresursen är nu monterad](media/storage-file-how-to-use-files-windows/4_MountOnWindows10.png)

7. **När du är klar toodismount (eller koppla från) hello Azure-filresursen du göra det genom att högerklicka på hello post för hello resurs under hello ”nätverksplatser” i Utforskaren och välja ”koppla från”**.

## <a name="mount-hello-azure-file-share-with-powershell"></a>Montera hello Azure-filresurs med PowerShell
1. **Använd hello följande kommando toomount hello Azure-filresursen**: spara tooreplace `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` med hello rätt information.

    ```PowerShell
    $acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
    New-PSDrive -Name <desired-drive-letter> -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\<share-name>" -Credential $credential
    ```

2. **Använd hello Azure-filresurs som du vill**.

3. **När du är klar demontera hello Azure filresurs med hjälp av följande kommando hello**.

    ```PowerShell
    Remove-PSDrive -Name <desired-drive-letter>
    ```

> [!Note]  
> Du kan använda hello `-Persist` parameter på `New-PSDrive` toomake hello Azure File share synliga toohello resten av hello OS när monterad.

## <a name="mount-hello-azure-file-share-with-command-prompt"></a>Montera hello Azure-filresurs med Kommandotolken
1. **Använd hello följande kommando toomount hello Azure-filresursen**: spara tooreplace `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` med hello rätt information.

    ```
    net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
    ```

2. **Använd hello Azure-filresurs som du vill**.

3. **När du är klar demontera hello Azure filresurs med hjälp av följande kommando hello**.

    ```
    net use <desired-drive-letter>: /delete
    ```

> [!Note]  
> Du kan konfigurera hello Azure File share tooautomatically återansluta vid omstart av bestående hello autentiseringsuppgifter i Windows. följande kommando hello behålls hello autentiseringsuppgifter:
>   ```
>   cmdkey /add:<storage-account-name>.file.core.windows.net /user:AZURE\<storage-account-name> /pass:<storage-account-key>
>   ```

## <a name="next-steps"></a>Nästa steg
Mer information om Azure File Storage finns på följande länkar.

* [Vanliga frågor och svar](storage-files-faq.md)
* [Felsökning](storage-troubleshoot-file-connection-problems.md)

### <a name="conceptual-articles-and-videos"></a>Begreppsrelaterade artiklar och videoklipp
* [Azure Files Storage: ett friktionslöst SMB-filsystem i molnet för Windows och Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Hur toouse Azure File storage med Linux](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-azure-file-storage"></a>Verktygsstöd för Azure File Storage
* [Använd Azure PowerShell med Azure Storage](storage-powershell-guide-full.md)
* [Hur toouse AzCopy med Microsoft Azure Storage](storage-use-azcopy.md)
* [Använda hello Azure CLI med Azure Storage](storage-azure-cli.md#create-and-manage-file-shares)
* [Felsökning av problem i Azure File Storage](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="blog-posts"></a>Blogginlägg
* [Azure File Storage finns nu allmänt tillgänglig](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Inuti Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Introduktion till Microsoft Azure File Service](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Migrera data tooAzure fil](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>Referens
* [Storage-klientbibliotek för .NET-referens](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [File Service REST API referens](http://msdn.microsoft.com/library/azure/dn167006.aspx)
