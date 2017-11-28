---
title: "aaaUse PowerShell tooget igång med Azure Data Lake Store | Microsoft Docs"
description: "Använda Azure PowerShell toocreate ett Data Lake Store-konto och utföra grundläggande åtgärder"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: bf85f369-f9aa-4ca1-9ae7-e03a78eb7290
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 9c958bfd63e412ec0b0a4113a149f61aee026bc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a><span data-ttu-id="e99bb-103">Kom igång med Azure Data Lake Store med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="e99bb-103">Get started with Azure Data Lake Store using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e99bb-104">Portal</span><span class="sxs-lookup"><span data-stu-id="e99bb-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="e99bb-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e99bb-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="e99bb-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="e99bb-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="e99bb-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="e99bb-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="e99bb-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="e99bb-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="e99bb-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e99bb-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="e99bb-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="e99bb-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="e99bb-111">Python</span><span class="sxs-lookup"><span data-stu-id="e99bb-111">Python</span></span>](data-lake-store-get-started-python.md)
>
>

<span data-ttu-id="e99bb-112">Lär dig hur toouse Azure PowerShell toocreate ett Azure Data Lake lagrar konto och utföra grundläggande åtgärder, till exempel skapa mappar, ladda upp och hämta datafiler, ta bort ditt konto, osv. Mer information om Data Lake Store finns i [Översikt över Data Lake Store](data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e99bb-112">Learn how toouse Azure PowerShell toocreate an Azure Data Lake Store account and perform basic operations such as create folders, upload and download data files, delete your account, etc. For more information about Data Lake Store, see [Overview of Data Lake Store](data-lake-store-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e99bb-113">Krav</span><span class="sxs-lookup"><span data-stu-id="e99bb-113">Prerequisites</span></span>
<span data-ttu-id="e99bb-114">Innan du påbörjar den här självstudien måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="e99bb-114">Before you begin this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="e99bb-115">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="e99bb-115">**An Azure subscription**.</span></span> <span data-ttu-id="e99bb-116">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e99bb-116">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="e99bb-117">**Installera Azure PowerShell 1.0 eller senare**.</span><span class="sxs-lookup"><span data-stu-id="e99bb-117">**Azure PowerShell 1.0 or greater**.</span></span> <span data-ttu-id="e99bb-118">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e99bb-118">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="authentication"></a><span data-ttu-id="e99bb-119">Autentisering</span><span class="sxs-lookup"><span data-stu-id="e99bb-119">Authentication</span></span>
<span data-ttu-id="e99bb-120">Den här artikeln använder en enklare metod för autentisering med Data Lake Store var du befinner dig ange tooenter dina Azure-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e99bb-120">This article uses a simpler authentication approach with Data Lake Store where you are prompted tooenter your Azure account credentials.</span></span> <span data-ttu-id="e99bb-121">hello åtkomst nivå tooData Lake Store-konto och filsystem regleras sedan hello åtkomstnivån för hello inloggad användare.</span><span class="sxs-lookup"><span data-stu-id="e99bb-121">hello access level tooData Lake Store account and file system is then governed by hello access level of hello logged in user.</span></span> <span data-ttu-id="e99bb-122">Men det finns andra metoder som korrekt tooauthenticate med Data Lake Store, som är **slutanvändarens autentisering** eller **tjänst-till-tjänst autentisering**.</span><span class="sxs-lookup"><span data-stu-id="e99bb-122">However, there are other approaches as well tooauthenticate with Data Lake Store, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="e99bb-123">Anvisningar och mer information om hur tooauthenticate, se [slutanvändarens autentisering](data-lake-store-end-user-authenticate-using-active-directory.md) eller [tjänst-till-tjänst autentisering](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="e99bb-123">For instructions and more information on how tooauthenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="create-an-azure-data-lake-store-account"></a><span data-ttu-id="e99bb-124">Skapa ett Azure Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="e99bb-124">Create an Azure Data Lake Store account</span></span>
1. <span data-ttu-id="e99bb-125">På skrivbordet och öppna ett nytt Windows PowerShell-fönster och ange hello följande fragment toolog i tooyour Azure-konto, ange hello prenumerationen och registrera hello Data Lake Store-providern.</span><span class="sxs-lookup"><span data-stu-id="e99bb-125">From your desktop, open a new Windows PowerShell window, and enter hello following snippet toolog in tooyour Azure account, set hello subscription, and register hello Data Lake Store provider.</span></span> <span data-ttu-id="e99bb-126">När du tillfrågas toolog i, se till att logga in som en hello prenumerationens admininistratörer/ägare:</span><span class="sxs-lookup"><span data-stu-id="e99bb-126">When prompted toolog in, make sure you log in as one of hello subscription admininistrators/owner:</span></span>

        # Log in tooyour Azure account
        Login-AzureRmAccount

        # List all hello subscriptions associated tooyour account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"
2. <span data-ttu-id="e99bb-127">Ett Azure Data Lake Store-konto är kopplat till en resursgrupp i Azure.</span><span class="sxs-lookup"><span data-stu-id="e99bb-127">An Azure Data Lake Store account is associated with an Azure Resource Group.</span></span> <span data-ttu-id="e99bb-128">Börja med att skapa en Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="e99bb-128">Start by creating an Azure Resource Group.</span></span>

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    <span data-ttu-id="e99bb-129">![Skapa en Azure-resursgrupp](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Skapa en Azure-resursgrupp")</span><span class="sxs-lookup"><span data-stu-id="e99bb-129">![Create an Azure Resource Group](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Create an Azure Resource Group")</span></span>
3. <span data-ttu-id="e99bb-130">Skapa ett Azure Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="e99bb-130">Create an Azure Data Lake Store account.</span></span> <span data-ttu-id="e99bb-131">hello-namn som du anger får bara innehålla gemena bokstäver och siffror.</span><span class="sxs-lookup"><span data-stu-id="e99bb-131">hello name you specify must only contain lowercase letters and numbers.</span></span>

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    <span data-ttu-id="e99bb-132">![Skapa ett Azure Data Lake Store-konto](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Skapa ett Azure Data Lake Store-konto")</span><span class="sxs-lookup"><span data-stu-id="e99bb-132">![Create an Azure Data Lake Store account](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Create an Azure Data Lake Store account")</span></span>
4. <span data-ttu-id="e99bb-133">Kontrollera att hello kontot har skapats.</span><span class="sxs-lookup"><span data-stu-id="e99bb-133">Verify that hello account is successfully created.</span></span>

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    <span data-ttu-id="e99bb-134">hello utdata för detta bör vara **SANT**.</span><span class="sxs-lookup"><span data-stu-id="e99bb-134">hello output for this should be **True**.</span></span>

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a><span data-ttu-id="e99bb-135">Skapa katalogstrukturer i din Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="e99bb-135">Create directory structures in your Azure Data Lake Store</span></span>
<span data-ttu-id="e99bb-136">Du kan skapa kataloger under toomanage din Azure Data Lake Store-konto och lagra data.</span><span class="sxs-lookup"><span data-stu-id="e99bb-136">You can create directories under your Azure Data Lake Store account toomanage and store data.</span></span>

1. <span data-ttu-id="e99bb-137">Ange en rotkatalog.</span><span class="sxs-lookup"><span data-stu-id="e99bb-137">Specify a root directory.</span></span>

        $myrootdir = "/"
2. <span data-ttu-id="e99bb-138">Skapa en ny katalog som kallas **mynewdirectory** under hello angivna roten.</span><span class="sxs-lookup"><span data-stu-id="e99bb-138">Create a new directory called **mynewdirectory** under hello specified root.</span></span>

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory
3. <span data-ttu-id="e99bb-139">Kontrollera att den nya katalogen hello har skapats.</span><span class="sxs-lookup"><span data-stu-id="e99bb-139">Verify that hello new directory is successfully created.</span></span>

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    <span data-ttu-id="e99bb-140">Du bör se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="e99bb-140">It should show an output like hello following:</span></span>

    <span data-ttu-id="e99bb-141">![Verifiera katalog](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Verifiera katalog")</span><span class="sxs-lookup"><span data-stu-id="e99bb-141">![Verify Directory](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Verify Directory")</span></span>

## <a name="upload-data-tooyour-azure-data-lake-store"></a><span data-ttu-id="e99bb-142">Ladda upp data tooyour Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="e99bb-142">Upload data tooyour Azure Data Lake Store</span></span>
<span data-ttu-id="e99bb-143">Du kan ladda upp din tooData Datasjölager direkt vid hello nivå eller tooa rotkatalog som du skapade i hello-konto.</span><span class="sxs-lookup"><span data-stu-id="e99bb-143">You can upload your data tooData Lake Store directly at hello root level or tooa directory that you created within hello account.</span></span> <span data-ttu-id="e99bb-144">hello fragmenten nedan visar hur tooupload vissa toohello datakatalog exempel (**mynewdirectory**) du skapade i föregående avsnitt i hello.</span><span class="sxs-lookup"><span data-stu-id="e99bb-144">hello snippets below demonstrate how tooupload some sample data toohello directory (**mynewdirectory**) you created in hello previous section.</span></span>

<span data-ttu-id="e99bb-145">Om du vill söka efter vissa exempel data tooupload, kan du få hello **Ambulansdata** mapp från hello [Azure Data Lake Git-lagringsplatsen](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span><span class="sxs-lookup"><span data-stu-id="e99bb-145">If you are looking for some sample data tooupload, you can get hello **Ambulance Data** folder from hello [Azure Data Lake Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span></span> <span data-ttu-id="e99bb-146">Hämta hello-filen och lagra den på en lokal katalog på din dator, till exempel C:\sampledata\.</span><span class="sxs-lookup"><span data-stu-id="e99bb-146">Download hello file and store it in a local directory on your computer, such as  C:\sampledata\.</span></span>

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a><span data-ttu-id="e99bb-147">Byt namn, hämta och ta bort data från din Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="e99bb-147">Rename, download, and delete data from your Data Lake Store</span></span>
<span data-ttu-id="e99bb-148">toorename en fil, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e99bb-148">toorename a file, use hello following command:</span></span>

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

<span data-ttu-id="e99bb-149">toodownload en fil, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e99bb-149">toodownload a file, use hello following command:</span></span>

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

<span data-ttu-id="e99bb-150">toodelete en fil, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e99bb-150">toodelete a file, use hello following command:</span></span>

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

<span data-ttu-id="e99bb-151">När du uppmanas, anger **Y** toodelete hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="e99bb-151">When prompted, enter **Y** toodelete hello item.</span></span> <span data-ttu-id="e99bb-152">Om du har mer än en fil toodelete kan du ge alla hello sökvägar avgränsade med kommatecken.</span><span class="sxs-lookup"><span data-stu-id="e99bb-152">If you have more than one file toodelete, you can provide all hello paths separated by comma.</span></span>

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a><span data-ttu-id="e99bb-153">Ta bort ditt Azure Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="e99bb-153">Delete your Azure Data Lake Store account</span></span>
<span data-ttu-id="e99bb-154">Använd följande kommando toodelete hello ditt Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="e99bb-154">Use hello following command toodelete your Data Lake Store account.</span></span>

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

<span data-ttu-id="e99bb-155">När du uppmanas, anger **Y** toodelete hello-konto.</span><span class="sxs-lookup"><span data-stu-id="e99bb-155">When prompted, enter **Y** toodelete hello account.</span></span>

## <a name="performance-guidance-while-using-powershell"></a><span data-ttu-id="e99bb-156">Prestandavägledning när du använder PowerShell</span><span class="sxs-lookup"><span data-stu-id="e99bb-156">Performance guidance while using PowerShell</span></span>

<span data-ttu-id="e99bb-157">Nedan visas hello viktigaste inställningar som kan vara ögonen öppna tooget hello bästa prestanda när du använder PowerShell toowork med Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="e99bb-157">Below are hello most important settings that can be tuned tooget hello best performance while using PowerShell toowork with Data Lake Store:</span></span>

| <span data-ttu-id="e99bb-158">Egenskap</span><span class="sxs-lookup"><span data-stu-id="e99bb-158">Property</span></span>            | <span data-ttu-id="e99bb-159">Standard</span><span class="sxs-lookup"><span data-stu-id="e99bb-159">Default</span></span> | <span data-ttu-id="e99bb-160">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e99bb-160">Description</span></span> |
|---------------------|---------|-------------|
| <span data-ttu-id="e99bb-161">PerFileThreadCount</span><span class="sxs-lookup"><span data-stu-id="e99bb-161">PerFileThreadCount</span></span>  | <span data-ttu-id="e99bb-162">10</span><span class="sxs-lookup"><span data-stu-id="e99bb-162">10</span></span>      | <span data-ttu-id="e99bb-163">Den här parametern kan du toochoose hello antalet parallella trådar för att överföra eller hämta varje fil.</span><span class="sxs-lookup"><span data-stu-id="e99bb-163">This parameter enables you toochoose hello number of parallel threads for uploading or downloading each file.</span></span> <span data-ttu-id="e99bb-164">Det här representerar hello högsta antal trådar som kan tilldelas per fil, men du får mindre trådar beroende på scenario (t.ex. Om du överför en fil på 1 KB får du en tråd även om du fråga efter 20 trådar).</span><span class="sxs-lookup"><span data-stu-id="e99bb-164">This number represents hello max threads that can be allocated per file, but you may get less threads depending on your scenario (e.g. if you are uploading a 1KB file, you will get one thread even if you ask for 20 threads).</span></span>  |
| <span data-ttu-id="e99bb-165">ConcurrentFileCount</span><span class="sxs-lookup"><span data-stu-id="e99bb-165">ConcurrentFileCount</span></span> | <span data-ttu-id="e99bb-166">10</span><span class="sxs-lookup"><span data-stu-id="e99bb-166">10</span></span>      | <span data-ttu-id="e99bb-167">Den här parametern är specifikt för att ladda upp och ned mappar.</span><span class="sxs-lookup"><span data-stu-id="e99bb-167">This parameter is specifically for uploading or downloading folders.</span></span> <span data-ttu-id="e99bb-168">Den här parametern anger hello antalet samtidiga filer som kan överföras eller hämtas.</span><span class="sxs-lookup"><span data-stu-id="e99bb-168">This parameter determines hello number of concurrent files that can be uploaded or downloaded.</span></span> <span data-ttu-id="e99bb-169">Det här representerar hello maximalt antal samtidiga filer som kan överföras eller hämtas åt gången, men du får mindre samtidighet beroende på scenario (t.ex. Om du överför två filer, får du två samtidiga filöverföringar även om du uppmanas 15).</span><span class="sxs-lookup"><span data-stu-id="e99bb-169">This number represents hello maximum number of concurrent files that can be uploaded or downloaded at one time, but you may get less concurrency depending on your scenario (e.g. if you are uploading two files, you will get two concurrent file uploads even if you ask for 15).</span></span> |

<span data-ttu-id="e99bb-170">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="e99bb-170">**Example**</span></span>

<span data-ttu-id="e99bb-171">Det här kommandot laddar ned filer från Azure Data Lake Store toohello användarens lokala enhet med 20 trådar per fil- och 100 samtidiga filer.</span><span class="sxs-lookup"><span data-stu-id="e99bb-171">This command downloads files from Azure Data Lake Store toohello user's local drive using 20 threads per file and 100 concurrent files.</span></span>

    Export-AzureRmDataLakeStoreItem -AccountName <Data Lake Store account name> -PerFileThreadCount 20-ConcurrentFileCount 100 -Path /Powershell/100GB/ -Destination C:\Performance\ -Force -Recurse

### <a name="how-do-i-determine-hello-value-tooset-for-these-parameters"></a><span data-ttu-id="e99bb-172">Hur vet hello värdet tooset för dessa parametrar?</span><span class="sxs-lookup"><span data-stu-id="e99bb-172">How do I determine hello value tooset for these parameters?</span></span>

<span data-ttu-id="e99bb-173">Här är några riktlinjer som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="e99bb-173">Here's some guidance that you can use.</span></span>

* <span data-ttu-id="e99bb-174">**Steg 1: Bestäm hello Totalt antal trådar** -bör du börja med beräkning av hello totala tråd antal toouse.</span><span class="sxs-lookup"><span data-stu-id="e99bb-174">**Step 1: Determine hello total thread count** - You should start by calculating hello total thread count toouse.</span></span> <span data-ttu-id="e99bb-175">Som en generell riktlinje bör du använda 6 trådar för varje fysisk kärna.</span><span class="sxs-lookup"><span data-stu-id="e99bb-175">As a general guideline, you should use 6 threads for each physical core.</span></span>

        Total thread count = total physical cores * 6

    <span data-ttu-id="e99bb-176">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="e99bb-176">**Example**</span></span>

    <span data-ttu-id="e99bb-177">Under förutsättning att du kör hello PowerShell-kommandon från en D14 virtuell dator som har 16 kärnor</span><span class="sxs-lookup"><span data-stu-id="e99bb-177">Assuming you are running hello PowerShell commands from a D14 VM that has 16 cores</span></span>

        Total thread count = 16 cores * 6 = 96 threads


* <span data-ttu-id="e99bb-178">**Steg 2: Beräkna PerFileThreadCount** -vi beräkna vår PerFileThreadCount baserat på hello storlek på hello-filer.</span><span class="sxs-lookup"><span data-stu-id="e99bb-178">**Step 2: Calculate PerFileThreadCount**  - We calculate our PerFileThreadCount based on hello size of hello files.</span></span> <span data-ttu-id="e99bb-179">För filer som är mindre än 2,5 GB finns det inget behov av toochange den här parametern eftersom hello standardvärdet 10 är tillräckligt.</span><span class="sxs-lookup"><span data-stu-id="e99bb-179">For files smaller than 2.5GB, there is no need toochange this parameter because hello default of 10 is sufficient.</span></span> <span data-ttu-id="e99bb-180">För filer som är större än 2,5 GB bör du använda 10 trådar som hello bas för hello första 2,5 GB och Lägg till 1 tråd för varje ytterligare 256 MB ökning av filstorlek.</span><span class="sxs-lookup"><span data-stu-id="e99bb-180">For files larger than 2.5GB, you should use 10 threads as hello base for hello first 2.5GB and add 1 thread for each additional 256MB increase in file size.</span></span> <span data-ttu-id="e99bb-181">Om du kopierar en mapp med många olika filstorlekar bör du överväga att gruppera dem enligt liknande filstorlekar.</span><span class="sxs-lookup"><span data-stu-id="e99bb-181">If you are copying a folder with a large range of file sizes, consider grouping them into similar file sizes.</span></span> <span data-ttu-id="e99bb-182">Olika stora filstorlekar kan orsaka bristfälliga prestanda.</span><span class="sxs-lookup"><span data-stu-id="e99bb-182">Having dissimilar file sizes may cause non-optimal performance.</span></span> <span data-ttu-id="e99bb-183">Om detta inte är möjligt toogroup liknande filstorlekar bör du ange PerFileThreadCount baserat på hello största filstorlek.</span><span class="sxs-lookup"><span data-stu-id="e99bb-183">If that's not possible toogroup similar file sizes, you should set PerFileThreadCount based on hello largest file size.</span></span>

        PerFileThreadCount = 10 threads for hello first 2.5GB + 1 thread for each additional 256MB increase in file size

    <span data-ttu-id="e99bb-184">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="e99bb-184">**Example**</span></span>

    <span data-ttu-id="e99bb-185">Anta att du har 100 filer från 1GB too10GB och använder vi hello 10GB som hello största filstorlek för formeln som skulle läsa som hello nedan.</span><span class="sxs-lookup"><span data-stu-id="e99bb-185">Assuming you have 100 files ranging from 1GB too10GB, we use hello 10GB as hello largest file size for equation, which would read like hello following.</span></span>

        PerFileThreadCount = 10 + ((10GB - 2.5GB) / 256MB) = 40 threads

* <span data-ttu-id="e99bb-186">**Steg 3: Beräkna ConcurrentFilecount** -Använd hello Totalt antal trådar och PerFileThreadCount toocalculate ConcurrentFileCount baserat på hello följande formel.</span><span class="sxs-lookup"><span data-stu-id="e99bb-186">**Step 3: Calculate ConcurrentFilecount** - Use hello total thread count and PerFileThreadCount toocalculate ConcurrentFileCount based on hello following equation.</span></span>

        Total thread count = PerFileThreadCount * ConcurrentFileCount

    <span data-ttu-id="e99bb-187">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="e99bb-187">**Example**</span></span>

    <span data-ttu-id="e99bb-188">Baserat på hello exempelvärden har vi använt</span><span class="sxs-lookup"><span data-stu-id="e99bb-188">Based on hello example values we have been using</span></span>

        96 = 40 * ConcurrentFileCount

    <span data-ttu-id="e99bb-189">Därför **ConcurrentFileCount** är **2.4**, där vi kan avrunda för**2**.</span><span class="sxs-lookup"><span data-stu-id="e99bb-189">So, **ConcurrentFileCount** is **2.4**, which we can round off too**2**.</span></span>

### <a name="further-tuning"></a><span data-ttu-id="e99bb-190">Ytterligare justering</span><span class="sxs-lookup"><span data-stu-id="e99bb-190">Further tuning</span></span>

<span data-ttu-id="e99bb-191">Du kan kräva att ytterligare finjustera eftersom det finns en mängd filen storlekar toowork med.</span><span class="sxs-lookup"><span data-stu-id="e99bb-191">You might require further tuning because there is a range of file sizes toowork with.</span></span> <span data-ttu-id="e99bb-192">hello ovan beräkning fungerar bra om alla eller de flesta hello filer är större och närmare toohello 10GB intervall.</span><span class="sxs-lookup"><span data-stu-id="e99bb-192">hello above calculation works well if all or most of hello files are larger and closer toohello 10GB range.</span></span> <span data-ttu-id="e99bb-193">Om det finns i stället många olika filstorlekar med många filer som är mindre kan du minska PerFileThreadCount.</span><span class="sxs-lookup"><span data-stu-id="e99bb-193">If instead, there are many different files sizes with many files being smaller, then you could reduce PerFileThreadCount.</span></span> <span data-ttu-id="e99bb-194">Vi kan öka ConcurrentFileCount genom att minska hello PerFileThreadCount.</span><span class="sxs-lookup"><span data-stu-id="e99bb-194">By reducing hello PerFileThreadCount, we can increase ConcurrentFileCount.</span></span> <span data-ttu-id="e99bb-195">Så om vi antar att de flesta av våra filer har mindre hello 5GB intervallet kan vi gör nytt våra beräkning:</span><span class="sxs-lookup"><span data-stu-id="e99bb-195">So, if we assume that most of our files are smaller in hello 5GB range, we can re-do our calculation:</span></span>

    PerFileThreadCount = 10 + ((5GB - 2.5GB) / 256MB) = 20

<span data-ttu-id="e99bb-196">Därför **ConcurrentFileCount** kommer nu att 96/20, vilket är 4.8 avrundas för**4**.</span><span class="sxs-lookup"><span data-stu-id="e99bb-196">So, **ConcurrentFileCount** will now be 96/20, which is 4.8, rounded off too**4**.</span></span>

<span data-ttu-id="e99bb-197">Du kan fortsätta tootune inställningarna genom att ändra hello **PerFileThreadCount** uppåt och nedåt beroende på hello distribution av din filstorlekar.</span><span class="sxs-lookup"><span data-stu-id="e99bb-197">You can continue tootune these settings by changing hello **PerFileThreadCount** up and down depending on hello distribution of your file sizes.</span></span>

### <a name="limitation"></a><span data-ttu-id="e99bb-198">Begränsning</span><span class="sxs-lookup"><span data-stu-id="e99bb-198">Limitation</span></span>

* <span data-ttu-id="e99bb-199">**Antalet filer som är mindre än ConcurrentFileCount**: om hello antalet filer som du överför är mindre än hello **ConcurrentFileCount** som du beräknade sedan minska  **ConcurrentFileCount** toobe toohello lika många filer.</span><span class="sxs-lookup"><span data-stu-id="e99bb-199">**Number of files is less than ConcurrentFileCount**: If hello number of files you are uploading is smaller than hello **ConcurrentFileCount** that you calculated, then you should reduce **ConcurrentFileCount** toobe equal toohello number of files.</span></span> <span data-ttu-id="e99bb-200">Du kan använda alla återstående trådar tooincrease **PerFileThreadCount**.</span><span class="sxs-lookup"><span data-stu-id="e99bb-200">You can use any remaining threads tooincrease **PerFileThreadCount**.</span></span>

* <span data-ttu-id="e99bb-201">**För många trådar**: Om du ökar tråd räkna för mycket utan att öka klusterstorleken på din, hello risk för försämrade prestanda.</span><span class="sxs-lookup"><span data-stu-id="e99bb-201">**Too many threads**: If you increase thread count too much without increasing your cluster size, you run hello risk of degraded performance.</span></span> <span data-ttu-id="e99bb-202">Det kan finnas konkurrens problem när du växlar kontext på hello CPU.</span><span class="sxs-lookup"><span data-stu-id="e99bb-202">There can be contention issues when context-switching on hello CPU.</span></span>

* <span data-ttu-id="e99bb-203">**Otillräcklig samtidighet**: om hello samtidighet räcker inte och klustret kanske för liten.</span><span class="sxs-lookup"><span data-stu-id="e99bb-203">**Insufficient concurrency**: If hello concurrency is not sufficient, then your cluster may be too small.</span></span> <span data-ttu-id="e99bb-204">Du kan öka hello antalet noder i klustret som ger mer samtidighet.</span><span class="sxs-lookup"><span data-stu-id="e99bb-204">You can increase hello number of nodes in your cluster which will give you more concurrency.</span></span>

* <span data-ttu-id="e99bb-205">**Begränsningsfel**: Om konkurrensen är för hög kan det leda till begränsningsfel.</span><span class="sxs-lookup"><span data-stu-id="e99bb-205">**Throttling errors**: You may see throttling errors if your concurrency is too high.</span></span> <span data-ttu-id="e99bb-206">Om du ser begränsning fel, ska du minskar hello eller kontakta oss.</span><span class="sxs-lookup"><span data-stu-id="e99bb-206">If you are seeing throttling errors, you should either reduce hello concurrency or contact us.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e99bb-207">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e99bb-207">Next steps</span></span>
* [<span data-ttu-id="e99bb-208">Säkra data i Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="e99bb-208">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="e99bb-209">Använd Azure Data Lake Analytics med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="e99bb-209">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="e99bb-210">Använd Azure HDInsight med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="e99bb-210">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

