---
title: "aaaGet igång med Lagringsutforskaren (förhandsversion) | Microsoft Docs"
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
ms.openlocfilehash: 57737b51baace92858eb07c7dbc3139bd7e041f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-storage-explorer-preview"></a><span data-ttu-id="a2f9b-103">Kom igång med Lagringsutforskaren (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="a2f9b-103">Get started with Storage Explorer (Preview)</span></span>
## <a name="overview"></a><span data-ttu-id="a2f9b-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="a2f9b-104">Overview</span></span>
<span data-ttu-id="a2f9b-105">Azure Lagringsutforskaren (förhandsversion) är en fristående app som du kan använda tooeasily fungerar med Azure Storage-data i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-105">Azure Storage Explorer (Preview) is a standalone app that enables you tooeasily work with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="a2f9b-106">I den här artikeln får du lära dig hello ansluter tooand hantera Azure storage-konton på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-106">In this article, you learn hello various ways of connecting tooand managing your Azure storage accounts.</span></span>

![Microsoft Azure Lagringsutforskaren (förhandsversion)][15]

## <a name="prerequisites"></a><span data-ttu-id="a2f9b-108">Krav</span><span class="sxs-lookup"><span data-stu-id="a2f9b-108">Prerequisites</span></span>
* [<span data-ttu-id="a2f9b-109">Hämta och installera Lagringsutforskaren (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="a2f9b-109">Download and install Storage Explorer (Preview)</span></span>](http://www.storageexplorer.com)

## <a name="connect-tooa-storage-account-or-service"></a><span data-ttu-id="a2f9b-110">Ansluta tooa storage-konto eller tjänst</span><span class="sxs-lookup"><span data-stu-id="a2f9b-110">Connect tooa storage account or service</span></span>
<span data-ttu-id="a2f9b-111">Lagringsutforskaren (förhandsversion) innehåller flera olika sätt tooconnect toostorage konton.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-111">Storage Explorer (Preview) provides several ways tooconnect toostorage accounts.</span></span> <span data-ttu-id="a2f9b-112">Du kan till exempel:</span><span class="sxs-lookup"><span data-stu-id="a2f9b-112">For example, you can:</span></span>
* <span data-ttu-id="a2f9b-113">Ansluta toostorage konton som är kopplade till dina Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-113">Connect toostorage accounts associated with your Azure subscriptions.</span></span>
* <span data-ttu-id="a2f9b-114">Ansluta toostorage konton och tjänster som delas från andra Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-114">Connect toostorage accounts and services that are shared from other Azure subscriptions.</span></span>
* <span data-ttu-id="a2f9b-115">Ansluta tooand hantera lokal lagring med hjälp av hello Azure Storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-115">Connect tooand manage local storage by using hello Azure Storage Emulator.</span></span> 

<span data-ttu-id="a2f9b-116">Dessutom kan du arbeta med lagringskonton i globala och nationella Azure:</span><span class="sxs-lookup"><span data-stu-id="a2f9b-116">In addition, you can work with storage accounts in global and national Azure:</span></span>

* <span data-ttu-id="a2f9b-117">[Ansluta tooan Azure-prenumeration](#connect-to-an-azure-subscription): hantera lagringsresurser som hör tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-117">[Connect tooan Azure subscription](#connect-to-an-azure-subscription): Manage storage resources that belong tooyour Azure subscription.</span></span>
* <span data-ttu-id="a2f9b-118">[Arbeta med lokal utveckling lagring](#work-with-local-development-storage): hantera lokal lagring med hjälp av hello Azure Storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-118">[Work with local development storage](#work-with-local-development-storage): Manage local storage by using hello Azure Storage Emulator.</span></span>
* <span data-ttu-id="a2f9b-119">[Koppla tooexternal lagring](#attach-or-detach-an-external-storage-account): hantera lagringsresurser som hör tooanother Azure-prenumeration eller som är under nationella Azure moln med hjälp av hello lagringskontots namn, nyckel och slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-119">[Attach tooexternal storage](#attach-or-detach-an-external-storage-account): Manage storage resources that belong tooanother Azure subscription or that are under national Azure clouds by using hello storage account's name, key, and endpoints.</span></span>
* <span data-ttu-id="a2f9b-120">[Koppla ett lagringskonto med hjälp av en SAS](#attach-storage-account-using-sas): hantera lagringsresurser som hör tooanother Azure-prenumeration med hjälp av en signatur för delad åtkomst (SAS).</span><span class="sxs-lookup"><span data-stu-id="a2f9b-120">[Attach a storage account by using an SAS](#attach-storage-account-using-sas): Manage storage resources that belong tooanother Azure subscription by using a shared access signature (SAS).</span></span>
* <span data-ttu-id="a2f9b-121">[Koppla en tjänst med hjälp av en SAS](#attach-service-using-sas): hantera en specifik lagringstjänst (blobbehållare, kö eller tabell) som tillhör tooanother Azure-prenumeration med hjälp av en SAS.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-121">[Attach a service by using an SAS](#attach-service-using-sas): Manage a specific storage service (blob container, queue, or table) that belongs tooanother Azure subscription by using an SAS.</span></span>

## <a name="connect-tooan-azure-subscription"></a><span data-ttu-id="a2f9b-122">Ansluta tooan Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a2f9b-122">Connect tooan Azure subscription</span></span>
> [!NOTE]
> <span data-ttu-id="a2f9b-123">Om du inte redan har ett Azure-konto kan du [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) eller [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="a2f9b-123">If you don't have an Azure account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>
>
>

1. <span data-ttu-id="a2f9b-124">I Lagringsutforskaren (förhandsversion) väljer du **Inställningar för Azure-konto**.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-124">In Storage Explorer (Preview), select **Azure Account settings**.</span></span>

    ![Inställningar för Azure-konto][0]

2. <span data-ttu-id="a2f9b-126">hello vänster visar alla hello Microsoft-konton som du har loggat in till.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-126">hello left pane displays all hello Microsoft accounts you've signed in to.</span></span> <span data-ttu-id="a2f9b-127">tooconnect tooanother kontot som väljer **Lägg till ett konto**, och följ sedan hello instruktioner toosign in med ett Microsoft-konto som är associerad med minst en aktiv Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-127">tooconnect tooanother account, select **Add an account**, and then follow hello instructions toosign in with a Microsoft account that is associated with at least one active Azure subscription.</span></span>

    >[!NOTE]
    ><span data-ttu-id="a2f9b-128">Ansluta toonational Azure (till exempel Azure Tyskland Azure Government och Azure Kina inloggning) stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-128">Connecting toonational Azure (such as Azure Germany, Azure Government, and Azure China via sign-in) is currently not supported.</span></span> <span data-ttu-id="a2f9b-129">Se hello ”Anslut eller koppla från ett externt lagringskonto” avsnittet om hur tooconnect toonational Azure storage-konton.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-129">See hello "Attach or detach an external storage account" section for how tooconnect toonational Azure storage accounts.</span></span>

3. <span data-ttu-id="a2f9b-130">När du loggar in med ett Microsoft-konto, hello vänster fönsterruta fylls med hello Azure-prenumerationer som är associerade med det kontot.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-130">After you successfully sign in with a Microsoft account, hello left pane is populated with hello Azure subscriptions associated with that account.</span></span> <span data-ttu-id="a2f9b-131">Välj hello Azure-prenumerationer som du vill toowork med och välj sedan **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-131">Select hello Azure subscriptions that you want toowork with, and then select **Apply**.</span></span> <span data-ttu-id="a2f9b-132">(Att välja **alla prenumerationer** knapparna för att välja alla eller inga av hello visas Azure-prenumerationer.)</span><span class="sxs-lookup"><span data-stu-id="a2f9b-132">(Selecting **All subscriptions** toggles selecting all or none of hello listed Azure subscriptions.)</span></span>

    ![Välja Azure-prenumerationer][3]  
    <span data-ttu-id="a2f9b-134">hello vänster visar hello storage-konton som är associerade med hello valda Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-134">hello left pane displays hello storage accounts associated with hello selected Azure subscriptions.</span></span>

    ![Valda Azure-prenumerationer][4]

## <a name="connect-tooan-azure-stack-subscription"></a><span data-ttu-id="a2f9b-136">Anslut tooan Stack för Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a2f9b-136">Connect tooan Azure Stack subscription</span></span>

<span data-ttu-id="a2f9b-137">Information om anslutande tooan Azure Stack-prenumerationen finns [ansluta Lagringsutforskaren tooan Azure Stack prenumeration](azure-stack/azure-stack-storage-connect-se.md).</span><span class="sxs-lookup"><span data-stu-id="a2f9b-137">For information about connecting tooan Azure Stack subscription, see [Connect Storage Explorer tooan Azure Stack subscription](azure-stack/azure-stack-storage-connect-se.md).</span></span>

## <a name="work-with-local-development-storage"></a><span data-ttu-id="a2f9b-138">Arbeta med lokal utvecklingslagring</span><span class="sxs-lookup"><span data-stu-id="a2f9b-138">Work with local development storage</span></span>
<span data-ttu-id="a2f9b-139">Med Lagringsutforskaren (förhandsversion) kan arbeta du mot lokal lagring med hjälp av hello Azure Storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-139">With Storage Explorer (Preview), you can work against local storage by using hello Azure Storage Emulator.</span></span> <span data-ttu-id="a2f9b-140">Den här metoden kan du skriva kod mot och testa lagring utan att ha ett lagringskonto som har distribuerats på Azure, eftersom hello lagringskontot emuleras av hello Azure Storage-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-140">This approach lets you write code against and test storage without necessarily having a storage account deployed on Azure, because hello storage account is being emulated by hello Azure Storage Emulator.</span></span>

> [!NOTE]
> <span data-ttu-id="a2f9b-141">hello Azure Storage-emulatorn stöds för närvarande endast för Windows.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-141">hello Azure Storage Emulator is currently supported only for Windows.</span></span>
>
>

1. <span data-ttu-id="a2f9b-142">Hello vänstra rutan i Lagringsutforskaren (förhandsversion), expandera hello **(lokala och bifogad)** > **Lagringskonton** > **(utveckling)** nod.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-142">In hello left pane of Storage Explorer (Preview), expand hello **(Local and Attached)** > **Storage Accounts** > **(Development)** node.</span></span>

    ![Noden Lokal utveckling][21]

2. <span data-ttu-id="a2f9b-144">Om du ännu inte har installerat hello Azure Storage-emulatorn är du tillfrågas toodo så via ett informationsfält.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-144">If you have not yet installed hello Azure Storage Emulator, you are prompted toodo so via an infobar.</span></span> <span data-ttu-id="a2f9b-145">Om hello Informationsfältet visas väljer du **Download hello senaste versionen**, och sedan installera hello-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-145">If hello infobar is displayed, select **Download hello latest version**, and then install hello emulator.</span></span>

    ![Fråga om att hämta Azure Storage-emulatorn][22]

3. <span data-ttu-id="a2f9b-147">När hello emulatorn har installerats kan du skapa och arbeta med lokala blobbar, köer och tabeller.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-147">After hello emulator is installed, you can create and work with local blobs, queues, and tables.</span></span> <span data-ttu-id="a2f9b-148">toolearn hur skriver toowork med varje lagringskonto, finns i hello följande:</span><span class="sxs-lookup"><span data-stu-id="a2f9b-148">toolearn how toowork with each storage account type, see one of hello following:</span></span>

    * [<span data-ttu-id="a2f9b-149">Hantera Azure-bloblagringsresurser</span><span class="sxs-lookup"><span data-stu-id="a2f9b-149">Manage Azure blob storage resources</span></span>](vs-azure-tools-storage-explorer-blobs.md)
    * <span data-ttu-id="a2f9b-150">Hantera Azure-filresurslagringsresurser: *kommer snart*</span><span class="sxs-lookup"><span data-stu-id="a2f9b-150">Manage Azure file share storage resources: *Coming soon*</span></span>
    * <span data-ttu-id="a2f9b-151">Hantera Azure-kölagringsresurser: *kommer snart*</span><span class="sxs-lookup"><span data-stu-id="a2f9b-151">Manage Azure queue storage resources: *Coming soon*</span></span>
    * <span data-ttu-id="a2f9b-152">Hantera Azure-tabellagringsresurser: *kommer snart*</span><span class="sxs-lookup"><span data-stu-id="a2f9b-152">Manage Azure table storage resources: *Coming soon*</span></span>

## <a name="attach-or-detach-an-external-storage-account"></a><span data-ttu-id="a2f9b-153">Ansluta eller koppla från ett externt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="a2f9b-153">Attach or detach an external storage account</span></span>
<span data-ttu-id="a2f9b-154">Du kan koppla tooexternal storage-konton så att storage-konton kan delas med Lagringsutforskaren (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="a2f9b-154">With Storage Explorer (Preview), you can attach tooexternal storage accounts so that storage accounts can be easily shared.</span></span> <span data-ttu-id="a2f9b-155">Det här avsnittet beskrivs hur tooattach too(and detach from) externa lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-155">This section explains how tooattach too(and detach from) external storage accounts.</span></span>

### <a name="get-hello-storage-account-credentials"></a><span data-ttu-id="a2f9b-156">Hämta hello lagringskontouppgifter</span><span class="sxs-lookup"><span data-stu-id="a2f9b-156">Get hello storage account credentials</span></span>
<span data-ttu-id="a2f9b-157">tooshare ett externt lagringskonto hello ägaren av det kontot måste först hämta hello autentiseringsuppgifter (kontonamnet och nyckeln) för hello-kontot och sedan dela informationen med hello person som vill tooattach toothat (externa) kontot.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-157">tooshare an external storage account, hello owner of that account must first get hello credentials (account name and key) for hello account and then share that information with hello person who wants tooattach toothat (external) account.</span></span> <span data-ttu-id="a2f9b-158">Du kan hämta hello lagringskontouppgifter via hello Azure-portalen genom att göra hello följande:</span><span class="sxs-lookup"><span data-stu-id="a2f9b-158">You can obtain hello storage account credentials via hello Azure portal by doing hello following:</span></span>

1. <span data-ttu-id="a2f9b-159">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a2f9b-159">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="a2f9b-160">Välj **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-160">Select **Browse**.</span></span>

3. <span data-ttu-id="a2f9b-161">Välj **Lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-161">Select **Storage Accounts**.</span></span>

4. <span data-ttu-id="a2f9b-162">På hello **Lagringskonton** bladet, Välj hello önskad storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-162">On hello **Storage Accounts** blade, select hello desired storage account.</span></span>

5. <span data-ttu-id="a2f9b-163">På hello **inställningar** bladet för hello markerad storage-konto, markerar du **åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-163">On hello **Settings** blade for hello selected storage account, select **Access keys**.</span></span>

    ![Alternativ för åtkomstnycklar][5]

6. <span data-ttu-id="a2f9b-165">På hello **åtkomstnycklar** bladet, kopiera hello **lagringskontonamnet** och **key1** värden som ska användas när du ansluter toohello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-165">On hello **Access keys** blade, copy hello **Storage account name** and **key1** values for use when attaching toohello storage account.</span></span>

    ![Åtkomstnycklar][6]

### <a name="attach-tooan-external-storage-account"></a><span data-ttu-id="a2f9b-167">Koppla tooan externt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="a2f9b-167">Attach tooan external storage account</span></span>
<span data-ttu-id="a2f9b-168">tooattach tooan externa lagringskonto måste hello kontots namn och nyckel.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-168">tooattach tooan external storage account, you need hello account's name and key.</span></span> <span data-ttu-id="a2f9b-169">Hej förklaras ”Get hello lagringskontouppgifter” hur dessa värden från tooobtain hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-169">hello "Get hello storage account credentials" section explains how tooobtain these values from hello Azure portal.</span></span> <span data-ttu-id="a2f9b-170">Men i hello portal hello kontonyckel kallas **key1**.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-170">However, in hello portal, hello account key is called **key1**.</span></span> <span data-ttu-id="a2f9b-171">Så där Lagringsutforskaren (förhandsversion) begär en kontonyckel, ange hello **key1** värde.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-171">So where Storage Explorer (Preview) asks for an account key, you enter hello **key1** value.</span></span>

1. <span data-ttu-id="a2f9b-172">I Lagringsutforskaren (förhandsversion), väljer **ansluta tooAzure lagring**.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-172">In Storage Explorer (Preview), select **Connect tooAzure storage**.</span></span>

    ![Ansluta tooAzure lagringsalternativ][23]

2. <span data-ttu-id="a2f9b-174">I hello **ansluta tooAzure lagring** dialogrutan Ange hello kontonyckel (hello **key1** värde från hello Azure-portalen), och välj sedan **nästa**.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-174">In hello **Connect tooAzure Storage** dialog box, specify hello account key (hello **key1** value from hello Azure portal), and then select **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a2f9b-175">Du kan ange hello lagringsanslutningssträngen från ett lagringskonto på nationella Azure.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-175">You can enter hello storage connection string from a storage account on national Azure.</span></span> <span data-ttu-id="a2f9b-176">Till exempel ange tooconnect tooAzure Tyskland storage-konton, anslutning strängar liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="a2f9b-176">For example, tooconnect tooAzure Germany storage accounts, enter connection strings similar toohello following:</span></span> 
    >
    >* <span data-ttu-id="a2f9b-177">DefaultEndpointsProtocol=https</span><span class="sxs-lookup"><span data-stu-id="a2f9b-177">DefaultEndpointsProtocol=https</span></span>
    >* <span data-ttu-id="a2f9b-178">AccountName=cawatest03</span><span class="sxs-lookup"><span data-stu-id="a2f9b-178">AccountName=cawatest03</span></span>
    >* <span data-ttu-id="a2f9b-179">AccountKey=<storage_account_key></span><span class="sxs-lookup"><span data-stu-id="a2f9b-179">AccountKey=<storage_account_key></span></span>
    >* <span data-ttu-id="a2f9b-180">EndpointSuffix=core.cloudapi.de</span><span class="sxs-lookup"><span data-stu-id="a2f9b-180">EndpointSuffix=core.cloudapi.de</span></span>
    
    ><span data-ttu-id="a2f9b-181">Du kan hämta hello anslutningssträngen från hello Azure portal i hello samma sätt som beskrivs i hello ”hämta hello lagringskontouppgifter” avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-181">You can get hello connection string from hello Azure portal in hello same way as described in hello "Get hello storage account credentials" section.</span></span>

    ![Ansluta tooAzure lagring dialogruta][24]

3. <span data-ttu-id="a2f9b-183">I hello **Anslut extern lagring** i dialogrutan hello **kontonamn** rutan, ange hello lagringskontonamn, ange andra inställningar och välj sedan **nästa**.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-183">In hello **Attach External Storage** dialog box, in hello **Account name** box, enter hello storage account name, specify any other desired settings, and then select **Next**.</span></span>

    ![Dialogrutan Anslut extern lagring][8]

4. <span data-ttu-id="a2f9b-185">I hello **anslutning sammanfattning** dialogrutan Kontrollera hello information.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-185">In hello **Connection Summary** dialog box, verify hello information.</span></span> <span data-ttu-id="a2f9b-186">Om du vill toochange något annat markerar **tillbaka** och ange hello önskade inställningar.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-186">If you want toochange anything, select **Back** and reenter hello desired settings.</span></span> 

5. <span data-ttu-id="a2f9b-187">Välj **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-187">Select **Connect**.</span></span>

6. <span data-ttu-id="a2f9b-188">När den har anslutits visas hello externt lagringskonto med **(externt)** läggs toohello lagringskontonamn.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-188">After it is successfully connected, hello external storage account is displayed with **(External)** appended toohello storage account name.</span></span>

    ![Resultatet av ansluter tooan externt lagringskonto][9]

### <a name="detach-from-an-external-storage-account"></a><span data-ttu-id="a2f9b-190">Ansluta från ett externt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="a2f9b-190">Detach from an external storage account</span></span>
1. <span data-ttu-id="a2f9b-191">Högerklicka på hello externa lagringskonto som du vill toodetach och välj sedan **Detach**.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-191">Right-click hello external storage account that you want toodetach, and then select **Detach**.</span></span>

    ![Koppla bort lagringsalternativ][10]

2. <span data-ttu-id="a2f9b-193">Markera i hello bekräftelsemeddelandet **Ja** tooconfirm hello vill koppla bort hello externa lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-193">In hello confirmation message, select **Yes** tooconfirm hello detachment from hello external storage account.</span></span>

## <a name="attach-a-storage-account-by-using-an-sas"></a><span data-ttu-id="a2f9b-194">Ansluta ett lagringskonto med hjälp av en SAS</span><span class="sxs-lookup"><span data-stu-id="a2f9b-194">Attach a storage account by using an SAS</span></span>
<span data-ttu-id="a2f9b-195">En [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) kan Hej administratör för Azure-prenumeration bevilja tillfällig åtkomst tooa storage-konto utan att behöva tooprovide autentiseringsuppgifter för Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-195">An [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) lets hello admin of an Azure subscription grant temporary access tooa storage account without having tooprovide Azure subscription credentials.</span></span>

<span data-ttu-id="a2f9b-196">tooillustrate det här scenariot kan vi säga att UserA är administratör för Azure-prenumeration och UserA vill tooallow b tooaccess en storage-konto under en begränsad tid med särskilda behörigheter:</span><span class="sxs-lookup"><span data-stu-id="a2f9b-196">tooillustrate this scenario, let's say that UserA is an admin of an Azure subscription, and UserA wants tooallow UserB tooaccess a storage account for a limited time with certain permissions:</span></span>

1. <span data-ttu-id="a2f9b-197">Användare a genererar en SAS (som består av hello anslutningssträngen för hello lagringskontot) för en viss tid tidsperiod och med hello önskade behörigheter.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-197">UserA generates an SAS (consisting of hello connection string for hello storage account) for a specific time period and with hello desired permissions.</span></span>

2. <span data-ttu-id="a2f9b-198">Resurser för UserA hello SAS med hello person (i vårt exempel b) som vill ha åtkomst toohello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-198">UserA shares hello SAS with hello person (UserB, in our example) who wants access toohello storage account.</span></span>  

3. <span data-ttu-id="a2f9b-199">B använder Lagringsutforskaren (förhandsversion) tooattach toohello konto som tillhör tooUserA med hjälp av hello angivna SAS.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-199">UserB uses Storage Explorer (Preview) tooattach toohello account that belongs tooUserA by using hello supplied SAS.</span></span>

### <a name="get-an-sas-for-hello-account-you-want-tooshare"></a><span data-ttu-id="a2f9b-200">Hämta en SAS för hello-konto som du vill tooshare</span><span class="sxs-lookup"><span data-stu-id="a2f9b-200">Get an SAS for hello account you want tooshare</span></span>
1. <span data-ttu-id="a2f9b-201">I Lagringsutforskaren (förhandsversion) högerklickar du på hello storage-konto som du vill dela och välj sedan **hämta signatur för delad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-201">In Storage Explorer (Preview), right-click hello storage account you want share, and then select **Get Shared Access Signature**.</span></span>

    ![Snabbmenyalternativet Hämta SAS][13]

2. <span data-ttu-id="a2f9b-203">I hello **signatur för delad åtkomst** dialogrutan Ange hello tidsintervall och behörigheter som du vill använda för hello kontot och välj sedan **skapa**.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-203">In hello **Shared Access Signature** dialog box, specify hello time frame and permissions that you want for hello account, and then select **Create**.</span></span>

    <span data-ttu-id="a2f9b-204">Dialogrutan ![Hämta SAS][14]</span><span class="sxs-lookup"><span data-stu-id="a2f9b-204">![Get SAS dialog box][14]</span></span>  
    <span data-ttu-id="a2f9b-205">Hej **signatur för delad åtkomst** dialogruta öppnas och visar hello SAS.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-205">hello **Shared Access Signature** dialog box opens and displays hello SAS.</span></span>

3. <span data-ttu-id="a2f9b-206">Nästa toohello **anslutningssträngen**väljer **kopiera** toocopy den toohello Urklipp och markera sedan **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-206">Next toohello **Connection String**, select **Copy** toocopy it toohello clipboard, and then select **Close**.</span></span>

### <a name="attach-toohello-shared-account-by-using-hello-sas"></a><span data-ttu-id="a2f9b-207">Koppla toohello delade kontot med hjälp av hello SAS</span><span class="sxs-lookup"><span data-stu-id="a2f9b-207">Attach toohello shared account by using hello SAS</span></span>
1. <span data-ttu-id="a2f9b-208">I Lagringsutforskaren (förhandsversion), väljer **ansluta tooAzure lagring**.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-208">In Storage Explorer (Preview), select **Connect tooAzure storage**.</span></span>

    ![Ansluta tooAzure lagringsalternativ][23]

2. <span data-ttu-id="a2f9b-210">I hello **ansluta tooAzure lagring** dialogrutan Ange hello anslutningssträng och välj sedan **nästa**.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-210">In hello **Connect tooAzure Storage** dialog box, specify hello connection string, and then select **Next**.</span></span>

    ![Ansluta tooAzure lagring dialogruta][24]

3. <span data-ttu-id="a2f9b-212">I hello **anslutning sammanfattning** dialogrutan Kontrollera hello information.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-212">In hello **Connection Summary** dialog box, verify hello information.</span></span> <span data-ttu-id="a2f9b-213">Välj toomake ändringar **tillbaka**, och ange sedan hello-inställningar som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-213">toomake changes, select **Back**, and then enter hello settings you want.</span></span> 

4. <span data-ttu-id="a2f9b-214">Välj **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-214">Select **Connect**.</span></span>

5. <span data-ttu-id="a2f9b-215">När den är ansluten hello storage-konto visas med **(SAS)** läggs toohello kontonamn som du har angett.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-215">After it is attached, hello storage account is displayed with **(SAS)** appended toohello account name that you supplied.</span></span>

    ![Resultatet av anslutna tooan konto med hjälp av SAS][17]

## <a name="attach-a-service-by-using-an-sas"></a><span data-ttu-id="a2f9b-217">Ansluta en tjänst med en SAS</span><span class="sxs-lookup"><span data-stu-id="a2f9b-217">Attach a service by using an SAS</span></span>
<span data-ttu-id="a2f9b-218">Hej förklaras ”koppla ett lagringskonto med hjälp av en SAS” hur administratör Azure-prenumeration kan bevilja tillfällig åtkomst tooa storage-konto genom att generera och dela en SAS för hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-218">hello "Attach a storage account by using an SAS" section explains how an Azure subscription admin can grant temporary access tooa storage account by generating and sharing an SAS for hello storage account.</span></span> <span data-ttu-id="a2f9b-219">På samma sätt kan en SAS genereras för en specifik tjänst (blobbehållare, kö eller tabell) i ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-219">Similarly, an SAS can be generated for a specific service (blob container, queue, or table) within a storage account.</span></span>  

### <a name="generate-an-sas-for-hello-service-that-you-want-tooshare"></a><span data-ttu-id="a2f9b-220">Generera en SAS för hello-tjänsten som du vill tooshare</span><span class="sxs-lookup"><span data-stu-id="a2f9b-220">Generate an SAS for hello service that you want tooshare</span></span>
<span data-ttu-id="a2f9b-221">I det här scenariot kan en tjänst vara en blobbehållare, en kö eller en tabell.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-221">In this context, a service can be a blob container, queue, or table.</span></span> <span data-ttu-id="a2f9b-222">toogenerate hello SAS för en tjänst, se:</span><span class="sxs-lookup"><span data-stu-id="a2f9b-222">toogenerate hello SAS for a listed service, see:</span></span>

* [<span data-ttu-id="a2f9b-223">Hämta hello SAS för en blob-behållare</span><span class="sxs-lookup"><span data-stu-id="a2f9b-223">Get hello SAS for a blob container</span></span>](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
* <span data-ttu-id="a2f9b-224">Hämta hello SAS för en filresurs: *kommer snart*</span><span class="sxs-lookup"><span data-stu-id="a2f9b-224">Get hello SAS for a file share: *Coming soon*</span></span>
* <span data-ttu-id="a2f9b-225">Hämta hello SAS för en kö: *kommer snart*</span><span class="sxs-lookup"><span data-stu-id="a2f9b-225">Get hello SAS for a queue: *Coming soon*</span></span>
* <span data-ttu-id="a2f9b-226">Hämta hello SAS för en tabell: *kommer snart*</span><span class="sxs-lookup"><span data-stu-id="a2f9b-226">Get hello SAS for a table: *Coming soon*</span></span>

### <a name="attach-toohello-shared-account-service-by-using-hello-sas"></a><span data-ttu-id="a2f9b-227">Koppla toohello delade kontot tjänsten med hjälp av hello SAS</span><span class="sxs-lookup"><span data-stu-id="a2f9b-227">Attach toohello shared account service by using hello SAS</span></span>
1. <span data-ttu-id="a2f9b-228">I Lagringsutforskaren (förhandsversion), väljer **ansluta tooAzure lagring**.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-228">In Storage Explorer (Preview), select **Connect tooAzure storage**.</span></span>

    ![Ansluta tooAzure lagringsalternativ][23]

2. <span data-ttu-id="a2f9b-230">I hello **ansluta tooAzure lagring** dialogrutan Ange hello SAS-URI och välj sedan **nästa**.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-230">In hello **Connect tooAzure Storage** dialog box, specify hello SAS URI, and then select **Next**.</span></span>

    ![Ansluta tooAzure lagring dialogruta][24]

3. <span data-ttu-id="a2f9b-232">I hello **anslutning sammanfattning** dialogrutan Kontrollera hello information.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-232">In hello **Connection Summary** dialog box, verify hello information.</span></span> <span data-ttu-id="a2f9b-233">Välj toomake ändringar **tillbaka**, och ange sedan hello-inställningar som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-233">toomake changes, select **Back**, and then enter hello settings you want.</span></span> 

4. <span data-ttu-id="a2f9b-234">Välj **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-234">Select **Connect**.</span></span>

5. <span data-ttu-id="a2f9b-235">När den är ansluten hello tjänsten visas under hello **(tjänsten SAS)** nod.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-235">After it is attached, hello newly attached service is displayed under hello **(Service SAS)** node.</span></span>

    ![Resultatet av koppla tooa delade tjänsten med hjälp av en SAS][20]

## <a name="search-for-storage-accounts"></a><span data-ttu-id="a2f9b-237">Söka efter lagringskonton</span><span class="sxs-lookup"><span data-stu-id="a2f9b-237">Search for storage accounts</span></span>
<span data-ttu-id="a2f9b-238">Om du har en lång lista med lagringskonton, är ett snabbt sätt toolocate ett visst lagringskonto toouse hello sökrutan överst hello i hello till vänster.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-238">If you have a long list of storage accounts, a quick way toolocate a particular storage account is toouse hello search box at hello top of hello left pane.</span></span>

<span data-ttu-id="a2f9b-239">När du skriver i sökrutan hello vänster hello fönsterruta visar hello storage-konton som matchar hello sökvärdet som du har skrivit in toothat punkt.</span><span class="sxs-lookup"><span data-stu-id="a2f9b-239">As you type in hello search box, hello left pane displays hello storage accounts that match hello search value you've entered up toothat point.</span></span> <span data-ttu-id="a2f9b-240">Till exempel en sökning för alla lagringskonton vars namn innehåller **tarcher** visas i följande skärmbild hello:</span><span class="sxs-lookup"><span data-stu-id="a2f9b-240">For example, a search for all storage accounts whose name contains **tarcher** is shown in hello following screenshot:</span></span>

![Lagringskontosökning][11]

## <a name="next-steps"></a><span data-ttu-id="a2f9b-242">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a2f9b-242">Next steps</span></span>
* [<span data-ttu-id="a2f9b-243">Hantera Azure Blob Storage-resurser med Lagringsutforskaren (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="a2f9b-243">Manage Azure Blob Storage resources with Storage Explorer (Preview)</span></span>](vs-azure-tools-storage-explorer-blobs.md)

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
