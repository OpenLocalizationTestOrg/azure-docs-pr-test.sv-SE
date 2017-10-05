---
title: "Läs Azure Service Fabric-terminologi | Microsoft Docs"
description: "En terminologi översikt över Service Fabric. Beskriver viktiga termer begrepp och termer som används i resten av dokumentationen."
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
ms.openlocfilehash: 42e5e651d5a3c08e829b48bb47e14458a5c65900
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="service-fabric-terminology-overview"></a>Översikt över Service Fabric-terminologi
Service Fabric är en plattform för distribuerade system som gör det enkelt att paketera, distribuera och hantera skalbara och tillförlitliga mikrotjänster. Det här avsnittet beskrivs de termer som används av Service Fabric att du förstår de termer som används i dokumentationen.

Koncept i det här avsnittet beskrivs också i följande videor för Microsoft Virtual Academy: <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965">viktiga begrepp</a>, <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tlkI046yC_2906218965">designläge begrepp</a>, och <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=x7CVH56yC_1406218965">körning begrepp</a>.

## <a name="infrastructure-concepts"></a>Begrepp för infrastrukturen
**Klustret**: en nätverksansluten uppsättning virtuella eller fysiska datorer som din mikrotjänster distribueras och hanteras.  Kluster kan skalas till tusentals datorer.

**Noden**: en dator eller virtuell dator som ingår i ett kluster kallas en nod. Varje nod har tilldelats ett nodnamn (en sträng). Noder har egenskaper som till exempel placeringsegenskaper. Varje dator eller virtuell dator har en Windows-tjänsten för automatisk start, `FabricHost.exe`, som startar vid start och startar sedan två körbara filer: `Fabric.exe` och `FabricGateway.exe`. Dessa två körbara filer som utgör noden. För att testa scenarier, du kan vara värd för flera noder i en enskild dator eller virtuell dator genom att köra flera instanser av `Fabric.exe` och `FabricGateway.exe`.

## <a name="application-concepts"></a>Program-begrepp
**Programtyp**: namn/version som tilldelats till en samling av tjänsttyper. Definierade i en `ApplicationManifest.xml` fil, inbäddat i ett paket programkatalogen, som sedan kopieras till Service Fabric-klustret image store. Du kan sedan skapa en namngiven programmet från den här tillämpningstypen i klustret.

Läs den [programmodell](service-fabric-application-model.md) artikel för mer information.

**Programpaketet**: en diskkatalog som innehåller programtypen `ApplicationManifest.xml` fil. Refererar till servicepaket för varje typ av tjänst som utgör programtypen. Filerna i katalogen program paketet kopieras till Service Fabric-klustret avbildningsarkivet. Ett programpaket för en typ av e-program kan exempelvis innehålla referenser till en kö servicepaket, en klientdel servicepaket och inget tjänstepaket för databasen.

**Programmet heter**: när ett programpaket kopieras till image store, du skapar en instans av programmet inom klustret genom att ange det programpaketet programtyp (med dess namn/version). Varje typ av programinstans tilldelas ett URI-namn som liknar: `"fabric:/MyNamedApp"`. Du kan skapa flera namngivna program från en enda programtyp inom ett kluster. Du kan också skapa namngivna program från annan programtyper. Varje namngivet program är oberoende av varandra hanterade och en ny version.      

**Service Type**: namn/version som tilldelats kod paket, data-paket och konfigurationspaket för en tjänst. Definierade i en `ServiceManifest.xml` fil, inbäddat i ett paket katalog och paketet katalog sedan refereras av ett programpaket `ApplicationManifest.xml` fil. I kluster kan när du har skapat ett namngivet program, du skapa en namngiven tjänst från en av de programtyp tjänsttyper. Service-typen `ServiceManifest.xml` filen beskriver tjänsten.

Läs den [programmodell](service-fabric-application-model.md) artikel för mer information.

Det finns två typer av tjänster:

* **Stateless:** använder en tillståndslös tjänst när tjänsten beständiga tillstånd lagras i en extern storage-tjänst som Azure Storage, Azure SQL Database eller Azure Cosmos DB. Använda en tillståndslös tjänst när tjänsten har ingen beständig lagring alls. Till exempel en Kalkylatorn tjänst där värden skickas till tjänsten, utförs en beräkning med dessa värden och resultatet returneras.
* **Tillståndskänslig:** använder en tillståndskänslig tjänst om du vill ha Service Fabric att hantera din tjänstens tillstånd via dess tillförlitliga samlingar eller Reliable Actors programmeringsmodeller. Ange hur många partitioner som du vill sprida ditt tillstånd över (för skalbarhet) om att skapa en namngiven tjänst. Ange hur många gånger för att replikera ditt tillstånd mellan noder (för tillförlitlighet). Varje namngivet tjänst har en enda primär replik och flera sekundära repliker. Du kan ändra din tjänsten tillstånd genom att skriva till den primära repliken. Service Fabric replikeras sedan det här tillståndet till de sekundära replikerna som synkronisera din tillstånd. Service Fabric identifierar automatiskt när en primär replik misslyckas och främjar en befintlig sekundär replik till en primär replik. Service Fabric skapar sedan en ny sekundär replik.  

**Tjänstpaket**: en diskkatalog som innehåller service-typen `ServiceManifest.xml` fil. Den här filen refererar till kod, statiska data och konfigurationspaket för tjänsttypen. Filerna i katalogen service paketet refereras av typen programmet `ApplicationManifest.xml` fil. Inget tjänstepaket kan till exempel referera till kod, statiska data och konfigurationspaket som utgör en databastjänst.

**Med namnet Service**: efter att ett namngivet program kan du skapa en instans av en av dess tjänsttyper i klustret genom att ange tjänsttypen (med dess namn/version). Varje typ tjänstinstans tilldelas ett URI-namn omfång under dess namngivna program-URI. Till exempel om du skapar en ”mindatabas” med namnet tjänsten inom en ”MyNamedApp” med namnet på programmet, URI: N ser ut som: `"fabric:/MyNamedApp/MyDatabase"`. Du kan skapa flera namngivna tjänster i en namngiven programmet. Varje tjänsten kan ha sin egen partitionsschema och instansreplik/räknar.

**Code paketet**: en diskkatalog som innehåller service-typen körbara filer (vanligtvis EXE/DLL-filer). Service-typen refererar till filer i katalogen kod paketet `ServiceManifest.xml` fil. När en namngiven tjänst skapas kopieras kodpaketet till noden eller noder som har markerats för att köra tjänsten. Därefter startar kod som körs. Det finns två typer av kod paketet körbara filer:

* **Gästen körbara filer**: körbara filer som kör som-på värdens operativsystem (Windows eller Linux). Som är dessa körbara filer inte länka till eller referera till Service Fabric runtime filer och har därför inte använda några Service Fabric programmeringsmodeller. Dessa körbara filer kan inte använda vissa Service Fabric-funktioner som namngivningstjänst för identifiering av slutpunkten. Gästen körbara filer kan inte rapportera belastning mätvärden som är specifika för varje service-instans.
* **Tjänsten värd körbara filer**: körbara filer som använder Service Fabric programmeringsmodeller genom att länka till Service Fabric runtime-filer, aktivering av Service Fabric-funktioner. En namngiven tjänstinstans slutpunkter kan registrera med Service Fabric-namngivningstjänst och kan också rapportera belastning mått.      

**Datapaketet**: en diskkatalog som innehåller service-typen statisk, skrivskyddade data (filer vanligtvis foto, ljud och video). Service-typen refererar till filer i katalogen data paketet `ServiceManifest.xml` fil. När en namngiven tjänst skapas kopieras datapaketet till noden eller noder som har markerats för att köra tjänsten.  Koden körs och kan nu komma åt datafilerna.

**Konfigurationspaketet**: en diskkatalog som innehåller service-typen statiska, skrivskyddad konfigurationsfiler (vanligtvis textfiler). Service-typen refererar till filer i paketet konfigurationskatalogen `ServiceManifest.xml` fil. När en namngiven tjänst skapas, kopieras filerna i konfigurationspaketet för en eller flera noder som kör tjänsten. Sedan koden körs och kan nu komma åt konfigurationsfiler.

**Behållare**: som standard Service Fabric distribuerar och aktiverar tjänster som processer. Service Fabric kan också distribuera tjänster i behållaren bilder. Behållare är en virtualiseringsteknik som virtualiserar det underliggande operativsystemet från program. Ett program och dess runtime, beroenden och systemets bibliotek kör du i en behållare med fullständig, privat åtkomst till vyn behållarens egna isolerade för konstruktionerna för operativsystemet. Service Fabric stöder Docker behållare på Linux- och Windows Server-behållare.  Mer information finns [Service Fabric och behållare](service-fabric-containers-overview.md).

**Partitionera schemat**: när du skapar en namngiven tjänst kan du ange ett partitionsschema. Tjänster med stora mängder tillstånd dela data mellan partitioner som sprids tillståndet för den klusternoder. Detta gör att din tjänsten tillstånd att skala. Inom en partition har tillståndslösa tjänster för namngivna instanser medan tillståndskänslig namngivna tjänster ha repliker. Vanligtvis har tillståndslösa namngivna tjänster bara en partition eftersom de har inget internt tillstånd. Partitionen instanser ange för tillgänglighet. Om en instans misslyckas andra instanser fortsätter att fungera normalt och Service Fabric skapas en ny instans. Stateful som heter tjänster upprätthålla deras tillstånd i repliker och varje partition har sin egen replikuppsättningen med alla tillstånd som ska synkroniseras. Service Fabric skapar en replik misslyckas, en ny replik från de befintliga replikeringarna.

Läs den [Partition Service Fabric reliable services](service-fabric-concepts-partitioning.md) artikel för mer information.

## <a name="system-services"></a>Systemtjänster
Det finns systemtjänster som skapas i varje kluster som innehåller plattformsfunktioner för Service Fabric.

**Naming Service**: varje Service Fabric-klustret har en Naming service som matchas tjänstnamn till en plats i klustret. Du hanterar tjänstnamn och egenskaper, liknande ett internet Domain Name Service (DNS) för klustret. Klienter kommunicera på ett säkert sätt med en nod i klustret med att använda för att lösa ett tjänstnamn och dess plats.  Att flytta program i klustret, till exempel på grund av fel resurs NLB eller storleksändring av klustret. Du kan utveckla tjänster och klienter som matcha klientens aktuella nätverksplats. Klienter kan hämta den faktiska datorn IP-adress och port där den körs.

Läs [kommunicera med tjänster](service-fabric-connect-and-communicate-with-services.md) mer information om kommunikationen klient- och API: er som fungerar med Naming service.

**Image Store-tjänsten**: varje Service Fabric-klustret har en Image Store-tjänsten där distribuerade, skapas en ny version programpaket hålls. Kopiera ett programpaket till Image Store och sedan registrera programtyp som ingår i det programpaketet. Efter det att programtypen har etablerats kan du skapa en namngiven program från den. Du kan avregistrera programtyp från Image Store-tjänsten när alla namngivna program har tagits bort.

Läs [förstå inställningen ImageStoreConnectionString](service-fabric-image-store-connection-string.md) för mer information om Image Store-tjänsten.

Läs den [distribuera ett program](service-fabric-deploy-remove-applications.md) artikel för mer information om hur du distribuerar program till Image store-tjänsten.

## <a name="built-in-programming-models"></a>Inbyggda programmeringsmodeller
.NET Framework programmeringsmodeller är tillgängliga för dig att skapa Service Fabric-tjänster:

**Reliable Services**: en API för att skapa tillståndslösa och tillståndskänsliga tjänster. Tillståndskänslig service lagrar sina tillstånd i tillförlitliga samlingar (till exempel en ordlista eller en kö). Du får också ansluter olika kommunikation stackar, till exempel webb-API och Windows Communication Foundation (WCF).

**Reliable Actors**: en API för att skapa tillståndslösa och tillståndskänsliga objekt genom virtuella aktören programmeringsmiljö. Den här modellen kan vara användbart när du har många oberoende enheter av beräkning och tillstånd. Eftersom den här modellen använder en bygger trådmodell, är det bäst att undvika kod som anropar till andra aktörer eller tjänster, eftersom en enskilda aktören inte kan behandla andra inkommande begäranden tills alla utgående förfrågningar har slutförts.

Läs den [väljer du en programmeringsmodellen för din tjänst](service-fabric-choose-framework.md) artikel för mer information.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nästa steg
Läs mer om Service Fabric:

* [Översikt över Service Fabric](service-fabric-overview.md)
* [Varför en mikrotjänster-lösning för att bygga program?](service-fabric-overview-microservices.md)
* [Programscenarier](service-fabric-application-scenarios.md)

