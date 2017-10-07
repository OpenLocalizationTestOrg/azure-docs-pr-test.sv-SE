---
title: aaaLearn Azure Service Fabric-terminologi | Microsoft Docs
description: "En terminologi översikt över Service Fabric. Beskriver viktiga termer begrepp och termer som används i hello resten av hello-dokumentationen."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: chackdan;subramar
ms.assetid: 3a970679-e19e-43b3-9be8-71773f307c57
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/02/2017
ms.author: ryanwi
ms.openlocfilehash: 4781fc0527b8a58e534183249bc2759aded2730b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-terminology-overview"></a>Översikt över Service Fabric-terminologi
Service Fabric är en plattform för distribuerade system som gör det enkelt toopackage, distribuera och hantera skalbara och tillförlitliga mikrotjänster. Den här artikeln information hello-terminologi som används av Service Fabric toounderstand hello termer som används i hello-dokumentationen.

hello begrepp i det här avsnittet beskrivs också i följande Microsoft Virtual Academy videor hello: <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">viktiga begrepp</a>, <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tlkI046yC_2906218965">designläge begrepp</a>, och <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=x7CVH56yC_1406218965">körning begrepp</a>.

## <a name="infrastructure-concepts"></a>Begrepp för infrastrukturen
**Klustret**: en nätverksansluten uppsättning virtuella eller fysiska datorer som din mikrotjänster distribueras och hanteras.  Kluster kan skala toothousands datorer.

**Noden**: en dator eller virtuell dator som ingår i ett kluster kallas en nod. Varje nod har tilldelats ett nodnamn (en sträng). Noder har egenskaper som till exempel placeringsegenskaper. Varje dator eller virtuell dator har en Windows-tjänsten för automatisk start, `FabricHost.exe`, som startar vid start och startar sedan två körbara filer: `Fabric.exe` och `FabricGateway.exe`. Dessa två körbara filer som utgör hello-nod. För att testa scenarier, du kan vara värd för flera noder i en enskild dator eller virtuell dator genom att köra flera instanser av `Fabric.exe` och `FabricGateway.exe`.

## <a name="application-concepts"></a>Program-begrepp
**Programtyp**: hello namn/version tilldelad tooa samling tjänsttyper. Definierade i en `ApplicationManifest.xml` fil, inbäddat i ett paket för programkatalogen, som sedan kopieras toohello Service Fabric klustret avbildningsarkivet. Du kan sedan skapa en namngiven programmet från den här tillämpningstypen inom hello kluster.

Läs hello [programmodell](service-fabric-application-model.md) artikel för mer information.

**Programpaketet**: en diskkatalog som innehåller hello programmet typen `ApplicationManifest.xml` fil. Referenser hello servicepaket för varje typ av tjänst som utgör hello programtyp. hello-filer i hello programkatalogen paketet är kopierade tooService Fabric klustret avbildningsarkivet. Ett programpaket för en typ av e-program kan exempelvis innehålla referenser tooa kön servicepaket, en klientdel servicepaket och inget tjänstepaket för databasen.

**Programmet heter**: när ett programpaket är kopierade toohello avbildningsarkivet kan du skapa en instans av programmet hello inom hello klustret genom att ange hello programmet paketet programtyp (med dess namn/version). Varje typ av programinstans tilldelas ett URI-namn som liknar: `"fabric:/MyNamedApp"`. Du kan skapa flera namngivna program från en enda programtyp inom ett kluster. Du kan också skapa namngivna program från annan programtyper. Varje namngivet program är oberoende av varandra hanterade och en ny version.      

**Service Type**: hello namn/version tilldelade tooa service code paket, data-paket och konfigurationspaket. Definierade i en `ServiceManifest.xml` fil, inbäddat i ett paket katalog och hello paketet katalog sedan refereras av ett programpaket `ApplicationManifest.xml` fil. Inom hello kluster kan när du har skapat ett namngivet program, du skapa en namngiven tjänst från en hello programmet typen tjänsttyper. Hej service-typen `ServiceManifest.xml` filen beskriver hello-tjänsten.

Läs hello [programmodell](service-fabric-application-model.md) artikel för mer information.

Det finns två typer av tjänster:

* **Stateless:** använder en tillståndslös tjänst när hello service beständiga tillstånd lagras i en extern storage-tjänst som Azure Storage, Azure SQL Database eller Azure Cosmos DB. Använd tillståndslösa tjänsten när hello-tjänsten har ingen beständig lagring alls. Till exempel en Kalkylatorn tjänst där värden skickas toohello tjänsten, utförs en beräkning med dessa värden och resultatet returneras.
* **Tillståndskänslig:** använder en tillståndskänslig service när du vill Service Fabric toomanage Tjänststatus via dess tillförlitliga samlingar eller Reliable Actors programmeringsmodeller. Ange hur många partitioner som du vill ha toospread ditt tillstånd över (för skalbarhet) om att skapa en namngiven tjänst. Ange hur många gånger tooreplicate ditt tillstånd mellan noder (för tillförlitlighet). Varje namngivet tjänst har en enda primär replik och flera sekundära repliker. Du kan ändra din tjänsten tillstånd genom att skriva toohello primära repliken. Service Fabric replikeras sedan det här tillståndet tooall hello sekundära repliker synkronisera din tillstånd. Service Fabric identifierar automatiskt när en primär replik misslyckas och främjar en befintlig sekundär replik tooa primära replik. Service Fabric skapar sedan en ny sekundär replik.  

**Tjänstpaket**: en diskkatalog som innehåller hello service typen `ServiceManifest.xml` fil. Den här filen refererar till hello kod, statiska data och konfigurationspaket för hello service-typen. hello filer i paketet katalog hello refereras av typen hello-programmet `ApplicationManifest.xml` fil. Inget tjänstepaket kan till exempel referera toohello kod, statiska data och konfigurationspaket som utgör en databastjänst.

**Med namnet Service**: när du har skapat en namngiven programmet, du kan skapa en instans av en av dess tjänsttyper i hello kluster genom att ange hello tjänsttypen (med dess namn/version). Varje typ tjänstinstans tilldelas ett URI-namn omfång under dess namngivna program-URI. Till exempel om du skapar en ”mindatabas” med namnet tjänsten inom en ”MyNamedApp” med namnet programmet hello URI ser ut som: `"fabric:/MyNamedApp/MyDatabase"`. Du kan skapa flera namngivna tjänster i en namngiven programmet. Varje tjänsten kan ha sin egen partitionsschema och instansreplik/räknar.

**Code paketet**: en diskkatalog som innehåller hello service type körbara filer (vanligtvis EXE/DLL-filer). hello-filer i katalogen för hello kod paketet refereras av hello service type `ServiceManifest.xml` fil. När en namngiven tjänst skapas är hello kodpaketet kopierade toohello nod eller noder valda toorun hello med namnet på tjänsten. Därefter startar hello kod körs. Det finns två typer av kod paketet körbara filer:

* **Gästen körbara filer**: körbara filer som kör som-på hello värdoperativsystem (Windows eller Linux). Det vill säga dessa körbara filer och gör inte tooor länkreferensen Service Fabric runtime filer och därför använda inte några Service Fabric programmeringsmodeller. Dessa körbara filer är toouse vissa Service Fabric-funktioner såsom hello naming service för identifiering av slutpunkten. Gästen körbara filer kan inte rapportera belastning mått specifika tooeach service-instans.
* **Tjänsten värd körbara filer**: körbara filer som använder Service Fabric programmeringsmodeller genom att länka tooService Fabric runtime-filer, aktivering av Service Fabric-funktioner. En namngiven tjänstinstans slutpunkter kan registrera med Service Fabric-namngivningstjänst och kan också rapportera belastning mått.      

**Datapaketet**: en diskkatalog som innehåller hello service type statisk, skrivskyddade data (filer vanligtvis foto, ljud och video). hello filer i katalogen för hello data paketet refereras av hello service type `ServiceManifest.xml` fil. När en namngiven tjänst skapas är hello datapaketet kopierade toohello nod eller noder valda toorun hello med namnet på tjänsten.  hello koden körs och kan nu komma åt hello datafiler.

**Konfigurationspaketet**: en diskkatalog som innehåller hello service type statisk, skrivskyddad configuration-filer (vanligtvis text). hello filer i paketet för hello konfigurationskatalogen refereras av hello service type `ServiceManifest.xml` fil. När en namngiven tjänst skapas hello filer i hello konfigurationspaketet är kopierade toohello en eller flera noder valda toorun hello namngivna service. Sedan hello koden körs och kan nu komma åt hello konfigurationsfiler.

**Behållare**: som standard Service Fabric distribuerar och aktiverar tjänster som processer. Service Fabric kan också distribuera tjänster i behållaren bilder. Behållare är en virtualiseringsteknik som virtualiserar hello underliggande operativsystemet från program. Ett program och dess runtime, beroenden och systemets bibliotek körs i en behållare med fullständig, privat åtkomst toohello behållarens egna isolerade vy för konstruktionerna för operativsystemet. Service Fabric stöder Docker behållare på Linux- och Windows Server-behållare.  Mer information finns [Service Fabric och behållare](service-fabric-containers-overview.md).

**Partitionera schemat**: när du skapar en namngiven tjänst kan du ange ett partitionsschema. Tjänster med stora mängder tillstånd dela hello data på partitioner, som sprids hello tillstånd över hello klusternoder. Detta gör att din tjänsten tillstånd tooscale. Inom en partition har tillståndslösa tjänster för namngivna instanser medan tillståndskänslig namngivna tjänster ha repliker. Vanligtvis har tillståndslösa namngivna tjänster bara en partition eftersom de har inget internt tillstånd. hello partition instanser ange för tillgänglighet. Om en instans misslyckas, andra instanser fortsätta toooperate normalt och Service Fabric skapas en ny instans. Stateful som heter tjänster upprätthålla deras tillstånd i repliker och varje partition har sin egen replikuppsättningen med alla hello tillstånd som ska synkroniseras. Service Fabric skapar en replik misslyckas, en ny replik från hello befintliga replikeringar.

Läs hello [Partition Service Fabric reliable services](service-fabric-concepts-partitioning.md) artikel för mer information.

## <a name="system-services"></a>Systemtjänster
Det finns systemtjänster som skapas i alla kluster med hello plattformsfunktioner för Service Fabric.

**Naming Service**: varje Service Fabric-klustret har en Naming service som matchar namnen tooa plats i hello kluster. Du hanterar hello tjänstnamn och egenskaper, liknande tooan internet Domain Name Service (DNS) för hello-kluster. Klienter kommunicera på ett säkert sätt med en nod i hello-kluster med hjälp av hello namngivningstjänst tooresolve ett tjänstnamn och dess plats.  Att flytta program inom hello klustret till exempel på grund av toofailures resurs NLB eller hello storleksändring av hello klustret. Du kan utveckla tjänster och klienter som löser hello aktuella nätverksplats. Klienter kan hämta hello faktiska datorn IP-adress och port där den körs.

Läs [kommunicera med tjänster](service-fabric-connect-and-communicate-with-services.md) mer information om hello klienten och tjänsten kommunikation API: er som fungerar med hello Naming service.

**Image Store-tjänsten**: varje Service Fabric-klustret har en Image Store-tjänsten där distribuerade, skapas en ny version programpaket hålls. Kopiera ett program paketet toohello Image Store och sedan registrera hello programtyp som ingår i det programpaketet. Efter hello programtyp har etablerats kan du skapa en namngiven program från den. Du kan avregistrera programtyp från hello Image Store-tjänsten när alla namngivna program har tagits bort.

Läs [förstå hello ImageStoreConnectionString inställningen](service-fabric-image-store-connection-string.md) mer information om hello Image Store-tjänsten.

Läs hello [distribuera ett program](service-fabric-deploy-remove-applications.md) artikel för mer information om hur du distribuerar program toohello Image store-tjänsten.

## <a name="built-in-programming-models"></a>Inbyggda programmeringsmodeller
Det finns .NET Framework programmeringsmodeller du toobuild Service Fabric-tjänster:

**Reliable Services**: en API toobuild tillståndslösa och tillståndskänsliga tjänster. Tillståndskänslig service lagrar sina tillstånd i tillförlitliga samlingar (till exempel en ordlista eller en kö). Du kan också få tooplug i olika kommunikation stackar, till exempel webb-API och Windows Communication Foundation (WCF).

**Reliable Actors**: en API toobuild tillståndslösa och tillståndskänsliga objekt genom hello virtuella aktören programmeringsmodell. Den här modellen kan vara användbart när du har många oberoende enheter av beräkning och tillstånd. Eftersom modellerna bygger trådmodell, är det bästa tooavoid kod som anropar ut tooother aktörer eller tjänster, eftersom en enskilda aktören inte kan behandla andra inkommande begäranden tills alla utgående förfrågningar har slutförts.

Läs hello [väljer du en programmeringsmodellen för din tjänst](service-fabric-choose-framework.md) artikel för mer information.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Nästa steg
Mer om Service Fabric toolearn:

* [Översikt över Service Fabric](service-fabric-overview.md)
* [Varför en mikrotjänster hanterar toobuilding program?](service-fabric-overview-microservices.md)
* [Programscenarier](service-fabric-application-scenarios.md)

