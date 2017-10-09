
# <a name="azure-and-internet-of-things"></a>Azure och Sakernas Internet

Välkommen tooMicrosoft Azure och hello Sakernas Internet (IoT). Den här artikeln introducerar en lösningsarkitektur för IoT som beskriver hello gemensamma egenskaperna för en IoT-lösning som du kan distribuera med Azure-tjänster. IoT-lösningar kräver säker, dubbelriktad kommunikation mellan enheter, möjligen numrering i hello miljoner och en lösningens serverdel. En lösningens serverdel kan till exempel använda automatiserad, förutsägbara analytics toouncover insikter från din enhet till moln händelseströmmen.

Azure IoT Hub är en viktig byggsten vid implementering av den här IoT-lösningsarkitekturen med hjälp av Azure-tjänster. IoT Suite tillhandahåller fullständiga, slutpunkt-till-slutpunkts-implementeringar av den här arkitekturen för specifika IoT-scenarier. Exempel:

* Hej *fjärrövervaknings* lösningen kan du toomonitor hello status för enheter som till exempel försäljningsautomater.
* Hej *förutsägande Underhåll* lösningen hjälper dig att tooanticipate underhållsbehov för enheter, till exempel pumpar på avlägsna pumpstationer och tooavoid Ej schemalagda driftstopp.
* Hej *anslutna factory* lösningen hjälper dig att tooconnect och övervaka dina industriella enheter.

## <a name="iot-solution-architecture"></a>IoT-lösningsarkitektur

hello följande diagram visar en typisk IoT-lösningsarkitektur. hello diagrammet innehåller inte hello namnen på eventuella specifika Azure-tjänster, men beskrivs hello nyckelelement i en allmän IoT-lösningsarkitektur. Samla in data som de skickar tooa molngateway i den här arkitekturen IoT-enheter. Hej molngatewayen tillhandahåller hello data tillgängliga för bearbetning av andra backend-tjänster från var data levereras tooother line-of-business-program eller toohuman operatörer via en instrumentpanel eller annan presentationsenhet.

![IoT-lösningsarkitektur][img-solution-architecture]

> [!NOTE]
> En detaljerad beskrivning av IoT-arkitekturen finns i hello [Referensarkitektur för Microsoft Azure IoT][lnk-refarch].

### <a name="device-connectivity"></a>Enhetsanslutning

I den här IoT-lösningsarkitekturen skickar enheter telemetri, till exempel sensoravläsningar från en pumpstation, tooa molnslutpunkt för lagring och bearbetning. I ett scenario med förutsägande Underhåll kan hello lösningens serverdel använda hello dataström med sensor data toodetermine när en specifik pump kräver underhåll. Enheter kan också ta emot och svara toocloud till enhet meddelanden genom att läsa meddelanden från en molnslutpunkt. Till exempel i hello förutsägande Underhåll scenariot hello lösning serverdel kan skicka meddelanden tooother pumpar i hello pumpa station toobegin omdirigering flöden precis innan toostart. Den här proceduren skulle se till att hello underhållsingenjören kan komma igång så fort HEN kommer dit.

En av hello största utmaningarna för IoT-projekt är hur tooreliably och på ett säkert sätt ansluta enheter toohello lösningens serverdel. IoT-enheter har olika egenskaper som jämfört med tooother klienter, till exempel webbläsare och mobilappar. IoT-enheter:

* Är ofta inbyggda system utan en mänsklig operatör.
* Kan distribueras på avlägsna platser, där fysisk åtkomst är dyr.
* Kan endast nås via hello lösningens serverdel. Det finns inga andra sätt toointeract med hello enhet.
* Kan ha begränsade ström- och bearbetningsresurser.
* Kan ha oregelbunden, långsam eller dyr nätverksanslutning.
* Kanske måste toouse egna, anpassade eller branschspecifika programprotokoll.
* Kan skapas med en stor mängd populära maskinvaru- och programvaruplattformar.

Dessutom toohello kraven ovan behöver alla IoT-lösningar också erbjuda skalbarhet, säkerhet och tillförlitlighet. hello är resulterande uppsättningen anslutningskrav svår och tidskrävande tooimplement med traditionella teknologier, till exempel webbehållare och asynkrona meddelandeköer. Azure IoT-hubb och hello gör SDK för Azure IoT-enheter enklare tooimplement lösningar som uppfyller dessa krav.

En enhet kan kommunicera direkt med en slutpunkt för en molngateway, eller om hello enheten inte kan använda någon av hello kommunikationsprotokoll som hello gateway har stöd för molnet, den kan ansluta via en mellanliggande gateway. Till exempel hello [Azure IoT-protokollgatewayen] [ lnk-protocol-gateway] kan utföra protokollöversättning om enheter inte kan använda någon av hello-protokoll som stöds i IoT-hubb.

### <a name="data-processing-and-analytics"></a>Databearbetning och analys

I hello moln är en serverdelen för IoT-lösning där de flesta av hello databearbetning sker, till exempel filtrering och aggregering av telemetri och vidarebefordring tooother tjänster. Hej IoT-lösningens serverdel:

* Tar emot telemetri från dina enheter och bestämmer hur tooprocess och lagra dessa data. 
* Kan låta dig toosend kommandon från hello moln toospecific enhet.
* Möjliggör enhetsregistrering som gör att du tooprovision enheter och toocontrol vilka enheter som får tooconnect tooyour infrastruktur.
* Aktiverar du tootrack hello status för enheter och övervaka deras verksamhet.

Hello scenario med förutsägande Underhåll lagrar hello lösningens serverdel historiska telemetridata. hello lösningens serverdel kan använda den här toouse tooidentify datamönster som anger att Underhåll på en specifik pump.

IoT-lösningar kan inkludera automatiska feedback-slingor. En analytics-modul i hello lösningens serverdel kan exempelvis identifiera från telemetrin att hello temperaturen för en specifik enhet är över normal driftsnivå. hello-lösning kan skicka en kommandot toohello enhet, instruktion tootake korrigerande åtgärder.

### <a name="presentation-and-business-connectivity"></a>Presentation och företagsanslutningar

hello presentation och lager för anslutningen kan användarna toointeract med hello IoT-lösningen och hello-enheter. Det gör att användare tooview och analysera hello data som samlas in från sina enheter. Vyerna kan ta hello form av instrumentpaneler eller BI-rapporter som kan visa både historiska data eller nästan realtidsdata. Operatör kan exempelvis kontrollera hello status viss pumpstation och se alla varningar som skapats av hello system. Lagret tillåter också integrering av hello IoT lösningens serverdel med befintliga av branschspecifika program tootie i företagets verksamhetsprocesser eller arbetsflöden. Till exempel kan hello förutsägande underhållslösningen integreras med ett system för schemaläggning som books en tekniker toovisit en pumpstation när hello lösningen identifierar en pump i behov av underhåll.

![Instrumentpanel för IoT-lösning][img-dashboard]

[img-solution-architecture]: ./media/iot-azure-and-iot/iot-reference-architecture.png
[img-dashboard]: ./media/iot-azure-and-iot/iot-suite.png

[lnk-machinelearning]: http://azure.microsoft.com/documentation/services/machine-learning/
[Azure IoT Suite]: http://azure.microsoft.com/solutions/iot
[lnk-protocol-gateway]:  ../articles/iot-hub/iot-hub-protocol-gateway.md
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
