---
title: aaaAzure Lagringskontolistan
description: "Hantera lagringsinställningarna för ditt konto med hello Azure Toolkit för Eclipse"
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: bbacfcd8-dbf5-4265-a930-59f508de5325
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 35e25881ca95ae4050a26283e4726d9549b37f46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-account-list"></a><span data-ttu-id="81d9d-103">Lista över Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="81d9d-103">Azure Storage Account List</span></span>
<span data-ttu-id="81d9d-104">Azure storage konton aktivera hämta platser toobe som används för JDK, programserver och godtyckliga komponenter, samt för att lagra tillstånd när du använder cachelagring.</span><span class="sxs-lookup"><span data-stu-id="81d9d-104">Azure storage accounts enable download locations toobe used for your JDK, application server, and arbitrary components, as well as for storing state when using caching.</span></span> <span data-ttu-id="81d9d-105">Eclipse upprätthåller en lista över kända lagringskonton som är tillgängliga tooyour projekt i Eclipse-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="81d9d-105">Eclipse maintains a list of known storage accounts that are available tooyour projects in your Eclipse workspace.</span></span> <span data-ttu-id="81d9d-106">tooopen hello **Lagringskonton** dialog, som har använt toomanage som listas i Eclipse klickar du på **fönstret**, klickar du på **inställningar**, expandera **Azure** , och klicka sedan på **Lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="81d9d-106">tooopen hello **Storage Accounts** dialog, which is used toomanage that list, within Eclipse, click **Window**, click **Preferences**, expand **Azure**, and then click **Storage Accounts**.</span></span>

<span data-ttu-id="81d9d-107">hello följande visar hello **Lagringskonton** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="81d9d-107">hello following shows hello **Storage Accounts** dialog.</span></span>

![][ic719496]

<span data-ttu-id="81d9d-108">Den här dialogrutan kan även öppnas från en **konton** länk i dialogrutor som använder storage-konton, till exempel hello följande:</span><span class="sxs-lookup"><span data-stu-id="81d9d-108">This dialog can also be opened from an **Accounts** link on dialog boxes that use storage accounts, such as hello following:</span></span>

* <span data-ttu-id="81d9d-109">Hej **JDK** för hello **serverkonfiguration** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="81d9d-109">hello **JDK** tab of hello **Server Configuration** dialog.</span></span>
* <span data-ttu-id="81d9d-110">Hej **Server** för hello **serverkonfiguration** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="81d9d-110">hello **Server** tab of hello **Server Configuration** dialog.</span></span>
* <span data-ttu-id="81d9d-111">Hej **Lägg till komponent** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="81d9d-111">hello **Add Component** dialog.</span></span>
* <span data-ttu-id="81d9d-112">Hej **cachelagring** egenskapsdialogrutan.</span><span class="sxs-lookup"><span data-stu-id="81d9d-112">hello **Caching** properties dialog.</span></span>

## <a name="tooimport-your-storage-accounts-using-a-publish-settings-file"></a><span data-ttu-id="81d9d-113">tooimport lagringen användarkonton med en publicera inställningsfil</span><span class="sxs-lookup"><span data-stu-id="81d9d-113">tooimport your storage accounts using a publish settings file</span></span>
1. <span data-ttu-id="81d9d-114">Inom hello **Lagringskonton** dialogrutan klickar du på **Import från PUBLICERINGSINSTÄLLNINGARNA filen**.</span><span class="sxs-lookup"><span data-stu-id="81d9d-114">Within hello **Storage Accounts** dialog, click **Import from PUBLISH-SETTINGS file**.</span></span>

2. <span data-ttu-id="81d9d-115">(Hoppa över det här steget om du redan har sparat en publicera inställningar filen tooyour lokal dator.) I hello **importera prenumerationsinformation** dialogrutan klickar du på **hämta PUBLICERINGSINSTÄLLNINGARNA filen**.</span><span class="sxs-lookup"><span data-stu-id="81d9d-115">(Skip this step if you have already saved a publish settings file tooyour local machine.) In hello **Import Subscription Information** dialog, click **Download PUBLISH-SETTINGS File**.</span></span> <span data-ttu-id="81d9d-116">Om du inte ännu loggat in på ditt Azure-konto, kommer du att ange toolog i.</span><span class="sxs-lookup"><span data-stu-id="81d9d-116">If you are not yet logged into your Azure account, you will be prompted toolog in.</span></span> <span data-ttu-id="81d9d-117">Sedan uppmanas du att toosave en Azure inställningsfilen för publicering.</span><span class="sxs-lookup"><span data-stu-id="81d9d-117">Then you'll be prompted toosave an Azure publish settings file.</span></span> <span data-ttu-id="81d9d-118">(Du kan ignorera hello resulterande anvisningarna som visas på hello inloggning sidor - de som tillhandahålls av hello Azure-portalen och är avsedda för användare i Visual Studio.) Spara den tooyour lokal dator.</span><span class="sxs-lookup"><span data-stu-id="81d9d-118">(You can ignore hello resulting instructions shown on hello logon pages - they are provided by hello Azure portal and are intended for Visual Studio users.) Save it tooyour local machine.</span></span>

3. <span data-ttu-id="81d9d-119">Fortfarande i hello **importera prenumerationsinformation** dialogrutan klickar du på hello **Bläddra** knappen, Välj hello Publicera fil med inställningar som du sparade lokalt tidigare och klicka sedan på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="81d9d-119">Still in hello **Import Subscription Information** dialog, click hello **Browse** button, select hello publish settings file that you saved locally previously, and then click **Open**.</span></span>

4. <span data-ttu-id="81d9d-120">Klicka på **OK** tooclose hello **importera prenumerationsinformation** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="81d9d-120">Click **OK** tooclose hello **Import Subscription Information** dialog.</span></span>

## <a name="toocreate-a-new-storage-account"></a><span data-ttu-id="81d9d-121">toocreate ett nytt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="81d9d-121">toocreate a new storage account</span></span>
1. <span data-ttu-id="81d9d-122">Inom hello **Lagringskonton** dialogrutan klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="81d9d-122">Within hello **Storage Accounts** dialog, click **Add**.</span></span>

2. <span data-ttu-id="81d9d-123">Inom hello **lägga till Lagringskontot** dialogrutan klickar du på **ny**.</span><span class="sxs-lookup"><span data-stu-id="81d9d-123">Within hello **Add Storage Account** dialog, click **New**.</span></span>

3. <span data-ttu-id="81d9d-124">Inom hello **Lagringskonto** dialogrutan, ange värden för hello följande:</span><span class="sxs-lookup"><span data-stu-id="81d9d-124">Within hello **New Storage Account** dialog, specify values for hello following:</span></span>

   * <span data-ttu-id="81d9d-125">Lagringskontonamn.</span><span class="sxs-lookup"><span data-stu-id="81d9d-125">Storage account name.</span></span>

   * <span data-ttu-id="81d9d-126">Platsen för hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="81d9d-126">Location of hello storage account.</span></span>

   * <span data-ttu-id="81d9d-127">Beskrivning av hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="81d9d-127">Description of hello storage account.</span></span>

   * <span data-ttu-id="81d9d-128">hello prenumeration toowhich hello lagringskontot tillhör.</span><span class="sxs-lookup"><span data-stu-id="81d9d-128">hello subscription toowhich hello storage account belongs.</span></span>

4. <span data-ttu-id="81d9d-129">Klicka på **OK** tooclose hello **Lagringskonto** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="81d9d-129">Click **OK** tooclose hello **New Storage Account** dialog.</span></span>

<span data-ttu-id="81d9d-130">Det kan ta flera minuter innan din lagring konto toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="81d9d-130">It may take several minutes for your storage account toobe created.</span></span> <span data-ttu-id="81d9d-131">När den har skapats klickar du på **OK** tooclose hello **lägga till Lagringskontot** dialogrutan, och det nya kontot läggs toohello lista över tillgängliga storage-konton.</span><span class="sxs-lookup"><span data-stu-id="81d9d-131">After it is created, click **OK** tooclose hello **Add Storage Account** dialog, and your new storage account will be added toohello list of available storage accounts.</span></span>

## <a name="tooadd-an-existing-storage-account-toohello-list"></a><span data-ttu-id="81d9d-132">tooadd en befintlig toohello lagringskontolistan</span><span class="sxs-lookup"><span data-stu-id="81d9d-132">tooadd an existing storage account toohello list</span></span>
1. <span data-ttu-id="81d9d-133">Om du inte redan har ett Azure storage-konto, skapa en genom att följa hello stegen som anges i hello **toocreate ett nytt avsnitt för storage-konto** ovan.</span><span class="sxs-lookup"><span data-stu-id="81d9d-133">If you do not already have a Azure storage account, create one by following hello steps listed in hello **toocreate a new storage account section** above.</span></span> <span data-ttu-id="81d9d-134">(Du kan också skapa ett nytt lagringskonto på hello [Azure-hanteringsportalen][Azure Management Portal].)</span><span class="sxs-lookup"><span data-stu-id="81d9d-134">(Alternatively, you can create a new storage account at hello [Azure Management Portal][Azure Management Portal].)</span></span>

2. <span data-ttu-id="81d9d-135">Inom hello **Lagringskonton** dialogrutan klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="81d9d-135">Within hello **Storage Accounts** dialog, click **Add**.</span></span>

3. <span data-ttu-id="81d9d-136">Inom hello **lägga till Lagringskontot** dialogrutan, ange värden för **namn** och **åtkomstnyckeln**.</span><span class="sxs-lookup"><span data-stu-id="81d9d-136">Within hello **Add Storage Account** dialog, enter values for **Name** and **Access Key**.</span></span> <span data-ttu-id="81d9d-137">hello namn och kontonyckel måste vara för ett befintligt Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="81d9d-137">hello account name and access key must be for an existing Azure storage account.</span></span> <span data-ttu-id="81d9d-138">Använd hello **lagring** avsnitt i hello [Azure-hanteringsportalen] [ Azure Management Portal] tooview lagringskontots namn och nycklar.</span><span class="sxs-lookup"><span data-stu-id="81d9d-138">Use hello **Storage** section of hello [Azure Management Portal][Azure Management Portal] tooview your storage account names and keys.</span></span> <span data-ttu-id="81d9d-139">Din **lägga till Lagringskontot** dialogrutan kommer att se liknande toohello följande.</span><span class="sxs-lookup"><span data-stu-id="81d9d-139">Your **Add Storage Account** dialog will look similar toohello following.</span></span>
   
   ![][ic719497]

4. <span data-ttu-id="81d9d-140">Klicka på **OK** tooclose hello **lägga till Lagringskontot** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="81d9d-140">Click **OK** tooclose hello **Add Storage Account** dialog.</span></span>

## <a name="toomodify-a-storage-account-toouse-a-new-access-key"></a><span data-ttu-id="81d9d-141">toomodify en storage-konto toouse en ny snabbtangent</span><span class="sxs-lookup"><span data-stu-id="81d9d-141">toomodify a storage account toouse a new access key</span></span>
1. <span data-ttu-id="81d9d-142">Inom hello **Lagringskonton** dialogrutan klickar du på hello storage-konto du vill tooedit och klicka sedan på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="81d9d-142">Within hello **Storage Accounts** dialog, click hello storage account that you want tooedit and then click **Edit**.</span></span>

2. <span data-ttu-id="81d9d-143">Inom hello **redigera åtkomstnyckeln för Lagringskontot** dialogrutan Ändra hello **åtkomstnyckeln** värde.</span><span class="sxs-lookup"><span data-stu-id="81d9d-143">Within hello **Edit Storage Account Access Key** dialog, modify hello **Access Key** value.</span></span>

3. <span data-ttu-id="81d9d-144">Klicka på **OK** tooclose hello **redigera åtkomstnyckeln för Lagringskontot** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="81d9d-144">Click **OK** tooclose hello **Edit Storage Account Access Key** dialog.</span></span>

## <a name="tooremove-a-storage-account-from-hello-list-maintained-in-eclipse"></a><span data-ttu-id="81d9d-145">tooremove ett lagringskonto hello listan underhålls i Eclipse</span><span class="sxs-lookup"><span data-stu-id="81d9d-145">tooremove a storage account from hello list maintained in Eclipse</span></span>
1. <span data-ttu-id="81d9d-146">Inom hello **Lagringskonton** dialogrutan klickar du på hello storage-konto du vill tooedit och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="81d9d-146">Within hello **Storage Accounts** dialog, click hello storage account that you want tooedit and then click **Remove**.</span></span>

2. <span data-ttu-id="81d9d-147">Klicka på **OK** när begärd tooremove hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="81d9d-147">Click **OK** when prompted tooremove hello storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="81d9d-148">Tar bort hello lagringskonto via hello **Lagringskonton** dialogrutan bara bort från hello lista med lagringskonton som visas i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="81d9d-148">Removing hello storage account through hello **Storage Accounts** dialog only removes it from hello list of storage accounts viewable within Eclipse.</span></span> <span data-ttu-id="81d9d-149">Hej lagringskontot tas inte bort från din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="81d9d-149">It does not remove hello storage account from your Azure subscription.</span></span> <span data-ttu-id="81d9d-150">Dessutom kan hello storage-konto visas igen i listan efter Eclipse laddar hello information om din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="81d9d-150">Additionally, hello storage account could appear again in your list after Eclipse reloads hello details of your subscription.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="81d9d-151">Se även</span><span class="sxs-lookup"><span data-stu-id="81d9d-151">See Also</span></span>
<span data-ttu-id="81d9d-152">[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="81d9d-152">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="81d9d-153">[Installera hello Azure Toolkit för Eclipse][Installing hello Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="81d9d-153">[Installing hello Azure Toolkit for Eclipse][Installing hello Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="81d9d-154">[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="81d9d-154">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="81d9d-155">Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="81d9d-155">For more information about using Azure with Java, see hello [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->
