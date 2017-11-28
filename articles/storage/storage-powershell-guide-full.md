---
title: "Använda Azure PowerShell med Azure Storage | Microsoft Docs"
description: "Lär dig hur du använder Azure PowerShell-cmdlets för Azure Storage för att skapa och hantera lagringskonton; Arbeta med blobbar, tabeller, köer och -filer. Konfigurera och efterfråga storage analytics och skapa signaturer för delad åtkomst."
services: storage
documentationcenter: na
author: robinsh
manager: timlt
ms.assetid: f4704f58-abc6-4f89-8b6d-1b1659746f5a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/03/2017
ms.author: robinsh
ms.openlocfilehash: 51e3e93ebedd31370857e61a00139294bcee9237
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-powershell-with-azure-storage"></a><span data-ttu-id="7f8b2-103">Använda Azure PowerShell med Azure Storage</span><span class="sxs-lookup"><span data-stu-id="7f8b2-103">Using Azure PowerShell with Azure Storage</span></span>
## <a name="overview"></a><span data-ttu-id="7f8b2-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="7f8b2-104">Overview</span></span>
<span data-ttu-id="7f8b2-105">Azure PowerShell är en modul som tillhandahåller för att hantera Azure via Windows PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-105">Azure PowerShell is a module that provides cmdlets to manage Azure through Windows PowerShell.</span></span> <span data-ttu-id="7f8b2-106">Det är ett uppgiftsbaserat kommandoradsgränssnitt och skriptspråk som utformats specifikt för systemadministration.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-106">It is a task-based command-line shell and scripting language designed especially for system administration.</span></span> <span data-ttu-id="7f8b2-107">Du kan enkelt styra och automatisera administrationen av dina Azure-tjänster och program med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-107">With PowerShell, you can easily control and automate the administration of your Azure services and applications.</span></span> <span data-ttu-id="7f8b2-108">Du kan till exempel använda cmdlets för att utföra samma uppgifter som du kan utföra via den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-108">For example, you can use the cmdlets to perform the same tasks that you can perform through the [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="7f8b2-109">I den här guiden kommer vi hur du använder den [Azure Storage-Cmdlets](/powershell/module/azurerm.storage/#storage) att utföra olika uppgifter för utveckling och administration med Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-109">In this guide, we'll explore how to use the [Azure Storage Cmdlets](/powershell/module/azurerm.storage/#storage) to perform a variety of development and administration tasks with Azure Storage.</span></span>

<span data-ttu-id="7f8b2-110">Den här guiden förutsätter att du har tidigare erfarenhet med [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) och [Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-110">This guide assumes that you have prior experience using [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) and [Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx).</span></span> <span data-ttu-id="7f8b2-111">Handboken innehåller ett antal skript för att demonstrera användningen av PowerShell med Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-111">The guide provides a number of scripts to demonstrate the usage of PowerShell with Azure Storage.</span></span> <span data-ttu-id="7f8b2-112">Du bör uppdatera skriptvariabler baserat på din konfiguration innan du kör varje skript.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-112">You should update the script variables based on your configuration before running each script.</span></span>

<span data-ttu-id="7f8b2-113">Det första avsnittet i den här guiden ger en snabb överblick över vid Azure Storage och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-113">The first section in this guide provides a quick glance at Azure Storage and PowerShell.</span></span> <span data-ttu-id="7f8b2-114">Detaljerad information och instruktioner som startar från den [krav för att använda Azure PowerShell med Azure Storage](#prerequisites-for-using-azure-powershell-with-azure-storage).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-114">For detailed information and instructions, start from the [Prerequisites for using Azure PowerShell with Azure Storage](#prerequisites-for-using-azure-powershell-with-azure-storage).</span></span>

## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a><span data-ttu-id="7f8b2-115">Komma igång med Azure Storage och PowerShell 5 minuter</span><span class="sxs-lookup"><span data-stu-id="7f8b2-115">Getting started with Azure Storage and PowerShell in 5 minutes</span></span>
<span data-ttu-id="7f8b2-116">Det här avsnittet visar hur du kommer åt Azure Storage via PowerShell 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-116">This section shows you how to access Azure Storage via PowerShell in 5 minutes.</span></span>

<span data-ttu-id="7f8b2-117">**Ny till Azure:** hämta Microsoft Azure-prenumeration och ett Microsoft-konto som är kopplade till den prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-117">**New to Azure:** Get a Microsoft Azure subscription and a Microsoft account associated with that subscription.</span></span> <span data-ttu-id="7f8b2-118">Mer information om köpalternativ för Azure finns [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/), [köpa alternativ](https://azure.microsoft.com/pricing/purchase-options/), och [Medlemserbjudanden](https://azure.microsoft.com/pricing/member-offers/) (för medlemmar i MSDN, Microsoft Partner Network, BizSpark och att andra Microsoft-program).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-118">For information on Azure purchase options, see [Free Trial](https://azure.microsoft.com/pricing/free-trial/), [Purchase Options](https://azure.microsoft.com/pricing/purchase-options/), and [Member Offers](https://azure.microsoft.com/pricing/member-offers/) (for members of MSDN, Microsoft Partner Network, and BizSpark, and other Microsoft programs).</span></span>

<span data-ttu-id="7f8b2-119">Se [Tilldela administratörsroller i Azure Active Directory (AD Azure)](https://msdn.microsoft.com/library/azure/hh531793.aspx) för mer information om Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-119">See [Assigning administrator roles in Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) for more information about Azure subscriptions.</span></span>

<span data-ttu-id="7f8b2-120">**När du har skapat ett Microsoft Azure-prenumeration och konto:**</span><span class="sxs-lookup"><span data-stu-id="7f8b2-120">**After creating a Microsoft Azure subscription and account:**</span></span>

1. <span data-ttu-id="7f8b2-121">Hämta och installera senaste [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/latest).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-121">Download and install the latest [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/latest).</span></span>
2. <span data-ttu-id="7f8b2-122">Starta Windows PowerShell Integrated Scripting Environment (ISE): Den lokala datorn, gå till den **starta** menyn.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-122">Start Windows PowerShell Integrated Scripting Environment (ISE): In your local computer, go to the **Start** menu.</span></span> <span data-ttu-id="7f8b2-123">Typen **Administrationsverktyg** och klickar för att köra den.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-123">Type **Administrative Tools** and click to run it.</span></span> <span data-ttu-id="7f8b2-124">I den **Administrationsverktyg** fönstret, högerklicka på **Windows PowerShell ISE**, klickar du på **kör som administratör**.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-124">In the **Administrative Tools** window, right-click **Windows PowerShell ISE**, click **Run as Administrator**.</span></span>
3. <span data-ttu-id="7f8b2-125">I **Windows PowerShell ISE**, klickar du på **filen** > **ny** att skapa en ny skriptfil.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-125">In **Windows PowerShell ISE**, click **File** > **New** to create a new script file.</span></span>
4. <span data-ttu-id="7f8b2-126">Nu får du ett enkelt skript som visar grundläggande PowerShell-kommandon för att få åtkomst till Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-126">Now, we'll give you a simple script that shows basic PowerShell commands to access Azure Storage.</span></span> <span data-ttu-id="7f8b2-127">Skriptet kommer först att fråga din Azure autentiseringsuppgifter för att lägga till din Azure-kontot till den lokala PowerShell-miljön.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-127">The script will first ask your Azure account credentials to add your Azure account to the local PowerShell environment.</span></span> <span data-ttu-id="7f8b2-128">Skriptet kommer sedan standardvärdet Azure-prenumeration och skapa ett nytt lagringskonto i Azure.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-128">Then, the script will set the default Azure subscription and create a new storage account in Azure.</span></span> <span data-ttu-id="7f8b2-129">Skriptet kommer sedan skapa en ny behållare i den här nya storage-konto och ladda upp en befintlig bildfil (blob) till behållaren.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-129">Next, the script will create a new container in this new storage account and upload an existing image file (blob) to that container.</span></span> <span data-ttu-id="7f8b2-130">När skriptet visar alla blobbar i behållaren, kommer den skapa en ny målkatalog i den lokala datorn och hämta avbildningsfilen.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-130">After the script lists all blobs in that container, it will create a new destination directory in your local computer and download the image file.</span></span>
5. <span data-ttu-id="7f8b2-131">I följande avsnitt i koden väljer du skriptet mellan anmärkningar **#begin** och **#end**.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-131">In the following code section, select the script between the remarks **#begin** and **#end**.</span></span> <span data-ttu-id="7f8b2-132">Tryck på CTRL + C för att kopiera den till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-132">Press CTRL+C to copy it to the clipboard.</span></span>

    ```powershell
    # begin
    # Update with the name of your subscription.
    $SubscriptionName = "YourSubscriptionName"
       
    # Give a name to your new storage account. It must be lowercase!
    $StorageAccountName = "yourstorageaccountname"
       
    # Choose "West US" as an example.
    $Location = "West US"
       
    # Give a name to your new container.
    $ContainerName = "imagecontainer"
       
    # Have an image file and a source directory in your local computer.
    $ImageToUpload = "C:\Images\HelloWorld.png"
       
    # A destination directory in your local computer.
    $DestinationFolder = "C:\DownloadImages"
       
    # Add your Azure account to the local PowerShell environment.
    Add-AzureAccount
       
    # Set a default Azure subscription.
    Select-AzureSubscription -SubscriptionName $SubscriptionName –Default
       
    # Create a new storage account.
    New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $Location
       
    # Set a default storage account.
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
       
    # Create a new container.
    New-AzureStorageContainer -Name $ContainerName -Permission Off
       
    # Upload a blob into a container.
    Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload
       
    # List all blobs in a container.
    Get-AzureStorageBlob -Container $ContainerName
       
    # Download blobs from the container:
    # Get a reference to a list of all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName
       
    # Create the destination directory.
    New-Item -Path $DestinationFolder -ItemType Directory -Force  
       
    # Download blobs into the local destination directory.
    $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
       
    # end
    ```

6. <span data-ttu-id="7f8b2-133">I **Windows PowerShell ISE**, tryck på CTRL + V för att kopiera skriptet.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-133">In **Windows PowerShell ISE**, press CTRL+V to copy the script.</span></span> <span data-ttu-id="7f8b2-134">Klicka på **filen** > **spara**.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-134">Click **File** > **Save**.</span></span> <span data-ttu-id="7f8b2-135">I den **Spara som** dialogruta, skriver du namnet på skriptfilen, till exempel ”mystoragescript”.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-135">In the **Save As** dialog window, type the name of the script file, such as "mystoragescript."</span></span> <span data-ttu-id="7f8b2-136">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-136">Click **Save**.</span></span>
7. <span data-ttu-id="7f8b2-137">Nu kan behöver du uppdatera skriptvariabler baserat på inställningarna.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-137">Now, you need to update the script variables based on your configuration settings.</span></span> <span data-ttu-id="7f8b2-138">Du måste uppdatera de **$SubscriptionName** variabeln med din egen prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-138">You must update the **$SubscriptionName** variable with your own subscription.</span></span> <span data-ttu-id="7f8b2-139">Du kan behålla variablerna som anges i skriptet eller uppdatera dem som du vill.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-139">You can keep the other variables as specified in the script or update them as you wish.</span></span>
   
   * <span data-ttu-id="7f8b2-140">**$SubscriptionName:** måste du uppdatera den här variabeln med prenumerationsnamn på din.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-140">**$SubscriptionName:** You must update this variable with your own subscription name.</span></span> <span data-ttu-id="7f8b2-141">Gör något av följande tre sätt att hitta namnet på din prenumeration:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-141">Follow one of the following three ways to locate the name of your subscription:</span></span>
     
    <span data-ttu-id="7f8b2-142">a.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-142">a.</span></span> <span data-ttu-id="7f8b2-143">I **Windows PowerShell ISE**, klickar du på **filen** > **ny** att skapa en ny skriptfil.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-143">In **Windows PowerShell ISE**, click **File** > **New** to create a new script file.</span></span> <span data-ttu-id="7f8b2-144">Kopiera följande skript till nya skriptfilen och klicka på **felsöka** > **kör**.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-144">Copy the following script to the new script file and click **Debug** > **Run**.</span></span> <span data-ttu-id="7f8b2-145">Följande skript kommer först att fråga din Azure autentiseringsuppgifter för att lägga till din Azure-kontot till den lokala PowerShell-miljön och visa alla prenumerationer som är anslutna till den lokala PowerShell-sessionen.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-145">The following script will first ask your Azure account credentials to add your Azure account to the local PowerShell environment and then show all the subscriptions that are connected to the local PowerShell session.</span></span> <span data-ttu-id="7f8b2-146">Anteckna namnet på den prenumeration som du vill använda när du följa de här självstudierna:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-146">Take a note of the name of the subscription that you want to use while following this tutorial:</span></span>
     
    ```powershell
    Add-AzureAccount 
      Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName
    ```

    <span data-ttu-id="7f8b2-147">b.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-147">b.</span></span> <span data-ttu-id="7f8b2-148">Hitta och kopiera ditt prenumerationsnamn i den [Azure-portalen](https://portal.azure.com), i hubbmenyn till vänster, klickar du på **prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-148">To locate and copy your subscription name in the [Azure portal](https://portal.azure.com), in the Hub menu on the left, click **Subscriptions**.</span></span> <span data-ttu-id="7f8b2-149">Kopiera prenumerationsnamnet som du vill använda när du kör skripten i den här guiden.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-149">Copy the name of subscription that you want to use while running the scripts in this guide.</span></span>
     
     ![Azure Portal](./media/storage-powershell-guide-full/Subscription_Previewportal.png)

    <span data-ttu-id="7f8b2-151">c.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-151">c.</span></span> <span data-ttu-id="7f8b2-152">Hitta och kopiera ditt prenumerationsnamn i den [klassiska Azure-portalen](https://manage.windowsazure.com/), bläddra nedåt och klicka på **inställningar** på vänster sida av portalen.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-152">To locate and copy your subscription name in the [Azure Classic Portal](https://manage.windowsazure.com/), scroll down and click **Settings** on the left side of the portal.</span></span> <span data-ttu-id="7f8b2-153">Klicka på **prenumerationer** att se en lista över dina prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-153">Click **Subscriptions** to see a list of your subscriptions.</span></span> <span data-ttu-id="7f8b2-154">Kopiera prenumerationsnamnet som du vill använda när du kör skripten som anges i den här guiden.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-154">Copy the name of subscription that you want to use while running the scripts given in this guide.</span></span>
     
     ![Klassisk Azure-portal](./media/storage-powershell-guide-full/Subscription_currentportal.png)

   * <span data-ttu-id="7f8b2-156">**$StorageAccountName:** använder det angivna namnet i skriptet eller ange ett nytt namn för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-156">**$StorageAccountName:** Use the given name in the script or enter a new name for your storage account.</span></span> <span data-ttu-id="7f8b2-157">**Viktigt:** namnet på lagringskontot måste vara unikt i Azure.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-157">**Important:** The name of the storage account must be unique in Azure.</span></span> <span data-ttu-id="7f8b2-158">Det måste vara gemena för!</span><span class="sxs-lookup"><span data-stu-id="7f8b2-158">It must be lowercase, too!</span></span>
   * <span data-ttu-id="7f8b2-159">**$Location:** använder den angivna ”USA, västra” i skriptet eller välj andra Azure platser, till exempel östra USA, Norra Europa och så vidare.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-159">**$Location:** Use the given "West US" in the script or choose other Azure locations, such as East US, North Europe, and so on.</span></span>
   * <span data-ttu-id="7f8b2-160">**$ContainerName:** använder det angivna namnet i skriptet eller ange ett nytt namn för din behållare.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-160">**$ContainerName:** Use the given name in the script or enter a new name for your container.</span></span>
   * <span data-ttu-id="7f8b2-161">**$ImageToUpload:** som anger en sökväg till en bild på den lokala datorn: ”C:\Images\HelloWorld.png”.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-161">**$ImageToUpload:** Enter a path to a picture on your local computer, such as: "C:\Images\HelloWorld.png".</span></span>
   * <span data-ttu-id="7f8b2-162">**$DestinationFolder:** anger en sökväg till en lokal katalog att lagra filer som hämtas från Azure Storage, exempelvis: ”C:\DownloadImages”.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-162">**$DestinationFolder:** Enter a path to a local directory to store files downloaded from Azure Storage, such as: "C:\DownloadImages".</span></span>
8. <span data-ttu-id="7f8b2-163">När du har uppdaterat skriptvariabler i filen ”mystoragescript.ps1” klickar du på **filen** > **spara**.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-163">After updating the script variables in the "mystoragescript.ps1" file, click **File** > **Save**.</span></span> <span data-ttu-id="7f8b2-164">Klicka på **felsöka** > **kör** eller tryck på **F5** att köra skriptet.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-164">Then, click **Debug** > **Run** or press **F5** to run the script.</span></span>  

<span data-ttu-id="7f8b2-165">När skriptet körs, bör du ha en lokal målmapp som innehåller hämtade avbildningsfilen.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-165">After the script runs, you should have a local destination folder that includes the downloaded image file.</span></span> <span data-ttu-id="7f8b2-166">Följande skärmbild visar ett exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-166">The following screenshot shows an example output:</span></span>

![Ladda ned Blobbar](./media/storage-powershell-guide-full/Blobdownload.png)

> [!NOTE]
> <span data-ttu-id="7f8b2-168">Avsnittet ”komma igång med Azure Storage och PowerShell 5 minuter” tillhandahålls en snabb introduktion på hur du använder Azure PowerShell med Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-168">The "Getting started with Azure Storage and PowerShell in 5 minutes" section provided a quick introduction on how to use Azure PowerShell with Azure Storage.</span></span> <span data-ttu-id="7f8b2-169">Mer detaljerad information och instruktioner bör du läsa följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-169">For detailed information and instructions, we encourage you to read the following sections.</span></span>
> 

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a><span data-ttu-id="7f8b2-170">Krav för att använda Azure PowerShell med Azure Storage</span><span class="sxs-lookup"><span data-stu-id="7f8b2-170">Prerequisites for using Azure PowerShell with Azure Storage</span></span>
<span data-ttu-id="7f8b2-171">Du behöver ett Azure-prenumeration och konto för att köra PowerShell-cmdlets som anges i den här guiden, enligt beskrivningen ovan.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-171">You need an Azure subscription and account to run the PowerShell cmdlets given in this guide, as described above.</span></span>

<span data-ttu-id="7f8b2-172">Azure PowerShell är en modul som tillhandahåller för att hantera Azure via Windows PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-172">Azure PowerShell is a module that provides cmdlets to manage Azure through Windows PowerShell.</span></span> <span data-ttu-id="7f8b2-173">Information om att installera och konfigurera Azure PowerShell finns i [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-173">For information on installing and setting up Azure PowerShell, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="7f8b2-174">Vi rekommenderar att du hämtar och installerar eller uppgraderar till senaste Azure PowerShell-modulen innan du använder den här guiden.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-174">We recommend that you download and install or upgrade to the latest Azure PowerShell module before using this guide.</span></span>

<span data-ttu-id="7f8b2-175">Du kan köra cmdlets i Windows PowerShell-konsolen som standard eller på Windows PowerShell Integrated Scripting Environment (ISE).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-175">You can run the cmdlets in the standard Windows PowerShell console or the Windows PowerShell Integrated Scripting Environment (ISE).</span></span> <span data-ttu-id="7f8b2-176">Till exempel för att öppna **Windows PowerShell ISE**, gå till Start-menyn, Skriv Administrationsverktyg och på att köra den.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-176">For example, to open **Windows PowerShell ISE**, go to the Start menu, type Administrative Tools and click to run it.</span></span> <span data-ttu-id="7f8b2-177">Högerklicka på Windows PowerShell ISE i fönstret Administrationsverktyg, klicka på Kör som administratör.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-177">In the Administrative Tools window, right-click Windows PowerShell ISE, click Run as Administrator.</span></span>

## <a name="how-to-manage-storage-accounts-in-azure"></a><span data-ttu-id="7f8b2-178">Så här hanterar du storage-konton i Azure</span><span class="sxs-lookup"><span data-stu-id="7f8b2-178">How to manage storage accounts in Azure</span></span>

<span data-ttu-id="7f8b2-179">Låt oss ta en titt på Hantera storage-konton i Azure med PowerShell</span><span class="sxs-lookup"><span data-stu-id="7f8b2-179">Let's take a look at managing storage accounts in Azure with PowerShell</span></span>

### <a name="how-to-set-a-default-azure-subscription"></a><span data-ttu-id="7f8b2-180">Hur du anger en Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="7f8b2-180">How to set a default Azure subscription</span></span>
<span data-ttu-id="7f8b2-181">Du måste autentisera klienten miljön med Azure via Azure Active Directory-autentisering eller certifikatbaserad autentisering för att hantera Azure Storage med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-181">To manage Azure Storage using Azure PowerShell, you need to authenticate your client environment with Azure via Azure Active Directory authentication or certificate-based authentication.</span></span> <span data-ttu-id="7f8b2-182">Detaljerad information finns i [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview) kursen.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-182">For detailed information, see [How to install and configure Azure PowerShell](/powershell/azure/overview) tutorial.</span></span> <span data-ttu-id="7f8b2-183">Den här guiden använder Azure Active Directory-autentisering.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-183">This guide uses the Azure Active Directory authentication.</span></span>

1. <span data-ttu-id="7f8b2-184">Skriv följande kommando för att lägga till din Azure i Windows PowerShell ISE-kontot till den lokala PowerShell-miljön:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-184">In Windows PowerShell ISE, type the following command to add your Azure account to the local PowerShell environment:</span></span>

    ```powershell
    Add-AzureAccount
    ```

2. <span data-ttu-id="7f8b2-185">I fönstret ”logga i till Microsoft Azure” anger du e-postadress och lösenord som är associerat med ditt konto.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-185">In the "Sign in to Microsoft Azure" window, type the email address and password associated with your account.</span></span> <span data-ttu-id="7f8b2-186">Azure autentiserar och sparar autentiseringsuppgifterna och stänger sedan fönstret.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-186">Azure authenticates and saves the credential information, and then closes the window.</span></span>

3. <span data-ttu-id="7f8b2-187">Kör följande kommando för att visa Azure-konton i din lokala PowerShell-miljö, och kontrollera att ditt konto finns i listan:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-187">Next, run the following command to view the Azure accounts in your local PowerShell environment, and verify that your account is listed:</span></span>
   
    ```powershell
    Get-AzureAccount
    ```
4. <span data-ttu-id="7f8b2-188">Sedan kör du följande cmdlet om du vill visa alla prenumerationer som är anslutna till den lokala PowerShell-sessionen och kontrollerar du att din prenumeration visas:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-188">Then, run the following cmdlet to view all the subscriptions that are connected to the local PowerShell session, and verify that your subscription is listed:</span></span>

    ```powershell
    Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`
    ```
5. <span data-ttu-id="7f8b2-189">Om du vill ange en Azure-prenumeration, kör du väljer AzureSubscription-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-189">To set a default Azure subscription, run the Select-AzureSubscription cmdlet:</span></span>

    ```powershell
    $SubscriptionName = 'Your subscription Name'
    Select-AzureSubscription -SubscriptionName $SubscriptionName –Default
    ```

6. <span data-ttu-id="7f8b2-190">Kontrollera namnet på standard-prenumeration genom att köra cmdlet Get-AzureSubscription:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-190">Verify the name of the default subscription by running the Get-AzureSubscription cmdlet:</span></span>

    ```powershell
    Get-AzureSubscription -Default
    ```

7. <span data-ttu-id="7f8b2-191">Om du vill se alla tillgängliga PowerShell-cmdlets för Azure Storage, kör du:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-191">To see all the available PowerShell cmdlets for Azure Storage, run:</span></span>
    
    ```powershell
    Get-Command -Module Azure -Noun *Storage*`
    ```

### <a name="how-to-create-a-new-azure-storage-account"></a><span data-ttu-id="7f8b2-192">Så här skapar du ett nytt Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="7f8b2-192">How to create a new Azure storage account</span></span>
<span data-ttu-id="7f8b2-193">För att använda Azure storage, behöver du ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-193">To use Azure storage, you will need a storage account.</span></span> <span data-ttu-id="7f8b2-194">Du kan skapa ett nytt Azure storage-konto när du har konfigurerat datorn för att ansluta till din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-194">You can create a new Azure storage account after you have configured your computer to connect to your subscription.</span></span>

1. <span data-ttu-id="7f8b2-195">Kör cmdleten Get-AzureLocation för att hitta alla tillgängliga datacenter-platser:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-195">Run the Get-AzureLocation cmdlet to find all the available datacenter locations:</span></span>

    ```powershell
    Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes
    ```

2. <span data-ttu-id="7f8b2-196">Kör cmdlet New-AzureStorageAccount om du vill skapa ett nytt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-196">Next, run the New-AzureStorageAccount cmdlet to create a new storage account.</span></span> <span data-ttu-id="7f8b2-197">I följande exempel skapas ett nytt lagringskonto i datacentret ”USA, västra”.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-197">The following example creates a new storage account in the "West US" datacenter.</span></span>
   
    ```powershell
    $location = "West US"
    $StorageAccountName = "yourstorageaccount"
    New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location
    ```

> [!IMPORTANT]
> <span data-ttu-id="7f8b2-198">Namnet på ditt lagringskonto måste vara unikt i Azure och måste vara gemener.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-198">The name of your storage account must be unique within Azure and must be lowercase.</span></span> <span data-ttu-id="7f8b2-199">Namnkonventioner och begränsningar finns i [om Azure Lagringskonton](storage-create-storage-account.md) och [namnge och referera till behållare, Blobbar och Metadata](http://msdn.microsoft.com/library/azure/dd135715.aspx).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-199">For naming conventions and restrictions, see [About Azure Storage Accounts](storage-create-storage-account.md) and [Naming and Referencing Containers, Blobs, and Metadata](http://msdn.microsoft.com/library/azure/dd135715.aspx).</span></span>
> 
> 

### <a name="how-to-set-a-default-azure-storage-account"></a><span data-ttu-id="7f8b2-200">Hur du ställer in ett standardkonto för Azure storage</span><span class="sxs-lookup"><span data-stu-id="7f8b2-200">How to set a default Azure storage account</span></span>
<span data-ttu-id="7f8b2-201">Du kan ha flera lagringskonton i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-201">You can have multiple storage accounts in your subscription.</span></span> <span data-ttu-id="7f8b2-202">Du kan välja en av dem och ange den som standardkontot för lagring för kommandona för lagring i samma PowerShell-sessionen.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-202">You can choose one of them and set it as the default storage account for all the storage commands in the same PowerShell session.</span></span> <span data-ttu-id="7f8b2-203">På så sätt kan du köra kommandona Azure PowerShell lagring utan att explicit ange lagring-kontexten.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-203">This enables you to run the Azure PowerShell storage commands without specifying the storage context explicitly.</span></span>

1. <span data-ttu-id="7f8b2-204">Om du vill ange en standardkontot för lagring för din prenumeration kan du köra cmdlet Set-AzureSubscription.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-204">To set a default storage account for your subscription, you can run the Set-AzureSubscription cmdlet.</span></span>

    ```powershell
    $SubscriptionName = "Your subscription name"
    $StorageAccountName = "yourstorageaccount"  
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

2. <span data-ttu-id="7f8b2-205">Kör cmdleten Get-AzureSubscription för att verifiera att storage-konto är kopplat till ditt standardkonto för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-205">Next, run the Get-AzureSubscription cmdlet to verify that the storage account is associated with your default subscription account.</span></span> <span data-ttu-id="7f8b2-206">Det här kommandot returnerar prenumerationens egenskaper på den aktuella prenumerationen, inklusive dess aktuella storage-konto.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-206">This command returns the subscription properties on the current subscription including its current storage account.</span></span>

    ```powershell
    Get-AzureSubscription –Current
    ```

### <a name="how-to-list-all-azure-storage-accounts-in-a-subscription"></a><span data-ttu-id="7f8b2-207">Visa en lista över alla Azure storage-konton i en prenumeration</span><span class="sxs-lookup"><span data-stu-id="7f8b2-207">How to list all Azure storage accounts in a subscription</span></span>
<span data-ttu-id="7f8b2-208">Varje Azure-prenumeration kan ha upp till 100 storage-konton.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-208">Each Azure subscription can have up to 100 storage accounts.</span></span> <span data-ttu-id="7f8b2-209">Den senaste informationen om du begränsar finns [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-209">For the most up-to-date information on limits, see [Azure Subscription and Service Limits, Quotas, and Constraints](../azure-subscription-service-limits.md).</span></span>

<span data-ttu-id="7f8b2-210">Kör följande cmdlet för att ta reda på namn och status för storage-konton i den aktuella prenumerationen:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-210">Run the following cmdlet to find out the name and status of the storage accounts in the current subscription:</span></span>

```powershell
Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus
```

### <a name="how-to-create-an-azure-storage-context"></a><span data-ttu-id="7f8b2-211">Så här skapar du en Azure storage-kontext</span><span class="sxs-lookup"><span data-stu-id="7f8b2-211">How to create an Azure storage context</span></span>
<span data-ttu-id="7f8b2-212">Azure storage-kontexten är ett objekt i PowerShell för att kapsla in autentiseringsuppgifterna för lagring.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-212">Azure storage context is an object in PowerShell to encapsulate the storage credentials.</span></span> <span data-ttu-id="7f8b2-213">Med en kontext för lagring medan köra alla efterföljande cmdlet gör att du kan autentisera din begäran utan att ange storage-konto och dess åtkomstnyckel explicit.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-213">Using a storage context while running any subsequent cmdlet enables you to authenticate your request without specifying the storage account and its access key explicitly.</span></span> <span data-ttu-id="7f8b2-214">Du kan skapa en kontext för lagring på många sätt, till exempel med hjälp av lagringskontots namn och åtkomstnyckel, signaturtoken för delad åtkomst (SAS), anslutningssträngen, eller anonym.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-214">You can create a storage context in many ways, such as using storage account name and access key, shared access signature (SAS) token, connection string, or anonymous.</span></span> <span data-ttu-id="7f8b2-215">Mer information finns i [ny AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-215">For more information, see [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext).</span></span>  

<span data-ttu-id="7f8b2-216">Använd någon av följande tre sätt att skapa en kontext för lagring:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-216">Use one of the following three ways to create a storage context:</span></span>

* <span data-ttu-id="7f8b2-217">Kör den [Get-AzureStorageKey](/powershell/module/azure.storage/get-azurestoragekey) för att ta reda på den primära lagringsåtkomstnyckel för Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-217">Run the [Get-AzureStorageKey](/powershell/module/azure.storage/get-azurestoragekey) cmdlet to find out the primary storage access key for your Azure storage account.</span></span> <span data-ttu-id="7f8b2-218">Därefter anropar den [ny AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext) för att skapa en kontext för lagring:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-218">Next, call the [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext) cmdlet to create a storage context:</span></span>

    ```powershell
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    ```

* <span data-ttu-id="7f8b2-219">Generera en token för delad åtkomst signaturen för ett Azure storage-behållare och använda den för att skapa en kontext för lagring:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-219">Generate a shared access signature token for an Azure storage container and use it to create a storage context:</span></span>

    ```powershell
    $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken
    ```

    <span data-ttu-id="7f8b2-220">Mer information finns i [ny AzureStorageContainerSASToken](/powershell/module/azure.storage/new-azurestoragecontainersastoken) och [med delad åtkomst signaturer (SAS)](storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-220">For more information, see [New-AzureStorageContainerSASToken](/powershell/module/azure.storage/new-azurestoragecontainersastoken) and [Using Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md).</span></span>

* <span data-ttu-id="7f8b2-221">I vissa fall kanske du vill ange Tjänsteslutpunkter när du skapar en ny lagring-kontext.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-221">In some cases, you may want to specify the service endpoints when you create a new storage context.</span></span> <span data-ttu-id="7f8b2-222">Detta kan vara nödvändigt när du har registrerat ett anpassat domännamn för ditt lagringskonto med Blob-tjänsten eller du vill använda en signatur för delad åtkomst för att komma åt lagringsresurser.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-222">This might be necessary when you have registered a custom domain name for your storage account with the Blob service or you want to use a shared access signature for accessing storage resources.</span></span> <span data-ttu-id="7f8b2-223">Ange Tjänsteslutpunkter i en anslutningssträng och använda den för att skapa en ny lagring kontext enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-223">Set the service endpoints in a connection string and use it to create a new storage context as shown below:</span></span>

    ```powershell
    $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
    $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString
    ```

<span data-ttu-id="7f8b2-224">Läs mer om hur du konfigurerar en lagringsanslutningssträng [konfigurerar anslutningssträngar](storage-configure-connection-string.md).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-224">For more information on how to configure a storage connection string, see [Configuring Connection Strings](storage-configure-connection-string.md).</span></span>

<span data-ttu-id="7f8b2-225">Nu när du har skapat din dator och lärt dig hur du hanterar prenumerationer och storage-konton med hjälp av Azure PowerShell, gå till nästa avsnitt för att lära dig hur du hanterar Azure-blobbar och blob ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-225">Now that you have set up your computer and learned how to manage subscriptions and storage accounts using Azure PowerShell, go to the next section to learn how to manage Azure blobs and blob snapshots.</span></span>

### <a name="how-to-retrieve-and-regenerate-azure-storage-keys"></a><span data-ttu-id="7f8b2-226">Hur du hämtar och återskapa nycklar för Azure storage</span><span class="sxs-lookup"><span data-stu-id="7f8b2-226">How to retrieve and regenerate Azure storage keys</span></span>
<span data-ttu-id="7f8b2-227">Ett Azure Storage-konto innehåller två nycklar.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-227">An Azure Storage account comes with two account keys.</span></span> <span data-ttu-id="7f8b2-228">Du kan använda följande cmdlet för att hämta dina nycklar.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-228">You can use the following cmdlet to retrieve your keys.</span></span>

```powershell
Get-AzureStorageKey -StorageAccountName "yourstorageaccount"
```

<span data-ttu-id="7f8b2-229">Du kan använda följande cmdlet för att hämta en viss nyckel.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-229">Use the following cmdlet to retrieve a specific key.</span></span> <span data-ttu-id="7f8b2-230">Giltiga värden är primär och sekundär.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-230">Valid values are Primary and Secondary.</span></span>  

```powershell
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary
    
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary
```

<span data-ttu-id="7f8b2-231">Om du vill återskapa dina nycklar, använder du följande cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-231">If you would like to regenerate your keys, use the following cmdlet.</span></span> <span data-ttu-id="7f8b2-232">Giltiga värden för - KeyType är ”primär” och ”sekundär”</span><span class="sxs-lookup"><span data-stu-id="7f8b2-232">Valid values for -KeyType are "Primary" and "Secondary"</span></span>

```powershell
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Primary"
    
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Secondary"
```

## <a name="how-to-manage-azure-blobs"></a><span data-ttu-id="7f8b2-233">Hantera Azure-blobbar</span><span class="sxs-lookup"><span data-stu-id="7f8b2-233">How to manage Azure blobs</span></span>
<span data-ttu-id="7f8b2-234">Azure Blob storage är en tjänst för att lagra stora mängder Ostrukturerade data, till exempel text eller binära data som kan nås från var som helst i världen via HTTP eller HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-234">Azure Blob storage is a service for storing large amounts of unstructured data, such as text or binary data, that can be accessed from anywhere in the world via HTTP or HTTPS.</span></span> <span data-ttu-id="7f8b2-235">Det här avsnittet förutsätter att du redan är bekant med principerna för Azure Blob Storage-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-235">This section assumes that you are already familiar with the Azure Blob Storage Service concepts.</span></span> <span data-ttu-id="7f8b2-236">Detaljerad information finns i [komma igång med Blob storage med hjälp av .NET](storage-dotnet-how-to-use-blobs.md) och [Blob-Tjänstkoncept](http://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-236">For detailed information, see [Get started with Blob storage using .NET](storage-dotnet-how-to-use-blobs.md) and [Blob Service Concepts](http://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>

### <a name="how-to-create-a-container"></a><span data-ttu-id="7f8b2-237">Så här skapar du en behållare</span><span class="sxs-lookup"><span data-stu-id="7f8b2-237">How to create a container</span></span>
<span data-ttu-id="7f8b2-238">Varje blobb i Azure-lagring måste vara i en behållare.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-238">Every blob in Azure storage must be in a container.</span></span> <span data-ttu-id="7f8b2-239">Du kan skapa en privat behållare med hjälp av cmdlet New-AzureStorageContainer:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-239">You can create a private container using the New-AzureStorageContainer cmdlet:</span></span>

```powershell
$StorageContainerName = "yourcontainername"
New-AzureStorageContainer -Name $StorageContainerName -Permission Off
```

> [!NOTE]
> <span data-ttu-id="7f8b2-240">Det finns tre nivåer av anonym läsbehörighet: **av**, **Blob**, och **behållare**.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-240">There are three levels of anonymous read access: **Off**, **Blob**, and **Container**.</span></span> <span data-ttu-id="7f8b2-241">Om du vill förhindra anonym åtkomst till blobbar, kan du ange parametern behörighet **av**.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-241">To prevent anonymous access to blobs, set the Permission parameter to **Off**.</span></span> <span data-ttu-id="7f8b2-242">Som standard den nya behållaren är privat och kan endast användas av ägare.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-242">By default, the new container is private and can be accessed only by the account owner.</span></span> <span data-ttu-id="7f8b2-243">Du vill tillåta anonym offentlig läsbehörighet till blob-resurser, men inte till metadata för behållaren eller i listan över blobbar i behållaren anger parametern behörighet **Blob**.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-243">To allow anonymous public read access to blob resources, but not to container metadata or to the list of blobs in the container, set the Permission parameter to **Blob**.</span></span> <span data-ttu-id="7f8b2-244">Om du vill ge fullständig offentlig läsbehörighet till blob resurser, metadata för behållaren och listan över blobbar i behållaren, ange parametern behörighet **behållare**.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-244">To allow full public read access to blob resources, container metadata, and the list of blobs in the container, set the Permission parameter to **Container**.</span></span> <span data-ttu-id="7f8b2-245">Mer information finns i [Hantera anonym läsbehörighet till behållare och blobbar](storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-245">For more information, see [Manage anonymous read access to containers and blobs](storage-manage-access-to-resources.md).</span></span>
> 
> 

### <a name="how-to-upload-a-blob-into-a-container"></a><span data-ttu-id="7f8b2-246">Hur du laddar upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="7f8b2-246">How to upload a blob into a container</span></span>
<span data-ttu-id="7f8b2-247">Azure Blob Storage stöder blockblobbar och sidblobbar.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-247">Azure Blob Storage supports block blobs and page blobs.</span></span> <span data-ttu-id="7f8b2-248">Mer information finns i [förstå Blockblobbar, Tilläggsblobbar och Sidblobbar](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-248">For more information, see [Understanding Block Blobs, Append BLobs, and Page Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span></span>

<span data-ttu-id="7f8b2-249">Om du vill överföra blobbar i en behållare, kan du använda den [Set AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-249">To upload blobs in to a container, you can use the [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) cmdlet.</span></span> <span data-ttu-id="7f8b2-250">Som standard överför det här kommandot lokala filer till en blockblobb.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-250">By default, this command uploads the local files to a block blob.</span></span> <span data-ttu-id="7f8b2-251">Du kan använda parametern - BlobType om du vill ange typen för blob.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-251">To specify the type for the blob, you can use the -BlobType parameter.</span></span>

<span data-ttu-id="7f8b2-252">Följande exempel kör den [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) för att hämta alla filer i den angivna mappen och skickar dem till nästa cmdlet genom att använda pipeline.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-252">The following example runs the [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) cmdlet to get all the files in the specified folder, and then passes them to the next cmdlet by using the pipeline operator.</span></span> <span data-ttu-id="7f8b2-253">Den [Set AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) cmdlet överför lokala filer till en behållare:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-253">The [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) cmdlet uploads the local files to your container:</span></span>

```powershell
Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"
```

### <a name="how-to-download-blobs-from-a-container"></a><span data-ttu-id="7f8b2-254">Hur du hämtar blobbar från en behållare</span><span class="sxs-lookup"><span data-stu-id="7f8b2-254">How to download blobs from a container</span></span>
<span data-ttu-id="7f8b2-255">I följande exempel visar hur du ladda ned blobbar från en behållare.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-255">The following example demonstrates how to download blobs from a container.</span></span> <span data-ttu-id="7f8b2-256">I exempel först upprättar en anslutning till Azure Storage med hjälp av kontexten för lagringskontot, som innehåller lagringskontonamn och den primära åtkomstnyckeln.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-256">The example first establishes a connection to Azure Storage using the storage account context, which includes the storage account name and its primary access key.</span></span> <span data-ttu-id="7f8b2-257">Sedan exemplet hämtar ett blob-referens med den [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-257">Then, the example retrieves a blob reference using the [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet.</span></span> <span data-ttu-id="7f8b2-258">Därefter i exemplet används den [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) för att ladda ned blobbar i den lokala målmappen.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-258">Next, the example uses the [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) cmdlet to download blobs into the local destination folder.</span></span>

```powershell
#Define the variables.
$ContainerName = "yourcontainername"
$DestinationFolder = "C:\DownloadImages"

#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#List all blobs in a container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Download blobs from a container.
New-Item -Path $DestinationFolder -ItemType Directory -Force
$blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx
```

### <a name="how-to-copy-blobs-from-one-storage-container-to-another"></a><span data-ttu-id="7f8b2-259">Kopiera blobbar från en lagringsbehållare till en annan</span><span class="sxs-lookup"><span data-stu-id="7f8b2-259">How to copy blobs from one storage container to another</span></span>
<span data-ttu-id="7f8b2-260">Du kan kopiera BLOB storage-konton och regioner asynkront.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-260">You can copy blobs across storage accounts and regions asynchronously.</span></span> <span data-ttu-id="7f8b2-261">Exemplet nedan visar hur du kopierar blobbar från en lagringsbehållare till en annan i två olika lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-261">The following example demonstrates how to copy blobs from one storage container to another in two different storage accounts.</span></span> <span data-ttu-id="7f8b2-262">I exemplet först ställer in variabler för käll- och storage-konton och skapar sedan en kontext för lagring för varje konto.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-262">The example first sets variables for source and destination storage accounts, and then creates a storage context for each account.</span></span> <span data-ttu-id="7f8b2-263">Därefter exemplet kopierar blobbar från behållaren källa till mål-behållaren med den [Start AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-263">Next, the example copies blobs from the source container to the destination container using the [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) cmdlet.</span></span> <span data-ttu-id="7f8b2-264">Exemplet förutsätter att käll- och storage-konton och behållare som redan finns.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-264">The example assumes that the source and destination storage accounts and containers already exist.</span></span>

```powershell
#Define the source storage account and context.
$SourceStorageAccountName = "yoursourcestorageaccount"
$SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
$SrcContainerName = "yoursrccontainername"
$SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

#Define the destination storage account and context.
$DestStorageAccountName = "yourdeststorageaccount"
$DestStorageAccountKey = "Storage key for yourdeststorageaccount"
$DestContainerName = "destcontainername"
$DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

#Get a reference to blobs in the source container.
$blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

#Copy blobs from one container to another.
$blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext
```

<span data-ttu-id="7f8b2-265">Observera att det här exemplet utför en asynkron kopia.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-265">Note that this example performs an asynchronous copy.</span></span> <span data-ttu-id="7f8b2-266">Du kan övervaka statusen för varje kopia genom att köra den [Get-AzureStorageBlobCopyState](/powershell/module/azure.storage/start-azurestorageblobcopystate) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-266">You can monitor the status of each copy by running the [Get-AzureStorageBlobCopyState](/powershell/module/azure.storage/start-azurestorageblobcopystate) cmdlet.</span></span>

### <a name="how-to-copy-blobs-from-a-secondary-location"></a><span data-ttu-id="7f8b2-267">Hur du kopierar blobar från en sekundär plats</span><span class="sxs-lookup"><span data-stu-id="7f8b2-267">How to copy blobs from a secondary location</span></span>
<span data-ttu-id="7f8b2-268">Du kan kopiera blobbar från den sekundära platsen för RA-GRS-aktiverat konto.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-268">You can copy blobs from the secondary location of a RA-GRS-enabled account.</span></span>

```powershell
#define secondary storage context using a connection string constructed from secondary endpoints.
$SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext
```

### <a name="how-to-delete-a-blob"></a><span data-ttu-id="7f8b2-269">Hur du tar bort en blobb</span><span class="sxs-lookup"><span data-stu-id="7f8b2-269">How to delete a blob</span></span>
<span data-ttu-id="7f8b2-270">Om du vill ta bort en blobb först hämta en blobbreferens och anropa sedan cmdleten Remove-AzureStorageBlob på den.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-270">To delete a blob, first get a blob reference and then call the Remove-AzureStorageBlob cmdlet on it.</span></span> <span data-ttu-id="7f8b2-271">I följande exempel tar bort alla blobbar i en viss behållare.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-271">The following example deletes all the blobs in a given container.</span></span> <span data-ttu-id="7f8b2-272">I exemplet först ställer in variabler för ett lagringskonto och skapar sedan en kontext för lagring.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-272">The example first sets variables for a storage account, and then creates a storage context.</span></span> <span data-ttu-id="7f8b2-273">Därefter exemplet hämtar ett blob-referens med den [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet och kör den [ta bort AzureStorageBlob](/powershell/module/azure.storage/remove-azurestorageblob) för att ta bort blobbar från en behållare i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-273">Next, the example retrieves a blob reference using the [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet and runs the [Remove-AzureStorageBlob](/powershell/module/azure.storage/remove-azurestorageblob) cmdlet to remove blobs from a container in Azure storage.</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "containername"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference to all the blobs in the container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Delete blobs in a specified container.
$blobs| Remove-AzureStorageBlob
```

## <a name="how-to-manage-azure-blob-snapshots"></a><span data-ttu-id="7f8b2-274">Hur du hanterar Azure blob-ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="7f8b2-274">How to manage Azure blob snapshots</span></span>
<span data-ttu-id="7f8b2-275">Azure kan du skapa en ögonblicksbild av en blob.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-275">Azure lets you create a snapshot of a blob.</span></span> <span data-ttu-id="7f8b2-276">En ögonblicksbild är en skrivskyddad version av en blob som utförs i en punkt i tiden.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-276">A snapshot is a read-only version of a blob that's taken at a point in time.</span></span> <span data-ttu-id="7f8b2-277">När en ögonblicksbild har skapats, kan den läsa, kopieras, eller ta bort, men inte har ändrats.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-277">Once a snapshot has been created, it can be read, copied, or deleted, but not modified.</span></span> <span data-ttu-id="7f8b2-278">Ögonblicksbilder är ett sätt att säkerhetskopiera en blob som det visas vid en tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-278">Snapshots provide a way to back up a blob as it appears at a moment in time.</span></span> <span data-ttu-id="7f8b2-279">Mer information finns i [skapa en ögonblicksbild av en Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-279">For more information, see [Creating a Snapshot of a Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span></span>

### <a name="how-to-create-a-blob-snapshot"></a><span data-ttu-id="7f8b2-280">Så här skapar du en blob-ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="7f8b2-280">How to create a blob snapshot</span></span>
<span data-ttu-id="7f8b2-281">Om du vill skapa en snaphot för en blobb först hämta en blobbreferens och sedan anropa den `ICloudBlob.CreateSnapshot` metoden på den.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-281">To create a snaphot of a blob, first get a blob reference and then call the `ICloudBlob.CreateSnapshot` method on it.</span></span> <span data-ttu-id="7f8b2-282">I följande exempel först ställer in variabler för ett lagringskonto och skapar sedan en kontext för lagring.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-282">The following example first sets variables for a storage account, and then creates a storage context.</span></span> <span data-ttu-id="7f8b2-283">Därefter exemplet hämtar ett blob-referens med den [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet och kör den [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) metod för att skapa en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-283">Next, the example retrieves a blob reference using the [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet and runs the [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) method to create a snapshot.</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "yourcontainername"
$BlobName = "yourblobname"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference to a blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

#Create a snapshot of the blob.
$snap = $blob.ICloudBlob.CreateSnapshot()
```

### <a name="how-to-list-a-blobs-snapshots"></a><span data-ttu-id="7f8b2-284">Visa en lista över en blob ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="7f8b2-284">How to list a blob's snapshots</span></span>
<span data-ttu-id="7f8b2-285">Du kan skapa så många ögonblicksbilder som du vill använda för en blob.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-285">You can create as many snapshots as you want for a blob.</span></span> <span data-ttu-id="7f8b2-286">Du kan visa ögonblicksbilder kopplade till ditt blob att spåra din aktuella ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-286">You can list the snapshots associated with your blob to track your current snapshots.</span></span> <span data-ttu-id="7f8b2-287">I följande exempel används en fördefinierad blob och anrop i [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) för att visa en lista med ögonblicksbilder för blobben.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-287">The following example uses a predefined blob and calls the [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet to list the snapshots of that blob.</span></span>  

```powershell
#Define the blob name.
$BlobName = "yourblobname"

#List the snapshots of a blob.
Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }
```

### <a name="how-to-copy-a-snapshot-of-a-blob"></a><span data-ttu-id="7f8b2-288">Så här kopierar du en ögonblicksbild av en blob</span><span class="sxs-lookup"><span data-stu-id="7f8b2-288">How to copy a snapshot of a blob</span></span>
<span data-ttu-id="7f8b2-289">Du kan kopiera en ögonblicksbild av en blob att återställa ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-289">You can copy a snapshot of a blob to restore the snapshot.</span></span> <span data-ttu-id="7f8b2-290">Detaljerad information och begränsningar finns i [skapa en ögonblicksbild av en Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-290">For detailed information and restrictions, see [Creating a Snapshot of a Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span></span> <span data-ttu-id="7f8b2-291">I följande exempel först ställer in variabler för ett lagringskonto och skapar sedan en kontext för lagring.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-291">The following example first sets variables for a storage account, and then creates a storage context.</span></span> <span data-ttu-id="7f8b2-292">Exemplet definierar sedan variablerna behållare och blob namn.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-292">Next, the example defines the container and blob name variables.</span></span> <span data-ttu-id="7f8b2-293">I exempel hämtar ett blob-referens med den [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet och kör den [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) metod för att skapa en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-293">The example retrieves a blob reference using the [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet and runs the [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) method to create a snapshot.</span></span> <span data-ttu-id="7f8b2-294">Sedan exemplet körs den [Start AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) för att kopiera ögonblicksbilden av en blob med ICloudBlob-objektet för käll-blob.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-294">Then, the example runs the [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) cmdlet to copy the snapshot of a blob using the ICloudBlob object for the source blob.</span></span> <span data-ttu-id="7f8b2-295">Glöm inte att uppdatera variablerna baserat på din konfiguration innan du kör exemplet.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-295">Be sure to update the variables based on your configuration before running the example.</span></span> <span data-ttu-id="7f8b2-296">Observera att följande exempel förutsätter att källan och målet behållare och käll-blob finns redan.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-296">Note that the following example assumes that the source and destination containers, and the source blob already exist.</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Define the variables.
$SrcContainerName = "yoursourcecontainername"
$DestContainerName = "yourdestcontainername"
$SrcBlobName = "yourblobname"
$DestBlobName = "CopyBlobName"

#Get a reference to a blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

#Create a snapshot of a blob.
$snap = $blob.ICloudBlob.CreateSnapshot()

#Copy the snapshot to another container.
Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName
```

<span data-ttu-id="7f8b2-297">Nu när du har lärt dig hur du hanterar Azure-blobbar och blob-ögonblicksbilder med Azure PowerShell, gå till nästa avsnitt lära dig hur du hanterar tabeller, köer och filer.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-297">Now that you have learned how to manage Azure blobs and blob snapshots with Azure PowerShell, go to the next section to learn how to manage tables, queues, and files.</span></span>

## <a name="how-to-manage-azure-tables-and-table-entities"></a><span data-ttu-id="7f8b2-298">Hantera Azure-tabeller och tabellentiteter</span><span class="sxs-lookup"><span data-stu-id="7f8b2-298">How to manage Azure tables and table entities</span></span>
<span data-ttu-id="7f8b2-299">Azure Table storage-tjänsten är en NoSQL-datalager som du kan använda för att lagra och fråga stora mängder strukturerad, icke-relationella data.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-299">Azure Table storage service is a NoSQL datastore, which you can use to store and query huge sets of structured, non-relational data.</span></span> <span data-ttu-id="7f8b2-300">Huvudkomponenterna i tjänsten är tabeller, enheter och egenskaper.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-300">The main components of the service are tables, entities, and properties.</span></span> <span data-ttu-id="7f8b2-301">En tabell är en samling med entiteter.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-301">A table is a collection of entities.</span></span> <span data-ttu-id="7f8b2-302">En entitet är en uppsättning egenskaper.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-302">An entity is a set of properties.</span></span> <span data-ttu-id="7f8b2-303">Varje entitet kan ha upp till 252 egenskaper, som är alla namn / värde-par.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-303">Each entity can have up to 252 properties, which are all name-value pairs.</span></span> <span data-ttu-id="7f8b2-304">Det här avsnittet förutsätter att du redan är bekant med principerna för Azure Table Storage-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-304">This section assumes that you are already familiar with the Azure Table Storage Service concepts.</span></span> <span data-ttu-id="7f8b2-305">Detaljerad information finns i [förstå den tabelltjänst-datamodellen](http://msdn.microsoft.com/library/azure/dd179338.aspx) och [komma igång med Azure Table storage med hjälp av .NET](storage-dotnet-how-to-use-tables.md).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-305">For detailed information, see [Understanding the Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx) and [Get started with Azure Table storage using .NET](storage-dotnet-how-to-use-tables.md).</span></span>

<span data-ttu-id="7f8b2-306">I följande underavsnitt lär du dig hur du hanterar Azure Table storage-tjänsten med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-306">In the following subsections, you'll learn how to manage Azure Table storage service using Azure PowerShell.</span></span> <span data-ttu-id="7f8b2-307">Scenarier som tas upp inkluderar **skapar**, **bort**, och **hämtar** **tabeller**, samt **att lägga till**, **frågar**, och **bort tabellentiteter**.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-307">The scenarios covered include **creating**, **deleting**, and **retrieving** **tables**, as well as **adding**, **querying**, and **deleting table entities**.</span></span>

### <a name="how-to-create-a-table"></a><span data-ttu-id="7f8b2-308">Så här skapar du en tabell</span><span class="sxs-lookup"><span data-stu-id="7f8b2-308">How to create a table</span></span>
<span data-ttu-id="7f8b2-309">Varje tabell måste finnas i ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-309">Every table must reside in an Azure storage account.</span></span> <span data-ttu-id="7f8b2-310">I följande exempel visar hur du skapar en tabell i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-310">The following example demonstrates how to create a table in Azure Storage.</span></span> <span data-ttu-id="7f8b2-311">I exempel först upprättar en anslutning till Azure Storage med hjälp av kontexten för lagringskontot, som innehåller lagringskontonamn och dess snabbtangent.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-311">The example first establishes a connection to Azure Storage using the storage account context, which includes the storage account name and its access key.</span></span> <span data-ttu-id="7f8b2-312">Därefter används den [ny AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) för att skapa en tabell i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-312">Next, it uses the [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) cmdlet to create a table in Azure Storage.</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

#Create a new table.
$tabName = "yourtablename"
New-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-to-retrieve-a-table"></a><span data-ttu-id="7f8b2-313">Hur du hämtar en tabell</span><span class="sxs-lookup"><span data-stu-id="7f8b2-313">How to retrieve a table</span></span>
<span data-ttu-id="7f8b2-314">Du kan fråga efter och hämta en eller alla tabeller i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-314">You can query and retrieve one or all tables in a Storage account.</span></span> <span data-ttu-id="7f8b2-315">I följande exempel visar hur du hämtar en given tabell med hjälp av den [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-315">The following example demonstrates how to retrieve a given table using the [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span>

```powershell
#Retrieve a table.
$tabName = "yourtablename"
Get-AzureStorageTable –Name $tabName –Context $Ctx
```

<span data-ttu-id="7f8b2-316">Om du anropar cmdleten Get-AzureStorageTable utan några parametrar hämtar alla lagringstabeller som för ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-316">If you call the Get-AzureStorageTable cmdlet without any parameters, it gets all storage tables for a Storage account.</span></span>

### <a name="how-to-delete-a-table"></a><span data-ttu-id="7f8b2-317">Hur du tar bort en tabell</span><span class="sxs-lookup"><span data-stu-id="7f8b2-317">How to delete a table</span></span>
<span data-ttu-id="7f8b2-318">Du kan ta bort en tabell från ett lagringskonto med hjälp av den [ta bort AzureStorageTable](/powershell/module/azure.storage/remove-azurestoragetable) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-318">You can delete a table from a storage account by using the [Remove-AzureStorageTable](/powershell/module/azure.storage/remove-azurestoragetable) cmdlet.</span></span>  

```powershell
#Delete a table.
$tabName = "yourtablename"
Remove-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-to-manage-table-entities"></a><span data-ttu-id="7f8b2-319">Så här hanterar du tabellentiteter</span><span class="sxs-lookup"><span data-stu-id="7f8b2-319">How to manage table entities</span></span>
<span data-ttu-id="7f8b2-320">För närvarande tillhandahåller inte Azure PowerShell-cmdletar för att hantera tabellentiteter direkt.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-320">Currently, Azure PowerShell does not provide cmdlets to manage table entities directly.</span></span> <span data-ttu-id="7f8b2-321">Om du vill utföra åtgärder på tabellentiteter, du kan använda de klasser som anges i den [Azure Storage-klientbibliotek för .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-321">To perform operations on table entities, you can use the classes given in the [Azure Storage Client Library for .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).</span></span>

#### <a name="how-to-add-table-entities"></a><span data-ttu-id="7f8b2-322">Hur du lägger till tabellentiteter</span><span class="sxs-lookup"><span data-stu-id="7f8b2-322">How to add table entities</span></span>
<span data-ttu-id="7f8b2-323">Om du vill lägga till en entitet i en tabell, först skapa ett objekt som definierar egenskaper för enhet.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-323">To add an entity to a table, first create an object that defines your entity properties.</span></span> <span data-ttu-id="7f8b2-324">En entitet kan ha upp till 255 egenskaper, inklusive 3 Systemegenskaper: **PartitionKey**, **RowKey**, och **tidsstämpel**.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-324">An entity can have up to 255 properties, including 3 system properties: **PartitionKey**, **RowKey**, and **Timestamp**.</span></span> <span data-ttu-id="7f8b2-325">Du ansvarar för att lägga till och uppdatera värdena för **PartitionKey** och **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-325">You are responsible for inserting and updating the values of **PartitionKey** and **RowKey**.</span></span> <span data-ttu-id="7f8b2-326">Servern hanterar värde för **tidsstämpel**, som inte ändras.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-326">The server manages the value of **Timestamp**, which cannot be modified.</span></span> <span data-ttu-id="7f8b2-327">Tillsammans i **PartitionKey** och **RowKey** identifiera varje entitet i en tabell.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-327">Together the **PartitionKey** and **RowKey** uniquely identify every entity within a table.</span></span>

* <span data-ttu-id="7f8b2-328">**PartitionKey**: Anger den partition som entiteten lagras i.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-328">**PartitionKey**: Determines the partition that the entity is stored in.</span></span>
* <span data-ttu-id="7f8b2-329">**RowKey**: identifierar entiteten i partitionen.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-329">**RowKey**: Uniquely identifies the entity within the partition.</span></span>

<span data-ttu-id="7f8b2-330">Du kan ange upp till 252 anpassade egenskaper för en entitet.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-330">You may define up to 252 custom properties for an entity.</span></span> <span data-ttu-id="7f8b2-331">Mer information finns i [förstå den tabelltjänst-datamodellen](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-331">For more information, see [Understanding the Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span></span>

<span data-ttu-id="7f8b2-332">I följande exempel visar hur du lägger till entiteter i en tabell.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-332">The following example demonstrates how to add entities to a table.</span></span> <span data-ttu-id="7f8b2-333">Exemplet visar hur du hämtar tabellen Personal och lägga till flera enheter till den.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-333">The example shows how to retrieve the employee table and add several entities into it.</span></span> <span data-ttu-id="7f8b2-334">Först upprättar den en anslutning till Azure Storage med hjälp av kontexten för lagringskontot, som innehåller lagringskontonamn och dess snabbtangent.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-334">First, it establishes a connection to Azure Storage using the storage account context, which includes the storage account name and its access key.</span></span> <span data-ttu-id="7f8b2-335">Sedan hämtar den angivna tabellen med hjälp av den [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-335">Next, it retrieves the given table using the [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span> <span data-ttu-id="7f8b2-336">Om tabellen inte finns i [ny AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) cmdlet som används för att skapa en tabell i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-336">If the table does not exist, the [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) cmdlet is used to create a table in Azure Storage.</span></span> <span data-ttu-id="7f8b2-337">Därefter definierar exemplet en egen funktion Add-enheten för att lägga till entiteter i tabellen genom att ange varje entitet partition och radnyckel.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-337">Next, the example defines a custom function Add-Entity to add entities to the table by specifying each entity's partition and row key.</span></span> <span data-ttu-id="7f8b2-338">Lägg till entiteten funktionsanrop den [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdleten på den [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) klassen för att skapa ett entitetsobjekt.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-338">The Add-Entity function calls the [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet on the [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) class to create an entity object.</span></span> <span data-ttu-id="7f8b2-339">Senare exemplet anropar den [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) -metoden i den här enhetsobjekt lägga till den i en tabell.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-339">Later, the example calls the [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) method on this entity object to add it to a table.</span></span>

```powershell
#Function Add-Entity: Adds an employee entity to a table.
function Add-Entity() {
    [CmdletBinding()]
    param(
       $table,
       [String]$partitionKey,
       [String]$rowKey,
       [String]$name,
       [Int]$id
    )

  $entity = New-Object -TypeName Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity -ArgumentList $partitionKey, $rowKey
  $entity.Properties.Add("Name", $name)
  $entity.Properties.Add("ID", $id)

  $result = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Insert($entity))
}

#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Retrieve the table if it already exists.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

#Create a new table if it does not exist.
if ($table -eq $null)
{
   $table = New-AzureStorageTable –Name $TableName -Context $Ctx
}

#Add multiple entities to a table.
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4
```

#### <a name="how-to-query-table-entities"></a><span data-ttu-id="7f8b2-340">Hur man frågar tabellentiteter</span><span class="sxs-lookup"><span data-stu-id="7f8b2-340">How to query table entities</span></span>
<span data-ttu-id="7f8b2-341">Om du vill fråga en tabell, använder den [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) klass.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-341">To query a table, use the [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) class.</span></span> <span data-ttu-id="7f8b2-342">Följande exempel förutsätter att du redan har kört skriptet i hur du lägger till entiteter avsnitt i handboken.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-342">The following example assumes that you've already run the script given in the How to add entities section of this guide.</span></span> <span data-ttu-id="7f8b2-343">I exempel först upprättar en anslutning till Azure Storage med hjälp av lagring kontext, som innehåller lagringskontonamn och dess snabbtangent.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-343">The example first establishes a connection to Azure Storage using the storage context, which includes the storage account name and its access key.</span></span> <span data-ttu-id="7f8b2-344">Därefter görs ett försök att hämta den tidigare skapade ”anställda” tabell genom att använda den [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-344">Next, it tries to retrieve the previously created "Employees" table using the [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span> <span data-ttu-id="7f8b2-345">Anropar den [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet på klassen Microsoft.WindowsAzure.Storage.Table.TableQuery skapar ett nytt frågeobjekt.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-345">Calling the [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet on the Microsoft.WindowsAzure.Storage.Table.TableQuery class creates a new query object.</span></span> <span data-ttu-id="7f8b2-346">I exempel söker efter de enheter som har en ”ID”-kolumn vars värde är 1 som anges i ett sträng-filter.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-346">The example looks for the entities that have an 'ID' column whose value is 1 as specified in a string filter.</span></span> <span data-ttu-id="7f8b2-347">Detaljerad information finns i [frågor till tabeller och de entiteter](http://msdn.microsoft.com/library/azure/dd894031.aspx).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-347">For detailed information, see [Querying Tables and Entities](http://msdn.microsoft.com/library/azure/dd894031.aspx).</span></span> <span data-ttu-id="7f8b2-348">När du kör den här frågan returnerar alla entiteter som matchar filterkriterierna.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-348">When you run this query, it returns all entities that match the filter criteria.</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Get a reference to a table.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx

#Create a table query.
$query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

#Define columns to select.
$list = New-Object System.Collections.Generic.List[string]
$list.Add("RowKey")
$list.Add("ID")
$list.Add("Name")

#Set query details.
$query.FilterString = "ID gt 0"
$query.SelectColumns = $list
$query.TakeCount = 20

#Execute the query.
$entities = $table.CloudTable.ExecuteQuery($query)

#Display entity properties with the table format.
$entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize
```

#### <a name="how-to-delete-table-entities"></a><span data-ttu-id="7f8b2-349">Ta bort tabellentiteter</span><span class="sxs-lookup"><span data-stu-id="7f8b2-349">How to delete table entities</span></span>
<span data-ttu-id="7f8b2-350">Du kan ta bort en entitet med dess partition och raden nycklar.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-350">You can delete an entity using its partition and row keys.</span></span> <span data-ttu-id="7f8b2-351">Följande exempel förutsätter att du redan har kört skriptet i hur du lägger till entiteter avsnitt i handboken.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-351">The following example assumes that you've already run the script given in the How to add entities section of this guide.</span></span> <span data-ttu-id="7f8b2-352">I exempel först upprättar en anslutning till Azure Storage med hjälp av lagring kontext, som innehåller lagringskontonamn och dess snabbtangent.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-352">The example first establishes a connection to Azure Storage using the storage context, which includes the storage account name and its access key.</span></span> <span data-ttu-id="7f8b2-353">Därefter görs ett försök att hämta den tidigare skapade ”anställda” tabell genom att använda den [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-353">Next, it tries to retrieve the previously created "Employees" table using the [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span> <span data-ttu-id="7f8b2-354">Om tabellen finns i exemplet anropar den [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) metod för att hämta en entitet baserat på dess partition och raden nyckelvärden.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-354">If the table exists, the example calls the [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) method to retrieve an entity based on its partition and row key values.</span></span> <span data-ttu-id="7f8b2-355">Skickar sedan enheten till den [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) metod för att ta bort.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-355">Then, pass the entity to the [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) method to delete.</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve the table.
$TableName = "Employees"
$table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

#If the table exists, start deleting its entities.
if ($table -ne $null) 
{
    #Together the PartitionKey and RowKey uniquely identify every  
    #entity within a table.
    $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
    $entity = $tableResult.Result
    if ($entity -ne $null)
    {
        #Delete the entity.
        $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
}
```

## <a name="how-to-manage-azure-queues-and-queue-messages"></a><span data-ttu-id="7f8b2-356">Hantera Azure-köer och -meddelanden i kö</span><span class="sxs-lookup"><span data-stu-id="7f8b2-356">How to manage Azure queues and queue messages</span></span>
<span data-ttu-id="7f8b2-357">Azure Queue Storage är en tjänst för att lagra stora mängder meddelanden som kan nås från var som helst i världen via autentiserade anrop med HTTP eller HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-357">Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in the world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="7f8b2-358">Det här avsnittet förutsätter att du redan är bekant med Azure Queue Storage Service-principerna.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-358">This section assumes that you are already familiar with the Azure Queue Storage Service concepts.</span></span> <span data-ttu-id="7f8b2-359">Detaljerad information finns i [komma igång med Azure Queue storage med hjälp av .NET](storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-359">For detailed information, see [Get started with Azure Queue storage using .NET](storage-dotnet-how-to-use-queues.md).</span></span>

<span data-ttu-id="7f8b2-360">Det här avsnittet visas hur du hanterar Azure Queue storage-tjänst med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-360">This section will show you how to manage Azure Queue storage service using Azure PowerShell.</span></span> <span data-ttu-id="7f8b2-361">Scenarier som tas upp inkluderar **infoga** och **bort** kö meddelanden, samt **skapar**, **ta bort**, och **hämta köer**.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-361">The scenarios covered include **inserting** and **deleting** queue messages, as well as **creating**, **deleting**, and **retrieving queues**.</span></span>

### <a name="how-to-create-a-queue"></a><span data-ttu-id="7f8b2-362">Så här skapar du en kö</span><span class="sxs-lookup"><span data-stu-id="7f8b2-362">How to create a queue</span></span>
<span data-ttu-id="7f8b2-363">I följande exempel upprättar en anslutning till Azure Storage med hjälp av kontexten för lagringskontot, som innehåller lagringskontonamn och dess åtkomstnyckel först.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-363">The following example first establishes a connection to Azure Storage using the storage account context, which includes the storage account name and its access key.</span></span> <span data-ttu-id="7f8b2-364">Därefter anropar [ny AzureStorageQueue](/powershell/module/azure.storage/new-azurestoragequeue) för att skapa en kö med namnet 'könamn'.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-364">Next, it calls [New-AzureStorageQueue](/powershell/module/azure.storage/new-azurestoragequeue) cmdlet to create a queue named 'queuename'.</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$QueueName = "queuename"
$Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx
```

<span data-ttu-id="7f8b2-365">Information om namngivningskonventioner för Azure-kötjänsten finns [namngivning av köer och Metadata](http://msdn.microsoft.com/library/azure/dd179349.aspx).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-365">For information on naming conventions for Azure Queue Service, see [Naming Queues and Metadata](http://msdn.microsoft.com/library/azure/dd179349.aspx).</span></span>

### <a name="how-to-retrieve-a-queue"></a><span data-ttu-id="7f8b2-366">Så här hämtar du en kö</span><span class="sxs-lookup"><span data-stu-id="7f8b2-366">How to retrieve a queue</span></span>
<span data-ttu-id="7f8b2-367">Du kan fråga efter och hämta en särskild kö eller en lista över alla köer i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-367">You can query and retrieve a specific queue or a list of all the queues in a Storage account.</span></span> <span data-ttu-id="7f8b2-368">I följande exempel visar hur du hämtar en kö som anges med hjälp av den [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-368">The following example demonstrates how to retrieve a specified queue using the [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) cmdlet.</span></span>

```powershell
#Retrieve a queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx
```

<span data-ttu-id="7f8b2-369">Om du anropar den [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) cmdlet utan några parametrar får den en lista över alla köer.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-369">If you call the [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) cmdlet without any parameters, it gets a list of all the queues.</span></span>

### <a name="how-to-delete-a-queue"></a><span data-ttu-id="7f8b2-370">Hur du tar bort en kö</span><span class="sxs-lookup"><span data-stu-id="7f8b2-370">How to delete a queue</span></span>
<span data-ttu-id="7f8b2-371">Anropa Remove-AzureStorageQueue cmdlet för att ta bort en kö och alla meddelanden i den.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-371">To delete a queue and all the messages contained in it, call the Remove-AzureStorageQueue cmdlet.</span></span> <span data-ttu-id="7f8b2-372">I följande exempel visas hur du tar bort en kö som anges med cmdlet Remove-AzureStorageQueue.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-372">The following example shows how to delete a specified queue using the Remove-AzureStorageQueue cmdlet.</span></span>

```powershell
#Delete a queue.
$QueueName = "yourqueuename"
Remove-AzureStorageQueue –Name $QueueName –Context $Ctx
```

#### <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="7f8b2-373">Infoga ett meddelande i en kö</span><span class="sxs-lookup"><span data-stu-id="7f8b2-373">How to insert a message into a queue</span></span>
<span data-ttu-id="7f8b2-374">Om du vill infoga ett meddelande i en befintlig kö, först skapa en ny instans av den [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) klass.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-374">To insert a message into an existing queue, first create a new instance of the [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) class.</span></span> <span data-ttu-id="7f8b2-375">Därefter anropar du [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx)-metoden.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-375">Next, call the [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) method.</span></span> <span data-ttu-id="7f8b2-376">En CloudQueueMessage kan skapas från en sträng (i UTF-8-format) eller en byte-matris.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-376">A CloudQueueMessage can be created from either a string (in UTF-8 format) or a byte array.</span></span>

<span data-ttu-id="7f8b2-377">Exemplet nedan visar hur du lägger till meddelandet till en kö.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-377">The following example demonstrates how to add message to a queue.</span></span> <span data-ttu-id="7f8b2-378">I exempel först upprättar en anslutning till Azure Storage med hjälp av kontexten för lagringskontot, som innehåller lagringskontonamn och dess snabbtangent.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-378">The example first establishes a connection to Azure Storage using the storage account context, which includes the storage account name and its access key.</span></span> <span data-ttu-id="7f8b2-379">Sedan hämtar den kö som anges med hjälp av den [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-379">Next, it retrieves the specified queue using the [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) cmdlet.</span></span> <span data-ttu-id="7f8b2-380">Om kön finns på [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet som används för att skapa en instans av den [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) klass.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-380">If the queue exists, the [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet is used to create an instance of the [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) class.</span></span> <span data-ttu-id="7f8b2-381">Senare exemplet anropar den [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) -metoden i det här meddelandeobjektet att lägga till en kö.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-381">Later, the example calls the [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) method on this message object to add it to a queue.</span></span> <span data-ttu-id="7f8b2-382">Här är kod som hämtar en kö och infogar meddelandet ”MessageInfo':</span><span class="sxs-lookup"><span data-stu-id="7f8b2-382">Here is code which retrieves a queue and inserts the message 'MessageInfo':</span></span>

```powershell
#Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve the queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

#If the queue exists, add a new message.
if ($Queue -ne $null) {
   # Create a new message using a constructor of the CloudQueueMessage class.
   $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

   # Add a new message to the queue.
   $Queue.CloudQueue.AddMessage($QueueMessage)
}
```

#### <a name="how-to-de-queue-at-the-next-message"></a><span data-ttu-id="7f8b2-383">Så här frigör kö nästa meddelande</span><span class="sxs-lookup"><span data-stu-id="7f8b2-383">How to de-queue at the next message</span></span>
<span data-ttu-id="7f8b2-384">Koden tar bort ett meddelande från en kö i två steg.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-384">Your code de-queues a message from a queue in two steps.</span></span> <span data-ttu-id="7f8b2-385">När du anropar den [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) metod du hämta nästa meddelande i en kö.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-385">When you call the [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) method, you get the next message in a queue.</span></span> <span data-ttu-id="7f8b2-386">Ett meddelande som returneras från **GetMessage** blir osynligt för andra meddelanden som läser kod i den här kön.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-386">A message returned from **GetMessage** becomes invisible to any other code reading messages from this queue.</span></span> <span data-ttu-id="7f8b2-387">Om du vill slutföra borttagningen av meddelandet från kön, måste du också anropa den [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) metod.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-387">To finish removing the message from the queue, you must also call the [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) method.</span></span> <span data-ttu-id="7f8b2-388">Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer att om din kod inte kan bearbeta ett meddelande på grund av ett maskin- eller programvarufel så kan en annan instans av koden hämta samma meddelande och försöka igen.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-388">This two-step process of removing a message assures that if your code fails to process a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="7f8b2-389">Koden anropar **DeleteMessage** direkt efter att meddelandet har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-389">Your code calls **DeleteMessage** right after the message has been processed.</span></span>

```powershell
# Define the storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

# Retrieve the queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

$InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

# Get the message object from the queue.
$QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
# Delete the message.
$Queue.CloudQueue.DeleteMessage($QueueMessage)
```

## <a name="how-to-manage-azure-file-shares-and-files"></a><span data-ttu-id="7f8b2-390">Hantera Azure-filresurser och filer</span><span class="sxs-lookup"><span data-stu-id="7f8b2-390">How to manage Azure file shares and files</span></span>
<span data-ttu-id="7f8b2-391">Azure File storage erbjuder delad lagring för program som använder SMB-standardprotokollet.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-391">Azure File storage offers shared storage for applications using the standard SMB protocol.</span></span> <span data-ttu-id="7f8b2-392">Microsoft Azure-datorer och molntjänster kan dela fildata över programkomponenter via monterade resurser och lokala program kan komma åt fildata på en resurs via fil-API för storage eller Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-392">Microsoft Azure virtual machines and cloud services can share file data across application components via mounted shares, and on-premises applications can access file data in a share via the File storage API or Azure PowerShell.</span></span>

<span data-ttu-id="7f8b2-393">Mer information om Azure File storage finns [Kom igång med Azure File storage i Windows](storage-dotnet-how-to-use-files.md) och [File Service REST API](http://msdn.microsoft.com/library/azure/dn167006.aspx).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-393">For more information on Azure File storage, see [Get started with Azure File storage on Windows](storage-dotnet-how-to-use-files.md) and [File Service REST API](http://msdn.microsoft.com/library/azure/dn167006.aspx).</span></span>

## <a name="how-to-set-and-query-storage-analytics"></a><span data-ttu-id="7f8b2-394">Hur du ställer in och frågan storage analytics</span><span class="sxs-lookup"><span data-stu-id="7f8b2-394">How to set and query storage analytics</span></span>
<span data-ttu-id="7f8b2-395">Du kan använda [Azure Storage Analytics](storage-analytics.md) samla in statistik för Azure storage-konton och logga in data om förfrågningar som skickas till ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-395">You can use [Azure Storage Analytics](storage-analytics.md) to collect metrics for your Azure storage accounts and log data about requests sent to your storage account.</span></span> <span data-ttu-id="7f8b2-396">Du kan använda storage-mätvärden för att övervaka hälsotillståndet för ett lagringskonto och lagring loggning för att diagnostisera och felsöka problem med ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-396">You can use storage metrics to monitor the health of a storage account, and storage logging to diagnose and troubleshoot issues with your storage account.</span></span> <span data-ttu-id="7f8b2-397">Du kan konfigurera övervakning med Azure-portalen eller Windows PowerShell eller via programmering med storage-klientbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-397">You can configure monitoring using the Azure portal or Windows PowerShell, or programmatically using the storage client library.</span></span> <span data-ttu-id="7f8b2-398">Lagring loggning sker serversidan och gör att du kan registrera information för både slutförda och misslyckade begäranden på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-398">Storage logging happens server-side and enables you to record details for both successful and failed requests in your storage account.</span></span> <span data-ttu-id="7f8b2-399">Dessa loggar kan du se detaljer för Läs-, Skriv- och delete-åtgärder mot dina tabeller, köer, blobbar samt och orsakerna till misslyckade begäranden.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-399">These logs enable you to see details of read, write, and delete operations against your tables, queues, and blobs as well as the reasons for failed requests.</span></span>

<span data-ttu-id="7f8b2-400">Information om hur du aktiverar och visa Storage Metrics data med hjälp av PowerShell finns [aktivera mätvärden för lagring med hjälp av PowerShell](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-400">To learn how to enable and view Storage Metrics data using PowerShell, see [How to enable Storage Metrics using PowerShell](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).</span></span>

<span data-ttu-id="7f8b2-401">Information om hur du aktiverar och hämta data för loggning av lagring med hjälp av PowerShell finns [Aktivera lagring loggning med hjälp av PowerShell](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) och [hitta din lagring loggning loggdata](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-401">To learn how to enable and retrieve Storage Logging data using PowerShell, see [How to enable Storage Logging using PowerShell](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) and [Finding your Storage Logging log data](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).</span></span>
<span data-ttu-id="7f8b2-402">Detaljerad information om hur du använder Storage-mätvärden och lagring loggning för felsökning av problem med lagring finns [övervakning, diagnostisera och felsöka Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-402">For detailed information on using Storage Metrics and Storage Logging to troubleshoot storage issues, see [Monitoring, Diagnosing, and Troubleshooting Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).</span></span>

## <a name="how-to-manage-shared-access-signature-sas-and-stored-access-policy"></a><span data-ttu-id="7f8b2-403">Hantera delade signatur åtkomst (SAS) och lagras åtkomstprincipen</span><span class="sxs-lookup"><span data-stu-id="7f8b2-403">How to manage Shared Access Signature (SAS) and Stored Access Policy</span></span>
<span data-ttu-id="7f8b2-404">Signaturer för delad åtkomst är en viktig del av säkerhetsmodellen för alla program som använder Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-404">Shared access signatures are an important part of the security model for any application using Azure Storage.</span></span> <span data-ttu-id="7f8b2-405">De är användbara för att tillhandahålla begränsade behörigheter till ditt lagringskonto till klienter som inte ska ha kontonyckel.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-405">They are useful for providing limited permissions to your storage account to clients that should not have the account key.</span></span> <span data-ttu-id="7f8b2-406">Som standard kan endast ägaren av storage-konto åtkomst till blobbar, tabeller och köer i kontot.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-406">By default, only the owner of the storage account may access blobs, tables, and queues within that account.</span></span> <span data-ttu-id="7f8b2-407">Om tjänsten eller programmet måste göra resurserna tillgängliga för andra klienter utan att dela din åtkomstnyckel, finns det tre alternativ:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-407">If your service or application needs to make these resources available to other clients without sharing your access key, you have three options:</span></span>

* <span data-ttu-id="7f8b2-408">Ange en behållare behörighet för att tillåta anonym läsbehörighet till behållaren och dess blobbar.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-408">Set a container's permissions to permit anonymous read access to the container and its blobs.</span></span> <span data-ttu-id="7f8b2-409">Detta är inte tillåtet för tabeller och köer.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-409">This is not allowed for tables or queues.</span></span>
* <span data-ttu-id="7f8b2-410">Använda en signatur för delad åtkomst som ger begränsad behörighet till behållare, blobbar, köer och tabeller för ett visst tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-410">Use a shared access signature that grants restricted access rights to containers, blobs, queues, and tables for a specific time interval.</span></span>
* <span data-ttu-id="7f8b2-411">Använda en lagrad åtkomstprincip för att hämta ytterligare en säkerhetsnivå för kontroll över signaturer för delad åtkomst för en behållare eller dess blobbar, en kö eller en tabell.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-411">Use a stored access policy to obtain an additional level of control over shared access signatures for a container or its blobs, for a queue, or for a table.</span></span> <span data-ttu-id="7f8b2-412">Den lagrade åtkomstprincipen kan du ändra starttid, förfallotiden eller behörigheter för en signatur, eller om du vill återkalla efter att det har utfärdats.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-412">The stored access policy allows you to change the start time, expiry time, or permissions for a signature, or to revoke it after it has been issued.</span></span>

<span data-ttu-id="7f8b2-413">En signatur för delad åtkomst kan ha ett av två sätt:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-413">A shared access signature can be in one of two forms:</span></span>

* <span data-ttu-id="7f8b2-414">**Ad hoc-SAS**: när du skapar en ad hoc-SAS starttid, förfallotid, och behörigheter för SAS har angetts för SAS-URI.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-414">**Ad hoc SAS**: When you create an ad hoc SAS, the start time, expiry time, and permissions for the SAS are all specified on the SAS URI.</span></span> <span data-ttu-id="7f8b2-415">Den här typen av SAS kan skapas på en behållare, blob, tabell eller kön och det är icke-återkallningsbar.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-415">This type of SAS may be created on a container, blob, table, or queue and it is non-revocable.</span></span>
* <span data-ttu-id="7f8b2-416">**SAS med lagrade åtkomstprincip**: en lagrade åtkomstprincip definieras på resursbehållaren en blob-behållare, tabeller eller kö - och du kan använda för att hantera begränsningar för en eller flera signaturer för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-416">**SAS with stored access policy**: A stored access policy is defined on a resource container a blob container, table, or queue - and you can use it to manage constraints for one or more shared access signatures.</span></span> <span data-ttu-id="7f8b2-417">När du kopplar en SAS med en lagrad till ärver SAS begränsningarna - starttid, förfallotiden och behörigheter - har definierats för den lagrade åtkomstprincipen.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-417">When you associate a SAS with a stored access policy, the SAS inherits the constraints - the start time, expiry time, and permissions - defined for the stored access policy.</span></span> <span data-ttu-id="7f8b2-418">Den här typen av SAS är återkallningsbar.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-418">This type of SAS is revocable.</span></span>

<span data-ttu-id="7f8b2-419">Mer information finns i [med delad åtkomst signaturer (SAS)](storage-dotnet-shared-access-signature-part-1.md) och [Hantera anonym läsbehörighet till behållare och blobbar](storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-419">For more information, see [Using Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md) and [Manage anonymous read access to containers and blobs](storage-manage-access-to-resources.md).</span></span>

<span data-ttu-id="7f8b2-420">I nästa avsnitt får du lära dig hur du skapar en signatur åtkomst-token och lagrade princip för delad åtkomst för Azure-tabeller.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-420">In the next sections, you will learn how to create a shared access signature token and stored access policy for Azure tables.</span></span> <span data-ttu-id="7f8b2-421">Azure PowerShell tillhandahåller liknande cmdlets för behållare, blobbar och köer samt.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-421">Azure PowerShell provides similar cmdlets for containers, blobs, and queues as well.</span></span> <span data-ttu-id="7f8b2-422">Om du vill köra skript i det här avsnittet kan du hämta den [Azure PowerShell version 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) eller senare.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-422">To run the scripts in this section, download the [Azure PowerShell version 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) or later.</span></span>

### <a name="how-to-create-a-policy-based-shared-access-signature-token"></a><span data-ttu-id="7f8b2-423">Så här skapar du en principbaserad signatur för delad åtkomst-token</span><span class="sxs-lookup"><span data-stu-id="7f8b2-423">How to create a policy-based Shared Access Signature token</span></span>
<span data-ttu-id="7f8b2-424">Använd cmdleten New-AzureStorageTableStoredAccessPolicy för att skapa en ny princip för lagrade åtkomst.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-424">Use the New-AzureStorageTableStoredAccessPolicy cmdlet to create a new stored access policy.</span></span> <span data-ttu-id="7f8b2-425">Anropa sedan den [ny AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) för att skapa en ny policy-baserad delad signatur åtkomsttoken för en tabell med Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-425">Then, call the [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) cmdlet to create a new policy-based shared access signature token for an Azure Storage table.</span></span>

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx
```

### <a name="how-to-create-an-ad-hoc-non-revocable-shared-access-signature-token"></a><span data-ttu-id="7f8b2-426">Så här skapar du en ad hoc-(icke-återkallningsbar) signatur för delad åtkomst-token</span><span class="sxs-lookup"><span data-stu-id="7f8b2-426">How to create an ad hoc (non-revocable) Shared Access Signature token</span></span>
<span data-ttu-id="7f8b2-427">Använd den [ny AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) för att skapa en ny ad hoc-(icke-återkallningsbar) signatur för delad åtkomst-token för en tabell med Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-427">Use the [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) cmdlet to create a new ad hoc (non-revocable) Shared Access Signature token for an Azure Storage table:</span></span>

```powershell
New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx
```
    
### <a name="how-to-create-a-stored-access-policy"></a><span data-ttu-id="7f8b2-428">Så här skapar du en princip för lagrade åtkomst</span><span class="sxs-lookup"><span data-stu-id="7f8b2-428">How to create a stored access policy</span></span>
<span data-ttu-id="7f8b2-429">Använd cmdlet New-AzureStorageTableStoredAccessPolicy för att skapa lagrade åtkomstprincip för en tabell med Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-429">Use the New-AzureStorageTableStoredAccessPolicy cmdlet to create a new stored access policy for an Azure Storage table:</span></span>

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
```
    
### <a name="how-to-update-a-stored-access-policy"></a><span data-ttu-id="7f8b2-430">Så här uppdaterar du en princip för lagrade åtkomst</span><span class="sxs-lookup"><span data-stu-id="7f8b2-430">How to update a stored access policy</span></span>
<span data-ttu-id="7f8b2-431">Använd cmdlet Set-AzureStorageTableStoredAccessPolicy för att uppdatera en befintlig lagrade åtkomstprincip för en tabell med Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-431">Use the Set-AzureStorageTableStoredAccessPolicy cmdlet to update an existing stored access policy for an Azure Storage table:</span></span>

```powershell
Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx
```

### <a name="how-to-delete-a-stored-access-policy"></a><span data-ttu-id="7f8b2-432">Hur du tar bort en princip för lagrade åtkomst</span><span class="sxs-lookup"><span data-stu-id="7f8b2-432">How to delete a stored access policy</span></span>
<span data-ttu-id="7f8b2-433">Använd cmdleten Remove-AzureStorageTableStoredAccessPolicy för att ta bort en princip för lagrade åtkomst på en tabell i Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-433">Use the Remove-AzureStorageTableStoredAccessPolicy cmdlet to delete a stored access policy on an Azure Storage table:</span></span>

```powershell
Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx
```

## <a name="how-to-use-azure-storage-for-us-government-and-azure-china"></a><span data-ttu-id="7f8b2-434">Hur du använder Azure Storage för amerikanska myndigheter och Azure Kina</span><span class="sxs-lookup"><span data-stu-id="7f8b2-434">How to use Azure Storage for U.S. government and Azure China</span></span>
<span data-ttu-id="7f8b2-435">En Azure-miljö är en oberoende distribution av Microsoft Azure som [Azure Government för USA: s regering](https://azure.microsoft.com/features/gov/), [AzureCloud för globala Azure](https://portal.azure.com), och [AzureChinaCloud för Azure som drivs av 21Vianet i Kina](http://www.windowsazure.cn/).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-435">An Azure environment is an independent deployment of Microsoft Azure, such as [Azure Government for U.S. government](https://azure.microsoft.com/features/gov/), [AzureCloud for global Azure](https://portal.azure.com), and [AzureChinaCloud for Azure operated by 21Vianet in China](http://www.windowsazure.cn/).</span></span> <span data-ttu-id="7f8b2-436">Du kan distribuera nya miljöer i Azure för amerikanska myndigheter och Azure Kina.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-436">You can deploy new Azure environments for U.S government and Azure China.</span></span>

<span data-ttu-id="7f8b2-437">Om du vill använda Azure Storage med AzureChinaCloud, måste du skapa en kontext för lagring som är associerad med AzureChinaCloud.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-437">To use Azure Storage with AzureChinaCloud, you need to create a storage context that is associated with AzureChinaCloud.</span></span> <span data-ttu-id="7f8b2-438">Följ dessa steg om du vill komma igång:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-438">Follow these steps to get you started:</span></span>

1. <span data-ttu-id="7f8b2-439">Kör den [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) för att visa tillgängliga Azure miljöer:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-439">Run the [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) cmdlet to see the available Azure environments:</span></span>
   
    ```powershell
    Get-AzureEnvironment
    ```

2. <span data-ttu-id="7f8b2-440">Lägg till ett Azure Kina konto i Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-440">Add an Azure China account to Windows PowerShell:</span></span>
   
    ```powershell
    Add-AzureAccount –Environment AzureChinaCloud
    ```

3. <span data-ttu-id="7f8b2-441">Skapa en kontext för lagring för ett AzureChinaCloud konto:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-441">Create a storage context for an AzureChinaCloud account:</span></span>
   
    ```powershell
    $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud
    ```

<span data-ttu-id="7f8b2-442">Använda Azure Storage med [USA Azure Government](https://azure.microsoft.com/features/gov/), bör du definiera en ny miljö och sedan skapa en ny lagring kontext med den här miljön:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-442">To use Azure Storage with [U.S. Azure Government](https://azure.microsoft.com/features/gov/), you should define a new environment and then create a new storage context with this environment:</span></span>

1. <span data-ttu-id="7f8b2-443">Kör den [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) för att visa tillgängliga Azure miljöer:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-443">Run the [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) cmdlet to see the available Azure environments:</span></span>

    ```powershell
    Get-AzureEnvironment
    ```

2. <span data-ttu-id="7f8b2-444">Lägg till ett konto i Azure som tillhör amerikanska myndigheter i Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-444">Add an Azure US Government account to Windows PowerShell:</span></span>
   
    ```powershell
    Add-AzureAccount –Environment AzureUSGovernment
    ```

3. <span data-ttu-id="7f8b2-445">Skapa en kontext för lagring för ett AzureUSGovernment konto:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-445">Create a storage context for an AzureUSGovernment account:</span></span>
   
    ```powershell
    $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureUSGovernment
    ```
     
<span data-ttu-id="7f8b2-446">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="7f8b2-446">For more information, see:</span></span>

* <span data-ttu-id="7f8b2-447">[Utvecklarhandbok för Microsoft Azure Government](../azure-government-developer-guide.md).</span><span class="sxs-lookup"><span data-stu-id="7f8b2-447">[Microsoft Azure Government Developer Guide](../azure-government-developer-guide.md).</span></span>
* [<span data-ttu-id="7f8b2-448">Översikt över skillnaderna när du skapar ett program på Kina Service</span><span class="sxs-lookup"><span data-stu-id="7f8b2-448">Overview of Differences When Creating an Application on China Service</span></span>](https://msdn.microsoft.com/library/azure/dn578439.aspx)

## <a name="next-steps"></a><span data-ttu-id="7f8b2-449">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7f8b2-449">Next Steps</span></span>
<span data-ttu-id="7f8b2-450">I den här guiden har du lärt dig hur du hanterar Azure Storage med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-450">In this guide, you've learned how to manage Azure Storage with Azure PowerShell.</span></span> <span data-ttu-id="7f8b2-451">Här följer några relaterade artiklar och resurser för att lära dig mer om dem.</span><span class="sxs-lookup"><span data-stu-id="7f8b2-451">Here are some related articles and resources for learning more about them.</span></span>

* [<span data-ttu-id="7f8b2-452">Azure Storage-dokumentation</span><span class="sxs-lookup"><span data-stu-id="7f8b2-452">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)
* [<span data-ttu-id="7f8b2-453">PowerShell-Cmdlets för Azure Storage</span><span class="sxs-lookup"><span data-stu-id="7f8b2-453">Azure Storage PowerShell Cmdlets</span></span>](/powershell/module/azurerm.storage/#storage)
* [<span data-ttu-id="7f8b2-454">Windows PowerShell-referens</span><span class="sxs-lookup"><span data-stu-id="7f8b2-454">Windows PowerShell Reference</span></span>](https://msdn.microsoft.com/library/ms714469.aspx)

[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How to manage storage accounts in Azure]: #manageaccount
[How to set a default Azure subscription]: #setdefsub
[How to create a new Azure storage account]: #createaccount
[How to set a default Azure storage account]: #defaultaccount
[How to list all Azure storage accounts in a subscription]: #listaccounts
[How to create an Azure storage context]: #createctx
[How to manage Azure blobs and blob snapshots]: #manageblobs
[How to create a container]: #container
[How to upload a blob into a container]: #uploadblob
[How to download blobs from a container]: #downblob
[How to copy blobs from one storage container to another]: #copyblob
[How to delete a blob]: #deleteblob
[How to manage Azure blob snapshots]: #manageshots
[How to create a blob snapshot]: #createshot
[How to list snapshots of a blob]: #listshot
[How to copy a snapshot of a blob]: #copyshot
[How to manage Azure tables and table entities]: #managetables
[How to create a table]: #createtable
[How to retrieve a table]: #gettable
[How to delete a table]: #remtable
[How to manage table entities]: #mngentity
[How to add table entities]: #addentity
[How to query table entities]: #queryentity
[How to delete table entities]: #deleteentity
[How to manage Azure queues and queue messages]: #managequeues
[How to create a queue]: #createqueue
[How to retrieve a queue]: #getqueue
[How to delete a queue]: #remqueue
[How to manage queue messages]: #mngqueuemsg
[How to insert a message into a queue]: #addqueuemsg
[How to de-queue at the next message]: #dequeuemsg
[How to manage Azure file shares and files]: #files
[How to set and query storage analytics]: #stganalytics
[How to manage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How to use Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next
