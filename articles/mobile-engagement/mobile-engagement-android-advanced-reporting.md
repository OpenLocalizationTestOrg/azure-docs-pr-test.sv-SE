---
title: "aaaAdvanced rapporteringsalternativen för Azure Mobile Engagement Android SDK"
description: "Beskriver hur toodo avancerade reporting toocapture analytics för Azure Mobile Engagement Android SDK"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 7da7abd5-19d6-4892-94d8-818e5424b2cd
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/10/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 5c8f4ea36c54715f4e09fd43c96132c15019a71b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-reporting-with-engagement-on-android"></a><span data-ttu-id="c191f-103">Avancerade rapportering med Engagement på Android</span><span class="sxs-lookup"><span data-stu-id="c191f-103">Advanced Reporting with Engagement on Android</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c191f-104">Universell Windows</span><span class="sxs-lookup"><span data-stu-id="c191f-104">Universal Windows</span></span>](mobile-engagement-windows-store-integrate-engagement.md)
> * [<span data-ttu-id="c191f-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="c191f-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="c191f-106">iOS</span><span class="sxs-lookup"><span data-stu-id="c191f-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="c191f-107">Android</span><span class="sxs-lookup"><span data-stu-id="c191f-107">Android</span></span>](mobile-engagement-android-advanced-reporting.md)
> 
> 

<span data-ttu-id="c191f-108">Det här avsnittet beskrivs ytterligare rapporteringsscenarier i din Android-App.</span><span class="sxs-lookup"><span data-stu-id="c191f-108">This topic describes additional reporting scenarios in your Android application.</span></span> <span data-ttu-id="c191f-109">Du kan använda dessa alternativ toohello app som skapats i hello [komma igång](mobile-engagement-android-get-started.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="c191f-109">You can apply these options toohello app created in hello [Getting Started](mobile-engagement-android-get-started.md) tutorial.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c191f-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c191f-110">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

<span data-ttu-id="c191f-111">hello kursen har du slutfört har avsiktligt direkt och enkel, men det är avancerade alternativ som du kan välja.</span><span class="sxs-lookup"><span data-stu-id="c191f-111">hello tutorial you completed was deliberately direct and simple, but there are advanced options you can choose.</span></span>

## <a name="modifying-your-activity-classes"></a><span data-ttu-id="c191f-112">Ändra din `Activity` klasser</span><span class="sxs-lookup"><span data-stu-id="c191f-112">Modifying your `Activity` classes</span></span>
<span data-ttu-id="c191f-113">I hello [komma igång-kursen](mobile-engagement-android-get-started.md), alla du hade toodo har toomake din `*Activity` underklasser ärver från hello motsvarande `Engagement*Activity` klasser.</span><span class="sxs-lookup"><span data-stu-id="c191f-113">In hello [Getting Started tutorial](mobile-engagement-android-get-started.md), all you had toodo was toomake your `*Activity` subclasses inherit from hello corresponding `Engagement*Activity` classes.</span></span> <span data-ttu-id="c191f-114">Till exempel om en bakåtkompatibel aktivitet utökat `ListActivity`, gör du det utöka `EngagementListActivity`.</span><span class="sxs-lookup"><span data-stu-id="c191f-114">For example, if your legacy activity extended `ListActivity`, you would make it extend `EngagementListActivity`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c191f-115">När du använder `EngagementListActivity` eller `EngagementExpandableListActivity`, kontrollera anrop för`requestWindowFeature(...);` görs före hello anrop för`super.onCreate(...);`, annars kraschar.</span><span class="sxs-lookup"><span data-stu-id="c191f-115">When using `EngagementListActivity` or `EngagementExpandableListActivity`, make sure any call too`requestWindowFeature(...);` is made before hello call too`super.onCreate(...);`, otherwise a crash occurs.</span></span>
> 
> 

<span data-ttu-id="c191f-116">Du hittar de här klasserna i hello `src` mappen och kopiera dem till ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="c191f-116">You can find these classes in hello `src` folder, and can copy them into your project.</span></span> <span data-ttu-id="c191f-117">hello klasser finns också i hello **JavaDoc**.</span><span class="sxs-lookup"><span data-stu-id="c191f-117">hello classes are also in hello **JavaDoc**.</span></span>

## <a name="alternate-method-call-startactivity-and-endactivity-manually"></a><span data-ttu-id="c191f-118">Alternativ metod: anropa `startActivity()` och `endActivity()` manuellt</span><span class="sxs-lookup"><span data-stu-id="c191f-118">Alternate method: call `startActivity()` and `endActivity()` manually</span></span>
<span data-ttu-id="c191f-119">Om du inte kan eller inte vill att toooverload din `Activity` klasser, du kan i stället börja och sluta dina aktiviteter genom att anropa hello `EngagementAgent`'s metoder direkt.</span><span class="sxs-lookup"><span data-stu-id="c191f-119">If you cannot or do not want toooverload your `Activity` classes, you can instead start and end your activities by calling hello `EngagementAgent`'s methods directly.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c191f-120">hello Android SDK anropar aldrig hello `endActivity()` metod, även om programmet hello avslutas (på Android program aldrig stängs).</span><span class="sxs-lookup"><span data-stu-id="c191f-120">hello Android SDK never calls hello `endActivity()` method, even when hello application is closed (on Android, applications are never closed).</span></span> <span data-ttu-id="c191f-121">Därför är det *hög* rekommenderas toocall hello `startActivity()` metod i hello `onResume` återanrop av *alla* dina aktiviteter och hello `endActivity()` metod i hello `onPause()` återanrop av *alla* dina aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="c191f-121">Thus, it is *HIGHLY* recommended toocall hello `startActivity()` method in hello `onResume` callback of *ALL* your activities, and hello `endActivity()` method in hello `onPause()` callback of *ALL* your activities.</span></span> <span data-ttu-id="c191f-122">Detta är hello endast sätt toobe till att sessioner inte sprids.</span><span class="sxs-lookup"><span data-stu-id="c191f-122">This is hello only way toobe sure that sessions are not leaked.</span></span> <span data-ttu-id="c191f-123">Om en session har läckts kopplar hello Engagement service aldrig från hello Engagement-serverdelen (eftersom hello tjänsten fortfarande ansluten så länge en session väntar).</span><span class="sxs-lookup"><span data-stu-id="c191f-123">If a session is leaked, hello Engagement service never disconnects from hello Engagement backend (since hello service stays connected as long as a session is pending).</span></span>
> 
> 

<span data-ttu-id="c191f-124">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="c191f-124">Here is an example:</span></span>

    public class MyActivity extends Some3rdPartyActivity
    {
      @Override
      protected void onResume()
      {
        super.onResume();
        String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at hello end.
        EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
      }

      @Override
      protected void onPause()
      {
        super.onPause();
        EngagementAgent.getInstance(this).endActivity();
      }
    }

<span data-ttu-id="c191f-125">Det här exemplet är liknande toohello `EngagementActivity` klassen och dess varianter vars källkoden har angetts i hello `src` mapp.</span><span class="sxs-lookup"><span data-stu-id="c191f-125">This example is similar toohello `EngagementActivity` class and its variants, whose source code is provided in hello `src` folder.</span></span>

## <a name="using-applicationoncreate"></a><span data-ttu-id="c191f-126">Med hjälp av Application.onCreate()</span><span class="sxs-lookup"><span data-stu-id="c191f-126">Using Application.onCreate()</span></span>
<span data-ttu-id="c191f-127">All kod som du `Application.onCreate()` och i andra program körs återanrop för ditt programs processer, inklusive hello Engagement service.</span><span class="sxs-lookup"><span data-stu-id="c191f-127">Any code you place in `Application.onCreate()` and in other application callbacks is run for all your application's processes, including hello Engagement service.</span></span> <span data-ttu-id="c191f-128">Det kan ha oönskade sidoeffekter, som inte behövs minnesallokering och trådar i hello Engagement process eller dubblett broadcast mottagare eller tjänster.</span><span class="sxs-lookup"><span data-stu-id="c191f-128">It may have unwanted side effects, like unneeded memory allocations and threads in hello Engagement's process, or duplicate broadcast receivers or services.</span></span>

<span data-ttu-id="c191f-129">Om du åsidosätter `Application.onCreate()`, rekommenderar vi att lägga till hello följande kodavsnitt hello början av din `Application.onCreate()` funktionen:</span><span class="sxs-lookup"><span data-stu-id="c191f-129">If you override `Application.onCreate()`, we recommend adding hello following code snippet at hello beginning of your `Application.onCreate()` function:</span></span>

     public void onCreate()
     {
       if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
         return;

       ... Your code...
     }

<span data-ttu-id="c191f-130">Du kan göra samma sak för hello `Application.onTerminate()`, `Application.onLowMemory()`, och `Application.onConfigurationChanged(...)`.</span><span class="sxs-lookup"><span data-stu-id="c191f-130">You can do hello same thing for `Application.onTerminate()`, `Application.onLowMemory()`, and `Application.onConfigurationChanged(...)`.</span></span>

<span data-ttu-id="c191f-131">Du kan också utöka `EngagementApplication` i stället för att utöka `Application`: hello återanrop `Application.onCreate()` hello processen kontroll och anrop `Application.onApplicationProcessCreate()` endast om hello aktuella processen inte hello en hello Engagement värdtjänsten hello samma regler gäller för Hej andra återanrop.</span><span class="sxs-lookup"><span data-stu-id="c191f-131">You can also extend `EngagementApplication` instead of extending `Application`: hello callback `Application.onCreate()` does hello process check and calls `Application.onApplicationProcessCreate()` only if hello current process is not hello one hosting hello Engagement service, hello same rules apply for hello other callbacks.</span></span>

## <a name="tags-in-hello-androidmanifestxml-file"></a><span data-ttu-id="c191f-132">Taggar i hello AndroidManifest.xml fil</span><span class="sxs-lookup"><span data-stu-id="c191f-132">Tags in hello AndroidManifest.xml file</span></span>
<span data-ttu-id="c191f-133">I hello service-taggen i hello AndroidManifest.xml filen hello `android:label` attribut kan du toochoose hello namnet på hello Engagement tjänsten som det visas tooend användarna på skärmen för hello ”körs tjänster” i sin telefon.</span><span class="sxs-lookup"><span data-stu-id="c191f-133">In hello service tag in hello AndroidManifest.xml file, hello `android:label` attribute allows you toochoose hello name of hello Engagement service as it appears tooend users in hello "Running services" screen of their phone.</span></span> <span data-ttu-id="c191f-134">Vi rekommenderar att det här attributet för`"<Your application name>Service"` (till exempel `"AcmeFunGameService"`).</span><span class="sxs-lookup"><span data-stu-id="c191f-134">We recommended setting this attribute too`"<Your application name>Service"` (for example, `"AcmeFunGameService"`).</span></span>

<span data-ttu-id="c191f-135">Att ange hello `android:process` attributet säkerställer att hello Engagement-tjänsten körs i en egen process (kör Engagement hello samma process som programmet gör main/UI-tråden potentiellt mindre känslig).</span><span class="sxs-lookup"><span data-stu-id="c191f-135">Specifying hello `android:process` attribute ensures that hello Engagement service runs in its own process (running Engagement in hello same process as your application makes your main/UI thread potentially less responsive).</span></span>

## <a name="building-with-proguard"></a><span data-ttu-id="c191f-136">Skapa med ProGuard</span><span class="sxs-lookup"><span data-stu-id="c191f-136">Building with ProGuard</span></span>
<span data-ttu-id="c191f-137">Om du skapar ditt programpaket med ProGuard måste tookeep vissa klasser.</span><span class="sxs-lookup"><span data-stu-id="c191f-137">If you build your application package with ProGuard, you need tookeep some classes.</span></span> <span data-ttu-id="c191f-138">Du kan använda följande konfiguration fragment hello:</span><span class="sxs-lookup"><span data-stu-id="c191f-138">You can use hello following configuration snippet:</span></span>

    -keep public class * extends android.os.IInterface
    -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
    <methods>;
     }
