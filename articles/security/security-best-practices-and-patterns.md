---
title: "aaaAzure säkerhet bästa metoder och mönster | Microsoft Docs"
description: "hello artikeln ger en introduktion om säkerhetsmetoder för Azure och mönster och en granskad lista över rekommenderade säkerhetsmetoder för olika Azure-resurser."
services: azure-security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TomSh
ms.assetid: 1cbbf8dc-ea94-4a7e-8fa0-c2cb198956c5
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/06/2017
ms.author: terrylan
ms.openlocfilehash: eaaa9457faa1d5906275eb1fd8988d4d4aad101a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-best-practices-and-patterns"></a><span data-ttu-id="e562e-103">Azure säkerhetsmetoder och mönster</span><span class="sxs-lookup"><span data-stu-id="e562e-103">Azure security best practices and patterns</span></span>
<span data-ttu-id="e562e-104">Vi har för närvarande hello följande artiklar om Azure security bästa praxis och mönster.</span><span class="sxs-lookup"><span data-stu-id="e562e-104">We currently have hello following Azure security best practices and patterns articles.</span></span> <span data-ttu-id="e562e-105">Se till att toovisit platsen uppdaterar regelbundet toosee tooour växande listan över Azure säkerhetsmetoder och mönster:</span><span class="sxs-lookup"><span data-stu-id="e562e-105">Make sure toovisit this site periodically toosee updates tooour growing list of Azure security best practices and patterns:</span></span>  

* [<span data-ttu-id="e562e-106">Säkerhetsmetoder för Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="e562e-106">Azure network security best practices</span></span>](azure-security-network-security-best-practices.md)
* [<span data-ttu-id="e562e-107">Metodtips för säkerhet för Azure data och kryptering</span><span class="sxs-lookup"><span data-stu-id="e562e-107">Azure data security and encryption best practices</span></span>](azure-security-data-encryption-best-practices.md)
* [<span data-ttu-id="e562e-108">Kontrollera säkerhetsmetoder Identitetshantering och åtkomst</span><span class="sxs-lookup"><span data-stu-id="e562e-108">Identity management and access control security best practices</span></span>](azure-security-identity-management-best-practices.md)
* [<span data-ttu-id="e562e-109">Sakernas Internet säkerhetsmetoder</span><span class="sxs-lookup"><span data-stu-id="e562e-109">Internet of Things security best practices</span></span>](azure-security-iot-best-practices.md)
* <span data-ttu-id="e562e-110">[Azure IaaS säkerhetsmetoder] (azure-security-iaas.md)</span><span class="sxs-lookup"><span data-stu-id="e562e-110">[Azure IaaS Security Best Practices] (azure-security-iaas.md)</span></span>
* [<span data-ttu-id="e562e-111">Azure gräns säkerhetsmetoder</span><span class="sxs-lookup"><span data-stu-id="e562e-111">Azure boundary security best practices</span></span>](../best-practices-network-security.md)
* [<span data-ttu-id="e562e-112">Implementera en säker hybridnätverksarkitektur i Azure</span><span class="sxs-lookup"><span data-stu-id="e562e-112">Implementing a secure hybrid network architecture in Azure</span></span>](../guidance/guidance-iaas-ra-secure-vnet-hybrid.md)
* <span data-ttu-id="e562e-113">[Azure PaaS Best Practices] (https://docs.microsoft.com/en-us/azure/security/security-paas-deployments)</span><span class="sxs-lookup"><span data-stu-id="e562e-113">[Azure PaaS Best Practices] (https://docs.microsoft.com/en-us/azure/security/security-paas-deployments)</span></span>

<span data-ttu-id="e562e-114">Azure tillhandahåller en säker plattform som du kan skapa dina lösningar.</span><span class="sxs-lookup"><span data-stu-id="e562e-114">Azure provides a secure platform on which you can build your solutions.</span></span> <span data-ttu-id="e562e-115">Vi erbjuder även tjänster och teknik toomake dina lösningar på Azure säkrare.</span><span class="sxs-lookup"><span data-stu-id="e562e-115">We also provide services and technologies toomake your solutions on Azure more secure.</span></span> <span data-ttu-id="e562e-116">På grund av hello många alternativ tillgängliga tooyou, har många av du Tonande intresse för vad Microsoft rekommenderar som bästa praxis och mönster för att förbättra säkerheten.</span><span class="sxs-lookup"><span data-stu-id="e562e-116">Because of hello many options available tooyou, many of you have voiced an interest in what Microsoft recommends as best practices and patterns for improving security.</span></span>

<span data-ttu-id="e562e-117">Vi förstår ditt intresse och har skapat en samling dokument som beskriver vad du kan göra det angivna hello rätt kontext, tooimprove hello säkerheten för Azure-distributioner.</span><span class="sxs-lookup"><span data-stu-id="e562e-117">We understand your interest and have created a collection of documents that describe things you can do, given hello right context, tooimprove hello security of Azure deployments.</span></span>

<span data-ttu-id="e562e-118">I dessa metodtips och mönster artiklarna diskuterar vi en samling med bästa praxis och användbara mönster efter specifika avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e562e-118">In these best practices and patterns articles, we discuss a collection of best practices and useful patterns for specific topics.</span></span> <span data-ttu-id="e562e-119">Dessa bästa praxis och mönster härleds från våra upplevelser med dessa tekniker och hello erfarenheter från kunder som dig själv.</span><span class="sxs-lookup"><span data-stu-id="e562e-119">These best practices and patterns are derived from our experiences with these technologies and hello experiences of customers like yourself.</span></span>

<span data-ttu-id="e562e-120">För varje rekommenderar vi strävar efter tooexplain:</span><span class="sxs-lookup"><span data-stu-id="e562e-120">For each best practice we strive tooexplain:</span></span>

* <span data-ttu-id="e562e-121">Vilka hello bra</span><span class="sxs-lookup"><span data-stu-id="e562e-121">What hello best practice is</span></span>
* <span data-ttu-id="e562e-122">Varför du vill tooenable som bästa praxis</span><span class="sxs-lookup"><span data-stu-id="e562e-122">Why you want tooenable that best practice</span></span>
* <span data-ttu-id="e562e-123">Vad kan vara hello resultat om du inte bästa praxis för tooenable hello</span><span class="sxs-lookup"><span data-stu-id="e562e-123">What might be hello result if you fail tooenable hello best practice</span></span>
* <span data-ttu-id="e562e-124">Möjliga alternativ toohello bästa praxis</span><span class="sxs-lookup"><span data-stu-id="e562e-124">Possible alternatives toohello best practice</span></span>
* <span data-ttu-id="e562e-125">Hur du kan lära dig tooenable hello bästa praxis</span><span class="sxs-lookup"><span data-stu-id="e562e-125">How you can learn tooenable hello best practice</span></span>

<span data-ttu-id="e562e-126">Vi hoppas tooincluding många fler artiklar om Azure säkerhetsarkitekturen och bästa praxis.</span><span class="sxs-lookup"><span data-stu-id="e562e-126">We look forward tooincluding many more articles on Azure security architecture and best practices.</span></span> <span data-ttu-id="e562e-127">Om det finns avsnitt som du vill att vi tooinclude, låt oss veta under hello diskussion längst ned hello i den här sidan.</span><span class="sxs-lookup"><span data-stu-id="e562e-127">If there are topics that you'd like us tooinclude, let us know in hello discussion area at hello bottom of this page.</span></span>
