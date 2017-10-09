## <a name="webapi-project"></a>WebAPI-projekt
1. Öppna i Visual Studio hello **AppBackend** projekt som du skapade i hello **meddela användare** kursen.
2. I Notifications.cs, Ersätt hello hela **meddelanden** klassen med följande kod hello. Vara säker på att tooreplace hello-platshållare med anslutningssträngen (med fullständig åtkomst) för meddelandehubben och hello hubbnamn. Du kan få värdena från hello [klassiska Azure-portalen](http://manage.windowsazure.com). Den här modulen representerar nu hello olika säkra meddelanden som skickas. En fullständig implementering ska hello meddelanden sparas i en databas. för enkelhetens skull lagrar i det här fallet vi dem i minnet.
   
        public class Notification
        {
            public int Id { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }

        public class Notifications
        {
            public static Notifications Instance = new Notifications();

            private List<Notification> notifications = new List<Notification>();

            public NotificationHubClient Hub { get; set; }

            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",     "{hub name}");
            }

            public Notification CreateNotification(string payload)
            {
                var notification = new Notification() {
                Id = notifications.Count,
                Payload = payload,
                Read = false
                };

                notifications.Add(notification);

                return notification;
            }

            public Notification ReadNotification(int id)
            {
                return notifications.ElementAt(id);
            }
        }

1. Ersätt hello kod inuti hello i NotificationsController.cs, **NotificationsController** klassen definition med hello följande kod. Den här komponenten implementerar ett sätt för hello enheten tooretrieve hello-meddelande på ett säkert sätt och ger ett sätt (som hello i den här kursen) tootrigger även en säker push tooyour enheter. Observera att när du skickar hello meddelandehubben toohello meddelande vi bara skicka en raw med hello ID hello meddelande (och inga faktiska meddelanden):
   
       public NotificationsController()
       {
           Notifications.Instance.CreateNotification("This is a secure notification!");
       }
   
       // GET api/notifications/id
       public Notification Get(int id)
       {
           return Notifications.Instance.ReadNotification(id);
       }
   
       public async Task<HttpResponseMessage> Post()
       {
           var secureNotificationInTheBackend = Notifications.Instance.CreateNotification("Secure confirmation.");
           var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;
   
           // windows
           var rawNotificationToBeSent = new Microsoft.Azure.NotificationHubs.WindowsNotification(secureNotificationInTheBackend.Id.ToString(),
                           new Dictionary<string, string> {
                               {"X-WNS-Type", "wns/raw"}
                           });
           await Notifications.Instance.Hub.SendNotificationAsync(rawNotificationToBeSent, usernameTag);
   
           // apns
           await Notifications.Instance.Hub.SendAppleNativeNotificationAsync("{\"aps\": {\"content-available\": 1}, \"secureId\": \"" + secureNotificationInTheBackend.Id.ToString() + "\"}", usernameTag);
   
           // gcm
           await Notifications.Instance.Hub.SendGcmNativeNotificationAsync("{\"data\": {\"secureId\": \"" + secureNotificationInTheBackend.Id.ToString() + "\"}}", usernameTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }


Observera att hello `Post` metoden nu skickar ett popup-meddelande. Skickar den ett raw meddelande som innehåller endast hello meddelande-ID och inte något känsligt innehåll. Se också till att toocomment hello skicka-åtgärden för hello plattformar som du inte har autentiseringsuppgifter som har konfigurerats på din meddelandehubb som de orsakar fel.

1. Nu kommer vi distribuera den här appen tooan Azure-webbplats i ordning toomake den tillgänglig från alla enheter. Högerklicka på hello **AppBackend** projektet och välj **publicera**.
2. Välj Azure-webbplatsen som publicera-mål. Logga in med ditt Azure-konto och markera en befintlig eller ny webbplats och anteckna hello **Måladress** egenskap i hello **anslutning** fliken. Vi kommer att referera toothis URL: en som din *backend endpoint* senare i den här kursen. Klicka på **Publicera**.

