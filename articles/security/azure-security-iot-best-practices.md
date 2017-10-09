---
title: "aaaInternet av saker säkerhetsmetoder | Microsoft Docs"
description: "hello får en granskad lista över Microsoft Internet saker säkerhetsmetoder och allmänna rekommendationer."
services: security
documentationcenter: na
author: TomShinder
manager: StevenPo
editor: TomSh
ms.assetid: 2d5598c5-4c30-481d-b8f4-51ee024ea9a7
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: yurid
ms.openlocfilehash: 7ee31c912e8ac230ffa5efcd5b4c2b0b0713584f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="internet-of-things-security-best-practices"></a><span data-ttu-id="9d830-103">Internet av saker säkerhetsmetoder</span><span class="sxs-lookup"><span data-stu-id="9d830-103">Internet of Things Security Best Practices</span></span>
<span data-ttu-id="9d830-104">Infrastruktur för att säkra hello Sakernas Internet (IoT) är en kritisk företag alla som sysslar med IoT-lösningar.</span><span class="sxs-lookup"><span data-stu-id="9d830-104">Securing hello Internet of Things (IoT) infrastructure is a critical undertaking for anyone involved with IoT solutions.</span></span> <span data-ttu-id="9d830-105">På grund av hello antalet enheter som är inblandade och hello relaterade distribuerade uppbyggnad enheterna hello inverkan en säkerhetshändelse toocompromise miljoner IoT-enheter är icke-trivial och kan ha stor inverkan.</span><span class="sxs-lookup"><span data-stu-id="9d830-105">Because of hello number of devices involved and hello distributed nature of these devices, hello impact a security event related toocompromise of millions of IoT devices is non-trivial and can have widespread impact.</span></span>

<span data-ttu-id="9d830-106">Därför säkerhetsbehov IoT säkerhet ingående-information.</span><span class="sxs-lookup"><span data-stu-id="9d830-106">For this reason, IoT security needs a security-in-depth approach.</span></span> <span data-ttu-id="9d830-107">Data måste toobe säker i hello molnet och det rör sig över privata och offentliga nätverk.</span><span class="sxs-lookup"><span data-stu-id="9d830-107">Data needs toobe secure in hello cloud and as it moves over private and public networks.</span></span> <span data-ttu-id="9d830-108">Metoderna måste toobe i plats toosecurely etablera hello IoT-enheter själva.</span><span class="sxs-lookup"><span data-stu-id="9d830-108">Methods need toobe in place toosecurely provision hello IoT devices themselves.</span></span> <span data-ttu-id="9d830-109">Varje lager från enheten, toonetwork, toocloud backend-behöver garantier för hög säkerhet.</span><span class="sxs-lookup"><span data-stu-id="9d830-109">Each layer, from device, toonetwork, toocloud back-end needs strong security assurances.</span></span>

<span data-ttu-id="9d830-110">Metodtips för IoT kan kategoriseras i hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="9d830-110">IoT best practices can be categorized in hello following way:</span></span>

* <span data-ttu-id="9d830-111">IoT-maskinvarutillverkaren eller integrator</span><span class="sxs-lookup"><span data-stu-id="9d830-111">IoT hardware manufacturer or integrator</span></span>
* <span data-ttu-id="9d830-112">IoT-lösningen utvecklare</span><span class="sxs-lookup"><span data-stu-id="9d830-112">IoT solution developer</span></span>
* <span data-ttu-id="9d830-113">Distribueraren för IoT-lösning</span><span class="sxs-lookup"><span data-stu-id="9d830-113">IoT solution deployer</span></span>
* <span data-ttu-id="9d830-114">IoT-lösningen operator</span><span class="sxs-lookup"><span data-stu-id="9d830-114">IoT solution operator</span></span>

<span data-ttu-id="9d830-115">Den här artikeln sammanfattar [Internet av saker säkerhetsmetoder](../iot-suite/iot-security-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="9d830-115">This article summarizes [Internet of Things Security Best Practices](../iot-suite/iot-security-best-practices.md).</span></span> <span data-ttu-id="9d830-116">Mer detaljerad information finns i artikel toothat.</span><span class="sxs-lookup"><span data-stu-id="9d830-116">Please refer toothat article for more detailed information.</span></span>

## <a name="iot-hardware-manufacturer-or-integrator"></a><span data-ttu-id="9d830-117">IoT-maskinvarutillverkaren eller integrator</span><span class="sxs-lookup"><span data-stu-id="9d830-117">IoT hardware manufacturer or integrator</span></span>
<span data-ttu-id="9d830-118">Följ hello metodtips nedan om du är en IoT programvaran eller en integrator maskinvara:</span><span class="sxs-lookup"><span data-stu-id="9d830-118">Follow hello best practices below if you are an IoT hardware manufacture or a hardware integrator:</span></span>

* <span data-ttu-id="9d830-119">**Omfång toominimum maskinvarukrav**: hello maskinvara design ska innehålla minsta funktioner som krävs för driften av hello maskinvara och inget mer.</span><span class="sxs-lookup"><span data-stu-id="9d830-119">**Scope hardware toominimum requirements**: hello hardware design should include minimum features required for operation of hello hardware, and nothing more.</span></span> 
* <span data-ttu-id="9d830-120">**Kontrollera maskinvara säkerhetsförsluten bevis**: skapa i mekanismer toodetect fysiska ändring av maskinvara, till exempel öppna hello luckan tar bort en del av hello osv.</span><span class="sxs-lookup"><span data-stu-id="9d830-120">**Make hardware tamper proof**: build in mechanisms toodetect physical tampering of hardware, such as opening hello device cover, removing a part of hello device, etc.</span></span> 
* <span data-ttu-id="9d830-121">**Skapa runt säker maskinvara**: om [kostnad för sålda varor](https://en.wikipedia.org/wiki/Cost_of_goods_sold) tillåta, skapa säkerhetsfunktioner, till exempel säker och krypterad lagring och Trusted Platform Module TPM-baserade Start-funktioner.</span><span class="sxs-lookup"><span data-stu-id="9d830-121">**Build around secure hardware**: if [COGS](https://en.wikipedia.org/wiki/Cost_of_goods_sold) permit, build security features such as secure and encrypted storage and Trusted Platform Module (TPM)-based boot functionality.</span></span>
* <span data-ttu-id="9d830-122">**Göra uppgraderingar säker**: uppgradera firmware under livslängden för hello enheten inte är.</span><span class="sxs-lookup"><span data-stu-id="9d830-122">**Make upgrades secure**: upgrading firmware during lifetime of hello device is inevitable.</span></span>

## <a name="iot-solution-developer"></a><span data-ttu-id="9d830-123">IoT-lösningen utvecklare</span><span class="sxs-lookup"><span data-stu-id="9d830-123">IoT solution developer</span></span>
<span data-ttu-id="9d830-124">Följ hello metodtips nedan om du utvecklar en IoT-lösningen:</span><span class="sxs-lookup"><span data-stu-id="9d830-124">Follow hello best practices below if you are an IoT solution developer:</span></span>

* <span data-ttu-id="9d830-125">**Följ säker software development metoder**: utveckla säkra program kräver grunden som du tänker på säkerheten från hello starten av hello projekt alla hello sätt tooits implementering, testning och distribution.</span><span class="sxs-lookup"><span data-stu-id="9d830-125">**Follow secure software development methodology**: developing secure software requires ground-up thinking about security from hello inception of hello project all hello way tooits implementation, testing, and deployment.</span></span>
* <span data-ttu-id="9d830-126">**Välj programvara med öppen källkod med försiktighet**: programvara med öppen källkod ger en möjlighet tooquickly utveckla lösningar.</span><span class="sxs-lookup"><span data-stu-id="9d830-126">**Choose open source software with care**: open source software provides an opportunity tooquickly develop solutions.</span></span>
* <span data-ttu-id="9d830-127">**Integrera med försiktighet**: många hello programvara säkerhetsbrister finns på hello gränsen för bibliotek och API: er.</span><span class="sxs-lookup"><span data-stu-id="9d830-127">**Integrate with care**: many of hello software security flaws exist at hello boundary of libraries and APIs.</span></span> 

## <a name="iot-solution-deployer"></a><span data-ttu-id="9d830-128">Distribueraren för IoT-lösning</span><span class="sxs-lookup"><span data-stu-id="9d830-128">IoT solution deployer</span></span>
<span data-ttu-id="9d830-129">Följ hello metodtips nedan om du är en IoT-lösningen deployer:</span><span class="sxs-lookup"><span data-stu-id="9d830-129">Follow hello best practices below if you are an IoT solution deployer:</span></span>

* <span data-ttu-id="9d830-130">**Distribuera maskinvara på ett säkert sätt**: IoT-distributioner kan kräva maskinvara toobe distribuerats i osäkra platser, exempelvis offentliga blanksteg eller oövervakade språk.</span><span class="sxs-lookup"><span data-stu-id="9d830-130">**Deploy hardware securely**: IoT deployments may require hardware toobe deployed in unsecure locations, such as in public spaces or unsupervised locales.</span></span>
* <span data-ttu-id="9d830-131">**Skydda autentiseringsnycklar**: under distribution varje enhet kräver enhets-ID och tillhörande autentiseringsnycklar som genererats av hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="9d830-131">**Keep authentication keys safe**: during deployment, each device requires device IDs and associated authentication keys generated by hello cloud service.</span></span> <span data-ttu-id="9d830-132">Skydda de här nycklarna fysiskt även efter hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="9d830-132">Keep these keys physically safe even after hello deployment.</span></span> <span data-ttu-id="9d830-133">Alla avslöjade nyckeln kan användas av en obehörig enhet toomasquerade som en befintlig enhet.</span><span class="sxs-lookup"><span data-stu-id="9d830-133">Any compromised key can be used by a malicious device toomasquerade as an existing device.</span></span>

## <a name="iot-solution-operator"></a><span data-ttu-id="9d830-134">IoT-lösningen operator</span><span class="sxs-lookup"><span data-stu-id="9d830-134">IoT solution operator</span></span>
<span data-ttu-id="9d830-135">Följ hello metodtips nedan om du är operatör IoT-lösningen:</span><span class="sxs-lookup"><span data-stu-id="9d830-135">Follow hello best practices below if you are an IoT solution operator:</span></span>

* <span data-ttu-id="9d830-136">**Behåll system toodate**: Kontrollera enhetens operativsystem och alla drivrutiner är uppdaterade toohello senaste versionerna.</span><span class="sxs-lookup"><span data-stu-id="9d830-136">**Keep systems up toodate**: ensure device operating systems and all device drivers are updated toohello latest versions.</span></span> 
* <span data-ttu-id="9d830-137">**Skydda mot skadlig aktivitet**: om hello operativsystemet tillåter placera hello senaste antivirus- och spionprogramsskydd funktionerna på varje enhets operativsystem.</span><span class="sxs-lookup"><span data-stu-id="9d830-137">**Protect against malicious activity**: if hello operating system permits, place hello latest anti-virus and anti-malware capabilities on each device operating system.</span></span> 
* <span data-ttu-id="9d830-138">**Granska ofta**: granskning IoT infrastruktur för säkerhetsrelaterade problem är nyckel när svarar toosecurity incidenter.</span><span class="sxs-lookup"><span data-stu-id="9d830-138">**Audit frequently**: auditing IoT infrastructure for security related issues is key when responding toosecurity incidents.</span></span>
* <span data-ttu-id="9d830-139">**Fysiskt skydda hello IoT infrastruktur**: hello sämsta attacker mot IoT infrastruktur startas med fysisk åtkomst toodevices.</span><span class="sxs-lookup"><span data-stu-id="9d830-139">**Physically protect hello IoT infrastructure**: hello worst security attacks against IoT infrastructure are launched using physical access toodevices.</span></span>
* <span data-ttu-id="9d830-140">**Skydda molnet autentiseringsuppgifter**: molnet autentiseringsuppgifter som används för att konfigurera och använda en IoT-distribution är eventuellt hello enklaste sättet toogain åtkomst och IoT-dator.</span><span class="sxs-lookup"><span data-stu-id="9d830-140">**Protect cloud credentials**: cloud authentication credentials used for configuring and operating an IoT deployment are possibly hello easiest way toogain access and compromise an IoT system.</span></span> 

