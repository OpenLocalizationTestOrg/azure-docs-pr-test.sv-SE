---
title: aaaAzure IoT-hubb ordlista | Microsoft Docs
description: "Utvecklarhandbok - en ordlista över vanliga villkoren tooAzure IoT-hubb."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 16ef29ea-a185-48c3-ba13-329325dc6716
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 217eb082c13e06df5c07543c65d498ad3e395939
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="glossary-of-iot-hub-terms"></a>Ordlista IoT-hubb
Den här artikeln innehåller några av hello vanliga termer som används i hello IoT-hubb artiklar.

## <a name="advanced-message-queueing-protocol"></a>Avancerade Message Queueing-protokoll
[Avancerade Message Queueing Protocol (AMQP)](https://www.amqp.org/) är en av hello messaging protokoll som [IoT-hubb](#iot-hub) har stöd för att kommunicera med enheter. Läs mer om hello messaging protokoll som stöds i IoT-hubb [skicka och ta emot meddelanden med IoT-hubben](iot-hub-devguide-messaging.md).

## <a name="azure-cli"></a>Azure CLI
Hej [Azure CLI](../cli-install-nodejs.md) är ett kommandoradsverktyg för plattformsoberoende, öppen källkod, shell-baserade, för att skapa och hantera resurser i Microsoft Azure. Den här versionen av hello CLI implementeras med hjälp av Node.js.

## <a name="azure-cli-20"></a>Azure CLI 2.0
Hej [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) är ett kommandoradsverktyg för plattformsoberoende, öppen källkod, shell-baserade, för att skapa och hantera resurser i Microsoft Azure. Den här förhandsversionen av hello CLI implementeras med hjälp av Python.


## <a name="azure-iot-device-sdks"></a>Azure IoT-enhet SDK
Det finns _enheten SDK_ tillgänglig för flera språk som gör att du toocreate [enhetsappar](#device-app) som interagerar med en IoT-hubb. Hej IoT-hubb självstudiekurser visar hur toouse dessa enheter SDK: er. Du kan hitta hello källkoden och ytterligare information om hello enhet SDK: er i den här GitHub [databasen](https://github.com/Azure/azure-iot-sdks).

## <a name="azure-iot-edge"></a>Azure IoT Edge
IoT-gräns kan du toowrite program som gör att gateway-anslutna enheter toocommunicate med [IoT-hubb](#iot-hub). Hej IoT kant självstudiekurser visar hur toouse den här tjänsten. Du kan hitta hello källkoden och ytterligare information om Azure IoT kant i den här GitHub [databasen](https://github.com/Azure/iot-edge).

## <a name="azure-iot-service-sdks"></a>Azure IoT service SDK
Det finns _SDK-tjänsten_ tillgänglig för flera språk som gör att du toocreate [backend-appar](#back-end-app) som interagerar med en IoT-hubb. Hej IoT-hubb självstudiekurser visar hur toouse dessa tjänsten SDK: er. Du kan hitta hello källkoden och ytterligare information om hello service SDK: er i den här GitHub [databasen](https://github.com/Azure/azure-iot-sdks).

## <a name="azure-portal"></a>Azure Portal
Hej [Microsoft Azure-portalen](https://portal.azure.com) är en central plats där du kan etablera och hantera Azure-resurser. Den organiserar dess innehåll med hjälp av _blad_. I vissa hello IoT-hubb självstudiekurser du kan bli ombedd toouse hello [klassiska Azure-portalen](https://manage.windowsazure.com).

## <a name="azure-powershell"></a>Azure PowerShell
[Azure PowerShell](/powershell/azure/overview) är en samling av cmdlets kan du använda toomanage Azure med Windows PowerShell. Du kan använda hello cmdlets toocreate, testa, distribuera och hantera lösningar och tjänster som levereras via hello Azure-plattformen.

## <a name="azure-resource-manager"></a>Azure Resource Manager
[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) kan du toowork med hello resurser i din lösning som en grupp. Du kan distribuera, uppdatera eller ta bort hello resurser i lösningen i en enda, samordnad åtgärd.

## <a name="azure-service-bus"></a>Azure Service Bus
[Service Bus](../service-bus/index.md) här moln-aktiverat kommunikation med meddelandetjänster för företag och vidarebefordrande kommunikation som hjälper dig att ansluta lokala lösningar hello moln. Vissa IoT-hubb självstudier att använda Service Bus [köer](../service-bus-messaging/service-bus-messaging-overview.md).

## <a name="azure-storage"></a>Azure Storage
[Azure Storage](../storage/common/storage-introduction.md) är en lagringslösning för molnet. Den omfattar hello Blob Storage-tjänst som du kan använda toostore Ostrukturerade objektdata. Vissa IoT-hubb självstudier använda blob storage.

## <a name="back-end-app"></a>Backend-app
Hello gäller [IoT-hubb](#iot-hub), en backend-app är en app som ansluter tooone hello service-riktade slutpunkter på en IoT-hubb. Till exempel en backend-app kan hämta [enhet till moln](#device-to-cloud)meddelanden eller hantera hello [identitetsregistret](#identity-registry). Vanligtvis en backend-app som körs i hello moln, men i många av hello självstudier hello backend-appar är konsolappar som körs på utvecklingsdatorn lokala.

## <a name="built-in-endpoints"></a>Inbyggda slutpunkter
Alla IoT-hubb innehåller en inbyggd [endpoint](iot-hub-devguide-endpoints.md) som Event Hub-kompatibel. Du kan använda någon mekanism som fungerar med Händelsehubbar tooread enhet till moln meddelanden från den här slutpunkten.

## <a name="cloud-gateway"></a>Gateway för moln
En molngateway upprättar anslutningarna för enheter som inte kan ansluta direkt för[IoT-hubb](#iot-hub). En molngateway finns i molnet hello i kontrast tooa [fältet gateway](#field-gateway) som körs lokalt tooyour enheter. Ett vanligt användningsfall för en molngateway är tooimplement översättning av protokollet för dina enheter.

## <a name="cloud-to-device"></a>Moln till enhet
Refererar toomessages som skickats från en IoT-hubb tooa ansluten enhet. Dessa meddelanden är ofta kommandon som instruerar hello enheten tootake en åtgärd. Mer information finns i [skicka och ta emot meddelanden med IoT-hubben](iot-hub-devguide-messaging.md).

## <a name="connection-string"></a>Anslutningssträng
Du använder anslutningssträngar i din app kod tooencapsulate hello information som krävs för tooconnect tooan slutpunkt. En anslutningssträng innehåller normalt sett hello adress hello slutpunkt och säkerhetsinformation men anslutningssträngen format variera mellan olika tjänster. Det finns två typer av anslutningssträngen som är associerade med hello IoT-hubb-tjänsten:
- *Enheten anslutningssträngar* aktivera enheter tooconnect toohello enheten riktade slutpunkter på en IoT-hubb.
- *IoT-hubb anslutningssträngar* aktivera backend-appar tooconnect toohello service-riktade slutpunkter på en IoT-hubb.

## <a name="custom-endpoints"></a>Anpassade slutpunkter
Du kan skapa anpassade [slutpunkter](iot-hub-devguide-endpoints.md) på en IoT-hubb toodeliver meddelanden som skickas av en [routningsregel](#routing-rules). Anpassade slutpunkter ansluter direkt tooan Event hub, Service Bus-kö eller ett Service Bus-ämne.

## <a name="custom-gateway"></a>Anpassade gateway
Gör att en gateway-anslutning för enheter som inte kan ansluta direkt för[IoT-hubb](#iot-hub). Du kan använda [Azure IoT kant](#azure-iot-edge) toobuild anpassade gateways som implementerar egen kod toohandle meddelanden anpassade protokollet konverteringar och annan bearbetning på hello kant.

## <a name="data-point-message"></a>Datapunkt meddelande
En datapunkt meddelandet är ett [enhet till moln](#device-to-cloud) meddelande som innehåller [telemetri](#telemetry) data, till exempel vindhastighet eller temperatur.

## <a name="desired-configuration"></a>Önskad konfigurationshantering
Hello kontexten för en [enheten dubbla](iot-hub-devguide-device-twins.md), önskad konfiguration refererar toohello fullständig uppsättning egenskaper och metadata i hello enheten dubbla som ska synkroniseras med hello enhet.

## <a name="desired-properties"></a>Egenskaper
Hello kontexten för en [enheten dubbla](iot-hub-devguide-device-twins.md), önskade egenskaper är ett delavsnitt i hello enheten dubbla som används med [rapporterade egenskaper](#reported-properties) toosynchronize enhetskonfigurationen eller villkor. Egenskaper kan endast anges en [backend-app](#back-end-app) och följas av hello [enhetsapp](#device-app).

## <a name="device-to-cloud"></a>Enhet till moln
Refererar toomessages som skickats från en ansluten enhet för[IoT-hubb](#iot-hub). Dessa meddelanden kan vara [datapunkt](#data-point-message) eller [interaktiva](#interactive-message) meddelanden. Mer information finns i [skicka och ta emot meddelanden med IoT-hubben](iot-hub-devguide-messaging.md).

## <a name="device"></a>Enhet
En enhet är vanligtvis en småskaliga, fristående datorenhet som kan samla in data eller kontrollera andra enheter hello gäller IoT är. En enhet kan exempelvis vara en miljön övervakning enhet eller en domänkontrollant för hello vattning och ventilation system i en växthusgaser. Hej [enhet katalogen](https://catalog.azureiotsuite.com/) innehåller en lista över maskinvara enheter certifierade toowork med [IoT-hubb](#iot-hub).

## <a name="device-app"></a>Enhetsapp
En enhet appen körs på din [enhet](#device) och hanterar hello kommunikation med din [IoT-hubb](#iot-hub). Normalt använder du något av hello [Azure IoT-enhet SDK](#azure-iot-device-sdks) när du implementerar en enhetsapp. I många av hello IoT självstudier, använder du en [simulerade enheten](#simulated-device) i informationssyfte.

## <a name="device-condition"></a>Enhetens tillstånd
Refererar toodevice tillståndsinformation, till exempel hello anslutningsmetod som används, som rapporteras av en [enhetsapp](#device-app). [Enhetsappar](#device-app) kan också rapportera deras funktioner. Du kan fråga efter villkor och kapaciteten information med hjälp av enheten twins.

## <a name="device-data"></a>Data på enheten
Enhetsdata refererar toohello per enhet data som lagras i hello IoT-hubb [identitetsregistret](#identity-registry). Det är möjligt tooimport och exportera dessa data.

## <a name="device-explorer"></a>Enheten explorer
Hej [enheten explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) är ett verktyg som körs på Windows och aktiverar du toomanage dina enheter i hello [identitetsregistret](#identity-registry).hello verktyget kan också skicka och ta emot meddelanden tooyour enheter.

## <a name="device-identities-rest-api"></a>REST API för enhetsidentiteter
Hej [enheten identiteter REST API](https://docs.microsoft.com/rest/api/iothub/iothubresource) kan dina enheter registrerade i hello toomanage [identitetsregistret](#identity-registry) med hjälp av REST-API. Normalt bör du använda en högre nivå hello [SDK-tjänsten](#azure-iot-service-sdks) enligt hello IoT-hubb självstudier.

## <a name="device-identity"></a>Enhetsidentitet
hello enhetsidentitet är hello Unik identifierare som tilldelas tooevery enhet registreras i hello [identitetsregistret](#identity-registry).

## <a name="device-management"></a>Enhetshantering
Enhetshantering omfattar hello hela livscykeln som är associerade med hantering av hello enheter i IoT-lösningen inklusive planering, etablering, konfigurera, övervaka och borttagning.

## <a name="device-management-patterns"></a>Enhetshanteringsmönster
[IoT-hubb](#iot-hub) aktiverar vanliga device management mönster inklusive startas om, utföra fabriksåterställning och utför uppdateringar av inbyggd programvara på dina enheter.

## <a name="device-messaging-rest-api"></a>REST API för enhetsmeddelanden
Du kan använda hello [Device Messaging REST API](https://docs.microsoft.com/rest/api/iothub/httpruntime) från en enhet toosend enhet till moln meddelanden tooan IoT-hubb och ta emot [moln till enhet](#cloud-to-device) meddelanden från en IoT-hubb. Normalt bör du använda en högre nivå hello [enheten SDK](#azure-iot-device-sdks) enligt hello IoT-hubb självstudier.

## <a name="device-provisioning"></a>Enhetsetableringen
Enhetsetableringen är hello process för att lägga till hello inledande [enhetsdata](#device-data) toohello lagras i din lösning. tooenable en ny enhet tooconnect tooyour hubb, måste du lägga till enheten ID och nycklar toohello IoT-hubb [identitetsregistret](#identity-registry). Som en del av hello etableringsprocessen, kanske du måste tooinitialize enhetsspecifika data i andra lösning Arkiv.

## <a name="device-twin"></a>Enhetstvilling
En [enheten dubbla](iot-hub-devguide-device-twins.md) är JSON-dokument som lagrar tillstånd enhetsinformation som metadata, konfigurationer och villkor. [IoT-hubb](#iot-hub) kvarstår en enhet dubbla för varje enhet som du etablerar i din IoT-hubb. Enheten twins aktivera toosynchronize [villkor som enheten](#device-condition) och konfigurationer mellan hello enheten och hello lösning serverdel. Du kan fråga enheten twins toolocate specifika enheter och fråga hello status för långvariga åtgärder.

## <a name="device-twin-queries"></a>Enheten dubbla frågor
[Enheten dubbla frågor](iot-hub-devguide-query-language.md) använda hello IoT-hubb för SQL-liknande språk tooretrieve frågeinformation från din enhet twins. Du kan använda samma IoT-hubb fråga tooretrieve språkinformation om hello [jobb](#job) körs i din IoT-hubb.

## <a name="device-twin-rest-api"></a>Enheten dubbla REST API
Du kan använda hello [enheten dubbla REST API](https://docs.microsoft.com/rest/api/iothub/devicetwinapi) från hello lösning serverdelsnätverk toomanage twins din enhet. hello API kan du tooretrieve och uppdatera [enheten dubbla](#device-twin) egenskaper och anropa [direkt metoder](#direct-method). Normalt bör du använda en högre nivå hello [SDK-tjänsten](#azure-iot-service-sdks) enligt hello IoT-hubb självstudier.

## <a name="device-twin-synchronization"></a>Dubbla synkronisering
Synkronisering av dubbla använder hello [önskade egenskaper](#desired-properties) i din enhet twins tooconfigure dina enheter och hämta [rapporterade egenskaper](#reported-properties) från dina enheter toostore i hello enheten dubbla.

## <a name="direct-method"></a>Direkt metod
En [direkt metod](iot-hub-devguide-direct-methods.md) är ett sätt för du tootrigger en metod tooexecute på en enhet genom att anropa API för din IoT-hubb.

## <a name="endpoint"></a>Slutpunkt
En IoT-hubb visar flera [slutpunkter](iot-hub-devguide-endpoints.md) som gör att din apps tooconnect toohello IoT-hubb. Finns enheten riktade slutpunkter som gör att enheter tooperform åtgärder, till exempel skicka [enhet till moln](#device-to-cloud) meddelanden och ta emot [moln till enhet](#cloud-to-device) meddelanden. Finns service-riktade management slutpunkter som möjliggör [backend-appar](#back-end-app) tooperform åtgärder som [enhetsidentitet](#device-identity) hantering och dubbla enhetshantering. Det finns service-riktade [inbyggda slutpunkter](#built-in-endpoints) för att läsa meddelanden från enhet till moln. Du kan skapa [anpassade slutpunkter](#custom-endpoints) tooreceive enhet till moln meddelanden som skickas av en [routningsregel](#routing-rules).

## <a name="event-hubs-service"></a>Tjänsten för Event Hubs
[Händelsehubbar](../event-hubs/event-hubs-what-is-event-hubs.md) är en mycket skalbar tjänst för dataingång som kan mata in miljontals händelser per sekund. hello-tjänsten kan du tooprocess och analysera hello stora mängder data som produceras av dina anslutna enheter och program. En jämförelse med hello IoT-hubb-tjänsten finns [jämförelse av Azure IoT Hub och Azure Event Hubs](iot-hub-compare-event-hubs.md).

## <a name="event-hub-compatible-endpoint"></a>Event Hub-kompatibel slutpunkt
tooread [enhet till moln](#device-to-cloud) meddelanden som skickas tooyour IoT-hubb, kan du ansluta tooan slutpunkt på din hubb och använda alla Event Hub-kompatibel metoden tooread dessa meddelanden. Hubb-kompatibel händelsemetoder är med hjälp av hello [Event Hubs SDK](../event-hubs/event-hubs-programming-guide.md) och [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md).

## <a name="field-gateway"></a>Fältet gateway
En gateway för fältet upprättar anslutningarna för enheter som inte kan ansluta direkt för[IoT-hubb](#iot-hub) och används normalt lokalt med dina enheter. Mer information finns i [vad är Azure IoT Hub?](iot-hub-what-is-iot-hub.md)

## <a name="free-account"></a>Kostnadsfritt konto
Du kan skapa en [kostnadsfritt Azure-konto](https://azure.microsoft.com/pricing/free-trial/) toocomplete hello IoT-hubb självstudier och experimentera med hello IoT-hubb-tjänsten (och andra Azure-tjänster).

## <a name="gateway"></a>Gateway
Gör att en gateway-anslutning för enheter som inte kan ansluta direkt för[IoT-hubb](#iot-hub). Se även [fältet Gateway](#field-gateway), [molnet Gateway](#cloud-gateway), och [anpassade Gateway](#custom-gateway).

## <a name="identity-registry"></a>Identitetsregistret
Hej [identitetsregistret](iot-hub-devguide-identity-registry.md) är hello inbyggda komponenten i en IoT-hubb som lagrar information om enskilda enheter som hello tillåtet tooconnect tooan IoT-hubb.

## <a name="interactive-message"></a>Interaktiva meddelande
En interaktiv meddelandet är ett [moln till enhet](#cloud-to-device) meddelanden som utlöser en omedelbar åtgärd i hello lösningens serverdel. Exempelvis kan en enhet skickar ett larm om fel som automatiskt ska loggas i tooa CRM-systemet.

## <a name="iot-hub"></a>IoT Hub
IoT-Hubbnamnrymd är en helt hanterad Azure-tjänst som möjliggör tillförlitlig och säker dubbelriktad kommunikation mellan miljoner enheter och en lösning tillbaka sluta. Mer information finns i [vad är Azure IoT Hub?](iot-hub-what-is-iot-hub.md) Med hjälp av din [Azure-prenumeration](#subscription), kan du skapa IoT hubs toohandle din IoT messaging arbetsbelastningar.

## <a name="iot-hub-metrics"></a>IoT-hubb mått
[IoT-hubb mått](iot-hub-metrics.md) ger data om hello tillstånd hello IoT-hubbar i din [Azure-prenumeration](#subscription). IoT-hubb mått aktivera du tooassess hello övergripande hälsa för hello-tjänsten och hello enheter anslutna tooit. IoT-hubb mått kan hjälpa dig att se vad som händer med din IoT-hubb och undersöka grundorsaken problem utan att behöva toocontact Azure-supporten.

## <a name="iot-hub-query-language"></a>IoT-hubb frågespråket
Hej [IoT-hubb frågespråket](iot-hub-devguide-query-language.md) är en SQL-liknande språk som du kan använda tooquery din [jobb](#job) och enheten twins.

## <a name="iot-hub-resource-provider-rest-api"></a>IoT-hubb Resursprovidern REST-API
Du kan använda hello [IoT Hub Resource Provider REST API](https://docs.microsoft.com/rest/api/iothub/resourceprovider/iot-hub-resource-provider-rest) toomanage hello IoT-hubbar i din [Azure-prenumeration](#subscription) utföra åtgärder som att skapa, uppdatera och ta bort hubs.

## <a name="iot-suite"></a>IoT Suite
Azure IoT Suite-paket tillsammans flera Azure-tjänster med förkonfigurerade lösningar. Dessa förkonfigurerade lösningar aktivera tooget igång snabbt med slutpunkt till slutpunkt-implementeringar av vanliga IoT-scenarier. Mer information finns i [vad är Azure IoT Suite?](../iot-suite/iot-suite-overview.md)

## <a name="iothub-explorer"></a>iothub explorer
Hej [iothub explorer](https://github.com/azure/iothub-explorer) är en plattformsoberoende, kommandorads-verktyget. hello-verktyget kan du toomanage dina enheter i hello [identitetsregistret](#identity-registry), skicka och motta meddelanden och filer från dina enheter och övervaka dina IoT hub-åtgärder.

## <a name="job"></a>Jobb
Din lösningens serverdel kan använda [jobb](iot-hub-devguide-jobs.md) tooschedule och spåra aktiviteter på en uppsättning enheter som registrerats med IoT-hubben. Aktiviteter omfattar uppdatera enheten dubbla [önskade egenskaper](#desired-properties), uppdaterar enheten dubbla [taggar](#tags), och anropar [direkt metoder](#direct-method). [IoT-hubb](#iot-hub) använder också jobb för[importera tooand export](iot-hub-devguide-identity-registry.md#import-and-export-device-identities) från hello [identitetsregistret](#identity-registry).

## <a name="jobs-rest-api"></a>Jobb REST-API
Hej [jobb REST API](https://docs.microsoft.com/rest/api/iothub/jobapi) kan du toomanage [jobb](#job) körs i din IoT-hubb.

## <a name="module"></a>Modul
I [Azure IoT kant](iot-hub-linux-iot-edge-get-started.md), [modulen](iot-hub-linux-iot-edge-get-started.md) är en komponent som utför en viss uppgift. Uppgifter kan innehålla vill föra in ett meddelande från en enhet, omvandla ett meddelande eller skicka ett meddelande tooan IoT-hubb. En koordinator ansvarar för att vidarebefordra meddelanden mellan moduler. Azure IoT-Edge innehåller en uppsättning exempel moduler. Du kan också skapa egna anpassade moduler.

## <a name="mqtt"></a>MQTT
[MQTT](http://mqtt.org/) är en av hello messaging protokoll som [IoT-hubb](#iot-hub) har stöd för att kommunicera med enheter. Läs mer om hello messaging protokoll som stöds i IoT-hubb [skicka och ta emot meddelanden med IoT-hubben](iot-hub-devguide-messaging.md).

## <a name="operations-monitoring"></a>Övervakning av åtgärder
IoT-hubb [operations övervakning](iot-hub-operations-monitoring.md) kan du toomonitor hello status för åtgärder för din IoT-hubb i realtid. [IoT-hubb](#iot-hub) spårar händelser över flera kategorier av åtgärder. Du kan välja att skicka händelser från en eller flera kategorier tooan IoT-hubb slutpunkt för bearbetning. Du kan övervaka hello data för fel eller ställa in mer komplexa bearbetning baserat på datamönster.

## <a name="physical-device"></a>Fysisk enhet
En fysisk enhet är en verklig enhet, till exempel en hallon Pi som ansluter tooan IoT-hubb. För enkelhetens skull använder många av hello IoT-hubb självstudier [simulerade enheter](#simulated-device) tooenable du toorun exempel på den lokala datorn.

## <a name="primary-and-secondary-keys"></a>Primära och sekundära nycklarna
När du ansluter tooa enhet-riktade eller service-riktade slutpunkt på en IoT-hubb din [anslutningssträngen](#connection-string) innehåller viktiga toogrant du åtkomst till. När du lägger till en enhet toohello [identitetsregistret](#identity-registry) eller Lägg till en [delad åtkomstprincip](#shared-access-policy) tooyour hubben, hello tjänsten skapar en primär och en sekundär nyckel. Med två nycklar kan du tooroll över från en nyckel tooanother när du uppdaterar en nyckel utan att förlora åtkomst toohello IoT-hubb.

## <a name="protocol-gateway"></a>Protocol-gateway
En protocol-gateway distribueras vanligtvis i hello molnet och tillhandahåller protokollet översättning för enheter som ansluter för[IoT-hubb](#iot-hub). Mer information finns i [vad är Azure IoT Hub?](iot-hub-what-is-iot-hub.md)

## <a name="quotas-and-throttling"></a>Kvoter och begränsningar
Det finns olika [kvoter](iot-hub-devguide-quotas-throttling.md) som gäller tooyour användning av [IoT-hubb](#iot-hub), många av hello kvoter varierar utifrån hello nivå i hello IoT-hubb. [IoT-hubb](#iot-hub) gäller även [begränsar](iot-hub-devguide-quotas-throttling.md) tooyour användning av hello service vid körning.

## <a name="reported-configuration"></a>Rapporterat konfiguration
Hello kontexten för en [enheten dubbla](iot-hub-devguide-device-twins.md), rapporterade konfiguration refererar toohello fullständig uppsättning egenskaper och metadata i hello enheten dubbla som ska rapporteras toohello lösningens serverdel.

## <a name="reported-properties"></a>Rapporterat egenskaper
Hello kontexten för en [enheten dubbla](iot-hub-devguide-device-twins.md), rapporterade egenskaper är delar av hello enheten dubbla används med [önskade egenskaper](#desired-properties) toosynchronize enhetskonfigurationen eller villkor. Rapporterade egenskaper kan endast anges av hello [enhetsapp](#device-app) och kan läsa och efterfrågas av en [backend-app](#back-end-app).

## <a name="resource-group"></a>Resursgrupp
[Azure Resource Manager](#azure-resource-manager) använder resurs grupper toogroup relaterade resurser tillsammans. Du kan använda en resurs grupp tooperform åtgärder på alla hello resurser på hello grupp samtidigt.

## <a name="retry-policy"></a>Försök princip
Du använder en försök princip toohandle [tillfälliga fel](https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx) när du ansluter tooa Molntjänsten.

## <a name="routing-rules"></a>Regler för Routning
Du konfigurerar [routningsregler](iot-hub-devguide-messages-read-custom.md) i din IoT-hubb tooroute meddelanden från enhet till moln tooa [inbyggd slutpunkt](#built-in-endpoints) eller för[anpassade slutpunkter](#custom-endpoints) för bearbetning av lösningens serverdel slut.

## <a name="sasl-plain"></a>SASL OFORMATERAD
SASL OFORMATERAD är ett protokoll som hello [AMQP](#advanced-message-queue-protocol) protokoll använder tootransfer säkerhetstoken.

## <a name="shared-access-signature"></a>Signatur för delad åtkomst
Delad åtkomst signaturer (SAS) är en autentiseringsmekanism baserat på SHA-256 säker hashvärden eller URI: er. SAS-autentisering har två komponenter: en _delad åtkomstprincip_ och en _signatur för delad åtkomst_ (kallas ofta för en token). En enhet använder SAS-tooauthenticate med en IoT-hubb. [Backend-appar](#back-end-app) också använda SAS tooauthenticate med hello service-riktade slutpunkter på en IoT-hubb. Normalt innehåller hello SAS-token i hello [anslutningssträngen](#connection-string) att en app använder tooestablish anslutning tooan IoT-hubb.

## <a name="shared-access-policy"></a>Princip för delad åtkomst
En princip för delad åtkomst definierar hello behörigheterna tooanyone som har en giltig [primära och sekundära nycklarna](#primary-and-secondary-keys) som är associerade med principen. Du kan hantera hello delade åtkomstprinciper och nycklar för din hubb i hello [portal](#azure-portal).

## <a name="simulated-device"></a>Simulerad enhet
För enkelhetens skull simuleras många hello IoT-hubb självstudier använder enheter tooenable du toorun exempel på den lokala datorn. Däremot ett [fysisk enhet](#physical-device) är en verklig enhet, till exempel en hallon Pi som ansluter tooan IoT-hubb.

## <a name="solution"></a>Lösning
En _lösning_ kan referera tooa Visual Studio-lösning som innehåller ett eller flera projekt. En _lösning_ tooan IoT-lösningen som innehåller element som enheter, kan även gå [enhetsappar](#device-app), en IoT-hubb, andra Azure-tjänster och [backend-appar](#back-end-app).

## <a name="subscription"></a>Prenumeration
En Azure-prenumeration är om fakturering sker. Varje Azure-resurs som du skapar eller du använder Azure-tjänsten är associerad med en enda prenumeration. Det gäller även många kvoter på hello nivå för en prenumeration.

## <a name="system-properties"></a>Systemegenskaper
Hello kontexten för en [enheten dubbla](iot-hub-devguide-device-twins.md), Systemegenskaper är skrivskyddad och inkluderar information om användning av hello-enhet, till exempel senaste aktivitet tid och anslutningen tillstånd.

## <a name="tags"></a>Taggar
Hello kontexten för en [enheten dubbla](iot-hub-devguide-device-twins.md), taggar är enhetens metadata lagras och hämtas med hello lösningens serverdel i hello form av ett JSON-dokument. Taggar är inte synliga tooapps på en enhet.

## <a name="telemetry"></a>Telemetri
Enheter samla in telemetridata, till exempel vindhastighet eller temperatur, och använda [datapunkt meddelanden](#data-point-messages) toosend hello telemetri tooan IoT-hubb.

## <a name="token-service"></a>Tokentjänsten
Du kan använda en säkerhetstokentjänst tooimplement autentiseringsmetod för dina enheter. Den använder en IoT-hubb [delad åtkomstprincip](#shared-access-policy) med **DeviceConnect** behörigheter toocreate *enheten omfång* token. Dessa token kan en enhet tooconnect tooyour IoT-hubb. En enhet använder en anpassad autentisering mekanism tooauthenticate med hello tokentjänsten. Om hello enhet autentiseras har utfärdar hello tokentjänsten en SAS-token för hello enheten toouse tooaccess din IoT-hubb.

## <a name="x509-client-certificate"></a>X.509-klientcertifikat
En enhet kan använda ett X.509-certifikat tooauthenticate med [IoT-hubb](#iot-hub). Med hjälp av ett X.509-certifikat är en alternativ toousing en [SAS-token](#shared-access-signature).
