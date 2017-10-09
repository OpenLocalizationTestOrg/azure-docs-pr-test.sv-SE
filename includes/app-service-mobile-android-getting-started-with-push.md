1. I din **app** projekt, öppna hello fil `AndroidManifest.xml`. Ersätt i hello kod i hello nästa två steg,  *`**my_app_package**`*  med hello hello app-paket för projektet. Detta är hello värdet för hello `package` attribut för hello `manifest` tagg.
2. Lägg till följande nya behörigheter efter hello befintliga hello `uses-permission` element:

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />
3. Lägg till följande kod efter hello hello `application` startkoden:

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                         android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>
4. Öppna hello filen *ToDoActivity.java*, och Lägg till följande importuttryck hello:

        import com.microsoft.windowsazure.notifications.NotificationsManager;
5. Lägg till hello följande privata variabeln toohello klass. Ersätt  *`<PROJECT_NUMBER>`*  med hello-projektnummer som tilldelats av Google tooyour app i hello föregående procedur.

        public static final String SENDER_ID = "<PROJECT_NUMBER>";
6. Ändra hello definitionen av *MobileServiceClient* från **privata** för**offentliga statiska**, så det nu ser ut så här:

        public static MobileServiceClient mClient;
7. Lägg till en ny klass toohandle meddelanden. Öppna hello i Projektutforskaren **src** > **huvudsakliga** > **java** noder och högerklicka på hello paketet namn nod. Klicka på **ny**, och klicka sedan på **Java-klass**.
8. I **namn**, typen `MyHandler`, och klicka sedan på **OK**.

    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)

9. Ersätt hello klassdeklarationen med i hello MyHandler fil:

        public class MyHandler extends NotificationsHandler {
10. Lägg till följande importuttryck för hello hello `MyHandler` klass:

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
11. Lägg till den här medlemmen toohello bredvid `MyHandler` klass:

        public static final int NOTIFICATION_ID = 1;
12. I hello `MyHandler` klassen och Lägg till följande kod toooverride hello hello **onRegistered** metod som registrerar enheten med hello Mobiltjänst notification hub.

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
       }
13. I hello `MyHandler` klassen och Lägg till följande kod toooverride hello hello **onReceive** metod, vilket gör hello meddelande toodisplay när den tas emot.

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
       }
14. Uppdatera hello tillbaka i TodoActivity.java-filen för hello **onCreate** metod för hello *ToDoActivity* klassen tooregister hello notification hanterare klass. Se till att tooadd koden efter hello *MobileServiceClient* instansieras.

        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    Appen är nu uppdaterade toosupport push-meddelanden.
