## <a name="webapi-project"></a><span data-ttu-id="d3fc5-101">WebAPI-projekt</span><span class="sxs-lookup"><span data-stu-id="d3fc5-101">WebAPI Project</span></span>
1. <span data-ttu-id="d3fc5-102">Öppna i Visual Studio den **AppBackend** projekt som du skapade i den **meddela användare** kursen.</span><span class="sxs-lookup"><span data-stu-id="d3fc5-102">In Visual Studio, open the **AppBackend** project that you created in the **Notify Users** tutorial.</span></span>
2. <span data-ttu-id="d3fc5-103">Ersätt hela i Notifications.cs, **meddelanden** klassen med följande kod.</span><span class="sxs-lookup"><span data-stu-id="d3fc5-103">In Notifications.cs, replace the whole **Notifications** class with the following code.</span></span> <span data-ttu-id="d3fc5-104">Se till att ersätta platshållarna med anslutningssträngen (med fullständig åtkomst) för meddelandehubben och hubbnamnet.</span><span class="sxs-lookup"><span data-stu-id="d3fc5-104">Be sure to replace the placeholders with your connection string (with full access) for your notification hub, and the hub name.</span></span> <span data-ttu-id="d3fc5-105">Du kan hämta dessa värden från den [klassiska Azure-portalen](http://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="d3fc5-105">You can obtain these values from the [Azure Classic Portal](http://manage.windowsazure.com).</span></span> <span data-ttu-id="d3fc5-106">Den här modulen representerar nu olika säkra meddelanden som skickas.</span><span class="sxs-lookup"><span data-stu-id="d3fc5-106">This module now represents the different secure notifications that will be sent.</span></span> <span data-ttu-id="d3fc5-107">En fullständig implementering ska meddelanden sparas i en databas. för enkelhetens skull lagrar i det här fallet vi dem i minnet.</span><span class="sxs-lookup"><span data-stu-id="d3fc5-107">In a complete implementation, the notifications will be stored in a database; for simplicity, in this case we store them in memory.</span></span>
   
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

1. <span data-ttu-id="d3fc5-108">I NotificationsController.cs, Ersätt Koden i den **NotificationsController** klassen med följande kod.</span><span class="sxs-lookup"><span data-stu-id="d3fc5-108">In NotificationsController.cs, replace the code inside the **NotificationsController** class definition with the following code.</span></span> <span data-ttu-id="d3fc5-109">Den här komponenten implementerar ett sätt för enheten för att hämta meddelandet på ett säkert sätt och ger också ett sätt (för den här självstudiekursen) för att utlösa en säker push till dina enheter.</span><span class="sxs-lookup"><span data-stu-id="d3fc5-109">This component implements a way for the device to retrieve the notification securely, and also provides a way (for the purposes of this tutorial) to trigger a secure push to your devices.</span></span> <span data-ttu-id="d3fc5-110">Observera att när du skickar meddelandet till meddelandehubben vi bara skicka en rå med ID meddelandet (och inga faktiska meddelanden):</span><span class="sxs-lookup"><span data-stu-id="d3fc5-110">Note that when sending the notification to the notification hub, we only send a raw notification with the ID of the notification (and no actual message):</span></span>
   
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


<span data-ttu-id="d3fc5-111">Observera att den `Post` metoden nu skickar ett popup-meddelande.</span><span class="sxs-lookup"><span data-stu-id="d3fc5-111">Note that the `Post` method now does not send a toast notification.</span></span> <span data-ttu-id="d3fc5-112">Skickar den ett raw meddelande som innehåller endast meddelande-ID och inte något känsligt innehåll.</span><span class="sxs-lookup"><span data-stu-id="d3fc5-112">It sends a raw notification that contains only the notification ID, and not any sensitive content.</span></span> <span data-ttu-id="d3fc5-113">Kontrollera också att kommentera send-åtgärden för plattformar som du inte har autentiseringsuppgifter som har konfigurerats på din meddelandehubb som de orsakar fel.</span><span class="sxs-lookup"><span data-stu-id="d3fc5-113">Also, make sure to comment the send operation for the platforms for which you do not have credentials configured on your notification hub, as they will result in errors.</span></span>

1. <span data-ttu-id="d3fc5-114">Vi ska nu omdistribuera den här appen till en Azure-webbplats för att göra den tillgänglig från alla enheter.</span><span class="sxs-lookup"><span data-stu-id="d3fc5-114">Now we will re-deploy this app to an Azure Website in order to make it accessible from all devices.</span></span> <span data-ttu-id="d3fc5-115">Högerklicka på **AppBackend**-projektet och välj **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="d3fc5-115">Right-click on the **AppBackend** project and select **Publish**.</span></span>
2. <span data-ttu-id="d3fc5-116">Välj Azure-webbplatsen som publicera-mål.</span><span class="sxs-lookup"><span data-stu-id="d3fc5-116">Select Azure Website as your publish target.</span></span> <span data-ttu-id="d3fc5-117">Logga in med ditt Azure-konto och markera en befintlig eller ny webbplats och anteckna den **Måladress** egenskap i den **anslutning** fliken.</span><span class="sxs-lookup"><span data-stu-id="d3fc5-117">Log in with your Azure account and select an existing or new Website, and make a note of the **destination URL** property in the **Connection** tab.</span></span> <span data-ttu-id="d3fc5-118">Vi ska referera till den här URL:en som *serverdelens slutpunkt* senare i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="d3fc5-118">We will refer to this URL as your *backend endpoint* later in this tutorial.</span></span> <span data-ttu-id="d3fc5-119">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="d3fc5-119">Click **Publish**.</span></span>

