---
title: "aaaAzure Service Fabric på Linux | Microsoft Docs"
description: "Service Fabric-kluster har stöd för Linux och Java, vilket innebär att du kommer att kunna toodeploy och värden Service Fabric-appar som skrivits i Java- och C# på Linux."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 459afade-145d-4ee6-b72b-ddf380ccd1bf
ms.service: service-fabric
ms.devlang: Java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: SubramaR
ms.openlocfilehash: ca0e092b00faec560963c0d6cc379593d085f6a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-on-linux"></a>Service Fabric på Linux
möjligt toobuild hello förhandsgranskning av Service Fabric på Linux, distribuera och hantera hög tillgänglighet och skalbara program på Linux precis som i Windows. hello Service Fabric ramverk (Reliable Services och Reliable Actors) är tillgängliga i Java på Linux i tillägg tooC # (.NET Core).  Du kan också skapa [körbara gästtjänster](service-fabric-deploy-existing-app.md) med valfritt språk eller ramverk. Hello preview stöder dessutom samordna Docker-behållare. Docker-behållare kan köra gäst körbara filer eller interna Service Fabric-tjänster som använder hello Service Fabric-ramverk.

Service Fabric på Linux är begreppsmässigt motsvarande tooService Fabric i Windows (förutom OS krav och programmeringsspråk som stöds). Därför de flesta av våra [befintliga dokumentationen](http://aka.ms/servicefabricdocs) gäller hur du bekanta dig med hello-teknik.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Service-Fabric-Linux-Preview/player]
>
>

## <a name="supported-operating-systems-and-programming-languages"></a>Operativsystem och programmeringsspråk som stöds
hello begränsad preview stöder hello skapas i en ruta utvecklingen av kluster dessutom toomulti datorn kluster i Azure som kör Ubuntu Server 16.04. hello preview stöder hello Reliable Actors och hello tillförlitliga tillståndslösa tjänster ramverk i Java- och C# i tillägg tooguest körbara filer och samordna Docker-behållare.  

> [!NOTE]
> Tillförlitliga samlingar stöds inte i Linux ännu. Vänteläge enbart stöds inte antingen - endast en ruta och flera datorer Azure Linux-kluster stöds i hello preview.
>
>


## <a name="supported-tooling"></a>Verktygsuppsättning som stöds
hello preview stöder interaktion med hello klustret via Service Fabric CLI. Integrering med Eclipse och Yeoman tillhandahålls för Java-utvecklare med Eclipse stöds på Linux och OS x. Hej OSX integrering använder en Linux VM under huven hello via vagrant. För C#-utvecklare tillhandahålls integrering med Yeoman toogenerate programmallar.

## <a name="next-steps"></a>Nästa steg

* Bekanta dig med hello [Reliable Actors](service-fabric-reliable-actors-introduction.md) och [Reliable Services](service-fabric-reliable-services-introduction.md) programmering ramverk
* [Förbereda utvecklingsmiljön i Linux](service-fabric-get-started-linux.md)
* [Förbereda utvecklingsmiljön i OSX](service-fabric-get-started-mac.md)
* [Skapa din första Service Fabric Java-program på Linux](service-fabric-create-your-first-linux-application-with-java.md)
* [Konfigurera Service Fabric kontinuerlig integrering och distribution med Jenkins och GitHub](service-fabric-cicd-your-linux-java-application-with-jenkins.md)
* [Skillnader mellan Service Fabric i Windows och Linux](service-fabric-linux-windows-differences.md)
