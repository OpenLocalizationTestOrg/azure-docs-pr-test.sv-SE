
1. <span data-ttu-id="e0b04-101">I hello lösning vy (eller **Solution Explorer** i Visual Studio), högerklicka på hello **komponenter** mapp, klickar du på **få fler komponenter...** , Sök efter hello **Google Cloud Messaging-klienten** komponenten och Lägg till den toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="e0b04-101">In hello Solution view (or **Solution Explorer** in Visual Studio), right-click hello **Components** folder, click  **Get More Components...**, search for hello **Google Cloud Messaging Client** component and add it toohello project.</span></span>
2. <span data-ttu-id="e0b04-102">Öppna projektfilen för hello ToDoActivity.cs och Lägg till hello följande med instruktionen toohello klass:</span><span class="sxs-lookup"><span data-stu-id="e0b04-102">Open hello ToDoActivity.cs project file and add hello following using statement toohello class:</span></span>
   
        using Gcm.Client;
3. <span data-ttu-id="e0b04-103">I hello **ToDoActivity** klassen, Lägg till följande nya koden hello:</span><span class="sxs-lookup"><span data-stu-id="e0b04-103">In hello **ToDoActivity** class, add hello following new code:</span></span> 
   
        // Create a new instance field for this activity.
        static ToDoActivity instance = new ToDoActivity();
   
        // Return hello current activity instance.
        public static ToDoActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }
        // Return hello Mobile Services client.
        public MobileServiceClient CurrentClient
        {
            get
            {
                return client;
            }
        }
   
    <span data-ttu-id="e0b04-104">Detta gör att du tooaccess hello mobila klienten instansen från hello push hanterare tjänstprocessen.</span><span class="sxs-lookup"><span data-stu-id="e0b04-104">This enables you tooaccess hello mobile client instance from hello push handler service process.</span></span>
4. <span data-ttu-id="e0b04-105">Lägg till följande kod toohello hello **OnCreate** metod efter hello **MobileServiceClient** skapas:</span><span class="sxs-lookup"><span data-stu-id="e0b04-105">Add hello following code toohello **OnCreate** method, after hello **MobileServiceClient** is created:</span></span>
   
       // Set hello current instance of TodoActivity.
       instance = this;
   
       // Make sure hello GCM client is set up correctly.
       GcmClient.CheckDevice(this);
       GcmClient.CheckManifest(this);
   
       // Register hello app for push notifications.
       GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

<span data-ttu-id="e0b04-106">Din **ToDoActivity** nu har förberetts för att lägga till push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="e0b04-106">Your **ToDoActivity** is now prepared for adding push notifications.</span></span>

