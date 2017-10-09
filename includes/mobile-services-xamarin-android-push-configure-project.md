
1. I hello lösning vy (eller **Solution Explorer** i Visual Studio), högerklicka på hello **komponenter** mapp, klickar du på **få fler komponenter...** , Sök efter hello **Google Cloud Messaging-klienten** komponenten och Lägg till den toohello projekt.
2. Öppna projektfilen för hello ToDoActivity.cs och Lägg till hello följande med instruktionen toohello klass:
   
        using Gcm.Client;
3. I hello **ToDoActivity** klassen, Lägg till följande nya koden hello: 
   
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
   
    Detta gör att du tooaccess hello mobila klienten instansen från hello push hanterare tjänstprocessen.
4. Lägg till följande kod toohello hello **OnCreate** metod efter hello **MobileServiceClient** skapas:
   
       // Set hello current instance of TodoActivity.
       instance = this;
   
       // Make sure hello GCM client is set up correctly.
       GcmClient.CheckDevice(this);
       GcmClient.CheckManifest(this);
   
       // Register hello app for push notifications.
       GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

Din **ToDoActivity** nu har förberetts för att lägga till push-meddelanden.

