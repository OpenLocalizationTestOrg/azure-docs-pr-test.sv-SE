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
# <a name="event-hubs-authentication-and-security-model-overview"></a>Autentisering och säkerhet modellen översikt över Event Hubs
säkerhetsmodell för hello Azure Event Hubs uppfyller hello följande krav:

* Endast klienter som presenterar giltiga autentiseringsuppgifter kan skicka data tooan händelsehubb.
* En klient kan personifiera en annan klient.
* En falsk klient blockeras från att skicka data tooan händelsehubb.

## <a name="client-authentication"></a>Klientautentisering
Hej Händelsehubbar säkerhetsmodellen bygger på en kombination av [delade signatur åtkomst (SAS)](../service-bus-messaging/service-bus-sas.md) token och *händelseutfärdare*. En händelseutfärdare definierar en virtuell slutpunkt för en händelsehubb. hello publisher går bara att använda toosend meddelanden tooan händelsehubb. Det är inte möjligt tooreceive meddelanden från en utgivare.

En händelsehubb använder normalt en utgivare per klient. Alla meddelanden som skickas tooany över hello utgivare i en händelsehubb är köas inom den händelsehubben. Utgivare Aktivera detaljerad åtkomstkontroll och begränsning.

Varje händelsehubbklient tilldelas ett unikt token som är överförda toohello klient. hello token produceras så att varje unik token beviljar åtkomst tooa olika unika utgivare. En klient som har en token kan bara skicka tooone publisher, men inga andra utgivare. Om flera klienter resursen hello samma token och sedan var och en av dem delar en utgivare.

Rekommenderas inte är möjligt tooequip enheter med token som ger direktåtkomst tooan händelsehubb. Alla enheter som innehåller detta token kan skicka meddelanden direkt till den händelsehubben. Dessa enheter inte ämne toothrottling. Hello-enhet kan dessutom övriga från att skicka toothat händelsehubb.

Alla token är signerade med en SAS-nyckel. Normalt alla token är signerade med hello samma nyckel. Klienter är inte medvetna om hello nyckeln. Detta förhindrar att andra klienter tillverkar token.

### <a name="create-hello-sas-key"></a>Skapa hello SAS-nyckel

När du skapar ett namnområde för Händelsehubbar hello tjänsten genererar en 256-bitars SAS-nyckel som heter **RootManageSharedAccessKey**. Den här nyckeln ger skicka lyssna och hantera rights toohello namnområde. Du kan också skapa ytterligare nycklar. Vi rekommenderar att du ger en nyckel som ger skickar behörigheter toohello specifik händelsehubb. För hello resten av det här avsnittet, förutsätts att den här nyckeln med namnet **EventHubSendKey**.

hello följande exempel skapar en bara skicka nyckel när du skapar hello händelsehubb:

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

### <a name="generate-tokens"></a>Generera token

Du kan generera token med hello SAS-nyckel. Du måste skapa en enda token per klient. Token kan sedan produceras med hello följande metod. Alla token genereras med hjälp av hello **EventHubSendKey** nyckel. Varje token tilldelas ett unikt URI.

```csharp
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

När du anropar den här metoden hello URI måste anges som `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`. För alla token hello URI är identisk med hello undantag av `PUBLISHER_NAME`, som ska vara olika för varje token. Vi rekommenderar `PUBLISHER_NAME` representerar hello-ID för hello-klient som tar emot säkerhetstoken.

Den här metoden skapar en token med hello följande struktur:

```csharp
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

hello token förfallotid har angetts i sekunder från den 1 januari 1970. hello följande är ett exempel på en token:

```csharp
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

Normalt har hello token en livslängd som liknar eller överskrider hello livslängd av hello-klienten. Om hello-klienten har hello kapaciteten tooobtain en ny token, kan token med en kortare livslängd användas.

### <a name="sending-data"></a>Skicka data
När du har skapat hello token etableras varje klient med sin egen unika token.

När hello klienten skickar data till en händelsehubb, taggar skicka begäran med hello-token. tooprevent en angripare från avlyssning och stjäla hello token, hello kommunikation mellan hello-klienten och hello händelsehubb måste ske över en krypterad kanal.

### <a name="blacklisting-clients"></a>Godkänna klienter
Om en token blir stulen av en angripare kan hello angripare personifiera hello klienten vars token stulna. Godkänna en klient återger den klienten inte kan användas tills det mottar en ny token som använder en annan utgivare.

## <a name="authentication-of-back-end-applications"></a>Autentisering av backend-program

tooauthenticate backend-program som använder hello data som genereras av Händelsehubbar klienter, Event Hubs använder en säkerhetsmodell som är liknande toohello modell som används för Service Bus-ämnen. En konsumentgrupp för Händelsehubbar är likvärdiga tooa prenumeration tooa Service Bus-ämne. En klient kan skapa en konsumentgrupp hello begäran toocreate hello konsumentgrupp åtföljs av en token som beviljar hantera behörigheter för hello händelsehubb, eller för hello namnområde toowhich hello händelsehubb tillhör. En klient är tillåten tooconsume data från en konsumentgrupp om hello får begäran åtföljs av en token som beviljar får behörighet på just den konsumentgruppen, hello händelsehubb eller händelsehubb för hello namnområde toowhich hello tillhör.

hello nuvarande version av Service Bus stöder inte SAS regler för enskilda prenumerationer. Detsamma gäller för Händelsehubbar konsumentgrupper hello. Stöd för SAS läggs för båda funktionerna i hello framtida.

Hello saknas SAS-autentisering för enskilda konsumentgrupper, kan du använda SAS nycklar toosecure alla konsumentgrupper med en gemensam nyckel. På så sätt kan en tooconsume programdata från någon av hello konsumentgrupper för en händelsehubb.

## <a name="next-steps"></a>Nästa steg
toolearn mer information om Händelsehubbar, finns hello följande avsnitt:

* [Event Hubs-översikt]
* [Översikt över signaturer för delad åtkomst]
* [Exempelprogram som använder Event Hubs]

[Event Hubs-översikt]: event-hubs-what-is-event-hubs.md
[Exempelprogram som använder Event Hubs]: https://github.com/Azure/azure-event-hubs/tree/master/samples
[Översikt över signaturer för delad åtkomst]: ../service-bus-messaging/service-bus-sas.md

