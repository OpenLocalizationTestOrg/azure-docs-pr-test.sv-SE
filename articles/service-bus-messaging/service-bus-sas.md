---
title: "aaaAzure Service Bus-autentisering med signaturer för delad åtkomst | Microsoft Docs"
description: "Översikt över Service Bus-autentisering med hjälp av signaturer för delad åtkomst översikt, information om SAS-autentisering med Azure Service Bus."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: 773bb11720384d7245820b56dc25b8e064ffa746
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-authentication-with-shared-access-signatures"></a>Service Bus-autentisering med signaturer för delad åtkomst

*Signaturer för delad åtkomst* (SAS) är hello primära säkerhetsmekanism för Service Bus-meddelanden. Den här artikeln beskrivs SAS, hur de fungerar och hur toouse dem i ett plattformsoberoende sätt.

SAS-autentisering gör det möjligt för program tooauthenticate tooService Bus med hjälp av en åtkomstnyckel som konfigurerats på hello namnområde eller hello messaging entitet (kö eller ett ämne) med vilka specifika rättigheter som är associerade. Du kan sedan använda den här nyckeln toogenerate en SAS-token som klienter i sin tur kan använda tooauthenticate tooService Bus.

Stöd för SAS-autentisering ingår i hello Azure SDK version 2.0 och senare.

## <a name="overview-of-sas"></a>Översikt över SAS

Signaturer för delad åtkomst är en autentiseringsmekanism baserat på SHA-256 säker hashvärden eller URI: er. SAS är en mycket kraftfull mekanism som används av alla Service Bus-tjänster. Faktiska används SAS består av två komponenter: en *delad åtkomstprincip* och en *signatur för delad åtkomst* (kallas ofta för en *token*).

SAS-autentisering i Service Bus omfattar hello konfigurationen av en krypteringsnyckel med associerade behörigheter på en Service Bus-resurs. Klienter anspråk åtkomst tooService Bus resurser genom att presentera en SAS-token. Denna token består av hello resurs-URI som används och ett förfallodatum som signerats med hello konfigurerade nyckeln.

Du kan konfigurera auktoriseringsregler för signatur för delad åtkomst på Service Bus [vidarebefordrar](service-bus-fundamentals-hybrid-solutions.md#relays), [köer](service-bus-fundamentals-hybrid-solutions.md#queues), och [avsnitt](service-bus-fundamentals-hybrid-solutions.md#topics).

SAS-autentisering använder hello följande element:

* [Auktoriseringsregeln för delad åtkomst](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule): en 256-bitars kryptografiska primärnyckel i Base64-representation, ett valfritt sekundärnyckeln och ett nyckelnamn och associerade behörigheter (en samling *lyssna*, *skicka*, eller *hantera* rättigheter).
* [Delad Åtkomstsignatur](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) token: genereras med hello HMAC SHA256 av en resurssträng som består av hello URI för hello-resurs som används och en utgången med hello kryptografisk nyckel. hello signatur och andra element som beskrivs i följande avsnitt hello formateras till en sträng tooform hello SAS-token.

## <a name="shared-access-policy"></a>Princip för delad åtkomst

En viktig sak toounderstand om SAS är att det börjar med en princip. För varje princip du bestämmer dig för tre uppgifter: **namn**, **omfång**, och **behörigheter**. Hej **namn** är precis som; ett unikt namn i detta scope. hello omfång kan du enkelt: dess hello URI för hello resursen i fråga. För en Service Bus-namnrymd hello scope är hello fullständigt kvalificerade domännamnet (FQDN) som `https://<yournamespace>.servicebus.windows.net/`.

hello behörigheter för en princip finns i stort sett självförklarande:

* Skicka
* Lyssna
* Hantera

När du har skapat hello principen tilldelas en *primärnyckel* och en *sekundärnyckeln*. Dessa är kryptografiskt starka nycklar. Inte förlorar dem eller läcka ut de - de alltid är tillgängliga i hello [Azure-portalen][Azure portal]. Du kan använda antingen hello genererade nycklar och återskapa dem när som helst. Men om du återskapa eller ändra hello primärnyckel i hello princip blir alla signaturer för delad åtkomst som skapats från den ogiltiga.

När du skapar ett namnområde för Service Bus, skapas automatiskt en princip för hello hela namnområdet som kallas **RootManageSharedAccessKey**, och den här principen har alla behörigheter. Du inte logga in som **rot**, så att inte använda den här principen om det finns en mycket bra anledning. Du kan skapa ytterligare principer i hello **konfigurera** för hello namnområde i hello-portalen. Det är viktigt toonote som en enda träd i Service Bus (namnområdet, kön, etc.) kan bara ha in too12 principer kopplade tooit.

## <a name="configuration-for-shared-access-signature-authentication"></a>Konfiguration för autentisering med signatur för delad åtkomst
Du kan konfigurera hello [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) regeln på Service Bus-namnområden, köer och ämnen. Konfigurera en [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) på en Service Bus prenumeration stöds inte för närvarande, men du kan använda regler som konfigurerats på ett namnområde eller avsnittet toosecure åtkomst toosubscriptions. En fungerande exempel som visar den här proceduren finns hello [autentisering med delad signatur åtkomst (SAS) med Service Bus prenumerationer](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c) exempel.

Maximalt 12 regler kan konfigureras på Service Bus-namnområde, kö eller i avsnittet. Regler som är konfigurerade på en Service Bus-namnrymd gäller tooall entiteter i namnutrymmet.

![SAS](./media/service-bus-sas/service-bus-namespace.png)

I den här bilden hello *manageRuleNS*, *sendRuleNS*, och *listenRuleNS* auktoriseringsregler tillämpa tooboth kön F1 och avsnittet T1, medan *listenRuleQ*  och *sendRuleQ* gäller endast tooqueue F1 och *sendRuleT* gäller endast tootopic T1.

Hej nyckelparametrar av en [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) är följande:

| Parameter | Beskrivning |
| --- | --- |
| *Nyckelnamn* |En sträng som beskriver hello auktoriseringsregeln. |
| *PrimaryKey* |En base64-kodad 256-bitars primär nyckel för signering och verifiera hello SAS-token. |
| *Sekundär nyckel* |En base64-kodad 256-bitars sekundär nyckel för signering och verifiera hello SAS-token. |
| *AccessRights* |En lista över åtkomsträttigheter Hej auktoriseringsregeln har beviljat. Dessa rättigheter kan vara en samling av lyssna, skicka och hantera rättigheter. |

När ett namnområde för Service Bus etableras en [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule), med [KeyName](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_KeyName) ställa in också**RootManageSharedAccessKey**, skapas som standard.

## <a name="generate-a-shared-access-signature-token"></a>Generera en signatur för delad åtkomst (token)

hello principen är inte hello åtkomsttoken för Service Bus. Det är hello objekt från vilka hello åtkomst-token genereras - med antingen hello primära och sekundära nycklarna. Alla klienter som har åtkomst toohello Signeringsnycklar som anges i hello auktoriseringsregeln för delad åtkomst kan generera hello SAS-token. hello-token genereras av noggrant utforma en sträng i hello följande format:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

Där `signature-string` är hello SHA-256-hash för hello omfattning hello token (**omfång** enligt beskrivningen i föregående avsnitt i hello) med en CRLF läggs och en förfallotiden (i sekunder sedan hello epok: `00:00:00 UTC` på 1 januari 1970). 

> [!NOTE]
> tooavoid taget kort token upphör att gälla, rekommenderas du att koda hello utgången tid-värden som på minst 32-bitars heltal eller helst en lång (64-bitars) heltal.  
> 
> 

hello hash verkar liknande toohello följande pseudokolumner koden och returnerar 32 byte.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

hello-hash-värden är i hello **SharedAccessSignature** sträng så att hello mottagaren kan beräkna hello hash med hello hello samma parametrar, toobe till att returnera samma resultat. hello URI anger hello omfattning och hello nyckelnamn identifierar hello för toobe används toocompute hello hash. Detta är viktigt ur säkerhetssynpunkt. Om hello signaturen inte matchar som beräknar vilka hello mottagaren (Service Bus), nekas åtkomst. Nu kan du vara säker hello sändaren hade toohello åtkomstnyckel och bör tilldelas hello rättigheter anges i hello-policy.

Observera att du ska använda hello kodad resurs-URI för den här åtgärden. hello resurs-URI är hello fullständiga URI: N för hello Service Bus resursåtkomst toowhich begärs. Till exempel `http://<namespace>.servicebus.windows.net/<entityPath>` eller `sb://<namespace>.servicebus.windows.net/<entityPath>`, dvs `http://contoso.servicebus.windows.net/contosoTopics/T1/Subscriptions/S3`.

auktoriseringsregeln för delad åtkomst av hello användas för signering måste konfigureras på hello enheten som anges av den här URI: N eller av en av dess hierarkiska överordnade. Till exempel `http://contoso.servicebus.windows.net/contosoTopics/T1` eller `http://contoso.servicebus.windows.net` i hello föregående exempel.

En SAS-token är giltig för alla resurser under hello `<resourceURI>` används i hello `signature-string`.

Hej [KeyName](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_KeyName) i hello SAS-token refererar toohello **keyName** av hello auktoriseringsregeln för delad åtkomst används toogenerate hello-token.

Hej *URL-kodade resourceURI* måste vara hello samma som hello URI som används i hello sträng till logga under hello beräkningen av hello signatur. Det bör vara [procent-kodade](https://msdn.microsoft.com/library/4fkewx0t.aspx).

Vi rekommenderar att du regelbundet återskapar hello nycklarna som används i hello [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) objekt. Program ska normalt använda hello [PrimaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_PrimaryKey) toogenerate en SAS-token. När du återskapar hello nycklar, bör du ersätta hello [sekundär nyckel](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_SecondaryKey) med hello gamla primära nyckeln och skapa en ny nyckel som hello nya primärnyckeln. Detta gör att du toocontinue använda token för auktorisering som har utfärdats med hello gamla primärnyckel och som ännu inte har gått ut.

Om du har toorevoke hello nycklar och en nyckel har komprometterats du återskapa båda hello [PrimaryKey](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_PrimaryKey) och hello [sekundär nyckel](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule#Microsoft_ServiceBus_Messaging_SharedAccessAuthorizationRule_SecondaryKey) av en [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule), ersätta dem med nya nycklar. Den här proceduren upphäver alla token som signerats med hello gamla nycklar.

## <a name="how-toouse-shared-access-signature-authentication-with-service-bus"></a>Hur toouse autentisering med signatur för delad åtkomst med Service Bus

hello följande scenarier är konfigurationen av regler för auktorisering, generering av SAS-token och klientauktorisering.

För en fullt fungerande exempel på ett Service Bus-program som visar hello konfiguration och använder SAS auktorisering, se [autentisering med signatur för delad åtkomst med Service Bus](http://code.msdn.microsoft.com/Shared-Access-Signature-0a88adf8). Ett relaterade exempel som visar hello användning av SAS-auktoriseringsregler som konfigurerats på namnområden eller avsnitt toosecure Service Bus prenumerationer finns här: [med delade signatur åtkomst (SAS)-autentisering med Service Bus-prenumerationer ](http://code.msdn.microsoft.com/Using-Shared-Access-e605b37c).

## <a name="access-shared-access-authorization-rules-on-a-namespace"></a>Åtkomst till delade åtkomst auktoriseringsregler för ett namnområde

Autentisering med datorcertifikat krävs för åtgärderna i hello rot för Service Bus-namnrymd. Du måste överföra ett certifikat för din Azure-prenumeration. tooupload ett hanteringscertifikat hello gör [här](../cloud-services/cloud-services-configure-ssl-certificate-portal.md#step-3-upload-a-certificate), med hjälp av hello [Azure-portalen][Azure portal]. Mer information om Azure-hanteringscertifikat finns hello [översikt över Azure certifikat](../cloud-services/cloud-services-certs-create.md#what-are-management-certificates).

hello-slutpunkt för att få åtkomst till auktoriseringsregler för delad åtkomst på en Service Bus-namnrymd är följande:

```http
https://management.core.windows.net/{subscriptionId}/services/ServiceBus/namespaces/{namespace}/AuthorizationRules/
```

toocreate en [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) objekt på en Service Bus-namnrymd, kör en POST-åtgärden på den här slutpunkten med information om hello serialiserad som JSON- eller XML. Exempel:

```csharp
// Base address for accessing authorization rules on a namespace
string baseAddress = @"https://management.core.windows.net/<subscriptionId>/services/ServiceBus/namespaces/<namespace>/AuthorizationRules/";

// Configure authorization rule with base64-encoded 256-bit key and Send rights
var sendRule = new SharedAccessAuthorizationRule("contosoSendAll",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Send });

// Operations on hello Service Bus namespace root require certificate authentication.
WebRequestHandler handler = new WebRequestHandler
{
    ClientCertificateOptions = ClientCertificateOption.Manual
};
// Access hello management certificate by subject name
handler.ClientCertificates.Add(GetCertificate(<certificateSN>));

HttpClient httpClient = new HttpClient(handler)
{
    BaseAddress = new Uri(baseAddress)
};
httpClient.DefaultRequestHeaders.Accept.Add(
    new MediaTypeWithQualityHeaderValue("application/json"));
httpClient.DefaultRequestHeaders.Add("x-ms-version", "2015-01-01");

// Execute a POST operation on hello baseAddress above toocreate an auth rule
var postResult = httpClient.PostAsJsonAsync("", sendRule).Result;
```

På liknande sätt kan använda en GET-åtgärd för hello endpoint tooread hello auktoriseringsregler konfigurerats på hello namnområde.

använda hello följande slutpunkt tooupdate eller ta bort en specifik auktoriseringsregel:

```http
https://management.core.windows.net/{subscriptionId}/services/ServiceBus/namespaces/{namespace}/AuthorizationRules/{KeyName}
```

## <a name="access-shared-access-authorization-rules-on-an-entity"></a>Åtkomst till delade åtkomst auktoriseringsregler för en entitet

Du kan komma åt en [Microsoft.ServiceBus.Messaging.SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) objekt som konfigurerats på en Service Bus-kö eller ett ämne via hello [AuthorizationRules](/dotnet/api/microsoft.servicebus.messaging.authorizationrules) samling i hello motsvarande [QueueDescription](/dotnet/api/microsoft.servicebus.messaging.queuedescription) eller [TopicDescription](/dotnet/api/microsoft.servicebus.messaging.topicdescription).

hello följande kod visar hur tooadd auktoriseringsregler för en kö.

```csharp
// Create an instance of NamespaceManager for hello operation
NamespaceManager nsm = NamespaceManager.CreateFromConnectionString(
    <connectionString> );
QueueDescription qd = new QueueDescription( <qPath> );

// Create a rule with send rights with keyName as "contosoQSendKey"
// and add it toohello queue description.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoSendKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Send }));

// Create a rule with listen rights with keyName as "contosoQListenKey"
// and add it toohello queue description.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoQListenKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] { AccessRights.Listen }));

// Create a rule with manage rights with keyName as "contosoQManageKey"
// and add it toohello queue description.
// A rule with manage rights must also have send and receive rights.
qd.Authorization.Add(new SharedAccessAuthorizationRule("contosoQManageKey",
    SharedAccessAuthorizationRule.GenerateRandomKey(),
    new[] {AccessRights.Manage, AccessRights.Listen, AccessRights.Send }));

// Create hello queue.
nsm.CreateQueue(qd);
```

## <a name="use-shared-access-signature-authorization"></a>Använda signatur för delad åtkomst auktoriseringsregler

Med hello Service Bus .NET-biblioteken hello Azure .NET SDK-program kan använda SAS-auktorisering via hello [SharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.sharedaccesssignaturetokenprovider) klass. hello visar följande kod hello användning av hello tokenleverantör toosend meddelanden tooa Service Bus-kö.

```csharp
Uri runtimeUri = ServiceBusEnvironment.CreateServiceUri("sb",
    <yourServiceNamespace>, string.Empty);
MessagingFactory mf = MessagingFactory.Create(runtimeUri,
    TokenProvider.CreateSharedAccessSignatureTokenProvider(keyName, key));
QueueClient sendClient = mf.CreateQueueClient(qPath);

//Sending hello message tooqueue.
BrokeredMessage helloMessage = new BrokeredMessage("Hello, Service Bus!");
helloMessage.MessageId = "SAS-Sample-Message";
sendClient.Send(helloMessage);
```

Program kan också använda SAS för autentisering med hjälp av en SAS-anslutningssträng i metoder som accepterar anslutningssträngar.

Observera att toouse SAS-auktorisering med Service Bus reläer, kan du använda SAS-nycklar som konfigurerats på hello Service Bus-namnrymd. Om du skapar ett relä uttryckligen på hello namnområde ([NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) med en [RelayDescription](/dotnet/api/microsoft.servicebus.messaging.relaydescription)) objekt du kan ange hello SAS-regler för att relay. toouse SAS-auktorisering med Service Bus-prenumerationer, du kan använda SAS-nycklar som konfigurerats på en Service Bus-namnrymd eller ett ämne.

## <a name="use-hello-shared-access-signature-at-http-level"></a>Använd hello signatur för delad åtkomst (på http-nivå)

Nu när du vet hur toocreate signaturer för delad åtkomst för alla entiteter i Service Bus, du är klar tooperform en HTTP POST:

```http
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 

Kom ihåg att det fungerar för alla. Du kan skapa SAS för kön, ämnet och prenumerationen. 

Om du ger en avsändare eller klienten en SAS-token, de inte har hello nyckeln direkt och de kan inte återställa hello hash tooobtain den. Som sådana har du kontroll över vad de kan komma åt och hur lång tid. En viktig sak tooremember är att om du ändrar hello primärnyckel i hello princip alla signaturer för delad åtkomst som skapats från den blir ogiltiga.

## <a name="use-hello-shared-access-signature-at-amqp-level"></a>Använd hello signatur för delad åtkomst (på AMQP nivå)

I föregående avsnitt hello sett du hur toouse hello SAS-token med en HTTP POST-begäran för att skicka data toohello Service Bus. Som du vet du kan komma åt Service Bus använder hello avancerade Message Queuing Protocol (AMQP) som är hello önskad protokollet toouse av prestandaskäl i många scenarier. hello SAS-token användning med AMQP beskrivs i hello dokumentet [AMQP Claim-Based Security Version 1.0](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) som är i utkastet sedan 2013 men väl stöds av Azure idag.

Innan du startar toosend data tooService Bus hello publisher måste skicka hello SAS-token i en AMQP tooa väldefinierade AMQP Meddelandenod med namnet **$cbs** (du kan se det som en ”särskilda” kö som används av hello service tooacquire och verifiera att alla hello SAS-token). hello publisher måste ange hello **ReplyTo** fältet i AMQP hälsningsmeddelande; detta är hello nod i vilka hello tjänsten svarar toohello utgivaren med hello resultatet av hello token verifiering (ett enkelt request/reply-mönster mellan utgivare och service). Detta svar noder har skapats ”på hello direkt”, talar om ”skapas dynamiskt fjärrnoden” enligt beskrivningen av hello AMQP 1.0-specifikationen. När du har kontrollerat att hello SAS-token är giltig hello publisher går framåt och starta toosend data toohello-tjänsten.

hello följande steg visar hur toosend hello SAS-token med AMQP-protokollet med hello [AMQP.Net Lite](https://github.com/Azure/amqpnetlite) bibliotek. Detta är användbart om du inte kan använda hello officiella Service Bus SDK (till exempel på WinRT .net Compact Framework, .net Framework Micro och Mono) utveckla C\#. Naturligtvis kan det här biblioteket är användbara toohelp förstå hur anspråksbaserade fungerar på hello AMQP nivå som du såg hur det fungerar på hello HTTP-nivå (med en HTTP POST-begäran och hello SAS token skickade inuti hello ”” autentiseringshuvud). Om du inte behöver dessa djup kunskaper om AMQP, kan du använda hello officiella Service Bus SDK med .net Framework-program, vilket gör det åt dig.

### <a name="c35"></a>C&#35;

```csharp
/// <summary>
/// Send claim-based security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) toosend</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct hello put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = Guid.NewGuid().ToString();
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive hello response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // hello sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

Hej `PutCbsToken()` metoden tar emot hello *anslutning* (AMQP anslutning-klassinstans som tillhandahålls av hello [AMQP .NET Lite biblioteket](https://github.com/Azure/amqpnetlite)) som representerar hello TCP-anslutningstjänst toohello och hello *sasToken* parameter hello SAS-token toosend. 

> [!NOTE]
> Det är viktigt att hello anslutningen har skapats med **SASL autentiseringsmekanism ange tooEXTERNAL** (och inte hello standard OFORMATERAD med användarnamn och lösenord som används när du inte behöver toosend hello SAS-token).
> 
> 

Därefter skapas hello två AMQP länkar för att skicka hello SAS-token och ta emot hello svar (hello token validering resultat) från hello-tjänsten.

AMQP hello-meddelande innehåller en uppsättning egenskaper och mer än ett enkelt meddelande. hello SAS-token är hello brödtext hello-meddelande (med sin konstruktor). Hej **”ReplyTo”** egenskapen toohello nodnamnet för att ta emot hello validering resultat på hello mottagaren länk (du kan ändra dess namn om du vill och den kommer att skapas dynamiskt av hello-tjänsten). hello sista tre program/anpassade egenskaper som används av hello service tooindicate vilken typ av åtgärd som den har tooexecute. Enligt beskrivningen av hello CBS utkast till en specifikation måste de vara hello **åtgärdsnamn** (”put-token”), hello **typ av token** (i det här fallet en ”servicebus.windows.net:sastoken”), och hello **” namnet ”för hello målgruppen** toowhich hello token gäller (hello hela entitet).

När du har skickat hello SAS-token på hello avsändaren länk läsa hello publisher hello svara på hello mottagaren länk. hello svar är ett enkelt AMQP-meddelande med en program-egenskap med namnet **”statuskod”** som kan innehålla hello samma värden som en HTTP-statuskod.

## <a name="rights-required-for-service-bus-operations"></a>Rättigheter som krävs för Service Bus-åtgärder

hello visar följande tabell hello behörigheter som krävs för olika åtgärder på Service Bus-resurser.

| Åtgärd | Anspråk krävs | Anspråk omfång |
| --- | --- | --- |
| **Namespace** | | |
| Konfigurera auktoriseringsregeln för ett namnområde |Hantera |Alla adresser i namnområdet |
| **Tjänsten registret** | | |
| Räkna upp privata principer |Hantera |Alla adresser i namnområdet |
| Börja lyssna på ett namnområde |Lyssna |Alla adresser i namnområdet |
| Skicka meddelanden tooa lyssnare i ett namnområde |Skicka |Alla adresser i namnområdet |
| **Kön** | | |
| Skapa en kö |Hantera |Alla adresser i namnområdet |
| Ta bort en kö |Hantera |En giltig kö-adress |
| Räkna upp köer |Hantera |$ Resurser/köer |
| Hämta hello beskrivning av kön |Hantera |En giltig kö-adress |
| Konfigurera auktoriseringsregeln för en kö |Hantera |En giltig kö-adress |
| Skicka i toohello kö |Skicka |En giltig kö-adress |
| Ta emot meddelanden från en kö |Lyssna |En giltig kö-adress |
| Avbryt eller slutföra meddelanden efter hello-meddelande i titt låsläge |Lyssna |En giltig kö-adress |
| Skjuta upp ett meddelande för senare hämtning |Lyssna |En giltig kö-adress |
| Systemkön ett meddelande |Lyssna |En giltig kö-adress |
| Hämta hello tillstånd är kopplat till en message queue-session |Lyssna |En giltig kö-adress |
| Ange hello tillstånd är kopplat till en message queue-session |Lyssna |En giltig kö-adress |
| **Avsnittet** | | |
| Skapa ett ämne |Hantera |Alla adresser i namnområdet |
| Ta bort ett ämne |Hantera |En giltig avsnittet adress |
| Räkna upp avsnitt |Hantera |$ Resurser/ämnen |
| Hämta hello avsnittet beskrivning |Hantera |En giltig avsnittet adress |
| Konfigurera auktoriseringsregeln för ett ämne |Hantera |En giltig avsnittet adress |
| Skicka toohello avsnittet |Skicka |En giltig avsnittet adress |
| **Prenumeration** | | |
| Skapa en prenumeration |Hantera |Alla adresser i namnområdet |
| Ta bort prenumeration |Hantera |.. /myTopic/subscriptions/mySubscription |
| Räkna upp prenumerationer |Hantera |.. myTopic-prenumerationer |
| Hämta prenumeration beskrivning |Hantera |.. /myTopic/subscriptions/mySubscription |
| Avbryt eller slutföra meddelanden efter hello-meddelande i titt låsläge |Lyssna |.. /myTopic/subscriptions/mySubscription |
| Skjuta upp ett meddelande för senare hämtning |Lyssna |.. /myTopic/subscriptions/mySubscription |
| Systemkön ett meddelande |Lyssna |.. /myTopic/subscriptions/mySubscription |
| Hämta hello-tillståndet associerat med ett session-avsnittet |Lyssna |.. /myTopic/subscriptions/mySubscription |
| Ange hello-tillståndet associerat med ett session-avsnittet |Lyssna |.. /myTopic/subscriptions/mySubscription |
| **Regler** | | |
| Skapa en regel |Hantera |.. /myTopic/subscriptions/mySubscription |
| Ta bort en regel |Hantera |.. /myTopic/subscriptions/mySubscription |
| Räkna upp regler |Hantera eller lyssna |.. /myTopic/subscriptions/mySubscription/rules 

## <a name="next-steps"></a>Nästa steg

toolearn mer om Service Bus-meddelanden finns i följande avsnitt hello.

* [Service Bus-grunder](service-bus-fundamentals-hybrid-solutions.md)
* [Service Bus-köer, ämnen och prenumerationer](service-bus-queues-topics-subscriptions.md)
* [Hur toouse Service Bus-köer](service-bus-dotnet-get-started-with-queues.md)
* [Hur toouse Service Bus-ämnen och prenumerationer](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[Azure portal]: https://portal.azure.com