---
title: "aaaGet igång med Azure Mobile Engagement för Xamarin.Android"
description: "Lär dig hur toouse Azure Mobile Engagement med analyser och Push-meddelanden för Xamarin.Android-appar."
services: mobile-engagement
documentationcenter: xamarin
author: piyushjo
manager: erikre
editor: 
ms.assetid: fb68cf98-08a2-41b5-8e59-757469de3fe7
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/16/2016
ms.author: piyushjo
ms.openlocfilehash: 9d584fea8e8153d511258cf9b6f87f31dac6aeca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-xamarinandroid-apps"></a><span data-ttu-id="86c17-103">Kom igång med Azure Mobile Engagement för Xamarin.Android-appar</span><span class="sxs-lookup"><span data-stu-id="86c17-103">Get Started with Azure Mobile Engagement for Xamarin.Android Apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="86c17-104">Det här avsnittet beskrivs hur du toouse Azure Mobile Engagement toounderstand appanvändningen och hur toosend push-meddelanden toosegmented användare i ett Xamarin.Android-program.</span><span class="sxs-lookup"><span data-stu-id="86c17-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and how toosend push notifications toosegmented users of a Xamarin.Android application.</span></span>
<span data-ttu-id="86c17-105">Den här kursen visar hello enkelt scenario för sändning med Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="86c17-105">This tutorial demonstrates hello simple broadcast scenario using Mobile Engagement.</span></span> <span data-ttu-id="86c17-106">I kursen får du skapa en tom Xamarin.Android-app som samlar in grundläggande data och tar emot push-meddelanden med hjälp av Google Cloud Messaging (GCM).</span><span class="sxs-lookup"><span data-stu-id="86c17-106">In it, you create a blank Xamarin.Android app that collects basic data and receives push notifications using Google Cloud Messaging (GCM).</span></span>

> [!NOTE]
> <span data-ttu-id="86c17-107">hello Azure Mobile Engagement-tjänsten kommer att dras tillbaka mars 2018 och är för närvarande bara tillgänglig tooexisting kunder.</span><span class="sxs-lookup"><span data-stu-id="86c17-107">hello Azure Mobile Engagement service will be retired March 2018 and is currently only available tooexisting customers.</span></span> <span data-ttu-id="86c17-108">Mer information finns i [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="86c17-108">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="86c17-109">Den här kursen kräver hello följande:</span><span class="sxs-lookup"><span data-stu-id="86c17-109">This tutorial requires hello following:</span></span>

* <span data-ttu-id="86c17-110">[Xamarin Studio](http://xamarin.com/studio).</span><span class="sxs-lookup"><span data-stu-id="86c17-110">[Xamarin Studio](http://xamarin.com/studio).</span></span> <span data-ttu-id="86c17-111">Du kan även använda Visual Studio med Xamarin men i den här kursen används Xamarin Studio.</span><span class="sxs-lookup"><span data-stu-id="86c17-111">You can also use Visual Studio with Xamarin but this tutorial uses Xamarin Studio.</span></span> <span data-ttu-id="86c17-112">Det finns installationsanvisningar i avsnittet om [konfiguration och installation för Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span><span class="sxs-lookup"><span data-stu-id="86c17-112">For installation instructions, see [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span></span>
* [<span data-ttu-id="86c17-113">Mobile Engagement Xamarin SDK</span><span class="sxs-lookup"><span data-stu-id="86c17-113">Mobile Engagement Xamarin SDK</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [!NOTE]
> <span data-ttu-id="86c17-114">toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="86c17-114">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="86c17-115">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="86c17-115">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="86c17-116">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).</span><span class="sxs-lookup"><span data-stu-id="86c17-116">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).</span></span>
> 
> 

## <span data-ttu-id="86c17-117"><a id="setup-azme"></a>Konfigurera Mobile Engagement för din Android-app</span><span class="sxs-lookup"><span data-stu-id="86c17-117"><a id="setup-azme"></a>Setup Mobile Engagement for your Android app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="86c17-118"><a id="connecting-app"></a>Ansluta appen toohello Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="86c17-118"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="86c17-119">Den här kursen behandlas en ”grundläggande integration”, vilket är hello minimal ange nödvändiga toocollect data och skicka ett push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="86c17-119">This tutorial presents a "basic integration", which is hello minimal set required toocollect data and send a push notification.</span></span> 

<span data-ttu-id="86c17-120">Vi skapar en grundläggande app i Xamarin Studio toodemonstrate hello-integration.</span><span class="sxs-lookup"><span data-stu-id="86c17-120">We will create a basic app with Xamarin Studio toodemonstrate hello integration.</span></span>

### <a name="create-a-new-xamarinandroid-project"></a><span data-ttu-id="86c17-121">Skapa ett nytt Xamarin.Android-projekt</span><span class="sxs-lookup"><span data-stu-id="86c17-121">Create a new Xamarin.Android project</span></span>
1. <span data-ttu-id="86c17-122">Starta **Xamarin Studio** gå för**filen** -> **ny** -> **lösning**</span><span class="sxs-lookup"><span data-stu-id="86c17-122">Launch **Xamarin Studio** Go too**File** -> **New** -> **Solution**</span></span> 
   
    ![][1]
2. <span data-ttu-id="86c17-123">Välj **Android-App** Kontrollera hello valt språk är **C#** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="86c17-123">Select **Android App** then make sure hello selected language is **C#** and click **Next**.</span></span>
   
    ![][2]
3. <span data-ttu-id="86c17-124">Fyll i hello **Appnamn** och hello **organisations-ID**.</span><span class="sxs-lookup"><span data-stu-id="86c17-124">Fill in hello **App Name** and hello **Organization Identifier**.</span></span> <span data-ttu-id="86c17-125">Se till att toocheckmark **Google Play Services** och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="86c17-125">Make sure toocheckmark **Google Play Services** and then click **Next**.</span></span> 
   
    ![][3]
4. <span data-ttu-id="86c17-126">Uppdatera hello **projektnamn**, **lösningsnamn** och **plats** om krävs och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="86c17-126">Update hello **Project Name**, **Solution Name** and **Location** if required and click **Create**.</span></span>
   
    ![][4]

<span data-ttu-id="86c17-127">Xamarin Studio skapar hello appen där Mobile Engagement ska integreras.</span><span class="sxs-lookup"><span data-stu-id="86c17-127">Xamarin Studio will create hello app in which we will integrate Mobile Engagement.</span></span> 

### <a name="connect-your-app-toomobile-engagement-backend"></a><span data-ttu-id="86c17-128">Ansluta appen tooMobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="86c17-128">Connect your app tooMobile Engagement backend</span></span>
1. <span data-ttu-id="86c17-129">Högerklicka på hello **paket** mapp i hello Lösningsfönstret och välj **lägga till paket...**</span><span class="sxs-lookup"><span data-stu-id="86c17-129">Right click hello **Packages** folder in hello Solution windows and select **Add Packages...**</span></span>
   
    ![][5]
2. <span data-ttu-id="86c17-130">Sök efter hello **Microsoft Azure Mobile Engagement Xamarin SDK** och lägga till den tooyour lösning.</span><span class="sxs-lookup"><span data-stu-id="86c17-130">Search for hello **Microsoft Azure Mobile Engagement Xamarin SDK** and add it tooyour solution.</span></span>  
   
    ![][6]
3. <span data-ttu-id="86c17-131">Öppna **MainActivity.cs** och Lägg till följande hello med-uttryck:</span><span class="sxs-lookup"><span data-stu-id="86c17-131">Open **MainActivity.cs** and add hello following using statements:</span></span>
   
        using Microsoft.Azure.Engagement;
        using Microsoft.Azure.Engagement.Activity;
4. <span data-ttu-id="86c17-132">I hello `OnCreate` metoden Lägg till hello följande tooinitialize hello anslutningen till Mobile Engagement-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="86c17-132">In hello `OnCreate` method, add hello following tooinitialize hello connection with Mobile Engagement backend.</span></span> <span data-ttu-id="86c17-133">Se till att tooadd din **ConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="86c17-133">Make sure tooadd your **ConnectionString**.</span></span> 
   
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.ConnectionString = "YourConnectionStringFromAzurePortal";
        EngagementAgent.Init(engagementConfiguration);

### <a name="add-permissions-and-a-service-declaration"></a><span data-ttu-id="86c17-134">Lägga till behörigheter och en tjänstedeklaration</span><span class="sxs-lookup"><span data-stu-id="86c17-134">Add permissions and a service declaration</span></span>
1. <span data-ttu-id="86c17-135">Öppna hello **Manifest.xml** filen hello egenskaper i mappen.</span><span class="sxs-lookup"><span data-stu-id="86c17-135">Open up hello **Manifest.xml** file under hello Properties folder.</span></span> <span data-ttu-id="86c17-136">Välj fliken källa så att du direkt uppdatera hello XML-källa.</span><span class="sxs-lookup"><span data-stu-id="86c17-136">Select Source tab so that you directly update hello XML source.</span></span>
2. <span data-ttu-id="86c17-137">Lägg till dessa behörigheter toohello Manifest.xml (som finns under hello **egenskaper** mapp) i ditt projekt omedelbart före eller efter hello `<application>` tagg:</span><span class="sxs-lookup"><span data-stu-id="86c17-137">Add these permissions toohello Manifest.xml (which can be found under hello **Properties** folder) of your project immediately before or after hello `<application>` tag:</span></span>
   
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
3. <span data-ttu-id="86c17-138">Lägg till följande hello mellan hello `<application>` och `</application>` taggar toodeclare hello agent-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="86c17-138">Add hello following between hello `<application>` and `</application>` tags toodeclare hello agent service:</span></span>
   
        <service
             android:name="com.microsoft.azure.engagement.service.EngagementService"
             android:exported="false"
             android:label="<Your application name>"
             android:process=":Engagement"/>
4. <span data-ttu-id="86c17-139">Ersätt i hello kod inklistrade, `"<Your application name>"` i hello etikett.</span><span class="sxs-lookup"><span data-stu-id="86c17-139">In hello code you just pasted, replace `"<Your application name>"` in hello label.</span></span> <span data-ttu-id="86c17-140">Detta visas i hello **inställningar** menyn där användare kan se vilka tjänster som körs på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="86c17-140">This is displayed in hello **Settings** menu where users can see services running on hello device.</span></span> <span data-ttu-id="86c17-141">Du kan lägga till hello ordet ”tjänst” i etiketten t.ex.</span><span class="sxs-lookup"><span data-stu-id="86c17-141">You can add hello word "Service" in that label for example.</span></span>

### <a name="send-a-screen-toomobile-engagement"></a><span data-ttu-id="86c17-142">Skicka en skärm tooMobile Engagement</span><span class="sxs-lookup"><span data-stu-id="86c17-142">Send a screen tooMobile Engagement</span></span>
<span data-ttu-id="86c17-143">Du måste skicka minst en skärm toohello Mobile Engagement-serverdelen i ordning toostart skicka data och säkerställa hello användarna är aktiva.</span><span class="sxs-lookup"><span data-stu-id="86c17-143">In order toostart sending data and ensuring hello users are active, you must send at least one screen toohello Mobile Engagement backend.</span></span> <span data-ttu-id="86c17-144">För att göra detta – se till att hello `MainActivity` ärver från `EngagementActivity` i stället för `Activity`.</span><span class="sxs-lookup"><span data-stu-id="86c17-144">For doing this - ensure that hello `MainActivity` inherits from `EngagementActivity` instead of `Activity`.</span></span>

    public class MainActivity : EngagementActivity

<span data-ttu-id="86c17-145">Som alternativ, om du inte kan ärva från `EngagementActivity`, måste du lägga till `.StartActivity`- och `.EndActivity`-metoder i `OnResume` respektive `OnPause`.</span><span class="sxs-lookup"><span data-stu-id="86c17-145">Alternatively, if you cannot inherit from `EngagementActivity` then you must add `.StartActivity` and `.EndActivity` methods in `OnResume` and `OnPause` respectively.</span></span>  

        protected override void OnResume()
            {
                EngagementAgent.StartActivity(EngagementAgentUtils.BuildEngagementActivityName(Java.Lang.Class.FromType(this.GetType())), null);
                base.OnResume();             
            }

            protected override void OnPause()
            {
                EngagementAgent.EndActivity();
                base.OnPause();            
            }

## <span data-ttu-id="86c17-146"><a id="monitor"></a>Anslut appen med realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="86c17-146"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="86c17-147"><a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="86c17-147"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="86c17-148">Mobile Engagement kan du toointeract med användarna, och med push-meddelanden och appmeddelanden i hello kontext kampanjer.</span><span class="sxs-lookup"><span data-stu-id="86c17-148">Mobile Engagement allows you toointeract with and REACH your users with push notifications and in-app messaging in hello context of campaigns.</span></span> <span data-ttu-id="86c17-149">Denna modul är heter REACH i hello Mobile Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="86c17-149">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="86c17-150">följande avsnitt hello ställer in din app tooreceive dem.</span><span class="sxs-lookup"><span data-stu-id="86c17-150">hello following sections sets up your app tooreceive them.</span></span>

[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[!INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[1]: ./media/mobile-engagement-xamarin-android-get-started/1.png
[2]: ./media/mobile-engagement-xamarin-android-get-started/2.png
[3]: ./media/mobile-engagement-xamarin-android-get-started/3.png
[4]: ./media/mobile-engagement-xamarin-android-get-started/4.png
[5]: ./media/mobile-engagement-xamarin-android-get-started/5.png
[6]: ./media/mobile-engagement-xamarin-android-get-started/6.png
