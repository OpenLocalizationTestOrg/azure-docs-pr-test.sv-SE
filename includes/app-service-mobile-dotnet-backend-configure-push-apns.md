
* **.NET-serverdel (C#)**:      
  
  1. Högerklicka på hello serverprojekt i Visual Studio och klicka på **hantera NuGet-paket**, söka efter `Microsoft.Azure.NotificationHubs`, klicka på **installera**. Detta installerar hello Meddelandehubbar biblioteket för att skicka meddelanden från serverdelen.
  2. Öppna i hello backend Visual Studio-projekt, **domänkontrollanter** > **TodoItemController.cs**. Överst hello i hello-fil, lägger du till följande hello `using` instruktionen:
     
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

                // iOS payload
                var appleNotificationPayload = "{\"aps\":{\"alert\":\"" + item.Text + "\"}}";

                try
                {
                    // Send hello push notification and log hello results.
                    var result = await hub.SendAppleNativeNotificationAsync(appleNotificationPayload);

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

* **Node.js-serverdel** : 
  
  1. Om du inte redan gjort det, [hämta hello snabbstartsprojekt](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) eller annan använda hello [online redigeraren i hello Azure-portalen](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).    
  2. Ersätt hello todoitem.js tabell skript med hello följande kod:

            var azureMobileApps = require('azure-mobile-apps'),
                promises = require('azure-mobile-apps/src/utilities/promises'),
                logger = require('azure-mobile-apps/src/logger');

            var table = azureMobileApps.table();

            // When adding record, send a push notification via APNS
            table.insert(function (context) {
                // For details of hello Notification Hubs JavaScript SDK, 
                // see http://aka.ms/nodejshubs
                logger.info('Running TodoItem.insert');

                // Create a payload that contains hello new item Text.
                var payload = "{\"aps\":{\"alert\":\"" + context.item.text + "\"}}";

                // Execute hello insert; Push as a post-execute action when results are returned as a Promise.
                return context.execute()
                    .then(function (results) {
                        // Only do hello push if configured
                        if (context.push) {
                            context.push.apns.send(null, payload, function (error) {
                                if (error) {
                                    logger.error('Error while sending push notification: ', error);
                                } else {
                                    logger.info('Push notification sent successfully!');
                                }
                            });
                        }
                        return results;
                    })
                    .catch(function (error) {
                        logger.error('Error while running context.execute: ', error);
                    });
            });

            module.exports = table;

    2. Publicera om hello serverprojekt när du redigerar hello-fil på den lokala datorn.
