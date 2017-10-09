---
title: "aaaUnderstand Azure IoT Hub säkerhet | Microsoft Docs"
description: "Utvecklare guide - hur toocontrol åt tooIoT hubb för appar för enheter och backend-appar. Innehåller information om säkerhetstoken och stöd för X.509-certifikat."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 45631e70-865b-4e06-bb1d-aae1175a52ba
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 717726328a6bb5c5c334a123d0abfed711b2c3b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="control-access-tooiot-hub"></a>Kontrollera åtkomst tooIoT Hub

Den här artikeln beskrivs hello alternativ för att skydda din IoT-hubb. IoT-hubb använder *behörigheter* toogrant åtkomst tooeach IoT-hubb slutpunkt. Behörigheter begränsa hello åtkomst tooan IoT-hubb baseras på funktionen.

Den här artikeln beskrivs:

* hello olika behörigheter som du kan bevilja tooa enhet eller backend-app tooaccess din IoT-hubb.
* Hej processen och hello autentiseringstoken används tooverify behörigheter.
* Hur autentiseringsuppgifter tooscope toolimit åtkomst toospecific resurser.
* IoT-hubb stöd för X.509-certifikat.
* Anpassade autentiseringsmekanismer som använder befintliga enhetens identitet register eller autentiseringsscheman.

### <a name="when-toouse"></a>När toouse

Du måste ha behörighet tooaccess någon hello IoT-hubbslutpunkter. Till exempel måste en enhet innehålla en token som innehåller säkerhetsreferenser tillsammans med alla meddelanden som skickas tooIoT hubb.

## <a name="access-control-and-permissions"></a>Åtkomstkontroll och behörigheter

Du kan ge [behörigheter](#iot-hub-permissions) i hello följande sätt:

* **IoT-hubb-nivå delade åtkomstprinciper**. Principer för delad åtkomst kan ge en kombination av [behörigheter](#iot-hub-permissions). Du kan definiera principer i hello [Azure-portalen][lnk-management-portal], eller programmässigt med hjälp av hello [IoT-hubb resursprovidern REST API: er][lnk-resource-provider-apis]. En nyligen skapade IoT-hubben har hello följande standardprinciperna:

  * **iothubowner**: princip med alla behörigheter.
  * **tjänsten**: princip med **ServiceConnect** behörighet.
  * **enheten**: princip med **DeviceConnect** behörighet.
  * **registryRead**: princip med **RegistryRead** behörighet.
  * **registryReadWrite**: princip med **RegistryRead** och RegistryWrite behörigheter.
  * **Per enhet säkerhetsreferenser**. Varje IoT-hubb innehåller en [identitetsregistret][lnk-identity-registry]. Du kan konfigurera säkerhetsreferenser som ger för varje enhet i den här identitetsregistret **DeviceConnect** behörigheter omfång toohello motsvarande enhet slutpunkter.

Till exempel i en typisk IoT-lösningen:

* hello device management komponenten använder hello *registryReadWrite* princip.
* hello händelse processorkomponent använder hello *service* princip.
* hello körning enheten business logic komponenten använder hello *service* princip.
* Enskilda enheter ansluter med autentiseringsuppgifter som lagras i registret för hello IoT hub-identitet.

> [!NOTE]
> Se [behörigheter](#iot-hub-permissions) detaljerad information.

## <a name="authentication"></a>Autentisering

Azure IoT-hubb beviljar åtkomst tooendpoints genom att verifiera en token mot hello delade åtkomstprinciper och identitet säkerhetsreferenser för registret.

Säkerhetsreferenser, till exempel symmetriska nycklar skickas aldrig via hello-överföring.

> [!NOTE]
> hello Azure IoT Hub-resursprovidern skyddas via Azure-prenumerationen, eftersom alla providrar på hello [Azure Resource Manager][lnk-azure-resource-manager].

Mer information om hur tooconstruct och använda säkerhetstoken finns [IoT-hubb säkerhetstoken][lnk-sas-tokens].

### <a name="protocol-specifics"></a>Protokollet programvarukrav

Varje protokoll som stöds, till exempel MQTT, AMQP och HTTP, transport token på olika sätt.

När du använder MQTT hello Anslut paket har hello deviceId som hello ClientId, {iothubhostname} / {deviceId} i hello användarnamn och en SAS-token i hello lösenordsfältet. {iothubhostname} bör hello fullständig CName för hello IoT-hubb (till exempel contoso.azure-devices.net).

När du använder [AMQP][lnk-amqp], IoT-hubb stöder [SASL OFORMATERAD] [ lnk-sasl-plain] och [AMQP anspråk baserade-säkerhet] [ lnk-cbs].

Om du använder AMQP anspråk baserade-säkerhet, hello standard anger hur tootransmit dessa token.

SASL OFORMATERAD hello **användarnamn** kan vara:

* `{policyName}@sas.root.{iothubName}`Om du använder token för IoT-hubb-nivå.
* `{deviceId}@sas.{iothubname}`Om du använder enheten omfång token.

I båda fallen hello lösenordsfältet innehåller hello-token i [IoT-hubb säkerhetstoken][lnk-sas-tokens].

HTTP implementerar autentisering genom att inkludera en giltig token i hello **auktorisering** huvudet i begäran.

#### <a name="example"></a>Exempel

Användarnamn (DeviceId är skiftlägeskänsligt):`iothubname.azure-devices.net/DeviceId`

Lösenord (generera SAS-token med hello [enheten explorer] [ lnk-device-explorer] verktyget):`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [!NOTE]
> Hej [Azure IoT SDK] [ lnk-sdks] automatiskt generera token när du ansluter toohello service. I vissa fall stöder inte hello Azure IoT SDK alla hello protokoll eller alla hello autentiseringsmetoder.

### <a name="special-considerations-for-sasl-plain"></a>Att tänka på SASL OFORMATERAD

När du använder SASL OFORMATERAD med AMQP kan kan en klient som ansluter tooan IoT-hubb använda en enda token för varje TCP-anslutning. När hello-token upphör att gälla, hello TCP-anslutningen kopplas från hello-tjänsten och utlöser en återanslutning. Det här beteendet skadas när det inte problematisk för en backend-app, för en enhetsapp för hello följande orsaker:

* Gateway ansluter vanligtvis för många enheter. När du använder SASL OFORMATERAD, har de toocreate en distinkta TCP-anslutning för varje enhet som ansluter tooan IoT-hubb. Det här scenariot avsevärt ökar hello förbrukningen av ström och nätverksresurser och ökar hello svarstiden för varje enhetsanslutning.
* Begränsad resurs enheter påverkas negativt av hello ökad användning av resurser tooreconnect efter varje token upphör att gälla.

## <a name="scope-iot-hub-level-credentials"></a>Autentiseringsuppgifter för scope IoT hub-nivå

Du kan definiera säkerhetsprinciper för IoT-hubb-nivå genom att skapa token med en skyddad resurs-URI. Slutpunkten toosend enhet till moln hälsningsmeddelande från en enhet är till exempel **/devices/ {deviceId} / meddelanden/händelser**. Du kan också använda en IoT-hubb-nivå delad åtkomstprincip med **DeviceConnect** behörigheter toosign en token vars resourceURI är **/devices/ {deviceId}**. Den här metoden skapar en token som är bara användbara toosend meddelanden för enheten **deviceId**.

Den här mekanismen är liknande toohello [Händelsehubbar utgivarprincip][lnk-event-hubs-publisher-policy], och möjliggör tooimplement anpassade autentiseringsmetoder.

## <a name="security-tokens"></a>Säkerhetstoken

IoT-hubb använder säkerhet tokens tooauthenticate enheter och tjänster tooavoid skicka nycklar hello uppkopplat läge. Dessutom är säkerhetstoken begränsad giltighetstid och omfång. [Azure IoT-SDK] [ lnk-sdks] automatiskt generera token utan någon specialkonfiguration. Vissa scenarier göra kräver toogenerate och använda säkerhetstoken direkt. Sådana scenarier är:

* hello direkt användning av hello MQTT, AMQP och HTTP.
* Hej implementering av hello tokentjänsten mönster, enligt beskrivningen i [anpassad enhetsautentisering][lnk-custom-auth].

IoT-hubb kan också enheter tooauthenticate med IoT-hubb med [X.509-certifikat][lnk-x509].

### <a name="security-token-structure"></a>Säkerhet token struktur

Hjälp säkerhet token toogrant tid begränsas åtkomsten toodevices och tjänster toospecific funktioner i IoT-hubb. tooget auktorisering tooconnect tooIoT Hub, enheter och tjänster måste skicka säkerhetstoken som signerade med en delad åtkomst eller en symmetrisk nyckel. Dessa nycklar lagras med en enhetsidentitet i hello identitetsregistret.

En token som signerats med en delad åtkomst viktiga beviljar åtkomst tooall hello funktionalitet som är associerade med hello delade åtkomstbehörigheter för principen. En token som signerats med en enhetsidentitet symmetrisk nyckel endast beviljar hello **DeviceConnect** för hello associerade enhetens identitet.

hello säkerhetstoken har hello följande format:

`SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

Här följer hello förväntade värden:

| Värde | Beskrivning |
| --- | --- |
| {signatur} |En sträng med HMAC SHA256 signatur hello formatet: `{URL-encoded-resourceURI} + "\n" + expiry`. **Viktiga**: hello nyckeln avkoda från base64 och användas som nyckel tooperform hello HMAC SHA256 beräkning. |
| {resourceURI} |URI-prefix (av segment) hello-slutpunkter som kan användas med denna token som börjar med värdnamnet för hello IoT-hubb (ingen protocol). Till exempel, `myHub.azure-devices.net/devices/device1` |
| {utgången} |UTF8 strängar för antal sekunder sedan hello epok 00:00:00 UTC på 1 januari 1970. |
| {URL-kodade-resourceURI} |Lägre fall URL-kodning av hello gemen resurs-URI |
| {policyName} |hello namnet på hello delad åtkomst princip toowhich refererar detta token. Hello token refererar inte fram om toodevice registret autentiseringsuppgifter. |

**Observera på prefixet**: hello URI-prefix beräknas av segment och inte av tecken. Till exempel `/a/b` är ett prefix för `/a/b/c` men inte för `/a/bc`.

hello följande Node.js utdrag visar en funktion kallad **generateSasToken** som beräknar hello token från hello indata `resourceUri, signingKey, policyName, expiresInMins`. hello nästa avsnitt innehåller information om hur tooinitialize hello olika indata för hello annan token användningsfall.

```nodejs
var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
    resourceUri = encodeURIComponent(resourceUri);

    // Set expiration in seconds
    var expires = (Date.now() / 1000) + expiresInMins * 60;
    expires = Math.ceil(expires);
    var toSign = resourceUri + '\n' + expires;

    // Use crypto
    var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
    hmac.update(toSign);
    var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

    // Construct autorization string
    var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
    + base64UriEncoded + "&se=" + expires;
    if (policyName) token += "&skn="+policyName;
    return token;
};
```

Som en jämförelse code hello motsvarande Python toogenerate en säkerhetstoken är:

```python
from base64 import b64encode, b64decode
from hashlib import sha256
from time import time
from urllib import quote_plus, urlencode
from hmac import HMAC

def generate_sas_token(uri, key, policy_name, expiry=3600):
    ttl = time() + expiry
    sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
    print sign_key
    signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

    rawtoken = {
        'sr' :  uri,
        'sig': signature,
        'se' : str(int(ttl))
    }

    if policy_name is not None:
        rawtoken['skn'] = policy_name

    return 'SharedAccessSignature ' + urlencode(rawtoken)
```

> [!NOTE]
> Eftersom hello giltighetstid hello token verifieras på IoT-hubb datorer måste hello drift på hello klocka hello-dator som genererar hello token vara minimal.

### <a name="use-sas-tokens-in-a-device-app"></a>Använd SAS-token i en enhetsapp

Det finns två sätt tooobtain **DeviceConnect** behörigheter med IoT-hubb med säkerhetstoken: använda en [symmetriska enhetsnyckel från hello identitetsregistret](#use-a-symmetric-key-in-the-identity-registry), eller Använd en [delade åtkomstnyckeln](#use-a-shared-access-policy).

Kom ihåg att alla funktioner som är tillgängliga från enheter exponeras avsiktligt på slutpunkter med prefixet `/devices/{deviceId}`.

> [!IMPORTANT]
> hello enda sättet att IoT-hubb autentiserar en enhet använder hello enheten identitet symmetrisk nyckel. I fall när en princip för delad åtkomst är används tooaccess enhetsfunktion hello lösningen måste ta hänsyn till hello-komponent som utfärdar hello säkerhetstoken som en betrodd underkomponenten.

hello enheten riktade slutpunkter är (oavsett hello protocol):

| Slutpunkt | Funktioner |
| --- | --- |
| `{iot hub host name}/devices/{deviceId}/messages/events` |Skicka meddelanden från enhet till moln. |
| `{iot hub host name}/devices/{deviceId}/devicebound` |Ta emot meddelanden moln till enhet. |

### <a name="use-a-symmetric-key-in-hello-identity-registry"></a>Använd en symmetrisk nyckel i registret för hello identitet

När du använder en enhetsidentitet symmetrisk nyckel toogenerate en token hello principnamn (`skn`) element av hello token utelämnas.

Exempelvis skapas en token tooaccess alla enhetsfunktion bör ha hello följande parametrar:

* resurs-URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* nyckel för signeringscertifikatet: en symmetrisk nyckel för hello `{device id}` identitet,
* Inga Principnamn
* helst upphör att gälla.

Ett exempel som använder hello föregående Node.js-funktion är:

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var deviceKey ="...";

var token = generateSasToken(endpoint, deviceKey, null, 60);
```

hello resultat som beviljar åtkomst tooall funktioner för device1 är:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697`

> [!NOTE]
> Det är möjligt toogenerate en SAS-token med hjälp av hello .NET [enheten explorer] [ lnk-device-explorer] verktyget eller hello plattformsoberoende, baserat på noden [iothub explorer] [ lnk-iothub-explorer] kommandoradsverktyg.

### <a name="use-a-shared-access-policy"></a>Använda en princip för delad åtkomst

När du skapar en token från en princip för delad åtkomst, ange hello `skn` fältnamnet toohello hello princip. Den här principen måste ge hello **DeviceConnect** behörighet.

hello två huvudscenarier för att använda funktionen för delad åtkomst principer tooaccess enhet är:

* [molnet protokollet gateways][lnk-endpoints],
* [token services] [ lnk-custom-auth] används tooimplement anpassade autentiseringsscheman.

Sedan hello kan princip för delad åtkomst eventuellt ge åtkomst tooconnect som en enhet, det är viktigt toouse hello rätt resurs URI när du skapar säkerhetstoken. Den här inställningen är särskilt viktigt för token-tjänster med tooscope hello token tooa enhet med hjälp av hello resurs-URI. Den här platsen är relevanta för protokollet gateways som de redan fördelar trafik för alla enheter.

Exempelvis en token tjänst med hello som skapats i förväg delad åtkomstprincip som heter **enhet** skulle skapa en token med hello följande parametrar:

* resurs-URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* nyckel för signeringscertifikatet: en av hello nycklar av hello `device` princip
* Principnamn: `device`,
* helst upphör att gälla.

Ett exempel som använder hello föregående Node.js-funktion är:

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

hello resultat som beviljar åtkomst tooall funktioner för device1 är:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device`

En protocol-gateway kan använda samma token för alla enheter som bara ange hello resurs-URI för hello`myhub.azure-devices.net/devices`.

### <a name="use-security-tokens-from-service-components"></a>Använd säkerhetstoken från tjänstkomponenter

Tjänstens komponenter kan bara generera säkerhetstoken med hjälp av principer för delad åtkomst ger hello lämpliga behörigheter som tidigare förklarats.

Här är hello servicefunktioner exponerad på hello slutpunkter:

| Slutpunkt | Funktioner |
| --- | --- |
| `{iot hub host name}/devices` |Skapa, uppdatera, hämta och ta bort enheten identiteter. |
| `{iot hub host name}/messages/events` |Ta emot meddelanden från enhet till moln. |
| `{iot hub host name}/servicebound/feedback` |Ta emot feedback för moln till enhet meddelanden. |
| `{iot hub host name}/devicebound` |Skicka meddelanden moln till enhet. |

Som exempel, en tjänst som genereras med hjälp av hello som skapats i förväg delad åtkomstprincip som heter **registryRead** skulle skapa en token med hello följande parametrar:

* resurs-URI: `{IoT hub name}.azure-devices.net/devices`,
* nyckel för signeringscertifikatet: en av hello nycklar av hello `registryRead` princip
* Principnamn: `registryRead`,
* helst upphör att gälla.

```nodejs
var endpoint ="myhub.azure-devices.net/devices";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

hello resultat som beviljar åtkomst tooread identiteter för alla enheter, är:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead`

## <a name="supported-x509-certificates"></a>Stöds X.509-certifikat

Du kan använda en enhet för tooauthenticate alla X.509-certifikat med IoT-hubben. Certifikat är:

* **Ett befintligt X.509-certifikat**. En enhet kanske redan har ett X.509-certifikat som är kopplade till den. hello enheten kan använda det här certifikatet tooauthenticate med IoT-hubben.
* **En egen genereras och självsignerade certifikat för X-509**. En enhetstillverkaren eller interna deployer kan generera dessa certifikat och lagra hello motsvarande privata nyckel (och certifikat) på hello enhet. Du kan använda verktyg som [OpenSSL] [ lnk-openssl] och [Windows SelfSignedCertificate] [ lnk-selfsigned] utility för detta ändamål.
* **X.509-certifikat som signerats av Certifikatutfärdaren**. tooidentify en enhet och autentisera med IoT-hubb, kan du använda ett X.509-certifikat som genereras och signerade av en certifikatutfärdare (CA). IoT-hubb kontrolleras endast att hello tumavtrycket presenteras matchar hello konfigurerade tumavtrycket. IotHub kan inte valideras hello certifikatkedjan.

En enhet kan antingen använda ett X.509-certifikat eller en säkerhetstoken för autentisering, men inte båda.

### <a name="register-an-x509-certificate-for-a-device"></a>Registrera ett X.509-certifikat för en enhet

Hej [Azure IoT Service SDK för C#] [ lnk-service-sdk] (version 1.0.8+) har stöd för registrering av en enhet som använder ett X.509-certifikat för autentisering. Andra API: er som import och export av enheter har också stöd för X.509-certifikat.

### <a name="c-support"></a>C\# stöd

Hej **RegistryManager** klassen innehåller en programmässiga sätt tooregister en enhet. I synnerhet hello **AddDeviceAsync** och **UpdateDeviceAsync** metoder aktivera tooregister och uppdatera en enhet i hello IoT-hubb identitetsregistret. Dessa två metoder ta en **enhet** instansen som indata. Hej **enhet** klassen innehåller en **autentisering** egenskapen som du kan använda toospecify primära och sekundära X.509 tumavtryck för certifikatet. hello tumavtrycket representerar en SHA-1-hash för hello X.509-certifikat (som lagras med hjälp av binär kodning DER). Du har hello möjlighet att ange tumavtryck för en primär eller sekundär tumavtryck eller båda. Primära och sekundära tumavtryck är stöds toohandle certifikat förnyelse scenarier.

> [!NOTE]
> IoT-hubb inte kräver eller lagra hello hela X.509-certifikat, endast hello tumavtryck.

Här följer ett exempel på C\# code fragment tooregister en enhet med ett X.509-certifikat:

```csharp
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "921BC9694ADEB8929D4F7FE4B9A3A6DE58B0790B"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-certificate-during-run-time-operations"></a>Använd ett X.509-certifikat under körning

Hej [Azure IoT-enhet SDK för .NET] [ lnk-client-sdk] (version 1.0.11+) kan du använda hello X.509-certifikat.

### <a name="c-support"></a>C\# stöd

Hej klassen **DeviceAuthenticationWithX509Certificate** stöder hello skapandet av **DeviceClient** instanser med hjälp av ett X.509-certifikat. hello X.509-certifikat måste ha formatet hello PFX (kallas även PKCS #12) som innehåller hello privata nyckel.

Här är ett kodfragment som exempel:

```csharp
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a>Anpassad autentisering

Du kan använda hello IoT-hubb [identitetsregistret] [ lnk-identity-registry] tooconfigure per enhet säkerhetsreferenser och åtkomst kontroll med hjälp av [token] [ lnk-sas-tokens] . Om det finns redan en anpassad identitet registret och/eller autentisering schemat för en IoT-lösning, kan du överväga att skapa en *token service* toointegrate infrastrukturen med IoT-hubben. På så sätt kan använda du andra IoT-funktioner i din lösning.

En token service är en anpassad molntjänst. Den använder en IoT-hubb *delad åtkomstprincip* med **DeviceConnect** behörigheter toocreate *enheten omfång* token. Dessa token kan en enhet tooconnect tooyour IoT-hubb.

![Steg för hello tokentjänsten mönster][img-tokenservice]

Här följer hello Huvudsteg för hello tokentjänsten mönster:

1. Skapa en IoT-hubb delad åtkomstprincip med **DeviceConnect** behörigheter för din IoT-hubb. Du kan skapa den här principen i hello [Azure-portalen] [ lnk-management-portal] eller programmässigt. hello-tokentjänsten använder den här principen toosign hello-token som skapas.
1. När en enhet måste tooaccess din IoT-hubb, begär en signerade token från token-tjänsten. hello enheten kan autentisera med din anpassade identiteten registret/schemat toodetermine hello enheten autentiseringsidentitet att hello tokentjänsten använder toocreate hello-token.
1. hello-tokentjänsten returnerar en token. hello token skapas med hjälp av `/devices/{deviceId}` som `resourceURI`, med `deviceId` som hello enhet som autentiseras. hello-tokentjänsten använder hello delad princip tooconstruct hello åtkomsttoken.
1. hello enhet använder hello token direkt med hello IoT-hubb.

> [!NOTE]
> Du kan använda hello .NET-klass [SharedAccessSignatureBuilder] [ lnk-dotnet-sas] eller hello Java-klass [IotHubServiceSasToken] [ lnk-java-sas] toocreate en token i din token tjänst.

hello-tokentjänsten kan ange hello token upphör att gälla efter behov. När hello-token upphör att gälla servrarna hello enhetsanslutning hello IoT-hubb. Hello enheten måste sedan begära en ny token från token hello-tjänsten. En kort förfallotid ökar hello belastningen på både hello enhets- och hello token.

För enheten tooconnect tooyour hubb måste du ändå lägga toohello identitetsregistret för IoT-hubb – även om hello enheten använder en token och inte en enhet viktiga tooconnect. Därför kan du fortsätta toouse per enhet åtkomstkontroll genom att aktivera eller inaktivera enheten identiteter i hello [identitetsregistret][lnk-identity-registry]. Den här metoden minskar hello riskerna med att använda token med lång upphör att gälla.

### <a name="comparison-with-a-custom-gateway"></a>Jämförelse med en anpassad gateway

hello-tokentjänsten mönstret är hello rekommenderat sätt tooimplement register-/ autentiseringsschema en anpassad identitet med IoT-hubben. Det här mönstret rekommenderas eftersom IoT-hubb fortsätter toohandle de flesta av hello lösning trafik. Om hello anpassade autentiseringsschemat så sammanflätade med hello-protokollet, du kan dock kräva en *anpassade gateway* tooprocess alla hello trafik. Ett exempel på en sådan situation använder[Transport Layer Security (TLS) och i förväg delade nycklar (PSKs)][lnk-tls-psk]. Mer information finns i hello [protokollgatewayen] [ lnk-protocols] avsnittet.

## <a name="reference-topics"></a>Referensinformation:

hello ger följande Referensinformation dig mer om styra åtkomst tooyour IoT-hubb.

## <a name="iot-hub-permissions"></a>Behörigheter för IoT-hubb

hello visar följande tabell hello behörigheter som du kan använda toocontrol åtkomst tooyour IoT-hubb.

| Behörighet | Anteckningar |
| --- | --- |
| **RegistryRead** |Ger läsbehörighet toohello identitetsregistret. Mer information finns i [identitetsregistret][lnk-identity-registry]. <br/>Den här behörigheten används av backend-molntjänster. |
| **RegistryReadWrite** |Ger Läs- och skrivbehörighet toohello identitetsregistret. Mer information finns i [identitetsregistret][lnk-identity-registry]. <br/>Den här behörigheten används av backend-molntjänster. |
| **ServiceConnect** |Ger åtkomst till toocloud service-riktade kommunikation och övervakning av slutpunkter. <br/>Beviljar behörighet tooreceive enhet till moln meddelanden, skicka meddelanden moln till enhet och hämta hello motsvarande leverans bekräftelser. <br/>Beviljar behörighet tooretrieve leverans bekräftelser för filen filöverföringar. <br/>Beviljar behörighet tooaccess enhet twins tooupdate taggar och önskade egenskaper hämta rapporterade egenskaper och köra frågor. <br/>Den här behörigheten används av backend-molntjänster. |
| **DeviceConnect** |Ger åtkomst till toodevice riktade slutpunkter. <br/>Beviljar behörighet toosend enhet till moln meddelanden och ta emot meddelanden moln till enhet. <br/>Beviljar behörighet tooperform Filöverföring från en enhet. <br/>Beviljar behörighet tooreceive enheten dubbla önskad egenskapen meddelanden och uppdatera enheten dubbla rapporterade egenskaper. <br/>Beviljar filöverföringar behörighet tooperform. <br/>Den här behörigheten används av enheter. |

## <a name="additional-reference-material"></a>Ytterligare referensmaterialet

Andra referensavsnitten i hello IoT-hubb Utvecklarhandbok inkluderar:

* [IoT-hubbslutpunkter] [ lnk-endpoints] beskriver hello olika slutpunkter som varje IoT-hubb visar för körning och hanteringsåtgärder.
* [Begränsning och kvoter] [ lnk-quotas] beskriver hello kvoter och begränsning beteenden som tillämpas toohello IoT-hubb-tjänsten.
* [Azure IoT-enheten och tjänsten SDK] [ lnk-sdks] visar hello olika språk SDK: er som du kan använda när du utvecklar appar för både enheten och tjänsten som interagerar med IoT-hubben.
* [IoT-hubb frågespråket] [ lnk-query] beskriver hello frågespråk som du kan använda tooretrieve information från IoT-hubb om enheten twins och jobb.
* [Stöd för IoT-hubb MQTT] [ lnk-devguide-mqtt] ger mer information om stöd för IoT-hubb för hello MQTT protokollet.

## <a name="next-steps"></a>Nästa steg

Nu har du fått veta hur toocontrol åt IoT-hubb, kanske du är intresserad av i följande avsnitt i IoT-hubb developer hello:

* [Använda enhetstillstånd twins toosynchronize och konfigurationer][lnk-devguide-device-twins]
* [Anropa en metod som är direkt på en enhet][lnk-devguide-directmethods]
* [Schema-jobb på flera enheter][lnk-devguide-jobs]

Om du vill tootry titt på hello begrepp som beskrivs i den här artikeln får du är intresserad av hello följande IoT-hubb Självstudier:

* [Kom igång med Azure IoT-hubb][lnk-getstarted-tutorial]
* [Hur toosend moln till enhet meddelanden med IoT-hubb][lnk-c2d-tutorial]
* [Hur tooprocess IoT-hubb meddelanden från enhet till moln][lnk-d2c-tutorial]

<!-- links and images -->

[img-tokenservice]: ./media/iot-hub-devguide-security/tokenservice.png
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-openssl]: https://www.openssl.org/
[lnk-selfsigned]: https://technet.microsoft.com/library/hh848633

[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-sas-tokens]: iot-hub-devguide-security.md#security-tokens
[lnk-amqp]: https://www.amqp.org/
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-management-portal]: https://portal.azure.com
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-dotnet-sas]: https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx
[lnk-java-sas]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service.auth._iot_hub_service_sas_token
[lnk-tls-psk]: https://tools.ietf.org/html/rfc4279
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-custom-auth]: iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: iot-hub-devguide-security.md#supported-x509-certificates
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
