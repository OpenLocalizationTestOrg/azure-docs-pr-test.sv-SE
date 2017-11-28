---
title: aaaSecure din Sakernas Internet (IoT) i Azure | Microsoft Docs
description: " Azure internet av saker (IoT) services erbjuder en mängd funktioner. Den här artikeln hjälper dig att förstå hur toosecure IoT-lösningar i Azure. "
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TomSh
ms.assetid: 1473c8dd-8669-48fb-86db-b3c50e2eaf59
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2017
ms.author: terrylan
ms.openlocfilehash: b6cb2ea1c1facada854fb52c55066f34a8289e47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="internet-of-things-security-overview"></a><span data-ttu-id="d145e-104">Översikt över säkerheten i Sakernas Internet</span><span class="sxs-lookup"><span data-stu-id="d145e-104">Internet of Things security overview</span></span>
<span data-ttu-id="d145e-105">Azure internet av saker (IoT) services erbjuder en mängd funktioner.</span><span class="sxs-lookup"><span data-stu-id="d145e-105">Azure internet of things (IoT) services offer a broad range of capabilities.</span></span> <span data-ttu-id="d145e-106">Med dessa tjänster i företagsklass kan du:</span><span class="sxs-lookup"><span data-stu-id="d145e-106">These enterprise grade services enable you to:</span></span>

* <span data-ttu-id="d145e-107">Samla in data från enheter</span><span class="sxs-lookup"><span data-stu-id="d145e-107">Collect data from devices</span></span>
* <span data-ttu-id="d145e-108">Analysera dataströmmar i rörelse</span><span class="sxs-lookup"><span data-stu-id="d145e-108">Analyze data streams in-motion</span></span>
* <span data-ttu-id="d145e-109">Lagra och skicka frågor mot stora datamängder</span><span class="sxs-lookup"><span data-stu-id="d145e-109">Store and query large data sets</span></span>
* <span data-ttu-id="d145e-110">Visualisera både realtidsdata och historiska data</span><span class="sxs-lookup"><span data-stu-id="d145e-110">Visualize both real-time and historical data</span></span>
* <span data-ttu-id="d145e-111">Integrera med back office-system</span><span class="sxs-lookup"><span data-stu-id="d145e-111">Integrate with back-office systems</span></span>

<span data-ttu-id="d145e-112">Dessa funktioner, Azure IoT Suite paket tillsammans toodeliver flera Azure-tjänster med anpassade tillägg som förkonfigurerade lösningar.</span><span class="sxs-lookup"><span data-stu-id="d145e-112">toodeliver these capabilities, Azure IoT Suite packages together multiple Azure services with custom extensions as preconfigured solutions.</span></span> <span data-ttu-id="d145e-113">Dessa förkonfigurerade lösningar är grundläggande implementeringar av vanliga IoT-lösningen mönster som hjälper tooreduce hello tid som går åt toodeliver IoT-lösningar.</span><span class="sxs-lookup"><span data-stu-id="d145e-113">These preconfigured solutions are base implementations of common IoT solution patterns that help tooreduce hello time you take toodeliver your IoT solutions.</span></span> <span data-ttu-id="d145e-114">Med hello IoT software development Kit kan du anpassa och utöka dessa lösningar toomeet dina egna behov.</span><span class="sxs-lookup"><span data-stu-id="d145e-114">Using hello IoT software development kits, you can customize and extend these solutions toomeet your own requirements.</span></span> <span data-ttu-id="d145e-115">Du kan också använda dessa lösningar som exempel eller mallar när du utvecklar nya IoT-lösningar.</span><span class="sxs-lookup"><span data-stu-id="d145e-115">You can also use these solutions as examples or templates when you are developing new IoT solutions.</span></span>

<span data-ttu-id="d145e-116">hello Azure IoT suite är en kraftfull lösning för din IoT-behov.</span><span class="sxs-lookup"><span data-stu-id="d145e-116">hello Azure IoT suite is a powerful solution for your IoT needs.</span></span> <span data-ttu-id="d145e-117">Det är dock upmost viktigt att IoT-lösningar är utformade med säkerhet i åtanke från hello start.</span><span class="sxs-lookup"><span data-stu-id="d145e-117">However, it’s of upmost importance that your IoT solutions are designed with security in mind from hello start.</span></span> <span data-ttu-id="d145e-118">På grund av hello finns så många IoT-enheter, kan en säkerhetsincident snabbt bli en omfattande händelse med betydande konsekvenser.</span><span class="sxs-lookup"><span data-stu-id="d145e-118">Because of hello sheer number of IoT devices, any security incident can quickly become a widespread event with significant consequences.</span></span>

<span data-ttu-id="d145e-119">toohelp du förstår hur toosecure IoT-lösningar har vi hello följande information.</span><span class="sxs-lookup"><span data-stu-id="d145e-119">toohelp you understand how toosecure your IoT solutions, we have hello following information.</span></span>

## <a name="security-architecture"></a><span data-ttu-id="d145e-120">Säkerhetsarkitektur</span><span class="sxs-lookup"><span data-stu-id="d145e-120">Security architecture</span></span>
<span data-ttu-id="d145e-121">När du designar ett system, är viktiga toounderstand hello potentiella hot toothat system och Lägg till lämpliga försvar därför hello system är utformad och konstruerad.</span><span class="sxs-lookup"><span data-stu-id="d145e-121">When designing a system, it is important toounderstand hello potential threats toothat system, and add appropriate defenses accordingly, as hello system is designed and architected.</span></span> <span data-ttu-id="d145e-122">Det är viktigt toodesign hello produkten från hello start med säkerhet i åtanke eftersom förstå hur en angripare kan vara kan toocompromise ett system som hjälper dig att se till att lämpliga åtgärder är på plats från hello början.</span><span class="sxs-lookup"><span data-stu-id="d145e-122">It is important toodesign hello product from hello start with security in mind because understanding how an attacker might be able toocompromise a system helps make sure appropriate mitigations are in place from hello beginning.</span></span>

<span data-ttu-id="d145e-123">Du kan lära dig om IoT-säkerhetsarkitekturen genom att läsa [Internet av saker säkerhetsarkitekturen](../iot-suite/iot-security-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="d145e-123">You can learn about IoT security architecture by reading [Internet of Things Security Architecture](../iot-suite/iot-security-architecture.md).</span></span>

<span data-ttu-id="d145e-124">Den här artikeln beskrivs hello följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="d145e-124">This article discusses hello following topics:</span></span>

* [<span data-ttu-id="d145e-125">Säkerhet börjar med en Hotmodell</span><span class="sxs-lookup"><span data-stu-id="d145e-125">Security Starts with a Threat Model</span></span>](../iot-suite/iot-security-architecture.md#security-starts-with-a-threat-model)
* [<span data-ttu-id="d145e-126">Säkerhet i IoT</span><span class="sxs-lookup"><span data-stu-id="d145e-126">Security in IoT</span></span>](../iot-suite/iot-security-architecture.md#security-in-iot)
* [<span data-ttu-id="d145e-127">Hot modellering hello Azure IoT-Referensarkitektur</span><span class="sxs-lookup"><span data-stu-id="d145e-127">Threat Modeling hello Azure IoT Reference Architecture</span></span>](../iot-suite/iot-security-architecture.md#threat-modeling-the-azure-iot-reference-architecture)

## <a name="security-from-hello-ground-up"></a><span data-ttu-id="d145e-128">Säkerhet från hello bakgrund</span><span class="sxs-lookup"><span data-stu-id="d145e-128">Security from hello ground up</span></span>
<span data-ttu-id="d145e-129">Hej IoT utgör unika säkerhet, sekretess och kompatibilitet utmaningar toobusinesses över hela världen.</span><span class="sxs-lookup"><span data-stu-id="d145e-129">hello IoT poses unique security, privacy, and compliance challenges toobusinesses worldwide.</span></span> <span data-ttu-id="d145e-130">Till skillnad från traditionella cyber teknik där problemen omfångsfasen handlar om programvara och hur den har implementerats gäller IoT vad som händer när hello cyber och hello fysiska världar Konvergera.</span><span class="sxs-lookup"><span data-stu-id="d145e-130">Unlike traditional cyber technology where these issues revolve around software and how it is implemented, IoT concerns what happens when hello cyber and hello physical worlds converge.</span></span> <span data-ttu-id="d145e-131">Skydda IoT-lösningar kräver att säkerställa säker etablering av enheter, säker anslutning mellan dessa enheter och hello molnet och säkert dataskydd i hello molnet under bearbetning och lagring.</span><span class="sxs-lookup"><span data-stu-id="d145e-131">Protecting IoT solutions requires ensuring secure provisioning of devices, secure connectivity between these devices and hello cloud, and secure data protection in hello cloud during processing and storage.</span></span> <span data-ttu-id="d145e-132">Arbeta mot dessa funktioner är dock begränsad resurs enheter, geografisk fördelning av distributioner och många enheter i en lösning.</span><span class="sxs-lookup"><span data-stu-id="d145e-132">Working against such functionality, however, are resource-constrained devices, geographic distribution of deployments, and many devices within a solution.</span></span>

<span data-ttu-id="d145e-133">Du kan lära dig hur toohandle säkerhet i dessa områden genom att läsa [Sakernas Internet security från hello bakgrund](../iot-suite/securing-iot-ground-up.md).</span><span class="sxs-lookup"><span data-stu-id="d145e-133">You can learn how toohandle security in these areas by reading [Internet of Things security from hello ground up](../iot-suite/securing-iot-ground-up.md).</span></span>

<span data-ttu-id="d145e-134">hello artikeln diskuteras hello följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="d145e-134">hello article discusses hello following topics:</span></span>

* [<span data-ttu-id="d145e-135">Säker infrastruktur från hello bakgrund</span><span class="sxs-lookup"><span data-stu-id="d145e-135">Secure infrastructure from hello ground up</span></span>](../iot-suite/securing-iot-ground-up.md#secure-infrastructure-from-the-ground-up)
* [<span data-ttu-id="d145e-136">Microsoft Azure – säker IoT-infrastruktur för ditt företag</span><span class="sxs-lookup"><span data-stu-id="d145e-136">Microsoft Azure – secure IoT infrastructure for your business</span></span>](../iot-suite/securing-iot-ground-up.md#microsoft-azure---secure-iot-infrastructure-for-your-business)

## <a name="best-practices"></a><span data-ttu-id="d145e-137">Metodtips</span><span class="sxs-lookup"><span data-stu-id="d145e-137">Best Practices</span></span>
<span data-ttu-id="d145e-138">Skydda en IoT-infrastruktur kräver en rigorösa security-strategi.</span><span class="sxs-lookup"><span data-stu-id="d145e-138">Securing an IoT infrastructure requires a rigorous security-in-depth strategy.</span></span> <span data-ttu-id="d145e-139">Från att skydda data i hello moln, skydda dataintegriteten medan under överföring via hello bygger offentliga internet toosecurely etablering enheter, varje lager större säkerhet säkerhet hello hela infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="d145e-139">From securing data in hello cloud, protecting data integrity while in transit over hello public internet, toosecurely provisioning devices, each layer builds greater security assurance in hello overall infrastructure.</span></span>

<span data-ttu-id="d145e-140">Lär du dig i Sakernas Internet security bästa praxis genom att läsa [Sakernas Internet säkerhetsmetoder](../iot-suite/iot-security-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="d145e-140">You can learn about Internet of Things security best practices by reading [Internet of Things security best practices](../iot-suite/iot-security-best-practices.md).</span></span>

<span data-ttu-id="d145e-141">hello artikeln diskuteras hello följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="d145e-141">hello article discusses hello following topics:</span></span>

* [<span data-ttu-id="d145e-142">IoT maskinvara tillverkare/integrator</span><span class="sxs-lookup"><span data-stu-id="d145e-142">IoT hardware manufacturer/integrator</span></span>](../iot-suite/iot-security-best-practices.md#iot-hardware-manufacturerintegrator)
* [<span data-ttu-id="d145e-143">IoT-lösningen utvecklare</span><span class="sxs-lookup"><span data-stu-id="d145e-143">IoT solution developer</span></span>](../iot-suite/iot-security-best-practices.md#iot-solution-developer)
* [<span data-ttu-id="d145e-144">Distribueraren för IoT-lösning</span><span class="sxs-lookup"><span data-stu-id="d145e-144">IoT solution deployer</span></span>](../iot-suite/iot-security-best-practices.md#iot-solution-deployer)
* [<span data-ttu-id="d145e-145">IoT-lösningen operator</span><span class="sxs-lookup"><span data-stu-id="d145e-145">IoT solution operator</span></span>](../iot-suite/iot-security-best-practices.md#iot-solution-operator)
