



När du skickar meddelanden om mallar behöver du bara tooprovide en uppsättning egenskaper, i vårt fall skickar hello uppsättning egenskaper som innehåller hello lokaliserad version av hello aktuella nyheter, till exempel:

    {
        "News_English": "World News in English!",
        "News_French": "World News in French!",
        "News_Mandarin": "World News in Mandarin!"
    }


Det här avsnittet visas hur toosend meddelanden med hjälp av en konsolapp

hello med kod sändningar tooboth Windows Store och iOS-enheter eftersom hello serverdel kan skicka tooany hello stöds enheter.

### <a name="toosend-notifications-using-a-c-console-app"></a>toosend meddelanden med en C#-konsolapp
Ändra hello `SendTemplateNotificationAsync` metod i hello konsolprogram som du skapade tidigare med hello följande kod. Observera hur i det här fallet finns inget behov av toosend flera meddelanden för olika språk och plattformar.

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


Observera att det här enkla anropet levererar hello lokaliserade typ av Nyheter för**alla** dina enheter, oavsett hello plattform som din Meddelandehubb bygger och levererar hello rätt interna nyttolast tooall hello enheter prenumererar på tooa specifik tagg.

### <a name="sending-hello-notification-with-mobile-services"></a>Skicka hello-meddelande med Mobile Services
Du kan använda hello följande skript i din Mobiltjänst Schemaläggaren:

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


