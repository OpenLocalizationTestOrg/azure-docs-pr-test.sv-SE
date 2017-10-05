---
title: Hotinformationsrapporter i Azure Security Center | Microsoft Docs
description: "Det här dokumentet beskriver hur du använder hotinformationsrapporter i Azure Security Center i samband med en undersökning för att få fram mer information om en säkerhetsvarning."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 5662e312-e8c2-4736-974e-576eeb333484
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: b4310cf4e6849c67031b3ec8b1fd5957e35f7ea6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-security-center-threat-intelligence-report"></a><span data-ttu-id="82cfb-103">Hotinformationsrapporter i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="82cfb-103">Azure Security Center Threat Intelligence Report</span></span>
<span data-ttu-id="82cfb-104">Det här dokumentet beskriver hur du kan lära dig mer om ett hot som genererat en säkerhetsvarning med hjälp av hotinformationsrapporter i Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="82cfb-104">This document explains how Azure Security Center Threat Intelligent Reports can help you learn more about a threat that generated a security alert.</span></span>

## <a name="what-is-a-threat-intelligence-report"></a><span data-ttu-id="82cfb-105">Vad är en hotinformationsrapport?</span><span class="sxs-lookup"><span data-stu-id="82cfb-105">What is a threat intelligence report?</span></span>
<span data-ttu-id="82cfb-106">Hotidentifieringen i Security Center sker genom övervakning av säkerhetsinformation från dina Azure-resurser, nätverket och anslutna partnerlösningar.</span><span class="sxs-lookup"><span data-stu-id="82cfb-106">Security Center threat detection works by monitoring security information from your Azure resources, the network, and connected partner solutions.</span></span> <span data-ttu-id="82cfb-107">Tjänsten analyserar den här informationen, och korrelerar ofta information från flera källor för att identifiera hot.</span><span class="sxs-lookup"><span data-stu-id="82cfb-107">It analyzes this information, often correlating information from multiple sources, to identify threats.</span></span> <span data-ttu-id="82cfb-108">Den här processen är en del av [identifieringsfunktionerna](security-center-detection-capabilities.md) i Security Center.</span><span class="sxs-lookup"><span data-stu-id="82cfb-108">This process is part of the Security Center [detection capabilities](security-center-detection-capabilities.md).</span></span>

<span data-ttu-id="82cfb-109">När Security Center identifierar ett hot utlöses en [säkerhetsvarning](security-center-managing-and-responding-alerts.md), som innehåller detaljerad information om en viss händelse, inklusive förslag på åtgärder.</span><span class="sxs-lookup"><span data-stu-id="82cfb-109">When Security Center identifies a threat, it will trigger a [security alert](security-center-managing-and-responding-alerts.md), which contains detailed information regarding a particular event, including suggestions for remediation.</span></span> <span data-ttu-id="82cfb-110">Security Center hjälper incidenthanteringsteamet att undersöka och åtgärda hot genom att tillhandahålla hotinformationsrapporter med information om hotet som har identifierats, inklusive information som:</span><span class="sxs-lookup"><span data-stu-id="82cfb-110">To assist incident response teams investigate and remediate threats, Security Center includes a threat intelligence report that contains information about the threat that was detected, including information such as the:</span></span>

* <span data-ttu-id="82cfb-111">Angriparens identitet eller associationer (om den här informationen är tillgänglig)</span><span class="sxs-lookup"><span data-stu-id="82cfb-111">Attacker’s identity or associations (if this information is available)</span></span>
* <span data-ttu-id="82cfb-112">Angriparens mål</span><span class="sxs-lookup"><span data-stu-id="82cfb-112">Attackers’ objectives</span></span>
* <span data-ttu-id="82cfb-113">Aktuella och historiska attackkampanjer (om den här informationen är tillgänglig)</span><span class="sxs-lookup"><span data-stu-id="82cfb-113">Current and historical attack campaigns (if this information is available)</span></span>
* <span data-ttu-id="82cfb-114">Angriparens metoder, verktyg och procedurer</span><span class="sxs-lookup"><span data-stu-id="82cfb-114">Attackers’ tactics, tools and procedures</span></span>
* <span data-ttu-id="82cfb-115">Associerade indikatorer för kompromettering, till exempel URL:er och filhashvärden</span><span class="sxs-lookup"><span data-stu-id="82cfb-115">Associated indicators of compromise (IoC) such as URLs and file hashes</span></span>
* <span data-ttu-id="82cfb-116">Viktimologi, vilket syftar på information om utbredningen inom en bransch och geografisk region som kan hjälpa dig att avgöra om dina Azure-resurser är utsatta för risk</span><span class="sxs-lookup"><span data-stu-id="82cfb-116">Victimology, which is the industry and geographic prevalence to assist you in determining if your Azure resources are at risk</span></span>
* <span data-ttu-id="82cfb-117">Information om rekommenderade åtgärder</span><span class="sxs-lookup"><span data-stu-id="82cfb-117">Mitigation and remediation information</span></span>

> [!NOTE]
> <span data-ttu-id="82cfb-118">Mängden information i en viss rapport varierar. Detaljnivån beror på den skadliga kodens aktivitet och spridning.</span><span class="sxs-lookup"><span data-stu-id="82cfb-118">The amount of information in any particular report will vary; the level of detail is based on the malware’s activity and prevalence.</span></span>
>
>

<span data-ttu-id="82cfb-119">Security Center tillhandahåller tre typer av hotrapporter, som kan variera beroende på attacken.</span><span class="sxs-lookup"><span data-stu-id="82cfb-119">Security Center has three types of threat reports, which can vary according to the attack.</span></span> <span data-ttu-id="82cfb-120">De tillgängliga rapporterna är:</span><span class="sxs-lookup"><span data-stu-id="82cfb-120">The reports available are:</span></span>

* <span data-ttu-id="82cfb-121">**Aktivitetsgruppsrapport**: Tillhandahåller detaljerad information om angripare, deras mål och metoder.</span><span class="sxs-lookup"><span data-stu-id="82cfb-121">**Activity Group Report**: provides deep dives into attackers, their objectives and tactics.</span></span>
* <span data-ttu-id="82cfb-122">**Kampanjrapport**: Fokuserar på information om specifika attackkampanjer.</span><span class="sxs-lookup"><span data-stu-id="82cfb-122">**Campaign Report**: focuses on details of specific attack campaigns.</span></span>
* <span data-ttu-id="82cfb-123">**Hotsammanfattningsrapport**: Omfattar alla objekt i de föregående två rapporterna.</span><span class="sxs-lookup"><span data-stu-id="82cfb-123">**Threat Summary Report**: covers all of the items in the previous two reports.</span></span>

<span data-ttu-id="82cfb-124">Den här typen av information är mycket användbar under [incidenthanteringsprocessen](security-center-incident-response.md), där man genom kontinuerliga undersökningar försöker förstå attackens källa, angriparens avsikter och hur problemet kan åtgärdas.</span><span class="sxs-lookup"><span data-stu-id="82cfb-124">This type of information is very useful during the [incident response](security-center-incident-response.md) process, where there is an ongoing investigation to understand the source of the attack, the attacker’s motivations, and what to do to mitigate this issue moving forward.</span></span>

## <a name="how-to-access-the-threat-intelligence-report"></a><span data-ttu-id="82cfb-125">Hur kommer du åt hotinformationsrapporten?</span><span class="sxs-lookup"><span data-stu-id="82cfb-125">How to access the threat intelligence report?</span></span>
<span data-ttu-id="82cfb-126">Du kan se aktuella aviseringar i rutan **Security alerts (Säkerhetsaviseringar)**.</span><span class="sxs-lookup"><span data-stu-id="82cfb-126">You can review your current alerts by looking at the **Security alerts** tile.</span></span> <span data-ttu-id="82cfb-127">Öppna Azure Portal och följ stegen nedan om du vill ha mer information om varje typ av varning:</span><span class="sxs-lookup"><span data-stu-id="82cfb-127">Open the Azure Portal and follow the steps below to see more details about each alert:</span></span>

1. <span data-ttu-id="82cfb-128">På instrumentpanelen i Security Center hittar du rutan **Security alerts (Säkerhetsaviseringar)**.</span><span class="sxs-lookup"><span data-stu-id="82cfb-128">On the Security Center dashboard, you will see the **Security alerts** tile.</span></span>
2. <span data-ttu-id="82cfb-129">Klicka på ikonen för att öppna bladet **Säkerhetsvarningar** som innehåller mer information om aviseringarna och klicka sedan på den säkerhetsvarning som du vill visa mer information om.</span><span class="sxs-lookup"><span data-stu-id="82cfb-129">Click the tile to open the **Security alerts** blade that contains more details about the alerts and click in the security alert that you want to obtain more information about.</span></span>

    ![Säkerhetsaviseringar](./media/security-center-threat-report/security-center-threat-report-fig1.png)
3. <span data-ttu-id="82cfb-131">I vårt exempel visar bladet **Suspicious process executed** (Misstänkt process körs) information om aviseringen, som du ser i bilden nedan:</span><span class="sxs-lookup"><span data-stu-id="82cfb-131">In this case the **Suspicious process executed** blade shows the details about the alert as shown in the figure below:</span></span>

    ![Utförlig information om säkerhetsaviseringar](./media/security-center-threat-report/security-center-threat-report-fig2.png)
4. <span data-ttu-id="82cfb-133">Mängden information som är tillgänglig för varje säkerhetsvarning varierar beroende på typen av avisering.</span><span class="sxs-lookup"><span data-stu-id="82cfb-133">The amount of information available for each security alert will vary according to the type of alert.</span></span> <span data-ttu-id="82cfb-134">Fältet **RAPPORTER** innehåller en länk till hotinformationsrapporten.</span><span class="sxs-lookup"><span data-stu-id="82cfb-134">In the **REPORTS** field you have a link to the threat intelligence report.</span></span> <span data-ttu-id="82cfb-135">Klicka på länken så öppnas ett nytt webbläsarfönster med en PDF-fil.</span><span class="sxs-lookup"><span data-stu-id="82cfb-135">Click on it and another browser window will appear with PDF file.</span></span>

   ![Val av lagringsutrymme](./media/security-center-threat-report/security-center-threat-report-fig3.png)

<span data-ttu-id="82cfb-137">Härifrån kan du hämta PDF-filen för den här rapporten och läsa mer om säkerhetsproblemet som har identifierats samt vidta åtgärder baserat på informationen.</span><span class="sxs-lookup"><span data-stu-id="82cfb-137">From here you can download the PDF for this report and read more about the security issue that was detected and take actions based on the information provided.</span></span>

## <a name="see-also"></a><span data-ttu-id="82cfb-138">Se även</span><span class="sxs-lookup"><span data-stu-id="82cfb-138">See also</span></span>
<span data-ttu-id="82cfb-139">I det här dokumentet har vi tittat på hur hotinformationsrapporterna i Azure Security Center kan hjälpa dig när du undersöker säkerhetsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="82cfb-139">In this document, you learned how Azure Security Center Threat Intelligent Reports can help during an investigation about security alerts.</span></span> <span data-ttu-id="82cfb-140">I följande avsnitt kan du lära dig mer om Azure Security Center:</span><span class="sxs-lookup"><span data-stu-id="82cfb-140">To learn more about Azure Security Center, see the following:</span></span>

* <span data-ttu-id="82cfb-141">[Vanliga frågor och svar om Azure Security Center](security-center-faq.md).</span><span class="sxs-lookup"><span data-stu-id="82cfb-141">[Azure Security Center FAQ](security-center-faq.md).</span></span> <span data-ttu-id="82cfb-142">Här finns vanliga frågor om att använda tjänsten.</span><span class="sxs-lookup"><span data-stu-id="82cfb-142">Find frequently asked questions about using the service.</span></span>
* [<span data-ttu-id="82cfb-143">Använda Azure Security Center vid incidenthantering</span><span class="sxs-lookup"><span data-stu-id="82cfb-143">Leveraging Azure Security Center for Incident Response</span></span>](security-center-incident-response.md)
* [<span data-ttu-id="82cfb-144">Identifieringsfunktioner i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="82cfb-144">Azure Security Center detection capabilities</span></span>](security-center-detection-capabilities.md)
* <span data-ttu-id="82cfb-145">[Planerings- och bruksanvisning för Azure Security Center](security-center-planning-and-operations-guide.md).</span><span class="sxs-lookup"><span data-stu-id="82cfb-145">[Azure Security Center planning and operations guide](security-center-planning-and-operations-guide.md).</span></span> <span data-ttu-id="82cfb-146">Lär dig mer om planering och viktiga designaspekter när du ska börja använda Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="82cfb-146">Learn how to plan and understand the design considerations to adopt Azure Security Center.</span></span>
* <span data-ttu-id="82cfb-147">[Hantera och åtgärda säkerhetsaviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="82cfb-147">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md).</span></span> <span data-ttu-id="82cfb-148">Lär dig att hantera och åtgärda säkerhetsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="82cfb-148">Learn how to manage and respond to security alerts.</span></span>
* [<span data-ttu-id="82cfb-149">Hantera säkerhetsincidenter i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="82cfb-149">Handling Security Incident in Azure Security Center</span></span>](security-center-incident.md)
* <span data-ttu-id="82cfb-150">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/).</span><span class="sxs-lookup"><span data-stu-id="82cfb-150">[Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/).</span></span> <span data-ttu-id="82cfb-151">Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure.</span><span class="sxs-lookup"><span data-stu-id="82cfb-151">Find blog posts about Azure security and compliance.</span></span>
