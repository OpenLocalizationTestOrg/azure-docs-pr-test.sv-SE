1. <span data-ttu-id="c1586-101">Skapa en ny klass i projektet med namnet hello `ToDoBroadcastReceiver`.</span><span class="sxs-lookup"><span data-stu-id="c1586-101">Create a new class in hello project called `ToDoBroadcastReceiver`.</span></span>
2. <span data-ttu-id="c1586-102">Lägg till hello följande using-instruktioner för**ToDoBroadcastReceiver** klass:</span><span class="sxs-lookup"><span data-stu-id="c1586-102">Add hello following using statements too**ToDoBroadcastReceiver** class:</span></span>
   
        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
3. <span data-ttu-id="c1586-103">Lägg till följande behörighetsbegäranden mellan hello hello **med** -satser och hello **namnområde** deklaration:</span><span class="sxs-lookup"><span data-stu-id="c1586-103">Add hello following permission requests between hello **using** statements and hello **namespace** declaration:</span></span>
   
        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
   
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
4. <span data-ttu-id="c1586-104">Ersätta befintliga hello **ToDoBroadcastReceiver** klassen definition med hello följande:</span><span class="sxs-lookup"><span data-stu-id="c1586-104">Replace hello existing **ToDoBroadcastReceiver** class definition with hello following:</span></span>
   
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
   
    <span data-ttu-id="c1586-105">I hello ovan koden, måste du ersätta  *`<PROJECT_NUMBER>`*  med hello-projektnummer som tilldelats av Google när du har etablerat din app i hello Google developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1586-105">In hello above code, you must replace *`<PROJECT_NUMBER>`* with hello project number assigned by Google when you provisioned your app in hello Google developer portal.</span></span> 
5. <span data-ttu-id="c1586-106">Hello ToDoBroadcastReceiver.cs projektfilen, lägga till följande kod som definierar hello hello **PushHandlerService** klass:</span><span class="sxs-lookup"><span data-stu-id="c1586-106">In hello ToDoBroadcastReceiver.cs project file, add hello following code that defines hello **PushHandlerService** class:</span></span>
   
        // hello ServiceAttribute must be applied toohello class.
        [Service] 
        public class PushHandlerService : GcmServiceBase
        {
            public static string RegistrationID { get; private set; }
   
            public PushHandlerService() : base(ToDoBroadcastReceiver.senderIDs) { }
        }
   
    <span data-ttu-id="c1586-107">Observera att den här klassen härleds från **GcmServiceBase** och den hello **Service** attributet måste vara tillämpas toothis klass.</span><span class="sxs-lookup"><span data-stu-id="c1586-107">Note that this class derives from **GcmServiceBase** and that hello **Service** attribute must be applied toothis class.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="c1586-108">Hej **GcmServiceBase** klassen implementerar hello **OnRegistered()**, **OnUnRegistered()**, **OnMessage()** och  **OnError()** metoder.</span><span class="sxs-lookup"><span data-stu-id="c1586-108">hello **GcmServiceBase** class implements hello **OnRegistered()**, **OnUnRegistered()**, **OnMessage()** and **OnError()** methods.</span></span> <span data-ttu-id="c1586-109">Du måste åsidosätta de här metoderna i hello **PushHandlerService** klass.</span><span class="sxs-lookup"><span data-stu-id="c1586-109">You must override these methods in hello **PushHandlerService** class.</span></span>
   > 
   > 
6. <span data-ttu-id="c1586-110">Lägg till följande kod toohello hello **PushHandlerService** klass som åsidosätter hello **OnRegistered** händelsehanterare.</span><span class="sxs-lookup"><span data-stu-id="c1586-110">Add hello following code toohello **PushHandlerService** class that overrides hello **OnRegistered** event handler.</span></span> 
   
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
   
    <span data-ttu-id="c1586-111">Den här metoden använder hello returnerade GCM registrering ID tooregister med Azure för push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c1586-111">This method uses hello returned GCM registration ID tooregister with Azure for push notifications.</span></span> <span data-ttu-id="c1586-112">Taggar kan endast läggas toohello registrering när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="c1586-112">Tags can only be added toohello registration after it is created.</span></span> <span data-ttu-id="c1586-113">Mer information finns i [så här: Lägg till taggar tooa enheten installation tooenable push-till-taggar](../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags).</span><span class="sxs-lookup"><span data-stu-id="c1586-113">For more information, see [How to: Add tags tooa device installation tooenable push-to-tags](../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags).</span></span>
7. <span data-ttu-id="c1586-114">Åsidosätt hello **OnMessage** metod i **PushHandlerService** med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="c1586-114">Override hello **OnMessage** method in **PushHandlerService** with hello following code:</span></span>
   
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
8. <span data-ttu-id="c1586-115">Åsidosätt hello **OnUnRegistered()** och **OnError()** metoder med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="c1586-115">Override hello **OnUnRegistered()** and **OnError()** methods with hello following code.</span></span>
   
       protected override void OnUnRegistered(Context context, string registrationId)
       {
           throw new NotImplementedException();
       }
   
       protected override void OnError(Context context, string errorId)
       {
           System.Diagnostics.Debug.WriteLine(
               string.Format("Error occurred in hello notification: {0}.", errorId));
       }

