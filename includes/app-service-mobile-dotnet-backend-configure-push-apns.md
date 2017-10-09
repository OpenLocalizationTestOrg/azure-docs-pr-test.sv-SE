
* <span data-ttu-id="6104e-101">**.NET-serverdel (C#)**:</span><span class="sxs-lookup"><span data-stu-id="6104e-101">**.NET backend (C#)**:</span></span>      
  
  1. <span data-ttu-id="6104e-102">Högerklicka på hello serverprojekt i Visual Studio och klicka på **hantera NuGet-paket**, söka efter `Microsoft.Azure.NotificationHubs`, klicka på **installera**.</span><span class="sxs-lookup"><span data-stu-id="6104e-102">In Visual Studio, right-click hello server project and click **Manage NuGet Packages**, search for `Microsoft.Azure.NotificationHubs`, then click **Install**.</span></span> <span data-ttu-id="6104e-103">Detta installerar hello Meddelandehubbar biblioteket för att skicka meddelanden från serverdelen.</span><span class="sxs-lookup"><span data-stu-id="6104e-103">This installs hello Notification Hubs library for sending notifications from your backend.</span></span>
  2. <span data-ttu-id="6104e-104">Öppna i hello backend Visual Studio-projekt, **domänkontrollanter** > **TodoItemController.cs**.</span><span class="sxs-lookup"><span data-stu-id="6104e-104">In hello backend's Visual Studio project, open **Controllers** > **TodoItemController.cs**.</span></span> <span data-ttu-id="6104e-105">Överst hello i hello-fil, lägger du till följande hello `using` instruktionen:</span><span class="sxs-lookup"><span data-stu-id="6104e-105">At hello top of hello file, add hello following `using` statement:</span></span>
     
          using Microsoft.Azure.Mobile.Server.Config;
          using Microsoft.Azure.NotificationHubs;

    3. <span data-ttu-id="6104e-106">Ersätt hello `PostTodoItem` metod med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="6104e-106">Replace hello `PostTodoItem` method with hello following code:</span></span>  

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

    4. <span data-ttu-id="6104e-107">Publicera hello serverprojekt.</span><span class="sxs-lookup"><span data-stu-id="6104e-107">Republish hello server project.</span></span>

* <span data-ttu-id="6104e-108">**Node.js-serverdel** :</span><span class="sxs-lookup"><span data-stu-id="6104e-108">**Node.js backend** :</span></span> 
  
  1. <span data-ttu-id="6104e-109">Om du inte redan gjort det, [hämta hello snabbstartsprojekt](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) eller annan använda hello [online redigeraren i hello Azure-portalen](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span><span class="sxs-lookup"><span data-stu-id="6104e-109">If you haven't already done so, [download hello quickstart project](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) or else use hello [online editor in hello Azure portal](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span></span>    
  2. <span data-ttu-id="6104e-110">Ersätt hello todoitem.js tabell skript med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="6104e-110">Replace hello todoitem.js table script with hello following code:</span></span>

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

    2. <span data-ttu-id="6104e-111">Publicera om hello serverprojekt när du redigerar hello-fil på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="6104e-111">When editing hello file on your local computer, republish hello server project.</span></span>
