



<span data-ttu-id="27c89-101">När du skickar meddelanden om mallar behöver du bara tooprovide en uppsättning egenskaper, i vårt fall skickar hello uppsättning egenskaper som innehåller hello lokaliserad version av hello aktuella nyheter, till exempel:</span><span class="sxs-lookup"><span data-stu-id="27c89-101">When you send template notifications you only need tooprovide a set of properties, in our case we will send hello set of properties containing hello localized version of hello current news, for instance:</span></span>

    {
        "News_English": "World News in English!",
        "News_French": "World News in French!",
        "News_Mandarin": "World News in Mandarin!"
    }


<span data-ttu-id="27c89-102">Det här avsnittet visas hur toosend meddelanden med hjälp av en konsolapp</span><span class="sxs-lookup"><span data-stu-id="27c89-102">This section shows how toosend notifications using a console app</span></span>

<span data-ttu-id="27c89-103">hello med kod sändningar tooboth Windows Store och iOS-enheter eftersom hello serverdel kan skicka tooany hello stöds enheter.</span><span class="sxs-lookup"><span data-stu-id="27c89-103">hello included code broadcasts tooboth Windows Store and iOS devices, since hello backend can broadcast tooany of hello supported devices.</span></span>

### <a name="toosend-notifications-using-a-c-console-app"></a><span data-ttu-id="27c89-104">toosend meddelanden med en C#-konsolapp</span><span class="sxs-lookup"><span data-stu-id="27c89-104">toosend notifications using a C# console app</span></span>
<span data-ttu-id="27c89-105">Ändra hello `SendTemplateNotificationAsync` metod i hello konsolprogram som du skapade tidigare med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="27c89-105">Modify hello `SendTemplateNotificationAsync` method in hello console app you previously created with hello following code.</span></span> <span data-ttu-id="27c89-106">Observera hur i det här fallet finns inget behov av toosend flera meddelanden för olika språk och plattformar.</span><span class="sxs-lookup"><span data-stu-id="27c89-106">Notice how in this case there is no need toosend multiple notifications for different locales and platforms.</span></span>

        private static async void SendTemplateNotificationAsync()
        {
            // Define hello notification hub.
            NotificationHubClient hub = 
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");

            // Sending hello notification as a template notification. All template registrations that contain 
            // "messageParam" or "News_<local selected>" and hello proper tags will receive hello notifications. 
            // This includes APNS, GCM, WNS, and MPNS template registrations.
            Dictionary<string, string> templateParams = new Dictionary<string, string>();

            // Create an array of breaking news categories.
            var categories = new string[] { "World", "Politics", "Business", "Technology", "Science", "Sports"};
            var locales = new string[] { "English", "French", "Mandarin" };

            foreach (var category in categories)
            {
                templateParams["messageParam"] = "Breaking " + category + " News!";

                // Sending localized News for each tag too...
                foreach( var locale in locales)
                {
                    string key = "News_" + locale;

                    // Your real localized news content would go here.
                    templateParams[key] = "Breaking " + category + " News in " + locale + "!";
                }

                await hub.SendTemplateNotificationAsync(templateParams, category);
            }
        }


<span data-ttu-id="27c89-107">Observera att det här enkla anropet levererar hello lokaliserade typ av Nyheter för**alla** dina enheter, oavsett hello plattform som din Meddelandehubb bygger och levererar hello rätt interna nyttolast tooall hello enheter prenumererar på tooa specifik tagg.</span><span class="sxs-lookup"><span data-stu-id="27c89-107">Note that this simple call will deliver hello localized piece of news too**all** your devices, irrespective of hello platform, as your Notification Hub builds and delivers hello correct native payload tooall hello devices subscribed tooa specific tag.</span></span>

### <a name="sending-hello-notification-with-mobile-services"></a><span data-ttu-id="27c89-108">Skicka hello-meddelande med Mobile Services</span><span class="sxs-lookup"><span data-stu-id="27c89-108">Sending hello notification with Mobile Services</span></span>
<span data-ttu-id="27c89-109">Du kan använda hello följande skript i din Mobiltjänst Schemaläggaren:</span><span class="sxs-lookup"><span data-stu-id="27c89-109">In your Mobile Service scheduler, you can use hello following script:</span></span>

    var azure = require('azure');
    var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string with full access>');
    var notification = {
            "News_English": "World News in English!",
            "News_French": "World News in French!",
            "News_Mandarin", "World News in Mandarin!"
    }
    notificationHubService.send('World', notification, function(error) {
        if (!error) {
            console.warn("Notification successful");
        }
    });


