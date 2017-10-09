I det här avsnittet kan uppdatera du kod i dina befintliga Mobile Apps serverdel projektet toosend ett push-meddelande varje gång ett nytt objekt läggs. Detta drivs av hello [mallen](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) funktion i Azure Notification Hubs, aktivera push-meddelanden för flera plattformar. hello olika klienter registreras för push-meddelanden med hjälp av mallar och en enda universal push får tooall klientplattformar.

Välj något av hello följande procedurer som matchar din serverdel projekttypen&mdash;antingen [.NET-serverdel](#dotnet) eller [Node.js-serverdel](#nodejs).

### <a name="dotnet"></a>.NET backend-projekt
1. Högerklicka på hello serverprojekt i Visual Studio och klicka på **hantera NuGet-paket**. Sök efter `Microsoft.Azure.NotificationHubs`, och klicka sedan på **installera**. Detta installerar hello Meddelandehubbar biblioteket för att skicka meddelanden från din serverdel.
2. Öppna i hello serverprojekt **domänkontrollanter** > **TodoItemController.cs**, och Lägg till följande hello med-uttryck:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
3. I hello **PostTodoItem** metod, lägga till hello följande kod efter hello-anropet för**InsertAsync**:  

        // Get hello settings for hello server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get hello Notification Hubs credentials for hello Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Sending hello message so that all template registrations that contain "messageParam"
        // will receive hello notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
        Dictionary<string,string> templateParams = new Dictionary<string,string>();
        templateParams["messageParam"] = item.Text + " was added toohello list.";

        try
        {
            // Send hello push notification and log hello results.
            var result = await hub.SendTemplateNotificationAsync(templateParams);

            // Write hello success result toohello logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write hello failure result toohello logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Detta skickar ett meddelande i mallen som innehåller hello-objekt. Text när ett nytt objekt infogas.
4. Publicera hello serverprojekt.

### <a name="nodejs"></a>Node.js backend-projekt
1. Om du inte redan gjort det, [hämta hello backend-snabbstartsprojekt](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), eller annan använda hello [online redigeraren i hello Azure-portalen](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
2. Ersätt hello befintlig kod i todoitem.js med hello följande:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about hello Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define hello template payload.
        var payload = '{"messageParam": "' + context.item.text + '" }';  

        // Execute hello insert.  hello insert returns hello results as a Promise,
        // Do hello push as a post-execute action within hello promise flow.
        return context.execute()
            .then(function (results) {
                // Only do hello push if configured
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
                // Don't forget tooreturn hello results from hello context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;  

    Detta skickar ett meddelande i mallen som innehåller hello item.text när ett nytt objekt infogas.
3. Publicera om hello serverprojekt när du redigerar hello-fil på den lokala datorn.
