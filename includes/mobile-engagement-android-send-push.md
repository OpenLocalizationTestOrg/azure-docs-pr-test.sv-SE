
### <a name="update-manifest-file-tooenable-notifications"></a>Uppdatera manifestfilen tooenable meddelanden
Kopiera hello i appen resurserna nedan i din manifest.XML-fil mellan hello `<application>` och `</application>` taggar.

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

### <a name="specify-an-icon-for-notifications"></a>Ange en ikon för meddelanden
Klistra in hello följande XML-kodstycke i din manifest.XML-fil mellan hello `<application>` och `</application>` taggar.

        <meta-data android:name="engagement:reach:notification:icon" android:value="engagement_close"/>

Detta definierar hello-ikon som visas både i systemmeddelanden och aviseringar via appen. Det är valfritt för aviseringar via app men obligatoriskt för systemmeddelanden. Android avvisar systemmeddelanden med ogiltiga ikoner.

Kontrollera att du använder en ikon som finns i en av hello **drawable** mappar (t.ex. ``engagement_close.png``). **mipmap**-mappen stöds inte.

> [!NOTE]
> Du bör inte använda hello **starta** ikonen. Den har en annan upplösning och ligger vanligtvis i hello mipmap-mapparna som inte stöds.
> 
> 

För riktiga appar kan du använda en ikon som passar för aviseringar enligt [designriktlinjerna för Android](http://developer.android.com/design/patterns/notifications.html).

> [!TIP]
> toobe att toouse rätt ikonupplösning, kan du titta på [exemplen](https://www.google.com/design/icons).
> Bläddra nedåt toohello **meddelande** avsnittet, klicka på en ikon och klicka sedan på `PNGS` toodownload hello drawable-ikonuppsättningen. Du kan se vilka drawable-mappar med vilken upplösning toouse för varje version av hello ikon.
> 
> 

### <a name="enable-your-app-tooreceive-gcm-push-notifications"></a>Aktivera din app tooreceive GCM push-meddelanden
1. Klistra in följande hello i din manifest.XML-fil mellan hello `<application>` och `</application>` taggar när du ersätter hello **avsändar-ID** från konsolen Firebase för projektet. hello \n är avsiktlig så se till att du avslutar hello projektnumret med det..
   
        <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
2. Klistra in hello koden nedan i din manifest.XML-fil mellan hello `<application>` och `</application>` taggar. Ersätt paketnamnet hello <Your package name>.
   
        <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
        android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
            </intent-filter>
        </receiver>
   
        <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<Your package name>" />
            </intent-filter>
        </receiver>
3. Lägg till hello sista uppsättningen behörigheter som är markerade innan hello `<application>` tagg. Ersätt `<Your package name>` av hello faktiska paketnamnet för programmet.
   
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="<Your package name>.permission.C2D_MESSAGE" />
        <permission android:name="<Your package name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

