---
title: "Skicka säkra Push-meddelanden med Azure Notification Hubs"
description: "Lär dig mer om att skicka säkra push-meddelanden till en Android-app från Azure. Kodexempel som skrivits i Java- och C#."
documentationcenter: android
keywords: push-meddelande, push-meddelanden, push-meddelanden, android push-meddelanden
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: daf3de1c-f6a9-43c4-8165-a76bfaa70893
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 29f8c516e611c13fb73c7edc15e7c52708c75bb0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="sending-secure-push-notifications-with-azure-notification-hubs"></a><span data-ttu-id="f3c8f-105">Skicka säkra Push-meddelanden med Azure Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="f3c8f-105">Sending Secure Push Notifications with Azure Notification Hubs</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f3c8f-106">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="f3c8f-106">Windows Universal</span></span>](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [<span data-ttu-id="f3c8f-107">iOS</span><span class="sxs-lookup"><span data-stu-id="f3c8f-107">iOS</span></span>](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [<span data-ttu-id="f3c8f-108">Android</span><span class="sxs-lookup"><span data-stu-id="f3c8f-108">Android</span></span>](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="f3c8f-109">Översikt</span><span class="sxs-lookup"><span data-stu-id="f3c8f-109">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f3c8f-110">Du måste ha ett aktivt Azure-konto för att slutföra den här kursen.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-110">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="f3c8f-111">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-111">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="f3c8f-112">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="f3c8f-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).</span></span>
> 
> 

<span data-ttu-id="f3c8f-113">Stöd för push-meddelanden i Microsoft Azure kan du komma åt en lätt att använda, multiplatform, skalats ut push-meddelande-infrastruktur, vilket förenklar implementeringen av push-meddelanden för konsument- och enterprise-program för mobila plattformar.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-113">Push notification support in Microsoft Azure enables you to access an easy-to-use, multiplatform, scaled-out push message infrastructure, which greatly simplifies the implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span>

<span data-ttu-id="f3c8f-114">På grund av reglerande säkerhetsbegränsningar, ibland ett program kan också innehålla något i meddelandet inte kan överföras via de standard push-infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-114">Due to regulatory or security constraints, sometimes an application might want to include something in the notification that cannot be transmitted through the standard push notification infrastructure.</span></span> <span data-ttu-id="f3c8f-115">Den här självstudiekursen beskrivs hur du kan uppnå samma upplevelse genom att skicka känslig information via en säker och autentiserad anslutning mellan klienten Android-enhet och appens serverdel.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-115">This tutorial describes how to achieve the same experience by sending sensitive information through a secure, authenticated connection between the client Android device and the app backend.</span></span>

<span data-ttu-id="f3c8f-116">På en hög nivå är flödet:</span><span class="sxs-lookup"><span data-stu-id="f3c8f-116">At a high level, the flow is as follows:</span></span>

1. <span data-ttu-id="f3c8f-117">På appens serverdel:</span><span class="sxs-lookup"><span data-stu-id="f3c8f-117">The app back-end:</span></span>
   * <span data-ttu-id="f3c8f-118">Lagrar säker nyttolast i backend-databas.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-118">Stores secure payload in back-end database.</span></span>
   * <span data-ttu-id="f3c8f-119">Skickar ID för det här meddelandet till Android-enhet (ingen säker information skickas).</span><span class="sxs-lookup"><span data-stu-id="f3c8f-119">Sends the ID of this notification to the Android device (no secure information is sent).</span></span>
2. <span data-ttu-id="f3c8f-120">Appen på enheten när du tar emot meddelandet:</span><span class="sxs-lookup"><span data-stu-id="f3c8f-120">The app on the device, when receiving the notification:</span></span>
   * <span data-ttu-id="f3c8f-121">Android-enhet kontaktar serverdelen begär säker nyttolasten.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-121">The Android device contacts the back-end requesting the secure payload.</span></span>
   * <span data-ttu-id="f3c8f-122">Appen kan du visa nyttolasten som ett meddelande på enheten.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-122">The app can show the payload as a notification on the device.</span></span>

<span data-ttu-id="f3c8f-123">Det är viktigt att Observera att i det föregående flödet (och i den här självstudiekursen) antar vi att enheten lagrar en autentiseringstoken i lokal lagring när en användare loggar in.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-123">It is important to note that in the preceding flow (and in this tutorial), we assume that the device stores an authentication token in local storage, after the user logs in.</span></span> <span data-ttu-id="f3c8f-124">Detta garanterar en helt integrerad upplevelse som enheten kan hämta den anmälan säker nyttolast med denna token.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-124">This guarantees a completely seamless experience, as the device can retrieve the notification’s secure payload using this token.</span></span> <span data-ttu-id="f3c8f-125">Om ditt program inte kan lagra autentiseringstoken på enheten, eller om dessa token kan ha gått, visas vid mottagning av push-meddelanden i appen enhet ett allmänt meddelande där användaren uppmanas att starta appen.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-125">If your application does not store authentication tokens on the device, or if these tokens can be expired, the device app, upon receiving the push notification should display a generic notification prompting the user to launch the app.</span></span> <span data-ttu-id="f3c8f-126">Appen sedan autentiserar användaren och visar nyttolasten för meddelanden.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-126">The app then authenticates the user and shows the notification payload.</span></span>

<span data-ttu-id="f3c8f-127">Den här kursen visar hur du skickar säker push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-127">This tutorial shows how to send secure push notifications.</span></span> <span data-ttu-id="f3c8f-128">Den bygger på den [meddela användare](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) självstudier, så bör du genomföra stegen i den här självstudiekursen först om du inte redan har gjort.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-128">It builds on the [Notify Users](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) tutorial, so you should complete the steps in that tutorial first if you haven't already.</span></span>

> [!NOTE]
> <span data-ttu-id="f3c8f-129">Den här kursen förutsätter att du har skapat och konfigurerat din meddelandehubb enligt beskrivningen i [komma igång med Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="f3c8f-129">This tutorial assumes that you have created and configured your notification hub as described in [Getting Started with Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).</span></span>
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-android-project"></a><span data-ttu-id="f3c8f-130">Ändra Android-projekt</span><span class="sxs-lookup"><span data-stu-id="f3c8f-130">Modify the Android project</span></span>
<span data-ttu-id="f3c8f-131">Nu när du har ändrat din appens serverdel att skicka bara den *id* av ett push-meddelande måste du ändra din Android-app för att hantera detta meddelande och ringa upp din serverdel för att hämta det säkra meddelandet som ska visas.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-131">Now that you modified your app back-end to send just the *id* of a push notification, you have to change your Android app to handle that notification and call back your back-end to retrieve the secure message to be displayed.</span></span>
<span data-ttu-id="f3c8f-132">Du måste se till att din Android-app användas att autentisera sig med din serverdel när den tar emot push-meddelanden för att åstadkomma detta.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-132">To achieve this goal, you have to make sure that your Android app knows how to authenticate itself with your back-end when it receives the push notifications.</span></span>

<span data-ttu-id="f3c8f-133">Vi kommer nu att ändra den *inloggning* flödet för att kunna spara huvudvärde autentisering i delade inställningar för din app.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-133">We will now modify the *login* flow in order to save the authentication header value in the shared preferences of your app.</span></span> <span data-ttu-id="f3c8f-134">Liknande metoder kan användas för att lagra alla autentiseringstoken (t.ex. OAuth-token) som appen kommer att använda utan att användarens autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-134">Analogous mechanisms can be used to store any authentication token (e.g. OAuth tokens) that the app will have to use without requiring user credentials.</span></span>

1. <span data-ttu-id="f3c8f-135">Lägg till följande konstanter i projektet Android-app överst i den **MainActivity** klass:</span><span class="sxs-lookup"><span data-stu-id="f3c8f-135">In your Android app project, add the following constants at the top of the **MainActivity** class:</span></span>
   
        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";
2. <span data-ttu-id="f3c8f-136">Fortfarande i den **MainActivity** klass, uppdaterar den `getAuthorizationHeader()` metoden innehåller följande kod:</span><span class="sxs-lookup"><span data-stu-id="f3c8f-136">Still in the **MainActivity** class, update the `getAuthorizationHeader()` method to contain the following code:</span></span>
   
        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);
   
            SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();
   
            return basicAuthHeader;
        }
3. <span data-ttu-id="f3c8f-137">Lägg till följande `import` instruktioner överst i den **MainActivity** fil:</span><span class="sxs-lookup"><span data-stu-id="f3c8f-137">Add the following `import` statements at the top of the **MainActivity** file:</span></span>
   
        import android.content.SharedPreferences;

<span data-ttu-id="f3c8f-138">Nu ska vi ändra hanteraren som anropas när meddelandet tas emot.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-138">Now we will change the handler that is called when the notification is received.</span></span>

1. <span data-ttu-id="f3c8f-139">I den **MyHandler** klassen ändra den `OnReceive()` metoden innehålla:</span><span class="sxs-lookup"><span data-stu-id="f3c8f-139">In the **MyHandler** class change the `OnReceive()` method to contain:</span></span>
   
        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }
2. <span data-ttu-id="f3c8f-140">Lägg sedan till den `retrieveNotification()` metod, ersätter platshållaren `{back-end endpoint}` med backend-slutpunkten som erhålls vid distribution av din serverdel:</span><span class="sxs-lookup"><span data-stu-id="f3c8f-140">Then add the `retrieveNotification()` method, replacing the placeholder `{back-end endpoint}` with the back-end endpoint obtained while deploying your back-end:</span></span>
   
        private void retrieveNotification(final String secureMessageId) {
            SharedPreferences sp = ctx.getSharedPreferences(MainActivity.NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            final String authorizationHeader = sp.getString(MainActivity.AUTHORIZATION_HEADER_PROPERTY, null);
   
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        HttpUriRequest request = new HttpGet("{back-end endpoint}/api/notifications/"+secureMessageId);
                        request.addHeader("Authorization", "Basic "+authorizationHeader);
                        HttpResponse response = new DefaultHttpClient().execute(request);
                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            Log.e("MainActivity", "Error retrieving secure notification" + response.getStatusLine().getStatusCode());
                            throw new RuntimeException("Error retrieving secure notification");
                        }
                        String secureNotificationJSON = EntityUtils.toString(response.getEntity());
                        JSONObject secureNotification = new JSONObject(secureNotificationJSON);
                        sendNotification(secureNotification.getString("Payload"));
                    } catch (Exception e) {
                        Log.e("MainActivity", "Failed to retrieve secure notification - " + e.getMessage());
                        return e;
                    }
                    return null;
                }
            }.execute(null, null, null);
        }

<span data-ttu-id="f3c8f-141">Den här metoden anropar din appens serverdel för att hämta det meddelandeinnehåll med de autentiseringsuppgifter som lagras i delade inställningar och visas som ett vanligt meddelande.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-141">This method calls your app back-end to retrieve the notification content using the credentials stored in the shared preferences and displays it as a normal notification.</span></span> <span data-ttu-id="f3c8f-142">Meddelandet verkar app användaren exakt samma sätt som andra push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-142">The notification looks to the app user exactly like any other push notification.</span></span>

<span data-ttu-id="f3c8f-143">Observera att det är bättre att hantera de egenskap som saknas autentisering sidhuvud eller underkännande av serverdelen.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-143">Note that it is preferable to handle the cases of missing authentication header property or rejection by the back-end.</span></span> <span data-ttu-id="f3c8f-144">Särskild hantering av dessa fall beror huvudsakligen på användarupplevelsen mål.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-144">The specific handling of these cases depend mostly on your target user experience.</span></span> <span data-ttu-id="f3c8f-145">Ett alternativ är att visa ett meddelande med en allmän fråga att autentisera användaren att hämta anmälan.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-145">One option is to display a notification with a generic prompt for the user to authenticate to retrieve the actual notification.</span></span>

## <a name="run-the-application"></a><span data-ttu-id="f3c8f-146">Köra programmet</span><span class="sxs-lookup"><span data-stu-id="f3c8f-146">Run the Application</span></span>
<span data-ttu-id="f3c8f-147">Om du vill köra programmet, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="f3c8f-147">To run the application, do the following:</span></span>

1. <span data-ttu-id="f3c8f-148">Kontrollera att **AppBackend** distribueras i Azure.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-148">Make sure **AppBackend** is deployed in Azure.</span></span> <span data-ttu-id="f3c8f-149">Om du använder Visual Studio, köra den **AppBackend** Web API-program.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-149">If using Visual Studio, run the **AppBackend** Web API application.</span></span> <span data-ttu-id="f3c8f-150">En ASP.NET-webbsida visas.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-150">An ASP.NET web page is displayed.</span></span>
2. <span data-ttu-id="f3c8f-151">Kör appen på en fysisk Android-enhet eller emulatorn i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-151">In Eclipse, run the app on a physical Android device or the emulator.</span></span>
3. <span data-ttu-id="f3c8f-152">Ange ett användarnamn och lösenord i Användargränssnittet Android app.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-152">In the Android app UI, enter a username and password.</span></span> <span data-ttu-id="f3c8f-153">Det kan vara valfri sträng, men de måste ha samma värde.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-153">These can be any string, but they must be the same value.</span></span>
4. <span data-ttu-id="f3c8f-154">I Android-appen UI, klickar du på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-154">In the Android app UI, click **Log in**.</span></span> <span data-ttu-id="f3c8f-155">Klicka på **skicka push**.</span><span class="sxs-lookup"><span data-stu-id="f3c8f-155">Then click **Send push**.</span></span>

