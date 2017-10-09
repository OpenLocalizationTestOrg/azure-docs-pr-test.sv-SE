---
title: aaaAzure Mobile Engagement Android SDK-Integration
description: "Senaste uppdateringarna och procedurer för Android SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 9ec3fab3-35ec-458e-bf41-6cdd69e3fa44
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 06/27/2016
ms.author: piyushjo
ms.openlocfilehash: 4ab6143771bdc0758a548abb529d6bde98fc0e4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-reach-on-android"></a>Hur tooIntegrate Engagement nå på Android
> [!IMPORTANT]
> Du måste följa hello integration beskrivs i hello hur tooIntegrate Engagement på Android dokumentet innan du följer den här guiden.
> 
> 

## <a name="standard-integration"></a>Standard-integrering

Kopiera Reach resursfiler från hello SDK i projektet:

* Kopiera hello filer från hello `res/layout` mappen levereras med hello SDK i hello `res/layout` för ditt program.
* Kopiera hello filer från hello `res/drawable` mappen levereras med hello SDK i hello `res/drawable` för ditt program.

Redigera din `AndroidManifest.xml` fil:

* Lägg till följande avsnitt hello (mellan hello `<application>` och `</application>` taggar):
  
          <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
              <category android:name="android.intent.category.DEFAULT" />
              <data android:mimeType="text/plain" />
            </intent-filter>
          </activity>
          <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
              <category android:name="android.intent.category.DEFAULT" />
              <data android:mimeType="text/html" />
            </intent-filter>
          </activity>
          <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
              <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
          </activity>
          <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
            <intent-filter>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
              <category android:name="android.intent.category.DEFAULT"/>
            </intent-filter>
          </activity>
          <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
            <intent-filter>
              <action android:name="android.intent.action.BOOT_COMPLETED"/>
              <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
              <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
              <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
            </intent-filter>
          </receiver>
          <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
            <intent-filter>
              <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
            </intent-filter>
          </receiver>
* Du behöver den här behörigheten tooreplay systemmeddelanden som inte har klickat på vid start (annars de sparas på disk men visas inte längre behöver du verkligen tooinclude detta).
  
          <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
* Ange en ikon som används för meddelanden (både i appen och system som) genom att kopiera och redigera efter avsnittet hello (mellan hello `<application>` och `</application>` taggar):
  
          <meta-data android:name="engagement:reach:notification:icon" android:value="<name_of_icon_WITHOUT_file_extension_and_WITHOUT_'@drawable/'>" />

> [!IMPORTANT]
> Det här avsnittet är **obligatoriska** om du tänker använda systemmeddelanden när du skapar Reach-kampanjer. Android förhindrar systemmeddelanden utan ikoner som visas. Så om du tar bort det här avsnittet slutanvändarna inte kommer att kunna tooreceive dem.
> 
> 

* Om du skapar kampanjer med systemmeddelanden med stor bild, behöver du tooadd hello följande behörigheter (efter hello `</application>` tagg) om de saknas:
  
          <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
          <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
  
  * På Android M och om ditt program riktar sig till Android API-nivå 23 eller högre, ``WRITE_EXTERNAL_STORAGE`` behörighet kräver användargodkännande av. Läs [i det här avsnittet](mobile-engagement-android-integrate-engagement.md#android-m-permissions).
* Nå kampanj för systemmeddelanden som du kan också ange i hello om hello enhet ska ringa och/eller vibrerar. För den toowork, du har toomake att du deklarerat hello följande behörighet (efter hello `</application>` tagg):
  
          <uses-permission android:name="android.permission.VIBRATE" />
  
  Utan den här behörigheten Android förhindrar systemmeddelanden visas när du har markerat hello ring eller hello vibrerar alternativ i hello Reach-kampanjhanterare.

## <a name="native-push"></a>Intern Push
Nu när du har konfigurerat Reach-modulen måste tooconfigure interna push toobe kan tooreceive hello kampanjer på hello enhet.

Vi stöder två tjänster på Android:

* Google Play-enheter: Använd [Google Cloud Messaging] av följande hello [hur tooIntegrate GCM med Engagement hjälper](mobile-engagement-android-gcm-integrate.md) guide.
* Amazon-enheter: Använd [Amazon Device Messaging] av följande hello [hur tooIntegrate ADM med Engagement hjälper](mobile-engagement-android-adm-integrate.md) guide.

Om du vill tootarget Amazon- och Google Play-enheter, dess möjliga toohave allt inuti 1 AndroidManifest.xml/APK för utveckling. Men när tooAmazon, de kan avvisa tillämpningsprogrammet om de hittar GCM-kod.

I så fall bör du använda flera APKs.

**Tillämpningsprogrammet är nu redo tooreceive och visa nå kampanjer!**

## <a name="how-toohandle-data-push"></a>Hur toohandle data push
### <a name="integration"></a>Integrering
Om du vill att din ansökan toobe kan tooreceive Reach data push-meddelanden måste du toocreate en underklass till `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` och referens i hello `AndroidManifest.xml` fil (mellan hello `<application>` och/eller `</application>` taggar):

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

Sedan kan du åsidosätta hello `onDataPushStringReceived` och `onDataPushBase64Received` återanrop. Här är ett exempel:

            public class MyDataPushReceiver extends EngagementReachDataPushReceiver
            {
              @Override
              protected Boolean onDataPushStringReceived(Context context, String category, String body)
              {
                Log.d("tmp", "String data push message received: " + body);
                return true;
              }

              @Override
              protected Boolean onDataPushBase64Received(Context context, String category, byte[] decodedBody, String encodedBody)
              {
                Log.d("tmp", "Base64 data push message received: " + encodedBody);
                // Do something useful with decodedBody like updating an image view
                return true;
              }
            }

### <a name="category"></a>Kategori
hello kategori parametern är valfri när du skapar en Data-Push-kampanj och gör att du toofilter data push-meddelanden. Detta är användbart om du har flera broadcast mottagare hantera olika typer av data-push, eller om du vill toopush olika typer av `Base64` data och vill tooidentify typ innan parsningen dem.

### <a name="callbacks-return-parameter"></a>Återanrop returparameter
Här följer några riktlinjer tooproperly referensen hello returparameter av `onDataPushStringReceived` och `onDataPushBase64Received`:

* En broadcast mottagare ska returnera `null` i hello återanrop om den inte vet hur toohandle en data-push. Du bör använda hello kategori toodetermine om broadcast mottagaren ska hantera hello data-push eller inte.
* En av hello broadcast mottagare ska returnera `true` i hello återanrop om den godkänner hello data-push.
* En av hello broadcast mottagare ska returnera `false` i hello återanrop om den identifierar hello data-push, men ignorerar oavsett orsak. Till exempel returnera `false` när hello mottagna data är ogiltiga.
* Om en broadcast-mottagare returnerar `true` medan en annan en returnerar `false` för hello samma data-push, hello beteendet är odefinierad gör aldrig som.

hello returtyp används endast för hello Reach statistik:

* `Replied`ökas om något av hello broadcast mottagare returneras antingen `true` eller `false`.
* `Actioned`ökas endast om en av hello broadcast mottagare returnerade `true`.

## <a name="how-toocustomize-campaigns"></a>Hur toocustomize kampanjer
Du kan ändra hello layouter i hello nå SDK toocustomize kampanjer.

Du bör hålla alla hello-identifierare som används i hello layouter och hålla hello typer av hello vyer som använder en identifierare, särskilt för text vyer och avbildningen vyer. Vissa vyer är bara använda toohide eller visa områden så att deras typen kan ändras. Kontrollera hello källkoden om du avser toochange hello typ av en vy i hello tillhandahålls layouter.

### <a name="notifications"></a>Meddelanden
Det finns två typer av aviseringar: systemet och i appen meddelanden som använder olika layoutfiler.

#### <a name="system-notifications"></a>Systemmeddelanden
toocustomize systemmeddelanden måste toouse hello **kategorier**. Du kan gå för[kategorier](#categories).

#### <a name="in-app-notifications"></a>Meddelanden i appen
Som standard ett meddelande i appen är en vy som läggs till dynamiskt toohello aktuell aktivitet användaren tack toohello Android gränssnittsmetod `addContentView()`. Detta kallas en meddelandet överlägget. Meddelande överlägg är bra för en snabb integrering eftersom de inte kräver någon du toomodify alla layouter i ditt program.

toomodify hello utseendet på din anmälan överlägg, du kan bara ändra hello filen `engagement_notification_area.xml` tooyour måste.

> [!NOTE]
> hello filen `engagement_notification_overlay.xml` är hello ett som är används toocreate en meddelandet överlägget hello-filen innehåller `engagement_notification_area.xml`. Du kan också anpassa den toosuit dina behov (till exempel för att placera hello meddelandefältet inom hello överlägget).
> 
> 

##### <a name="include-notification-layout-as-part-of-an-activity-layout"></a>Inkludera meddelande layout som en del av en aktivitet layout
Överlägg kan är bra för en snabb integrering men vara olämplig eller ha sidoeffekter i särskilda fall. hello överlägget system kan anpassas på en aktivitetsnivå, vilket gör det enkelt tooprevent sidoeffekter för särskilda aktiviteter.

Du kan bestämma tooinclude layout för våra meddelanden i din befintliga layout tack toohello Android **inkluderar** instruktionen. hello följande är ett exempel på en ändrade `ListActivity` layout som bara innehåller en `ListView`.

**Innan du integrering av Engagement:**

            <?xml version="1.0" encoding="utf-8"?>
            <ListView
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@android:id/list"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent" />

**Efter integrering av Engagement:**

            <?xml version="1.0" encoding="utf-8"?>
            <LinearLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <ListView
                android:id="@android:id/list"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:layout_weight="1" />

              <include layout="@layout/engagement_notification_area" />

            </LinearLayout>

I det här exemplet vi har lagt till en överordnad behållare eftersom hello ursprungliga layouten används en listvy som hello element på översta nivån. Vi har även lagt till `android:layout_weight="1"` toobe kan tooadd vyn under en listvy konfigurerats med `android:layout_height="fill_parent"`.

Hej Engagement nå SDK identifierar automatiskt hello notification layouten ingår i den här aktiviteten och kommer inte att lägga till ett överlägg för den här aktiviteten.

> [!TIP]
> Om du använder en ListActivity i ditt program hindrar ett synliga Reach-överlägg dig från att korsreagerande tooclicked objekt i listvyn hello längre. Detta är ett känt problem. toowork problemet vi rekommenderar att du tooembed hello notification layout i listan aktivitet layouten som i föregående exempel för hello.
> 
> 

##### <a name="disabling-application-notification-per-activity"></a>Inaktivera programmet meddelanden per aktivitet
Om du inte vill hello överlägget toobe lagt till tooyour aktivitet och om du inte anger layout för hello-meddelande med din egen layout, kan du inaktivera hello överlägget för aktiviteten i hello `AndroidManifest.xml` genom att lägga till en `meta-data` avsnittet precis som i följande hello Exempel:

            <activity android:name="SplashScreenActivity">
              <meta-data android:name="engagement:notification:overlay" android:value="false"/>
            </activity>

#### <a name="categories"></a>Kategorier
När du ändrar hello tillhandahålls layouter ändra hello utseende på alla meddelanden. Kategorier kan toodefine olika riktas söks meddelanden (möjligen beteenden). En kategori kan anges när du skapar en Reach-kampanj. Tänk på att kategorier kan du anpassa meddelanden och avsökningar, som beskrivs senare i det här dokumentet.

tooregister en kategori hanterare för dina meddelanden behöver du tooadd ett anrop när programmet hello har initierats.

> [!IMPORTANT]
> Läs hello varning om hello android: processen attributet \<android-sdk-engagement-process\> i hello hur tooIntegrate Engagement på Android avsnittet innan du fortsätter.
> 
> 

hello följande exempel förutsätter att du bekräftade hello föregående varning och använder en underklass till `EngagementApplication`:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), "myCategory");
              }
            }

Hej `MyNotifier` objektet är hello implementering av hello meddelandehanteraren kategori. Det är antingen en implementering av hello `EngagementNotifier` -gränssnittet eller en underordnad klass av hello standardimplementering: `EngagementDefaultNotifier`.

Observera att hello samma meddelaren kan hantera flera kategorier, kan du registrera dem så här:

            reachAgent.registerNotifier(new MyNotifier(this), "myCategory", "myAnotherCategory");

tooreplace hello kategori standardimplementering, kan du registrera din implementering som i följande exempel hello:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), Intent.CATEGORY_DEFAULT); // "android.intent.category.DEFAULT"
              }
            }

hello aktuella kategori som används i en hanterare skickas som en parameter i de flesta metoder som du kan åsidosätta i `EngagementDefaultNotifier`.

Den skickas antingen som en `String` parametern eller indirekt i en `EngagementReachContent` objekt som har en `getCategory()` metod.

Du kan ändra de flesta av processen för hello-meddelande med omdefiniering metoder på `EngagementDefaultNotifier`för mer avancerad anpassning känna sig fria tootake en titt på hello teknisk dokumentation och hello källkoden.

##### <a name="in-app-notifications"></a>Meddelanden i appen
Om du bara vill toouse alternativa layouter för en viss kategori, kan du implementera detta som i följande exempel hello:

            public class MyNotifier extends EngagementDefaultNotifier
            {
              public MyNotifier(Context context)
              {
                super(context);
              }

              @Override
              protected int getOverlayLayoutId(String category)
              {
                return R.layout.my_notification_overlay;
              }


              @Override
              public Integer getOverlayViewId(String category)
              {
                return R.id.my_notification_overlay;
              }

              @Override
              public Integer getInAppAreaId(String category)
              {
                return R.id.my_notification_area;
              }
            }

**Exempel på `my_notification_overlay.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/my_notification_overlay"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <include layout="@layout/my_notification_area" />

            </RelativeLayout>

Som du ser skiljer hello överlägget visa identifierare sig hello standard en. Det är viktigt att varje layout använder en unik identifierare för överlägg.

**Exempel på `my_notification_area.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <merge
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <RelativeLayout
                android:id="@+id/my_notification_area"
                android:layout_width="fill_parent"
                android:layout_height="64dp"
                android:layout_alignParentTop="true"
                android:background="#B000">

                <LinearLayout
                  android:orientation="horizontal"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent"
                  android:gravity="center_vertical">

                  <ImageView
                    android:id="@+id/engagement_notification_icon"
                    android:layout_width="48dp"
                    android:layout_height="48dp" />

                  <LinearLayout
                    android:id="@+id/engagement_notification_text"
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent"
                    android:layout_weight="1"
                    android:gravity="center_vertical">

                    <TextView
                      android:id="@+id/engagement_notification_title"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:singleLine="true"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Medium" />

                    <TextView
                      android:id="@+id/engagement_notification_message"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:maxLines="2"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Small" />

                  </LinearLayout>

                  <ImageView
                    android:id="@+id/engagement_notification_image"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:adjustViewBounds="true" />

                  <ImageButton
                    android:id="@+id/engagement_notification_close_area"
                    android:visibility="invisible"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:src="@android:drawable/btn_dialog"
                    android:background="#0F00" />

                </LinearLayout>

                <ImageButton
                  android:id="@+id/engagement_notification_close"
                  android:layout_width="wrap_content"
                  android:layout_height="fill_parent"
                  android:layout_alignParentRight="true"
                  android:src="@android:drawable/btn_dialog"
                  android:background="#0F00" />

              </RelativeLayout>

            </merge>

Som du ser skiljer hello notification området Visa identifierare sig hello standard en. Det är viktigt att varje layout använder en unik identifierare för meddelandefält.

Det här enkla exemplet på kategorin gör ett program (eller i appen) meddelanden som visas överst hello i hello-skärmen. Vi har inte ändrats hello standard identifierare som används i hello meddelandefältet sig själv.

Om du vill att du har tooredefine hello toochange `EngagementDefaultNotifier.prepareInAppArea` metod. Det rekommenderas toolook på hello teknisk dokumentation och hello källkoden för `EngagementNotifier` och `EngagementDefaultNotifier` om du vill använda denna nivå av avancerad anpassning.

##### <a name="system-notifications"></a>Systemmeddelanden
Genom att utöka `EngagementDefaultNotifier`, kan du åsidosätta `onNotificationPrepared` tooalter hello-meddelande som har sammanställts av hello standardimplementering.

Exempel:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content)
              throws RuntimeException
            {
              if ("ongoing".equals(content.getCategory()))
                notification.flags |= Notification.FLAG_ONGOING_EVENT;
              return true;
            }

Det här exemplet gör en systemavisering för en innehåll visas som en pågående händelse när hello ”pågående” kategori används.

Om du vill toobuild hello `Notification` objekt från början, du kan returnera `false` toohello metod och anropet `notify` själv på hello `NotificationManager`. I så fall är det viktigt att du behåller en `contentIntent`, `deleteIntent` och hello meddelande-ID som används av `EngagementReachReceiver`.

Här är en korrekt exempel på sådana implementering:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content) throws RuntimeException
            {
              /* Required fields */
              NotificationCompat.Builder builder = new NotificationCompat.Builder(mContext)
                .setSmallIcon(notification.icon)              // icon is mandatory
                .setContentIntent(notification.contentIntent) // keep content intent
                .setDeleteIntent(notification.deleteIntent);  // keep delete intent

              /* Your customization */
              // builder.set...

              /* Dismiss option can be managed only after build */
              Notification myNotification = builder.build();
              if (!content.isNotificationCloseable())
                myNotification.flags |= Notification.FLAG_NO_CLEAR;

              /* Notify here instead of super class */
              NotificationManager manager = (NotificationManager) mContext.getSystemService(Context.NOTIFICATION_SERVICE);
              manager.notify(getNotificationId(content), myNotification); // notice hello call tooget hello right identifier

              /* Return false, we notify ourselves */
              return false;
            }

##### <a name="notification-only-announcements"></a>Meddelande endast meddelanden
hello hantering av hello klickar du på ett meddelande som endast meddelande kan anpassas genom att åsidosätta `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` toomodify hello förberedd `Intent`. Med den här metoden kan du tootune hello flaggor enkelt.

Till exempel tooadd hello `SINGLE_TOP` flaggan:

            @Override
            protected Intent onNotifAnnouncementIntentPrepared(EngagementNotifAnnouncement notifAnnouncement,
              Intent intent)
            {
              intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
              return intent;
            }

För äldre Engagement användare, Observera att systemmeddelanden utan åtgärd URL nu startar programmet hello om den var i bakgrunden, så den här metoden kan anropas med ett meddelande utan åtgärds-URL. Du bör som när du anpassar hello avsikt.

Du kan också implementeras `EngagementNotifier.executeNotifAnnouncementAction` från början.

##### <a name="notification-life-cycle"></a>Livscykel för meddelande
När du använder hello standardkategori, vissa livscykel metoder anropas på hello `EngagementReachInteractiveContent` objekt tooreport statistik och uppdatera hello kampanj tillstånd:

* När hello-meddelande visas i programmet eller placera i hello statusfältet, hello `displayNotification` metoden anropas (som rapporterar statistik) av `EngagementReachAgent` om `handleNotification` returnerar `true`.
* Om hello-meddelande avvisas hello `exitNotification` metoden anropas statistik rapporteras och nästa kampanjer kan nu bearbetas.
* Om du hello-meddelande `actionNotification` är anropas statistik rapporteras och hello associerade avsikten har startats.

Om din implementering av `EngagementNotifier` kringgår hello standardbeteendet har du toocall metoderna livscykel själv. hello som följande exempel visar tillfällen där hello standardbeteendet kringgås:

* Du inte utökar `EngagementDefaultNotifier`, t.ex. du har implementerat kategori hantering från grunden.
* För systemmeddelanden, du overrode hello `onNotificationPrepared` och du ändrat `contentIntent` eller `deleteIntent` i hello `Notification` objekt.
* För meddelanden i appen, overrode `prepareInAppArea`, vara säker på att toomap minst `actionNotification` tooone U.I-kontroller.

> [!NOTE]
> Om `handleNotification` returnerar ett undantag, hello innehåll tas bort och `dropContent` anropas. Detta rapporteras i statistik och nästa kampanjer kan nu bearbetas.
> 
> 

### <a name="announcements-and-polls"></a>Meddelanden och avsökningar
#### <a name="layouts"></a>Layouter
Du kan ändra hello `engagement_text_announcement.xml`, `engagement_web_announcement.xml` och `engagement_poll.xml` filer toocustomize text meddelanden, web meddelanden och avsökningar.

De här filerna dela två vanliga layouter för hello rubrikområdet och hello knappområde. hello layouten för hello är `engagement_content_title.xml` och använder hello eponymous drawable-filen för hello bakgrund. hello layout hello åtgärd och avsluta knappar är `engagement_button_bar.xml` och använder hello eponymous drawable-filen för hello bakgrund.

I en omröstning hello fråga layout och sina val är dynamiskt förstoras med flera gånger hello `engagement_question.xml` layoutfilen för hello frågor och hello `engagement_choice.xml` -filen för hello val.

#### <a name="categories"></a>Kategorier
##### <a name="alternate-layouts"></a>Alternativa layouter
Hello kampanj kategori kan vara används toohave alternativa layouter för meddelanden och avsökningar som meddelanden.

Till exempel toocreate en kategori för en SMS-meddelande, kan du utöka `EngagementTextAnnouncementActivity` och hänvisar det hello `AndroidManifest.xml` fil:

            <activity android:name="com.your_company.MyCustomTextAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>

Observera hello kategorin i hello avsikt filter används toomake hello skillnaden med hello standardaktivitet meddelande.

hello nå SDK använder hello avsiktshantering system tooresolve hello rätt aktivitet för en viss kategori och använder hello standard kategori om hello matchningen misslyckades.

Du kan ha tooimplement `MyCustomTextAnnouncementActivity`, om du bara vill toochange hello layout (men behåll hello samma vy identifierare), du precis har toodefine hello klassen som i följande exempel hello:

            public class MyCustomTextAnnouncementActivity extends EngagementTextAnnouncementActivity
            {
              @Override
              protected String getLayoutName()
              {
                return "my_text_announcement";  // tell super class toouse R.layout.my_text_announcement
              }
            }

tooreplace hello standardkategori av text-meddelanden, ersätts `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` av din implementering.

Web meddelanden och avsökningar kan anpassas på ett liknande sätt.

För web-meddelanden kan du utöka `EngagementWebAnnouncementActivity` och deklarera dina aktiviteter i hello `AndroidManifest.xml` precis som i följande exempel hello:

            <activity android:name="com.your_company.MyCustomWebAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/html" />    <!-- only difference with text announcements in hello intent is hello data mime type -->
              </intent-filter>
            </activity>

För avsökningar kan du utöka `EngagementPollActivity` och deklarera dina i hello `AndroidManifest.xml` precis som i följande exempel hello:

            <activity android:name="com.your_company.MyCustomPollActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="my_category" />
              </intent-filter>
            </activity>

##### <a name="implementation-from-scratch"></a>Implementeringen från grunden
Du kan implementera kategorier för dina aktiviteter för meddelande (och avsökning) utan att utöka en hello `Engagement*Activity` klasser som tillhandahålls av hello nå SDK. Detta är användbart till exempel om du vill toodefine en layout som inte använder samma visar hello som hello standard layouter.

För avancerade notification anpassning bör som toolook på hello källkoden för hello standardimplementeringen.

Här följer några saker tookeep i åtanke: Reach startas hello aktivitet med ett specifikt syfte (motsvarande toohello avsiktshantering filter) plus en extra parameter som är hello innehåll identifierare.

tooretrieve hello innehållsobjekt som innehåller hello fält som du angav när skapar hello kampanj på hello-webbplatsen du kan göra detta:

            public class MyCustomTextAnnouncement extends EngagementActivity
            {
              private EngagementAnnouncement mContent;

              @Override
              protected void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);

                /* Get content */
                mContent = EngagementReachAgent.getInstance(this).getContent(getIntent());
                if (mContent == null)
                {
                  /* If problem with content, exit */
                  finish();
                  return;
                }

                setContentView(R.layout.my_text_announcement);

                /* Configure views by querying fields on mContent */
                // ...
              }
            }

För statistik, ska du rapportera hello innehållet visas i hello `onResume` händelse:

            @Override
            protected void onResume()
            {
             /* Mark hello content displayed */
             mContent.displayContent(this);
             super.onResume();
            }

Sedan Glöm inte toocall antingen `actionContent(this)` eller `exitContent(this)` på hello innehållsobjekt innan hello aktivitet försätts i bakgrunden.

Om du inte anropa antingen `actionContent` eller `exitContent`, statistik inte skickas (dvs. inga analyser på hello kampanj) och flera allt hello nästa kampanjer meddelas inte förrän hello programprocessen har startats om.

Orientering eller andra ändringar i konfigurationen kan fatta hello kod komplicerade toodetermine om hello aktivitet försätts i bakgrunden eller inte, hello standardimplementeringen gör att hello innehåll rapporteras som avslutats om hello användaren lämnar hello-aktivitet (antingen med När du trycker på `HOME` eller `BACK`) men inte om hello orientering ändras.

Här ingår hello intressanta i hello implementering:

            @Override
            protected void onUserLeaveHint()
            {
              finish();
            }

            @Override
            protected void onPause()
            {
              if (isFinishing() && mContent != null)
              {
                /*
                 * Exit content on exit, this is has no effect if another process method has already been
                 * called so we don't have toocheck anything here.
                 */
                mContent.exitContent(this);
              }
              super.onPause();
            }

Som du ser om du anropas `actionContent(this)` sedan klar hello aktiviteten `exitContent(this)` på ett säkert sätt kan anropas utan att ha någon effekt.

[here]:http://developer.android.com/tools/extras/support-library.html#Downloading
[Google Cloud Messaging]:http://developer.android.com/guide/google/gcm/index.html
[Amazon Device Messaging]:https://developer.amazon.com/sdk/adm.html
