
### <a name="update-manifest-file-to-enable-notifications"></a><span data-ttu-id="1313d-101">Uppdatera manifestfilen om du vill aktivera meddelanden</span><span class="sxs-lookup"><span data-stu-id="1313d-101">Update manifest file to enable notifications</span></span>
<span data-ttu-id="1313d-102">Kopiera resurserna för aviseringar via app nedan och klistra in dem i din Manifest.xml-fil mellan taggarna `<application>` och `</application>`.</span><span class="sxs-lookup"><span data-stu-id="1313d-102">Copy the in-app messaging resources below into your Manifest.xml between the `<application>` and `</application>` tags.</span></span>

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

### <a name="specify-an-icon-for-notifications"></a><span data-ttu-id="1313d-103">Ange en ikon för meddelanden</span><span class="sxs-lookup"><span data-stu-id="1313d-103">Specify an icon for notifications</span></span>
<span data-ttu-id="1313d-104">Klistra in följande XML-kodstycke i din Manifest.xml-fil mellan taggarna `<application>` och `</application>`.</span><span class="sxs-lookup"><span data-stu-id="1313d-104">Paste the following XML snippet in your Manifest.xml between the `<application>` and `</application>` tags.</span></span>

        <meta-data android:name="engagement:reach:notification:icon" android:value="engagement_close"/>

<span data-ttu-id="1313d-105">Detta definierar ikonen som visas både i systemmeddelanden och aviseringar via appen.</span><span class="sxs-lookup"><span data-stu-id="1313d-105">This defines the icon that is displayed both in system and in-app notifications.</span></span> <span data-ttu-id="1313d-106">Det är valfritt för aviseringar via app men obligatoriskt för systemmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="1313d-106">It is optional for in-app notifications however mandatory for system notifications.</span></span> <span data-ttu-id="1313d-107">Android avvisar systemmeddelanden med ogiltiga ikoner.</span><span class="sxs-lookup"><span data-stu-id="1313d-107">Android will rejects system notifications with invalid icons.</span></span>

<span data-ttu-id="1313d-108">Se till att du använder en ikon som finns i en av **drawable**-mapparna (t.ex. ``engagement_close.png``).</span><span class="sxs-lookup"><span data-stu-id="1313d-108">Make sure you are using an icon that exists in one of the **drawable** folders (like ``engagement_close.png``).</span></span> <span data-ttu-id="1313d-109">**mipmap**-mappen stöds inte.</span><span class="sxs-lookup"><span data-stu-id="1313d-109">**mipmap** folder isn't supported.</span></span>

> [!NOTE]
> <span data-ttu-id="1313d-110">Du bör inte använda **launcher**-ikonen.</span><span class="sxs-lookup"><span data-stu-id="1313d-110">You should not use the **launcher** icon.</span></span> <span data-ttu-id="1313d-111">Den har en annan upplösning och ligger vanligtvis i mipmap-mapparna som inte stöds.</span><span class="sxs-lookup"><span data-stu-id="1313d-111">It has a different resolution and is usually in the mipmap folders, which we don't support.</span></span>
> 
> 

<span data-ttu-id="1313d-112">För riktiga appar kan du använda en ikon som passar för aviseringar enligt [designriktlinjerna för Android](http://developer.android.com/design/patterns/notifications.html).</span><span class="sxs-lookup"><span data-stu-id="1313d-112">For real apps, you can use an icon that is suitable for notifications per [Android design guidelines](http://developer.android.com/design/patterns/notifications.html).</span></span>

> [!TIP]
> <span data-ttu-id="1313d-113">För att vara säker på att du använder rätt ikonupplösning, kan du kolla på [de här exemplen](https://www.google.com/design/icons).</span><span class="sxs-lookup"><span data-stu-id="1313d-113">To be sure to use correct icon resolutions, you can look at [these examples](https://www.google.com/design/icons).</span></span>
> <span data-ttu-id="1313d-114">Bläddra ned till **Notification**-avsnittet, klicka på en ikon och klicka sedan på `PNGS` för att hämta drawable-ikonuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="1313d-114">Scroll down to the **Notification** section, click an icon, and then click `PNGS` to download the icon drawable set.</span></span> <span data-ttu-id="1313d-115">Du kan se vilka drawable-mappar med vilken upplösning som ska användas för varje version av ikonen.</span><span class="sxs-lookup"><span data-stu-id="1313d-115">You can see what drawable folders with which resolution to use for each version of the icon.</span></span>
> 
> 

### <a name="enable-your-app-to-receive-gcm-push-notifications"></a><span data-ttu-id="1313d-116">Konfigurera appen för att ta emot push-meddelanden med GCM</span><span class="sxs-lookup"><span data-stu-id="1313d-116">Enable your app to receive GCM push notifications</span></span>
1. <span data-ttu-id="1313d-117">Klistra in följande i din Manifest.xml mellan taggarna `<application>` och `</application>` efter att du ersatt **Avsändar-ID** som du fick från din Firebase-projektkonsol.</span><span class="sxs-lookup"><span data-stu-id="1313d-117">Paste the following into your Manifest.xml between the `<application>` and `</application>` tags after replacing the **Sender ID** obtained from your Firebase project console.</span></span> <span data-ttu-id="1313d-118">Markeringen ”\n” är avsiktlig, så se till att du avslutar projektnumret med det.</span><span class="sxs-lookup"><span data-stu-id="1313d-118">The \n is intentional so make sure that you end the project number with it.</span></span>
   
        <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
2. <span data-ttu-id="1313d-119">Klistra in koden nedan i din Manifest.xml-fil mellan taggarna `<application>` och `</application>`.</span><span class="sxs-lookup"><span data-stu-id="1313d-119">Paste the code below into your Manifest.xml between the `<application>` and `</application>` tags.</span></span> <span data-ttu-id="1313d-120">Ersätt paketnamnet <Your package name>.</span><span class="sxs-lookup"><span data-stu-id="1313d-120">Replace the package name <Your package name>.</span></span>
   
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
3. <span data-ttu-id="1313d-121">Lägg till den sista uppsättningen behörigheter som är markerade innan `<application>`-taggen.</span><span class="sxs-lookup"><span data-stu-id="1313d-121">Add the last set of permissions that are highlighted before the `<application>` tag.</span></span> <span data-ttu-id="1313d-122">Ersätt `<Your package name>` med det faktiska paketnamnet för programmet.</span><span class="sxs-lookup"><span data-stu-id="1313d-122">Replace `<Your package name>` by the actual package name of your application.</span></span>
   
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="<Your package name>.permission.C2D_MESSAGE" />
        <permission android:name="<Your package name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

