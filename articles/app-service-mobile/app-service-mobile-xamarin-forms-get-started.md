---
title: "Kom igång med Mobile Apps med hjälp av Xamarin.Forms"
description: "I den här kursen får du hjälp att komma igång med Mobile Apps för Xamarin.Forms-utveckling"
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
ms.openlocfilehash: ee12caaad4095cff6dae3282f747ae804f93db81
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-xamarinforms-app"></a><span data-ttu-id="0ae83-103">Skapa en Xamarin.Forms-app</span><span class="sxs-lookup"><span data-stu-id="0ae83-103">Create a Xamarin.Forms app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="0ae83-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="0ae83-104">Overview</span></span>
<span data-ttu-id="0ae83-105">I den här kursen får du lära dig hur du lägger till en molnbaserad serverdelstjänst i en Xamarin.Forms-mobilapp med hjälp av funktionen Mobile Apps i Azure App Service som serverdel.</span><span class="sxs-lookup"><span data-stu-id="0ae83-105">This tutorial shows you how to add a cloud-based back-end service to a Xamarin.Forms mobile app by using the Mobile Apps feature of Azure App Service as the back end.</span></span> <span data-ttu-id="0ae83-106">Du skapar både en ny Mobile Apps-serverdel och en enkel ”todo list”-app med Xamarin.Forms där appdata lagras i Azure.</span><span class="sxs-lookup"><span data-stu-id="0ae83-106">You create both a new Mobile Apps back end and a simple to-do list Xamarin.Forms app that stores app data in Azure.</span></span>

<span data-ttu-id="0ae83-107">Du måste slutföra den här kursen innan du börjar någon annan kurs om Mobilappar för Xamarin.Forms-appar.</span><span class="sxs-lookup"><span data-stu-id="0ae83-107">Completing this tutorial is a prerequisite for all other Mobile Apps tutorials for Xamarin.Forms.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0ae83-108">Krav</span><span class="sxs-lookup"><span data-stu-id="0ae83-108">Prerequisites</span></span>
<span data-ttu-id="0ae83-109">För att kunna genomföra den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="0ae83-109">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="0ae83-110">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="0ae83-110">An active Azure account.</span></span> <span data-ttu-id="0ae83-111">Om du inte har ett konto kan du registrera dig för en utvärderingsversion av Azure och få upp till tio mobilappar utan kostnad som du kan fortsätta att använda även efter utvärderingsperiodens slut.</span><span class="sxs-lookup"><span data-stu-id="0ae83-111">If you don't have an account, you can sign up for an Azure trial and get up to 10 free mobile apps that you can keep using even after your trial ends.</span></span> <span data-ttu-id="0ae83-112">Mer information finns i [Kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0ae83-112">For more information, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="0ae83-113">Visual Studio med Xamarin.</span><span class="sxs-lookup"><span data-stu-id="0ae83-113">Visual Studio with Xamarin.</span></span> <span data-ttu-id="0ae83-114">Mer information finns på sidan för [installation och konfiguration av Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span><span class="sxs-lookup"><span data-stu-id="0ae83-114">For information, see the [Set up and install Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) page.</span></span>

* <span data-ttu-id="0ae83-115">En Mac med Xcode v7.0 eller senare och Xamarin Studio Community installerat.</span><span class="sxs-lookup"><span data-stu-id="0ae83-115">A Mac with Xcode v7.0 or later and Xamarin Studio Community installed.</span></span> <span data-ttu-id="0ae83-116">Information finns på sidan för [installation och konfiguration av Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) och på sidan för [konfiguration, installation och verifiering för Mac-användare](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).</span><span class="sxs-lookup"><span data-stu-id="0ae83-116">For information, see [Set up and install Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) and [Set up, install, and verify for Mac users](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).</span></span>

## <a name="create-a-new-mobile-apps-back-end"></a><span data-ttu-id="0ae83-117">Skapa en ny Mobile Apps-serverdel</span><span class="sxs-lookup"><span data-stu-id="0ae83-117">Create a new Mobile Apps back end</span></span>

<span data-ttu-id="0ae83-118">Så här skapar du en ny Mobile Apps-serverdel:</span><span class="sxs-lookup"><span data-stu-id="0ae83-118">To create a new Mobile Apps back end, do the following:</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

<span data-ttu-id="0ae83-119">Nu har du konfigurerat en Mobile Apps-serverdel som du kan använda för dina mobilklientprogram.</span><span class="sxs-lookup"><span data-stu-id="0ae83-119">You have now set up a Mobile Apps back end that your mobile client applications can use.</span></span> <span data-ttu-id="0ae83-120">I nästa steg får du ladda ned ett serverprojekt för en enkel ”todo list”-serverdel och sedan publicera den på Azure.</span><span class="sxs-lookup"><span data-stu-id="0ae83-120">Next, you download a server project for a simple to-do list back end and then publish it to Azure.</span></span>

## <a name="configure-the-server-project"></a><span data-ttu-id="0ae83-121">Konfigurera serverprojektet</span><span class="sxs-lookup"><span data-stu-id="0ae83-121">Configure the server project</span></span>

<span data-ttu-id="0ae83-122">Så här konfigurerar du serverprojektet för att använda antingen Node.js- eller .NET-serverdelen:</span><span class="sxs-lookup"><span data-stu-id="0ae83-122">To configure the server project to use either the Node.js or .NET back end, do the following:</span></span>

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinforms-solution"></a><span data-ttu-id="0ae83-123">Hämta och kör Xamarin.Forms-lösningen</span><span class="sxs-lookup"><span data-stu-id="0ae83-123">Download and run the Xamarin.Forms solution</span></span>

<span data-ttu-id="0ae83-124">Du kan ladda ned lösningen på två sätt.</span><span class="sxs-lookup"><span data-stu-id="0ae83-124">You can download the solution in either of two ways.</span></span> <span data-ttu-id="0ae83-125">Du kan ladda ned lösningen till en Mac och öppna den i Xamarin Studio eller så kan du ladda ned den till en dator med Windows och öppna den i Visual Studio genom en nätverksansluten Mac och sedan bygga iOS-appen där.</span><span class="sxs-lookup"><span data-stu-id="0ae83-125">Download it to a Mac and open it in Xamarin Studio, or download it to a Windows computer and open it in Visual Studio by using a networked Mac for building the iOS app.</span></span> <span data-ttu-id="0ae83-126">Mer information finns på sidan för att [installera och konfigurera Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span><span class="sxs-lookup"><span data-stu-id="0ae83-126">For more information, see [Set up and install Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span></span>

<span data-ttu-id="0ae83-127">Gör följande på en Mac- eller Windows-dator:</span><span class="sxs-lookup"><span data-stu-id="0ae83-127">On a Mac or Windows computer, do the following:</span></span>

1. <span data-ttu-id="0ae83-128">Gå till [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="0ae83-128">Go to the [Azure portal].</span></span>

2. <span data-ttu-id="0ae83-129">Välj **Kom igång** > **Xamarin.Forms** under **Mobilt** på bladet **Inställningar** för din mobilapp.</span><span class="sxs-lookup"><span data-stu-id="0ae83-129">On the **Settings** blade for your mobile app, under **Mobile**, select **Get Started** > **Xamarin.Forms**.</span></span> <span data-ttu-id="0ae83-130">Välj **Skapa en ny app** under **steg 3** och välj sedan **Ladda ned**.</span><span class="sxs-lookup"><span data-stu-id="0ae83-130">Under **step 3**, select  **Create a new app**, and then select **Download**.</span></span>

   <span data-ttu-id="0ae83-131">Ett projekt laddas ned med ett klientprogram som är kopplat till din mobilapp.</span><span class="sxs-lookup"><span data-stu-id="0ae83-131">This action downloads a project that contains a client application that's connected to your mobile app.</span></span> <span data-ttu-id="0ae83-132">Spara den komprimerade projektfilen lokalt på datorn och notera var du sparar den.</span><span class="sxs-lookup"><span data-stu-id="0ae83-132">Save the compressed project file to your local computer, and make a note of where you save it.</span></span>

3. <span data-ttu-id="0ae83-133">Extrahera projektet som du laddade ned och öppna det i Xamarin Studio (Mac) eller Visual Studio (Windows).</span><span class="sxs-lookup"><span data-stu-id="0ae83-133">Extract the project that you downloaded, and then open it in Xamarin Studio (Mac) or Visual Studio (Windows).</span></span>

   ![Extraherat projekt i Xamarin Studio][9]

   ![Extraherat projekt i Visual Studio][8]

## <a name="optional-run-the-ios-project"></a><span data-ttu-id="0ae83-136">(Valfritt) Kör iOS-projektet</span><span class="sxs-lookup"><span data-stu-id="0ae83-136">(Optional) Run the iOS project</span></span>
<span data-ttu-id="0ae83-137">I det här avsnittet kör du Xamarin iOS-projektet för iOS-enheter.</span><span class="sxs-lookup"><span data-stu-id="0ae83-137">In this section, you run the Xamarin iOS project for iOS devices.</span></span> <span data-ttu-id="0ae83-138">Du kan hoppa över det här avsnittet om du inte arbetar med iOS-enheter.</span><span class="sxs-lookup"><span data-stu-id="0ae83-138">You can skip this section if you are not working with iOS devices.</span></span>

#### <a name="in-xamarin-studio"></a><span data-ttu-id="0ae83-139">I Xamarin Studio</span><span class="sxs-lookup"><span data-stu-id="0ae83-139">In Xamarin Studio</span></span>
1. <span data-ttu-id="0ae83-140">Högerklicka på iOS-projektet och klicka sedan på **Set As Startup Project** (Ange som startprojekt).</span><span class="sxs-lookup"><span data-stu-id="0ae83-140">Right-click the iOS project, and then select **Set As Startup Project**.</span></span>

2. <span data-ttu-id="0ae83-141">Gå till menyn **Run** (Kör) och välj **Start Debugging** (Starta felsökning) för att bygga projektet och starta appen i iPhone-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="0ae83-141">On the **Run** menu, select **Start Debugging** to build the project and start the app in the iPhone emulator.</span></span>

#### <a name="in-visual-studio"></a><span data-ttu-id="0ae83-142">I Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="0ae83-142">In Visual Studio</span></span>
1. <span data-ttu-id="0ae83-143">Högerklicka på iOS-projektet och välj sedan **Set As Startup Project** (Ange som startprojekt).</span><span class="sxs-lookup"><span data-stu-id="0ae83-143">Right-click the iOS project, and then select **Set as StartUp Project**.</span></span>

2. <span data-ttu-id="0ae83-144">Gå till menyn **Build** (Skapa) och välj **Configuration Manager** (Konfigurationshanteraren).</span><span class="sxs-lookup"><span data-stu-id="0ae83-144">On the **Build** menu, select **Configuration Manager**.</span></span>

3. <span data-ttu-id="0ae83-145">I dialogrutan **Configuration Manager** (Konfigurationshanteraren) markerar du kryssrutorna **Build** (Skapa) och **Deploy** (Distribuera) bredvid iOS-projektet.</span><span class="sxs-lookup"><span data-stu-id="0ae83-145">In the **Configuration Manager** dialog box, select the **Build** and **Deploy** check boxes next to the iOS project.</span></span>

4. <span data-ttu-id="0ae83-146">Välj **F5** för att skapa projektet och starta appen i iPhone-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="0ae83-146">To build the project and start the app in the iPhone emulator, select the **F5** key.</span></span>

   > [!NOTE]
   > <span data-ttu-id="0ae83-147">Om du får problem med att skapa projektet kan du köra NuGet Package Manager och uppdatera till den senaste versionen av stödpaketen till Xamarin.</span><span class="sxs-lookup"><span data-stu-id="0ae83-147">If you have problems building the project, run the NuGet package manager and update to the latest version of the Xamarin support packages.</span></span> <span data-ttu-id="0ae83-148">Snabbstartsprojekt kan vara långsamma att uppdatera till de senaste versionerna.</span><span class="sxs-lookup"><span data-stu-id="0ae83-148">Quickstart projects might be slow to update to the latest versions.</span></span>    
   >
   >

5. <span data-ttu-id="0ae83-149">Ange en beskrivande text i appen, till exempel *Läs om Xamarin* och välj sedan plustecknet (**+**).</span><span class="sxs-lookup"><span data-stu-id="0ae83-149">In the app, type meaningful text, such as *Learn Xamarin*, and then select the plus sign (**+**).</span></span>

    ![][10]

    <span data-ttu-id="0ae83-150">En post-begäran skickas till den nya Mobile Apps-serverdelen som finns på Azure.</span><span class="sxs-lookup"><span data-stu-id="0ae83-150">This action sends a post request to the new Mobile Apps back end that's hosted in Azure.</span></span> <span data-ttu-id="0ae83-151">Data från begäran infogas i tabellen TodoItem.</span><span class="sxs-lookup"><span data-stu-id="0ae83-151">Data from the request is inserted into the TodoItem table.</span></span> <span data-ttu-id="0ae83-152">Objekt som lagras i tabellen returneras av Mobile Apps-serverdelen och data visas i listan.</span><span class="sxs-lookup"><span data-stu-id="0ae83-152">Items that are stored in the table are returned by the Mobile Apps back end, and the data is displayed in the list.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0ae83-153">Du hittar koden som ansluter till Mobile Apps-serverdelen i C#-filen TodoItemManager.cs i det portabla klassbiblioteksprojektet i lösningen.</span><span class="sxs-lookup"><span data-stu-id="0ae83-153">You'll find the code that accesses your Mobile Apps back end in the TodoItemManager.cs C# file of the portable class library project of your solution.</span></span>
    >
    >

## <a name="optional-run-the-android-project"></a><span data-ttu-id="0ae83-154">(Valfritt) Kör Androidprojektet</span><span class="sxs-lookup"><span data-stu-id="0ae83-154">(Optional) Run the Android project</span></span>
<span data-ttu-id="0ae83-155">I det här avsnittet kör du Xamarin droid-projektet för Android.</span><span class="sxs-lookup"><span data-stu-id="0ae83-155">In this section, you run the Xamarin droid project for Android.</span></span> <span data-ttu-id="0ae83-156">Du kan hoppa över det här avsnittet om du inte arbetar med Androidenheter.</span><span class="sxs-lookup"><span data-stu-id="0ae83-156">You can skip this section if you are not working with Android devices.</span></span>

#### <a name="in-xamarin-studio"></a><span data-ttu-id="0ae83-157">I Xamarin Studio</span><span class="sxs-lookup"><span data-stu-id="0ae83-157">In Xamarin Studio</span></span>

1. <span data-ttu-id="0ae83-158">Högerklicka på Android-projektet och välj sedan **Set As Startup Project** (Ange som startprojekt).</span><span class="sxs-lookup"><span data-stu-id="0ae83-158">Right-click the Android project, and then select **Set As Startup Project**.</span></span>

2. <span data-ttu-id="0ae83-159">Gå till menyn **Run** (Kör) och välj **Start Debugging** (Starta felsökning) för att skapa projektet och starta appen i en Android-emulator.</span><span class="sxs-lookup"><span data-stu-id="0ae83-159">To build the project and start the app in an Android emulator, on the **Run** menu, select **Start Debugging**.</span></span>

#### <a name="in-visual-studio"></a><span data-ttu-id="0ae83-160">I Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="0ae83-160">In Visual Studio</span></span>

1. <span data-ttu-id="0ae83-161">Högerklicka på Android-projektet (Droid) och välj sedan **Set as StartUp Project** (Ange som startprojekt).</span><span class="sxs-lookup"><span data-stu-id="0ae83-161">Right-click the Android (Droid) project, and then select **Set as StartUp Project**.</span></span>

2. <span data-ttu-id="0ae83-162">Gå till menyn **Build** (Skapa) och välj **Configuration Manager** (Konfigurationshanteraren).</span><span class="sxs-lookup"><span data-stu-id="0ae83-162">On the **Build** menu, select **Configuration Manager**.</span></span>

3. <span data-ttu-id="0ae83-163">I dialogrutan **Configuration Manager** (Konfigurationshanteraren) markerar du kryssrutorna **Build** (Skapa) och **Deploy** (Distribuera) bredvid Android-projektet.</span><span class="sxs-lookup"><span data-stu-id="0ae83-163">In the **Configuration Manager** dialog box, select the **Build** and **Deploy** check boxes next to the Android project.</span></span>

4. <span data-ttu-id="0ae83-164">Välj **F5** för att skapa projektet och starta appen i en Android-emulator.</span><span class="sxs-lookup"><span data-stu-id="0ae83-164">To build the project and start the app in an Android emulator, select the **F5** key.</span></span>

   > [!NOTE]
   > <span data-ttu-id="0ae83-165">Om du får problem med att skapa projektet kan du köra NuGet Package Manager och uppdatera till den senaste versionen av stödpaketen till Xamarin.</span><span class="sxs-lookup"><span data-stu-id="0ae83-165">If you have problems building the project, run the NuGet package manager and update to the latest version of the Xamarin support packages.</span></span> <span data-ttu-id="0ae83-166">Snabbstartsprojekt kan vara långsamma att uppdatera till de senaste versionerna.</span><span class="sxs-lookup"><span data-stu-id="0ae83-166">Quickstart projects might be slow to update to the latest versions.</span></span>    
   >
   >

5. <span data-ttu-id="0ae83-167">Ange en beskrivande text i appen, till exempel *Läs om Xamarin* och välj sedan plustecknet (**+**).</span><span class="sxs-lookup"><span data-stu-id="0ae83-167">In the app, type meaningful text, such as *Learn Xamarin*, and then select the plus sign (**+**).</span></span>

    ![][11]
    
    <span data-ttu-id="0ae83-168">En post-begäran skickas till den nya Mobile Apps-serverdelen som finns på Azure.</span><span class="sxs-lookup"><span data-stu-id="0ae83-168">This action sends a post request to the new Mobile Apps back end that's hosted in Azure.</span></span> <span data-ttu-id="0ae83-169">Data från begäran infogas i tabellen TodoItem.</span><span class="sxs-lookup"><span data-stu-id="0ae83-169">Data from the request is inserted into the TodoItem table.</span></span> <span data-ttu-id="0ae83-170">Objekt som lagras i tabellen returneras av Mobile Apps-serverdelen och data visas i listan.</span><span class="sxs-lookup"><span data-stu-id="0ae83-170">Items that are stored in the table are returned by the Mobile Apps back end, and the data is displayed in the list.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="0ae83-171">Du hittar koden som ansluter till Mobile Apps-serverdelen i C#-filen TodoItemManager.cs i det portabla klassbiblioteksprojektet i lösningen.</span><span class="sxs-lookup"><span data-stu-id="0ae83-171">You'll find the code that accesses your Mobile Apps back end in the TodoItemManager.cs C# file of the portable class library project of your solution.</span></span>
    >
    >

## <a name="optional-run-the-windows-project"></a><span data-ttu-id="0ae83-172">(Valfritt) Kör Windowsprojektet</span><span class="sxs-lookup"><span data-stu-id="0ae83-172">(Optional) Run the Windows project</span></span>

<span data-ttu-id="0ae83-173">I det här avsnittet kör du Xamarin WinApp-projektet för Windowsenheter.</span><span class="sxs-lookup"><span data-stu-id="0ae83-173">In this section, you run the Xamarin WinApp project for Windows devices.</span></span> <span data-ttu-id="0ae83-174">Du kan hoppa över det här avsnittet om du inte arbetar med Windowsenheter.</span><span class="sxs-lookup"><span data-stu-id="0ae83-174">You can skip this section if you are not working with Windows devices.</span></span>

#### <a name="in-visual-studio"></a><span data-ttu-id="0ae83-175">I Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="0ae83-175">In Visual Studio</span></span>

1. <span data-ttu-id="0ae83-176">Högerklicka på något av Windows-projekten och välj sedan **Set as StartUp Project** (Ange som startprojekt).</span><span class="sxs-lookup"><span data-stu-id="0ae83-176">Right-click any of the Windows projects, and then select **Set as StartUp Project**.</span></span>

2. <span data-ttu-id="0ae83-177">Gå till menyn **Build** (Skapa) och välj **Configuration Manager** (Konfigurationshanteraren).</span><span class="sxs-lookup"><span data-stu-id="0ae83-177">On the **Build** menu, select **Configuration Manager**.</span></span>

3. <span data-ttu-id="0ae83-178">I dialogrutan **Configuration Manager** (Konfigurationshanteraren) markerar du kryssrutorna **Build** (Skapa) och **Deploy** (Distribuera) bredvid Windows-projektet du valde.</span><span class="sxs-lookup"><span data-stu-id="0ae83-178">In the **Configuration Manager** dialog box, select the **Build** and **Deploy** check boxes next to the Windows project that you chose.</span></span>

4. <span data-ttu-id="0ae83-179">Välj **F5** för att skapa projektet och starta appen i en Windows-emulator.</span><span class="sxs-lookup"><span data-stu-id="0ae83-179">To build the project and start the app in a Windows emulator, select the **F5** key.</span></span>

   > [!NOTE]
   > <span data-ttu-id="0ae83-180">Om du får problem med att skapa projektet kan du köra NuGet Package Manager och uppdatera till den senaste versionen av stödpaketen till Xamarin.</span><span class="sxs-lookup"><span data-stu-id="0ae83-180">If you have problems building the project, run the NuGet package manager and update to the latest version of the Xamarin support packages.</span></span> <span data-ttu-id="0ae83-181">Snabbstartsprojekt kan vara långsamma att uppdatera till de senaste versionerna.</span><span class="sxs-lookup"><span data-stu-id="0ae83-181">Quickstart projects might be slow to update to the latest versions.</span></span>    
   >
   >

5. <span data-ttu-id="0ae83-182">Ange en beskrivande text i appen, till exempel *Läs om Xamarin* och välj sedan plustecknet (**+**).</span><span class="sxs-lookup"><span data-stu-id="0ae83-182">In the app, type meaningful text, such as *Learn Xamarin*, and then select the plus sign (**+**).</span></span>

    <span data-ttu-id="0ae83-183">En post-begäran skickas till den nya Mobile Apps-serverdelen som finns på Azure.</span><span class="sxs-lookup"><span data-stu-id="0ae83-183">This action sends a post request to the new Mobile Apps back end that's hosted in Azure.</span></span> <span data-ttu-id="0ae83-184">Data från begäran infogas i tabellen TodoItem.</span><span class="sxs-lookup"><span data-stu-id="0ae83-184">Data from the request is inserted into the TodoItem table.</span></span> <span data-ttu-id="0ae83-185">Objekt som lagras i tabellen returneras av Mobile Apps-serverdelen och data visas i listan.</span><span class="sxs-lookup"><span data-stu-id="0ae83-185">Items that are stored in the table are returned by the Mobile Apps back end, and the data is displayed in the list.</span></span>
    
    ![][12]
    
    > [!NOTE]
    > <span data-ttu-id="0ae83-186">Du hittar koden som ansluter till Mobile Apps-serverdelen i C#-filen TodoItemManager.cs i det portabla klassbiblioteksprojektet i lösningen.</span><span class="sxs-lookup"><span data-stu-id="0ae83-186">You'll find the code that accesses your Mobile Apps back end in the TodoItemManager.cs C# file of the portable class library project of your solution.</span></span>
    >
    >

## <a name="next-steps"></a><span data-ttu-id="0ae83-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0ae83-187">Next steps</span></span>

* [<span data-ttu-id="0ae83-188">Lägg till autentisering i appen</span><span class="sxs-lookup"><span data-stu-id="0ae83-188">Add authentication to your app</span></span>](app-service-mobile-xamarin-forms-get-started-users.md)  
  <span data-ttu-id="0ae83-189">Läs om hur du autentiserar användare i appen med en identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="0ae83-189">Learn how to authenticate users of your app with an identity provider.</span></span>

* [<span data-ttu-id="0ae83-190">Lägg till push-meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="0ae83-190">Add push notifications to your app</span></span>](app-service-mobile-xamarin-forms-get-started-push.md)  
  <span data-ttu-id="0ae83-191">Läs om hur du lägger till stöd för push-meddelanden i appen och konfigurerar Mobile Apps-serverdelen så att Azure Notification Hubs används för att skicka push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="0ae83-191">Learn how to add push notifications support to your app and configure your Mobile Apps back end to use Azure Notification Hubs to send the push notifications.</span></span>

* [<span data-ttu-id="0ae83-192">Aktivera offlinesynkronisering av appen</span><span class="sxs-lookup"><span data-stu-id="0ae83-192">Enable offline sync for your app</span></span>](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  <span data-ttu-id="0ae83-193">Läs om hur du lägger till offlinestöd i appen genom en Mobile Apps-serverdel.</span><span class="sxs-lookup"><span data-stu-id="0ae83-193">Learn how to add offline support for your app by using a Mobile Apps back end.</span></span> <span data-ttu-id="0ae83-194">Med offlinesynkronisering kan du visa, lägga till eller ändra mobilappens data, även när det inte finns någon nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="0ae83-194">With offline sync, you can view, add, or modify your mobile app's data, even when there is no network connection.</span></span>

* [<span data-ttu-id="0ae83-195">Använda den hanterade klienten för Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="0ae83-195">Use the managed client for Mobile Apps</span></span>](app-service-mobile-dotnet-how-to-use-client-library.md)  
  <span data-ttu-id="0ae83-196">Läs om hur du arbetar med den hanterade klient-SDK:n i Xamarin-appen.</span><span class="sxs-lookup"><span data-stu-id="0ae83-196">Learn how to work with the managed client SDK in your Xamarin app.</span></span>

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
