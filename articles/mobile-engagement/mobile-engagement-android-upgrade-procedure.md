---
title: aaaAzure Mobile Engagement Android SDK-Integration
description: "Senaste uppdateringarna och procedurer för Android SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 11618586-c709-49ca-bcd8-745323ff1af6
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: df5c82812fe0a242eaa5df8c906030237215b7eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-procedures"></a>Uppgraderingsprocesser
Om du redan har integrerat en äldre version av våra SDK i ditt program, har du tooconsider hello följande punkter när du uppgraderar hello SDK.

Du kan ha toofollow flera procedurer om du har missat flera versioner av hello SDK. Till exempel om du migrerar från 1.4.0 too1.6.0 som du har toofirst följer hello ”från 1.4.0 too1.5.0” proceduren sedan hello ”från 1.5.0 too1.6.0” proceduren.

Oavsett vilken hello-version som du uppgraderar från, du har tooreplace hello `mobile-engagement-VERSION.jar` med hello ny.

## <a name="from-420-too421"></a>Från 4.2.0 too4.2.1
Det här steget kan faktiskt utförs på alla versioner av hello SDK, är en säkerhetsförbättring av när du integrerar räckvidden aktiviteter.

Du bör nu lägga till `exported="false"` tooall Reach-aktiviteter.

Reach-aktiviteter bör nu se ut så här din `AndroidManifest.xml`:

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

## <a name="from-400-too410"></a>Från 4.0.0 too4.1.0
hello SDK nu hantera nya behörighetsmodellen från Android M.

Om du använder funktioner för plats eller stor bild meddelanden Läs [i det här avsnittet](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

Dessutom toohello nya behörighetsmodellen vi stöder nu konfigurera funktioner för plats vid körning.
Vi är fortfarande är kompatibla med hello manifestet parametrar för plats men den är nu föråldrad. toouse runtime-konfiguration, ta bort hello de följande avsnitten från din ``AndroidManifest.xml``:

    <meta-data
      android:name="engagement:locationReport:lazyArea"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:background"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:fine"
      android:value="true"/>

och läsa [uppdateras det här förfarandet](mobile-engagement-android-integrate-engagement.md#location-reporting) toouse runtime konfiguration i stället.

## <a name="from-300-too400"></a>Från 3.0.0 too4.0.0
### <a name="native-push"></a>Intern push
Intern push (GCM/ADM) nu används också för i appen meddelanden så att du måste konfigurera hello native push-autentiseringsuppgifter för alla typer av push-kampanj.

Om den inte redan Följ [proceduren](mobile-engagement-android-integrate-engagement-reach.md#native-push).

### <a name="androidmanifestxml"></a>AndroidManifest.xml
Reach-integrering har ändrats i ``AndroidManifest.xml``.

Ersätt detta:

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>

Av

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
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

Det finns eventuellt en skärm för inläsning nu när du klickar på ett meddelande (med text/webbinnehåll) eller en avsökning.
Du har tooadd detta för de kampanjer toowork i 4.0.0:

    <activity
      android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity"
      android:theme="@android:style/Theme.Dialog">
      <intent-filter>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </activity>

### <a name="resources"></a>Resurser
Bädda in hello nya `res/layout/engagement_loading.xml` filen i projektet.

## <a name="from-240-too300"></a>Från 2.4.0 too3.0.0
hello beskrivs nedan hur toomigrate SDK-integration från hello Capptain tjänsten erbjuds av Capptain SAS i en app med Azure Mobile Engagement. Om du migrerar från en tidigare version finns hello Capptain webbplats toomigrate too2.4.0 först och sedan använda hello nedan.

> [!IMPORTANT]
> Capptain Mobile Engagement är inte hello samma tjänster och hello proceduren som anges nedan endast highlights hur toomigrate hello-klientappen. Migrera hello SDK i hello app migreras inte data från hello Capptain servrar toohello Mobile Engagement-servrar.
> 
> 

### <a name="jar-file"></a>JAR-filen
Ersätt `capptain.jar` av `mobile-engagement-VERSION.jar` i din `libs` mapp.

### <a name="resource-files"></a>Resursfiler
Varje resursfil som vi angav (föregås av `capptain_`) har toobe ersättas med hello nya (med prefixet `engagement_`).

Om du har anpassat dessa filer, har du toore-tillämpa anpassningen på nya filer hello **alla hello identifierare i hello resursfiler har bytt**.

### <a name="application-id"></a>Program-ID:t
Engagement använder nu en anslutning tooconfigure hello SDK strängidentifierare, till exempel hello identifierare.

Du har toouse `EngagementAgent.init` metod i din Starta aktivitet så här:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

hello anslutningssträngen för ditt program visas på Azure Portal.

Ta bort alla anrop för`CapptainAgent.configure` som `EngagementAgent.init` ersätter den här metoden.

Hej `appId` inte längre kan konfigureras med `AndroidManifest.xml`.

Ta bort det här avsnittet från din `AndroidManifest.xml` om du har:

            <meta-data android:name="capptain:appId" android:value="<YOUR_APPID>"/>

### <a name="java-api"></a>Java-API
Varje anrop tooany Java-klass av våra SDK har toobe har fått nytt namn; till exempel `CapptainAgent.getInstance(this)` måste ändras `EngagementAgent.getInstance(this)`, `extends CapptainActivity` måste ändras `extends EngagementActivity` etc...

Om du har integrerat med standard agent inställningsfiler hello Standardfilnamnet är nu `engagement.agent` och hello nyckel `engagement:agent`.

När du skapar web meddelanden hello Javascript binder är nu `engagementReachContent`.

### <a name="androidmanifestxml"></a>AndroidManifest.xml
En mängd ändringar har hänt det hello tjänsten delas inte längre och mycket mottagare kan inte exporteras längre.

Hej tjänstedeklaration är nu enklare; ta bort hello avsiktshantering filter och alla metadata i den och lägga till `exportable=false`.

Dessutom allt har bytt namn till toouse engagement.

Nu ser ut som:

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

Om du vill tooenable test loggar hello metadata har nu flyttats toohello programmet taggen och har bytt namn:

            <application>

              <meta-data android:name="engagement:log:test" android:value="true" />

              <service/>

            </application>

Alla andra metadata har precis bytt namn, här är hello fullständig lista (självklart byta endast hello som du använder):

            <meta-data
              android:name="engagement:reportCrash"
              android:value="true"/>
            <meta-data
              android:name="engagement:sessionTimeout"
              android:value="10000"/>
            <meta-data
              android:name="engagement:burstThreshold"
              android:value="0"/>
            <meta-data
              android:name="engagement:connection:delay"
              android:value="0"/>
            <meta-data
              android:name="engagement:locationReport:lazyArea"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:background"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:fine"
              android:value="false"/>
            <meta-data
              android:name="engagement:agent:settings:name"
              android:value="engagement.agent"/>
            <meta-data
              android:name="engagement:agent:settings:mode"
              android:value="0"/>
            <meta-data
              android:name="engagement:gcm:sender"
              android:value="<YOUR_PROJECT_NUMBER>\n"/>
            <meta-data
              android:name="engagement:adm:register"
              android:value="true"/>
            <meta-data
              android:name="engagement:reach:notification:icon"
              android:value="<DRAWABLE_NAME_WITHOUT_EXTENSION>"/>

            <activity android:name="SomeActivityWithoutReachOverlay">
              <meta-data
                android:name="engagement:notification:overlay"
                android:value="false"/>
            </activity>

Google Play och SmartAd spårning har tagits bort från SDK du precis har tooremove detta utan byte:

            <meta-data 
                android:name="capptain:track:installReferrerForwardList"
                android:value="com.class1,com.class2"/>
            <meta-data
                android:name="capptain:track:adservers"
                android:value="smartad" />

hello Reach aktiviteter deklareras nu så här:

            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/plain"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/html"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>

Om du har anpassade Reach-aktiviteter, du behöver bara toochange hello avsiktshantering åtgärder toomatch antingen `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` eller `com.microsoft.azure.engagement.reach.intent.action.POLL`.

hello broadcast mottagare har bytt namn plus vi nu lägga till `exported=false`. Här är hello fullständig lista över hello mottagare med hello nya specifikationen (självklart byta endast hello som du använder):

            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
              android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver
              android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver"
              android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
                <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
              </intent-filter>
            </receiver>

            <receiver
              android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
              android:permission="com.amazon.device.messaging.permission.SEND">
              <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
                <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementConnectionReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.CONNECTED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.DISCONNECTED"/>
              </intent-filter>
            </receiver>

            <receiver
              android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementMessageReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.MESSAGE"/>
              </intent-filter>
            </receiver>

Spåra mottagaren har tagits bort, så att du har tooremove i det här avsnittet:

          <receiver android:name="com.ubikod.capptain.android.sdk.track.CapptainTrackReceiver">
            <intent-filter>
              <action android:name="com.ubikod.capptain.intent.action.APPID_GOT" />
              <!-- possibly <action android:name="com.android.vending.INSTALL_REFERRER" /> -->
            </intent-filter>
          </receiver>

Observera att hello deklarationen av din implementering av hello sänder mottagaren **EngagementMessageReceiver** har ändrats i hello `AndroidManifest.xml`. Detta beror på att hello API toosend och ta bort valfri XMPP meddelanden från valfri XMPP entiteter och hello API toosend och ta emot meddelanden mellan enheter har tagits bort. Du har alltså också toodelete hello efter återanrop från din **EngagementMessageReceiver** implementering:

            protected void onDeviceMessageReceived(android.content.Context context, java.lang.String deviceId, java.lang.String payload)

och

            protected void onXMPPMessageReceived(android.content.Context context, android.os.Bundle message)

ta bort alla anrop på **EngagementAgent** för:

            sendMessageToDevice(java.lang.String deviceId, java.lang.String payload, java.lang.String packageName)

och

            sendXMPPMessage(android.os.Bundle msg)

### <a name="proguard"></a>Proguard
Proguard konfiguration kan påverkas av rebranding hello regler nu ser ut som:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }

            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
              <methods>;
            }

