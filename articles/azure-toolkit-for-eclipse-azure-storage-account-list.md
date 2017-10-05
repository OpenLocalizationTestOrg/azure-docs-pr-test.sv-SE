---
title: "Lista över Azure Storage-konto"
description: "Hantera lagringsinställningarna för ditt konto med hjälp av Azure-verktygen för Eclipse"
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
ms.openlocfilehash: f859efa389d3fe0b4b7b16255d57f1aa13123319
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-storage-account-list"></a><span data-ttu-id="6244d-103">Lista över Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="6244d-103">Azure Storage Account List</span></span>
<span data-ttu-id="6244d-104">Azure storage-konton aktivera hämtningsplatser som ska användas för JDK, programserver och godtyckliga komponenter, samt för att lagra tillstånd när du använder cachelagring.</span><span class="sxs-lookup"><span data-stu-id="6244d-104">Azure storage accounts enable download locations to be used for your JDK, application server, and arbitrary components, as well as for storing state when using caching.</span></span> <span data-ttu-id="6244d-105">Eclipse upprätthåller en lista över kända lagringskonton som är tillgängliga för ditt projekt i Eclipse-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="6244d-105">Eclipse maintains a list of known storage accounts that are available to your projects in your Eclipse workspace.</span></span> <span data-ttu-id="6244d-106">Öppna den **Lagringskonton** dialog, som används för att hantera listan i Eclipse klickar du på **fönstret**, klickar du på **inställningar**, expandera **Azure**, och klicka sedan på **Lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="6244d-106">To open the **Storage Accounts** dialog, which is used to manage that list, within Eclipse, click **Window**, click **Preferences**, expand **Azure**, and then click **Storage Accounts**.</span></span>

<span data-ttu-id="6244d-107">I följande visas den **Lagringskonton** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6244d-107">The following shows the **Storage Accounts** dialog.</span></span>

![][ic719496]

<span data-ttu-id="6244d-108">Den här dialogrutan kan även öppnas från en **konton** länk i dialogrutor som använder storage-konton, till exempel följande:</span><span class="sxs-lookup"><span data-stu-id="6244d-108">This dialog can also be opened from an **Accounts** link on dialog boxes that use storage accounts, such as the following:</span></span>

* <span data-ttu-id="6244d-109">Den **JDK** för den **serverkonfiguration** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6244d-109">The **JDK** tab of the **Server Configuration** dialog.</span></span>
* <span data-ttu-id="6244d-110">Den **Server** för den **serverkonfiguration** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6244d-110">The **Server** tab of the **Server Configuration** dialog.</span></span>
* <span data-ttu-id="6244d-111">Den **Lägg till komponent** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6244d-111">The **Add Component** dialog.</span></span>
* <span data-ttu-id="6244d-112">Den **cachelagring** egenskapsdialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6244d-112">The **Caching** properties dialog.</span></span>

## <a name="to-import-your-storage-accounts-using-a-publish-settings-file"></a><span data-ttu-id="6244d-113">Importera dina lagringskonton med hjälp av en fil med inställningar i Publicera</span><span class="sxs-lookup"><span data-stu-id="6244d-113">To import your storage accounts using a publish settings file</span></span>
1. <span data-ttu-id="6244d-114">I den **Lagringskonton** dialogrutan klickar du på **Import från PUBLICERINGSINSTÄLLNINGARNA filen**.</span><span class="sxs-lookup"><span data-stu-id="6244d-114">Within the **Storage Accounts** dialog, click **Import from PUBLISH-SETTINGS file**.</span></span>

2. <span data-ttu-id="6244d-115">(Hoppa över det här steget om du redan har sparat en Publicera fil på den lokala datorn.) I den **importera prenumerationsinformation** dialogrutan klickar du på **hämta PUBLICERINGSINSTÄLLNINGARNA filen**.</span><span class="sxs-lookup"><span data-stu-id="6244d-115">(Skip this step if you have already saved a publish settings file to your local machine.) In the **Import Subscription Information** dialog, click **Download PUBLISH-SETTINGS File**.</span></span> <span data-ttu-id="6244d-116">Om du inte ännu loggat in på ditt Azure-konto, uppmanas du att logga in.</span><span class="sxs-lookup"><span data-stu-id="6244d-116">If you are not yet logged into your Azure account, you will be prompted to log in.</span></span> <span data-ttu-id="6244d-117">Du uppmanas sedan att spara en Azure inställningsfilen för publicering.</span><span class="sxs-lookup"><span data-stu-id="6244d-117">Then you'll be prompted to save an Azure publish settings file.</span></span> <span data-ttu-id="6244d-118">(Du kan ignorera resulterande anvisningarna som visas på sidorna för inloggning - de som tillhandahålls av Azure portal och är avsedda för användare i Visual Studio.) Spara den på din lokala dator.</span><span class="sxs-lookup"><span data-stu-id="6244d-118">(You can ignore the resulting instructions shown on the logon pages - they are provided by the Azure portal and are intended for Visual Studio users.) Save it to your local machine.</span></span>

3. <span data-ttu-id="6244d-119">Fortfarande i den **importera prenumerationsinformation** dialogrutan klickar du på den **Bläddra** knappen Välj publicera inställningsfilen som du sparade lokalt tidigare och klicka sedan på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="6244d-119">Still in the **Import Subscription Information** dialog, click the **Browse** button, select the publish settings file that you saved locally previously, and then click **Open**.</span></span>

4. <span data-ttu-id="6244d-120">Klicka på **OK** att stänga den **importera prenumerationsinformation** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6244d-120">Click **OK** to close the **Import Subscription Information** dialog.</span></span>

## <a name="to-create-a-new-storage-account"></a><span data-ttu-id="6244d-121">Skapa ett nytt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="6244d-121">To create a new storage account</span></span>
1. <span data-ttu-id="6244d-122">I den **Lagringskonton** dialogrutan klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="6244d-122">Within the **Storage Accounts** dialog, click **Add**.</span></span>

2. <span data-ttu-id="6244d-123">I den **lägga till Lagringskontot** dialogrutan klickar du på **ny**.</span><span class="sxs-lookup"><span data-stu-id="6244d-123">Within the **Add Storage Account** dialog, click **New**.</span></span>

3. <span data-ttu-id="6244d-124">I den **Lagringskonto** dialogrutan, ange värden för följande:</span><span class="sxs-lookup"><span data-stu-id="6244d-124">Within the **New Storage Account** dialog, specify values for the following:</span></span>

   * <span data-ttu-id="6244d-125">Lagringskontonamn.</span><span class="sxs-lookup"><span data-stu-id="6244d-125">Storage account name.</span></span>

   * <span data-ttu-id="6244d-126">Platsen för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="6244d-126">Location of the storage account.</span></span>

   * <span data-ttu-id="6244d-127">Beskrivning av lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="6244d-127">Description of the storage account.</span></span>

   * <span data-ttu-id="6244d-128">Den prenumeration som lagringskontot tillhör.</span><span class="sxs-lookup"><span data-stu-id="6244d-128">The subscription to which the storage account belongs.</span></span>

4. <span data-ttu-id="6244d-129">Klicka på **OK** att stänga den **Lagringskonto** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6244d-129">Click **OK** to close the **New Storage Account** dialog.</span></span>

<span data-ttu-id="6244d-130">Det kan ta flera minuter för ditt lagringskonto skapas.</span><span class="sxs-lookup"><span data-stu-id="6244d-130">It may take several minutes for your storage account to be created.</span></span> <span data-ttu-id="6244d-131">När den har skapats klickar du på **OK** att stänga den **lägga till Lagringskontot** dialogrutan och det nya kontot läggs till i listan över tillgängliga storage-konton.</span><span class="sxs-lookup"><span data-stu-id="6244d-131">After it is created, click **OK** to close the **Add Storage Account** dialog, and your new storage account will be added to the list of available storage accounts.</span></span>

## <a name="to-add-an-existing-storage-account-to-the-list"></a><span data-ttu-id="6244d-132">Att lägga till ett befintligt lagringskonto i listan</span><span class="sxs-lookup"><span data-stu-id="6244d-132">To add an existing storage account to the list</span></span>
1. <span data-ttu-id="6244d-133">Om du inte redan har ett Azure storage-konto kan du skapa en genom att följa stegen i den **att skapa ett nytt avsnitt för storage-konto** ovan.</span><span class="sxs-lookup"><span data-stu-id="6244d-133">If you do not already have a Azure storage account, create one by following the steps listed in the **To create a new storage account section** above.</span></span> <span data-ttu-id="6244d-134">(Du kan också skapa ett nytt lagringskonto på den [Azure-hanteringsportalen][Azure Management Portal].)</span><span class="sxs-lookup"><span data-stu-id="6244d-134">(Alternatively, you can create a new storage account at the [Azure Management Portal][Azure Management Portal].)</span></span>

2. <span data-ttu-id="6244d-135">I den **Lagringskonton** dialogrutan klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="6244d-135">Within the **Storage Accounts** dialog, click **Add**.</span></span>

3. <span data-ttu-id="6244d-136">I den **lägga till Lagringskontot** dialogrutan Ange värden för **namn** och **åtkomstnyckeln**.</span><span class="sxs-lookup"><span data-stu-id="6244d-136">Within the **Add Storage Account** dialog, enter values for **Name** and **Access Key**.</span></span> <span data-ttu-id="6244d-137">Namn och kontonyckel måste vara för ett befintligt Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="6244d-137">The account name and access key must be for an existing Azure storage account.</span></span> <span data-ttu-id="6244d-138">Använd den **lagring** avsnitt i den [Azure-hanteringsportalen] [ Azure Management Portal] att visa dina lagringskontonamn och nycklar.</span><span class="sxs-lookup"><span data-stu-id="6244d-138">Use the **Storage** section of the [Azure Management Portal][Azure Management Portal] to view your storage account names and keys.</span></span> <span data-ttu-id="6244d-139">Din **lägga till Lagringskontot** dialogrutan ser ut ungefär så här.</span><span class="sxs-lookup"><span data-stu-id="6244d-139">Your **Add Storage Account** dialog will look similar to the following.</span></span>
   
   ![][ic719497]

4. <span data-ttu-id="6244d-140">Klicka på **OK** att stänga den **lägga till Lagringskontot** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6244d-140">Click **OK** to close the **Add Storage Account** dialog.</span></span>

## <a name="to-modify-a-storage-account-to-use-a-new-access-key"></a><span data-ttu-id="6244d-141">Så här ändrar du ett storage-konto om du vill använda en ny snabbtangent</span><span class="sxs-lookup"><span data-stu-id="6244d-141">To modify a storage account to use a new access key</span></span>
1. <span data-ttu-id="6244d-142">I den **Lagringskonton** dialogrutan, klickar du på lagringen konto som du vill redigera och klicka sedan på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="6244d-142">Within the **Storage Accounts** dialog, click the storage account that you want to edit and then click **Edit**.</span></span>

2. <span data-ttu-id="6244d-143">I den **redigera åtkomstnyckeln för Lagringskontot** dialogrutan Ändra den **åtkomstnyckeln** värde.</span><span class="sxs-lookup"><span data-stu-id="6244d-143">Within the **Edit Storage Account Access Key** dialog, modify the **Access Key** value.</span></span>

3. <span data-ttu-id="6244d-144">Klicka på **OK** att stänga den **redigera åtkomstnyckeln för Lagringskontot** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6244d-144">Click **OK** to close the **Edit Storage Account Access Key** dialog.</span></span>

## <a name="to-remove-a-storage-account-from-the-list-maintained-in-eclipse"></a><span data-ttu-id="6244d-145">Ta bort ett lagringskonto i listan som underhålls i Eclipse</span><span class="sxs-lookup"><span data-stu-id="6244d-145">To remove a storage account from the list maintained in Eclipse</span></span>
1. <span data-ttu-id="6244d-146">I den **Lagringskonton** dialogrutan, klickar du på lagringen konto som du vill redigera och klicka sedan på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="6244d-146">Within the **Storage Accounts** dialog, click the storage account that you want to edit and then click **Remove**.</span></span>

2. <span data-ttu-id="6244d-147">Klicka på **OK** när du uppmanas att ta bort lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="6244d-147">Click **OK** when prompted to remove the storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="6244d-148">Tar bort lagringskonto via den **Lagringskonton** dialogrutan bara tar bort den från listan över storage-konton visas i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="6244d-148">Removing the storage account through the **Storage Accounts** dialog only removes it from the list of storage accounts viewable within Eclipse.</span></span> <span data-ttu-id="6244d-149">Lagringskontot tas inte bort från din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6244d-149">It does not remove the storage account from your Azure subscription.</span></span> <span data-ttu-id="6244d-150">Dessutom kan lagringskontot visas igen i listan efter Eclipse läser information om din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6244d-150">Additionally, the storage account could appear again in your list after Eclipse reloads the details of your subscription.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="6244d-151">Se även</span><span class="sxs-lookup"><span data-stu-id="6244d-151">See Also</span></span>
<span data-ttu-id="6244d-152">[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="6244d-152">[Azure Toolkit for Eclipse][Azure Toolkit for Eclipse]</span></span>

<span data-ttu-id="6244d-153">[Installera Azure Toolkit för Eclipse][Installing the Azure Toolkit for Eclipse]</span><span class="sxs-lookup"><span data-stu-id="6244d-153">[Installing the Azure Toolkit for Eclipse][Installing the Azure Toolkit for Eclipse]</span></span> 

<span data-ttu-id="6244d-154">[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]</span><span class="sxs-lookup"><span data-stu-id="6244d-154">[Creating a Hello World Application for Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]</span></span>

<span data-ttu-id="6244d-155">Mer information om hur du använder Azure med Java finns det [Azure Java Developer Center][Azure Java Developer Center].</span><span class="sxs-lookup"><span data-stu-id="6244d-155">For more information about using Azure with Java, see the [Azure Java Developer Center][Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->
