---
title: "aaaOverview av Service Fabric och behållare | Microsoft Docs"
description: "En översikt över Service Fabric och hello användning av behållare toodeploy mikrotjänster program. Den här artikeln innehåller en översikt över hur behållare kan användas och hello tillgängliga funktioner i Service Fabric."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: c98b3fcb-c992-4dd9-b67d-2598a9bf8aab
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 5/16/2017
ms.author: msfussell
ms.openlocfilehash: fce94c4b476351c90f23f706aab8bc17319cce22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-and-containers"></a>Service Fabric och behållare
> [!NOTE]
> Den här funktionen är i förhandsvisning för Linux.  Distribuera behållare tooa Service Fabric-kluster i Windows 10 inte stöds ännu (kommer snart). 
>   

## <a name="introduction"></a>Introduktion
Azure Service Fabric är en [orchestrator](service-fabric-cluster-resource-manager-introduction.md) av tjänster i ett kluster på datorer med års användnings- och optimering i massiv skala tjänster på Microsoft. Tjänster kan utvecklas på många sätt, från att använda hello [Service Fabric programmeringsmodeller](service-fabric-choose-framework.md) toodeploying [gäst körbara filer](service-fabric-deploy-existing-app.md). Som standard Service Fabric distribuerar och aktiverar de här tjänsterna som processer. Processer ange hello snabbaste aktivering och högsta densitet användning av hello resurser i ett kluster. Service Fabric kan också distribuera tjänster i behållaren bilder. Allt du kan blanda tjänster i processer och tjänster i behållare i hello samma program. 

## <a name="containers-and-service-fabric-roadmap"></a>Behållare och Service Fabric-översikt
Många förbättringar som planeras att behållaren orchestration med Service Fabric i kommande versioner. Förbättringarna innefattar funktioner för utökad nätverk med stöd för virtuella nätverk i ett program, säkerhet, bättre diagnostik och verktygsstöd. Service Fabric kan du blanda behållare paketerat med befintlig kod (till exempel IIS MVC-appar) med tjänster som utvecklats med hello Service Fabric programmeringsmodeller toocreate-program.  Du kan köra flera program på ett kluster. 

## <a name="what-are-containers"></a>Vad är behållare?
Behållare är inkapslade, individuellt distribuerbara komponenter som körs som testisolerade instanser på hello samma kernel tootake nytta av virtualisering som innehåller ett operativsystem. Därför körs varje program och dess runtime, beroenden och systemets bibliotek i en behållare med fullständig, privat åtkomst toohello behållarens egna isolerade vy för konstruktionerna för operativsystemet. Tillsammans med mobilitet och är av säkerhets- och isolering hello största fördelen för användning av behållare med Service Fabric som annars kör tjänster i processer.

Behållare är en virtualiseringsteknik som virtualiserar hello underliggande operativsystemet från program. Behållare tillhandahålla en miljö som inte ändras för program toorun med olika typer av isolering. Behållare körs direkt ovanpå hello kernel och har en isolerad vy av hello filsystemet och andra resurser. Datorer jämfört med toovirtual behållare har hello följande fördelar:

* **Liten**: behållare använder en enda utrymme och lager versioner och uppdateringar tooincrease lagringseffektiviteten.
* **Snabb**: behållare har inte tooboot ett hela operativsystem, så de kan starta mycket snabbare vanligtvis i sekunder.
* **Överföring av**: en avbildning av programmet kan vara port toorun i hello molnet lokalt i virtuella datorer eller direkt på fysiska datorer.
* **Resurs-styrning**: en behållare kan begränsa hello fysiska resurser som den kan använda på värden.

## <a name="container-types"></a>Behållartyper
Service Fabric stöder behållare på Linux- och Windows och stöder även Hyper-V-isoleringsläge på hello senare. 

### <a name="docker-containers-on-linux"></a>Docker behållare på Linux
Docker ger avancerade API: er toocreate och hantera behållare ovanpå Linux kernel-behållare. Docker-hubben är en central databas toostore och hämta avbildningar för behållaren.
En självstudiekurs finns [distribuera en Docker-behållare tooService Fabric](service-fabric-get-started-containers-linux.md).

### <a name="windows-server-containers"></a>Windows Server-behållare
Windows Server 2016 innehåller två olika typer av behållare som skiljer sig åt i hello andelen angivna isolering. Windows Server-behållare och Docker-behållare är liknande eftersom både namnområdet och filen system isolering men resursen hello kernel med hello-värden som de körs på. På Linux för, denna isolering traditionellt har angetts av `cgroups` och `namespaces`, och Windows Server-behållare fungerar på samma sätt.

Windows Hyper-V-behållare ger mer isolering och säkerhet, eftersom varje behållare inte delar hello operativsystemets kärna med andra behållare eller hello värden. Med den här högre, säkerhetsisolering riktas Hyper-V-behållare mot skadliga, flera scenarier.
En självstudiekurs finns [distribuera en Windows-behållaren tooService Fabric](service-fabric-get-started-containers.md).

hello följande bild visar hello olika typer av virtualisering och isolering nivåer som är tillgängliga i hello-operativsystemet.
![Service Fabric-plattformen][Image1]

## <a name="scenarios-for-using-containers"></a>Scenarier för användning av behållare
Här följer vanliga exempel där en behållare är ett bra alternativ:

* **IIS lyfta och flytta**: Om du har befintliga [ASP.NET MVC](https://www.asp.net/mvc) appar som du vill toocontinue toouse placera dem i en behållare i stället för migrera dem tooASP.NET kärnor. Apparna ASP.NET MVC beror på Internet Information Services (IIS). Du kan paketera programmen i behållaren bilder från hello precreated IIS-avbildning och distribuera dem med Service Fabric. Se [behållare bilder på Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/quick_start/quick_start_images) information om hur toocreate IIS-avbildningar.
* **Blanda behållare och Service Fabric mikrotjänster**: Använd en befintlig behållare avbildning för en del av ditt program. Du kan till exempel använda hello [NGINX-behållaren](https://hub.docker.com/_/nginx/) för hello Frontwebb för programmet och tillståndskänsliga tjänster för hello mer intensiva backend-beräkning.
* **Minska påverkan av ”störningar grannar” services**: du kan använda hello resurs styrning möjligheten för behållare toorestrict hello resurser som en tjänst används på en värd. Om services kan använda många resurser och påverkar hello prestandan för andra (till exempel en tidskrävande, fråga-liknande åtgärd) kan du placera dessa tjänster i behållare som har resurs styrning.

## <a name="service-fabric-support-for-containers"></a>Service Fabric-stöd för behållare
Service Fabric stöder för närvarande distribution av Docker-behållare i Linux och Windows Server-behållare på Windows Server 2016 tillsammans med stöd för Hyper-V-isoleringsläge. 

I hello Service Fabric [programmodell](service-fabric-application-model.md), en behållare representerar en programvärd i vilken flera tjänst repliker placeras. Service Fabric kan köra behållare och hello scenario är liknande toohello [gäst körbara scenariot](service-fabric-deploy-existing-app.md), där du paketerar ett befintligt program i en behållare. Det här scenariot är hello vanliga användningsfall för behållare och exempel kör ett program som skrivits med valfritt språk och ramverk, men inte använder hello inbyggda Service Fabric programmeringsmodeller.

Du kan dessutom köra Service Fabric-tjänster i behållare också. Stöd för Service Fabric-tjänster som körs i behållare som är begränsad för närvarande men toobe förbättrad i kommande versioner.

* **Tillförlitliga tillståndslösa tjänster i behållare**: tillförlitliga tillståndslösa tjänster med hjälp av hello Reliable Services programmeringsmodeller stöds bara på Linux. Stöd för tillförlitlig tillståndslösa tjänster i Windows-behållare är planerad för framtida versioner.
* **Tillståndskänsliga tjänster i behållare**: dessa tjänster använder hello Reliable Actors eller Reliable Services programmeringsmodell. Stöd för körs tillståndskänsliga tjänster i behållare kommer att läggas till i framtiden släpp.

Service Fabric har flera behållare funktioner som hjälper dig skapa program som består av mikrotjänster som är behållare. Service Fabric erbjuder hello följande funktioner för container tjänster:

* Distribution av avbildningar för behållaren och aktivering.
* Resurs-styrning.
* Autentisering för databasen.
* Behållaren toohost port portmappning.
* Identifiering av behållare till en annan och kommunikation.
* Möjlighet tooconfigure och ange miljövariabler.

## <a name="next-steps"></a>Nästa steg
I den här artikeln har du lärt dig om behållare, Service Fabric är en behållare för orchestrator, och den Service Fabric har funktioner som stöder behållare. Som ett nästa steg ska vi gå över exempel på respektive hello funktioner tooshow du hur toouse dem.

[Distribuera en Windows-behållaren tooService Fabric på Windows Server 2016](service-fabric-get-started-containers.md)

[Distribuera en Docker-behållare tooService Fabric på Linux](service-fabric-get-started-containers-linux.md)

[Image1]: media/service-fabric-containers/Service-Fabric-Types-of-Isolation.png
