I det här avsnittet uppdatera koden i Mobile Apps serverdel projektet att skicka ett push-meddelande varje gång en ny artikel har lagts till. Detta drivs av den [mallen](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) funktion i Azure Notification Hubs, aktivera push-meddelanden för flera plattformar. Olika klienter registreras för push-meddelanden med hjälp av mallar och en enda universal push får åtkomst till alla klientplattformar.

Välj något av följande procedurer som matchar din serverdel projekttypen&mdash;antingen [.NET-serverdel](#dotnet) eller [Node.js-serverdel](#nodejs).

### <a name="dotnet"></a>.NET backend-projekt
1. Högerklicka på serverprojekt i Visual Studio och klicka på **hantera NuGet-paket**. Sök efter `Microsoft.Azure.NotificationHubs`, och klicka sedan på **installera**. Detta installerar Notification Hubs-biblioteket för att skicka meddelanden från din serverdel.
2. Öppna i project server **domänkontrollanter** > **TodoItemController.cs**, och Lägg till följande using-instruktioner:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
3. I den **PostTodoItem** metod, Lägg till följande kod efter anropet till **InsertAsync**:  

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Sending the message so that all template registrations that contain "messageParam"
        // will receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
        Dictionary<string,string> templateParams = new Dictionary<string,string>();
        templateParams["messageParam"] = item.Text + " was added to the list.";

        try
        {
            // Send the push notification and log the results.
            var result = await hub.SendTemplateNotificationAsync(templateParams);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Detta skickar ett meddelande i mallen som innehåller objektet. Text när ett nytt objekt infogas.
4. Publicera om serverprojektet.

### <a name="nodejs"></a>Node.js backend-projekt
1. Om du inte redan gjort det, [hämta backend-snabbstartsprojekt](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), eller annan användning av [online redigeraren i Azure portal](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
2. Ersätt den befintliga koden i todoitem.js med följande:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define the template payload.
        var payload = '{"messageParam": "' + context.item.text + '" }';  

        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a template notification.
                    context.push.send(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;  

    Detta skickar ett meddelande i mallen som innehåller item.text när ett nytt objekt infogas.
3. Publicera om serverprojektet när du redigerar filen på den lokala datorn.
