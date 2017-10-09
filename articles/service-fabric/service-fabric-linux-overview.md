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
# <a name="service-fabric-on-linux"></a><span data-ttu-id="13a6c-103">Service Fabric på Linux</span><span class="sxs-lookup"><span data-stu-id="13a6c-103">Service Fabric on Linux</span></span>
<span data-ttu-id="13a6c-104">möjligt toobuild hello förhandsgranskning av Service Fabric på Linux, distribuera och hantera hög tillgänglighet och skalbara program på Linux precis som i Windows.</span><span class="sxs-lookup"><span data-stu-id="13a6c-104">hello preview of Service Fabric on Linux enables you toobuild, deploy, and manage highly available, highly scalable applications on Linux just as you would on Windows.</span></span> <span data-ttu-id="13a6c-105">hello Service Fabric ramverk (Reliable Services och Reliable Actors) är tillgängliga i Java på Linux i tillägg tooC # (.NET Core).</span><span class="sxs-lookup"><span data-stu-id="13a6c-105">hello Service Fabric frameworks (Reliable Services and Reliable Actors) are available in Java on Linux in addition tooC# (.NET Core).</span></span>  <span data-ttu-id="13a6c-106">Du kan också skapa [körbara gästtjänster](service-fabric-deploy-existing-app.md) med valfritt språk eller ramverk.</span><span class="sxs-lookup"><span data-stu-id="13a6c-106">You can also build [guest executable services](service-fabric-deploy-existing-app.md) with any language or framework.</span></span> <span data-ttu-id="13a6c-107">Hello preview stöder dessutom samordna Docker-behållare.</span><span class="sxs-lookup"><span data-stu-id="13a6c-107">In addition, hello preview also supports orchestrating Docker containers.</span></span> <span data-ttu-id="13a6c-108">Docker-behållare kan köra gäst körbara filer eller interna Service Fabric-tjänster som använder hello Service Fabric-ramverk.</span><span class="sxs-lookup"><span data-stu-id="13a6c-108">Docker containers can run guest executables or native Service Fabric services, which use hello Service Fabric frameworks.</span></span>

<span data-ttu-id="13a6c-109">Service Fabric på Linux är begreppsmässigt motsvarande tooService Fabric i Windows (förutom OS krav och programmeringsspråk som stöds).</span><span class="sxs-lookup"><span data-stu-id="13a6c-109">Service Fabric on Linux is conceptually equivalent tooService Fabric on Windows (except for OS specifics and programming language support).</span></span> <span data-ttu-id="13a6c-110">Därför de flesta av våra [befintliga dokumentationen](http://aka.ms/servicefabricdocs) gäller hur du bekanta dig med hello-teknik.</span><span class="sxs-lookup"><span data-stu-id="13a6c-110">Thus, most of our [existing documentation](http://aka.ms/servicefabricdocs) applies in helping you get familiar with hello technology.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Service-Fabric-Linux-Preview/player]
>
>

## <a name="supported-operating-systems-and-programming-languages"></a><span data-ttu-id="13a6c-111">Operativsystem och programmeringsspråk som stöds</span><span class="sxs-lookup"><span data-stu-id="13a6c-111">Supported operating systems and programming languages</span></span>
<span data-ttu-id="13a6c-112">hello begränsad preview stöder hello skapas i en ruta utvecklingen av kluster dessutom toomulti datorn kluster i Azure som kör Ubuntu Server 16.04.</span><span class="sxs-lookup"><span data-stu-id="13a6c-112">hello limited preview supports hello creation of one-box development clusters in addition toomulti-machine clusters in Azure running Ubuntu Server 16.04.</span></span> <span data-ttu-id="13a6c-113">hello preview stöder hello Reliable Actors och hello tillförlitliga tillståndslösa tjänster ramverk i Java- och C# i tillägg tooguest körbara filer och samordna Docker-behållare.</span><span class="sxs-lookup"><span data-stu-id="13a6c-113">hello preview supports hello Reliable Actors and hello Reliable Stateless Services frameworks in Java and C# in addition tooguest executables and orchestrating Docker containers.</span></span>  

> [!NOTE]
> <span data-ttu-id="13a6c-114">Tillförlitliga samlingar stöds inte i Linux ännu.</span><span class="sxs-lookup"><span data-stu-id="13a6c-114">Reliable Collections are not supported in Linux yet.</span></span> <span data-ttu-id="13a6c-115">Vänteläge enbart stöds inte antingen - endast en ruta och flera datorer Azure Linux-kluster stöds i hello preview.</span><span class="sxs-lookup"><span data-stu-id="13a6c-115">Stand alone clusters aren't supported either - only one box and Azure Linux multi-machine clusters are supported in hello preview.</span></span>
>
>


## <a name="supported-tooling"></a><span data-ttu-id="13a6c-116">Verktygsuppsättning som stöds</span><span class="sxs-lookup"><span data-stu-id="13a6c-116">Supported tooling</span></span>
<span data-ttu-id="13a6c-117">hello preview stöder interaktion med hello klustret via Service Fabric CLI.</span><span class="sxs-lookup"><span data-stu-id="13a6c-117">hello preview supports interaction with hello cluster through Service Fabric CLI.</span></span> <span data-ttu-id="13a6c-118">Integrering med Eclipse och Yeoman tillhandahålls för Java-utvecklare med Eclipse stöds på Linux och OS x.</span><span class="sxs-lookup"><span data-stu-id="13a6c-118">For Java developers, integration with Eclipse and Yeoman is provided with Eclipse supported on Linux and on OSX.</span></span> <span data-ttu-id="13a6c-119">Hej OSX integrering använder en Linux VM under huven hello via vagrant.</span><span class="sxs-lookup"><span data-stu-id="13a6c-119">hello OSX integration uses a Linux VM under hello hood via vagrant.</span></span> <span data-ttu-id="13a6c-120">För C#-utvecklare tillhandahålls integrering med Yeoman toogenerate programmallar.</span><span class="sxs-lookup"><span data-stu-id="13a6c-120">For C# developers, integration with Yeoman is provided toogenerate application templates.</span></span>

## <a name="next-steps"></a><span data-ttu-id="13a6c-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="13a6c-121">Next steps</span></span>

* <span data-ttu-id="13a6c-122">Bekanta dig med hello [Reliable Actors](service-fabric-reliable-actors-introduction.md) och [Reliable Services](service-fabric-reliable-services-introduction.md) programmering ramverk</span><span class="sxs-lookup"><span data-stu-id="13a6c-122">Get familiar with hello [Reliable Actors](service-fabric-reliable-actors-introduction.md) and [Reliable Services](service-fabric-reliable-services-introduction.md) programming frameworks</span></span>
* [<span data-ttu-id="13a6c-123">Förbereda utvecklingsmiljön i Linux</span><span class="sxs-lookup"><span data-stu-id="13a6c-123">Prepare your development environment on Linux</span></span>](service-fabric-get-started-linux.md)
* [<span data-ttu-id="13a6c-124">Förbereda utvecklingsmiljön i OSX</span><span class="sxs-lookup"><span data-stu-id="13a6c-124">Prepare your development environment on OSX</span></span>](service-fabric-get-started-mac.md)
* [<span data-ttu-id="13a6c-125">Skapa din första Service Fabric Java-program på Linux</span><span class="sxs-lookup"><span data-stu-id="13a6c-125">Create your first Service Fabric Java application on Linux</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="13a6c-126">Konfigurera Service Fabric kontinuerlig integrering och distribution med Jenkins och GitHub</span><span class="sxs-lookup"><span data-stu-id="13a6c-126">Setup Service Fabric continuous integration and deployment with Jenkins and GitHub</span></span>](service-fabric-cicd-your-linux-java-application-with-jenkins.md)
* [<span data-ttu-id="13a6c-127">Skillnader mellan Service Fabric i Windows och Linux</span><span class="sxs-lookup"><span data-stu-id="13a6c-127">Service Fabric Windows/Linux differences</span></span>](service-fabric-linux-windows-differences.md)
