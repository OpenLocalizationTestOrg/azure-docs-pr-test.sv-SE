1. Skapa en ny klass i projektet med namnet hello `ToDoBroadcastReceiver`.
2. Lägg till hello följande using-instruktioner för**ToDoBroadcastReceiver** klass:
   
        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
3. Lägg till följande behörighetsbegäranden mellan hello hello **med** -satser och hello **namnområde** deklaration:
   
        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
   
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
4. Ersätta befintliga hello **ToDoBroadcastReceiver** klassen definition med hello följande:
   
        [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, 
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, 
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, 
        Categories = new string[] { "@PACKAGE_NAME@" })]
        public class ToDoBroadcastReceiver : GcmBroadcastReceiverBase<PushHandlerService>
        {
            // Set hello Google app ID.
            public static string[] senderIDs = new string[] { "<PROJECT_NUMBER>" };
        }
   
    I hello ovan koden, måste du ersätta  *`<PROJECT_NUMBER>`*  med hello-projektnummer som tilldelats av Google när du har etablerat din app i hello Google developer-portalen. 
5. Hello ToDoBroadcastReceiver.cs projektfilen, lägga till följande kod som definierar hello hello **PushHandlerService** klass:
   
        // hello ServiceAttribute must be applied toohello class.
        [Service] 
        public class PushHandlerService : GcmServiceBase
        {
            public static string RegistrationID { get; private set; }
   
            public PushHandlerService() : base(ToDoBroadcastReceiver.senderIDs) { }
        }
   
    Observera att den här klassen härleds från **GcmServiceBase** och den hello **Service** attributet måste vara tillämpas toothis klass.
   
   > [!NOTE]
   > Hej **GcmServiceBase** klassen implementerar hello **OnRegistered()**, **OnUnRegistered()**, **OnMessage()** och  **OnError()** metoder. Du måste åsidosätta de här metoderna i hello **PushHandlerService** klass.
   > 
   > 
6. Lägg till följande kod toohello hello **PushHandlerService** klass som åsidosätter hello **OnRegistered** händelsehanterare. 
   
        protected override void OnRegistered(Context context, string registrationId)
        {
            System.Diagnostics.Debug.WriteLine("hello device has been registered with GCM.", "Success!");
   
            // Get hello MobileServiceClient from hello current activity instance.
            MobileServiceClient client = ToDoActivity.CurrentActivity.CurrentClient;
            var push = client.GetPush();
   
            // Define a message body for GCM.
            const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";
   
            // Define hello template registration as JSON.
            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
              {"body", templateBodyGCM }
            };
   
            try
            {
                // Make sure we run hello registration on hello same thread as hello activity, 
                // tooavoid threading errors.
                ToDoActivity.CurrentActivity.RunOnUiThread(
   
                    // Register hello template with Notification Hubs.
                    async () => await push.RegisterAsync(registrationId, templates));
   
                System.Diagnostics.Debug.WriteLine(
                    string.Format("Push Installation Id", push.InstallationId.ToString()));
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(
                    string.Format("Error with Azure push registration: {0}", ex.Message));
            }
        }
   
    Den här metoden använder hello returnerade GCM registrering ID tooregister med Azure för push-meddelanden. Taggar kan endast läggas toohello registrering när den har skapats. Mer information finns i [så här: Lägg till taggar tooa enheten installation tooenable push-till-taggar](../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags).
7. Åsidosätt hello **OnMessage** metod i **PushHandlerService** med hello följande kod:
   
       protected override void OnMessage(Context context, Intent intent)
       {          
           string message = string.Empty;
   
           // Extract hello push notification message from hello intent.
           if (intent.Extras.ContainsKey("message"))
           {
               message = intent.Extras.Get("message").ToString();
               var title = "New item added:";
   
               // Create a notification manager toosend hello notification.
               var notificationManager = 
                   GetSystemService(Context.NotificationService) as NotificationManager;
   
               // Create a new intent tooshow hello notification in hello UI. 
               PendingIntent contentIntent = 
                   PendingIntent.GetActivity(context, 0, 
                   new Intent(this, typeof(ToDoActivity)), 0);              
   
               // Create hello notification using hello builder.
               var builder = new Notification.Builder(context);
               builder.SetAutoCancel(true);
               builder.SetContentTitle(title);
               builder.SetContentText(message);
               builder.SetSmallIcon(Resource.Drawable.ic_launcher);
               builder.SetContentIntent(contentIntent);
               var notification = builder.Build();
   
               // Display hello notification in hello Notifications Area.
               notificationManager.Notify(1, notification);
   
           }
       }
8. Åsidosätt hello **OnUnRegistered()** och **OnError()** metoder med hello följande kod.
   
       protected override void OnUnRegistered(Context context, string registrationId)
       {
           throw new NotImplementedException();
       }
   
       protected override void OnError(Context context, string errorId)
       {
           System.Diagnostics.Debug.WriteLine(
               string.Format("Error occurred in hello notification: {0}.", errorId));
       }

