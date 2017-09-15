1. <span data-ttu-id="31348-101">I din **app** projekt, öppna filen `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="31348-101">In your **app** project, open the file `AndroidManifest.xml`.</span></span> <span data-ttu-id="31348-102">Ersätt i koden i följande två steg  *`**my_app_package**`*  med namnet på app-paket för projektet.</span><span class="sxs-lookup"><span data-stu-id="31348-102">In the code in the next two steps, replace *`**my_app_package**`* with the name of the app package for your project.</span></span> <span data-ttu-id="31348-103">Detta är värdet för den `package` attribut för den `manifest` tagg.</span><span class="sxs-lookup"><span data-stu-id="31348-103">This is the value of the `package` attribute of the `manifest` tag.</span></span>
2. <span data-ttu-id="31348-104">Lägg till följande nya behörigheter efter den befintliga `uses-permission` element:</span><span class="sxs-lookup"><span data-stu-id="31348-104">Add the following new permissions after the existing `uses-permission` element:</span></span>

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />
3. <span data-ttu-id="31348-105">Lägg till följande kod efter den `application` startkoden:</span><span class="sxs-lookup"><span data-stu-id="31348-105">Add the following code after the `application` opening tag:</span></span>

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                         android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>
4. <span data-ttu-id="31348-106">Öppna filen *ToDoActivity.java*, och Lägg till följande importuttryck:</span><span class="sxs-lookup"><span data-stu-id="31348-106">Open the file *ToDoActivity.java*, and add the following import statement:</span></span>

        import com.microsoft.windowsazure.notifications.NotificationsManager;
5. <span data-ttu-id="31348-107">Lägg till följande privata variabeln i klassen.</span><span class="sxs-lookup"><span data-stu-id="31348-107">Add the following private variable to the class.</span></span> <span data-ttu-id="31348-108">Ersätt  *`<PROJECT_NUMBER>`*  med projektnumret Google har tilldelats din app i föregående procedur.</span><span class="sxs-lookup"><span data-stu-id="31348-108">Replace *`<PROJECT_NUMBER>`* with the project number assigned by Google to your app in the preceding procedure.</span></span>

        public static final String SENDER_ID = "<PROJECT_NUMBER>";
6. <span data-ttu-id="31348-109">Ändra definitionen av *MobileServiceClient* från **privata** till **offentliga statiska**, så det nu ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="31348-109">Change the definition of *MobileServiceClient* from **private** to **public static**, so it now looks like this:</span></span>

        public static MobileServiceClient mClient;
7. <span data-ttu-id="31348-110">Lägg till en ny klass för att hantera meddelanden.</span><span class="sxs-lookup"><span data-stu-id="31348-110">Add a new class to handle notifications.</span></span> <span data-ttu-id="31348-111">Öppna i Projektutforskaren den **src** > **huvudsakliga** > **java** noder och högerklicka på noden paket namn.</span><span class="sxs-lookup"><span data-stu-id="31348-111">In Project Explorer, open the **src** > **main** > **java** nodes, and right-click the package name node.</span></span> <span data-ttu-id="31348-112">Klicka på **ny**, och klicka sedan på **Java-klass**.</span><span class="sxs-lookup"><span data-stu-id="31348-112">Click **New**, and then click **Java Class**.</span></span>
8. <span data-ttu-id="31348-113">I **namn**, typen `MyHandler`, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="31348-113">In **Name**, type `MyHandler`, and then click **OK**.</span></span>

    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)

9. <span data-ttu-id="31348-114">Ersätt klassdeklarationen med i filen MyHandler:</span><span class="sxs-lookup"><span data-stu-id="31348-114">In the MyHandler file, replace the class declaration with:</span></span>

        public class MyHandler extends NotificationsHandler {
10. <span data-ttu-id="31348-115">Lägg till följande importuttryck för den `MyHandler` klass:</span><span class="sxs-lookup"><span data-stu-id="31348-115">Add the following import statements for the `MyHandler` class:</span></span>

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
11. <span data-ttu-id="31348-116">Sedan till den här medlemmen till den `MyHandler` klass:</span><span class="sxs-lookup"><span data-stu-id="31348-116">Next add this member to the `MyHandler` class:</span></span>

        public static final int NOTIFICATION_ID = 1;
12. <span data-ttu-id="31348-117">I den `MyHandler` klassen och Lägg till följande kod för att åsidosätta den **onRegistered** metod som registrerar din enhet med meddelandehubben mobiltjänst.</span><span class="sxs-lookup"><span data-stu-id="31348-117">In the `MyHandler` class, add the following code to override the **onRegistered** method, which registers your device with the mobile service notification hub.</span></span>

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
       <span data-ttu-id="31348-118">}</span><span class="sxs-lookup"><span data-stu-id="31348-118">}</span></span>
13. <span data-ttu-id="31348-119">I den `MyHandler` klassen och Lägg till följande kod för att åsidosätta den **onReceive** metod, vilket gör meddelandet ska visas när den tas emot.</span><span class="sxs-lookup"><span data-stu-id="31348-119">In the `MyHandler` class, add the following code to override the **onReceive** method, which causes the notification to display when it is received.</span></span>

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
       <span data-ttu-id="31348-120">}</span><span class="sxs-lookup"><span data-stu-id="31348-120">}</span></span>
14. <span data-ttu-id="31348-121">Uppdatera tillbaka i TodoActivity.java-filen i **onCreate** metod för den *ToDoActivity* klassen för att registrera meddelande hanterare klassen.</span><span class="sxs-lookup"><span data-stu-id="31348-121">Back in the TodoActivity.java file, update the **onCreate** method of the *ToDoActivity* class to register the notification handler class.</span></span> <span data-ttu-id="31348-122">Se till att lägga till den här koden efter den *MobileServiceClient* instansieras.</span><span class="sxs-lookup"><span data-stu-id="31348-122">Make sure to add this code after the *MobileServiceClient* is instantiated.</span></span>

        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    <span data-ttu-id="31348-123">Appen har uppdaterats för att stödja push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="31348-123">Your app is now updated to support push notifications.</span></span>
