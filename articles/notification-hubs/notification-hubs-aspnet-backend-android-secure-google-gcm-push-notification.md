---
title: aaaSending Secure Push-meddelanden med Azure Notification Hubs
description: "Lär dig hur säker toosend push-meddelanden tooan Android-app från Azure. Kodexempel som skrivits i Java- och C#."
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
ms.openlocfilehash: d07943c4691ed07acb987086228ef565e6281d57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sending-secure-push-notifications-with-azure-notification-hubs"></a><span data-ttu-id="52781-105">Skicka säkra Push-meddelanden med Azure Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="52781-105">Sending Secure Push Notifications with Azure Notification Hubs</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="52781-106">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="52781-106">Windows Universal</span></span>](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [<span data-ttu-id="52781-107">iOS</span><span class="sxs-lookup"><span data-stu-id="52781-107">iOS</span></span>](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [<span data-ttu-id="52781-108">Android</span><span class="sxs-lookup"><span data-stu-id="52781-108">Android</span></span>](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="52781-109">Översikt</span><span class="sxs-lookup"><span data-stu-id="52781-109">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="52781-110">toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="52781-110">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="52781-111">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="52781-111">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="52781-112">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="52781-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).</span></span>
> 
> 

<span data-ttu-id="52781-113">Stöd för push-meddelanden i Microsoft Azure kan du tooaccess en lätt att använda flera plattformar, skalats ut push meddelandet infrastruktur, vilket förenklar hello implementering av push-meddelanden för konsument- och enterprise-program för mobila plattformar.</span><span class="sxs-lookup"><span data-stu-id="52781-113">Push notification support in Microsoft Azure enables you tooaccess an easy-to-use, multiplatform, scaled-out push message infrastructure, which greatly simplifies hello implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span>

<span data-ttu-id="52781-114">På grund av tooregulatory eller säkerhet begränsningar kanske ibland ett program tooinclude någonting i hello-meddelande som kan överföras via hello standard push-infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="52781-114">Due tooregulatory or security constraints, sometimes an application might want tooinclude something in hello notification that cannot be transmitted through hello standard push notification infrastructure.</span></span> <span data-ttu-id="52781-115">Den här självstudiekursen beskrivs hur tooachieve hello samma upplevelse genom att skicka känslig information via en säker och autentiserad anslutning mellan hello klienten Android-enhet och hello-appserverdelen.</span><span class="sxs-lookup"><span data-stu-id="52781-115">This tutorial describes how tooachieve hello same experience by sending sensitive information through a secure, authenticated connection between hello client Android device and hello app backend.</span></span>

<span data-ttu-id="52781-116">På en hög nivå är hello flödet följande:</span><span class="sxs-lookup"><span data-stu-id="52781-116">At a high level, hello flow is as follows:</span></span>

1. <span data-ttu-id="52781-117">hello appens serverdel:</span><span class="sxs-lookup"><span data-stu-id="52781-117">hello app back-end:</span></span>
   * <span data-ttu-id="52781-118">Lagrar säker nyttolast i backend-databas.</span><span class="sxs-lookup"><span data-stu-id="52781-118">Stores secure payload in back-end database.</span></span>
   * <span data-ttu-id="52781-119">Skickar hello-ID för det här meddelandet toohello Android-enhet (ingen säker information skickas).</span><span class="sxs-lookup"><span data-stu-id="52781-119">Sends hello ID of this notification toohello Android device (no secure information is sent).</span></span>
2. <span data-ttu-id="52781-120">hello appen på hello enhet, när du tar emot hello-meddelande:</span><span class="sxs-lookup"><span data-stu-id="52781-120">hello app on hello device, when receiving hello notification:</span></span>
   * <span data-ttu-id="52781-121">hello Android-enhet kontaktar hello backend-begärande hello säker nyttolast.</span><span class="sxs-lookup"><span data-stu-id="52781-121">hello Android device contacts hello back-end requesting hello secure payload.</span></span>
   * <span data-ttu-id="52781-122">hello app kan du visa hello-nyttolast som ett meddelande på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="52781-122">hello app can show hello payload as a notification on hello device.</span></span>

<span data-ttu-id="52781-123">Det är viktigt toonote att vi antar hello enheten i hello föregående flöde (och i den här självstudiekursen) lagrar en autentiseringstoken i lokal lagring när hello användare loggar in.</span><span class="sxs-lookup"><span data-stu-id="52781-123">It is important toonote that in hello preceding flow (and in this tutorial), we assume that hello device stores an authentication token in local storage, after hello user logs in.</span></span> <span data-ttu-id="52781-124">Detta garanterar en helt integrerad upplevelse som hello enhet kan hämta hello-meddelande säker nyttolast med denna token.</span><span class="sxs-lookup"><span data-stu-id="52781-124">This guarantees a completely seamless experience, as hello device can retrieve hello notification’s secure payload using this token.</span></span> <span data-ttu-id="52781-125">Om ditt program inte kan lagra autentiseringstoken på hello enhet eller om dessa token kan ha gått, ska hello enhetsapp vid mottagning av hello push-meddelanden visa ett allmänt meddelande som uppmanar hello användaren toolaunch hello app.</span><span class="sxs-lookup"><span data-stu-id="52781-125">If your application does not store authentication tokens on hello device, or if these tokens can be expired, hello device app, upon receiving hello push notification should display a generic notification prompting hello user toolaunch hello app.</span></span> <span data-ttu-id="52781-126">hello app sedan autentiserar hello användare och visar hello notification nyttolast.</span><span class="sxs-lookup"><span data-stu-id="52781-126">hello app then authenticates hello user and shows hello notification payload.</span></span>

<span data-ttu-id="52781-127">Den här kursen visar hur säker toosend push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="52781-127">This tutorial shows how toosend secure push notifications.</span></span> <span data-ttu-id="52781-128">Den bygger på hello [meddela användare](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) självstudier, så bör du genomföra hello stegen i den här självstudiekursen först om du inte redan har gjort.</span><span class="sxs-lookup"><span data-stu-id="52781-128">It builds on hello [Notify Users](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) tutorial, so you should complete hello steps in that tutorial first if you haven't already.</span></span>

> [!NOTE]
> <span data-ttu-id="52781-129">Den här kursen förutsätter att du har skapat och konfigurerat din meddelandehubb enligt beskrivningen i [komma igång med Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="52781-129">This tutorial assumes that you have created and configured your notification hub as described in [Getting Started with Notification Hubs (Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).</span></span>
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-hello-android-project"></a><span data-ttu-id="52781-130">Ändra hello Android-projekt</span><span class="sxs-lookup"><span data-stu-id="52781-130">Modify hello Android project</span></span>
<span data-ttu-id="52781-131">Nu när du har ändrat din app backend-toosend bara hello *id* för push-meddelanden du har toochange din Android-app toohandle att meddelanden och motringning backend-tooretrieve-hello säkra meddelandet toobe visas.</span><span class="sxs-lookup"><span data-stu-id="52781-131">Now that you modified your app back-end toosend just hello *id* of a push notification, you have toochange your Android app toohandle that notification and call back your back-end tooretrieve hello secure message toobe displayed.</span></span>
<span data-ttu-id="52781-132">tooachieve det här målet, har toomake till att din Android-app vet hur tooauthenticate med din serverdel när den tar emot hello push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="52781-132">tooachieve this goal, you have toomake sure that your Android app knows how tooauthenticate itself with your back-end when it receives hello push notifications.</span></span>

<span data-ttu-id="52781-133">Vi kommer nu att ändra hello *inloggning* flödet i ordning toosave hello autentisering huvudets värde i hello delade inställningar för din app.</span><span class="sxs-lookup"><span data-stu-id="52781-133">We will now modify hello *login* flow in order toosave hello authentication header value in hello shared preferences of your app.</span></span> <span data-ttu-id="52781-134">Liknande mekanismer kan vara används toostore någon autentiseringstoken (t.ex. OAuth-token) som hello app har toouse utan att användarens autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="52781-134">Analogous mechanisms can be used toostore any authentication token (e.g. OAuth tokens) that hello app will have toouse without requiring user credentials.</span></span>

1. <span data-ttu-id="52781-135">Lägg till följande konstanter hello överst i hello hello i projektet Android-app **MainActivity** klass:</span><span class="sxs-lookup"><span data-stu-id="52781-135">In your Android app project, add hello following constants at hello top of hello **MainActivity** class:</span></span>
   
        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";
2. <span data-ttu-id="52781-136">Fortfarande i hello **MainActivity** klass, uppdatering hello `getAuthorizationHeader()` metoden toocontain hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="52781-136">Still in hello **MainActivity** class, update hello `getAuthorizationHeader()` method toocontain hello following code:</span></span>
   
        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);
   
            SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();
   
            return basicAuthHeader;
        }
3. <span data-ttu-id="52781-137">Lägg till följande hello `import` instruktioner överst hello i hello **MainActivity** fil:</span><span class="sxs-lookup"><span data-stu-id="52781-137">Add hello following `import` statements at hello top of hello **MainActivity** file:</span></span>
   
        import android.content.SharedPreferences;

<span data-ttu-id="52781-138">Nu ska vi ändra hello hanterare som anropas när hello meddelandet tas emot.</span><span class="sxs-lookup"><span data-stu-id="52781-138">Now we will change hello handler that is called when hello notification is received.</span></span>

1. <span data-ttu-id="52781-139">I hello **MyHandler** klassen ändra hello `OnReceive()` metoden toocontain:</span><span class="sxs-lookup"><span data-stu-id="52781-139">In hello **MyHandler** class change hello `OnReceive()` method toocontain:</span></span>
   
        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }
2. <span data-ttu-id="52781-140">Lägg sedan till hello `retrieveNotification()` metod, ersätter hello platshållare `{back-end endpoint}` med hello backend-slutpunkt som erhålls vid distribution av din serverdel:</span><span class="sxs-lookup"><span data-stu-id="52781-140">Then add hello `retrieveNotification()` method, replacing hello placeholder `{back-end endpoint}` with hello back-end endpoint obtained while deploying your back-end:</span></span>
   
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
                        Log.e("MainActivity", "Failed tooretrieve secure notification - " + e.getMessage());
                        return e;
                    }
                    return null;
                }
            }.execute(null, null, null);
        }

<span data-ttu-id="52781-141">Den här metoden anropar app backend-tooretrieve hello-meddelande innehåll med hello autentiseringsuppgifter lagras i hello delade inställningar och visar den som ett vanligt meddelande.</span><span class="sxs-lookup"><span data-stu-id="52781-141">This method calls your app back-end tooretrieve hello notification content using hello credentials stored in hello shared preferences and displays it as a normal notification.</span></span> <span data-ttu-id="52781-142">hello-meddelande verkar toohello app användaren exakt samma sätt som andra push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="52781-142">hello notification looks toohello app user exactly like any other push notification.</span></span>

<span data-ttu-id="52781-143">Observera att det är bättre toohandle hello fall egenskap som saknas autentisering sidhuvud eller avvisning hello serverdel.</span><span class="sxs-lookup"><span data-stu-id="52781-143">Note that it is preferable toohandle hello cases of missing authentication header property or rejection by hello back-end.</span></span> <span data-ttu-id="52781-144">hello Särskild hantering av dessa fall beror huvudsakligen på användarupplevelsen mål.</span><span class="sxs-lookup"><span data-stu-id="52781-144">hello specific handling of these cases depend mostly on your target user experience.</span></span> <span data-ttu-id="52781-145">Ett alternativ är toodisplay ett meddelande med en allmän fråga om hello tooauthenticate tooretrieve hello faktiska användarmeddelande.</span><span class="sxs-lookup"><span data-stu-id="52781-145">One option is toodisplay a notification with a generic prompt for hello user tooauthenticate tooretrieve hello actual notification.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="52781-146">Kör hello program</span><span class="sxs-lookup"><span data-stu-id="52781-146">Run hello Application</span></span>
<span data-ttu-id="52781-147">toorun Hej program, hello följande:</span><span class="sxs-lookup"><span data-stu-id="52781-147">toorun hello application, do hello following:</span></span>

1. <span data-ttu-id="52781-148">Kontrollera att **AppBackend** distribueras i Azure.</span><span class="sxs-lookup"><span data-stu-id="52781-148">Make sure **AppBackend** is deployed in Azure.</span></span> <span data-ttu-id="52781-149">Om du använder Visual Studio, köra hello **AppBackend** Web API-program.</span><span class="sxs-lookup"><span data-stu-id="52781-149">If using Visual Studio, run hello **AppBackend** Web API application.</span></span> <span data-ttu-id="52781-150">En ASP.NET-webbsida visas.</span><span class="sxs-lookup"><span data-stu-id="52781-150">An ASP.NET web page is displayed.</span></span>
2. <span data-ttu-id="52781-151">Kör hello app på en fysisk enhet eller hello androidemulator i Eclipse.</span><span class="sxs-lookup"><span data-stu-id="52781-151">In Eclipse, run hello app on a physical Android device or hello emulator.</span></span>
3. <span data-ttu-id="52781-152">Ange ett användarnamn och lösenord i hello Android app Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="52781-152">In hello Android app UI, enter a username and password.</span></span> <span data-ttu-id="52781-153">Dessa kan vara valfri sträng, men de måste vara hello samma värde.</span><span class="sxs-lookup"><span data-stu-id="52781-153">These can be any string, but they must be hello same value.</span></span>
4. <span data-ttu-id="52781-154">I hello Android UI-app, klickar du på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="52781-154">In hello Android app UI, click **Log in**.</span></span> <span data-ttu-id="52781-155">Klicka på **skicka push**.</span><span class="sxs-lookup"><span data-stu-id="52781-155">Then click **Send push**.</span></span>

