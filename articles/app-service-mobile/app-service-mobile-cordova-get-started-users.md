---
title: "aaaAdd autentisering på Apache Cordova med Mobilappar | Microsoft Docs"
description: "Lär dig hur toouse Mobile Apps i Azure App Service tooauthenticate användare i din Apache Cordova-app via en mängd olika identitetsleverantörer, inklusive Google, Facebook, Twitter och Microsoft."
services: app-service\mobile
documentationcenter: javascript
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 10dd6dc9-ddf5-423d-8205-00ad74929f0d
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 61a05c5ac67fd0f0bc4c9d6920954a9b464a0a8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-apache-cordova-app"></a><span data-ttu-id="41776-103">Lägg till autentisering tooyour Apache Cordova-app</span><span class="sxs-lookup"><span data-stu-id="41776-103">Add authentication tooyour Apache Cordova app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a><span data-ttu-id="41776-104">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="41776-104">Summary</span></span>
<span data-ttu-id="41776-105">I kursen får du lägga till autentisering toohello todolist snabbstartsprojekt på Apache Cordova med hjälp av en stöds identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="41776-105">In this tutorial, you add authentication toohello todolist quickstart project on Apache Cordova using a supported identity provider.</span></span> <span data-ttu-id="41776-106">Den här kursen är baserad på hello [Kom igång med Mobile Apps] självstudien måste du slutföra först.</span><span class="sxs-lookup"><span data-stu-id="41776-106">This tutorial is based on hello [Get started with Mobile Apps] tutorial, which you must complete first.</span></span>

## <span data-ttu-id="41776-107"><a name="register"></a>Registrera din app för autentisering och konfigurera hello Apptjänst</span><span class="sxs-lookup"><span data-stu-id="41776-107"><a name="register"></a>Register your app for authentication and configure hello App Service</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[<span data-ttu-id="41776-108">Titta på en video som visar liknande steg</span><span class="sxs-lookup"><span data-stu-id="41776-108">Watch a video showing similar steps</span></span>](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

## <span data-ttu-id="41776-109"><a name="permissions"></a>Begränsa behörigheter tooauthenticated användare</span><span class="sxs-lookup"><span data-stu-id="41776-109"><a name="permissions"></a>Restrict permissions tooauthenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="41776-110">Nu kan du kontrollera att anonym åtkomst tooyour backend har inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="41776-110">Now, you can verify that anonymous access tooyour backend has been disabled.</span></span> <span data-ttu-id="41776-111">I Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="41776-111">In Visual Studio:</span></span>

* <span data-ttu-id="41776-112">Öppna hello-projekt som du skapade när du slutfört självstudiekursen hello [Kom igång med Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="41776-112">Open hello project that you created when you completed hello tutorial [Get started with Mobile Apps].</span></span>
* <span data-ttu-id="41776-113">Kör ditt program i hello **Google Android-emulatorn**.</span><span class="sxs-lookup"><span data-stu-id="41776-113">Run your application in hello **Google Android Emulator**.</span></span>
* <span data-ttu-id="41776-114">Kontrollera att ett oväntat fel i anslutningen visas när hello appen startar.</span><span class="sxs-lookup"><span data-stu-id="41776-114">Verify that an Unexpected Connection Failure is shown after hello app starts.</span></span>

<span data-ttu-id="41776-115">Därefter uppdatera hello app tooauthenticate användare innan du begär resurser från hello mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="41776-115">Next, update hello app tooauthenticate users before requesting resources from hello Mobile App backend.</span></span>

## <span data-ttu-id="41776-116"><a name="add-authentication"></a>Lägg till autentisering toohello app</span><span class="sxs-lookup"><span data-stu-id="41776-116"><a name="add-authentication"></a>Add authentication toohello app</span></span>
1. <span data-ttu-id="41776-117">Öppna projektet i **Visual Studio**sedan öppnar hello `www/index.html` filen för redigering.</span><span class="sxs-lookup"><span data-stu-id="41776-117">Open your project in **Visual Studio**, then open hello `www/index.html` file for editing.</span></span>
2. <span data-ttu-id="41776-118">Leta upp hello `Content-Security-Policy` meta-tagg i head hello-avsnitt.</span><span class="sxs-lookup"><span data-stu-id="41776-118">Locate hello `Content-Security-Policy` meta tag in hello head section.</span></span>  <span data-ttu-id="41776-119">Lägg till hello OAuth värden toohello listan över tillåtna källor.</span><span class="sxs-lookup"><span data-stu-id="41776-119">Add hello OAuth host toohello list of allowed sources.</span></span>

   | <span data-ttu-id="41776-120">Leverantör</span><span class="sxs-lookup"><span data-stu-id="41776-120">Provider</span></span> | <span data-ttu-id="41776-121">Namn på SDK-Provider</span><span class="sxs-lookup"><span data-stu-id="41776-121">SDK Provider Name</span></span> | <span data-ttu-id="41776-122">OAuth-värden</span><span class="sxs-lookup"><span data-stu-id="41776-122">OAuth Host</span></span> |
   |:--- |:--- |:--- |
   | <span data-ttu-id="41776-123">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="41776-123">Azure Active Directory</span></span> | <span data-ttu-id="41776-124">aad</span><span class="sxs-lookup"><span data-stu-id="41776-124">aad</span></span> | <span data-ttu-id="41776-125">https://login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="41776-125">https://login.microsoftonline.com</span></span> |
   | <span data-ttu-id="41776-126">Facebook</span><span class="sxs-lookup"><span data-stu-id="41776-126">Facebook</span></span> | <span data-ttu-id="41776-127">Facebook</span><span class="sxs-lookup"><span data-stu-id="41776-127">facebook</span></span> | <span data-ttu-id="41776-128">https://www.Facebook.com</span><span class="sxs-lookup"><span data-stu-id="41776-128">https://www.facebook.com</span></span> |
   | <span data-ttu-id="41776-129">Google</span><span class="sxs-lookup"><span data-stu-id="41776-129">Google</span></span> | <span data-ttu-id="41776-130">Google</span><span class="sxs-lookup"><span data-stu-id="41776-130">google</span></span> | <span data-ttu-id="41776-131">https://accounts.Google.com</span><span class="sxs-lookup"><span data-stu-id="41776-131">https://accounts.google.com</span></span> |
   | <span data-ttu-id="41776-132">Microsoft</span><span class="sxs-lookup"><span data-stu-id="41776-132">Microsoft</span></span> | <span data-ttu-id="41776-133">MicrosoftAccount</span><span class="sxs-lookup"><span data-stu-id="41776-133">microsoftaccount</span></span> | <span data-ttu-id="41776-134">https://login.live.com</span><span class="sxs-lookup"><span data-stu-id="41776-134">https://login.live.com</span></span> |
   | <span data-ttu-id="41776-135">Twitter</span><span class="sxs-lookup"><span data-stu-id="41776-135">Twitter</span></span> | <span data-ttu-id="41776-136">Twitter</span><span class="sxs-lookup"><span data-stu-id="41776-136">twitter</span></span> | <span data-ttu-id="41776-137">https://API.Twitter.com</span><span class="sxs-lookup"><span data-stu-id="41776-137">https://api.twitter.com</span></span> |

    <span data-ttu-id="41776-138">Ett exempel innehåll--säkerhetsprincip (implementerat för Azure Active Directory) är följande:</span><span class="sxs-lookup"><span data-stu-id="41776-138">An example Content-Security-Policy (implemented for Azure Active Directory) is as follows:</span></span>

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.microsoftonline.com https://yourapp.azurewebsites.net; style-src 'self'">

    <span data-ttu-id="41776-139">Ersätt `https://login.microsoftonline.com` med hello OAuth-värden från hello föregående tabell.</span><span class="sxs-lookup"><span data-stu-id="41776-139">Replace `https://login.microsoftonline.com` with hello OAuth host from hello preceding table.</span></span>  <span data-ttu-id="41776-140">Mer information om hello innehåll säkerhetsprincip meta-tagg finns hello [innehåll säkerhetsprincip dokumentationen].</span><span class="sxs-lookup"><span data-stu-id="41776-140">For more information about hello content-security-policy meta tag, see hello [Content-Security-Policy documentation].</span></span>

    <span data-ttu-id="41776-141">Vissa autentiseringsproviders kräver inte innehåll säkerhetsprincip ändras när den används på lämplig mobila enheter.</span><span class="sxs-lookup"><span data-stu-id="41776-141">Some authentication providers do not require Content-Security-Policy changes when used on appropriate mobile devices.</span></span>  <span data-ttu-id="41776-142">Inget innehåll säkerhetsprincip ändringar krävs till exempel vid användning av Google-autentisering på en Android-enhet.</span><span class="sxs-lookup"><span data-stu-id="41776-142">For example, no Content-Security-Policy changes are required when using Google authentication on an Android device.</span></span>

3. <span data-ttu-id="41776-143">Öppna hello `www/js/index.js` filen för redigering, leta upp hello `onDeviceReady()` metod, och under hello klienten skapas kod lägger du till följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="41776-143">Open hello `www/js/index.js` file for editing, locate hello `onDeviceReady()` method, and under hello client  creation code add hello following code:</span></span>

        // Login toohello service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh hello todoItems
                refreshDisplay();

                // Wire up hello UI Event Handler for hello Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    <span data-ttu-id="41776-144">Den här koden ersätter hello befintlig kod som skapar hello tabellreferens och uppdaterar hello Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="41776-144">This code replaces hello existing code that creates hello table reference and refreshes hello UI.</span></span>

    <span data-ttu-id="41776-145">Hej login() metoden startar autentisering med hello-providern.</span><span class="sxs-lookup"><span data-stu-id="41776-145">hello login() method starts authentication with hello provider.</span></span> <span data-ttu-id="41776-146">Hej login() metoden är en async-funktion som returnerar en JavaScript-Promise.</span><span class="sxs-lookup"><span data-stu-id="41776-146">hello login() method is an async function that returns a JavaScript Promise.</span></span>  <span data-ttu-id="41776-147">hello resten av hello initieringen placeras inuti hello promise svar så att den inte körs förrän hello login() metod.</span><span class="sxs-lookup"><span data-stu-id="41776-147">hello rest of hello initialization is placed inside hello promise response so that it is not executed until hello login() method completes.</span></span>

4. <span data-ttu-id="41776-148">I hello kod som du just lagt till, ersätta `SDK_Provider_Name` med hello namnet på leverantören inloggningen.</span><span class="sxs-lookup"><span data-stu-id="41776-148">In hello code that you just added, replace `SDK_Provider_Name` with hello name of your login provider.</span></span> <span data-ttu-id="41776-149">Till exempel för Azure Active Directory, använda `client.login('aad')`.</span><span class="sxs-lookup"><span data-stu-id="41776-149">For example, for Azure Active Directory, use `client.login('aad')`.</span></span>
5. <span data-ttu-id="41776-150">Kör projektet.</span><span class="sxs-lookup"><span data-stu-id="41776-150">Run your project.</span></span>  <span data-ttu-id="41776-151">När hello projektet har slutfört initiering, visar programmet hello OAuth-inloggningssidan för hello valt autentiseringsprovider.</span><span class="sxs-lookup"><span data-stu-id="41776-151">When hello project has finished initializing, your application shows hello OAuth login page for hello chosen authentication provider.</span></span>

## <span data-ttu-id="41776-152"><a name="next-steps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="41776-152"><a name="next-steps"></a>Next Steps</span></span>
* <span data-ttu-id="41776-153">Lär dig mer [om verifiering] med Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="41776-153">Learn more [About Authentication] with Azure App Service.</span></span>
* <span data-ttu-id="41776-154">Fortsätta hello kursen genom att lägga till [Push-meddelanden] tooyour Apache Cordova-app.</span><span class="sxs-lookup"><span data-stu-id="41776-154">Continue hello tutorial by adding [Push Notifications] tooyour Apache Cordova app.</span></span>

<span data-ttu-id="41776-155">Lär dig hur toouse hello SDK: er.</span><span class="sxs-lookup"><span data-stu-id="41776-155">Learn how toouse hello SDKs.</span></span>

* <span data-ttu-id="41776-156">[Apache Cordova-SDK]</span><span class="sxs-lookup"><span data-stu-id="41776-156">[Apache Cordova SDK]</span></span>
* <span data-ttu-id="41776-157">[ASP.NET Server-SDK]</span><span class="sxs-lookup"><span data-stu-id="41776-157">[ASP.NET Server SDK]</span></span>
* <span data-ttu-id="41776-158">[Node.js Server-SDK]</span><span class="sxs-lookup"><span data-stu-id="41776-158">[Node.js Server SDK]</span></span>

<!-- URLs. -->
[Kom igång med Mobile Apps]: app-service-mobile-cordova-get-started.md
[innehåll säkerhetsprincip dokumentationen]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[Push-meddelanden]: app-service-mobile-cordova-get-started-push.md
[om verifiering]: app-service-mobile-auth.md
[Apache Cordova-SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server-SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server-SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
