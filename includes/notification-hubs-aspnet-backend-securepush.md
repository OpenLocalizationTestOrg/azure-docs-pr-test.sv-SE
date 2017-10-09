## <a name="webapi-project"></a><span data-ttu-id="82f85-101">WebAPI-projekt</span><span class="sxs-lookup"><span data-stu-id="82f85-101">WebAPI Project</span></span>
1. <span data-ttu-id="82f85-102">Öppna i Visual Studio hello **AppBackend** projekt som du skapade i hello **meddela användare** kursen.</span><span class="sxs-lookup"><span data-stu-id="82f85-102">In Visual Studio, open hello **AppBackend** project that you created in hello **Notify Users** tutorial.</span></span>
2. <span data-ttu-id="82f85-103">I Notifications.cs, Ersätt hello hela **meddelanden** klassen med följande kod hello.</span><span class="sxs-lookup"><span data-stu-id="82f85-103">In Notifications.cs, replace hello whole **Notifications** class with hello following code.</span></span> <span data-ttu-id="82f85-104">Vara säker på att tooreplace hello-platshållare med anslutningssträngen (med fullständig åtkomst) för meddelandehubben och hello hubbnamn.</span><span class="sxs-lookup"><span data-stu-id="82f85-104">Be sure tooreplace hello placeholders with your connection string (with full access) for your notification hub, and hello hub name.</span></span> <span data-ttu-id="82f85-105">Du kan få värdena från hello [klassiska Azure-portalen](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="82f85-105">You can obtain these values from hello [Azure Classic Portal](http://manage.windowsazure.com).</span></span> <span data-ttu-id="82f85-106">Den här modulen representerar nu hello olika säkra meddelanden som skickas.</span><span class="sxs-lookup"><span data-stu-id="82f85-106">This module now represents hello different secure notifications that will be sent.</span></span> <span data-ttu-id="82f85-107">En fullständig implementering ska hello meddelanden sparas i en databas. för enkelhetens skull lagrar i det här fallet vi dem i minnet.</span><span class="sxs-lookup"><span data-stu-id="82f85-107">In a complete implementation, hello notifications will be stored in a database; for simplicity, in this case we store them in memory.</span></span>
   
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

1. <span data-ttu-id="82f85-108">Ersätt hello kod inuti hello i NotificationsController.cs, **NotificationsController** klassen definition med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="82f85-108">In NotificationsController.cs, replace hello code inside hello **NotificationsController** class definition with hello following code.</span></span> <span data-ttu-id="82f85-109">Den här komponenten implementerar ett sätt för hello enheten tooretrieve hello-meddelande på ett säkert sätt och ger ett sätt (som hello i den här kursen) tootrigger även en säker push tooyour enheter.</span><span class="sxs-lookup"><span data-stu-id="82f85-109">This component implements a way for hello device tooretrieve hello notification securely, and also provides a way (for hello purposes of this tutorial) tootrigger a secure push tooyour devices.</span></span> <span data-ttu-id="82f85-110">Observera att när du skickar hello meddelandehubben toohello meddelande vi bara skicka en raw med hello ID hello meddelande (och inga faktiska meddelanden):</span><span class="sxs-lookup"><span data-stu-id="82f85-110">Note that when sending hello notification toohello notification hub, we only send a raw notification with hello ID of hello notification (and no actual message):</span></span>
   
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


<span data-ttu-id="82f85-111">Observera att hello `Post` metoden nu skickar ett popup-meddelande.</span><span class="sxs-lookup"><span data-stu-id="82f85-111">Note that hello `Post` method now does not send a toast notification.</span></span> <span data-ttu-id="82f85-112">Skickar den ett raw meddelande som innehåller endast hello meddelande-ID och inte något känsligt innehåll.</span><span class="sxs-lookup"><span data-stu-id="82f85-112">It sends a raw notification that contains only hello notification ID, and not any sensitive content.</span></span> <span data-ttu-id="82f85-113">Se också till att toocomment hello skicka-åtgärden för hello plattformar som du inte har autentiseringsuppgifter som har konfigurerats på din meddelandehubb som de orsakar fel.</span><span class="sxs-lookup"><span data-stu-id="82f85-113">Also, make sure toocomment hello send operation for hello platforms for which you do not have credentials configured on your notification hub, as they will result in errors.</span></span>

1. <span data-ttu-id="82f85-114">Nu kommer vi distribuera den här appen tooan Azure-webbplats i ordning toomake den tillgänglig från alla enheter.</span><span class="sxs-lookup"><span data-stu-id="82f85-114">Now we will re-deploy this app tooan Azure Website in order toomake it accessible from all devices.</span></span> <span data-ttu-id="82f85-115">Högerklicka på hello **AppBackend** projektet och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="82f85-115">Right-click on hello **AppBackend** project and select **Publish**.</span></span>
2. <span data-ttu-id="82f85-116">Välj Azure-webbplatsen som publicera-mål.</span><span class="sxs-lookup"><span data-stu-id="82f85-116">Select Azure Website as your publish target.</span></span> <span data-ttu-id="82f85-117">Logga in med ditt Azure-konto och markera en befintlig eller ny webbplats och anteckna hello **Måladress** egenskap i hello **anslutning** fliken. Vi kommer att referera toothis URL: en som din *backend endpoint* senare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="82f85-117">Log in with your Azure account and select an existing or new Website, and make a note of hello **destination URL** property in hello **Connection** tab. We will refer toothis URL as your *backend endpoint* later in this tutorial.</span></span> <span data-ttu-id="82f85-118">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="82f85-118">Click **Publish**.</span></span>

