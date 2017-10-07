---
title: aaaRegistration Management
description: "Det här avsnittet beskrivs hur tooregister enheter med notification hubs finns i ordning tooreceive push-meddelanden."
services: notification-hubs
documentationcenter: .net
author: ysxu
manager: erikre
editor: 
ms.assetid: fd0ee230-132c-4143-b4f9-65cef7f463a1
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 76471a45c7a0da1614ceed82b73cdb3319979ff7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="registration-management"></a>Registreringshantering
## <a name="overview"></a>Översikt
Det här avsnittet beskrivs hur tooregister enheter med notification hubs finns i ordning tooreceive push-meddelanden. hello avsnittet beskriver registreringar på en hög nivå och sedan introducerar hello två huvudsakliga mönster för att registrera enheter: registrering från hello enhet direkt toohello meddelandehubb och registrera via en serverdel för programmet. 

## <a name="what-is-device-registration"></a>Vad är registrering av enheten
Registrering av enheten med en Meddelandehubb åstadkoms med hjälp av en **registrering** eller **Installation**.

#### <a name="registrations"></a>Registreringar
En registrering associerar hello Platform Notification Service (PNS) hantera för en enhet med taggar och eventuellt en mall. Hej PNS-handtag kan vara en ChannelURI eller enhetstoken GCM registrerings-id. Taggar är används tooroute meddelanden toohello rätt uppsättning enhetshandtag. Mer information finns i [Routning och Tagguttryck](notification-hubs-tags-segment-push-message.md). Mallar är används tooimplement per registrering omvandling. Mer information finns i [mallar](notification-hubs-templates-cross-platform-push-messages.md).

#### <a name="installations"></a>Installationer
En Installation är en förbättrad registrering som innehåller en egenskapsuppsättning för push-relaterade egenskaper. Det är hello senaste och bästa metoden tooregistering dina enheter. Men det inte stöds av .NET SDK på klientsidan ([Notification Hub SDK för backend-åtgärder](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) ännu.  Det innebär att om du registrerar från hello själva klientenheten har du toouse hello [Notification Hub REST API](https://msdn.microsoft.com/library/mt621153.aspx) hanterar toosupport installationer. Om du använder en serverdelstjänst, bör du kunna toouse [Notification Hub SDK för backend-åtgärder](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

hello nedan följer några viktiga fördelar toousing installationer:

* Skapar eller uppdaterar en installation är helt idempotent. Så du kan köra det igen utan frågor om duplicerade registreringar.
* Hej installationsmodell gör det enkelt toodo enskilda push - specifik enhet som mål. En system-tagg **”$InstallationId: [installationId]”** läggs till automatiskt med varje installation baserad registrering. Så kan du anropa en skicka toothis taggen tootarget en specifik enhet utan att behöva toodo ytterligare kodning.
* Med installationer kan också du toodo partiella registrering uppdateringar. hello deluppdatering av en installation krävs en korrigering metod med hello [JSON-korrigering standard](https://tools.ietf.org/html/rfc6902). Detta är särskilt användbart när du vill tooupdate taggar på hello registrering. Du inte har toopull ned hello hela registrering och skicka om alla tidigare hello-taggar.

En installation kan innehålla hello hello följande egenskaper. En fullständig lista över hello installationen egenskaper Se [skapa eller skriva över en Installation med REST API](https://msdn.microsoft.com/library/azure/mt621153.aspx) eller [installationsegenskaper](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx) för hello.

    // Example installation format tooshow some supported properties
    {
        installationId: "",
        expirationTime: "",
        tags: [],
        platform: "",
        pushChannel: "",
        ………
        templates: {
            "templateName1" : {
                body: "",
                tags: [] },
            "templateName2" : {
                body: "",
                // Headers are for Windows Store only
                headers: {
                    "X-WNS-Type": "wns/tile" }
                tags: [] }
        },
        secondaryTiles: {
            "tileId1": {
                pushChannel: "",
                tags: [],
                templates: {
                    "otherTemplate": {
                        bodyTemplate: "",
                        headers: {
                            ... }
                        tags: [] }
                }
            }
        }
    }



Det är viktigt toonote som registreringar och installationer som standard inte längre upphör att gälla.

Registreringar och installationer måste innehålla en giltig PNS-referens för varje enhet/kanal. Eftersom PNS-handtag kan endast hämtas i ett klientprogram på hello enhet, är en mönstret tooregister direkt på enheten med hello-klientappen. På hello relaterade andra hand, säkerhetsaspekter och affärslogik tootags kan kräva att du toomanage device registration hello appens serverdel. 

#### <a name="templates"></a>Mallar
Om du vill toouse [mallar](notification-hubs-templates-cross-platform-push-messages.md), hello enhetsinstallation också hålla alla mallar som är associerade med enheten i en JSON-format (se exemplet ovan). hello mallnamn hjälpa riktade olika mallar för hello samma enhet.

Observera att varje mallnamn mappas tooa mall text och ett valfritt antal taggar. Dessutom kan varje plattform har ytterligare egenskaper. För Windows Store (med WNS) och Windows Phone 8 (med MPNS), kan ytterligare en uppsättning huvuden ingå i hello mallen. Du kan ange ett konstant eller -tooa malluttryck en utgången egenskapen tooeither APN hello gäller. En fullständig lista över hello installationen egenskaper Se [skapa eller skriva över en Installation med övriga](https://msdn.microsoft.com/library/azure/mt621153.aspx) avsnittet.

#### <a name="secondary-tiles-for-windows-store-apps"></a>Sekundär paneler för Windows Store-appar
För Windows Store-klientprogram hello skicka meddelanden toosecondary paneler är detsamma som att skicka dem toohello primära. Detta stöds även för installationer. Observera att sekundära brickor har en annan ChannelUri vilka hello SDK på din klientapp hanterar transparent.

Hej SecondaryTiles ordlista använder hello samma TileId som används toocreate hello SecondaryTiles objektet i Windows Store-app.
Som med hello kan primära ChannelUri, ChannelUris sekundära brickor ändra när som helst. I ordning tookeep hello installationer i hello notification hub uppdaterats hello enheten måste uppdatera dem med hello aktuella ChannelUris hello sekundära brickor.

## <a name="registration-management-from-hello-device"></a>Hantering av registrering från hello-enhet
När du hanterar registrering av enheten från klientprogram ansvarar endast hello serverdelen för att skicka meddelanden. Klientprogram hålla PNS-handtag in toodate och registrera taggar. hello följande bild illustrerar detta mönster.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-device.png)

hello enheten först hämtar hello PNS hantera från hello PNS sedan registrerar hello meddelandehubben direkt. När hello registreringen är klar kan hello appserverdelen skicka ett meddelande som mål att registreringen. Mer information om hur toosend meddelanden finns [Routning och Tagguttryck](notification-hubs-tags-segment-push-message.md).
Observera att i det här fallet används bara lyssna rättigheter tooaccess din notification hubs från hello enhet. Mer information finns i [säkerhet](notification-hubs-push-notification-security.md).

Hello enklaste metoden är att registrera från hello enhet, men den har vissa nackdelar.
hello första Nackdelen är att ett klientprogram kan uppdatera taggarna endast när hello appen är aktiv. Till exempel om en användare har två enheter som registreras taggar relaterade toosport team när hello första enheten registreras för en ytterligare tagg (till exempel Seahawks), får hello andra enhet hello meddelanden om hello Seahawks tills hello-appen på hello andra enhet körs en gång. När taggar som påverkas av flera enheter, är hantera taggar från hello backend generellt önskvärt alternativet.
hello registrering hantering från hello-klientappen andra Nackdelen är att, eftersom kan vara över appar, skydda hello registrering toospecific taggar kräver extra noggrann enligt beskrivningen i hello ”taggen säkerhetsnivå”.

#### <a name="example-code-tooregister-with-a-notification-hub-from-a-device-using-an-installation"></a>Exempel kod tooregister med en meddelandehubb från en enhet med en installation
Just nu är detta stöds endast med hjälp av hello [Notification Hub REST API](https://msdn.microsoft.com/library/mt621153.aspx).

Du kan också använda hello korrigering metod med hello [JSON-korrigering standard](https://tools.ietf.org/html/rfc6902) för att uppdatera hello-installationen.

    class DeviceInstallation
    {
        public string installationId { get; set; }
        public string platform { get; set; }
        public string pushChannel { get; set; }
        public string[] tags { get; set; }
    }

    private async Task<HttpStatusCode> CreateOrUpdateInstallationAsync(DeviceInstallation deviceInstallation,
         string hubName, string listenConnectionString)
    {
        if (deviceInstallation.installationId == null)
            return HttpStatusCode.BadRequest;

        // Parse connection string (https://msdn.microsoft.com/library/azure/dn495627.aspx)
        ConnectionStringUtility connectionSaSUtil = new ConnectionStringUtility(listenConnectionString);
        string hubResource = "installations/" + deviceInstallation.installationId + "?";
        string apiVersion = "api-version=2015-04";

        // Determine hello targetUri that we will sign
        string uri = connectionSaSUtil.Endpoint + hubName + "/" + hubResource + apiVersion;

        //=== Generate SaS Security Token for Authorization header ===
        // See, https://msdn.microsoft.com/library/azure/dn495627.aspx
        string SasToken = connectionSaSUtil.getSaSToken(uri, 60);

        using (var httpClient = new HttpClient())
        {
            string json = JsonConvert.SerializeObject(deviceInstallation);

            httpClient.DefaultRequestHeaders.Add("Authorization", SasToken);

            var response = await httpClient.PutAsync(uri, new StringContent(json, System.Text.Encoding.UTF8, "application/json"));
            return response.StatusCode;
        }
    }

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    string installationId = null;
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a installation id in application data, create and store as application data.
    if (!settings.ContainsKey("__NHInstallationId"))
    {
        installationId = Guid.NewGuid().ToString();
        settings.Add("__NHInstallationId", installationId);
    }

    installationId = (string)settings["__NHInstallationId"];

    var deviceInstallation = new DeviceInstallation
    {
        installationId = installationId,
        platform = "wns",
        pushChannel = channel.Uri,
        //tags = tags.ToArray<string>()
    };

    var statusCode = await CreateOrUpdateInstallationAsync(deviceInstallation, 
                        "<HUBNAME>", "<SHARED LISTEN CONNECTION STRING>");

    if (statusCode != HttpStatusCode.Accepted)
    {
        var dialog = new MessageDialog(statusCode.ToString(), "Registration failed. Installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
    else
    {
        var dialog = new MessageDialog("Registration successful using installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }



#### <a name="example-code-tooregister-with-a-notification-hub-from-a-device-using-a-registration"></a>Exempel kod tooregister med en meddelandehubb från en enhet med en registrering
Dessa metoder skapa eller uppdatera en registrering för hello enhet där de anropas. Det innebär att i tooupdate hello referensen eller hello taggar, du måste skriva över hela hello-registrering. Kom ihåg att registreringar är tillfälligt, så du bör alltid ha en tillförlitlig lagring med aktuella hello-taggar som behöver en specifik enhet.

    // Initialize hello Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // hello Device id from hello PNS
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // If you are registering from hello client itself, then store this registration id in device
    // storage. Then when hello app starts, you can check if a registration id already exists or not before
    // creating.
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a registration id in application data, store in application data.
    if (!settings.ContainsKey("__NHRegistrationId"))
    {
        // make sure there are no existing registrations for this push handle (used for iOS and Android)    
        string newRegistrationId = null;
        var registrations = await hub.GetRegistrationsByChannelAsync(pushChannel.Uri, 100);
        foreach (RegistrationDescription registration in registrations)
        {
            if (newRegistrationId == null)
            {
                newRegistrationId = registration.RegistrationId;
            }
            else
            {
                await hub.DeleteRegistrationAsync(registration);
            }
        }

        newRegistrationId = await hub.CreateRegistrationIdAsync();

        settings.Add("__NHRegistrationId", newRegistrationId);
    }

    string regId = (string)settings["__NHRegistrationId"];

    RegistrationDescription registration = new WindowsRegistrationDescription(pushChannel.Uri);
    registration.RegistrationId = regId;
    registration.Tags = new HashSet<string>(YourTags);

    try
    {
        await hub.CreateOrUpdateRegistrationAsync(registration);
    }
    catch (Microsoft.WindowsAzure.Messaging.RegistrationGoneException e)
    {
        settings.Remove("__NHRegistrationId");
    }


## <a name="registration-management-from-a-backend"></a>Registrering-hantering från en serverdel
Hantera registreringar från hello backend kräver skriva ytterligare kod. hello-app från hello enhet måste ange hello uppdaterade PNS referensen toohello backend varje gång hello appen startar (tillsammans med taggar och mallar) och hello backend måste uppdatera det här handtaget på hello meddelandehubben. hello följande bild illustrerar den här designen.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-backend.png)

hello fördelarna med att hantera registreringar från hello backend är hello möjlighet toomodify taggar tooregistrations även när hello motsvarande appen på hello enhet är inaktiv och tooauthenticate hello-klientappen innan du lägger till en tagg tooits registrering.

#### <a name="example-code-tooregister-with-a-notification-hub-from-a-backend-using-an-installation"></a>Exempel kod tooregister med en meddelandehubb från en serverdel som använder en installation
hello klientenheten fortfarande hämtar dess PNS-handtag och relevanta installationsegenskaper som tidigare och samtal som en anpassad API på hello-serverdel som kan utföra hello registrering och auktorisera taggar etc. hello backend kan utnyttja hello [Notification Hub SDK för backend operations](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Du kan också använda hello korrigering metod med hello [JSON-korrigering standard](https://tools.ietf.org/html/rfc6902) för att uppdatera hello-installationen.

    // Initialize hello Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // Custom API on hello backend
    public async Task<HttpResponseMessage> Put(DeviceInstallation deviceUpdate)
    {

        Installation installation = new Installation();
        installation.InstallationId = deviceUpdate.InstallationId;
        installation.PushChannel = deviceUpdate.Handle;
        installation.Tags = deviceUpdate.Tags;

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                installation.Platform = NotificationPlatform.Mpns;
                break;
            case "wns":
                installation.Platform = NotificationPlatform.Wns;
                break;
            case "apns":
                installation.Platform = NotificationPlatform.Apns;
                break;
            case "gcm":
                installation.Platform = NotificationPlatform.Gcm;
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }


        // In hello backend we can control if a user is allowed tooadd tags
        //installation.Tags = new List<string>(deviceUpdate.Tags);
        //installation.Tags.Add("username:" + username);

        await hub.CreateOrUpdateInstallationAsync(installation);

        return Request.CreateResponse(HttpStatusCode.OK);
    }


#### <a name="example-code-tooregister-with-a-notification-hub-from-a-device-using-a-registration-id"></a>Exempel kod tooregister med en meddelandehubb från en enhet med ett registrerings-id
Du kan utföra grundläggande åtgärder för CRUDS för registreringar från din Apps serverdel. Exempel:

    var hub = NotificationHubClient.CreateClientFromConnectionString("{connectionString}", "hubName");

    // create a registration description object of hello correct type, e.g.
    var reg = new WindowsRegistrationDescription(channelUri, tags);

    // Create
    await hub.CreateRegistrationAsync(reg);

    // Get by id
    var r = await hub.GetRegistrationAsync<RegistrationDescription>("id");

    // update
    r.Tags.Add("myTag");

    // update on hub
    await hub.UpdateRegistrationAsync(r);

    // delete
    await hub.DeleteRegistrationAsync(r);


hello backend måste hantera samtidighet mellan registrering uppdateringar. Service Bus erbjuder optimistisk samtidighetskontroll för hantering av registreringen. Detta har implementerats med hello användningen av ETag på registrering hanteringsåtgärder på hello HTTP-nivå. Den här funktionen används transparent av Microsoft SDKs som utlöser ett undantag om en uppdatering avvisas samtidighet skäl. Hej appserverdelen ansvarar för att hantera dessa undantag och försöker igen hello update om det behövs.

