<span data-ttu-id="4c4eb-101">I det här avsnittet kan uppdatera du kod i dina befintliga Mobile Apps serverdel projektet toosend ett push-meddelande varje gång ett nytt objekt läggs.</span><span class="sxs-lookup"><span data-stu-id="4c4eb-101">In this section, you update code in your existing Mobile Apps back-end project toosend a push notification every time a new item is added.</span></span> <span data-ttu-id="4c4eb-102">Detta drivs av hello [mallen](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) funktion i Azure Notification Hubs, aktivera push-meddelanden för flera plattformar.</span><span class="sxs-lookup"><span data-stu-id="4c4eb-102">This is powered by hello [template](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) feature of Azure Notification Hubs, enabling cross-platform pushes.</span></span> <span data-ttu-id="4c4eb-103">hello olika klienter registreras för push-meddelanden med hjälp av mallar och en enda universal push får tooall klientplattformar.</span><span class="sxs-lookup"><span data-stu-id="4c4eb-103">hello various clients are registered for push notifications using templates, and a single universal push can get tooall client platforms.</span></span>

<span data-ttu-id="4c4eb-104">Välj något av hello följande procedurer som matchar din serverdel projekttypen&mdash;antingen [.NET-serverdel](#dotnet) eller [Node.js-serverdel](#nodejs).</span><span class="sxs-lookup"><span data-stu-id="4c4eb-104">Choose one of hello following procedures that matches your back-end project type&mdash;either [.NET back end](#dotnet) or [Node.js back end](#nodejs).</span></span>

### <span data-ttu-id="4c4eb-105"><a name="dotnet"></a>.NET backend-projekt</span><span class="sxs-lookup"><span data-stu-id="4c4eb-105"><a name="dotnet"></a>.NET back-end project</span></span>
1. <span data-ttu-id="4c4eb-106">Högerklicka på hello serverprojekt i Visual Studio och klicka på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="4c4eb-106">In Visual Studio, right-click hello server project and click **Manage NuGet Packages**.</span></span> <span data-ttu-id="4c4eb-107">Sök efter `Microsoft.Azure.NotificationHubs`, och klicka sedan på **installera**.</span><span class="sxs-lookup"><span data-stu-id="4c4eb-107">Search for `Microsoft.Azure.NotificationHubs`, and then click **Install**.</span></span> <span data-ttu-id="4c4eb-108">Detta installerar hello Meddelandehubbar biblioteket för att skicka meddelanden från din serverdel.</span><span class="sxs-lookup"><span data-stu-id="4c4eb-108">This installs hello Notification Hubs library for sending notifications from your back end.</span></span>
2. <span data-ttu-id="4c4eb-109">Öppna i hello serverprojekt **domänkontrollanter** > **TodoItemController.cs**, och Lägg till följande hello med-uttryck:</span><span class="sxs-lookup"><span data-stu-id="4c4eb-109">In hello server project, open **Controllers** > **TodoItemController.cs**, and add hello following using statements:</span></span>

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
3. <span data-ttu-id="4c4eb-110">I hello **PostTodoItem** metod, lägga till hello följande kod efter hello-anropet för**InsertAsync**:</span><span class="sxs-lookup"><span data-stu-id="4c4eb-110">In hello **PostTodoItem** method, add hello following code after hello call too**InsertAsync**:</span></span>  

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

    <span data-ttu-id="4c4eb-111">Detta skickar ett meddelande i mallen som innehåller hello-objekt. Text när ett nytt objekt infogas.</span><span class="sxs-lookup"><span data-stu-id="4c4eb-111">This sends a template notification that contains hello item.Text when a new item is inserted.</span></span>
4. <span data-ttu-id="4c4eb-112">Publicera hello serverprojekt.</span><span class="sxs-lookup"><span data-stu-id="4c4eb-112">Republish hello server project.</span></span>

### <span data-ttu-id="4c4eb-113"><a name="nodejs"></a>Node.js backend-projekt</span><span class="sxs-lookup"><span data-stu-id="4c4eb-113"><a name="nodejs"></a>Node.js back-end project</span></span>
1. <span data-ttu-id="4c4eb-114">Om du inte redan gjort det, [hämta hello backend-snabbstartsprojekt](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), eller annan använda hello [online redigeraren i hello Azure-portalen](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span><span class="sxs-lookup"><span data-stu-id="4c4eb-114">If you haven't already done so, [download hello quickstart back-end project](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), or else use hello [online editor in hello Azure portal](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span></span>
2. <span data-ttu-id="4c4eb-115">Ersätt hello befintlig kod i todoitem.js med hello följande:</span><span class="sxs-lookup"><span data-stu-id="4c4eb-115">Replace hello existing code in todoitem.js with hello following:</span></span>

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

    <span data-ttu-id="4c4eb-116">Detta skickar ett meddelande i mallen som innehåller hello item.text när ett nytt objekt infogas.</span><span class="sxs-lookup"><span data-stu-id="4c4eb-116">This sends a template notification that contains hello item.text when a new item is inserted.</span></span>
3. <span data-ttu-id="4c4eb-117">Publicera om hello serverprojekt när du redigerar hello-fil på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="4c4eb-117">When editing hello file on your local computer, republish hello server project.</span></span>
