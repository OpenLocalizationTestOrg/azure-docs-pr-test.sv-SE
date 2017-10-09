---
title: "aaaGet igång med Mobile Apps med hjälp av Xamarin.Forms"
description: "Följ den här självstudiekursen toostart med Mobilappar för Xamarin.Forms-utveckling"
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 5e692220-cc89-4548-96c8-35259722acf5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: af6b1c1ce4cf91c397552aa3d8ee40728129238c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-xamarinforms-app"></a><span data-ttu-id="603bf-103">Skapa en Xamarin.Forms-app</span><span class="sxs-lookup"><span data-stu-id="603bf-103">Create a Xamarin.Forms app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="603bf-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="603bf-104">Overview</span></span>
<span data-ttu-id="603bf-105">Den här kursen visar hur tooadd en molnbaserad serverdelstjänst tooa Xamarin.Forms-mobilapp med hjälp av hello Mobile Apps-funktionen i Azure App Service som hello serverdel.</span><span class="sxs-lookup"><span data-stu-id="603bf-105">This tutorial shows you how tooadd a cloud-based back-end service tooa Xamarin.Forms mobile app by using hello Mobile Apps feature of Azure App Service as hello back end.</span></span> <span data-ttu-id="603bf-106">Du skapar både en ny Mobile Apps-serverdel och en enkel ”todo list”-app med Xamarin.Forms där appdata lagras i Azure.</span><span class="sxs-lookup"><span data-stu-id="603bf-106">You create both a new Mobile Apps back end and a simple to-do list Xamarin.Forms app that stores app data in Azure.</span></span>

<span data-ttu-id="603bf-107">Du måste slutföra den här kursen innan du börjar någon annan kurs om Mobilappar för Xamarin.Forms-appar.</span><span class="sxs-lookup"><span data-stu-id="603bf-107">Completing this tutorial is a prerequisite for all other Mobile Apps tutorials for Xamarin.Forms.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="603bf-108">Krav</span><span class="sxs-lookup"><span data-stu-id="603bf-108">Prerequisites</span></span>
<span data-ttu-id="603bf-109">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="603bf-109">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="603bf-110">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="603bf-110">An active Azure account.</span></span> <span data-ttu-id="603bf-111">Om du inte har ett konto kan du registrera dig för en utvärderingsversion av Azure och få upp too10 mobilappar utan kostnad som du kan fortsätta att använda även efter utvärderingsperiodens slut.</span><span class="sxs-lookup"><span data-stu-id="603bf-111">If you don't have an account, you can sign up for an Azure trial and get up too10 free mobile apps that you can keep using even after your trial ends.</span></span> <span data-ttu-id="603bf-112">Mer information finns i [Kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="603bf-112">For more information, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="603bf-113">Visual Studio med Xamarin.</span><span class="sxs-lookup"><span data-stu-id="603bf-113">Visual Studio with Xamarin.</span></span> <span data-ttu-id="603bf-114">Mer information finns i hello [ställa upp och installera Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) sidan.</span><span class="sxs-lookup"><span data-stu-id="603bf-114">For information, see hello [Set up and install Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) page.</span></span>

* <span data-ttu-id="603bf-115">En Mac med Xcode v7.0 eller senare och Xamarin Studio Community installerat.</span><span class="sxs-lookup"><span data-stu-id="603bf-115">A Mac with Xcode v7.0 or later and Xamarin Studio Community installed.</span></span> <span data-ttu-id="603bf-116">Information finns på sidan för [installation och konfiguration av Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) och på sidan för [konfiguration, installation och verifiering för Mac-användare](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).</span><span class="sxs-lookup"><span data-stu-id="603bf-116">For information, see [Set up and install Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) and [Set up, install, and verify for Mac users](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).</span></span>

## <a name="create-a-new-mobile-apps-back-end"></a><span data-ttu-id="603bf-117">Skapa en ny Mobile Apps-serverdel</span><span class="sxs-lookup"><span data-stu-id="603bf-117">Create a new Mobile Apps back end</span></span>

<span data-ttu-id="603bf-118">toocreate en ny Mobile Apps tillbaka sluta hello följande:</span><span class="sxs-lookup"><span data-stu-id="603bf-118">toocreate a new Mobile Apps back end, do hello following:</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

<span data-ttu-id="603bf-119">Nu har du konfigurerat en Mobile Apps-serverdel som du kan använda för dina mobilklientprogram.</span><span class="sxs-lookup"><span data-stu-id="603bf-119">You have now set up a Mobile Apps back end that your mobile client applications can use.</span></span> <span data-ttu-id="603bf-120">Därefter ladda ned ett serverprojekt för en serverdel för enkel att göra-lista och sedan publicera tooAzure.</span><span class="sxs-lookup"><span data-stu-id="603bf-120">Next, you download a server project for a simple to-do list back end and then publish it tooAzure.</span></span>

## <a name="configure-hello-server-project"></a><span data-ttu-id="603bf-121">Konfigurera hello serverprojekt</span><span class="sxs-lookup"><span data-stu-id="603bf-121">Configure hello server project</span></span>

<span data-ttu-id="603bf-122">tooconfigure hello server projektet toouse Hej Node.js eller .NET-serverdel, hello följande:</span><span class="sxs-lookup"><span data-stu-id="603bf-122">tooconfigure hello server project toouse either hello Node.js or .NET back end, do hello following:</span></span>

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-xamarinforms-solution"></a><span data-ttu-id="603bf-123">Hämta och köra hello Xamarin.Forms-lösningen</span><span class="sxs-lookup"><span data-stu-id="603bf-123">Download and run hello Xamarin.Forms solution</span></span>

<span data-ttu-id="603bf-124">Du kan hämta hello lösning på två sätt.</span><span class="sxs-lookup"><span data-stu-id="603bf-124">You can download hello solution in either of two ways.</span></span> <span data-ttu-id="603bf-125">Hämta den tooa Mac och öppna det i Xamarin Studio eller hämta den tooa Windows-dator och öppna den i Visual Studio med hjälp av en nätverksansluten Mac för att skapa hello iOS-app.</span><span class="sxs-lookup"><span data-stu-id="603bf-125">Download it tooa Mac and open it in Xamarin Studio, or download it tooa Windows computer and open it in Visual Studio by using a networked Mac for building hello iOS app.</span></span> <span data-ttu-id="603bf-126">Mer information finns på sidan för att [installera och konfigurera Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span><span class="sxs-lookup"><span data-stu-id="603bf-126">For more information, see [Set up and install Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span></span>

<span data-ttu-id="603bf-127">På en Mac- eller Windows-dator, hello följande:</span><span class="sxs-lookup"><span data-stu-id="603bf-127">On a Mac or Windows computer, do hello following:</span></span>

1. <span data-ttu-id="603bf-128">Gå toohello [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="603bf-128">Go toohello [Azure portal].</span></span>

2. <span data-ttu-id="603bf-129">På hello **inställningar** bladet för din mobila app under **Mobile**väljer **Kom igång** > **Xamarin.Forms**.</span><span class="sxs-lookup"><span data-stu-id="603bf-129">On hello **Settings** blade for your mobile app, under **Mobile**, select **Get Started** > **Xamarin.Forms**.</span></span> <span data-ttu-id="603bf-130">Välj **Skapa en ny app** under **steg 3** och välj sedan **Ladda ned**.</span><span class="sxs-lookup"><span data-stu-id="603bf-130">Under **step 3**, select  **Create a new app**, and then select **Download**.</span></span>

   <span data-ttu-id="603bf-131">Den här åtgärden hämtar ett projekt som innehåller ett klientprogram som är anslutna tooyour mobila appar.</span><span class="sxs-lookup"><span data-stu-id="603bf-131">This action downloads a project that contains a client application that's connected tooyour mobile app.</span></span> <span data-ttu-id="603bf-132">Spara hello komprimerade projektet filen tooyour lokala datorn och notera var du sparar den.</span><span class="sxs-lookup"><span data-stu-id="603bf-132">Save hello compressed project file tooyour local computer, and make a note of where you save it.</span></span>

3. <span data-ttu-id="603bf-133">Extrahera hello-projekt som du laddade ned och öppna det i Xamarin Studio (Mac) eller Visual Studio (Windows).</span><span class="sxs-lookup"><span data-stu-id="603bf-133">Extract hello project that you downloaded, and then open it in Xamarin Studio (Mac) or Visual Studio (Windows).</span></span>

   ![Extraherat projekt i Xamarin Studio][9]

   ![Extraherat projekt i Visual Studio][8]

## <a name="optional-run-hello-ios-project"></a><span data-ttu-id="603bf-136">(Valfritt) Kör hello iOS-projekt</span><span class="sxs-lookup"><span data-stu-id="603bf-136">(Optional) Run hello iOS project</span></span>
<span data-ttu-id="603bf-137">I det här avsnittet kan du köra hello Xamarin iOS-projektet för iOS-enheter.</span><span class="sxs-lookup"><span data-stu-id="603bf-137">In this section, you run hello Xamarin iOS project for iOS devices.</span></span> <span data-ttu-id="603bf-138">Du kan hoppa över det här avsnittet om du inte arbetar med iOS-enheter.</span><span class="sxs-lookup"><span data-stu-id="603bf-138">You can skip this section if you are not working with iOS devices.</span></span>

#### <a name="in-xamarin-studio"></a><span data-ttu-id="603bf-139">I Xamarin Studio</span><span class="sxs-lookup"><span data-stu-id="603bf-139">In Xamarin Studio</span></span>
1. <span data-ttu-id="603bf-140">Högerklicka på hello iOS-projektet och välj sedan **Set As Startup Project**.</span><span class="sxs-lookup"><span data-stu-id="603bf-140">Right-click hello iOS project, and then select **Set As Startup Project**.</span></span>

2. <span data-ttu-id="603bf-141">På hello **kör** väljer du **Start Debugging** toobuild hello projektet och starta hello app i hello iPhone-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="603bf-141">On hello **Run** menu, select **Start Debugging** toobuild hello project and start hello app in hello iPhone emulator.</span></span>

#### <a name="in-visual-studio"></a><span data-ttu-id="603bf-142">I Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="603bf-142">In Visual Studio</span></span>
1. <span data-ttu-id="603bf-143">Högerklicka på hello iOS-projektet och välj sedan **Set as StartUp Project**.</span><span class="sxs-lookup"><span data-stu-id="603bf-143">Right-click hello iOS project, and then select **Set as StartUp Project**.</span></span>

2. <span data-ttu-id="603bf-144">På hello **skapa** väljer du **Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="603bf-144">On hello **Build** menu, select **Configuration Manager**.</span></span>

3. <span data-ttu-id="603bf-145">I hello **Configuration Manager** dialogrutan, Välj hello **skapa** och **distribuera** kryssrutorna nästa toohello iOS-projekt.</span><span class="sxs-lookup"><span data-stu-id="603bf-145">In hello **Configuration Manager** dialog box, select hello **Build** and **Deploy** check boxes next toohello iOS project.</span></span>

4. <span data-ttu-id="603bf-146">toobuild hello projektet och starta hello app i hello iPhone-emulatorn väljer hello **F5** nyckel.</span><span class="sxs-lookup"><span data-stu-id="603bf-146">toobuild hello project and start hello app in hello iPhone emulator, select hello **F5** key.</span></span>

   > [!NOTE]
   > <span data-ttu-id="603bf-147">Om du har problem i bygget hello projektet kör hello NuGet package manager och uppdaterar toohello senaste versionen av hello Xamarin paket.</span><span class="sxs-lookup"><span data-stu-id="603bf-147">If you have problems building hello project, run hello NuGet package manager and update toohello latest version of hello Xamarin support packages.</span></span> <span data-ttu-id="603bf-148">Snabbstartsprojekt kan vara långsam tooupdate toohello senaste versionerna.</span><span class="sxs-lookup"><span data-stu-id="603bf-148">Quickstart projects might be slow tooupdate toohello latest versions.</span></span>    
   >
   >

5. <span data-ttu-id="603bf-149">I hello app, ange en beskrivande text som *Läs om Xamarin*, och sedan väljer hello plustecken (**+**).</span><span class="sxs-lookup"><span data-stu-id="603bf-149">In hello app, type meaningful text, such as *Learn Xamarin*, and then select hello plus sign (**+**).</span></span>

    ![][10]

    <span data-ttu-id="603bf-150">Den här åtgärden skickar en post-begäran toohello nya Mobile Apps serverdel som finns i Azure.</span><span class="sxs-lookup"><span data-stu-id="603bf-150">This action sends a post request toohello new Mobile Apps back end that's hosted in Azure.</span></span> <span data-ttu-id="603bf-151">Data från hello begäran infogas i hello TodoItem-tabell.</span><span class="sxs-lookup"><span data-stu-id="603bf-151">Data from hello request is inserted into hello TodoItem table.</span></span> <span data-ttu-id="603bf-152">Objekt som lagras i hello tabellen returneras av hello Mobile Apps tillbaka slut och hello data visas i hello lista.</span><span class="sxs-lookup"><span data-stu-id="603bf-152">Items that are stored in hello table are returned by hello Mobile Apps back end, and hello data is displayed in hello list.</span></span>

    > [!NOTE]
    > <span data-ttu-id="603bf-153">Du hittar hello koden som ansluter till din Mobile Apps-serverdel i hello TodoItemManager.cs C#-filen hello portabla klassbiblioteksprojektet i lösningen.</span><span class="sxs-lookup"><span data-stu-id="603bf-153">You'll find hello code that accesses your Mobile Apps back end in hello TodoItemManager.cs C# file of hello portable class library project of your solution.</span></span>
    >
    >

## <a name="optional-run-hello-android-project"></a><span data-ttu-id="603bf-154">(Valfritt) Kör hello Android-projekt</span><span class="sxs-lookup"><span data-stu-id="603bf-154">(Optional) Run hello Android project</span></span>
<span data-ttu-id="603bf-155">I det här avsnittet kan köra du hello Xamarin droid-projektet för Android.</span><span class="sxs-lookup"><span data-stu-id="603bf-155">In this section, you run hello Xamarin droid project for Android.</span></span> <span data-ttu-id="603bf-156">Du kan hoppa över det här avsnittet om du inte arbetar med Androidenheter.</span><span class="sxs-lookup"><span data-stu-id="603bf-156">You can skip this section if you are not working with Android devices.</span></span>

#### <a name="in-xamarin-studio"></a><span data-ttu-id="603bf-157">I Xamarin Studio</span><span class="sxs-lookup"><span data-stu-id="603bf-157">In Xamarin Studio</span></span>

1. <span data-ttu-id="603bf-158">Högerklicka på hello Android-projekt och välj sedan **Set As Startup Project**.</span><span class="sxs-lookup"><span data-stu-id="603bf-158">Right-click hello Android project, and then select **Set As Startup Project**.</span></span>

2. <span data-ttu-id="603bf-159">toobuild hello projektet och starta hello app i en androidemulator på hello **kör** väljer du **Start Debugging**.</span><span class="sxs-lookup"><span data-stu-id="603bf-159">toobuild hello project and start hello app in an Android emulator, on hello **Run** menu, select **Start Debugging**.</span></span>

#### <a name="in-visual-studio"></a><span data-ttu-id="603bf-160">I Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="603bf-160">In Visual Studio</span></span>

1. <span data-ttu-id="603bf-161">Högerklicka på hello Android (Droid)-projektet och välj sedan **Set as StartUp Project**.</span><span class="sxs-lookup"><span data-stu-id="603bf-161">Right-click hello Android (Droid) project, and then select **Set as StartUp Project**.</span></span>

2. <span data-ttu-id="603bf-162">På hello **skapa** väljer du **Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="603bf-162">On hello **Build** menu, select **Configuration Manager**.</span></span>

3. <span data-ttu-id="603bf-163">I hello **Configuration Manager** dialogrutan, Välj hello **skapa** och **distribuera** kryssrutorna nästa toohello Android-projekt.</span><span class="sxs-lookup"><span data-stu-id="603bf-163">In hello **Configuration Manager** dialog box, select hello **Build** and **Deploy** check boxes next toohello Android project.</span></span>

4. <span data-ttu-id="603bf-164">toobuild hello projektet och starta hello app i en androidemulator väljer hello **F5** nyckel.</span><span class="sxs-lookup"><span data-stu-id="603bf-164">toobuild hello project and start hello app in an Android emulator, select hello **F5** key.</span></span>

   > [!NOTE]
   > <span data-ttu-id="603bf-165">Om du har problem i bygget hello projektet kör hello NuGet package manager och uppdaterar toohello senaste versionen av hello Xamarin paket.</span><span class="sxs-lookup"><span data-stu-id="603bf-165">If you have problems building hello project, run hello NuGet package manager and update toohello latest version of hello Xamarin support packages.</span></span> <span data-ttu-id="603bf-166">Snabbstartsprojekt kan vara långsam tooupdate toohello senaste versionerna.</span><span class="sxs-lookup"><span data-stu-id="603bf-166">Quickstart projects might be slow tooupdate toohello latest versions.</span></span>    
   >
   >

5. <span data-ttu-id="603bf-167">I hello app, ange en beskrivande text som *Läs om Xamarin*, och sedan väljer hello plustecken (**+**).</span><span class="sxs-lookup"><span data-stu-id="603bf-167">In hello app, type meaningful text, such as *Learn Xamarin*, and then select hello plus sign (**+**).</span></span>

    ![][11]
    
    <span data-ttu-id="603bf-168">Den här åtgärden skickar en post-begäran toohello nya Mobile Apps serverdel som finns i Azure.</span><span class="sxs-lookup"><span data-stu-id="603bf-168">This action sends a post request toohello new Mobile Apps back end that's hosted in Azure.</span></span> <span data-ttu-id="603bf-169">Data från hello begäran infogas i hello TodoItem-tabell.</span><span class="sxs-lookup"><span data-stu-id="603bf-169">Data from hello request is inserted into hello TodoItem table.</span></span> <span data-ttu-id="603bf-170">Objekt som lagras i hello tabellen returneras av hello Mobile Apps tillbaka slut och hello data visas i hello lista.</span><span class="sxs-lookup"><span data-stu-id="603bf-170">Items that are stored in hello table are returned by hello Mobile Apps back end, and hello data is displayed in hello list.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="603bf-171">Du hittar hello koden som ansluter till din Mobile Apps-serverdel i hello TodoItemManager.cs C#-filen hello portabla klassbiblioteksprojektet i lösningen.</span><span class="sxs-lookup"><span data-stu-id="603bf-171">You'll find hello code that accesses your Mobile Apps back end in hello TodoItemManager.cs C# file of hello portable class library project of your solution.</span></span>
    >
    >

## <a name="optional-run-hello-windows-project"></a><span data-ttu-id="603bf-172">(Valfritt) Kör Windows hello-projekt</span><span class="sxs-lookup"><span data-stu-id="603bf-172">(Optional) Run hello Windows project</span></span>

<span data-ttu-id="603bf-173">I det här avsnittet kan du köra hello Xamarin WinApp-projektet för Windows-enheter.</span><span class="sxs-lookup"><span data-stu-id="603bf-173">In this section, you run hello Xamarin WinApp project for Windows devices.</span></span> <span data-ttu-id="603bf-174">Du kan hoppa över det här avsnittet om du inte arbetar med Windowsenheter.</span><span class="sxs-lookup"><span data-stu-id="603bf-174">You can skip this section if you are not working with Windows devices.</span></span>

#### <a name="in-visual-studio"></a><span data-ttu-id="603bf-175">I Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="603bf-175">In Visual Studio</span></span>

1. <span data-ttu-id="603bf-176">Högerklicka på något av windowsprojekten hello och välj sedan **Set as StartUp Project**.</span><span class="sxs-lookup"><span data-stu-id="603bf-176">Right-click any of hello Windows projects, and then select **Set as StartUp Project**.</span></span>

2. <span data-ttu-id="603bf-177">På hello **skapa** väljer du **Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="603bf-177">On hello **Build** menu, select **Configuration Manager**.</span></span>

3. <span data-ttu-id="603bf-178">I hello **Configuration Manager** dialogrutan, Välj hello **skapa** och **distribuera** kryssrutorna nästa toohello Windows-projekt som du har valt.</span><span class="sxs-lookup"><span data-stu-id="603bf-178">In hello **Configuration Manager** dialog box, select hello **Build** and **Deploy** check boxes next toohello Windows project that you chose.</span></span>

4. <span data-ttu-id="603bf-179">toobuild hello projektet och starta hello app i en Windows-emulator, Välj hello **F5** nyckel.</span><span class="sxs-lookup"><span data-stu-id="603bf-179">toobuild hello project and start hello app in a Windows emulator, select hello **F5** key.</span></span>

   > [!NOTE]
   > <span data-ttu-id="603bf-180">Om du har problem i bygget hello projektet kör hello NuGet package manager och uppdaterar toohello senaste versionen av hello Xamarin paket.</span><span class="sxs-lookup"><span data-stu-id="603bf-180">If you have problems building hello project, run hello NuGet package manager and update toohello latest version of hello Xamarin support packages.</span></span> <span data-ttu-id="603bf-181">Snabbstartsprojekt kan vara långsam tooupdate toohello senaste versionerna.</span><span class="sxs-lookup"><span data-stu-id="603bf-181">Quickstart projects might be slow tooupdate toohello latest versions.</span></span>    
   >
   >

5. <span data-ttu-id="603bf-182">I hello app, ange en beskrivande text som *Läs om Xamarin*, och sedan väljer hello plustecken (**+**).</span><span class="sxs-lookup"><span data-stu-id="603bf-182">In hello app, type meaningful text, such as *Learn Xamarin*, and then select hello plus sign (**+**).</span></span>

    <span data-ttu-id="603bf-183">Den här åtgärden skickar en post-begäran toohello nya Mobile Apps serverdel som finns i Azure.</span><span class="sxs-lookup"><span data-stu-id="603bf-183">This action sends a post request toohello new Mobile Apps back end that's hosted in Azure.</span></span> <span data-ttu-id="603bf-184">Data från hello begäran infogas i hello TodoItem-tabell.</span><span class="sxs-lookup"><span data-stu-id="603bf-184">Data from hello request is inserted into hello TodoItem table.</span></span> <span data-ttu-id="603bf-185">Objekt som lagras i hello tabellen returneras av hello Mobile Apps tillbaka slut och hello data visas i hello lista.</span><span class="sxs-lookup"><span data-stu-id="603bf-185">Items that are stored in hello table are returned by hello Mobile Apps back end, and hello data is displayed in hello list.</span></span>
    
    ![][12]
    
    > [!NOTE]
    > <span data-ttu-id="603bf-186">Du hittar hello koden som ansluter till din Mobile Apps-serverdel i hello TodoItemManager.cs C#-filen hello portabla klassbiblioteksprojektet i lösningen.</span><span class="sxs-lookup"><span data-stu-id="603bf-186">You'll find hello code that accesses your Mobile Apps back end in hello TodoItemManager.cs C# file of hello portable class library project of your solution.</span></span>
    >
    >

## <a name="next-steps"></a><span data-ttu-id="603bf-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="603bf-187">Next steps</span></span>

* [<span data-ttu-id="603bf-188">Lägg till autentisering tooyour app</span><span class="sxs-lookup"><span data-stu-id="603bf-188">Add authentication tooyour app</span></span>](app-service-mobile-xamarin-forms-get-started-users.md)  
  <span data-ttu-id="603bf-189">Lär dig hur tooauthenticate användare i appen med en identitetsprovider.</span><span class="sxs-lookup"><span data-stu-id="603bf-189">Learn how tooauthenticate users of your app with an identity provider.</span></span>

* [<span data-ttu-id="603bf-190">Lägg till push-meddelanden tooyour app</span><span class="sxs-lookup"><span data-stu-id="603bf-190">Add push notifications tooyour app</span></span>](app-service-mobile-xamarin-forms-get-started-push.md)  
  <span data-ttu-id="603bf-191">Lär dig hur tooadd push-meddelanden stöder tooyour app och konfigurera din Mobile Apps serverdel toouse Azure Notification Hubs toosend hello push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="603bf-191">Learn how tooadd push notifications support tooyour app and configure your Mobile Apps back end toouse Azure Notification Hubs toosend hello push notifications.</span></span>

* [<span data-ttu-id="603bf-192">Aktivera offlinesynkronisering av appen</span><span class="sxs-lookup"><span data-stu-id="603bf-192">Enable offline sync for your app</span></span>](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  <span data-ttu-id="603bf-193">Lär dig hur tooadd offline stöd för din app genom att använda en Mobile Apps-serverdel.</span><span class="sxs-lookup"><span data-stu-id="603bf-193">Learn how tooadd offline support for your app by using a Mobile Apps back end.</span></span> <span data-ttu-id="603bf-194">Med offlinesynkronisering kan du visa, lägga till eller ändra mobilappens data, även när det inte finns någon nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="603bf-194">With offline sync, you can view, add, or modify your mobile app's data, even when there is no network connection.</span></span>

* [<span data-ttu-id="603bf-195">Använd hello hanterad klient för Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="603bf-195">Use hello managed client for Mobile Apps</span></span>](app-service-mobile-dotnet-how-to-use-client-library.md)  
  <span data-ttu-id="603bf-196">Lär dig hur toowork med hello hanterade klient-SDK i Xamarin-app.</span><span class="sxs-lookup"><span data-stu-id="603bf-196">Learn how toowork with hello managed client SDK in your Xamarin app.</span></span>

<!-- Anchors. -->
[Get started with Mobile Apps back ends]:#getting-started
[Create a new Mobile Apps back end]:#create-new-service
[Next steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure-portalen]: https://portal.azure.com/
