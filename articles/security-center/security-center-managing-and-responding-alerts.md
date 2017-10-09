---
title: "aaaManage säkerhetsaviseringar i Azure Security Center | Microsoft Docs"
description: "Det här dokumentet hjälper du toouse Azure Security Center funktioner toomanage och svara toosecurity aviseringar."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: b88a8df7-6979-479b-8039-04da1b8737a7
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: yurid
ms.openlocfilehash: f1cb7e4770776827b75ed15893914678c1f44216
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-and-responding-toosecurity-alerts-in-azure-security-center"></a><span data-ttu-id="4c8e6-103">Hantera och åtgärda toosecurity aviseringar i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="4c8e6-103">Managing and responding toosecurity alerts in Azure Security Center</span></span>
<span data-ttu-id="4c8e6-104">Det här dokumentet hjälper dig att använda Azure Security Center toomanage och svara toosecurity aviseringar.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-104">This document helps you use Azure Security Center toomanage and respond toosecurity alerts.</span></span>

> [!NOTE]
> <span data-ttu-id="4c8e6-105">tooenable avancerade identifieringar, uppgradera tooAzure Security Center Standard.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-105">tooenable advanced detections, upgrade tooAzure Security Center Standard.</span></span> <span data-ttu-id="4c8e6-106">En kostnadsfri 60-dagars utvärderingsversion är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-106">A free 60-day trial is available.</span></span> <span data-ttu-id="4c8e6-107">tooupgrade, Välj prisnivån i hello [säkerhetsprincip](security-center-policies.md).</span><span class="sxs-lookup"><span data-stu-id="4c8e6-107">tooupgrade, select Pricing Tier in hello [Security Policy](security-center-policies.md).</span></span> <span data-ttu-id="4c8e6-108">Se [priser för Azure Security Center](security-center-pricing.md) toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-108">See [Azure Security Center pricing](security-center-pricing.md) toolearn more.</span></span>
>
>

## <a name="what-are-security-alerts"></a><span data-ttu-id="4c8e6-109">Vad är säkerhetsaviseringar?</span><span class="sxs-lookup"><span data-stu-id="4c8e6-109">What are security alerts?</span></span>
<span data-ttu-id="4c8e6-110">Security Center automatiskt samlar in, analyserar, och integrerar loggdata från resurserna i Azure, hello nätverk och anslutna partnerlösningar som brandväggen och endpoint protection lösningar, toodetect verkliga hot och falsklarm.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-110">Security Center automatically collects, analyzes, and integrates log data from your Azure resources, hello network, and connected partner solutions, like firewall and endpoint protection solutions, toodetect real threats and reduce false positives.</span></span> <span data-ttu-id="4c8e6-111">En lista med prioritetssorterade säkerhetsaviseringar visas i Security Center tillsammans med hello information du behöver tooquickly undersöka hello problem och rekommendationer för hur tooremediate angreppet.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-111">A list of prioritized security alerts is shown in Security Center along with hello information you need tooquickly investigate hello problem and recommendations for how tooremediate an attack.</span></span>


> [!NOTE]
> <span data-ttu-id="4c8e6-112">Mer information om hur identifieringsfunktionerna i Security Center fungerar finns i [Identifieringsfunktioner i Azure Security Center](security-center-detection-capabilities.md).</span><span class="sxs-lookup"><span data-stu-id="4c8e6-112">For more information about how Security Center detection capabilities work, read [Azure Security Center Detection Capabilities](security-center-detection-capabilities.md).</span></span>
>
>

## <a name="managing-security-alerts"></a><span data-ttu-id="4c8e6-113">Hantera säkerhetsaviseringar</span><span class="sxs-lookup"><span data-stu-id="4c8e6-113">Managing security alerts</span></span>
<span data-ttu-id="4c8e6-114">Du kan granska aktuella aviseringar genom att titta på hello **säkerhetsaviseringar** panelen.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-114">You can review your current alerts by looking at hello **Security alerts** tile.</span></span> <span data-ttu-id="4c8e6-115">Öppna Azure-portalen och gör hello nedan toosee mer information om varje avisering:</span><span class="sxs-lookup"><span data-stu-id="4c8e6-115">Open Azure Portal and follow hello steps below toosee more details about each alert:</span></span>

1. <span data-ttu-id="4c8e6-116">På instrumentpanelen för hello Security Center ser du hello **säkerhetsaviseringar** panelen.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-116">On hello Security Center dashboard, you will see hello **Security alerts** tile.</span></span>

    ![Panelen Säkerhetsaviseringar i Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig1-ga.png)

2. <span data-ttu-id="4c8e6-118">Klicka på hello panelen tooopen hello **säkerhetsaviseringar** blad som innehåller mer information om hello aviseringar som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-118">Click hello tile tooopen hello **Security alerts** blade that contains more details about hello alerts as shown below.</span></span>

   ![hello bladet med säkerhetsaviseringar i Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig2-ga.png)

<span data-ttu-id="4c8e6-120">Hello längst ned i det här bladet är hello information för varje avisering.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-120">In hello bottom part of this blade are hello details for each alert.</span></span> <span data-ttu-id="4c8e6-121">toosort, klicka på hello-kolumn som du vill toosort av.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-121">toosort, click hello column that you want toosort by.</span></span> <span data-ttu-id="4c8e6-122">hello definition för varje kolumn anges nedan:</span><span class="sxs-lookup"><span data-stu-id="4c8e6-122">hello definition for each column is given below:</span></span>

* <span data-ttu-id="4c8e6-123">**Beskrivning**: en kort förklaring av hello avisering.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-123">**Description**: A brief explanation of hello alert.</span></span>
* <span data-ttu-id="4c8e6-124">**Count (Antal)**: antalet problem av just den här typen som upptäckts en specifik dag</span><span class="sxs-lookup"><span data-stu-id="4c8e6-124">**Count**: A list of all alerts of this specific type that were detected on a specific day.</span></span>
* <span data-ttu-id="4c8e6-125">**Upptäckt av**: hello tjänst som utlöste aviseringen hello.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-125">**Detected by**: hello service that was responsible for triggering hello alert.</span></span>
* <span data-ttu-id="4c8e6-126">**Datum**: hello datum hello händelsen inträffade.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-126">**Date**: hello date that hello event occurred.</span></span>
* <span data-ttu-id="4c8e6-127">**Tillstånd**: hello aktuell status för den här aviseringen.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-127">**State**: hello current state for that alert.</span></span> <span data-ttu-id="4c8e6-128">Det finns två tillstånd:</span><span class="sxs-lookup"><span data-stu-id="4c8e6-128">There are two types of states:</span></span>
  * <span data-ttu-id="4c8e6-129">**Aktiva**: hello Säkerhetsvarning har upptäckts.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-129">**Active**: hello security alert has been detected.</span></span>
* <span data-ttu-id="4c8e6-130">**Allvarlighetsgrad**: hello allvarlighetsgrad, vilket kan vara hög, medel eller låg.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-130">**Severity**: hello severity level, which can be high, medium or low.</span></span>

### <a name="filtering-alerts"></a><span data-ttu-id="4c8e6-131">Filtrera varningar</span><span class="sxs-lookup"><span data-stu-id="4c8e6-131">Filtering alerts</span></span>
<span data-ttu-id="4c8e6-132">Aviseringarna kan filtreras efter datum, status och allvarlighetsgrad.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-132">You can filter alerts based on date, state, and severity.</span></span> <span data-ttu-id="4c8e6-133">Filtrera aviseringarna kan vara användbart för scenarier där du behöver toonarrow hello omfattning säkerhet aviseringar visas.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-133">Filtering alerts can be useful for scenarios where you need toonarrow hello scope of security alerts show.</span></span> <span data-ttu-id="4c8e6-134">Exempelvis kanske du vill tooaddress säkerhetsaviseringar som uppstått i hello senaste dygnet eftersom du undersöker ett potentiellt angrepp i hello system.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-134">For example, you might you want tooaddress security alerts that occurred in hello last 24 hours because you are investigating a potential breach in hello system.</span></span>

1. <span data-ttu-id="4c8e6-135">Klicka på **Filter** på hello **säkerhetsaviseringar** bladet.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-135">Click **Filter** on hello **Security Alerts** blade.</span></span> <span data-ttu-id="4c8e6-136">Hej **Filter** blad öppnas och du väljer hello datum, status och allvarlighetsgrad värden som du vill toosee.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-136">hello **Filter** blade opens and you select hello date, state, and severity values you wish toosee.</span></span>

    ![Filtrera varningar i Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig3-2017.png)

### <a name="respond-toosecurity-alerts"></a><span data-ttu-id="4c8e6-138">Svara toosecurity aviseringar</span><span class="sxs-lookup"><span data-stu-id="4c8e6-138">Respond toosecurity alerts</span></span>
<span data-ttu-id="4c8e6-139">Välj en avisering toolearn säkerhet mer om hello var som utlöste hello avisering och vad, om sådant finns, hur du behöver tootake tooremediate angreppet.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-139">Select a security alert toolearn more about hello event(s) that triggered hello alert and what, if any, steps you need tootake tooremediate an attack.</span></span> <span data-ttu-id="4c8e6-140">Säkerhetsaviseringarna är indelade i grupper efter typ och datum.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-140">Security alerts are grouped by type and date.</span></span> <span data-ttu-id="4c8e6-141">Klicka på en säkerhetsavisering öppnas ett blad som innehåller en lista över hello grupperade aviseringar.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-141">Clicking a security alert will open a blade containing a list of hello grouped alerts.</span></span>

![Svara toosecurity aviseringar i Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig5-ga.png)

<span data-ttu-id="4c8e6-143">I det här fallet finns hello aviseringar som har utlösts toosuspicious Remote Desktop Protocol (RDP)-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-143">In this case, hello alerts that were triggered refer toosuspicious Remote Desktop Protocol (RDP) activity.</span></span> <span data-ttu-id="4c8e6-144">hello första kolumnen ser du vilka resurser som är angripna; hello andra visar hur många gånger hello resurs angripna; hello tredje visar hello tid hello attack; hello fjärde visar tillståndet för hello avisering; och hello femte visar hello allvarlighetsgraden hello-attack.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-144">hello first column shows which resources were attacked; hello second shows how many times hello resource was attacked; hello third shows hello time of hello attack; hello fourth shows state of hello alert; and hello fifth shows hello severity of hello attack.</span></span> <span data-ttu-id="4c8e6-145">När du har granskat informationen klickar du på hello angripna resursen och ett nytt blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-145">After reviewing this information, click hello resource that was attacked and a new blade will open.</span></span>

![Förslag på vad toodo om säkerhet aviseringar i Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig6-ga.png)

<span data-ttu-id="4c8e6-147">I hello **beskrivning** på det här bladet hittar du mer information om den här händelsen.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-147">In hello **Description** field of this blade you will find more details about this event.</span></span> <span data-ttu-id="4c8e6-148">Dessa ytterligare information kan du se vilka utlösta hello säkerhet aviseringen hello målresurs, när tillämpliga hello käll-IP-adress och rekommendationer om hur tooremediate.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-148">These additional details offer insight into what triggered hello security alert, hello target resource, when applicable hello source IP address, and recommendations about how tooremediate.</span></span>  <span data-ttu-id="4c8e6-149">I vissa fall ska hello källans IP-adress vara tomt (inte tillgängligt) eftersom inte alla säkerhetshändelseloggar i Windows hello IP-adress.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-149">In some instances, hello source IP address will be empty (not available) because not all Windows security events logs include hello IP address.</span></span>

<span data-ttu-id="4c8e6-150">hello reparation föreslås av Security Center varierar bl.a toohello säkerhetsavisering.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-150">hello remediation suggested by Security Center will vary according toohello security alert.</span></span> <span data-ttu-id="4c8e6-151">I vissa fall kan du kanske toouse andra funktioner i Azure-tooimplement hello rekommenderade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-151">In some cases, you may have toouse other Azure capabilities tooimplement hello recommended remediation.</span></span> <span data-ttu-id="4c8e6-152">Till exempel hello reparation för det här angreppet är tooblacklist hello IP-adress som angreppet kommer ifrån med hjälp av en [nätverks-ACL](../virtual-network/virtual-networks-acl.md) eller en [nätverkssäkerhetsgruppen](../virtual-network/virtual-networks-nsg.md) regeln.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-152">For example, hello remediation for this attack is tooblacklist hello IP address that is generating this attack by using a [network ACL](../virtual-network/virtual-networks-acl.md) or a [network security group](../virtual-network/virtual-networks-nsg.md) rule.</span></span>

> [!NOTE]
> <span data-ttu-id="4c8e6-153">Mer information om hello olika typer av aviseringar [säkerhetsaviseringar efter typ i Azure Security Center](security-center-alerts-type.md).</span><span class="sxs-lookup"><span data-stu-id="4c8e6-153">For more information on hello different types of alerts, read [Security Alerts by Type in Azure Security Center](security-center-alerts-type.md).</span></span>
>
>

## <a name="see-also"></a><span data-ttu-id="4c8e6-154">Se även</span><span class="sxs-lookup"><span data-stu-id="4c8e6-154">See also</span></span>
<span data-ttu-id="4c8e6-155">I det här dokumentet du lärt dig hur tooconfigure säkerhetsprinciper i Security Center.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-155">In this document, you learned how tooconfigure security policies in Security Center.</span></span> <span data-ttu-id="4c8e6-156">toolearn mer om Security Center finns hello följande:</span><span class="sxs-lookup"><span data-stu-id="4c8e6-156">toolearn more about Security Center, see hello following:</span></span>

* [<span data-ttu-id="4c8e6-157">Hantera säkerhetsincidenter i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="4c8e6-157">Handling Security Incident in Azure Security Center</span></span>](security-center-incident.md)
* [<span data-ttu-id="4c8e6-158">Identifieringsfunktioner i Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="4c8e6-158">Azure Security Center Detection Capabilities</span></span>](security-center-detection-capabilities.md)
* [<span data-ttu-id="4c8e6-159">Planerings- och bruksanvisning för Azure Security Center</span><span class="sxs-lookup"><span data-stu-id="4c8e6-159">Azure Security Center Planning and Operations Guide</span></span>](security-center-planning-and-operations-guide.md)
* <span data-ttu-id="4c8e6-160">[Vanliga frågor om Azure Security Center](security-center-faq.md) – finns vanliga frågor om hur du använder hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-160">[Azure Security Center FAQ](security-center-faq.md) — Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="4c8e6-161">[Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) – Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure.</span><span class="sxs-lookup"><span data-stu-id="4c8e6-161">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) — Find blog posts about Azure security and compliance.</span></span>
