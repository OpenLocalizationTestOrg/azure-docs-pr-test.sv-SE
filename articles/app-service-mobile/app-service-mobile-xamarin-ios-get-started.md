---
title: "aaaGet igång med Azure Apptjänst Mobilappar för Xamarin.iOS-appar | Microsoft Docs"
description: "Följ den här självstudiekursen tooget igång med Mobilappar för Xamarin.iOS-utveckling."
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 14428794-52ad-4b51-956c-deb296cafa34
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: syntaxc4
ms.openlocfilehash: 524c5ac4d8a29d7cb858f74132aad5d6e2201d02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-xamarinios-app"></a><span data-ttu-id="82e51-103">Skapa en Xamarin.iOS-app</span><span class="sxs-lookup"><span data-stu-id="82e51-103">Create a Xamarin.iOS app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="82e51-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="82e51-104">Overview</span></span>
<span data-ttu-id="82e51-105">Den här kursen visar hur tooadd en molnbaserad serverdel tjänsten tooa Xamarin.iOS-mobilapp med hjälp av en mobilappsserverdel i Azure.</span><span class="sxs-lookup"><span data-stu-id="82e51-105">This tutorial shows you how tooadd a cloud-based backend service tooa Xamarin.iOS mobile app by using an Azure mobile app backend.</span></span>  <span data-ttu-id="82e51-106">Du skapar både en ny mobilapp-serverdel och en enkel *Att göra-lista* Xamarin.iOS-app som lagrar app-data i Azure.</span><span class="sxs-lookup"><span data-stu-id="82e51-106">You create both a new mobile app backend and a simple *Todo list* Xamarin.iOS app that stores app data in Azure.</span></span>

<span data-ttu-id="82e51-107">Den här kursen är en förutsättning för alla andra Xamarin.iOS-kurs om hur du använder funktionen för hello Mobile Apps i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="82e51-107">Completing this tutorial is a prerequisite for all other Xamarin.iOS tutorials about using hello Mobile Apps feature in Azure App Service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="82e51-108">Krav</span><span class="sxs-lookup"><span data-stu-id="82e51-108">Prerequisites</span></span>
<span data-ttu-id="82e51-109">toocomplete den här kursen behöver du hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="82e51-109">toocomplete this tutorial, you need hello following prerequisites:</span></span>

* <span data-ttu-id="82e51-110">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="82e51-110">An active Azure account.</span></span> <span data-ttu-id="82e51-111">Om du inte har ett konto kan registrera dig för en utvärderingsversion av Azure och få upp too10 mobilappar utan kostnad som du kan fortsätta att använda även efter utvärderingsperiodens slut.</span><span class="sxs-lookup"><span data-stu-id="82e51-111">If you don't have an account, sign up for an Azure trial and get up too10 free mobile apps that you can keep using even after your trial ends.</span></span> <span data-ttu-id="82e51-112">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="82e51-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="82e51-113">Visual Studio med Xamarin.</span><span class="sxs-lookup"><span data-stu-id="82e51-113">Visual Studio with Xamarin.</span></span> <span data-ttu-id="82e51-114">Instruktioner finns i avsnittet om [konfiguration och installation av Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span><span class="sxs-lookup"><span data-stu-id="82e51-114">See [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) for instructions.</span></span>
* <span data-ttu-id="82e51-115">En Mac med Xcode v7.0 eller senare och Xamarin Studio Community installerat.</span><span class="sxs-lookup"><span data-stu-id="82e51-115">A Mac with Xcode v7.0 or later and Xamarin Studio Community installed.</span></span> <span data-ttu-id="82e51-116">Se avsnittet om [konfiguration och installation av Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) och om [konfiguration, installation och verifieringar för Mac-användare](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).</span><span class="sxs-lookup"><span data-stu-id="82e51-116">See [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) and [Setup, install, and verifications for Mac users](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).</span></span>

## <a name="create-an-azure-mobile-app-backend"></a><span data-ttu-id="82e51-117">Skapa en mobilapp-serverdel i Azure</span><span class="sxs-lookup"><span data-stu-id="82e51-117">Create an Azure Mobile App backend</span></span>
<span data-ttu-id="82e51-118">Följ dessa steg toocreate en mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="82e51-118">Follow these steps toocreate a Mobile App backend.</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-hello-server-project"></a><span data-ttu-id="82e51-119">Konfigurera hello serverprojekt</span><span class="sxs-lookup"><span data-stu-id="82e51-119">Configure hello server project</span></span>
<span data-ttu-id="82e51-120">Du har nu skapat en mobilsappsserverdel i Azure som kan användas av dina mobilklientprogram.</span><span class="sxs-lookup"><span data-stu-id="82e51-120">You have now provisioned an Azure Mobile App backend that can be used by your mobile client applications.</span></span> <span data-ttu-id="82e51-121">Därefter ladda ned ett serverprojekt för en enkel todo-list ”serverdel och publicera den tooAzure.</span><span class="sxs-lookup"><span data-stu-id="82e51-121">Next, download a server project for a simple "todo list" backend and publish it tooAzure.</span></span>

<span data-ttu-id="82e51-122">Följ följande steg tooconfigure hello server projektet toouse hello antingen hello Node.js eller .NET-serverdel.</span><span class="sxs-lookup"><span data-stu-id="82e51-122">Follow hello following steps tooconfigure hello server project toouse either hello Node.js or .NET backend.</span></span>

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-xamarinios-app"></a><span data-ttu-id="82e51-123">Hämta och köra hello Xamarin.iOS-app</span><span class="sxs-lookup"><span data-stu-id="82e51-123">Download and run hello Xamarin.iOS app</span></span>
1. <span data-ttu-id="82e51-124">Öppna hello [Azure-portalen] i ett webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="82e51-124">Open hello [Azure portal] in a browser window.</span></span>
2. <span data-ttu-id="82e51-125">Klicka på hello inställningsbladet för Mobilappen **Kom igång** > **Xamarin.iOS**.</span><span class="sxs-lookup"><span data-stu-id="82e51-125">On hello settings blade for your Mobile App, click **Get Started** > **Xamarin.iOS**.</span></span> <span data-ttu-id="82e51-126">Vid steg 3, klickar du på **Skapa en ny app** om det inte redan är valt.</span><span class="sxs-lookup"><span data-stu-id="82e51-126">Under step 3, click **Create a new app** if it's not already selected.</span></span>  <span data-ttu-id="82e51-127">Klicka sedan på hello **hämta** knappen.</span><span class="sxs-lookup"><span data-stu-id="82e51-127">Next click hello **Download** button.</span></span>

      <span data-ttu-id="82e51-128">Ett klientprogram som ansluter tooyour mobila serverdel hämtas.</span><span class="sxs-lookup"><span data-stu-id="82e51-128">A client application that connects tooyour mobile backend is downloaded.</span></span> <span data-ttu-id="82e51-129">Spara hello komprimerade projektfilen lokalt på datorn och notera var du sparar den.</span><span class="sxs-lookup"><span data-stu-id="82e51-129">Save hello compressed project file to your local computer, and make a note of where you save it.</span></span>
3. <span data-ttu-id="82e51-130">Extrahera hello-projekt som du laddade ned och öppna det i Xamarin Studio (eller Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="82e51-130">Extract hello project that you downloaded, and then open it in Xamarin Studio (or Visual Studio).</span></span>

    ![][9]

    ![][8]
4. <span data-ttu-id="82e51-131">Tryck på hello F5 viktiga toobuild hello projektet och starta hello app i hello iPhone-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="82e51-131">Press hello F5 key toobuild hello project and start hello app in hello iPhone emulator.</span></span>
5. <span data-ttu-id="82e51-132">I hello app, ange en beskrivande text som *Läs om Xamarin*, och klicka sedan på hello  **+**  knappen.</span><span class="sxs-lookup"><span data-stu-id="82e51-132">In hello app, type meaningful text, such as *Learn Xamarin*, and then click hello **+** button.</span></span>

    ![][10]

    <span data-ttu-id="82e51-133">Data från hello begäran infogas i hello TodoItem-tabell.</span><span class="sxs-lookup"><span data-stu-id="82e51-133">Data from hello request is inserted into hello TodoItem table.</span></span> <span data-ttu-id="82e51-134">Objekt som lagras i hello tabellen returneras av hello mobilappsserverdel och data visas i hello-listan.</span><span class="sxs-lookup"><span data-stu-id="82e51-134">Items stored in hello table are returned by hello mobile app backend, and the data is displayed in hello list.</span></span>

> [!NOTE]
> <span data-ttu-id="82e51-135">Du kan granska hello koden som ansluter till din mobila app backend tooquery och infoga data i hello C#-filen QSTodoService.cs.</span><span class="sxs-lookup"><span data-stu-id="82e51-135">You can review hello code that accesses your mobile app backend tooquery and insert data in hello QSTodoService.cs C# file.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="82e51-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="82e51-136">Next steps</span></span>
* [<span data-ttu-id="82e51-137">Lägg till Offline Sync tooyour app</span><span class="sxs-lookup"><span data-stu-id="82e51-137">Add Offline Sync tooyour app</span></span>](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [<span data-ttu-id="82e51-138">Lägg till autentisering tooyour app</span><span class="sxs-lookup"><span data-stu-id="82e51-138">Add authentication tooyour app </span></span>](app-service-mobile-xamarin-ios-get-started-users.md)
* [<span data-ttu-id="82e51-139">Lägg till push-meddelanden tooyour Xamarin.Android-app</span><span class="sxs-lookup"><span data-stu-id="82e51-139">Add push notifications tooyour Xamarin.Android app</span></span>](app-service-mobile-xamarin-ios-get-started-push.md)
* [<span data-ttu-id="82e51-140">Hur toouse hello hanterade klienten för Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="82e51-140">How toouse hello managed client for Azure Mobile Apps</span></span>](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Azure-portalen]: https://portal.azure.com/
