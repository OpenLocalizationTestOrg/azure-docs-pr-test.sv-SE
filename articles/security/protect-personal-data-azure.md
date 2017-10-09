---
title: aaaProtect personliga data i Microsoft Azure | Microsoft Docs
description: "Först artikel i en serie med artiklar toohelp du använder Azure tooprotect personliga data"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: cbffd3872552cbd0f12539535898c41ecf7789e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="protect-personal-data-in-microsoft-azure"></a><span data-ttu-id="d9f54-103">Skydda personliga data i Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="d9f54-103">Protect personal data in Microsoft Azure</span></span>

<span data-ttu-id="d9f54-104">Den här artikeln introducerar ett antal artiklar som hjälper dig att använda Azure security teknik och tjänster tooprotect personliga data.</span><span class="sxs-lookup"><span data-stu-id="d9f54-104">This article introduces a series of articles that help you use Azure security technologies and services tooprotect personal data.</span></span> <span data-ttu-id="d9f54-105">Detta är ett krav för många företag och industrin efterlevnad och styrning initiativ.</span><span class="sxs-lookup"><span data-stu-id="d9f54-105">This is a key requirement for many corporate and industry compliance and governance initiatives.</span></span> <span data-ttu-id="d9f54-106">hello scenario, problem och företagets mål ingår.</span><span class="sxs-lookup"><span data-stu-id="d9f54-106">hello scenario, problem statement and company goals are included here.</span></span>

## <a name="scenario-and-problem-statement"></a><span data-ttu-id="d9f54-107">Scenariot och problembeskrivningen</span><span class="sxs-lookup"><span data-stu-id="d9f54-107">Scenario and problem statement</span></span>

<span data-ttu-id="d9f54-108">Ett stort kryssning företag, sitt säte i hello USA utökar dess operations toooffer färdvägar i hello Medelhavet, Adriatiska havet, baltiska havet samt hello brittiska staterna.</span><span class="sxs-lookup"><span data-stu-id="d9f54-108">A large cruise company, headquartered in hello United States, is expanding its operations toooffer itineraries in hello Mediterranean, Adriatic, and Baltic seas, as well as hello British Isles.</span></span> <span data-ttu-id="d9f54-109">toosupport dessa ansträngningar den genererade flera mindre kryssning rader i Italien Tyskland, Danmark och hello Storbritannien</span><span class="sxs-lookup"><span data-stu-id="d9f54-109">toosupport those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark and hello U.K.</span></span>

<span data-ttu-id="d9f54-110">hello företag använder Microsoft Azure toostore företagsdata i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="d9f54-110">hello company uses Microsoft Azure toostore corporate data in hello cloud.</span></span> <span data-ttu-id="d9f54-111">Detta kan inkludera medarbetare och/eller kundinformation som:</span><span class="sxs-lookup"><span data-stu-id="d9f54-111">This may include employee and/or customer information such as:</span></span>

- <span data-ttu-id="d9f54-112">adresser</span><span class="sxs-lookup"><span data-stu-id="d9f54-112">addresses</span></span>
- <span data-ttu-id="d9f54-113">telefonnummer</span><span class="sxs-lookup"><span data-stu-id="d9f54-113">phone numbers</span></span>
- <span data-ttu-id="d9f54-114">skatt ID-nummer</span><span class="sxs-lookup"><span data-stu-id="d9f54-114">tax identification numbers</span></span>
- <span data-ttu-id="d9f54-115">medicinska information</span><span class="sxs-lookup"><span data-stu-id="d9f54-115">medical information</span></span>
- <span data-ttu-id="d9f54-116">kreditkortsinformation</span><span class="sxs-lookup"><span data-stu-id="d9f54-116">credit card information</span></span>

<span data-ttu-id="d9f54-117">hello företag måste skydda hello säkerheten för de anställda och kunder data när du gör data tillgängliga toothose avdelningar som behöver den.</span><span class="sxs-lookup"><span data-stu-id="d9f54-117">hello company must protect hello privacy of employee and customer data while making data accessible toothose departments that need it.</span></span> <span data-ttu-id="d9f54-118">(till exempel löneuppgifter och -reservationer avdelningar)</span><span class="sxs-lookup"><span data-stu-id="d9f54-118">(such as payroll and reservations departments)</span></span>

## <a name="company-goals"></a><span data-ttu-id="d9f54-119">Företagets mål</span><span class="sxs-lookup"><span data-stu-id="d9f54-119">Company goals</span></span> 

- <span data-ttu-id="d9f54-120">Datakällor som innehåller personuppgifter krypteras när som finns i molnet.</span><span class="sxs-lookup"><span data-stu-id="d9f54-120">Data sources that contain personal data are encrypted when residing in cloud storage.</span></span>

- <span data-ttu-id="d9f54-121">Personliga data som överförs från en plats tooanother är krypterad under överföringen.</span><span class="sxs-lookup"><span data-stu-id="d9f54-121">Personal data that is transferred from one location tooanother is encrypted while in-transit.</span></span> <span data-ttu-id="d9f54-122">Detta är SANT om hello data färdas över hello virtuellt nätverk eller över hello Internet mellan hello företagets datacenter och hello Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="d9f54-122">This is true if hello data is traveling across hello virtual network or across hello Internet between hello corporate datacenter and hello Azure cloud.</span></span>

- <span data-ttu-id="d9f54-123">Sekretess och integriteten för personliga data är skyddat från obehörig åtkomst av starka Identitetshantering och tekniker för åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="d9f54-123">Confidentiality and integrity of personal data is protected from unauthorized access by strong identity management and access control technologies.</span></span>

- <span data-ttu-id="d9f54-124">Personliga data skyddas mot exponering via data intrång via övervakning för säkerhetsrisker och hot.</span><span class="sxs-lookup"><span data-stu-id="d9f54-124">Personal data is protected from exposure via data breach via monitoring for vulnerabilities and threats.</span></span>

- <span data-ttu-id="d9f54-125">hello säkerhetstillståndet hos Azure-tjänster som lagrar eller skickar personliga data används för att utvärdera tooidentify affärsmöjligheter toobetter skydda personliga data.</span><span class="sxs-lookup"><span data-stu-id="d9f54-125">hello security state of Azure services that store or transmit personal data is assessed tooidentify opportunities toobetter protect personal data.</span></span>

## <a name="data-protection-guidance"></a><span data-ttu-id="d9f54-126">Data protection vägledning</span><span class="sxs-lookup"><span data-stu-id="d9f54-126">Data protection guidance</span></span>

<span data-ttu-id="d9f54-127">hello följande artiklar innehåller teknisk hur-tooguidance som hjälper dig uppnå hello personliga data protection mål som anges ovan:</span><span class="sxs-lookup"><span data-stu-id="d9f54-127">hello following articles contain technical how-tooguidance that will help you attain hello personal data protection goals listed above:</span></span>

- [<span data-ttu-id="d9f54-128">Azure krypteringstekniker</span><span class="sxs-lookup"><span data-stu-id="d9f54-128">Azure encryption technologies</span></span>](protect-personal-data-at-rest.md)

- [<span data-ttu-id="d9f54-129">Azure krypteringstekniker</span><span class="sxs-lookup"><span data-stu-id="d9f54-129">Azure encryption technologies</span></span>](protect-personal-data-in-transit-encryption.md)

- [<span data-ttu-id="d9f54-130">Azure identitets- och -tekniker</span><span class="sxs-lookup"><span data-stu-id="d9f54-130">Azure identity and access technologies</span></span>](protect-personal-data-identity-access-controls.md)

- [<span data-ttu-id="d9f54-131">Säkerhetstekniker i Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="d9f54-131">Azure network security technologies</span></span>](protect-personal-data-network-security.md)

- [<span data-ttu-id="d9f54-132">Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="d9f54-132">Azure Security Center</span></span>](protect-personal-data-azure-security-center.md)



## <a name="next-steps"></a><span data-ttu-id="d9f54-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d9f54-133">Next steps</span></span>

- [<span data-ttu-id="d9f54-134">Azure-säkerhet Information plats</span><span class="sxs-lookup"><span data-stu-id="d9f54-134">Azure Security Information Site</span></span>](https://aka.ms/AzureSecInfo)

- [<span data-ttu-id="d9f54-135">Microsoft Trust Center</span><span class="sxs-lookup"><span data-stu-id="d9f54-135">Microsoft Trust Center</span></span>](https://www.microsoft.com/TrustCenter/default.aspx)

- [<span data-ttu-id="d9f54-136">Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="d9f54-136">Azure Security Center</span></span>](https://azure.microsoft.com/services/security-center/)

- [<span data-ttu-id="d9f54-137">Azure-säkerhetsteamets blogg</span><span class="sxs-lookup"><span data-stu-id="d9f54-137">Azure Security Team Blog</span></span>](https://www.azuresecurityorg)

- [<span data-ttu-id="d9f54-138">Azure.com blogg - säkerhet</span><span class="sxs-lookup"><span data-stu-id="d9f54-138">Azure.com Blog - Security</span></span>](https://azure.microsoft.com/blog/topics/security/)
