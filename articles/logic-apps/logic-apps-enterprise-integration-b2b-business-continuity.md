---
title: "aaaDisaster återställning för B2B-integrering konto - Azure Logic Apps | Microsoft Docs"
description: "Logic Apps B2B-katastrofåterställning"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: e86564a3c5a2607d22514936c606e2843cba0416
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="logic-apps-b2b-cross-region-disaster-recovery"></a><span data-ttu-id="4d6af-103">Logic Apps B2B mellan region-katastrofåterställning</span><span class="sxs-lookup"><span data-stu-id="4d6af-103">Logic Apps B2B cross-region disaster recovery</span></span>

<span data-ttu-id="4d6af-104">B2B arbetsbelastningar involverar pengar transaktioner som order och fakturor.</span><span class="sxs-lookup"><span data-stu-id="4d6af-104">B2B workloads involve money transactions like orders and invoices.</span></span> <span data-ttu-id="4d6af-105">Det är viktigt för ett företag tooquickly Återställ toomeet hello affärsnivå SLA överens med sina partner under en katastrofåterställning-händelse.</span><span class="sxs-lookup"><span data-stu-id="4d6af-105">During a disaster event, it's critical for a business tooquickly recover toomeet hello business-level SLAs agreed upon with their partners.</span></span> <span data-ttu-id="4d6af-106">Den här artikeln visar hur toobuild kontinuitet för företag planerar för B2B-arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="4d6af-106">This article demonstrates how toobuild a business continuity plan for B2B workloads.</span></span> 

* <span data-ttu-id="4d6af-107">Disaster Recovery beredskap</span><span class="sxs-lookup"><span data-stu-id="4d6af-107">Disaster Recovery readiness</span></span> 
* <span data-ttu-id="4d6af-108">Växla över toosecondary region under en katastrofåterställning-händelse</span><span class="sxs-lookup"><span data-stu-id="4d6af-108">Fail over toosecondary region during a disaster event</span></span> 
* <span data-ttu-id="4d6af-109">Infaller tooprimary region efter en katastrof-händelse</span><span class="sxs-lookup"><span data-stu-id="4d6af-109">Fall back tooprimary region after a disaster event</span></span>

## <a name="disaster-recovery-readiness"></a><span data-ttu-id="4d6af-110">Disaster Recovery beredskap</span><span class="sxs-lookup"><span data-stu-id="4d6af-110">Disaster Recovery readiness</span></span>  

1. <span data-ttu-id="4d6af-111">Ange en sekundär region och skapa en [integrering konto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) i hello sekundär region.</span><span class="sxs-lookup"><span data-stu-id="4d6af-111">Identify a secondary region and create an [integration account](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) in hello secondary region.</span></span>

2. <span data-ttu-id="4d6af-112">Lägg till partners, scheman och avtal för hello krävs meddelandet flöden där hello kör status måste toobe replikeras toosecondary region integration konto.</span><span class="sxs-lookup"><span data-stu-id="4d6af-112">Add partners, schemas, and agreements for hello required message flows where hello run status needs toobe replicated toosecondary region integration account.</span></span>

   > [!TIP]
   > <span data-ttu-id="4d6af-113">Kontrollera att det finns konsekvens i hello integration konto artefakts namngivningskonvention över regioner.</span><span class="sxs-lookup"><span data-stu-id="4d6af-113">Make sure there's consistency in hello integration account artifact's naming convention across regions.</span></span> 

3. <span data-ttu-id="4d6af-114">toopull hello kör status från hello primära regionen, skapa en logikapp i hello sekundär region.</span><span class="sxs-lookup"><span data-stu-id="4d6af-114">toopull hello run status from hello primary region, create a logic app in hello secondary region.</span></span> 

   <span data-ttu-id="4d6af-115">Den här logikapp ska ha en *utlösaren* och en *åtgärd*.</span><span class="sxs-lookup"><span data-stu-id="4d6af-115">This logic app should have a *trigger* and an *action*.</span></span> 
   <span data-ttu-id="4d6af-116">hello utlösaren ska ansluta tooprimary region integration konto och hello-åtgärd ska ansluta toosecondary region integration konto.</span><span class="sxs-lookup"><span data-stu-id="4d6af-116">hello trigger should connect tooprimary region integration account, and hello action should connect toosecondary region integration account.</span></span> 
   <span data-ttu-id="4d6af-117">Baserat på hello tidsintervall hello utlösare avsöker hello primära region som kör status tabell och hämtar hello nya poster, om sådana finns.</span><span class="sxs-lookup"><span data-stu-id="4d6af-117">Based on hello time interval, hello trigger polls hello primary region run status table and pulls hello new records, if any.</span></span> <span data-ttu-id="4d6af-118">hello åtgärden uppdaterar dem toosecondary region integration konto.</span><span class="sxs-lookup"><span data-stu-id="4d6af-118">hello action updates them toosecondary region integration account.</span></span> 
   <span data-ttu-id="4d6af-119">Detta hjälper tooget inkrementell Körningsstatus från primära region toosecondary region.</span><span class="sxs-lookup"><span data-stu-id="4d6af-119">This helps tooget incremental runtime status from primary region toosecondary region.</span></span>

4. <span data-ttu-id="4d6af-120">Kontinuitet för företag i Logic Apps integration kontot är utformad toosupport baserat på B2B - X12 AS2 och EDIFACT-protokoll.</span><span class="sxs-lookup"><span data-stu-id="4d6af-120">Business continuity in Logic Apps integration account is designed toosupport based on B2B protocols - X12, AS2, and EDIFACT.</span></span> <span data-ttu-id="4d6af-121">toofind beskrivs stegen, Välj hello respektive länkar.</span><span class="sxs-lookup"><span data-stu-id="4d6af-121">toofind detailed steps, select hello respective links.</span></span>

5. <span data-ttu-id="4d6af-122">Hej rekommendation är toodeploy alla primära region resurser i en sekundär region för.</span><span class="sxs-lookup"><span data-stu-id="4d6af-122">hello recommendation is toodeploy all primary region resources in a secondary region too.</span></span> 

   <span data-ttu-id="4d6af-123">Primär region resurser inkluderar Azure SQL Database eller Azure Cosmos DB, Azure Service Bus och Azure Event Hubs används för meddelanden, Azure API Management och hello Azure Logic Apps-funktionen i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="4d6af-123">Primary region resources include Azure SQL Database or Azure Cosmos DB, Azure Service Bus and Azure Event Hubs used for messaging, Azure API Management, and hello Azure Logic Apps feature in Azure App Service.</span></span>   

6. <span data-ttu-id="4d6af-124">Upprätta en anslutning från en primär region tooa sekundär region.</span><span class="sxs-lookup"><span data-stu-id="4d6af-124">Establish a connection from a primary region tooa secondary region.</span></span> <span data-ttu-id="4d6af-125">toopull hello kör status från en primär region, skapa en logikapp i en sekundär region.</span><span class="sxs-lookup"><span data-stu-id="4d6af-125">toopull hello run status from a primary region, create a logic app in a secondary region.</span></span> 

   <span data-ttu-id="4d6af-126">Hej logikapp ska ha en utlösare och en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="4d6af-126">hello logic app should have a trigger and an action.</span></span> 
   <span data-ttu-id="4d6af-127">hello utlösaren ska ansluta tooa primär region integration konto.</span><span class="sxs-lookup"><span data-stu-id="4d6af-127">hello trigger should connect tooa primary region integration account.</span></span> 
   <span data-ttu-id="4d6af-128">hello åtgärden bör ansluta tooa sekundär region integration konto.</span><span class="sxs-lookup"><span data-stu-id="4d6af-128">hello action should connect tooa secondary region integration account.</span></span> 
   <span data-ttu-id="4d6af-129">Baserat på hello tidsintervall hello utlösare avsöker hello primära region som kör status tabell och hämtar hello nya poster, om sådana finns.</span><span class="sxs-lookup"><span data-stu-id="4d6af-129">Based on hello time interval, hello trigger polls hello primary region run status table and pulls hello new records, if any.</span></span> 
   <span data-ttu-id="4d6af-130">hello åtgärden uppdaterar dem tooa sekundär region integration konto.</span><span class="sxs-lookup"><span data-stu-id="4d6af-130">hello action updates them tooa secondary region integration account.</span></span> 
   <span data-ttu-id="4d6af-131">Den här processen kan tooget inkrementell Körningsstatus från hello primär region toohello sekundär region.</span><span class="sxs-lookup"><span data-stu-id="4d6af-131">This process helps tooget incremental runtime status from hello primary region toohello secondary region.</span></span>

<span data-ttu-id="4d6af-132">Kontinuitet för företag i ett konto för Logic Apps-integration stöder baserat på hello B2B-protokollen X12, AS2 och EDIFACT.</span><span class="sxs-lookup"><span data-stu-id="4d6af-132">Business continuity in a Logic Apps integration account provides support based on hello B2B protocols X12, AS2, and EDIFACT.</span></span> <span data-ttu-id="4d6af-133">Detaljerade anvisningar om hur du använder X12 och AS2 finns [X12](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#x12) och [AS2](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#as2) i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="4d6af-133">For detailed steps on using X12 and AS2, see [X12](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#x12) and [AS2](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#as2) in this article.</span></span>

## <a name="fail-over-tooa-secondary-region-during-a-disaster-event"></a><span data-ttu-id="4d6af-134">Växla över tooa sekundär region under en katastrofåterställning-händelse</span><span class="sxs-lookup"><span data-stu-id="4d6af-134">Fail over tooa secondary region during a disaster event</span></span>

<span data-ttu-id="4d6af-135">Under en katastrofåterställning händelse, när hello primära region inte är tillgänglig för affärskontinuitet, trafiken toohello sekundär region.</span><span class="sxs-lookup"><span data-stu-id="4d6af-135">During a disaster event, when hello primary region is not available for business continuity, direct traffic toohello secondary region.</span></span> <span data-ttu-id="4d6af-136">En sekundär region hjälper en business toorecover fungerar snabbt toomeet hello Återställningspunktmål/Återställningstidsmål överens med sina partner.</span><span class="sxs-lookup"><span data-stu-id="4d6af-136">A secondary region helps a business toorecover functions quickly toomeet hello RPO/RTO agreed upon by their partners.</span></span> <span data-ttu-id="4d6af-137">Det minskar också ansträngningar toofail över från en region tooanother region.</span><span class="sxs-lookup"><span data-stu-id="4d6af-137">It also minimizes efforts toofail over from one region tooanother region.</span></span> 

<span data-ttu-id="4d6af-138">Det finns en förväntade fördröjning vid kopiering av kontrollen siffror från en primär region tooa sekundär region.</span><span class="sxs-lookup"><span data-stu-id="4d6af-138">There is an expected latency while copying control numbers from a primary region tooa secondary region.</span></span> <span data-ttu-id="4d6af-139">tooavoid skicka dubbla genererade kontrollen siffror toopartners under en katastrofåterställning händelse är hello rekommendation tooincrement hello kontrollen siffror i hello sekundär region avtal med hjälp av [PowerShell-cmdlets](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).</span><span class="sxs-lookup"><span data-stu-id="4d6af-139">tooavoid sending duplicate generated control numbers toopartners during a disaster event, hello recommendation is tooincrement hello control numbers in hello secondary region agreements by using [PowerShell cmdlets](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).</span></span>

## <a name="fall-back-tooa-primary-region-post-disaster-event"></a><span data-ttu-id="4d6af-140">Återgå tooa primär region efter katastrofåterställning händelse</span><span class="sxs-lookup"><span data-stu-id="4d6af-140">Fall back tooa primary region post-disaster event</span></span>

<span data-ttu-id="4d6af-141">toofall tillbaka tooa primär region när den är tillgänglig, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="4d6af-141">toofall back tooa primary region when it is available, follow these steps:</span></span>

1. <span data-ttu-id="4d6af-142">Stoppa ta emot meddelanden från partner i hello sekundär region.</span><span class="sxs-lookup"><span data-stu-id="4d6af-142">Stop accepting messages from partners in hello secondary region.</span></span>  

2. <span data-ttu-id="4d6af-143">Öka hello genereras kontrollen siffror för alla hello primär region avtal med hjälp av [PowerShell-cmdlets](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).</span><span class="sxs-lookup"><span data-stu-id="4d6af-143">Increment hello generated control numbers for all hello primary region agreements by using [PowerShell cmdlets](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).</span></span>  

3. <span data-ttu-id="4d6af-144">Trafiken från hello sekundär region toohello primär region.</span><span class="sxs-lookup"><span data-stu-id="4d6af-144">Direct traffic from hello secondary region toohello primary region.</span></span>

4. <span data-ttu-id="4d6af-145">Kontrollera att hello logikappen som skapats i hello sekundär region för att ta emot kör status från hello primär region har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="4d6af-145">Check that hello logic app created in hello secondary region for pulling run status from hello primary region is enabled.</span></span>

## <a name="x12"></a><span data-ttu-id="4d6af-146">X12</span><span class="sxs-lookup"><span data-stu-id="4d6af-146">X12</span></span> 

<span data-ttu-id="4d6af-147">Kontinuitet för företag för EDI X 12 dokument baserat på kontrollen siffror:</span><span class="sxs-lookup"><span data-stu-id="4d6af-147">Business continuity for EDI X12 documents is based on control numbers:</span></span>

> [!TIP]
> <span data-ttu-id="4d6af-148">Du kan också använda hello [X12 snabb start mallen](https://azure.microsoft.com/documentation/templates/201-logic-app-x12-disaster-recovery-replication/) toocreate logic apps.</span><span class="sxs-lookup"><span data-stu-id="4d6af-148">You can also use hello [X12 quick start template](https://azure.microsoft.com/documentation/templates/201-logic-app-x12-disaster-recovery-replication/) toocreate logic apps.</span></span> <span data-ttu-id="4d6af-149">Skapa primära och sekundära integrationskonton är förutsättningar toouse hello mallen.</span><span class="sxs-lookup"><span data-stu-id="4d6af-149">Creating primary and secondary integration accounts are prerequisites toouse hello template.</span></span> <span data-ttu-id="4d6af-150">hello kan mall toocreate två logikappar, en för mottagna kontrollen tal och en annan för genererade kontrollen tal.</span><span class="sxs-lookup"><span data-stu-id="4d6af-150">hello template helps toocreate two logic apps, one for received control numbers and another for generated control numbers.</span></span> <span data-ttu-id="4d6af-151">Respektive utlösare och åtgärder skapas i hello logikappar ansluter hello utlösaren toohello primära integration konto och hello åtgärdskontot toohello sekundära integrering.</span><span class="sxs-lookup"><span data-stu-id="4d6af-151">Respective triggers and actions are created in hello logic apps, connecting hello trigger toohello primary integration account and hello action toohello secondary integration account.</span></span>

<span data-ttu-id="4d6af-152">**Förutsättningar**</span><span class="sxs-lookup"><span data-stu-id="4d6af-152">**Prerequisites**</span></span>

<span data-ttu-id="4d6af-153">tooenable katastrofåterställning för inkommande meddelanden väljer hello dubbla Kontrollera inställningarna i hello X12 avtal får inställningarna.</span><span class="sxs-lookup"><span data-stu-id="4d6af-153">tooenable disaster recovery for inbound messages, select hello duplicate check settings in hello X12 agreement's Receive Settings.</span></span>

![Välj kontrollen av dubblett-inställningar](./media/logic-apps-enterprise-integration-b2b-business-continuity/dupcheck.png)  

1. <span data-ttu-id="4d6af-155">Skapa en [logikapp](../logic-apps/logic-apps-create-a-logic-app.md) i en sekundär region.</span><span class="sxs-lookup"><span data-stu-id="4d6af-155">Create a [logic app](../logic-apps/logic-apps-create-a-logic-app.md) in a secondary region.</span></span>    

2. <span data-ttu-id="4d6af-156">Sök på **X12**, och välj **X12-när ett kontrollnummer ändras**.</span><span class="sxs-lookup"><span data-stu-id="4d6af-156">Search on **X12**, and select **X12 - When a control number is modified**.</span></span>   

   ![Sök efter X12](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn1.png)

   <span data-ttu-id="4d6af-158">hello utlösaren uppmanar tooestablish ett anslutningskonto tooan integrering.</span><span class="sxs-lookup"><span data-stu-id="4d6af-158">hello trigger prompts you tooestablish a connection tooan integration account.</span></span> 
   <span data-ttu-id="4d6af-159">hello utlösaren ska vara anslutna tooa primär region integration konto.</span><span class="sxs-lookup"><span data-stu-id="4d6af-159">hello trigger should be connected tooa primary region integration account.</span></span>

3. <span data-ttu-id="4d6af-160">Ange ett anslutningsnamn, Välj din *primära region integration konto* från hello listan, och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="4d6af-160">Enter a connection name, select your *primary region integration account* from hello list, and choose **Create**.</span></span>   

   ![Primär region integration kontonamn](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn2.png)

4. <span data-ttu-id="4d6af-162">Hej **DateTime toostart kontroll nummer sync** inställningen är valfri.</span><span class="sxs-lookup"><span data-stu-id="4d6af-162">hello **DateTime toostart control number sync** setting is optional.</span></span> <span data-ttu-id="4d6af-163">Hej **frekvens** kan anges för**dag**, **timme**, **minut**, eller **andra** med ett intervall.</span><span class="sxs-lookup"><span data-stu-id="4d6af-163">hello **Frequency** can be set too**Day**, **Hour**, **Minute**, or **Second** with an interval.</span></span>   

   ![DateTime och frekvens](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

5. <span data-ttu-id="4d6af-165">Välj **nytt steg** > **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="4d6af-165">Select **New step** > **Add an action**.</span></span>

   ![Nytt steg, Lägg till en åtgärd](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

6. <span data-ttu-id="4d6af-167">Sök på **X12**, och välj **X12-Lägg till eller uppdatera kontroll siffror**.</span><span class="sxs-lookup"><span data-stu-id="4d6af-167">Search on **X12**, and select **X12 - Add or update control numbers**.</span></span>   

   ![Lägg till eller uppdatera kontroll siffror](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn5.png)

7. <span data-ttu-id="4d6af-169">tooconnect tooa sekundär region integration åtgärdskonto, Välj **ändra anslutningen** > **Lägg till ny anslutning** för en lista över hello tillgängliga integrationskonton.</span><span class="sxs-lookup"><span data-stu-id="4d6af-169">tooconnect an action tooa secondary region integration account, select **Change connection** > **Add new connection** for a list of hello available integration accounts.</span></span> <span data-ttu-id="4d6af-170">Ange ett anslutningsnamn, Välj din *sekundär region integration konto* från hello listan, och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="4d6af-170">Enter a connection name, select your *secondary region integration account* from hello list, and choose **Create**.</span></span> 

   ![Sekundär region integration kontonamn](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

8. <span data-ttu-id="4d6af-172">Växla tooraw indata genom att klicka på ikonen hello i övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="4d6af-172">Switch tooraw inputs by clicking on hello icon in upper right corner.</span></span>

   ![Växla tooraw indata](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12rawinputs.png)

9. <span data-ttu-id="4d6af-174">Välj brödtext hello väljaren för dynamiskt innehåll och spara hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="4d6af-174">Select Body from hello dynamic content picker, and save hello logic app.</span></span>

   ![Dynamiskt innehåll fält](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn7.png)

   <span data-ttu-id="4d6af-176">Baserat på hello tidsintervall hello utlösare avsöker hello primära region som tagits emot kontrollen register och hämtar hello nya poster.</span><span class="sxs-lookup"><span data-stu-id="4d6af-176">Based on hello time interval, hello trigger polls hello primary region received control number table and pulls hello new records.</span></span> 
   <span data-ttu-id="4d6af-177">hello åtgärden uppdaterar hello posterna i hello sekundär region integration konto.</span><span class="sxs-lookup"><span data-stu-id="4d6af-177">hello action updates hello records in hello secondary region integration account.</span></span> 
   <span data-ttu-id="4d6af-178">Om det finns inga uppdateringar, hello utlösaren status visas som **ignoreras**.</span><span class="sxs-lookup"><span data-stu-id="4d6af-178">If there are no updates, hello trigger status appears as **Skipped**.</span></span>   

   ![Register för kontrollen](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

<span data-ttu-id="4d6af-180">Baserat på hello tidsintervall replikerar hello inkrementell Körningsstatus från en primär region tooa sekundär region.</span><span class="sxs-lookup"><span data-stu-id="4d6af-180">Based on hello time interval, hello incremental runtime status replicates from a primary region tooa secondary region.</span></span> <span data-ttu-id="4d6af-181">Under en katastrofåterställning händelse, när hello primära regionen inte är tillgänglig, direkt trafik toohello sekundär region för verksamhetskontinuitet.</span><span class="sxs-lookup"><span data-stu-id="4d6af-181">During a disaster event, when hello primary region is not available, direct traffic toohello secondary region for business continuity.</span></span> 

## <a name="edifact"></a><span data-ttu-id="4d6af-182">EDIFACT</span><span class="sxs-lookup"><span data-stu-id="4d6af-182">EDIFACT</span></span> 

<span data-ttu-id="4d6af-183">Kontinuitet för företag för EDI EDIFACT-dokument baseras på kontrollen siffror.</span><span class="sxs-lookup"><span data-stu-id="4d6af-183">Business continuity for EDI EDIFACT documents is based on control numbers.</span></span>

<span data-ttu-id="4d6af-184">**Förutsättningar**</span><span class="sxs-lookup"><span data-stu-id="4d6af-184">**Prerequisites**</span></span>

<span data-ttu-id="4d6af-185">tooenable katastrofåterställning för inkommande meddelanden väljer hello dubbla Kontrollera inställningarna i din EDIFACT-avtal får inställningarna.</span><span class="sxs-lookup"><span data-stu-id="4d6af-185">tooenable disaster recovery for inbound messages, select hello duplicate check settings in your EDIFACT agreement's Receive Settings.</span></span>

![Välj kontrollen av dubblett-inställningar](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactdupcheck.png)  

1. <span data-ttu-id="4d6af-187">Skapa en [logikapp](../logic-apps/logic-apps-create-a-logic-app.md) i en sekundär region.</span><span class="sxs-lookup"><span data-stu-id="4d6af-187">Create a [logic app](../logic-apps/logic-apps-create-a-logic-app.md) in a secondary region.</span></span>    

2. <span data-ttu-id="4d6af-188">Sök på **EDIFACT**, och välj **EDIFACT - när ett kontrollnummer ändras**.</span><span class="sxs-lookup"><span data-stu-id="4d6af-188">Search on **EDIFACT**, and select **EDIFACT - When a control number is modified**.</span></span>

   ![Sök efter EDIFACT](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactcn1.png)

   <span data-ttu-id="4d6af-190">hello utlösaren uppmanar tooestablish ett anslutningskonto tooan integrering.</span><span class="sxs-lookup"><span data-stu-id="4d6af-190">hello trigger prompts you tooestablish a connection tooan integration account.</span></span> 
   <span data-ttu-id="4d6af-191">hello utlösaren ska vara anslutna tooa primär region integration konto.</span><span class="sxs-lookup"><span data-stu-id="4d6af-191">hello trigger should be connected tooa primary region integration account.</span></span> 

3. <span data-ttu-id="4d6af-192">Ange ett anslutningsnamn, Välj din *primära region integration konto* från hello listan, och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="4d6af-192">Enter a connection name, select your *primary region integration account* from hello list, and choose **Create**.</span></span>    

   ![Primär region integration kontonamn](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN2.png)

4. <span data-ttu-id="4d6af-194">Hej **DateTime toostart kontroll nummer sync** inställningen är valfri.</span><span class="sxs-lookup"><span data-stu-id="4d6af-194">hello **DateTime toostart control number sync** setting is optional.</span></span> <span data-ttu-id="4d6af-195">Hej **frekvens** kan anges för**dag**, **timme**, **minut**, eller **andra** med ett intervall.</span><span class="sxs-lookup"><span data-stu-id="4d6af-195">hello **Frequency** can be set too**Day**, **Hour**, **Minute**, or **Second** with an interval.</span></span>    

   ![DateTime och frekvens](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

6. <span data-ttu-id="4d6af-197">Välj **nytt steg** > **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="4d6af-197">Select **New step** > **Add an action**.</span></span>    

   ![Nytt steg, Lägg till en åtgärd](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

7. <span data-ttu-id="4d6af-199">Sök på **EDIFACT**, och välj **EDIFACT - Lägg till eller uppdatera kontroll siffror**.</span><span class="sxs-lookup"><span data-stu-id="4d6af-199">Search on **EDIFACT**, and select **EDIFACT - Add or update control numbers**.</span></span>   

   ![Lägg till eller uppdatera kontroll siffror](./media/logic-apps-enterprise-integration-b2b-business-continuity/EdifactChooseAction.png)

8. <span data-ttu-id="4d6af-201">tooconnect tooa sekundär region integration åtgärdskonto, Välj **ändra anslutningen** > **Lägg till ny anslutning** för en lista över hello tillgängliga integrationskonton.</span><span class="sxs-lookup"><span data-stu-id="4d6af-201">tooconnect an action tooa secondary region integration account, select **Change connection** > **Add new connection** for a list of hello available integration accounts.</span></span> <span data-ttu-id="4d6af-202">Ange ett anslutningsnamn, Välj din *sekundär region integration konto* från hello listan, och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="4d6af-202">Enter a connection name, select your *secondary region integration account* from hello list, and choose **Create**.</span></span>

   ![Sekundär region integration kontonamn](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

9. <span data-ttu-id="4d6af-204">Växla tooraw indata genom att klicka på ikonen hello i övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="4d6af-204">Switch tooraw inputs by clicking on hello icon in upper right corner.</span></span>

   ![Växla tooraw indata](./media/logic-apps-enterprise-integration-b2b-business-continuity/Edifactrawinputs.png)

10. <span data-ttu-id="4d6af-206">Välj brödtext hello väljaren för dynamiskt innehåll och spara hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="4d6af-206">Select Body from hello dynamic content picker, and save hello logic app.</span></span>   

   ![Dynamiskt innehåll fält](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN7.png)

   <span data-ttu-id="4d6af-208">Baserat på hello tidsintervall hello utlösare avsöker hello primära region som tagits emot kontrollen register och hämtar hello nya poster.</span><span class="sxs-lookup"><span data-stu-id="4d6af-208">Based on hello time interval, hello trigger polls hello primary region received control number table and pulls hello new records.</span></span>
   <span data-ttu-id="4d6af-209">hello åtgärden uppdaterar hello poster toohello sekundär region integration konto.</span><span class="sxs-lookup"><span data-stu-id="4d6af-209">hello action updates hello records toohello secondary region integration account.</span></span> 
   <span data-ttu-id="4d6af-210">Om det finns inga uppdateringar, hello utlösaren status visas som **ignoreras**.</span><span class="sxs-lookup"><span data-stu-id="4d6af-210">If there are no updates, hello trigger status appears as **Skipped**.</span></span>

   ![Register för kontrollen](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

<span data-ttu-id="4d6af-212">Baserat på hello tidsintervall replikerar hello inkrementell Körningsstatus från en primär region tooa sekundär region.</span><span class="sxs-lookup"><span data-stu-id="4d6af-212">Based on hello time interval, hello incremental runtime status replicates from a primary region tooa secondary region.</span></span> <span data-ttu-id="4d6af-213">Under en katastrofåterställning händelse, när hello primära regionen inte är tillgänglig, direkt trafik toohello sekundär region för verksamhetskontinuitet.</span><span class="sxs-lookup"><span data-stu-id="4d6af-213">During a disaster event, when hello primary region is not available, direct traffic toohello secondary region for business continuity.</span></span> 

## <a name="as2"></a><span data-ttu-id="4d6af-214">AS2</span><span class="sxs-lookup"><span data-stu-id="4d6af-214">AS2</span></span> 

<span data-ttu-id="4d6af-215">Kontinuitet för företag för dokument som använder hello AS2-protokollet är baserat på hello-meddelande-ID och hello MIC värdet.</span><span class="sxs-lookup"><span data-stu-id="4d6af-215">Business continuity for documents that use hello AS2 protocol is based on hello message ID and hello MIC value.</span></span>

> [!TIP]
> <span data-ttu-id="4d6af-216">Du kan också använda hello [AS2 Snabbstart mallen](https://github.com/Azure/azure-quickstart-templates/pull/3302) toocreate logic apps.</span><span class="sxs-lookup"><span data-stu-id="4d6af-216">You can also use hello [AS2 quick start template](https://github.com/Azure/azure-quickstart-templates/pull/3302) toocreate logic apps.</span></span> <span data-ttu-id="4d6af-217">Skapa primära och sekundära integrationskonton är förutsättningar toouse hello mallen.</span><span class="sxs-lookup"><span data-stu-id="4d6af-217">Creating primary and secondary integration accounts are prerequisites toouse hello template.</span></span> <span data-ttu-id="4d6af-218">hello mallen hjälper dig att skapa en logikapp som har en utlösare och en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="4d6af-218">hello template helps create a logic app that has a trigger and an action.</span></span> <span data-ttu-id="4d6af-219">Hej logikapp skapar en anslutning från ett konto för utlösaren tooa primära integrering och ett åtgärdskonto tooa sekundära integrering.</span><span class="sxs-lookup"><span data-stu-id="4d6af-219">hello logic app creates a connection from a trigger tooa primary integration account and an action tooa secondary integration account.</span></span>

1. <span data-ttu-id="4d6af-220">Skapa en [logikapp](../logic-apps/logic-apps-create-a-logic-app.md) i hello sekundär region.</span><span class="sxs-lookup"><span data-stu-id="4d6af-220">Create a [logic app](../logic-apps/logic-apps-create-a-logic-app.md) in hello secondary region.</span></span>  

2. <span data-ttu-id="4d6af-221">Sök på **AS2**, och välj **AS2 - när ett MIC värdet skapas**.</span><span class="sxs-lookup"><span data-stu-id="4d6af-221">Search on **AS2**, and select **AS2 - When a MIC value is created**.</span></span>   

   ![Sök efter AS2](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid1.png)

   <span data-ttu-id="4d6af-223">En utlösare efterfrågar tooestablish ett anslutningskonto tooan integrering.</span><span class="sxs-lookup"><span data-stu-id="4d6af-223">A trigger prompts you tooestablish a connection tooan integration account.</span></span> 
   <span data-ttu-id="4d6af-224">hello utlösaren ska vara anslutna tooa primär region integration konto.</span><span class="sxs-lookup"><span data-stu-id="4d6af-224">hello trigger should be connected tooa primary region integration account.</span></span> 
   
3. <span data-ttu-id="4d6af-225">Ange ett anslutningsnamn, Välj din *primära region integration konto* från hello listan, och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="4d6af-225">Enter a connection name, select your *primary region integration account* from hello list, and choose **Create**.</span></span>

   ![Primär region integration kontonamn](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid2.png)

4. <span data-ttu-id="4d6af-227">Hej **DateTime toostart MIC värdet sync** inställningen är valfri.</span><span class="sxs-lookup"><span data-stu-id="4d6af-227">hello **DateTime toostart MIC value sync** setting is optional.</span></span> <span data-ttu-id="4d6af-228">Hej **frekvens** kan anges för**dag**, **timme**, **minut**, eller **andra** med ett intervall.</span><span class="sxs-lookup"><span data-stu-id="4d6af-228">hello **Frequency** can be set too**Day**, **Hour**, **Minute**, or **Second** with an interval.</span></span>   

   ![DateTime och frekvens](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid3.png)

5. <span data-ttu-id="4d6af-230">Välj **nytt steg** > **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="4d6af-230">Select **New step** > **Add an action**.</span></span>  

   ![Nytt steg, Lägg till en åtgärd](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid4.png)

6. <span data-ttu-id="4d6af-232">Sök på **AS2**, och välj **AS2 - Lägg till eller uppdatera MIC innehållet**.</span><span class="sxs-lookup"><span data-stu-id="4d6af-232">Search on **AS2**, and select **AS2 - Add or update MIC contents**.</span></span>  

   ![MIC tillägg eller uppdatering](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid5.png)

7. <span data-ttu-id="4d6af-234">tooconnect tooa sekundära integration åtgärdskonto, Välj **ändra anslutningen** > **Lägg till ny anslutning** för en lista över hello tillgängliga integrationskonton.</span><span class="sxs-lookup"><span data-stu-id="4d6af-234">tooconnect an action tooa secondary integration account, select **Change connection** > **Add new connection** for a list of hello available integration accounts.</span></span> <span data-ttu-id="4d6af-235">Ange ett anslutningsnamn, Välj din *sekundär region integration konto* från hello listan, och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="4d6af-235">Enter a connection name, select your *secondary region integration account* from hello list, and choose **Create**.</span></span>

   ![Sekundär region integration kontonamn](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid6.png)

8. <span data-ttu-id="4d6af-237">Växla tooraw indata genom att klicka på ikonen hello i övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="4d6af-237">Switch tooraw inputs by clicking on hello icon in upper right corner.</span></span>

   ![Växla tooraw indata](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2rawinputs.png)

9. <span data-ttu-id="4d6af-239">Välj brödtext hello väljaren för dynamiskt innehåll och spara hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="4d6af-239">Select Body from hello dynamic content picker, and save hello logic app.</span></span>   

   ![Dynamiskt innehåll](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid7.png)

   <span data-ttu-id="4d6af-241">Baserat på hello tidsintervall hello utlösare avsöker hello primär region tabell och hämtar hello nya poster.</span><span class="sxs-lookup"><span data-stu-id="4d6af-241">Based on hello time interval, hello trigger polls hello primary region table and pulls hello new records.</span></span> <span data-ttu-id="4d6af-242">hello åtgärden uppdaterar dem toohello sekundär region integration konto.</span><span class="sxs-lookup"><span data-stu-id="4d6af-242">hello action updates them toohello secondary region integration account.</span></span> 
   <span data-ttu-id="4d6af-243">Om det finns inga uppdateringar, hello utlösaren status visas som **ignoreras**.</span><span class="sxs-lookup"><span data-stu-id="4d6af-243">If there are no updates, hello trigger status appears as **Skipped**.</span></span>  

   ![Primär region tabell](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid8.png)

<span data-ttu-id="4d6af-245">Baserat på hello tidsintervall replikerar hello inkrementell Körningsstatus från hello primär region toohello sekundär region.</span><span class="sxs-lookup"><span data-stu-id="4d6af-245">Based on hello time interval, hello incremental runtime status replicates from hello primary region toohello secondary region.</span></span> <span data-ttu-id="4d6af-246">Under en katastrofåterställning händelse, när hello primära regionen inte är tillgänglig, direkt trafik toohello sekundär region för verksamhetskontinuitet.</span><span class="sxs-lookup"><span data-stu-id="4d6af-246">During a disaster event, when hello primary region is not available, direct traffic toohello secondary region for business continuity.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="4d6af-247">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4d6af-247">Next steps</span></span>

[<span data-ttu-id="4d6af-248">Övervaka B2B-meddelanden</span><span class="sxs-lookup"><span data-stu-id="4d6af-248">Monitor B2B messages</span></span>](logic-apps-monitor-b2b-message.md)

