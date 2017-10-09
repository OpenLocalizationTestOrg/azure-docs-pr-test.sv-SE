---
title: aaaUsing Azure PowerShell med Azure Storage | Microsoft Docs
description: "Lär dig hur toouse hello Azure PowerShell-cmdlets för Azure Storage toocreate och hantera lagringskonton; Arbeta med blobbar, tabeller, köer och -filer. Konfigurera och efterfråga storage analytics och skapa signaturer för delad åtkomst."
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
ms.openlocfilehash: befe7adda2384f8bcdb8b9f1a063e4eafc158271
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-powershell-with-azure-storage"></a><span data-ttu-id="bd47a-103">Använda Azure PowerShell med Azure Storage</span><span class="sxs-lookup"><span data-stu-id="bd47a-103">Using Azure PowerShell with Azure Storage</span></span>
## <a name="overview"></a><span data-ttu-id="bd47a-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="bd47a-104">Overview</span></span>
<span data-ttu-id="bd47a-105">Azure PowerShell är en modul som tillhandahåller cmdlets toomanage Azure via Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bd47a-105">Azure PowerShell is a module that provides cmdlets toomanage Azure through Windows PowerShell.</span></span> <span data-ttu-id="bd47a-106">Det är ett uppgiftsbaserat kommandoradsgränssnitt och skriptspråk som utformats specifikt för systemadministration.</span><span class="sxs-lookup"><span data-stu-id="bd47a-106">It is a task-based command-line shell and scripting language designed especially for system administration.</span></span> <span data-ttu-id="bd47a-107">Du kan enkelt styra och automatisera hello administration av dina Azure-tjänster och program med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bd47a-107">With PowerShell, you can easily control and automate hello administration of your Azure services and applications.</span></span> <span data-ttu-id="bd47a-108">Du kan till exempel använda samma uppgifter som du kan utföra via hello hello cmdlets tooperform hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="bd47a-108">For example, you can use hello cmdlets tooperform hello same tasks that you can perform through hello [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="bd47a-109">I den här handboken vi ska visa hur toouse hello [Azure Storage-Cmdlets](/powershell/module/azurerm.storage/#storage) tooperform en rad olika uppgifter för utveckling och administration med Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="bd47a-109">In this guide, we'll explore how toouse hello [Azure Storage Cmdlets](/powershell/module/azurerm.storage/#storage) tooperform a variety of development and administration tasks with Azure Storage.</span></span>

<span data-ttu-id="bd47a-110">Den här guiden förutsätter att du har tidigare erfarenhet med [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) och [Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd47a-110">This guide assumes that you have prior experience using [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) and [Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx).</span></span> <span data-ttu-id="bd47a-111">hello guiden ger ett antal skript toodemonstrate hello användning av PowerShell med Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="bd47a-111">hello guide provides a number of scripts toodemonstrate hello usage of PowerShell with Azure Storage.</span></span> <span data-ttu-id="bd47a-112">Du bör uppdatera hello skriptvariabler baserat på din konfiguration innan du kör varje skript.</span><span class="sxs-lookup"><span data-stu-id="bd47a-112">You should update hello script variables based on your configuration before running each script.</span></span>

<span data-ttu-id="bd47a-113">hello första delen i den här guiden ger en snabb överblick över vid Azure Storage och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bd47a-113">hello first section in this guide provides a quick glance at Azure Storage and PowerShell.</span></span> <span data-ttu-id="bd47a-114">Detaljerad information och instruktioner som startar från hello [krav för att använda Azure PowerShell med Azure Storage](#prerequisites-for-using-azure-powershell-with-azure-storage).</span><span class="sxs-lookup"><span data-stu-id="bd47a-114">For detailed information and instructions, start from hello [Prerequisites for using Azure PowerShell with Azure Storage](#prerequisites-for-using-azure-powershell-with-azure-storage).</span></span>

## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a><span data-ttu-id="bd47a-115">Komma igång med Azure Storage och PowerShell 5 minuter</span><span class="sxs-lookup"><span data-stu-id="bd47a-115">Getting started with Azure Storage and PowerShell in 5 minutes</span></span>
<span data-ttu-id="bd47a-116">Det här avsnittet beskrivs hur du tooaccess Azure Storage via PowerShell 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="bd47a-116">This section shows you how tooaccess Azure Storage via PowerShell in 5 minutes.</span></span>

<span data-ttu-id="bd47a-117">**Nya tooAzure:** hämta Microsoft Azure-prenumeration och ett Microsoft-konto som är kopplade till den prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="bd47a-117">**New tooAzure:** Get a Microsoft Azure subscription and a Microsoft account associated with that subscription.</span></span> <span data-ttu-id="bd47a-118">Mer information om köpalternativ för Azure finns [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/), [köpa alternativ](https://azure.microsoft.com/pricing/purchase-options/), och [Medlemserbjudanden](https://azure.microsoft.com/pricing/member-offers/) (för medlemmar i MSDN, Microsoft Partner Network, BizSpark och att andra Microsoft-program).</span><span class="sxs-lookup"><span data-stu-id="bd47a-118">For information on Azure purchase options, see [Free Trial](https://azure.microsoft.com/pricing/free-trial/), [Purchase Options](https://azure.microsoft.com/pricing/purchase-options/), and [Member Offers](https://azure.microsoft.com/pricing/member-offers/) (for members of MSDN, Microsoft Partner Network, and BizSpark, and other Microsoft programs).</span></span>

<span data-ttu-id="bd47a-119">Se [Tilldela administratörsroller i Azure Active Directory (AD Azure)](https://msdn.microsoft.com/library/azure/hh531793.aspx) för mer information om Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="bd47a-119">See [Assigning administrator roles in Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) for more information about Azure subscriptions.</span></span>

<span data-ttu-id="bd47a-120">**När du har skapat ett Microsoft Azure-prenumeration och konto:**</span><span class="sxs-lookup"><span data-stu-id="bd47a-120">**After creating a Microsoft Azure subscription and account:**</span></span>

1. <span data-ttu-id="bd47a-121">Hämta och installera hello senaste [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/latest).</span><span class="sxs-lookup"><span data-stu-id="bd47a-121">Download and install hello latest [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/latest).</span></span>
2. <span data-ttu-id="bd47a-122">Starta Windows PowerShell Integrated Scripting Environment (ISE): Den lokala datorn, gå i toohello **starta** menyn.</span><span class="sxs-lookup"><span data-stu-id="bd47a-122">Start Windows PowerShell Integrated Scripting Environment (ISE): In your local computer, go toohello **Start** menu.</span></span> <span data-ttu-id="bd47a-123">Typen **Administrationsverktyg** och klicka på toorun den.</span><span class="sxs-lookup"><span data-stu-id="bd47a-123">Type **Administrative Tools** and click toorun it.</span></span> <span data-ttu-id="bd47a-124">I hello **Administrationsverktyg** fönstret, högerklicka på **Windows PowerShell ISE**, klickar du på **kör som administratör**.</span><span class="sxs-lookup"><span data-stu-id="bd47a-124">In hello **Administrative Tools** window, right-click **Windows PowerShell ISE**, click **Run as Administrator**.</span></span>
3. <span data-ttu-id="bd47a-125">I **Windows PowerShell ISE**, klickar du på **filen** > **ny** toocreate en ny skriptfil.</span><span class="sxs-lookup"><span data-stu-id="bd47a-125">In **Windows PowerShell ISE**, click **File** > **New** toocreate a new script file.</span></span>
4. <span data-ttu-id="bd47a-126">Nu får du ett enkelt skript som visar grundläggande PowerShell-kommandon tooaccess Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="bd47a-126">Now, we'll give you a simple script that shows basic PowerShell commands tooaccess Azure Storage.</span></span> <span data-ttu-id="bd47a-127">hello skript ber först ditt Azure-konto autentiseringsuppgifter tooadd din Azure-konto toohello lokala PowerShell-miljö.</span><span class="sxs-lookup"><span data-stu-id="bd47a-127">hello script will first ask your Azure account credentials tooadd your Azure account toohello local PowerShell environment.</span></span> <span data-ttu-id="bd47a-128">Hello-skriptet kommer sedan ange hello standardvärdet Azure-prenumeration och skapa ett nytt lagringskonto i Azure.</span><span class="sxs-lookup"><span data-stu-id="bd47a-128">Then, hello script will set hello default Azure subscription and create a new storage account in Azure.</span></span> <span data-ttu-id="bd47a-129">Hello-skriptet kommer sedan skapa en ny behållare i den här nya storage-konto och överför en befintlig avbildning fil (blob) toothat behållare.</span><span class="sxs-lookup"><span data-stu-id="bd47a-129">Next, hello script will create a new container in this new storage account and upload an existing image file (blob) toothat container.</span></span> <span data-ttu-id="bd47a-130">När hello-skriptet visar alla blobbar i behållaren, kommer den skapa en ny målkatalog i den lokala datorn och hämta hello image-filen.</span><span class="sxs-lookup"><span data-stu-id="bd47a-130">After hello script lists all blobs in that container, it will create a new destination directory in your local computer and download hello image file.</span></span>
5. <span data-ttu-id="bd47a-131">I följande avsnitt med exempelkod hello, väljer du hello skript mellan hello anmärkningar **#begin** och **#end**.</span><span class="sxs-lookup"><span data-stu-id="bd47a-131">In hello following code section, select hello script between hello remarks **#begin** and **#end**.</span></span> <span data-ttu-id="bd47a-132">Tryck på CTRL + C toocopy den toohello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="bd47a-132">Press CTRL+C toocopy it toohello clipboard.</span></span>

    ```powershell
    # begin
    # Update with hello name of your subscription.
    $SubscriptionName = "YourSubscriptionName"
       
    # Give a name tooyour new storage account. It must be lowercase!
    $StorageAccountName = "yourstorageaccountname"
       
    # Choose "West US" as an example.
    $Location = "West US"
       
    # Give a name tooyour new container.
    $ContainerName = "imagecontainer"
       
    # Have an image file and a source directory in your local computer.
    $ImageToUpload = "C:\Images\HelloWorld.png"
       
    # A destination directory in your local computer.
    $DestinationFolder = "C:\DownloadImages"
       
    # Add your Azure account toohello local PowerShell environment.
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
       
    # Download blobs from hello container:
    # Get a reference tooa list of all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName
       
    # Create hello destination directory.
    New-Item -Path $DestinationFolder -ItemType Directory -Force  
       
    # Download blobs into hello local destination directory.
    $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
       
    # end
    ```

6. <span data-ttu-id="bd47a-133">I **Windows PowerShell ISE**, tryck på CTRL + V toocopy hello skript.</span><span class="sxs-lookup"><span data-stu-id="bd47a-133">In **Windows PowerShell ISE**, press CTRL+V toocopy hello script.</span></span> <span data-ttu-id="bd47a-134">Klicka på **filen** > **spara**.</span><span class="sxs-lookup"><span data-stu-id="bd47a-134">Click **File** > **Save**.</span></span> <span data-ttu-id="bd47a-135">I hello **Spara som** dialogruta, hello-typnamn för hello skriptfil, till exempel ”mystoragescript”.</span><span class="sxs-lookup"><span data-stu-id="bd47a-135">In hello **Save As** dialog window, type hello name of hello script file, such as "mystoragescript."</span></span> <span data-ttu-id="bd47a-136">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="bd47a-136">Click **Save**.</span></span>
7. <span data-ttu-id="bd47a-137">Du måste nu tooupdate hello skriptvariabler baserat på inställningarna.</span><span class="sxs-lookup"><span data-stu-id="bd47a-137">Now, you need tooupdate hello script variables based on your configuration settings.</span></span> <span data-ttu-id="bd47a-138">Du måste uppdatera hello **$SubscriptionName** variabeln med din egen prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bd47a-138">You must update hello **$SubscriptionName** variable with your own subscription.</span></span> <span data-ttu-id="bd47a-139">Du kan behålla hello andra variabler som anges i hello skript eller uppdatera dem som du vill.</span><span class="sxs-lookup"><span data-stu-id="bd47a-139">You can keep hello other variables as specified in hello script or update them as you wish.</span></span>
   
   * <span data-ttu-id="bd47a-140">**$SubscriptionName:** måste du uppdatera den här variabeln med prenumerationsnamn på din.</span><span class="sxs-lookup"><span data-stu-id="bd47a-140">**$SubscriptionName:** You must update this variable with your own subscription name.</span></span> <span data-ttu-id="bd47a-141">Gör något av följande tre sätt toolocate hello namnet på din prenumeration hello:</span><span class="sxs-lookup"><span data-stu-id="bd47a-141">Follow one of hello following three ways toolocate hello name of your subscription:</span></span>
     
    <span data-ttu-id="bd47a-142">a.</span><span class="sxs-lookup"><span data-stu-id="bd47a-142">a.</span></span> <span data-ttu-id="bd47a-143">I **Windows PowerShell ISE**, klickar du på **filen** > **ny** toocreate en ny skriptfil.</span><span class="sxs-lookup"><span data-stu-id="bd47a-143">In **Windows PowerShell ISE**, click **File** > **New** toocreate a new script file.</span></span> <span data-ttu-id="bd47a-144">Kopiera hello följande skript toohello nya skriptfilen och på **felsöka** > **kör**.</span><span class="sxs-lookup"><span data-stu-id="bd47a-144">Copy hello following script toohello new script file and click **Debug** > **Run**.</span></span> <span data-ttu-id="bd47a-145">hello följande skript kommer först be din Azure-konto autentiseringsuppgifter tooadd din Azure-konto toohello lokala PowerShell-miljö och sedan visa alla hello-prenumerationer som är anslutna toohello lokala PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="bd47a-145">hello following script will first ask your Azure account credentials tooadd your Azure account toohello local PowerShell environment and then show all hello subscriptions that are connected toohello local PowerShell session.</span></span> <span data-ttu-id="bd47a-146">Anteckna hello namn på hello prenumeration som du vill toouse när följa de här självstudierna:</span><span class="sxs-lookup"><span data-stu-id="bd47a-146">Take a note of hello name of hello subscription that you want toouse while following this tutorial:</span></span>
     
    ```powershell
    Add-AzureAccount 
      Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName
    ```

    <span data-ttu-id="bd47a-147">b.</span><span class="sxs-lookup"><span data-stu-id="bd47a-147">b.</span></span> <span data-ttu-id="bd47a-148">toolocate och kopiera prenumerationen namn i hello [Azure-portalen](https://portal.azure.com), i hello hubbmenyn på hello vänster, klicka på **prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="bd47a-148">toolocate and copy your subscription name in hello [Azure portal](https://portal.azure.com), in hello Hub menu on hello left, click **Subscriptions**.</span></span> <span data-ttu-id="bd47a-149">Kopiera hello namn av prenumeration som du vill toouse när du kör hello skript i den här guiden.</span><span class="sxs-lookup"><span data-stu-id="bd47a-149">Copy hello name of subscription that you want toouse while running hello scripts in this guide.</span></span>
     
     ![Azure Portal](./media/storage-powershell-guide-full/Subscription_Previewportal.png)

    <span data-ttu-id="bd47a-151">c.</span><span class="sxs-lookup"><span data-stu-id="bd47a-151">c.</span></span> <span data-ttu-id="bd47a-152">toolocate och kopiera prenumerationen namn i hello [klassiska Azure-portalen](https://manage.windowsazure.com/), bläddra nedåt och klicka på **inställningar** på vänster sida av hello portal hello.</span><span class="sxs-lookup"><span data-stu-id="bd47a-152">toolocate and copy your subscription name in hello [Azure Classic Portal](https://manage.windowsazure.com/), scroll down and click **Settings** on hello left side of hello portal.</span></span> <span data-ttu-id="bd47a-153">Klicka på **prenumerationer** toosee en lista över dina prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="bd47a-153">Click **Subscriptions** toosee a list of your subscriptions.</span></span> <span data-ttu-id="bd47a-154">Kopiera hello namn av prenumeration som du vill toouse när du kör hello-skript som anges i den här guiden.</span><span class="sxs-lookup"><span data-stu-id="bd47a-154">Copy hello name of subscription that you want toouse while running hello scripts given in this guide.</span></span>
     
     ![Klassisk Azure-portal](./media/storage-powershell-guide-full/Subscription_currentportal.png)

   * <span data-ttu-id="bd47a-156">**$StorageAccountName:** använda hello får namn i skriptet hello eller ange ett nytt namn för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="bd47a-156">**$StorageAccountName:** Use hello given name in hello script or enter a new name for your storage account.</span></span> <span data-ttu-id="bd47a-157">**Viktigt:** hello namnet på hello storage-konto måste vara unikt i Azure.</span><span class="sxs-lookup"><span data-stu-id="bd47a-157">**Important:** hello name of hello storage account must be unique in Azure.</span></span> <span data-ttu-id="bd47a-158">Det måste vara gemena för!</span><span class="sxs-lookup"><span data-stu-id="bd47a-158">It must be lowercase, too!</span></span>
   * <span data-ttu-id="bd47a-159">**$Location:** använda hello anges ”USA, västra” i hello skript eller välj andra Azure platser, till exempel östra USA, Norra Europa och så vidare.</span><span class="sxs-lookup"><span data-stu-id="bd47a-159">**$Location:** Use hello given "West US" in hello script or choose other Azure locations, such as East US, North Europe, and so on.</span></span>
   * <span data-ttu-id="bd47a-160">**$ContainerName:** använda hello får namn i skriptet hello eller ange ett nytt namn för din behållare.</span><span class="sxs-lookup"><span data-stu-id="bd47a-160">**$ContainerName:** Use hello given name in hello script or enter a new name for your container.</span></span>
   * <span data-ttu-id="bd47a-161">**$ImageToUpload:** ange en sökväg tooa bild på den lokala datorn, till exempel: ”C:\Images\HelloWorld.png”.</span><span class="sxs-lookup"><span data-stu-id="bd47a-161">**$ImageToUpload:** Enter a path tooa picture on your local computer, such as: "C:\Images\HelloWorld.png".</span></span>
   * <span data-ttu-id="bd47a-162">**$DestinationFolder:** ange sökvägen tooa lokal katalog toostore filer som hämtas från Azure Storage: ”C:\DownloadImages”.</span><span class="sxs-lookup"><span data-stu-id="bd47a-162">**$DestinationFolder:** Enter a path tooa local directory toostore files downloaded from Azure Storage, such as: "C:\DownloadImages".</span></span>
8. <span data-ttu-id="bd47a-163">När du har uppdaterat hello skriptvariabler i hello ”mystoragescript.ps1”-fil klickar du på **filen** > **spara**.</span><span class="sxs-lookup"><span data-stu-id="bd47a-163">After updating hello script variables in hello "mystoragescript.ps1" file, click **File** > **Save**.</span></span> <span data-ttu-id="bd47a-164">Klicka på **felsöka** > **kör** eller tryck på **F5** toorun hello skript.</span><span class="sxs-lookup"><span data-stu-id="bd47a-164">Then, click **Debug** > **Run** or press **F5** toorun hello script.</span></span>  

<span data-ttu-id="bd47a-165">När hello skriptet körs, bör du ha en lokal målmapp som innehåller hello hämtade image-filen.</span><span class="sxs-lookup"><span data-stu-id="bd47a-165">After hello script runs, you should have a local destination folder that includes hello downloaded image file.</span></span> <span data-ttu-id="bd47a-166">hello följande skärmbild visar ett exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="bd47a-166">hello following screenshot shows an example output:</span></span>

![Ladda ned Blobbar](./media/storage-powershell-guide-full/Blobdownload.png)

> [!NOTE]
> <span data-ttu-id="bd47a-168">hello ”avsnittet komma igång med Azure Storage och PowerShell 5 minuter” tillhandahålls en snabb introduktion på hur toouse Azure PowerShell med Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="bd47a-168">hello "Getting started with Azure Storage and PowerShell in 5 minutes" section provided a quick introduction on how toouse Azure PowerShell with Azure Storage.</span></span> <span data-ttu-id="bd47a-169">För detaljerad information och instruktioner, rekommenderar vi att du tooread hello följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bd47a-169">For detailed information and instructions, we encourage you tooread hello following sections.</span></span>
> 

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a><span data-ttu-id="bd47a-170">Krav för att använda Azure PowerShell med Azure Storage</span><span class="sxs-lookup"><span data-stu-id="bd47a-170">Prerequisites for using Azure PowerShell with Azure Storage</span></span>
<span data-ttu-id="bd47a-171">Du behöver ett Azure-prenumeration och konto toorun hello PowerShell-cmdlets i den här guiden enligt beskrivningen ovan.</span><span class="sxs-lookup"><span data-stu-id="bd47a-171">You need an Azure subscription and account toorun hello PowerShell cmdlets given in this guide, as described above.</span></span>

<span data-ttu-id="bd47a-172">Azure PowerShell är en modul som tillhandahåller cmdlets toomanage Azure via Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bd47a-172">Azure PowerShell is a module that provides cmdlets toomanage Azure through Windows PowerShell.</span></span> <span data-ttu-id="bd47a-173">Information om att installera och konfigurera Azure PowerShell finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bd47a-173">For information on installing and setting up Azure PowerShell, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="bd47a-174">Vi rekommenderar att du hämtar och installerar eller uppgraderar toohello senaste Azure PowerShell-modulen innan du använder den här guiden.</span><span class="sxs-lookup"><span data-stu-id="bd47a-174">We recommend that you download and install or upgrade toohello latest Azure PowerShell module before using this guide.</span></span>

<span data-ttu-id="bd47a-175">Du kan köra hello-cmdlets i Windows PowerShell-konsol med hello standard eller hello Windows PowerShell Integrated Scripting Environment (ISE).</span><span class="sxs-lookup"><span data-stu-id="bd47a-175">You can run hello cmdlets in hello standard Windows PowerShell console or hello Windows PowerShell Integrated Scripting Environment (ISE).</span></span> <span data-ttu-id="bd47a-176">Till exempel tooopen **Windows PowerShell ISE**, gå toohello Start-menyn, Skriv Administrationsverktyg och på toorun den.</span><span class="sxs-lookup"><span data-stu-id="bd47a-176">For example, tooopen **Windows PowerShell ISE**, go toohello Start menu, type Administrative Tools and click toorun it.</span></span> <span data-ttu-id="bd47a-177">Högerklicka på Windows PowerShell ISE i hello Administrationsverktyg fönster, klicka på Kör som administratör.</span><span class="sxs-lookup"><span data-stu-id="bd47a-177">In hello Administrative Tools window, right-click Windows PowerShell ISE, click Run as Administrator.</span></span>

## <a name="how-toomanage-storage-accounts-in-azure"></a><span data-ttu-id="bd47a-178">Hur toomanage lagringskonton i Azure</span><span class="sxs-lookup"><span data-stu-id="bd47a-178">How toomanage storage accounts in Azure</span></span>

<span data-ttu-id="bd47a-179">Låt oss ta en titt på Hantera storage-konton i Azure med PowerShell</span><span class="sxs-lookup"><span data-stu-id="bd47a-179">Let's take a look at managing storage accounts in Azure with PowerShell</span></span>

### <a name="how-tooset-a-default-azure-subscription"></a><span data-ttu-id="bd47a-180">Hur tooset standard Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="bd47a-180">How tooset a default Azure subscription</span></span>
<span data-ttu-id="bd47a-181">toomanage Azure Storage med hjälp av Azure PowerShell, behöver du tooauthenticate din klientmiljö med Azure via Azure Active Directory-autentisering eller certifikatbaserad autentisering.</span><span class="sxs-lookup"><span data-stu-id="bd47a-181">toomanage Azure Storage using Azure PowerShell, you need tooauthenticate your client environment with Azure via Azure Active Directory authentication or certificate-based authentication.</span></span> <span data-ttu-id="bd47a-182">Detaljerad information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) kursen.</span><span class="sxs-lookup"><span data-stu-id="bd47a-182">For detailed information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) tutorial.</span></span> <span data-ttu-id="bd47a-183">Den här guiden använder hello Azure Active Directory-autentisering.</span><span class="sxs-lookup"><span data-stu-id="bd47a-183">This guide uses hello Azure Active Directory authentication.</span></span>

1. <span data-ttu-id="bd47a-184">I Windows PowerShell ISE skriver du följande kommando tooadd hello din Azure-konto toohello lokala PowerShell-miljö:</span><span class="sxs-lookup"><span data-stu-id="bd47a-184">In Windows PowerShell ISE, type hello following command tooadd your Azure account toohello local PowerShell environment:</span></span>

    ```powershell
    Add-AzureAccount
    ```

2. <span data-ttu-id="bd47a-185">I fönstret för hello ”logga in tooMicrosoft Azure”, typen hello e-postadress och lösenord som är associerat med ditt konto.</span><span class="sxs-lookup"><span data-stu-id="bd47a-185">In hello "Sign in tooMicrosoft Azure" window, type hello email address and password associated with your account.</span></span> <span data-ttu-id="bd47a-186">Azure autentiserar sparar hello autentiseringsuppgifter och sedan stänger hello-fönstret.</span><span class="sxs-lookup"><span data-stu-id="bd47a-186">Azure authenticates and saves hello credential information, and then closes hello window.</span></span>

3. <span data-ttu-id="bd47a-187">Kör hello efter kommandot tooview hello Azure-konton i din lokala PowerShell-miljö och kontrollera att ditt konto finns:</span><span class="sxs-lookup"><span data-stu-id="bd47a-187">Next, run hello following command tooview hello Azure accounts in your local PowerShell environment, and verify that your account is listed:</span></span>
   
    ```powershell
    Get-AzureAccount
    ```
4. <span data-ttu-id="bd47a-188">Sedan kör följande cmdlet tooview hello alla hello-prenumerationer som är anslutna toohello lokala PowerShell-session och kontrollerar du att din prenumeration visas:</span><span class="sxs-lookup"><span data-stu-id="bd47a-188">Then, run hello following cmdlet tooview all hello subscriptions that are connected toohello local PowerShell session, and verify that your subscription is listed:</span></span>

    ```powershell
    Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`
    ```
5. <span data-ttu-id="bd47a-189">tooset standard Azure-prenumeration, kör hello väljer AzureSubscription cmdlet:</span><span class="sxs-lookup"><span data-stu-id="bd47a-189">tooset a default Azure subscription, run hello Select-AzureSubscription cmdlet:</span></span>

    ```powershell
    $SubscriptionName = 'Your subscription Name'
    Select-AzureSubscription -SubscriptionName $SubscriptionName –Default
    ```

6. <span data-ttu-id="bd47a-190">Kontrollera hello namn på hello standard prenumeration genom att köra hello Get-AzureSubscription cmdlet:</span><span class="sxs-lookup"><span data-stu-id="bd47a-190">Verify hello name of hello default subscription by running hello Get-AzureSubscription cmdlet:</span></span>

    ```powershell
    Get-AzureSubscription -Default
    ```

7. <span data-ttu-id="bd47a-191">toosee alla hello tillgängliga PowerShell-cmdlets för Azure Storage, kör:</span><span class="sxs-lookup"><span data-stu-id="bd47a-191">toosee all hello available PowerShell cmdlets for Azure Storage, run:</span></span>
    
    ```powershell
    Get-Command -Module Azure -Noun *Storage*`
    ```

### <a name="how-toocreate-a-new-azure-storage-account"></a><span data-ttu-id="bd47a-192">Hur toocreate ett nytt Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="bd47a-192">How toocreate a new Azure storage account</span></span>
<span data-ttu-id="bd47a-193">toouse Azure storage, behöver ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="bd47a-193">toouse Azure storage, you will need a storage account.</span></span> <span data-ttu-id="bd47a-194">Du kan skapa ett nytt Azure storage-konto när du har konfigurerat din dator tooconnect tooyour prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bd47a-194">You can create a new Azure storage account after you have configured your computer tooconnect tooyour subscription.</span></span>

1. <span data-ttu-id="bd47a-195">Kör hello Get-AzureLocation cmdlet toofind alla hello tillgängliga datacenter platser:</span><span class="sxs-lookup"><span data-stu-id="bd47a-195">Run hello Get-AzureLocation cmdlet toofind all hello available datacenter locations:</span></span>

    ```powershell
    Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes
    ```

2. <span data-ttu-id="bd47a-196">Kör hello ny AzureStorageAccount cmdlet toocreate ett nytt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="bd47a-196">Next, run hello New-AzureStorageAccount cmdlet toocreate a new storage account.</span></span> <span data-ttu-id="bd47a-197">hello följande exempel skapas ett nytt lagringskonto i hello ”USA, västra” datacenter.</span><span class="sxs-lookup"><span data-stu-id="bd47a-197">hello following example creates a new storage account in hello "West US" datacenter.</span></span>
   
    ```powershell
    $location = "West US"
    $StorageAccountName = "yourstorageaccount"
    New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location
    ```

> [!IMPORTANT]
> <span data-ttu-id="bd47a-198">hello namnet på ditt lagringskonto måste vara unikt i Azure och måste vara gemener.</span><span class="sxs-lookup"><span data-stu-id="bd47a-198">hello name of your storage account must be unique within Azure and must be lowercase.</span></span> <span data-ttu-id="bd47a-199">Namnkonventioner och begränsningar finns i [om Azure Lagringskonton](storage-create-storage-account.md) och [namnge och referera till behållare, Blobbar och Metadata](http://msdn.microsoft.com/library/azure/dd135715.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd47a-199">For naming conventions and restrictions, see [About Azure Storage Accounts](storage-create-storage-account.md) and [Naming and Referencing Containers, Blobs, and Metadata](http://msdn.microsoft.com/library/azure/dd135715.aspx).</span></span>
> 
> 

### <a name="how-tooset-a-default-azure-storage-account"></a><span data-ttu-id="bd47a-200">Hur tooset ett standardkonto för Azure storage</span><span class="sxs-lookup"><span data-stu-id="bd47a-200">How tooset a default Azure storage account</span></span>
<span data-ttu-id="bd47a-201">Du kan ha flera lagringskonton i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bd47a-201">You can have multiple storage accounts in your subscription.</span></span> <span data-ttu-id="bd47a-202">Du kan välja en av dem och ange den som hello standardkontot för lagring för alla hello lagring kommandon i hello samma PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="bd47a-202">You can choose one of them and set it as hello default storage account for all hello storage commands in hello same PowerShell session.</span></span> <span data-ttu-id="bd47a-203">Detta gör toorun Azure PowerShell-kommandon hello lagring utan att ange hello lagring kontexten explicit.</span><span class="sxs-lookup"><span data-stu-id="bd47a-203">This enables you toorun hello Azure PowerShell storage commands without specifying hello storage context explicitly.</span></span>

1. <span data-ttu-id="bd47a-204">Du kan köra hello-cmdlet Set-AzureSubscription tooset en standardkontot för lagring för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bd47a-204">tooset a default storage account for your subscription, you can run hello Set-AzureSubscription cmdlet.</span></span>

    ```powershell
    $SubscriptionName = "Your subscription name"
    $StorageAccountName = "yourstorageaccount"  
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName
    ```

2. <span data-ttu-id="bd47a-205">Kör hello Get-AzureSubscription cmdlet tooverify som hello storage-konto är kopplad till ditt standardkonto för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="bd47a-205">Next, run hello Get-AzureSubscription cmdlet tooverify that hello storage account is associated with your default subscription account.</span></span> <span data-ttu-id="bd47a-206">Det här kommandot returnerar hello Prenumerationsegenskaper på hello aktuell prenumeration, inklusive dess aktuella storage-konto.</span><span class="sxs-lookup"><span data-stu-id="bd47a-206">This command returns hello subscription properties on hello current subscription including its current storage account.</span></span>

    ```powershell
    Get-AzureSubscription –Current
    ```

### <a name="how-toolist-all-azure-storage-accounts-in-a-subscription"></a><span data-ttu-id="bd47a-207">Hur toolist alla Azure storage-konton i en prenumeration</span><span class="sxs-lookup"><span data-stu-id="bd47a-207">How toolist all Azure storage accounts in a subscription</span></span>
<span data-ttu-id="bd47a-208">Varje Azure-prenumeration kan ha upp too100 storage-konton.</span><span class="sxs-lookup"><span data-stu-id="bd47a-208">Each Azure subscription can have up too100 storage accounts.</span></span> <span data-ttu-id="bd47a-209">Hello allra senaste informationen om du begränsar finns [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="bd47a-209">For hello most up-to-date information on limits, see [Azure Subscription and Service Limits, Quotas, and Constraints](../azure-subscription-service-limits.md).</span></span>

<span data-ttu-id="bd47a-210">Kör följande cmdlet toofind ut hello namn och status för hello storage-konton i hello nuvarande prenumeration hello:</span><span class="sxs-lookup"><span data-stu-id="bd47a-210">Run hello following cmdlet toofind out hello name and status of hello storage accounts in hello current subscription:</span></span>

```powershell
Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus
```

### <a name="how-toocreate-an-azure-storage-context"></a><span data-ttu-id="bd47a-211">Hur toocreate en Azure storage-kontext</span><span class="sxs-lookup"><span data-stu-id="bd47a-211">How toocreate an Azure storage context</span></span>
<span data-ttu-id="bd47a-212">Azure storage-kontexten är ett objekt i PowerShell tooencapsulate hello lagring autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="bd47a-212">Azure storage context is an object in PowerShell tooencapsulate hello storage credentials.</span></span> <span data-ttu-id="bd47a-213">Med en kontext som lagring när du kör alla efterföljande cmdletar kan du tooauthenticate din begäran utan att ange hello storage-konto och dess åtkomstnyckel explicit.</span><span class="sxs-lookup"><span data-stu-id="bd47a-213">Using a storage context while running any subsequent cmdlet enables you tooauthenticate your request without specifying hello storage account and its access key explicitly.</span></span> <span data-ttu-id="bd47a-214">Du kan skapa en kontext för lagring på många sätt, till exempel med hjälp av lagringskontots namn och åtkomstnyckel, signaturtoken för delad åtkomst (SAS), anslutningssträngen, eller anonym.</span><span class="sxs-lookup"><span data-stu-id="bd47a-214">You can create a storage context in many ways, such as using storage account name and access key, shared access signature (SAS) token, connection string, or anonymous.</span></span> <span data-ttu-id="bd47a-215">Mer information finns i [ny AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext).</span><span class="sxs-lookup"><span data-stu-id="bd47a-215">For more information, see [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext).</span></span>  

<span data-ttu-id="bd47a-216">Använd någon av följande tre sätt toocreate hello en kontext för lagring:</span><span class="sxs-lookup"><span data-stu-id="bd47a-216">Use one of hello following three ways toocreate a storage context:</span></span>

* <span data-ttu-id="bd47a-217">Kör hello [Get-AzureStorageKey](/powershell/module/azure.storage/get-azurestoragekey) cmdlet toofind ut hello primära lagringsåtkomstnyckel för Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="bd47a-217">Run hello [Get-AzureStorageKey](/powershell/module/azure.storage/get-azurestoragekey) cmdlet toofind out hello primary storage access key for your Azure storage account.</span></span> <span data-ttu-id="bd47a-218">Därefter anropar hello [ny AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext) cmdlet toocreate en kontext för lagring:</span><span class="sxs-lookup"><span data-stu-id="bd47a-218">Next, call hello [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext) cmdlet toocreate a storage context:</span></span>

    ```powershell
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    ```

* <span data-ttu-id="bd47a-219">Generera en token för delad åtkomst signaturen för ett Azure storage-behållare och använda den toocreate en kontext för lagring:</span><span class="sxs-lookup"><span data-stu-id="bd47a-219">Generate a shared access signature token for an Azure storage container and use it toocreate a storage context:</span></span>

    ```powershell
    $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken
    ```

    <span data-ttu-id="bd47a-220">Mer information finns i [ny AzureStorageContainerSASToken](/powershell/module/azure.storage/new-azurestoragecontainersastoken) och [med delad åtkomst signaturer (SAS)](storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="bd47a-220">For more information, see [New-AzureStorageContainerSASToken](/powershell/module/azure.storage/new-azurestoragecontainersastoken) and [Using Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md).</span></span>

* <span data-ttu-id="bd47a-221">I vissa fall kan kanske du vill toospecify hello slutpunkter när du skapar en ny lagring-kontext.</span><span class="sxs-lookup"><span data-stu-id="bd47a-221">In some cases, you may want toospecify hello service endpoints when you create a new storage context.</span></span> <span data-ttu-id="bd47a-222">Detta kan vara nödvändigt när du har registrerat ett anpassat domännamn för ditt lagringskonto med hello Blob-tjänsten eller om du vill toouse en signatur för delad åtkomst för att komma åt lagringsresurser.</span><span class="sxs-lookup"><span data-stu-id="bd47a-222">This might be necessary when you have registered a custom domain name for your storage account with hello Blob service or you want toouse a shared access signature for accessing storage resources.</span></span> <span data-ttu-id="bd47a-223">Ange hello slutpunkter i en anslutningssträng och använda den toocreate en ny lagring kontext enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="bd47a-223">Set hello service endpoints in a connection string and use it toocreate a new storage context as shown below:</span></span>

    ```powershell
    $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
    $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString
    ```

<span data-ttu-id="bd47a-224">Mer information om hur tooconfigure en lagringsanslutningssträng finns [konfigurerar anslutningssträngar](storage-configure-connection-string.md).</span><span class="sxs-lookup"><span data-stu-id="bd47a-224">For more information on how tooconfigure a storage connection string, see [Configuring Connection Strings](storage-configure-connection-string.md).</span></span>

<span data-ttu-id="bd47a-225">Nu när du har skapat din dator och lärt dig hur toomanage prenumerationer och storage-konton med hjälp av Azure PowerShell gå toohello nästa avsnitt toolearn hur toomanage Azure BLOB-objekt och blob ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="bd47a-225">Now that you have set up your computer and learned how toomanage subscriptions and storage accounts using Azure PowerShell, go toohello next section toolearn how toomanage Azure blobs and blob snapshots.</span></span>

### <a name="how-tooretrieve-and-regenerate-azure-storage-keys"></a><span data-ttu-id="bd47a-226">Hur tooretrieve och generera nycklar för Azure storage</span><span class="sxs-lookup"><span data-stu-id="bd47a-226">How tooretrieve and regenerate Azure storage keys</span></span>
<span data-ttu-id="bd47a-227">Ett Azure Storage-konto innehåller två nycklar.</span><span class="sxs-lookup"><span data-stu-id="bd47a-227">An Azure Storage account comes with two account keys.</span></span> <span data-ttu-id="bd47a-228">Du kan använda följande cmdlet tooretrieve hello dina nycklar.</span><span class="sxs-lookup"><span data-stu-id="bd47a-228">You can use hello following cmdlet tooretrieve your keys.</span></span>

```powershell
Get-AzureStorageKey -StorageAccountName "yourstorageaccount"
```

<span data-ttu-id="bd47a-229">Använd hello följande cmdlet tooretrieve en viss nyckel.</span><span class="sxs-lookup"><span data-stu-id="bd47a-229">Use hello following cmdlet tooretrieve a specific key.</span></span> <span data-ttu-id="bd47a-230">Giltiga värden är primär och sekundär.</span><span class="sxs-lookup"><span data-stu-id="bd47a-230">Valid values are Primary and Secondary.</span></span>  

```powershell
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary
    
(Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary
```

<span data-ttu-id="bd47a-231">Om du vill att tooregenerate dina nycklar använder hello följande cmdlet.</span><span class="sxs-lookup"><span data-stu-id="bd47a-231">If you would like tooregenerate your keys, use hello following cmdlet.</span></span> <span data-ttu-id="bd47a-232">Giltiga värden för - KeyType är ”primär” och ”sekundär”</span><span class="sxs-lookup"><span data-stu-id="bd47a-232">Valid values for -KeyType are "Primary" and "Secondary"</span></span>

```powershell
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Primary"
    
New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType "Secondary"
```

## <a name="how-toomanage-azure-blobs"></a><span data-ttu-id="bd47a-233">Hur toomanage Azure BLOB-objekt</span><span class="sxs-lookup"><span data-stu-id="bd47a-233">How toomanage Azure blobs</span></span>
<span data-ttu-id="bd47a-234">Azure Blob storage är en tjänst för att lagra stora mängder Ostrukturerade data, till exempel text eller binära data som kan nås från var som helst i hello world via HTTP eller HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bd47a-234">Azure Blob storage is a service for storing large amounts of unstructured data, such as text or binary data, that can be accessed from anywhere in hello world via HTTP or HTTPS.</span></span> <span data-ttu-id="bd47a-235">Det här avsnittet förutsätter att du redan är bekant med principerna för hello Azure Blob Storage-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bd47a-235">This section assumes that you are already familiar with hello Azure Blob Storage Service concepts.</span></span> <span data-ttu-id="bd47a-236">Detaljerad information finns i [komma igång med Blob storage med hjälp av .NET](storage-dotnet-how-to-use-blobs.md) och [Blob-Tjänstkoncept](http://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd47a-236">For detailed information, see [Get started with Blob storage using .NET](storage-dotnet-how-to-use-blobs.md) and [Blob Service Concepts](http://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>

### <a name="how-toocreate-a-container"></a><span data-ttu-id="bd47a-237">Hur toocreate en behållare</span><span class="sxs-lookup"><span data-stu-id="bd47a-237">How toocreate a container</span></span>
<span data-ttu-id="bd47a-238">Varje blobb i Azure-lagring måste vara i en behållare.</span><span class="sxs-lookup"><span data-stu-id="bd47a-238">Every blob in Azure storage must be in a container.</span></span> <span data-ttu-id="bd47a-239">Du kan skapa en privat behållare med hello ny AzureStorageContainer cmdlet:</span><span class="sxs-lookup"><span data-stu-id="bd47a-239">You can create a private container using hello New-AzureStorageContainer cmdlet:</span></span>

```powershell
$StorageContainerName = "yourcontainername"
New-AzureStorageContainer -Name $StorageContainerName -Permission Off
```

> [!NOTE]
> <span data-ttu-id="bd47a-240">Det finns tre nivåer av anonym läsbehörighet: **av**, **Blob**, och **behållare**.</span><span class="sxs-lookup"><span data-stu-id="bd47a-240">There are three levels of anonymous read access: **Off**, **Blob**, and **Container**.</span></span> <span data-ttu-id="bd47a-241">tooprevent anonym åtkomst till tooblobs parameter för hello behörighet för**av**.</span><span class="sxs-lookup"><span data-stu-id="bd47a-241">tooprevent anonymous access tooblobs, set hello Permission parameter too**Off**.</span></span> <span data-ttu-id="bd47a-242">Som standard hello ny behållare är privat och kan endast användas av hello Kontoägare.</span><span class="sxs-lookup"><span data-stu-id="bd47a-242">By default, hello new container is private and can be accessed only by hello account owner.</span></span> <span data-ttu-id="bd47a-243">tooallow anonym offentliga läsa åtkomst tooblob resurser, men inte toocontainer metadata eller toohello lista över blobbar i hello behållare genom att ange hello behörighet parametern för**Blob**.</span><span class="sxs-lookup"><span data-stu-id="bd47a-243">tooallow anonymous public read access tooblob resources, but not toocontainer metadata or toohello list of blobs in hello container, set hello Permission parameter too**Blob**.</span></span> <span data-ttu-id="bd47a-244">tooallow fullständig offentliga läsa komma åt resurser i tooblob, metadata för behållaren och hello lista över blobbar i hello behållare genom att ange hello behörighet parametern för**behållare**.</span><span class="sxs-lookup"><span data-stu-id="bd47a-244">tooallow full public read access tooblob resources, container metadata, and hello list of blobs in hello container, set hello Permission parameter too**Container**.</span></span> <span data-ttu-id="bd47a-245">Mer information finns i [Hantera anonym läsbehörighet toocontainers och blobbar](storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="bd47a-245">For more information, see [Manage anonymous read access toocontainers and blobs](storage-manage-access-to-resources.md).</span></span>
> 
> 

### <a name="how-tooupload-a-blob-into-a-container"></a><span data-ttu-id="bd47a-246">Hur tooupload en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="bd47a-246">How tooupload a blob into a container</span></span>
<span data-ttu-id="bd47a-247">Azure Blob Storage stöder blockblobbar och sidblobbar.</span><span class="sxs-lookup"><span data-stu-id="bd47a-247">Azure Blob Storage supports block blobs and page blobs.</span></span> <span data-ttu-id="bd47a-248">Mer information finns i [förstå Blockblobbar, Tilläggsblobbar och Sidblobbar](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd47a-248">For more information, see [Understanding Block Blobs, Append BLobs, and Page Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span></span>

<span data-ttu-id="bd47a-249">tooupload blobbar i behållaren tooa, kan du använda hello [Set AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="bd47a-249">tooupload blobs in tooa container, you can use hello [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) cmdlet.</span></span> <span data-ttu-id="bd47a-250">Som standard överför det här kommandot hello lokala filer tooa blockblob.</span><span class="sxs-lookup"><span data-stu-id="bd47a-250">By default, this command uploads hello local files tooa block blob.</span></span> <span data-ttu-id="bd47a-251">Du kan använda hello - BlobType parametern toospecify hello typ för hello blob.</span><span class="sxs-lookup"><span data-stu-id="bd47a-251">toospecify hello type for hello blob, you can use hello -BlobType parameter.</span></span>

<span data-ttu-id="bd47a-252">hello följande exempel kör hello [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) cmdlet tooget alla hello filer i hello angiven mapp och sedan skickar dem toohello nästa cmdlet genom att använda hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="bd47a-252">hello following example runs hello [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) cmdlet tooget all hello files in hello specified folder, and then passes them toohello next cmdlet by using hello pipeline operator.</span></span> <span data-ttu-id="bd47a-253">Hej [Set AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) cmdlet överför hello lokala filer tooyour behållare:</span><span class="sxs-lookup"><span data-stu-id="bd47a-253">hello [Set-AzureStorageBlobContent](/powershell/module/azure.storage/set-azurestorageblobcontent) cmdlet uploads hello local files tooyour container:</span></span>

```powershell
Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"
```

### <a name="how-toodownload-blobs-from-a-container"></a><span data-ttu-id="bd47a-254">Hur toodownload BLOB-objekt från en behållare</span><span class="sxs-lookup"><span data-stu-id="bd47a-254">How toodownload blobs from a container</span></span>
<span data-ttu-id="bd47a-255">hello som följande exempel visar hur toodownload BLOB-objekt från en behållare.</span><span class="sxs-lookup"><span data-stu-id="bd47a-255">hello following example demonstrates how toodownload blobs from a container.</span></span> <span data-ttu-id="bd47a-256">hello exempel först upprättar en anslutning tooAzure lagring med hjälp av hello kontexten för lagringskontot, vilket innefattar hello lagringskontonamn och den primära åtkomstnyckeln.</span><span class="sxs-lookup"><span data-stu-id="bd47a-256">hello example first establishes a connection tooAzure Storage using hello storage account context, which includes hello storage account name and its primary access key.</span></span> <span data-ttu-id="bd47a-257">Sedan hello exemplet hämtas en blobbreferens med hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="bd47a-257">Then, hello example retrieves a blob reference using hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet.</span></span> <span data-ttu-id="bd47a-258">Därefter hello exemplet används hello [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) cmdlet toodownload blobbar i hello lokala målmappen.</span><span class="sxs-lookup"><span data-stu-id="bd47a-258">Next, hello example uses hello [Get-AzureStorageBlobContent](/powershell/module/azure.storage/get-azurestorageblobcontent) cmdlet toodownload blobs into hello local destination folder.</span></span>

```powershell
#Define hello variables.
$ContainerName = "yourcontainername"
$DestinationFolder = "C:\DownloadImages"

#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#List all blobs in a container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Download blobs from a container.
New-Item -Path $DestinationFolder -ItemType Directory -Force
$blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx
```

### <a name="how-toocopy-blobs-from-one-storage-container-tooanother"></a><span data-ttu-id="bd47a-259">Hur toocopy BLOB-objekt från en lagring behållare tooanother</span><span class="sxs-lookup"><span data-stu-id="bd47a-259">How toocopy blobs from one storage container tooanother</span></span>
<span data-ttu-id="bd47a-260">Du kan kopiera BLOB storage-konton och regioner asynkront.</span><span class="sxs-lookup"><span data-stu-id="bd47a-260">You can copy blobs across storage accounts and regions asynchronously.</span></span> <span data-ttu-id="bd47a-261">hello visar exemplet nedan hur toocopy BLOB-objekt från en lagring behållaren tooanother i två olika lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="bd47a-261">hello following example demonstrates how toocopy blobs from one storage container tooanother in two different storage accounts.</span></span> <span data-ttu-id="bd47a-262">hello exempel först ställer in variabler för käll- och storage-konton och skapar sedan en kontext för lagring för varje konto.</span><span class="sxs-lookup"><span data-stu-id="bd47a-262">hello example first sets variables for source and destination storage accounts, and then creates a storage context for each account.</span></span> <span data-ttu-id="bd47a-263">Därefter hello exempel kopieras blobbar från hello källa behållaren toohello målbehållare med hello [Start AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="bd47a-263">Next, hello example copies blobs from hello source container toohello destination container using hello [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) cmdlet.</span></span> <span data-ttu-id="bd47a-264">hello exemplet förutsätter att hello käll- och storage-konton och behållare finns redan.</span><span class="sxs-lookup"><span data-stu-id="bd47a-264">hello example assumes that hello source and destination storage accounts and containers already exist.</span></span>

```powershell
#Define hello source storage account and context.
$SourceStorageAccountName = "yoursourcestorageaccount"
$SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
$SrcContainerName = "yoursrccontainername"
$SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

#Define hello destination storage account and context.
$DestStorageAccountName = "yourdeststorageaccount"
$DestStorageAccountKey = "Storage key for yourdeststorageaccount"
$DestContainerName = "destcontainername"
$DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

#Get a reference tooblobs in hello source container.
$blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

#Copy blobs from one container tooanother.
$blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext
```

<span data-ttu-id="bd47a-265">Observera att det här exemplet utför en asynkron kopia.</span><span class="sxs-lookup"><span data-stu-id="bd47a-265">Note that this example performs an asynchronous copy.</span></span> <span data-ttu-id="bd47a-266">Du kan övervaka hello status för varje kopia genom att köra hello [Get-AzureStorageBlobCopyState](/powershell/module/azure.storage/start-azurestorageblobcopystate) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="bd47a-266">You can monitor hello status of each copy by running hello [Get-AzureStorageBlobCopyState](/powershell/module/azure.storage/start-azurestorageblobcopystate) cmdlet.</span></span>

### <a name="how-toocopy-blobs-from-a-secondary-location"></a><span data-ttu-id="bd47a-267">Hur toocopy BLOB-objekt från en sekundär plats</span><span class="sxs-lookup"><span data-stu-id="bd47a-267">How toocopy blobs from a secondary location</span></span>
<span data-ttu-id="bd47a-268">Du kan kopiera blobbar från hello sekundära platsen för RA-GRS-aktiverat konto.</span><span class="sxs-lookup"><span data-stu-id="bd47a-268">You can copy blobs from hello secondary location of a RA-GRS-enabled account.</span></span>

```powershell
#define secondary storage context using a connection string constructed from secondary endpoints.
$SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext
```

### <a name="how-toodelete-a-blob"></a><span data-ttu-id="bd47a-269">Hur toodelete en blob</span><span class="sxs-lookup"><span data-stu-id="bd47a-269">How toodelete a blob</span></span>
<span data-ttu-id="bd47a-270">toodelete blob först hämta en blobbreferens och sedan anropa hello Remove-AzureStorageBlob cmdlet på den.</span><span class="sxs-lookup"><span data-stu-id="bd47a-270">toodelete a blob, first get a blob reference and then call hello Remove-AzureStorageBlob cmdlet on it.</span></span> <span data-ttu-id="bd47a-271">hello följande exempel tar bort alla hello blobbar i en viss behållare.</span><span class="sxs-lookup"><span data-stu-id="bd47a-271">hello following example deletes all hello blobs in a given container.</span></span> <span data-ttu-id="bd47a-272">hello exempel först ställer in variabler för ett lagringskonto och skapar sedan en kontext för lagring.</span><span class="sxs-lookup"><span data-stu-id="bd47a-272">hello example first sets variables for a storage account, and then creates a storage context.</span></span> <span data-ttu-id="bd47a-273">Därefter hello exemplet hämtas en blobbreferens med hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet och kör hello [ta bort AzureStorageBlob](/powershell/module/azure.storage/remove-azurestorageblob) cmdlet tooremove blobbar från en behållare i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="bd47a-273">Next, hello example retrieves a blob reference using hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet and runs hello [Remove-AzureStorageBlob](/powershell/module/azure.storage/remove-azurestorageblob) cmdlet tooremove blobs from a container in Azure storage.</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "containername"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference tooall hello blobs in hello container.
$blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

#Delete blobs in a specified container.
$blobs| Remove-AzureStorageBlob
```

## <a name="how-toomanage-azure-blob-snapshots"></a><span data-ttu-id="bd47a-274">Hur toomanage Azure blob-ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="bd47a-274">How toomanage Azure blob snapshots</span></span>
<span data-ttu-id="bd47a-275">Azure kan du skapa en ögonblicksbild av en blob.</span><span class="sxs-lookup"><span data-stu-id="bd47a-275">Azure lets you create a snapshot of a blob.</span></span> <span data-ttu-id="bd47a-276">En ögonblicksbild är en skrivskyddad version av en blob som utförs i en punkt i tiden.</span><span class="sxs-lookup"><span data-stu-id="bd47a-276">A snapshot is a read-only version of a blob that's taken at a point in time.</span></span> <span data-ttu-id="bd47a-277">När en ögonblicksbild har skapats, kan den läsa, kopieras, eller ta bort, men inte har ändrats.</span><span class="sxs-lookup"><span data-stu-id="bd47a-277">Once a snapshot has been created, it can be read, copied, or deleted, but not modified.</span></span> <span data-ttu-id="bd47a-278">Ögonblicksbilder ger ett sätt tooback upp en blob som det visas vid en tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="bd47a-278">Snapshots provide a way tooback up a blob as it appears at a moment in time.</span></span> <span data-ttu-id="bd47a-279">Mer information finns i [skapa en ögonblicksbild av en Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd47a-279">For more information, see [Creating a Snapshot of a Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span></span>

### <a name="how-toocreate-a-blob-snapshot"></a><span data-ttu-id="bd47a-280">Hur toocreate en blob-ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="bd47a-280">How toocreate a blob snapshot</span></span>
<span data-ttu-id="bd47a-281">toocreate en snaphot för en blobb först hämta en blobbreferens och sedan anropa hello `ICloudBlob.CreateSnapshot` metod på den.</span><span class="sxs-lookup"><span data-stu-id="bd47a-281">toocreate a snaphot of a blob, first get a blob reference and then call hello `ICloudBlob.CreateSnapshot` method on it.</span></span> <span data-ttu-id="bd47a-282">hello följande exempel först ställer in variabler för ett lagringskonto och skapar sedan en kontext för lagring.</span><span class="sxs-lookup"><span data-stu-id="bd47a-282">hello following example first sets variables for a storage account, and then creates a storage context.</span></span> <span data-ttu-id="bd47a-283">Därefter hello exemplet hämtas en blobbreferens med hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet och kör hello [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) metoden toocreate en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="bd47a-283">Next, hello example retrieves a blob reference using hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet and runs hello [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) method toocreate a snapshot.</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$ContainerName = "yourcontainername"
$BlobName = "yourblobname"
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Get a reference tooa blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

#Create a snapshot of hello blob.
$snap = $blob.ICloudBlob.CreateSnapshot()
```

### <a name="how-toolist-a-blobs-snapshots"></a><span data-ttu-id="bd47a-284">Hur toolist en blob ögonblicksbilder</span><span class="sxs-lookup"><span data-stu-id="bd47a-284">How toolist a blob's snapshots</span></span>
<span data-ttu-id="bd47a-285">Du kan skapa så många ögonblicksbilder som du vill använda för en blob.</span><span class="sxs-lookup"><span data-stu-id="bd47a-285">You can create as many snapshots as you want for a blob.</span></span> <span data-ttu-id="bd47a-286">Du kan ange hello ögonblicksbilder kopplade till blob-tootrack din aktuella ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="bd47a-286">You can list hello snapshots associated with your blob tootrack your current snapshots.</span></span> <span data-ttu-id="bd47a-287">hello följande exempel används en fördefinierad blob och anrop hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet toolist hello ögonblicksbilder för blobben.</span><span class="sxs-lookup"><span data-stu-id="bd47a-287">hello following example uses a predefined blob and calls hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet toolist hello snapshots of that blob.</span></span>  

```powershell
#Define hello blob name.
$BlobName = "yourblobname"

#List hello snapshots of a blob.
Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }
```

### <a name="how-toocopy-a-snapshot-of-a-blob"></a><span data-ttu-id="bd47a-288">Hur toocopy en ögonblicksbild av en blob</span><span class="sxs-lookup"><span data-stu-id="bd47a-288">How toocopy a snapshot of a blob</span></span>
<span data-ttu-id="bd47a-289">Du kan kopiera en ögonblicksbild av en blob toorestore hello ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="bd47a-289">You can copy a snapshot of a blob toorestore hello snapshot.</span></span> <span data-ttu-id="bd47a-290">Detaljerad information och begränsningar finns i [skapa en ögonblicksbild av en Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd47a-290">For detailed information and restrictions, see [Creating a Snapshot of a Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).</span></span> <span data-ttu-id="bd47a-291">hello följande exempel först ställer in variabler för ett lagringskonto och skapar sedan en kontext för lagring.</span><span class="sxs-lookup"><span data-stu-id="bd47a-291">hello following example first sets variables for a storage account, and then creates a storage context.</span></span> <span data-ttu-id="bd47a-292">Hello exempel definierar sedan hello-behållaren och blob namn variabler.</span><span class="sxs-lookup"><span data-stu-id="bd47a-292">Next, hello example defines hello container and blob name variables.</span></span> <span data-ttu-id="bd47a-293">hello exemplet hämtas en blobbreferens med hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet och kör hello [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) metoden toocreate en ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="bd47a-293">hello example retrieves a blob reference using hello [Get-AzureStorageBlob](/powershell/module/azure.storage/get-azurestorageblob) cmdlet and runs hello [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) method toocreate a snapshot.</span></span> <span data-ttu-id="bd47a-294">Sedan hello exempel kör hello [Start AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) cmdlet toocopy hello ögonblicksbild av en blob med hello ICloudBlob objektet för hello källa blob.</span><span class="sxs-lookup"><span data-stu-id="bd47a-294">Then, hello example runs hello [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) cmdlet toocopy hello snapshot of a blob using hello ICloudBlob object for hello source blob.</span></span> <span data-ttu-id="bd47a-295">Kontrollera att baseras tooupdate hello variabler på din konfiguration innan du kör hello exempel.</span><span class="sxs-lookup"><span data-stu-id="bd47a-295">Be sure tooupdate hello variables based on your configuration before running hello example.</span></span> <span data-ttu-id="bd47a-296">Den hello följande exempel förutsätter att hello käll- och behållare och hello källa blob finns redan.</span><span class="sxs-lookup"><span data-stu-id="bd47a-296">Note that hello following example assumes that hello source and destination containers, and hello source blob already exist.</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

#Define hello variables.
$SrcContainerName = "yoursourcecontainername"
$DestContainerName = "yourdestcontainername"
$SrcBlobName = "yourblobname"
$DestBlobName = "CopyBlobName"

#Get a reference tooa blob.
$blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

#Create a snapshot of a blob.
$snap = $blob.ICloudBlob.CreateSnapshot()

#Copy hello snapshot tooanother container.
Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName
```

<span data-ttu-id="bd47a-297">Nu när du har lärt dig hur toomanage Azure BLOB-objekt och blob-ögonblicksbilder med Azure PowerShell gå toohello nästa avsnitt toolearn hur toomanage tabeller, köer och filer.</span><span class="sxs-lookup"><span data-stu-id="bd47a-297">Now that you have learned how toomanage Azure blobs and blob snapshots with Azure PowerShell, go toohello next section toolearn how toomanage tables, queues, and files.</span></span>

## <a name="how-toomanage-azure-tables-and-table-entities"></a><span data-ttu-id="bd47a-298">Hur toomanage Azure tabeller och tabell entiteter</span><span class="sxs-lookup"><span data-stu-id="bd47a-298">How toomanage Azure tables and table entities</span></span>
<span data-ttu-id="bd47a-299">Azure Table storage-tjänsten är en NoSQL-datalager som du kan använda toostore och fråga stora mängder strukturerad, icke-relationella data.</span><span class="sxs-lookup"><span data-stu-id="bd47a-299">Azure Table storage service is a NoSQL datastore, which you can use toostore and query huge sets of structured, non-relational data.</span></span> <span data-ttu-id="bd47a-300">hello huvudkomponenterna i hello-tjänsten är tabeller, enheter och egenskaper.</span><span class="sxs-lookup"><span data-stu-id="bd47a-300">hello main components of hello service are tables, entities, and properties.</span></span> <span data-ttu-id="bd47a-301">En tabell är en samling med entiteter.</span><span class="sxs-lookup"><span data-stu-id="bd47a-301">A table is a collection of entities.</span></span> <span data-ttu-id="bd47a-302">En entitet är en uppsättning egenskaper.</span><span class="sxs-lookup"><span data-stu-id="bd47a-302">An entity is a set of properties.</span></span> <span data-ttu-id="bd47a-303">Varje entitet kan ha upp too252 egenskaper, som är alla namn / värde-par.</span><span class="sxs-lookup"><span data-stu-id="bd47a-303">Each entity can have up too252 properties, which are all name-value pairs.</span></span> <span data-ttu-id="bd47a-304">Det här avsnittet förutsätter att du redan är bekant med principerna för hello Azure Table Storage-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bd47a-304">This section assumes that you are already familiar with hello Azure Table Storage Service concepts.</span></span> <span data-ttu-id="bd47a-305">Detaljerad information finns i [hello förstå tabelltjänst-datamodellen](http://msdn.microsoft.com/library/azure/dd179338.aspx) och [komma igång med Azure Table storage med hjälp av .NET](storage-dotnet-how-to-use-tables.md).</span><span class="sxs-lookup"><span data-stu-id="bd47a-305">For detailed information, see [Understanding hello Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx) and [Get started with Azure Table storage using .NET](storage-dotnet-how-to-use-tables.md).</span></span>

<span data-ttu-id="bd47a-306">I följande underavsnitt hello, lär du dig hur toomanage Azure Table storage tjänst med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bd47a-306">In hello following subsections, you'll learn how toomanage Azure Table storage service using Azure PowerShell.</span></span> <span data-ttu-id="bd47a-307">hello beskrivs scenarier där **skapar**, **bort**, och **hämtar** **tabeller**, samt **att lägga till**, **frågar**, och **bort tabellentiteter**.</span><span class="sxs-lookup"><span data-stu-id="bd47a-307">hello scenarios covered include **creating**, **deleting**, and **retrieving** **tables**, as well as **adding**, **querying**, and **deleting table entities**.</span></span>

### <a name="how-toocreate-a-table"></a><span data-ttu-id="bd47a-308">Hur toocreate en tabell</span><span class="sxs-lookup"><span data-stu-id="bd47a-308">How toocreate a table</span></span>
<span data-ttu-id="bd47a-309">Varje tabell måste finnas i ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="bd47a-309">Every table must reside in an Azure storage account.</span></span> <span data-ttu-id="bd47a-310">hello exemplet nedan visar hur toocreate en tabell i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="bd47a-310">hello following example demonstrates how toocreate a table in Azure Storage.</span></span> <span data-ttu-id="bd47a-311">hello exempel först upprättar en anslutning tooAzure lagring med hjälp av hello kontexten för lagringskontot, vilket innefattar hello lagringskontonamn och dess snabbtangent.</span><span class="sxs-lookup"><span data-stu-id="bd47a-311">hello example first establishes a connection tooAzure Storage using hello storage account context, which includes hello storage account name and its access key.</span></span> <span data-ttu-id="bd47a-312">Därefter används hello [ny AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) cmdlet toocreate en tabell i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="bd47a-312">Next, it uses hello [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) cmdlet toocreate a table in Azure Storage.</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = "Storage key for yourstorageaccount ends with =="
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

#Create a new table.
$tabName = "yourtablename"
New-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-tooretrieve-a-table"></a><span data-ttu-id="bd47a-313">Hur tooretrieve en tabell</span><span class="sxs-lookup"><span data-stu-id="bd47a-313">How tooretrieve a table</span></span>
<span data-ttu-id="bd47a-314">Du kan fråga efter och hämta en eller alla tabeller i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="bd47a-314">You can query and retrieve one or all tables in a Storage account.</span></span> <span data-ttu-id="bd47a-315">hello exemplet nedan visar hur en given tabell med hjälp av tooretrieve hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="bd47a-315">hello following example demonstrates how tooretrieve a given table using hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span>

```powershell
#Retrieve a table.
$tabName = "yourtablename"
Get-AzureStorageTable –Name $tabName –Context $Ctx
```

<span data-ttu-id="bd47a-316">Om du anropar hello Get-AzureStorageTable cmdlet utan några parametrar hämtar alla lagringstabeller som för ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="bd47a-316">If you call hello Get-AzureStorageTable cmdlet without any parameters, it gets all storage tables for a Storage account.</span></span>

### <a name="how-toodelete-a-table"></a><span data-ttu-id="bd47a-317">Hur toodelete en tabell</span><span class="sxs-lookup"><span data-stu-id="bd47a-317">How toodelete a table</span></span>
<span data-ttu-id="bd47a-318">Du kan ta bort en tabell från ett lagringskonto med hjälp av hello [ta bort AzureStorageTable](/powershell/module/azure.storage/remove-azurestoragetable) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="bd47a-318">You can delete a table from a storage account by using hello [Remove-AzureStorageTable](/powershell/module/azure.storage/remove-azurestoragetable) cmdlet.</span></span>  

```powershell
#Delete a table.
$tabName = "yourtablename"
Remove-AzureStorageTable –Name $tabName –Context $Ctx
```

### <a name="how-toomanage-table-entities"></a><span data-ttu-id="bd47a-319">Hur toomanage tabell entiteter</span><span class="sxs-lookup"><span data-stu-id="bd47a-319">How toomanage table entities</span></span>
<span data-ttu-id="bd47a-320">För närvarande tillhandahålla Azure PowerShell inte cmdlets toomanage tabellentiteter direkt.</span><span class="sxs-lookup"><span data-stu-id="bd47a-320">Currently, Azure PowerShell does not provide cmdlets toomanage table entities directly.</span></span> <span data-ttu-id="bd47a-321">tooperform åtgärder på tabellentiteter, kan du använda hello klasser i hello [Azure Storage-klientbibliotek för .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd47a-321">tooperform operations on table entities, you can use hello classes given in hello [Azure Storage Client Library for .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).</span></span>

#### <a name="how-tooadd-table-entities"></a><span data-ttu-id="bd47a-322">Hur tooadd tabell entiteter</span><span class="sxs-lookup"><span data-stu-id="bd47a-322">How tooadd table entities</span></span>
<span data-ttu-id="bd47a-323">tooadd en entitet tooa tabell först skapa ett objekt som definierar egenskaper för enhet.</span><span class="sxs-lookup"><span data-stu-id="bd47a-323">tooadd an entity tooa table, first create an object that defines your entity properties.</span></span> <span data-ttu-id="bd47a-324">En entitet kan ha upp too255 egenskaper, inklusive 3 Systemegenskaper: **PartitionKey**, **RowKey**, och **tidsstämpel**.</span><span class="sxs-lookup"><span data-stu-id="bd47a-324">An entity can have up too255 properties, including 3 system properties: **PartitionKey**, **RowKey**, and **Timestamp**.</span></span> <span data-ttu-id="bd47a-325">Du ansvarar för att lägga till och uppdatera hello värdena för **PartitionKey** och **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="bd47a-325">You are responsible for inserting and updating hello values of **PartitionKey** and **RowKey**.</span></span> <span data-ttu-id="bd47a-326">hello-servern hanterar hello värdet för **tidsstämpel**, som inte ändras.</span><span class="sxs-lookup"><span data-stu-id="bd47a-326">hello server manages hello value of **Timestamp**, which cannot be modified.</span></span> <span data-ttu-id="bd47a-327">Tillsammans hello **PartitionKey** och **RowKey** identifiera varje entitet i en tabell.</span><span class="sxs-lookup"><span data-stu-id="bd47a-327">Together hello **PartitionKey** and **RowKey** uniquely identify every entity within a table.</span></span>

* <span data-ttu-id="bd47a-328">**PartitionKey**: Anger hello-partition som hello entiteten är lagrad i.</span><span class="sxs-lookup"><span data-stu-id="bd47a-328">**PartitionKey**: Determines hello partition that hello entity is stored in.</span></span>
* <span data-ttu-id="bd47a-329">**RowKey**: identifierar hello entiteten inom hello partition.</span><span class="sxs-lookup"><span data-stu-id="bd47a-329">**RowKey**: Uniquely identifies hello entity within hello partition.</span></span>

<span data-ttu-id="bd47a-330">Du kan definiera too252 anpassade egenskaper för en entitet.</span><span class="sxs-lookup"><span data-stu-id="bd47a-330">You may define up too252 custom properties for an entity.</span></span> <span data-ttu-id="bd47a-331">Mer information finns i [hello förstå tabelltjänst-datamodellen](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd47a-331">For more information, see [Understanding hello Table Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx).</span></span>

<span data-ttu-id="bd47a-332">hello exemplet nedan visar hur tooadd entiteter tooa tabell.</span><span class="sxs-lookup"><span data-stu-id="bd47a-332">hello following example demonstrates how tooadd entities tooa table.</span></span> <span data-ttu-id="bd47a-333">hello exempel visas hur tooretrieve hello medarbetare tabell och lägga till flera enheter till den.</span><span class="sxs-lookup"><span data-stu-id="bd47a-333">hello example shows how tooretrieve hello employee table and add several entities into it.</span></span> <span data-ttu-id="bd47a-334">Först den upprättar en anslutning tooAzure lagring med hjälp av hello kontexten för lagringskontot, vilket innefattar hello lagringskontonamn och dess snabbtangent.</span><span class="sxs-lookup"><span data-stu-id="bd47a-334">First, it establishes a connection tooAzure Storage using hello storage account context, which includes hello storage account name and its access key.</span></span> <span data-ttu-id="bd47a-335">Sedan hämtar hello får tabellen med hjälp av hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="bd47a-335">Next, it retrieves hello given table using hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span> <span data-ttu-id="bd47a-336">Om hello tabellen inte finns, hello [ny AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) cmdlet har använt toocreate en tabell i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="bd47a-336">If hello table does not exist, hello [New-AzureStorageTable](/powershell/module/azure.storage/new-azurestoragetable) cmdlet is used toocreate a table in Azure Storage.</span></span> <span data-ttu-id="bd47a-337">Därefter definierar hello exempel en egen funktion Lägg till entiteten tooadd entiteter toohello tabell genom att ange varje entitet partition och radnyckel.</span><span class="sxs-lookup"><span data-stu-id="bd47a-337">Next, hello example defines a custom function Add-Entity tooadd entities toohello table by specifying each entity's partition and row key.</span></span> <span data-ttu-id="bd47a-338">hello Lägg till entiteten funktionen anrop hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet på hello [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) klassen toocreate ett entitetsobjekt.</span><span class="sxs-lookup"><span data-stu-id="bd47a-338">hello Add-Entity function calls hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet on hello [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) class toocreate an entity object.</span></span> <span data-ttu-id="bd47a-339">Senare hello exemplet anropar hello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) -metoden i den här entiteten objektet tooadd den tooa tabell.</span><span class="sxs-lookup"><span data-stu-id="bd47a-339">Later, hello example calls hello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) method on this entity object tooadd it tooa table.</span></span>

```powershell
#Function Add-Entity: Adds an employee entity tooa table.
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

#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Retrieve hello table if it already exists.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

#Create a new table if it does not exist.
if ($table -eq $null)
{
   $table = New-AzureStorageTable –Name $TableName -Context $Ctx
}

#Add multiple entities tooa table.
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4
```

#### <a name="how-tooquery-table-entities"></a><span data-ttu-id="bd47a-340">Hur tooquery tabell entiteter</span><span class="sxs-lookup"><span data-stu-id="bd47a-340">How tooquery table entities</span></span>
<span data-ttu-id="bd47a-341">tooquery en tabell, använder hello [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) klass.</span><span class="sxs-lookup"><span data-stu-id="bd47a-341">tooquery a table, use hello [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) class.</span></span> <span data-ttu-id="bd47a-342">hello följande exempel förutsätter att du redan har kört hello-skriptet som anges i hello hur tooadd entiteter avsnitt i handboken.</span><span class="sxs-lookup"><span data-stu-id="bd47a-342">hello following example assumes that you've already run hello script given in hello How tooadd entities section of this guide.</span></span> <span data-ttu-id="bd47a-343">hello exempel först upprättar en anslutning tooAzure lagring med hello lagring kontext, vilket innefattar hello lagringskontonamn och dess snabbtangent.</span><span class="sxs-lookup"><span data-stu-id="bd47a-343">hello example first establishes a connection tooAzure Storage using hello storage context, which includes hello storage account name and its access key.</span></span> <span data-ttu-id="bd47a-344">Försök därefter tooretrieve hello skapat ”anställda” tabell med hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="bd47a-344">Next, it tries tooretrieve hello previously created "Employees" table using hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span> <span data-ttu-id="bd47a-345">Anropar hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet på hello Microsoft.WindowsAzure.Storage.Table.TableQuery klassen skapas ett nytt frågeobjekt.</span><span class="sxs-lookup"><span data-stu-id="bd47a-345">Calling hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet on hello Microsoft.WindowsAzure.Storage.Table.TableQuery class creates a new query object.</span></span> <span data-ttu-id="bd47a-346">hello exempel söker efter hello entiteter som har en ”ID”-kolumn vars värde är 1 som anges i ett sträng-filter.</span><span class="sxs-lookup"><span data-stu-id="bd47a-346">hello example looks for hello entities that have an 'ID' column whose value is 1 as specified in a string filter.</span></span> <span data-ttu-id="bd47a-347">Detaljerad information finns i [frågor till tabeller och de entiteter](http://msdn.microsoft.com/library/azure/dd894031.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd47a-347">For detailed information, see [Querying Tables and Entities](http://msdn.microsoft.com/library/azure/dd894031.aspx).</span></span> <span data-ttu-id="bd47a-348">När du kör den här frågan returnerar alla entiteter som matchar hello filtervillkor.</span><span class="sxs-lookup"><span data-stu-id="bd47a-348">When you run this query, it returns all entities that match hello filter criteria.</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$TableName = "Employees"

#Get a reference tooa table.
$table = Get-AzureStorageTable –Name $TableName -Context $Ctx

#Create a table query.
$query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

#Define columns tooselect.
$list = New-Object System.Collections.Generic.List[string]
$list.Add("RowKey")
$list.Add("ID")
$list.Add("Name")

#Set query details.
$query.FilterString = "ID gt 0"
$query.SelectColumns = $list
$query.TakeCount = 20

#Execute hello query.
$entities = $table.CloudTable.ExecuteQuery($query)

#Display entity properties with hello table format.
$entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize
```

#### <a name="how-toodelete-table-entities"></a><span data-ttu-id="bd47a-349">Hur toodelete tabell entiteter</span><span class="sxs-lookup"><span data-stu-id="bd47a-349">How toodelete table entities</span></span>
<span data-ttu-id="bd47a-350">Du kan ta bort en entitet med dess partition och raden nycklar.</span><span class="sxs-lookup"><span data-stu-id="bd47a-350">You can delete an entity using its partition and row keys.</span></span> <span data-ttu-id="bd47a-351">hello följande exempel förutsätter att du redan har kört hello-skriptet som anges i hello hur tooadd entiteter avsnitt i handboken.</span><span class="sxs-lookup"><span data-stu-id="bd47a-351">hello following example assumes that you've already run hello script given in hello How tooadd entities section of this guide.</span></span> <span data-ttu-id="bd47a-352">hello exempel först upprättar en anslutning tooAzure lagring med hello lagring kontext, vilket innefattar hello lagringskontonamn och dess snabbtangent.</span><span class="sxs-lookup"><span data-stu-id="bd47a-352">hello example first establishes a connection tooAzure Storage using hello storage context, which includes hello storage account name and its access key.</span></span> <span data-ttu-id="bd47a-353">Försök därefter tooretrieve hello skapat ”anställda” tabell med hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="bd47a-353">Next, it tries tooretrieve hello previously created "Employees" table using hello [Get-AzureStorageTable](/powershell/module/azure.storage/get-azurestoragetable) cmdlet.</span></span> <span data-ttu-id="bd47a-354">Om hello tabellen finns hello exemplet anropar hello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) metoden tooretrieve en entitet baserat på dess partition och raden nyckelvärden.</span><span class="sxs-lookup"><span data-stu-id="bd47a-354">If hello table exists, hello example calls hello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) method tooretrieve an entity based on its partition and row key values.</span></span> <span data-ttu-id="bd47a-355">Sedan skicka hello entiteten toohello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) metoden toodelete.</span><span class="sxs-lookup"><span data-stu-id="bd47a-355">Then, pass hello entity toohello [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) method toodelete.</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve hello table.
$TableName = "Employees"
$table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

#If hello table exists, start deleting its entities.
if ($table -ne $null) 
{
    #Together hello PartitionKey and RowKey uniquely identify every  
    #entity within a table.
    $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
    $entity = $tableResult.Result
    if ($entity -ne $null)
    {
        #Delete hello entity.
        $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
}
```

## <a name="how-toomanage-azure-queues-and-queue-messages"></a><span data-ttu-id="bd47a-356">Hur toomanage Azure köer och meddelanden i kö</span><span class="sxs-lookup"><span data-stu-id="bd47a-356">How toomanage Azure queues and queue messages</span></span>
<span data-ttu-id="bd47a-357">Azure Queue storage är en tjänst för att lagra stora mängder meddelanden som kan nås från var som helst i Hej världen via autentiserade anrop med HTTP eller HTTPS.</span><span class="sxs-lookup"><span data-stu-id="bd47a-357">Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in hello world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="bd47a-358">Det här avsnittet förutsätter att du redan är bekant med principerna för hello Azure Queue Storage-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bd47a-358">This section assumes that you are already familiar with hello Azure Queue Storage Service concepts.</span></span> <span data-ttu-id="bd47a-359">Detaljerad information finns i [komma igång med Azure Queue storage med hjälp av .NET](storage-dotnet-how-to-use-queues.md).</span><span class="sxs-lookup"><span data-stu-id="bd47a-359">For detailed information, see [Get started with Azure Queue storage using .NET](storage-dotnet-how-to-use-queues.md).</span></span>

<span data-ttu-id="bd47a-360">Det här avsnittet visar hur toomanage Azure Queue storage tjänst med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bd47a-360">This section will show you how toomanage Azure Queue storage service using Azure PowerShell.</span></span> <span data-ttu-id="bd47a-361">hello beskrivs scenarier där **infoga** och **bort** kö meddelanden, samt **skapar**, **ta bort**, och **hämtar köer**.</span><span class="sxs-lookup"><span data-stu-id="bd47a-361">hello scenarios covered include **inserting** and **deleting** queue messages, as well as **creating**, **deleting**, and **retrieving queues**.</span></span>

### <a name="how-toocreate-a-queue"></a><span data-ttu-id="bd47a-362">Hur toocreate en kö</span><span class="sxs-lookup"><span data-stu-id="bd47a-362">How toocreate a queue</span></span>
<span data-ttu-id="bd47a-363">hello följande exempel först upprättar en anslutning tooAzure lagring med hjälp av hello kontexten för lagringskontot, vilket innefattar hello lagringskontonamn och dess snabbtangent.</span><span class="sxs-lookup"><span data-stu-id="bd47a-363">hello following example first establishes a connection tooAzure Storage using hello storage account context, which includes hello storage account name and its access key.</span></span> <span data-ttu-id="bd47a-364">Därefter anropar [ny AzureStorageQueue](/powershell/module/azure.storage/new-azurestoragequeue) cmdlet toocreate en kö med namnet 'könamn'.</span><span class="sxs-lookup"><span data-stu-id="bd47a-364">Next, it calls [New-AzureStorageQueue](/powershell/module/azure.storage/new-azurestoragequeue) cmdlet toocreate a queue named 'queuename'.</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
$QueueName = "queuename"
$Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx
```

<span data-ttu-id="bd47a-365">Information om namngivningskonventioner för Azure-kötjänsten finns [namngivning av köer och Metadata](http://msdn.microsoft.com/library/azure/dd179349.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd47a-365">For information on naming conventions for Azure Queue Service, see [Naming Queues and Metadata](http://msdn.microsoft.com/library/azure/dd179349.aspx).</span></span>

### <a name="how-tooretrieve-a-queue"></a><span data-ttu-id="bd47a-366">Hur tooretrieve en kö</span><span class="sxs-lookup"><span data-stu-id="bd47a-366">How tooretrieve a queue</span></span>
<span data-ttu-id="bd47a-367">Du kan fråga efter och hämta en särskild kö eller en lista över alla hello köer i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="bd47a-367">You can query and retrieve a specific queue or a list of all hello queues in a Storage account.</span></span> <span data-ttu-id="bd47a-368">hello exemplet nedan visar hur en kö som anges med hjälp av tooretrieve hello [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="bd47a-368">hello following example demonstrates how tooretrieve a specified queue using hello [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) cmdlet.</span></span>

```powershell
#Retrieve a queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx
```

<span data-ttu-id="bd47a-369">Om du anropar hello [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) cmdlet utan några parametrar får den en lista över alla hello köer.</span><span class="sxs-lookup"><span data-stu-id="bd47a-369">If you call hello [Get-AzureStorageQueue](/powershell/module/azure.storage/get-azurestoragequeue) cmdlet without any parameters, it gets a list of all hello queues.</span></span>

### <a name="how-toodelete-a-queue"></a><span data-ttu-id="bd47a-370">Hur toodelete en kö</span><span class="sxs-lookup"><span data-stu-id="bd47a-370">How toodelete a queue</span></span>
<span data-ttu-id="bd47a-371">toodelete en kö och alla hälsningsmeddelande finns i den anropet hello Remove-AzureStorageQueue cmdlet.</span><span class="sxs-lookup"><span data-stu-id="bd47a-371">toodelete a queue and all hello messages contained in it, call hello Remove-AzureStorageQueue cmdlet.</span></span> <span data-ttu-id="bd47a-372">hello som följande exempel visar hur en kö som anges med hjälp av toodelete hello Remove-AzureStorageQueue cmdlet.</span><span class="sxs-lookup"><span data-stu-id="bd47a-372">hello following example shows how toodelete a specified queue using hello Remove-AzureStorageQueue cmdlet.</span></span>

```powershell
#Delete a queue.
$QueueName = "yourqueuename"
Remove-AzureStorageQueue –Name $QueueName –Context $Ctx
```

#### <a name="how-tooinsert-a-message-into-a-queue"></a><span data-ttu-id="bd47a-373">Hur tooinsert ett meddelande i en kö</span><span class="sxs-lookup"><span data-stu-id="bd47a-373">How tooinsert a message into a queue</span></span>
<span data-ttu-id="bd47a-374">tooinsert ett meddelande i en befintlig kö först skapa en ny instans av hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) klass.</span><span class="sxs-lookup"><span data-stu-id="bd47a-374">tooinsert a message into an existing queue, first create a new instance of hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) class.</span></span> <span data-ttu-id="bd47a-375">Därefter anropar hello [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) metod.</span><span class="sxs-lookup"><span data-stu-id="bd47a-375">Next, call hello [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) method.</span></span> <span data-ttu-id="bd47a-376">En CloudQueueMessage kan skapas från en sträng (i UTF-8-format) eller en byte-matris.</span><span class="sxs-lookup"><span data-stu-id="bd47a-376">A CloudQueueMessage can be created from either a string (in UTF-8 format) or a byte array.</span></span>

<span data-ttu-id="bd47a-377">hello som följande exempel visar hur tooadd meddelande tooa kön.</span><span class="sxs-lookup"><span data-stu-id="bd47a-377">hello following example demonstrates how tooadd message tooa queue.</span></span> <span data-ttu-id="bd47a-378">hello exempel först upprättar en anslutning tooAzure lagring med hjälp av hello kontexten för lagringskontot, vilket innefattar hello lagringskontonamn och dess snabbtangent.</span><span class="sxs-lookup"><span data-stu-id="bd47a-378">hello example first establishes a connection tooAzure Storage using hello storage account context, which includes hello storage account name and its access key.</span></span> <span data-ttu-id="bd47a-379">Sedan hämtar hello kö som anges med hjälp av hello [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="bd47a-379">Next, it retrieves hello specified queue using hello [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) cmdlet.</span></span> <span data-ttu-id="bd47a-380">Om hello kön finns hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet har använt toocreate en instans av hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) klass.</span><span class="sxs-lookup"><span data-stu-id="bd47a-380">If hello queue exists, hello [New-Object](http://technet.microsoft.com/library/hh849885.aspx) cmdlet is used toocreate an instance of hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) class.</span></span> <span data-ttu-id="bd47a-381">Senare hello exemplet anropar hello [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) -metoden i det här meddelandet objektet tooadd den tooa kön.</span><span class="sxs-lookup"><span data-stu-id="bd47a-381">Later, hello example calls hello [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) method on this message object tooadd it tooa queue.</span></span> <span data-ttu-id="bd47a-382">Här är kod som hämtar en kö och infogar 'MessageInfo' hello-meddelande:</span><span class="sxs-lookup"><span data-stu-id="bd47a-382">Here is code which retrieves a queue and inserts hello message 'MessageInfo':</span></span>

```powershell
#Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

#Retrieve hello queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

#If hello queue exists, add a new message.
if ($Queue -ne $null) {
   # Create a new message using a constructor of hello CloudQueueMessage class.
   $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

   # Add a new message toohello queue.
   $Queue.CloudQueue.AddMessage($QueueMessage)
}
```

#### <a name="how-toode-queue-at-hello-next-message"></a><span data-ttu-id="bd47a-383">Hur toode-kö på hello nästa meddelande</span><span class="sxs-lookup"><span data-stu-id="bd47a-383">How toode-queue at hello next message</span></span>
<span data-ttu-id="bd47a-384">Koden tar bort ett meddelande från en kö i två steg.</span><span class="sxs-lookup"><span data-stu-id="bd47a-384">Your code de-queues a message from a queue in two steps.</span></span> <span data-ttu-id="bd47a-385">När du anropar hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) metod, du får hello nästa meddelande i en kö.</span><span class="sxs-lookup"><span data-stu-id="bd47a-385">When you call hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) method, you get hello next message in a queue.</span></span> <span data-ttu-id="bd47a-386">Ett meddelande som returneras från **GetMessage** blir osynligt tooany annan kod läsa meddelanden från den här kön.</span><span class="sxs-lookup"><span data-stu-id="bd47a-386">A message returned from **GetMessage** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="bd47a-387">toofinish att ta bort hello-meddelande från kön hello, måste du också anropa hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) metod.</span><span class="sxs-lookup"><span data-stu-id="bd47a-387">toofinish removing hello message from hello queue, you must also call hello [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) method.</span></span> <span data-ttu-id="bd47a-388">Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer som om din kod inte tooprocess får ett meddelande på grund av fel toohardware eller programvara, en annan instans av koden hello samma meddelande och försök igen.</span><span class="sxs-lookup"><span data-stu-id="bd47a-388">This two-step process of removing a message assures that if your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="bd47a-389">Koden anropar **DeleteMessage** direkt efter hello-meddelande har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="bd47a-389">Your code calls **DeleteMessage** right after hello message has been processed.</span></span>

```powershell
# Define hello storage account and context.
$StorageAccountName = "yourstorageaccount"
$StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
$Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

# Retrieve hello queue.
$QueueName = "queuename"
$Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

$InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

# Get hello message object from hello queue.
$QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
# Delete hello message.
$Queue.CloudQueue.DeleteMessage($QueueMessage)
```

## <a name="how-toomanage-azure-file-shares-and-files"></a><span data-ttu-id="bd47a-390">Hur toomanage Azure filen delar och filer</span><span class="sxs-lookup"><span data-stu-id="bd47a-390">How toomanage Azure file shares and files</span></span>
<span data-ttu-id="bd47a-391">Azure File storage erbjuder delad lagring för program som använder hello SMB-standardprotokollet.</span><span class="sxs-lookup"><span data-stu-id="bd47a-391">Azure File storage offers shared storage for applications using hello standard SMB protocol.</span></span> <span data-ttu-id="bd47a-392">Microsoft Azure-datorer och molntjänster kan dela fildata över programkomponenter via monterade resurser och lokala program kan komma åt fildata på en resurs via hello fil-API för storage eller Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bd47a-392">Microsoft Azure virtual machines and cloud services can share file data across application components via mounted shares, and on-premises applications can access file data in a share via hello File storage API or Azure PowerShell.</span></span>

<span data-ttu-id="bd47a-393">Mer information om Azure File storage finns [Kom igång med Azure File storage i Windows](storage-dotnet-how-to-use-files.md) och [File Service REST API](http://msdn.microsoft.com/library/azure/dn167006.aspx).</span><span class="sxs-lookup"><span data-stu-id="bd47a-393">For more information on Azure File storage, see [Get started with Azure File storage on Windows](storage-dotnet-how-to-use-files.md) and [File Service REST API](http://msdn.microsoft.com/library/azure/dn167006.aspx).</span></span>

## <a name="how-tooset-and-query-storage-analytics"></a><span data-ttu-id="bd47a-394">Hur tooset och fråga storage analytics</span><span class="sxs-lookup"><span data-stu-id="bd47a-394">How tooset and query storage analytics</span></span>
<span data-ttu-id="bd47a-395">Du kan använda [Azure Storage Analytics](storage-analytics.md) toocollect mått för Azure storage-konton och loggdata om begäranden skickas tooyour storage-konto.</span><span class="sxs-lookup"><span data-stu-id="bd47a-395">You can use [Azure Storage Analytics](storage-analytics.md) toocollect metrics for your Azure storage accounts and log data about requests sent tooyour storage account.</span></span> <span data-ttu-id="bd47a-396">Du kan använda lagring mått toomonitor hello hälsotillståndet för ett lagringskonto och lagring loggning toodiagnose och felsöka problem med ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="bd47a-396">You can use storage metrics toomonitor hello health of a storage account, and storage logging toodiagnose and troubleshoot issues with your storage account.</span></span> <span data-ttu-id="bd47a-397">Du kan konfigurera övervakning med hjälp av hello Azure-portalen eller Windows PowerShell eller via programmering med hello storage-klientbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="bd47a-397">You can configure monitoring using hello Azure portal or Windows PowerShell, or programmatically using hello storage client library.</span></span> <span data-ttu-id="bd47a-398">Lagring loggning sker serversidan och aktiverar toorecord information för både slutförda och misslyckade begäranden på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="bd47a-398">Storage logging happens server-side and enables you toorecord details for both successful and failed requests in your storage account.</span></span> <span data-ttu-id="bd47a-399">Dessa loggar aktivera toosee detaljer för Läs-, Skriv- och delete-åtgärder mot dina tabeller, köer och blobbar samt hello orsakerna till misslyckade begäranden.</span><span class="sxs-lookup"><span data-stu-id="bd47a-399">These logs enable you toosee details of read, write, and delete operations against your tables, queues, and blobs as well as hello reasons for failed requests.</span></span>

<span data-ttu-id="bd47a-400">hur tooenable och visa Storage-mätvärden data med hjälp av PowerShell, se toolearn [hur tooenable Storage-mätvärden med hjälp av PowerShell](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).</span><span class="sxs-lookup"><span data-stu-id="bd47a-400">toolearn how tooenable and view Storage Metrics data using PowerShell, see [How tooenable Storage Metrics using PowerShell](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).</span></span>

<span data-ttu-id="bd47a-401">hur tooenable och hämta Storage data för loggning med hjälp av PowerShell, se toolearn [hur tooenable lagring loggning med hjälp av PowerShell](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) och [hitta din lagring loggning loggdata](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).</span><span class="sxs-lookup"><span data-stu-id="bd47a-401">toolearn how tooenable and retrieve Storage Logging data using PowerShell, see [How tooenable Storage Logging using PowerShell](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) and [Finding your Storage Logging log data](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).</span></span>
<span data-ttu-id="bd47a-402">Detaljerad information om hur du använder Storage-mätvärden och lagring loggning tootroubleshoot lagringsproblem finns [övervakning, diagnostisera och felsöka Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="bd47a-402">For detailed information on using Storage Metrics and Storage Logging tootroubleshoot storage issues, see [Monitoring, Diagnosing, and Troubleshooting Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).</span></span>

## <a name="how-toomanage-shared-access-signature-sas-and-stored-access-policy"></a><span data-ttu-id="bd47a-403">Hur toomanage delad Åtkomstsignatur (SAS) och lagras åtkomstprincipen</span><span class="sxs-lookup"><span data-stu-id="bd47a-403">How toomanage Shared Access Signature (SAS) and Stored Access Policy</span></span>
<span data-ttu-id="bd47a-404">Signaturer för delad åtkomst är en viktig del av hello säkerhetsmodell för alla program som använder Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="bd47a-404">Shared access signatures are an important part of hello security model for any application using Azure Storage.</span></span> <span data-ttu-id="bd47a-405">De är användbara för att tillhandahålla begränsade behörigheter tooyour storage-konto tooclients som inte ska ha hello kontonyckel.</span><span class="sxs-lookup"><span data-stu-id="bd47a-405">They are useful for providing limited permissions tooyour storage account tooclients that should not have hello account key.</span></span> <span data-ttu-id="bd47a-406">Som standard har bara hello ägare hello storage-konto kan komma åt blobbar, tabeller och köer i kontot.</span><span class="sxs-lookup"><span data-stu-id="bd47a-406">By default, only hello owner of hello storage account may access blobs, tables, and queues within that account.</span></span> <span data-ttu-id="bd47a-407">Om tjänsten eller programmet måste du toomake dessa resurser tillgängliga tooother klienter utan att dela din åtkomstnyckel, finns det tre alternativ:</span><span class="sxs-lookup"><span data-stu-id="bd47a-407">If your service or application needs toomake these resources available tooother clients without sharing your access key, you have three options:</span></span>

* <span data-ttu-id="bd47a-408">Ange en behållare behörigheter toopermit anonym läsbehörighet toohello behållaren och dess blobbar.</span><span class="sxs-lookup"><span data-stu-id="bd47a-408">Set a container's permissions toopermit anonymous read access toohello container and its blobs.</span></span> <span data-ttu-id="bd47a-409">Detta är inte tillåtet för tabeller och köer.</span><span class="sxs-lookup"><span data-stu-id="bd47a-409">This is not allowed for tables or queues.</span></span>
* <span data-ttu-id="bd47a-410">Använda en signatur för delad åtkomst som ger begränsad åtkomst rättigheter toocontainers, blobbar, köer och tabeller för ett visst tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="bd47a-410">Use a shared access signature that grants restricted access rights toocontainers, blobs, queues, and tables for a specific time interval.</span></span>
* <span data-ttu-id="bd47a-411">Använda en lagrad åtkomst princip tooobtain ytterligare en säkerhetsnivå för kontroll över signaturer för delad åtkomst för en behållare eller dess blobbar, en kö eller en tabell.</span><span class="sxs-lookup"><span data-stu-id="bd47a-411">Use a stored access policy tooobtain an additional level of control over shared access signatures for a container or its blobs, for a queue, or for a table.</span></span> <span data-ttu-id="bd47a-412">hello lagrade åtkomstprincip kan du toochange hello starttid, förfallotiden eller behörigheter för en signatur eller toorevoke det efter att det har utfärdats.</span><span class="sxs-lookup"><span data-stu-id="bd47a-412">hello stored access policy allows you toochange hello start time, expiry time, or permissions for a signature, or toorevoke it after it has been issued.</span></span>

<span data-ttu-id="bd47a-413">En signatur för delad åtkomst kan ha ett av två sätt:</span><span class="sxs-lookup"><span data-stu-id="bd47a-413">A shared access signature can be in one of two forms:</span></span>

* <span data-ttu-id="bd47a-414">**Ad hoc-SAS**: när du skapar en ad hoc-SAS, hello starttid, förfallotiden och behörigheter för hello SAS har angetts för hello SAS-URI.</span><span class="sxs-lookup"><span data-stu-id="bd47a-414">**Ad hoc SAS**: When you create an ad hoc SAS, hello start time, expiry time, and permissions for hello SAS are all specified on hello SAS URI.</span></span> <span data-ttu-id="bd47a-415">Den här typen av SAS kan skapas på en behållare, blob, tabell eller kön och det är icke-återkallningsbar.</span><span class="sxs-lookup"><span data-stu-id="bd47a-415">This type of SAS may be created on a container, blob, table, or queue and it is non-revocable.</span></span>
* <span data-ttu-id="bd47a-416">**SAS med lagrade åtkomstprincip**: en lagrade princip har definierats för resursbehållaren en blob-behållare, tabeller eller kö - och du kan använda den toomanage begränsningar för en eller flera signaturer för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="bd47a-416">**SAS with stored access policy**: A stored access policy is defined on a resource container a blob container, table, or queue - and you can use it toomanage constraints for one or more shared access signatures.</span></span> <span data-ttu-id="bd47a-417">När du kopplar en SAS med en lagrad till hello SAS ärver begränsningarna hello - hello starttid, förfallotiden samt behörigheter - har definierats för hello lagras åtkomstprincip.</span><span class="sxs-lookup"><span data-stu-id="bd47a-417">When you associate a SAS with a stored access policy, hello SAS inherits hello constraints - hello start time, expiry time, and permissions - defined for hello stored access policy.</span></span> <span data-ttu-id="bd47a-418">Den här typen av SAS är återkallningsbar.</span><span class="sxs-lookup"><span data-stu-id="bd47a-418">This type of SAS is revocable.</span></span>

<span data-ttu-id="bd47a-419">Mer information finns i [med delad åtkomst signaturer (SAS)](storage-dotnet-shared-access-signature-part-1.md) och [Hantera anonym läsbehörighet toocontainers och blobbar](storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="bd47a-419">For more information, see [Using Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md) and [Manage anonymous read access toocontainers and blobs](storage-manage-access-to-resources.md).</span></span>

<span data-ttu-id="bd47a-420">I hello nästa avsnitt får du lära dig hur toocreate en signatur åtkomst-token och lagrade princip för delad åtkomst för Azure-tabeller.</span><span class="sxs-lookup"><span data-stu-id="bd47a-420">In hello next sections, you will learn how toocreate a shared access signature token and stored access policy for Azure tables.</span></span> <span data-ttu-id="bd47a-421">Azure PowerShell tillhandahåller liknande cmdlets för behållare, blobbar och köer samt.</span><span class="sxs-lookup"><span data-stu-id="bd47a-421">Azure PowerShell provides similar cmdlets for containers, blobs, and queues as well.</span></span> <span data-ttu-id="bd47a-422">toorun hello skript i det här avsnittet hämta hello [Azure PowerShell version 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) eller senare.</span><span class="sxs-lookup"><span data-stu-id="bd47a-422">toorun hello scripts in this section, download hello [Azure PowerShell version 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) or later.</span></span>

### <a name="how-toocreate-a-policy-based-shared-access-signature-token"></a><span data-ttu-id="bd47a-423">Hur toocreate en principbaserad signatur för delad åtkomst-token</span><span class="sxs-lookup"><span data-stu-id="bd47a-423">How toocreate a policy-based Shared Access Signature token</span></span>
<span data-ttu-id="bd47a-424">Använd hello ny AzureStorageTableStoredAccessPolicy cmdlet toocreate en ny lagrade åtkomstprincip.</span><span class="sxs-lookup"><span data-stu-id="bd47a-424">Use hello New-AzureStorageTableStoredAccessPolicy cmdlet toocreate a new stored access policy.</span></span> <span data-ttu-id="bd47a-425">Anropa sedan hello [ny AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) cmdlet toocreate en ny policy-baserad delad signatur åtkomsttoken för en tabell med Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="bd47a-425">Then, call hello [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) cmdlet toocreate a new policy-based shared access signature token for an Azure Storage table.</span></span>

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx
```

### <a name="how-toocreate-an-ad-hoc-non-revocable-shared-access-signature-token"></a><span data-ttu-id="bd47a-426">Hur toocreate en ad hoc-(icke-återkallningsbar) signatur för delad åtkomst-token</span><span class="sxs-lookup"><span data-stu-id="bd47a-426">How toocreate an ad hoc (non-revocable) Shared Access Signature token</span></span>
<span data-ttu-id="bd47a-427">Använd hello [ny AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) cmdlet toocreate en ny ad hoc-(icke-återkallningsbar) signatur för delad åtkomst-token för en tabell med Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="bd47a-427">Use hello [New-AzureStorageTableSASToken](/powershell/module/azure.storage/new-azurestoragetablesastoken) cmdlet toocreate a new ad hoc (non-revocable) Shared Access Signature token for an Azure Storage table:</span></span>

```powershell
New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx
```
    
### <a name="how-toocreate-a-stored-access-policy"></a><span data-ttu-id="bd47a-428">Hur toocreate lagrade åtkomstprincipen</span><span class="sxs-lookup"><span data-stu-id="bd47a-428">How toocreate a stored access policy</span></span>
<span data-ttu-id="bd47a-429">Använd hello ny AzureStorageTableStoredAccessPolicy cmdlet toocreate en ny lagrade åtkomstprincip för en tabell med Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="bd47a-429">Use hello New-AzureStorageTableStoredAccessPolicy cmdlet toocreate a new stored access policy for an Azure Storage table:</span></span>

```powershell
$policy = "policy1"
New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
```
    
### <a name="how-tooupdate-a-stored-access-policy"></a><span data-ttu-id="bd47a-430">Hur tooupdate lagrade åtkomstprincipen</span><span class="sxs-lookup"><span data-stu-id="bd47a-430">How tooupdate a stored access policy</span></span>
<span data-ttu-id="bd47a-431">Använd hello Set-AzureStorageTableStoredAccessPolicy cmdlet tooupdate en befintlig lagrade åtkomstprincip för en tabell med Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="bd47a-431">Use hello Set-AzureStorageTableStoredAccessPolicy cmdlet tooupdate an existing stored access policy for an Azure Storage table:</span></span>

```powershell
Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx
```

### <a name="how-toodelete-a-stored-access-policy"></a><span data-ttu-id="bd47a-432">Hur toodelete lagrade åtkomstprincipen</span><span class="sxs-lookup"><span data-stu-id="bd47a-432">How toodelete a stored access policy</span></span>
<span data-ttu-id="bd47a-433">Använd hello Remove-AzureStorageTableStoredAccessPolicy cmdlet toodelete lagrade åtkomstprincipen på en tabell i Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="bd47a-433">Use hello Remove-AzureStorageTableStoredAccessPolicy cmdlet toodelete a stored access policy on an Azure Storage table:</span></span>

```powershell
Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx
```

## <a name="how-toouse-azure-storage-for-us-government-and-azure-china"></a><span data-ttu-id="bd47a-434">Hur toouse Azure Storage för amerikanska myndigheter och Azure Kina</span><span class="sxs-lookup"><span data-stu-id="bd47a-434">How toouse Azure Storage for U.S. government and Azure China</span></span>
<span data-ttu-id="bd47a-435">En Azure-miljö är en oberoende distribution av Microsoft Azure som [Azure Government för USA: s regering](https://azure.microsoft.com/features/gov/), [AzureCloud för globala Azure](https://portal.azure.com), och [AzureChinaCloud för Azure som drivs av 21Vianet i Kina](http://www.windowsazure.cn/).</span><span class="sxs-lookup"><span data-stu-id="bd47a-435">An Azure environment is an independent deployment of Microsoft Azure, such as [Azure Government for U.S. government](https://azure.microsoft.com/features/gov/), [AzureCloud for global Azure](https://portal.azure.com), and [AzureChinaCloud for Azure operated by 21Vianet in China](http://www.windowsazure.cn/).</span></span> <span data-ttu-id="bd47a-436">Du kan distribuera nya miljöer i Azure för amerikanska myndigheter och Azure Kina.</span><span class="sxs-lookup"><span data-stu-id="bd47a-436">You can deploy new Azure environments for U.S government and Azure China.</span></span>

<span data-ttu-id="bd47a-437">toouse Azure Storage med AzureChinaCloud måste toocreate en kontext för lagring som är associerad med AzureChinaCloud.</span><span class="sxs-lookup"><span data-stu-id="bd47a-437">toouse Azure Storage with AzureChinaCloud, you need toocreate a storage context that is associated with AzureChinaCloud.</span></span> <span data-ttu-id="bd47a-438">Följ dessa steg tooget som du startade:</span><span class="sxs-lookup"><span data-stu-id="bd47a-438">Follow these steps tooget you started:</span></span>

1. <span data-ttu-id="bd47a-439">Kör hello [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) cmdlet toosee hello tillgängliga Azure miljöer:</span><span class="sxs-lookup"><span data-stu-id="bd47a-439">Run hello [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) cmdlet toosee hello available Azure environments:</span></span>
   
    ```powershell
    Get-AzureEnvironment
    ```

2. <span data-ttu-id="bd47a-440">Lägg till en Azure Kina konto tooWindows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="bd47a-440">Add an Azure China account tooWindows PowerShell:</span></span>
   
    ```powershell
    Add-AzureAccount –Environment AzureChinaCloud
    ```

3. <span data-ttu-id="bd47a-441">Skapa en kontext för lagring för ett AzureChinaCloud konto:</span><span class="sxs-lookup"><span data-stu-id="bd47a-441">Create a storage context for an AzureChinaCloud account:</span></span>
   
    ```powershell
    $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud
    ```

<span data-ttu-id="bd47a-442">toouse Azure Storage med [USA Azure Government](https://azure.microsoft.com/features/gov/), bör du definiera en ny miljö och sedan skapa en ny lagring kontext med den här miljön:</span><span class="sxs-lookup"><span data-stu-id="bd47a-442">toouse Azure Storage with [U.S. Azure Government](https://azure.microsoft.com/features/gov/), you should define a new environment and then create a new storage context with this environment:</span></span>

1. <span data-ttu-id="bd47a-443">Kör hello [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) cmdlet toosee hello tillgängliga Azure miljöer:</span><span class="sxs-lookup"><span data-stu-id="bd47a-443">Run hello [Get-AzureEnvironment](/powershell/module/azure/get-azureenvironment?view=azuresmps-3.7.0) cmdlet toosee hello available Azure environments:</span></span>

    ```powershell
    Get-AzureEnvironment
    ```

2. <span data-ttu-id="bd47a-444">Lägg till en Azure som tillhör amerikanska myndigheter konto tooWindows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="bd47a-444">Add an Azure US Government account tooWindows PowerShell:</span></span>
   
    ```powershell
    Add-AzureAccount –Environment AzureUSGovernment
    ```

3. <span data-ttu-id="bd47a-445">Skapa en kontext för lagring för ett AzureUSGovernment konto:</span><span class="sxs-lookup"><span data-stu-id="bd47a-445">Create a storage context for an AzureUSGovernment account:</span></span>
   
    ```powershell
    $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureUSGovernment
    ```
     
<span data-ttu-id="bd47a-446">Mer information finns i:</span><span class="sxs-lookup"><span data-stu-id="bd47a-446">For more information, see:</span></span>

* <span data-ttu-id="bd47a-447">[Utvecklarhandbok för Microsoft Azure Government](../azure-government-developer-guide.md).</span><span class="sxs-lookup"><span data-stu-id="bd47a-447">[Microsoft Azure Government Developer Guide](../azure-government-developer-guide.md).</span></span>
* [<span data-ttu-id="bd47a-448">Översikt över skillnaderna när du skapar ett program på Kina Service</span><span class="sxs-lookup"><span data-stu-id="bd47a-448">Overview of Differences When Creating an Application on China Service</span></span>](https://msdn.microsoft.com/library/azure/dn578439.aspx)

## <a name="next-steps"></a><span data-ttu-id="bd47a-449">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bd47a-449">Next Steps</span></span>
<span data-ttu-id="bd47a-450">I den här guiden har du lärt dig hur toomanage Azure Storage med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bd47a-450">In this guide, you've learned how toomanage Azure Storage with Azure PowerShell.</span></span> <span data-ttu-id="bd47a-451">Här följer några relaterade artiklar och resurser för att lära dig mer om dem.</span><span class="sxs-lookup"><span data-stu-id="bd47a-451">Here are some related articles and resources for learning more about them.</span></span>

* [<span data-ttu-id="bd47a-452">Azure Storage-dokumentation</span><span class="sxs-lookup"><span data-stu-id="bd47a-452">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)
* [<span data-ttu-id="bd47a-453">PowerShell-Cmdlets för Azure Storage</span><span class="sxs-lookup"><span data-stu-id="bd47a-453">Azure Storage PowerShell Cmdlets</span></span>](/powershell/module/azurerm.storage/#storage)
* [<span data-ttu-id="bd47a-454">Windows PowerShell-referens</span><span class="sxs-lookup"><span data-stu-id="bd47a-454">Windows PowerShell Reference</span></span>](https://msdn.microsoft.com/library/ms714469.aspx)

[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How toomanage storage accounts in Azure]: #manageaccount
[How tooset a default Azure subscription]: #setdefsub
[How toocreate a new Azure storage account]: #createaccount
[How tooset a default Azure storage account]: #defaultaccount
[How toolist all Azure storage accounts in a subscription]: #listaccounts
[How toocreate an Azure storage context]: #createctx
[How toomanage Azure blobs and blob snapshots]: #manageblobs
[How toocreate a container]: #container
[How tooupload a blob into a container]: #uploadblob
[How toodownload blobs from a container]: #downblob
[How toocopy blobs from one storage container tooanother]: #copyblob
[How toodelete a blob]: #deleteblob
[How toomanage Azure blob snapshots]: #manageshots
[How toocreate a blob snapshot]: #createshot
[How toolist snapshots of a blob]: #listshot
[How toocopy a snapshot of a blob]: #copyshot
[How toomanage Azure tables and table entities]: #managetables
[How toocreate a table]: #createtable
[How tooretrieve a table]: #gettable
[How toodelete a table]: #remtable
[How toomanage table entities]: #mngentity
[How tooadd table entities]: #addentity
[How tooquery table entities]: #queryentity
[How toodelete table entities]: #deleteentity
[How toomanage Azure queues and queue messages]: #managequeues
[How toocreate a queue]: #createqueue
[How tooretrieve a queue]: #getqueue
[How toodelete a queue]: #remqueue
[How toomanage queue messages]: #mngqueuemsg
[How tooinsert a message into a queue]: #addqueuemsg
[How toode-queue at hello next message]: #dequeuemsg
[How toomanage Azure file shares and files]: #files
[How tooset and query storage analytics]: #stganalytics
[How toomanage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How toouse Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next
