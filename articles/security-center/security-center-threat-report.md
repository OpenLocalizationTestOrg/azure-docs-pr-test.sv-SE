---
title: aaaAzure Security Center hot intelligence rapporten | Microsoft Docs
description: "Det här dokumentet hjälper dig att toouse Azure Security Center hot Intelligent rapporter under en undersökning toofind mer information om en säkerhetsavisering."
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
ms.openlocfilehash: c888cfac1dd8b057616a6b8e6c6f6b67b552f2e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-threat-intelligence-report"></a><span data-ttu-id="1ded6-103">Hotinformationsrapporter i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="1ded6-103">Azure Security Center Threat Intelligence Report</span></span>
<span data-ttu-id="1ded6-104">Det här dokumentet beskriver hur du kan lära dig mer om ett hot som genererat en säkerhetsvarning med hjälp av hotinformationsrapporter i Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="1ded6-104">This document explains how Azure Security Center Threat Intelligent Reports can help you learn more about a threat that generated a security alert.</span></span>

## <a name="what-is-a-threat-intelligence-report"></a><span data-ttu-id="1ded6-105">Vad är en hotinformationsrapport?</span><span class="sxs-lookup"><span data-stu-id="1ded6-105">What is a threat intelligence report?</span></span>
<span data-ttu-id="1ded6-106">Hotidentifiering Security Center fungerar genom att övervaka säkerhetsinformation från Azure-resurser, hello nätverket och anslutna partnerlösningar.</span><span class="sxs-lookup"><span data-stu-id="1ded6-106">Security Center threat detection works by monitoring security information from your Azure resources, hello network, and connected partner solutions.</span></span> <span data-ttu-id="1ded6-107">Den här informationen kan ofta korrelerar information från flera källor, tooidentify hot analyseras.</span><span class="sxs-lookup"><span data-stu-id="1ded6-107">It analyzes this information, often correlating information from multiple sources, tooidentify threats.</span></span> <span data-ttu-id="1ded6-108">Den här processen är en del av hello Security Center [identifieringsfunktionerna](security-center-detection-capabilities.md).</span><span class="sxs-lookup"><span data-stu-id="1ded6-108">This process is part of hello Security Center [detection capabilities](security-center-detection-capabilities.md).</span></span>

<span data-ttu-id="1ded6-109">När Security Center identifierar ett hot utlöses en [säkerhetsvarning](security-center-managing-and-responding-alerts.md), som innehåller detaljerad information om en viss händelse, inklusive förslag på åtgärder.</span><span class="sxs-lookup"><span data-stu-id="1ded6-109">When Security Center identifies a threat, it will trigger a [security alert](security-center-managing-and-responding-alerts.md), which contains detailed information regarding a particular event, including suggestions for remediation.</span></span> <span data-ttu-id="1ded6-110">tooassist incidenter team undersöka och åtgärda hot, Security Center har en hot intelligence-rapport som innehåller information om hello hot som har identifierats, inklusive information som den:</span><span class="sxs-lookup"><span data-stu-id="1ded6-110">tooassist incident response teams investigate and remediate threats, Security Center includes a threat intelligence report that contains information about hello threat that was detected, including information such as the:</span></span>

* <span data-ttu-id="1ded6-111">Angriparens identitet eller associationer (om den här informationen är tillgänglig)</span><span class="sxs-lookup"><span data-stu-id="1ded6-111">Attacker’s identity or associations (if this information is available)</span></span>
* <span data-ttu-id="1ded6-112">Angriparens mål</span><span class="sxs-lookup"><span data-stu-id="1ded6-112">Attackers’ objectives</span></span>
* <span data-ttu-id="1ded6-113">Aktuella och historiska attackkampanjer (om den här informationen är tillgänglig)</span><span class="sxs-lookup"><span data-stu-id="1ded6-113">Current and historical attack campaigns (if this information is available)</span></span>
* <span data-ttu-id="1ded6-114">Angriparens metoder, verktyg och procedurer</span><span class="sxs-lookup"><span data-stu-id="1ded6-114">Attackers’ tactics, tools and procedures</span></span>
* <span data-ttu-id="1ded6-115">Associerade indikatorer för kompromettering, till exempel URL:er och filhashvärden</span><span class="sxs-lookup"><span data-stu-id="1ded6-115">Associated indicators of compromise (IoC) such as URLs and file hashes</span></span>
* <span data-ttu-id="1ded6-116">Victimology som är hello branschen och geografiska förekomsten tooassist du för att fastställa om din Azure-resurser som är i fara</span><span class="sxs-lookup"><span data-stu-id="1ded6-116">Victimology, which is hello industry and geographic prevalence tooassist you in determining if your Azure resources are at risk</span></span>
* <span data-ttu-id="1ded6-117">Information om rekommenderade åtgärder</span><span class="sxs-lookup"><span data-stu-id="1ded6-117">Mitigation and remediation information</span></span>

> [!NOTE]
> <span data-ttu-id="1ded6-118">hello mängden information i någon viss rapport varierar; hello detaljnivå baseras på hello skadlig aktivitet och få information om användningsmönster.</span><span class="sxs-lookup"><span data-stu-id="1ded6-118">hello amount of information in any particular report will vary; hello level of detail is based on hello malware’s activity and prevalence.</span></span>
>
>

<span data-ttu-id="1ded6-119">Security Center har tre typer av hot rapporter som kan variera bl.a toohello attack.</span><span class="sxs-lookup"><span data-stu-id="1ded6-119">Security Center has three types of threat reports, which can vary according toohello attack.</span></span> <span data-ttu-id="1ded6-120">hello-rapporter som är tillgängliga är:</span><span class="sxs-lookup"><span data-stu-id="1ded6-120">hello reports available are:</span></span>

* <span data-ttu-id="1ded6-121">**Aktivitetsgruppsrapport**: Tillhandahåller detaljerad information om angripare, deras mål och metoder.</span><span class="sxs-lookup"><span data-stu-id="1ded6-121">**Activity Group Report**: provides deep dives into attackers, their objectives and tactics.</span></span>
* <span data-ttu-id="1ded6-122">**Kampanjrapport**: Fokuserar på information om specifika attackkampanjer.</span><span class="sxs-lookup"><span data-stu-id="1ded6-122">**Campaign Report**: focuses on details of specific attack campaigns.</span></span>
* <span data-ttu-id="1ded6-123">**Hot sammanfattningsrapport över**: omfattar alla hello objekt i hello föregående två rapporter.</span><span class="sxs-lookup"><span data-stu-id="1ded6-123">**Threat Summary Report**: covers all of hello items in hello previous two reports.</span></span>

<span data-ttu-id="1ded6-124">Den här typen av information är mycket användbar vid hello [incidentsvar](security-center-incident-response.md) processer, där det finns en pågående undersökning toounderstand hello källa för hello attack hello angripare motiveringen och vad toodo toomitigate detta problemet vidarebefordras.</span><span class="sxs-lookup"><span data-stu-id="1ded6-124">This type of information is very useful during hello [incident response](security-center-incident-response.md) process, where there is an ongoing investigation toounderstand hello source of hello attack, hello attacker’s motivations, and what toodo toomitigate this issue moving forward.</span></span>

## <a name="how-tooaccess-hello-threat-intelligence-report"></a><span data-ttu-id="1ded6-125">Hur tooaccess hello hot intelligence-rapporten?</span><span class="sxs-lookup"><span data-stu-id="1ded6-125">How tooaccess hello threat intelligence report?</span></span>
<span data-ttu-id="1ded6-126">Du kan granska aktuella aviseringar genom att titta på hello **säkerhetsaviseringar** panelen.</span><span class="sxs-lookup"><span data-stu-id="1ded6-126">You can review your current alerts by looking at hello **Security alerts** tile.</span></span> <span data-ttu-id="1ded6-127">Öppna hello Azure-portalen och gör hello nedan toosee mer information om varje avisering:</span><span class="sxs-lookup"><span data-stu-id="1ded6-127">Open hello Azure Portal and follow hello steps below toosee more details about each alert:</span></span>

1. <span data-ttu-id="1ded6-128">På instrumentpanelen för hello Security Center ser du hello **säkerhetsaviseringar** panelen.</span><span class="sxs-lookup"><span data-stu-id="1ded6-128">On hello Security Center dashboard, you will see hello **Security alerts** tile.</span></span>
2. <span data-ttu-id="1ded6-129">Klicka på hello panelen tooopen hello **säkerhetsaviseringar** blad som innehåller mer information om hello aviseringar och klicka på i hello Säkerhetsvarning som du vill tooobtain mer information om.</span><span class="sxs-lookup"><span data-stu-id="1ded6-129">Click hello tile tooopen hello **Security alerts** blade that contains more details about hello alerts and click in hello security alert that you want tooobtain more information about.</span></span>

    ![Säkerhetsaviseringar](./media/security-center-threat-report/security-center-threat-report-fig1.png)
3. <span data-ttu-id="1ded6-131">I det här fallet hello **misstänkta processen körs** bladet visar hello information om hello avisering som visas i hello bilden nedan:</span><span class="sxs-lookup"><span data-stu-id="1ded6-131">In this case hello **Suspicious process executed** blade shows hello details about hello alert as shown in hello figure below:</span></span>

    ![Utförlig information om säkerhetsaviseringar](./media/security-center-threat-report/security-center-threat-report-fig2.png)
4. <span data-ttu-id="1ded6-133">hello mängden information som är tillgängliga för varje Säkerhetsvarning varierar bl.a toohello typen av avisering.</span><span class="sxs-lookup"><span data-stu-id="1ded6-133">hello amount of information available for each security alert will vary according toohello type of alert.</span></span> <span data-ttu-id="1ded6-134">I hello **rapporter** fält som du har en toohello hot intelligence rapport.</span><span class="sxs-lookup"><span data-stu-id="1ded6-134">In hello **REPORTS** field you have a link toohello threat intelligence report.</span></span> <span data-ttu-id="1ded6-135">Klicka på länken så öppnas ett nytt webbläsarfönster med en PDF-fil.</span><span class="sxs-lookup"><span data-stu-id="1ded6-135">Click on it and another browser window will appear with PDF file.</span></span>

   ![Val av lagringsutrymme](./media/security-center-threat-report/security-center-threat-report-fig3.png)

<span data-ttu-id="1ded6-137">Du kan hämta hello PDF-filen för den här rapporten och Läs mer om hello säkerhet problem som har identifierats och vidta åtgärder utifrån hello informationen här.</span><span class="sxs-lookup"><span data-stu-id="1ded6-137">From here you can download hello PDF for this report and read more about hello security issue that was detected and take actions based on hello information provided.</span></span>

## <a name="see-also"></a><span data-ttu-id="1ded6-138">Se även</span><span class="sxs-lookup"><span data-stu-id="1ded6-138">See also</span></span>
<span data-ttu-id="1ded6-139">I det här dokumentet har vi tittat på hur hotinformationsrapporterna i Azure Security Center kan hjälpa dig när du undersöker säkerhetsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="1ded6-139">In this document, you learned how Azure Security Center Threat Intelligent Reports can help during an investigation about security alerts.</span></span> <span data-ttu-id="1ded6-140">toolearn mer om Azure Security Center finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="1ded6-140">toolearn more about Azure Security Center, see hello following:</span></span>

* <span data-ttu-id="1ded6-141">[Vanliga frågor och svar om Azure Security Center](security-center-faq.md).</span><span class="sxs-lookup"><span data-stu-id="1ded6-141">[Azure Security Center FAQ](security-center-faq.md).</span></span> <span data-ttu-id="1ded6-142">Sök efter vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1ded6-142">Find frequently asked questions about using hello service.</span></span>
* [<span data-ttu-id="1ded6-143">Använda Azure Security Center vid incidenthantering</span><span class="sxs-lookup"><span data-stu-id="1ded6-143">Leveraging Azure Security Center for Incident Response</span></span>](security-center-incident-response.md)
* [<span data-ttu-id="1ded6-144">Identifieringsfunktioner i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="1ded6-144">Azure Security Center detection capabilities</span></span>](security-center-detection-capabilities.md)
* <span data-ttu-id="1ded6-145">[Planerings- och bruksanvisning för Azure Security Center](security-center-planning-and-operations-guide.md).</span><span class="sxs-lookup"><span data-stu-id="1ded6-145">[Azure Security Center planning and operations guide](security-center-planning-and-operations-guide.md).</span></span> <span data-ttu-id="1ded6-146">Lär dig hur tooplan och förstå hello design överväganden tooadopt Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="1ded6-146">Learn how tooplan and understand hello design considerations tooadopt Azure Security Center.</span></span>
* <span data-ttu-id="1ded6-147">[Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="1ded6-147">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md).</span></span> <span data-ttu-id="1ded6-148">Lär dig hur toomanage och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="1ded6-148">Learn how toomanage and respond toosecurity alerts.</span></span>
* [<span data-ttu-id="1ded6-149">Hantera säkerhetsincidenter i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="1ded6-149">Handling Security Incident in Azure Security Center</span></span>](security-center-incident.md)
* <span data-ttu-id="1ded6-150">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/).</span><span class="sxs-lookup"><span data-stu-id="1ded6-150">[Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/).</span></span> <span data-ttu-id="1ded6-151">Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure.</span><span class="sxs-lookup"><span data-stu-id="1ded6-151">Find blog posts about Azure security and compliance.</span></span>
