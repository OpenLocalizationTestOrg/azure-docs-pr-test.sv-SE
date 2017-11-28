---
title: "aaaCreate en universella Windowsplattformen (UWP) som använder på Mobile Apps | Microsoft Docs"
description: "Följ den här självstudiekursen tooget igång med att använda mobilappserverdelar för utveckling av universella Windowsplattformen (UWP) appar i C#, Visual Basic eller JavaScript."
services: app-service\mobile
documentationcenter: windows
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 47124296-2908-4d92-85e0-05c4aa6db916
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: d0f57bca5a8195b8b0461b8f7a0d8516371d56a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-app"></a><span data-ttu-id="c01c1-103">Skapa en Windows-app</span><span class="sxs-lookup"><span data-stu-id="c01c1-103">Create a Windows app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="c01c1-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="c01c1-104">Overview</span></span>
<span data-ttu-id="c01c1-105">Den här kursen visar hur tooadd en molnbaserad serverdel tjänsten tooa universella Windowsplattformen (UWP) app.</span><span class="sxs-lookup"><span data-stu-id="c01c1-105">This tutorial shows you how tooadd a cloud-based backend service tooa Universal Windows Platform (UWP) app.</span></span> <span data-ttu-id="c01c1-106">Mer information om Mobile Apps finns [här](app-service-mobile-value-prop.md).</span><span class="sxs-lookup"><span data-stu-id="c01c1-106">For more information, see [What are Mobile Apps](app-service-mobile-value-prop.md).</span></span> <span data-ttu-id="c01c1-107">hello nedan visas skärmdumpar från hello slutförts app:</span><span class="sxs-lookup"><span data-stu-id="c01c1-107">hello following are screen captures from hello completed app:</span></span>

![Färdig skrivbordsapp](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)   
<span data-ttu-id="c01c1-109">Visad på ett skrivbord</span><span class="sxs-lookup"><span data-stu-id="c01c1-109">Running on a desktop.</span></span>

![Färdig mobilapp](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)  
<span data-ttu-id="c01c1-111">Visad på en telefon</span><span class="sxs-lookup"><span data-stu-id="c01c1-111">Running on a phone</span></span>

<span data-ttu-id="c01c1-112">Du måste slutföra den här kursen innan du går någon annan kurs om Mobilappar för UWP-appar.</span><span class="sxs-lookup"><span data-stu-id="c01c1-112">Completing this tutorial is a prerequisite for all other Mobile App tutorials for UWP apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c01c1-113">Krav</span><span class="sxs-lookup"><span data-stu-id="c01c1-113">Prerequisites</span></span>
<span data-ttu-id="c01c1-114">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="c01c1-114">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="c01c1-115">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="c01c1-115">An active Azure account.</span></span> <span data-ttu-id="c01c1-116">Om du inte har ett konto kan du registrera dig för en utvärderingsversion av Azure och få upp too10 mobilappar utan kostnad som du kan fortsätta att använda även efter utvärderingsperiodens slut.</span><span class="sxs-lookup"><span data-stu-id="c01c1-116">If you don't have an account, you can sign up for an Azure trial and get up too10 free mobile apps that you can keep using even after your trial ends.</span></span> <span data-ttu-id="c01c1-117">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c01c1-117">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="c01c1-118">[Visual Studio Community 2015] eller senare version.</span><span class="sxs-lookup"><span data-stu-id="c01c1-118">[Visual Studio Community 2015] or a later version.</span></span>

## <a name="create-a-new-azure-mobile-app-backend"></a><span data-ttu-id="c01c1-119">Skapa en ny mobilappsserverdel i Azure</span><span class="sxs-lookup"><span data-stu-id="c01c1-119">Create a new Azure Mobile App backend</span></span>
<span data-ttu-id="c01c1-120">Följ dessa steg toocreate en ny mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="c01c1-120">Follow these steps toocreate a new Mobile App backend.</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

<span data-ttu-id="c01c1-121">Du har nu skapat en mobilsappsserverdel i Azure som kan användas av dina mobilklientprogram.</span><span class="sxs-lookup"><span data-stu-id="c01c1-121">You have now provisioned an Azure Mobile App backend that can be used by your mobile client applications.</span></span> <span data-ttu-id="c01c1-122">Därefter måste du ladda ned ett serverprojekt för en enkel todo-list ”serverdel och publicera den tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c01c1-122">Next, you will download a server project for a simple "todo list" backend and publish it tooAzure.</span></span>

## <a name="configure-hello-server-project"></a><span data-ttu-id="c01c1-123">Konfigurera hello serverprojekt</span><span class="sxs-lookup"><span data-stu-id="c01c1-123">Configure hello server project</span></span>
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-client-project"></a><span data-ttu-id="c01c1-124">Hämta och kör klientprojektet hello</span><span class="sxs-lookup"><span data-stu-id="c01c1-124">Download and run hello client project</span></span>
<span data-ttu-id="c01c1-125">När du har konfigurerat serverdelen för Mobilappen kan du antingen skapa en ny klientapp eller ändra en befintlig app tooconnect tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c01c1-125">Once you have configured your Mobile App backend, you can either create a new client app or modify an existing app tooconnect tooAzure.</span></span> <span data-ttu-id="c01c1-126">I det här avsnittet kan du ladda ned en UWP-appsmall som är anpassade tooconnect tooyour mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="c01c1-126">In this section, you download a UWP app template project that is customized tooconnect tooyour Mobile App backend.</span></span>

1. <span data-ttu-id="c01c1-127">Tillbaka i hello **Snabbstart** klickar du på bladet för din mobilappsserverdel **skapar en ny app** > **hämta**, extrahera hello komprimerade projektfilerna tooyour lokal dator.</span><span class="sxs-lookup"><span data-stu-id="c01c1-127">Back in hello **Quick start** blade for your Mobile App backend, click **Create a new app** > **Download**, then extract hello compressed project files tooyour local computer.</span></span>

    ![Hämta ett snabbstartsprojekt för Windows](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-app-windows-quickstart.png)
2. <span data-ttu-id="c01c1-129">(Valfritt) Lägg till hello UWP-app-projekt toohello samma lösning som serverprojektet hello.</span><span class="sxs-lookup"><span data-stu-id="c01c1-129">(Optional) Add hello UWP app project toohello same solution as hello server project.</span></span> <span data-ttu-id="c01c1-130">Detta gör det enklare toodebug och testa både hello appen och hello backend i hello samma Visual Studio-lösning om du väljer toodo så.</span><span class="sxs-lookup"><span data-stu-id="c01c1-130">This makes it easier toodebug and test both hello app and hello backend in hello same Visual Studio solution, if you choose toodo so.</span></span> <span data-ttu-id="c01c1-131">tooadd en UWP-app-projekt toohello lösning, måste du använda Visual Studio 2015 eller en senare version.</span><span class="sxs-lookup"><span data-stu-id="c01c1-131">tooadd a UWP app project toohello solution, you must be using Visual Studio 2015 or a later version.</span></span>
3. <span data-ttu-id="c01c1-132">Hej UWP-appen som Startprojekt hello, tryck på hello F5 viktiga toodeploy och kör hello app.</span><span class="sxs-lookup"><span data-stu-id="c01c1-132">With hello UWP app as hello startup project, press hello F5 key toodeploy and run hello app.</span></span>
4. <span data-ttu-id="c01c1-133">I hello app, ange en beskrivande text som *fullständig hello kursen*, i hello **infoga en TodoItem** textrutan och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="c01c1-133">In hello app, type meaningful text, such as *Complete hello tutorial*, in hello **Insert a TodoItem** text box, and then click **Save**.</span></span>

    ![Färdig Windows-snabbstartsapp på skrivbord](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    <span data-ttu-id="c01c1-135">Detta skickar en POST-begäran toohello ny mobilappsserverdel som finns i Azure.</span><span class="sxs-lookup"><span data-stu-id="c01c1-135">This sends a POST request toohello new mobile app backend that's hosted in Azure.</span></span>
5. <span data-ttu-id="c01c1-136">(Valfritt) Stoppa hello appen och starta om den på en annan enhet eller mobilemulator.</span><span class="sxs-lookup"><span data-stu-id="c01c1-136">(Optional) Stop hello app and restart it on a different device or mobile emulator.</span></span>

    ![Färdig Windows-snabbstartsapp på telefon](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)

    <span data-ttu-id="c01c1-138">Observera att data som sparades hello föregående steg läses från Azure när hello UWP-appen startar.</span><span class="sxs-lookup"><span data-stu-id="c01c1-138">Notice that data saved from hello previous step is loaded from Azure after hello UWP app starts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c01c1-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c01c1-139">Next steps</span></span>
* [<span data-ttu-id="c01c1-140">Lägg till autentisering tooyour app</span><span class="sxs-lookup"><span data-stu-id="c01c1-140">Add authentication tooyour app</span></span>](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  <span data-ttu-id="c01c1-141">Lär dig hur tooauthenticate användare i appen med en identitetsprovider.</span><span class="sxs-lookup"><span data-stu-id="c01c1-141">Learn how tooauthenticate users of your app with an identity provider.</span></span>
* [<span data-ttu-id="c01c1-142">Lägg till push-meddelanden tooyour app</span><span class="sxs-lookup"><span data-stu-id="c01c1-142">Add push notifications tooyour app</span></span>](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  <span data-ttu-id="c01c1-143">Lär dig hur tooadd push-meddelanden stöder tooyour app och konfigurera din Mobilapp backend toouse Azure Notification Hubs toosend push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c01c1-143">Learn how tooadd push notifications support tooyour app and configure your Mobile App backend toouse Azure Notification Hubs toosend push notifications.</span></span>
* [<span data-ttu-id="c01c1-144">Aktivera offlinesynkronisering av appen</span><span class="sxs-lookup"><span data-stu-id="c01c1-144">Enable offline sync for your app</span></span>](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  <span data-ttu-id="c01c1-145">Lär dig hur tooadd offline stöder din app att använda en mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="c01c1-145">Learn how tooadd offline support your app using an Mobile App backend.</span></span> <span data-ttu-id="c01c1-146">Offlinesynkronisering kan slutanvändarna toointeract med en mobil app&mdash;visa, lägga till eller ändra data&mdash;även om det inte finns någon nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="c01c1-146">Offline sync allows end-users toointeract with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Mobile App SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2015]: https://go.microsoft.com/fwLink/p/?LinkID=534203
