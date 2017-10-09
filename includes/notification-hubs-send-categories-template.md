
<span data-ttu-id="77ef8-101">Det här avsnittet visar hur toosend senaste nytt som taggats mallen meddelanden från en .NET-konsolapp.</span><span class="sxs-lookup"><span data-stu-id="77ef8-101">This section shows how toosend breaking news as tagged template notifications from a .NET console app.</span></span>

<span data-ttu-id="77ef8-102">Om du använder Mobile Apps finns toohello [Lägg till push-meddelanden för Mobile Apps] självstudier och välj din plattform hello överst.</span><span class="sxs-lookup"><span data-stu-id="77ef8-102">If you are using Mobile Apps please refer toohello [Add push notifications for Mobile Apps] tutorial and select your platform at hello top.</span></span>

<span data-ttu-id="77ef8-103">Om du vill toouse Java eller PHP finns för[hur toouse Notification Hubs från Java/PHP].</span><span class="sxs-lookup"><span data-stu-id="77ef8-103">If you want toouse Java or PHP refer too[How toouse Notification Hubs from Java/PHP].</span></span> <span data-ttu-id="77ef8-104">Du kan skicka meddelanden från alla backend med den [Notification Hub REST-gränssnittet].</span><span class="sxs-lookup"><span data-stu-id="77ef8-104">You can send notifications from any backend using the [Notification Hubs REST interface].</span></span>

<span data-ttu-id="77ef8-105">Hoppa över steg 1 – 3 om du har skapat hello konsolprogram för att skicka meddelanden när du slutfört [Kom igång med Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="77ef8-105">Skip steps 1-3 if you created hello console app for sending notifications when you completed [Get started with Notification Hubs].</span></span>

1. <span data-ttu-id="77ef8-106">Skapa ett nytt Visual C#-konsolprogram i Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="77ef8-106">In Visual Studio create a new Visual C# console application:</span></span>
   
       ![][13]
2. <span data-ttu-id="77ef8-107">Hello Visual Studio huvudmenyn klickar du på **verktyg**, **Library Package Manager**, och **Pakethanterarkonsolen**sedan i konsolfönstret hello skriver du följande och tryck på **Ange**:</span><span class="sxs-lookup"><span data-stu-id="77ef8-107">In hello Visual Studio main menu, click **Tools**, **Library Package Manager**, and **Package Manager Console**, then in hello console window type the  following and press **Enter**:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    <span data-ttu-id="77ef8-108">Detta lägger till en referens toohello Azure Notification Hubs SDK med hjälp av hello [Microsoft.Azure.Notification Hubs NuGet-paketet].</span><span class="sxs-lookup"><span data-stu-id="77ef8-108">This adds a reference toohello Azure Notification Hubs SDK using hello [Microsoft.Azure.Notification Hubs NuGet package].</span></span>
3. <span data-ttu-id="77ef8-109">Öppna hello filen Program.cs och Lägg till följande hello `using` instruktionen:</span><span class="sxs-lookup"><span data-stu-id="77ef8-109">Open hello file Program.cs and add hello following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
4. <span data-ttu-id="77ef8-110">I hello `Program` klassen, Lägg till följande metod hello eller ersätta den om den redan finns:</span><span class="sxs-lookup"><span data-stu-id="77ef8-110">In hello `Program` class, add hello following method, or replace it if it already exists:</span></span>
   
        private static async void SendTemplateNotificationAsync()
        {
            // Define hello notification hub.
            NotificationHubClient hub =
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");
   
            // Create an array of breaking news categories.
            var categories = new string[] { "World", "Politics", "Business",
                                            "Technology", "Science", "Sports"};
   
            // Sending hello notification as a template notification. All template registrations that contain
            // "messageParam" and hello proper tags will receive hello notifications.
            // This includes APNS, GCM, WNS, and MPNS template registrations.
   
            Dictionary<string, string> templateParams = new Dictionary<string, string>();
   
            foreach (var category in categories)
            {
                templateParams["messageParam"] = "Breaking " + category + " News!";
                await hub.SendTemplateNotificationAsync(templateParams, category);
            }
         }
   
    <span data-ttu-id="77ef8-111">Den här koden skickar ett meddelande om mallen för varje hello sex taggar i hello Strängmatrisen.</span><span class="sxs-lookup"><span data-stu-id="77ef8-111">This code sends a template notification for each of hello six tags in hello string array.</span></span> <span data-ttu-id="77ef8-112">hello användning av taggar ser till att enheter får aviseringar endast för registrerade hello kategorier.</span><span class="sxs-lookup"><span data-stu-id="77ef8-112">hello use of tags makes sure that devices receive notifications only for hello registered categories.</span></span>
5. <span data-ttu-id="77ef8-113">Ersätt hello i hello ovan kod `<hub name>` och `<connection string with full access>` -platshållare med notification hub namn och hello anslutningssträngen för *DefaultFullSharedAccessSignature* hello instrumentpanel i din meddelandehubb .</span><span class="sxs-lookup"><span data-stu-id="77ef8-113">In hello above code, replace hello `<hub name>` and `<connection string with full access>` placeholders with your notification hub name and hello connection  string for *DefaultFullSharedAccessSignature* from hello dashboard of your notification hub.</span></span>
6. <span data-ttu-id="77ef8-114">Lägg till följande rader i hello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="77ef8-114">Add hello following lines in hello **Main** method:</span></span>
   
         SendTemplateNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="77ef8-115">Skapa hello-konsolapp.</span><span class="sxs-lookup"><span data-stu-id="77ef8-115">Build hello console app.</span></span>

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

<!-- URLs. -->
[Kom igång med Notification Hubs]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Notification Hub REST-gränssnittet]: http://msdn.microsoft.com/library/windowsazure/dn223264.aspx
[Lägg till push-meddelanden för Mobile Apps]: ../articles/app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md
[hur toouse Notification Hubs från Java/PHP]: ../articles/notification-hubs/notification-hubs-java-push-notification-tutorial.md
[Microsoft.Azure.Notification Hubs NuGet-paketet]: http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/
