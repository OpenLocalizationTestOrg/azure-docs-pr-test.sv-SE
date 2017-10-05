---
title: "Kom igång med Lagringsutforskaren (förhandsversion) | Microsoft Docs"
description: "Hantera Azure-lagringsresurser med Lagringsutforskaren (förhandsversion)"
services: storage
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1ed0f096-494d-49c4-ab71-f4164ee19ec8
ms.service: storage
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/17/2017
ms.author: kraigb
ms.openlocfilehash: 1794a86a4185d587cf184a1f61a5720e2ab65e92
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-storage-explorer-preview"></a><span data-ttu-id="83473-103">Kom igång med Lagringsutforskaren (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="83473-103">Get started with Storage Explorer (Preview)</span></span>
## <a name="overview"></a><span data-ttu-id="83473-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="83473-104">Overview</span></span>
<span data-ttu-id="83473-105">Azure Lagringsutforskaren (förhandsversion) är en fristående app som gör det enkelt att arbeta med Azure Storage-data i Windows, Mac OS och Linux.</span><span class="sxs-lookup"><span data-stu-id="83473-105">Azure Storage Explorer (Preview) is a standalone app that enables you to easily work with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="83473-106">I den här artikeln lär du dig hur du kan ansluta till och hantera dina Azure Storage-konton på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="83473-106">In this article, you learn the various ways of connecting to and managing your Azure storage accounts.</span></span>

![Microsoft Azure Lagringsutforskaren (förhandsversion)][15]

## <a name="prerequisites"></a><span data-ttu-id="83473-108">Krav</span><span class="sxs-lookup"><span data-stu-id="83473-108">Prerequisites</span></span>
* [<span data-ttu-id="83473-109">Hämta och installera Lagringsutforskaren (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="83473-109">Download and install Storage Explorer (Preview)</span></span>](http://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a><span data-ttu-id="83473-110">Ansluta till ett lagringskonto eller en tjänst</span><span class="sxs-lookup"><span data-stu-id="83473-110">Connect to a storage account or service</span></span>
<span data-ttu-id="83473-111">Med Lagringsutforskaren (förhandsversion) kan du ansluta till lagringskonton på flera olika sätt.</span><span class="sxs-lookup"><span data-stu-id="83473-111">Storage Explorer (Preview) provides several ways to connect to storage accounts.</span></span> <span data-ttu-id="83473-112">Du kan till exempel:</span><span class="sxs-lookup"><span data-stu-id="83473-112">For example, you can:</span></span>
* <span data-ttu-id="83473-113">Ansluta till lagringskonton som är associerade med dina Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="83473-113">Connect to storage accounts associated with your Azure subscriptions.</span></span>
* <span data-ttu-id="83473-114">Ansluta till lagringskonton och tjänster som delas från andra Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="83473-114">Connect to storage accounts and services that are shared from other Azure subscriptions.</span></span>
* <span data-ttu-id="83473-115">Ansluta till och hantera lokal lagring med hjälp av Azure Storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="83473-115">Connect to and manage local storage by using the Azure Storage Emulator.</span></span> 

<span data-ttu-id="83473-116">Dessutom kan du arbeta med lagringskonton i globala och nationella Azure:</span><span class="sxs-lookup"><span data-stu-id="83473-116">In addition, you can work with storage accounts in global and national Azure:</span></span>

* <span data-ttu-id="83473-117">[Ansluta till en Azure-prenumeration](#connect-to-an-azure-subscription): hantera lagringsresurser som hör till din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="83473-117">[Connect to an Azure subscription](#connect-to-an-azure-subscription): Manage storage resources that belong to your Azure subscription.</span></span>
* <span data-ttu-id="83473-118">[Arbeta med lokal utvecklingslagring](#work-with-local-development-storage): hantera lokal lagring med hjälp av Azure Storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="83473-118">[Work with local development storage](#work-with-local-development-storage): Manage local storage by using the Azure Storage Emulator.</span></span>
* <span data-ttu-id="83473-119">[Ansluta till extern lagring](#attach-or-detach-an-external-storage-account): hantera lagringsresurser som hör till en annan Azure-prenumeration eller nationella Azure-moln med hjälp av lagringskontots namn, nyckel och slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="83473-119">[Attach to external storage](#attach-or-detach-an-external-storage-account): Manage storage resources that belong to another Azure subscription or that are under national Azure clouds by using the storage account's name, key, and endpoints.</span></span>
* <span data-ttu-id="83473-120">[Ansluta ett lagringskonto med hjälp av en SAS](#attach-storage-account-using-sas): hantera lagringsresurser som tillhör en annan Azure-prenumeration med hjälp av en signatur för delad åtkomst (SAS).</span><span class="sxs-lookup"><span data-stu-id="83473-120">[Attach a storage account by using an SAS](#attach-storage-account-using-sas): Manage storage resources that belong to another Azure subscription by using a shared access signature (SAS).</span></span>
* <span data-ttu-id="83473-121">[Ansluta en tjänst med hjälp av SAS](#attach-service-using-sas): hantera en specifik lagringstjänst (blobbehållare, kö eller tabell) som hör till en annan Azure-prenumeration med hjälp av en SAS.</span><span class="sxs-lookup"><span data-stu-id="83473-121">[Attach a service by using an SAS](#attach-service-using-sas): Manage a specific storage service (blob container, queue, or table) that belongs to another Azure subscription by using an SAS.</span></span>

## <a name="connect-to-an-azure-subscription"></a><span data-ttu-id="83473-122">Ansluta till en Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="83473-122">Connect to an Azure subscription</span></span>
> [!NOTE]
> <span data-ttu-id="83473-123">Om du inte redan har ett Azure-konto kan du [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) eller [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="83473-123">If you don't have an Azure account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>
>
>

1. <span data-ttu-id="83473-124">I Lagringsutforskaren (förhandsversion) väljer du **Inställningar för Azure-konto**.</span><span class="sxs-lookup"><span data-stu-id="83473-124">In Storage Explorer (Preview), select **Azure Account settings**.</span></span>

    ![Inställningar för Azure-konto][0]

2. <span data-ttu-id="83473-126">Den vänstra rutan visar alla Microsoft-konton som du har loggat in på.</span><span class="sxs-lookup"><span data-stu-id="83473-126">The left pane displays all the Microsoft accounts you've signed in to.</span></span> <span data-ttu-id="83473-127">Om du vill ansluta till ett annat konto väljer du **Lägg till ett konto** och följer instruktionerna för att logga in med ett Microsoft-konto som är kopplat till minst en aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="83473-127">To connect to another account, select **Add an account**, and then follow the instructions to sign in with a Microsoft account that is associated with at least one active Azure subscription.</span></span>

    >[!NOTE]
    ><span data-ttu-id="83473-128">För närvarande går det inte att ansluta till nationella Azure (t.ex. Azure Germany, Azure Government och Azure China) via inloggning.</span><span class="sxs-lookup"><span data-stu-id="83473-128">Connecting to national Azure (such as Azure Germany, Azure Government, and Azure China via sign-in) is currently not supported.</span></span> <span data-ttu-id="83473-129">Mer information om hur du ansluter till nationella Azure Storage-konton finns i avsnittet Ansluta eller koppla från ett externt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="83473-129">See the "Attach or detach an external storage account" section for how to connect to national Azure storage accounts.</span></span>

3. <span data-ttu-id="83473-130">När du har loggat in med ett Microsoft-konto fylls den vänstra rutan i med de Azure-prenumerationer som är kopplade till kontot.</span><span class="sxs-lookup"><span data-stu-id="83473-130">After you successfully sign in with a Microsoft account, the left pane is populated with the Azure subscriptions associated with that account.</span></span> <span data-ttu-id="83473-131">Välj de Azure-prenumerationer som du vill arbeta med och välj sedan **Använd**.</span><span class="sxs-lookup"><span data-stu-id="83473-131">Select the Azure subscriptions that you want to work with, and then select **Apply**.</span></span> <span data-ttu-id="83473-132">(Om du klickar på **Alla prenumerationer** växlar du mellan att välja alla eller inga av de Azure-prenumerationer som visas.)</span><span class="sxs-lookup"><span data-stu-id="83473-132">(Selecting **All subscriptions** toggles selecting all or none of the listed Azure subscriptions.)</span></span>

    ![Välja Azure-prenumerationer][3]  
    <span data-ttu-id="83473-134">I den vänstra rutan visas lagringskontona som är kopplade till de valda Azure-prenumerationerna.</span><span class="sxs-lookup"><span data-stu-id="83473-134">The left pane displays the storage accounts associated with the selected Azure subscriptions.</span></span>

    ![Valda Azure-prenumerationer][4]

## <a name="connect-to-an-azure-stack-subscription"></a><span data-ttu-id="83473-136">Ansluta till en Azure Stack-prenumeration</span><span class="sxs-lookup"><span data-stu-id="83473-136">Connect to an Azure Stack subscription</span></span>

<span data-ttu-id="83473-137">Information om hur du ansluter till en Azure Stackprenumeration finns i [Ansluta Storage Explorer till en prenumeration på Azure Stack](azure-stack/azure-stack-storage-connect-se.md).</span><span class="sxs-lookup"><span data-stu-id="83473-137">For information about connecting to an Azure Stack subscription, see [Connect Storage Explorer to an Azure Stack subscription](azure-stack/azure-stack-storage-connect-se.md).</span></span>

## <a name="work-with-local-development-storage"></a><span data-ttu-id="83473-138">Arbeta med lokal utvecklingslagring</span><span class="sxs-lookup"><span data-stu-id="83473-138">Work with local development storage</span></span>
<span data-ttu-id="83473-139">Med Lagringsutforskaren (förhandsversion) kan du arbeta mot lokal lagring med hjälp av Azure Storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="83473-139">With Storage Explorer (Preview), you can work against local storage by using the Azure Storage Emulator.</span></span> <span data-ttu-id="83473-140">På så sätt kan du skriva kod mot och testa lagring utan att nödvändigtvis ha distribuerat ett lagringskonto i Azure (eftersom lagringskontot emuleras av Azure Storage-emulatorn).</span><span class="sxs-lookup"><span data-stu-id="83473-140">This approach lets you write code against and test storage without necessarily having a storage account deployed on Azure, because the storage account is being emulated by the Azure Storage Emulator.</span></span>

> [!NOTE]
> <span data-ttu-id="83473-141">Azure Storage-emulatorn stöds för närvarande endast för Windows.</span><span class="sxs-lookup"><span data-stu-id="83473-141">The Azure Storage Emulator is currently supported only for Windows.</span></span>
>
>

1. <span data-ttu-id="83473-142">I den vänstra rutan i Lagringsutforskaren (förhandsversion) utökar du noden **(Lokala och anslutna)** > **Lagringskonton** > **(Utveckling)**.</span><span class="sxs-lookup"><span data-stu-id="83473-142">In the left pane of Storage Explorer (Preview), expand the **(Local and Attached)** > **Storage Accounts** > **(Development)** node.</span></span>

    ![Noden Lokal utveckling][21]

2. <span data-ttu-id="83473-144">Om du inte har installerat Azure Storage-emulatorn än uppmanas du att göra det via ett informationsfält.</span><span class="sxs-lookup"><span data-stu-id="83473-144">If you have not yet installed the Azure Storage Emulator, you are prompted to do so via an infobar.</span></span> <span data-ttu-id="83473-145">Om informationsfältet visas väljer du **Hämta den senaste versionen** och installerar sedan emulatorn.</span><span class="sxs-lookup"><span data-stu-id="83473-145">If the infobar is displayed, select **Download the latest version**, and then install the emulator.</span></span>

    ![Fråga om att hämta Azure Storage-emulatorn][22]

3. <span data-ttu-id="83473-147">När emulatorn har installerats kan du skapa och arbeta med lokala blobbar, köer och tabeller.</span><span class="sxs-lookup"><span data-stu-id="83473-147">After the emulator is installed, you can create and work with local blobs, queues, and tables.</span></span> <span data-ttu-id="83473-148">Mer information om hur du arbetar med varje typ av lagringskonto finns i följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="83473-148">To learn how to work with each storage account type, see one of the following:</span></span>

    * [<span data-ttu-id="83473-149">Hantera Azure-bloblagringsresurser</span><span class="sxs-lookup"><span data-stu-id="83473-149">Manage Azure blob storage resources</span></span>](vs-azure-tools-storage-explorer-blobs.md)
    * <span data-ttu-id="83473-150">Hantera Azure-filresurslagringsresurser: *kommer snart*</span><span class="sxs-lookup"><span data-stu-id="83473-150">Manage Azure file share storage resources: *Coming soon*</span></span>
    * <span data-ttu-id="83473-151">Hantera Azure-kölagringsresurser: *kommer snart*</span><span class="sxs-lookup"><span data-stu-id="83473-151">Manage Azure queue storage resources: *Coming soon*</span></span>
    * <span data-ttu-id="83473-152">Hantera Azure-tabellagringsresurser: *kommer snart*</span><span class="sxs-lookup"><span data-stu-id="83473-152">Manage Azure table storage resources: *Coming soon*</span></span>

## <a name="attach-or-detach-an-external-storage-account"></a><span data-ttu-id="83473-153">Ansluta eller koppla från ett externt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="83473-153">Attach or detach an external storage account</span></span>
<span data-ttu-id="83473-154">Med Lagringsutforskaren (förhandsversion) kan du ansluta till externa lagringskonton så att du enkelt kan dela lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="83473-154">With Storage Explorer (Preview), you can attach to external storage accounts so that storage accounts can be easily shared.</span></span> <span data-ttu-id="83473-155">Det här avsnittet beskriver hur du ansluter till (och kopplar från) externa lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="83473-155">This section explains how to attach to (and detach from) external storage accounts.</span></span>

### <a name="get-the-storage-account-credentials"></a><span data-ttu-id="83473-156">Hämta autentiseringsuppgifterna för lagringskontot</span><span class="sxs-lookup"><span data-stu-id="83473-156">Get the storage account credentials</span></span>
<span data-ttu-id="83473-157">För att kunna dela ett externt lagringskonto måste kontoägaren först hämta autentiseringsuppgifterna (kontonamnet och nyckeln) för kontot och sedan dela den informationen med den person som ska ansluta till det (externa) kontot.</span><span class="sxs-lookup"><span data-stu-id="83473-157">To share an external storage account, the owner of that account must first get the credentials (account name and key) for the account and then share that information with the person who wants to attach to that (external) account.</span></span> <span data-ttu-id="83473-158">Så här hämtar du autentiseringsuppgifterna för ett lagringskonto via Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="83473-158">You can obtain the storage account credentials via the Azure portal by doing the following:</span></span>

1. <span data-ttu-id="83473-159">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="83473-159">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="83473-160">Välj **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="83473-160">Select **Browse**.</span></span>

3. <span data-ttu-id="83473-161">Välj **Lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="83473-161">Select **Storage Accounts**.</span></span>

4. <span data-ttu-id="83473-162">Välj önskat lagringskonto på bladet **Lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="83473-162">On the **Storage Accounts** blade, select the desired storage account.</span></span>

5. <span data-ttu-id="83473-163">Välj **Åtkomstnycklar** på bladet **Inställningar** för det valda lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="83473-163">On the **Settings** blade for the selected storage account, select **Access keys**.</span></span>

    ![Alternativ för åtkomstnycklar][5]

6. <span data-ttu-id="83473-165">På bladet **Åtkomstnycklar** kopierar du värdena för **lagringskontonnamn** och **nyckel1** som används för att ansluta till lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="83473-165">On the **Access keys** blade, copy the **Storage account name** and **key1** values for use when attaching to the storage account.</span></span>

    ![Åtkomstnycklar][6]

### <a name="attach-to-an-external-storage-account"></a><span data-ttu-id="83473-167">Ansluta till ett externt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="83473-167">Attach to an external storage account</span></span>
<span data-ttu-id="83473-168">Om du vill ansluta till ett externt lagringskonto behöver du ha tillgång till kontots namn och nyckel.</span><span class="sxs-lookup"><span data-stu-id="83473-168">To attach to an external storage account, you need the account's name and key.</span></span> <span data-ttu-id="83473-169">I avsnittet Hämta autentiseringsuppgifter för lagringskonto beskrivs hur du hämtar dessa värden från Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="83473-169">The "Get the storage account credentials" section explains how to obtain these values from the Azure portal.</span></span> <span data-ttu-id="83473-170">Men i portalen kallas kontonyckeln **nyckel1**.</span><span class="sxs-lookup"><span data-stu-id="83473-170">However, in the portal, the account key is called **key1**.</span></span> <span data-ttu-id="83473-171">Om Lagringsutforskaren (förhandsgranskning) begär en kontonyckel anger du alltså **nyckel1**-värdet.</span><span class="sxs-lookup"><span data-stu-id="83473-171">So where Storage Explorer (Preview) asks for an account key, you enter the **key1** value.</span></span>

1. <span data-ttu-id="83473-172">I Lagringsutforskaren (förhandsversion) väljer du **Anslut till Azure Storage**.</span><span class="sxs-lookup"><span data-stu-id="83473-172">In Storage Explorer (Preview), select **Connect to Azure storage**.</span></span>

    ![Alternativet Anslut till Azure Storage][23]

2. <span data-ttu-id="83473-174">I dialogrutan **Anslut till Azure Storage** anger du kontonyckeln (**nyckel 1**-värdet från Azure Portal) och väljer sedan **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="83473-174">In the **Connect to Azure Storage** dialog box, specify the account key (the **key1** value from the Azure portal), and then select **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="83473-175">Du kan ange en anslutningssträng för lagring från ett lagringskonto på nationella Azure.</span><span class="sxs-lookup"><span data-stu-id="83473-175">You can enter the storage connection string from a storage account on national Azure.</span></span> <span data-ttu-id="83473-176">Om du till exempel vill ansluta till Azure Germany-lagringskonton anger du anslutningssträngar som ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="83473-176">For example, to connect to Azure Germany storage accounts, enter connection strings similar to the following:</span></span> 
    >
    >* <span data-ttu-id="83473-177">DefaultEndpointsProtocol=https</span><span class="sxs-lookup"><span data-stu-id="83473-177">DefaultEndpointsProtocol=https</span></span>
    >* <span data-ttu-id="83473-178">AccountName=cawatest03</span><span class="sxs-lookup"><span data-stu-id="83473-178">AccountName=cawatest03</span></span>
    >* <span data-ttu-id="83473-179">AccountKey=<storage_account_key></span><span class="sxs-lookup"><span data-stu-id="83473-179">AccountKey=<storage_account_key></span></span>
    >* <span data-ttu-id="83473-180">EndpointSuffix=core.cloudapi.de</span><span class="sxs-lookup"><span data-stu-id="83473-180">EndpointSuffix=core.cloudapi.de</span></span>
    
    ><span data-ttu-id="83473-181">Du kan hämta anslutningssträngen på Azure Portal på samma sätt som beskrivs i avsnittet Hämta autentiseringsuppgifterna för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="83473-181">You can get the connection string from the Azure portal in the same way as described in the "Get the storage account credentials" section.</span></span>

    ![Dialogrutan Anslut till Azure Storage][24]

3. <span data-ttu-id="83473-183">I dialogrutan **Anslut extern lagring** anger du lagringskontots namn i rutan **Kontonamn**, anger eventuella andra önskade inställningar och väljer sedan **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="83473-183">In the **Attach External Storage** dialog box, in the **Account name** box, enter the storage account name, specify any other desired settings, and then select **Next**.</span></span>

    ![Dialogrutan Anslut extern lagring][8]

4. <span data-ttu-id="83473-185">Kontrollera uppgifterna i dialogrutan **Anslutningssammanfattning**.</span><span class="sxs-lookup"><span data-stu-id="83473-185">In the **Connection Summary** dialog box, verify the information.</span></span> <span data-ttu-id="83473-186">Om du vill ändra något väljer du **Tillbaka** och anger sedan önskade inställningar på nytt.</span><span class="sxs-lookup"><span data-stu-id="83473-186">If you want to change anything, select **Back** and reenter the desired settings.</span></span> 

5. <span data-ttu-id="83473-187">Välj **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="83473-187">Select **Connect**.</span></span>

6. <span data-ttu-id="83473-188">När det externa lagringskontot har anslutits visas det med texten **(Externt)** sist i namnet.</span><span class="sxs-lookup"><span data-stu-id="83473-188">After it is successfully connected, the external storage account is displayed with **(External)** appended to the storage account name.</span></span>

    ![Resultatet av att ansluta till ett externt lagringskonto][9]

### <a name="detach-from-an-external-storage-account"></a><span data-ttu-id="83473-190">Ansluta från ett externt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="83473-190">Detach from an external storage account</span></span>
1. <span data-ttu-id="83473-191">Högerklicka på det externa lagringskonto som du vill koppla från och välj sedan **Koppla från**.</span><span class="sxs-lookup"><span data-stu-id="83473-191">Right-click the external storage account that you want to detach, and then select **Detach**.</span></span>

    ![Koppla bort lagringsalternativ][10]

2. <span data-ttu-id="83473-193">I bekräftelsemeddelandet väljer du **Ja** för att bekräfta att du vill koppla bort det externa lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="83473-193">In the confirmation message, select **Yes** to confirm the detachment from the external storage account.</span></span>

## <a name="attach-a-storage-account-by-using-an-sas"></a><span data-ttu-id="83473-194">Ansluta ett lagringskonto med hjälp av en SAS</span><span class="sxs-lookup"><span data-stu-id="83473-194">Attach a storage account by using an SAS</span></span>
<span data-ttu-id="83473-195">Med en [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) kan administratören för en Azure-prenumeration bevilja tillfällig åtkomst till ett lagringskonto utan att uppge autentiseringsuppgifter för Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="83473-195">An [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) lets the admin of an Azure subscription grant temporary access to a storage account without having to provide Azure subscription credentials.</span></span>

<span data-ttu-id="83473-196">För att illustrera detta antar vi att Användare A är administratör för en Azure-prenumeration och vill ge Användare B åtkomst till ett lagringskonto under en begränsad tid med särskilda behörigheter:</span><span class="sxs-lookup"><span data-stu-id="83473-196">To illustrate this scenario, let's say that UserA is an admin of an Azure subscription, and UserA wants to allow UserB to access a storage account for a limited time with certain permissions:</span></span>

1. <span data-ttu-id="83473-197">Användare A genererar en SAS (som består av anslutningssträngen för lagringskontot) för en viss tidsperiod och med önskade behörigheter.</span><span class="sxs-lookup"><span data-stu-id="83473-197">UserA generates an SAS (consisting of the connection string for the storage account) for a specific time period and with the desired permissions.</span></span>

2. <span data-ttu-id="83473-198">Användare A delar SAS med den person (Användare B i vårt exempel) som vill ha åtkomst till lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="83473-198">UserA shares the SAS with the person (UserB, in our example) who wants access to the storage account.</span></span>  

3. <span data-ttu-id="83473-199">Användare B använder Lagringsutforskaren (förhandsversion) för att ansluta till kontot som hör till Användare A med hjälp av den SAS som han eller hon fått.</span><span class="sxs-lookup"><span data-stu-id="83473-199">UserB uses Storage Explorer (Preview) to attach to the account that belongs to UserA by using the supplied SAS.</span></span>

### <a name="get-an-sas-for-the-account-you-want-to-share"></a><span data-ttu-id="83473-200">Hämta en SAS för det konto som du vill dela</span><span class="sxs-lookup"><span data-stu-id="83473-200">Get an SAS for the account you want to share</span></span>
1. <span data-ttu-id="83473-201">I Lagringshanteraren (förhandsversion) högerklickar du på det lagringskonto som du vill dela och väljer sedan **Hämta signatur för delad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="83473-201">In Storage Explorer (Preview), right-click the storage account you want share, and then select **Get Shared Access Signature**.</span></span>

    ![Snabbmenyalternativet Hämta SAS][13]

2. <span data-ttu-id="83473-203">I dialogrutan **Signatur för delad åtkomst** anger du det tidsintervall och de behörigheter som du vill använda för kontot och väljer sedan **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="83473-203">In the **Shared Access Signature** dialog box, specify the time frame and permissions that you want for the account, and then select **Create**.</span></span>

    <span data-ttu-id="83473-204">Dialogrutan ![Hämta SAS][14]</span><span class="sxs-lookup"><span data-stu-id="83473-204">![Get SAS dialog box][14]</span></span>  
    <span data-ttu-id="83473-205">SAS visas i dialogrutan **Signatur för delad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="83473-205">The **Shared Access Signature** dialog box opens and displays the SAS.</span></span>

3. <span data-ttu-id="83473-206">Välj **Kopiera** bredvid **anslutningssträngen** för att kopiera den till Urklipp och välj sedan **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="83473-206">Next to the **Connection String**, select **Copy** to copy it to the clipboard, and then select **Close**.</span></span>

### <a name="attach-to-the-shared-account-by-using-the-sas"></a><span data-ttu-id="83473-207">Ansluta till det delade kontot med hjälp av SAS</span><span class="sxs-lookup"><span data-stu-id="83473-207">Attach to the shared account by using the SAS</span></span>
1. <span data-ttu-id="83473-208">I Lagringsutforskaren (förhandsversion) väljer du **Anslut till Azure Storage**.</span><span class="sxs-lookup"><span data-stu-id="83473-208">In Storage Explorer (Preview), select **Connect to Azure storage**.</span></span>

    ![Alternativet Anslut till Azure Storage][23]

2. <span data-ttu-id="83473-210">I dialogrutan **Anslut till Azure Storage** anger du anslutningssträngen och väljer sedan **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="83473-210">In the **Connect to Azure Storage** dialog box, specify the connection string, and then select **Next**.</span></span>

    ![Dialogrutan Anslut till Azure Storage][24]

3. <span data-ttu-id="83473-212">Kontrollera uppgifterna i dialogrutan **Anslutningssammanfattning**.</span><span class="sxs-lookup"><span data-stu-id="83473-212">In the **Connection Summary** dialog box, verify the information.</span></span> <span data-ttu-id="83473-213">Om du vill göra ändringar väljer du **Bakåt** och anger sedan önskade inställningar.</span><span class="sxs-lookup"><span data-stu-id="83473-213">To make changes, select **Back**, and then enter the settings you want.</span></span> 

4. <span data-ttu-id="83473-214">Välj **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="83473-214">Select **Connect**.</span></span>

5. <span data-ttu-id="83473-215">När lagringskontot har anslutits visas det med texten **(SAS)** sist i kontonamnet som du angav.</span><span class="sxs-lookup"><span data-stu-id="83473-215">After it is attached, the storage account is displayed with **(SAS)** appended to the account name that you supplied.</span></span>

    ![Resultatet av anslutning till ett konto med SAS][17]

## <a name="attach-a-service-by-using-an-sas"></a><span data-ttu-id="83473-217">Ansluta en tjänst med en SAS</span><span class="sxs-lookup"><span data-stu-id="83473-217">Attach a service by using an SAS</span></span>
<span data-ttu-id="83473-218">I avsnittet Ansluta ett lagringskonto med hjälp av SAS beskrivs hur en administratör för en Azure-prenumeration kan bevilja tillfällig åtkomst till ett lagringskonto genom att generera och dela en SAS för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="83473-218">The "Attach a storage account by using an SAS" section explains how an Azure subscription admin can grant temporary access to a storage account by generating and sharing an SAS for the storage account.</span></span> <span data-ttu-id="83473-219">På samma sätt kan en SAS genereras för en specifik tjänst (blobbehållare, kö eller tabell) i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="83473-219">Similarly, an SAS can be generated for a specific service (blob container, queue, or table) within a storage account.</span></span>  

### <a name="generate-an-sas-for-the-service-that-you-want-to-share"></a><span data-ttu-id="83473-220">Generera en SAS för den tjänst som du vill dela</span><span class="sxs-lookup"><span data-stu-id="83473-220">Generate an SAS for the service that you want to share</span></span>
<span data-ttu-id="83473-221">I det här scenariot kan en tjänst vara en blobbehållare, en kö eller en tabell.</span><span class="sxs-lookup"><span data-stu-id="83473-221">In this context, a service can be a blob container, queue, or table.</span></span> <span data-ttu-id="83473-222">Om du vill generera SAS för en tjänst kan du läsa:</span><span class="sxs-lookup"><span data-stu-id="83473-222">To generate the SAS for a listed service, see:</span></span>

* [<span data-ttu-id="83473-223">Hämta SAS för en blobbehållare</span><span class="sxs-lookup"><span data-stu-id="83473-223">Get the SAS for a blob container</span></span>](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
* <span data-ttu-id="83473-224">Hämta SAS för en filresurs: *kommer snart*</span><span class="sxs-lookup"><span data-stu-id="83473-224">Get the SAS for a file share: *Coming soon*</span></span>
* <span data-ttu-id="83473-225">Hämta SAS för en kö: *kommer snart*</span><span class="sxs-lookup"><span data-stu-id="83473-225">Get the SAS for a queue: *Coming soon*</span></span>
* <span data-ttu-id="83473-226">Hämta SAS för en tabell: *kommer snart*</span><span class="sxs-lookup"><span data-stu-id="83473-226">Get the SAS for a table: *Coming soon*</span></span>

### <a name="attach-to-the-shared-account-service-by-using-the-sas"></a><span data-ttu-id="83473-227">Ansluta till tjänsten för det delade kontot med hjälp av SAS</span><span class="sxs-lookup"><span data-stu-id="83473-227">Attach to the shared account service by using the SAS</span></span>
1. <span data-ttu-id="83473-228">I Lagringsutforskaren (förhandsversion) väljer du **Anslut till Azure Storage**.</span><span class="sxs-lookup"><span data-stu-id="83473-228">In Storage Explorer (Preview), select **Connect to Azure storage**.</span></span>

    ![Alternativet Anslut till Azure Storage][23]

2. <span data-ttu-id="83473-230">I dialogrutan **Anslut till Azure Storage** anger du SAS-URI och väljer sedan **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="83473-230">In the **Connect to Azure Storage** dialog box, specify the SAS URI, and then select **Next**.</span></span>

    ![Dialogrutan Anslut till Azure Storage][24]

3. <span data-ttu-id="83473-232">Kontrollera uppgifterna i dialogrutan **Anslutningssammanfattning**.</span><span class="sxs-lookup"><span data-stu-id="83473-232">In the **Connection Summary** dialog box, verify the information.</span></span> <span data-ttu-id="83473-233">Om du vill göra ändringar väljer du **Bakåt** och anger sedan önskade inställningar.</span><span class="sxs-lookup"><span data-stu-id="83473-233">To make changes, select **Back**, and then enter the settings you want.</span></span> 

4. <span data-ttu-id="83473-234">Välj **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="83473-234">Select **Connect**.</span></span>

5. <span data-ttu-id="83473-235">När tjänsten har anslutits visas den under noden **(SAS för tjänst)**.</span><span class="sxs-lookup"><span data-stu-id="83473-235">After it is attached, the newly attached service is displayed under the **(Service SAS)** node.</span></span>

    ![Resultatet av att ansluta till en delad tjänst med en SAS][20]

## <a name="search-for-storage-accounts"></a><span data-ttu-id="83473-237">Söka efter lagringskonton</span><span class="sxs-lookup"><span data-stu-id="83473-237">Search for storage accounts</span></span>
<span data-ttu-id="83473-238">Om du har en lång lista med lagringskonton kan du snabbt hitta ett visst lagringskonto genom att använda sökrutan överst i den vänstra rutan.</span><span class="sxs-lookup"><span data-stu-id="83473-238">If you have a long list of storage accounts, a quick way to locate a particular storage account is to use the search box at the top of the left pane.</span></span>

<span data-ttu-id="83473-239">När du skriver i sökrutan visas de lagringskonton som matchar sökvärdet som du har skrivit hittills i den vänstra rutan.</span><span class="sxs-lookup"><span data-stu-id="83473-239">As you type in the search box, the left pane displays the storage accounts that match the search value you've entered up to that point.</span></span> <span data-ttu-id="83473-240">På följande skärmbild visas ett exempel på en sökning efter alla lagringskonton vars namn innehåller **tarcher**:</span><span class="sxs-lookup"><span data-stu-id="83473-240">For example, a search for all storage accounts whose name contains **tarcher** is shown in the following screenshot:</span></span>

![Lagringskontosökning][11]

## <a name="next-steps"></a><span data-ttu-id="83473-242">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="83473-242">Next steps</span></span>
* [<span data-ttu-id="83473-243">Hantera Azure Blob Storage-resurser med Lagringsutforskaren (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="83473-243">Manage Azure Blob Storage resources with Storage Explorer (Preview)</span></span>](vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-icon.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-account-link.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/subscriptions-list.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-accounts-list.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/mase.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png
[25]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-certificate-azure-stack.png
[26]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/export-root-cert-azure-stack.png
[27]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/import-azure-stack-cert-storage-explorer.png
[28]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/select-target-azure-stack.png
[29]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-azure-stack-account.png
[30]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/select-accounts-azure-stack.png
[31]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/azure-stack-storage-account-list.png
