---
title: "Montera en filresurs på Azure och få åtkomst till resursen i Windows | Microsoft Docs"
description: "Montera en filresurs på Azure och få åtkomst till resursen i Windows."
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
ms.openlocfilehash: e911e787cd1e29b2bbeaa648869c50245f2dd9ba
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="mount-an-azure-file-share-and-access-the-share-in-windows"></a><span data-ttu-id="656a1-103">Montera en filresurs på Azure och få åtkomst till resursen i Windows</span><span class="sxs-lookup"><span data-stu-id="656a1-103">Mount an Azure File share and access the share in Windows</span></span>
<span data-ttu-id="656a1-104">[Azure File Storage](storage-dotnet-how-to-use-files.md) är Microsofts lättanvända filsystem i molnet.</span><span class="sxs-lookup"><span data-stu-id="656a1-104">[Azure File storage](storage-dotnet-how-to-use-files.md) is Microsoft's easy to use cloud file system.</span></span> <span data-ttu-id="656a1-105">Azure-filresurser kan monteras i Windows och Windows Server.</span><span class="sxs-lookup"><span data-stu-id="656a1-105">Azure File shares can be mounted in Windows and Windows Server.</span></span> <span data-ttu-id="656a1-106">Den här artikeln visar tre olika sätt att montera en Azure-filresurs på Windows: med användargränssnittet Utforskaren, via PowerShell eller via Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="656a1-106">This article shows three different ways to mount an Azure File share on Windows: with the File Explorer UI, via PowerShell, and via the Command Prompt.</span></span> 

<span data-ttu-id="656a1-107">Operativsystemet måste stödja SMB 3.0 för att montera en Azure-filresurs utanför den Azure-region som den finns i, till exempel lokalt eller i en annan Azure-region.</span><span class="sxs-lookup"><span data-stu-id="656a1-107">In order to mount an Azure File share outside of the Azure region it is hosted in, such as on-premises or in a different Azure region, the OS must support SMB 3.0.</span></span> 

<span data-ttu-id="656a1-108">Azure File-resursen kan monteras på Windows-datorn antingen lokalt eller i Azure VM beroende på operativsystemversion.</span><span class="sxs-lookup"><span data-stu-id="656a1-108">Azure File share can be mounted on Windows machine either on-premises or in Azure VM depending on OS version.</span></span> <span data-ttu-id="656a1-109">Tabellen nedan visar den</span><span class="sxs-lookup"><span data-stu-id="656a1-109">Below table illustrates the</span></span> 

| <span data-ttu-id="656a1-110">Windows-version</span><span class="sxs-lookup"><span data-stu-id="656a1-110">Windows Version</span></span>        | <span data-ttu-id="656a1-111">SMB-version</span><span class="sxs-lookup"><span data-stu-id="656a1-111">SMB Version</span></span> |<span data-ttu-id="656a1-112">Monteras på Azure VM</span><span class="sxs-lookup"><span data-stu-id="656a1-112">Mountable On Azure VM</span></span>|<span data-ttu-id="656a1-113">Monteras lokalt</span><span class="sxs-lookup"><span data-stu-id="656a1-113">Mountable On-Premise</span></span>|
|------------------------|-------------|---------------------|---------------------|
| <span data-ttu-id="656a1-114">Windows 7</span><span class="sxs-lookup"><span data-stu-id="656a1-114">Windows 7</span></span>              | <span data-ttu-id="656a1-115">SMB 2.1</span><span class="sxs-lookup"><span data-stu-id="656a1-115">SMB 2.1</span></span>     | <span data-ttu-id="656a1-116">Ja</span><span class="sxs-lookup"><span data-stu-id="656a1-116">Yes</span></span>                 | <span data-ttu-id="656a1-117">Nej</span><span class="sxs-lookup"><span data-stu-id="656a1-117">No</span></span>                  |
| <span data-ttu-id="656a1-118">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="656a1-118">Windows Server 2008 R2</span></span> | <span data-ttu-id="656a1-119">SMB 2.1</span><span class="sxs-lookup"><span data-stu-id="656a1-119">SMB 2.1</span></span>     | <span data-ttu-id="656a1-120">Ja</span><span class="sxs-lookup"><span data-stu-id="656a1-120">Yes</span></span>                 | <span data-ttu-id="656a1-121">Nej</span><span class="sxs-lookup"><span data-stu-id="656a1-121">No</span></span>                  |
| <span data-ttu-id="656a1-122">Windows 8</span><span class="sxs-lookup"><span data-stu-id="656a1-122">Windows 8</span></span>              | <span data-ttu-id="656a1-123">SMB 3.0</span><span class="sxs-lookup"><span data-stu-id="656a1-123">SMB 3.0</span></span>     | <span data-ttu-id="656a1-124">Ja</span><span class="sxs-lookup"><span data-stu-id="656a1-124">Yes</span></span>                 | <span data-ttu-id="656a1-125">Ja</span><span class="sxs-lookup"><span data-stu-id="656a1-125">Yes</span></span>                 |
| <span data-ttu-id="656a1-126">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="656a1-126">Windows Server 2012</span></span>    | <span data-ttu-id="656a1-127">SMB 3.0</span><span class="sxs-lookup"><span data-stu-id="656a1-127">SMB 3.0</span></span>     | <span data-ttu-id="656a1-128">Ja</span><span class="sxs-lookup"><span data-stu-id="656a1-128">Yes</span></span>                 | <span data-ttu-id="656a1-129">Ja</span><span class="sxs-lookup"><span data-stu-id="656a1-129">Yes</span></span>                 |
| <span data-ttu-id="656a1-130">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="656a1-130">Windows Server 2012 R2</span></span> | <span data-ttu-id="656a1-131">SMB 3.0</span><span class="sxs-lookup"><span data-stu-id="656a1-131">SMB 3.0</span></span>     | <span data-ttu-id="656a1-132">Ja</span><span class="sxs-lookup"><span data-stu-id="656a1-132">Yes</span></span>                 | <span data-ttu-id="656a1-133">Ja</span><span class="sxs-lookup"><span data-stu-id="656a1-133">Yes</span></span>                 |
| <span data-ttu-id="656a1-134">Windows 10</span><span class="sxs-lookup"><span data-stu-id="656a1-134">Windows 10</span></span>             | <span data-ttu-id="656a1-135">SMB 3.0</span><span class="sxs-lookup"><span data-stu-id="656a1-135">SMB 3.0</span></span>     | <span data-ttu-id="656a1-136">Ja</span><span class="sxs-lookup"><span data-stu-id="656a1-136">Yes</span></span>                 | <span data-ttu-id="656a1-137">Ja</span><span class="sxs-lookup"><span data-stu-id="656a1-137">Yes</span></span>                 |

> [!Note]  
> <span data-ttu-id="656a1-138">Vi rekommenderar alltid den senaste uppdateringen för din version av Windows.</span><span class="sxs-lookup"><span data-stu-id="656a1-138">We always recommend taking the most recent KB for your version of Windows.</span></span>

## <a name="aprerequisites-for-mounting-azure-file-share-with-windows"></a><span data-ttu-id="656a1-139"></a>Förutsättningar för att montera Azure-filresurser med Windows</span><span class="sxs-lookup"><span data-stu-id="656a1-139"></a>Prerequisites for Mounting Azure File Share with Windows</span></span> 
* <span data-ttu-id="656a1-140">**Lagringskontonamn**: Om du vill montera en Azure-filresurs behöver du namnet på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="656a1-140">**Storage Account Name**: To mount an Azure File share, you will need the name of the storage account.</span></span>

* <span data-ttu-id="656a1-141">**Lagringskontonyckel**: Om du vill montera en Azure-filresurs behöver du den primära (eller sekundära) lagringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="656a1-141">**Storage Account Key**: To mount an Azure File share, you will need the primary (or secondary) storage key.</span></span> <span data-ttu-id="656a1-142">SAS-nycklar stöds inte för montering.</span><span class="sxs-lookup"><span data-stu-id="656a1-142">SAS keys are not currently supported for mounting.</span></span>

* <span data-ttu-id="656a1-143">**Kontrollera att port 445 är öppen**: Azure File Storage använder SMB-protokollet.</span><span class="sxs-lookup"><span data-stu-id="656a1-143">**Ensure port 445 is open**: Azure File storage uses SMB protocol.</span></span> <span data-ttu-id="656a1-144">SMB kommunicerar via TCP-port 445. Kontrollera om din brandvägg blockerar TCP-port 445 från klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="656a1-144">SMB communicates over TCP port 445 - check to see if your firewall is not blocking TCP ports 445 from client machine.</span></span>

## <a name="mount-the-azure-file-share-with-file-explorer"></a><span data-ttu-id="656a1-145">Montera Azure-filresursen med Utforskaren</span><span class="sxs-lookup"><span data-stu-id="656a1-145">Mount the Azure File share with File Explorer</span></span>
> [!Note]  
> <span data-ttu-id="656a1-146">Observera att följande instruktioner visas på Windows 10 och kan skilja sig en aning på äldre versioner.</span><span class="sxs-lookup"><span data-stu-id="656a1-146">Note that the following instructions are shown on Windows 10 and may differ slightly on older releases.</span></span> 

1. <span data-ttu-id="656a1-147">**Öppna Utforskaren**: Detta kan göras genom att öppna från startmenyn eller genom att använda tangentbordsgenvägen Win+E.</span><span class="sxs-lookup"><span data-stu-id="656a1-147">**Open File Explorer**: This can be done by opening from the Start Menu, or by pressing Win+E shortcut.</span></span>

2. <span data-ttu-id="656a1-148">**Navigera till objektet "Den här datorn" på vänster sida av fönstret. Detta ändrar menyerna i menyfliksområdet. Under menyn Dator väljer du "Anslut nätverksenhet"**.</span><span class="sxs-lookup"><span data-stu-id="656a1-148">**Navigate to the "This PC" item on the left-hand side of the window. This will change the menus available in the ribbon. Under the Computer menu, select "Map Network Drive"**.</span></span>
    
    ![En skärmbild av den nedrullningsbara menyn "Anslut nätverksenhet"](media/storage-file-how-to-use-files-windows/1_MountOnWindows10.png)

3. <span data-ttu-id="656a1-150">**Kopiera UNC-sökvägen från fönstret "Anslut" i Azure Portal**: En detaljerad beskrivning av hur du hittar den här informationen hittar du [här](storage-file-how-to-use-files-portal.md#connect-to-file-share).</span><span class="sxs-lookup"><span data-stu-id="656a1-150">**Copy the UNC path from the "Connect" pane in the Azure portal**: A detailed description of how to find this information can be found [here](storage-file-how-to-use-files-portal.md#connect-to-file-share).</span></span>

    ![UNC-sökvägen från fönstret Anslut i Azure File Storage](media/storage-file-how-to-use-files-windows/portal_netuse_connect.png)

4. <span data-ttu-id="656a1-152">**Välj enhetsbeteckningen och ange UNC-sökvägen.**</span><span class="sxs-lookup"><span data-stu-id="656a1-152">**Select the Drive letter and enter the UNC path.**</span></span> 
    
    ![En skärmbild av dialogrutan "Anslut nätverksenhet"](media/storage-file-how-to-use-files-windows/2_MountOnWindows10.png)

5. <span data-ttu-id="656a1-154">**Använd lagringskontonamnet som börjar med `Azure\` som användarnamnet och en lagringskontonyckel som lösenord.**</span><span class="sxs-lookup"><span data-stu-id="656a1-154">**Use the Storage Account Name prepended with `Azure\` as the username and a Storage Account Key as the password.**</span></span>
    
    ![En skärmbild av dialogrutan med nätverksautentiseringsuppgifter](media/storage-file-how-to-use-files-windows/3_MountOnWindows10.png)

6. <span data-ttu-id="656a1-156">**Använd Azure-filresurser som du vill**.</span><span class="sxs-lookup"><span data-stu-id="656a1-156">**Use Azure File share as desired**.</span></span>
    
    ![Azure-filresursen är nu monterad](media/storage-file-how-to-use-files-windows/4_MountOnWindows10.png)

7. <span data-ttu-id="656a1-158">**När du är redo att demontera (eller koppla från) Azure-filresursen så kan du göra det genom att högerklicka på posten för resursen under "Nätverksplatser" i Utforskaren och välja "Koppla från"**.</span><span class="sxs-lookup"><span data-stu-id="656a1-158">**When you are ready to dismount (or disconnect) the Azure File share, you can do so by right clicking on the entry for the share under the "Network locations" in File Explorer and selecting "Disconnect"**.</span></span>

## <a name="mount-the-azure-file-share-with-powershell"></a><span data-ttu-id="656a1-159">Montera Azure-filresursen med PowerShell</span><span class="sxs-lookup"><span data-stu-id="656a1-159">Mount the Azure File share with PowerShell</span></span>
1. <span data-ttu-id="656a1-160">**Använd följande kommando för att montera Azure-filresursen**: Kom ihåg att ersätta `<storage-account-name>`, `<share-name>`, `<storage-account-key>` och `<desired-drive-letter>` med rätt information.</span><span class="sxs-lookup"><span data-stu-id="656a1-160">**Use the following command to mount the Azure File share**: Remember to replace `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` with the proper information.</span></span>

    ```PowerShell
    $acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
    New-PSDrive -Name <desired-drive-letter> -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\<share-name>" -Credential $credential
    ```

2. <span data-ttu-id="656a1-161">**Använd Azure-filresursen som du vill**.</span><span class="sxs-lookup"><span data-stu-id="656a1-161">**Use the Azure File share as desired**.</span></span>

3. <span data-ttu-id="656a1-162">**Demontera Azure-filresursen med följande kommando när du är klar**.</span><span class="sxs-lookup"><span data-stu-id="656a1-162">**When you are finished, dismount the Azure File share using the following command**.</span></span>

    ```PowerShell
    Remove-PSDrive -Name <desired-drive-letter>
    ```

> [!Note]  
> <span data-ttu-id="656a1-163">Du kan använda `-Persist`-parametern på `New-PSDrive` för att göra Azure-filresursen synlig för resten av operativsystemet när den är monterad.</span><span class="sxs-lookup"><span data-stu-id="656a1-163">You may use the `-Persist` parameter on `New-PSDrive` to make the Azure File share visible to the rest of the OS while mounted.</span></span>

## <a name="mount-the-azure-file-share-with-command-prompt"></a><span data-ttu-id="656a1-164">Montera Azure-filresursen med Kommandotolken</span><span class="sxs-lookup"><span data-stu-id="656a1-164">Mount the Azure File share with Command Prompt</span></span>
1. <span data-ttu-id="656a1-165">**Använd följande kommando för att montera Azure-filresursen**: Kom ihåg att ersätta `<storage-account-name>`, `<share-name>`, `<storage-account-key>` och `<desired-drive-letter>` med rätt information.</span><span class="sxs-lookup"><span data-stu-id="656a1-165">**Use the following command to mount the Azure File share**: Remember to replace `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` with the proper information.</span></span>

    ```
    net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
    ```

2. <span data-ttu-id="656a1-166">**Använd Azure-filresursen som du vill**.</span><span class="sxs-lookup"><span data-stu-id="656a1-166">**Use the Azure File share as desired**.</span></span>

3. <span data-ttu-id="656a1-167">**Demontera Azure-filresursen med följande kommando när du är klar**.</span><span class="sxs-lookup"><span data-stu-id="656a1-167">**When you are finished, dismount the Azure File share using the following command**.</span></span>

    ```
    net use <desired-drive-letter>: /delete
    ```

> [!Note]  
> <span data-ttu-id="656a1-168">Du kan konfigurera Azure-filresursen för att återansluta automatiskt efter omstart genom att spara autentiseringsuppgifterna i Windows.</span><span class="sxs-lookup"><span data-stu-id="656a1-168">You can configure the Azure File share to automatically reconnect on reboot by persisting the credentials in Windows.</span></span> <span data-ttu-id="656a1-169">Följande kommando sparar autentiseringsuppgifter:</span><span class="sxs-lookup"><span data-stu-id="656a1-169">The following command will persist the credentials:</span></span>
>   ```
>   cmdkey /add:<storage-account-name>.file.core.windows.net /user:AZURE\<storage-account-name> /pass:<storage-account-key>
>   ```

## <a name="next-steps"></a><span data-ttu-id="656a1-170">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="656a1-170">Next steps</span></span>
<span data-ttu-id="656a1-171">Mer information om Azure File Storage finns på följande länkar.</span><span class="sxs-lookup"><span data-stu-id="656a1-171">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="656a1-172">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="656a1-172">FAQ</span></span>](storage-files-faq.md)
* [<span data-ttu-id="656a1-173">Felsökning</span><span class="sxs-lookup"><span data-stu-id="656a1-173">Troubleshooting</span></span>](storage-troubleshoot-file-connection-problems.md)

### <a name="conceptual-articles-and-videos"></a><span data-ttu-id="656a1-174">Begreppsrelaterade artiklar och videoklipp</span><span class="sxs-lookup"><span data-stu-id="656a1-174">Conceptual articles and videos</span></span>
* [<span data-ttu-id="656a1-175">Azure Files Storage: ett friktionslöst SMB-filsystem i molnet för Windows och Linux</span><span class="sxs-lookup"><span data-stu-id="656a1-175">Azure File storage: a frictionless cloud SMB file system for Windows and Linux</span></span>](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [<span data-ttu-id="656a1-176">Använd Azure File Storage med Linux</span><span class="sxs-lookup"><span data-stu-id="656a1-176">How to use Azure File storage with Linux</span></span>](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-azure-file-storage"></a><span data-ttu-id="656a1-177">Verktygsstöd för Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="656a1-177">Tooling support for Azure File storage</span></span>
* [<span data-ttu-id="656a1-178">Använd Azure PowerShell med Azure Storage</span><span class="sxs-lookup"><span data-stu-id="656a1-178">Using Azure PowerShell with Azure Storage</span></span>](storage-powershell-guide-full.md)
* [<span data-ttu-id="656a1-179">Använd AzCopy med Microsoft Azure Storage</span><span class="sxs-lookup"><span data-stu-id="656a1-179">How to use AzCopy with Microsoft Azure Storage</span></span>](storage-use-azcopy.md)
* [<span data-ttu-id="656a1-180">Använd Azure CLI:et med Azure Storage</span><span class="sxs-lookup"><span data-stu-id="656a1-180">Using the Azure CLI with Azure Storage</span></span>](storage-azure-cli.md#create-and-manage-file-shares)
* [<span data-ttu-id="656a1-181">Felsökning av problem i Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="656a1-181">Troubleshooting Azure File storage problems</span></span>](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="blog-posts"></a><span data-ttu-id="656a1-182">Blogginlägg</span><span class="sxs-lookup"><span data-stu-id="656a1-182">Blog posts</span></span>
* [<span data-ttu-id="656a1-183">Azure File Storage finns nu allmänt tillgänglig</span><span class="sxs-lookup"><span data-stu-id="656a1-183">Azure File storage is now generally available</span></span>](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [<span data-ttu-id="656a1-184">Inuti Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="656a1-184">Inside Azure File storage</span></span>](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [<span data-ttu-id="656a1-185">Introduktion till Microsoft Azure File Service</span><span class="sxs-lookup"><span data-stu-id="656a1-185">Introducing Microsoft Azure File Service</span></span>](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [<span data-ttu-id="656a1-186">Migrera data till Azure File</span><span class="sxs-lookup"><span data-stu-id="656a1-186">Migrating data to Azure File </span></span>](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a><span data-ttu-id="656a1-187">Referens</span><span class="sxs-lookup"><span data-stu-id="656a1-187">Reference</span></span>
* [<span data-ttu-id="656a1-188">Storage-klientbibliotek för .NET-referens</span><span class="sxs-lookup"><span data-stu-id="656a1-188">Storage Client Library for .NET reference</span></span>](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [<span data-ttu-id="656a1-189">File Service REST API referens</span><span class="sxs-lookup"><span data-stu-id="656a1-189">File Service REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dn167006.aspx)
