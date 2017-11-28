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
# <a name="registration-management"></a><span data-ttu-id="f88a9-103">Registreringshantering</span><span class="sxs-lookup"><span data-stu-id="f88a9-103">Registration management</span></span>
## <a name="overview"></a><span data-ttu-id="f88a9-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="f88a9-104">Overview</span></span>
<span data-ttu-id="f88a9-105">Det här avsnittet beskrivs hur tooregister enheter med notification hubs finns i ordning tooreceive push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="f88a9-105">This topic explains how tooregister devices with notification hubs in order tooreceive push notifications.</span></span> <span data-ttu-id="f88a9-106">hello avsnittet beskriver registreringar på en hög nivå och sedan introducerar hello två huvudsakliga mönster för att registrera enheter: registrering från hello enhet direkt toohello meddelandehubb och registrera via en serverdel för programmet.</span><span class="sxs-lookup"><span data-stu-id="f88a9-106">hello topic describes registrations at a high level, then introduces hello two main patterns for registering devices: registering from hello device directly toohello notification hub, and registering through an application backend.</span></span> 

## <a name="what-is-device-registration"></a><span data-ttu-id="f88a9-107">Vad är registrering av enheten</span><span class="sxs-lookup"><span data-stu-id="f88a9-107">What is device registration</span></span>
<span data-ttu-id="f88a9-108">Registrering av enheten med en Meddelandehubb åstadkoms med hjälp av en **registrering** eller **Installation**.</span><span class="sxs-lookup"><span data-stu-id="f88a9-108">Device registration with a Notification Hub is accomplished using a **Registration** or **Installation**.</span></span>

#### <a name="registrations"></a><span data-ttu-id="f88a9-109">Registreringar</span><span class="sxs-lookup"><span data-stu-id="f88a9-109">Registrations</span></span>
<span data-ttu-id="f88a9-110">En registrering associerar hello Platform Notification Service (PNS) hantera för en enhet med taggar och eventuellt en mall.</span><span class="sxs-lookup"><span data-stu-id="f88a9-110">A registration associates hello Platform Notification Service (PNS) handle for a device with tags and possibly a template.</span></span> <span data-ttu-id="f88a9-111">Hej PNS-handtag kan vara en ChannelURI eller enhetstoken GCM registrerings-id. Taggar är används tooroute meddelanden toohello rätt uppsättning enhetshandtag.</span><span class="sxs-lookup"><span data-stu-id="f88a9-111">hello PNS handle could be a ChannelURI, device token, or GCM registration id. Tags are used tooroute notifications toohello correct set of device handles.</span></span> <span data-ttu-id="f88a9-112">Mer information finns i [Routning och Tagguttryck](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="f88a9-112">For more information, see [Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span> <span data-ttu-id="f88a9-113">Mallar är används tooimplement per registrering omvandling.</span><span class="sxs-lookup"><span data-stu-id="f88a9-113">Templates are used tooimplement per-registration transformation.</span></span> <span data-ttu-id="f88a9-114">Mer information finns i [mallar](notification-hubs-templates-cross-platform-push-messages.md).</span><span class="sxs-lookup"><span data-stu-id="f88a9-114">For more information, see [Templates](notification-hubs-templates-cross-platform-push-messages.md).</span></span>

#### <a name="installations"></a><span data-ttu-id="f88a9-115">Installationer</span><span class="sxs-lookup"><span data-stu-id="f88a9-115">Installations</span></span>
<span data-ttu-id="f88a9-116">En Installation är en förbättrad registrering som innehåller en egenskapsuppsättning för push-relaterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="f88a9-116">An Installation is an enhanced registration that includes a bag of push related properties.</span></span> <span data-ttu-id="f88a9-117">Det är hello senaste och bästa metoden tooregistering dina enheter.</span><span class="sxs-lookup"><span data-stu-id="f88a9-117">It is hello latest and best approach tooregistering your devices.</span></span> <span data-ttu-id="f88a9-118">Men det inte stöds av .NET SDK på klientsidan ([Notification Hub SDK för backend-åtgärder](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) ännu.</span><span class="sxs-lookup"><span data-stu-id="f88a9-118">However, it is not supported by client side .NET SDK ([Notification Hub SDK for backend operations](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) as of yet.</span></span>  <span data-ttu-id="f88a9-119">Det innebär att om du registrerar från hello själva klientenheten har du toouse hello [Notification Hub REST API](https://msdn.microsoft.com/library/mt621153.aspx) hanterar toosupport installationer.</span><span class="sxs-lookup"><span data-stu-id="f88a9-119">This means if you are registering from hello client device itself, you would have toouse hello [Notification Hubs REST API](https://msdn.microsoft.com/library/mt621153.aspx) approach toosupport installations.</span></span> <span data-ttu-id="f88a9-120">Om du använder en serverdelstjänst, bör du kunna toouse [Notification Hub SDK för backend-åtgärder](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="f88a9-120">If you are using a backend service, you should be able toouse [Notification Hub SDK for backend operations](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>

<span data-ttu-id="f88a9-121">hello nedan följer några viktiga fördelar toousing installationer:</span><span class="sxs-lookup"><span data-stu-id="f88a9-121">hello following are some key advantages toousing installations:</span></span>

* <span data-ttu-id="f88a9-122">Skapar eller uppdaterar en installation är helt idempotent.</span><span class="sxs-lookup"><span data-stu-id="f88a9-122">Creating or updating an installation is fully idempotent.</span></span> <span data-ttu-id="f88a9-123">Så du kan köra det igen utan frågor om duplicerade registreringar.</span><span class="sxs-lookup"><span data-stu-id="f88a9-123">So you can retry it without any concerns about duplicate registrations.</span></span>
* <span data-ttu-id="f88a9-124">Hej installationsmodell gör det enkelt toodo enskilda push - specifik enhet som mål.</span><span class="sxs-lookup"><span data-stu-id="f88a9-124">hello installation model makes it easy toodo individual pushes - targeting specific device.</span></span> <span data-ttu-id="f88a9-125">En system-tagg **”$InstallationId: [installationId]”** läggs till automatiskt med varje installation baserad registrering.</span><span class="sxs-lookup"><span data-stu-id="f88a9-125">A system tag **"$InstallationId:[installationId]"** is automatically added with each installation based registration.</span></span> <span data-ttu-id="f88a9-126">Så kan du anropa en skicka toothis taggen tootarget en specifik enhet utan att behöva toodo ytterligare kodning.</span><span class="sxs-lookup"><span data-stu-id="f88a9-126">So you can call a send toothis tag tootarget a specific device without having toodo any additional coding.</span></span>
* <span data-ttu-id="f88a9-127">Med installationer kan också du toodo partiella registrering uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="f88a9-127">Using installations also enables you toodo partial registration updates.</span></span> <span data-ttu-id="f88a9-128">hello deluppdatering av en installation krävs en korrigering metod med hello [JSON-korrigering standard](https://tools.ietf.org/html/rfc6902).</span><span class="sxs-lookup"><span data-stu-id="f88a9-128">hello partial update of an installation is requested with a PATCH method using hello [JSON-Patch standard](https://tools.ietf.org/html/rfc6902).</span></span> <span data-ttu-id="f88a9-129">Detta är särskilt användbart när du vill tooupdate taggar på hello registrering.</span><span class="sxs-lookup"><span data-stu-id="f88a9-129">This is particularly useful when you want tooupdate tags on hello registration.</span></span> <span data-ttu-id="f88a9-130">Du inte har toopull ned hello hela registrering och skicka om alla tidigare hello-taggar.</span><span class="sxs-lookup"><span data-stu-id="f88a9-130">You don't have toopull down hello entire registration and then resend all hello previous tags again.</span></span>

<span data-ttu-id="f88a9-131">En installation kan innehålla hello hello följande egenskaper.</span><span class="sxs-lookup"><span data-stu-id="f88a9-131">An installation can contain hello hello following properties.</span></span> <span data-ttu-id="f88a9-132">En fullständig lista över hello installationen egenskaper Se [skapa eller skriva över en Installation med REST API](https://msdn.microsoft.com/library/azure/mt621153.aspx) eller [installationsegenskaper](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx) för hello.</span><span class="sxs-lookup"><span data-stu-id="f88a9-132">For a complete listing of hello installation properties see, [Create or Overwrite an Installation with REST API](https://msdn.microsoft.com/library/azure/mt621153.aspx) or [Installation Properties](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx) for hello .</span></span>

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



<span data-ttu-id="f88a9-133">Det är viktigt toonote som registreringar och installationer som standard inte längre upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="f88a9-133">It is important toonote that registrations and installations by default no longer expire.</span></span>

<span data-ttu-id="f88a9-134">Registreringar och installationer måste innehålla en giltig PNS-referens för varje enhet/kanal.</span><span class="sxs-lookup"><span data-stu-id="f88a9-134">Registrations and installations must contain a valid PNS handle for each device/channel.</span></span> <span data-ttu-id="f88a9-135">Eftersom PNS-handtag kan endast hämtas i ett klientprogram på hello enhet, är en mönstret tooregister direkt på enheten med hello-klientappen.</span><span class="sxs-lookup"><span data-stu-id="f88a9-135">Because PNS handles can only be obtained in a client app on hello device, one pattern is tooregister directly on that device with hello client app.</span></span> <span data-ttu-id="f88a9-136">På hello relaterade andra hand, säkerhetsaspekter och affärslogik tootags kan kräva att du toomanage device registration hello appens serverdel.</span><span class="sxs-lookup"><span data-stu-id="f88a9-136">On hello other hand, security considerations and business logic related tootags might require you toomanage device registration in hello app back-end.</span></span> 

#### <a name="templates"></a><span data-ttu-id="f88a9-137">Mallar</span><span class="sxs-lookup"><span data-stu-id="f88a9-137">Templates</span></span>
<span data-ttu-id="f88a9-138">Om du vill toouse [mallar](notification-hubs-templates-cross-platform-push-messages.md), hello enhetsinstallation också hålla alla mallar som är associerade med enheten i en JSON-format (se exemplet ovan).</span><span class="sxs-lookup"><span data-stu-id="f88a9-138">If you want toouse [Templates](notification-hubs-templates-cross-platform-push-messages.md), hello device installation also hold all templates associated with that device in a JSON format (see sample above).</span></span> <span data-ttu-id="f88a9-139">hello mallnamn hjälpa riktade olika mallar för hello samma enhet.</span><span class="sxs-lookup"><span data-stu-id="f88a9-139">hello template names help target different templates for hello same device.</span></span>

<span data-ttu-id="f88a9-140">Observera att varje mallnamn mappas tooa mall text och ett valfritt antal taggar.</span><span class="sxs-lookup"><span data-stu-id="f88a9-140">Note that each template name maps tooa template body and an optional set of tags.</span></span> <span data-ttu-id="f88a9-141">Dessutom kan varje plattform har ytterligare egenskaper.</span><span class="sxs-lookup"><span data-stu-id="f88a9-141">Moreover, each platform can have additional template properties.</span></span> <span data-ttu-id="f88a9-142">För Windows Store (med WNS) och Windows Phone 8 (med MPNS), kan ytterligare en uppsättning huvuden ingå i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="f88a9-142">For Windows Store (using WNS) and Windows Phone 8 (using MPNS), an additional set of headers can be part of hello template.</span></span> <span data-ttu-id="f88a9-143">Du kan ange ett konstant eller -tooa malluttryck en utgången egenskapen tooeither APN hello gäller.</span><span class="sxs-lookup"><span data-stu-id="f88a9-143">In hello case of APNs, you can set an expiry property tooeither a constant or tooa template expression.</span></span> <span data-ttu-id="f88a9-144">En fullständig lista över hello installationen egenskaper Se [skapa eller skriva över en Installation med övriga](https://msdn.microsoft.com/library/azure/mt621153.aspx) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="f88a9-144">For a complete listing of hello installation properties see, [Create or Overwrite an Installation with REST](https://msdn.microsoft.com/library/azure/mt621153.aspx) topic.</span></span>

#### <a name="secondary-tiles-for-windows-store-apps"></a><span data-ttu-id="f88a9-145">Sekundär paneler för Windows Store-appar</span><span class="sxs-lookup"><span data-stu-id="f88a9-145">Secondary Tiles for Windows Store Apps</span></span>
<span data-ttu-id="f88a9-146">För Windows Store-klientprogram hello skicka meddelanden toosecondary paneler är detsamma som att skicka dem toohello primära.</span><span class="sxs-lookup"><span data-stu-id="f88a9-146">For Windows Store client applications, sending notifications toosecondary tiles is hello same as sending them toohello primary one.</span></span> <span data-ttu-id="f88a9-147">Detta stöds även för installationer.</span><span class="sxs-lookup"><span data-stu-id="f88a9-147">This is also supported in installations.</span></span> <span data-ttu-id="f88a9-148">Observera att sekundära brickor har en annan ChannelUri vilka hello SDK på din klientapp hanterar transparent.</span><span class="sxs-lookup"><span data-stu-id="f88a9-148">Note that secondary tiles have a different ChannelUri, which hello SDK on your client app handles transparently.</span></span>

<span data-ttu-id="f88a9-149">Hej SecondaryTiles ordlista använder hello samma TileId som används toocreate hello SecondaryTiles objektet i Windows Store-app.</span><span class="sxs-lookup"><span data-stu-id="f88a9-149">hello SecondaryTiles dictionary uses hello same TileId that is used toocreate hello SecondaryTiles object in your Windows Store app.</span></span>
<span data-ttu-id="f88a9-150">Som med hello kan primära ChannelUri, ChannelUris sekundära brickor ändra när som helst.</span><span class="sxs-lookup"><span data-stu-id="f88a9-150">As with hello primary ChannelUri, ChannelUris of secondary tiles can change at any moment.</span></span> <span data-ttu-id="f88a9-151">I ordning tookeep hello installationer i hello notification hub uppdaterats hello enheten måste uppdatera dem med hello aktuella ChannelUris hello sekundära brickor.</span><span class="sxs-lookup"><span data-stu-id="f88a9-151">In order tookeep hello installations in hello notification hub updated, hello device must refresh them with hello current ChannelUris of hello secondary tiles.</span></span>

## <a name="registration-management-from-hello-device"></a><span data-ttu-id="f88a9-152">Hantering av registrering från hello-enhet</span><span class="sxs-lookup"><span data-stu-id="f88a9-152">Registration management from hello device</span></span>
<span data-ttu-id="f88a9-153">När du hanterar registrering av enheten från klientprogram ansvarar endast hello serverdelen för att skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="f88a9-153">When managing device registration from client apps, hello backend is only responsible for sending notifications.</span></span> <span data-ttu-id="f88a9-154">Klientprogram hålla PNS-handtag in toodate och registrera taggar.</span><span class="sxs-lookup"><span data-stu-id="f88a9-154">Client apps keep PNS handles up toodate, and register tags.</span></span> <span data-ttu-id="f88a9-155">hello följande bild illustrerar detta mönster.</span><span class="sxs-lookup"><span data-stu-id="f88a9-155">hello following picture illustrates this pattern.</span></span>

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-device.png)

<span data-ttu-id="f88a9-156">hello enheten först hämtar hello PNS hantera från hello PNS sedan registrerar hello meddelandehubben direkt.</span><span class="sxs-lookup"><span data-stu-id="f88a9-156">hello device first retrieves hello PNS handle from hello PNS, then registers with hello notification hub directly.</span></span> <span data-ttu-id="f88a9-157">När hello registreringen är klar kan hello appserverdelen skicka ett meddelande som mål att registreringen.</span><span class="sxs-lookup"><span data-stu-id="f88a9-157">After hello registration is successful, hello app backend can send a notification targeting that registration.</span></span> <span data-ttu-id="f88a9-158">Mer information om hur toosend meddelanden finns [Routning och Tagguttryck](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="f88a9-158">For more information about how toosend notifications, see [Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>
<span data-ttu-id="f88a9-159">Observera att i det här fallet används bara lyssna rättigheter tooaccess din notification hubs från hello enhet.</span><span class="sxs-lookup"><span data-stu-id="f88a9-159">Note that in this case, you will use only Listen rights tooaccess your notification hubs from hello device.</span></span> <span data-ttu-id="f88a9-160">Mer information finns i [säkerhet](notification-hubs-push-notification-security.md).</span><span class="sxs-lookup"><span data-stu-id="f88a9-160">For more information, see [Security](notification-hubs-push-notification-security.md).</span></span>

<span data-ttu-id="f88a9-161">Hello enklaste metoden är att registrera från hello enhet, men den har vissa nackdelar.</span><span class="sxs-lookup"><span data-stu-id="f88a9-161">Registering from hello device is hello simplest method, but it has some drawbacks.</span></span>
<span data-ttu-id="f88a9-162">hello första Nackdelen är att ett klientprogram kan uppdatera taggarna endast när hello appen är aktiv.</span><span class="sxs-lookup"><span data-stu-id="f88a9-162">hello first drawback is that a client app can only update its tags when hello app is active.</span></span> <span data-ttu-id="f88a9-163">Till exempel om en användare har två enheter som registreras taggar relaterade toosport team när hello första enheten registreras för en ytterligare tagg (till exempel Seahawks), får hello andra enhet hello meddelanden om hello Seahawks tills hello-appen på hello andra enhet körs en gång.</span><span class="sxs-lookup"><span data-stu-id="f88a9-163">For example, if a user has two devices that register tags related toosport teams, when hello first device registers for an additional tag (for example, Seahawks), hello second device will not receive hello notifications about hello Seahawks until hello app on hello second device is executed a second time.</span></span> <span data-ttu-id="f88a9-164">När taggar som påverkas av flera enheter, är hantera taggar från hello backend generellt önskvärt alternativet.</span><span class="sxs-lookup"><span data-stu-id="f88a9-164">More generally, when tags are affected by multiple devices, managing tags from hello backend is a desirable option.</span></span>
<span data-ttu-id="f88a9-165">hello registrering hantering från hello-klientappen andra Nackdelen är att, eftersom kan vara över appar, skydda hello registrering toospecific taggar kräver extra noggrann enligt beskrivningen i hello ”taggen säkerhetsnivå”.</span><span class="sxs-lookup"><span data-stu-id="f88a9-165">hello second drawback of registration management from hello client app is that, since apps can be hacked, securing hello registration toospecific tags requires extra care, as explained in hello section “Tag-level security.”</span></span>

#### <a name="example-code-tooregister-with-a-notification-hub-from-a-device-using-an-installation"></a><span data-ttu-id="f88a9-166">Exempel kod tooregister med en meddelandehubb från en enhet med en installation</span><span class="sxs-lookup"><span data-stu-id="f88a9-166">Example code tooregister with a notification hub from a device using an installation</span></span>
<span data-ttu-id="f88a9-167">Just nu är detta stöds endast med hjälp av hello [Notification Hub REST API](https://msdn.microsoft.com/library/mt621153.aspx).</span><span class="sxs-lookup"><span data-stu-id="f88a9-167">At this time, this is only supported using hello [Notification Hubs REST API](https://msdn.microsoft.com/library/mt621153.aspx).</span></span>

<span data-ttu-id="f88a9-168">Du kan också använda hello korrigering metod med hello [JSON-korrigering standard](https://tools.ietf.org/html/rfc6902) för att uppdatera hello-installationen.</span><span class="sxs-lookup"><span data-stu-id="f88a9-168">You can also use hello PATCH method using hello [JSON-Patch standard](https://tools.ietf.org/html/rfc6902) for updating hello installation.</span></span>

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



#### <a name="example-code-tooregister-with-a-notification-hub-from-a-device-using-a-registration"></a><span data-ttu-id="f88a9-169">Exempel kod tooregister med en meddelandehubb från en enhet med en registrering</span><span class="sxs-lookup"><span data-stu-id="f88a9-169">Example code tooregister with a notification hub from a device using a registration</span></span>
<span data-ttu-id="f88a9-170">Dessa metoder skapa eller uppdatera en registrering för hello enhet där de anropas.</span><span class="sxs-lookup"><span data-stu-id="f88a9-170">These methods create or update a registration for hello device on which they are called.</span></span> <span data-ttu-id="f88a9-171">Det innebär att i tooupdate hello referensen eller hello taggar, du måste skriva över hela hello-registrering.</span><span class="sxs-lookup"><span data-stu-id="f88a9-171">This means that in order tooupdate hello handle or hello tags, you must overwrite hello entire registration.</span></span> <span data-ttu-id="f88a9-172">Kom ihåg att registreringar är tillfälligt, så du bör alltid ha en tillförlitlig lagring med aktuella hello-taggar som behöver en specifik enhet.</span><span class="sxs-lookup"><span data-stu-id="f88a9-172">Remember that registrations are transient, so you should always have a reliable store with hello current tags that a specific device needs.</span></span>

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


## <a name="registration-management-from-a-backend"></a><span data-ttu-id="f88a9-173">Registrering-hantering från en serverdel</span><span class="sxs-lookup"><span data-stu-id="f88a9-173">Registration management from a backend</span></span>
<span data-ttu-id="f88a9-174">Hantera registreringar från hello backend kräver skriva ytterligare kod.</span><span class="sxs-lookup"><span data-stu-id="f88a9-174">Managing registrations from hello backend requires writing additional code.</span></span> <span data-ttu-id="f88a9-175">hello-app från hello enhet måste ange hello uppdaterade PNS referensen toohello backend varje gång hello appen startar (tillsammans med taggar och mallar) och hello backend måste uppdatera det här handtaget på hello meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="f88a9-175">hello app from hello device must provide hello updated PNS handle toohello backend every time hello app starts (along with tags and templates), and hello backend must update this handle on hello notification hub.</span></span> <span data-ttu-id="f88a9-176">hello följande bild illustrerar den här designen.</span><span class="sxs-lookup"><span data-stu-id="f88a9-176">hello following picture illustrates this design.</span></span>

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-backend.png)

<span data-ttu-id="f88a9-177">hello fördelarna med att hantera registreringar från hello backend är hello möjlighet toomodify taggar tooregistrations även när hello motsvarande appen på hello enhet är inaktiv och tooauthenticate hello-klientappen innan du lägger till en tagg tooits registrering.</span><span class="sxs-lookup"><span data-stu-id="f88a9-177">hello advantages of managing registrations from hello backend include hello ability toomodify tags tooregistrations even when hello corresponding app on hello device is inactive, and tooauthenticate hello client app before adding a tag tooits registration.</span></span>

#### <a name="example-code-tooregister-with-a-notification-hub-from-a-backend-using-an-installation"></a><span data-ttu-id="f88a9-178">Exempel kod tooregister med en meddelandehubb från en serverdel som använder en installation</span><span class="sxs-lookup"><span data-stu-id="f88a9-178">Example code tooregister with a notification hub from a backend using an installation</span></span>
<span data-ttu-id="f88a9-179">hello klientenheten fortfarande hämtar dess PNS-handtag och relevanta installationsegenskaper som tidigare och samtal som en anpassad API på hello-serverdel som kan utföra hello registrering och auktorisera taggar etc. hello backend kan utnyttja hello [Notification Hub SDK för backend operations](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="f88a9-179">hello client device still gets its PNS handle and relevant installation properties as before and calls a custom API on hello backend that can perform hello registration and authorize tags etc. hello backend can leverage hello [Notification Hub SDK for backend operations](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>

<span data-ttu-id="f88a9-180">Du kan också använda hello korrigering metod med hello [JSON-korrigering standard](https://tools.ietf.org/html/rfc6902) för att uppdatera hello-installationen.</span><span class="sxs-lookup"><span data-stu-id="f88a9-180">You can also use hello PATCH method using hello [JSON-Patch standard](https://tools.ietf.org/html/rfc6902) for updating hello installation.</span></span>

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


#### <a name="example-code-tooregister-with-a-notification-hub-from-a-device-using-a-registration-id"></a><span data-ttu-id="f88a9-181">Exempel kod tooregister med en meddelandehubb från en enhet med ett registrerings-id</span><span class="sxs-lookup"><span data-stu-id="f88a9-181">Example code tooregister with a notification hub from a device using a registration id</span></span>
<span data-ttu-id="f88a9-182">Du kan utföra grundläggande åtgärder för CRUDS för registreringar från din Apps serverdel.</span><span class="sxs-lookup"><span data-stu-id="f88a9-182">From your app backend, you can perform basic CRUDS operations on registrations.</span></span> <span data-ttu-id="f88a9-183">Exempel:</span><span class="sxs-lookup"><span data-stu-id="f88a9-183">For example:</span></span>

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


<span data-ttu-id="f88a9-184">hello backend måste hantera samtidighet mellan registrering uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="f88a9-184">hello backend must handle concurrency between registration updates.</span></span> <span data-ttu-id="f88a9-185">Service Bus erbjuder optimistisk samtidighetskontroll för hantering av registreringen.</span><span class="sxs-lookup"><span data-stu-id="f88a9-185">Service Bus offers optimistic concurrency control for registration management.</span></span> <span data-ttu-id="f88a9-186">Detta har implementerats med hello användningen av ETag på registrering hanteringsåtgärder på hello HTTP-nivå.</span><span class="sxs-lookup"><span data-stu-id="f88a9-186">At hello HTTP level, this is implemented with hello use of ETag on registration management operations.</span></span> <span data-ttu-id="f88a9-187">Den här funktionen används transparent av Microsoft SDKs som utlöser ett undantag om en uppdatering avvisas samtidighet skäl.</span><span class="sxs-lookup"><span data-stu-id="f88a9-187">This feature is transparently used by Microsoft SDKs, which throw an exception if an update is rejected for concurrency reasons.</span></span> <span data-ttu-id="f88a9-188">Hej appserverdelen ansvarar för att hantera dessa undantag och försöker igen hello update om det behövs.</span><span class="sxs-lookup"><span data-stu-id="f88a9-188">hello app backend is responsible for handling these exceptions and retrying hello update if required.</span></span>

