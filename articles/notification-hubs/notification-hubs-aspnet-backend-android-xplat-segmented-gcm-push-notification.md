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
# <a name="use-notification-hubs-toosend-breaking-news"></a>Använda Notification Hubs toosend senaste nytt
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Översikt
Det här avsnittet beskrivs hur du toouse Azure Notification Hubs toobroadcast senaste nyheterna meddelanden tooan Android-app. När du är klar kommer du att kan tooregister för att analysera nyhetskategorier som du är intresserad av och får endast push-meddelanden för dessa kategorier. Det här scenariot är ett vanligt mönster för många appar där meddelanden har toobe skickas toogroups med användare som har tidigare ha deklarerats intresse för dem, t.ex. RSS-läsare, appar för musik fläktar, osv.

Broadcast scenarier aktiveras genom att inkludera en eller flera *taggar* när du skapar en registrering i hello meddelandehubben. När meddelanden skickas tooa tagg, får alla enheter som har registrerats för hello tagg hello-meddelande. Eftersom taggar är bara strängar kan har de inte toobe etableras i förväg. Mer information om taggar finns för[Notification Hubs Routning och Tagguttryck](notification-hubs-tags-segment-push-message.md).

## <a name="prerequisites"></a>Krav
Det här avsnittet bygger på hello-app som du skapade i [Kom igång med Notification Hubs][get-started]. Innan du börjar den här kursen ska du måste redan har slutfört [Kom igång med Notification Hubs][get-started].

## <a name="add-category-selection-toohello-app"></a>Lägg till kategori markeringen toohello app
hello första steget är tooadd hello UI-element tooyour befintliga huvudaktiviteten som möjliggör hello användaren tooselect kategorier tooregister. hello kategorier som är markerad som en användare som lagras på hello enhet. När hello appen startar, skapas en enhetsregistrering i meddelandehubben med hello valda kategorier som taggar.

1. Öppna filen res/layout/activity_main.xml och ersätta hello innehåll med hello följande:
   
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
2. Öppna filen res/values/strings.xml och lägga till hello följande rader:
   
        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>
   
    Main_activity.xml grafiska layouten bör nu se ut så här:
   
    ![][A1]
3. Nu skapa en klass **meddelanden** i hello samma paket som din **MainActivity** klass.
   
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
   
    Den här klassen används hello lokal lagring toostore hello kategorier av nyheter som att den här enheten har tooreceive. Det innehåller även metoder tooregister för dessa kategorier.
4. I din **MainActivity** klassen ta bort din privata fält för **NotificationHub** och **GoogleCloudMessaging**, och Lägg till ett fält för **meddelanden**:
   
        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;
5. Sedan hello **onCreate** metod, ta bort hello initieringen av hello **hubb** fältet och hello **registerWithNotificationHubs** metod. Lägg till följande rader som initierar en instans för hello hello **meddelanden** klass. 

        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            MyHandler.mainActivity = this;

            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);

            notifications = new Notifications(this, SENDER_ID, HubName, HubListenConnectionString);

            notifications.subscribeToCategories(notifications.retrieveCategories());
        }

    `HubName`och `HubListenConnectionString` redan ska anges med hello `<hub name>` och `<connection string with listen access>` -platshållare med notification hub namn och hello anslutningssträngen för *DefaultListenSharedAccessSignature* som du fick tidigare.

    > [AZURE.NOTE] Eftersom autentiseringsuppgifterna som distribueras med ett klientprogram inte är vanligtvis säker kan distribuera du endast hello nyckel för lyssna åtkomst med din klientapp. Lyssna åtkomst aktiverar inte går att ändra din app tooregister för meddelanden, men befintliga registreringar och går inte att skicka meddelanden. hello fullständig åtkomstnyckel används i en skyddad backend-tjänst för att skicka meddelanden och ändra befintliga registreringar.


1. Lägg sedan till följande hello importerar och `subscribe` metoden toohandle hello prenumerera knappen klickar du på händelse:
   
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
   
    Den här metoden skapar en lista över kategorier och använder hello **meddelanden** klassen toostore hello listan i hello lokal lagring och registrera hello motsvarande taggar med meddelandehubben. När kategorier ändras, återskapas hello registrering med hello nya kategorier.

Appen är nu kan toostore en uppsättning kategorier i lokal lagring på hello enheten och registrerar med hello notification hub när hello användarändringar hello val av kategorier.

## <a name="register-for-notifications"></a>Registrera dig för meddelanden
De här stegen registrera hello meddelandehubben på Start med hello kategorier som har lagrats i lokal lagring.

> [!NOTE]
> Eftersom hello registrationId som tilldelats av Google Cloud Messaging (GCM) kan ändras när som helst, bör du registrera för meddelanden ofta tooavoid meddelandet-fel. Det här exemplet registrerar för meddelande varje gång hello appen startas. För appar som körs ofta mer än en gång om dagen, du kan förmodligen hoppa över registrering toopreserve bandbredd om mindre än en dag har gått sedan hello tidigare registreringen.
> 
> 

1. Lägg till följande kod hello slutet av hello hello **onCreate** metod i hello **MainActivity** klass:
   
        notifications.subscribeToCategories(notifications.retrieveCategories());
   
    Detta säkerställer att varje gång hello appen startar hämtas hello kategorier från lokal lagring och begär en registrering för dessa kategorier. 
2. Uppdatera hello `onStart()` metod för hello `MainActivity` klassen enligt följande:
   
    @Overrideskyddade void onStart() {
   
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
    }
   
    Detta uppdaterar hello huvudaktiviteten baserat på status för hello tidigare sparad kategorier.

hello appen är nu klar och kan lagra en uppsättning kategorier i hello enheten lokal lagring används tooregister hello meddelandehubben varje gång hello användarändringar hello val av kategorier. Nu ska definierar vi en serverdel som kan skicka kategori meddelanden toothis app.

## <a name="sending-tagged-notifications"></a>Skicka taggade meddelanden
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-hello-app-and-generate-notifications"></a>Kör hello app och generera meddelanden
1. Skapa hello appen och starta den på en enhet eller emulator i Android Studio.
   
    Obs hello appen Användargränssnittet innehåller en uppsättning växlar som kan du välja hello kategorier toosubscribe till.
2. Aktivera en eller flera kategorier växlar och klicka sedan på **prenumerera**.
   
    hello appen konverterar hello valda kategorier till taggar och begär en ny registrering av enheten för hello valt taggar från hello meddelandehubben. hello registrerade kategorier returneras och visas i ett popup-meddelande.
3. Skicka ett nytt meddelande genom att köra hello .NET-konsolapp.  Du kan också skicka taggade mall-meddelanden med hello felsökningsfliken i meddelandehubben i hello [klassiska Azure-portalen].
   
    Meddelanden om hello valda kategorier visas som popup-meddelanden.

## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen vi lärt dig hur toobroadcast senaste nytt efter kategori. Överväg att fylla i en av följande kurser som markerar andra scenarion med avancerad Meddelandehubbar hello:

* [Använda Notification Hubs toobroadcast lokaliserade senaste nyheterna]
  
    Lär dig hur lokaliserade tooexpand hello bryta nyheter app tooenable skicka meddelanden.

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
