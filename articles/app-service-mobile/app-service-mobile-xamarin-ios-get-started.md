---
title: "Kom igång med Azure Apptjänst Mobile Apps för Xamarin.iOS-appar | Microsoft Docs"
description: "I den här kursen får du hjälp att komma igång med Mobile Apps för Xamarin.iOS-utveckling."
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
ms.openlocfilehash: 8dc965df2cd45366970effb29f246b0045a94717
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-xamarinios-app"></a><span data-ttu-id="59123-103">Skapa en Xamarin.iOS-app</span><span class="sxs-lookup"><span data-stu-id="59123-103">Create a Xamarin.iOS app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="59123-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="59123-104">Overview</span></span>
<span data-ttu-id="59123-105">I den här kursen får du lära dig hur du lägger till en molnbaserad serverdelstjänst i en Xamarin.iOS-mobilapp med hjälp av en mobilappsserverdel i Azure.</span><span class="sxs-lookup"><span data-stu-id="59123-105">This tutorial shows you how to add a cloud-based backend service to a Xamarin.iOS mobile app by using an Azure mobile app backend.</span></span>  <span data-ttu-id="59123-106">Du skapar både en ny mobilapp-serverdel och en enkel *Att göra-lista* Xamarin.iOS-app som lagrar app-data i Azure.</span><span class="sxs-lookup"><span data-stu-id="59123-106">You create both a new mobile app backend and a simple *Todo list* Xamarin.iOS app that stores app data in Azure.</span></span>

<span data-ttu-id="59123-107">Du måste slutföra den här kursen innan du påbörjar någon annan Xamarin.iOS-kurs om att använda funktionen Mobile Apps i Azure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="59123-107">Completing this tutorial is a prerequisite for all other Xamarin.iOS tutorials about using the Mobile Apps feature in Azure App Service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="59123-108">Krav</span><span class="sxs-lookup"><span data-stu-id="59123-108">Prerequisites</span></span>
<span data-ttu-id="59123-109">För att slutföra den här självstudien, finns följande förhandskrav:</span><span class="sxs-lookup"><span data-stu-id="59123-109">To complete this tutorial, you need the following prerequisites:</span></span>

* <span data-ttu-id="59123-110">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="59123-110">An active Azure account.</span></span> <span data-ttu-id="59123-111">Om du inte har ett konto kan du registrera dig för en utvärderingsversion av Azure och få upp till tio mobilappar utan kostnad som du kan fortsätta att använda även efter utvärderingsperiodens slut.</span><span class="sxs-lookup"><span data-stu-id="59123-111">If you don't have an account, sign up for an Azure trial and get up to 10 free mobile apps that you can keep using even after your trial ends.</span></span> <span data-ttu-id="59123-112">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="59123-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="59123-113">Visual Studio med Xamarin.</span><span class="sxs-lookup"><span data-stu-id="59123-113">Visual Studio with Xamarin.</span></span> <span data-ttu-id="59123-114">Instruktioner finns i avsnittet om [konfiguration och installation av Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span><span class="sxs-lookup"><span data-stu-id="59123-114">See [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) for instructions.</span></span>
* <span data-ttu-id="59123-115">En Mac med Xcode v7.0 eller senare och Xamarin Studio Community installerat.</span><span class="sxs-lookup"><span data-stu-id="59123-115">A Mac with Xcode v7.0 or later and Xamarin Studio Community installed.</span></span> <span data-ttu-id="59123-116">Se avsnittet om [konfiguration och installation av Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) och om [konfiguration, installation och verifieringar för Mac-användare](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).</span><span class="sxs-lookup"><span data-stu-id="59123-116">See [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) and [Setup, install, and verifications for Mac users](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).</span></span>

## <a name="create-an-azure-mobile-app-backend"></a><span data-ttu-id="59123-117">Skapa en mobilapp-serverdel i Azure</span><span class="sxs-lookup"><span data-stu-id="59123-117">Create an Azure Mobile App backend</span></span>
<span data-ttu-id="59123-118">Följ de här stegen för att skapa en mobilapp-serverdel.</span><span class="sxs-lookup"><span data-stu-id="59123-118">Follow these steps to create a Mobile App backend.</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a><span data-ttu-id="59123-119">Konfigurera serverprojektet</span><span class="sxs-lookup"><span data-stu-id="59123-119">Configure the server project</span></span>
<span data-ttu-id="59123-120">Du har nu skapat en mobilsappsserverdel i Azure som kan användas av dina mobilklientprogram.</span><span class="sxs-lookup"><span data-stu-id="59123-120">You have now provisioned an Azure Mobile App backend that can be used by your mobile client applications.</span></span> <span data-ttu-id="59123-121">Därefter, hämtar du ett serverprojekt för en enkel ”att göra lista”-serverdel och publicerar den till Azure.</span><span class="sxs-lookup"><span data-stu-id="59123-121">Next, download a server project for a simple "todo list" backend and publish it to Azure.</span></span>

<span data-ttu-id="59123-122">Följ följande steg för att konfigurera serverprojektet att använda sig av antingen Node.js- eller .NET-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="59123-122">Follow the following steps to configure the server project to use either the Node.js or .NET backend.</span></span>

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinios-app"></a><span data-ttu-id="59123-123">Hämta och kör Xamarin.iOS-appen</span><span class="sxs-lookup"><span data-stu-id="59123-123">Download and run the Xamarin.iOS app</span></span>
1. <span data-ttu-id="59123-124">Öppna [Azure-portalen] i ett webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="59123-124">Open the [Azure portal] in a browser window.</span></span>
2. <span data-ttu-id="59123-125">På inställningsbladet för mobilappen klickar du på **Kom igång** > **Xamarin.iOS**.</span><span class="sxs-lookup"><span data-stu-id="59123-125">On the settings blade for your Mobile App, click **Get Started** > **Xamarin.iOS**.</span></span> <span data-ttu-id="59123-126">Vid steg 3, klickar du på **Skapa en ny app** om det inte redan är valt.</span><span class="sxs-lookup"><span data-stu-id="59123-126">Under step 3, click **Create a new app** if it's not already selected.</span></span>  <span data-ttu-id="59123-127">Klicka sedan på **Hämta**.</span><span class="sxs-lookup"><span data-stu-id="59123-127">Next click the **Download** button.</span></span>

      <span data-ttu-id="59123-128">Ett klientprogram som ansluter till din mobila serverdel hämtas.</span><span class="sxs-lookup"><span data-stu-id="59123-128">A client application that connects to your mobile backend is downloaded.</span></span> <span data-ttu-id="59123-129">Spara den komprimerade projektfilen lokalt på datorn och notera var du sparar den.</span><span class="sxs-lookup"><span data-stu-id="59123-129">Save the compressed project file to your local computer, and make a note of where you save it.</span></span>
3. <span data-ttu-id="59123-130">Extrahera projektet som du laddade ned och öppna det i Xamarin Studio (eller Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="59123-130">Extract the project that you downloaded, and then open it in Xamarin Studio (or Visual Studio).</span></span>

    ![][9]

    ![][8]
4. <span data-ttu-id="59123-131">Tryck på F5 för att skapa projektet och starta appen i iPhone-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="59123-131">Press the F5 key to build the project and start the app in the iPhone emulator.</span></span>
5. <span data-ttu-id="59123-132">Ange en beskrivande text i appen, till exempel  *Läs om Xamarin*, och klicka sedan på **+**.</span><span class="sxs-lookup"><span data-stu-id="59123-132">In the app, type meaningful text, such as *Learn Xamarin*, and then click the **+** button.</span></span>

    ![][10]

    <span data-ttu-id="59123-133">Data från begäran infogas i tabellen TodoItem.</span><span class="sxs-lookup"><span data-stu-id="59123-133">Data from the request is inserted into the TodoItem table.</span></span> <span data-ttu-id="59123-134">Objekt som lagras i tabellen returneras av mobilappsserverdelen och data visas i listan.</span><span class="sxs-lookup"><span data-stu-id="59123-134">Items stored in the table are returned by the mobile app backend, and the data is displayed in the list.</span></span>

> [!NOTE]
> <span data-ttu-id="59123-135">Du kan se koden som ansluter till mobilappsserverdelen för att fråga efter och infoga data i C#-filen QSTodoService.cs.</span><span class="sxs-lookup"><span data-stu-id="59123-135">You can review the code that accesses your mobile app backend to query and insert data in the QSTodoService.cs C# file.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="59123-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="59123-136">Next steps</span></span>
* [<span data-ttu-id="59123-137">Lägg till offlinesynkronisering till din app</span><span class="sxs-lookup"><span data-stu-id="59123-137">Add Offline Sync to your app</span></span>](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [<span data-ttu-id="59123-138">Lägg till autentisering till din app </span><span class="sxs-lookup"><span data-stu-id="59123-138">Add authentication to your app </span></span>](app-service-mobile-xamarin-ios-get-started-users.md)
* [<span data-ttu-id="59123-139">Lägg till push-meddelanden till din Xamarin.Android-app</span><span class="sxs-lookup"><span data-stu-id="59123-139">Add push notifications to your Xamarin.Android app</span></span>](app-service-mobile-xamarin-ios-get-started-push.md)
* [<span data-ttu-id="59123-140">Så här använder du den hanterade klienten för Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="59123-140">How to use the managed client for Azure Mobile Apps</span></span>](app-service-mobile-dotnet-how-to-use-client-library.md)

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
<span data-ttu-id="59123-141">[Azure-portalen]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="59123-141">[Azure portal]: https://portal.azure.com/</span></span>
