---
title: "Skydda ditt nätverk i Azure Security Center | Microsoft Docs"
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
ms.openlocfilehash: 00b715507a7c3a4d784b800e7bf0c700f6ea6ff1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="protecting-your-network-in-azure-security-center"></a><span data-ttu-id="ab826-103">Skydda ditt nätverk i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="ab826-103">Protecting your network in Azure Security Center</span></span>
<span data-ttu-id="ab826-104">Azure Security Center analyserar säkerhetstillståndet hos dina Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="ab826-104">Azure Security Center analyzes the security state of your Azure resources.</span></span> <span data-ttu-id="ab826-105">När Security Center identifierar potentiella säkerhetsrisker, skapar rekommendationer som hjälper dig att konfigurera nödvändiga kontroller.</span><span class="sxs-lookup"><span data-stu-id="ab826-105">When Security Center identifies potential security vulnerabilities, it creates recommendations that guide you through the process of configuring the needed controls.</span></span>  <span data-ttu-id="ab826-106">Rekommendationer gäller för Azure resurstyper: virtuella datorer (VM), nätverk, SQL och program.</span><span class="sxs-lookup"><span data-stu-id="ab826-106">Recommendations apply to Azure resource types: virtual machines (VMs), networking, SQL, and applications.</span></span>

<span data-ttu-id="ab826-107">Den här artikeln tar rekommendationer som gäller för ditt nätverk.</span><span class="sxs-lookup"><span data-stu-id="ab826-107">This article addresses recommendations that apply to your network.</span></span>  <span data-ttu-id="ab826-108">Rekommendationer Nätverkscenter runt nästa generation brandväggar, Nätverkssäkerhetsgrupper, konfigurera regler för inkommande trafik och mer.</span><span class="sxs-lookup"><span data-stu-id="ab826-108">Network recommendations center around next generation firewalls, Network Security Groups, configuring inbound traffic rules, and more.</span></span>  <span data-ttu-id="ab826-109">Använd tabellen nedan som referens för att förstå rekommendationerna som tillgängliga nätverk och det var och en gör om du använder den.</span><span class="sxs-lookup"><span data-stu-id="ab826-109">Use the table below as a reference to help you understand the available network recommendations and what each one does if you apply it.</span></span>

## <a name="available-network-recommendations"></a><span data-ttu-id="ab826-110">Tillgängliga nätverksrekommendationer</span><span class="sxs-lookup"><span data-stu-id="ab826-110">Available network recommendations</span></span>
| <span data-ttu-id="ab826-111">Rekommendation</span><span class="sxs-lookup"><span data-stu-id="ab826-111">Recommendation</span></span> | <span data-ttu-id="ab826-112">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ab826-112">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="ab826-113">Lägga till en nästa generations brandvägg</span><span class="sxs-lookup"><span data-stu-id="ab826-113">Add a Next Generation Firewall</span></span>](security-center-add-next-generation-firewall.md) |<span data-ttu-id="ab826-114">Rekommenderar att du lägger till en nästa generations Brandvägg från en Microsoft-partner att öka din säkerhetsskydd.</span><span class="sxs-lookup"><span data-stu-id="ab826-114">Recommends that you add a Next Generation Firewall (NGFW) from a Microsoft partner to increase your security protections.</span></span> |
| [<span data-ttu-id="ab826-115">Dirigera trafiken endast via NGFW</span><span class="sxs-lookup"><span data-stu-id="ab826-115">Route traffic through NGFW only</span></span>](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |<span data-ttu-id="ab826-116">Rekommenderar att du konfigurerar (NSG) regler för nätverkssäkerhetsgrupper som tvingar inkommande trafik till den virtuella datorn via din nästa generations Brandvägg.</span><span class="sxs-lookup"><span data-stu-id="ab826-116">Recommends that you configure network security group (NSG) rules that force inbound traffic to your VM through your NGFW.</span></span> |
| [<span data-ttu-id="ab826-117">Aktivera Nätverkssäkerhetsgrupper för undernät eller virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="ab826-117">Enable Network Security Groups on subnets or virtual machines</span></span>](security-center-enable-network-security-groups.md) |<span data-ttu-id="ab826-118">Rekommenderar att du aktiverar NSG: er på undernät eller virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ab826-118">Recommends that you enable NSGs on subnets or VMs.</span></span> |
| [<span data-ttu-id="ab826-119">Begränsa åtkomst via Internetuppkopplad slutpunkt</span><span class="sxs-lookup"><span data-stu-id="ab826-119">Restrict access through Internet facing endpoint</span></span>](security-center-restrict-access-through-internet-facing-endpoints.md) |<span data-ttu-id="ab826-120">Rekommenderar att du konfigurerar regler för inkommande trafik för NSG: er.</span><span class="sxs-lookup"><span data-stu-id="ab826-120">Recommends that you configure inbound traffic rules for NSGs.</span></span> |

## <a name="see-also"></a><span data-ttu-id="ab826-121">Se även</span><span class="sxs-lookup"><span data-stu-id="ab826-121">See also</span></span>
<span data-ttu-id="ab826-122">Mer information om rekommendationer som gäller för andra typer av Azure finns i:</span><span class="sxs-lookup"><span data-stu-id="ab826-122">To learn more about recommendations that apply to other Azure resource types, see the following:</span></span>

* [<span data-ttu-id="ab826-123">Skydda dina virtuella datorer i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="ab826-123">Protecting your virtual machines in Azure Security Center</span></span>](security-center-virtual-machine-recommendations.md)
* [<span data-ttu-id="ab826-124">Skydda dina program i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="ab826-124">Protecting your applications in Azure Security Center</span></span>](security-center-application-recommendations.md)
* [<span data-ttu-id="ab826-125">Skydda din SQL Azure-tjänst i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="ab826-125">Protecting your Azure SQL service in Azure Security Center</span></span>](security-center-sql-service-recommendations.md)

<span data-ttu-id="ab826-126">I följande avsnitt kan du lära dig mer om Security Center:</span><span class="sxs-lookup"><span data-stu-id="ab826-126">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="ab826-127">[Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Här får du lära dig hur du ställer in säkerhetsprinciper för prenumerationer och resursgrupper i Azure.</span><span class="sxs-lookup"><span data-stu-id="ab826-127">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="ab826-128">[Hantera och åtgärda säkerhetsaviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Här får du lära dig hur du hanterar och åtgärdar säkerhetsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="ab826-128">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="ab826-129">[Vanliga frågor och svar om Azure Security Center](security-center-faq.md) – Här hittar du vanliga frågor och svar om tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ab826-129">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using the service.</span></span>
