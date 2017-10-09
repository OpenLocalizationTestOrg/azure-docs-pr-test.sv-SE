---
title: "aaaHandling säkerhetsaviseringar i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet hjälper dig att toouse Azure Security Center funktioner toohandle säkerhetsincidenter."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: e8feb669-8f30-49eb-ba38-046edf3f9656
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: yurid
ms.openlocfilehash: edb911c298a2ce93cd0ea5b22ce002005040090f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="handling-security-incidents-in-azure-security-center"></a><span data-ttu-id="1e6d6-103">Hantera säkerhetsincidenter i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="1e6d6-103">Handling Security Incidents in Azure Security Center</span></span>
<span data-ttu-id="1e6d6-104">Triaging och undersöker säkerhetsaviseringar kan ta lång tid för även hello mest skicklig säkerhetsanalytiker och många är det svårt tooeven vet var toobegin.</span><span class="sxs-lookup"><span data-stu-id="1e6d6-104">Triaging and investigating security alerts can be time consuming for even hello most skilled security analysts, and for many it is hard tooeven know where toobegin.</span></span> <span data-ttu-id="1e6d6-105">Med hjälp av [analytics](security-center-detection-capabilities.md) tooconnect hello information mellan olika [säkerhetsaviseringar](security-center-managing-and-responding-alerts.md), Security Center kan förse dig med en enda vy av en attack kampanj och alla hello relaterade aviseringar – du kan snabbt förstå vilka åtgärder hello angripare tog och vilka resurser har påverkas.</span><span class="sxs-lookup"><span data-stu-id="1e6d6-105">By using [analytics](security-center-detection-capabilities.md) tooconnect hello information between distinct [security alerts](security-center-managing-and-responding-alerts.md), Security Center can provide you with a single view of an attack campaign and all of hello related alerts – you can quickly understand what actions hello attacker took and what resources were impacted.</span></span>

<span data-ttu-id="1e6d6-106">Det här dokumentet beskrivs hur toouse säkerhet varnar kapaciteten i Security Center tooassist hantering av säkerhetsincidenter.</span><span class="sxs-lookup"><span data-stu-id="1e6d6-106">This document discusses how toouse security alert capability in Security Center tooassist you handling security incidents.</span></span>

## <a name="what-is-a-security-incident"></a><span data-ttu-id="1e6d6-107">Vad är en säkerhetsincident?</span><span class="sxs-lookup"><span data-stu-id="1e6d6-107">What is a security incident?</span></span>
<span data-ttu-id="1e6d6-108">I Security Center är en säkerhetsincident en sammanställning av alla aviseringar för en resurs som överensstämmer med särskilda [händelsekedjemönster](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/).</span><span class="sxs-lookup"><span data-stu-id="1e6d6-108">In Security Center, a security incident is an aggregation of all alerts for a resource that align with [kill chain](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) patterns.</span></span> <span data-ttu-id="1e6d6-109">Incidenter visas i hello [säkerhetsaviseringar](security-center-managing-and-responding-alerts.md) panelen och bladet.</span><span class="sxs-lookup"><span data-stu-id="1e6d6-109">Incidents appear in hello [Security Alerts](security-center-managing-and-responding-alerts.md) tile and blade.</span></span> <span data-ttu-id="1e6d6-110">En Incident avslöja hello lista över relaterade aviseringar, vilket gör att du tooobtain mer information om varje förekomst.</span><span class="sxs-lookup"><span data-stu-id="1e6d6-110">An Incident will reveal hello list of related alerts, which enables you tooobtain more information about each occurrence.</span></span>

## <a name="managing-security-incidents"></a><span data-ttu-id="1e6d6-111">Hantera säkerhetsincidenter</span><span class="sxs-lookup"><span data-stu-id="1e6d6-111">Managing security incidents</span></span>
<span data-ttu-id="1e6d6-112">Du kan granska din aktuella säkerhetsincidenter genom att titta på hello säkerhetsrutan aviseringar.</span><span class="sxs-lookup"><span data-stu-id="1e6d6-112">You can review your current security incidents by looking at hello security alerts tile.</span></span> <span data-ttu-id="1e6d6-113">Komma åt hello Azure-portalen och gör hello nedan toosee mer information om varje säkerhetsincident:</span><span class="sxs-lookup"><span data-stu-id="1e6d6-113">Access hello Azure Portal and follow hello steps below toosee more details about each security incident:</span></span>

1. <span data-ttu-id="1e6d6-114">På instrumentpanelen för hello Security Center ser du hello **säkerhetsaviseringar** panelen.</span><span class="sxs-lookup"><span data-stu-id="1e6d6-114">On hello Security Center dashboard, you will see hello **Security alerts** tile.</span></span>

    ![Panelen Säkerhetsaviseringar i Security Center](./media/security-center-incident/security-center-incident-fig1.png)

2. <span data-ttu-id="1e6d6-116">Klicka på den här panelen tooexpand och om en säkerhetsincident har identifierats, visas den under hello säkerhet aviseringar diagram som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="1e6d6-116">Click on this tile tooexpand it and if a security incident is detected, it will appear under hello security alerts graph as shown  below:</span></span>

    ![Säkerhetsincident](./media/security-center-incident/security-center-incident-fig2.png)

3. <span data-ttu-id="1e6d6-118">Observera att hello säkerhet incidentbeskrivning har olika ikoner jämfört med tooother aviseringar.</span><span class="sxs-lookup"><span data-stu-id="1e6d6-118">Notice that hello security incident description has a different icon compared tooother alerts.</span></span> <span data-ttu-id="1e6d6-119">Klicka på den tooview mer information om den här incidenten.</span><span class="sxs-lookup"><span data-stu-id="1e6d6-119">Click on it tooview more details about this incident.</span></span>

    ![Säkerhetsincident](./media/security-center-incident/security-center-incident-fig3.png)

4. <span data-ttu-id="1e6d6-121">På hello **incident** bladet visas mer information om den här säkerhetsincident som innehåller en fullständig beskrivning, dess allvarlighetsgrad (som i det här fallet är hög), det aktuella tillståndet (i det här fallet är det fortfarande *active*, vilket innebär att hello användaren har inte tagit en åtgärd tooit – kan du göra det genom att högerklicka på hello incident i hello **säkerhetsaviseringar** bladet), hello angripna resursen (i det här fallet *VM1*) hello steg för hello incidenten och i hello längst ned i fönstret har du hello aviseringar som ingår i den här incidenten.</span><span class="sxs-lookup"><span data-stu-id="1e6d6-121">On hello **incident** blade you will see more details about this security incident, which includes its full description, its severity (which in this case is high), its current state (in this case it is still *active*, which implies hello user hasn't taken an action tooit - this can be done by right clicking on hello incident in hello **Security alerts** blade), hello attacked resource (in this case *VM1*), hello remediation steps for hello incident, and in hello bottom pane you have hello alerts that were included in this incident.</span></span> <span data-ttu-id="1e6d6-122">Om du vill tooobtain mer information om varje avisering, öppnas Klicka på den och ett nytt blad som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="1e6d6-122">If you want tooobtain more information on each alert, just click on it and another blade will open, as shown below:</span></span>

    ![Säkerhetsincident](./media/security-center-incident/security-center-incident-fig4.png)

<span data-ttu-id="1e6d6-124">hello information om det här bladet varierar bl.a toohello avisering.</span><span class="sxs-lookup"><span data-stu-id="1e6d6-124">hello information on this blade will vary according toohello alert.</span></span> <span data-ttu-id="1e6d6-125">Läs [hantering och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) för mer information om hur toomanage aviseringarna.</span><span class="sxs-lookup"><span data-stu-id="1e6d6-125">Read [Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) for more information on how toomanage these alerts.</span></span> <span data-ttu-id="1e6d6-126">Viktigt att tänka på för den här funktionen:</span><span class="sxs-lookup"><span data-stu-id="1e6d6-126">Some important considerations regarding this capability:</span></span>

* <span data-ttu-id="1e6d6-127">Ett nytt filter kan du toocustomize din tooIncident Visa endast aviseringar endast eller båda.</span><span class="sxs-lookup"><span data-stu-id="1e6d6-127">A new filter enables you toocustomize your view tooIncident only, Alerts only, or both.</span></span>
* <span data-ttu-id="1e6d6-128">hello samma avisering kan finnas som en del av en Incident (om tillämpligt), samt toobe visas som en fristående avisering.</span><span class="sxs-lookup"><span data-stu-id="1e6d6-128">hello same alert can exist as part of an Incident (if applicable), as well as toobe visible as a standalone alert.</span></span>

## <a name="see-also"></a><span data-ttu-id="1e6d6-129">Se även</span><span class="sxs-lookup"><span data-stu-id="1e6d6-129">See also</span></span>
<span data-ttu-id="1e6d6-130">I det här dokumentet du har lärt dig hur toouse hello säkerhet incident kapaciteten i Security Center.</span><span class="sxs-lookup"><span data-stu-id="1e6d6-130">In this document, you learned how toouse hello security incident capability in Security Center.</span></span> <span data-ttu-id="1e6d6-131">toolearn mer om Security Center finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="1e6d6-131">toolearn more about Security Center, see hello following:</span></span>

* [<span data-ttu-id="1e6d6-132">Hantera och åtgärda toosecurity aviseringar i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="1e6d6-132">Managing and responding toosecurity alerts in Azure Security Center</span></span>](security-center-managing-and-responding-alerts.md)
* [<span data-ttu-id="1e6d6-133">Identifieringsfunktioner i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="1e6d6-133">Azure Security Center Detection Capabilities</span></span>](security-center-detection-capabilities.md)
* [<span data-ttu-id="1e6d6-134">Planerings- och bruksanvisning för Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="1e6d6-134">Azure Security Center Planning and Operations Guide</span></span>](security-center-planning-and-operations-guide.md)
* [<span data-ttu-id="1e6d6-135">Hantera och åtgärda toosecurity aviseringar i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="1e6d6-135">Managing and responding toosecurity alerts in Azure Security Center</span></span>](security-center-managing-and-responding-alerts.md)
* <span data-ttu-id="1e6d6-136">[Vanliga frågor om Azure Security Center](security-center-faq.md)--finns vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1e6d6-136">[Azure Security Center FAQ](security-center-faq.md)--Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="1e6d6-137">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/): Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure.</span><span class="sxs-lookup"><span data-stu-id="1e6d6-137">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/)--Find blog posts about Azure security and compliance.</span></span>
