---
title: Skydda dina Internet saker (IoT) i Azure | Microsoft Docs
description: " Azure internet av saker (IoT) services erbjuder en mängd funktioner. Den här artikeln hjälper dig att förstå hur du skyddar din IoT-lösningar i Azure. "
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
ms.openlocfilehash: 3793f5453b74b6c06d9e58b426d89099298e1288
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="internet-of-things-security-overview"></a><span data-ttu-id="2f90e-104">Översikt över säkerheten i Sakernas Internet</span><span class="sxs-lookup"><span data-stu-id="2f90e-104">Internet of Things security overview</span></span>
<span data-ttu-id="2f90e-105">Azure internet av saker (IoT) services erbjuder en mängd funktioner.</span><span class="sxs-lookup"><span data-stu-id="2f90e-105">Azure internet of things (IoT) services offer a broad range of capabilities.</span></span> <span data-ttu-id="2f90e-106">Med dessa tjänster i företagsklass kan du:</span><span class="sxs-lookup"><span data-stu-id="2f90e-106">These enterprise grade services enable you to:</span></span>

* <span data-ttu-id="2f90e-107">Samla in data från enheter</span><span class="sxs-lookup"><span data-stu-id="2f90e-107">Collect data from devices</span></span>
* <span data-ttu-id="2f90e-108">Analysera dataströmmar i rörelse</span><span class="sxs-lookup"><span data-stu-id="2f90e-108">Analyze data streams in-motion</span></span>
* <span data-ttu-id="2f90e-109">Lagra och skicka frågor mot stora datamängder</span><span class="sxs-lookup"><span data-stu-id="2f90e-109">Store and query large data sets</span></span>
* <span data-ttu-id="2f90e-110">Visualisera både realtidsdata och historiska data</span><span class="sxs-lookup"><span data-stu-id="2f90e-110">Visualize both real-time and historical data</span></span>
* <span data-ttu-id="2f90e-111">Integrera med back office-system</span><span class="sxs-lookup"><span data-stu-id="2f90e-111">Integrate with back-office systems</span></span>

<span data-ttu-id="2f90e-112">Att tillhandahålla dessa funktioner, Azure IoT Suite-paket tillsammans flera Azure-tjänster med anpassade tillägg som förkonfigurerade lösningar.</span><span class="sxs-lookup"><span data-stu-id="2f90e-112">To deliver these capabilities, Azure IoT Suite packages together multiple Azure services with custom extensions as preconfigured solutions.</span></span> <span data-ttu-id="2f90e-113">Dessa förkonfigurerade lösningar är grundläggande implementeringar av vanliga IoT-lösningsmönster som kan minska den tid det tar att leverera IoT-lösningar.</span><span class="sxs-lookup"><span data-stu-id="2f90e-113">These preconfigured solutions are base implementations of common IoT solution patterns that help to reduce the time you take to deliver your IoT solutions.</span></span> <span data-ttu-id="2f90e-114">Med IoT software development Kit kan du anpassa och utöka dessa lösningar för att uppfylla dina egna behov.</span><span class="sxs-lookup"><span data-stu-id="2f90e-114">Using the IoT software development kits, you can customize and extend these solutions to meet your own requirements.</span></span> <span data-ttu-id="2f90e-115">Du kan också använda dessa lösningar som exempel eller mallar när du utvecklar nya IoT-lösningar.</span><span class="sxs-lookup"><span data-stu-id="2f90e-115">You can also use these solutions as examples or templates when you are developing new IoT solutions.</span></span>

<span data-ttu-id="2f90e-116">Azure IoT suite är en kraftfull lösning för din IoT-behov.</span><span class="sxs-lookup"><span data-stu-id="2f90e-116">The Azure IoT suite is a powerful solution for your IoT needs.</span></span> <span data-ttu-id="2f90e-117">Det är dock upmost viktigt att IoT-lösningar är utformade med säkerhet i åtanke från början.</span><span class="sxs-lookup"><span data-stu-id="2f90e-117">However, it’s of upmost importance that your IoT solutions are designed with security in mind from the start.</span></span> <span data-ttu-id="2f90e-118">På grund av IoT-enheter finns så många bli en säkerhetsincident snabbt en omfattande händelse med betydande konsekvenser.</span><span class="sxs-lookup"><span data-stu-id="2f90e-118">Because of the sheer number of IoT devices, any security incident can quickly become a widespread event with significant consequences.</span></span>

<span data-ttu-id="2f90e-119">Vi har följande information för att hjälpa dig att förstå hur du skyddar din IoT-lösningar.</span><span class="sxs-lookup"><span data-stu-id="2f90e-119">To help you understand how to secure your IoT solutions, we have the following information.</span></span>

## <a name="security-architecture"></a><span data-ttu-id="2f90e-120">Säkerhetsarkitektur</span><span class="sxs-lookup"><span data-stu-id="2f90e-120">Security architecture</span></span>
<span data-ttu-id="2f90e-121">När ett system utformas, är det viktigt att förstå potentiella hot på systemet och lägga till lämpliga försvar därför eftersom systemet är utformad och konstruerad.</span><span class="sxs-lookup"><span data-stu-id="2f90e-121">When designing a system, it is important to understand the potential threats to that system, and add appropriate defenses accordingly, as the system is designed and architected.</span></span> <span data-ttu-id="2f90e-122">Det är viktigt att utforma produkten från början med säkerhet i åtanke eftersom förstå hur en angripare kan vara att en dator gör att lämpliga åtgärder finns på plats från början.</span><span class="sxs-lookup"><span data-stu-id="2f90e-122">It is important to design the product from the start with security in mind because understanding how an attacker might be able to compromise a system helps make sure appropriate mitigations are in place from the beginning.</span></span>

<span data-ttu-id="2f90e-123">Du kan lära dig om IoT-säkerhetsarkitekturen genom att läsa [Internet av saker säkerhetsarkitekturen](../iot-suite/iot-security-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="2f90e-123">You can learn about IoT security architecture by reading [Internet of Things Security Architecture](../iot-suite/iot-security-architecture.md).</span></span>

<span data-ttu-id="2f90e-124">Den här artikeln beskrivs i följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="2f90e-124">This article discusses the following topics:</span></span>

* [<span data-ttu-id="2f90e-125">Säkerhet börjar med en Hotmodell</span><span class="sxs-lookup"><span data-stu-id="2f90e-125">Security Starts with a Threat Model</span></span>](../iot-suite/iot-security-architecture.md#security-starts-with-a-threat-model)
* [<span data-ttu-id="2f90e-126">Säkerhet i IoT</span><span class="sxs-lookup"><span data-stu-id="2f90e-126">Security in IoT</span></span>](../iot-suite/iot-security-architecture.md#security-in-iot)
* [<span data-ttu-id="2f90e-127">Hot Modeling referens för Azure IoT-arkitektur</span><span class="sxs-lookup"><span data-stu-id="2f90e-127">Threat Modeling the Azure IoT Reference Architecture</span></span>](../iot-suite/iot-security-architecture.md#threat-modeling-the-azure-iot-reference-architecture)

## <a name="security-from-the-ground-up"></a><span data-ttu-id="2f90e-128">Säkerhet från grunden</span><span class="sxs-lookup"><span data-stu-id="2f90e-128">Security from the ground up</span></span>
<span data-ttu-id="2f90e-129">IoT utgör unika säkerhet, sekretess och kompatibilitet utmaningar för företag över hela världen.</span><span class="sxs-lookup"><span data-stu-id="2f90e-129">The IoT poses unique security, privacy, and compliance challenges to businesses worldwide.</span></span> <span data-ttu-id="2f90e-130">Till skillnad från traditionella cyber teknik där problemen omfångsfasen handlar om programvara och hur den har implementerats gäller IoT vad som händer när cyber och fysiska arbetslivet Konvergera.</span><span class="sxs-lookup"><span data-stu-id="2f90e-130">Unlike traditional cyber technology where these issues revolve around software and how it is implemented, IoT concerns what happens when the cyber and the physical worlds converge.</span></span> <span data-ttu-id="2f90e-131">Skydda IoT-lösningar kräver att säkerställa säker etablering av enheter, säker anslutning mellan dessa enheter och molnet och säkert dataskydd i molnet under bearbetning och lagring.</span><span class="sxs-lookup"><span data-stu-id="2f90e-131">Protecting IoT solutions requires ensuring secure provisioning of devices, secure connectivity between these devices and the cloud, and secure data protection in the cloud during processing and storage.</span></span> <span data-ttu-id="2f90e-132">Arbeta mot dessa funktioner är dock begränsad resurs enheter, geografisk fördelning av distributioner och många enheter i en lösning.</span><span class="sxs-lookup"><span data-stu-id="2f90e-132">Working against such functionality, however, are resource-constrained devices, geographic distribution of deployments, and many devices within a solution.</span></span>

<span data-ttu-id="2f90e-133">Du kan lära dig att hantera säkerhet i dessa områden genom att läsa [Sakernas Internet security från grunden](../iot-suite/securing-iot-ground-up.md).</span><span class="sxs-lookup"><span data-stu-id="2f90e-133">You can learn how to handle security in these areas by reading [Internet of Things security from the ground up](../iot-suite/securing-iot-ground-up.md).</span></span>

<span data-ttu-id="2f90e-134">Här beskrivs i följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="2f90e-134">The article discusses the following topics:</span></span>

* [<span data-ttu-id="2f90e-135">Säker infrastruktur från grunden</span><span class="sxs-lookup"><span data-stu-id="2f90e-135">Secure infrastructure from the ground up</span></span>](../iot-suite/securing-iot-ground-up.md#secure-infrastructure-from-the-ground-up)
* [<span data-ttu-id="2f90e-136">Microsoft Azure – säker IoT-infrastruktur för ditt företag</span><span class="sxs-lookup"><span data-stu-id="2f90e-136">Microsoft Azure – secure IoT infrastructure for your business</span></span>](../iot-suite/securing-iot-ground-up.md#microsoft-azure---secure-iot-infrastructure-for-your-business)

## <a name="best-practices"></a><span data-ttu-id="2f90e-137">Metodtips</span><span class="sxs-lookup"><span data-stu-id="2f90e-137">Best Practices</span></span>
<span data-ttu-id="2f90e-138">Skydda en IoT-infrastruktur kräver en rigorösa security-strategi.</span><span class="sxs-lookup"><span data-stu-id="2f90e-138">Securing an IoT infrastructure requires a rigorous security-in-depth strategy.</span></span> <span data-ttu-id="2f90e-139">Från att skydda data i molnet, skydda dataintegriteten som överförs via det offentliga internet, till att etablera enheter på ett säkert sätt, skapar varje lager större säkerhet säkerhet i hela infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="2f90e-139">From securing data in the cloud, protecting data integrity while in transit over the public internet, to securely provisioning devices, each layer builds greater security assurance in the overall infrastructure.</span></span>

<span data-ttu-id="2f90e-140">Lär du dig i Sakernas Internet security bästa praxis genom att läsa [Sakernas Internet säkerhetsmetoder](../iot-suite/iot-security-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="2f90e-140">You can learn about Internet of Things security best practices by reading [Internet of Things security best practices](../iot-suite/iot-security-best-practices.md).</span></span>

<span data-ttu-id="2f90e-141">Här beskrivs i följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="2f90e-141">The article discusses the following topics:</span></span>

* [<span data-ttu-id="2f90e-142">IoT maskinvara tillverkare/integrator</span><span class="sxs-lookup"><span data-stu-id="2f90e-142">IoT hardware manufacturer/integrator</span></span>](../iot-suite/iot-security-best-practices.md#iot-hardware-manufacturerintegrator)
* [<span data-ttu-id="2f90e-143">IoT-lösningen utvecklare</span><span class="sxs-lookup"><span data-stu-id="2f90e-143">IoT solution developer</span></span>](../iot-suite/iot-security-best-practices.md#iot-solution-developer)
* [<span data-ttu-id="2f90e-144">Distribueraren för IoT-lösning</span><span class="sxs-lookup"><span data-stu-id="2f90e-144">IoT solution deployer</span></span>](../iot-suite/iot-security-best-practices.md#iot-solution-deployer)
* [<span data-ttu-id="2f90e-145">IoT-lösningen operator</span><span class="sxs-lookup"><span data-stu-id="2f90e-145">IoT solution operator</span></span>](../iot-suite/iot-security-best-practices.md#iot-solution-operator)
