<span data-ttu-id="d3efa-101">I det här avsnittet uppdatera koden i Mobile Apps serverdel projektet att skicka ett push-meddelande varje gång en ny artikel har lagts till.</span><span class="sxs-lookup"><span data-stu-id="d3efa-101">In this section, you update code in your existing Mobile Apps back-end project to send a push notification every time a new item is added.</span></span> <span data-ttu-id="d3efa-102">Detta drivs av den [mallen](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) funktion i Azure Notification Hubs, aktivera push-meddelanden för flera plattformar.</span><span class="sxs-lookup"><span data-stu-id="d3efa-102">This is powered by the [template](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) feature of Azure Notification Hubs, enabling cross-platform pushes.</span></span> <span data-ttu-id="d3efa-103">Olika klienter registreras för push-meddelanden med hjälp av mallar och en enda universal push får åtkomst till alla klientplattformar.</span><span class="sxs-lookup"><span data-stu-id="d3efa-103">The various clients are registered for push notifications using templates, and a single universal push can get to all client platforms.</span></span>

<span data-ttu-id="d3efa-104">Välj något av följande procedurer som matchar din serverdel projekttypen&mdash;antingen [.NET-serverdel](#dotnet) eller [Node.js-serverdel](#nodejs).</span><span class="sxs-lookup"><span data-stu-id="d3efa-104">Choose one of the following procedures that matches your back-end project type&mdash;either [.NET back end](#dotnet) or [Node.js back end](#nodejs).</span></span>

### <span data-ttu-id="d3efa-105"><a name="dotnet"></a>.NET backend-projekt</span><span class="sxs-lookup"><span data-stu-id="d3efa-105"><a name="dotnet"></a>.NET back-end project</span></span>
1. <span data-ttu-id="d3efa-106">Högerklicka på serverprojekt i Visual Studio och klicka på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="d3efa-106">In Visual Studio, right-click the server project and click **Manage NuGet Packages**.</span></span> <span data-ttu-id="d3efa-107">Sök efter `Microsoft.Azure.NotificationHubs`, och klicka sedan på **installera**.</span><span class="sxs-lookup"><span data-stu-id="d3efa-107">Search for `Microsoft.Azure.NotificationHubs`, and then click **Install**.</span></span> <span data-ttu-id="d3efa-108">Detta installerar Notification Hubs-biblioteket för att skicka meddelanden från din serverdel.</span><span class="sxs-lookup"><span data-stu-id="d3efa-108">This installs the Notification Hubs library for sending notifications from your back end.</span></span>
2. <span data-ttu-id="d3efa-109">Öppna i project server **domänkontrollanter** > **TodoItemController.cs**, och Lägg till följande using-instruktioner:</span><span class="sxs-lookup"><span data-stu-id="d3efa-109">In the server project, open **Controllers** > **TodoItemController.cs**, and add the following using statements:</span></span>

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
3. <span data-ttu-id="d3efa-110">I den **PostTodoItem** metod, Lägg till följande kod efter anropet till **InsertAsync**:</span><span class="sxs-lookup"><span data-stu-id="d3efa-110">In the **PostTodoItem** method, add the following code after the call to **InsertAsync**:</span></span>  

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

    <span data-ttu-id="d3efa-111">Detta skickar ett meddelande i mallen som innehåller objektet. Text när ett nytt objekt infogas.</span><span class="sxs-lookup"><span data-stu-id="d3efa-111">This sends a template notification that contains the item.Text when a new item is inserted.</span></span>
4. <span data-ttu-id="d3efa-112">Publicera om serverprojektet.</span><span class="sxs-lookup"><span data-stu-id="d3efa-112">Republish the server project.</span></span>

### <span data-ttu-id="d3efa-113"><a name="nodejs"></a>Node.js backend-projekt</span><span class="sxs-lookup"><span data-stu-id="d3efa-113"><a name="nodejs"></a>Node.js back-end project</span></span>
1. <span data-ttu-id="d3efa-114">Om du inte redan gjort det, [hämta backend-snabbstartsprojekt](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), eller annan användning av [online redigeraren i Azure portal](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span><span class="sxs-lookup"><span data-stu-id="d3efa-114">If you haven't already done so, [download the quickstart back-end project](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart), or else use the [online editor in the Azure portal](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span></span>
2. <span data-ttu-id="d3efa-115">Ersätt den befintliga koden i todoitem.js med följande:</span><span class="sxs-lookup"><span data-stu-id="d3efa-115">Replace the existing code in todoitem.js with the following:</span></span>

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

    <span data-ttu-id="d3efa-116">Detta skickar ett meddelande i mallen som innehåller item.text när ett nytt objekt infogas.</span><span class="sxs-lookup"><span data-stu-id="d3efa-116">This sends a template notification that contains the item.text when a new item is inserted.</span></span>
3. <span data-ttu-id="d3efa-117">Publicera om serverprojektet när du redigerar filen på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="d3efa-117">When editing the file on your local computer, republish the server project.</span></span>
