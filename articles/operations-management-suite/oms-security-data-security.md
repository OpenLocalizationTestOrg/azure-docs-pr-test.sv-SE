---
title: "aaaOperations Management Suite säkerhets- och granska lösningen datasäkerhet | Microsoft Docs"
description: "Det här dokumentet beskriver hur data hanteras och skyddas i säkerhets- och granskningslösningen i Operations Management Suite."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 9cdf7deb-2a30-4672-b89f-71179ee8326a
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: 9c4181b3b491e4f7f0c57d7252eca78a819722d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="operations-management-suite-security-and-audit-solution-data-security"></a><span data-ttu-id="73b91-103">Datasäkerhet i säkerhets- och granskningslösningen i Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="73b91-103">Operations Management Suite Security and Audit solution data security</span></span>
<span data-ttu-id="73b91-104">toohelp kunder förebygga, upptäcka och åtgärda toothreats, [Operations Management Suite (OMS) säkerhets- och granska lösningen](operations-management-suite-overview.md) samlar in och bearbetar data om dina resurser, som omfattar:</span><span class="sxs-lookup"><span data-stu-id="73b91-104">toohelp customers prevent, detect, and respond toothreats, [Operations Management Suite  (OMS) Security and Audit Solution](operations-management-suite-overview.md) collects and processes data about your resources, which includes:</span></span>

* <span data-ttu-id="73b91-105">Säkerhetshändelselogg</span><span class="sxs-lookup"><span data-stu-id="73b91-105">Security event log</span></span>
* <span data-ttu-id="73b91-106">ETW-händelser (Event Tracing for Windows)</span><span class="sxs-lookup"><span data-stu-id="73b91-106">Event Tracing for Windows (ETW) events</span></span>
* <span data-ttu-id="73b91-107">AppLocker-granskningshändelser</span><span class="sxs-lookup"><span data-stu-id="73b91-107">AppLocker auditing events</span></span>
* <span data-ttu-id="73b91-108">Windows-brandväggslogg</span><span class="sxs-lookup"><span data-stu-id="73b91-108">Windows Firewall log</span></span>
* <span data-ttu-id="73b91-109">Advanced Threat Analytics-händelser</span><span class="sxs-lookup"><span data-stu-id="73b91-109">Advanced Threat Analytics events</span></span>
* <span data-ttu-id="73b91-110">Resultat från utvärdering av säkerhetsbaslinje</span><span class="sxs-lookup"><span data-stu-id="73b91-110">Results of baseline assessment</span></span>
* <span data-ttu-id="73b91-111">Resultat från utvärdering av program mot skadlig kod</span><span class="sxs-lookup"><span data-stu-id="73b91-111">Results of antimalware assessment</span></span>
* <span data-ttu-id="73b91-112">Resultat från utvärdering av uppdateringar/korrigeringar</span><span class="sxs-lookup"><span data-stu-id="73b91-112">Results of update/patch assessment</span></span>
* <span data-ttu-id="73b91-113">Systemloggar dataströmmar som explicit har aktiverats på hello-agent</span><span class="sxs-lookup"><span data-stu-id="73b91-113">Syslogs streams that are explicitly enabled on hello agent</span></span>

<span data-ttu-id="73b91-114">Vi gör starka åtaganden tooprotect hello sekretess och säkerhet för dessa data.</span><span class="sxs-lookup"><span data-stu-id="73b91-114">We make strong commitments tooprotect hello privacy and security of this data.</span></span> <span data-ttu-id="73b91-115">Microsoft följer riktlinjer för toostrict efterlevnad och säkerhet – från kodning toooperating en tjänst.</span><span class="sxs-lookup"><span data-stu-id="73b91-115">Microsoft adheres toostrict compliance and security guidelines—from coding toooperating a service.</span></span>
<span data-ttu-id="73b91-116">Den här artikeln beskriver hur data hanteras och skyddas i säkerhets- och granskningslösningen i OMS.</span><span class="sxs-lookup"><span data-stu-id="73b91-116">This article explains how data is managed and safeguarded in OMS Security and Audit Solution.</span></span>

## <a name="data-sources"></a><span data-ttu-id="73b91-117">Datakällor</span><span class="sxs-lookup"><span data-stu-id="73b91-117">Data sources</span></span>
<span data-ttu-id="73b91-118">OMS säkerhets- och granska lösningen analysera data från virtuella datorer och fysiska datorer där hello OMS-agenten är installerad.</span><span class="sxs-lookup"><span data-stu-id="73b91-118">OMS Security and Audit Solution analyze data from your Virtual Machines and physical computers where hello OMS Agent is installed.</span></span> <span data-ttu-id="73b91-119">Säkerhets- och granskningslösningen i OMS kan samla in konfigurationsinformation om säkerhetshändelser, t.ex. Windows-händelser, granskningsloggar, IIS-loggar och syslog-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="73b91-119">OMS Security and Audit Solution can collect configuration information about security events, such as Windows event, audit logs, IIS logs and syslog messages.</span></span> <span data-ttu-id="73b91-120">Exempel på den här typen av data är: operativsystemets typ och version, aktiva processer, datornamn, IP-adresser, inloggad användare och klient-ID.</span><span class="sxs-lookup"><span data-stu-id="73b91-120">Examples of such data are: operating system type and version, running processes, machine name, IP addresses, logged in user, and tenant ID.</span></span>  

## <a name="data-protection"></a><span data-ttu-id="73b91-121">Dataskydd</span><span class="sxs-lookup"><span data-stu-id="73b91-121">Data protection</span></span>
<span data-ttu-id="73b91-122">**Dataavgränsning**: Data lagras logiskt separat på varje komponent i hela hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="73b91-122">**Data segregation**: Data is kept logically separate on each component throughout hello service.</span></span> <span data-ttu-id="73b91-123">Alla data taggas efter organisation.</span><span class="sxs-lookup"><span data-stu-id="73b91-123">All data is tagged per organization.</span></span> <span data-ttu-id="73b91-124">Den här taggning kvarstår under hello data livscykel och den tillämpas på varje nivå av hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="73b91-124">This tagging persists throughout hello data lifecycle, and it is enforced at each layer of hello service.</span></span> 

<span data-ttu-id="73b91-125">**Dataåtkomst**: tooprovide säkerhetsrekommendationer och undersöka potentiella säkerhetshot, Microsoft-Personal kan komma åt information som samlas in och analyseras av tjänster.</span><span class="sxs-lookup"><span data-stu-id="73b91-125">**Data access**: tooprovide security recommendations and investigate potential security threats, Microsoft personnel may access information collected or analyzed by services.</span></span> <span data-ttu-id="73b91-126">Vi följer toohello [licensvillkor för Microsoft Online Services](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) och [sekretesspolicy](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), vilket tillstånd att Microsoft inte använder kundinformation eller härledd information från den för reklam eller liknande kommersiella ändamål.</span><span class="sxs-lookup"><span data-stu-id="73b91-126">We adhere toohello [Microsoft Online Services Terms](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) and [Privacy Statement](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), which state that Microsoft will not use Customer Data or derive information from it for any advertising or similar commercial purposes.</span></span> <span data-ttu-id="73b91-127">tooprovide säkerhetsrekommendationer och undersöka potentiella säkerhetshot, Microsoft-Personal kan komma åt information som samlas in och analyseras av tjänster.</span><span class="sxs-lookup"><span data-stu-id="73b91-127">tooprovide security recommendations and investigate potential security threats, Microsoft personnel may access information collected or analyzed by services.</span></span> <span data-ttu-id="73b91-128">Vi använder bara kundinformation som behövs tooprovide du med Azure-tjänsterna, inklusive syfte som är kompatibla med dessa tjänster.</span><span class="sxs-lookup"><span data-stu-id="73b91-128">We only use Customer Data as needed tooprovide you with Azure services, including purposes compatible with providing those services.</span></span> <span data-ttu-id="73b91-129">Du kan behålla alla rättigheter tooyour egna data.</span><span class="sxs-lookup"><span data-stu-id="73b91-129">You retain all rights tooyour own data.</span></span>

<span data-ttu-id="73b91-130">**Använd data**: Microsoft använder mönster och hotinformation ses över flera innehavare tooenhance våra förhindra och upptäcka funktioner; vi gör i enlighet med hello sekretess åtaganden som beskrivs i vår [sekretess Instruktionen](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).</span><span class="sxs-lookup"><span data-stu-id="73b91-130">**Data use**: Microsoft uses patterns and threat intelligence seen across multiple tenants tooenhance our prevention and detection capabilities; we do so in accordance with hello privacy commitments described in our [Privacy Statement](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="73b91-131">Dataplats har konfigurerats på hello OMS arbetsytan nivå under hello Skapa arbetsyta, vilket är en del av hello inledande OMS säkerhet och granska konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="73b91-131">Data location is configured at hello OMS workspace level, during hello workspace creation, which is part of hello initial OMS Security and Audit configuration process.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="73b91-132">Se även</span><span class="sxs-lookup"><span data-stu-id="73b91-132">See also</span></span>
<span data-ttu-id="73b91-133">I det här dokumentet har du lärt dig hur data hanteras och skyddas i OMS.</span><span class="sxs-lookup"><span data-stu-id="73b91-133">In this document, you learned how data is managed and safeguarded in OMS.</span></span> <span data-ttu-id="73b91-134">toolearn mer om OMS säkerhets- och granska lösningen, se:</span><span class="sxs-lookup"><span data-stu-id="73b91-134">toolearn more about OMS Security and Audit solution, see:</span></span>

* [<span data-ttu-id="73b91-135">Översikt över Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="73b91-135">Operations Management Suite (OMS) overview</span></span>](operations-management-suite-overview.md)
* [<span data-ttu-id="73b91-136">Övervakning och svarar tooSecurity varningar i Operations Management Suite säkerhet och granska lösning</span><span class="sxs-lookup"><span data-stu-id="73b91-136">Monitoring and Responding tooSecurity Alerts in Operations Management Suite Security and Audit Solution</span></span>](oms-security-responding-alerts.md)
* [<span data-ttu-id="73b91-137">Övervaka resurser i säkerhets- och granskningslösningen i Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="73b91-137">Monitoring Resources in Operations Management Suite Security and Audit Solution</span></span>](oms-security-monitoring-resources.md)

