Använd hello-metod som passar din serverdel projekttypen&mdash;antingen [.NET-serverdel](#dotnet) eller [Node.js-serverdel](#nodejs).

### <a name="dotnet"></a>.NET backend-projekt
1. Högerklicka på hello serverprojekt i Visual Studio och klicka på **hantera NuGet-paket**. Sök efter `Microsoft.Azure.NotificationHubs`, och klicka sedan på **installera**. Detta installerar hello Meddelandehubbar klientbiblioteket.
2. Hello domänkontrollanter i mappen öppna TodoItemController.cs och Lägg till följande hello `using` instruktioner:

        using Microsoft.Azure.Mobile.Server.Config;
        using Microsoft.Azure.NotificationHubs;
3. Ersätt hello `PostTodoItem` metod med hello följande kod:  

        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            TodoItem current = await InsertAsync(item);
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

            // Android payload
            var androidNotificationPayload = "{ \"data\" : {\"message\":\"" + item.Text + "\"}}";

            try
            {
                // Send hello push notification and log hello results.
                var result = await hub.SendGcmNativeNotificationAsync(androidNotificationPayload);

                // Write hello success result toohello logs.
                config.Services.GetTraceWriter().Info(result.State.ToString());
            }
            catch (System.Exception ex)
            {
                // Write hello failure result toohello logs.
                config.Services.GetTraceWriter()
                    .Error(ex.Message, null, "Push.SendAsync Error");
            }
            return CreatedAtRoute("Tables", new { id = current.Id }, current);
        }

4. Publicera hello serverprojekt.

### <a name="nodejs"></a>Node.js backend-projekt
1. Om du inte redan gjort det, [hämta hello snabbstartsprojekt](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), eller annan använda hello [online redigeraren i hello Azure-portalen](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
2. Ersätt hello befintlig kod i hello todoitem.js filen med hello följande:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about hello Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define hello GCM payload.
        var payload = {
            "data": {
                "message": context.item.text
            }
        };   

        // Execute hello insert.  hello insert returns hello results as a Promise,
        // Do hello push as a post-execute action within hello promise flow.
        return context.execute()
            .then(function (results) {
                // Only do hello push if configured
                if (context.push) {
                    // Send a GCM native notification.
                    context.push.gcm.send(null, payload, function (error) {
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

    Detta skickar ett GCM-meddelande som innehåller hello item.text när nya todo-objektet infogas.
3. Publicera om hello serverprojekt när du redigerar hello-fil i den lokala datorn.
