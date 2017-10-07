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
# <a name="mount-an-azure-file-share-and-access-hello-share-in-windows"></a><span data-ttu-id="92871-103">Montera en filresurs i Azure och åtkomst hello resurs i Windows</span><span class="sxs-lookup"><span data-stu-id="92871-103">Mount an Azure File share and access hello share in Windows</span></span>
<span data-ttu-id="92871-104">[Azure File storage](storage-dotnet-how-to-use-files.md) är Microsofts enkelt toouse moln-filsystem.</span><span class="sxs-lookup"><span data-stu-id="92871-104">[Azure File storage](storage-dotnet-how-to-use-files.md) is Microsoft's easy toouse cloud file system.</span></span> <span data-ttu-id="92871-105">Azure-filresurser kan monteras i Windows och Windows Server.</span><span class="sxs-lookup"><span data-stu-id="92871-105">Azure File shares can be mounted in Windows and Windows Server.</span></span> <span data-ttu-id="92871-106">Den här artikeln visar tre olika sätt toomount en Azure-filresurs på Windows: med hello File Explorer-Gränssnittet via PowerShell och via hello kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="92871-106">This article shows three different ways toomount an Azure File share on Windows: with hello File Explorer UI, via PowerShell, and via hello Command Prompt.</span></span> 

<span data-ttu-id="92871-107">I ordning toomount en Azure-filresurs utanför hello Azure-region finns i, till exempel lokalt eller i en annan Azure-region, stöder hello OS SMB 3.0.</span><span class="sxs-lookup"><span data-stu-id="92871-107">In order toomount an Azure File share outside of hello Azure region it is hosted in, such as on-premises or in a different Azure region, hello OS must support SMB 3.0.</span></span> 

<span data-ttu-id="92871-108">Azure File-resursen kan monteras på Windows-datorn antingen lokalt eller i Azure VM beroende på operativsystemversion.</span><span class="sxs-lookup"><span data-stu-id="92871-108">Azure File share can be mounted on Windows machine either on-premises or in Azure VM depending on OS version.</span></span> <span data-ttu-id="92871-109">Tabellen nedan visar hello</span><span class="sxs-lookup"><span data-stu-id="92871-109">Below table illustrates hello</span></span> 

| <span data-ttu-id="92871-110">Windows-version</span><span class="sxs-lookup"><span data-stu-id="92871-110">Windows Version</span></span>        | <span data-ttu-id="92871-111">SMB-version</span><span class="sxs-lookup"><span data-stu-id="92871-111">SMB Version</span></span> |<span data-ttu-id="92871-112">Monteras på Azure VM</span><span class="sxs-lookup"><span data-stu-id="92871-112">Mountable On Azure VM</span></span>|<span data-ttu-id="92871-113">Monteras lokalt</span><span class="sxs-lookup"><span data-stu-id="92871-113">Mountable On-Premise</span></span>|
|------------------------|-------------|---------------------|---------------------|
| <span data-ttu-id="92871-114">Windows 7</span><span class="sxs-lookup"><span data-stu-id="92871-114">Windows 7</span></span>              | <span data-ttu-id="92871-115">SMB 2.1</span><span class="sxs-lookup"><span data-stu-id="92871-115">SMB 2.1</span></span>     | <span data-ttu-id="92871-116">Ja</span><span class="sxs-lookup"><span data-stu-id="92871-116">Yes</span></span>                 | <span data-ttu-id="92871-117">Nej</span><span class="sxs-lookup"><span data-stu-id="92871-117">No</span></span>                  |
| <span data-ttu-id="92871-118">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="92871-118">Windows Server 2008 R2</span></span> | <span data-ttu-id="92871-119">SMB 2.1</span><span class="sxs-lookup"><span data-stu-id="92871-119">SMB 2.1</span></span>     | <span data-ttu-id="92871-120">Ja</span><span class="sxs-lookup"><span data-stu-id="92871-120">Yes</span></span>                 | <span data-ttu-id="92871-121">Nej</span><span class="sxs-lookup"><span data-stu-id="92871-121">No</span></span>                  |
| <span data-ttu-id="92871-122">Windows 8</span><span class="sxs-lookup"><span data-stu-id="92871-122">Windows 8</span></span>              | <span data-ttu-id="92871-123">SMB 3.0</span><span class="sxs-lookup"><span data-stu-id="92871-123">SMB 3.0</span></span>     | <span data-ttu-id="92871-124">Ja</span><span class="sxs-lookup"><span data-stu-id="92871-124">Yes</span></span>                 | <span data-ttu-id="92871-125">Ja</span><span class="sxs-lookup"><span data-stu-id="92871-125">Yes</span></span>                 |
| <span data-ttu-id="92871-126">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="92871-126">Windows Server 2012</span></span>    | <span data-ttu-id="92871-127">SMB 3.0</span><span class="sxs-lookup"><span data-stu-id="92871-127">SMB 3.0</span></span>     | <span data-ttu-id="92871-128">Ja</span><span class="sxs-lookup"><span data-stu-id="92871-128">Yes</span></span>                 | <span data-ttu-id="92871-129">Ja</span><span class="sxs-lookup"><span data-stu-id="92871-129">Yes</span></span>                 |
| <span data-ttu-id="92871-130">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="92871-130">Windows Server 2012 R2</span></span> | <span data-ttu-id="92871-131">SMB 3.0</span><span class="sxs-lookup"><span data-stu-id="92871-131">SMB 3.0</span></span>     | <span data-ttu-id="92871-132">Ja</span><span class="sxs-lookup"><span data-stu-id="92871-132">Yes</span></span>                 | <span data-ttu-id="92871-133">Ja</span><span class="sxs-lookup"><span data-stu-id="92871-133">Yes</span></span>                 |
| <span data-ttu-id="92871-134">Windows 10</span><span class="sxs-lookup"><span data-stu-id="92871-134">Windows 10</span></span>             | <span data-ttu-id="92871-135">SMB 3.0</span><span class="sxs-lookup"><span data-stu-id="92871-135">SMB 3.0</span></span>     | <span data-ttu-id="92871-136">Ja</span><span class="sxs-lookup"><span data-stu-id="92871-136">Yes</span></span>                 | <span data-ttu-id="92871-137">Ja</span><span class="sxs-lookup"><span data-stu-id="92871-137">Yes</span></span>                 |

> [!Note]  
> <span data-ttu-id="92871-138">Vi rekommenderar att alltid hello senaste KB för din version av Windows.</span><span class="sxs-lookup"><span data-stu-id="92871-138">We always recommend taking hello most recent KB for your version of Windows.</span></span>

## <a name="aprerequisites-for-mounting-azure-file-share-with-windows"></a><span data-ttu-id="92871-139"></a>Förutsättningar för att montera Azure-filresurser med Windows</span><span class="sxs-lookup"><span data-stu-id="92871-139"></a>Prerequisites for Mounting Azure File Share with Windows</span></span> 
* <span data-ttu-id="92871-140">**Lagringskontonamnet**: toomount ett Azure-filresurs, du behöver hello namnet på hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="92871-140">**Storage Account Name**: toomount an Azure File share, you will need hello name of hello storage account.</span></span>

* <span data-ttu-id="92871-141">**Lagringskontonyckel**: toomount ett Azure-filresurs, du behöver hello primära (eller sekundära) lagringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="92871-141">**Storage Account Key**: toomount an Azure File share, you will need hello primary (or secondary) storage key.</span></span> <span data-ttu-id="92871-142">SAS-nycklar stöds inte för montering.</span><span class="sxs-lookup"><span data-stu-id="92871-142">SAS keys are not currently supported for mounting.</span></span>

* <span data-ttu-id="92871-143">**Kontrollera att port 445 är öppen**: Azure File Storage använder SMB-protokollet.</span><span class="sxs-lookup"><span data-stu-id="92871-143">**Ensure port 445 is open**: Azure File storage uses SMB protocol.</span></span> <span data-ttu-id="92871-144">SMB kommunicerar via TCP-port 445 - Kontrollera toosee om brandväggen inte blockerar TCP-portar 445 från klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="92871-144">SMB communicates over TCP port 445 - check toosee if your firewall is not blocking TCP ports 445 from client machine.</span></span>

## <a name="mount-hello-azure-file-share-with-file-explorer"></a><span data-ttu-id="92871-145">Montera hello Azure-filresurs med Utforskaren</span><span class="sxs-lookup"><span data-stu-id="92871-145">Mount hello Azure File share with File Explorer</span></span>
> [!Note]  
> <span data-ttu-id="92871-146">Observera att hello följa anvisningar visas på Windows 10 och kan skilja sig på äldre versioner.</span><span class="sxs-lookup"><span data-stu-id="92871-146">Note that hello following instructions are shown on Windows 10 and may differ slightly on older releases.</span></span> 

1. <span data-ttu-id="92871-147">**Öppna Utforskaren**: Detta kan göras genom att öppna från hello Start-menyn eller genom att trycka på Win + E genväg.</span><span class="sxs-lookup"><span data-stu-id="92871-147">**Open File Explorer**: This can be done by opening from hello Start Menu, or by pressing Win+E shortcut.</span></span>

2. <span data-ttu-id="92871-148">**Navigera toohello ”den här datorn” objektet hello vänster av hello-fönstret. Detta ändrar hello menyerna hello menyfliksområdet. Under hello dator-menyn väljer du ”karta nätverksenhet”**.</span><span class="sxs-lookup"><span data-stu-id="92871-148">**Navigate toohello "This PC" item on hello left-hand side of hello window. This will change hello menus available in hello ribbon. Under hello Computer menu, select "Map Network Drive"**.</span></span>
    
    ![En skärmbild av hello ”mappa nätverksenheten” nedrullningsbara menyn](media/storage-file-how-to-use-files-windows/1_MountOnWindows10.png)

3. <span data-ttu-id="92871-150">**Kopiera hello UNC-sökväg från hello ”Anslut” fönster i hello Azure-portalen**: en detaljerad beskrivning av hur toofind den här informationen kan hittas [här](storage-file-how-to-use-files-portal.md#connect-to-file-share).</span><span class="sxs-lookup"><span data-stu-id="92871-150">**Copy hello UNC path from hello "Connect" pane in hello Azure portal**: A detailed description of how toofind this information can be found [here](storage-file-how-to-use-files-portal.md#connect-to-file-share).</span></span>

    ![hello UNC-sökväg i hello Azure File storage Anslut rutan](media/storage-file-how-to-use-files-windows/portal_netuse_connect.png)

4. <span data-ttu-id="92871-152">**Välj hello enhetsbeteckning och ange hello UNC-sökväg.**</span><span class="sxs-lookup"><span data-stu-id="92871-152">**Select hello Drive letter and enter hello UNC path.**</span></span> 
    
    ![En skärmbild av hello ”karta nätverksenhet” dialogrutan](media/storage-file-how-to-use-files-windows/2_MountOnWindows10.png)

5. <span data-ttu-id="92871-154">**Använd hello Lagringskontonamnet föregås av `Azure\` som hello användarnamn och en Lagringskontonyckel som hello lösenord.**</span><span class="sxs-lookup"><span data-stu-id="92871-154">**Use hello Storage Account Name prepended with `Azure\` as hello username and a Storage Account Key as hello password.**</span></span>
    
    ![En skärmbild av hello Autentiseringsdialogrutan för nätverk](media/storage-file-how-to-use-files-windows/3_MountOnWindows10.png)

6. <span data-ttu-id="92871-156">**Använd Azure-filresurser som du vill**.</span><span class="sxs-lookup"><span data-stu-id="92871-156">**Use Azure File share as desired**.</span></span>
    
    ![Azure-filresursen är nu monterad](media/storage-file-how-to-use-files-windows/4_MountOnWindows10.png)

7. <span data-ttu-id="92871-158">**När du är klar toodismount (eller koppla från) hello Azure-filresursen du göra det genom att högerklicka på hello post för hello resurs under hello ”nätverksplatser” i Utforskaren och välja ”koppla från”**.</span><span class="sxs-lookup"><span data-stu-id="92871-158">**When you are ready toodismount (or disconnect) hello Azure File share, you can do so by right clicking on hello entry for hello share under hello "Network locations" in File Explorer and selecting "Disconnect"**.</span></span>

## <a name="mount-hello-azure-file-share-with-powershell"></a><span data-ttu-id="92871-159">Montera hello Azure-filresurs med PowerShell</span><span class="sxs-lookup"><span data-stu-id="92871-159">Mount hello Azure File share with PowerShell</span></span>
1. <span data-ttu-id="92871-160">**Använd hello följande kommando toomount hello Azure-filresursen**: spara tooreplace `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` med hello rätt information.</span><span class="sxs-lookup"><span data-stu-id="92871-160">**Use hello following command toomount hello Azure File share**: Remember tooreplace `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` with hello proper information.</span></span>

    ```PowerShell
    $acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
    New-PSDrive -Name <desired-drive-letter> -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\<share-name>" -Credential $credential
    ```

2. <span data-ttu-id="92871-161">**Använd hello Azure-filresurs som du vill**.</span><span class="sxs-lookup"><span data-stu-id="92871-161">**Use hello Azure File share as desired**.</span></span>

3. <span data-ttu-id="92871-162">**När du är klar demontera hello Azure filresurs med hjälp av följande kommando hello**.</span><span class="sxs-lookup"><span data-stu-id="92871-162">**When you are finished, dismount hello Azure File share using hello following command**.</span></span>

    ```PowerShell
    Remove-PSDrive -Name <desired-drive-letter>
    ```

> [!Note]  
> <span data-ttu-id="92871-163">Du kan använda hello `-Persist` parameter på `New-PSDrive` toomake hello Azure File share synliga toohello resten av hello OS när monterad.</span><span class="sxs-lookup"><span data-stu-id="92871-163">You may use hello `-Persist` parameter on `New-PSDrive` toomake hello Azure File share visible toohello rest of hello OS while mounted.</span></span>

## <a name="mount-hello-azure-file-share-with-command-prompt"></a><span data-ttu-id="92871-164">Montera hello Azure-filresurs med Kommandotolken</span><span class="sxs-lookup"><span data-stu-id="92871-164">Mount hello Azure File share with Command Prompt</span></span>
1. <span data-ttu-id="92871-165">**Använd hello följande kommando toomount hello Azure-filresursen**: spara tooreplace `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` med hello rätt information.</span><span class="sxs-lookup"><span data-stu-id="92871-165">**Use hello following command toomount hello Azure File share**: Remember tooreplace `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` with hello proper information.</span></span>

    ```
    net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
    ```

2. <span data-ttu-id="92871-166">**Använd hello Azure-filresurs som du vill**.</span><span class="sxs-lookup"><span data-stu-id="92871-166">**Use hello Azure File share as desired**.</span></span>

3. <span data-ttu-id="92871-167">**När du är klar demontera hello Azure filresurs med hjälp av följande kommando hello**.</span><span class="sxs-lookup"><span data-stu-id="92871-167">**When you are finished, dismount hello Azure File share using hello following command**.</span></span>

    ```
    net use <desired-drive-letter>: /delete
    ```

> [!Note]  
> <span data-ttu-id="92871-168">Du kan konfigurera hello Azure File share tooautomatically återansluta vid omstart av bestående hello autentiseringsuppgifter i Windows.</span><span class="sxs-lookup"><span data-stu-id="92871-168">You can configure hello Azure File share tooautomatically reconnect on reboot by persisting hello credentials in Windows.</span></span> <span data-ttu-id="92871-169">följande kommando hello behålls hello autentiseringsuppgifter:</span><span class="sxs-lookup"><span data-stu-id="92871-169">hello following command will persist hello credentials:</span></span>
>   ```
>   cmdkey /add:<storage-account-name>.file.core.windows.net /user:AZURE\<storage-account-name> /pass:<storage-account-key>
>   ```

## <a name="next-steps"></a><span data-ttu-id="92871-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="92871-170">Next steps</span></span>
<span data-ttu-id="92871-171">Mer information om Azure File Storage finns på följande länkar.</span><span class="sxs-lookup"><span data-stu-id="92871-171">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="92871-172">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="92871-172">FAQ</span></span>](storage-files-faq.md)
* [<span data-ttu-id="92871-173">Felsökning</span><span class="sxs-lookup"><span data-stu-id="92871-173">Troubleshooting</span></span>](storage-troubleshoot-file-connection-problems.md)

### <a name="conceptual-articles-and-videos"></a><span data-ttu-id="92871-174">Begreppsrelaterade artiklar och videoklipp</span><span class="sxs-lookup"><span data-stu-id="92871-174">Conceptual articles and videos</span></span>
* [<span data-ttu-id="92871-175">Azure Files Storage: ett friktionslöst SMB-filsystem i molnet för Windows och Linux</span><span class="sxs-lookup"><span data-stu-id="92871-175">Azure File storage: a frictionless cloud SMB file system for Windows and Linux</span></span>](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [<span data-ttu-id="92871-176">Hur toouse Azure File storage med Linux</span><span class="sxs-lookup"><span data-stu-id="92871-176">How toouse Azure File storage with Linux</span></span>](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-azure-file-storage"></a><span data-ttu-id="92871-177">Verktygsstöd för Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="92871-177">Tooling support for Azure File storage</span></span>
* [<span data-ttu-id="92871-178">Använd Azure PowerShell med Azure Storage</span><span class="sxs-lookup"><span data-stu-id="92871-178">Using Azure PowerShell with Azure Storage</span></span>](storage-powershell-guide-full.md)
* [<span data-ttu-id="92871-179">Hur toouse AzCopy med Microsoft Azure Storage</span><span class="sxs-lookup"><span data-stu-id="92871-179">How toouse AzCopy with Microsoft Azure Storage</span></span>](storage-use-azcopy.md)
* [<span data-ttu-id="92871-180">Använda hello Azure CLI med Azure Storage</span><span class="sxs-lookup"><span data-stu-id="92871-180">Using hello Azure CLI with Azure Storage</span></span>](storage-azure-cli.md#create-and-manage-file-shares)
* [<span data-ttu-id="92871-181">Felsökning av problem i Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="92871-181">Troubleshooting Azure File storage problems</span></span>](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="blog-posts"></a><span data-ttu-id="92871-182">Blogginlägg</span><span class="sxs-lookup"><span data-stu-id="92871-182">Blog posts</span></span>
* [<span data-ttu-id="92871-183">Azure File Storage finns nu allmänt tillgänglig</span><span class="sxs-lookup"><span data-stu-id="92871-183">Azure File storage is now generally available</span></span>](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [<span data-ttu-id="92871-184">Inuti Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="92871-184">Inside Azure File storage</span></span>](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [<span data-ttu-id="92871-185">Introduktion till Microsoft Azure File Service</span><span class="sxs-lookup"><span data-stu-id="92871-185">Introducing Microsoft Azure File Service</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [<span data-ttu-id="92871-186">Migrera data tooAzure fil</span><span class="sxs-lookup"><span data-stu-id="92871-186">Migrating data tooAzure File </span></span>](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a><span data-ttu-id="92871-187">Referens</span><span class="sxs-lookup"><span data-stu-id="92871-187">Reference</span></span>
* [<span data-ttu-id="92871-188">Storage-klientbibliotek för .NET-referens</span><span class="sxs-lookup"><span data-stu-id="92871-188">Storage Client Library for .NET reference</span></span>](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [<span data-ttu-id="92871-189">File Service REST API referens</span><span class="sxs-lookup"><span data-stu-id="92871-189">File Service REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dn167006.aspx)
