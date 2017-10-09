---
title: "viktig information för aaaMicrosoft Azure Lagringsutforskaren (förhandsversion) | Microsoft Docs"
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
ms.openlocfilehash: 44ff6dc8e2015f4eb71fa13098b832fbbf48ccac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-azure-storage-explorer-preview-release-notes"></a><span data-ttu-id="0fed1-103">Viktig information för Microsoft Azure Lagringsutforskaren (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="0fed1-103">Microsoft Azure Storage Explorer (Preview) release notes</span></span>

<span data-ttu-id="0fed1-104">Den här artikeln innehåller hello versionen viktig information för Azure Lagringsutforskaren 0.8.16 (förhandsgranskning) samt viktig information för tidigare versioner.</span><span class="sxs-lookup"><span data-stu-id="0fed1-104">This article contains hello release notes for Azure Storage Explorer 0.8.16 (Preview) release, as well as release notes for previous versions.</span></span>

<span data-ttu-id="0fed1-105">[Microsoft Azure Lagringsutforskaren (förhandsversion)](./vs-azure-tools-storage-manage-with-storage-explorer.md) är en fristående app som du kan använda tooeasily fungerar med Azure Storage-data i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="0fed1-105">[Microsoft Azure Storage Explorer (Preview)](./vs-azure-tools-storage-manage-with-storage-explorer.md) is a standalone app that enables you tooeasily work with Azure Storage data on Windows, macOS, and Linux.</span></span>

## <a name="version-0816-preview"></a><span data-ttu-id="0fed1-106">Version 0.8.16 (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="0fed1-106">Version 0.8.16 (Preview)</span></span>
<span data-ttu-id="0fed1-107">8/21/2017</span><span class="sxs-lookup"><span data-stu-id="0fed1-107">8/21/2017</span></span>

### <a name="download-azure-storage-explorer-0816-preview"></a><span data-ttu-id="0fed1-108">Hämta Azure Lagringsutforskaren 0.8.16 (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="0fed1-108">Download Azure Storage Explorer 0.8.16 (Preview)</span></span>
- [<span data-ttu-id="0fed1-109">Azure Lagringsutforskaren 0.8.16 (förhandsversion) för Windows</span><span class="sxs-lookup"><span data-stu-id="0fed1-109">Azure Storage Explorer 0.8.16 (Preview) for Windows</span></span>](https://go.microsoft.com/fwlink/?LinkId=708343)
- [<span data-ttu-id="0fed1-110">Azure Lagringsutforskaren (förhandsversion) för 0.8.16 för Mac</span><span class="sxs-lookup"><span data-stu-id="0fed1-110">Azure Storage Explorer 0.8.16 (Preview) for Mac</span></span>](https://go.microsoft.com/fwlink/?LinkId=708342)
- [<span data-ttu-id="0fed1-111">Azure Lagringsutforskaren (förhandsversion) för 0.8.16 för Linux</span><span class="sxs-lookup"><span data-stu-id="0fed1-111">Azure Storage Explorer 0.8.16 (Preview) for Linux</span></span>](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a><span data-ttu-id="0fed1-112">Ny</span><span class="sxs-lookup"><span data-stu-id="0fed1-112">New</span></span>
* <span data-ttu-id="0fed1-113">När du öppnar en blob uppmanas Lagringsutforskaren du tooupload hello ned filen om en ändring identifieras</span><span class="sxs-lookup"><span data-stu-id="0fed1-113">When you open a blob, Storage Explorer will prompt you tooupload hello downloaded file if a change is detected</span></span>
* <span data-ttu-id="0fed1-114">Förbättrad Azure Stack inloggning</span><span class="sxs-lookup"><span data-stu-id="0fed1-114">Enhanced Azure Stack sign-in experience</span></span>
* <span data-ttu-id="0fed1-115">Förbättrad hello prestanda för att ladda upp/hämta många små filer på hello samma tid</span><span class="sxs-lookup"><span data-stu-id="0fed1-115">Improved hello performance of uploading/downloading many small files at hello same time</span></span>


### <a name="fixes"></a><span data-ttu-id="0fed1-116">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="0fed1-116">Fixes</span></span>
* <span data-ttu-id="0fed1-117">För vissa typer av blob leder väljer för ”Ersätt” under en överför hamnar i konflikt ibland hello överför startas.</span><span class="sxs-lookup"><span data-stu-id="0fed1-117">For some blob types, choosing too"replace" during an upload conflict would sometimes result in hello upload being restarted.</span></span> 
* <span data-ttu-id="0fed1-118">I version 0.8.15 skulle överföringar ibland av stopp vid 99%.</span><span class="sxs-lookup"><span data-stu-id="0fed1-118">In version 0.8.15, uploads would sometimes stall at 99%.</span></span>
* <span data-ttu-id="0fed1-119">När du överför filer tooa filresurs, om du väljer tooupload tooa directory som ännu inte finns, misslyckas överföra.</span><span class="sxs-lookup"><span data-stu-id="0fed1-119">When uploading files tooa file share, if you chose tooupload tooa directory which did not yet exist, your upload would fail.</span></span>
* <span data-ttu-id="0fed1-120">Lagringsutforskaren felaktigt generera tidsstämplar för signaturer för delad åtkomst och tabellen.</span><span class="sxs-lookup"><span data-stu-id="0fed1-120">Storage Explorer was incorrectly generating time stamps for shared access signatures and table queries.</span></span>


<span data-ttu-id="0fed1-121">Kända problem</span><span class="sxs-lookup"><span data-stu-id="0fed1-121">Known Issues</span></span>
* <span data-ttu-id="0fed1-122">Med hjälp av ett namn och nyckel anslutningssträngen fungerar inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="0fed1-122">Using a name and key connection string does not currently work.</span></span> <span data-ttu-id="0fed1-123">Den kommer att åtgärdas i hello nästa utgåva.</span><span class="sxs-lookup"><span data-stu-id="0fed1-123">It will be fixed in hello next release.</span></span> <span data-ttu-id="0fed1-124">Fram till dess kan du använda bifoga med namn och nyckel.</span><span class="sxs-lookup"><span data-stu-id="0fed1-124">Until then you can use attach with name and key.</span></span>
* <span data-ttu-id="0fed1-125">Om du försöker tooopen en fil med ett ogiltigt filnamn för Windows resulterar hello download i en fil hittades inte.</span><span class="sxs-lookup"><span data-stu-id="0fed1-125">If you try tooopen a file with an invalid Windows file name, hello download will result in a file not found error.</span></span>
* <span data-ttu-id="0fed1-126">När du klickar på ”Avbryt” för en uppgift, kan det ta ett tag för att aktiviteten toocancel.</span><span class="sxs-lookup"><span data-stu-id="0fed1-126">After clicking "Cancel" on a task, it may take a while for that task toocancel.</span></span> <span data-ttu-id="0fed1-127">Detta är en begränsning för hello Azure lagringsnod bibliotek.</span><span class="sxs-lookup"><span data-stu-id="0fed1-127">This is a limitation of hello Azure Storage Node library.</span></span>
* <span data-ttu-id="0fed1-128">När du har slutfört en blob-överför uppdateras hello fliken som initierade överföringen hello.</span><span class="sxs-lookup"><span data-stu-id="0fed1-128">After completing a blob upload, hello tab which initiated hello upload is refreshed.</span></span> <span data-ttu-id="0fed1-129">Detta är en förändring jämfört med tidigare beteende och medför även du toobe tas tillbaka toohello rot hello behållare i.</span><span class="sxs-lookup"><span data-stu-id="0fed1-129">This is a change from previous behavior, and will also cause you toobe taken back toohello root of hello container you are in.</span></span>
* <span data-ttu-id="0fed1-130">Om du väljer hello fel PIN-kod/smartkort-certifikat och du behöver toorestart i ordning toohave Lagringsutforskaren Glöm detta beslut.</span><span class="sxs-lookup"><span data-stu-id="0fed1-130">If you choose hello wrong PIN/Smartcard certificate, then you will need toorestart in order toohave Storage Explorer forget that decision.</span></span>
* <span data-ttu-id="0fed1-131">hello konto inställningar panelen kan indikera att du behöver tooreenter autentiseringsuppgifter toofilter prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="0fed1-131">hello account settings panel may show that you need tooreenter credentials toofilter subscriptions.</span></span>
* <span data-ttu-id="0fed1-132">Byta namn på blobbar (individuellt eller i en bytt namn till blob-behållare) bevaras inte ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="0fed1-132">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="0fed1-133">Alla andra egenskaper och metadata för blobbar, filer och entiteter bevaras vid ett byte av namn.</span><span class="sxs-lookup"><span data-stu-id="0fed1-133">All other properties and metadata for blobs, files and entities are preserved during a rename.</span></span>
* <span data-ttu-id="0fed1-134">Även om Azure-stacken inte stöder filresurser, visas en filresurser nod fortfarande under ett bifogade Stack för Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="0fed1-134">Although Azure Stack doesn't currently support Files Shares, a File Shares node still appears under an attached Azure Stack storage account.</span></span>
* <span data-ttu-id="0fed1-135">För användare på Ubuntu 14.04 måste tooensure GCC är toodate – kan du göra det genom att köra hello följande kommandon och sedan starta om datorn:</span><span class="sxs-lookup"><span data-stu-id="0fed1-135">For users on Ubuntu 14.04, you will need tooensure GCC is up toodate - this can be done by running hello following commands, and then restarting your machine:</span></span>

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* <span data-ttu-id="0fed1-136">Du behöver tooinstall GConf – detta kan göras genom att köra hello följande kommandon och sedan starta om datorn för användare på Ubuntu nr 17.04 från:</span><span class="sxs-lookup"><span data-stu-id="0fed1-136">For users on Ubuntu 17.04, you will need tooinstall GConf - this can be done by running hello following commands, and then restarting your machine:</span></span>

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-0814-preview"></a><span data-ttu-id="0fed1-137">Version 0.8.14 (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="0fed1-137">Version 0.8.14 (Preview)</span></span>
<span data-ttu-id="0fed1-138">06/22/2017</span><span class="sxs-lookup"><span data-stu-id="0fed1-138">06/22/2017</span></span>

### <a name="download-azure-storage-explorer-0814-preview"></a><span data-ttu-id="0fed1-139">Hämta Azure Lagringsutforskaren 0.8.14 (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="0fed1-139">Download Azure Storage Explorer 0.8.14 (Preview)</span></span>
* [<span data-ttu-id="0fed1-140">Hämta Azure Lagringsutforskaren 0.8.14 (förhandsversion) för Windows</span><span class="sxs-lookup"><span data-stu-id="0fed1-140">Download Azure Storage Explorer 0.8.14 (Preview) for Windows</span></span>](https://go.microsoft.com/fwlink/?LinkId=809306)
* [<span data-ttu-id="0fed1-141">Hämta Azure Lagringsutforskaren (förhandsversion) för 0.8.14 för Mac</span><span class="sxs-lookup"><span data-stu-id="0fed1-141">Download Azure Storage Explorer 0.8.14 (Preview) for Mac</span></span>](https://go.microsoft.com/fwlink/?LinkId=809307)
* [<span data-ttu-id="0fed1-142">Hämta Azure Lagringsutforskaren (förhandsversion) för 0.8.14 för Linux</span><span class="sxs-lookup"><span data-stu-id="0fed1-142">Download Azure Storage Explorer 0.8.14 (Preview) for Linux</span></span>](https://go.microsoft.com/fwlink/?LinkId=809308)

### <a name="new"></a><span data-ttu-id="0fed1-143">Ny</span><span class="sxs-lookup"><span data-stu-id="0fed1-143">New</span></span>

* <span data-ttu-id="0fed1-144">Uppdaterade Electron version too1.7.2 i ordning tootake nytta av flera viktiga säkerhetsuppdateringar</span><span class="sxs-lookup"><span data-stu-id="0fed1-144">Updated Electron version too1.7.2 in order tootake advantage of several critical security updates</span></span>
* <span data-ttu-id="0fed1-145">Du kan snabbt ansluta hello online felsökningsguiden hello Hjälp-menyn</span><span class="sxs-lookup"><span data-stu-id="0fed1-145">You can now quickly access hello online troubleshooting guide from hello help menu</span></span>
* <span data-ttu-id="0fed1-146">Lagringsutforskaren felsökning [Guide][2]</span><span class="sxs-lookup"><span data-stu-id="0fed1-146">Storage Explorer Troubleshooting [Guide][2]</span></span>
* <span data-ttu-id="0fed1-147">[Instruktioner] [ 3] ansluta tooan Stack för Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="0fed1-147">[Instructions][3] on connecting tooan Azure Stack subscription</span></span>

### <a name="known-issues"></a><span data-ttu-id="0fed1-148">Kända problem</span><span class="sxs-lookup"><span data-stu-id="0fed1-148">Known Issues</span></span>

* <span data-ttu-id="0fed1-149">Knapparna i hello ta bort mappen bekräftelsedialogruta registrera inte med hello musklickningar på Linux.</span><span class="sxs-lookup"><span data-stu-id="0fed1-149">Buttons on hello delete folder confirmation dialog don't register with hello mouse clicks on Linux.</span></span> <span data-ttu-id="0fed1-150">Lösningen är toouse hello RETUR-tangenten</span><span class="sxs-lookup"><span data-stu-id="0fed1-150">Workaround is toouse hello Enter key</span></span>
* <span data-ttu-id="0fed1-151">Om du väljer hello fel PIN-kod/smartkortcertifikat sedan behöver du toorestart i ordning toohave Lagringsutforskaren Glöm hello beslut</span><span class="sxs-lookup"><span data-stu-id="0fed1-151">If you choose hello wrong PIN/Smartcard certificate then you will need toorestart in order toohave Storage Explorer forget hello decision</span></span>
* <span data-ttu-id="0fed1-152">Med mer än 3 grupper av blobbar eller filer överföra i hello samma kan tid orsaka fel</span><span class="sxs-lookup"><span data-stu-id="0fed1-152">Having more than 3 groups of blobs or files uploading at hello same time may cause errors</span></span>
* <span data-ttu-id="0fed1-153">hello konto inställningar panelen kan indikera att du behöver tooreenter autentiseringsuppgifter i ordning toofilter prenumerationer</span><span class="sxs-lookup"><span data-stu-id="0fed1-153">hello account settings panel may show that you need tooreenter credentials in order toofilter subscriptions</span></span>
* <span data-ttu-id="0fed1-154">Byta namn på blobbar (individuellt eller i en bytt namn till blob-behållare) bevaras inte ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="0fed1-154">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="0fed1-155">Alla andra egenskaper och metadata för blobbar, filer och entiteter bevaras vid ett byte av namn.</span><span class="sxs-lookup"><span data-stu-id="0fed1-155">All other properties and metadata for blobs, files and entities are preserved during a rename.</span></span>
* <span data-ttu-id="0fed1-156">Även om Azure-stacken inte stöder filresurser, visas en filresurser nod fortfarande under ett bifogade Stack för Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="0fed1-156">Although Azure Stack doesn't currently support File Shares, a File Shares node still appears under an attached Azure Stack storage account.</span></span> 
* <span data-ttu-id="0fed1-157">Ubuntu 14.04 installera behov gcc version uppdateras eller uppgraderas – steg tooupgrade finns nedan:</span><span class="sxs-lookup"><span data-stu-id="0fed1-157">Ubuntu 14.04 install needs gcc version updated or upgraded – steps tooupgrade are below:</span></span>

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```




## <a name="previous-releases"></a><span data-ttu-id="0fed1-158">Tidigare versioner</span><span class="sxs-lookup"><span data-stu-id="0fed1-158">Previous releases</span></span>

* [<span data-ttu-id="0fed1-159">Version 0.8.13</span><span class="sxs-lookup"><span data-stu-id="0fed1-159">Version 0.8.13</span></span>](#version-0813)
* [<span data-ttu-id="0fed1-160">Version 0.8.12 / 0.8.11 / 0.8.10</span><span class="sxs-lookup"><span data-stu-id="0fed1-160">Version 0.8.12 / 0.8.11 / 0.8.10</span></span>](#version-0812--0811--0810)
* [<span data-ttu-id="0fed1-161">Version 0.8.9 / 0.8.8</span><span class="sxs-lookup"><span data-stu-id="0fed1-161">Version 0.8.9 / 0.8.8</span></span>](#version-089--088)
* [<span data-ttu-id="0fed1-162">Version 0.8.7</span><span class="sxs-lookup"><span data-stu-id="0fed1-162">Version 0.8.7</span></span>](#version-087)
* [<span data-ttu-id="0fed1-163">Version 0.8.6</span><span class="sxs-lookup"><span data-stu-id="0fed1-163">Version 0.8.6</span></span>](#version-086)
* [<span data-ttu-id="0fed1-164">Version 0.8.5</span><span class="sxs-lookup"><span data-stu-id="0fed1-164">Version 0.8.5</span></span>](#version-085)
* [<span data-ttu-id="0fed1-165">Version 0.8.4</span><span class="sxs-lookup"><span data-stu-id="0fed1-165">Version 0.8.4</span></span>](#version-084)
* [<span data-ttu-id="0fed1-166">Version 0.8.3</span><span class="sxs-lookup"><span data-stu-id="0fed1-166">Version 0.8.3</span></span>](#version-083)
* [<span data-ttu-id="0fed1-167">Version 0.8.2</span><span class="sxs-lookup"><span data-stu-id="0fed1-167">Version 0.8.2</span></span>](#version-082)
* [<span data-ttu-id="0fed1-168">Version 0.8.0</span><span class="sxs-lookup"><span data-stu-id="0fed1-168">Version 0.8.0</span></span>](#version-080)
* [<span data-ttu-id="0fed1-169">Version 0.7.20160509.0</span><span class="sxs-lookup"><span data-stu-id="0fed1-169">Version 0.7.20160509.0</span></span>](#version-07201605090)
* [<span data-ttu-id="0fed1-170">Version 0.7.20160325.0</span><span class="sxs-lookup"><span data-stu-id="0fed1-170">Version 0.7.20160325.0</span></span>](#version-07201603250)
* [<span data-ttu-id="0fed1-171">Version 0.7.20160129.1</span><span class="sxs-lookup"><span data-stu-id="0fed1-171">Version 0.7.20160129.1</span></span>](#version-07201601291)
* [<span data-ttu-id="0fed1-172">Version 0.7.20160105.0</span><span class="sxs-lookup"><span data-stu-id="0fed1-172">Version 0.7.20160105.0</span></span>](#version-07201601050)
* [<span data-ttu-id="0fed1-173">Version 0.7.20151116.0</span><span class="sxs-lookup"><span data-stu-id="0fed1-173">Version 0.7.20151116.0</span></span>](#version-07201511160)


### <a name="version-0813"></a><span data-ttu-id="0fed1-174">Version 0.8.13</span><span class="sxs-lookup"><span data-stu-id="0fed1-174">Version 0.8.13</span></span>
<span data-ttu-id="0fed1-175">05/12/2017</span><span class="sxs-lookup"><span data-stu-id="0fed1-175">05/12/2017</span></span>

#### <a name="new"></a><span data-ttu-id="0fed1-176">Ny</span><span class="sxs-lookup"><span data-stu-id="0fed1-176">New</span></span>

* <span data-ttu-id="0fed1-177">Lagringsutforskaren felsökning [Guide][2]</span><span class="sxs-lookup"><span data-stu-id="0fed1-177">Storage Explorer Troubleshooting [Guide][2]</span></span>
* <span data-ttu-id="0fed1-178">[Instruktioner] [ 3] ansluta tooan Stack för Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="0fed1-178">[Instructions][3] on connecting tooan Azure Stack subscription</span></span>

#### <a name="fixes"></a><span data-ttu-id="0fed1-179">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="0fed1-179">Fixes</span></span>

* <span data-ttu-id="0fed1-180">Fast: Ladda upp filen hunnit hög orsaka en out-of-minnesfel</span><span class="sxs-lookup"><span data-stu-id="0fed1-180">Fixed: File upload had a high chance of causing an out of memory error</span></span>
* <span data-ttu-id="0fed1-181">Fast: Du kan nu logga in med PIN-kod/smartkort</span><span class="sxs-lookup"><span data-stu-id="0fed1-181">Fixed: You can now sign in with PIN/Smartcard</span></span>
* <span data-ttu-id="0fed1-182">Fast: Öppna i portalen nu fungerar med Azure Kina, Tyskland Azure, Azure som tillhör amerikanska myndigheter och Azure-stacken</span><span class="sxs-lookup"><span data-stu-id="0fed1-182">Fixed: Open in Portal now works with Azure China, Azure Germany, Azure US Government, and Azure Stack</span></span>
* <span data-ttu-id="0fed1-183">Fast: Går inte att överföra en mapp tooa blob-behållare, ”otillåten åtgärd” skulle ibland uppstå fel</span><span class="sxs-lookup"><span data-stu-id="0fed1-183">Fixed: While uploading a folder tooa blob container, an "Illegal operation" error would sometimes occur</span></span>
* <span data-ttu-id="0fed1-184">Fast: Markera alla inaktiverades vid hantering av ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="0fed1-184">Fixed: Select all was disabled while managing snapshots</span></span>
* <span data-ttu-id="0fed1-185">Fast: hello metadata för grundläggande hello-blob kan komma att skrivas över när du har visat hello egenskaper av dess ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="0fed1-185">Fixed: hello metadata of hello base blob might get overwritten after viewing hello properties of its snapshots</span></span>

#### <a name="known-issues"></a><span data-ttu-id="0fed1-186">Kända problem</span><span class="sxs-lookup"><span data-stu-id="0fed1-186">Known Issues</span></span>

* <span data-ttu-id="0fed1-187">Om du väljer hello fel PIN-kod/smartkortcertifikat sedan behöver du toorestart i ordning toohave Lagringsutforskaren Glöm hello beslut</span><span class="sxs-lookup"><span data-stu-id="0fed1-187">If you choose hello wrong PIN/Smartcard certificate then you will need toorestart in order toohave Storage Explorer forget hello decision</span></span>
* <span data-ttu-id="0fed1-188">När du har zoomat in eller ut får hello zoomningsnivån tillfälligt återställa toohello Standardnivå</span><span class="sxs-lookup"><span data-stu-id="0fed1-188">While zoomed in or out, hello zoom level may momentarily reset toohello default level</span></span>
* <span data-ttu-id="0fed1-189">Med mer än 3 grupper av blobbar eller filer överföra i hello samma kan tid orsaka fel</span><span class="sxs-lookup"><span data-stu-id="0fed1-189">Having more than 3 groups of blobs or files uploading at hello same time may cause errors</span></span>
* <span data-ttu-id="0fed1-190">hello konto inställningar panelen kan indikera att du behöver tooreenter autentiseringsuppgifter i ordning toofilter prenumerationer</span><span class="sxs-lookup"><span data-stu-id="0fed1-190">hello account settings panel may show that you need tooreenter credentials in order toofilter subscriptions</span></span>
* <span data-ttu-id="0fed1-191">Byta namn på blobbar (individuellt eller i en bytt namn till blob-behållare) bevaras inte ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="0fed1-191">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="0fed1-192">Alla andra egenskaper och metadata för blobbar, filer och entiteter bevaras vid ett byte av namn.</span><span class="sxs-lookup"><span data-stu-id="0fed1-192">All other properties and metadata for blobs, files and entities are preserved during a rename.</span></span>
* <span data-ttu-id="0fed1-193">Även om Azure-stacken inte stöder filresurser, visas en filresurser nod fortfarande under ett bifogade Stack för Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="0fed1-193">Although Azure Stack doesn't currently support Files Shares, a File Shares node still appears under an attached Azure Stack storage account.</span></span> 
* <span data-ttu-id="0fed1-194">Ubuntu 14.04 installera behov gcc version uppdateras eller uppgraderas – steg tooupgrade finns nedan:</span><span class="sxs-lookup"><span data-stu-id="0fed1-194">Ubuntu 14.04 install needs gcc version updated or upgraded – steps tooupgrade are below:</span></span>

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-0812--0811--0810"></a><span data-ttu-id="0fed1-195">Version 0.8.12 / 0.8.11 / 0.8.10</span><span class="sxs-lookup"><span data-stu-id="0fed1-195">Version 0.8.12 / 0.8.11 / 0.8.10</span></span>
<span data-ttu-id="0fed1-196">04/07/2017</span><span class="sxs-lookup"><span data-stu-id="0fed1-196">04/07/2017</span></span>

#### <a name="new"></a><span data-ttu-id="0fed1-197">Ny</span><span class="sxs-lookup"><span data-stu-id="0fed1-197">New</span></span>

* <span data-ttu-id="0fed1-198">Lagringsutforskaren stängs nu automatiskt när du installerar en uppdatering från hello uppdateringsmeddelande</span><span class="sxs-lookup"><span data-stu-id="0fed1-198">Storage Explorer will now automatically close when you install an update from hello update notification</span></span>
* <span data-ttu-id="0fed1-199">Lokalt Snabbåtkomst ger en förbättrad upplevelse för att arbeta med resurserna används ofta</span><span class="sxs-lookup"><span data-stu-id="0fed1-199">In-place quick access provides an enhanced experience for working with your frequently accessed resources</span></span>
* <span data-ttu-id="0fed1-200">I Redigeraren för hello Blob-behållare, kan du nu se vilken virtuell dator som tillhör en lånade blob</span><span class="sxs-lookup"><span data-stu-id="0fed1-200">In hello Blob Container editor, you can now see which virtual machine a leased blob belongs to</span></span>
* <span data-ttu-id="0fed1-201">Du kan nu komprimera hello vänster panel</span><span class="sxs-lookup"><span data-stu-id="0fed1-201">You can now collapse hello left side panel</span></span>
* <span data-ttu-id="0fed1-202">Identifiering nu körs vid hello samma tid som nedladdning</span><span class="sxs-lookup"><span data-stu-id="0fed1-202">Discovery now runs at hello same time as download</span></span>
* <span data-ttu-id="0fed1-203">Använd statistik i hello Blob-behållaren och filresursen tabell redigerare toosee hello storleken på ditt val eller resurs</span><span class="sxs-lookup"><span data-stu-id="0fed1-203">Use Statistics in hello Blob Container, File Share, and Table editors toosee hello size of your resource or selection</span></span>
* <span data-ttu-id="0fed1-204">Du kan nu logga in tooAzure Active Directory (AAD) baserad Azure Stack-konton.</span><span class="sxs-lookup"><span data-stu-id="0fed1-204">You can now sign-in tooAzure Active Directory (AAD) based Azure Stack accounts.</span></span> 
* <span data-ttu-id="0fed1-205">Du kan nu arkivera Överför filer över 32 MB tooPremium storage-konton</span><span class="sxs-lookup"><span data-stu-id="0fed1-205">You can now upload archive files over 32MB tooPremium storage accounts</span></span>
* <span data-ttu-id="0fed1-206">Förbättrat stöd för hjälpmedel</span><span class="sxs-lookup"><span data-stu-id="0fed1-206">Improved accessibility support</span></span>
* <span data-ttu-id="0fed1-207">Nu kan du lägga till betrodda Base64-kodat X.509-SSL-certifikat genom att gå tooEdit -&gt; SSL-certifikat -&gt; Importera certifikat</span><span class="sxs-lookup"><span data-stu-id="0fed1-207">You can now add trusted Base-64 encoded X.509 SSL certificates by going tooEdit -&gt; SSL Certificates -&gt; Import Certificates</span></span>

#### <a name="fixes"></a><span data-ttu-id="0fed1-208">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="0fed1-208">Fixes</span></span>

* <span data-ttu-id="0fed1-209">Fast: när du har uppdaterat en kontoautentiseringsuppgifter hello trädvyn kan ibland uppdateras inte automatiskt</span><span class="sxs-lookup"><span data-stu-id="0fed1-209">Fixed: after refreshing an account's credentials, hello tree view would sometimes not automatically refresh</span></span>
* <span data-ttu-id="0fed1-210">Fast: Generera en SAS för emulatorn köer och tabeller skulle leda till en ogiltig URL</span><span class="sxs-lookup"><span data-stu-id="0fed1-210">Fixed: generating a SAS for emulator queues and tables would result in an invalid URL</span></span>
* <span data-ttu-id="0fed1-211">Fast: premium storage-konton kan nu utökas när en proxy är aktiverat</span><span class="sxs-lookup"><span data-stu-id="0fed1-211">Fixed: premium storage accounts can now be expanded while a proxy is enabled</span></span>
* <span data-ttu-id="0fed1-212">Fast: hello knappen Tillämpa på hello konton hanteringssidan inte fungerar om du hade 1 eller 0 konton som har valts</span><span class="sxs-lookup"><span data-stu-id="0fed1-212">Fixed: hello apply button on hello accounts management page would not work if you had 1 or 0 accounts selected</span></span>
* <span data-ttu-id="0fed1-213">Fast: överföring av blobbar som kräver konfliktlösningar kan misslyckas - fast i 0.8.11</span><span class="sxs-lookup"><span data-stu-id="0fed1-213">Fixed: uploading blobs that require conflict resolutions may fail - fixed in 0.8.11</span></span> 
* <span data-ttu-id="0fed1-214">Fast: skicka feedback har brutits i 0.8.11 - fast i 0.8.12</span><span class="sxs-lookup"><span data-stu-id="0fed1-214">Fixed: sending feedback was broken in 0.8.11 - fixed in 0.8.12</span></span> 

#### <a name="known-issues"></a><span data-ttu-id="0fed1-215">Kända problem</span><span class="sxs-lookup"><span data-stu-id="0fed1-215">Known Issues</span></span>

* <span data-ttu-id="0fed1-216">När du har uppgraderat too0.8.10 toorefresh måste alla dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0fed1-216">After upgrading too0.8.10, you will need toorefresh all of your credentials.</span></span>
* <span data-ttu-id="0fed1-217">När du har zoomat in eller ut kan tillfälligt hello zoomningsnivån återställa toohello Standardnivå.</span><span class="sxs-lookup"><span data-stu-id="0fed1-217">While zoomed in or out, hello zoom level may momentarily reset toohello default level.</span></span>
* <span data-ttu-id="0fed1-218">Med mer än 3 grupper av blobbar eller filer överföra i hello samma kan tid orsaka fel.</span><span class="sxs-lookup"><span data-stu-id="0fed1-218">Having more than 3 groups of blobs or files uploading at hello same time may cause errors.</span></span>
* <span data-ttu-id="0fed1-219">hello konto inställningar panelen kan indikera att du behöver tooreenter autentiseringsuppgifter i ordning toofilter prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="0fed1-219">hello account settings panel may show that you need tooreenter credentials in order toofilter subscriptions.</span></span>
* <span data-ttu-id="0fed1-220">Byta namn på blobbar (individuellt eller i en bytt namn till blob-behållare) bevaras inte ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="0fed1-220">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="0fed1-221">Alla andra egenskaper och metadata för blobbar, filer och entiteter bevaras vid ett byte av namn.</span><span class="sxs-lookup"><span data-stu-id="0fed1-221">All other properties and metadata for blobs, files and entities are preserved during a rename.</span></span>
* <span data-ttu-id="0fed1-222">Även om Azure-stacken inte stöder filresurser, visas en filresurser nod fortfarande under ett bifogade Stack för Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="0fed1-222">Although Azure Stack doesn't currently support Files Shares, a File Shares node still appears under an attached Azure Stack storage account.</span></span> 
* <span data-ttu-id="0fed1-223">Ubuntu 14.04 installera behov gcc version uppdateras eller uppgraderas – steg tooupgrade finns nedan:</span><span class="sxs-lookup"><span data-stu-id="0fed1-223">Ubuntu 14.04 install needs gcc version updated or upgraded – steps tooupgrade are below:</span></span>

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-089--088"></a><span data-ttu-id="0fed1-224">Version 0.8.9 / 0.8.8</span><span class="sxs-lookup"><span data-stu-id="0fed1-224">Version 0.8.9 / 0.8.8</span></span>
<span data-ttu-id="0fed1-225">02/23/2017</span><span class="sxs-lookup"><span data-stu-id="0fed1-225">02/23/2017</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/R6gonK3cYAc?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/SrRPCm94mfE?ecver=1" frameborder="0" allowfullscreen></iframe>


#### <a name="new"></a><span data-ttu-id="0fed1-226">Ny</span><span class="sxs-lookup"><span data-stu-id="0fed1-226">New</span></span>

* <span data-ttu-id="0fed1-227">Lagringsutforskaren 0.8.9 kommer automatiskt att hämta hello senaste versionen för uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="0fed1-227">Storage Explorer 0.8.9 will automatically download hello latest version for updates.</span></span>
* <span data-ttu-id="0fed1-228">Snabbkorrigering: med hjälp av en portal generera SAS URI tooattach ett lagringskonto skulle resultera i ett fel.</span><span class="sxs-lookup"><span data-stu-id="0fed1-228">Hotfix: using a portal generated SAS URI tooattach a storage account would result in an error.</span></span>
* <span data-ttu-id="0fed1-229">Du kan nu skapa, hantera och främja blob ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="0fed1-229">You can now create, manage, and promote blob snapshots.</span></span>
* <span data-ttu-id="0fed1-230">Du kan nu logga in tooAzure Kina, Azure Tyskland och Azure som tillhör amerikanska myndigheter konton.</span><span class="sxs-lookup"><span data-stu-id="0fed1-230">You can now sign in tooAzure China, Azure Germany, and Azure US Government accounts.</span></span>
* <span data-ttu-id="0fed1-231">Du kan nu ändra hello zoomningsnivån.</span><span class="sxs-lookup"><span data-stu-id="0fed1-231">You can now change hello zoom level.</span></span> <span data-ttu-id="0fed1-232">Använd hello alternativen i hello Visa-menyn tooZoom i Zooma ut och Återställ Zoom.</span><span class="sxs-lookup"><span data-stu-id="0fed1-232">Use hello options in hello View menu tooZoom In, Zoom Out, and Reset Zoom.</span></span>
* <span data-ttu-id="0fed1-233">Unicode-tecken stöds nu i Användarmetadata för blobbar och filer.</span><span class="sxs-lookup"><span data-stu-id="0fed1-233">Unicode characters are now supported in user metadata for blobs and files.</span></span>
* <span data-ttu-id="0fed1-234">Hjälpmedelsförbättringar.</span><span class="sxs-lookup"><span data-stu-id="0fed1-234">Accessibility improvements.</span></span>
* <span data-ttu-id="0fed1-235">hello nästa version viktig information kan visas från hello uppdateringsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="0fed1-235">hello next version's release notes can be viewed from hello update notification.</span></span> <span data-ttu-id="0fed1-236">Du kan också visa hello aktuella viktig information hello Hjälp-menyn.</span><span class="sxs-lookup"><span data-stu-id="0fed1-236">You can also view hello current release notes from hello Help menu.</span></span>

#### <a name="fixes"></a><span data-ttu-id="0fed1-237">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="0fed1-237">Fixes</span></span>

* <span data-ttu-id="0fed1-238">Fast: hello versionsnumret nu visas korrekt på Kontrollpanelen i Windows</span><span class="sxs-lookup"><span data-stu-id="0fed1-238">Fixed: hello version number is now correctly displayed in Control Panel on Windows</span></span>
* <span data-ttu-id="0fed1-239">Fast: sökning är inte längre begränsade too50, 000 noder</span><span class="sxs-lookup"><span data-stu-id="0fed1-239">Fixed: search is no longer limited too50,000 nodes</span></span>
* <span data-ttu-id="0fed1-240">Fast: Överför tooa filresurs roterade alltid hello målkatalogen inte redan finns</span><span class="sxs-lookup"><span data-stu-id="0fed1-240">Fixed: upload tooa file share spun forever if hello destination directory did not already exist</span></span>
* <span data-ttu-id="0fed1-241">Fast: förbättrad stabilitet för lång överföring</span><span class="sxs-lookup"><span data-stu-id="0fed1-241">Fixed: improved stability for long uploads and downloads</span></span>

#### <a name="known-issues"></a><span data-ttu-id="0fed1-242">Kända problem</span><span class="sxs-lookup"><span data-stu-id="0fed1-242">Known Issues</span></span>

* <span data-ttu-id="0fed1-243">När du har zoomat in eller ut kan tillfälligt hello zoomningsnivån återställa toohello Standardnivå.</span><span class="sxs-lookup"><span data-stu-id="0fed1-243">While zoomed in or out, hello zoom level may momentarily reset toohello default level.</span></span>
* <span data-ttu-id="0fed1-244">Snabbåtkomst fungerar bara med prenumerationen baserat objekt.</span><span class="sxs-lookup"><span data-stu-id="0fed1-244">Quick Access only works with subscription based items.</span></span> <span data-ttu-id="0fed1-245">Lokala resurser eller resurser som är anslutna via en nyckel eller SAS-token stöds inte i den här versionen.</span><span class="sxs-lookup"><span data-stu-id="0fed1-245">Local resources or resources attached via key or SAS token are not supported in this release.</span></span>
* <span data-ttu-id="0fed1-246">Det kan ta några sekunder toonavigate toohello målresurs, beroende på hur många resurser som du har Snabbåtkomst.</span><span class="sxs-lookup"><span data-stu-id="0fed1-246">It may take Quick Access a few seconds toonavigate toohello target resource, depending on how many resources you have.</span></span>
* <span data-ttu-id="0fed1-247">Med mer än 3 grupper av blobbar eller filer överföra i hello samma kan tid orsaka fel.</span><span class="sxs-lookup"><span data-stu-id="0fed1-247">Having more than 3 groups of blobs or files uploading at hello same time may cause errors.</span></span>

<span data-ttu-id="0fed1-248">12/16/2016</span><span class="sxs-lookup"><span data-stu-id="0fed1-248">12/16/2016</span></span>
### <a name="version-087"></a><span data-ttu-id="0fed1-249">Version 0.8.7</span><span class="sxs-lookup"><span data-stu-id="0fed1-249">Version 0.8.7</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/Me4Y4jxoer8?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="0fed1-250">Ny</span><span class="sxs-lookup"><span data-stu-id="0fed1-250">New</span></span>

* <span data-ttu-id="0fed1-251">Du kan välja hur tooresolve konflikter hello början av en uppdatering, hämta eller kopiera session i hello aktiviteter fönster</span><span class="sxs-lookup"><span data-stu-id="0fed1-251">You can choose how tooresolve conflicts at hello beginning of an update, download or copy session in hello Activities window</span></span>
* <span data-ttu-id="0fed1-252">Hovra över en fliken toosee hello fullständig sökväg till hello lagringsresurs</span><span class="sxs-lookup"><span data-stu-id="0fed1-252">Hover over a tab toosee hello full path of hello storage resource</span></span>
* <span data-ttu-id="0fed1-253">När du klickar på en flik, synkronisering och dess plats i navigeringsfönstret för hello vänster</span><span class="sxs-lookup"><span data-stu-id="0fed1-253">When you click on a tab, it synchronizes with its location in hello left side navigation pane</span></span>

#### <a name="fixes"></a><span data-ttu-id="0fed1-254">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="0fed1-254">Fixes</span></span>

* <span data-ttu-id="0fed1-255">Fast: Lagringsutforskaren är nu ett betrott app på Mac</span><span class="sxs-lookup"><span data-stu-id="0fed1-255">Fixed: Storage Explorer is now a trusted app on Mac</span></span>
* <span data-ttu-id="0fed1-256">Fast: Ubuntu 14.04 igen stöds</span><span class="sxs-lookup"><span data-stu-id="0fed1-256">Fixed: Ubuntu 14.04 is again supported</span></span>
* <span data-ttu-id="0fed1-257">Fast: Ibland hello Lägg till konto UI blinkar när inläsning av prenumerationer</span><span class="sxs-lookup"><span data-stu-id="0fed1-257">Fixed: Sometimes hello add account UI flashes when loading subscriptions</span></span>
* <span data-ttu-id="0fed1-258">Fast: Ibland inte alla lagringsresurser ingick i navigeringsfönstret för hello vänster</span><span class="sxs-lookup"><span data-stu-id="0fed1-258">Fixed: Sometimes not all storage resources were listed in hello left side navigation pane</span></span>
* <span data-ttu-id="0fed1-259">Fast: hello åtgärdsfönstret ibland visas tom åtgärder</span><span class="sxs-lookup"><span data-stu-id="0fed1-259">Fixed: hello action pane sometimes displayed empty actions</span></span>
* <span data-ttu-id="0fed1-260">Fast: hello fönsterstorlek från hello senast stängdes session nu sparas</span><span class="sxs-lookup"><span data-stu-id="0fed1-260">Fixed: hello window size from hello last closed session is now retained</span></span>
* <span data-ttu-id="0fed1-261">Fast: Du kan öppna flera flikar för hello samma resurs med hello snabbmenyn</span><span class="sxs-lookup"><span data-stu-id="0fed1-261">Fixed: You can open multiple tabs for hello same resource using hello context menu</span></span>

#### <a name="known-issues"></a><span data-ttu-id="0fed1-262">Kända problem</span><span class="sxs-lookup"><span data-stu-id="0fed1-262">Known Issues</span></span>

* <span data-ttu-id="0fed1-263">Snabbåtkomst fungerar bara med prenumerationen baserat objekt.</span><span class="sxs-lookup"><span data-stu-id="0fed1-263">Quick Access only works with subscription based items.</span></span> <span data-ttu-id="0fed1-264">Lokala resurser eller resurser som är anslutna via en nyckel eller SAS-token stöds inte i den här versionen</span><span class="sxs-lookup"><span data-stu-id="0fed1-264">Local resources or resources attached via key or SAS token are not supported in this release</span></span>
* <span data-ttu-id="0fed1-265">Det kan ta Snabbåtkomst några sekunder toonavigate toohello målresurs, beroende på hur många resurser som du har</span><span class="sxs-lookup"><span data-stu-id="0fed1-265">It may take Quick Access a few seconds toonavigate toohello target resource, depending on how many resources you have</span></span>
* <span data-ttu-id="0fed1-266">Med mer än 3 grupper av blobbar eller filer överföra i hello samma kan tid orsaka fel</span><span class="sxs-lookup"><span data-stu-id="0fed1-266">Having more than 3 groups of blobs or files uploading at hello same time may cause errors</span></span>
* <span data-ttu-id="0fed1-267">Referenser för sökning efter det att söka i ungefär 50 000 noder - prestanda kan påverkas eller kan orsaka undantag</span><span class="sxs-lookup"><span data-stu-id="0fed1-267">Search handles searching across roughly 50,000 nodes - after this, performance may be impacted or may cause unhandled exception</span></span>
* <span data-ttu-id="0fed1-268">För hello första gången du använder hello Lagringsutforskaren på macOS, kan det finnas flera prompter som ber om användarens behörighet tooaccess nyckelringar.</span><span class="sxs-lookup"><span data-stu-id="0fed1-268">For hello first time using hello Storage Explorer on macOS, you might see multiple prompts asking for user's permission tooaccess keychain.</span></span> <span data-ttu-id="0fed1-269">Vi rekommenderar att du väljer Tillåt alltid så hello prompten inte visas igen</span><span class="sxs-lookup"><span data-stu-id="0fed1-269">We suggest you select Always Allow so hello prompt won't show up again</span></span>

<span data-ttu-id="0fed1-270">11/18/2016</span><span class="sxs-lookup"><span data-stu-id="0fed1-270">11/18/2016</span></span>
### <a name="version-086"></a><span data-ttu-id="0fed1-271">Version 0.8.6</span><span class="sxs-lookup"><span data-stu-id="0fed1-271">Version 0.8.6</span></span>

#### <a name="new"></a><span data-ttu-id="0fed1-272">Ny</span><span class="sxs-lookup"><span data-stu-id="0fed1-272">New</span></span>

* <span data-ttu-id="0fed1-273">Du kan nu PIN-kod används oftast services toohello Snabbåtkomst för enkel navigering</span><span class="sxs-lookup"><span data-stu-id="0fed1-273">You can now pin most frequently used services toohello Quick Access for easy navigation</span></span>
* <span data-ttu-id="0fed1-274">Nu kan du öppna flera redigerare på olika flikar.</span><span class="sxs-lookup"><span data-stu-id="0fed1-274">You can now open multiple editors in different tabs.</span></span> <span data-ttu-id="0fed1-275">Enkelklickning tooopen tillfälliga fliken. Dubbelklicka på tooopen en permanent flik. Du kan också klicka på hello tillfälliga fliken toomake den permanenta fliken</span><span class="sxs-lookup"><span data-stu-id="0fed1-275">Single click tooopen a temporary tab; double click tooopen a permanent tab. You can also click on hello temporary tab toomake it a permanent tab</span></span>
* <span data-ttu-id="0fed1-276">Vi har gjort märkbar prestanda och stabilitetsförbättringar för överföring och hämtning, särskilt för stora filer på snabb datorer</span><span class="sxs-lookup"><span data-stu-id="0fed1-276">We have made noticeable performance and stability improvements for uploads and downloads, especially for large files on fast machines</span></span>
* <span data-ttu-id="0fed1-277">Nu kan skapa tomma ”virtuell” mappar i blob-behållare</span><span class="sxs-lookup"><span data-stu-id="0fed1-277">Empty "virtual" folders can now be created in blob containers</span></span>
* <span data-ttu-id="0fed1-278">Vi har nytt introducerades bred sökning med vår nya förbättrad sökningen så att du nu har två alternativ för att söka efter:</span><span class="sxs-lookup"><span data-stu-id="0fed1-278">We have re-introduced scoped search with our new enhanced substring search, so you now have two options for searching:</span></span> 
    * <span data-ttu-id="0fed1-279">Globala Sök - bara ange en sökterm i hello Sök textruta</span><span class="sxs-lookup"><span data-stu-id="0fed1-279">Global search - just enter a search term into hello search textbox</span></span>
    * <span data-ttu-id="0fed1-280">Bred sökning - Klicka på hello förstoringsglas ikonen nästa tooa nod, och sedan lägga till en sökning termen toohello end hello sökväg eller högerklicka och välj ”Sök från hit”</span><span class="sxs-lookup"><span data-stu-id="0fed1-280">Scoped search - click hello magnifying glass icon next tooa node, then add a search term toohello end of hello path, or right-click and select "Search from Here"</span></span>
* <span data-ttu-id="0fed1-281">Vi har lagt till olika teman: ljus (standard), mörkt, hög kontrast svart och hög kontrast vit.</span><span class="sxs-lookup"><span data-stu-id="0fed1-281">We have added various themes: Light (default), Dark, High Contrast Black, and High Contrast White.</span></span> <span data-ttu-id="0fed1-282">Gå tooEdit -&gt; teman toochange önskemål teman</span><span class="sxs-lookup"><span data-stu-id="0fed1-282">Go tooEdit -&gt; Themes toochange your theming preference</span></span>
* <span data-ttu-id="0fed1-283">Du kan ändra Blob och filegenskaper</span><span class="sxs-lookup"><span data-stu-id="0fed1-283">You can modify Blob and file properties</span></span>
* <span data-ttu-id="0fed1-284">Vi stöder nu kodade (base64) och kodat Kömeddelanden</span><span class="sxs-lookup"><span data-stu-id="0fed1-284">We now support encoded (base64) and unencoded queue messages</span></span>
* <span data-ttu-id="0fed1-285">På Linux för, ett 64-bitars operativsystem är nu krävs.</span><span class="sxs-lookup"><span data-stu-id="0fed1-285">On Linux, a 64-bit OS is now required.</span></span> <span data-ttu-id="0fed1-286">Den här versionen stöder vi endast 64-bitars Ubuntu 16.04.1 LTS</span><span class="sxs-lookup"><span data-stu-id="0fed1-286">For this release we only support 64-bit Ubuntu 16.04.1 LTS</span></span>
* <span data-ttu-id="0fed1-287">Vi har uppdaterat vår logotypen!</span><span class="sxs-lookup"><span data-stu-id="0fed1-287">We have updated our logo!</span></span>

#### <a name="fixes"></a><span data-ttu-id="0fed1-288">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="0fed1-288">Fixes</span></span>

* <span data-ttu-id="0fed1-289">Fast: Skärmen låser problem</span><span class="sxs-lookup"><span data-stu-id="0fed1-289">Fixed: Screen freezing problems</span></span>
* <span data-ttu-id="0fed1-290">Fast: Förbättrad säkerhet</span><span class="sxs-lookup"><span data-stu-id="0fed1-290">Fixed: Enhanced security</span></span>
* <span data-ttu-id="0fed1-291">Fast: Ibland dubbla bifogade konton kan visas</span><span class="sxs-lookup"><span data-stu-id="0fed1-291">Fixed: Sometimes duplicate attached accounts could appear</span></span>
* <span data-ttu-id="0fed1-292">Fast: En blob med en odefinierad innehållstyp generera ett undantag</span><span class="sxs-lookup"><span data-stu-id="0fed1-292">Fixed: A blob with an undefined content type could generate an exception</span></span>
* <span data-ttu-id="0fed1-293">Fast: Öppna hello frågan panelen på en tom tabell var inte möjligt</span><span class="sxs-lookup"><span data-stu-id="0fed1-293">Fixed: Opening hello Query Panel on an empty table was not possible</span></span>
* <span data-ttu-id="0fed1-294">Fast: Varierar buggar i sökningen</span><span class="sxs-lookup"><span data-stu-id="0fed1-294">Fixed: Varies bugs in Search</span></span>
* <span data-ttu-id="0fed1-295">Fast: Ökade hello antal resurser som lästs in från 50 too100 när du klickar på ”mer belastning”</span><span class="sxs-lookup"><span data-stu-id="0fed1-295">Fixed: Increased hello number of resources loaded from 50 too100 when clicking "Load More"</span></span>
* <span data-ttu-id="0fed1-296">Fast: Första körningen om ett konto som har loggat in, vi nu välja alla prenumerationer för kontot som standard</span><span class="sxs-lookup"><span data-stu-id="0fed1-296">Fixed: On first run, if an account is signed into, we now select all subscriptions for that account by default</span></span> 

#### <a name="known-issues"></a><span data-ttu-id="0fed1-297">Kända problem</span><span class="sxs-lookup"><span data-stu-id="0fed1-297">Known Issues</span></span>

* <span data-ttu-id="0fed1-298">Den här versionen av hello Lagringsutforskaren körs inte på Ubuntu 14.04</span><span class="sxs-lookup"><span data-stu-id="0fed1-298">This release of hello Storage Explorer does not run on Ubuntu 14.04</span></span>
* <span data-ttu-id="0fed1-299">tooopen flera flikar för hello samma resurs, vill inte kontinuerligt klickar du på hello samma resurs.</span><span class="sxs-lookup"><span data-stu-id="0fed1-299">tooopen multiple tabs for hello same resource, do not continuously click on hello same resource.</span></span> <span data-ttu-id="0fed1-300">Klicka på en annan resurs och sedan gå tillbaka och klicka sedan på hello ursprungliga resurs tooopen den igen i en annan flik</span><span class="sxs-lookup"><span data-stu-id="0fed1-300">Click on another resource and then go back and then click on hello original resource tooopen it again in another tab</span></span> 
* <span data-ttu-id="0fed1-301">Snabbåtkomst fungerar bara med prenumerationen baserat objekt.</span><span class="sxs-lookup"><span data-stu-id="0fed1-301">Quick Access only works with subscription based items.</span></span> <span data-ttu-id="0fed1-302">Lokala resurser eller resurser som är anslutna via en nyckel eller SAS-token stöds inte i den här versionen</span><span class="sxs-lookup"><span data-stu-id="0fed1-302">Local resources or resources attached via key or SAS token are not supported in this release</span></span>
* <span data-ttu-id="0fed1-303">Det kan ta Snabbåtkomst några sekunder toonavigate toohello målresurs, beroende på hur många resurser som du har</span><span class="sxs-lookup"><span data-stu-id="0fed1-303">It may take Quick Access a few seconds toonavigate toohello target resource, depending on how many resources you have</span></span>
* <span data-ttu-id="0fed1-304">Med mer än 3 grupper av blobbar eller filer överföra i hello samma kan tid orsaka fel</span><span class="sxs-lookup"><span data-stu-id="0fed1-304">Having more than 3 groups of blobs or files uploading at hello same time may cause errors</span></span>
* <span data-ttu-id="0fed1-305">Referenser för sökning efter det att söka i ungefär 50 000 noder - prestanda kan påverkas eller kan orsaka undantag</span><span class="sxs-lookup"><span data-stu-id="0fed1-305">Search handles searching across roughly 50,000 nodes - after this, performance may be impacted or may cause unhandled exception</span></span>

<span data-ttu-id="0fed1-306">10/03/2016</span><span class="sxs-lookup"><span data-stu-id="0fed1-306">10/03/2016</span></span>
### <a name="version-085"></a><span data-ttu-id="0fed1-307">Version 0.8.5</span><span class="sxs-lookup"><span data-stu-id="0fed1-307">Version 0.8.5</span></span>

#### <a name="new"></a><span data-ttu-id="0fed1-308">Ny</span><span class="sxs-lookup"><span data-stu-id="0fed1-308">New</span></span>

* <span data-ttu-id="0fed1-309">Kan nu använda Portal-genererade SAS nycklar tooattach tooStorage konton och resurser</span><span class="sxs-lookup"><span data-stu-id="0fed1-309">Can now use Portal-generated SAS keys tooattach tooStorage Accounts and resources</span></span>

#### <a name="fixes"></a><span data-ttu-id="0fed1-310">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="0fed1-310">Fixes</span></span>

* <span data-ttu-id="0fed1-311">Fast: konkurrenstillstånd under sökningen ibland orsakade noder toobecome inte är expanderbara</span><span class="sxs-lookup"><span data-stu-id="0fed1-311">Fixed: race condition during search sometimes caused nodes toobecome non-expandable</span></span>
* <span data-ttu-id="0fed1-312">Fast: ”Använd HTTP” fungerar inte när du ansluter tooStorage konton med kontonamn och nyckel</span><span class="sxs-lookup"><span data-stu-id="0fed1-312">Fixed: "Use HTTP" doesn't work when connecting tooStorage Accounts with account name and key</span></span>
* <span data-ttu-id="0fed1-313">Fast: SAS-nycklar (som är särskilt Portal-genererade) returnerar ett ”avslutande snedstreck” fel</span><span class="sxs-lookup"><span data-stu-id="0fed1-313">Fixed: SAS keys (specially Portal-generated ones) return a "trailing slash" error</span></span>
* <span data-ttu-id="0fed1-314">Fast: importera tabell problem</span><span class="sxs-lookup"><span data-stu-id="0fed1-314">Fixed: table import issues</span></span>
    * <span data-ttu-id="0fed1-315">Ibland har partitionsnyckel och radnyckel återförts</span><span class="sxs-lookup"><span data-stu-id="0fed1-315">Sometimes partition key and row key were reversed</span></span>
    * <span data-ttu-id="0fed1-316">Det går inte tooread ”null” partitionsnycklar</span><span class="sxs-lookup"><span data-stu-id="0fed1-316">Unable tooread "null" Partition Keys</span></span>

#### <a name="known-issues"></a><span data-ttu-id="0fed1-317">Kända problem</span><span class="sxs-lookup"><span data-stu-id="0fed1-317">Known Issues</span></span>

* <span data-ttu-id="0fed1-318">Referenser för sökning efter det att söka i ungefär 50 000 noder - kan prestanda påverkas</span><span class="sxs-lookup"><span data-stu-id="0fed1-318">Search handles searching across roughly 50,000 nodes - after this, performance may be impacted</span></span>
* <span data-ttu-id="0fed1-319">Azure-stacken stöder inte filer, så försök tooexpand filer visas ett fel</span><span class="sxs-lookup"><span data-stu-id="0fed1-319">Azure Stack doesn't currently support Files, so trying tooexpand Files will show an error</span></span>

<span data-ttu-id="0fed1-320">09/12/2016</span><span class="sxs-lookup"><span data-stu-id="0fed1-320">09/12/2016</span></span>
### <a name="version-084"></a><span data-ttu-id="0fed1-321">Version 0.8.4</span><span class="sxs-lookup"><span data-stu-id="0fed1-321">Version 0.8.4</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/cr5tOGyGrIQ?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="0fed1-322">Ny</span><span class="sxs-lookup"><span data-stu-id="0fed1-322">New</span></span>

* <span data-ttu-id="0fed1-323">Generera Direktlänkar toostorage konton, behållare, köer, tabeller, eller filresurser för delning och enkel åtkomst till resurser för tooyour - Windows och Mac OS x-stöd</span><span class="sxs-lookup"><span data-stu-id="0fed1-323">Generate direct links toostorage accounts, containers, queues, tables, or file shares for sharing and easy access tooyour resources - Windows and Mac OS support</span></span>
* <span data-ttu-id="0fed1-324">Sök efter din blob-behållare, tabeller, köer, filresurser eller storage-konton från hello sökrutan</span><span class="sxs-lookup"><span data-stu-id="0fed1-324">Search for your blob containers, tables, queues, file shares, or storage accounts from hello search box</span></span>
* <span data-ttu-id="0fed1-325">Nu kan du gruppera-satser i hello tabell Frågebyggaren</span><span class="sxs-lookup"><span data-stu-id="0fed1-325">You can now group clauses in hello table query builder</span></span>
* <span data-ttu-id="0fed1-326">Byt namn på och kopiera och klistra in blob-behållare, filresurser, tabeller, BLOB, blob mappar, filer och kataloger från i SAS-anslutna konton och behållare</span><span class="sxs-lookup"><span data-stu-id="0fed1-326">Rename and copy/paste blob containers, file shares, tables, blobs, blob folders, files and directories from within SAS-attached accounts and containers</span></span>
* <span data-ttu-id="0fed1-327">Byta namn på och kopiera blob-behållare och filresurser bevara nu egenskaper och metadata</span><span class="sxs-lookup"><span data-stu-id="0fed1-327">Renaming and copying blob containers and file shares now preserve properties and metadata</span></span>

#### <a name="fixes"></a><span data-ttu-id="0fed1-328">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="0fed1-328">Fixes</span></span>

* <span data-ttu-id="0fed1-329">Fast: Det går inte att redigera tabellentiteter om de innehåller boolean eller binära egenskaper</span><span class="sxs-lookup"><span data-stu-id="0fed1-329">Fixed: cannot edit table entities if they contain boolean or binary properties</span></span>

#### <a name="known-issues"></a><span data-ttu-id="0fed1-330">Kända problem</span><span class="sxs-lookup"><span data-stu-id="0fed1-330">Known Issues</span></span>

* <span data-ttu-id="0fed1-331">Referenser för sökning efter det att söka i ungefär 50 000 noder - kan prestanda påverkas</span><span class="sxs-lookup"><span data-stu-id="0fed1-331">Search handles searching across roughly 50,000 nodes - after this, performance may be impacted</span></span>

<span data-ttu-id="0fed1-332">08/03/2016</span><span class="sxs-lookup"><span data-stu-id="0fed1-332">08/03/2016</span></span>
### <a name="version-083"></a><span data-ttu-id="0fed1-333">Version 0.8.3</span><span class="sxs-lookup"><span data-stu-id="0fed1-333">Version 0.8.3</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/HeGW-jkSd9Y?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="0fed1-334">Ny</span><span class="sxs-lookup"><span data-stu-id="0fed1-334">New</span></span>

* <span data-ttu-id="0fed1-335">Byt namn på behållare, tabeller, filresurser</span><span class="sxs-lookup"><span data-stu-id="0fed1-335">Rename containers, tables, file shares</span></span>
* <span data-ttu-id="0fed1-336">Bättre frågan builder upplevelse</span><span class="sxs-lookup"><span data-stu-id="0fed1-336">Improved Query builder experience</span></span>
* <span data-ttu-id="0fed1-337">Möjlighet toosave och Läs in frågor</span><span class="sxs-lookup"><span data-stu-id="0fed1-337">Ability toosave and load queries</span></span>
* <span data-ttu-id="0fed1-338">Direkta länkar toostorage konton eller behållare, köer, tabeller eller filresurser för delning och lätt att komma åt dina resurser (endast för Windows - macOS stöder kommer snart!)</span><span class="sxs-lookup"><span data-stu-id="0fed1-338">Direct links toostorage accounts or containers, queues, tables, or file shares for sharing and easily accessing your resources (Windows-only - macOS support coming soon!)</span></span>
* <span data-ttu-id="0fed1-339">Möjlighet toomanage och konfigurera CORS-regler</span><span class="sxs-lookup"><span data-stu-id="0fed1-339">Ability toomanage and configure CORS rules</span></span>

#### <a name="fixes"></a><span data-ttu-id="0fed1-340">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="0fed1-340">Fixes</span></span>

* <span data-ttu-id="0fed1-341">Fast: Microsoft Accounts kräver omautentisering var 8 – 12: e timme</span><span class="sxs-lookup"><span data-stu-id="0fed1-341">Fixed: Microsoft Accounts require re-authentication every 8-12 hours</span></span>

#### <a name="known-issues"></a><span data-ttu-id="0fed1-342">Kända problem</span><span class="sxs-lookup"><span data-stu-id="0fed1-342">Known Issues</span></span>

* <span data-ttu-id="0fed1-343">Ibland hello Användargränssnittet verka fryst – maximerar fönstret hello hjälper dig att lösa problemet</span><span class="sxs-lookup"><span data-stu-id="0fed1-343">Sometimes hello UI might appear frozen - maximizing hello window helps resolve this issue</span></span>
* <span data-ttu-id="0fed1-344">installera macOS kan kräva förhöjd behörighet</span><span class="sxs-lookup"><span data-stu-id="0fed1-344">macOS install may require elevated permissions</span></span>
* <span data-ttu-id="0fed1-345">Konto inställningar panelen kan indikera att du behöver tooreenter autentiseringsuppgifter i ordning toofilter prenumerationer</span><span class="sxs-lookup"><span data-stu-id="0fed1-345">Account settings panel may show that you need tooreenter credentials in order toofilter subscriptions</span></span>
* <span data-ttu-id="0fed1-346">Byta namn på filresurser, blob-behållare och tabeller bevaras inte metadata eller andra egenskaper hello behållare, till exempel filresurskvoten, offentliga åtkomstnivå eller åtkomstprinciper</span><span class="sxs-lookup"><span data-stu-id="0fed1-346">Renaming file shares, blob containers, and tables does not preserve metadata or other properties on hello container, such as file share quota, public access level or access policies</span></span>
* <span data-ttu-id="0fed1-347">Byta namn på blobbar (individuellt eller i en bytt namn till blob-behållare) bevaras inte ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="0fed1-347">Renaming blobs (individually or inside a renamed blob container) does not preserve snapshots.</span></span> <span data-ttu-id="0fed1-348">Alla andra egenskaper och metadata för blobbar, filer och entiteter bevaras vid ett byte av namn</span><span class="sxs-lookup"><span data-stu-id="0fed1-348">All other properties and metadata for blobs, files and entities are preserved during a rename</span></span>
* <span data-ttu-id="0fed1-349">Kopiera eller byta namn på resurser fungerar inte i SAS-anslutna konton</span><span class="sxs-lookup"><span data-stu-id="0fed1-349">Copying or renaming resources does not work within SAS-attached accounts</span></span>

<span data-ttu-id="0fed1-350">07/07/2016</span><span class="sxs-lookup"><span data-stu-id="0fed1-350">07/07/2016</span></span>
### <a name="version-082"></a><span data-ttu-id="0fed1-351">Version 0.8.2</span><span class="sxs-lookup"><span data-stu-id="0fed1-351">Version 0.8.2</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/nYgKbRUNYZA?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="0fed1-352">Ny</span><span class="sxs-lookup"><span data-stu-id="0fed1-352">New</span></span>

* <span data-ttu-id="0fed1-353">Storage-konton är grupperade efter prenumerationer; utveckling lagring och resurser som är anslutna via SAS eller nyckeln visas under noden (lokala och bifogad)</span><span class="sxs-lookup"><span data-stu-id="0fed1-353">Storage Accounts are grouped by subscriptions; development storage and resources attached via key or SAS are shown under (Local and Attached) node</span></span>
* <span data-ttu-id="0fed1-354">Logga ut från konton ”Azure kontoinställningar” Kontrollpanelen</span><span class="sxs-lookup"><span data-stu-id="0fed1-354">Sign off from accounts in "Azure Account Settings" panel</span></span>
* <span data-ttu-id="0fed1-355">Konfigurera proxy-inställningar tooenable och hantera inloggning</span><span class="sxs-lookup"><span data-stu-id="0fed1-355">Configure proxy settings tooenable and manage sign-in</span></span>
* <span data-ttu-id="0fed1-356">Skapa och ta bort blob-lån</span><span class="sxs-lookup"><span data-stu-id="0fed1-356">Create and break blob leases</span></span>
* <span data-ttu-id="0fed1-357">Öppna blob-behållare, köer, tabeller och filer med enkelklickning</span><span class="sxs-lookup"><span data-stu-id="0fed1-357">Open blob containers, queues, tables, and files with single-click</span></span>

#### <a name="fixes"></a><span data-ttu-id="0fed1-358">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="0fed1-358">Fixes</span></span>

* <span data-ttu-id="0fed1-359">Fast: Kömeddelanden infogas med .NET eller Java-bibliotek är inte korrekt avkoda från base64</span><span class="sxs-lookup"><span data-stu-id="0fed1-359">Fixed: queue messages inserted with .NET or Java libraries are not properly decoded from base64</span></span>
* <span data-ttu-id="0fed1-360">Fast: $metrics tabeller visas inte för Blob Storage-konton</span><span class="sxs-lookup"><span data-stu-id="0fed1-360">Fixed: $metrics tables are not shown for Blob Storage accounts</span></span>
* <span data-ttu-id="0fed1-361">Fast: tabeller noden fungerar inte för lokal lagring för (utveckling)</span><span class="sxs-lookup"><span data-stu-id="0fed1-361">Fixed: tables node does not work for local (Development) storage</span></span>

#### <a name="known-issues"></a><span data-ttu-id="0fed1-362">Kända problem</span><span class="sxs-lookup"><span data-stu-id="0fed1-362">Known Issues</span></span>

* <span data-ttu-id="0fed1-363">installera macOS kan kräva förhöjd behörighet</span><span class="sxs-lookup"><span data-stu-id="0fed1-363">macOS install may require elevated permissions</span></span>

<span data-ttu-id="0fed1-364">06/15/2016</span><span class="sxs-lookup"><span data-stu-id="0fed1-364">06/15/2016</span></span>
### <a name="version-080"></a><span data-ttu-id="0fed1-365">Version 0.8.0</span><span class="sxs-lookup"><span data-stu-id="0fed1-365">Version 0.8.0</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/ycfQhKztSIY?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/k4_kOUCZ0WA?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/3zEXJcGdl_k?ecver=1" frameborder="0" allowfullscreen></iframe>

#### <a name="new"></a><span data-ttu-id="0fed1-366">Ny</span><span class="sxs-lookup"><span data-stu-id="0fed1-366">New</span></span>

* <span data-ttu-id="0fed1-367">Stöd för resursen: visa, överföra, hämta, kopiera filer och kataloger, SAS-URI: er (skapa och ansluta)</span><span class="sxs-lookup"><span data-stu-id="0fed1-367">File share support: viewing, uploading, downloading, copying files and directories, SAS URIs (create and connect)</span></span>
* <span data-ttu-id="0fed1-368">Förbättrad användarupplevelse för att ansluta tooStorage med SAS URI: er eller nycklar</span><span class="sxs-lookup"><span data-stu-id="0fed1-368">Improved user experience for connecting tooStorage with SAS URIs or account keys</span></span>
* <span data-ttu-id="0fed1-369">Exportera tabell frågeresultat</span><span class="sxs-lookup"><span data-stu-id="0fed1-369">Export table query results</span></span>
* <span data-ttu-id="0fed1-370">Tabell kolumnordning och anpassning</span><span class="sxs-lookup"><span data-stu-id="0fed1-370">Table column reordering and customization</span></span>
* <span data-ttu-id="0fed1-371">Visa $logs blob-behållare och $metrics tabeller för Storage-konton med aktiverad</span><span class="sxs-lookup"><span data-stu-id="0fed1-371">Viewing $logs blob containers and $metrics tables for Storage Accounts with enabled metrics</span></span>
* <span data-ttu-id="0fed1-372">Förbättrad exportera och importera beteende, innehåller nu egenskapsvärdetypen</span><span class="sxs-lookup"><span data-stu-id="0fed1-372">Improved export and import behavior, now includes property value type</span></span>

#### <a name="fixes"></a><span data-ttu-id="0fed1-373">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="0fed1-373">Fixes</span></span>

* <span data-ttu-id="0fed1-374">Fast: överföra eller hämta stora blobbar kan resultera i ofullständiga överföringar/hämtningar</span><span class="sxs-lookup"><span data-stu-id="0fed1-374">Fixed: uploading or downloading large blobs can result in incomplete uploads/downloads</span></span>
* <span data-ttu-id="0fed1-375">Fast: redigera, lägga till eller importera en entitet med ett numeriskt värde (”1”) konverteras den toodouble</span><span class="sxs-lookup"><span data-stu-id="0fed1-375">Fixed: editing, adding, or importing an entity with a numeric string value ("1") will convert it toodouble</span></span>
* <span data-ttu-id="0fed1-376">Fast: Tooexpand hello tabell nod i hello lokala utvecklingsmiljö</span><span class="sxs-lookup"><span data-stu-id="0fed1-376">Fixed: Unable tooexpand hello table node in hello local development environment</span></span>

#### <a name="known-issues"></a><span data-ttu-id="0fed1-377">Kända problem</span><span class="sxs-lookup"><span data-stu-id="0fed1-377">Known Issues</span></span>

* <span data-ttu-id="0fed1-378">$metrics tabeller är inte synliga för Blob Storage-konton</span><span class="sxs-lookup"><span data-stu-id="0fed1-378">$metrics tables are not visible for Blob Storage accounts</span></span>
* <span data-ttu-id="0fed1-379">Kömeddelanden som lagts till via programmering kan inte visas korrekt om hälsningsmeddelande kodas med Base64-kodning</span><span class="sxs-lookup"><span data-stu-id="0fed1-379">Queue messages added programmatically may not be displayed correctly if hello messages are encoded using Base64 encoding</span></span>

<span data-ttu-id="0fed1-380">05/17/2016</span><span class="sxs-lookup"><span data-stu-id="0fed1-380">05/17/2016</span></span>
### <a name="version-07201605090"></a><span data-ttu-id="0fed1-381">Version 0.7.20160509.0</span><span class="sxs-lookup"><span data-stu-id="0fed1-381">Version 0.7.20160509.0</span></span>

#### <a name="new"></a><span data-ttu-id="0fed1-382">Ny</span><span class="sxs-lookup"><span data-stu-id="0fed1-382">New</span></span>

* <span data-ttu-id="0fed1-383">Bättre felhantering för appen kraschar</span><span class="sxs-lookup"><span data-stu-id="0fed1-383">Better error handling for app crashes</span></span>

#### <a name="fixes"></a><span data-ttu-id="0fed1-384">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="0fed1-384">Fixes</span></span>

* <span data-ttu-id="0fed1-385">Fast bugg där Informationsfältet meddelanden ibland inte visas när behövde inloggningsuppgifter</span><span class="sxs-lookup"><span data-stu-id="0fed1-385">Fixed bug where InfoBar messages sometimes don't show up when sign-in credentials were required</span></span>

#### <a name="known-issues"></a><span data-ttu-id="0fed1-386">Kända problem</span><span class="sxs-lookup"><span data-stu-id="0fed1-386">Known Issues</span></span>

* <span data-ttu-id="0fed1-387">Tabeller: Lägga till, redigera eller importera en entitet som har en egenskap med ett ambiguously numeriska värde, till exempel ”1” eller ”1.0”, och hello användaren försöker toosend det som ett `Edm.String`, hello värdet kommer tillbaka via hello klient API som en Edm.Double</span><span class="sxs-lookup"><span data-stu-id="0fed1-387">Tables: Adding, editing, or importing an entity that has a property with an ambiguously numeric value, such as "1" or "1.0", and hello user tries toosend it as an `Edm.String`, hello value will come back through hello client API as an Edm.Double</span></span>

<span data-ttu-id="0fed1-388">03/31/2016</span><span class="sxs-lookup"><span data-stu-id="0fed1-388">03/31/2016</span></span>

### <a name="version-07201603250"></a><span data-ttu-id="0fed1-389">Version 0.7.20160325.0</span><span class="sxs-lookup"><span data-stu-id="0fed1-389">Version 0.7.20160325.0</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/imbgBRHX65A?ecver=1" frameborder="0" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/ceX-P8XZ-s8?ecver=1" frameborder="0" allowfullscreen></iframe>


#### <a name="new"></a><span data-ttu-id="0fed1-390">Ny</span><span class="sxs-lookup"><span data-stu-id="0fed1-390">New</span></span>

* <span data-ttu-id="0fed1-391">Tabell stöd: visa, frågor, exportera, importera och CRUD-åtgärder för entiteter</span><span class="sxs-lookup"><span data-stu-id="0fed1-391">Table support: viewing, querying, export, import, and CRUD operations for entities</span></span>
* <span data-ttu-id="0fed1-392">Kö stöd: visa, lägga till dequeueing meddelanden</span><span class="sxs-lookup"><span data-stu-id="0fed1-392">Queue support: viewing, adding, dequeueing messages</span></span>
* <span data-ttu-id="0fed1-393">Generera SAS URI: er för Storage-konton</span><span class="sxs-lookup"><span data-stu-id="0fed1-393">Generating SAS URIs for Storage Accounts</span></span>
* <span data-ttu-id="0fed1-394">Ansluta tooStorage konton med SAS URI: er</span><span class="sxs-lookup"><span data-stu-id="0fed1-394">Connecting tooStorage Accounts with SAS URIs</span></span>
* <span data-ttu-id="0fed1-395">Meddelanden om uppdateringar för framtida uppdateringar tooStorage Explorer</span><span class="sxs-lookup"><span data-stu-id="0fed1-395">Update notifications for future updates tooStorage Explorer</span></span>
* <span data-ttu-id="0fed1-396">Uppdaterade känsla och utseende</span><span class="sxs-lookup"><span data-stu-id="0fed1-396">Updated look and feel</span></span>

#### <a name="fixes"></a><span data-ttu-id="0fed1-397">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="0fed1-397">Fixes</span></span>

* <span data-ttu-id="0fed1-398">Förbättringar av prestanda och tillförlitlighet</span><span class="sxs-lookup"><span data-stu-id="0fed1-398">Performance and reliability improvements</span></span>

### <a name="known-issues-amp-mitigations"></a><span data-ttu-id="0fed1-399">Kända problem &amp; åtgärder</span><span class="sxs-lookup"><span data-stu-id="0fed1-399">Known Issues &amp; Mitigations</span></span>

* <span data-ttu-id="0fed1-400">Hämta stora blob filer fungerar inte - vi rekommenderar att du använder AzCopy medan vi löser problemet</span><span class="sxs-lookup"><span data-stu-id="0fed1-400">Download of large blob files does not work correctly - we recommend using AzCopy while we address this issue</span></span> 
* <span data-ttu-id="0fed1-401">Autentiseringsuppgifter att inte hämta och cachelagras om hello arbetsmapp hittas inte eller kan inte skrivas till</span><span class="sxs-lookup"><span data-stu-id="0fed1-401">Account credentials will not be retrieved nor cached if hello home folder cannot be found or cannot be written to</span></span>
* <span data-ttu-id="0fed1-402">Om vi lägger till, redigera eller importera en entitet som har en egenskap med ett ambiguously numeriska värde, till exempel ”1” eller ”1.0”, och hello användare försöker toosend det som ett `Edm.String`, hello värdet kommer tillbaka via hello klient API som en Edm.Double</span><span class="sxs-lookup"><span data-stu-id="0fed1-402">If we are adding, editing, or importing an entity that has a property with an ambiguously numeric value, such as "1" or "1.0", and hello user tries toosend it as an `Edm.String`, hello value will come back through hello client API as an Edm.Double</span></span>
* <span data-ttu-id="0fed1-403">När du importerar CSV-filer med flera rader poster kan hello data hämta hackas eller kodade</span><span class="sxs-lookup"><span data-stu-id="0fed1-403">When importing CSV files with multiline records, hello data may get chopped or scrambled</span></span>

<span data-ttu-id="0fed1-404">02/03/2016</span><span class="sxs-lookup"><span data-stu-id="0fed1-404">02/03/2016</span></span>

### <a name="version-07201601291"></a><span data-ttu-id="0fed1-405">Version 0.7.20160129.1</span><span class="sxs-lookup"><span data-stu-id="0fed1-405">Version 0.7.20160129.1</span></span>

#### <a name="fixes"></a><span data-ttu-id="0fed1-406">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="0fed1-406">Fixes</span></span>

* <span data-ttu-id="0fed1-407">Förbättrad prestanda när du laddar upp, hämta och kopiera BLOB</span><span class="sxs-lookup"><span data-stu-id="0fed1-407">Improved overall performance when uploading, downloading and copying blobs</span></span>

<span data-ttu-id="0fed1-408">01/14/2016</span><span class="sxs-lookup"><span data-stu-id="0fed1-408">01/14/2016</span></span>

### <a name="version-07201601050"></a><span data-ttu-id="0fed1-409">Version 0.7.20160105.0</span><span class="sxs-lookup"><span data-stu-id="0fed1-409">Version 0.7.20160105.0</span></span>

#### <a name="new"></a><span data-ttu-id="0fed1-410">Ny</span><span class="sxs-lookup"><span data-stu-id="0fed1-410">New</span></span>

* <span data-ttu-id="0fed1-411">Linux-support (paritet funktioner tooOSX)</span><span class="sxs-lookup"><span data-stu-id="0fed1-411">Linux support (parity features tooOSX)</span></span>
* <span data-ttu-id="0fed1-412">Lägga till blob-behållare med delad åtkomst signaturer (SAS)-nyckel</span><span class="sxs-lookup"><span data-stu-id="0fed1-412">Add blob containers with Shared Access Signatures (SAS) key</span></span>
* <span data-ttu-id="0fed1-413">Lägg till Lagringskonton för Azure Kina</span><span class="sxs-lookup"><span data-stu-id="0fed1-413">Add Storage Accounts for Azure China</span></span>
* <span data-ttu-id="0fed1-414">Lägg till Storage-konton med anpassade slutpunkter</span><span class="sxs-lookup"><span data-stu-id="0fed1-414">Add Storage Accounts with custom endpoints</span></span>
* <span data-ttu-id="0fed1-415">Öppna och visa hello innehållet text- och blobbar</span><span class="sxs-lookup"><span data-stu-id="0fed1-415">Open and view hello contents text and picture blobs</span></span>
* <span data-ttu-id="0fed1-416">Visa och redigera blob-egenskaper och metadata</span><span class="sxs-lookup"><span data-stu-id="0fed1-416">View and edit blob properties and metadata</span></span>

#### <a name="fixes"></a><span data-ttu-id="0fed1-417">Korrigeringar</span><span class="sxs-lookup"><span data-stu-id="0fed1-417">Fixes</span></span>

* <span data-ttu-id="0fed1-418">Fast: överföra eller hämta ett stort antal blobbar (500 +) kan ibland orsaka hello app toohave en vit skärm</span><span class="sxs-lookup"><span data-stu-id="0fed1-418">Fixed: uploading or download a large number of blobs (500+) may sometimes cause hello app toohave a white screen</span></span> 
* <span data-ttu-id="0fed1-419">Fast: när du ställer in blob-behållaren offentliga åtkomstnivå hello nya värdet uppdateras inte förrän du har angett hello fokus på hello behållare igen.</span><span class="sxs-lookup"><span data-stu-id="0fed1-419">Fixed: when setting blob container public access level, hello new value is not updated until you re-set hello focus on hello container.</span></span> <span data-ttu-id="0fed1-420">Dessutom hello dialogrutan alltid som standard för ”ingen offentlig åtkomst” och inte hello faktiska aktuella värde.</span><span class="sxs-lookup"><span data-stu-id="0fed1-420">Also, hello dialog always defaults too"No public access", and not hello actual current value.</span></span>
* <span data-ttu-id="0fed1-421">Stöd för bättre övergripande tangentbord/tillgänglighet och UI</span><span class="sxs-lookup"><span data-stu-id="0fed1-421">Better overall keyboard/accessibility and UI support</span></span>
* <span data-ttu-id="0fed1-422">Spår historik texten ska radbrytas när den är lång med ett blanksteg</span><span class="sxs-lookup"><span data-stu-id="0fed1-422">Breadcrumbs history text wraps when it's long with white space</span></span>
* <span data-ttu-id="0fed1-423">SAS-dialogrutan har stöd för verifiering av indata</span><span class="sxs-lookup"><span data-stu-id="0fed1-423">SAS dialog supports input validation</span></span>
* <span data-ttu-id="0fed1-424">Lokal lagring fortsätter toobe tillgängliga även om användarens autentiseringsuppgifter har upphört att gälla</span><span class="sxs-lookup"><span data-stu-id="0fed1-424">Local storage continues toobe available even if user credentials have expired</span></span>
* <span data-ttu-id="0fed1-425">När du tar bort en öppen blobbehållare stängs hello blob explorer hello höger</span><span class="sxs-lookup"><span data-stu-id="0fed1-425">When an opened blob container is deleted, hello blob explorer on hello right side is closed</span></span>

#### <a name="known-issues"></a><span data-ttu-id="0fed1-426">Kända problem</span><span class="sxs-lookup"><span data-stu-id="0fed1-426">Known Issues</span></span>

* <span data-ttu-id="0fed1-427">Installera Linux måste gcc version uppdateras eller uppgraderas – steg tooupgrade finns nedan:</span><span class="sxs-lookup"><span data-stu-id="0fed1-427">Linux install needs gcc version updated or upgraded – steps tooupgrade are below:</span></span> 
    * `sudo add-apt-repository ppa:ubuntu-toolchain-r/test`
    * `sudo apt-get update`
    * `sudo apt-get upgrade`
    * `sudo apt-get dist-upgrade`

<span data-ttu-id="0fed1-428">11/18/2015</span><span class="sxs-lookup"><span data-stu-id="0fed1-428">11/18/2015</span></span>
### <a name="version-07201511160"></a><span data-ttu-id="0fed1-429">Version 0.7.20151116.0</span><span class="sxs-lookup"><span data-stu-id="0fed1-429">Version 0.7.20151116.0</span></span>

#### <a name="new"></a><span data-ttu-id="0fed1-430">Ny</span><span class="sxs-lookup"><span data-stu-id="0fed1-430">New</span></span>

* <span data-ttu-id="0fed1-431">macOS och versioner av Windows</span><span class="sxs-lookup"><span data-stu-id="0fed1-431">macOS, and Windows versions</span></span>
* <span data-ttu-id="0fed1-432">Logga in tooview Storage-konton – använda din organisations konto, Account, 2FA osv.</span><span class="sxs-lookup"><span data-stu-id="0fed1-432">Sign in tooview your Storage Accounts – use your Org Account, Microsoft Account, 2FA, etc.</span></span>
* <span data-ttu-id="0fed1-433">Lokal utveckling lagring (Använd storage-emulatorn endast för Windows)</span><span class="sxs-lookup"><span data-stu-id="0fed1-433">Local development storage (use storage emulator, Windows-only)</span></span>
* <span data-ttu-id="0fed1-434">Stöd för Azure Resource Manager och klassisk resurs</span><span class="sxs-lookup"><span data-stu-id="0fed1-434">Azure Resource Manager and Classic resource support</span></span>
* <span data-ttu-id="0fed1-435">Skapa och ta bort blobbar, köer och tabeller</span><span class="sxs-lookup"><span data-stu-id="0fed1-435">Create and delete blobs, queues, or tables</span></span>
* <span data-ttu-id="0fed1-436">Sök efter specifika blobbar, köer och tabeller</span><span class="sxs-lookup"><span data-stu-id="0fed1-436">Search for specific blobs, queues, or tables</span></span>
* <span data-ttu-id="0fed1-437">Utforska hello innehållet i blob-behållare</span><span class="sxs-lookup"><span data-stu-id="0fed1-437">Explore hello contents of blob containers</span></span>
* <span data-ttu-id="0fed1-438">Visa och navigera genom kataloger</span><span class="sxs-lookup"><span data-stu-id="0fed1-438">View and navigate through directories</span></span>
* <span data-ttu-id="0fed1-439">Ladda upp, hämta och ta bort blobbar och mappar</span><span class="sxs-lookup"><span data-stu-id="0fed1-439">Upload, download, and delete blobs and folders</span></span>
* <span data-ttu-id="0fed1-440">Visa och redigera blob-egenskaper och metadata</span><span class="sxs-lookup"><span data-stu-id="0fed1-440">View and edit blob properties and metadata</span></span>
* <span data-ttu-id="0fed1-441">Generera en SAS-nycklar</span><span class="sxs-lookup"><span data-stu-id="0fed1-441">Generate SAS keys</span></span>
* <span data-ttu-id="0fed1-442">Hantera och skapa lagras åtkomst principer SAP)</span><span class="sxs-lookup"><span data-stu-id="0fed1-442">Manage and create Stored Access Policies (SAP)</span></span>
* <span data-ttu-id="0fed1-443">Sök efter blobbar efter prefix</span><span class="sxs-lookup"><span data-stu-id="0fed1-443">Search for blobs by prefix</span></span>
* <span data-ttu-id="0fed1-444">Dra 'n släpper filer tooupload eller ladda ned</span><span class="sxs-lookup"><span data-stu-id="0fed1-444">Drag 'n drop files tooupload or download</span></span>

#### <a name="known-issues"></a><span data-ttu-id="0fed1-445">Kända problem</span><span class="sxs-lookup"><span data-stu-id="0fed1-445">Known Issues</span></span>

* <span data-ttu-id="0fed1-446">När du ställer in blob-behållaren offentliga åtkomstnivå uppdateras hello nya värdet inte förrän du har angett hello fokus på hello behållare igen</span><span class="sxs-lookup"><span data-stu-id="0fed1-446">When setting blob container public access level, hello new value is not updated until you re-set hello focus on hello container</span></span>
* <span data-ttu-id="0fed1-447">När du öppnar hello dialogrutan tooset hello offentliga åtkomstnivå visas alltid ”ingen offentlig åtkomst” som standard hello och inte hello faktiska aktuella värde</span><span class="sxs-lookup"><span data-stu-id="0fed1-447">When you open hello dialog tooset hello public access level, it always shows "No public access" as hello default, and not hello actual current value</span></span>
* <span data-ttu-id="0fed1-448">Det går inte att byta namn på hämtade blobbar</span><span class="sxs-lookup"><span data-stu-id="0fed1-448">Cannot rename downloaded blobs</span></span>
* <span data-ttu-id="0fed1-449">Aktiviteten loggposter kommer ibland ”fastna” i en pågående tillstånd när ett fel inträffar och hello fel visas inte</span><span class="sxs-lookup"><span data-stu-id="0fed1-449">Activity log entries will sometimes get "stuck" in an in progress state when an error occurs, and hello error is not displayed</span></span>
* <span data-ttu-id="0fed1-450">Ibland krascher och aktiverar eller inaktiverar helt vit vid tooupload eller hämta ett stort antal blobbar</span><span class="sxs-lookup"><span data-stu-id="0fed1-450">Sometimes crashes or turns completely white when trying tooupload or download a large number of blobs</span></span>
* <span data-ttu-id="0fed1-451">Ibland avbryter en kopieringsåtgärd fungerar inte</span><span class="sxs-lookup"><span data-stu-id="0fed1-451">Sometimes canceling a copy operation does not work</span></span>
* <span data-ttu-id="0fed1-452">När du skapar en behållare (tabell-blob/kön), om du vill ange ett ogiltigt namn och fortsätta toocreate en annan under en annan Behållartyp du kan inte ange fokus på hello ny typ</span><span class="sxs-lookup"><span data-stu-id="0fed1-452">During creating a container (blob/queue/table), if you input an invalid name and proceed toocreate another under a different container type you cannot set focus on hello new type</span></span>
* <span data-ttu-id="0fed1-453">Det går inte att skapa en ny mapp eller byta namn på mappen</span><span class="sxs-lookup"><span data-stu-id="0fed1-453">Can't create new folder or rename folder</span></span>




[2]: https://support.microsoft.com/en-us/help/4021389/storage-explorer-troubleshooting-guide
[3]: vs-azure-tools-storage-manage-with-storage-explorer.md