



<span data-ttu-id="167d5-101">När du skickar meddelanden om mallar du behöver bara ange en uppsättning egenskaper, i vårt fall skickar vi en uppsättning egenskaper som innehåller den lokaliserade versionen av den aktuella nyheten, till exempel:</span><span class="sxs-lookup"><span data-stu-id="167d5-101">When you send template notifications you only need to provide a set of properties, in our case we will send the set of properties containing the localized version of the current news, for instance:</span></span>

    {
        "News_English": "World News in English!",
        "News_French": "World News in French!",
        "News_Mandarin": "World News in Mandarin!"
    }


<span data-ttu-id="167d5-102">Det här avsnittet visas hur du skickar meddelanden med hjälp av en konsolapp</span><span class="sxs-lookup"><span data-stu-id="167d5-102">This section shows how to send notifications using a console app</span></span>

<span data-ttu-id="167d5-103">Inkluderade koden skickar till Windows Store- och iOS-enheter, eftersom serverdelen kan skicka till någon av enheterna som stöds.</span><span class="sxs-lookup"><span data-stu-id="167d5-103">The included code broadcasts to both Windows Store and iOS devices, since the backend can broadcast to any of the supported devices.</span></span>

### <a name="to-send-notifications-using-a-c-console-app"></a><span data-ttu-id="167d5-104">Att skicka meddelanden med en C#-konsolapp</span><span class="sxs-lookup"><span data-stu-id="167d5-104">To send notifications using a C# console app</span></span>
<span data-ttu-id="167d5-105">Ändra den `SendTemplateNotificationAsync` metoden i konsolen appen som du skapade tidigare med följande kod.</span><span class="sxs-lookup"><span data-stu-id="167d5-105">Modify the `SendTemplateNotificationAsync` method in the console app you previously created with the following code.</span></span> <span data-ttu-id="167d5-106">Observera hur i det här fallet finns inget behov av att skicka flera meddelanden för olika språk och plattformar.</span><span class="sxs-lookup"><span data-stu-id="167d5-106">Notice how in this case there is no need to send multiple notifications for different locales and platforms.</span></span>

        private static async void SendTemplateNotificationAsync()
        {
            // Define the notification hub.
            NotificationHubClient hub = 
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");

            // Sending the notification as a template notification. All template registrations that contain 
            // "messageParam" or "News_<local selected>" and the proper tags will receive the notifications. 
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


<span data-ttu-id="167d5-107">Observera att det här enkla anropet levererar lokaliserade typ av nyheterna till **alla** dina enheter, oavsett plattformen som din Meddelandehubb bygger och ger korrekt interna nyttolasten för alla enheter som prenumererar på en specifik taggen.</span><span class="sxs-lookup"><span data-stu-id="167d5-107">Note that this simple call will deliver the localized piece of news to **all** your devices, irrespective of the platform, as your Notification Hub builds and delivers the correct native payload to all the devices subscribed to a specific tag.</span></span>

### <a name="sending-the-notification-with-mobile-services"></a><span data-ttu-id="167d5-108">Skicka meddelanden med Mobile Services</span><span class="sxs-lookup"><span data-stu-id="167d5-108">Sending the notification with Mobile Services</span></span>
<span data-ttu-id="167d5-109">Du kan använda följande skript i din Mobiltjänst Schemaläggaren:</span><span class="sxs-lookup"><span data-stu-id="167d5-109">In your Mobile Service scheduler, you can use the following script:</span></span>

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


