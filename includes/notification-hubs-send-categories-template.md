
Det här avsnittet visar hur toosend senaste nytt som taggats mallen meddelanden från en .NET-konsolapp.

Om du använder Mobile Apps finns toohello [Lägg till push-meddelanden för Mobile Apps] självstudier och välj din plattform hello överst.

Om du vill toouse Java eller PHP finns för[hur toouse Notification Hubs från Java/PHP]. Du kan skicka meddelanden från alla backend med den [Notification Hub REST-gränssnittet].

Hoppa över steg 1 – 3 om du har skapat hello konsolprogram för att skicka meddelanden när du slutfört [Kom igång med Notification Hubs].

1. Skapa ett nytt Visual C#-konsolprogram i Visual Studio:
   
       ![][13]
2. Hello Visual Studio huvudmenyn klickar du på **verktyg**, **Library Package Manager**, och **Pakethanterarkonsolen**sedan i konsolfönstret hello skriver du följande och tryck på **Ange**:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Detta lägger till en referens toohello Azure Notification Hubs SDK med hjälp av hello [Microsoft.Azure.Notification Hubs NuGet-paketet].
3. Öppna hello filen Program.cs och Lägg till följande hello `using` instruktionen:
   
        using Microsoft.Azure.NotificationHubs;
4. I hello `Program` klassen, Lägg till följande metod hello eller ersätta den om den redan finns:
   
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
   
    Den här koden skickar ett meddelande om mallen för varje hello sex taggar i hello Strängmatrisen. hello användning av taggar ser till att enheter får aviseringar endast för registrerade hello kategorier.
5. Ersätt hello i hello ovan kod `<hub name>` och `<connection string with full access>` -platshållare med notification hub namn och hello anslutningssträngen för *DefaultFullSharedAccessSignature* hello instrumentpanel i din meddelandehubb .
6. Lägg till följande rader i hello hello **Main** metoden:
   
         SendTemplateNotificationAsync();
         Console.ReadLine();
7. Skapa hello-konsolapp.

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

<!-- URLs. -->
[Kom igång med Notification Hubs]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Notification Hub REST-gränssnittet]: http://msdn.microsoft.com/library/windowsazure/dn223264.aspx
[Lägg till push-meddelanden för Mobile Apps]: ../articles/app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md
[hur toouse Notification Hubs från Java/PHP]: ../articles/notification-hubs/notification-hubs-java-push-notification-tutorial.md
[Microsoft.Azure.Notification Hubs NuGet-paketet]: http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/
