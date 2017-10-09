# <a name="secure-your-iot-deployment"></a>Skydda distributionen av IoT
Den här artikeln innehåller hello nästa detaljnivå för att skydda hello Azure IoT-baserade Sakernas Internet (IoT) infrastruktur. Den länkar tooimplementation information för att konfigurera och distribuera varje komponent. Det ger också jämförelser och val mellan olika konkurrerande metoder.

Säkra hello Azure IoT-distributionen kan delas in i följande tre säkerhetsområden hello:

* **Enheternas säkerhet**: skydda hello IoT-enhet när den har distribuerats i hello vilda.
* **Anslutningssäkerhet**: säkerställer att alla data som överförs mellan hello IoT-enhet och IoT-hubben är konfidentiellt och förfalska.
* **Cloud Security**: tillhandahåller en innebär toosecure data medan den går genom och lagras i hello molnet.

![Tre säkerhetsområden][img-overview]

## <a name="secure-device-provisioning-and-authentication"></a>Säker enhetsetableringen och autentisering
hello Azure IoT Suite skyddar IoT-enheter med hello följande två metoder:

* Genom att tillhandahålla en unik identitetsnyckel (säkerhetstoken) för varje enhet som kan användas av hello enheten toocommunicate med hello IoT-hubb.
* Med hjälp av en på enhet [X.509-certifikat] [ lnk-x509] och den privata nyckeln som en innebär tooauthenticate hello enheten toohello IoT-hubb. Den här autentiseringsmetoden garanterar att hello privata nyckel på hello enheten inte är känd utanför hello enhet när som helst ger en högre säkerhetsnivå.

hello token säkerhetsmetod ger autentisering för varje anrop gjorda av hello enheten tooIoT Hub genom att associera hello symmetrisk nyckel tooeach anrop. X.509-baserad autentisering tillåter autentisering för en IoT-enhet på hello fysiska lagret som en del av hello TLS anslutningen upprättas. hello säkerhets-token-baserade metoden kan användas utan hello X.509-autentisering som är ett mindre säkert mönster. hello val mellan hello två metoder i första hand bestäms av hur säker hello enhetsautentisering måste toobe och tillgänglighet för säker lagring på hello enhet (toostore hello privata nyckel på ett säkert sätt).

## <a name="iot-hub-security-tokens"></a>Säkerhetstoken i IoT Hub
IoT-hubb använder säkerhet token tooauthenticate enheter och tjänster tooavoid, skickar nycklar i hello nätverk. Dessutom är säkerhetstoken begränsad giltighetstid och omfång. Azure IoT-SDK: er generera automatiskt token utan någon specialkonfiguration. Vissa scenarier, men kräver hello användaren toogenerate och använder säkerhetstoken direkt. Dessa inkluderar hello direkt användning av hello MQTT, AMQP och HTTP- eller hello implementering av hello tokentjänsten mönster.

Mer information om hello struktur hello säkerhetstoken och användning finns i hello följande artiklar:

* [Säkerhet token struktur][lnk-security-tokens]
* [Med hjälp av SAS-token som en enhet][lnk-sas-tokens]

Varje IoT-hubben har en [identitetsregistret] [ lnk-identity-registry] som kan vara används toocreate per enhet resurser i hello-tjänst, till exempel en kö som innehåller relä moln till enhet meddelanden och tooallow access toohello enheten riktade slutpunkter. Hej IoT-hubb identitetsregistret ger säker lagring av enheten identiteter och säkerhetsnycklar för en lösning. En eller flera enheter identiteter kan läggas till tooan Tillåt lista eller en Blockeringslista aktivera fullständig kontroll över enheten. hello innehåller följande artiklar mer information om hello struktur hello identitetsregistret och åtgärder som stöds.

[IoT-hubb stöder till exempel MQTT, AMQP och HTTP-protokoll][lnk-protocols]. Var och en av dessa protokoll använder säkerhetstoken från hello IoT enhet tooIoT hubb på olika sätt:

* AMQP: SASL vanlig AMQP anspråksbaserad säkerhet och ({policyName}@sas.root. { iothubName} hello gäller IoT hub-nivå token; {deviceId} vid enheten omfång token).
* MQTT: Anslut paket använder {deviceId} som hello {ClientId}, {IoThubhostname} / {deviceId} i hello **användarnamn** fältet och en SAS-token i hello **lösenord** fältet.
* HTTP: Ogiltig token är i hello authorization-huvud i begäran.

IoT-hubb identitetsregistret kan använda tooconfigure per enhet säkerhetsreferenser och åtkomstkontroll. Men om en IoT-lösningen har redan en betydande investering i en [anpassade identitet registret och/eller autentisering schemat][lnk-custom-auth], kan integreras i en befintlig infrastruktur med IoT-hubben genom att skapa en token tjänst.

### <a name="x509-certificate-based-device-authentication"></a>X.509 certifikatbaserad autentisering
Hej användning av en [enhetsbaserad X.509-certifikat] [ lnk-use-x509] och dess associerade privata och offentliga nyckelpar tillåter ytterligare autentisering vid hello fysiska skiktet. hello privata nyckeln lagras på ett säkert sätt i hello enheten och är inte kan upptäckas utanför hello enhet. hello X.509-certifikat innehåller information om hello-enhet, till exempel enhets-ID och andra organisatoriska information. En signatur för hello certifikat ska genereras med hjälp av hello privata nyckel.

Övergripande enheten etablering flöde:

* Associera en identifierare tooa fysisk enhet – enhetsidentitet och/eller X.509-certifikat associerade toohello enheten under enhet tillverkar eller idriftsättning.
* Skapa en motsvarande identity-transaktion i IoT-hubb – enhetsidentitet och associerade enhetsinformation i hello IoT-hubb identitetsregistret.
* Tumavtrycket för X.509-certifikatet lagras säkert i IoT-hubb identitetsregistret.

### <a name="root-certificate-on-device"></a>Rotcertifikatet på enheter
När du upprättar en säker TLS-anslutning med IoT-hubben autentiserar hello IoT-enhet IoT-hubb med ett rotcertifikat som är en del av hello enheten SDK. Hello-certifikatet för hello C klient-SDK finns hello i mappen ”\\c\\certifikat” under hello roten för hello lagringsplatsen. Även om dessa rotcertifikat är långlivade, kan de fortfarande eller upphöra att gälla. Om det går inte att uppdatera hello certifikatet på hello enheten hello enheten inte kan ansluta toosubsequently toohello IoT-hubb (eller andra Molntjänsten). Med ett rotcertifikat för innebär tooupdate hello när hello IoT-enhet har distribuerats kommer ett effektivt sätt att minska denna risk.

## <a name="securing-hello-connection"></a>Att säkra hello-anslutning
Internet-anslutning mellan hello IoT-enhet och IoT-hubben är säkrad via hello Transport Layer Security (TLS)-standard. Azure IoT stöder [TLS 1.2][lnk-tls12], TLS 1.1 och TLS 1.0 i den här ordningen. Stöd för TLS 1.0 tillhandahålls endast för bakåtkompatibilitet. Det rekommenderas toouse TLS 1.2 eftersom hello ger bäst skydd.

Azure IoT Suite stöder hello följande krypteringssviter, i den här ordningen.

| Chiffersvit | Längd |
| --- | --- |
| TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA384 (0xc028) ECDH secp384r1 (eq. FS 7680 bitar RSA) |256 |
| TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0xc027) ECDH secp256r1 (eq. FS 3072 bitar RSA) |128 |
| TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 (eq. FS 7680 bitar RSA) |256 |
| TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 (eq. FS 3072 bitar RSA) |128 |
| TLS\_RSA\_WITH\_AES\_256\_GCM\_SHA384 (0x9d) |256 |
| TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256 (0x9c) |128 |
| TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA256 (0x3d) |256 |
| TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0x3c) |128 |
| TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA (0x35) |256 |
| TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA (0x2f) |128 |
| TLS\_RSA\_WITH\_3DES\_Nederländerna\_CBC\_SHA (0xa) |112 |

## <a name="securing-hello-cloud"></a>Att säkra hello-moln
Azure IoT-hubb tillåter definition av [åtkomstprinciper kontrollen] [ lnk-protocols] för varje säkerhetsnyckel. Den använder hello följande uppsättning behörigheter toogrant åtkomst tooeach IoT-hubb slutpunkter. Behörigheter begränsa hello åtkomst tooan IoT-hubb som baseras på funktionen.

* **RegistryRead**. Ger läsbehörighet toohello identitetsregistret. Mer information finns i [identitetsregistret][lnk-identity-registry].
* **RegistryReadWrite**. Ger Läs- och skrivbehörighet toohello identitetsregistret. Mer information finns i [identitetsregistret][lnk-identity-registry].
* **ServiceConnect**. Ger åtkomst till toocloud service-riktade kommunikation och övervakning av slutpunkter. Till exempel beviljas behörighet tooback slutpunkt cloud services tooreceive meddelanden från enhet till moln, skicka meddelanden moln till enhet och hämta hello motsvarande leverans bekräftelser.
* **DeviceConnect**. Ger åtkomst till toodevice riktade slutpunkter. Till exempel ger behörighet toosend meddelanden från enhet till moln och ta emot meddelanden moln till enhet. Den här behörigheten används av enheter.

Det finns två sätt tooobtain **DeviceConnect** behörigheter med IoT-hubb med [säkerhetstoken][lnk-sas-tokens]: med identitetsnyckel en enhet eller en nyckel för delad åtkomst. Är dessutom viktigt toonote som alla funktioner som är tillgängliga från enheter exponeras avsiktligt på slutpunkter med prefixet `/devices/{deviceId}`.

[Tjänstens komponenter kan bara generera säkerhetstoken] [ lnk-service-tokens] använder delad åtkomstprinciper bevilja hello behörighet.

Azure IoT-hubb och andra tjänster som kan vara en del av lösningen hello Tillåt hantering av användare som använder hello Azure Active Directory.

Data som inhämtas av Azure IoT-hubb kan användas av en mängd olika tjänster, till exempel Azure Stream Analytics och Azure-blobblagring. Dessa tjänster tillåta åtkomst. Läs mer om dessa tjänster och alternativen nedan:

* [Azure DocumentDB][lnk-docdb]: en skalbar, -indexerat databastjänst för halvstrukturerade data som hanterar metadata för hello enheter du etablera, till exempel attribut, konfiguration och egenskaper för säkerhetsbehörighet. DocumentDB ger hög prestanda och hög genomströmning bearbetning, schema-oberoende indexering av data och en omfattande SQL-gränssnitt.
* [Azure Stream Analytics][lnk-asa]: realtid dataströmmen bearbetning i hello moln som gör att du toorapidly utveckla och distribuera en billig analytics lösning toouncover realtidsinsikter från enheter, sensorer, infrastruktur och program. hello data från den här fullständigt hanterade tjänsten kan skala tooany volym och ändå upprätthålla högt genomflöde, låg latens och återhämtning.
* [Azure Apptjänst][lnk-appservices]: en cloud platform toobuild kraftfulla webb- och mobilappar som ansluter toodata var som helst, i hello molnet eller lokalt. Skapa spännande mobilappar för iOS, Android och Windows. Integrera med programvara som en tjänst (SaaS) och enterprise-program med out box anslutningen toodozens av molnbaserade tjänster och företagsprogram. Koden i dina Favoriter språk och IDE (.NET, Node.js, PHP, Python eller Java) toobuild webbprogram och API: er snabbare än någonsin tidigare.
* [Logic Apps][lnk-logicapps]: hello Logic Apps-funktionen i Azure App Service hjälper till att integrera din IoT-lösningen tooyour befintliga line-of-business-system och automatisera arbetsflödesprocesser. Logic Apps gör det möjligt för utvecklare toodesign arbetsflöden som startar från en utlösare och sedan exekverar en följd av steg, regler och åtgärder som använder kraftfulla kopplingar toointegrate med dina affärsprocesser. Logic Apps erbjuder out box-anslutningen tooa brett spektrum av SaaS, molnbaserade, och lokala program.
* [Azure-blobblagring][lnk-blob]: tillförlitliga ekonomiska molnlagring för hello data som dina enheter skickar toohello moln.

## <a name="conclusion"></a>Slutsats
Den här artikeln innehåller översikt över implementering information för att utforma och distribuera en IoT-infrastruktur med hjälp av Azure IoT. Konfigurera varje komponent toobe säker är nyckeln på säkra hello övergripande IoT-infrastruktur. hello designalternativ som är tillgängliga i Azure IoT tillhandahåller en viss nivå flexibilitet och val. varje alternativ kan dock ha säkerhetsaspekter. Du rekommenderas att var och en av dessa alternativ att utvärderas via en utvärdering av risk/kostnad.

[img-overview]: media/iot-secure-your-deployment/overview.png

[lnk-security-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#security-token-structure
[lnk-sas-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app
[lnk-identity-registry]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-protocols]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-custom-auth]: ../articles/iot-hub/iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: http://www.itu.int/rec/T-REC-X.509-201210-I/en
[lnk-use-x509]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-tls12]: https://tools.ietf.org/html/rfc5246
[lnk-service-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#use-security-tokens-from-service-components
[lnk-docdb]: https://azure.microsoft.com/services/documentdb/
[lnk-asa]: https://azure.microsoft.com/services/stream-analytics/
[lnk-appservices]: https://azure.microsoft.com/services/app-service/
[lnk-logicapps]: https://azure.microsoft.com/services/app-service/logic/
[lnk-blob]: https://azure.microsoft.com/services/storage/
