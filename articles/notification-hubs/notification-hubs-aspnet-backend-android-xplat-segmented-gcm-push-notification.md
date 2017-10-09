---
title: "aaaNotification bryter nyheter kurs om Händelsehubbar - Android"
description: "Lär dig hur toouse Azure Service Bus Notification Hubs toosend bryter nyheter meddelanden tooAndroid enheter."
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
ms.openlocfilehash: e6eb41bec95c67d7dc059f560194966d04400494
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-notification-hubs-toosend-breaking-news"></a><span data-ttu-id="c1f12-103">Använda Notification Hubs toosend senaste nytt</span><span class="sxs-lookup"><span data-stu-id="c1f12-103">Use Notification Hubs toosend breaking news</span></span>
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a><span data-ttu-id="c1f12-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="c1f12-104">Overview</span></span>
<span data-ttu-id="c1f12-105">Det här avsnittet beskrivs hur du toouse Azure Notification Hubs toobroadcast senaste nyheterna meddelanden tooan Android-app.</span><span class="sxs-lookup"><span data-stu-id="c1f12-105">This topic shows you how toouse Azure Notification Hubs toobroadcast breaking news notifications tooan Android app.</span></span> <span data-ttu-id="c1f12-106">När du är klar kommer du att kan tooregister för att analysera nyhetskategorier som du är intresserad av och får endast push-meddelanden för dessa kategorier.</span><span class="sxs-lookup"><span data-stu-id="c1f12-106">When complete, you will be able tooregister for breaking news categories you are interested in, and receive only push notifications for those categories.</span></span> <span data-ttu-id="c1f12-107">Det här scenariot är ett vanligt mönster för många appar där meddelanden har toobe skickas toogroups med användare som har tidigare ha deklarerats intresse för dem, t.ex. RSS-läsare, appar för musik fläktar, osv.</span><span class="sxs-lookup"><span data-stu-id="c1f12-107">This scenario is a common pattern for many apps where notifications have toobe sent toogroups of users that have previously declared interest in them, e.g. RSS reader, apps for music fans, etc.</span></span>

<span data-ttu-id="c1f12-108">Broadcast scenarier aktiveras genom att inkludera en eller flera *taggar* när du skapar en registrering i hello meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="c1f12-108">Broadcast scenarios are enabled by including one or more *tags* when creating a registration in hello notification hub.</span></span> <span data-ttu-id="c1f12-109">När meddelanden skickas tooa tagg, får alla enheter som har registrerats för hello tagg hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="c1f12-109">When notifications are sent tooa tag, all devices that have registered for hello tag will receive hello notification.</span></span> <span data-ttu-id="c1f12-110">Eftersom taggar är bara strängar kan har de inte toobe etableras i förväg.</span><span class="sxs-lookup"><span data-stu-id="c1f12-110">Because tags are simply strings, they do not have toobe provisioned in advance.</span></span> <span data-ttu-id="c1f12-111">Mer information om taggar finns för[Notification Hubs Routning och Tagguttryck](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="c1f12-111">For more information about tags, refer too[Notification Hubs Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c1f12-112">Krav</span><span class="sxs-lookup"><span data-stu-id="c1f12-112">Prerequisites</span></span>
<span data-ttu-id="c1f12-113">Det här avsnittet bygger på hello-app som du skapade i [Kom igång med Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="c1f12-113">This topic builds on hello app you created in [Get started with Notification Hubs][get-started].</span></span> <span data-ttu-id="c1f12-114">Innan du börjar den här kursen ska du måste redan har slutfört [Kom igång med Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="c1f12-114">Before starting this tutorial, you must have already completed [Get started with Notification Hubs][get-started].</span></span>

## <a name="add-category-selection-toohello-app"></a><span data-ttu-id="c1f12-115">Lägg till kategori markeringen toohello app</span><span class="sxs-lookup"><span data-stu-id="c1f12-115">Add category selection toohello app</span></span>
<span data-ttu-id="c1f12-116">hello första steget är tooadd hello UI-element tooyour befintliga huvudaktiviteten som möjliggör hello användaren tooselect kategorier tooregister.</span><span class="sxs-lookup"><span data-stu-id="c1f12-116">hello first step is tooadd hello UI elements tooyour existing main activity that enable hello user tooselect categories tooregister.</span></span> <span data-ttu-id="c1f12-117">hello kategorier som är markerad som en användare som lagras på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="c1f12-117">hello categories selected by a user are stored on hello device.</span></span> <span data-ttu-id="c1f12-118">När hello appen startar, skapas en enhetsregistrering i meddelandehubben med hello valda kategorier som taggar.</span><span class="sxs-lookup"><span data-stu-id="c1f12-118">When hello app starts, a device registration is created in your notification hub with hello selected categories as tags.</span></span>

1. <span data-ttu-id="c1f12-119">Öppna filen res/layout/activity_main.xml och ersätta hello innehåll med hello följande:</span><span class="sxs-lookup"><span data-stu-id="c1f12-119">Open your res/layout/activity_main.xml file, and substitute hello content with hello following:</span></span>
   
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
2. <span data-ttu-id="c1f12-120">Öppna filen res/values/strings.xml och lägga till hello följande rader:</span><span class="sxs-lookup"><span data-stu-id="c1f12-120">Open your res/values/strings.xml file and add hello following lines:</span></span>
   
        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>
   
    <span data-ttu-id="c1f12-121">Main_activity.xml grafiska layouten bör nu se ut så här:</span><span class="sxs-lookup"><span data-stu-id="c1f12-121">Your main_activity.xml graphical layout should now look like this:</span></span>
   
    ![][A1]
3. <span data-ttu-id="c1f12-122">Nu skapa en klass **meddelanden** i hello samma paket som din **MainActivity** klass.</span><span class="sxs-lookup"><span data-stu-id="c1f12-122">Now create a class **Notifications** in hello same package as your **MainActivity** class.</span></span>
   
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
                            Log.e("MainActivity", "Failed tooregister - " + e.getMessage());
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
   
    <span data-ttu-id="c1f12-123">Den här klassen används hello lokal lagring toostore hello kategorier av nyheter som att den här enheten har tooreceive.</span><span class="sxs-lookup"><span data-stu-id="c1f12-123">This class uses hello local storage toostore hello categories of news that this device has tooreceive.</span></span> <span data-ttu-id="c1f12-124">Det innehåller även metoder tooregister för dessa kategorier.</span><span class="sxs-lookup"><span data-stu-id="c1f12-124">It also contains methods tooregister for these categories.</span></span>
4. <span data-ttu-id="c1f12-125">I din **MainActivity** klassen ta bort din privata fält för **NotificationHub** och **GoogleCloudMessaging**, och Lägg till ett fält för **meddelanden**:</span><span class="sxs-lookup"><span data-stu-id="c1f12-125">In your **MainActivity** class remove your private fields for **NotificationHub** and **GoogleCloudMessaging**, and add a field for **Notifications**:</span></span>
   
        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;
5. <span data-ttu-id="c1f12-126">Sedan hello **onCreate** metod, ta bort hello initieringen av hello **hubb** fältet och hello **registerWithNotificationHubs** metod.</span><span class="sxs-lookup"><span data-stu-id="c1f12-126">Then, in hello **onCreate** method, remove hello initialization of hello **hub** field and hello **registerWithNotificationHubs** method.</span></span> <span data-ttu-id="c1f12-127">Lägg till följande rader som initierar en instans för hello hello **meddelanden** klass.</span><span class="sxs-lookup"><span data-stu-id="c1f12-127">Then add hello following lines which initialize an instance of hello **Notifications** class.</span></span> 

        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            MyHandler.mainActivity = this;

            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);

            notifications = new Notifications(this, SENDER_ID, HubName, HubListenConnectionString);

            notifications.subscribeToCategories(notifications.retrieveCategories());
        }

    <span data-ttu-id="c1f12-128">`HubName`och `HubListenConnectionString` redan ska anges med hello `<hub name>` och `<connection string with listen access>` -platshållare med notification hub namn och hello anslutningssträngen för *DefaultListenSharedAccessSignature* som du fick tidigare.</span><span class="sxs-lookup"><span data-stu-id="c1f12-128">`HubName` and `HubListenConnectionString` should already be set with hello `<hub name>` and `<connection string with listen access>` placeholders with your notification hub name and hello connection string for *DefaultListenSharedAccessSignature* that you obtained earlier.</span></span>

    > [AZURE.NOTE] <span data-ttu-id="c1f12-129">Eftersom autentiseringsuppgifterna som distribueras med ett klientprogram inte är vanligtvis säker kan distribuera du endast hello nyckel för lyssna åtkomst med din klientapp.</span><span class="sxs-lookup"><span data-stu-id="c1f12-129">Because credentials that are distributed with a client app are not generally secure, you should only distribute hello key for listen access with your client app.</span></span> <span data-ttu-id="c1f12-130">Lyssna åtkomst aktiverar inte går att ändra din app tooregister för meddelanden, men befintliga registreringar och går inte att skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c1f12-130">Listen access enables your app tooregister for notifications, but existing registrations cannot be modified and notifications cannot be sent.</span></span> <span data-ttu-id="c1f12-131">hello fullständig åtkomstnyckel används i en skyddad backend-tjänst för att skicka meddelanden och ändra befintliga registreringar.</span><span class="sxs-lookup"><span data-stu-id="c1f12-131">hello full access key is used in a secured backend service for sending notifications and changing existing registrations.</span></span>


1. <span data-ttu-id="c1f12-132">Lägg sedan till följande hello importerar och `subscribe` metoden toohandle hello prenumerera knappen klickar du på händelse:</span><span class="sxs-lookup"><span data-stu-id="c1f12-132">Then, add hello following imports and `subscribe` method toohandle hello subscribe button click event:</span></span>
   
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
   
    <span data-ttu-id="c1f12-133">Den här metoden skapar en lista över kategorier och använder hello **meddelanden** klassen toostore hello listan i hello lokal lagring och registrera hello motsvarande taggar med meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="c1f12-133">This method creates a list of categories and uses hello **Notifications** class toostore hello list in hello local storage and register hello corresponding tags with your notification hub.</span></span> <span data-ttu-id="c1f12-134">När kategorier ändras, återskapas hello registrering med hello nya kategorier.</span><span class="sxs-lookup"><span data-stu-id="c1f12-134">When categories are changed, hello registration is recreated with hello new categories.</span></span>

<span data-ttu-id="c1f12-135">Appen är nu kan toostore en uppsättning kategorier i lokal lagring på hello enheten och registrerar med hello notification hub när hello användarändringar hello val av kategorier.</span><span class="sxs-lookup"><span data-stu-id="c1f12-135">Your app is now able toostore a set of categories in local storage on hello device and register with hello notification hub whenever hello user changes hello selection of categories.</span></span>

## <a name="register-for-notifications"></a><span data-ttu-id="c1f12-136">Registrera dig för meddelanden</span><span class="sxs-lookup"><span data-stu-id="c1f12-136">Register for notifications</span></span>
<span data-ttu-id="c1f12-137">De här stegen registrera hello meddelandehubben på Start med hello kategorier som har lagrats i lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="c1f12-137">These steps register with hello notification hub on startup using hello categories that have been stored in local storage.</span></span>

> [!NOTE]
> <span data-ttu-id="c1f12-138">Eftersom hello registrationId som tilldelats av Google Cloud Messaging (GCM) kan ändras när som helst, bör du registrera för meddelanden ofta tooavoid meddelandet-fel.</span><span class="sxs-lookup"><span data-stu-id="c1f12-138">Because hello registrationId assigned by Google Cloud Messaging (GCM) can change at any time, you should register for notifications frequently tooavoid notification failures.</span></span> <span data-ttu-id="c1f12-139">Det här exemplet registrerar för meddelande varje gång hello appen startas.</span><span class="sxs-lookup"><span data-stu-id="c1f12-139">This example registers for notification every time that hello app starts.</span></span> <span data-ttu-id="c1f12-140">För appar som körs ofta mer än en gång om dagen, du kan förmodligen hoppa över registrering toopreserve bandbredd om mindre än en dag har gått sedan hello tidigare registreringen.</span><span class="sxs-lookup"><span data-stu-id="c1f12-140">For apps that are run frequently, more than once a day, you can probably skip registration toopreserve bandwidth if less than a day has passed since hello previous registration.</span></span>
> 
> 

1. <span data-ttu-id="c1f12-141">Lägg till följande kod hello slutet av hello hello **onCreate** metod i hello **MainActivity** klass:</span><span class="sxs-lookup"><span data-stu-id="c1f12-141">Add hello following code at hello end of hello **onCreate** method in hello **MainActivity** class:</span></span>
   
        notifications.subscribeToCategories(notifications.retrieveCategories());
   
    <span data-ttu-id="c1f12-142">Detta säkerställer att varje gång hello appen startar hämtas hello kategorier från lokal lagring och begär en registrering för dessa kategorier.</span><span class="sxs-lookup"><span data-stu-id="c1f12-142">This makes sure that every time hello app starts it retrieves hello categories from local storage and requests a registeration for these categories.</span></span> 
2. <span data-ttu-id="c1f12-143">Uppdatera hello `onStart()` metod för hello `MainActivity` klassen enligt följande:</span><span class="sxs-lookup"><span data-stu-id="c1f12-143">Then update hello `onStart()` method of hello `MainActivity` class as follows:</span></span>
   
    <span data-ttu-id="c1f12-144">@Overrideskyddade void onStart() {</span><span class="sxs-lookup"><span data-stu-id="c1f12-144">@Override  protected void onStart() {</span></span>
   
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
    <span data-ttu-id="c1f12-145">}</span><span class="sxs-lookup"><span data-stu-id="c1f12-145">}</span></span>
   
    <span data-ttu-id="c1f12-146">Detta uppdaterar hello huvudaktiviteten baserat på status för hello tidigare sparad kategorier.</span><span class="sxs-lookup"><span data-stu-id="c1f12-146">This updates hello main activity based on hello status of previously saved categories.</span></span>

<span data-ttu-id="c1f12-147">hello appen är nu klar och kan lagra en uppsättning kategorier i hello enheten lokal lagring används tooregister hello meddelandehubben varje gång hello användarändringar hello val av kategorier.</span><span class="sxs-lookup"><span data-stu-id="c1f12-147">hello app is now complete and can store a set of categories in hello device local storage used tooregister with hello notification hub whenever hello user changes hello selection of categories.</span></span> <span data-ttu-id="c1f12-148">Nu ska definierar vi en serverdel som kan skicka kategori meddelanden toothis app.</span><span class="sxs-lookup"><span data-stu-id="c1f12-148">Next, we will define a backend that can send category notifications toothis app.</span></span>

## <a name="sending-tagged-notifications"></a><span data-ttu-id="c1f12-149">Skicka taggade meddelanden</span><span class="sxs-lookup"><span data-stu-id="c1f12-149">Sending tagged notifications</span></span>
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-hello-app-and-generate-notifications"></a><span data-ttu-id="c1f12-150">Kör hello app och generera meddelanden</span><span class="sxs-lookup"><span data-stu-id="c1f12-150">Run hello app and generate notifications</span></span>
1. <span data-ttu-id="c1f12-151">Skapa hello appen och starta den på en enhet eller emulator i Android Studio.</span><span class="sxs-lookup"><span data-stu-id="c1f12-151">In Android Studio, build hello app and start it on a device or emulator.</span></span>
   
    <span data-ttu-id="c1f12-152">Obs hello appen Användargränssnittet innehåller en uppsättning växlar som kan du välja hello kategorier toosubscribe till.</span><span class="sxs-lookup"><span data-stu-id="c1f12-152">Note that hello app UI provides a set of toggles that lets you choose hello categories toosubscribe to.</span></span>
2. <span data-ttu-id="c1f12-153">Aktivera en eller flera kategorier växlar och klicka sedan på **prenumerera**.</span><span class="sxs-lookup"><span data-stu-id="c1f12-153">Enable one or more categories toggles, then click **Subscribe**.</span></span>
   
    <span data-ttu-id="c1f12-154">hello appen konverterar hello valda kategorier till taggar och begär en ny registrering av enheten för hello valt taggar från hello meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="c1f12-154">hello app converts hello selected categories into tags and requests a new device registration for hello selected tags from hello notification hub.</span></span> <span data-ttu-id="c1f12-155">hello registrerade kategorier returneras och visas i ett popup-meddelande.</span><span class="sxs-lookup"><span data-stu-id="c1f12-155">hello registered categories are returned and displayed in a toast notification.</span></span>
3. <span data-ttu-id="c1f12-156">Skicka ett nytt meddelande genom att köra hello .NET-konsolapp.</span><span class="sxs-lookup"><span data-stu-id="c1f12-156">Send a new notification by running hello .NET Console app.</span></span>  <span data-ttu-id="c1f12-157">Du kan också skicka taggade mall-meddelanden med hello felsökningsfliken i meddelandehubben i hello [klassiska Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="c1f12-157">Alternatively, you can send tagged template notifications using hello debug tab of your notification hub in hello [Azure Classic Portal].</span></span>
   
    <span data-ttu-id="c1f12-158">Meddelanden om hello valda kategorier visas som popup-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c1f12-158">Notifications for hello selected categories appear as toast notifications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c1f12-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c1f12-159">Next steps</span></span>
<span data-ttu-id="c1f12-160">I den här självstudiekursen vi lärt dig hur toobroadcast senaste nytt efter kategori.</span><span class="sxs-lookup"><span data-stu-id="c1f12-160">In this tutorial we learned how toobroadcast breaking news by category.</span></span> <span data-ttu-id="c1f12-161">Överväg att fylla i en av följande kurser som markerar andra scenarion med avancerad Meddelandehubbar hello:</span><span class="sxs-lookup"><span data-stu-id="c1f12-161">Consider completing one of hello following tutorials that highlight other advanced Notification Hubs scenarios:</span></span>

* <span data-ttu-id="c1f12-162">[Använda Notification Hubs toobroadcast lokaliserade senaste nyheterna]</span><span class="sxs-lookup"><span data-stu-id="c1f12-162">[Use Notification Hubs toobroadcast localized breaking news]</span></span>
  
    <span data-ttu-id="c1f12-163">Lär dig hur lokaliserade tooexpand hello bryta nyheter app tooenable skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c1f12-163">Learn how tooexpand hello breaking news app tooenable sending localized notifications.</span></span>

<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[get-started]: notification-hubs-android-push-notification-google-gcm-get-started.md
[Använda Notification Hubs toobroadcast lokaliserade senaste nyheterna]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-toofor Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[klassiska Azure-portalen]: https://manage.windowsazure.com
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
