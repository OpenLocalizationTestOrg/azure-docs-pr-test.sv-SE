---
title: "Översikt över Azure Event Hubs autentisering och säkerhetsmodell | Microsoft Docs"
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
ms.openlocfilehash: 5abdbf70d4fdb2c7feb0f3537ecc0f2abf0775a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="event-hubs-authentication-and-security-model-overview"></a><span data-ttu-id="8c72e-103">Autentisering och säkerhet modellen översikt över Event Hubs</span><span class="sxs-lookup"><span data-stu-id="8c72e-103">Event Hubs authentication and security model overview</span></span>
<span data-ttu-id="8c72e-104">Händelsehubbar i Azure-säkerhetsmodellen uppfyller följande krav:</span><span class="sxs-lookup"><span data-stu-id="8c72e-104">The Azure Event Hubs security model meets the following requirements:</span></span>

* <span data-ttu-id="8c72e-105">Endast klienter som presenterar giltiga autentiseringsuppgifter kan skicka data till en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="8c72e-105">Only clients that present valid credentials can send data to an event hub.</span></span>
* <span data-ttu-id="8c72e-106">En klient kan personifiera en annan klient.</span><span class="sxs-lookup"><span data-stu-id="8c72e-106">A client cannot impersonate another client.</span></span>
* <span data-ttu-id="8c72e-107">En falsk klient blockeras från att skicka data till en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="8c72e-107">A rogue client can be blocked from sending data to an event hub.</span></span>

## <a name="client-authentication"></a><span data-ttu-id="8c72e-108">Klientautentisering</span><span class="sxs-lookup"><span data-stu-id="8c72e-108">Client authentication</span></span>
<span data-ttu-id="8c72e-109">Händelsehubbar-säkerhetsmodellen bygger på en kombination av [delade signatur åtkomst (SAS)](../service-bus-messaging/service-bus-sas.md) token och *händelseutfärdare*.</span><span class="sxs-lookup"><span data-stu-id="8c72e-109">The Event Hubs security model is based on a combination of [Shared Access Signature (SAS)](../service-bus-messaging/service-bus-sas.md) tokens and *event publishers*.</span></span> <span data-ttu-id="8c72e-110">En händelseutfärdare definierar en virtuell slutpunkt för en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="8c72e-110">An event publisher defines a virtual endpoint for an event hub.</span></span> <span data-ttu-id="8c72e-111">Utgivaren kan bara användas för att skicka meddelanden till en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="8c72e-111">The publisher can only be used to send messages to an event hub.</span></span> <span data-ttu-id="8c72e-112">Det går inte att ta emot meddelanden från en utgivare.</span><span class="sxs-lookup"><span data-stu-id="8c72e-112">It is not possible to receive messages from a publisher.</span></span>

<span data-ttu-id="8c72e-113">En händelsehubb använder normalt en utgivare per klient.</span><span class="sxs-lookup"><span data-stu-id="8c72e-113">Typically, an event hub employs one publisher per client.</span></span> <span data-ttu-id="8c72e-114">Alla meddelanden som skickas till någon av utgivare i en händelsehubb är köas inom den händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="8c72e-114">All messages that are sent to any of the publishers of an event hub are enqueued within that event hub.</span></span> <span data-ttu-id="8c72e-115">Utgivare Aktivera detaljerad åtkomstkontroll och begränsning.</span><span class="sxs-lookup"><span data-stu-id="8c72e-115">Publishers enable fine-grained access control and throttling.</span></span>

<span data-ttu-id="8c72e-116">Varje händelsehubbklient tilldelas ett unikt token som överförs till klienten.</span><span class="sxs-lookup"><span data-stu-id="8c72e-116">Each Event Hubs client is assigned a unique token, which is uploaded to the client.</span></span> <span data-ttu-id="8c72e-117">Token som tillverkas så att varje unik token ger tillgång till en annan unik utgivare.</span><span class="sxs-lookup"><span data-stu-id="8c72e-117">The tokens are produced such that each unique token grants access to a different unique publisher.</span></span> <span data-ttu-id="8c72e-118">En klient som har en token kan bara skicka till en utgivare, men inga andra utgivare.</span><span class="sxs-lookup"><span data-stu-id="8c72e-118">A client that possesses a token can only send to one publisher, but no other publisher.</span></span> <span data-ttu-id="8c72e-119">Om flera klienter delar samma token, delar dem en utgivare.</span><span class="sxs-lookup"><span data-stu-id="8c72e-119">If multiple clients share the same token, then each of them shares a publisher.</span></span>

<span data-ttu-id="8c72e-120">Det är möjligt att förse enheter med token som ger direktåtkomst till en händelsehubb rekommenderas inte.</span><span class="sxs-lookup"><span data-stu-id="8c72e-120">Although not recommended, it is possible to equip devices with tokens that grant direct access to an event hub.</span></span> <span data-ttu-id="8c72e-121">Alla enheter som innehåller detta token kan skicka meddelanden direkt till den händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="8c72e-121">Any device that holds this token can send messages directly into that event hub.</span></span> <span data-ttu-id="8c72e-122">Dessa enheter inte omfattas begränsning.</span><span class="sxs-lookup"><span data-stu-id="8c72e-122">Such a device will not be subject to throttling.</span></span> <span data-ttu-id="8c72e-123">Dessutom kan du övriga enheten från att skicka till den event hub.</span><span class="sxs-lookup"><span data-stu-id="8c72e-123">Furthermore, the device cannot be blacklisted from sending to that event hub.</span></span>

<span data-ttu-id="8c72e-124">Alla token är signerade med en SAS-nyckel.</span><span class="sxs-lookup"><span data-stu-id="8c72e-124">All tokens are signed with a SAS key.</span></span> <span data-ttu-id="8c72e-125">Alla token är vanligtvis signerade med samma nyckel.</span><span class="sxs-lookup"><span data-stu-id="8c72e-125">Typically, all tokens are signed with the same key.</span></span> <span data-ttu-id="8c72e-126">Klienter är inte medvetna om nyckeln. Detta förhindrar att andra klienter tillverkar token.</span><span class="sxs-lookup"><span data-stu-id="8c72e-126">Clients are not aware of the key; this prevents other clients from manufacturing tokens.</span></span>

### <a name="create-the-sas-key"></a><span data-ttu-id="8c72e-127">Skapa SAS-nyckeln</span><span class="sxs-lookup"><span data-stu-id="8c72e-127">Create the SAS key</span></span>

<span data-ttu-id="8c72e-128">När du skapar ett namnområde för Händelsehubbar tjänsten genererar en 256-bitars SAS-nyckel som heter **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="8c72e-128">When creating an Event Hubs namespace, the service generates a 256-bit SAS key named **RootManageSharedAccessKey**.</span></span> <span data-ttu-id="8c72e-129">Den här nyckeln ger skicka lyssna och hantera rättigheter till i namnområdet.</span><span class="sxs-lookup"><span data-stu-id="8c72e-129">This key grants send, listen, and manage rights to the namespace.</span></span> <span data-ttu-id="8c72e-130">Du kan också skapa ytterligare nycklar.</span><span class="sxs-lookup"><span data-stu-id="8c72e-130">You can also create additional keys.</span></span> <span data-ttu-id="8c72e-131">Vi rekommenderar att du ger en nyckel som ger skickar behörigheter till specifika händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="8c72e-131">It is recommended that you produce a key that grants send permissions to the specific event hub.</span></span> <span data-ttu-id="8c72e-132">Under resten av det här avsnittet förutsätts att den här nyckeln med namnet **EventHubSendKey**.</span><span class="sxs-lookup"><span data-stu-id="8c72e-132">For the remainder of this topic, it is assumed that you named this key **EventHubSendKey**.</span></span>

<span data-ttu-id="8c72e-133">I följande exempel skapas en endast send-nyckel när du skapar händelsehubben:</span><span class="sxs-lookup"><span data-stu-id="8c72e-133">The following example creates a send-only key when creating the event hub:</span></span>

```csharp
// Create namespace manager.
string serviceNamespace = "YOUR_NAMESPACE";
string namespaceManageKeyName = "RootManageSharedAccessKey";
string namespaceManageKey = "YOUR_ROOT_MANAGE_SHARED_ACCESS_KEY";
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, string.Empty);
TokenProvider td = TokenProvider.CreateSharedAccessSignatureTokenProvider(namespaceManageKeyName, namespaceManageKey);
NamespaceManager nm = new NamespaceManager(namespaceUri, namespaceManageTokenProvider);

// Create event hub with a SAS rule that enables sending to that event hub
EventHubDescription ed = new EventHubDescription("MY_EVENT_HUB") { PartitionCount = 32 };
string eventHubSendKeyName = "EventHubSendKey";
string eventHubSendKey = SharedAccessAuthorizationRule.GenerateRandomKey();
SharedAccessAuthorizationRule eventHubSendRule = new SharedAccessAuthorizationRule(eventHubSendKeyName, eventHubSendKey, new[] { AccessRights.Send });
ed.Authorization.Add(eventHubSendRule); 
nm.CreateEventHub(ed);
```

### <a name="generate-tokens"></a><span data-ttu-id="8c72e-134">Generera token</span><span class="sxs-lookup"><span data-stu-id="8c72e-134">Generate tokens</span></span>

<span data-ttu-id="8c72e-135">Du kan generera token med SAS-nyckeln.</span><span class="sxs-lookup"><span data-stu-id="8c72e-135">You can generate tokens using the SAS key.</span></span> <span data-ttu-id="8c72e-136">Du måste skapa en enda token per klient.</span><span class="sxs-lookup"><span data-stu-id="8c72e-136">You must produce only one token per client.</span></span> <span data-ttu-id="8c72e-137">Token kan sedan produceras med följande metod.</span><span class="sxs-lookup"><span data-stu-id="8c72e-137">Tokens can then be produced using the following method.</span></span> <span data-ttu-id="8c72e-138">Alla token genereras med hjälp av den **EventHubSendKey** nyckel.</span><span class="sxs-lookup"><span data-stu-id="8c72e-138">All tokens are generated using the **EventHubSendKey** key.</span></span> <span data-ttu-id="8c72e-139">Varje token tilldelas ett unikt URI.</span><span class="sxs-lookup"><span data-stu-id="8c72e-139">Each token is assigned a unique URI.</span></span>

```csharp
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

<span data-ttu-id="8c72e-140">När du anropar den här metoden URI: N måste anges som `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`.</span><span class="sxs-lookup"><span data-stu-id="8c72e-140">When calling this method, the URI should be specified as `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`.</span></span> <span data-ttu-id="8c72e-141">För alla token URI är identiska, med undantag av `PUBLISHER_NAME`, som ska vara olika för varje token.</span><span class="sxs-lookup"><span data-stu-id="8c72e-141">For all tokens, the URI is identical, with the exception of `PUBLISHER_NAME`, which should be different for each token.</span></span> <span data-ttu-id="8c72e-142">Vi rekommenderar `PUBLISHER_NAME` representerar ID för klienten som tar emot säkerhetstoken.</span><span class="sxs-lookup"><span data-stu-id="8c72e-142">Ideally, `PUBLISHER_NAME` represents the ID of the client that receives that token.</span></span>

<span data-ttu-id="8c72e-143">Den här metoden skapar en token med följande struktur:</span><span class="sxs-lookup"><span data-stu-id="8c72e-143">This method generates a token with the following structure:</span></span>

```csharp
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

<span data-ttu-id="8c72e-144">Förfallotid för token har angetts i sekunder från den 1 januari 1970.</span><span class="sxs-lookup"><span data-stu-id="8c72e-144">The token expiration time is specified in seconds from Jan 1, 1970.</span></span> <span data-ttu-id="8c72e-145">Följande är ett exempel på en token:</span><span class="sxs-lookup"><span data-stu-id="8c72e-145">The following is an example of a token:</span></span>

```csharp
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

<span data-ttu-id="8c72e-146">Token har vanligtvis en livslängd som liknar eller överskrider livslängden på klienten.</span><span class="sxs-lookup"><span data-stu-id="8c72e-146">Typically, the tokens have a lifespan that resembles or exceeds the lifespan of the client.</span></span> <span data-ttu-id="8c72e-147">Om klienten har möjlighet att få en ny token, kan token med en kortare livslängd användas.</span><span class="sxs-lookup"><span data-stu-id="8c72e-147">If the client has the capability to obtain a new token, tokens with a shorter lifespan can be used.</span></span>

### <a name="sending-data"></a><span data-ttu-id="8c72e-148">Skicka data</span><span class="sxs-lookup"><span data-stu-id="8c72e-148">Sending data</span></span>
<span data-ttu-id="8c72e-149">När token har skapats kan etableras varje klient med sin egen unika token.</span><span class="sxs-lookup"><span data-stu-id="8c72e-149">Once the tokens have been created, each client is provisioned with its own unique token.</span></span>

<span data-ttu-id="8c72e-150">När klienten skickar data till en händelsehubb, taggar skicka begäran till token.</span><span class="sxs-lookup"><span data-stu-id="8c72e-150">When the client sends data into an event hub, it tags its send request with the token.</span></span> <span data-ttu-id="8c72e-151">Kommunikationen mellan klienten och händelsehubben måste ske över en krypterad kanal för att hindra en angripare från avlyssning och stjäla token.</span><span class="sxs-lookup"><span data-stu-id="8c72e-151">To prevent an attacker from eavesdropping and stealing the token, the communication between the client and the event hub must occur over an encrypted channel.</span></span>

### <a name="blacklisting-clients"></a><span data-ttu-id="8c72e-152">Godkänna klienter</span><span class="sxs-lookup"><span data-stu-id="8c72e-152">Blacklisting clients</span></span>
<span data-ttu-id="8c72e-153">Om en token blir stulen av en angripare kan angripare imitera klienten vars token stulna.</span><span class="sxs-lookup"><span data-stu-id="8c72e-153">If a token is stolen by an attacker, the attacker can impersonate the client whose token has been stolen.</span></span> <span data-ttu-id="8c72e-154">Godkänna en klient återger den klienten inte kan användas tills det mottar en ny token som använder en annan utgivare.</span><span class="sxs-lookup"><span data-stu-id="8c72e-154">Blacklisting a client renders that client unusable until it receives a new token that uses a different publisher.</span></span>

## <a name="authentication-of-back-end-applications"></a><span data-ttu-id="8c72e-155">Autentisering av backend-program</span><span class="sxs-lookup"><span data-stu-id="8c72e-155">Authentication of back-end applications</span></span>

<span data-ttu-id="8c72e-156">Händelsehubbar använder en säkerhetsmodell som liknar den modell som används för Service Bus-ämnen för att autentisera backend-program som använder data som genereras av Händelsehubbar klienter.</span><span class="sxs-lookup"><span data-stu-id="8c72e-156">To authenticate back-end applications that consume the data generated by Event Hubs clients, Event Hubs employs a security model that is similar to the model that is used for Service Bus topics.</span></span> <span data-ttu-id="8c72e-157">En konsumentgrupp Händelsehubbar motsvarar en prenumeration på en Service Bus-ämne.</span><span class="sxs-lookup"><span data-stu-id="8c72e-157">An Event Hubs consumer group is equivalent to a subscription to a Service Bus topic.</span></span> <span data-ttu-id="8c72e-158">En klient kan skapa en konsumentgrupp om begäran att skapa konsumentgruppen åtföljs av en token som beviljar hanterar behörighet för händelsehubben eller för det namnområde där händelsehubben tillhör.</span><span class="sxs-lookup"><span data-stu-id="8c72e-158">A client can create a consumer group if the request to create the consumer group is accompanied by a token that grants manage privileges for the event hub, or for the namespace to which the event hub belongs.</span></span> <span data-ttu-id="8c72e-159">En klient är tillåtet att använda data från en konsumentgrupp receive-begäran åtföljs av en token som ger receive rättigheter på just den konsumentgruppen, händelsehubben eller det namnområde där händelsehubben tillhör.</span><span class="sxs-lookup"><span data-stu-id="8c72e-159">A client is allowed to consume data from a consumer group if the receive request is accompanied by a token that grants receive rights on that consumer group, the event hub, or the namespace to which the event hub belongs.</span></span>

<span data-ttu-id="8c72e-160">Den aktuella versionen av Service Bus stöder inte SAS regler för enskilda prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="8c72e-160">The current version of Service Bus does not support SAS rules for individual subscriptions.</span></span> <span data-ttu-id="8c72e-161">Detsamma gäller för Händelsehubbar konsumentgrupper.</span><span class="sxs-lookup"><span data-stu-id="8c72e-161">The same holds true for Event Hubs consumer groups.</span></span> <span data-ttu-id="8c72e-162">Stöd för SAS läggs för båda funktionerna i framtiden.</span><span class="sxs-lookup"><span data-stu-id="8c72e-162">SAS support will be added for both features in the future.</span></span>

<span data-ttu-id="8c72e-163">SAS-autentisering för enskilda konsumentgrupper saknas, kan du använda SAS-nycklar för att skydda alla konsumentgrupper med en gemensam nyckel.</span><span class="sxs-lookup"><span data-stu-id="8c72e-163">In the absence of SAS authentication for individual consumer groups, you can use SAS keys to secure all consumer groups with a common key.</span></span> <span data-ttu-id="8c72e-164">Den här metoden kan program använda data från någon av konsumentgrupper för en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="8c72e-164">This approach enables an application to consume data from any of the consumer groups of an event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8c72e-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8c72e-165">Next steps</span></span>
<span data-ttu-id="8c72e-166">Mer information om Händelsehubbar finns i följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="8c72e-166">To learn more about Event Hubs, visit the following topics:</span></span>

* <span data-ttu-id="8c72e-167">[Event Hubs-översikt]</span><span class="sxs-lookup"><span data-stu-id="8c72e-167">[Event Hubs overview]</span></span>
* <span data-ttu-id="8c72e-168">[Översikt över signaturer för delad åtkomst]</span><span class="sxs-lookup"><span data-stu-id="8c72e-168">[Overview of Shared Access Signatures]</span></span>
* <span data-ttu-id="8c72e-169">[Exempelprogram som använder Event Hubs]</span><span class="sxs-lookup"><span data-stu-id="8c72e-169">[Sample applications that use Event Hubs]</span></span>

<span data-ttu-id="8c72e-170">[Event Hubs-översikt]: event-hubs-what-is-event-hubs.md</span><span class="sxs-lookup"><span data-stu-id="8c72e-170">[Event Hubs overview]: event-hubs-what-is-event-hubs.md</span></span>
<span data-ttu-id="8c72e-171">[Exempelprogram som använder Event Hubs]: https://github.com/Azure/azure-event-hubs/tree/master/samples</span><span class="sxs-lookup"><span data-stu-id="8c72e-171">[Sample applications that use Event Hubs]: https://github.com/Azure/azure-event-hubs/tree/master/samples</span></span>
<span data-ttu-id="8c72e-172">[Översikt över signaturer för delad åtkomst]: ../service-bus-messaging/service-bus-sas.md</span><span class="sxs-lookup"><span data-stu-id="8c72e-172">[Overview of Shared Access Signatures]: ../service-bus-messaging/service-bus-sas.md</span></span>

