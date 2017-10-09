
### <a name="update-manifest-file-tooenable-notifications"></a><span data-ttu-id="47b49-101">Uppdatera manifestfilen tooenable meddelanden</span><span class="sxs-lookup"><span data-stu-id="47b49-101">Update manifest file tooenable notifications</span></span>
<span data-ttu-id="47b49-102">Kopiera hello i appen resurserna nedan i din manifest.XML-fil mellan hello `<application>` och `</application>` taggar.</span><span class="sxs-lookup"><span data-stu-id="47b49-102">Copy hello in-app messaging resources below into your Manifest.xml between hello `<application>` and `</application>` tags.</span></span>

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

### <a name="specify-an-icon-for-notifications"></a><span data-ttu-id="47b49-103">Ange en ikon för meddelanden</span><span class="sxs-lookup"><span data-stu-id="47b49-103">Specify an icon for notifications</span></span>
<span data-ttu-id="47b49-104">Klistra in hello följande XML-kodstycke i din manifest.XML-fil mellan hello `<application>` och `</application>` taggar.</span><span class="sxs-lookup"><span data-stu-id="47b49-104">Paste hello following XML snippet in your Manifest.xml between hello `<application>` and `</application>` tags.</span></span>

        <meta-data android:name="engagement:reach:notification:icon" android:value="engagement_close"/>

<span data-ttu-id="47b49-105">Detta definierar hello-ikon som visas både i systemmeddelanden och aviseringar via appen.</span><span class="sxs-lookup"><span data-stu-id="47b49-105">This defines hello icon that is displayed both in system and in-app notifications.</span></span> <span data-ttu-id="47b49-106">Det är valfritt för aviseringar via app men obligatoriskt för systemmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="47b49-106">It is optional for in-app notifications however mandatory for system notifications.</span></span> <span data-ttu-id="47b49-107">Android avvisar systemmeddelanden med ogiltiga ikoner.</span><span class="sxs-lookup"><span data-stu-id="47b49-107">Android will rejects system notifications with invalid icons.</span></span>

<span data-ttu-id="47b49-108">Kontrollera att du använder en ikon som finns i en av hello **drawable** mappar (t.ex. ``engagement_close.png``).</span><span class="sxs-lookup"><span data-stu-id="47b49-108">Make sure you are using an icon that exists in one of hello **drawable** folders (like ``engagement_close.png``).</span></span> <span data-ttu-id="47b49-109">**mipmap**-mappen stöds inte.</span><span class="sxs-lookup"><span data-stu-id="47b49-109">**mipmap** folder isn't supported.</span></span>

> [!NOTE]
> <span data-ttu-id="47b49-110">Du bör inte använda hello **starta** ikonen.</span><span class="sxs-lookup"><span data-stu-id="47b49-110">You should not use hello **launcher** icon.</span></span> <span data-ttu-id="47b49-111">Den har en annan upplösning och ligger vanligtvis i hello mipmap-mapparna som inte stöds.</span><span class="sxs-lookup"><span data-stu-id="47b49-111">It has a different resolution and is usually in hello mipmap folders, which we don't support.</span></span>
> 
> 

<span data-ttu-id="47b49-112">För riktiga appar kan du använda en ikon som passar för aviseringar enligt [designriktlinjerna för Android](http://developer.android.com/design/patterns/notifications.html).</span><span class="sxs-lookup"><span data-stu-id="47b49-112">For real apps, you can use an icon that is suitable for notifications per [Android design guidelines](http://developer.android.com/design/patterns/notifications.html).</span></span>

> [!TIP]
> <span data-ttu-id="47b49-113">toobe att toouse rätt ikonupplösning, kan du titta på [exemplen](https://www.google.com/design/icons).</span><span class="sxs-lookup"><span data-stu-id="47b49-113">toobe sure toouse correct icon resolutions, you can look at [these examples](https://www.google.com/design/icons).</span></span>
> <span data-ttu-id="47b49-114">Bläddra nedåt toohello **meddelande** avsnittet, klicka på en ikon och klicka sedan på `PNGS` toodownload hello drawable-ikonuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="47b49-114">Scroll down toohello **Notification** section, click an icon, and then click `PNGS` toodownload hello icon drawable set.</span></span> <span data-ttu-id="47b49-115">Du kan se vilka drawable-mappar med vilken upplösning toouse för varje version av hello ikon.</span><span class="sxs-lookup"><span data-stu-id="47b49-115">You can see what drawable folders with which resolution toouse for each version of hello icon.</span></span>
> 
> 

### <a name="enable-your-app-tooreceive-gcm-push-notifications"></a><span data-ttu-id="47b49-116">Aktivera din app tooreceive GCM push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="47b49-116">Enable your app tooreceive GCM push notifications</span></span>
1. <span data-ttu-id="47b49-117">Klistra in följande hello i din manifest.XML-fil mellan hello `<application>` och `</application>` taggar när du ersätter hello **avsändar-ID** från konsolen Firebase för projektet.</span><span class="sxs-lookup"><span data-stu-id="47b49-117">Paste hello following into your Manifest.xml between hello `<application>` and `</application>` tags after replacing hello **Sender ID** obtained from your Firebase project console.</span></span> <span data-ttu-id="47b49-118">hello \n är avsiktlig så se till att du avslutar hello projektnumret med det..</span><span class="sxs-lookup"><span data-stu-id="47b49-118">hello \n is intentional so make sure that you end hello project number with it.</span></span>
   
        <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
2. <span data-ttu-id="47b49-119">Klistra in hello koden nedan i din manifest.XML-fil mellan hello `<application>` och `</application>` taggar.</span><span class="sxs-lookup"><span data-stu-id="47b49-119">Paste hello code below into your Manifest.xml between hello `<application>` and `</application>` tags.</span></span> <span data-ttu-id="47b49-120">Ersätt paketnamnet hello <Your package name>.</span><span class="sxs-lookup"><span data-stu-id="47b49-120">Replace hello package name <Your package name>.</span></span>
   
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
3. <span data-ttu-id="47b49-121">Lägg till hello sista uppsättningen behörigheter som är markerade innan hello `<application>` tagg.</span><span class="sxs-lookup"><span data-stu-id="47b49-121">Add hello last set of permissions that are highlighted before hello `<application>` tag.</span></span> <span data-ttu-id="47b49-122">Ersätt `<Your package name>` av hello faktiska paketnamnet för programmet.</span><span class="sxs-lookup"><span data-stu-id="47b49-122">Replace `<Your package name>` by hello actual package name of your application.</span></span>
   
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="<Your package name>.permission.C2D_MESSAGE" />
        <permission android:name="<Your package name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

