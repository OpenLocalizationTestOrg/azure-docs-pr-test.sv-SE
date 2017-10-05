---
title: "Viktig information för Microsoft Azure Lagringsutforskaren (förhandsversion) | Microsoft Docs"
description: "Viktig information för Microsoft Azure Lagringsutforskaren (förhandsversion)"
services: storage
documentationcenter: na
author: cawa
manager: paulyuk
editor: 
ms.assetid: 
ms.service: storage
ms.devlang: multiple
ms.topic: release-notes
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2017
ms.author: cawa
ms.openlocfilehash: 63a24f6b153390533bba0888fd1051508c65bf6e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="microsoft-azure-storage-explorer-preview-release-notes"></a><span data-ttu-id="232e4-103">Viktig information för Microsoft Azure Lagringsutforskaren (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="232e4-103">Microsoft Azure Storage Explorer (Preview) release notes</span></span>

<span data-ttu-id="232e4-104">Den här artikeln innehåller versionen viktig information för Azure Lagringsutforskaren 0.8.16 (förhandsgranskning) samt viktig information för tidigare versioner.</span><span class="sxs-lookup"><span data-stu-id="232e4-104">This article contains the release notes for Azure Storage Explorer 0.8.16 (Preview) release, as well as release notes for previous versions.</span></span>

<span data-ttu-id="232e4-105">[Microsoft Azure Lagringsutforskaren (förhandsversion)](./vs-azure-tools-storage-manage-with-storage-explorer.md) är en fristående app som gör det enkelt att arbeta med Azure Storage-data i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="232e4-105">[Microsoft Azure Storage Explorer (Preview)](./vs-azure-tools-storage-manage-with-storage-explorer.md) is a standalone app that enables you to easily work with Azure Storage data on Windows, macOS, and Linux.</span></span>

## <a name="version-0816-preview"></a><span data-ttu-id="232e4-106">Version 0.8.16 (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="232e4-106">Version 0.8.16 (Preview)</span></span>
<span data-ttu-id="232e4-107">8/21/2017</span><span class="sxs-lookup"><span data-stu-id="232e4-107">8/21/2017</span></span>

### <a name="download-azure-storage-explorer-0816-preview"></a><span data-ttu-id="232e4-108">Hämta Azure Lagringsutforskaren 0.8.16 (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="232e4-108">Download Azure Storage Explorer 0.8.16 (Preview)</span></span>
- [<span data-ttu-id="232e4-109">Azure Lagringsutforskaren 0.8.16 (förhandsversion) för Windows</span><span class="sxs-lookup"><span data-stu-id="232e4-109">Azure Storage Explorer 0.8.16 (Preview) for Windows</span></span>](https://go.microsoft.com/fwlink/?LinkId=708343)
- [<span data-ttu-id="232e4-110">Azure Lagringsutforskaren (förhandsversion) för 0.8.16 för Mac</span><span class="sxs-lookup"><span data-stu-id="232e4-110">Azure Storage Explorer 0.8.16 (Preview) for Mac</span></span>](https://go.microsoft.com/fwlink/?LinkId=708342)
- [<span data-ttu-id="232e4-111">Azure Lagringsutforskaren (förhandsversion) för 0.8.16 för Linux</span><span class="sxs-lookup"><span data-stu-id="232e4-111">Azure Storage Explorer 0.8.16 (Preview) for Linux</span></span>](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a><span data-ttu-id="232e4-112">Ny</span><span class="sxs-lookup"><span data-stu-id="232e4-112">New</span></span>
* <span data-ttu-id="232e4-113">När du öppnar en blob uppmanas Lagringsutforskaren du att ladda upp den hämta filen om en ändring identifieras</span><span class="sxs-lookup"><span data-stu-id="232e4-113">When you open a blob, Storage Explorer will prompt you to upload the downloaded file if a change is detected</span></span>
* <span data-ttu-id="232e4-114">Förbättrad Azure Stack inloggning</span><span class="sxs-lookup"><span data-stu-id="232e4-114">Enhanced Azure Stack sign-in experience</span></span>
* <span data-ttu-id="232e4-115">Bättre prestanda för att ladda upp/hämta många små filer på samma gång</span><span class="sxs-lookup"><span data-stu-id="232e4-115">Improved the performance of uploading/downloading many small files at the same time</span></span>


### <a name="fixes"></a><span data-ttu-id="232e4-116">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="232e4-116">Fixes</span></span>
* <span data-ttu-id="232e4-117">För vissa blob-typer leder om du väljer att ”replace” under en överför hamnar i konflikt ibland överföringen startas.</span><span class="sxs-lookup"><span data-stu-id="232e4-117">For some blob types, choosing to "replace" during an upload conflict would sometimes result in the upload being restarted.</span></span> 
* <span data-ttu-id="232e4-118">I version 0.8.15 skulle överföringar ibland av stopp vid 99%.</span><span class="sxs-lookup"><span data-stu-id="232e4-118">In version 0.8.15, uploads would sometimes stall at 99%.</span></span>
* <span data-ttu-id="232e4-119">När överföringen av filer till en filresurs, om du vill överföra till en katalog som inte ännu finns, misslyckas överföra.</span><span class="sxs-lookup"><span data-stu-id="232e4-119">When uploading files to a file share, if you chose to upload to a directory which did not yet exist, your upload would fail.</span></span>
* <span data-ttu-id="232e4-120">Lagringsutforskaren felaktigt generera tidsstämplar för signaturer för delad åtkomst och tabellen.</span><span class="sxs-lookup"><span data-stu-id="232e4-120">Storage Explorer was incorrectly generating time stamps for shared access signatures and table queries.</span></span>


<span data-ttu-id="232e4-121">Kända problem</span><span class="sxs-lookup"><span data-stu-id="232e4-121">Known Issues</span></span>
* <span data-ttu-id="232e4-122">Med hjälp av ett namn och nyckel anslutningssträngen fungerar inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="232e4-122">Using a name and key connection string does not currently work.</span></span> <span data-ttu-id="232e4-123">Den kommer att åtgärdas i nästa version.</span><span class="sxs-lookup"><span data-stu-id="232e4-123">It will be fixed in the next release.</span></span> <span data-ttu-id="232e4-124">Fram till dess kan du använda bifoga med namn och nyckel.</span><span class="sxs-lookup"><span data-stu-id="232e4-124">Until then you can use attach with name and key.</span></span>
* <span data-ttu-id="232e4-125">Om du försöker öppna en fil med ett ogiltigt Windows-filnamn resulterar nedladdningen i en fil hittades inte.</span><span class="sxs-lookup"><span data-stu-id="232e4-125">If you try to open a file with an invalid Windows file name, the download will result in a file not found error.</span></span>
* <span data-ttu-id="232e4-126">När du klickar på ”Avbryt” för en uppgift, kan det ta ett tag att avbryta aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="232e4-126">After clicking "Cancel" on a task, it may take a while for that task to cancel.</span></span> <span data-ttu-id="232e4-127">Detta är en begränsning i biblioteket för Azure Storage-nod.</span><span class="sxs-lookup"><span data-stu-id="232e4-127">This is a limitation of the Azure Storage Node library.</span></span>
* <span data-ttu-id="232e4-128">Fliken som initierade överföringen uppdateras när du har slutfört en blob-överföring.</span><span class="sxs-lookup"><span data-stu-id="232e4-128">After completing a blob upload, the tab which initiated the upload is refreshed.</span></span> <span data-ttu-id="232e4-129">Detta är en förändring från tidigare beteende och medför även du tas tillbaka till roten i behållaren.</span><span class="sxs-lookup"><span data-stu-id="232e4-129">This is a change from previous behavior, and will also cause you to be taken back to the root of the container you are in.</span></span>
* <span data-ttu-id="232e4-130">Om du väljer fel PIN-kod/smartkort-certifikat måste startas om för att få Lagringsutforskaren glömmer detta beslut.</span><span class="sxs-lookup"><span data-stu-id="232e4-130">If you choose the wrong PIN/Smartcard certificate, then you will need to restart in order to have Storage Explorer forget that decision.</span></span>
* <span data-ttu-id="232e4-131">Panelen konto inställningar kan indikera att du måste ange autentiseringsuppgifter för att filtrera prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="232e4-131">The account settings panel may show that you need to reenter credentials to filter subscriptions.</span></span>
* <span data-ttu-id="232e4-132">Byta namn på blobbar (individuellt eller i en bytt namn till blob-behållare) bevaras inte ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="232e4-132">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="232e4-133">Alla andra egenskaper och metadata för blobbar, filer och entiteter bevaras vid ett byte av namn.</span><span class="sxs-lookup"><span data-stu-id="232e4-133">All other properties and metadata for blobs, files and entities are preserved during a rename.</span></span>
* <span data-ttu-id="232e4-134">Även om Azure-stacken inte stöder filresurser, visas en filresurser nod fortfarande under ett bifogade Stack för Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="232e4-134">Although Azure Stack doesn't currently support Files Shares, a File Shares node still appears under an attached Azure Stack storage account.</span></span>
* <span data-ttu-id="232e4-135">Du behöver kontrollera GCC är uppdaterad – kan du göra det genom att köra följande kommandon och sedan starta om datorn för användare på Ubuntu 14.04:</span><span class="sxs-lookup"><span data-stu-id="232e4-135">For users on Ubuntu 14.04, you will need to ensure GCC is up to date - this can be done by running the following commands, and then restarting your machine:</span></span>

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* <span data-ttu-id="232e4-136">Du måste installera GConf – kan du göra det genom att köra följande kommandon och sedan starta om datorn för användare på Ubuntu nr 17.04 från:</span><span class="sxs-lookup"><span data-stu-id="232e4-136">For users on Ubuntu 17.04, you will need to install GConf - this can be done by running the following commands, and then restarting your machine:</span></span>

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-0814-preview"></a><span data-ttu-id="232e4-137">Version 0.8.14 (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="232e4-137">Version 0.8.14 (Preview)</span></span>
<span data-ttu-id="232e4-138">06/22/2017</span><span class="sxs-lookup"><span data-stu-id="232e4-138">06/22/2017</span></span>

### <a name="download-azure-storage-explorer-0814-preview"></a><span data-ttu-id="232e4-139">Hämta Azure Lagringsutforskaren 0.8.14 (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="232e4-139">Download Azure Storage Explorer 0.8.14 (Preview)</span></span>
* [<span data-ttu-id="232e4-140">Hämta Azure Lagringsutforskaren 0.8.14 (förhandsversion) för Windows</span><span class="sxs-lookup"><span data-stu-id="232e4-140">Download Azure Storage Explorer 0.8.14 (Preview) for Windows</span></span>](https://go.microsoft.com/fwlink/?LinkId=809306)
* [<span data-ttu-id="232e4-141">Hämta Azure Lagringsutforskaren (förhandsversion) för 0.8.14 för Mac</span><span class="sxs-lookup"><span data-stu-id="232e4-141">Download Azure Storage Explorer 0.8.14 (Preview) for Mac</span></span>](https://go.microsoft.com/fwlink/?LinkId=809307)
* [<span data-ttu-id="232e4-142">Hämta Azure Lagringsutforskaren (förhandsversion) för 0.8.14 för Linux</span><span class="sxs-lookup"><span data-stu-id="232e4-142">Download Azure Storage Explorer 0.8.14 (Preview) for Linux</span></span>](https://go.microsoft.com/fwlink/?LinkId=809308)

### <a name="new"></a><span data-ttu-id="232e4-143">Ny</span><span class="sxs-lookup"><span data-stu-id="232e4-143">New</span></span>

* <span data-ttu-id="232e4-144">Uppdaterade Electron versionen till 1.7.2 för att kunna dra nytta av flera viktiga säkerhetsuppdateringar</span><span class="sxs-lookup"><span data-stu-id="232e4-144">Updated Electron version to 1.7.2 in order to take advantage of several critical security updates</span></span>
* <span data-ttu-id="232e4-145">Du kan nu snabbt komma åt online felsökningsguiden från Hjälp-menyn</span><span class="sxs-lookup"><span data-stu-id="232e4-145">You can now quickly access the online troubleshooting guide from the help menu</span></span>
* <span data-ttu-id="232e4-146">Lagringsutforskaren felsökning [Guide][2]</span><span class="sxs-lookup"><span data-stu-id="232e4-146">Storage Explorer Troubleshooting [Guide][2]</span></span>
* <span data-ttu-id="232e4-147">[Instruktioner] [ 3] om hur du ansluter till en Azure-Stack-prenumeration</span><span class="sxs-lookup"><span data-stu-id="232e4-147">[Instructions][3] on connecting to an Azure Stack subscription</span></span>

### <a name="known-issues"></a><span data-ttu-id="232e4-148">Kända problem</span><span class="sxs-lookup"><span data-stu-id="232e4-148">Known Issues</span></span>

* <span data-ttu-id="232e4-149">Knapparna i bekräftelsedialogrutan ta bort mappen registrera inte med musklickningar på Linux.</span><span class="sxs-lookup"><span data-stu-id="232e4-149">Buttons on the delete folder confirmation dialog don't register with the mouse clicks on Linux.</span></span> <span data-ttu-id="232e4-150">Lösning är att använda på RETUR</span><span class="sxs-lookup"><span data-stu-id="232e4-150">Workaround is to use the Enter key</span></span>
* <span data-ttu-id="232e4-151">Om du väljer fel PIN-kod/smartkort-certifikat måste startas om för att få Lagringsutforskaren glömmer beslutet</span><span class="sxs-lookup"><span data-stu-id="232e4-151">If you choose the wrong PIN/Smartcard certificate then you will need to restart in order to have Storage Explorer forget the decision</span></span>
* <span data-ttu-id="232e4-152">Med mer än 3 grupper av blobbar eller filer som överför samtidigt kan orsaka fel</span><span class="sxs-lookup"><span data-stu-id="232e4-152">Having more than 3 groups of blobs or files uploading at the same time may cause errors</span></span>
* <span data-ttu-id="232e4-153">Panelen konto inställningar kan indikera att du måste ange autentiseringsuppgifter för att filtrera prenumerationer</span><span class="sxs-lookup"><span data-stu-id="232e4-153">The account settings panel may show that you need to reenter credentials in order to filter subscriptions</span></span>
* <span data-ttu-id="232e4-154">Byta namn på blobbar (individuellt eller i en bytt namn till blob-behållare) bevaras inte ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="232e4-154">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="232e4-155">Alla andra egenskaper och metadata för blobbar, filer och entiteter bevaras vid ett byte av namn.</span><span class="sxs-lookup"><span data-stu-id="232e4-155">All other properties and metadata for blobs, files and entities are preserved during a rename.</span></span>
* <span data-ttu-id="232e4-156">Även om Azure-stacken inte stöder filresurser, visas en filresurser nod fortfarande under ett bifogade Stack för Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="232e4-156">Although Azure Stack doesn't currently support File Shares, a File Shares node still appears under an attached Azure Stack storage account.</span></span> 
* <span data-ttu-id="232e4-157">Ubuntu 14.04 installera måste gcc version uppdateras eller uppgraderas – steg för att uppgradera nedan:</span><span class="sxs-lookup"><span data-stu-id="232e4-157">Ubuntu 14.04 install needs gcc version updated or upgraded – steps to upgrade are below:</span></span>

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```




## <a name="previous-releases"></a><span data-ttu-id="232e4-158">Tidigare versioner</span><span class="sxs-lookup"><span data-stu-id="232e4-158">Previous releases</span></span>

* [<span data-ttu-id="232e4-159">Version 0.8.13</span><span class="sxs-lookup"><span data-stu-id="232e4-159">Version 0.8.13</span></span>](#version-0813)
* [<span data-ttu-id="232e4-160">Version 0.8.12 / 0.8.11 / 0.8.10</span><span class="sxs-lookup"><span data-stu-id="232e4-160">Version 0.8.12 / 0.8.11 / 0.8.10</span></span>](#version-0812--0811--0810)
* [<span data-ttu-id="232e4-161">Version 0.8.9 / 0.8.8</span><span class="sxs-lookup"><span data-stu-id="232e4-161">Version 0.8.9 / 0.8.8</span></span>](#version-089--088)
* [<span data-ttu-id="232e4-162">Version 0.8.7</span><span class="sxs-lookup"><span data-stu-id="232e4-162">Version 0.8.7</span></span>](#version-087)
* [<span data-ttu-id="232e4-163">Version 0.8.6</span><span class="sxs-lookup"><span data-stu-id="232e4-163">Version 0.8.6</span></span>](#version-086)
* [<span data-ttu-id="232e4-164">Version 0.8.5</span><span class="sxs-lookup"><span data-stu-id="232e4-164">Version 0.8.5</span></span>](#version-085)
* [<span data-ttu-id="232e4-165">Version 0.8.4</span><span class="sxs-lookup"><span data-stu-id="232e4-165">Version 0.8.4</span></span>](#version-084)
* [<span data-ttu-id="232e4-166">Version 0.8.3</span><span class="sxs-lookup"><span data-stu-id="232e4-166">Version 0.8.3</span></span>](#version-083)
* [<span data-ttu-id="232e4-167">Version 0.8.2</span><span class="sxs-lookup"><span data-stu-id="232e4-167">Version 0.8.2</span></span>](#version-082)
* [<span data-ttu-id="232e4-168">Version 0.8.0</span><span class="sxs-lookup"><span data-stu-id="232e4-168">Version 0.8.0</span></span>](#version-080)
* [<span data-ttu-id="232e4-169">Version 0.7.20160509.0</span><span class="sxs-lookup"><span data-stu-id="232e4-169">Version 0.7.20160509.0</span></span>](#version-07201605090)
* [<span data-ttu-id="232e4-170">Version 0.7.20160325.0</span><span class="sxs-lookup"><span data-stu-id="232e4-170">Version 0.7.20160325.0</span></span>](#version-07201603250)
* [<span data-ttu-id="232e4-171">Version 0.7.20160129.1</span><span class="sxs-lookup"><span data-stu-id="232e4-171">Version 0.7.20160129.1</span></span>](#version-07201601291)
* [<span data-ttu-id="232e4-172">Version 0.7.20160105.0</span><span class="sxs-lookup"><span data-stu-id="232e4-172">Version 0.7.20160105.0</span></span>](#version-07201601050)
* [<span data-ttu-id="232e4-173">Version 0.7.20151116.0</span><span class="sxs-lookup"><span data-stu-id="232e4-173">Version 0.7.20151116.0</span></span>](#version-07201511160)


### <a name="version-0813"></a><span data-ttu-id="232e4-174">Version 0.8.13</span><span class="sxs-lookup"><span data-stu-id="232e4-174">Version 0.8.13</span></span>
<span data-ttu-id="232e4-175">05/12/2017</span><span class="sxs-lookup"><span data-stu-id="232e4-175">05/12/2017</span></span>

#### <a name="new"></a><span data-ttu-id="232e4-176">Ny</span><span class="sxs-lookup"><span data-stu-id="232e4-176">New</span></span>

* <span data-ttu-id="232e4-177">Lagringsutforskaren felsökning [Guide][2]</span><span class="sxs-lookup"><span data-stu-id="232e4-177">Storage Explorer Troubleshooting [Guide][2]</span></span>
* <span data-ttu-id="232e4-178">[Instruktioner] [ 3] om hur du ansluter till en Azure-Stack-prenumeration</span><span class="sxs-lookup"><span data-stu-id="232e4-178">[Instructions][3] on connecting to an Azure Stack subscription</span></span>

#### <a name="fixes"></a><span data-ttu-id="232e4-179">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="232e4-179">Fixes</span></span>

* <span data-ttu-id="232e4-180">Fast: Ladda upp filen hunnit hög orsaka en out-of-minnesfel</span><span class="sxs-lookup"><span data-stu-id="232e4-180">Fixed: File upload had a high chance of causing an out of memory error</span></span>
* <span data-ttu-id="232e4-181">Fast: Du kan nu logga in med PIN-kod/smartkort</span><span class="sxs-lookup"><span data-stu-id="232e4-181">Fixed: You can now sign in with PIN/Smartcard</span></span>
* <span data-ttu-id="232e4-182">Fast: Öppna i portalen nu fungerar med Azure Kina, Tyskland Azure, Azure som tillhör amerikanska myndigheter och Azure-stacken</span><span class="sxs-lookup"><span data-stu-id="232e4-182">Fixed: Open in Portal now works with Azure China, Azure Germany, Azure US Government, and Azure Stack</span></span>
* <span data-ttu-id="232e4-183">Fast: Går inte att överföra en mapp till en blobbbehållare, ”otillåten åtgärd” skulle ibland uppstå fel</span><span class="sxs-lookup"><span data-stu-id="232e4-183">Fixed: While uploading a folder to a blob container, an "Illegal operation" error would sometimes occur</span></span>
* <span data-ttu-id="232e4-184">Fast: Markera alla inaktiverades vid hantering av ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="232e4-184">Fixed: Select all was disabled while managing snapshots</span></span>
* <span data-ttu-id="232e4-185">Fast: Metadata för grundläggande blob kan komma att skrivas över när du visar egenskaperna för dess ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="232e4-185">Fixed: The metadata of the base blob might get overwritten after viewing the properties of its snapshots</span></span>

#### <a name="known-issues"></a><span data-ttu-id="232e4-186">Kända problem</span><span class="sxs-lookup"><span data-stu-id="232e4-186">Known Issues</span></span>

* <span data-ttu-id="232e4-187">Om du väljer fel PIN-kod/smartkort-certifikat måste startas om för att få Lagringsutforskaren glömmer beslutet</span><span class="sxs-lookup"><span data-stu-id="232e4-187">If you choose the wrong PIN/Smartcard certificate then you will need to restart in order to have Storage Explorer forget the decision</span></span>
* <span data-ttu-id="232e4-188">När du har zoomat in eller ut kan zoomnivån tillfälligt återställas till Standardnivå</span><span class="sxs-lookup"><span data-stu-id="232e4-188">While zoomed in or out, the zoom level may momentarily reset to the default level</span></span>
* <span data-ttu-id="232e4-189">Med mer än 3 grupper av blobbar eller filer som överför samtidigt kan orsaka fel</span><span class="sxs-lookup"><span data-stu-id="232e4-189">Having more than 3 groups of blobs or files uploading at the same time may cause errors</span></span>
* <span data-ttu-id="232e4-190">Panelen konto inställningar kan indikera att du måste ange autentiseringsuppgifter för att filtrera prenumerationer</span><span class="sxs-lookup"><span data-stu-id="232e4-190">The account settings panel may show that you need to reenter credentials in order to filter subscriptions</span></span>
* <span data-ttu-id="232e4-191">Byta namn på blobbar (individuellt eller i en bytt namn till blob-behållare) bevaras inte ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="232e4-191">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="232e4-192">Alla andra egenskaper och metadata för blobbar, filer och entiteter bevaras vid ett byte av namn.</span><span class="sxs-lookup"><span data-stu-id="232e4-192">All other properties and metadata for blobs, files and entities are preserved during a rename.</span></span>
* <span data-ttu-id="232e4-193">Även om Azure-stacken inte stöder filresurser, visas en filresurser nod fortfarande under ett bifogade Stack för Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="232e4-193">Although Azure Stack doesn't currently support Files Shares, a File Shares node still appears under an attached Azure Stack storage account.</span></span> 
* <span data-ttu-id="232e4-194">Ubuntu 14.04 installera måste gcc version uppdateras eller uppgraderas – steg för att uppgradera nedan:</span><span class="sxs-lookup"><span data-stu-id="232e4-194">Ubuntu 14.04 install needs gcc version updated or upgraded – steps to upgrade are below:</span></span>

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-0812--0811--0810"></a><span data-ttu-id="232e4-195">Version 0.8.12 / 0.8.11 / 0.8.10</span><span class="sxs-lookup"><span data-stu-id="232e4-195">Version 0.8.12 / 0.8.11 / 0.8.10</span></span>
<span data-ttu-id="232e4-196">04/07/2017</span><span class="sxs-lookup"><span data-stu-id="232e4-196">04/07/2017</span></span>

#### <a name="new"></a><span data-ttu-id="232e4-197">Ny</span><span class="sxs-lookup"><span data-stu-id="232e4-197">New</span></span>

* <span data-ttu-id="232e4-198">Lagringsutforskaren stängs nu automatiskt när du installerar en uppdatering från meddelanden om uppdateringar</span><span class="sxs-lookup"><span data-stu-id="232e4-198">Storage Explorer will now automatically close when you install an update from the update notification</span></span>
* <span data-ttu-id="232e4-199">Lokalt Snabbåtkomst ger en förbättrad upplevelse för att arbeta med resurserna används ofta</span><span class="sxs-lookup"><span data-stu-id="232e4-199">In-place quick access provides an enhanced experience for working with your frequently accessed resources</span></span>
* <span data-ttu-id="232e4-200">I Redigeraren för Blob-behållare kan du nu se vilken virtuell dator som tillhör en lånade blob</span><span class="sxs-lookup"><span data-stu-id="232e4-200">In the Blob Container editor, you can now see which virtual machine a leased blob belongs to</span></span>
* <span data-ttu-id="232e4-201">Du kan nu komprimera panelen vänster</span><span class="sxs-lookup"><span data-stu-id="232e4-201">You can now collapse the left side panel</span></span>
* <span data-ttu-id="232e4-202">Identifiering nu körs samtidigt som nedladdning</span><span class="sxs-lookup"><span data-stu-id="232e4-202">Discovery now runs at the same time as download</span></span>
* <span data-ttu-id="232e4-203">Använda statistik i Blob-behållaren och filresursen tabell redigerare för att se storleken på ditt val eller resurs</span><span class="sxs-lookup"><span data-stu-id="232e4-203">Use Statistics in the Blob Container, File Share, and Table editors to see the size of your resource or selection</span></span>
* <span data-ttu-id="232e4-204">Du kan nu logga in till Azure Active Directory (AAD) baserad Azure Stack-konton.</span><span class="sxs-lookup"><span data-stu-id="232e4-204">You can now sign-in to Azure Active Directory (AAD) based Azure Stack accounts.</span></span> 
* <span data-ttu-id="232e4-205">Du kan överföra arkivfiler över 32MB till Premium storage-konton</span><span class="sxs-lookup"><span data-stu-id="232e4-205">You can now upload archive files over 32MB to Premium storage accounts</span></span>
* <span data-ttu-id="232e4-206">Förbättrat stöd för hjälpmedel</span><span class="sxs-lookup"><span data-stu-id="232e4-206">Improved accessibility support</span></span>
* <span data-ttu-id="232e4-207">Nu kan du lägga till betrodda Base64-kodat X.509-SSL-certifikat genom att gå till redigera -&gt; SSL-certifikat -&gt; Importera certifikat</span><span class="sxs-lookup"><span data-stu-id="232e4-207">You can now add trusted Base-64 encoded X.509 SSL certificates by going to Edit -&gt; SSL Certificates -&gt; Import Certificates</span></span>

#### <a name="fixes"></a><span data-ttu-id="232e4-208">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="232e4-208">Fixes</span></span>

* <span data-ttu-id="232e4-209">Fast: när du har uppdaterat en kontoautentiseringsuppgifter trädvyn kan ibland uppdateras inte automatiskt</span><span class="sxs-lookup"><span data-stu-id="232e4-209">Fixed: after refreshing an account's credentials, the tree view would sometimes not automatically refresh</span></span>
* <span data-ttu-id="232e4-210">Fast: Generera en SAS för emulatorn köer och tabeller skulle leda till en ogiltig URL</span><span class="sxs-lookup"><span data-stu-id="232e4-210">Fixed: generating a SAS for emulator queues and tables would result in an invalid URL</span></span>
* <span data-ttu-id="232e4-211">Fast: premium storage-konton kan nu utökas när en proxy är aktiverat</span><span class="sxs-lookup"><span data-stu-id="232e4-211">Fixed: premium storage accounts can now be expanded while a proxy is enabled</span></span>
* <span data-ttu-id="232e4-212">Fast: Verkställ-knappen på sidan för hantering av konton fungerar inte om du hade 1 eller 0 konton som har valts</span><span class="sxs-lookup"><span data-stu-id="232e4-212">Fixed: the apply button on the accounts management page would not work if you had 1 or 0 accounts selected</span></span>
* <span data-ttu-id="232e4-213">Fast: överföring av blobbar som kräver konfliktlösningar kan misslyckas - fast i 0.8.11</span><span class="sxs-lookup"><span data-stu-id="232e4-213">Fixed: uploading blobs that require conflict resolutions may fail - fixed in 0.8.11</span></span> 
* <span data-ttu-id="232e4-214">Fast: skicka feedback har brutits i 0.8.11 - fast i 0.8.12</span><span class="sxs-lookup"><span data-stu-id="232e4-214">Fixed: sending feedback was broken in 0.8.11 - fixed in 0.8.12</span></span> 

#### <a name="known-issues"></a><span data-ttu-id="232e4-215">Kända problem</span><span class="sxs-lookup"><span data-stu-id="232e4-215">Known Issues</span></span>

* <span data-ttu-id="232e4-216">Efter uppgraderingen till 0.8.10, behöver du uppdatera alla dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="232e4-216">After upgrading to 0.8.10, you will need to refresh all of your credentials.</span></span>
* <span data-ttu-id="232e4-217">När du har zoomat in eller ut kan tillfälligt zoomnivån återställa till Standardnivå.</span><span class="sxs-lookup"><span data-stu-id="232e4-217">While zoomed in or out, the zoom level may momentarily reset to the default level.</span></span>
* <span data-ttu-id="232e4-218">Med mer än 3 grupper av blobbar eller filer som överför samtidigt kan orsaka fel.</span><span class="sxs-lookup"><span data-stu-id="232e4-218">Having more than 3 groups of blobs or files uploading at the same time may cause errors.</span></span>
* <span data-ttu-id="232e4-219">Panelen konto inställningar kan indikera att du måste ange autentiseringsuppgifter för att filtrera prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="232e4-219">The account settings panel may show that you need to reenter credentials in order to filter subscriptions.</span></span>
* <span data-ttu-id="232e4-220">Byta namn på blobbar (individuellt eller i en bytt namn till blob-behållare) bevaras inte ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="232e4-220">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="232e4-221">Alla andra egenskaper och metadata för blobbar, filer och entiteter bevaras vid ett byte av namn.</span><span class="sxs-lookup"><span data-stu-id="232e4-221">All other properties and metadata for blobs, files and entities are preserved during a rename.</span></span>
* <span data-ttu-id="232e4-222">Även om Azure-stacken inte stöder filresurser, visas en filresurser nod fortfarande under ett bifogade Stack för Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="232e4-222">Although Azure Stack doesn't currently support Files Shares, a File Shares node still appears under an attached Azure Stack storage account.</span></span> 
* <span data-ttu-id="232e4-223">Ubuntu 14.04 installera måste gcc version uppdateras eller uppgraderas – steg för att uppgradera nedan:</span><span class="sxs-lookup"><span data-stu-id="232e4-223">Ubuntu 14.04 install needs gcc version updated or upgraded – steps to upgrade are below:</span></span>

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-089--088"></a><span data-ttu-id="232e4-224">Version 0.8.9 / 0.8.8</span><span class="sxs-lookup"><span data-stu-id="232e4-224">Version 0.8.9 / 0.8.8</span></span>
<span data-ttu-id="232e4-225">02/23/2017</span><span class="sxs-lookup"><span data-stu-id="232e4-225">02/23/2017</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/R6gonK3cYAc?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/SrRPCm94mfE?ecver=1" frameborder="0" allowfullscreen></iframe>


#### <a name="new"></a><span data-ttu-id="232e4-226">Ny</span><span class="sxs-lookup"><span data-stu-id="232e4-226">New</span></span>

* <span data-ttu-id="232e4-227">Lagringsutforskaren 0.8.9 kommer automatiskt att hämta den senaste versionen för uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="232e4-227">Storage Explorer 0.8.9 will automatically download the latest version for updates.</span></span>
* <span data-ttu-id="232e4-228">Snabbkorrigering: med hjälp av en portal resulterar genererade SAS-URI att koppla ett lagringskonto i ett fel.</span><span class="sxs-lookup"><span data-stu-id="232e4-228">Hotfix: using a portal generated SAS URI to attach a storage account would result in an error.</span></span>
* <span data-ttu-id="232e4-229">Du kan nu skapa, hantera och främja blob ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="232e4-229">You can now create, manage, and promote blob snapshots.</span></span>
* <span data-ttu-id="232e4-230">Du kan nu logga in på Azure Kina, Azure Tyskland och Azure som tillhör amerikanska myndigheter konton.</span><span class="sxs-lookup"><span data-stu-id="232e4-230">You can now sign in to Azure China, Azure Germany, and Azure US Government accounts.</span></span>
* <span data-ttu-id="232e4-231">Du kan nu ändra zoomnivån.</span><span class="sxs-lookup"><span data-stu-id="232e4-231">You can now change the zoom level.</span></span> <span data-ttu-id="232e4-232">Använd alternativen i menyn Visa för att Zooma In, Zooma ut och Återställ Zoom.</span><span class="sxs-lookup"><span data-stu-id="232e4-232">Use the options in the View menu to Zoom In, Zoom Out, and Reset Zoom.</span></span>
* <span data-ttu-id="232e4-233">Unicode-tecken stöds nu i Användarmetadata för blobbar och filer.</span><span class="sxs-lookup"><span data-stu-id="232e4-233">Unicode characters are now supported in user metadata for blobs and files.</span></span>
* <span data-ttu-id="232e4-234">Hjälpmedelsförbättringar.</span><span class="sxs-lookup"><span data-stu-id="232e4-234">Accessibility improvements.</span></span>
* <span data-ttu-id="232e4-235">I nästa version viktig information kan visas från meddelanden om uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="232e4-235">The next version's release notes can be viewed from the update notification.</span></span> <span data-ttu-id="232e4-236">Du kan också visa aktuella viktig information från Hjälp-menyn.</span><span class="sxs-lookup"><span data-stu-id="232e4-236">You can also view the current release notes from the Help menu.</span></span>

#### <a name="fixes"></a><span data-ttu-id="232e4-237">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="232e4-237">Fixes</span></span>

* <span data-ttu-id="232e4-238">Fast: versionsnumret nu visas korrekt på Kontrollpanelen i Windows</span><span class="sxs-lookup"><span data-stu-id="232e4-238">Fixed: the version number is now correctly displayed in Control Panel on Windows</span></span>
* <span data-ttu-id="232e4-239">Fast: sökning är inte längre begränsad till 50 000 noder</span><span class="sxs-lookup"><span data-stu-id="232e4-239">Fixed: search is no longer limited to 50,000 nodes</span></span>
* <span data-ttu-id="232e4-240">Fast: överföra till en filresurs som alltid roterade målkatalogen inte redan finns</span><span class="sxs-lookup"><span data-stu-id="232e4-240">Fixed: upload to a file share spun forever if the destination directory did not already exist</span></span>
* <span data-ttu-id="232e4-241">Fast: förbättrad stabilitet för lång överföring</span><span class="sxs-lookup"><span data-stu-id="232e4-241">Fixed: improved stability for long uploads and downloads</span></span>

#### <a name="known-issues"></a><span data-ttu-id="232e4-242">Kända problem</span><span class="sxs-lookup"><span data-stu-id="232e4-242">Known Issues</span></span>

* <span data-ttu-id="232e4-243">När du har zoomat in eller ut kan tillfälligt zoomnivån återställa till Standardnivå.</span><span class="sxs-lookup"><span data-stu-id="232e4-243">While zoomed in or out, the zoom level may momentarily reset to the default level.</span></span>
* <span data-ttu-id="232e4-244">Snabbåtkomst fungerar bara med prenumerationen baserat objekt.</span><span class="sxs-lookup"><span data-stu-id="232e4-244">Quick Access only works with subscription based items.</span></span> <span data-ttu-id="232e4-245">Lokala resurser eller resurser som är anslutna via en nyckel eller SAS-token stöds inte i den här versionen.</span><span class="sxs-lookup"><span data-stu-id="232e4-245">Local resources or resources attached via key or SAS token are not supported in this release.</span></span>
* <span data-ttu-id="232e4-246">Snabbåtkomst kan det ta ett par sekunder att navigera till målresursen, beroende på hur många resurser som du har.</span><span class="sxs-lookup"><span data-stu-id="232e4-246">It may take Quick Access a few seconds to navigate to the target resource, depending on how many resources you have.</span></span>
* <span data-ttu-id="232e4-247">Med mer än 3 grupper av blobbar eller filer som överför samtidigt kan orsaka fel.</span><span class="sxs-lookup"><span data-stu-id="232e4-247">Having more than 3 groups of blobs or files uploading at the same time may cause errors.</span></span>

<span data-ttu-id="232e4-248">12/16/2016</span><span class="sxs-lookup"><span data-stu-id="232e4-248">12/16/2016</span></span>
### <a name="version-087"></a><span data-ttu-id="232e4-249">Version 0.8.7</span><span class="sxs-lookup"><span data-stu-id="232e4-249">Version 0.8.7</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/Me4Y4jxoer8?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="232e4-250">Ny</span><span class="sxs-lookup"><span data-stu-id="232e4-250">New</span></span>

* <span data-ttu-id="232e4-251">Du kan välja hur du löser konflikter i början av en uppdatering, hämta eller kopiera session i fönstret aktiviteter</span><span class="sxs-lookup"><span data-stu-id="232e4-251">You can choose how to resolve conflicts at the beginning of an update, download or copy session in the Activities window</span></span>
* <span data-ttu-id="232e4-252">Hovra över en flik om du vill se den fullständiga sökvägen till resursen lagring</span><span class="sxs-lookup"><span data-stu-id="232e4-252">Hover over a tab to see the full path of the storage resource</span></span>
* <span data-ttu-id="232e4-253">När du klickar på en flik, synkronisering och dess plats i navigeringsfönstret vänster</span><span class="sxs-lookup"><span data-stu-id="232e4-253">When you click on a tab, it synchronizes with its location in the left side navigation pane</span></span>

#### <a name="fixes"></a><span data-ttu-id="232e4-254">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="232e4-254">Fixes</span></span>

* <span data-ttu-id="232e4-255">Fast: Lagringsutforskaren är nu ett betrott app på Mac</span><span class="sxs-lookup"><span data-stu-id="232e4-255">Fixed: Storage Explorer is now a trusted app on Mac</span></span>
* <span data-ttu-id="232e4-256">Fast: Ubuntu 14.04 igen stöds</span><span class="sxs-lookup"><span data-stu-id="232e4-256">Fixed: Ubuntu 14.04 is again supported</span></span>
* <span data-ttu-id="232e4-257">Fast: Ibland Lägg till konto UI blinkar när inläsning av prenumerationer</span><span class="sxs-lookup"><span data-stu-id="232e4-257">Fixed: Sometimes the add account UI flashes when loading subscriptions</span></span>
* <span data-ttu-id="232e4-258">Fast: Ibland inte alla lagringsresurser ingick i navigeringsfönstret vänster</span><span class="sxs-lookup"><span data-stu-id="232e4-258">Fixed: Sometimes not all storage resources were listed in the left side navigation pane</span></span>
* <span data-ttu-id="232e4-259">Fast: Åtgärdsfönstret ibland visas tom åtgärder</span><span class="sxs-lookup"><span data-stu-id="232e4-259">Fixed: The action pane sometimes displayed empty actions</span></span>
* <span data-ttu-id="232e4-260">Fast: Fönsterstorlek från den senaste stängd sessionen nu sparas</span><span class="sxs-lookup"><span data-stu-id="232e4-260">Fixed: The window size from the last closed session is now retained</span></span>
* <span data-ttu-id="232e4-261">Fast: Du kan öppna flera flikar för samma resurs med hjälp av snabbmenyn</span><span class="sxs-lookup"><span data-stu-id="232e4-261">Fixed: You can open multiple tabs for the same resource using the context menu</span></span>

#### <a name="known-issues"></a><span data-ttu-id="232e4-262">Kända problem</span><span class="sxs-lookup"><span data-stu-id="232e4-262">Known Issues</span></span>

* <span data-ttu-id="232e4-263">Snabbåtkomst fungerar bara med prenumerationen baserat objekt.</span><span class="sxs-lookup"><span data-stu-id="232e4-263">Quick Access only works with subscription based items.</span></span> <span data-ttu-id="232e4-264">Lokala resurser eller resurser som är anslutna via en nyckel eller SAS-token stöds inte i den här versionen</span><span class="sxs-lookup"><span data-stu-id="232e4-264">Local resources or resources attached via key or SAS token are not supported in this release</span></span>
* <span data-ttu-id="232e4-265">Det kan ta Snabbåtkomst en några sekunder att navigera till målresursen, beroende på hur många resurser som du har</span><span class="sxs-lookup"><span data-stu-id="232e4-265">It may take Quick Access a few seconds to navigate to the target resource, depending on how many resources you have</span></span>
* <span data-ttu-id="232e4-266">Med mer än 3 grupper av blobbar eller filer som överför samtidigt kan orsaka fel</span><span class="sxs-lookup"><span data-stu-id="232e4-266">Having more than 3 groups of blobs or files uploading at the same time may cause errors</span></span>
* <span data-ttu-id="232e4-267">Referenser för sökning efter det att söka i ungefär 50 000 noder - prestanda kan påverkas eller kan orsaka undantag</span><span class="sxs-lookup"><span data-stu-id="232e4-267">Search handles searching across roughly 50,000 nodes - after this, performance may be impacted or may cause unhandled exception</span></span>
* <span data-ttu-id="232e4-268">Du kan se flera prompter som ber om användarens tillstånd att komma åt nyckelringar för första gången med Lagringsutforskaren på macOS.</span><span class="sxs-lookup"><span data-stu-id="232e4-268">For the first time using the Storage Explorer on macOS, you might see multiple prompts asking for user's permission to access keychain.</span></span> <span data-ttu-id="232e4-269">Vi rekommenderar att du väljer Tillåt alltid så uppmaningen inte visas igen</span><span class="sxs-lookup"><span data-stu-id="232e4-269">We suggest you select Always Allow so the prompt won't show up again</span></span>

<span data-ttu-id="232e4-270">11/18/2016</span><span class="sxs-lookup"><span data-stu-id="232e4-270">11/18/2016</span></span>
### <a name="version-086"></a><span data-ttu-id="232e4-271">Version 0.8.6</span><span class="sxs-lookup"><span data-stu-id="232e4-271">Version 0.8.6</span></span>

#### <a name="new"></a><span data-ttu-id="232e4-272">Ny</span><span class="sxs-lookup"><span data-stu-id="232e4-272">New</span></span>

* <span data-ttu-id="232e4-273">Du kan nu PIN-kod används oftast Snabbåtkomst-tjänster för enkel navigering</span><span class="sxs-lookup"><span data-stu-id="232e4-273">You can now pin most frequently used services to the Quick Access for easy navigation</span></span>
* <span data-ttu-id="232e4-274">Nu kan du öppna flera redigerare på olika flikar.</span><span class="sxs-lookup"><span data-stu-id="232e4-274">You can now open multiple editors in different tabs.</span></span> <span data-ttu-id="232e4-275">Enskild klickar för att öppna en tillfällig flik; Dubbelklicka om du vill öppna en permanent flik. Du kan också klicka på fliken tillfälligt så att den permanenta fliken</span><span class="sxs-lookup"><span data-stu-id="232e4-275">Single click to open a temporary tab; double click to open a permanent tab. You can also click on the temporary tab to make it a permanent tab</span></span>
* <span data-ttu-id="232e4-276">Vi har gjort märkbar prestanda och stabilitetsförbättringar för överföring och hämtning, särskilt för stora filer på snabb datorer</span><span class="sxs-lookup"><span data-stu-id="232e4-276">We have made noticeable performance and stability improvements for uploads and downloads, especially for large files on fast machines</span></span>
* <span data-ttu-id="232e4-277">Nu kan skapa tomma ”virtuell” mappar i blob-behållare</span><span class="sxs-lookup"><span data-stu-id="232e4-277">Empty "virtual" folders can now be created in blob containers</span></span>
* <span data-ttu-id="232e4-278">Vi har nytt introducerades bred sökning med vår nya förbättrad sökningen så att du nu har två alternativ för att söka efter:</span><span class="sxs-lookup"><span data-stu-id="232e4-278">We have re-introduced scoped search with our new enhanced substring search, so you now have two options for searching:</span></span> 
    * <span data-ttu-id="232e4-279">Globala Sök - bara ange en sökterm i textrutan Sök</span><span class="sxs-lookup"><span data-stu-id="232e4-279">Global search - just enter a search term into the search textbox</span></span>
    * <span data-ttu-id="232e4-280">Bred sökning - Klicka på förstoringsglasikonen bredvid en nod och sedan lägga till en sökterm i slutet av sökvägen, eller högerklicka och välj ”Sök från hit”</span><span class="sxs-lookup"><span data-stu-id="232e4-280">Scoped search - click the magnifying glass icon next to a node, then add a search term to the end of the path, or right-click and select "Search from Here"</span></span>
* <span data-ttu-id="232e4-281">Vi har lagt till olika teman: ljus (standard), mörkt, hög kontrast svart och hög kontrast vit.</span><span class="sxs-lookup"><span data-stu-id="232e4-281">We have added various themes: Light (default), Dark, High Contrast Black, and High Contrast White.</span></span> <span data-ttu-id="232e4-282">Gå till redigera -&gt; teman för att ändra inställningarna teman</span><span class="sxs-lookup"><span data-stu-id="232e4-282">Go to Edit -&gt; Themes to change your theming preference</span></span>
* <span data-ttu-id="232e4-283">Du kan ändra Blob och filegenskaper</span><span class="sxs-lookup"><span data-stu-id="232e4-283">You can modify Blob and file properties</span></span>
* <span data-ttu-id="232e4-284">Vi stöder nu kodade (base64) och kodat Kömeddelanden</span><span class="sxs-lookup"><span data-stu-id="232e4-284">We now support encoded (base64) and unencoded queue messages</span></span>
* <span data-ttu-id="232e4-285">På Linux för, ett 64-bitars operativsystem är nu krävs.</span><span class="sxs-lookup"><span data-stu-id="232e4-285">On Linux, a 64-bit OS is now required.</span></span> <span data-ttu-id="232e4-286">Den här versionen stöder vi endast 64-bitars Ubuntu 16.04.1 LTS</span><span class="sxs-lookup"><span data-stu-id="232e4-286">For this release we only support 64-bit Ubuntu 16.04.1 LTS</span></span>
* <span data-ttu-id="232e4-287">Vi har uppdaterat vår logotypen!</span><span class="sxs-lookup"><span data-stu-id="232e4-287">We have updated our logo!</span></span>

#### <a name="fixes"></a><span data-ttu-id="232e4-288">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="232e4-288">Fixes</span></span>

* <span data-ttu-id="232e4-289">Fast: Skärmen låser problem</span><span class="sxs-lookup"><span data-stu-id="232e4-289">Fixed: Screen freezing problems</span></span>
* <span data-ttu-id="232e4-290">Fast: Förbättrad säkerhet</span><span class="sxs-lookup"><span data-stu-id="232e4-290">Fixed: Enhanced security</span></span>
* <span data-ttu-id="232e4-291">Fast: Ibland dubbla bifogade konton kan visas</span><span class="sxs-lookup"><span data-stu-id="232e4-291">Fixed: Sometimes duplicate attached accounts could appear</span></span>
* <span data-ttu-id="232e4-292">Fast: En blob med en odefinierad innehållstyp generera ett undantag</span><span class="sxs-lookup"><span data-stu-id="232e4-292">Fixed: A blob with an undefined content type could generate an exception</span></span>
* <span data-ttu-id="232e4-293">Fast: Öppna panelen fråga på en tom tabell var inte möjligt</span><span class="sxs-lookup"><span data-stu-id="232e4-293">Fixed: Opening the Query Panel on an empty table was not possible</span></span>
* <span data-ttu-id="232e4-294">Fast: Varierar buggar i sökningen</span><span class="sxs-lookup"><span data-stu-id="232e4-294">Fixed: Varies bugs in Search</span></span>
* <span data-ttu-id="232e4-295">Fast: Öka antalet resurser som lästs in från 50 till 100 när du klickar på ”mer belastning”</span><span class="sxs-lookup"><span data-stu-id="232e4-295">Fixed: Increased the number of resources loaded from 50 to 100 when clicking "Load More"</span></span>
* <span data-ttu-id="232e4-296">Fast: Första körningen om ett konto som har loggat in, vi nu välja alla prenumerationer för kontot som standard</span><span class="sxs-lookup"><span data-stu-id="232e4-296">Fixed: On first run, if an account is signed into, we now select all subscriptions for that account by default</span></span> 

#### <a name="known-issues"></a><span data-ttu-id="232e4-297">Kända problem</span><span class="sxs-lookup"><span data-stu-id="232e4-297">Known Issues</span></span>

* <span data-ttu-id="232e4-298">Den här versionen av Lagringsutforskaren körs inte på Ubuntu 14.04</span><span class="sxs-lookup"><span data-stu-id="232e4-298">This release of the Storage Explorer does not run on Ubuntu 14.04</span></span>
* <span data-ttu-id="232e4-299">Om du vill öppna flera flikar för samma resurs kontinuerligt Klicka inte på samma resurs.</span><span class="sxs-lookup"><span data-stu-id="232e4-299">To open multiple tabs for the same resource, do not continuously click on the same resource.</span></span> <span data-ttu-id="232e4-300">Klicka på en annan resurs och sedan gå tillbaka och klicka sedan på den ursprungliga resursen att öppna den igen i en annan flik</span><span class="sxs-lookup"><span data-stu-id="232e4-300">Click on another resource and then go back and then click on the original resource to open it again in another tab</span></span> 
* <span data-ttu-id="232e4-301">Snabbåtkomst fungerar bara med prenumerationen baserat objekt.</span><span class="sxs-lookup"><span data-stu-id="232e4-301">Quick Access only works with subscription based items.</span></span> <span data-ttu-id="232e4-302">Lokala resurser eller resurser som är anslutna via en nyckel eller SAS-token stöds inte i den här versionen</span><span class="sxs-lookup"><span data-stu-id="232e4-302">Local resources or resources attached via key or SAS token are not supported in this release</span></span>
* <span data-ttu-id="232e4-303">Det kan ta Snabbåtkomst en några sekunder att navigera till målresursen, beroende på hur många resurser som du har</span><span class="sxs-lookup"><span data-stu-id="232e4-303">It may take Quick Access a few seconds to navigate to the target resource, depending on how many resources you have</span></span>
* <span data-ttu-id="232e4-304">Med mer än 3 grupper av blobbar eller filer som överför samtidigt kan orsaka fel</span><span class="sxs-lookup"><span data-stu-id="232e4-304">Having more than 3 groups of blobs or files uploading at the same time may cause errors</span></span>
* <span data-ttu-id="232e4-305">Referenser för sökning efter det att söka i ungefär 50 000 noder - prestanda kan påverkas eller kan orsaka undantag</span><span class="sxs-lookup"><span data-stu-id="232e4-305">Search handles searching across roughly 50,000 nodes - after this, performance may be impacted or may cause unhandled exception</span></span>

<span data-ttu-id="232e4-306">10/03/2016</span><span class="sxs-lookup"><span data-stu-id="232e4-306">10/03/2016</span></span>
### <a name="version-085"></a><span data-ttu-id="232e4-307">Version 0.8.5</span><span class="sxs-lookup"><span data-stu-id="232e4-307">Version 0.8.5</span></span>

#### <a name="new"></a><span data-ttu-id="232e4-308">Ny</span><span class="sxs-lookup"><span data-stu-id="232e4-308">New</span></span>

* <span data-ttu-id="232e4-309">Kan nu använda Portal-genererade SAS nycklar för att ansluta till Lagringskonton och resurser</span><span class="sxs-lookup"><span data-stu-id="232e4-309">Can now use Portal-generated SAS keys to attach to Storage Accounts and resources</span></span>

#### <a name="fixes"></a><span data-ttu-id="232e4-310">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="232e4-310">Fixes</span></span>

* <span data-ttu-id="232e4-311">Fast: konkurrenstillstånd under sökningen ibland orsakade noder blir inte är expanderbara</span><span class="sxs-lookup"><span data-stu-id="232e4-311">Fixed: race condition during search sometimes caused nodes to become non-expandable</span></span>
* <span data-ttu-id="232e4-312">Fast: ”Använd HTTP” fungerar inte när du ansluter till Storage-konton med kontonamn och nyckel</span><span class="sxs-lookup"><span data-stu-id="232e4-312">Fixed: "Use HTTP" doesn't work when connecting to Storage Accounts with account name and key</span></span>
* <span data-ttu-id="232e4-313">Fast: SAS-nycklar (som är särskilt Portal-genererade) returnerar ett ”avslutande snedstreck” fel</span><span class="sxs-lookup"><span data-stu-id="232e4-313">Fixed: SAS keys (specially Portal-generated ones) return a "trailing slash" error</span></span>
* <span data-ttu-id="232e4-314">Fast: importera tabell problem</span><span class="sxs-lookup"><span data-stu-id="232e4-314">Fixed: table import issues</span></span>
    * <span data-ttu-id="232e4-315">Ibland har partitionsnyckel och radnyckel återförts</span><span class="sxs-lookup"><span data-stu-id="232e4-315">Sometimes partition key and row key were reversed</span></span>
    * <span data-ttu-id="232e4-316">Det gick inte att läsa ”null” partitionsnycklar</span><span class="sxs-lookup"><span data-stu-id="232e4-316">Unable to read "null" Partition Keys</span></span>

#### <a name="known-issues"></a><span data-ttu-id="232e4-317">Kända problem</span><span class="sxs-lookup"><span data-stu-id="232e4-317">Known Issues</span></span>

* <span data-ttu-id="232e4-318">Referenser för sökning efter det att söka i ungefär 50 000 noder - kan prestanda påverkas</span><span class="sxs-lookup"><span data-stu-id="232e4-318">Search handles searching across roughly 50,000 nodes - after this, performance may be impacted</span></span>
* <span data-ttu-id="232e4-319">Azure-stacken stöder inte filer, så försök att expandera filer visas ett fel</span><span class="sxs-lookup"><span data-stu-id="232e4-319">Azure Stack doesn't currently support Files, so trying to expand Files will show an error</span></span>

<span data-ttu-id="232e4-320">09/12/2016</span><span class="sxs-lookup"><span data-stu-id="232e4-320">09/12/2016</span></span>
### <a name="version-084"></a><span data-ttu-id="232e4-321">Version 0.8.4</span><span class="sxs-lookup"><span data-stu-id="232e4-321">Version 0.8.4</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/cr5tOGyGrIQ?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="232e4-322">Ny</span><span class="sxs-lookup"><span data-stu-id="232e4-322">New</span></span>

* <span data-ttu-id="232e4-323">Generera Direktlänkar till storage-konton, behållare, köer, tabeller eller filresurser för att dela och stöd för enkel åtkomst till dina resurser - Windows- och Mac OS x</span><span class="sxs-lookup"><span data-stu-id="232e4-323">Generate direct links to storage accounts, containers, queues, tables, or file shares for sharing and easy access to your resources - Windows and Mac OS support</span></span>
* <span data-ttu-id="232e4-324">Sök efter din blob-behållare, tabeller, köer, filresurser eller storage-konton från sökrutan</span><span class="sxs-lookup"><span data-stu-id="232e4-324">Search for your blob containers, tables, queues, file shares, or storage accounts from the search box</span></span>
* <span data-ttu-id="232e4-325">Nu kan du gruppera-satser i tabellen Frågebyggaren</span><span class="sxs-lookup"><span data-stu-id="232e4-325">You can now group clauses in the table query builder</span></span>
* <span data-ttu-id="232e4-326">Byt namn på och kopiera och klistra in blob-behållare, filresurser, tabeller, BLOB, blob mappar, filer och kataloger från i SAS-anslutna konton och behållare</span><span class="sxs-lookup"><span data-stu-id="232e4-326">Rename and copy/paste blob containers, file shares, tables, blobs, blob folders, files and directories from within SAS-attached accounts and containers</span></span>
* <span data-ttu-id="232e4-327">Byta namn på och kopiera blob-behållare och filresurser bevara nu egenskaper och metadata</span><span class="sxs-lookup"><span data-stu-id="232e4-327">Renaming and copying blob containers and file shares now preserve properties and metadata</span></span>

#### <a name="fixes"></a><span data-ttu-id="232e4-328">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="232e4-328">Fixes</span></span>

* <span data-ttu-id="232e4-329">Fast: Det går inte att redigera tabellentiteter om de innehåller boolean eller binära egenskaper</span><span class="sxs-lookup"><span data-stu-id="232e4-329">Fixed: cannot edit table entities if they contain boolean or binary properties</span></span>

#### <a name="known-issues"></a><span data-ttu-id="232e4-330">Kända problem</span><span class="sxs-lookup"><span data-stu-id="232e4-330">Known Issues</span></span>

* <span data-ttu-id="232e4-331">Referenser för sökning efter det att söka i ungefär 50 000 noder - kan prestanda påverkas</span><span class="sxs-lookup"><span data-stu-id="232e4-331">Search handles searching across roughly 50,000 nodes - after this, performance may be impacted</span></span>

<span data-ttu-id="232e4-332">08/03/2016</span><span class="sxs-lookup"><span data-stu-id="232e4-332">08/03/2016</span></span>
### <a name="version-083"></a><span data-ttu-id="232e4-333">Version 0.8.3</span><span class="sxs-lookup"><span data-stu-id="232e4-333">Version 0.8.3</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/HeGW-jkSd9Y?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="232e4-334">Ny</span><span class="sxs-lookup"><span data-stu-id="232e4-334">New</span></span>

* <span data-ttu-id="232e4-335">Byt namn på behållare, tabeller, filresurser</span><span class="sxs-lookup"><span data-stu-id="232e4-335">Rename containers, tables, file shares</span></span>
* <span data-ttu-id="232e4-336">Bättre frågan builder upplevelse</span><span class="sxs-lookup"><span data-stu-id="232e4-336">Improved Query builder experience</span></span>
* <span data-ttu-id="232e4-337">Möjlighet att spara och läsa in frågor</span><span class="sxs-lookup"><span data-stu-id="232e4-337">Ability to save and load queries</span></span>
* <span data-ttu-id="232e4-338">Direkta länkar till storage-konton eller behållare, köer, tabeller eller filresurser för delning och lätt att komma åt dina resurser (endast för Windows - macOS stöder kommer snart!)</span><span class="sxs-lookup"><span data-stu-id="232e4-338">Direct links to storage accounts or containers, queues, tables, or file shares for sharing and easily accessing your resources (Windows-only - macOS support coming soon!)</span></span>
* <span data-ttu-id="232e4-339">Möjlighet att hantera och konfigurera CORS-regler</span><span class="sxs-lookup"><span data-stu-id="232e4-339">Ability to manage and configure CORS rules</span></span>

#### <a name="fixes"></a><span data-ttu-id="232e4-340">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="232e4-340">Fixes</span></span>

* <span data-ttu-id="232e4-341">Fast: Microsoft Accounts kräver omautentisering var 8 – 12: e timme</span><span class="sxs-lookup"><span data-stu-id="232e4-341">Fixed: Microsoft Accounts require re-authentication every 8-12 hours</span></span>

#### <a name="known-issues"></a><span data-ttu-id="232e4-342">Kända problem</span><span class="sxs-lookup"><span data-stu-id="232e4-342">Known Issues</span></span>

* <span data-ttu-id="232e4-343">Ibland Användargränssnittet verka fryst – maximerar fönstret hjälper dig att lösa problemet</span><span class="sxs-lookup"><span data-stu-id="232e4-343">Sometimes the UI might appear frozen - maximizing the window helps resolve this issue</span></span>
* <span data-ttu-id="232e4-344">installera macOS kan kräva förhöjd behörighet</span><span class="sxs-lookup"><span data-stu-id="232e4-344">macOS install may require elevated permissions</span></span>
* <span data-ttu-id="232e4-345">Konto inställningar panelen kan indikera att du måste ange autentiseringsuppgifter för att filtrera prenumerationer</span><span class="sxs-lookup"><span data-stu-id="232e4-345">Account settings panel may show that you need to reenter credentials in order to filter subscriptions</span></span>
* <span data-ttu-id="232e4-346">Byta namn på filresurser, blob-behållare och tabeller bevaras inte metadata eller andra egenskaper för behållare, till exempel filresurskvoten, offentliga åtkomstnivå eller åtkomstprinciper</span><span class="sxs-lookup"><span data-stu-id="232e4-346">Renaming file shares, blob containers, and tables does not preserve metadata or other properties on the container, such as file share quota, public access level or access policies</span></span>
* <span data-ttu-id="232e4-347">Byta namn på blobbar (individuellt eller i en bytt namn till blob-behållare) bevaras inte ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="232e4-347">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="232e4-348">Alla andra egenskaper och metadata för blobbar, filer och entiteter bevaras vid ett byte av namn</span><span class="sxs-lookup"><span data-stu-id="232e4-348">All other properties and metadata for blobs, files and entities are preserved during a rename</span></span>
* <span data-ttu-id="232e4-349">Kopiera eller byta namn på resurser fungerar inte i SAS-anslutna konton</span><span class="sxs-lookup"><span data-stu-id="232e4-349">Copying or renaming resources does not work within SAS-attached accounts</span></span>

<span data-ttu-id="232e4-350">07/07/2016</span><span class="sxs-lookup"><span data-stu-id="232e4-350">07/07/2016</span></span>
### <a name="version-082"></a><span data-ttu-id="232e4-351">Version 0.8.2</span><span class="sxs-lookup"><span data-stu-id="232e4-351">Version 0.8.2</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/nYgKbRUNYZA?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="232e4-352">Ny</span><span class="sxs-lookup"><span data-stu-id="232e4-352">New</span></span>

* <span data-ttu-id="232e4-353">Storage-konton är grupperade efter prenumerationer; utveckling lagring och resurser som är anslutna via SAS eller nyckeln visas under noden (lokala och bifogad)</span><span class="sxs-lookup"><span data-stu-id="232e4-353">Storage Accounts are grouped by subscriptions; development storage and resources attached via key or SAS are shown under (Local and Attached) node</span></span>
* <span data-ttu-id="232e4-354">Logga ut från konton ”Azure kontoinställningar” Kontrollpanelen</span><span class="sxs-lookup"><span data-stu-id="232e4-354">Sign off from accounts in "Azure Account Settings" panel</span></span>
* <span data-ttu-id="232e4-355">Konfigurera proxyinställningar för att aktivera och hantera inloggning</span><span class="sxs-lookup"><span data-stu-id="232e4-355">Configure proxy settings to enable and manage sign-in</span></span>
* <span data-ttu-id="232e4-356">Skapa och ta bort blob-lån</span><span class="sxs-lookup"><span data-stu-id="232e4-356">Create and break blob leases</span></span>
* <span data-ttu-id="232e4-357">Öppna blob-behållare, köer, tabeller och filer med enkelklickning</span><span class="sxs-lookup"><span data-stu-id="232e4-357">Open blob containers, queues, tables, and files with single-click</span></span>

#### <a name="fixes"></a><span data-ttu-id="232e4-358">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="232e4-358">Fixes</span></span>

* <span data-ttu-id="232e4-359">Fast: Kömeddelanden infogas med .NET eller Java-bibliotek är inte korrekt avkoda från base64</span><span class="sxs-lookup"><span data-stu-id="232e4-359">Fixed: queue messages inserted with .NET or Java libraries are not properly decoded from base64</span></span>
* <span data-ttu-id="232e4-360">Fast: $metrics tabeller visas inte för Blob Storage-konton</span><span class="sxs-lookup"><span data-stu-id="232e4-360">Fixed: $metrics tables are not shown for Blob Storage accounts</span></span>
* <span data-ttu-id="232e4-361">Fast: tabeller noden fungerar inte för lokal lagring för (utveckling)</span><span class="sxs-lookup"><span data-stu-id="232e4-361">Fixed: tables node does not work for local (Development) storage</span></span>

#### <a name="known-issues"></a><span data-ttu-id="232e4-362">Kända problem</span><span class="sxs-lookup"><span data-stu-id="232e4-362">Known Issues</span></span>

* <span data-ttu-id="232e4-363">installera macOS kan kräva förhöjd behörighet</span><span class="sxs-lookup"><span data-stu-id="232e4-363">macOS install may require elevated permissions</span></span>

<span data-ttu-id="232e4-364">06/15/2016</span><span class="sxs-lookup"><span data-stu-id="232e4-364">06/15/2016</span></span>
### <a name="version-080"></a><span data-ttu-id="232e4-365">Version 0.8.0</span><span class="sxs-lookup"><span data-stu-id="232e4-365">Version 0.8.0</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/ycfQhKztSIY?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/k4_kOUCZ0WA?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/3zEXJcGdl_k?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="232e4-366">Ny</span><span class="sxs-lookup"><span data-stu-id="232e4-366">New</span></span>

* <span data-ttu-id="232e4-367">Stöd för resursen: visa, överföra, hämta, kopiera filer och kataloger, SAS-URI: er (skapa och ansluta)</span><span class="sxs-lookup"><span data-stu-id="232e4-367">File share support: viewing, uploading, downloading, copying files and directories, SAS URIs (create and connect)</span></span>
* <span data-ttu-id="232e4-368">Förbättrad användarupplevelse för anslutning till lagring med SAS URI: er eller konto nycklar</span><span class="sxs-lookup"><span data-stu-id="232e4-368">Improved user experience for connecting to Storage with SAS URIs or account keys</span></span>
* <span data-ttu-id="232e4-369">Exportera tabell frågeresultat</span><span class="sxs-lookup"><span data-stu-id="232e4-369">Export table query results</span></span>
* <span data-ttu-id="232e4-370">Tabell kolumnordning och anpassning</span><span class="sxs-lookup"><span data-stu-id="232e4-370">Table column reordering and customization</span></span>
* <span data-ttu-id="232e4-371">Visa $logs blob-behållare och $metrics tabeller för Storage-konton med aktiverad</span><span class="sxs-lookup"><span data-stu-id="232e4-371">Viewing $logs blob containers and $metrics tables for Storage Accounts with enabled metrics</span></span>
* <span data-ttu-id="232e4-372">Förbättrad exportera och importera beteende, innehåller nu egenskapsvärdetypen</span><span class="sxs-lookup"><span data-stu-id="232e4-372">Improved export and import behavior, now includes property value type</span></span>

#### <a name="fixes"></a><span data-ttu-id="232e4-373">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="232e4-373">Fixes</span></span>

* <span data-ttu-id="232e4-374">Fast: överföra eller hämta stora blobbar kan resultera i ofullständiga överföringar/hämtningar</span><span class="sxs-lookup"><span data-stu-id="232e4-374">Fixed: uploading or downloading large blobs can result in incomplete uploads/downloads</span></span>
* <span data-ttu-id="232e4-375">Fast: redigera, lägga till eller importera en entitet med ett numeriskt värde (”1”) konverteras till dubbla</span><span class="sxs-lookup"><span data-stu-id="232e4-375">Fixed: editing, adding, or importing an entity with a numeric string value ("1") will convert it to double</span></span>
* <span data-ttu-id="232e4-376">Fast: Det gick inte att expandera noden tabellen i den lokala utvecklingsmiljön</span><span class="sxs-lookup"><span data-stu-id="232e4-376">Fixed: Unable to expand the table node in the local development environment</span></span>

#### <a name="known-issues"></a><span data-ttu-id="232e4-377">Kända problem</span><span class="sxs-lookup"><span data-stu-id="232e4-377">Known Issues</span></span>

* <span data-ttu-id="232e4-378">$metrics tabeller är inte synliga för Blob Storage-konton</span><span class="sxs-lookup"><span data-stu-id="232e4-378">$metrics tables are not visible for Blob Storage accounts</span></span>
* <span data-ttu-id="232e4-379">Kömeddelanden som lagts till via programmering kan inte visas korrekt om meddelanden är kodad med Base64-kodning</span><span class="sxs-lookup"><span data-stu-id="232e4-379">Queue messages added programmatically may not be displayed correctly if the messages are encoded using Base64 encoding</span></span>

<span data-ttu-id="232e4-380">05/17/2016</span><span class="sxs-lookup"><span data-stu-id="232e4-380">05/17/2016</span></span>
### <a name="version-07201605090"></a><span data-ttu-id="232e4-381">Version 0.7.20160509.0</span><span class="sxs-lookup"><span data-stu-id="232e4-381">Version 0.7.20160509.0</span></span>

#### <a name="new"></a><span data-ttu-id="232e4-382">Ny</span><span class="sxs-lookup"><span data-stu-id="232e4-382">New</span></span>

* <span data-ttu-id="232e4-383">Bättre felhantering för appen kraschar</span><span class="sxs-lookup"><span data-stu-id="232e4-383">Better error handling for app crashes</span></span>

#### <a name="fixes"></a><span data-ttu-id="232e4-384">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="232e4-384">Fixes</span></span>

* <span data-ttu-id="232e4-385">Fast bugg där Informationsfältet meddelanden ibland inte visas när behövde inloggningsuppgifter</span><span class="sxs-lookup"><span data-stu-id="232e4-385">Fixed bug where InfoBar messages sometimes don't show up when sign-in credentials were required</span></span>

#### <a name="known-issues"></a><span data-ttu-id="232e4-386">Kända problem</span><span class="sxs-lookup"><span data-stu-id="232e4-386">Known Issues</span></span>

* <span data-ttu-id="232e4-387">Tabeller: Lägga till, redigera eller importera en entitet som har en egenskap med ett ambiguously numeriska värde, till exempel ”1” eller ”1.0”, och användaren försöker skicka den som en `Edm.String`, värdet kommer tillbaka till klienten API som en Edm.Double</span><span class="sxs-lookup"><span data-stu-id="232e4-387">Tables: Adding, editing, or importing an entity that has a property with an ambiguously numeric value, such as "1" or "1.0", and the user tries to send it as an `Edm.String`, the value will come back through the client API as an Edm.Double</span></span>

<span data-ttu-id="232e4-388">03/31/2016</span><span class="sxs-lookup"><span data-stu-id="232e4-388">03/31/2016</span></span>

### <a name="version-07201603250"></a><span data-ttu-id="232e4-389">Version 0.7.20160325.0</span><span class="sxs-lookup"><span data-stu-id="232e4-389">Version 0.7.20160325.0</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/imbgBRHX65A?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/ceX-P8XZ-s8?ecver=1" frameborder="0" allowfullscreen></iframe>


#### <a name="new"></a><span data-ttu-id="232e4-390">Ny</span><span class="sxs-lookup"><span data-stu-id="232e4-390">New</span></span>

* <span data-ttu-id="232e4-391">Tabell stöd: visa, frågor, exportera, importera och CRUD-åtgärder för entiteter</span><span class="sxs-lookup"><span data-stu-id="232e4-391">Table support: viewing, querying, export, import, and CRUD operations for entities</span></span>
* <span data-ttu-id="232e4-392">Kö stöd: visa, lägga till dequeueing meddelanden</span><span class="sxs-lookup"><span data-stu-id="232e4-392">Queue support: viewing, adding, dequeueing messages</span></span>
* <span data-ttu-id="232e4-393">Generera SAS URI: er för Storage-konton</span><span class="sxs-lookup"><span data-stu-id="232e4-393">Generating SAS URIs for Storage Accounts</span></span>
* <span data-ttu-id="232e4-394">Ansluta till Lagringskonton med SAS URI: er</span><span class="sxs-lookup"><span data-stu-id="232e4-394">Connecting to Storage Accounts with SAS URIs</span></span>
* <span data-ttu-id="232e4-395">Meddelanden om uppdateringar för framtida uppdateringar Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="232e4-395">Update notifications for future updates to Storage Explorer</span></span>
* <span data-ttu-id="232e4-396">Uppdaterade känsla och utseende</span><span class="sxs-lookup"><span data-stu-id="232e4-396">Updated look and feel</span></span>

#### <a name="fixes"></a><span data-ttu-id="232e4-397">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="232e4-397">Fixes</span></span>

* <span data-ttu-id="232e4-398">Förbättringar av prestanda och tillförlitlighet</span><span class="sxs-lookup"><span data-stu-id="232e4-398">Performance and reliability improvements</span></span>

### <a name="known-issues-amp-mitigations"></a><span data-ttu-id="232e4-399">Kända problem &amp; åtgärder</span><span class="sxs-lookup"><span data-stu-id="232e4-399">Known Issues &amp; Mitigations</span></span>

* <span data-ttu-id="232e4-400">Hämta stora blob filer fungerar inte - vi rekommenderar att du använder AzCopy medan vi löser problemet</span><span class="sxs-lookup"><span data-stu-id="232e4-400">Download of large blob files does not work correctly - we recommend using AzCopy while we address this issue</span></span> 
* <span data-ttu-id="232e4-401">Autentiseringsuppgifter att inte hämta och cachelagras om arbetsmappen hittas inte eller kan inte skrivas till</span><span class="sxs-lookup"><span data-stu-id="232e4-401">Account credentials will not be retrieved nor cached if the home folder cannot be found or cannot be written to</span></span>
* <span data-ttu-id="232e4-402">Om vi lägger till, redigera eller importera en entitet som har en egenskap med ett ambiguously numeriska värde, till exempel ”1” eller ”1.0”, och användaren försöker skicka den som en `Edm.String`, värdet kommer tillbaka till klienten API som en Edm.Double</span><span class="sxs-lookup"><span data-stu-id="232e4-402">If we are adding, editing, or importing an entity that has a property with an ambiguously numeric value, such as "1" or "1.0", and the user tries to send it as an `Edm.String`, the value will come back through the client API as an Edm.Double</span></span>
* <span data-ttu-id="232e4-403">När du importerar CSV-filer med flera rader innehåller kan data få hackas eller kodade</span><span class="sxs-lookup"><span data-stu-id="232e4-403">When importing CSV files with multiline records, the data may get chopped or scrambled</span></span>

<span data-ttu-id="232e4-404">02/03/2016</span><span class="sxs-lookup"><span data-stu-id="232e4-404">02/03/2016</span></span>

### <a name="version-07201601291"></a><span data-ttu-id="232e4-405">Version 0.7.20160129.1</span><span class="sxs-lookup"><span data-stu-id="232e4-405">Version 0.7.20160129.1</span></span>

#### <a name="fixes"></a><span data-ttu-id="232e4-406">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="232e4-406">Fixes</span></span>

* <span data-ttu-id="232e4-407">Förbättrad prestanda när du laddar upp, hämta och kopiera BLOB</span><span class="sxs-lookup"><span data-stu-id="232e4-407">Improved overall performance when uploading, downloading and copying blobs</span></span>

<span data-ttu-id="232e4-408">01/14/2016</span><span class="sxs-lookup"><span data-stu-id="232e4-408">01/14/2016</span></span>

### <a name="version-07201601050"></a><span data-ttu-id="232e4-409">Version 0.7.20160105.0</span><span class="sxs-lookup"><span data-stu-id="232e4-409">Version 0.7.20160105.0</span></span>

#### <a name="new"></a><span data-ttu-id="232e4-410">Ny</span><span class="sxs-lookup"><span data-stu-id="232e4-410">New</span></span>

* <span data-ttu-id="232e4-411">Linux-support (OSX paritet funktioner)</span><span class="sxs-lookup"><span data-stu-id="232e4-411">Linux support (parity features to OSX)</span></span>
* <span data-ttu-id="232e4-412">Lägga till blob-behållare med delad åtkomst signaturer (SAS)-nyckel</span><span class="sxs-lookup"><span data-stu-id="232e4-412">Add blob containers with Shared Access Signatures (SAS) key</span></span>
* <span data-ttu-id="232e4-413">Lägg till Lagringskonton för Azure Kina</span><span class="sxs-lookup"><span data-stu-id="232e4-413">Add Storage Accounts for Azure China</span></span>
* <span data-ttu-id="232e4-414">Lägg till Storage-konton med anpassade slutpunkter</span><span class="sxs-lookup"><span data-stu-id="232e4-414">Add Storage Accounts with custom endpoints</span></span>
* <span data-ttu-id="232e4-415">Öppna och visa innehållet text- och blobbar</span><span class="sxs-lookup"><span data-stu-id="232e4-415">Open and view the contents text and picture blobs</span></span>
* <span data-ttu-id="232e4-416">Visa och redigera blob-egenskaper och metadata</span><span class="sxs-lookup"><span data-stu-id="232e4-416">View and edit blob properties and metadata</span></span>

#### <a name="fixes"></a><span data-ttu-id="232e4-417">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="232e4-417">Fixes</span></span>

* <span data-ttu-id="232e4-418">Fast: överföra eller hämta ett stort antal blobbar (500 +) kan ibland orsaka att appen har en vit skärm</span><span class="sxs-lookup"><span data-stu-id="232e4-418">Fixed: uploading or download a large number of blobs (500+) may sometimes cause the app to have a white screen</span></span> 
* <span data-ttu-id="232e4-419">Fast: när blob-behållaren offentliga åtkomstnivå kan det nya värdet uppdateras inte förrän du har angett fokus på behållaren igen.</span><span class="sxs-lookup"><span data-stu-id="232e4-419">Fixed: when setting blob container public access level, the new value is not updated until you re-set the focus on the container.</span></span> <span data-ttu-id="232e4-420">Dessutom dialogrutan alltid som standard ”ingen offentlig åtkomst” och inte det faktiska aktuella värdet.</span><span class="sxs-lookup"><span data-stu-id="232e4-420">Also, the dialog always defaults to "No public access", and not the actual current value.</span></span>
* <span data-ttu-id="232e4-421">Stöd för bättre övergripande tangentbord/tillgänglighet och UI</span><span class="sxs-lookup"><span data-stu-id="232e4-421">Better overall keyboard/accessibility and UI support</span></span>
* <span data-ttu-id="232e4-422">Spår historik texten ska radbrytas när den är lång med ett blanksteg</span><span class="sxs-lookup"><span data-stu-id="232e4-422">Breadcrumbs history text wraps when it's long with white space</span></span>
* <span data-ttu-id="232e4-423">SAS-dialogrutan har stöd för verifiering av indata</span><span class="sxs-lookup"><span data-stu-id="232e4-423">SAS dialog supports input validation</span></span>
* <span data-ttu-id="232e4-424">Lokal lagring fortsätter att fungera även om användarens autentiseringsuppgifter har upphört att gälla</span><span class="sxs-lookup"><span data-stu-id="232e4-424">Local storage continues to be available even if user credentials have expired</span></span>
* <span data-ttu-id="232e4-425">När du tar bort en öppen blobbehållare är blob-Utforskaren på höger sida stängd</span><span class="sxs-lookup"><span data-stu-id="232e4-425">When an opened blob container is deleted, the blob explorer on the right side is closed</span></span>

#### <a name="known-issues"></a><span data-ttu-id="232e4-426">Kända problem</span><span class="sxs-lookup"><span data-stu-id="232e4-426">Known Issues</span></span>

* <span data-ttu-id="232e4-427">Linux-installationen måste gcc version uppdateras eller uppgraderas – steg för att uppgradera nedan:</span><span class="sxs-lookup"><span data-stu-id="232e4-427">Linux install needs gcc version updated or upgraded – steps to upgrade are below:</span></span> 
    * `sudo add-apt-repository ppa:ubuntu-toolchain-r/test`
    * `sudo apt-get update`
    * `sudo apt-get upgrade`
    * `sudo apt-get dist-upgrade`

<span data-ttu-id="232e4-428">11/18/2015</span><span class="sxs-lookup"><span data-stu-id="232e4-428">11/18/2015</span></span>
### <a name="version-07201511160"></a><span data-ttu-id="232e4-429">Version 0.7.20151116.0</span><span class="sxs-lookup"><span data-stu-id="232e4-429">Version 0.7.20151116.0</span></span>

#### <a name="new"></a><span data-ttu-id="232e4-430">Ny</span><span class="sxs-lookup"><span data-stu-id="232e4-430">New</span></span>

* <span data-ttu-id="232e4-431">macOS och versioner av Windows</span><span class="sxs-lookup"><span data-stu-id="232e4-431">macOS, and Windows versions</span></span>
* <span data-ttu-id="232e4-432">Logga in att visa dina Lagringskonton – använda din organisations konto, Account, 2FA osv.</span><span class="sxs-lookup"><span data-stu-id="232e4-432">Sign in to view your Storage Accounts – use your Org Account, Microsoft Account, 2FA, etc.</span></span>
* <span data-ttu-id="232e4-433">Lokal utveckling lagring (Använd storage-emulatorn endast för Windows)</span><span class="sxs-lookup"><span data-stu-id="232e4-433">Local development storage (use storage emulator, Windows-only)</span></span>
* <span data-ttu-id="232e4-434">Stöd för Azure Resource Manager och klassisk resurs</span><span class="sxs-lookup"><span data-stu-id="232e4-434">Azure Resource Manager and Classic resource support</span></span>
* <span data-ttu-id="232e4-435">Skapa och ta bort blobbar, köer och tabeller</span><span class="sxs-lookup"><span data-stu-id="232e4-435">Create and delete blobs, queues, or tables</span></span>
* <span data-ttu-id="232e4-436">Sök efter specifika blobbar, köer och tabeller</span><span class="sxs-lookup"><span data-stu-id="232e4-436">Search for specific blobs, queues, or tables</span></span>
* <span data-ttu-id="232e4-437">Utforska innehållet i blob-behållare</span><span class="sxs-lookup"><span data-stu-id="232e4-437">Explore the contents of blob containers</span></span>
* <span data-ttu-id="232e4-438">Visa och navigera genom kataloger</span><span class="sxs-lookup"><span data-stu-id="232e4-438">View and navigate through directories</span></span>
* <span data-ttu-id="232e4-439">Ladda upp, hämta och ta bort blobbar och mappar</span><span class="sxs-lookup"><span data-stu-id="232e4-439">Upload, download, and delete blobs and folders</span></span>
* <span data-ttu-id="232e4-440">Visa och redigera blob-egenskaper och metadata</span><span class="sxs-lookup"><span data-stu-id="232e4-440">View and edit blob properties and metadata</span></span>
* <span data-ttu-id="232e4-441">Generera en SAS-nycklar</span><span class="sxs-lookup"><span data-stu-id="232e4-441">Generate SAS keys</span></span>
* <span data-ttu-id="232e4-442">Hantera och skapa lagras åtkomst principer SAP)</span><span class="sxs-lookup"><span data-stu-id="232e4-442">Manage and create Stored Access Policies (SAP)</span></span>
* <span data-ttu-id="232e4-443">Sök efter blobbar efter prefix</span><span class="sxs-lookup"><span data-stu-id="232e4-443">Search for blobs by prefix</span></span>
* <span data-ttu-id="232e4-444">Dra 'n släppa filer att överföra eller hämta</span><span class="sxs-lookup"><span data-stu-id="232e4-444">Drag 'n drop files to upload or download</span></span>

#### <a name="known-issues"></a><span data-ttu-id="232e4-445">Kända problem</span><span class="sxs-lookup"><span data-stu-id="232e4-445">Known Issues</span></span>

* <span data-ttu-id="232e4-446">När du ställer in blob-behållaren offentliga åtkomstnivå, uppdateras inte det nya värdet förrän du har angett fokus på behållaren igen</span><span class="sxs-lookup"><span data-stu-id="232e4-446">When setting blob container public access level, the new value is not updated until you re-set the focus on the container</span></span>
* <span data-ttu-id="232e4-447">När du öppnar dialogrutan för att ställa in allmän åtkomst visas alltid ”ingen offentlig åtkomst” som standard och inte faktiska aktuella värde</span><span class="sxs-lookup"><span data-stu-id="232e4-447">When you open the dialog to set the public access level, it always shows "No public access" as the default, and not the actual current value</span></span>
* <span data-ttu-id="232e4-448">Det går inte att byta namn på hämtade blobbar</span><span class="sxs-lookup"><span data-stu-id="232e4-448">Cannot rename downloaded blobs</span></span>
* <span data-ttu-id="232e4-449">Aktiviteten loggposter kommer ibland ”fastna” i en pågående tillstånd när ett fel inträffar och felet inte visas</span><span class="sxs-lookup"><span data-stu-id="232e4-449">Activity log entries will sometimes get "stuck" in an in progress state when an error occurs, and the error is not displayed</span></span>
* <span data-ttu-id="232e4-450">Ibland kraschar eller stängs helt vit när du försöker överföra eller hämta ett stort antal blobbar</span><span class="sxs-lookup"><span data-stu-id="232e4-450">Sometimes crashes or turns completely white when trying to upload or download a large number of blobs</span></span>
* <span data-ttu-id="232e4-451">Ibland avbryter en kopieringsåtgärd fungerar inte</span><span class="sxs-lookup"><span data-stu-id="232e4-451">Sometimes canceling a copy operation does not work</span></span>
* <span data-ttu-id="232e4-452">När du skapar en behållare (tabell-blob/kön), om du vill ange ett ogiltigt namn och fortsätta med att skapa en annan under en annan Behållartyp kan inte du ange fokus på den nya typen</span><span class="sxs-lookup"><span data-stu-id="232e4-452">During creating a container (blob/queue/table), if you input an invalid name and proceed to create another under a different container type you cannot set focus on the new type</span></span>
* <span data-ttu-id="232e4-453">Det går inte att skapa en ny mapp eller byta namn på mappen</span><span class="sxs-lookup"><span data-stu-id="232e4-453">Can't create new folder or rename folder</span></span>




[2]: https://support.microsoft.com/en-us/help/4021389/storage-explorer-troubleshooting-guide
[3]: vs-azure-tools-storage-manage-with-storage-explorer.md