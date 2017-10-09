---
title: aaaDifferences mellan Cloud Services och Service Fabric | Microsoft Docs
description: "En översikt för att migrera program från molntjänster tooService Fabric."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 0b87b1d3-88ad-4658-a465-9f05a3376dee
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: bbc5ef4fe0fe1b0da55454cb6b766925030198fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="learn-about-hello-differences-between-cloud-services-and-service-fabric-before-migrating-applications"></a>Lär dig mer om hello skillnaderna mellan molntjänster och Service Fabric innan du migrerar program.
Microsoft Azure Service Fabric är hello nästa generations program molnplattform för mycket skalbar, tillförlitlig hög distribuerade program. Det har många nya funktioner för paketering, distribution, uppgradera och hantera distribuerade molnprogram. 

Det här är ett inledande guiden toomigrating program från molntjänster tooService Fabric. Den fokuserar huvudsakligen på arkitektur och utforma skillnaderna mellan molntjänster och Service Fabric.

## <a name="applications-and-infrastructure"></a>Program och infrastruktur
En grundläggande skillnaden mellan Cloud Services och Service Fabric är hello förhållandet mellan virtuella datorer, arbetsbelastningar och program. En arbetsbelastning här definieras som hello koden du skriver tooperform en viss uppgift eller tillhandahålla en tjänst.

* **Molntjänster är hur du distribuerar program som virtuella datorer.** hello koden du skriver är tätt kopplade tooa VM-instans, till exempel en webbplats eller en Worker-rollen. toodeploy en arbetsbelastning i Cloud Services är toodeploy en eller flera Virtuella instanser att köra hello-arbetsbelastningar. Det finns ingen åtskillnad mellan program och virtuella datorer och så det finns ingen formell definition av ett program. Ett program kan betraktas som en uppsättning webb- eller Arbetsrollen instanser i en distribution för molntjänster eller en hel Cloud Services-distribution. I det här exemplet visas ett program som en uppsättning rollinstanser.

![Tjänster för molnprogram och topologi][1]

* **Service Fabric handlar om hur du distribuerar program tooexisting virtuella datorer eller datorer som kör Service Fabric på Windows- eller Linux.** hello-tjänster som du skriver är helt frikopplad från hello underliggande infrastruktur som representeras direkt av hello Service Fabric-programplattformen så att ett program kan vara distribuerade toomultiple miljöer. En arbetsbelastning i Service Fabric kallas ”tjänst” och en eller flera tjänster grupperas i ett formellt definierade program som körs på programmet hello Service Fabric-plattformen. Flera program kan vara distribuerade tooa Service Fabric-kluster.

![Service Fabric-program och -topologi][2]

Service Fabric själva är plattform programnivå som körs på Windows- eller Linux, Cloud Services är ett system för att distribuera Azure-hanterade virtuella datorer med arbetsbelastningar som är anslutna.
hello har Service Fabric-program många fördelar:

* Snabb distribution gånger. Skapa VM-instanser kan ta lång tid. I Service Fabric distribuerade virtuella datorer bara när tooform ett kluster som är värd för hello Service Fabric-programplattformen. Från den punkten på kan programpaket vara distribuerade toohello mycket snabbt.
* Hög densitet värd. I Cloud Services, en Worker-rollen virtuell dator är värd för en arbetsbelastning. I Service Fabric är program separat från hello virtuella datorer som kör dem, vilket innebär att du kan distribuera ett stort antal program tooa litet antal virtuella datorer som kan sänka övergripande kostnader för större distributioner.
* hello Service Fabric-plattformen kan köras var som helst som har Windows Server- eller Linux-datorer, oavsett om Azure eller lokalt. hello-plattformen ger ett Abstraktionslager över hello underliggande infrastruktur så att programmet kan köras på olika miljöer. 
* Hantering av distribuerade program. Service Fabric är en plattform som värd för distribuerade program, men även hjälper dig att hantera livscykeln oberoende av hello som värd för Virtuella eller datorn livscykel.

## <a name="application-architecture"></a>Programarkitektur
hello arkitekturen för en Cloud Services-program innehåller vanligtvis ett stort antal externa tjänstberoenden som Service Bus, Azure Table och Blob Storage, SQL, Redis och andra toomanage hello tillstånd och data för ett program och kommunikation mellan webbtjänst - och arbetsroller i en distribution med molntjänster. Ett exempel på en fullständig Cloud Services-program kan se ut så här:  

![Cloud Services-arkitektur][9]

Service Fabric-program kan också välja toouse hello samma externa tjänster i en fullständig ansökan. I det här exemplet molntjänster arkitektur hello enklaste migreringsvägen från molntjänster tooService Fabric är tooreplace distribution endast för hello molntjänster med ett Service Fabric-program att hålla hello övergripande arkitektur hello samma. hello webb- och arbetsroller kan vara port tooService Fabric tillståndslösa tjänster med minimala kodändringar.

![Arkitektur för Service Fabric efter enkel migrering][10]

I det här skedet hello-system bör fortsätta toowork hello samma som tidigare. Dra fördel av Service Fabric tillståndskänslig funktioner, externa tillstånd lagrar kan vara internalized som stateful services tillämpliga. Detta är mer komplicerat än en enkel migrering av webb- och arbetsroller tooService Fabric tillståndslösa tjänster, eftersom den kräver att skriva anpassade tjänster som erbjuder likvärdiga funktioner tooyour program som hello externa tjänster gjorde före. hello fördelar gör detta: 

* Ta bort externa beroenden 
* Enhetlig hello distribution, hantering och uppgradera modeller. 

Ett exempel resulterande arkitektur internalizing dessa tjänster kan se ut så här:

![Arkitektur för Service Fabric efter fullständig migrering][11]

## <a name="communication-and-workflow"></a>Kommunikation och arbetsflöde
De flesta Molntjänsten program består av fler än en nivå. På liknande sätt kan består ett Service Fabric-program av mer än en tjänst (vanligtvis många tjänster). Två vanliga kommunikation modeller är direkt kommunikation och via en extern beständig lagring.

### <a name="direct-communication"></a>Direkt kommunikation
Med direkt kommunikation kommunicera nivåer direkt med slutpunkten som exponeras av varje nivå. I tillståndslös miljöer som molntjänster, detta innebär att välja en instans av en VM-roll, antingen slumpmässigt eller resursallokering toobalance belastning och anslutande tooits endpoint direkt.

![Cloud Services direkt kommunikation][5]

 Direkt kommunikation är en gemensam kommunikationsmodell i Service Fabric. hello viktigaste skillnaden mellan Service Fabric och Cloud Services är i molntjänster du ansluter tooa VM, medan du Anslut tooa service i Service Fabric. Detta är en viktig skillnad några skäl:

* Tjänster i Service Fabric är inte bunden toohello virtuella datorer som är värdar för dem. tjänster kan flytta runt i hello kluster och i själva verket är förväntade toomove runt av olika skäl: resurs belastningsutjämning, redundans, program och infrastrukturen uppgraderingar och placering eller läsa in villkor. Detta innebär en tjänstinstans adress kan ändras när som helst. 
* En virtuell dator i Service Fabric kan vara värd för flera tjänster med unika slutpunkter.

Service Fabric innehåller en service discovery mekanism som kallas hello Naming Service, som kan vara används tooresolve slutpunkts-adresser av tjänster. 

![Service Fabric direkt kommunikation][6]

### <a name="queues"></a>Köer
En gemensam mekanism för kommunikation mellan nivåer i tillståndslös miljöer, till exempel molntjänster är toouse en extern lagringsenhet kön toodurably lagra arbetsuppgifter från ett lager tooanother. Ett vanligt scenario är en webbnivå som skickar jobb tooan Azure Queue eller Service Bus där Arbetsrollen instanser kan status Created och bearbeta hello jobb.

![Cloud Services kön kommunikation][7]

hello samma kommunikation modell kan användas i Service Fabric. Detta kan vara användbart när du migrerar en befintlig molntjänster programmet tooService Fabric. 

![Service Fabric direkt kommunikation][8]

## <a name="next-steps"></a>Nästa steg
hello enklaste migreringsvägen från molntjänster tooService Fabric är tooreplace endast hello molntjänster distribution med ett Service Fabric-program att hålla hello övergripande arkitektur i ditt program ungefär hello samma. hello följande artikel innehåller en guide toohelp konvertera en webb- eller Arbetsrollen tooa tillståndslösa Service Fabric-tjänsten.

* [Enkel migrering: konvertera en webb- eller Arbetsrollen tooa tillståndslösa Service Fabric-tjänsten](service-fabric-cloud-services-migration-worker-role-stateless-service.md)

<!--Image references-->
[1]: ./media/service-fabric-cloud-services-migration-differences/topology-cloud-services.png
[2]: ./media/service-fabric-cloud-services-migration-differences/topology-service-fabric.png
[5]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-direct.png
[6]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-direct.png
[7]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-queues.png
[8]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-queues.png
[9]: ./media/service-fabric-cloud-services-migration-differences/cloud-services-architecture.png
[10]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-simple.png
[11]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-full.png
