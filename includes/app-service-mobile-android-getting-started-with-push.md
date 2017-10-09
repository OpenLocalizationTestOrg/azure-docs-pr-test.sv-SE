1. <span data-ttu-id="78b93-101">I din **app** projekt, öppna hello fil `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="78b93-101">In your **app** project, open hello file `AndroidManifest.xml`.</span></span> <span data-ttu-id="78b93-102">Ersätt i hello kod i hello nästa två steg,  *`**my_app_package**`*  med hello hello app-paket för projektet.</span><span class="sxs-lookup"><span data-stu-id="78b93-102">In hello code in hello next two steps, replace *`**my_app_package**`* with hello name of hello app package for your project.</span></span> <span data-ttu-id="78b93-103">Detta är hello värdet för hello `package` attribut för hello `manifest` tagg.</span><span class="sxs-lookup"><span data-stu-id="78b93-103">This is hello value of hello `package` attribute of hello `manifest` tag.</span></span>
2. <span data-ttu-id="78b93-104">Lägg till följande nya behörigheter efter hello befintliga hello `uses-permission` element:</span><span class="sxs-lookup"><span data-stu-id="78b93-104">Add hello following new permissions after hello existing `uses-permission` element:</span></span>

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />
3. <span data-ttu-id="78b93-105">Lägg till följande kod efter hello hello `application` startkoden:</span><span class="sxs-lookup"><span data-stu-id="78b93-105">Add hello following code after hello `application` opening tag:</span></span>

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                         android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>
4. <span data-ttu-id="78b93-106">Öppna hello filen *ToDoActivity.java*, och Lägg till följande importuttryck hello:</span><span class="sxs-lookup"><span data-stu-id="78b93-106">Open hello file *ToDoActivity.java*, and add hello following import statement:</span></span>

        import com.microsoft.windowsazure.notifications.NotificationsManager;
5. <span data-ttu-id="78b93-107">Lägg till hello följande privata variabeln toohello klass.</span><span class="sxs-lookup"><span data-stu-id="78b93-107">Add hello following private variable toohello class.</span></span> <span data-ttu-id="78b93-108">Ersätt  *`<PROJECT_NUMBER>`*  med hello-projektnummer som tilldelats av Google tooyour app i hello föregående procedur.</span><span class="sxs-lookup"><span data-stu-id="78b93-108">Replace *`<PROJECT_NUMBER>`* with hello project number assigned by Google tooyour app in hello preceding procedure.</span></span>

        public static final String SENDER_ID = "<PROJECT_NUMBER>";
6. <span data-ttu-id="78b93-109">Ändra hello definitionen av *MobileServiceClient* från **privata** för**offentliga statiska**, så det nu ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="78b93-109">Change hello definition of *MobileServiceClient* from **private** too**public static**, so it now looks like this:</span></span>

        public static MobileServiceClient mClient;
7. <span data-ttu-id="78b93-110">Lägg till en ny klass toohandle meddelanden.</span><span class="sxs-lookup"><span data-stu-id="78b93-110">Add a new class toohandle notifications.</span></span> <span data-ttu-id="78b93-111">Öppna hello i Projektutforskaren **src** > **huvudsakliga** > **java** noder och högerklicka på hello paketet namn nod.</span><span class="sxs-lookup"><span data-stu-id="78b93-111">In Project Explorer, open hello **src** > **main** > **java** nodes, and right-click hello package name node.</span></span> <span data-ttu-id="78b93-112">Klicka på **ny**, och klicka sedan på **Java-klass**.</span><span class="sxs-lookup"><span data-stu-id="78b93-112">Click **New**, and then click **Java Class**.</span></span>
8. <span data-ttu-id="78b93-113">I **namn**, typen `MyHandler`, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="78b93-113">In **Name**, type `MyHandler`, and then click **OK**.</span></span>

    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)

9. <span data-ttu-id="78b93-114">Ersätt hello klassdeklarationen med i hello MyHandler fil:</span><span class="sxs-lookup"><span data-stu-id="78b93-114">In hello MyHandler file, replace hello class declaration with:</span></span>

        public class MyHandler extends NotificationsHandler {
10. <span data-ttu-id="78b93-115">Lägg till följande importuttryck för hello hello `MyHandler` klass:</span><span class="sxs-lookup"><span data-stu-id="78b93-115">Add hello following import statements for hello `MyHandler` class:</span></span>

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
11. <span data-ttu-id="78b93-116">Lägg till den här medlemmen toohello bredvid `MyHandler` klass:</span><span class="sxs-lookup"><span data-stu-id="78b93-116">Next add this member toohello `MyHandler` class:</span></span>

        public static final int NOTIFICATION_ID = 1;
12. <span data-ttu-id="78b93-117">I hello `MyHandler` klassen och Lägg till följande kod toooverride hello hello **onRegistered** metod som registrerar enheten med hello Mobiltjänst notification hub.</span><span class="sxs-lookup"><span data-stu-id="78b93-117">In hello `MyHandler` class, add hello following code toooverride hello **onRegistered** method, which registers your device with hello mobile service notification hub.</span></span>

        @Override
        public void onRegistered(Context context,  final String gcmRegistrationId) {
           super.onRegistered(context, gcmRegistrationId);

           new AsyncTask<Void, Void, Void>() {

               protected Void doInBackground(Void... params) {
                   try {
                       ToDoActivity.mClient.getPush().register(gcmRegistrationId);
                       return null;
                   }
                   catch(Exception e) {
                       // handle error                
                   }
                   return null;              
               }
           }.execute();
       <span data-ttu-id="78b93-118">}</span><span class="sxs-lookup"><span data-stu-id="78b93-118">}</span></span>
13. <span data-ttu-id="78b93-119">I hello `MyHandler` klassen och Lägg till följande kod toooverride hello hello **onReceive** metod, vilket gör hello meddelande toodisplay när den tas emot.</span><span class="sxs-lookup"><span data-stu-id="78b93-119">In hello `MyHandler` class, add hello following code toooverride hello **onReceive** method, which causes hello notification toodisplay when it is received.</span></span>

        @Override
        public void onReceive(Context context, Bundle bundle) {
               String msg = bundle.getString("message");

               PendingIntent contentIntent = PendingIntent.getActivity(context,
                       0, // requestCode
                       new Intent(context, ToDoActivity.class),
                       0); // flags

               Notification notification = new NotificationCompat.Builder(context)
                       .setSmallIcon(R.drawable.ic_launcher)
                       .setContentTitle("Notification Hub Demo")
                       .setStyle(new NotificationCompat.BigTextStyle().bigText(msg))
                       .setContentText(msg)
                       .setContentIntent(contentIntent)
                       .build();

               NotificationManager notificationManager = (NotificationManager)
                       context.getSystemService(Context.NOTIFICATION_SERVICE);
               notificationManager.notify(NOTIFICATION_ID, notification);
       <span data-ttu-id="78b93-120">}</span><span class="sxs-lookup"><span data-stu-id="78b93-120">}</span></span>
14. <span data-ttu-id="78b93-121">Uppdatera hello tillbaka i TodoActivity.java-filen för hello **onCreate** metod för hello *ToDoActivity* klassen tooregister hello notification hanterare klass.</span><span class="sxs-lookup"><span data-stu-id="78b93-121">Back in hello TodoActivity.java file, update hello **onCreate** method of hello *ToDoActivity* class tooregister hello notification handler class.</span></span> <span data-ttu-id="78b93-122">Se till att tooadd koden efter hello *MobileServiceClient* instansieras.</span><span class="sxs-lookup"><span data-stu-id="78b93-122">Make sure tooadd this code after hello *MobileServiceClient* is instantiated.</span></span>

        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    <span data-ttu-id="78b93-123">Appen är nu uppdaterade toosupport push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="78b93-123">Your app is now updated toosupport push notifications.</span></span>
