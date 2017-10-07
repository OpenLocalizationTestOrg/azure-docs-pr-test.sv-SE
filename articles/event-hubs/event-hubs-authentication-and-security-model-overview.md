---
title: "aaaOverview av Händelsehubbar i Azure-autentisering och säkerhetsmodell | Microsoft Docs"
description: "Autentisering och säkerhet modellen översikt över Event Hubs."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 93841e30-0c5c-4719-9dc1-57a4814342e7
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/30/2017
ms.author: sethm;clemensv
ms.openlocfilehash: e57ccda33e5ee20e635487cf91d9e8af594d3bd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-authentication-and-security-model-overview"></a><span data-ttu-id="356bf-103">Autentisering och säkerhet modellen översikt över Event Hubs</span><span class="sxs-lookup"><span data-stu-id="356bf-103">Event Hubs authentication and security model overview</span></span>
<span data-ttu-id="356bf-104">säkerhetsmodell för hello Azure Event Hubs uppfyller hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="356bf-104">hello Azure Event Hubs security model meets hello following requirements:</span></span>

* <span data-ttu-id="356bf-105">Endast klienter som presenterar giltiga autentiseringsuppgifter kan skicka data tooan händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="356bf-105">Only clients that present valid credentials can send data tooan event hub.</span></span>
* <span data-ttu-id="356bf-106">En klient kan personifiera en annan klient.</span><span class="sxs-lookup"><span data-stu-id="356bf-106">A client cannot impersonate another client.</span></span>
* <span data-ttu-id="356bf-107">En falsk klient blockeras från att skicka data tooan händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="356bf-107">A rogue client can be blocked from sending data tooan event hub.</span></span>

## <a name="client-authentication"></a><span data-ttu-id="356bf-108">Klientautentisering</span><span class="sxs-lookup"><span data-stu-id="356bf-108">Client authentication</span></span>
<span data-ttu-id="356bf-109">Hej Händelsehubbar säkerhetsmodellen bygger på en kombination av [delade signatur åtkomst (SAS)](../service-bus-messaging/service-bus-sas.md) token och *händelseutfärdare*.</span><span class="sxs-lookup"><span data-stu-id="356bf-109">hello Event Hubs security model is based on a combination of [Shared Access Signature (SAS)](../service-bus-messaging/service-bus-sas.md) tokens and *event publishers*.</span></span> <span data-ttu-id="356bf-110">En händelseutfärdare definierar en virtuell slutpunkt för en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="356bf-110">An event publisher defines a virtual endpoint for an event hub.</span></span> <span data-ttu-id="356bf-111">hello publisher går bara att använda toosend meddelanden tooan händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="356bf-111">hello publisher can only be used toosend messages tooan event hub.</span></span> <span data-ttu-id="356bf-112">Det är inte möjligt tooreceive meddelanden från en utgivare.</span><span class="sxs-lookup"><span data-stu-id="356bf-112">It is not possible tooreceive messages from a publisher.</span></span>

<span data-ttu-id="356bf-113">En händelsehubb använder normalt en utgivare per klient.</span><span class="sxs-lookup"><span data-stu-id="356bf-113">Typically, an event hub employs one publisher per client.</span></span> <span data-ttu-id="356bf-114">Alla meddelanden som skickas tooany över hello utgivare i en händelsehubb är köas inom den händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="356bf-114">All messages that are sent tooany of hello publishers of an event hub are enqueued within that event hub.</span></span> <span data-ttu-id="356bf-115">Utgivare Aktivera detaljerad åtkomstkontroll och begränsning.</span><span class="sxs-lookup"><span data-stu-id="356bf-115">Publishers enable fine-grained access control and throttling.</span></span>

<span data-ttu-id="356bf-116">Varje händelsehubbklient tilldelas ett unikt token som är överförda toohello klient.</span><span class="sxs-lookup"><span data-stu-id="356bf-116">Each Event Hubs client is assigned a unique token, which is uploaded toohello client.</span></span> <span data-ttu-id="356bf-117">hello token produceras så att varje unik token beviljar åtkomst tooa olika unika utgivare.</span><span class="sxs-lookup"><span data-stu-id="356bf-117">hello tokens are produced such that each unique token grants access tooa different unique publisher.</span></span> <span data-ttu-id="356bf-118">En klient som har en token kan bara skicka tooone publisher, men inga andra utgivare.</span><span class="sxs-lookup"><span data-stu-id="356bf-118">A client that possesses a token can only send tooone publisher, but no other publisher.</span></span> <span data-ttu-id="356bf-119">Om flera klienter resursen hello samma token och sedan var och en av dem delar en utgivare.</span><span class="sxs-lookup"><span data-stu-id="356bf-119">If multiple clients share hello same token, then each of them shares a publisher.</span></span>

<span data-ttu-id="356bf-120">Rekommenderas inte är möjligt tooequip enheter med token som ger direktåtkomst tooan händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="356bf-120">Although not recommended, it is possible tooequip devices with tokens that grant direct access tooan event hub.</span></span> <span data-ttu-id="356bf-121">Alla enheter som innehåller detta token kan skicka meddelanden direkt till den händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="356bf-121">Any device that holds this token can send messages directly into that event hub.</span></span> <span data-ttu-id="356bf-122">Dessa enheter inte ämne toothrottling.</span><span class="sxs-lookup"><span data-stu-id="356bf-122">Such a device will not be subject toothrottling.</span></span> <span data-ttu-id="356bf-123">Hello-enhet kan dessutom övriga från att skicka toothat händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="356bf-123">Furthermore, hello device cannot be blacklisted from sending toothat event hub.</span></span>

<span data-ttu-id="356bf-124">Alla token är signerade med en SAS-nyckel.</span><span class="sxs-lookup"><span data-stu-id="356bf-124">All tokens are signed with a SAS key.</span></span> <span data-ttu-id="356bf-125">Normalt alla token är signerade med hello samma nyckel.</span><span class="sxs-lookup"><span data-stu-id="356bf-125">Typically, all tokens are signed with hello same key.</span></span> <span data-ttu-id="356bf-126">Klienter är inte medvetna om hello nyckeln. Detta förhindrar att andra klienter tillverkar token.</span><span class="sxs-lookup"><span data-stu-id="356bf-126">Clients are not aware of hello key; this prevents other clients from manufacturing tokens.</span></span>

### <a name="create-hello-sas-key"></a><span data-ttu-id="356bf-127">Skapa hello SAS-nyckel</span><span class="sxs-lookup"><span data-stu-id="356bf-127">Create hello SAS key</span></span>

<span data-ttu-id="356bf-128">När du skapar ett namnområde för Händelsehubbar hello tjänsten genererar en 256-bitars SAS-nyckel som heter **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="356bf-128">When creating an Event Hubs namespace, hello service generates a 256-bit SAS key named **RootManageSharedAccessKey**.</span></span> <span data-ttu-id="356bf-129">Den här nyckeln ger skicka lyssna och hantera rights toohello namnområde.</span><span class="sxs-lookup"><span data-stu-id="356bf-129">This key grants send, listen, and manage rights toohello namespace.</span></span> <span data-ttu-id="356bf-130">Du kan också skapa ytterligare nycklar.</span><span class="sxs-lookup"><span data-stu-id="356bf-130">You can also create additional keys.</span></span> <span data-ttu-id="356bf-131">Vi rekommenderar att du ger en nyckel som ger skickar behörigheter toohello specifik händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="356bf-131">It is recommended that you produce a key that grants send permissions toohello specific event hub.</span></span> <span data-ttu-id="356bf-132">För hello resten av det här avsnittet, förutsätts att den här nyckeln med namnet **EventHubSendKey**.</span><span class="sxs-lookup"><span data-stu-id="356bf-132">For hello remainder of this topic, it is assumed that you named this key **EventHubSendKey**.</span></span>

<span data-ttu-id="356bf-133">hello följande exempel skapar en bara skicka nyckel när du skapar hello händelsehubb:</span><span class="sxs-lookup"><span data-stu-id="356bf-133">hello following example creates a send-only key when creating hello event hub:</span></span>

```csharp
// Create namespace manager.
string serviceNamespace = "YOUR_NAMESPACE";
string namespaceManageKeyName = "RootManageSharedAccessKey";
string namespaceManageKey = "YOUR_ROOT_MANAGE_SHARED_ACCESS_KEY";
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, string.Empty);
TokenProvider td = TokenProvider.CreateSharedAccessSignatureTokenProvider(namespaceManageKeyName, namespaceManageKey);
NamespaceManager nm = new NamespaceManager(namespaceUri, namespaceManageTokenProvider);

// Create event hub with a SAS rule that enables sending toothat event hub
EventHubDescription ed = new EventHubDescription("MY_EVENT_HUB") { PartitionCount = 32 };
string eventHubSendKeyName = "EventHubSendKey";
string eventHubSendKey = SharedAccessAuthorizationRule.GenerateRandomKey();
SharedAccessAuthorizationRule eventHubSendRule = new SharedAccessAuthorizationRule(eventHubSendKeyName, eventHubSendKey, new[] { AccessRights.Send });
ed.Authorization.Add(eventHubSendRule); 
nm.CreateEventHub(ed);
```

### <a name="generate-tokens"></a><span data-ttu-id="356bf-134">Generera token</span><span class="sxs-lookup"><span data-stu-id="356bf-134">Generate tokens</span></span>

<span data-ttu-id="356bf-135">Du kan generera token med hello SAS-nyckel.</span><span class="sxs-lookup"><span data-stu-id="356bf-135">You can generate tokens using hello SAS key.</span></span> <span data-ttu-id="356bf-136">Du måste skapa en enda token per klient.</span><span class="sxs-lookup"><span data-stu-id="356bf-136">You must produce only one token per client.</span></span> <span data-ttu-id="356bf-137">Token kan sedan produceras med hello följande metod.</span><span class="sxs-lookup"><span data-stu-id="356bf-137">Tokens can then be produced using hello following method.</span></span> <span data-ttu-id="356bf-138">Alla token genereras med hjälp av hello **EventHubSendKey** nyckel.</span><span class="sxs-lookup"><span data-stu-id="356bf-138">All tokens are generated using hello **EventHubSendKey** key.</span></span> <span data-ttu-id="356bf-139">Varje token tilldelas ett unikt URI.</span><span class="sxs-lookup"><span data-stu-id="356bf-139">Each token is assigned a unique URI.</span></span>

```csharp
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

<span data-ttu-id="356bf-140">När du anropar den här metoden hello URI måste anges som `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="356bf-140">When calling this method, hello URI should be specified as `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`.</span></span> <span data-ttu-id="356bf-141">För alla token hello URI är identisk med hello undantag av `PUBLISHER_NAME`, som ska vara olika för varje token.</span><span class="sxs-lookup"><span data-stu-id="356bf-141">For all tokens, hello URI is identical, with hello exception of `PUBLISHER_NAME`, which should be different for each token.</span></span> <span data-ttu-id="356bf-142">Vi rekommenderar `PUBLISHER_NAME` representerar hello-ID för hello-klient som tar emot säkerhetstoken.</span><span class="sxs-lookup"><span data-stu-id="356bf-142">Ideally, `PUBLISHER_NAME` represents hello ID of hello client that receives that token.</span></span>

<span data-ttu-id="356bf-143">Den här metoden skapar en token med hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="356bf-143">This method generates a token with hello following structure:</span></span>

```csharp
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

<span data-ttu-id="356bf-144">hello token förfallotid har angetts i sekunder från den 1 januari 1970.</span><span class="sxs-lookup"><span data-stu-id="356bf-144">hello token expiration time is specified in seconds from Jan 1, 1970.</span></span> <span data-ttu-id="356bf-145">hello följande är ett exempel på en token:</span><span class="sxs-lookup"><span data-stu-id="356bf-145">hello following is an example of a token:</span></span>

```csharp
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

<span data-ttu-id="356bf-146">Normalt har hello token en livslängd som liknar eller överskrider hello livslängd av hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="356bf-146">Typically, hello tokens have a lifespan that resembles or exceeds hello lifespan of hello client.</span></span> <span data-ttu-id="356bf-147">Om hello-klienten har hello kapaciteten tooobtain en ny token, kan token med en kortare livslängd användas.</span><span class="sxs-lookup"><span data-stu-id="356bf-147">If hello client has hello capability tooobtain a new token, tokens with a shorter lifespan can be used.</span></span>

### <a name="sending-data"></a><span data-ttu-id="356bf-148">Skicka data</span><span class="sxs-lookup"><span data-stu-id="356bf-148">Sending data</span></span>
<span data-ttu-id="356bf-149">När du har skapat hello token etableras varje klient med sin egen unika token.</span><span class="sxs-lookup"><span data-stu-id="356bf-149">Once hello tokens have been created, each client is provisioned with its own unique token.</span></span>

<span data-ttu-id="356bf-150">När hello klienten skickar data till en händelsehubb, taggar skicka begäran med hello-token.</span><span class="sxs-lookup"><span data-stu-id="356bf-150">When hello client sends data into an event hub, it tags its send request with hello token.</span></span> <span data-ttu-id="356bf-151">tooprevent en angripare från avlyssning och stjäla hello token, hello kommunikation mellan hello-klienten och hello händelsehubb måste ske över en krypterad kanal.</span><span class="sxs-lookup"><span data-stu-id="356bf-151">tooprevent an attacker from eavesdropping and stealing hello token, hello communication between hello client and hello event hub must occur over an encrypted channel.</span></span>

### <a name="blacklisting-clients"></a><span data-ttu-id="356bf-152">Godkänna klienter</span><span class="sxs-lookup"><span data-stu-id="356bf-152">Blacklisting clients</span></span>
<span data-ttu-id="356bf-153">Om en token blir stulen av en angripare kan hello angripare personifiera hello klienten vars token stulna.</span><span class="sxs-lookup"><span data-stu-id="356bf-153">If a token is stolen by an attacker, hello attacker can impersonate hello client whose token has been stolen.</span></span> <span data-ttu-id="356bf-154">Godkänna en klient återger den klienten inte kan användas tills det mottar en ny token som använder en annan utgivare.</span><span class="sxs-lookup"><span data-stu-id="356bf-154">Blacklisting a client renders that client unusable until it receives a new token that uses a different publisher.</span></span>

## <a name="authentication-of-back-end-applications"></a><span data-ttu-id="356bf-155">Autentisering av backend-program</span><span class="sxs-lookup"><span data-stu-id="356bf-155">Authentication of back-end applications</span></span>

<span data-ttu-id="356bf-156">tooauthenticate backend-program som använder hello data som genereras av Händelsehubbar klienter, Event Hubs använder en säkerhetsmodell som är liknande toohello modell som används för Service Bus-ämnen.</span><span class="sxs-lookup"><span data-stu-id="356bf-156">tooauthenticate back-end applications that consume hello data generated by Event Hubs clients, Event Hubs employs a security model that is similar toohello model that is used for Service Bus topics.</span></span> <span data-ttu-id="356bf-157">En konsumentgrupp för Händelsehubbar är likvärdiga tooa prenumeration tooa Service Bus-ämne.</span><span class="sxs-lookup"><span data-stu-id="356bf-157">An Event Hubs consumer group is equivalent tooa subscription tooa Service Bus topic.</span></span> <span data-ttu-id="356bf-158">En klient kan skapa en konsumentgrupp hello begäran toocreate hello konsumentgrupp åtföljs av en token som beviljar hantera behörigheter för hello händelsehubb, eller för hello namnområde toowhich hello händelsehubb tillhör.</span><span class="sxs-lookup"><span data-stu-id="356bf-158">A client can create a consumer group if hello request toocreate hello consumer group is accompanied by a token that grants manage privileges for hello event hub, or for hello namespace toowhich hello event hub belongs.</span></span> <span data-ttu-id="356bf-159">En klient är tillåten tooconsume data från en konsumentgrupp om hello får begäran åtföljs av en token som beviljar får behörighet på just den konsumentgruppen, hello händelsehubb eller händelsehubb för hello namnområde toowhich hello tillhör.</span><span class="sxs-lookup"><span data-stu-id="356bf-159">A client is allowed tooconsume data from a consumer group if hello receive request is accompanied by a token that grants receive rights on that consumer group, hello event hub, or hello namespace toowhich hello event hub belongs.</span></span>

<span data-ttu-id="356bf-160">hello nuvarande version av Service Bus stöder inte SAS regler för enskilda prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="356bf-160">hello current version of Service Bus does not support SAS rules for individual subscriptions.</span></span> <span data-ttu-id="356bf-161">Detsamma gäller för Händelsehubbar konsumentgrupper hello.</span><span class="sxs-lookup"><span data-stu-id="356bf-161">hello same holds true for Event Hubs consumer groups.</span></span> <span data-ttu-id="356bf-162">Stöd för SAS läggs för båda funktionerna i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="356bf-162">SAS support will be added for both features in hello future.</span></span>

<span data-ttu-id="356bf-163">Hello saknas SAS-autentisering för enskilda konsumentgrupper, kan du använda SAS nycklar toosecure alla konsumentgrupper med en gemensam nyckel.</span><span class="sxs-lookup"><span data-stu-id="356bf-163">In hello absence of SAS authentication for individual consumer groups, you can use SAS keys toosecure all consumer groups with a common key.</span></span> <span data-ttu-id="356bf-164">På så sätt kan en tooconsume programdata från någon av hello konsumentgrupper för en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="356bf-164">This approach enables an application tooconsume data from any of hello consumer groups of an event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="356bf-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="356bf-165">Next steps</span></span>
<span data-ttu-id="356bf-166">toolearn mer information om Händelsehubbar, finns hello följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="356bf-166">toolearn more about Event Hubs, visit hello following topics:</span></span>

* <span data-ttu-id="356bf-167">[Event Hubs-översikt]</span><span class="sxs-lookup"><span data-stu-id="356bf-167">[Event Hubs overview]</span></span>
* <span data-ttu-id="356bf-168">[Översikt över signaturer för delad åtkomst]</span><span class="sxs-lookup"><span data-stu-id="356bf-168">[Overview of Shared Access Signatures]</span></span>
* <span data-ttu-id="356bf-169">[Exempelprogram som använder Event Hubs]</span><span class="sxs-lookup"><span data-stu-id="356bf-169">[Sample applications that use Event Hubs]</span></span>

[Event Hubs-översikt]: event-hubs-what-is-event-hubs.md
[Exempelprogram som använder Event Hubs]: https://github.com/Azure/azure-event-hubs/tree/master/samples
[Översikt över signaturer för delad åtkomst]: ../service-bus-messaging/service-bus-sas.md

