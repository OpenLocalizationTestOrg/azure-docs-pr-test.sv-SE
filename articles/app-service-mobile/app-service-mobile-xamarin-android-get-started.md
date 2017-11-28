---
title: "aaaGet igång med Azure Mobilappar för Xamarin.Android-appar"
description: "Följ den här självstudiekursen tooget igång med Azure Mobilappar för Xamarin Android-utveckling"
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 81649dd3-544f-40ff-b9b7-60c66d683e60
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 38710660d9328fe3c068efca972f76aa8b6e049b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-xamarinandroid-app"></a><span data-ttu-id="5f11d-103">Skapa en Xamarin.Android-app</span><span class="sxs-lookup"><span data-stu-id="5f11d-103">Create a Xamarin.Android App</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="5f11d-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="5f11d-104">Overview</span></span>
<span data-ttu-id="5f11d-105">Den här kursen visar hur tooadd en molnbaserad serverdel tjänsten tooa Xamarin.Android-app.</span><span class="sxs-lookup"><span data-stu-id="5f11d-105">This tutorial shows you how tooadd a cloud-based backend service tooa Xamarin.Android app.</span></span> <span data-ttu-id="5f11d-106">Mer information om Mobile Apps finns [här](app-service-mobile-value-prop.md).</span><span class="sxs-lookup"><span data-stu-id="5f11d-106">For more information, see [What are Mobile Apps](app-service-mobile-value-prop.md).</span></span>

<span data-ttu-id="5f11d-107">En skärmbild från hello slutförts app understiger:</span><span class="sxs-lookup"><span data-stu-id="5f11d-107">A screenshot from hello completed app is below:</span></span>

![][0]

<span data-ttu-id="5f11d-108">Du måste slutföra den här kursen innan du börjar någon annan kurs om Mobile Apps för Xamarin.Android-appar.</span><span class="sxs-lookup"><span data-stu-id="5f11d-108">Completing this tutorial is a prerequisite for all other Mobile Apps tutorials for Xamarin.Android apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5f11d-109">Krav</span><span class="sxs-lookup"><span data-stu-id="5f11d-109">Prerequisites</span></span>
<span data-ttu-id="5f11d-110">toocomplete den här kursen behöver du hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="5f11d-110">toocomplete this tutorial, you need hello following prerequisites:</span></span>

* <span data-ttu-id="5f11d-111">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="5f11d-111">An active Azure account.</span></span> <span data-ttu-id="5f11d-112">Om du inte har ett konto kan registrera dig för en utvärderingsversion av Azure och få upp too10 ledigt Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="5f11d-112">If you don't have an account, sign up for an Azure trial and get up too10 free Mobile Apps.</span></span> <span data-ttu-id="5f11d-113">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5f11d-113">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="5f11d-114">Visual Studio med Xamarin.</span><span class="sxs-lookup"><span data-stu-id="5f11d-114">Visual Studio with Xamarin.</span></span> <span data-ttu-id="5f11d-115">Instruktioner finns i avsnittet om [konfiguration och installation av Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f11d-115">See [Setup and install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) for instructions.</span></span>

## <a name="create-an-azure-mobile-app-backend"></a><span data-ttu-id="5f11d-116">Skapa en mobilapp-serverdel i Azure</span><span class="sxs-lookup"><span data-stu-id="5f11d-116">Create an Azure Mobile App backend</span></span>
<span data-ttu-id="5f11d-117">Följ dessa steg toocreate en mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="5f11d-117">Follow these steps toocreate a Mobile App backend.</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

<span data-ttu-id="5f11d-118">Du har nu skapat en mobilsappsserverdel i Azure som kan användas av dina mobilklientprogram.</span><span class="sxs-lookup"><span data-stu-id="5f11d-118">You have now provisioned an Azure Mobile App backend that can be used by your mobile client applications.</span></span> <span data-ttu-id="5f11d-119">Därefter ladda ned ett serverprojekt för en enkel todo-list ”serverdel och publicera den tooAzure.</span><span class="sxs-lookup"><span data-stu-id="5f11d-119">Next, download a server project for a simple "todo list" backend and publish it tooAzure.</span></span>

## <a name="configure-hello-server-project"></a><span data-ttu-id="5f11d-120">Konfigurera hello serverprojekt</span><span class="sxs-lookup"><span data-stu-id="5f11d-120">Configure hello server project</span></span>
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-xamarinandroid-app"></a><span data-ttu-id="5f11d-121">Hämta och köra hello Xamarin.Android-app</span><span class="sxs-lookup"><span data-stu-id="5f11d-121">Download and run hello Xamarin.Android app</span></span>
1. <span data-ttu-id="5f11d-122">Under **ladda ned och kör ditt Xamarin.Android-projekt**, klicka på hello **hämta** knappen.</span><span class="sxs-lookup"><span data-stu-id="5f11d-122">Under **Download and run your Xamarin.Android project**, click hello **Download** button.</span></span>

      <span data-ttu-id="5f11d-123">Spara hello komprimerade projektet filen tooyour lokala datorn och notera var du sparar den.</span><span class="sxs-lookup"><span data-stu-id="5f11d-123">Save hello compressed project file tooyour local computer, and make a note of where you save it.</span></span>
2. <span data-ttu-id="5f11d-124">Tryck på hello **F5** nyckeln toobuild hello projektet och starta hello app.</span><span class="sxs-lookup"><span data-stu-id="5f11d-124">Press hello **F5** key toobuild hello project and start hello app.</span></span>
3. <span data-ttu-id="5f11d-125">I hello app, ange en beskrivande text som *fullständig hello kursen* och klicka sedan på hello **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="5f11d-125">In hello app, type meaningful text, such as *Complete hello tutorial* and then click hello **Add** button.</span></span>

    ![][10]

    <span data-ttu-id="5f11d-126">Data från hello begäran infogas i hello TodoItem-tabell.</span><span class="sxs-lookup"><span data-stu-id="5f11d-126">Data from hello request is inserted into hello TodoItem table.</span></span> <span data-ttu-id="5f11d-127">Objekt som lagras i hello tabellen returneras av hello mobilappsserverdel och informationen visas i hello lista.</span><span class="sxs-lookup"><span data-stu-id="5f11d-127">Items stored in hello table are returned by hello mobile app backend, and the data appears in hello list.</span></span>

   > [!NOTE]
   > <span data-ttu-id="5f11d-128">Du kan granska hello koden som ansluter till din mobila app backend tooquery och infoga data som finns i hello C#-filen ToDoActivity.cs.</span><span class="sxs-lookup"><span data-stu-id="5f11d-128">You can review hello code that accesses your mobile app backend tooquery and insert data, which is found in hello ToDoActivity.cs C# file.</span></span>
   >
   >

## <a name="next-steps"></a><span data-ttu-id="5f11d-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5f11d-129">Next steps</span></span>
* [<span data-ttu-id="5f11d-130">Lägg till Offline Sync tooyour app</span><span class="sxs-lookup"><span data-stu-id="5f11d-130">Add Offline Sync tooyour app</span></span>](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [<span data-ttu-id="5f11d-131">Lägg till autentisering tooyour app</span><span class="sxs-lookup"><span data-stu-id="5f11d-131">Add authentication tooyour app </span></span>](app-service-mobile-xamarin-android-get-started-users.md)
* [<span data-ttu-id="5f11d-132">Lägg till push-meddelanden tooyour Xamarin.Android-app</span><span class="sxs-lookup"><span data-stu-id="5f11d-132">Add push notifications tooyour Xamarin.Android app</span></span>](app-service-mobile-xamarin-android-get-started-push.md)
* [<span data-ttu-id="5f11d-133">Hur toouse hello hanterade klienten för Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="5f11d-133">How toouse hello managed client for Azure Mobile Apps</span></span>](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
