---
title: "Använder PowerShell för att komma igång med Azure Data Lake Store | Microsoft Docs"
description: "Använd Azure PowerShell för att skapa ett Data Lake Store-konto och utföra grundläggande åtgärder"
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
ms.openlocfilehash: 23c9aaa089251bff5132652475f4daadc2c128fe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a><span data-ttu-id="67dd0-103">Kom igång med Azure Data Lake Store med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="67dd0-103">Get started with Azure Data Lake Store using Azure PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="67dd0-104">Portal</span><span class="sxs-lookup"><span data-stu-id="67dd0-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="67dd0-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="67dd0-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="67dd0-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="67dd0-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="67dd0-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="67dd0-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="67dd0-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="67dd0-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="67dd0-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="67dd0-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="67dd0-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="67dd0-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="67dd0-111">Python</span><span class="sxs-lookup"><span data-stu-id="67dd0-111">Python</span></span>](data-lake-store-get-started-python.md)
>
>

<span data-ttu-id="67dd0-112">Lär dig mer om att använda Azure PowerShell för att skapa ett Azure Data Lake Store-konto och utföra grundläggande åtgärder, till exempel skapa mappar, ladda upp och hämta filer, ta bort ditt konto, osv. Mer information om Data Lake Store finns i [Översikt över Data Lake Store](data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="67dd0-112">Learn how to use Azure PowerShell to create an Azure Data Lake Store account and perform basic operations such as create folders, upload and download data files, delete your account, etc. For more information about Data Lake Store, see [Overview of Data Lake Store](data-lake-store-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="67dd0-113">Krav</span><span class="sxs-lookup"><span data-stu-id="67dd0-113">Prerequisites</span></span>
<span data-ttu-id="67dd0-114">Innan du påbörjar de här självstudierna måste du ha:</span><span class="sxs-lookup"><span data-stu-id="67dd0-114">Before you begin this tutorial, you must have the following:</span></span>

* <span data-ttu-id="67dd0-115">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="67dd0-115">**An Azure subscription**.</span></span> <span data-ttu-id="67dd0-116">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="67dd0-116">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="67dd0-117">**Installera Azure PowerShell 1.0 eller senare**.</span><span class="sxs-lookup"><span data-stu-id="67dd0-117">**Azure PowerShell 1.0 or greater**.</span></span> <span data-ttu-id="67dd0-118">Se [Så här installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="67dd0-118">See [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="authentication"></a><span data-ttu-id="67dd0-119">Autentisering</span><span class="sxs-lookup"><span data-stu-id="67dd0-119">Authentication</span></span>
<span data-ttu-id="67dd0-120">Den här artikeln använder en enklare metod för autentisering med Data Lake Store där du uppmanas att ange autentiseringsuppgifterna för ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="67dd0-120">This article uses a simpler authentication approach with Data Lake Store where you are prompted to enter your Azure account credentials.</span></span> <span data-ttu-id="67dd0-121">Åtkomstnivå till Data Lake Store-kontot och filsystemet styrs av åtkomstnivån för den inloggade användaren.</span><span class="sxs-lookup"><span data-stu-id="67dd0-121">The access level to Data Lake Store account and file system is then governed by the access level of the logged in user.</span></span> <span data-ttu-id="67dd0-122">Det finns dock olika sätt att autentisera med Data Lake Store: **slutanvändarens autentisering** eller **serviceautentisering**.</span><span class="sxs-lookup"><span data-stu-id="67dd0-122">However, there are other approaches as well to authenticate with Data Lake Store, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="67dd0-123">Instruktioner och mer information om hur du autentiserar finns i [Slutanvändarautentisering](data-lake-store-end-user-authenticate-using-active-directory.md) eller [Tjänst-till-tjänst-autentisering](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="67dd0-123">For instructions and more information on how to authenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>

## <a name="create-an-azure-data-lake-store-account"></a><span data-ttu-id="67dd0-124">Skapa ett Azure Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="67dd0-124">Create an Azure Data Lake Store account</span></span>
1. <span data-ttu-id="67dd0-125">Öppna ett nytt Windows PowerShell-fönster på skrivbordet och ange följande fragment för att logga in på kontot, ställ in prenumerationen och registrera Data Lake Store-providern.</span><span class="sxs-lookup"><span data-stu-id="67dd0-125">From your desktop, open a new Windows PowerShell window, and enter the following snippet to log in to your Azure account, set the subscription, and register the Data Lake Store provider.</span></span> <span data-ttu-id="67dd0-126">När du uppmanas att logga in, se till att logga in som en av prenumerationens admininistratörer/ägare:</span><span class="sxs-lookup"><span data-stu-id="67dd0-126">When prompted to log in, make sure you log in as one of the subscription admininistrators/owner:</span></span>

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"
2. <span data-ttu-id="67dd0-127">Ett Azure Data Lake Store-konto är kopplat till en resursgrupp i Azure.</span><span class="sxs-lookup"><span data-stu-id="67dd0-127">An Azure Data Lake Store account is associated with an Azure Resource Group.</span></span> <span data-ttu-id="67dd0-128">Börja med att skapa en Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="67dd0-128">Start by creating an Azure Resource Group.</span></span>

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    <span data-ttu-id="67dd0-129">![Skapa en Azure-resursgrupp](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Skapa en Azure-resursgrupp")</span><span class="sxs-lookup"><span data-stu-id="67dd0-129">![Create an Azure Resource Group](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Create an Azure Resource Group")</span></span>
3. <span data-ttu-id="67dd0-130">Skapa ett Azure Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="67dd0-130">Create an Azure Data Lake Store account.</span></span> <span data-ttu-id="67dd0-131">Namnet du anger får bara innehålla gemener och siffror.</span><span class="sxs-lookup"><span data-stu-id="67dd0-131">The name you specify must only contain lowercase letters and numbers.</span></span>

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    <span data-ttu-id="67dd0-132">![Skapa ett Azure Data Lake Store-konto](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Skapa ett Azure Data Lake Store-konto")</span><span class="sxs-lookup"><span data-stu-id="67dd0-132">![Create an Azure Data Lake Store account](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Create an Azure Data Lake Store account")</span></span>
4. <span data-ttu-id="67dd0-133">Kontrollera att kontot har skapats.</span><span class="sxs-lookup"><span data-stu-id="67dd0-133">Verify that the account is successfully created.</span></span>

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    <span data-ttu-id="67dd0-134">Utdata för detta ska vara **Sant**.</span><span class="sxs-lookup"><span data-stu-id="67dd0-134">The output for this should be **True**.</span></span>

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a><span data-ttu-id="67dd0-135">Skapa katalogstrukturer i din Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="67dd0-135">Create directory structures in your Azure Data Lake Store</span></span>
<span data-ttu-id="67dd0-136">Du kan skapa kataloger under Azure Data Lake Store-kontot för att hantera och lagra data.</span><span class="sxs-lookup"><span data-stu-id="67dd0-136">You can create directories under your Azure Data Lake Store account to manage and store data.</span></span>

1. <span data-ttu-id="67dd0-137">Ange en rotkatalog.</span><span class="sxs-lookup"><span data-stu-id="67dd0-137">Specify a root directory.</span></span>

        $myrootdir = "/"
2. <span data-ttu-id="67dd0-138">Skapa en ny katalog som kallas **mynewdirectory** under den angivna roten.</span><span class="sxs-lookup"><span data-stu-id="67dd0-138">Create a new directory called **mynewdirectory** under the specified root.</span></span>

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory
3. <span data-ttu-id="67dd0-139">Kontrollera att den nya katalogen har skapats.</span><span class="sxs-lookup"><span data-stu-id="67dd0-139">Verify that the new directory is successfully created.</span></span>

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    <span data-ttu-id="67dd0-140">Du bör se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="67dd0-140">It should show an output like the following:</span></span>

    <span data-ttu-id="67dd0-141">![Verifiera katalog](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Verifiera katalog")</span><span class="sxs-lookup"><span data-stu-id="67dd0-141">![Verify Directory](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Verify Directory")</span></span>

## <a name="upload-data-to-your-azure-data-lake-store"></a><span data-ttu-id="67dd0-142">Ladda upp data till din Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="67dd0-142">Upload data to your Azure Data Lake Store</span></span>
<span data-ttu-id="67dd0-143">Du kan ladda upp data till Data Lake Store direkt på rotnivå eller till en katalog som du har skapat i kontot.</span><span class="sxs-lookup"><span data-stu-id="67dd0-143">You can upload your data to Data Lake Store directly at the root level or to a directory that you created within the account.</span></span> <span data-ttu-id="67dd0-144">Fragmenten nedan visar hur du kan ladda upp exempeldata till katalogen (**mynewdirectory**) som du skapade i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="67dd0-144">The snippets below demonstrate how to upload some sample data to the directory (**mynewdirectory**) you created in the previous section.</span></span>

<span data-ttu-id="67dd0-145">Om du behöver exempeldata att ladda upp, kan du hämta mappen **Ambulansdata** från [Azure Data Lake Git-lagringsplatsen](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span><span class="sxs-lookup"><span data-stu-id="67dd0-145">If you are looking for some sample data to upload, you can get the **Ambulance Data** folder from the [Azure Data Lake Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span></span> <span data-ttu-id="67dd0-146">Hämta filen och lagra den i en lokal katalog på datorn, till exempel C:\sampledata\.</span><span class="sxs-lookup"><span data-stu-id="67dd0-146">Download the file and store it in a local directory on your computer, such as  C:\sampledata\.</span></span>

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a><span data-ttu-id="67dd0-147">Byt namn, hämta och ta bort data från din Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="67dd0-147">Rename, download, and delete data from your Data Lake Store</span></span>
<span data-ttu-id="67dd0-148">Om du vill byta namn på en fil, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="67dd0-148">To rename a file, use the following command:</span></span>

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

<span data-ttu-id="67dd0-149">Om du vill hämta en fil, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="67dd0-149">To download a file, use the following command:</span></span>

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

<span data-ttu-id="67dd0-150">Om du vill ta bort en fil, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="67dd0-150">To delete a file, use the following command:</span></span>

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

<span data-ttu-id="67dd0-151">När du uppmanas, trycker du på **Y** för att ta bort objektet.</span><span class="sxs-lookup"><span data-stu-id="67dd0-151">When prompted, enter **Y** to delete the item.</span></span> <span data-ttu-id="67dd0-152">Om du har fler än en fil att ta bort, kan du ange alla sökvägar avgränsade med kommatecken.</span><span class="sxs-lookup"><span data-stu-id="67dd0-152">If you have more than one file to delete, you can provide all the paths separated by comma.</span></span>

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a><span data-ttu-id="67dd0-153">Ta bort ditt Azure Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="67dd0-153">Delete your Azure Data Lake Store account</span></span>
<span data-ttu-id="67dd0-154">Använd följande kommando för att ta bort ditt Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="67dd0-154">Use the following command to delete your Data Lake Store account.</span></span>

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

<span data-ttu-id="67dd0-155">När du uppmanas, anger du **Y** för att ta bort kontot.</span><span class="sxs-lookup"><span data-stu-id="67dd0-155">When prompted, enter **Y** to delete the account.</span></span>

## <a name="performance-guidance-while-using-powershell"></a><span data-ttu-id="67dd0-156">Prestandavägledning när du använder PowerShell</span><span class="sxs-lookup"><span data-stu-id="67dd0-156">Performance guidance while using PowerShell</span></span>

<span data-ttu-id="67dd0-157">Nedan visas de viktigaste inställningar som du kan ställa in för att få bästa prestanda när du använder PowerShell för att arbeta med Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="67dd0-157">Below are the most important settings that can be tuned to get the best performance while using PowerShell to work with Data Lake Store:</span></span>

| <span data-ttu-id="67dd0-158">Egenskap</span><span class="sxs-lookup"><span data-stu-id="67dd0-158">Property</span></span>            | <span data-ttu-id="67dd0-159">Standard</span><span class="sxs-lookup"><span data-stu-id="67dd0-159">Default</span></span> | <span data-ttu-id="67dd0-160">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="67dd0-160">Description</span></span> |
|---------------------|---------|-------------|
| <span data-ttu-id="67dd0-161">PerFileThreadCount</span><span class="sxs-lookup"><span data-stu-id="67dd0-161">PerFileThreadCount</span></span>  | <span data-ttu-id="67dd0-162">10</span><span class="sxs-lookup"><span data-stu-id="67dd0-162">10</span></span>      | <span data-ttu-id="67dd0-163">Med den här parametern kan du välja antalet parallella trådar för att ladda upp eller ned varje fil.</span><span class="sxs-lookup"><span data-stu-id="67dd0-163">This parameter enables you to choose the number of parallel threads for uploading or downloading each file.</span></span> <span data-ttu-id="67dd0-164">Antalet representerar det högsta antal trådar som går att allokera per fil, men du kan få färre trådar beroende på ditt scenario (om du t.ex. laddar upp en fil på 1 KB får du en tråd även om du ber om 20 trådar).</span><span class="sxs-lookup"><span data-stu-id="67dd0-164">This number represents the max threads that can be allocated per file, but you may get less threads depending on your scenario (e.g. if you are uploading a 1KB file, you will get one thread even if you ask for 20 threads).</span></span>  |
| <span data-ttu-id="67dd0-165">ConcurrentFileCount</span><span class="sxs-lookup"><span data-stu-id="67dd0-165">ConcurrentFileCount</span></span> | <span data-ttu-id="67dd0-166">10</span><span class="sxs-lookup"><span data-stu-id="67dd0-166">10</span></span>      | <span data-ttu-id="67dd0-167">Den här parametern är specifikt för att ladda upp och ned mappar.</span><span class="sxs-lookup"><span data-stu-id="67dd0-167">This parameter is specifically for uploading or downloading folders.</span></span> <span data-ttu-id="67dd0-168">Den här parametern anger antalet samtidiga filer som kan laddas upp eller ned.</span><span class="sxs-lookup"><span data-stu-id="67dd0-168">This parameter determines the number of concurrent files that can be uploaded or downloaded.</span></span> <span data-ttu-id="67dd0-169">Siffran representerar det högsta antalet filer som samtidigt kan laddas upp eller ned samtidigt, men du kan få mindre samtidighet beroende på ditt scenario (om du exempelvis laddar upp två filer får du två samtidiga filuppladddningar även om du ber om 15).</span><span class="sxs-lookup"><span data-stu-id="67dd0-169">This number represents the maximum number of concurrent files that can be uploaded or downloaded at one time, but you may get less concurrency depending on your scenario (e.g. if you are uploading two files, you will get two concurrent file uploads even if you ask for 15).</span></span> |

<span data-ttu-id="67dd0-170">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="67dd0-170">**Example**</span></span>

<span data-ttu-id="67dd0-171">Det här kommandot laddar ned filer från Azure Data Lake Store till användarens lokala enhet med 20 trådar per fil och 100 samtidiga filer.</span><span class="sxs-lookup"><span data-stu-id="67dd0-171">This command downloads files from Azure Data Lake Store to the user's local drive using 20 threads per file and 100 concurrent files.</span></span>

    Export-AzureRmDataLakeStoreItem -AccountName <Data Lake Store account name> -PerFileThreadCount 20-ConcurrentFileCount 100 -Path /Powershell/100GB/ -Destination C:\Performance\ -Force -Recurse

### <a name="how-do-i-determine-the-value-to-set-for-these-parameters"></a><span data-ttu-id="67dd0-172">Hur tar jag reda på värdet som angetts för dessa parametrar?</span><span class="sxs-lookup"><span data-stu-id="67dd0-172">How do I determine the value to set for these parameters?</span></span>

<span data-ttu-id="67dd0-173">Här är några riktlinjer som du kan använda.</span><span class="sxs-lookup"><span data-stu-id="67dd0-173">Here's some guidance that you can use.</span></span>

* <span data-ttu-id="67dd0-174">**Steg 1: Fastställ det totala antalet trådar** – Du ska börja med att beräkna det totala antalet trådar som ska användas.</span><span class="sxs-lookup"><span data-stu-id="67dd0-174">**Step 1: Determine the total thread count** - You should start by calculating the total thread count to use.</span></span> <span data-ttu-id="67dd0-175">Som en generell riktlinje bör du använda 6 trådar för varje fysisk kärna.</span><span class="sxs-lookup"><span data-stu-id="67dd0-175">As a general guideline, you should use 6 threads for each physical core.</span></span>

        Total thread count = total physical cores * 6

    <span data-ttu-id="67dd0-176">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="67dd0-176">**Example**</span></span>

    <span data-ttu-id="67dd0-177">Vi antar att du kör PowerShell-kommandon från en virtuell D14-dator med 16 kärnor</span><span class="sxs-lookup"><span data-stu-id="67dd0-177">Assuming you are running the PowerShell commands from a D14 VM that has 16 cores</span></span>

        Total thread count = 16 cores * 6 = 96 threads


* <span data-ttu-id="67dd0-178">**Steg 2: Beräkna PerFileThreadCount** – Vi beräknar vår PerFileThreadCount baserat på filernas storlek.</span><span class="sxs-lookup"><span data-stu-id="67dd0-178">**Step 2: Calculate PerFileThreadCount**  - We calculate our PerFileThreadCount based on the size of the files.</span></span> <span data-ttu-id="67dd0-179">För filer som är mindre än 2,5 GB behöver vi inte ändra den här parametern eftersom standardvärdet 10 är tillräckligt.</span><span class="sxs-lookup"><span data-stu-id="67dd0-179">For files smaller than 2.5GB, there is no need to change this parameter because the default of 10 is sufficient.</span></span> <span data-ttu-id="67dd0-180">För filer som är större än 2,5 GB ska du använda 10 trådar som bas för de första 2,5 GB och lägga till en tråd för varje ytterligare ökning på 256 MB.</span><span class="sxs-lookup"><span data-stu-id="67dd0-180">For files larger than 2.5GB, you should use 10 threads as the base for the first 2.5GB and add 1 thread for each additional 256MB increase in file size.</span></span> <span data-ttu-id="67dd0-181">Om du kopierar en mapp med många olika filstorlekar bör du överväga att gruppera dem enligt liknande filstorlekar.</span><span class="sxs-lookup"><span data-stu-id="67dd0-181">If you are copying a folder with a large range of file sizes, consider grouping them into similar file sizes.</span></span> <span data-ttu-id="67dd0-182">Olika stora filstorlekar kan orsaka bristfälliga prestanda.</span><span class="sxs-lookup"><span data-stu-id="67dd0-182">Having dissimilar file sizes may cause non-optimal performance.</span></span> <span data-ttu-id="67dd0-183">Om du inte kan gruppera liknande filstorlekar ska du ställa in PerFileThreadCount baserat på den största filstorleken.</span><span class="sxs-lookup"><span data-stu-id="67dd0-183">If that's not possible to group similar file sizes, you should set PerFileThreadCount based on the largest file size.</span></span>

        PerFileThreadCount = 10 threads for the first 2.5GB + 1 thread for each additional 256MB increase in file size

    <span data-ttu-id="67dd0-184">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="67dd0-184">**Example**</span></span>

    <span data-ttu-id="67dd0-185">Om vi antar att du har 100 filer från 1 GB till 10 GB använder vi 10 GB som den största filstorleken för beräkningen, vilket skulle se ut ungefär som följande.</span><span class="sxs-lookup"><span data-stu-id="67dd0-185">Assuming you have 100 files ranging from 1GB to 10GB, we use the 10GB as the largest file size for equation, which would read like the following.</span></span>

        PerFileThreadCount = 10 + ((10GB - 2.5GB) / 256MB) = 40 threads

* <span data-ttu-id="67dd0-186">**Steg 3: Beräkna ConcurrentFilecount** – Använd det totala antalet trådar och PerFileThreadCount för att beräkna ConcurrentFileCount baserat på följande beräkning.</span><span class="sxs-lookup"><span data-stu-id="67dd0-186">**Step 3: Calculate ConcurrentFilecount** - Use the total thread count and PerFileThreadCount to calculate ConcurrentFileCount based on the following equation.</span></span>

        Total thread count = PerFileThreadCount * ConcurrentFileCount

    <span data-ttu-id="67dd0-187">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="67dd0-187">**Example**</span></span>

    <span data-ttu-id="67dd0-188">Utifrån de exempelvärden vi har använt</span><span class="sxs-lookup"><span data-stu-id="67dd0-188">Based on the example values we have been using</span></span>

        96 = 40 * ConcurrentFileCount

    <span data-ttu-id="67dd0-189">Så, **ConcurrentFileCount** är **2,4**, vilket vi kan runda av till **2**.</span><span class="sxs-lookup"><span data-stu-id="67dd0-189">So, **ConcurrentFileCount** is **2.4**, which we can round off to **2**.</span></span>

### <a name="further-tuning"></a><span data-ttu-id="67dd0-190">Ytterligare justering</span><span class="sxs-lookup"><span data-stu-id="67dd0-190">Further tuning</span></span>

<span data-ttu-id="67dd0-191">Du kan behöva justera ytterligare eftersom det finns en mängd filstorlekar att arbeta med.</span><span class="sxs-lookup"><span data-stu-id="67dd0-191">You might require further tuning because there is a range of file sizes to work with.</span></span> <span data-ttu-id="67dd0-192">Beräkningen ovan fungerar bra om alla eller de flesta av filerna är större och närmare intervallet 10 GB.</span><span class="sxs-lookup"><span data-stu-id="67dd0-192">The above calculation works well if all or most of the files are larger and closer to the 10GB range.</span></span> <span data-ttu-id="67dd0-193">Om det finns i stället många olika filstorlekar med många filer som är mindre kan du minska PerFileThreadCount.</span><span class="sxs-lookup"><span data-stu-id="67dd0-193">If instead, there are many different files sizes with many files being smaller, then you could reduce PerFileThreadCount.</span></span> <span data-ttu-id="67dd0-194">Du kan öka ConcurrentFileCount genom att minska PerFileThreadCount.</span><span class="sxs-lookup"><span data-stu-id="67dd0-194">By reducing the PerFileThreadCount, we can increase ConcurrentFileCount.</span></span> <span data-ttu-id="67dd0-195">Så om vi förutsätter att de flesta av våra filer är mindre än intervallet 5 GB kan vi göra om vår beräkning:</span><span class="sxs-lookup"><span data-stu-id="67dd0-195">So, if we assume that most of our files are smaller in the 5GB range, we can re-do our calculation:</span></span>

    PerFileThreadCount = 10 + ((5GB - 2.5GB) / 256MB) = 20

<span data-ttu-id="67dd0-196">Alltså blir **ConcurrentFileCount** nu 96/20, vilket är 4,8, avrundat till **4**.</span><span class="sxs-lookup"><span data-stu-id="67dd0-196">So, **ConcurrentFileCount** will now be 96/20, which is 4.8, rounded off to **4**.</span></span>

<span data-ttu-id="67dd0-197">Du kan fortsätta att finjustera inställningarna genom att ändra **PerFileThreadCount** upp och ned beroende på distributionen av din filstorlek.</span><span class="sxs-lookup"><span data-stu-id="67dd0-197">You can continue to tune these settings by changing the **PerFileThreadCount** up and down depending on the distribution of your file sizes.</span></span>

### <a name="limitation"></a><span data-ttu-id="67dd0-198">Begränsning</span><span class="sxs-lookup"><span data-stu-id="67dd0-198">Limitation</span></span>

* <span data-ttu-id="67dd0-199">**Antalet filer är mindre än ConcurrentFileCount**: Om antalet filer som du laddar upp är mindre än **ConcurrentFileCount** du har beräknat ska du minska **ConcurrentFileCount** så att det motsvarar antalet filer.</span><span class="sxs-lookup"><span data-stu-id="67dd0-199">**Number of files is less than ConcurrentFileCount**: If the number of files you are uploading is smaller than the **ConcurrentFileCount** that you calculated, then you should reduce **ConcurrentFileCount** to be equal to the number of files.</span></span> <span data-ttu-id="67dd0-200">Du kan använda eventuella återstående trådar för att öka **PerFileThreadCount**.</span><span class="sxs-lookup"><span data-stu-id="67dd0-200">You can use any remaining threads to increase **PerFileThreadCount**.</span></span>

* <span data-ttu-id="67dd0-201">**För många trådar**: Om du ökar antalet trådar för mycket utan att öka klusterstorleken riskerar du att prestanda försämras.</span><span class="sxs-lookup"><span data-stu-id="67dd0-201">**Too many threads**: If you increase thread count too much without increasing your cluster size, you run the risk of degraded performance.</span></span> <span data-ttu-id="67dd0-202">Det kan uppstå konkurrensproblem när du växlar innehåll på processorn.</span><span class="sxs-lookup"><span data-stu-id="67dd0-202">There can be contention issues when context-switching on the CPU.</span></span>

* <span data-ttu-id="67dd0-203">**Otillräcklig samtidighet**: Om samtidigheten inte är tillräcklig kan klustret vara för litet.</span><span class="sxs-lookup"><span data-stu-id="67dd0-203">**Insufficient concurrency**: If the concurrency is not sufficient, then your cluster may be too small.</span></span> <span data-ttu-id="67dd0-204">Du kan öka antalet noder i klustret så att du får större samtidighet.</span><span class="sxs-lookup"><span data-stu-id="67dd0-204">You can increase the number of nodes in your cluster which will give you more concurrency.</span></span>

* <span data-ttu-id="67dd0-205">**Begränsningsfel**: Om konkurrensen är för hög kan det leda till begränsningsfel.</span><span class="sxs-lookup"><span data-stu-id="67dd0-205">**Throttling errors**: You may see throttling errors if your concurrency is too high.</span></span> <span data-ttu-id="67dd0-206">Om du råkar ut för begränsningsfel ska du antingen minska samtidigheten eller kontakta oss.</span><span class="sxs-lookup"><span data-stu-id="67dd0-206">If you are seeing throttling errors, you should either reduce the concurrency or contact us.</span></span>

## <a name="next-steps"></a><span data-ttu-id="67dd0-207">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="67dd0-207">Next steps</span></span>
* [<span data-ttu-id="67dd0-208">Säkra data i Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="67dd0-208">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="67dd0-209">Använd Azure Data Lake Analytics med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="67dd0-209">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="67dd0-210">Använd Azure HDInsight med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="67dd0-210">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

