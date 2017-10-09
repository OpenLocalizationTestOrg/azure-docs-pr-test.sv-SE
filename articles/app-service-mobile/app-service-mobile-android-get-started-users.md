---
title: "aaaAdd autentisering på Android med Mobile Apps | Microsoft Docs"
description: "Lär dig hur toouse hello Mobile Apps-funktionen i Azure App Service tooauthenticate användare av din Android-app via en mängd olika identitetsleverantörer, inklusive Google, Facebook, Twitter och Microsoft."
services: app-service\mobile
documentationcenter: android
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 1fc8e7c1-6c3c-40f4-9967-9cf5e21fc4e1
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 01f608f996c931c643790ed2778df11cf590c903
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-android-app"></a><span data-ttu-id="89102-103">Lägg till autentisering tooyour Android-app</span><span class="sxs-lookup"><span data-stu-id="89102-103">Add authentication tooyour Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a><span data-ttu-id="89102-104">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="89102-104">Summary</span></span>
<span data-ttu-id="89102-105">I kursen får du lägga till autentisering toohello todolist snabbstartsprojekt på Android med hjälp av en stöds identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="89102-105">In this tutorial, you add authentication toohello todolist quickstart project on Android by using a supported identity provider.</span></span> <span data-ttu-id="89102-106">Den här kursen är baserad på hello [Kom igång med Mobile Apps] självstudien måste du slutföra först.</span><span class="sxs-lookup"><span data-stu-id="89102-106">This tutorial is based on hello [Get started with Mobile Apps] tutorial, which you must complete first.</span></span>

## <span data-ttu-id="89102-107"><a name="register"></a>Registrera din app för autentisering och konfigurera Azure App Service</span><span class="sxs-lookup"><span data-stu-id="89102-107"><a name="register"></a>Register your app for authentication and configure Azure App Service</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="89102-108"><a name="redirecturl"></a>Lägg till din app toohello tillåtna externa omdirigerings-URL</span><span class="sxs-lookup"><span data-stu-id="89102-108"><a name="redirecturl"></a>Add your app toohello Allowed External Redirect URLs</span></span>

<span data-ttu-id="89102-109">Säker autentisering måste du definiera en ny URL-schema för din app.</span><span class="sxs-lookup"><span data-stu-id="89102-109">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="89102-110">Detta tillåter hello autentisering system tooredirect tillbaka tooyour appen när hello-autentiseringen är klar.</span><span class="sxs-lookup"><span data-stu-id="89102-110">This allows hello authentication system tooredirect back tooyour app once hello authentication process is complete.</span></span> <span data-ttu-id="89102-111">I den här självstudiekursen kommer vi använda hello URL-schema _appname_ i hela.</span><span class="sxs-lookup"><span data-stu-id="89102-111">In this tutorial, we use hello URL scheme _appname_ throughout.</span></span> <span data-ttu-id="89102-112">Du kan dock använda alla URL-schema som du väljer.</span><span class="sxs-lookup"><span data-stu-id="89102-112">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="89102-113">Det bör vara unikt tooyour mobila program.</span><span class="sxs-lookup"><span data-stu-id="89102-113">It should be unique tooyour mobile application.</span></span> <span data-ttu-id="89102-114">tooenable hello omdirigering på serversidan hello:</span><span class="sxs-lookup"><span data-stu-id="89102-114">tooenable hello redirection on hello server side:</span></span>

1. <span data-ttu-id="89102-115">I hello [Azure portal], väljer du din Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="89102-115">In hello [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="89102-116">Klicka på hello **autentisering / auktorisering** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="89102-116">Click hello **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="89102-117">I hello **tillåtna externa omdirigerings-URL: er**, ange `appname://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="89102-117">In hello **Allowed External Redirect URLs**, enter `appname://easyauth.callback`.</span></span>  <span data-ttu-id="89102-118">Hej _appname_ i den här strängen är hello URL-schema för din mobila program.</span><span class="sxs-lookup"><span data-stu-id="89102-118">hello _appname_ in this string is hello URL Scheme for your mobile application.</span></span>  <span data-ttu-id="89102-119">Det bör följa den normala URL specifikation för ett protokoll (Använd bokstäver och siffror och börja med en bokstav).</span><span class="sxs-lookup"><span data-stu-id="89102-119">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="89102-120">Du bör anteckna hello sträng som du väljer när du behöver tooadjust mobilprogram koden med hello URL-schemat på flera platser.</span><span class="sxs-lookup"><span data-stu-id="89102-120">You should make a note of hello string that you choose as you will need tooadjust your mobile application code with hello URL Scheme in several places.</span></span>

4. <span data-ttu-id="89102-121">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="89102-121">Click **OK**.</span></span>

5. <span data-ttu-id="89102-122">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="89102-122">Click **Save**.</span></span>

## <span data-ttu-id="89102-123"><a name="permissions"></a>Begränsa behörigheter tooauthenticated användare</span><span class="sxs-lookup"><span data-stu-id="89102-123"><a name="permissions"></a>Restrict permissions tooauthenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

* <span data-ttu-id="89102-124">Öppna hello-projektet som du har slutfört med hello kursen i Android Studio [Kom igång med Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="89102-124">In Android Studio, open hello project you completed with hello tutorial [Get started with Mobile Apps].</span></span> <span data-ttu-id="89102-125">Från hello **kör** -menyn klickar du på **kör app**, och kontrollera att ett undantag med en statuskod 401 (obehörig) utlöses efter hello appen startar.</span><span class="sxs-lookup"><span data-stu-id="89102-125">From hello **Run** menu, click **Run app**, and verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after hello app starts.</span></span>

     <span data-ttu-id="89102-126">Det här undantaget inträffar eftersom hello app försök tooaccess hello tillbaka som en oautentiserad användare, men hello *TodoItem* tabellen nu kräver autentisering.</span><span class="sxs-lookup"><span data-stu-id="89102-126">This exception happens because hello app attempts tooaccess hello back end as an unauthenticated user, but hello *TodoItem* table now requires authentication.</span></span>

<span data-ttu-id="89102-127">Därefter uppdaterar du hello app tooauthenticate användare innan du begär resurser från hello Mobile Apps tillbaka avslutas.</span><span class="sxs-lookup"><span data-stu-id="89102-127">Next, you update hello app tooauthenticate users before requesting resources from hello Mobile Apps back end.</span></span> 

## <a name="add-authentication-toohello-app"></a><span data-ttu-id="89102-128">Lägg till autentisering toohello app</span><span class="sxs-lookup"><span data-stu-id="89102-128">Add authentication toohello app</span></span>
[!INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]



## <span data-ttu-id="89102-129"><a name="cache-tokens"></a>Cache-autentiseringstoken på hello-klient</span><span class="sxs-lookup"><span data-stu-id="89102-129"><a name="cache-tokens"></a>Cache authentication tokens on hello client</span></span>
[!INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

## <a name="next-steps"></a><span data-ttu-id="89102-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="89102-130">Next steps</span></span>
<span data-ttu-id="89102-131">Nu när du har slutfört den här självstudiekursen för grundläggande autentisering, Överväg fortsätter tooone av hello följande kurser:</span><span class="sxs-lookup"><span data-stu-id="89102-131">Now that you completed this basic authentication tutorial, consider continuing on tooone of hello following tutorials:</span></span>

* <span data-ttu-id="89102-132">[Lägg till push-meddelanden tooyour Android app](app-service-mobile-android-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="89102-132">[Add push notifications tooyour Android app](app-service-mobile-android-get-started-push.md).</span></span>
  <span data-ttu-id="89102-133">Lär dig hur tooconfigure Mobile Apps tillbaka avslutas toouse Azure notification hubs toosend push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="89102-133">Learn how tooconfigure your Mobile Apps back end toouse Azure notification hubs toosend push notifications.</span></span>
* <span data-ttu-id="89102-134">[Aktivera offlinesynkronisering av din Android-app](app-service-mobile-android-get-started-offline-data.md).</span><span class="sxs-lookup"><span data-stu-id="89102-134">[Enable offline sync for your Android app](app-service-mobile-android-get-started-offline-data.md).</span></span>
  <span data-ttu-id="89102-135">Lär dig hur tooadd offline stöder tooyour appen med hjälp av en Mobile Apps-serverdel.</span><span class="sxs-lookup"><span data-stu-id="89102-135">Learn how tooadd offline support tooyour app by using a Mobile Apps back end.</span></span> <span data-ttu-id="89102-136">Med offlinesynkronisering, användare kan interagera med en mobil app&mdash;visa, lägga till eller ändra data&mdash;även om det inte finns någon nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="89102-136">With offline sync, users can interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions tooauthenticated users]: #permissions
[Add authentication toohello app]: #add-authentication
[Store authentication tokens on hello client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Kom igång med Mobile Apps]: app-service-mobile-android-get-started.md
