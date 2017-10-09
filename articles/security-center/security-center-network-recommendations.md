---
title: "aaaProtecting nätverket i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet behandlar rekommendationerna i Azure Security Center som hjälper dig skydda dina Azure-nätverk och uppfyller säkerhetsprinciper."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 96c55a02-afd6-478b-9c1f-039528f3dea0
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: terrylan
ms.openlocfilehash: 053738da432edf13b40172fb44d2044702dd8211
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="protecting-your-network-in-azure-security-center"></a><span data-ttu-id="9fb37-103">Skydda ditt nätverk i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="9fb37-103">Protecting your network in Azure Security Center</span></span>
<span data-ttu-id="9fb37-104">Azure Security Center analyserar hello säkerhetstillståndet hos dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="9fb37-104">Azure Security Center analyzes hello security state of your Azure resources.</span></span> <span data-ttu-id="9fb37-105">När Security Center identifierar potentiella säkerhetsrisker, skapar rekommendationer som guidar dig igenom hello konfigureringen av hello behövs kontroller.</span><span class="sxs-lookup"><span data-stu-id="9fb37-105">When Security Center identifies potential security vulnerabilities, it creates recommendations that guide you through hello process of configuring hello needed controls.</span></span>  <span data-ttu-id="9fb37-106">Rekommendationer gäller tooAzure resurstyper: virtuella datorer (VM), nätverk, SQL och program.</span><span class="sxs-lookup"><span data-stu-id="9fb37-106">Recommendations apply tooAzure resource types: virtual machines (VMs), networking, SQL, and applications.</span></span>

<span data-ttu-id="9fb37-107">Den här artikeln tar rekommendationer som gäller tooyour nätverk.</span><span class="sxs-lookup"><span data-stu-id="9fb37-107">This article addresses recommendations that apply tooyour network.</span></span>  <span data-ttu-id="9fb37-108">Rekommendationer Nätverkscenter runt nästa generation brandväggar, Nätverkssäkerhetsgrupper, konfigurera regler för inkommande trafik och mer.</span><span class="sxs-lookup"><span data-stu-id="9fb37-108">Network recommendations center around next generation firewalls, Network Security Groups, configuring inbound traffic rules, and more.</span></span>  <span data-ttu-id="9fb37-109">Använd hello tabellen nedan som en referens toohelp du förstå hello tillgängliga nätverksrekommendationer och det var och en gör om du använder den.</span><span class="sxs-lookup"><span data-stu-id="9fb37-109">Use hello table below as a reference toohelp you understand hello available network recommendations and what each one does if you apply it.</span></span>

## <a name="available-network-recommendations"></a><span data-ttu-id="9fb37-110">Tillgängliga nätverksrekommendationer</span><span class="sxs-lookup"><span data-stu-id="9fb37-110">Available network recommendations</span></span>
| <span data-ttu-id="9fb37-111">Rekommendation</span><span class="sxs-lookup"><span data-stu-id="9fb37-111">Recommendation</span></span> | <span data-ttu-id="9fb37-112">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9fb37-112">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="9fb37-113">Lägga till en nästa generations brandvägg</span><span class="sxs-lookup"><span data-stu-id="9fb37-113">Add a Next Generation Firewall</span></span>](security-center-add-next-generation-firewall.md) |<span data-ttu-id="9fb37-114">Rekommenderar att du lägger till en nästa generations Brandvägg från en Microsoft-partner tooincrease din säkerhetsskydd.</span><span class="sxs-lookup"><span data-stu-id="9fb37-114">Recommends that you add a Next Generation Firewall (NGFW) from a Microsoft partner tooincrease your security protections.</span></span> |
| [<span data-ttu-id="9fb37-115">Dirigera trafiken endast via NGFW</span><span class="sxs-lookup"><span data-stu-id="9fb37-115">Route traffic through NGFW only</span></span>](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |<span data-ttu-id="9fb37-116">Rekommenderar att du konfigurerar (NSG) regler för nätverkssäkerhetsgrupper som tvingar inkommande trafik tooyour VM via din nästa generations Brandvägg.</span><span class="sxs-lookup"><span data-stu-id="9fb37-116">Recommends that you configure network security group (NSG) rules that force inbound traffic tooyour VM through your NGFW.</span></span> |
| [<span data-ttu-id="9fb37-117">Aktivera Nätverkssäkerhetsgrupper för undernät eller virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="9fb37-117">Enable Network Security Groups on subnets or virtual machines</span></span>](security-center-enable-network-security-groups.md) |<span data-ttu-id="9fb37-118">Rekommenderar att du aktiverar NSG: er på undernät eller virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="9fb37-118">Recommends that you enable NSGs on subnets or VMs.</span></span> |
| [<span data-ttu-id="9fb37-119">Begränsa åtkomst via Internetuppkopplad slutpunkt</span><span class="sxs-lookup"><span data-stu-id="9fb37-119">Restrict access through Internet facing endpoint</span></span>](security-center-restrict-access-through-internet-facing-endpoints.md) |<span data-ttu-id="9fb37-120">Rekommenderar att du konfigurerar regler för inkommande trafik för NSG: er.</span><span class="sxs-lookup"><span data-stu-id="9fb37-120">Recommends that you configure inbound traffic rules for NSGs.</span></span> |

## <a name="see-also"></a><span data-ttu-id="9fb37-121">Se även</span><span class="sxs-lookup"><span data-stu-id="9fb37-121">See also</span></span>
<span data-ttu-id="9fb37-122">toolearn mer om rekommendationer som gäller tooother Azure resurstyper finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="9fb37-122">toolearn more about recommendations that apply tooother Azure resource types, see hello following:</span></span>

* [<span data-ttu-id="9fb37-123">Skydda dina virtuella datorer i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="9fb37-123">Protecting your virtual machines in Azure Security Center</span></span>](security-center-virtual-machine-recommendations.md)
* [<span data-ttu-id="9fb37-124">Skydda dina program i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="9fb37-124">Protecting your applications in Azure Security Center</span></span>](security-center-application-recommendations.md)
* [<span data-ttu-id="9fb37-125">Skydda din SQL Azure-tjänst i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="9fb37-125">Protecting your Azure SQL service in Azure Security Center</span></span>](security-center-sql-service-recommendations.md)

<span data-ttu-id="9fb37-126">toolearn mer om Security Center finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="9fb37-126">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="9fb37-127">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="9fb37-127">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="9fb37-128">[Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="9fb37-128">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="9fb37-129">[Vanliga frågor om Azure Security Center](security-center-faq.md) --finns vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9fb37-129">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
