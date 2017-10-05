---
title: "Datasäkerhet i säkerhets- och granskningslösningen i Operations Management Suite | Microsoft Docs"
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
ms.openlocfilehash: 3b6327b1f5150f32afd71639f32c55d823f1d1f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="operations-management-suite-security-and-audit-solution-data-security"></a><span data-ttu-id="33204-103">Datasäkerhet i säkerhets- och granskningslösningen i Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="33204-103">Operations Management Suite Security and Audit solution data security</span></span>
<span data-ttu-id="33204-104">[Säkerhets- och granskningslösningen i Operations Management Suite (OMS)](operations-management-suite-overview.md) hjälper dig att förhindra, upptäcka och svara på hot genom att samla in och bearbeta data om dina resurser, inklusive:</span><span class="sxs-lookup"><span data-stu-id="33204-104">To help customers prevent, detect, and respond to threats, [Operations Management Suite  (OMS) Security and Audit Solution](operations-management-suite-overview.md) collects and processes data about your resources, which includes:</span></span>

* <span data-ttu-id="33204-105">Säkerhetshändelselogg</span><span class="sxs-lookup"><span data-stu-id="33204-105">Security event log</span></span>
* <span data-ttu-id="33204-106">ETW-händelser (Event Tracing for Windows)</span><span class="sxs-lookup"><span data-stu-id="33204-106">Event Tracing for Windows (ETW) events</span></span>
* <span data-ttu-id="33204-107">AppLocker-granskningshändelser</span><span class="sxs-lookup"><span data-stu-id="33204-107">AppLocker auditing events</span></span>
* <span data-ttu-id="33204-108">Windows-brandväggslogg</span><span class="sxs-lookup"><span data-stu-id="33204-108">Windows Firewall log</span></span>
* <span data-ttu-id="33204-109">Advanced Threat Analytics-händelser</span><span class="sxs-lookup"><span data-stu-id="33204-109">Advanced Threat Analytics events</span></span>
* <span data-ttu-id="33204-110">Resultat från utvärdering av säkerhetsbaslinje</span><span class="sxs-lookup"><span data-stu-id="33204-110">Results of baseline assessment</span></span>
* <span data-ttu-id="33204-111">Resultat från utvärdering av program mot skadlig kod</span><span class="sxs-lookup"><span data-stu-id="33204-111">Results of antimalware assessment</span></span>
* <span data-ttu-id="33204-112">Resultat från utvärdering av uppdateringar/korrigeringar</span><span class="sxs-lookup"><span data-stu-id="33204-112">Results of update/patch assessment</span></span>
* <span data-ttu-id="33204-113">Syslog-dataströmmar som uttryckligen har aktiverats på agenten</span><span class="sxs-lookup"><span data-stu-id="33204-113">Syslogs streams that are explicitly enabled on the agent</span></span>

<span data-ttu-id="33204-114">Vi arbetar hårt för att skydda sekretessen och säkerheten för dessa data.</span><span class="sxs-lookup"><span data-stu-id="33204-114">We make strong commitments to protect the privacy and security of this data.</span></span> <span data-ttu-id="33204-115">Microsoft följer strikta riktlinjer för efterlevnad och säkerhet – från kodning till driften av en tjänst.</span><span class="sxs-lookup"><span data-stu-id="33204-115">Microsoft adheres to strict compliance and security guidelines—from coding to operating a service.</span></span>
<span data-ttu-id="33204-116">Den här artikeln beskriver hur data hanteras och skyddas i säkerhets- och granskningslösningen i OMS.</span><span class="sxs-lookup"><span data-stu-id="33204-116">This article explains how data is managed and safeguarded in OMS Security and Audit Solution.</span></span>

## <a name="data-sources"></a><span data-ttu-id="33204-117">Datakällor</span><span class="sxs-lookup"><span data-stu-id="33204-117">Data sources</span></span>
<span data-ttu-id="33204-118">Säkerhets- och granskningslösningen i OMS analyserar data från virtuella och fysiska datorer som OMS-agenten är installerad på.</span><span class="sxs-lookup"><span data-stu-id="33204-118">OMS Security and Audit Solution analyze data from your Virtual Machines and physical computers where the OMS Agent is installed.</span></span> <span data-ttu-id="33204-119">Säkerhets- och granskningslösningen i OMS kan samla in konfigurationsinformation om säkerhetshändelser, t.ex. Windows-händelser, granskningsloggar, IIS-loggar och syslog-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="33204-119">OMS Security and Audit Solution can collect configuration information about security events, such as Windows event, audit logs, IIS logs and syslog messages.</span></span> <span data-ttu-id="33204-120">Exempel på den här typen av data är: operativsystemets typ och version, aktiva processer, datornamn, IP-adresser, inloggad användare och klient-ID.</span><span class="sxs-lookup"><span data-stu-id="33204-120">Examples of such data are: operating system type and version, running processes, machine name, IP addresses, logged in user, and tenant ID.</span></span>  

## <a name="data-protection"></a><span data-ttu-id="33204-121">Dataskydd</span><span class="sxs-lookup"><span data-stu-id="33204-121">Data protection</span></span>
<span data-ttu-id="33204-122">**Datauppdelning**: Data lagras logiskt och separat på varje komponent i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="33204-122">**Data segregation**: Data is kept logically separate on each component throughout the service.</span></span> <span data-ttu-id="33204-123">Alla data taggas efter organisation.</span><span class="sxs-lookup"><span data-stu-id="33204-123">All data is tagged per organization.</span></span> <span data-ttu-id="33204-124">Den här taggningen finns kvar i informationens hela livscykel och används på varje lager i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="33204-124">This tagging persists throughout the data lifecycle, and it is enforced at each layer of the service.</span></span> 

<span data-ttu-id="33204-125">**Dataåtkomst**: För att kunna erbjuda säkerhetsrekommendationer och undersöka möjliga säkerhetshot kan Microsofts personal komma åt information som samlas in eller analyseras av tjänster.</span><span class="sxs-lookup"><span data-stu-id="33204-125">**Data access**: To provide security recommendations and investigate potential security threats, Microsoft personnel may access information collected or analyzed by services.</span></span> <span data-ttu-id="33204-126">Vi följer [villkoren för Microsoft Online Services](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) och tillhörande [sekretesspolicy](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), som garanterar att Microsoft inte använder kunddata eller härleder information från dem för reklamändamål eller i liknande kommersiellt syfte.</span><span class="sxs-lookup"><span data-stu-id="33204-126">We adhere to the [Microsoft Online Services Terms](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) and [Privacy Statement](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), which state that Microsoft will not use Customer Data or derive information from it for any advertising or similar commercial purposes.</span></span> <span data-ttu-id="33204-127">För att kunna erbjuda säkerhetsrekommendationer och undersöka möjliga säkerhetshot kan Microsofts personal komma åt information som samlas in eller analyseras av tjänster.</span><span class="sxs-lookup"><span data-stu-id="33204-127">To provide security recommendations and investigate potential security threats, Microsoft personnel may access information collected or analyzed by services.</span></span> <span data-ttu-id="33204-128">Vi använder bara kunddata i den mån det är nödvändigt för att tillhandahålla Azure-tjänsterna, samt för därtill relaterade ändamål.</span><span class="sxs-lookup"><span data-stu-id="33204-128">We only use Customer Data as needed to provide you with Azure services, including purposes compatible with providing those services.</span></span> <span data-ttu-id="33204-129">Du äger alla rättigheter till dina egna data.</span><span class="sxs-lookup"><span data-stu-id="33204-129">You retain all rights to your own data.</span></span>

<span data-ttu-id="33204-130">**Dataanvändning**: Microsoft använder mönster och hotinformation som identifieras från flera klientorganisationer i syfte att förbättra våra skydds- och identifieringsfunktioner. Vi gör detta i enlighet med våra sekretessåtaganden som beskrivs i [sekretesspolicyn](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).</span><span class="sxs-lookup"><span data-stu-id="33204-130">**Data use**: Microsoft uses patterns and threat intelligence seen across multiple tenants to enhance our prevention and detection capabilities; we do so in accordance with the privacy commitments described in our [Privacy Statement](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="33204-131">Platsen för data konfigureras på arbetsytenivå i OMS när arbetsytan skapas, vilket är ett av stegen i den ursprungliga konfigurationsprocessen för säkerhets- och granskningslösningen i OMS.</span><span class="sxs-lookup"><span data-stu-id="33204-131">Data location is configured at the OMS workspace level, during the workspace creation, which is part of the initial OMS Security and Audit configuration process.</span></span>
> 
> 

## <a name="see-also"></a><span data-ttu-id="33204-132">Se även</span><span class="sxs-lookup"><span data-stu-id="33204-132">See also</span></span>
<span data-ttu-id="33204-133">I det här dokumentet har du lärt dig hur data hanteras och skyddas i OMS.</span><span class="sxs-lookup"><span data-stu-id="33204-133">In this document, you learned how data is managed and safeguarded in OMS.</span></span> <span data-ttu-id="33204-134">Mer information om säkerhets- och granskningslösningen i OMS finns här:</span><span class="sxs-lookup"><span data-stu-id="33204-134">To learn more about OMS Security and Audit solution, see:</span></span>

* [<span data-ttu-id="33204-135">Översikt över Operations Management Suite (OMS)</span><span class="sxs-lookup"><span data-stu-id="33204-135">Operations Management Suite (OMS) overview</span></span>](operations-management-suite-overview.md)
* [<span data-ttu-id="33204-136">Övervaka och svara på säkerhetsaviseringar i säkerhets- och granskningslösningen i Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="33204-136">Monitoring and Responding to Security Alerts in Operations Management Suite Security and Audit Solution</span></span>](oms-security-responding-alerts.md)
* [<span data-ttu-id="33204-137">Övervaka resurser i säkerhets- och granskningslösningen i Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="33204-137">Monitoring Resources in Operations Management Suite Security and Audit Solution</span></span>](oms-security-monitoring-resources.md)

