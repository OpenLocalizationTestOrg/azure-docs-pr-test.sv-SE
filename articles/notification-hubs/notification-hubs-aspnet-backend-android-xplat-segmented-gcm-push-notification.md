---
title: "Notification Hubs senaste nytt självstudiekursen - Android"
description: "Lär dig hur du använder Azure Service Bus Notification Hubs för att skicka senaste nyheterna meddelanden till Android-enheter."
services: notification-hubs
documentationcenter: android
author: ysxu
manager: erikre
editor: 
ms.assetid: 3c23cb80-9d35-4dde-b26d-a7bfd4cb8f81
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 76ec01c874fceedab7d76b2ef58e4b45b5489f58
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-notification-hubs-to-send-breaking-news"></a><span data-ttu-id="fd2c8-103">Använda Notification Hubs för att skicka de senaste nyheterna</span><span class="sxs-lookup"><span data-stu-id="fd2c8-103">Use Notification Hubs to send breaking news</span></span>
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a><span data-ttu-id="fd2c8-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="fd2c8-104">Overview</span></span>
<span data-ttu-id="fd2c8-105">Det här avsnittet visar hur du använder Azure Notification Hubs för att sända senaste nyheterna meddelanden till en Android-app.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-105">This topic shows you how to use Azure Notification Hubs to broadcast breaking news notifications to an Android app.</span></span> <span data-ttu-id="fd2c8-106">När du är klar kommer du att kunna registrera för att analysera nyhetskategorier som du är intresserad av och får endast push-meddelanden för dessa kategorier.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-106">When complete, you will be able to register for breaking news categories you are interested in, and receive only push notifications for those categories.</span></span> <span data-ttu-id="fd2c8-107">Det här scenariot är ett vanligt mönster för många appar där meddelanden har skickas till grupper av användare som har tidigare ha deklarerats intresse för dem, t.ex. RSS-läsare, appar för musik fläktar, osv.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-107">This scenario is a common pattern for many apps where notifications have to be sent to groups of users that have previously declared interest in them, e.g. RSS reader, apps for music fans, etc.</span></span>

<span data-ttu-id="fd2c8-108">Broadcast scenarier aktiveras genom att inkludera en eller flera *taggar* när du skapar en registrering i meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-108">Broadcast scenarios are enabled by including one or more *tags* when creating a registration in the notification hub.</span></span> <span data-ttu-id="fd2c8-109">När meddelanden skickas till en tagg för tar alla enheter som har registrerats för taggen emot meddelandet.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-109">When notifications are sent to a tag, all devices that have registered for the tag will receive the notification.</span></span> <span data-ttu-id="fd2c8-110">Eftersom taggar är bara strängar, behöver de inte etableras i förväg.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-110">Because tags are simply strings, they do not have to be provisioned in advance.</span></span> <span data-ttu-id="fd2c8-111">Mer information om taggar finns i [Notification Hubs Routning och Tagguttryck](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="fd2c8-111">For more information about tags, refer to [Notification Hubs Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fd2c8-112">Krav</span><span class="sxs-lookup"><span data-stu-id="fd2c8-112">Prerequisites</span></span>
<span data-ttu-id="fd2c8-113">Det här avsnittet bygger på appen som du skapade i [Kom igång med Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="fd2c8-113">This topic builds on the app you created in [Get started with Notification Hubs][get-started].</span></span> <span data-ttu-id="fd2c8-114">Innan du börjar den här kursen ska du måste redan har slutfört [Kom igång med Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="fd2c8-114">Before starting this tutorial, you must have already completed [Get started with Notification Hubs][get-started].</span></span>

## <a name="add-category-selection-to-the-app"></a><span data-ttu-id="fd2c8-115">Lägg till kategori markeringen i appen</span><span class="sxs-lookup"><span data-stu-id="fd2c8-115">Add category selection to the app</span></span>
<span data-ttu-id="fd2c8-116">Det första steget är att lägga till de UI-element i din befintliga huvudaktiviteten som gör att användaren kan välja kategorier för att registrera.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-116">The first step is to add the UI elements to your existing main activity that enable the user to select categories to register.</span></span> <span data-ttu-id="fd2c8-117">Vilka kategorier av en användare som lagras på enheten.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-117">The categories selected by a user are stored on the device.</span></span> <span data-ttu-id="fd2c8-118">När appen startar skapas en enhetsregistrering i din meddelandehubb med de valda kategorierna som taggar.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-118">When the app starts, a device registration is created in your notification hub with the selected categories as tags.</span></span>

1. <span data-ttu-id="fd2c8-119">Öppna filen res/layout/activity_main.xml och Ersätt innehållet med följande:</span><span class="sxs-lookup"><span data-stu-id="fd2c8-119">Open your res/layout/activity_main.xml file, and substitute the content with the following:</span></span>
   
        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:paddingBottom="@dimen/activity_vertical_margin"
            android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            tools:context="com.example.breakingnews.MainActivity"
            android:orientation="vertical">
   
                <CheckBox
                    android:id="@+id/worldBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_world" />
                <CheckBox
                    android:id="@+id/politicsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_politics" />
                <CheckBox
                    android:id="@+id/businessBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_business" />
                <CheckBox
                    android:id="@+id/technologyBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_technology" />
                <CheckBox
                    android:id="@+id/scienceBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_science" />
                <CheckBox
                    android:id="@+id/sportsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_sports" />
                <Button
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:onClick="subscribe"
                    android:text="@string/button_subscribe" />
        </LinearLayout>
2. <span data-ttu-id="fd2c8-120">Öppna filen res/values/strings.xml och Lägg till följande rader:</span><span class="sxs-lookup"><span data-stu-id="fd2c8-120">Open your res/values/strings.xml file and add the following lines:</span></span>
   
        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>
   
    <span data-ttu-id="fd2c8-121">Main_activity.xml grafiska layouten bör nu se ut så här:</span><span class="sxs-lookup"><span data-stu-id="fd2c8-121">Your main_activity.xml graphical layout should now look like this:</span></span>
   
    ![][A1]
3. <span data-ttu-id="fd2c8-122">Nu skapa en klass **meddelanden** i samma paket som din **MainActivity** klass.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-122">Now create a class **Notifications** in the same package as your **MainActivity** class.</span></span>
   
        import java.util.HashSet;
        import java.util.Set;
   
        import android.content.Context;
        import android.content.SharedPreferences;
        import android.os.AsyncTask;
        import android.util.Log;
        import android.widget.Toast;
   
        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.microsoft.windowsazure.messaging.NotificationHub;
   
        public class Notifications {
            private static final String PREFS_NAME = "BreakingNewsCategories";
            private GoogleCloudMessaging gcm;
            private NotificationHub hub;
            private Context context;
            private String senderId;
   
            public Notifications(Context context, String senderId, String hubName, 
                                    String listenConnectionString) {
                this.context = context;
                this.senderId = senderId;
   
                gcm = GoogleCloudMessaging.getInstance(context);
                hub = new NotificationHub(hubName, listenConnectionString, context);
            }
   
            public void storeCategoriesAndSubscribe(Set<String> categories)
            {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                settings.edit().putStringSet("categories", categories).commit();
                subscribeToCategories(categories);
            }
   
            public Set<String> retrieveCategories() {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                return settings.getStringSet("categories", new HashSet<String>());
            }
   
            public void subscribeToCategories(final Set<String> categories) {
                new AsyncTask<Object, Object, Object>() {
                    @Override
                    protected Object doInBackground(Object... params) {
                        try {
                            String regid = gcm.register(senderId);
   
                            String templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";
   
                            hub.registerTemplate(regid,"simpleGCMTemplate", templateBodyGCM, 
                                categories.toArray(new String[categories.size()]));
                        } catch (Exception e) {
                            Log.e("MainActivity", "Failed to register - " + e.getMessage());
                            return e;
                        }
                        return null;
                    }
   
                    protected void onPostExecute(Object result) {
                        String message = "Subscribed for categories: "
                                + categories.toString();
                        Toast.makeText(context, message,
                                Toast.LENGTH_LONG).show();
                    }
                }.execute(null, null, null);
            }
   
        }
   
    <span data-ttu-id="fd2c8-123">Den här klassen använder lokal lagring för att lagra kategorier av nyheter som den här enheten ska ta emot.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-123">This class uses the local storage to store the categories of news that this device has to receive.</span></span> <span data-ttu-id="fd2c8-124">Den innehåller också metoder för att registrera dig för dessa kategorier.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-124">It also contains methods to register for these categories.</span></span>
4. <span data-ttu-id="fd2c8-125">I din **MainActivity** klassen ta bort din privata fält för **NotificationHub** och **GoogleCloudMessaging**, och Lägg till ett fält för **meddelanden**:</span><span class="sxs-lookup"><span data-stu-id="fd2c8-125">In your **MainActivity** class remove your private fields for **NotificationHub** and **GoogleCloudMessaging**, and add a field for **Notifications**:</span></span>
   
        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;
5. <span data-ttu-id="fd2c8-126">I den **onCreate** metod, ta bort initieringen av den **hubb** fält och **registerWithNotificationHubs** metod.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-126">Then, in the **onCreate** method, remove the initialization of the **hub** field and the **registerWithNotificationHubs** method.</span></span> <span data-ttu-id="fd2c8-127">Lägg till följande rader som initierar en instans av den **meddelanden** klass.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-127">Then add the following lines which initialize an instance of the **Notifications** class.</span></span> 

        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            MyHandler.mainActivity = this;

            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);

            notifications = new Notifications(this, SENDER_ID, HubName, HubListenConnectionString);

            notifications.subscribeToCategories(notifications.retrieveCategories());
        }

    <span data-ttu-id="fd2c8-128">`HubName`och `HubListenConnectionString` redan ska anges med den `<hub name>` och `<connection string with listen access>` platshållarna med namnet på din meddelandehubb och anslutningssträngen för *DefaultListenSharedAccessSignature* som du fick tidigare.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-128">`HubName` and `HubListenConnectionString` should already be set with the `<hub name>` and `<connection string with listen access>` placeholders with your notification hub name and the connection string for *DefaultListenSharedAccessSignature* that you obtained earlier.</span></span>

    > [AZURE.NOTE] <span data-ttu-id="fd2c8-129">Eftersom autentiseringsuppgifterna som distribueras med ett klientprogram inte är vanligtvis säker kan distribuera du endast nyckel för lyssna åtkomst med din klientapp.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-129">Because credentials that are distributed with a client app are not generally secure, you should only distribute the key for listen access with your client app.</span></span> <span data-ttu-id="fd2c8-130">Lyssna åtkomst aktiverar inte går att ändra din app att registrera för meddelanden, men befintliga registreringar och går inte att skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-130">Listen access enables your app to register for notifications, but existing registrations cannot be modified and notifications cannot be sent.</span></span> <span data-ttu-id="fd2c8-131">Fullständig åtkomst-nyckeln används i en skyddad backend-tjänst för att skicka meddelanden och ändra befintliga registreringar.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-131">The full access key is used in a secured backend service for sending notifications and changing existing registrations.</span></span>


1. <span data-ttu-id="fd2c8-132">Lägg sedan till följande importer och `subscribe` metod för att hantera knappen prenumerera på händelse:</span><span class="sxs-lookup"><span data-stu-id="fd2c8-132">Then, add the following imports and `subscribe` method to handle the subscribe button click event:</span></span>
   
        import android.widget.CheckBox;
        import java.util.HashSet;
        import java.util.Set;
   
        public void subscribe(View sender) {
            final Set<String> categories = new HashSet<String>();
   
            CheckBox world = (CheckBox) findViewById(R.id.worldBox);
            if (world.isChecked())
                categories.add("world");
            CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
            if (politics.isChecked())
                categories.add("politics");
            CheckBox business = (CheckBox) findViewById(R.id.businessBox);
            if (business.isChecked())
                categories.add("business");
            CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
            if (technology.isChecked())
                categories.add("technology");
            CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
            if (science.isChecked())
                categories.add("science");
            CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
            if (sports.isChecked())
                categories.add("sports");
   
            notifications.storeCategoriesAndSubscribe(categories);
        }
   
    <span data-ttu-id="fd2c8-133">Den här metoden skapar en lista över kategorier och använder den **meddelanden** klass som lagrar listan i enhetens lokala lagring och registrera motsvarande taggar med meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-133">This method creates a list of categories and uses the **Notifications** class to store the list in the local storage and register the corresponding tags with your notification hub.</span></span> <span data-ttu-id="fd2c8-134">När kategorier ändras, återskapas registreringen med de nya kategorierna.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-134">When categories are changed, the registration is recreated with the new categories.</span></span>

<span data-ttu-id="fd2c8-135">Appen är nu kunna lagra en uppsättning kategorier i lokal lagring på enheten och registrera med notification hub när användaren ändrar valet av kategorier.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-135">Your app is now able to store a set of categories in local storage on the device and register with the notification hub whenever the user changes the selection of categories.</span></span>

## <a name="register-for-notifications"></a><span data-ttu-id="fd2c8-136">Registrera dig för meddelanden</span><span class="sxs-lookup"><span data-stu-id="fd2c8-136">Register for notifications</span></span>
<span data-ttu-id="fd2c8-137">De här stegen registrera med notification hub vid start med hjälp av kategorier som har lagrats i lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-137">These steps register with the notification hub on startup using the categories that have been stored in local storage.</span></span>

> [!NOTE]
> <span data-ttu-id="fd2c8-138">Eftersom registrationId som tilldelats av Google Cloud Messaging (GCM) kan ändras när som helst, bör du registrera dig för meddelanden ofta att undvika fel i meddelande.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-138">Because the registrationId assigned by Google Cloud Messaging (GCM) can change at any time, you should register for notifications frequently to avoid notification failures.</span></span> <span data-ttu-id="fd2c8-139">Det här exemplet registrerar för meddelande varje gång appen startar.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-139">This example registers for notification every time that the app starts.</span></span> <span data-ttu-id="fd2c8-140">För appar som körs ofta mer än en gång om dagen, du kan förmodligen hoppa över registrering för att bevara bandbredd om mindre än en dag har gått sedan den tidigare registreringen.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-140">For apps that are run frequently, more than once a day, you can probably skip registration to preserve bandwidth if less than a day has passed since the previous registration.</span></span>
> 
> 

1. <span data-ttu-id="fd2c8-141">Lägg till följande kod i slutet av den **onCreate** metod i den **MainActivity** klass:</span><span class="sxs-lookup"><span data-stu-id="fd2c8-141">Add the following code at the end of the **onCreate** method in the **MainActivity** class:</span></span>
   
        notifications.subscribeToCategories(notifications.retrieveCategories());
   
    <span data-ttu-id="fd2c8-142">Detta säkerställer att varje gång appen startas hämtas kategorier från lokal lagring och begär en registrering för dessa kategorier.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-142">This makes sure that every time the app starts it retrieves the categories from local storage and requests a registeration for these categories.</span></span> 
2. <span data-ttu-id="fd2c8-143">Uppdatera sedan den `onStart()` metod för den `MainActivity` klassen enligt följande:</span><span class="sxs-lookup"><span data-stu-id="fd2c8-143">Then update the `onStart()` method of the `MainActivity` class as follows:</span></span>
   
    <span data-ttu-id="fd2c8-144">@Overrideskyddade void onStart() {</span><span class="sxs-lookup"><span data-stu-id="fd2c8-144">@Override  protected void onStart() {</span></span>
   
        super.onStart();
        isVisible = true;
   
        Set<String> categories = notifications.retrieveCategories();
   
        CheckBox world = (CheckBox) findViewById(R.id.worldBox);
        world.setChecked(categories.contains("world"));
        CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
        politics.setChecked(categories.contains("politics"));
        CheckBox business = (CheckBox) findViewById(R.id.businessBox);
        business.setChecked(categories.contains("business"));
        CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
        technology.setChecked(categories.contains("technology"));
        CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
        science.setChecked(categories.contains("science"));
        CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
        sports.setChecked(categories.contains("sports"));
    <span data-ttu-id="fd2c8-145">}</span><span class="sxs-lookup"><span data-stu-id="fd2c8-145">}</span></span>
   
    <span data-ttu-id="fd2c8-146">Detta uppdaterar huvudaktiviteten baserat på status för tidigare sparad kategorier.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-146">This updates the main activity based on the status of previously saved categories.</span></span>

<span data-ttu-id="fd2c8-147">Appen är nu klar och kan lagra en uppsättning kategorier i enhetens lokala lagring som används för att registrera med notification hub när användaren ändrar valet av kategorier.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-147">The app is now complete and can store a set of categories in the device local storage used to register with the notification hub whenever the user changes the selection of categories.</span></span> <span data-ttu-id="fd2c8-148">Nu ska definierar vi en serverdel som kategori meddelanden kan skickas till den här appen.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-148">Next, we will define a backend that can send category notifications to this app.</span></span>

## <a name="sending-tagged-notifications"></a><span data-ttu-id="fd2c8-149">Skicka taggade meddelanden</span><span class="sxs-lookup"><span data-stu-id="fd2c8-149">Sending tagged notifications</span></span>
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-the-app-and-generate-notifications"></a><span data-ttu-id="fd2c8-150">Kör appen och generera meddelanden</span><span class="sxs-lookup"><span data-stu-id="fd2c8-150">Run the app and generate notifications</span></span>
1. <span data-ttu-id="fd2c8-151">I Android Studio skapar appen och starta den på en enhet eller emulator.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-151">In Android Studio, build the app and start it on a device or emulator.</span></span>
   
    <span data-ttu-id="fd2c8-152">Observera att appen Användargränssnittet innehåller en uppsättning växlar som låter dig välja kategorier för att prenumerera på.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-152">Note that the app UI provides a set of toggles that lets you choose the categories to subscribe to.</span></span>
2. <span data-ttu-id="fd2c8-153">Aktivera en eller flera kategorier växlar och klicka sedan på **prenumerera**.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-153">Enable one or more categories toggles, then click **Subscribe**.</span></span>
   
    <span data-ttu-id="fd2c8-154">Appen konverterar valda kategorier till taggar och begär en ny enhetsregistrering för de valda taggarna från meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-154">The app converts the selected categories into tags and requests a new device registration for the selected tags from the notification hub.</span></span> <span data-ttu-id="fd2c8-155">Registrerade kategorier returneras och visas i ett popup-meddelande.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-155">The registered categories are returned and displayed in a toast notification.</span></span>
3. <span data-ttu-id="fd2c8-156">Skicka ett nytt meddelande genom att köra .NET-konsolapp.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-156">Send a new notification by running the .NET Console app.</span></span>  <span data-ttu-id="fd2c8-157">Du kan också skicka taggade mall-meddelanden via felsökningsfliken i meddelandehubben i den [klassiska Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="fd2c8-157">Alternatively, you can send tagged template notifications using the debug tab of your notification hub in the [Azure Classic Portal].</span></span>
   
    <span data-ttu-id="fd2c8-158">Meddelanden i valda kategorier visas som popup-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-158">Notifications for the selected categories appear as toast notifications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fd2c8-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fd2c8-159">Next steps</span></span>
<span data-ttu-id="fd2c8-160">Vi lärt dig hur du broadcast senaste nyheterna efter kategori i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-160">In this tutorial we learned how to broadcast breaking news by category.</span></span> <span data-ttu-id="fd2c8-161">Överväg att fylla i en av följande kurser som markerar andra avancerade Meddelandehubbar scenarier:</span><span class="sxs-lookup"><span data-stu-id="fd2c8-161">Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:</span></span>

* <span data-ttu-id="fd2c8-162">[Använda Notification Hubs för att sända lokaliserade senaste nyheterna]</span><span class="sxs-lookup"><span data-stu-id="fd2c8-162">[Use Notification Hubs to broadcast localized breaking news]</span></span>
  
    <span data-ttu-id="fd2c8-163">Lär dig mer om att utöka senaste nyheterna appen om du vill aktivera skicka lokaliserade meddelanden.</span><span class="sxs-lookup"><span data-stu-id="fd2c8-163">Learn how to expand the breaking news app to enable sending localized notifications.</span></span>

<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[get-started]: notification-hubs-android-push-notification-google-gcm-get-started.md
<span data-ttu-id="fd2c8-164">[Använda Notification Hubs för att sända lokaliserade senaste nyheterna]: /manage/services/notification-hubs/breaking-news-localized-dotnet/</span><span class="sxs-lookup"><span data-stu-id="fd2c8-164">[Use Notification Hubs to broadcast localized breaking news]: /manage/services/notification-hubs/breaking-news-localized-dotnet/</span></span>
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
<span data-ttu-id="fd2c8-165">[klassiska Azure-portalen]: https://manage.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="fd2c8-165">[Azure Classic Portal]: https://manage.windowsazure.com</span></span>
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
